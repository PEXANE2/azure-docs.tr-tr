---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/14/2019
ms.author: glenga
ms.openlocfilehash: f64892193eb6cfcce8f948b54e5557b5fa3d90ab
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
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

Aşağıdaki özellikler mevcuttur: `extensionBundle`

| Özellik | Açıklama |
| -------- | ----------- |
| id | Microsoft Azure İşlevler uzantısı paketleri için ad alanı. |
| version | Paketin yüklenmesi için sürümü. İşlevler çalışma süresi her zaman sürüm aralığı veya aralığı tarafından tanımlanan maksimum izin verilen sürümü seçer. Yukarıdaki sürüm değeri, 1.0.0'dan 2.0.0'a kadar olan tüm paket sürümlerine izin verir, ancak 2.0.0 dahil değildir. Daha fazla bilgi [için, sürüm aralıklarını belirtmek için aralık gösterimine](/nuget/reference/package-versioning#version-ranges)bakın. |

Paket değişiminde paket olarak sürümler artış. Büyük sürüm değişiklikleri, paketteki paketler büyük bir sürüm le birlikte artınca oluşur. Paketteki ana sürüm değişiklikleri genellikle Işlevler çalışma zamanının ana sürümündeki bir değişiklikle çakışıyor.  

Varsayılan paket tarafından yüklenen geçerli uzantı kümesi bu [extensions.json dosyasında](https://github.com/Azure/azure-functions-extension-bundles/blob/dev/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json)numaralandırılır.
