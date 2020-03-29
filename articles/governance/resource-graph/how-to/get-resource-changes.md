---
title: Kaynak değişikliklerini alma
description: Kaynağın ne zaman değiştirildiğini nasıl bulacağımı anlayın, değiştirilen özelliklerin listesini alın ve diffs'i değerlendirin.
ms.date: 10/09/2019
ms.topic: how-to
ms.openlocfilehash: 9504ac77fc4a3b03434912cc65284e2001df6e03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873038"
---
# <a name="get-resource-changes"></a>Kaynak değişikliklerini alma

Kaynaklar, günlük kullanım, yeniden yapılandırma ve hatta yeniden dağıtım sırasında değiştirilir.
Değişiklik bir bireyden veya otomatik bir işlemle gelebilir. Çoğu değişiklik tasarım gereğidir, ama bazen değildir. Azure Kaynak Grafiği, değişiklik geçmişinin son 14 gününde şunları yapmanızı sağlar:

- Azure Kaynak Yöneticisi özelliğinde değişikliklerin ne zaman algılandığını bulma
- Her kaynak değişikliği için özellik değişikliği ayrıntılarına bakın
- Algılanan değişiklikten önce ve sonra kaynağın tam karşılaştırmasını görün

Değişiklik algılama ve ayrıntılar aşağıdaki örnek senaryolar için değerlidir:

- Olay yönetimi sırasında _potansiyel olarak_ ilgili değişiklikleri anlamak için. Belirli bir zaman penceresinde olayları değiştirme sorgusu ve değişiklik ayrıntılarını değerlendirin.
- CMDB olarak bilinen Yapılandırma Yönetimi Veritabanını güncel tutmak. Tüm kaynakları ve tüm mülk kümelerini zamanlanmış bir frekansta yenilemek yerine, yalnızca değişenleri alın.
- Kaynak uyumluluk durumunu değiştirdiğinde diğer özelliklerin değiştirilmiş olabileceğini anlama. Bu ek özelliklerin değerlendirilmesi, Azure İlkesi tanımı yla yönetilmesi gereken diğer özelliklere ilişkin öngörüler sağlayabilir.

Bu makalede, kaynak grafiğiSDK aracılığıyla bu bilgilerin nasıl toplandığı gösterilmektedir. Bu bilgileri Azure portalında görmek için Azure İlkesi'nin [Değişiklik geçmişi](../../policy/how-to/determine-non-compliance.md#change-history-preview) veya Azure Etkinlik Günlüğü [Değişikliği geçmişi'ne](../../../azure-monitor/platform/activity-log-view.md#azure-portal)bakın.
Altyapı katmanından uygulama dağıtımına kadar uygulamalarınızda yapılan değişikliklerle ilgili ayrıntılar için Azure Monitor'da [Uygulama Değişikliği Çözümlemesi (önizleme) kullanın'](../../../azure-monitor/app/change-analysis.md) a bakın.

> [!NOTE]
> Kaynak Grafiği'ndeki değişiklik ayrıntıları Kaynak Yöneticisi özellikleri içindir. Sanal bir makinedeki değişiklikleri izlemek için Azure Automation'ın [Değişiklik izleme](../../../automation/automation-change-tracking.md) veya Azure İlkesi'nin [VM'ler için Konuk](../../policy/concepts/guest-configuration.md)Yapılandırması'na bakın.

> [!IMPORTANT]
> Azure Kaynak Grafiği'ndeki değişiklik geçmişi Genel Önizleme'dedir.

## <a name="find-detected-change-events-and-view-change-details"></a>Algılanan değişiklik olaylarını bulma ve değişiklik ayrıntılarını görüntüleme

Bir kaynakta neyin değiştiğini görmenin ilk adımı, zaman içinde o kaynakla ilgili değişiklik olaylarını bulmaktır. Her değişiklik olayı, kaynakta nelerin değiştiğine ilişkin ayrıntıları da içerir. Bu adım kaynak **Changes** REST bitiş noktası üzerinden yapılır.

**kaynak Changes** bitiş noktası istek gövdesinde aşağıdaki parametreleri kabul eder:

