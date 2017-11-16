---
ms.date: 2017-06-12
contributor: JKeithB
ms.topic: conceptual
keywords: "gyűjtemény, a powershell, a parancsmag, a psgallery"
title: psgallery_pseditions
ms.openlocfilehash: 6634da5c2dadee9c0c6470b3d3e8883e6d02160f
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/12/2017
---
# <a name="items-with-compatible-powershell-editions"></a>Elemek verzióval kompatibilis PowerShell
Az 5.1-es verziótól kezdődően a PowerShell különböző kiadásokban érhető el, amelyek különböző szolgáltatáskészleteket és platformkompatibilitást kínálnak.

- **Desktop kiadás:** A .NET-keretrendszeren alapul, és a Windows teljes erőforrás-igényű kiadásain, például a Server Core és a Windows asztali kiadásain futtatott PowerShell-verziókat célzó szkriptekhez és modulokhoz biztosít kompatibilitást.
- **Core kiadás:** .NET Core-on alapul, és a Windows csökkentett erőforrás-igényű kiadásain, például a Nano Serveren és a Windows IoT kiadásokon futtatott PowerShell-verziókat célzó szkriptekhez és modulokhoz biztosít kompatibilitást.

## <a name="powershell-gallery-extracts-supported-pseditions-metadata-and-allows-you-to-filters-the-items-compatible-for-specific-powershell-editions"></a>PowerShell-galériában bontja ki a támogatott PSEditions metaadatok, és lehetővé teszi a szűrők a cikkek kompatibilis a megadott PowerShell esetén

Ha egy cikk kompatibilis PSEditions megadott, "PowerShell kiadások" részeként jelennek lap az elem megjelenítése, illetve is elemek eredményekben.
![Elem a lap megjelenítése az PSEditions](Images/ItemDisplayPageWithPSEditions.PNG)

## <a name="search-for-items-in-the-gallery-ui-which-works-on-powershellcore"></a>Keresse meg a felhasználói felület, amely működik-e az PowerShellCore gyűjteményben szereplő elemek
Címkék használata: "PSEdition_Desktop" és a címkék: "PSEdition_Core" szűrőkkel PowerShell-galériában elemeket.

### <a name="use-tagspseditioncore-to-search-items-compatible-with-powershell-core-edition"></a>Címkék használata: "PSEdition_Core" kompatibilis PowerShell Core Edition elemek kereséséhez.
![Keresési eredmények Core PSEdition kompatibilis elemek](Images/SearchResultsWithPSEditions.PNG)

### <a name="use-tagspseditiondesktop-to-search-items-compatible-with-powershell-desktop-edition"></a>Címkék használata: "PSEdition_Desktop" kompatibilis PowerShell asztali Edition elemek kereséséhez.
![Az asztali PSEdition kompatibilis elemek keresési eredmények](Images/SearchResultsWithPSEdition_Desktop.PNG)

## <a name="more-details-on-authoring-and-finding-the-items-with-compatible-powershell-editions"></a>További részleteket a jelentéskészítő és -verzióval kompatibilis PowerShell cikkek keresése
### <a name="modules-with-pseditionspsgetmodulemodulewithpseditionsupportmd"></a>[PSEditions rendelkező modulok](../psget/module/modulewithpseditionsupport.md)
### <a name="scripts-with-pseditionspsgetscriptscriptwithpseditionsupportmd"></a>[A PSEditions parancsfájlok](../psget/script/scriptwithpseditionsupport.md)
