---
ms.date: 06/09/2017
schema: 2.0.0
keywords: PowerShell
title: Licencfeltételek elfogadását igénylő modulok
ms.openlocfilehash: fe197ea271e18580a221ad4d5245b685bd81775b
ms.sourcegitcommit: e9ad4d85fd7eb72fb5bc37f6ca3ae1282ae3c6d7
ms.contentlocale: hu-HU
ms.lasthandoff: 05/10/2018
---
# <a name="modules-requiring-license-acceptance"></a><span data-ttu-id="6152a-103">Licencfeltételek elfogadását igénylő modulok</span><span class="sxs-lookup"><span data-stu-id="6152a-103">Modules Requiring License Acceptance</span></span>

## <a name="synopsis"></a><span data-ttu-id="6152a-104">ÖSSZEGZÉST</span><span class="sxs-lookup"><span data-stu-id="6152a-104">SYNOPSIS</span></span>

<span data-ttu-id="6152a-105">Az egyes modul közzétevők jogi részlegek szükséges, hogy az ügyfelek kell explicit módon fogadnia a licencfeltételeket a modul PowerShell-galériából telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="6152a-105">Legal departments for some module publishers require that customers must explicitly accept the license before installing their module from PowerShell Gallery.</span></span> <span data-ttu-id="6152a-106">Ha a felhasználó telepíti, frissítéseket, vagy egy modul segítségével PowerShellGet, közvetlenül vagy egy másik elem függőségei menti, és a modult a felhasználónak kell vállalja, hogy a licenc, a felhasználó jeleznie kell, a licenc elfogadja őket, vagy a művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="6152a-106">If a user installs, updates, or saves a module using PowerShellGet, whether directly or as a dependency for another item, and that module requires the user to agree to a license, the user must indicate they accept the license or the operation fails.</span></span>

## <a name="publish-requirements-for-modules"></a><span data-ttu-id="6152a-107">Modulok követelményei közzététele</span><span class="sxs-lookup"><span data-stu-id="6152a-107">Publish Requirements for Modules</span></span>

<span data-ttu-id="6152a-108">Modulok, amelyeket szeretne felhasználótól megkövetelje-licenc szükséges következő követelményeknek kell teljesítéséhez:</span><span class="sxs-lookup"><span data-stu-id="6152a-108">Modules that would like to require users to accept license should fulfill following requirements:</span></span>

- <span data-ttu-id="6152a-109">Moduljegyzék PSData szakasza tartalmaznia kell RequireLicenseAcceptance = $True.</span><span class="sxs-lookup"><span data-stu-id="6152a-109">PSData section of module manifest should include RequireLicenseAcceptance = $True.</span></span>
- <span data-ttu-id="6152a-110">Modul gyökérkönyvtárában license.txt fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6152a-110">Module should contain license.txt file in root directory.</span></span>
- <span data-ttu-id="6152a-111">Moduljegyzék licenc Uri kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="6152a-111">Module manifest should contain License Uri.</span></span>
- <span data-ttu-id="6152a-112">A modul PowerShellGet formátum 2.0-s verziójával és a fenti közzé kell tenni.</span><span class="sxs-lookup"><span data-stu-id="6152a-112">Module should be published with PowerShellGet Format Version 2.0 and above.</span></span>

## <a name="impact-on-installsaveupdate-module"></a><span data-ttu-id="6152a-113">Telepítés/Save/frissítés-modul gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="6152a-113">Impact on Install/Save/Update-Module</span></span>

