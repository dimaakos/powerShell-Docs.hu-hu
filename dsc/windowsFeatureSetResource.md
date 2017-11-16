---
ms.date: 2017-06-12
author: eslesar
ms.topic: conceptual
keywords: "a DSC, a powershell, a konfiguráció, a beállítása"
title: "A DSC WindowsFeatureSet erőforrás"
ms.openlocfilehash: 3cdabc36ef35c2bf912ac54393fe40024a8e8bc0
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/12/2017
---
# <a name="dsc-windowsfeatureset-resource"></a>A DSC WindowsFeatureSet erőforrás

> Vonatkozik: A Windows PowerShell 5.0

A **WindowsFeatureSet** erőforrás a Windows PowerShell szükséges konfiguráló (DSC) Győződjön meg arról, hogy szerepkörök és szolgáltatások hozzáadása vagy eltávolítása egy célcsomóponttal mechanizmust biztosít.
Az erőforrás egy [összetett erőforrás](authoringResourceComposite.md) , amely meghívja a [WindowsFeature erőforrás](windowsfeatureResource.md) az egyes szolgáltatásokhoz megadott a `Name` tulajdonság.

Ehhez az erőforráshoz akkor használja, ha a Windows-szolgáltatások számos állapot konfigurálni szeretné.

## <a name="syntax"></a>Szintaxis

```
WindowsFeatureSet [string] #ResourceName
{
    Name = [string[]] 
    [ Ensure = [string] { Absent | Present }  ]
    [ Source = [string] ]
    [ IncludeAllSubFeature = [bool] ]
    [ Credential = [PSCredential] ]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
    
}
```

## <a name="properties"></a>Tulajdonságok

|  Tulajdonság  |  Leírás   | 
|---|---| 
| Név| A szerepkörök vagy szolgáltatások biztosítása kívánt nevét hozzáadásakor vagy eltávolításakor. Ez megegyezik a **neve** tulajdonsága a [Get-WindowsFeature](https://technet.microsoft.com/en-us/library/jj205469.aspx) parancsmag, és nem a megjelenített név a szerepkörök vagy szolgáltatások.| 
| hitelesítő adatok| A hitelesítő adatok hozzáadása vagy eltávolítása a szerepkörök vagy szolgáltatások használatára.| 
| Győződjön meg arról| Azt jelzi, hogy a szerepkörök vagy szolgáltatások kerülnek. Ezzel biztosíthatja, hogy a szerepkörök vagy szolgáltatások hozzáadott, állítsa be ezt a tulajdonságot "Elérhető" Győződjön meg arról, hogy eltávolítja a szerepkörök vagy szolgáltatások, a tulajdonság értéke "Hiányzik".| 
| IncludeAllSubFeature| Ez a tulajdonság beállítása **$true** tartalmazza a Funkciók, adja meg az összes szükséges alfunkció a **neve** tulajdonság.| 
| LogPath| A naplófájl elérési útja a kívánt való bejelentkezéshez a műveletet az erőforrás-szolgáltató.| 
| dependsOn| Azt jelzi, hogy egy másik erőforrás konfigurációjának kell futtatni, mielőtt ehhez az erőforráshoz van konfigurálva. Például, ha az erőforrás-konfiguráció azonosítója blokk futtatni kívánt parancsfájl első az __ResourceName__ és annak típusa __ResourceType__, az e tulajdonság használatával szintaxisa a következő `DependsOn = "[ResourceType]ResourceName"`.| 
| Forrás| Azt jelzi, használható a telepítéshez, a forrás-fájl helyét, ha szükséges.| 

## <a name="example"></a>Példa

A következő konfigurációs biztosítja, hogy a **webkiszolgáló** (IIS) és **SMTP-kiszolgáló** funkciók és összes részszolgáltatása, is települnek.

```powershell
configuration FeatureSetTest
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    Node localhost
    {

        WindowsFeatureSet WindowsFeatureSetExample
        {
            Name                    = @("SMTP-Server", "Web-Server")
            Ensure                  = 'Present'
            IncludeAllSubFeature    = $true
        } 
    }
}
```
