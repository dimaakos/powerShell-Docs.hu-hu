---
ms.date: 06/12/2017
author: JKeithB
ms.topic: reference
keywords: WMF, powershell, beállítás
ms.openlocfilehash: 3f2d03311f71ec9298b61c125326ad1cd8783173
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/09/2018
---
# <a name="format-hex"></a>Format-Hex
**Hexadecimális formátumú** lehetővé teszi, hogy a szöveges vagy bináris adatok megtekintéséhez hexadecimális formátumban, tanulmányozza a [hexadecimális formátumban](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.utility/format-hex)

## <a name="example-1"></a>1. példa
Hexadecimális formátumú karakterlánc tartalmának megtekintése.

```powershell
"This is a very long line to force the line folding in Format-Hex cmdlet" | Format-Hex
```

Kimenetek
```
PS C:\> This is a very long line to force the line folding in Format-Hex cmdlet" | Format-Hex


           00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F

00000000   54 68 69 73 20 69 73 20 61 20 76 65 72 79 20 6C  This is a very l
00000010   6F 6E 67 20 6C 69 6E 65 20 74 6F 20 66 6F 72 63  ong line to forc
00000020   65 20 74 68 65 20 6C 69 6E 65 20 66 6F 6C 64 69  e the line foldi
00000030   6E 67 20 69 6E 20 46 6F 72 6D 61 74 2D 48 65 78  ng in Format-Hex
00000040   20 63 6D 64 6C 65 74                              cmdlet


PS C:\>
```