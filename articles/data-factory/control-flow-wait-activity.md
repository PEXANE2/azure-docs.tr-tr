---
title: Azure Data Factory bekleyen etkinlik | Microsoft Docs
description: Bekleme etkinliği, belirtilen dönem için işlem hattının yürütülmesini duraklatır.
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
ms.openlocfilehash: 48a722979e61209a855dd1fec22fcdcc756ae1ce
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70142429"
---
# <a name="execute-wait-activity-in-azure-data-factory"></a>Azure Data Factory 'de bekleme etkinliğini Yürüt
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
name | `Wait` Etkinliğin adı. | Dize | Evet
type | **Wait**olarak ayarlanmalıdır. | Dize | Evet
Waittimeınseconds | İşlem hattının işleme devam etmeden önce bekleyeceği saniye sayısı. | Integer | Evet

## <a name="example"></a>Örnek

> [!NOTE]
> Bu bölüm, işlem hattını çalıştırmak için JSON tanımları ve örnek PowerShell komutları sağlar. Azure PowerShell ve JSON tanımlarını kullanarak Data Factory işlem hattı oluşturmaya yönelik adım adım yönergeler için bkz. [öğretici: Azure PowerShell kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md).

### <a name="pipeline-with-wait-activity"></a>Bekleme etkinliği ile işlem hattı
Bu örnekte, işlem hattının iki etkinliği vardır: **Until** ve **bekle**. Bekleme etkinliği bir saniye bekleyecek şekilde yapılandırılmıştır. İşlem hattı, her çalıştırma arasında ikinci bir bekleme süresine sahip bir döngüde Web etkinliğini çalıştırır. 

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
Data Factory tarafından desteklenen diğer denetim akışı etkinliklerini görün: 

- [If Koşulu Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
- [Bitiş Etkinliği](control-flow-until-activity.md)
