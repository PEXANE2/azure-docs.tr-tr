---
title: Azure Data Factory doğrulama etkinliği
description: Doğrulama etkinliği, eklenen veri kümesini kullanıcının belirttiği ölçütlere göre doğrulayıp işlem hattının yürütülmesine devam etmez.
services: data-factory
documentationcenter: ''
author: dcstwh
ms.author: weetok
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/25/2019
ms.openlocfilehash: 63f4a7df76fc0480a2b0cdf2de13ff5e85aa93ef
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96485867"
---
# <a name="validation-activity-in-azure-data-factory"></a>Azure Data Factory doğrulama etkinliği
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

İşlem hattının yalnızca yürütmeye devam etmesini sağlamak için bir işlem hattında bir doğrulama kullanabilirsiniz, bu işlem, eklenen veri kümesi başvurusunun var olduğunu doğrulandıktan sonra, belirtilen ölçütlere uyduğunda veya zaman aşımına ulaşılmıştır.


## <a name="syntax"></a>Syntax

```json

{
    "name": "Validation_Activity",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_File",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "minimumSize": 20
    }
},
{
    "name": "Validation_Activity_Folder",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_Folder",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "childItems": true
    }
}

```


## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
name | ' Doğrulama ' etkinliğinin adı | Dize | Evet |
tür | **Doğrulama** olarak ayarlanmalıdır. | Dize | Evet |
veri kümesi | Etkinlik, bu veri kümesi başvurusunun var olduğunu ve belirtilen ölçütlere uyup uymadığını ya da zaman aşımına ulaşılmasına kadar yürütmeyi engeller. Belirtilen veri kümesi "MinimumSize" veya "ChildItems" özelliğini desteklemelidir. | Veri kümesi başvurusu | Evet |
timeout | Çalıştırılacak etkinliğinin zaman aşımını belirtir. Değer belirtilmemişse, varsayılan değer 7 gündür ("7.00:00:00"). Biçim d. hh: mm: ss şeklindedir | Dize | Hayır |
kullanılmadığında | Doğrulama denemeleri arasındaki saniye cinsinden gecikme. Değer belirtilmemişse, varsayılan değer 10 saniyedir. | Tamsayı | Hayır |
childItems | Klasörde alt öğeler olup olmadığını denetler. -True olarak ayarlanabilir: klasörün var olduğunu ve öğe olduğunu doğrulayın. Klasörde en az bir öğe mevcut olana kadar veya zaman aşımı değerine ulaşılana kadar engeller.-false: klasörün var olduğunu ve boş olduğunu doğrulayın. Klasör boş olana veya zaman aşımı değerine ulaşılana kadar engeller. Hiçbir değer belirtilmemişse, etkinlik, klasör mevcut olana kadar veya zaman aşımına ulaşılıncaya kadar engeller. | Boole | Hayır |
minimumSize | Bir dosyanın bayt cinsinden en küçük boyutu. Değer belirtilmemişse, varsayılan değer 0 bayttır | Tamsayı | Hayır |


## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinliklerini görün:

- [If Condition Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Çalıştırma Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her Bir Etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web Etkinliği](control-flow-web-activity.md)
- [Until Etkinliği](control-flow-until-activity.md)
