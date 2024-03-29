---
ms.date: 06/12/2017
author: JKeithB
ms.topic: reference
keywords: WMF, powershell, beállítás
ms.openlocfilehash: a83c6a75080d7671bbddf265b68c8b3a8d089335
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/09/2018
---
# <a name="powershell-script-discovery-install-and-management-with-powershellget"></a>PowerShell-parancsfájl felderítési, telepítés és kezelés PowerShellGet az

A WMF 5.0 RTM verzióban PowerShellGet kapott hozzá megosztása szolgáltatás PowerShell parancsfájlt.
A következő új parancsmagok a PowerShellGet modul támogatásához a PowerShell-parancsfájlok hozzáadódnak.
```powershell
PS C:\\windows\\system32&gt; Get-Command \*script\* -Module PowerShellGet | Sort-Object -Property Noun, Verb
CommandType Name Version Source
----------- ---- ------- ------
Function Get-InstalledScript 1.0.0.1 PowerShellGet
Function Find-Script 1.0.0.1 PowerShellGet
Function Install-Script 1.0.0.1 PowerShellGet
Function Publish-Script 1.0.0.1 PowerShellGet
Function Save-Script 1.0.0.1 PowerShellGet
Function Uninstall-Script 1.0.0.1 PowerShellGet
Function Update-Script 1.0.0.1 PowerShellGet
Function New-ScriptFileInfo 1.0.0.1 PowerShellGet
Function Test-ScriptFileInfo 1.0.0.1 PowerShellGet
Function Update-ScriptFileInfo 1.0.0.1 PowerShellGet
-   **Find-Script** cmdlet lets you to discover the script files with different search criteria like name, tag, filter, command name, version range, exact version, all versions, including its dependencies and from specific or all registered repositories.
-   **Save-Script** cmdlet lets you to review the script file by saving it to a specified location.
-   **Install-Script** cmdlet lets you to install a specific script file along with its dependencies to the specified scope. By default, scripts are installed to the AllUsers scope.
-   **Update-Script** cmdlet lets you to do in-place update of the script files which were installed using Install-Script cmdlet.
-   **Get-InstalledScript** cmdlet lets you to get the list of script files which were installed using Install-Script cmdlet.
-   **Uninstall-Script** cmdlet lets you to uninstall the installed script files.
-   **Publish-Script** cmdlet lets you to publish your script file with valid metadata like Version, Guid, Author, and Description, etc.
-   **New-ScriptFileInfo** cmdlet lets you to create a new script file with metadata like Version, Guid, Author, and Description, etc.
-   **Update-ScriptFileInfo** cmdlet lets you to update the existing script file metadata.
-   **Test-ScriptFileInfo** cmdlet lets you to validate and get the script file metadata.
```

