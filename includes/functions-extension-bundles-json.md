---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/14/2019
ms.author: glenga
ms.openlocfilehash: f47a543143c949715fe2a49adccf074759a346fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79382168"
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

Varsayılan paket tarafından yüklenen geçerli uzantı kümesi bu [extensions.json dosyasında](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json)numaralandırılır.