- **sourceId** \[\]gerekli : Azure kaynağı üzerinde değişiklik aramak için.
- **interval** \[gerekli\]aralık : **Zulu Saat Dilimini (Z)** kullanarak bir değişiklik olayının ne zaman denetlenecek lerine göre _başlangıç_ ve _bitiş_ tarihleri olan bir özellik.
- **fetchPropertyChanges** (isteğe bağlı): Yanıt nesnesi özellik değişiklikleri içeriyorsa ayarlayan bir boolean özelliği.

Örnek istek gövdesi:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "interval": {
        "start": "2019-09-28T00:00:00.000Z",
        "end": "2019-09-29T00:00:00.000Z"
    },
    "fetchPropertyChanges": true
}
```

Yukarıdaki istek gövdesi ile, **kaynak Değişiklikleri** için REST API URI:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChanges?api-version=2018-09-01-preview
```

Yanıt bu örneğe benzer:

```json
{
    "changes": [
        {
            "changeId": "{\"beforeId\":\"3262e382-9f73-4866-a2e9-9d9dbee6a796\",\"beforeTime\":\"2019-09-28T00:45:35.012Z\",\"afterId\":\"6178968e-981e-4dac-ac37-340ee73eb577\",\"afterTime\":\"2019-09-28T00:52:53.371Z\"}",
            "beforeSnapshot": {
                "snapshotId": "3262e382-9f73-4866-a2e9-9d9dbee6a796",
                "timestamp": "2019-09-28T00:45:35.012Z"
            },
            "afterSnapshot": {
                "snapshotId": "6178968e-981e-4dac-ac37-340ee73eb577",
                "timestamp": "2019-09-28T00:52:53.371Z"
            },
            "changeType": "Create"
        },
        {
            "changeId": "{\"beforeId\":\"a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c\",\"beforeTime\":\"2019-09-28T00:43:38.366Z\",\"afterId\":\"3262e382-9f73-4866-a2e9-9d9dbee6a796\",\"afterTime\":\"2019-09-28T00:45:35.012Z\"}",
            "beforeSnapshot": {
                "snapshotId": "a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c",
                "timestamp": "2019-09-28T00:43:38.366Z"
            },
            "afterSnapshot": {
                "snapshotId": "3262e382-9f73-4866-a2e9-9d9dbee6a796",
                "timestamp": "2019-09-28T00:45:35.012Z"
            },
            "changeType": "Delete"
        },
        {
            "changeId": "{\"beforeId\":\"b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c\",\"beforeTime\":\"2019-09-28T00:43:15.518Z\",\"afterId\":\"a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c\",\"afterTime\":\"2019-09-28T00:43:38.366Z\"}",
            "beforeSnapshot": {
                "snapshotId": "b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c",
                "timestamp": "2019-09-28T00:43:15.518Z"
            },
            "afterSnapshot": {
                "snapshotId": "a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c",
                "timestamp": "2019-09-28T00:43:38.366Z"
            },
            "propertyChanges": [
                {
                    "propertyName": "tags.org",
                    "afterValue": "compute",
                    "changeCategory": "User",
                    "changeType": "Insert"
                },
                {
                    "propertyName": "tags.team",
                    "afterValue": "ARG",
                    "changeCategory": "User",
                    "changeType": "Insert"
                }
            ],
            "changeType": "Update"
        },
        {
            "changeId": "{\"beforeId\":\"19d12ab1-6ac6-4cd7-a2fe-d453a8e5b268\",\"beforeTime\":\"2019-09-28T00:42:46.839Z\",\"afterId\":\"b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c\",\"afterTime\":\"2019-09-28T00:43:15.518Z\"}",
            "beforeSnapshot": {
                "snapshotId": "19d12ab1-6ac6-4cd7-a2fe-d453a8e5b268",
                "timestamp": "2019-09-28T00:42:46.839Z"
            },
            "afterSnapshot": {
                "snapshotId": "b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c",
                "timestamp": "2019-09-28T00:43:15.518Z"
            },
            "propertyChanges": [{
                "propertyName": "tags.cgtest",
                "afterValue": "hello",
                "changeCategory": "User",
                "changeType": "Insert"
            }],
            "changeType": "Update"
        }
    ]
}
```

