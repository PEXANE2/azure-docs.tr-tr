---
title: Kaynak değişikliklerini alma
description: Bir kaynağın ne zaman değiştirildiğini bulmayı ve değiştirilen özelliklerin bir listesini almayı anlayın.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: resource-graph
ms.openlocfilehash: 2027f56d44be14895a40550d78a79d9e9dda9d97
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2019
ms.locfileid: "71980318"
---
# <a name="get-resource-changes"></a>Kaynak değişikliklerini alma

Kaynaklar günlük kullanım, yeniden yapılandırma ve hatta yeniden dağıtım kursu aracılığıyla değiştirilir.
Değişiklik, bir bireden veya otomatikleştirilmiş bir işlemden gelebilir. Çoğu değişiklik tasarıma göre yapılır, ancak bazen değildir. Değişiklik geçmişinin son 14 günü ile Azure Kaynak Grafiği şunları yapmanızı sağlar:

- Bir Azure Resource Manager özelliğinde ne zaman değişiklik algılandığını öğrenme.
- Bu değişiklik olayı kapsamında hangi özelliklerin değiştirildiğini görme.

Değişiklik algılama ve Ayrıntılar aşağıdaki örnek senaryolar için değerlidir:

- Olay yönetimi sırasında, _potansiyel_ olarak ilgili değişiklikleri anlayın. Belirli bir zaman penceresi sırasında değişiklik olayları için sorgulama yapın ve değişiklik ayrıntılarını değerlendirin.
- CMDB olarak bilinen bir yapılandırma yönetimi veritabanının tutulması, güncel. Tüm kaynakları ve bunların tam özellik kümelerini zamanlanan bir sıklıkta yenilemek yerine yalnızca nelerin değiştiğini alın.
- Bir kaynağın uyumluluk durumu değiştiğinde başka hangi özelliklerin değiştirildiğini anlama. Bu ek özelliklerin değerlendirilmesi, bir Azure Ilke tanımıyla yönetilmesi gerekebilecek diğer özelliklerle ilgili öngörüler sağlayabilir.

Bu makalede, kaynak grafiğinin SDK 'Sı aracılığıyla bu bilgilerin nasıl toplanacağı gösterilmektedir. Bu bilgileri Azure portal görmek için bkz. Azure Ilkesinin [değişiklik geçmişi](../../policy/how-to/determine-non-compliance.md#change-history-preview) veya Azure etkinlik günlüğü [değişiklik geçmişi](../../../azure-monitor/platform/activity-log-view.md#azure-portal).

> [!NOTE]
> Kaynak grafiğinde değişiklik ayrıntıları Kaynak Yöneticisi özellikleri içindir. Bir sanal makine içindeki değişiklikleri izlemek için bkz. Azure Otomasyonu 'nun [VM 'ler Için](../../policy/concepts/guest-configuration.md) [değişiklik Izleme](../../../automation/automation-change-tracking.md) veya Azure ilkesinin Konuk yapılandırması.

> [!IMPORTANT]
> Azure Kaynak grafiğindeki değişiklik geçmişi genel önizlemede.

## <a name="find-when-changes-were-detected"></a>Değişikliklerin ne zaman algılandığını bul

Bir kaynakta nelerin değiştiğini görmekte olan ilk adım, bu kaynakla ilgili değişiklik olaylarını bir zaman penceresi içinde bulmadır. Bu adım, **Resourcechanges** REST uç noktası aracılığıyla yapılır.

**Resourcechanges** uç noktası, istek gövdesinde iki parametre gerektirir:

- **RESOURCEID**: üzerinde değişiklik aranacak Azure kaynağı.
- **Aralık**: **Zulu saat dilimi (Z)** kullanarak bir değişiklik olayının ne zaman denetleneceği için _Başlangıç_ ve _bitiş_ tarihleri içeren bir özellik.

Örnek istek gövdesi:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "interval": {
        "start": "2019-03-28T00:00:00.000Z",
        "end": "2019-03-31T00:00:00.000Z"
    }
}
```

Yukarıdaki istek gövdesiyle, **Resourcechanges** için REST API URI 'si şu şekilde olur:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChanges?api-version=2018-09-01-preview
```

Yanıt aşağıdaki örneğe benzer şekilde görünür:

```json
{
    "changes": [{
            "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}",
            "beforeSnapshot": {
                "timestamp": "2019-03-29T01:32:05.993Z"
            },
            "afterSnapshot": {
                "timestamp": "2019-03-29T01:54:24.42Z"
            }
        },
        {
            "changeId": "9dc352cb-b7c1-4198-9eda-e5e3ed66aec8",
            "beforeSnapshot": {
                "timestamp": "2019-03-28T10:30:19.68Z"
            },
            "afterSnapshot": {
                "timestamp": "2019-03-28T21:12:31.337Z"
            }
        }
    ]
}
```

**RESOURCEID** için algılanan her değişiklik olayı, bu kaynak için benzersiz olan bir **changeıd** değerine sahiptir. **Changeıd** dizesi bazen başka özellikler de içerebilirken, yalnızca benzersiz olması garanti edilir. Değişiklik kaydı, önceki ve sonraki anlık görüntülerin alındığı zamanları içerir.
Bu zaman penceresinde değişiklik olayı bir noktada gerçekleşti.

## <a name="see-what-properties-changed"></a>Bkz. özellikleri değiştirme

**Resourcechanges** uç noktasındaki **Changeıd** Ile, **resourcechangedetails** REST uç noktası daha sonra değişiklik olayının özelliklerini almak için kullanılır.

**Resourcechangedetails** uç noktası, istek gövdesinde iki parametre gerektirir:

- **RESOURCEID**: üzerinde değişiklik aranacak Azure kaynağı.
- **Changeıd**: **resourcechanges**'ten toplanan **RESOURCEID** için benzersiz değişiklik olayı.

Örnek istek gövdesi:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}"
}
```

Yukarıdaki istek gövdesiyle, **Resourcechangedetails** için REST API URI 'si:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChangeDetails?api-version=2018-09-01-preview
```

Yanıt aşağıdaki örneğe benzer şekilde görünür:

```json
{
    "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}",
    "beforeSnapshot": {
        "timestamp": "2019-03-29T01:32:05.993Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "westus",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": false,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "westus",
                "statusOfPrimary": "available"
            }
        }
    },
    "afterSnapshot": {
        "timestamp": "2019-03-29T01:54:24.42Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "westus",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": true,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "westus",
                "statusOfPrimary": "available"
            }
        }
    }
}
```

**Beforesnapshot** ve **aftersnapshot** her biri anlık görüntünün alındığı zamanı ve o anda özellikleri sağlar. Değişiklik, bu anlık görüntüler arasındaki bir noktada gerçekleşti. Yukarıdaki örneğe bakarak, değiştirilen özelliğin **supportsHttpsTrafficOnly**olduğunu görebiliriz.

Sonuçları programlı bir şekilde karşılaştırmak için, her anlık görüntünün **içerik** bölümünü karşılaştırarak farkı saptayın. Anlık görüntünün tamamını karşılaştırırsanız, zaman **damgası** beklenmesine rağmen her zaman fark olarak gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Başlangıç sorgularında](../samples/starter.md)kullanılan dil.
- Gelişmiş [sorgularda](../samples/advanced.md)gelişmiş kullanımlar bölümüne bakın.
- [Kaynakları keşfetmeye](../concepts/explore-resources.md)öğrenin.