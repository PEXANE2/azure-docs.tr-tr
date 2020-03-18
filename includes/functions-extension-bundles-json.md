---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/14/2019
ms.author: glenga
ms.openlocfilehash: f47a543143c949715fe2a49adccf074759a346fa
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
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

Aşağıdaki özellikler `extensionBundle` öğesinde kullanıma sunuldu:

| Özellik | Açıklama |
| -------- | ----------- |
| id | Microsoft Azure İşlevleri uzantı paket gruplarına yönelik ad alanı. |
| version | Yüklenecek paket grubunun sürümü. İşlevler çalışma zamanı her zaman sürüm aralığı tarafından tanımlanan, izin verilen en yüksek sürümü seçer. Yukarıdaki sürüm değeri 1.0.0’dan yüksek tüm paket grubu sürümlerine izin verir ancak izin verilen sürüm aralığı 2.0.0’ı içermez. Daha fazla bilgi için bkz. [sürüm aralıklarını belirtmeye yönelik aralık gösterimi](/nuget/reference/package-versioning#version-ranges). |

Paket grubu sürümleri paket grubu değişikliklerinde paket olarak artar. Ana sürüm değişiklikleri, paket grubu içindeki paketler bir ana sürüm kadar artırıldığında oluşur. Paket grubundaki ana sürüm değişiklikleri genelde İşlevler çalışma zamanının ana sürümündeki değişikliklerle kesişir.  

Varsayılan paket grubu tarafından yüklenen mevcut uzantı kümesi bu [extensions.json dosyasında](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json) numaralandırılır.