- <span data-ttu-id="6152a-114">Telepítés/Save/frissítési parancsmagok fogja támogatni egy új paraméter, amely fog viselkedni, mintha a felhasználói licenc látott – AcceptLicense.</span><span class="sxs-lookup"><span data-stu-id="6152a-114">Install/Save/Update cmdlets will support a new parameter –AcceptLicense that will behave as though the user saw the license.</span></span>
- <span data-ttu-id="6152a-115">Ha RequiredLicenseAcceptance tulajdonság igaz értékű, és – AcceptLicense nincs megadva, a felhasználó a license.txt, és megjelenítendő jelenik meg: &quot;elfogadja ezeket a licencfeltételeket (Yes/No/YesToAll/NoToAll)&quot;.</span><span class="sxs-lookup"><span data-stu-id="6152a-115">If RequiredLicenseAcceptance is True and –AcceptLicense is not specified, the user will be shown the license.txt, and prompted with: &quot;Do you accept these license terms (Yes/No/YesToAll/NoToAll)&quot;.</span></span>
  - <span data-ttu-id="6152a-116">Ha elfogadja a licencfeltételeket</span><span class="sxs-lookup"><span data-stu-id="6152a-116">If the license is accepted</span></span>
    - <span data-ttu-id="6152a-117">**Mentés-modul:** fogja másolni. a modul a felhasználó&#39;s rendszer</span><span class="sxs-lookup"><span data-stu-id="6152a-117">**Save-Module:** the module will be copied to the user&#39;s system</span></span>
    - <span data-ttu-id="6152a-118">**Install-modul:** fogja másolni. a modul a felhasználó&#39;s rendszer a megfelelő mappába (hatókör alapján)</span><span class="sxs-lookup"><span data-stu-id="6152a-118">**Install-Module:** the module will be copied to the user&#39;s system to the proper folder (based on scope)</span></span>
    - <span data-ttu-id="6152a-119">**Frissítés-modul:** a modul frissülni fog.</span><span class="sxs-lookup"><span data-stu-id="6152a-119">**Update-Module:** the module will be updated.</span></span>
  - <span data-ttu-id="6152a-120">Ha a rendszer elutasította, a licenc.</span><span class="sxs-lookup"><span data-stu-id="6152a-120">If the license is declined.</span></span>
    - <span data-ttu-id="6152a-121">A művelet megszakad.</span><span class="sxs-lookup"><span data-stu-id="6152a-121">Operation will be cancelled.</span></span>
- <span data-ttu-id="6152a-122">Az összes parancsmag rákeres a metaadatok (requireLicenseAcceptance és adatformátum-verzió), amely szerint a licencszerződés elfogadását szükség</span><span class="sxs-lookup"><span data-stu-id="6152a-122">All cmdlets will check for the metadata(requireLicenseAcceptance and Format Version) that says a license acceptance is required</span></span>
  - <span data-ttu-id="6152a-123">Formátum az ügyfél verziója régebbi, mint 2,0, a művelet sikertelen, és kérje meg, hogy frissítse az ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="6152a-123">If format version of client is older than 2.0, operation will fail and ask the user to update the client.</span></span>
  - <span data-ttu-id="6152a-124">Ha a modul lett közzétéve, hogy a régebbi, mint a 2.0-s verziójú adatformátum, requireLicenseAcceptance jelző figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="6152a-124">If module was published with format version older than 2.0, requireLicenseAcceptance flag will be ignored.</span></span>


 ## <a name="module-dependencies"></a><span data-ttu-id="6152a-125">A modul függőségek</span><span class="sxs-lookup"><span data-stu-id="6152a-125">Module Dependencies</span></span>
- <span data-ttu-id="6152a-126">Telepítés/Save/frissítése közben. a művelet, ha egy függő modul (valami mással függ a modul) igényel licenc elfogadása, akkor a licenc elfogadása viselkedését (fent) lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="6152a-126">During Install/Save/Update operation, if a dependent module(something else depends on the module) requires license acceptance, then the license acceptance behavior (above) will be required.</span></span>
- <span data-ttu-id="6152a-127">A modul verzió már szerepel a helyi katalógus telepítését a rendszer, ha azt szeretné szülőkönyvtár a licenc.</span><span class="sxs-lookup"><span data-stu-id="6152a-127">If the module version is already listed in the local catalog as being installed on the system, we would bypass the license checking.</span></span>
- <span data-ttu-id="6152a-128">Install-mentés-vagy frissítési művelet során egy függő modul licencre van szükség, és a licencszerződés elfogadását nem történik meg, ha a művelet meghiúsul, és hajtsa végre a szokásos folyamatok nem sikerült telepíteni vagy Mentés vagy frissíteni a cikkhez.</span><span class="sxs-lookup"><span data-stu-id="6152a-128">During Install/Save/Update operation, if a dependent module requires a license, and the license acceptance does not occur, the operation will fail and follow normal processes for the item failed to install/save/update.</span></span>

 ## <a name="impact-on--force"></a><span data-ttu-id="6152a-129">A - Force gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="6152a-129">Impact on -Force</span></span>

