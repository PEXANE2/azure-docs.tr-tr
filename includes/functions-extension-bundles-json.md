---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/14/2019
ms.author: glenga
ms.openlocfilehash: b67e2bf2ae5af2feb334e898ce69fd5b959c7cf0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "88689582"
---
```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

Aşağıdaki özellikler ' de mevcuttur `extensionBundle` :

| Özellik | Açıklama |
| -------- | ----------- |
| kimlik | Microsoft Azure Işlevleri Uzantısı paketleri için ad alanı. |
| sürüm | Yüklenecek paket sürümü. Işlevler çalışma zamanı her zaman, sürüm aralığı veya aralığı tarafından tanımlanan izin verilen en fazla sürümü seçer. Yukarıdaki sürüm değeri, 1.0.0 'e kadar tüm paket sürümlerinin 2.0.0 dahil değil, izin vermez. Daha fazla bilgi için bkz. [Sürüm aralıklarını belirtmek için Aralık gösterimi](/nuget/reference/package-versioning#version-ranges). |