---
title: Azure Veri Fabrikası'nda Değişken Etkinliği Ayarlama
description: Veri Fabrikası ardışık hattında tanımlanan varolan bir değişkenin değerini ayarlamak için Değişken Kümesi etkinliğini nasıl kullanacağınızı öğrenin
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
ms.openlocfilehash: e736cc95628bd0e15bdb7ffd425608278788c353
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879283"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda değişken etkinliği ayarlama

Veri Fabrikası ardışık dizisinde tanımlanan String, Bool veya Array türünden varolan bir değişkenin değerini ayarlamak için Değişken Ayarını Ayarla etkinliğini kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
ad | Boru hattındaki etkinliğin adı | evet
açıklama | Etkinliğin ne yaptığını açıklayan metin | hayır
type | **SetVariable** olarak ayarlanmalıdır | evet
value | Değişkenin atanacağı string literal veya ifade nesnesi değeri | evet
değişkenAd | Bu etkinlik tarafından ayarlanacak değişkenin adı | evet

## <a name="incrementing-a-variable"></a>Değişkeni artımlama

Azure Veri Fabrikası'ndaki değişkenleri içeren yaygın bir senaryo, bir ana veya foreach etkinliği içinde bir değişkeni yineleyici olarak kullanmaktır. Bir dizi değişken etkinliğinde, alana ayarlanan `value` değişkene başvuru yapamazsınız. Bu sınırlamayı geçici olarak çözüme getirmek için geçici bir değişken ayarlayın ve ardından ikinci bir ayarlı değişken etkinliği oluşturun. İkinci set değişken etkinliği geçici değişken için yineleyici değerini ayarlar. 

Aşağıda bu desen bir örnektir:

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
Veri Fabrikası tarafından desteklenen ilgili bir kontrol akışı etkinliği hakkında bilgi edinin: 

- [Değişken Etkinlik Ekleme](control-flow-append-variable-activity.md)
