---
ms.date: 06/12/2017
author: JKeithB
ms.topic: reference
keywords: WMF, powershell, beállítás
ms.openlocfilehash: 89e996942cdc2609c670e8e5ba2c576ff6342a9c
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/09/2018
---
# <a name="powershellget-cmdlets-for-module-management"></a>PowerShellGet-parancsmagok modulkezeléshez

- [Keresés – DscResource](https://technet.microsoft.com/library/mt654006.aspx)
- [A modul keresése](https://technet.microsoft.com/library/dn807167.aspx)
- [Find-Script](https://technet.microsoft.com/library/mt654001.aspx)
- [Get-InstalledModule](https://technet.microsoft.com/en-us/library/mt653990.aspx)
- [Get-InstalledScript](https://technet.microsoft.com/en-us/library/mt653994.aspx)
- [Get-PSRepository](https://technet.microsoft.com/en-us/library/dn807170.aspx)
- [Install-modul](https://technet.microsoft.com/en-us/library/dn807162.aspx)
- [Install-Script](https://technet.microsoft.com/en-us/library/mt653998.aspx)
- [New-ScriptFileInfo](https://technet.microsoft.com/en-us/library/mt653995.aspx)
- [Publish-Module](https://technet.microsoft.com/en-us/library/dn807163.aspx)
- [Publish-Script](https://technet.microsoft.com/en-us/library/mt654003.aspx)
- [Register-PSRepository](https://technet.microsoft.com/en-us/library/dn807168.aspx)
- [Save-Module](https://technet.microsoft.com/en-us/library/mt653992.aspx)
- [Save-Script](https://technet.microsoft.com/en-us/library/mt654004.aspx)
- [Set-PSRepository](https://technet.microsoft.com/en-us/library/dn807165.aspx)
- [Test-ScriptFileInfo](https://technet.microsoft.com/en-us/library/mt654005.aspx)
- [Távolítsa el modul](https://technet.microsoft.com/en-us/library/mt653996.aspx)
- [Távolítsa el parancsfájl](https://technet.microsoft.com/en-us/library/mt653989.aspx)
- [Update-Module](https://technet.microsoft.com/en-us/library/dn807166.aspx)
- [Update-ModuleManifest](https://technet.microsoft.com/en-us/library/mt654002.aspx)
- [Update-Script](https://technet.microsoft.com/en-us/library/mt653997.aspx)
- [Update-ScriptFileInfo](https://technet.microsoft.com/en-us/library/mt653991.aspx)
- [PSRepository regisztrációjának törlése](https://technet.microsoft.com/en-us/library/dn807161.aspx)

## <a name="module-dependency-installation-support-get-installedmodule-and-uninstall-module-cmdlets"></a>Modul függőségi telepítési támogatja, a Get-InstalledModule és az Uninstall-modul parancsmagjai
- A Publish-modul a parancsmag modul függőségek feltöltési felvételére. PSModuleInfo RequiredModules és NestedModules listája közzé kell tenni a függőségi lista a modulok készítéséhez használják.
- A telepítés- és frissítés-modul parancsmagokkal hozzáadott függőségi telepítési támogatást. Modul függőségek vannak telepítve, és alapértelmezés szerint frissíti.
- A keresés-modul a parancsmag modul függőségek szerepeljenek az eredmények egy - IncludeDependencies paraméter hozzá.
- A keresett modul - MaximumVersion támogatása Install-modul, és a frissítés-modul parancsmagokkal.
- A hozzáadott új Get-InstalledModule és eltávolítás-modul parancsmagokkal.

## <a name="powershellget-cmdlets-demo-with-module-dependencies-support"></a>Támogatja a PowerShellGet parancsmagok bemutató modul függőségek:

### <a name="ensure-that-module-dependencies-are-available-on-the-repository"></a>Győződjön meg arról, hogy a modul függőségek érhetők el a tárházban:
```powershell
Find-Module -Repository LocalRepo -Name RequiredModule1,RequiredModule2,RequiredModule3,NestedRequiredModule1,NestedRequiredModule2,NestedRequiredModule3 | Sort-Object -Property Name

Version    Name                     Repository    Description
-------    ----                     ----------    -----------
2.5        NestedRequiredModule1    LocalRepo     NestedRequiredModule1 module
2.5        NestedRequiredModule2    LocalRepo     NestedRequiredModule2 module
2.0        NestedRequiredModule3    LocalRepo     NestedRequiredModule3 module
2.5        RequiredModule1          LocalRepo     RequiredModule1 module
2.5        RequiredModule2          LocalRepo     RequiredModule2 module
2.0        RequiredModule3          LocalRepo     RequiredModule3 module
```

### <a name="create-a-module-with-dependencies-that-are-specified-in-the-requiredmodules-and-nestedmodules-properties-of-its-module-manifest"></a>Hozzon létre egy modult, amely szerepel a moduljegyzékben RequiredModules és NestedModules tulajdonságainak függőségekkel rendelkező.
```powershell
$RequiredModules = @('RequiredModule1',
                     @{ModuleName = 'RequiredModule2'; ModuleVersion = '1.5'; },
                     @{ModuleName = 'RequiredModule3'; RequiredVersion = '2.0'; })

$NestedRequiredModules = @('TestDepWithNestedRequiredModules1.psm1', 'NestedRequiredModule1',
                            @{ModuleName = 'NestedRequiredModule2'; ModuleVersion = '1.5'; },
                            @{ModuleName = 'NestedRequiredModule3'; RequiredVersion = '2.0'; })

New-ModuleManifest -Path 'C:\Program Files\WindowsPowerShell\Modules\TestDepWithNestedRequiredModules1\TestDepWithNestedRequiredModules1.psd1' `
-NestedModules $NestedRequiredModules -RequiredModules $RequiredModules -ModuleVersion "1.0" -Description "TestDepWithNestedRequiredModules1 module"
```

###  <a name="publish-two-versions-10-and-20-of-the-testdepwithnestedrequiredmodules1-module-with-dependencies-to-the-repository"></a>Két verziója közzététele (**"1.0"** és **"2.0"**) a tárházba függőségekkel rendelkező TestDepWithNestedRequiredModules1 modul.
```powershell
Publish-Module -Name TestDepWithNestedRequiredModules1 -Repository LocalRepo -NuGetApiKey "MyNuGet-ApiKey-For-LocalRepo"
```

###  <a name="find-the-testdepwithnestedrequiredmodules1-module-with-its-dependencies-by-specifying--includedependencies"></a>A modul keresése zajlik TestDepWithNestedRequiredModules1 a függőségekkel rendelkező - IncludeDependencies megadásával.
```powershell
Find-Module -Name TestDepWithNestedRequiredModules1 -Repository LocalRepo –IncludeDependencies -MaximumVersion "1.0"

Version    Name                                Repository  Description
-------    ----                                ----------  -----------
1.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module
2.5        RequiredModule1                     LocalRepo   RequiredModule1 module
2.5        RequiredModule2                     LocalRepo   RequiredModule2 module
2.0        RequiredModule3                     LocalRepo   RequiredModule3 module
2.5        NestedRequiredModule1               LocalRepo   NestedRequiredModule1 module
2.5        NestedRequiredModule2               LocalRepo   NestedRequiredModule2 module
2.0        NestedRequiredModule3               LocalRepo   NestedRequiredModule3 module
```

### <a name="use-find-module-metadata-to-find-the-module-dependencies"></a>A modul függőségek kereséséhez használja a keresés-modul metaadatai.
```powershell
$psgetModuleInfo = Find-Module -Repository MSPSGallery -Name ModuleWithDependencies2
$psgetModuleInfo.Dependencies.ModuleName

RequiredModule1
RequiredModule2
RequiredModule3
NestedRequiredModule1
NestedRequiredModule2
NestedRequiredModule3

$psgetModuleInfo.Dependencies

Name Value
---- -----
ModuleName RequiredModule1
CanonicalId PowerShellGet:RequiredModule1/#http://psget/psGallery/api/v2/

ModuleName RequiredModule2
ModuleVersion 2.0
CanonicalId PowerShellGet:RequiredModule2/2.0#http://psget/psGallery/api/v2/

ModuleName RequiredModule3
RequiredVersion 2.5
CanonicalId PowerShellGet:RequiredModule3/2.5#http://psget/psGallery/api/v2/

ModuleName NestedRequiredModule1
CanonicalId PowerShellGet:NestedRequiredModule1/#http://psget/psGallery/api/v2/

ModuleName NestedRequiredModule2
ModuleVersion 2.0
CanonicalId PowerShellGet:NestedRequiredModule2/2.0#http://psget/psGallery/api/v2/

ModuleName NestedRequiredModule3
RequiredVersion 2.5
CanonicalId PowerShellGet:NestedRequiredModule3/2.5#http://psget/psGallery/api/v2/
```

###  <a name="install-the-testdepwithnestedrequiredmodules1-module-with-dependencies"></a>A TestDepWithNestedRequiredModules1 modul telepítése függőségekkel rendelkező.
```powershell
Install-Module -Name TestDepWithNestedRequiredModules1 -Repository LocalRepo -RequiredVersion "1.0"
Get-InstalledModule

Version    Name                    Repository   Description
-------    ----                    ----------   -----------
1.0        NestedRequiredModule1   LocalRepo    NestedRequiredModule1 module
2.5        NestedRequiredModule2   LocalRepo    NestedRequiredModule2 module
2.0        NestedRequiredModule3   LocalRepo    NestedRequiredModule3 module
1.0        RequiredModule1         LocalRepo    RequiredModule1 module
2.5        RequiredModule2                    LocalRepo    RequiredModule2 module
2.0        RequiredModule3                    LocalRepo    RequiredModule3 module
1.0        TestDepWithNestedRequiredModules1  LocalRepo    TestDepWithNestedRequiredModules1 module
```

###  <a name="update-the-testdepwithnestedrequiredmodules1-module-with-dependencies"></a>Frissítse a TestDepWithNestedRequiredModules1 modul függőségek.
```powershell
Find-Module -Name TestDepWithNestedRequiredModules1 -Repository LocalRepo -AllVersions

Version    Name                                Repository  Description
-------    ----                                ----------  -----------
1.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module
2.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module

Update-Module -Name TestDepWithNestedRequiredModules1 -RequiredVersion 2.0
Get-InstalledModule

Version    Name                                Repository  Description
-------    ----                                ----------  -----------
1.0        NestedRequiredModule1               LocalRepo   NestedRequiredModule1 module
2.5        NestedRequiredModule2               LocalRepo   NestedRequiredModule2 module
2.0        NestedRequiredModule3               LocalRepo   NestedRequiredModule3 module
2.5        NestedRequiredModule3               LocalRepo   NestedRequiredModule3 module
1.0        RequiredModule1                     LocalRepo   RequiredModule1 module
2.5        RequiredModule2                     LocalRepo   RequiredModule2 module
2.0        RequiredModule3                     LocalRepo   RequiredModule3 module
2.5        RequiredModule3                     LocalRepo   RequiredModule3 module
1.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module
2.0        TestDepWithNestedRequiredModules1   LocalRepo   TestDepWithNestedRequiredModules1 module
```

###  <a name="run-the-uninstall-module-cmdlet-to-uninstall-a-module-that-you-installed-by-using-powershellget"></a>Futtassa az Uninstall-modul parancsmagot egy modul, amely PowerShellGet segítségével telepített eltávolítása.
Ha bármely más modul attól függ, hogy a modul, amely a törölni kívánt, PowerShellGet hibát jelez.
```powershell
Get-InstalledModule -Name RequiredModule1 | Uninstall-Module

PackageManagement\Uninstall-Package : The module 'RequiredModule1' of version '2.5' in module base folder 'C:\Program Files\WindowsPowerShell\Modules\RequiredModule1\2.5' cannot be uninstalled, because one or more other modules 'ModuleWithDependencies2' are dependent on this module. Uninstall the modules that depend on this module before uninstalling module 'RequiredModule1'.
At C:\Program Files\WindowsPowerShell\Modules\PowerShellGet\PSGet.psm1:1303 char:25
+ ... $null = PackageManagement\\Uninstall-Package @PSBoundParameters
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo : InvalidOperation: (Microsoft.Power...ninstallPackage:UninstallPackage) [Uninstall-Package], Exception
+ FullyQualifiedErrorId : UnableToUninstallAsOtherModulesNeedThisModule,Uninstall-Package,Microsoft.PowerShell.PackageManagement.Cmdlets.UninstallPackage
```

## <a name="save-module-cmdlet"></a>Mentés-modul a parancsmag
```powershell
Save-Module -Repository MSPSGallery -Name ModuleWithDependencies2 -Path C:\MySavedModuleLocation
dir C:\MySavedModuleLocation

Directory: C:\MySavedModuleLocation

Mode LastWriteTime Length Name
---- ------------- ------ ----
d----- 4/21/2015 5:40 PM ModuleWithDependencies2
d----- 4/21/2015 5:40 PM NestedRequiredModule1
d----- 4/21/2015 5:40 PM NestedRequiredModule2
d----- 4/21/2015 5:40 PM NestedRequiredModule3
d----- 4/21/2015 5:40 PM RequiredModule1
d----- 4/21/2015 5:40 PM RequiredModule2
d----- 4/21/2015 5:40 PM RequiredModule3
```

## <a name="update-modulemanifest-cmdlet"></a>Frissítés-ModuleManifest parancsmag
Ez a parancsmag bemeneti tulajdonság értékekkel jegyzékfájl frissítése érdekében használatos. Minden teszt-ModuleManifest hajtja paraméterek vesz igénybe.

Azt figyelje meg, hogy a modul szerzők nagy szeretné adja meg "\*" FunctionsToExport, CmdletsToExport, például az exportált értékek stb. Modul közzététele PowerShell gyűjteményébe, során nem meghatározott funkciók és parancsok lesz nem kitöltve megfelelően alakzatot a gyűjteményben. Ezért javasoljuk, hogy modul szerzők frissítése megfelelő értékekkel a jegyzékfájlban.

Ha tulajdonságok exportált modulok, a frissítés-ModuleManifest tölti a megadott jegyzékfájl exportált funkciók, a parancsmagok, a változók stb adataival:
```powershell
Get-Content -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1"
@{
# Script module or binary module file associated with this manifest.
# RootModule = ''
# Version number of this module.
ModuleVersion = '2.5'
# ID used to uniquely identify this module
GUID = '610e5c5b-dc42-4eaa-8511-ebfb44066d5e'

#(Other properties removed here for Simplicity…)

# Functions to export from this module
FunctionsToExport = '*'
# Cmdlets to export from this module
CmdletsToExport = '*'
# Variables to export from this module
VariablesToExport = '*'
# Aliases to export from this module
AliasesToExport = '*'
}
```

Miután a frissítés-ModuleManifest:
```powershell
Update-ModuleManifest -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1"
Get-Content -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1"
#
# Module manifest for module 'NewManifest'
#
# Generated by: author name
#
# Generated on: 11/13/2015
#
@{
# Script module or binary module file associated with this manifest.
# RootModule = ''
# Version number of this module.
ModuleVersion = '2.5'
# ID used to uniquely identify this module
GUID = '610e5c5b-dc42-4eaa-8511-ebfb44066d5e'
# Functions to export from this module
FunctionsToExport = 'Get-FooFn Get-FooWF'
# Cmdlets to export from this module
CmdletsToExport = 'Test-PSGetTestCmdlet'
}
```

Minden modul vannak társítva metaadatmezőket. Metaadatok helyes megjelenítéséhez a PowrShell gyűjteménye, a frissítés-ModuleManifest segítségével feltöltése PrivateData a mezőket.
```powershell
Update-ModuleManifest -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1" -Tags "Tag1" -LicenseUri "http://license.com" -ProjectUri "http://project.com" -IconUri "http://icon.com" -ReleaseNotes "Test module"
```
A jegyzékfájl sablonból PrivateData hashtable tulajdonságai a következők:
```powershell
# Private data to pass to the module specified in RootModule/ModuleToProcess. This may also contain a PSData hashtable with additional module metadata used by PowerShell.
PrivateData = @{
    PSData = @{
        # Tags applied to this module. These help with module discovery in online galleries.
        # Tags = @()

        # A URL to the license for this module.
        # LicenseUri = ''

        # A URL to the main website for this project.
        # ProjectUri = ''

        # A URL to an icon representing this module.
        # IconUri = ''

        # ReleaseNotes of this module
        # ReleaseNotes = ''

        # External dependent modules of this module
        # ExternalModuleDependencies = ''
    } # End of PSData hashtable
} # End of PrivateData hashtable
```
***Megjegyzés:*** DscResourcesToExport csak a legújabb PowerShell 5.0-s verziója támogatott. Jelenleg nem lehet a mező frissítéséhez, ha a korábbi PowerShell-verziót futtat.