<span data-ttu-id="6152a-130">Megadásával – kényszerített nincs elegendő fogadnia a licencfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="6152a-130">Specifying –Force is NOT sufficient to accept a license.</span></span> <span data-ttu-id="6152a-131">– AcceptLicense jogosultsága a telepítéséhez szükség.</span><span class="sxs-lookup"><span data-stu-id="6152a-131">–AcceptLicense is required for permission to install.</span></span> <span data-ttu-id="6152a-132">Ha – Force meg van adva, RequiredLicenseAcceptance tulajdonság igaz értékű, és – AcceptLicense nincs megadva, a művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="6152a-132">If –Force is specified, RequiredLicenseAcceptance is True, and –AcceptLicense is NOT specified, the operation will fail.</span></span>

## <a name="examples"></a><span data-ttu-id="6152a-133">PÉLDÁK</span><span class="sxs-lookup"><span data-stu-id="6152a-133">EXAMPLES</span></span>

### <a name="example-1-update-module-manifest-to-require-license-acceptance"></a><span data-ttu-id="6152a-134">1. példa: Frissítés modul Alkalmazásjegyzéket licencszerződés elfogadására van szükség</span><span class="sxs-lookup"><span data-stu-id="6152a-134">Example 1: Update Module Manifest to require license acceptance</span></span>

```PowerShell
PS> Update-ModuleManifest -Path C:\modulemanifest.psd1 -RequireLicenseAcceptance

PrivateData = @{

    PSData = @{
        # Flag to indicate whether the module requires explicit user acceptance
        RequireLicenseAcceptance = $true
    } # End of PSData hashtable

 } # End of PrivateData hashtable
```

<span data-ttu-id="6152a-135">Ez a parancs frissíti a jegyzékfájlt és a RequireLicenseAcceptance jelző IGAZ értékűre állítja be.</span><span class="sxs-lookup"><span data-stu-id="6152a-135">This command updates the manifest file and sets the RequireLicenseAcceptance flag to true.</span></span>

### <a name="example-2-install-module-requiring-license-acceptance"></a><span data-ttu-id="6152a-136">2. példa: Telepítés modul megkövetelését licenc elfogadása</span><span class="sxs-lookup"><span data-stu-id="6152a-136">Example 2: Install Module requiring license acceptance</span></span>

```PowerShell
PS> Install-Module -Name ModuleRequireLicenseAcceptance

License Acceptance

License 2.0
Copyright (c) 2016 PowerShell Team
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software.

Do you accept the license terms for module 'ModuleRequireLicenseAcceptance'.
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):

```

<span data-ttu-id="6152a-137">Ez a parancs megjeleníti a licenc license.txt fájlból, és megkérdezi a felhasználót, hogy elfogadja a licencfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="6152a-137">This command shows the license from license.txt file and prompts the user to accept the license.</span></span>

### <a name="example-3-install-module-requiring-license-acceptance-with--acceptlicense"></a><span data-ttu-id="6152a-138">3. példa: Telepítés modul megkövetelését licenc elfogadása - AcceptLicense</span><span class="sxs-lookup"><span data-stu-id="6152a-138">Example 3: Install Module requiring license acceptance with -AcceptLicense</span></span>

```PowerShell
PS> Install-Module -Name ModuleRequireLicenseAcceptance -AcceptLicense
```

<span data-ttu-id="6152a-139">Bármely prompt licenc elfogadása nélkül a modul telepítve van.</span><span class="sxs-lookup"><span data-stu-id="6152a-139">Module is installed without any prompt to accept license.</span></span>

### <a name="example-4-install-module-requiring-license-acceptance-with--force"></a><span data-ttu-id="6152a-140">4. példa: Telepítés modul megkövetelését licenc elfogadás és a - Force</span><span class="sxs-lookup"><span data-stu-id="6152a-140">Example 4: Install Module requiring license acceptance with -Force</span></span>

