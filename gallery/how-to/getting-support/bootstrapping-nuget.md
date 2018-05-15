
---
<span data-ttu-id="037c1-101">MS.Date: 06/12/2017 közreműködői: manikb ms.topic: kulcsszavak hivatkozni: gyűjtemény, a powershell, a parancsmag, a psget cím: NuGet rendszerindítása</span><span class="sxs-lookup"><span data-stu-id="037c1-101">ms.date :  06/12/2017 contributor:  manikb ms.topic:  reference keywords:  gallery,powershell,cmdlet,psget title:  Bootstrapping NuGet</span></span>
---
# <a name="bootstrap-the-nuget-provider-and-nugetexe"></a><span data-ttu-id="037c1-102">A NuGet-szolgáltató és NuGet.exe bootstrap</span><span class="sxs-lookup"><span data-stu-id="037c1-102">Bootstrap the NuGet provider and NuGet.exe</span></span>

<span data-ttu-id="037c1-103">NuGet.exe nem szerepel a legújabb NuGet-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="037c1-103">NuGet.exe is not included in the latest NuGet provider.</span></span>
<span data-ttu-id="037c1-104">A modul vagy a parancsfájl működésére közzétételéhez PowerShellGet a bináris végrehajtható NuGet.exe van szükség.</span><span class="sxs-lookup"><span data-stu-id="037c1-104">For publish operations of either a module or script, PowerShellGet requires the binary executable NuGet.exe.</span></span>
<span data-ttu-id="037c1-105">Csak a NuGet-szolgáltató szükség, minden más műveletek, beleértve a *található*, *telepítése*, *mentése*, és *eltávolítása*.</span><span class="sxs-lookup"><span data-stu-id="037c1-105">Only the NuGet provider is required for all other operations, including *find*, *install*, *save*, and *uninstall*.</span></span>
<span data-ttu-id="037c1-106">PowerShellGet kezeléséhez vagy logikát tartalmaz, a kombinált rendszerindítási a NuGet-szolgáltató és NuGet.exe vagy rendszerindítási csak a NuGet-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="037c1-106">PowerShellGet includes logic to handle either a combined bootstrap of the NuGet provider and NuGet.exe, or bootstrap of only the NuGet provider.</span></span>
<span data-ttu-id="037c1-107">Mindkét esetben csak egyetlen azonnali üzenetben jöjjön létre.</span><span class="sxs-lookup"><span data-stu-id="037c1-107">In either case, only a single prompt message should occur.</span></span>
<span data-ttu-id="037c1-108">Ha a számítógép nem csatlakozik az internethez, a felhasználónak vagy rendszergazdának át kell másolnia a NuGet-szolgáltató és/vagy a NuGet.exe fájl megbízható példánya a leválasztott gép.</span><span class="sxs-lookup"><span data-stu-id="037c1-108">If the machine is not connected to the Internet, the user or an administrator must copy a trusted instance of the NuGet provider and/or the NuGet.exe file to the disconnected machine.</span></span>

