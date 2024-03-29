---
ms.date: 06/12/2017
ms.topic: conceptual
keywords: a DSC, a powershell, a konfiguráció, a beállítása
title: Egypéldányos DSC-erőforrás írása (ajánlott eljárás)
ms.openlocfilehash: fc118fd8b0d91d2001030769ac7e3c6321972905
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/09/2018
---
# <a name="writing-a-single-instance-dsc-resource-best-practice"></a>Egypéldányos DSC-erőforrás írása (ajánlott eljárás)

>**Megjegyzés:** Ez a témakör ismerteti a DSC erőforrása, amely lehetővé teszi, hogy csak egy példányban konfiguráció meghatározása esetén ajánlott eljárás. Jelenleg nincs ehhez beépített DSC szolgáltatás. Amely a későbbiekben változhat.

Vannak olyan helyzetek, ahol nem is engedélyezni szeretné egy erőforrást egy konfigurációja többször használható. Például a egy előző megvalósításában a [xTimeZone](https://github.com/PowerShell/xTimeZone) erőforrás, a konfiguráció sikerült hívja az erőforrás többször, az időzóna beállítása minden erőforrás blokkban különböző beállítását:

```powershell
Configuration SetTimeZone
{
    Param
    (
        [String[]]$NodeName = $env:COMPUTERNAME

    )

    Import-DSCResource -ModuleName xTimeZone


    Node $NodeName
    {
         xTimeZone TimeZoneExample
         {

            TimeZone = 'Eastern Standard Time'
         }

         xTimeZone TimeZoneExample2
         {

            TimeZone = 'Pacific Standard Time'

         }

    }
}
```

Ez a DSC-erőforrás kulcsok működik módjával. Egy erőforrás rendelkeznie kell legalább egy kulcstulajdonságot. Egy erőforrás-példány egyedi, ha az érték az összes kulcstulajdonságait egyedi minősül. Előző végrehajtása során a [xTimeZone](https://github.com/PowerShell/xTimeZone) erőforrás volt csak egy tulajdonságot--**időzóna**, amely szükséges kulcsként. Ebből kifolyólag például a fenti konfigurációt volna fordítási és figyelmeztetés nélkül futtatja. Egyes a **xTimeZone** erőforrás blokkok egyedinek számít. Ez a csomópont oda-vissza az időzóna váltás ismételten alkalmazni kívánt konfigurációs okozna.

Annak biztosítása érdekében, hogy egy konfigurációs sikerült egy célcsomóponttal időzónáját csak egyszer, az erőforrás frissült a második tulajdonság, **IsSingleInstance**, amely a kulcstulajdonság vált.
A **IsSingleInstance** korlátozódott egyetlen érték "Yes" használatával egy **ValueMap**. Az erőforrás a régi MOF-séma a következő volt:

```powershell
[ClassVersion("1.0.0.0"), FriendlyName("xTimeZone")]
class xTimeZone : OMI_BaseResource
{
    [Key, Description("Specifies the TimeZone.")] String TimeZone;
};
```

Az erőforrás a frissített MOF-séma a következő:

```powershell
[ClassVersion("1.0.0.0"), FriendlyName("xTimeZone")]
class xTimeZone : OMI_BaseResource
{
    [Key, Description("Specifies the resource is a single instance, the value must be 'Yes'"), ValueMap{"Yes"}, Values{"Yes"}] String IsSingleInstance;
    [Required, Description("Specifies the TimeZone.")] String TimeZone;
};
```

Az erőforrást parancsprogram is frissült, használja az új paramétert. A régi erőforrást parancsprogram a következő:

```powershell
function Get-TargetResource
{
    [CmdletBinding()]
    [OutputType([Hashtable])]
    param
    (
        [parameter(Mandatory = $true)]
        [ValidateSet('Yes')]
        [String]
        $IsSingleInstance,

        [parameter(Mandatory = $true)]
        [ValidateNotNullOrEmpty()]
        [String]
        $TimeZone
    )

    #Get the current TimeZone
    $CurrentTimeZone = Get-TimeZone

    $returnValue = @{
        TimeZone = $CurrentTimeZone
        IsSingleInstance = 'Yes'
    }

    #Output the target resource
    $returnValue
}


function Set-TargetResource
{
    [CmdletBinding(SupportsShouldProcess=$true)]
    param
    (
        [parameter(Mandatory = $true)]
        [ValidateSet('Yes')]
        [String]
        $IsSingleInstance,

        [parameter(Mandatory = $true)]
        [ValidateNotNullOrEmpty()]
        [String]
        $TimeZone
    )

    #Output the result of Get-TargetResource function.
    $CurrentTimeZone = Get-TimeZone

    if($PSCmdlet.ShouldProcess("'$TimeZone'","Replace the System Time Zone"))
    {
        try
        {
            if($CurrentTimeZone -ne $TimeZone)
            {
                Write-Verbose -Verbose "Setting the TimeZone"
                Set-TimeZone -TimeZone $TimeZone}
            else
            {
                Write-Verbose -Verbose "TimeZone already set to $TimeZone"
            }
        }
        catch
        {
            $ErrorMsg = $_.Exception.Message
            Write-Verbose -Verbose $ErrorMsg
        }
    }
}


function Test-TargetResource
{
    [CmdletBinding()]
    [OutputType([Boolean])]
    param
    (
        [parameter(Mandatory = $true)]
        [ValidateSet('Yes')]
        [String]
        $IsSingleInstance,

        [parameter(Mandatory = $true)]
        [ValidateNotNullOrEmpty()]
        [String]
        $TimeZone
    )

    #Output from Get-TargetResource
    $CurrentTimeZone = Get-TimeZone

    if($TimeZone -eq $CurrentTimeZone)
    {
        return $true
    }
    else
    {
        return $false
    }
}

Function Get-TimeZone {
    [CmdletBinding()]
    param()

    & tzutil.exe /g
}

Function Set-TimeZone {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory=$true)]
        [System.String]
        $TimeZone
    )

    try
    {
        & tzutil.exe /s $TimeZone
    }
    catch
    {
        $ErrorMsg = $_.Exception.Message
        Write-Verbose $ErrorMsg
    }
}

Export-ModuleMember -Function *-TargetResource
```

Figyelje meg, hogy a **időzóna** tulajdonság már nincs egy kulcsot. Most, ha egy konfigurációs megpróbálja kétszer időzónájának beállítása (használatával két különböző **xTimeZone** másik blokkok **időzóna** értékek), megpróbálja a konfiguráció-fordítási hiba következtében:

```powershell
Test-ConflictingResources : A conflict was detected between resources '[xTimeZone]TimeZoneExample (::15::10::xTimeZone)' and
'[xTimeZone]TimeZoneExample2 (::22::10::xTimeZone)' in node 'CONTOSO-CLIENT'. Resources have identical key properties but there are differences in the
following non-key properties: 'TimeZone'. Values 'Eastern Standard Time' don't match values 'Pacific Standard Time'. Please update these property
values so that they are identical in both cases.
At line:271 char:9
+         Test-ConflictingResources $keywordName $canonicalizedValue $k ...
+         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Write-Error], InvalidOperationException
    + FullyQualifiedErrorId : ConflictingDuplicateResource,Test-ConflictingResources
Errors occurred while processing configuration 'SetTimeZone'.
At C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules\PSDesiredStateConfiguration\PSDesiredStateConfiguration.psm1:3705 char:5
+     throw $ErrorRecord
+     ~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (SetTimeZone:String) [], InvalidOperationException
    + FullyQualifiedErrorId : FailToProcessConfiguration
```