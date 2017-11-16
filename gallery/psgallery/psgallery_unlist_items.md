---
ms.date: 2017-06-12
contributor: JKeithB
ms.topic: conceptual
keywords: "gyűjtemény, a powershell, a parancsmag, a psgallery"
title: psgallery_unlist_items
ms.openlocfilehash: 8fa09c77e144f14bf0fd3493dff7650897100715
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/12/2017
---
# <a name="unlisting-items"></a>Unlisting elemek

**Ezért egy elem éppen eltávolítja a PowerShell-galériából lehetőség nincs felfedve?**

A PowerShell-galériában nem támogatja a felhasználók saját elemek véglegesen törli. Ez lehetővé teszi, hogy mások az elemek anélkül, hogy a jövőben lehetséges oldaltörések függőségek végrehajthat. Például ha a Pester modul az Azure-moduljának függ, és az Azure rendszer eltávolítja a modult a gyűjteményből, akkor a felhasználó többé nem használja a Pester modul.

Helyett egy elem eltávolítása, azonban akkor is unlist, helyette.

**Mire unlisting hajtsa végre a megfelelő elemre a PowerShell-galériában?**

Egy elem modul vagy a PowerShell-galériában parancsfájl például unlisting eltávolítja a elemek lap.
Listán nem szereplő elemek emellett nem lesz felderíthető Keresősáv használatával.
Töltse le a listán nem szereplő elem csak úgy adhatja meg a pontos nevét és verzióját, az elem.
Emiatt egy elem unlisting nem megszakítja más modulok vagy parancsfájlok, amelyek függenek.

A cikk unlist, keresse fel az elem részleteit megjelenítő oldalra, és törlése lehetőséggel. Törölje a jelet az "Felsorolt" jelölőnégyzetből, és kattintson a Mentés gombra.

**Hogyan távolíthatom elemet?**

Ha egy olyan forgatókönyvet, ahol elem törlését szükség, lépjen kapcsolatba a PowerShell-Galériabeli rendszergazdák.
Érvényes törlés forgatókönyvek a következők:
- Szerzői jogsértéssel kapcsolatos problémákat.
- Az elem tartalma potenciálisan káros tartalmaz.
- Az elem tartalmaz bizalmas adatokat.

Küldje el a törlése elem elküldeni a kérelmet a PowerShell-Galériabeli rendszergazdák, látogasson el a cikk információs lapját, és válassza ki a forduljon a támogatási szolgálathoz.  

