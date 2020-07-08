---
title: Azure Data Factory değişken etkinliğini ayarla
description: Bir Data Factory işlem hattında tanımlanmış var olan bir değişkenin değerini ayarlamak için değişken ayarla etkinliğini nasıl kullanacağınızı öğrenin
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/07/2020
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.openlocfilehash: a0b5fa16658d3e354bcb4f90ad998997fc844a84
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83832801"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Azure Data Factory değişken etkinliğini ayarla
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bir Data Factory işlem hattında tanımlanmış dize, bool veya dizi türünde bir değişkenin değerini ayarlamak için değişken ayarla etkinliğini kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
name | İşlem hattının etkinlik adı | evet
açıklama | Etkinliğin ne yaptığını açıklayan metin | hayır
tür | **SetVariable** olarak ayarlanmalıdır | evet
değer | Değişkenin atandığı dize sabiti veya ifade nesnesi değeri | evet
variableName | Bu etkinlik tarafından ayarlanan değişkenin adı | evet

## <a name="incrementing-a-variable"></a>Bir değişkeni artırma

Azure Data Factory değişkenleri içeren yaygın bir senaryo, bir Until veya ForEach etkinliği içinde bir yineleyici olarak bir değişken kullanmaktır. Bir değişken kümesi etkinliğinde, alanında ayarlanan değişkene başvurulamıyor `value` . Bu sınırlamaya geçici çözüm olarak, geçici bir değişken ayarlayın ve ardından ikinci bir değişken kümesi etkinliği oluşturun. İkinci değişken kümesi etkinliği yineleyicinin değerini geçici değişken olarak ayarlar. 

Aşağıda bu düzenin bir örneği verilmiştir:

![Artış değişkeni](media/control-flow-set-variable-activity/increment-variable.png "Artış değişkeni")

``` json
{
    "name": "pipeline3",
    "properties": {
        "activities": [
            {
                "name": "Set I",
                "type": "SetVariable",
                "dependsOn": [
                    {
                        "activity": "Increment J",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "userProperties": [],
                "typeProperties": {
                    "variableName": "i",
                    "value": {
                        "value": "@variables('j')",
                        "type": "Expression"
                    }
                }
            },
            {
                "name": "Increment J",
                "type": "SetVariable",
                "dependsOn": [],
                "userProperties": [],
                "typeProperties": {
                    "variableName": "j",
                    "value": {
                        "value": "@string(add(int(variables('i')), 1))",
                        "type": "Expression"
                    }
                }
            }
        ],
        "variables": {
            "i": {
                "type": "String",
                "defaultValue": "0"
            },
            "j": {
                "type": "String",
                "defaultValue": "0"
            }
        },
        "annotations": []
    }
}
```


## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen ilgili bir denetim akışı etkinliği hakkında bilgi edinin: 

- [Değişken Etkinlik Ekleme](control-flow-append-variable-activity.md)
