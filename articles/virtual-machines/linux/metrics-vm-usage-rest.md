---
title: REST API'sini kullanarak Azure Sanal Makine kullanım verilerini alın
description: Sanal Makine için kullanım ölçümlerini toplamak için Azure REST API'lerini kullanın.
author: rloutlaw
ms.service: virtual-machines
ms.subservice: monitoring
ms.custom: REST
ms.topic: article
ms.date: 06/13/2018
ms.author: routlaw
ms.openlocfilehash: 07e91f3d9fd32f01db91415bfd90746cd1aef403
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78944747"
---
# <a name="get-virtual-machine-usage-metrics-using-the-rest-api"></a>REST API'sini kullanarak Sanal Makine kullanım ölçümlerini alın

Bu örnek, [Azure REST API'sini](/rest/api/azure/)kullanarak bir [Linux Sanal Makine](https://docs.microsoft.com/azure/virtual-machines/linux/monitor) için CPU kullanımının nasıl alınır olduğunu gösterir.

REST API'si için eksiksiz başvuru belgeleri ve ek örnekler [Azure Monitor REST başvurusunda](/rest/api/monitor)mevcuttur. 

## <a name="build-the-request"></a>İsteği oluşturma

Sanal Makine'den Yüzde [CPU metriklerini](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftcomputevirtualmachines) toplamak için aşağıdaki GET isteğini kullanın

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmname}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=Percentage%20CPU&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
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
| resourceGroupName | Kaynakla ilişkili Azure kaynak grubunun adı. Bu değeri Azure Kaynak Yöneticisi API, CLI veya portaldan alabilirsiniz. |
| vmname | Azure Sanal Makine'nin adı. |
| metrik adlar | Geçerli [Yük Dengeleyici ölçümlerinin](/azure/load-balancer/load-balancer-standard-diagnostics)virgülle ayrılmış listesi. |
| api-sürümü | İstek için kullanılacak API sürümü.<br /><br /> Bu belge, yukarıdaki `2018-01-01`URL'de yer alan api sürümünü kapsar.  |
| Timespan | Döndürülen ölçümlerin `startDateTime_ISO/endDateTime_ISO` zaman aralığını tanımlayan aşağıdaki biçimi içeren dize. Bu isteğe bağlı parametre, örnekte bir günlük veri değerini döndürecek şekilde ayarlanmıştır. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>İstek gövdesi

Bu işlem için istek organına gerek yoktur.

## <a name="handle-the-response"></a>Yanıtı işleme

Durum kodu 200, metrik değerler listesi başarıyla döndürüldüğünde döndürülür. [Başvuru belgelerinde](/rest/api/monitor/metrics/list#errorresponse)hata kodlarının tam listesi bulunur.

## <a name="example-response"></a>Örnek yanıt 

```json
{
    "cost": 0,
    "timespan": "2018-06-08T23:48:10Z/2018-06-09T00:48:10Z",
    "interval": "PT1M",
    "value": [
        {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmname}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=Percentage%20CPU",
            "type": "Microsoft.Insights/metrics",
            "name": {
                "value": "Percentage CPU",
                "localizedValue": "Percentage CPU"
            },
            "unit": "Percent",
            "timeseries": [
                {
                    "metadatavalues": [],
                    "data": [
                        {
                            "timeStamp": "2018-06-08T23:48:00Z",
                            "average": 0.44
                        },
                        {
                            "timeStamp": "2018-06-08T23:49:00Z",
                            "average": 0.31
                        },
                        {
                            "timeStamp": "2018-06-08T23:50:00Z",
                            "average": 0.29
                        },
                        {
                            "timeStamp": "2018-06-08T23:51:00Z",
                            "average": 0.29
                        },
                        {
                            "timeStamp": "2018-06-08T23:52:00Z",
                            "average": 0.285
                        } ]
                } ]
        } ]
}
```