Parancsfájl megosztási parancsmagok szintaxisa:
```powershell
**New-ScriptFileInfo** \[-Path\] &lt;string&gt; -Description &lt;string&gt; \[-Version &lt;version&gt;\] \[-Author &lt;string&gt;\] \[-Guid &lt;guid&gt;\] \[-CompanyName &lt;string&gt;\] \[-Copyright &lt;string&gt;\] \[-RequiredModules &lt;Object\[\]&gt;\] \[-ExternalModuleDependencies &lt;string\[\]&gt;\] \[-RequiredScripts &lt;string\[\]&gt;\] \[-ExternalScriptDependencies &lt;string\[\]&gt;\] \[-Tags &lt;string\[\]&gt;\] \[-ProjectUri &lt;uri&gt;\] \[-LicenseUri &lt;uri&gt;\] \[-IconUri &lt;uri&gt;\] \[-ReleaseNotes &lt;string\[\]&gt;\] \[-PassThru\] \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Test-ScriptFileInfo** \[-Path\] &lt;string&gt; \[&lt;CommonParameters&gt;\]

**Test-ScriptFileInfo** -LiteralPath &lt;string&gt; \[&lt;CommonParameters&gt;\]

**Update-ScriptFileInfo** \[-Path\] &lt;string&gt; \[-Version &lt;version&gt;\] \[-Author &lt;string&gt;\] \[-Guid &lt;guid&gt;\] \[-Description &lt;string&gt;\] \[-CompanyName &lt;string&gt;\] \[-Copyright &lt;string&gt;\] \[-RequiredModules &lt;Object\[\]&gt;\] \[-ExternalModuleDependencies &lt;string\[\]&gt;\] \[-RequiredScripts &lt;string\[\]&gt;\] \[-ExternalScriptDependencies &lt;string\[\]&gt;\] \[-Tags &lt;string\[\]&gt;\] \[-ProjectUri &lt;uri&gt;\] \[-LicenseUri &lt;uri&gt;\] \[-IconUri &lt;uri&gt;\] \[-ReleaseNotes &lt;string\[\]&gt;\] \[-PassThru\] \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Update-ScriptFileInfo** \[-LiteralPath\] &lt;string&gt; \[-Version &lt;version&gt;\] \[-Author &lt;string&gt;\] \[-Guid &lt;guid&gt;\] \[-Description &lt;string&gt;\] \[-CompanyName &lt;string&gt;\] \[-Copyright &lt;string&gt;\] \[-RequiredModules &lt;Object\[\]&gt;\] \[-ExternalModuleDependencies &lt;string\[\]&gt;\] \[-RequiredScripts &lt;string\[\]&gt;\] \[-ExternalScriptDependencies &lt;string\[\]&gt;\] \[-Tags &lt;string\[\]&gt;\] \[-ProjectUri &lt;uri&gt;\] \[-LicenseUri &lt;uri&gt;\] \[-IconUri &lt;uri&gt;\] \[-ReleaseNotes &lt;string\[\]&gt;\] \[-PassThru\] \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Find-Script** \[\[-Name\] &lt;string\[\]&gt;\] \[-MinimumVersion &lt;version&gt;\] \[-MaximumVersion &lt;version&gt;\] \[-RequiredVersion &lt;version&gt;\] \[-AllVersions\] \[-IncludeDependencies\] \[-Filter &lt;string&gt;\] \[-Tag &lt;string\[\]&gt;\] \[-Includes &lt;string\[\]&gt;\] \[-Command &lt;string\[\]&gt;\] \[-Repository &lt;string\[\]&gt;\] \[&lt;CommonParameters&gt;\]

**Install-Script** \[-Name\] &lt;string\[\]&gt; \[-MinimumVersion &lt;version&gt;\] \[-MaximumVersion &lt;version&gt;\] \[-RequiredVersion &lt;version&gt;\] \[-Repository &lt;string\[\]&gt;\] \[-Scope &lt;string&gt;\] \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Install-Script** \[-InputObject\] &lt;psobject\[\]&gt; \[-Scope &lt;string&gt;\] \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Update-Script** \[\[-Name\] &lt;string\[\]&gt;\] \[-RequiredVersion &lt;version&gt;\] \[-MaximumVersion &lt;version&gt;\] \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Get-InstalledScript** \[\[-Name\] &lt;string\[\]&gt;\] \[-MinimumVersion &lt;version&gt;\] \[-RequiredVersion &lt;version&gt;\] \[-MaximumVersion &lt;version&gt;\] \[&lt;CommonParameters&gt;\]

**Uninstall-Script** \[-Name\] &lt;string\[\]&gt; \[-MinimumVersion &lt;version&gt;\] \[-RequiredVersion &lt;version&gt;\] \[-MaximumVersion &lt;version&gt;\] \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Uninstall-Script** \[-InputObject\] &lt;psobject\[\]&gt; \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Save-Script** \[-Name\] &lt;string\[\]&gt; -Path &lt;string&gt; \[-MinimumVersion &lt;version&gt;\] \[-MaximumVersion &lt;version&gt;\] \[-RequiredVersion &lt;version&gt;\] \[-Repository &lt;string\[\]&gt;\] \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Save-Script** \[-Name\] &lt;string\[\]&gt; -LiteralPath &lt;string&gt; \[-MinimumVersion &lt;version&gt;\] \[-MaximumVersion &lt;version&gt;\] \[-RequiredVersion &lt;version&gt;\] \[-Repository &lt;string\[\]&gt;\] \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Save-Script** \[-InputObject\] &lt;psobject\[\]&gt; -LiteralPath &lt;string&gt; \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Save-Script** \[-InputObject\] &lt;psobject\[\]&gt; -Path &lt;string&gt; \[-Force\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Publish-Script** -Path &lt;string&gt; \[-NuGetApiKey &lt;string&gt;\] \[-Repository &lt;string&gt;\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]

**Publish-Script** -LiteralPath &lt;string&gt; \[-NuGetApiKey &lt;string&gt;\] \[-Repository &lt;string&gt;\] \[-WhatIf\] \[-Confirm\] \[&lt;CommonParameters&gt;\]
```