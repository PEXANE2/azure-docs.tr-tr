---
title: Azure Veri Fabrikası'nda doğrulama etkinliği
description: Doğrulama etkinliği, kullanıcının belirlediği belirli ölçütlerle ekteki veri kümesini doğrulayana kadar ardışık hatlar yürütmeye devam etmez.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/25/2019
ms.openlocfilehash: f63c78c59d7d6be3c66ea0785389eff73e3bff60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73678360"
---
# <a name="validation-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda doğrulama etkinliği
Ardışık hatlar sisteminde doğrulama, yalnızca ekteki veri kümesi başvurusu var olduğunu, belirtilen ölçütleri karşıladığını veya zaman adedine ulaşıldığını doğruladıktan sonra yürütmeye devam etmesini sağlamak için bir doğrulama yı kullanabilirsiniz.


## <a name="syntax"></a>Sözdizimi

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
ad | 'Doğrulama' etkinliğinin adı | Dize | Evet |
type | **Doğrulama**olarak ayarlanmalıdır. | Dize | Evet |
Dataset | Etkinlik, bu veri kümesi başvurusu var olduğunu ve belirtilen ölçütleri karşıladığını veya zaman adedine ulaşıldığını doğrulayana kadar yürütmeyi engeller. Sağlanan Veri Kümesi "Minimum Boyut" veya "ChildItems" özelliğini desteklemelidir. | Dataset başvurusu | Evet |
timeout | Çalıştırılacak etkinliğinin zaman aşımını belirtir. Değer belirtilmemişse, varsayılan değer 7 gündür ("7.00:00:00"). Biçim d.hh:mm:ss | Dize | Hayır |
Uyku | Doğrulama girişimleri arasında saniye cinsinden bir gecikme. Değer belirtilmemişse, varsayılan değer 10 saniyedir. | Tamsayı | Hayır |
Childıtems | Klasörde alt öğeler olup olmadığını denetler. Doğru olarak ayarlanabilir : Klasörün var olduğunu ve öğeleri olduğunu doğrulayın. Klasörde en az bir öğe bulunana veya zaman önce değerine ulaşılAna kadar engeller.-false: Klasörün var olduğunu ve boş olduğunu doğrulayın. Klasör boşalana veya zaman adedine ulaşana kadar engeller. Değer belirtilmemişse, klasör var olana veya zaman anına erişilene kadar etkinlik engellenir. | Boole | Hayır |
Minimumsize | Baytlarda bir dosyanın minimum boyutu. Değer belirtilmemişse, varsayılan değer 0 bayt | Tamsayı | Hayır |


## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası tarafından desteklenen diğer kontrol akışı etkinliklerine bakın:

- [If Koşulu Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Çalıştırma Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her Bir Etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web Etkinliği](control-flow-web-activity.md)
- [Until Etkinliği](control-flow-until-activity.md)
