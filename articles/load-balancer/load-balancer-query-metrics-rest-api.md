---
title: REST API ölçümleri alma
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure Load Balancer için sistem durumu ve kullanım ölçümleri toplamak üzere Azure REST API 'Lerini kullanmaya başlayın.
services: sql-database
author: asudbring
manager: KumudD
ms.service: load-balancer
ms.custom: REST, seodec18
ms.topic: how-to
ms.date: 11/19/2019
ms.author: allensu
ms.openlocfilehash: 3b5aedb20bc7a8d2aa6f3aa3d8691a71af4cd3a2
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84808379"
---
# <a name="get-load-balancer-usage-metrics-using-the-rest-api"></a>REST API kullanarak Load Balancer kullanım ölçümleri alın

[Azure REST API](/rest/api/azure/)kullanarak bir zaman aralığı için [Standart Load Balancer](/azure/load-balancer/load-balancer-standard-overview) işlenen bayt sayısını toplayın.

Tüm başvuru belgelerini ve REST API için ek örnekleri [Azure IZLEYICI Rest başvurusunda](/rest/api/monitor)bulabilirsiniz. 

## <a name="build-the-request"></a>İsteği oluşturma

Bir Standart Load Balancer [ByteCount ölçümünü](/azure/load-balancer/load-balancer-standard-diagnostics#multi-dimensional-metrics) toplamak IÇIN aşağıdaki get isteğini kullanın. 

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=ByteCount&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
```

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki üstbilgiler gereklidir: 

|İstek üst bilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. `application/json` olarak ayarlayın.|  
|*Yetkilendirme:*|Gereklidir. Geçerli bir `Bearer` [erişim belirtecine](/rest/api/azure/#authorization-code-grant-interactive-clients) ayarlayın. |  

### <a name="uri-parameters"></a>URI parametreleri

| Name | Description |
| :--- | :---------- |
| subscriptionId | Bir Azure aboneliğini tanımlayan abonelik KIMLIĞI. Birden çok aboneliğiniz varsa bkz. [birden çok abonelikle çalışma](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). |
| resourceGroupName | Kaynağı içeren kaynak grubunun adı. Bu değeri Azure Resource Manager API, CLı veya portaldan elde edebilirsiniz. |
| loadBalancerName | Azure Load Balancer adı. |
| ölçüm adları | Geçerli [Load Balancer ölçümlerinin](/azure/load-balancer/load-balancer-standard-diagnostics)virgülle ayrılmış listesi. |
| api-sürümü | İstek için kullanılacak API sürümü.<br /><br /> Bu belge `2018-01-01` , yukarıdaki URL 'ye dahil edilen api sürümünü içerir.  |
| timespan | Sorgunun TimeSpan değeri. Bu, aşağıdaki biçime sahip bir dizedir `startDateTime_ISO/endDateTime_ISO` . Bu isteğe bağlı parametre, örnekteki verilerin bir gününü döndürecek şekilde ayarlanır. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>İstek gövdesi

Bu işlem için istek gövdesi gerekli değil.

## <a name="handle-the-response"></a>Yanıtı işleme

200 durum kodu, ölçüm değerleri listesi başarıyla döndürüldüğünde döndürülür. [Başvuru belgelerinde](/rest/api/monitor/metrics/list#errorresponse)hata kodlarının tam bir listesi bulunur.

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
