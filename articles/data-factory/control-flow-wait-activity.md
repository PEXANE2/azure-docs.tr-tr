---
title: Azure Veri Fabrikası'nda bekleme etkinliği
description: Bekle etkinliği, belirtilen dönem için ardışık noktanın yürütülmesini duraklatır.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/12/2018
ms.openlocfilehash: f9dd53fded06eec169219d00993620a0f2aa2bf0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73678236"
---
# <a name="execute-wait-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda bekleme etkinliğini yürütme
İşlem hattında Bekleme etkinliğini kullandığınızda, işlem hattı izleyen etkinlikleri yürütmeye devam etmeden önce belirtilen süre kadar bekler. 

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "MyWaitActivity",
    "type": "Wait",
    "typeProperties": {
        "waitTimeInSeconds": 1
    }
}

```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | `Wait` Etkinliğin adı. | Dize | Evet
type | **Bekle'** ye ayarlanmalıdır. | Dize | Evet
waitTimeInSeconds | İşleme devam etmeden önce ardışık işlemin beklediği saniye sayısı. | Tamsayı | Evet

## <a name="example"></a>Örnek

> [!NOTE]
> Bu bölümde, ardışık hattı çalıştırmak için JSON tanımları ve örnek PowerShell komutları sağlar. Azure PowerShell ve JSON tanımlarını kullanarak bir Veri Fabrikası ardışık hattı oluşturmak için adım adım yönergeleri içeren bir yol [için, bkz.](quickstart-create-data-factory-powershell.md)

### <a name="pipeline-with-wait-activity"></a>Bekleme etkinliği olan ardışık hatlar
Bu örnekte, ardışık iki etkinlik vardır: **Kadar** ve **Bekleyin.** Bekle etkinliği bir saniye bekleyecek şekilde yapılandırılır. Ardışık hatlar, Web etkinliğini her çalıştırma arasında bir saniyebekleme süresiyle bir döngü içinde çalıştırın. 

```json
{
    "name": "DoUntilPipeline",
    "properties": {
        "activities": [
            {
                "type": "Until",
                "typeProperties": {
                    "expression": {
                        "value": "@equals('Failed', coalesce(body('MyUnauthenticatedActivity')?.status, actions('MyUnauthenticatedActivity')?.status, 'null'))",
                        "type": "Expression"
                    },
                    "timeout": "00:00:01",
                    "activities": [
                        {
                            "name": "MyUnauthenticatedActivity",
                            "type": "WebActivity",
                            "typeProperties": {
                                "method": "get",
                                "url": "https://www.fake.com/",
                                "headers": {
                                    "Content-Type": "application/json"
                                }
                            },
                            "dependsOn": [
                                {
                                    "activity": "MyWaitActivity",
                                    "dependencyConditions": [ "Succeeded" ]
                                }
                            ]
                        },
                        {
                            "type": "Wait",
                            "typeProperties": {
                                "waitTimeInSeconds": 1
                            },
                            "name": "MyWaitActivity"
                        }
                    ]
                },
                "name": "MyUntilActivity"
            }
        ]
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
