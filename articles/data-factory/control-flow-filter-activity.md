---
title: Azure Data Factory etkinliği filtrele
description: Filtre etkinliği girdileri filtreler.
author: dcstwh
ms.author: weetok
ms.reviewer: maghan
ms.service: data-factory
ms.topic: conceptual
ms.date: 05/04/2018
ms.openlocfilehash: 035ee5a5f1fe193fcb4579f1d46472ad9cd0b8b5
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100385600"
---
# <a name="filter-activity-in-azure-data-factory"></a>Azure Data Factory etkinliği filtrele
Bir giriş dizisine filtre ifadesi uygulamak için bir işlem hattındaki filtre etkinliklerini kullanabilirsiniz. 
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

## <a name="syntax"></a>Syntax

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
name | `Filter`Etkinliğin adı. | Dize | Yes
tür | **Filter** olarak ayarlanmalıdır. | Dize | Yes
koşul | Girişi filtrelemek için kullanılacak koşul. | Expression | Yes
öğeler | Filtrenin uygulanması gereken giriş dizisi. | Expression | Yes

## <a name="example"></a>Örnek

Bu örnekte, işlem hattının iki etkinliği vardır: **Filter** ve **foreach**. Filtre etkinliği, giriş dizisini 3 ' ten büyük bir değere sahip öğeler için filtrelemek üzere yapılandırılmıştır. ForEach etkinliği daha sonra filtrelenmiş değerler üzerinde yinelenir ve değişken **testini** geçerli değere ayarlar.

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
Data Factory tarafından desteklenen diğer denetim akışı etkinliklerini görün: 

- [If Condition Etkinliği](control-flow-if-condition-activity.md)
- [İşlem hattı yürütme etkinliği](control-flow-execute-pipeline-activity.md)
- [Her Bir Etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web Etkinliği](control-flow-web-activity.md)
- [Until Etkinliği](control-flow-until-activity.md)