><span data-ttu-id="037c1-109">**Megjegyzés:**: 6-os verziótól kezdődően a NuGet-szolgáltató része a PowerShell telepítése.</span><span class="sxs-lookup"><span data-stu-id="037c1-109">**Note**: Starting with version 6, the NuGet provider is included in the installation of PowerShell.</span></span> [http://github.com/powershell/powershell](http://github.com/powershell/powershell)

## <a name="resolving-error-when-the-nuget-provider-has-not-been-installed-on-a-machine-that-is-internet-connected"></a><span data-ttu-id="037c1-110">Kapcsolódó hiba elhárításához, amikor a NuGet-szolgáltató nem telepíthető olyan számítógépen, amelyen Internet</span><span class="sxs-lookup"><span data-stu-id="037c1-110">Resolving error when the NuGet provider has not been installed on a machine that is Internet connected</span></span>

```powershell
PS> Find-Module -Repository PSGallery -Verbose -Name Contoso

NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
'C:\Users\manikb\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running 'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import the NuGet provider
now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): n
Find-Module : NuGet provider is required to interact with NuGet-based repositories. Please ensure that '2.8.5.201' or newer version of NuGet provider is installed.
At line:1 char:1
+ Find-Module -Repository PSGallery -Verbose -Name Contoso
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Find-Module], InvalidOperationException
   + FullyQualifiedErrorId : CouldNotInstallNuGetProvider,Find-Module

PS> Find-Module -Repository PSGallery -Verbose -Name Contoso

NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
'C:\Users\manikb\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running 'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import the NuGet provider
now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet provider.

Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
2.5        Contoso                             Module     PSGallery        Contoso module
```

## <a name="resolving-error-when-the-nuget-provider-is-available-and-nugetexe-is-not-available-during-the-publish-operation-on-a-machine-that-is-internet-connected"></a><span data-ttu-id="037c1-111">Kapcsolódó hiba elhárításához, amikor a NuGet-szolgáltató nem érhető el, és NuGet.exe nem érhető el az olyan gépen, amely internetes közzététel művelet során</span><span class="sxs-lookup"><span data-stu-id="037c1-111">Resolving error when the NuGet provider is available and NuGet.exe is not available during the publish operation on a machine that is Internet connected</span></span>

```powershell
PS> Publish-Module -Name Contoso -Repository PSGallery -Verbose

NuGet.exe is required to continue
PowerShellGet requires NuGet.exe to publish an item to the NuGet-based repositories. NuGet.exe must be available under one of the paths specified in PATH environment variable value. Do you want PowerShellGet to install NuGet.exe now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): N
Publish-Module : NuGet.exe is required to interact with NuGet-based repositories. Please ensure that NuGet.exe is available under one of the paths specified in PATH environment variable value.
At line:1 char:1
+ Publish-Module -Name Contoso -Repository PSGallery -Verbose
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Publish-Module], InvalidOperationException
    + FullyQualifiedErrorId : CouldNotInstallNuGetExe,Publish-Module

PS> Publish-Module -Name Contoso -Repository PSGallery -Verbose

NuGet.exe is required to continue
PowerShellGet requires NuGet.exe to publish an item to the NuGet-based repositories. NuGet.exe must be available under one of the paths specified in PATH environment variable value. Do you want PowerShellGet to install NuGet.exe now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet.exe.
VERBOSE: Successfully published module 'Contoso' to the module publish location 'https://www.powershellgallery.com/api/v2/'. Please allow few minutes for 'Contoso' to show up in the search results.
```

## <a name="resolving-error-when-both-nuget-provider-and-nugetexe-are-not-available-during-the-publish-operation-on-a-machine-that-is-internet-connected"></a><span data-ttu-id="037c1-112">Kapcsolódó hiba elhárításához, amikor NuGet-szolgáltató és NuGet.exe is nem érhető el az olyan gépen, amely internetes közzététel művelet során</span><span class="sxs-lookup"><span data-stu-id="037c1-112">Resolving error when both NuGet provider and NuGet.exe are not available during the publish operation on a machine that is Internet connected</span></span>

```powershell
PS> Publish-Module -Name Contoso -Repository PSGallery -Verbose

NuGet.exe and NuGet provider are required to continue
PowerShellGet requires NuGet.exe and NuGet provider version '2.8.5.201' or newer to interact with the NuGet-based repositories. Do you want PowerShellGet to install both NuGet.exe and NuGet provider now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): N
Publish-Module : PowerShellGet requires NuGet.exe and NuGet provider version '2.8.5.201' or newer to interact with the NuGet-based repositories. Please ensure that '2.8.5.201' or newer version of NuGet provider is installed and NuGet.exe is available under
one of the paths specified in PATH environment variable value.
At line:1 char:1
+ Publish-Module -Name Contoso -Repository PSGallery -Verbose
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Publish-Module], InvalidOperationException
    + FullyQualifiedErrorId : CouldNotInstallNuGetBinaries,Publish-Module

PS> Publish-Module -Name Contoso -Repository PSGallery -Verbose

NuGet.exe and NuGet provider are required to continue
PowerShellGet requires NuGet.exe and NuGet provider version '2.8.5.201' or newer to interact with the NuGet-based repositories. Do you want PowerShellGet to install both NuGet.exe and NuGet provider now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet provider.
VERBOSE: Installing NuGet.exe.
VERBOSE: Successfully published module 'Contoso' to the module publish location 'https://www.powershellgallery.com/api/v2/'. Please allow few minutes for 'Contoso' to show up in the search results.
```

## <a name="manually-bootstrapping-the-nuget-provider-on-a-machine-that-is-not-connected-to-the-internet"></a><span data-ttu-id="037c1-113">A NuGet-szolgáltató olyan gépen, amely nem kapcsolódik az internethez manuálisan rendszerindítása</span><span class="sxs-lookup"><span data-stu-id="037c1-113">Manually bootstrapping the NuGet provider on a machine that is not connected to the Internet</span></span>

<span data-ttu-id="037c1-114">A fenti bemutatott folyamatok tegyük fel, a számítógép csatlakozik az internethez, és letöltheti a fájlokat egy nyilvános helyről.</span><span class="sxs-lookup"><span data-stu-id="037c1-114">The processes demonstrated above assume the machine is connected to the Internet and can download files from a public location.</span></span>
<span data-ttu-id="037c1-115">Ha ez nem lehetséges, az egyetlen lehetősége a gépek a fenti eljárások bootstrap, és manuálisan másolja a szolgáltató a folyamatot, az offline megbízható elkülönített csomópont.</span><span class="sxs-lookup"><span data-stu-id="037c1-115">If that is not possible, the only option is to bootstrap a machine using the processes given above, and manually copy the provider to the isolated node through an offline trusted process.</span></span>
<span data-ttu-id="037c1-116">Ebben a forgatókönyvben a leggyakrabban használt használati eset akkor, ha egy titkos gyűjtemény érhető el egy elkülönített környezet támogatásához.</span><span class="sxs-lookup"><span data-stu-id="037c1-116">The most common use case for this scenario is when a private gallery is available to support an isolated environment.</span></span>

<span data-ttu-id="037c1-117">Egy internethez csatlakoztatott számítógép rendszerindításának a fenti folyamatot követve, miután található szolgáltató fájlok helyét:</span><span class="sxs-lookup"><span data-stu-id="037c1-117">After following the process above to bootstrap an Internet connected machine, you will find provider files in the location:</span></span>

```
C:\Program Files\PackageManagement\ProviderAssemblies\
```

<span data-ttu-id="037c1-118">A fájl/mappa szerkezete a NuGet-szolgáltató (és valószínűleg különböző verziószámú) lesz:</span><span class="sxs-lookup"><span data-stu-id="037c1-118">The folder/file structure of the NuGet provider will be (possibly with a different version number):</span></span>

<span data-ttu-id="037c1-119">NuGet</span><span class="sxs-lookup"><span data-stu-id="037c1-119">NuGet</span></span><br>
<span data-ttu-id="037c1-120">--2.8.5.208</span><span class="sxs-lookup"><span data-stu-id="037c1-120">--2.8.5.208</span></span><br>
<span data-ttu-id="037c1-121">---Microsoft.PackageManagement.NuGetProvider.dll</span><span class="sxs-lookup"><span data-stu-id="037c1-121">----Microsoft.PackageManagement.NuGetProvider.dll</span></span>

<span data-ttu-id="037c1-122">Ezeket a mappákat és a kapcsolat nélküli gépekre megbízható eljárással fájl másolása.</span><span class="sxs-lookup"><span data-stu-id="037c1-122">Copy these folders and file using a trusted process to the offline machines.</span></span>

## <a name="manually-bootstrapping-nugetexe-to-support-publish-operations-on-a-machine-that-is-not-connected-to-the-internet"></a><span data-ttu-id="037c1-123">Olyan gépen, amely nem kapcsolódik az internethez műveletek manuálisan rendszerindítása támogatásához NuGet.exe közzététele</span><span class="sxs-lookup"><span data-stu-id="037c1-123">Manually bootstrapping NuGet.exe to support publish operations on a machine that is not connected to the Internet</span></span>

<span data-ttu-id="037c1-124">A folyamat a NuGet-szolgáltatót, manuálisan bootstrap, ha a gép használandó modulok vagy parancsfájlok közzététele egy titkos gyűjtemény használata mellett a *Publish-modul* vagy *Publish-parancsfájl* parancsmagok a NuGet.exe bináris végrehajtható fájl lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="037c1-124">In addition to the process to manually bootstrap the NuGet provider, if the machine will be used to publish modules or scripts to a private gallery using the *Publish-Module* or *Publish-Script* cmdlets, the NuGet.exe binary executable file will be required.</span></span>
<span data-ttu-id="037c1-125">Ebben a forgatókönyvben a leggyakrabban használt használati eset akkor, ha egy titkos gyűjtemény érhető el egy elkülönített környezet támogatásához.</span><span class="sxs-lookup"><span data-stu-id="037c1-125">The most common use case for this scenario is when a private gallery is available to support an isolated environment.</span></span>
<span data-ttu-id="037c1-126">A NuGet.exe fájl két lehetőség áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="037c1-126">There are two options to obtain the NuGet.exe file.</span></span>

<span data-ttu-id="037c1-127">Egy lehetőség egy olyan számítógépen, amelyen az internethez csatlakoztatott bootstrap, és másolja a fájlokat megbízható folyamatok használata a kapcsolat nélküli gépek.</span><span class="sxs-lookup"><span data-stu-id="037c1-127">One option is to bootstrap a machine that is Internet connected and copy the files to the offline machines using a trusted process.</span></span>
<span data-ttu-id="037c1-128">Az Internet csatlakoztatott számítógép rendszerindítása, miután a NuGet.exe bináris két mappák egyikében található:</span><span class="sxs-lookup"><span data-stu-id="037c1-128">After bootstrapping the Internet connected machine, the NuGet.exe binary will be located in one of two folders:</span></span>

<span data-ttu-id="037c1-129">Ha a *Publish-modul* vagy *Publish-parancsfájl* parancsmagok végrehajtódtak emelt szintű engedélyekkel (egy rendszergazdaként):</span><span class="sxs-lookup"><span data-stu-id="037c1-129">If the *Publish-Module* or *Publish-Script* cmdlets were executed with elevated permissions (As an Administrator):</span></span>

```
$env:ProgramData\Microsoft\Windows\PowerShell\PowerShellGet
```

<span data-ttu-id="037c1-130">Ha a parancsmagok végrehajtása nem emelt szintű engedélyekkel rendelkező felhasználóként történt:</span><span class="sxs-lookup"><span data-stu-id="037c1-130">If the cmdlets were executed as a user without elevated permissions:</span></span>

```
$env:userprofile\AppData\Local\Microsoft\Windows\PowerShell\PowerShellGet\
```

<span data-ttu-id="037c1-131">Egy második lehetőség a NuGet.Org webhely NuGet.exe töltheti le: [https://dist.nuget.org/index.html](https://dist.nuget.org/index.html)</span><span class="sxs-lookup"><span data-stu-id="037c1-131">A second option is to download NuGet.exe from the NuGet.Org website: [https://dist.nuget.org/index.html](https://dist.nuget.org/index.html)</span></span><br>
<span data-ttu-id="037c1-132">Ha egy éles gépek Nuget verziót választja, ellenőrizze, hogy az újabb, mint 2.8.5.208, és azonosítja a verzióját, amely "ajánlott" lett címkézve.</span><span class="sxs-lookup"><span data-stu-id="037c1-132">When selecting a NugGet version for production machines, make sure it is later than 2.8.5.208, and identify the version that has been labeled "recommended".</span></span>
<span data-ttu-id="037c1-133">Fontos, hogy a fájl feloldása, ha a böngésző használatával lett letöltve.</span><span class="sxs-lookup"><span data-stu-id="037c1-133">Remember to unblock the file if it was downloaded using a browser.</span></span>
<span data-ttu-id="037c1-134">Ennek segítségével hajtható végre a *Unblock-fájl* parancsmag.</span><span class="sxs-lookup"><span data-stu-id="037c1-134">This can be performed by using the *Unblock-File* cmdlet.</span></span>

<span data-ttu-id="037c1-135">Mindkét esetben a NuGet.exe fájl másolhat tetszőleges helyére *$env: elérési*, azonban a szabványos helyekre:</span><span class="sxs-lookup"><span data-stu-id="037c1-135">In either case, the NuGet.exe file can be copied to any location in *$env:path*, but the standard locations are:</span></span>

<span data-ttu-id="037c1-136">A végrehajtható fájl elérhetővé tétele, hogy minden felhasználó használhatja *Publish-modul* és *Publish-parancsfájl* parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="037c1-136">To make the executable available so that all users can use *Publish-Module* and *Publish-Script* cmdlets:</span></span>

```
$env:ProgramData\Microsoft\Windows\PowerShell\PowerShellGet
```

<span data-ttu-id="037c1-137">A végrehajtható fájl csak egy adott felhasználó rendelkezésre állásúvá tételéhez másolja a hely csak a felhasználó profiljában:</span><span class="sxs-lookup"><span data-stu-id="037c1-137">To make the executable available to only a specific user, copy to the location within only that user's profile:</span></span>

```
$env:userprofile\AppData\Local\Microsoft\Windows\PowerShell\PowerShellGet\
```