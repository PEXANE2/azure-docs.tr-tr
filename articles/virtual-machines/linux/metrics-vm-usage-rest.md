---
title: REST API kullanarak Azure sanal makine kullanım verilerini alın
description: Azure REST API 'Lerini kullanarak bir sanal makine için kullanım ölçümleri toplayın.
author: rloutlaw
ms.service: virtual-machines
ms.subservice: monitoring
ms.custom: REST
ms.topic: article
ms.date: 06/13/2018
ms.author: routlaw
ms.openlocfilehash: 07e91f3d9fd32f01db91415bfd90746cd1aef403
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78944747"
---
# <a name="get-virtual-machine-usage-metrics-using-the-rest-api"></a>REST API kullanarak sanal makine kullanım ölçümlerini alın

Bu örnek, [Azure REST API](/rest/api/azure/)kullanarak bir [Linux sanal makinesi](https://docs.microsoft.com/azure/virtual-machines/linux/monitor) için CPU kullanımının nasıl alınacağını gösterir.

Tüm başvuru belgelerini ve REST API için ek örnekleri [Azure IZLEYICI Rest başvurusunda](/rest/api/monitor)bulabilirsiniz. 

## <a name="build-the-request"></a>İsteği oluşturma

Bir sanal makineden alınan [CPU ölçüsünü yüzde](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftcomputevirtualmachines) olarak toplamak IÇIN aşağıdaki get isteğini kullanın

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmname}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=Percentage%20CPU&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
```

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki üstbilgiler gereklidir: 

|İstek üst bilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. `application/json` olarak ayarlayın.|  
|*Yetkilendirme:*|Gereklidir. Geçerli bir `Bearer` [erişim belirtecine](/rest/api/azure/#authorization-code-grant-interactive-clients) ayarlayın. |  

### <a name="uri-parameters"></a>URI parametreleri

| Adı | Açıklama |
| :--- | :---------- |
| subscriptionId | Bir Azure aboneliğini tanımlayan abonelik KIMLIĞI. Birden çok aboneliğiniz varsa bkz. [birden çok abonelikle çalışma](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). |
| resourceGroupName | Kaynakla ilişkili Azure Kaynak grubunun adı. Bu değeri Azure Resource Manager API, CLı veya portaldan edinebilirsiniz. |
| VMName | Azure sanal makinesinin adı. |
| metricnames | Geçerli [Load Balancer ölçümlerinin](/azure/load-balancer/load-balancer-standard-diagnostics)virgülle ayrılmış listesi. |
| api-sürümü | İstek için kullanılacak API sürümü.<br /><br /> Bu belge, yukarıdaki URL 'ye `2018-01-01`dahil edilen api sürümünü içerir.  |
| timespan | Döndürülen ölçümlerin zaman aralığını tanımlayan `startDateTime_ISO/endDateTime_ISO` aşağıdaki biçimdeki dize. Bu isteğe bağlı parametre, örnekteki verilerin bir gününü döndürecek şekilde ayarlanır. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>İstek gövdesi

Bu işlem için istek gövdesi gerekli değil.

## <a name="handle-the-response"></a>Yanıtı işleme

200 durum kodu, ölçüm değerleri listesi başarıyla döndürüldüğünde döndürülür. [Başvuru belgelerinde](/rest/api/monitor/metrics/list#errorresponse)hata kodlarının tam bir listesi bulunur.

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