```PowerShell
PS> Install-Module -Name ModuleRequireLicenseAcceptance -Force
PackageManagement\Install-Package : License Acceptance is required for module 'ModuleRequireLicenseAcceptance'. Please specify '-AcceptLicense' to perform this operation.
At C:\Program Files\WindowsPowerShell\Modules\PowerShellGet\1.1.3.3\PSModule.psm1:1837 char:21
+ ...          $null = PackageManagement\Install-Package @PSBoundParameters
+                      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (Microsoft.Power....InstallPackage:InstallPackage) [Install-Package], E
   xception
    + FullyQualifiedErrorId : ForceAcceptLicense,Install-PackageUtility,Microsoft.PowerShell.PackageManagement.Cmdlets
   .InstallPackage
```

### <a name="example-5-install-module-with-dependencies-requiring-license-acceptance"></a><span data-ttu-id="6152a-141">5. példa: Telepítés modul licenc elfogadása igénylő függőségekkel rendelkező</span><span class="sxs-lookup"><span data-stu-id="6152a-141">Example 5: Install Module with dependencies requiring license acceptance</span></span>

<span data-ttu-id="6152a-142">A modul "ModuleWithDependency" modul "ModuleRequireLicenseAcceptance" függ.</span><span class="sxs-lookup"><span data-stu-id="6152a-142">Module 'ModuleWithDependency' depends on module 'ModuleRequireLicenseAcceptance'.</span></span> <span data-ttu-id="6152a-143">Licenc elfogadása felhasználótól.</span><span class="sxs-lookup"><span data-stu-id="6152a-143">User is prompted to Accept License.</span></span>

```PowerShell
PS> Install-Module -Name ModuleWithDependency

License Acceptance
MIT License 2.0
Copyright (c) 2016 PowerShell Team
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software.

Do you accept the license terms for module 'ModuleRequireLicenseAcceptance'.
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
```

### <a name="example-6-install-module-with-dependencies-requiring-license-acceptance-and--acceptlicense"></a><span data-ttu-id="6152a-144">6. példa: Telepítés modul licenc elfogadása és - AcceptLicense függőségekkel rendelkező</span><span class="sxs-lookup"><span data-stu-id="6152a-144">Example 6: Install Module with dependencies requiring license acceptance and -AcceptLicense</span></span>

<span data-ttu-id="6152a-145">A modul "ModuleWithDependency" modul "ModuleRequireLicenseAcceptance" függ.</span><span class="sxs-lookup"><span data-stu-id="6152a-145">Module 'ModuleWithDependency' depends on module 'ModuleRequireLicenseAcceptance'.</span></span> <span data-ttu-id="6152a-146">Nem felhasználótól fogadnia a licencfeltételeket, mert - AcceptLicense sincs megadva.</span><span class="sxs-lookup"><span data-stu-id="6152a-146">User is not prompted to accept license as -AcceptLicense is specified.</span></span>

```PowerShell
PS>  Install-Module -Name ModuleWithDependency -AcceptLicense
```

### <a name="example-7-install-module-requiring-license-acceptance-on-a-client-older-than-psgetformatversion-20"></a><span data-ttu-id="6152a-147">7. példa: Egy régebbi, mint PSGetFormatVersion 2.0 ügyfél licencszerződés elfogadására igénylő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="6152a-147">Example 7: Install module requiring license acceptance on a client older than PSGetFormatVersion 2.0</span></span>

```PowerShell
PS C:\windows\system32> Install-Module -Name ModuleRequireLicenseAcceptance

WARNING: The specified module 'ModuleRequireLicenseAcceptance' with PowerShellGetFormatVersion '2.0' is not supported by the current version of PowerShellGet. Get the latest version of the PowerShellGet module to install this module, 'ModuleRequireLicenseAcceptance'.

```

### <a name="example-8-save-module-requiring-license-acceptance"></a><span data-ttu-id="6152a-148">8. példa: Mentés modul igénylő licenc elfogadása</span><span class="sxs-lookup"><span data-stu-id="6152a-148">Example 8: Save Module requiring license acceptance</span></span>