**resourceId** için algılanan her değişiklik olayı aşağıdaki özelliklere sahiptir:

- **changeId** - Bu değer bu kaynağa özgüdür. **ChangeId** dizesi bazen başka özellikler içerebilir, ancak yalnızca benzersiz olacağı garanti edilir.
- **beforeSnapshot** - Değişiklik algılandıktan önce alınan kaynak anlık görüntüsünün **anlık** görüntüsünü ve **zaman damgasını** içerir.
- **afterSnapshot** - Bir değişiklik algılandıktan sonra alınan kaynak anlık görüntüsünün **anlık** görüntüsünü ve **zaman damgasını** içerir.
- **changeType** - Önceki Snapshot ve **afterSnapshot** arasındaki tüm değişiklik **afterSnapshot**kaydı için algılanan değişiklik türünü açıklar. Değerler şunlardır: _Oluştur,_ _Güncelleştir_ve _Sil_. **özellikDeğişiklikleri** özellik dizisi yalnızca **changeType** _Güncelleştirildiğinde_eklenir.
- **özelliklerDeğişiklikler** - Bu özellik **dizisi, önceki Snapshot** ve **afterSnapshot**arasında güncelleştirilen tüm kaynak özelliklerinin ayrıntılarını verir:
  - **propertyName** - Değiştirilen kaynak özelliğinin adı.
  - **changeCategory** - Değişikliği neyin yaptığını açıklar. Değerler şunlardır: _Sistem_ ve _Kullanıcı_.
  - **changeType** - Tek tek kaynak özelliği için algılanan değişiklik türünü açıklar.
    Değerler şunlardır: _Ekle_, _Güncelleştir_, _Kaldır_.
  - **beforeValue** - **öncekiSnapshot'taki**kaynak özelliğinin değeri. **changeType** _Insert_olduğunda görüntülenmez.
  - **afterValue** - **AfterSnapshot'taki**kaynak özelliğinin değeri. **ChangeType** _Kaldır_olduğunda görüntülenmez.

## <a name="compare-resource-changes"></a>Kaynak değişikliklerini karşılaştırma

**Kaynak Changes** **endpoint'teki changeId** **ile, kaynak ChangeDetails** REST bitiş noktası, değiştirilen kaynağın önceki ve sonraki anlık görüntülerini almak için kullanılır.

**kaynakChangeDetails** bitiş noktası istek gövdesinde iki parametre gerektirir:

- **resourceId**: Değişiklikleri karşılaştırmak için Azure kaynağı.
- **changeId**: **Kaynak Değişiklikleri'nden**toplanan **kaynakId** için benzersiz değişiklik olayı.

Örnek istek gövdesi:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"afterTime\":'2019-05-10T00:00:00.000Z\"}"
}
```

Yukarıdaki istek gövdesi ile, **kaynak ChangeDetails** için REST API URI:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChangeDetails?api-version=2018-09-01-preview
```

Yanıt bu örneğe benzer:

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

**önceki Snapshot** ve **afterSnapshot** her anlık görüntü ve o zaman özellikleri alınmıştır zaman verir. Değişiklik, bu anlık görüntüler arasında bir noktada gerçekleşti. Yukarıdaki örneğe baktığımızda, değişen özelliğin **httpsTrafficOnly'u desteklediğini**görebiliriz.

Sonuçları karşılaştırmak için, **kaynak Değişiklikleri'ndeki** **değişiklik** özelliğini kullanın veya farkı belirlemek için **kaynakChangeDetails'daki** her anlık görüntünün **içerik** bölümünü değerlendirin. Anlık görüntüleri karşılaştırırsanız, **zaman damgası** beklenen olmasına rağmen her zaman bir fark olarak gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- [Başlangıç sorgularında](../samples/starter.md)kullanılan dile bakın.
- [Gelişmiş sorgularda](../samples/advanced.md)gelişmiş kullanımlara bakın.
- Kaynakları nasıl [keşfedebilirsiniz](../concepts/explore-resources.md)hakkında daha fazla bilgi edinin.