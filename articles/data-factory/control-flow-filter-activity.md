---
title: Azure Veri Fabrikası'nda filtreleme etkinliği
description: Filtre etkinliği girişleri filtreler.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/04/2018
ms.openlocfilehash: 5794a2c754e2082fdf45ad689bdb89ea494541c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73679867"
---
# <a name="filter-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda filtreleme etkinliği
Bir giriş dizisine filtre ifadesi uygulamak için bir filtre etkinliğini ardışık düzende kullanabilirsiniz. 

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "MyFilterActivity",
    "type": "filter",
    "typeProperties": {
        "condition": "<condition>",
        "items": "<input array>"
    }
}
```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | `Filter` Etkinliğin adı. | Dize | Evet
type | **Filtrelenecek**şekilde ayarlanmalıdır. | Dize | Evet
Durum | Girişi filtrelemek için kullanılacak koşul. | İfadeler | Evet
Bileşen | Hangi filtrenin uygulanması gerektiğini giriş dizisi. | İfadeler | Evet

## <a name="example"></a>Örnek

Bu örnekte, ardışık iki etkinlik vardır: **Filtre** ve **ForEach**. Filtre etkinliği, 3'ten büyük bir değeri olan öğeler için giriş dizisini filtrelemek için yapılandırılır. ForEach etkinliği daha sonra filtre uygulanmış değerler üzerinde yinelenir ve değişken **testini** geçerli değere ayarlar.

```json
{
    "name": "PipelineName",
    "properties": {
        "activities": [{
                "name": "MyFilterActivity",
                "type": "filter",
                "typeProperties": {
                    "condition": "@greater(item(),3)",
                    "items": "@pipeline().parameters.inputs"
                }
            },
            {
            "name": "MyForEach",
            "type": "ForEach",
            "dependsOn": [
                {
                    "activity": "MyFilterActivity",
                    "dependencyConditions": [
                        "Succeeded"
                    ]
                }
            ],
            "userProperties": [],
            "typeProperties": {
                "items": {
                    "value": "@activity('MyFilterActivity').output.value",
                    "type": "Expression"
                },
                "isSequential": "false",
                "batchCount": 1,
                "activities": [
                    {
                        "name": "Set Variable1",
                        "type": "SetVariable",
                        "dependsOn": [],
                        "userProperties": [],
                        "typeProperties": {
                            "variableName": "test",
                            "value": {
                                "value": "@string(item())",
                                "type": "Expression"
                            }
                        }
                    }
                ]
            }
        }],
        "parameters": {
            "inputs": {
                "type": "Array",
                "defaultValue": [1, 2, 3, 4, 5, 6]
            }
        },
        "variables": {
            "test": {
                "type": "String"
            }
        },
        "annotations": []
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası tarafından desteklenen diğer kontrol akışı etkinliklerine bakın: 

- [If Koşulu Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Çalıştırma Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her Bir Etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web Etkinliği](control-flow-web-activity.md)
- [Until Etkinliği](control-flow-until-activity.md)
