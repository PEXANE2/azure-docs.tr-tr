---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/14/2019
ms.author: glenga
ms.openlocfilehash: f64892193eb6cfcce8f948b54e5557b5fa3d90ab
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80878263"
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

Paket sürümleri artıkında paket değişim artışı. Paketteki paketler ana bir sürümle artında ana sürüm değişiklikleri oluşur. Paketteki ana sürüm değişiklikleri genellikle Işlevler çalışma zamanının ana sürümündeki bir değişiklikle birlikte bulunur.  

Varsayılan paket tarafından yüklenen geçerli uzantı kümesi, [dosyasında buextensions.js](https://github.com/Azure/azure-functions-extension-bundles/blob/dev/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json)numaralandırılır.