```PowerShell
PS> Save-Module -Name ModuleRequireLicenseAcceptance -Path C:\Saved

License Acceptance

License 2.0
Copyright (c) 2016 PowerShell Team
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software.

Do you accept the license terms for module 'ModuleRequireLicenseAcceptance'.
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
```

<span data-ttu-id="6152a-149">Ez a parancs megjeleníti a licenc license.txt fájlból, és megkérdezi a felhasználót, hogy elfogadja a licencfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="6152a-149">This command shows the license from license.txt file and prompts the user to accept the license.</span></span>

### <a name="example-9-save-module-requiring-license-acceptance-with--acceptlicense"></a><span data-ttu-id="6152a-150">9. példa: Mentés licenc elfogadás és - AcceptLicense igénylő modul</span><span class="sxs-lookup"><span data-stu-id="6152a-150">Example 9: Save Module requiring license acceptance with -AcceptLicense</span></span>

```PowerShell
PS> Save-Module -Name ModuleRequireLicenseAcceptance -AcceptLicense -Path C:\Saved
```

<span data-ttu-id="6152a-151">A modul bármely prompt licenc elfogadása nélkül menti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="6152a-151">Module is saved without any prompt to accept license.</span></span>

### <a name="example-10-update-module-requiring-license-acceptance"></a><span data-ttu-id="6152a-152">10. példa: Frissítés modul megkövetelését licenc elfogadása</span><span class="sxs-lookup"><span data-stu-id="6152a-152">Example 10: Update Module requiring license acceptance</span></span>

```PowerShell
PS> Update-Module -Name ModuleRequireLicenseAcceptance

License Acceptance

License 2.0
Copyright (c) 2016 PowerShell Team
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software.

Do you accept the license terms for module 'ModuleRequireLicenseAcceptance'.
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
```

<span data-ttu-id="6152a-153">Ez a parancs megjeleníti a licenc license.txt fájlból, és megkérdezi a felhasználót, hogy elfogadja a licencfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="6152a-153">This command shows the license from license.txt file and prompts the user to accept the license.</span></span>

### <a name="example-11-update-module-requiring-license-acceptance-with--acceptlicense"></a><span data-ttu-id="6152a-154">11. példa: Frissítés modul megkövetelését licenc elfogadása - AcceptLicense</span><span class="sxs-lookup"><span data-stu-id="6152a-154">Example 11: Update Module requiring license acceptance with -AcceptLicense</span></span>

```PowerShell
PS> Update-Module -Name ModuleRequireLicenseAcceptance -AcceptLicense
```

<span data-ttu-id="6152a-155">Modul frissül bármely prompt licenc elfogadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="6152a-155">Module is updated without any prompt to accept license.</span></span>

## <a name="more-details"></a><span data-ttu-id="6152a-156">További részletekért</span><span class="sxs-lookup"><span data-stu-id="6152a-156">More details</span></span>

### <a name="require-license-acceptance-for-scriptsscript-license-acceptancemd"></a>[<span data-ttu-id="6152a-157">Licencfeltételek elfogadásának megkövetelése a parancsfájlokhoz</span><span class="sxs-lookup"><span data-stu-id="6152a-157">Require License Acceptance for Scripts</span></span>](./script-license-acceptance.md)

### <a name="require-license-acceptance-support-on-powershellgalleryhow-toworking-with-itemsitems-that-require-license-acceptancemd"></a>[<span data-ttu-id="6152a-158">A PowerShellGallery licenc elfogadása támogatásra van szüksége</span><span class="sxs-lookup"><span data-stu-id="6152a-158">Require License Acceptance support on PowerShellGallery</span></span>](../how-to/working-with-items/items-that-require-license-acceptance.md)

### <a name="require-license-acceptance-on-deploy-to-azure-automationhow-toworking-with-itemsdeploy-to-azure-automationmd"></a>[<span data-ttu-id="6152a-159">Licencfeltételek elfogadásának kérése az Azure Automation szolgáltatásban való üzembe helyezéshez</span><span class="sxs-lookup"><span data-stu-id="6152a-159">Require License Acceptance on Deploy to Azure Automation</span></span>](../how-to/working-with-items/deploy-to-azure-automation.md)