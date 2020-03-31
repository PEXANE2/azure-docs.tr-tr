---
title: REST API ile ölçümleri alın
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure Yük Dengeleyicisi için sistem durumu ve kullanım ölçümlerini toplamak için Azure REST API'lerini kullanmaya başlayın.
services: sql-database
author: asudbring
manager: KumudD
ms.service: load-balancer
ms.custom: REST, seodec18
ms.topic: article
ms.date: 11/19/2019
ms.author: allensu
ms.openlocfilehash: 760ec8a945ab88b63dde2de75f5354818facf4f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74225250"
---
# <a name="get-load-balancer-usage-metrics-using-the-rest-api"></a>REST API'sini kullanarak Yük Dengeleyici kullanım ölçümlerini alın

[Azure REST API'sini](/rest/api/azure/)kullanarak [bir Standart Yük Dengeleyicisi](/azure/load-balancer/load-balancer-standard-overview) tarafından bir süre için işlenen bayt sayısını toplayın.

REST API'si için eksiksiz başvuru belgeleri ve ek örnekler [Azure Monitor REST başvurusunda](/rest/api/monitor)mevcuttur. 

## <a name="build-the-request"></a>İsteği oluşturma

[ByteCount](/azure/load-balancer/load-balancer-standard-diagnostics#multi-dimensional-metrics) ölçümçünü standart Yük Dengeleyicisinden toplamak için aşağıdaki GET isteğini kullanımını 

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=ByteCount&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
```

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki üstbilgiler gereklidir: 

|İstek üst bilgisi|Açıklama|  
|--------------------|-----------------|  
|*İçerik Türü:*|Gereklidir. `application/json` olarak ayarlayın.|  
|*Yetkilendirme:*|Gereklidir. Geçerli bir `Bearer` [erişim belirtecine](/rest/api/azure/#authorization-code-grant-interactive-clients) ayarlayın. |  

### <a name="uri-parameters"></a>URI parametreleri

| Adı | Açıklama |
| :--- | :---------- |
| subscriptionId | Azure aboneliğini tanımlayan abonelik kimliği. Birden çok aboneliğiniz varsa, [bkz.](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest) |
| resourceGroupName | Kaynağı içeren kaynak grubunun adı. Bu değeri Azure Kaynak Yöneticisi API, CLI veya portaldan edinebilirsiniz. |
| loadBalancerName | Azure Yük Dengeleyicisinin adı. |
| metrik adlar | Geçerli [Yük Dengeleyici ölçümlerinin](/azure/load-balancer/load-balancer-standard-diagnostics)virgülle ayrılmış listesi. |
| api-sürümü | İstek için kullanılacak API sürümü.<br /><br /> Bu belge, yukarıdaki `2018-01-01`URL'de yer alan api sürümünü kapsar.  |
| Timespan | Sorgunun zaman ası. Aşağıdaki biçime sahip bir `startDateTime_ISO/endDateTime_ISO`dize. Bu isteğe bağlı parametre, örnekte bir günlük veri değerini döndürecek şekilde ayarlanmıştır. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>İstek gövdesi

Bu işlem için istek organına gerek yoktur.

## <a name="handle-the-response"></a>Yanıtı işleme

Durum kodu 200, metrik değerler listesi başarıyla döndürüldüğünde döndürülür. [Başvuru belgelerinde](/rest/api/monitor/metrics/list#errorresponse)hata kodlarının tam listesi bulunur.

## <a name="example-response"></a>Örnek yanıt 

```json
{
    "cost": 0,
    "timespan": "2018-06-05T03:00:00Z/2018-06-07T03:00:00Z",
    "interval": "PT1M",
    "value": [
        {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/Microsoft.Insights/metrics/ByteCount",
            "type": "Microsoft.Insights/metrics",
            "name": {
                "value": "ByteCount",
                "localizedValue": "Byte Count"
            },
            "unit": "Count",
            "timeseries": [
                {
                    "metadatavalues": [],
                    "data": [
                        {
                            "timeStamp": "2018-06-06T17:24:00Z",
                            "total": 1067921034.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:25:00Z",
                            "total": 0.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:26:00Z",
                            "total": 3781344.0
                        },
                    ]
                }
            ]
        }
    ],
    "namespace": "Microsoft.Network/loadBalancers",
    "resourceregion": "eastus"
}
```
