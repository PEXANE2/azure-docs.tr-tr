---
title: Azure Izleyici 'yi kullanarak veri fabrikalarını izleme | Microsoft Docs
description: Azure Data Factory bilgi ile tanılama günlüklerini etkinleştirerek Data Factory işlem hatlarını izlemek için Azure Izleyici 'yi nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: shlo
ms.openlocfilehash: 6bad74d33f5d50bb7a35de69927bf97daad07798
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326866"
---
# <a name="alert-and-monitor-data-factories-using-azure-monitor"></a>Azure Izleyici kullanarak veri fabrikalarını uyarma ve Izleme
Bulut uygulamaları birçok hareketli parça ile karmaşıktır. İzleme, uygulamanızın sağlıklı durumda kaldığından ve çalıştığından emin olmak için veri sağlar. Ayrıca, olası sorunları veya geçmişteki sorunları gidermenize yardımcı olur. Ayrıca, uygulamanız hakkında derin Öngörüler elde etmek için izleme verilerini de kullanabilirsiniz. Bu bilgi, uygulama performansını veya bakımlılığını iyileştirebilmeniz ya da aksi takdirde el ile müdahale gerektiren eylemleri otomatikleştirmenize yardımcı olabilir.

Azure Izleyici Microsoft Azure ' de çoğu hizmet için temel düzeyde altyapı ölçümleri ve Günlükler sağlar. Ayrıntılar için bkz. [izlemeye genel bakış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor). Azure tanılama günlükleri, bu kaynağın çalışması hakkında zengin ve sık veriler sağlayan bir kaynak tarafından oluşturulan günlüklerdir. Data Factory Azure Izleyici 'de tanılama günlükleri çıkışı verir.

## <a name="persist-data-factory-data"></a>Data Factory verileri kalıcı yap
Data Factory yalnızca 45 gün boyunca işlem hattı çalıştırma verilerini depolar. İşlem hattı çalıştırma verilerini 45 günden daha uzun bir süre boyunca kalıcı hale getirmek istiyorsanız Azure Izleyici 'yi kullanarak yalnızca tanılama günlüklerini analiz için yönlendiremezsiniz, bu sayede seçtiğiniz süre için fabrika bilgilerine sahip olursunuz.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

* Denetim veya el ile inceleme için bunları bir **depolama hesabına** kaydedin. Tanılama ayarlarını kullanarak saklama süresini (gün olarak) belirtebilirsiniz.
* Bunları bir üçüncü taraf hizmeti veya Power BI gibi özel analizler tarafından alma için **Event Hubs** .
* **Log Analytics** ile çözümleyin

Günlükleri yayan kaynakla aynı abonelikte olmayan bir depolama hesabı veya Olay Hub 'ı ad alanı kullanabilirsiniz. Ayarı yapılandıran kullanıcının her iki aboneliğe da uygun rol tabanlı erişim denetimi (RBAC) erişimi olması gerekir.

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama

### <a name="diagnostic-settings"></a>Tanılama ayarları
İşlem dışı kaynaklar için tanılama günlükleri, Tanılama ayarları kullanılarak yapılandırılır. Kaynak denetimi için Tanılama ayarları:

* Tanılama günlüklerinin gönderildiği (depolama hesabı, Event Hubs veya Azure Izleyici günlükleri).
* Hangi günlük kategorilerinin gönderildiği.
* Her günlük kategorisinin bir depolama hesabında ne kadar süreyle korunması gerekir.
* Bekletme günü sayısının sıfır günlükler süresiz olarak tutulur anlamına gelir. Aksi takdirde, değeri herhangi bir sayıda gün 1 ile 2147483647 arasında olabilir.
* Bekletme ilkeleri ayarlanır ancak günlükleri bir depolama hesabında depolamak devre dışı bırakılır (örneğin yalnızca Event Hubs veya Azure Izleyici günlükleri seçenekleri seçilidir), bekletme ilkelerinin hiçbir etkisi yoktur.
* Bekletme ilkeleri uygulanan günlük, olduğundan, bir günün (UTC), şu anda sonra saklama günü günlüklerinden sonunda İlkesi silindi. Örneğin, bir günlük bir bekletme ilkesi olsaydı, bugün günün başında dünden önceki gün kayıtları silinir.

### <a name="enable-diagnostic-logs-via-rest-apis"></a>REST API 'Leri aracılığıyla tanılama günlüklerini etkinleştirme

Azure Izleyici 'de bir tanılama ayarı oluşturun veya güncelleştirin REST API

**İstek**
```
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Üst bilgileri**
* `{api-version}` yerine `2016-09-01` yazın.
* Tanılama `{resource-id}` ayarlarını düzenlemek istediğiniz kaynağın kaynak kimliğiyle değiştirin. [Azure kaynaklarınızı yönetmek Için kaynak gruplarını kullanma](../azure-resource-manager/manage-resource-groups-portal.md)hakkında daha fazla bilgi için.
* `Content-Type` Üstbilgiyi olarak`application/json`ayarlayın.
* Yetkilendirme üst bilgisini, Azure Active Directory aldığınız bir JSON Web belirtecine ayarlayın. Daha fazla bilgi için bkz. [Istekleri kimlik doğrulama](../active-directory/develop/authentication-scenarios.md).

**Gövde**
```json
{
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "metrics": [
        ],
        "logs": [
                {
                    "category": "PipelineRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "TriggerRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "ActivityRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                }
            ]
    },
    "location": ""
}
```

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| storageAccountId |String | Tanılama günlükleri göndermek istediğiniz depolama hesabının kaynak KIMLIĞI |
| serviceBusRuleId |String | ' In, akış tanılama günlükleri için Event Hubs oluşturulmasını istediğiniz Service Bus ad alanının Service Bus kural KIMLIĞI. Kural KIMLIĞI şu biçimdedir: "{Service Bus kaynak KIMLIĞI}/authorizationkuralkuralları/{Key Name}".|
| workspaceId | Karmaşık tür | Ölçüm zaman ve saklama ilkeleri dizisi. Şu anda bu özellik boştur. |
|metrics| Çağrılan işlem hattına geçirilecek işlem hattının parametre değerleri| Bağımsız değişken değerlerine yönelik bir JSON nesnesi eşleme parametre adları |
| logs| Karmaşık tür| Kaynak türü için bir tanılama günlüğü kategorisinin adı. Bir kaynak için tanılama günlük kategorilerinin listesini almak için, önce tanılama ayarlarını al işlemini gerçekleştirin. |
| category| String| Günlük kategorileri ve bunların saklama ilkeleri dizisi |
| timeGrain | String | ISO 8601 Duration biçiminde yakalanan ölçümlerin ayrıntı düzeyi. PT1M (bir dakika) olmalıdır|
| enabled| Boole değeri | Bu ölçüm veya günlük kategorisinin toplanmasını bu kaynak için etkin olup olmadığını belirtir|
| retentionPolicy| Karmaşık tür| Ölçüm veya günlük kategorisi için bekletme ilkesini açıklar. Yalnızca depolama hesabı seçeneği için kullanılır.|
| days| Int| Ölçüm veya günlüklerin saklanacağı gün sayısı. 0 değeri, günlükleri süresiz olarak tutar. Yalnızca depolama hesabı seçeneği için kullanılır. |

**Yanıt**

200 TAMAM


```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```

Azure Izleyici REST API tanılama ayarı hakkında bilgi alın

**İstek**
```
GET
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Üst bilgileri**
* `{api-version}` yerine `2016-09-01` yazın.
* Tanılama `{resource-id}` ayarlarını düzenlemek istediğiniz kaynağın kaynak kimliğiyle değiştirin. Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma hakkında daha fazla bilgi için.
* `Content-Type` Üstbilgiyi olarak`application/json`ayarlayın.
* Yetkilendirme üst bilgisini, Azure Active Directory elde ettiğiniz bir JSON Web Token ayarlayın. Daha fazla bilgi için bkz. istekleri kimlik doğrulama.

**Yanıt**

200 TAMAM

```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.Storage/storageAccounts/azmonlogs",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.EventHub/namespaces/shloeventhub/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/ADF/providers/Microsoft.OperationalInsights/workspaces/mihaipie",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```
[Daha fazla bilgi](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)

## <a name="schema-of-logs--events"></a>Günlük & olaylarının şeması

### <a name="azure-monitor-schema"></a>Azure Izleyici şeması

#### <a name="activity-run-logs-attributes"></a>Etkinlik çalıştırma günlükleri öznitelikleri

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "activityRunId":"",
   "pipelineRunId":"",
   "resourceId":"",
   "category":"ActivityRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "activityName":"",
   "start":"",
   "end":"",
   "properties":
       {
          "Input": "{
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "BlobSink"
              }
           }",
          "Output": "{"dataRead":121,"dataWritten":121,"copyDuration":5,
               "throughput":0.0236328132,"errors":[]}",
          "Error": "{
              "errorCode": "null",
              "message": "null",
              "failureType": "null",
              "target": "CopyBlobtoBlob"
        }
    }
}
```

| Özellik | Tür | Açıklama | Örnek |
| --- | --- | --- | --- |
| Düzey |Dize | Tanılama günlüklerinin düzeyi. Düzey 4 her zaman etkinlik çalıştırma günlükleri için durumdur. | `4`  |
| correlationId |Dize | Belirli bir isteği uçtan uca izlemeye yönelik benzersiz KIMLIK | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | Dize | Olayın zaman aralığı, UTC biçimindeki zamanı`YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|activityRunId| Dize| Etkinlik çalıştırmasının KIMLIĞI | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|pipelineRunId| Dize| İşlem hattı çalıştırmasının KIMLIĞI | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| Dize | Data Factory kaynağı için ilişkili kaynak KIMLIĞI | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Dize | Tanılama günlükleri kategorisi. Bu özelliği "Activityçalıştırmaları" olarak ayarlayın | `ActivityRuns` |
|level| Dize | Tanılama günlüklerinin düzeyi. Bu özelliği "bilgilendirme" olarak ayarlayın | `Informational` |
|operationName| Dize |Durumu olan etkinliğin adı. Durum başlangıç sinyali ise, bu, olur `MyActivity -`. Durum bitiş `MyActivity - Succeeded` sinyaliyle, son durumu ise | `MyActivity - Succeeded` |
|Ardışık Düzen adı| Dize | İşlem hattının adı | `MyPipeline` |
|Özelliğinde| Dize | Etkinliğin adı | `MyActivity` |
|Başlangıç| Dize | Etkinlik zaman aralığı, UTC biçiminde çalışma başlangıcı | `2017-06-26T20:55:29.5007959Z`|
|end| Dize | Etkinlik zaman aralığı, UTC biçiminde çalışır. Etkinlik henüz bitmemişse (bir etkinliğin başlaması için tanılama günlüğü), varsayılan değeri `1601-01-01T00:00:00Z` ayarlanır.  | `2017-06-26T20:55:29.5007959Z` |

#### <a name="pipeline-run-logs-attributes"></a>İşlem hattı çalıştırma günlüğü öznitelikleri

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "runId":"",
   "resourceId":"",
   "category":"PipelineRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "start":"",
   "end":"",
   "status":"",
   "properties":
    {
      "Parameters": {
        "<parameter1Name>": "<parameter1Value>"
      },
      "SystemParameters": {
        "ExecutionStart": "",
        "TriggerId": "",
        "SubscriptionId": ""
      }
    }
}
```

| Özellik | Tür | Açıklama | Örnek |
| --- | --- | --- | --- |
| Düzey |Dize | Tanılama günlüklerinin düzeyi. Düzey 4, etkinlik çalıştırma günlüklerinin durumdur. | `4`  |
| correlationId |Dize | Belirli bir isteği uçtan uca izlemeye yönelik benzersiz KIMLIK | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | Dize | Olayın zaman aralığı, UTC biçimindeki zamanı`YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|RunId| Dize| İşlem hattı çalıştırmasının KIMLIĞI | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| Dize | Data Factory kaynağı için ilişkili kaynak KIMLIĞI | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Dize | Tanılama günlükleri kategorisi. Bu özelliği "ardışık düzen eylemsizlik" olarak ayarlayın | `PipelineRuns` |
|level| Dize | Tanılama günlüklerinin düzeyi. Bu özelliği "bilgilendirme" olarak ayarlayın | `Informational` |
|operationName| Dize |Durumu olan işlem hattının adı. İşlem hattı çalıştırması tamamlandığında, son durumu "işlem hattı-başarılı"| `MyPipeline - Succeeded` |
|Ardışık Düzen adı| Dize | İşlem hattının adı | `MyPipeline` |
|Başlangıç| Dize | Etkinlik zaman aralığı, UTC biçiminde çalışma başlangıcı | `2017-06-26T20:55:29.5007959Z`|
|end| Dize | Etkinliğin sonu TimeSpan, UTC biçiminde çalışır. Etkinlik henüz bitmemişse (bir etkinliğin başlaması için tanılama günlüğü), varsayılan değeri `1601-01-01T00:00:00Z` ayarlanır.  | `2017-06-26T20:55:29.5007959Z` |
|status| Dize | İşlem hattı çalıştırmasının son durumu (başarılı veya başarısız) | `Succeeded`|

#### <a name="trigger-run-logs-attributes"></a>Çalışma günlükleri özniteliklerini tetikleme

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "triggerId":"",
   "resourceId":"",
   "category":"TriggerRuns",
   "level":"Informational",
   "operationName":"",
   "triggerName":"",
   "triggerType":"",
   "triggerEvent":"",
   "start":"",
   "status":"",
   "properties":
   {
      "Parameters": {
        "TriggerTime": "",
       "ScheduleTime": ""
      },
      "SystemParameters": {}
    }
}

```

| Özellik | Tür | Açıklama | Örnek |
| --- | --- | --- | --- |
| Düzey |Dize | Tanılama günlüklerinin düzeyi. Etkinlik çalıştırma günlükleri için düzey 4 ' e ayarlanır. | `4`  |
| correlationId |Dize | Belirli bir isteği uçtan uca izlemeye yönelik benzersiz KIMLIK | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | Dize | Olayın zaman aralığı, UTC biçimindeki zamanı`YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|Triggerıd| Dize| Tetikleyici çalıştırmasının KIMLIĞI | `08587023010602533858661257311` |
|resourceId| Dize | Data Factory kaynağı için ilişkili kaynak KIMLIĞI | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Dize | Tanılama günlükleri kategorisi. Bu özelliği "ardışık düzen eylemsizlik" olarak ayarlayın | `PipelineRuns` |
|level| Dize | Tanılama günlüklerinin düzeyi. Bu özelliği "bilgilendirme" olarak ayarlayın | `Informational` |
|operationName| Dize |Başarıyla başlatılıp tetiklenmediğini son durumu olan tetikleyicinin adı. Sinyal başarılı olduysa "MyTrigger-Succeeded"| `MyTrigger - Succeeded` |
|triggerName| Dize | Tetikleyicinin adı | `MyTrigger` |
|triggerType| Dize | Tetikleyicinin türü (El Ile tetikleyici veya zamanlama tetikleyicisi) | `ScheduleTrigger` |
|triggerEvent| Dize | Tetikleyicinin olayı | `ScheduleTime - 2017-07-06T01:50:25Z` |
|Başlangıç| Dize | Zaman aralığı, UTC biçiminde tetikleme tetiklemesi | `2017-06-26T20:55:29.5007959Z`|
|status| Dize | Tetikleyicinin başarıyla başlatılıp başlatılmayacağını (başarılı veya başarısız) ilişkin nihai durum | `Succeeded`|

### <a name="log-analytics-schema"></a>Log Analytics şeması

Log Analytics, aşağıdaki özel durumlarla birlikte Azure Izleyici 'den şemayı devralır:

* Her sütun adının ilk harfi büyük harfle alınır; Örneğin, Azure Izleyici 'deki *bağıntıkimliği* , Log Analytics Için *CorrelationId* olacaktır.
* Sütun *düzeyi* bırakılacak.
* Dinamik sütun *özellikleri* , AŞAĞıDAKI dinamik JSON blob türü olarak korunur:

    | Azure Izleyici sütunu | Log Analytics sütunu | Type |
    | --- | --- | --- |
    | $. Properties. UserProperties | UserProperties | Dinamik |
    | $. Properties. Açıklamaları | Açıklamaları | Dinamik |
    | $. Properties. Girişinin | Girdi | Dinamik |
    | $. Properties. Çıktıların | Output | Dinamik |
    | $. Properties. Hata. errorCode | hata kodu | int |
    | $. Properties. Hata. ileti | Hata | dize |
    | $. Properties. Hatayla | Hata | Dinamik |
    | $. Properties. Öncül | Öncül | Dinamik |
    | $. Properties. Parametrelere | Parametreler | Dinamik |
    | $. Properties. SystemParameters | SystemParameters | Dinamik |
    | $. Properties. Lerimi | Tags | Dinamik |
    
## <a name="metrics"></a>Ölçümler

Azure Izleyici, Azure 'da iş yüklerinizin performans ve sistem durumu hakkında görünürlük elde etmek için telemetri kullanmanıza olanak sağlar. Azure Telemetri verilerinin en önemli türü, çoğu Azure kaynağı tarafından yayınlanan ölçümleridir (performans sayaçları da denir). Azure Izleyici, izleme ve sorun giderme amacıyla bu ölçümleri yapılandırmak ve kullanmak için çeşitli yollar sağlar.

ADFV2 aşağıdaki ölçümleri yayar:

| **Ölçüm**           | **Ölçüm görünen adı**         | **Birim** | **Toplama türü** | **Açıklama**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| Ardışık düzen Inesucceededçalıştırmaları | Başarılı işlem hattı çalışma ölçümleri | Count    | Toplam                | Toplam işlem hattı çalışması bir dakika penceresinde başarılı oldu |
| Pipelinefailedçalıştırmaları   | Başarısız işlem hattı çalıştırmaları ölçümleri    | Count    | Toplam                | Toplam işlem hattı çalışması bir dakika penceresinde başarısız oldu    |
| ActivitySucceededRuns | Başarılı etkinlik ölçümleri çalıştırıyor | Count    | Toplam                | Toplam etkinlik çalıştırmaları bir dakika penceresinde başarılı oldu  |
| Activityfailedçalıştırmaları   | Başarısız etkinlik çalıştırma ölçümleri    | Count    | Toplam                | Bir dakika penceresinde başarısız olan toplam etkinlik çalıştırması     |
| TriggerSucceededRuns | Başarılı tetikleyici çalışan ölçümleri  | Count    | Toplam                | Dakikada toplam tetikleyici çalıştırması başarılı oldu   |
| Triggerfailedçalıştırmaları    | Başarısız tetikleyici çalıştırma ölçümleri     | Count    | Toplam                | Bir dakika penceresi içinde toplam tetikleyici çalıştırması başarısız oldu      |

Ölçümlere erişmek için [Azure izleyici veri platformundaki](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics)yönergeleri doldurun.

## <a name="monitor-data-factory-metrics-with-azure-monitor"></a>Azure Izleyici ile Data Factory ölçümlerini izleme

Verileri Azure Izleyici 'ye yönlendirmek için Azure Izleyici ile Azure Data Factory tümleştirme kullanabilirsiniz. Bu tümleştirme, aşağıdaki senaryolarda faydalıdır:

1.  Azure Izleyici 'ye Data Factory tarafından yayınlanan zengin bir ölçüm kümesine karmaşık sorgular yazmak istiyorsunuz. Ayrıca, bu sorgular üzerinde Azure Izleyici aracılığıyla özel uyarılar oluşturabilirsiniz.

2.  Veri fabrikaları genelinde izlemek istiyorsunuz. Birden çok veri fabrikasına ait verileri tek bir Azure Izleyici çalışma alanına yönlendirebilirsiniz.

Bu özelliğin yedi dakikalık bir girişi ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

### <a name="configure-diagnostic-settings-and-workspace"></a>Tanılama ayarlarını ve çalışma alanını yapılandırma

Veri fabrikanızın tanılama ayarlarını etkinleştirin.

1. Portalda Azure Izleyicisi ' ne gidin ve **Ayarlar** menüsünde **Tanılama ayarları** ' na tıklayın.

2. Tanılama ayarı ayarlamak istediğiniz veri fabrikasını seçin.
    
3. Seçtiğiniz veri fabrikasında hiçbir ayar yoksa, bir ayar oluşturmanız istenir. "Tanılamayı Aç" tıklayın

   ![Tanılama ayarını - mevcut hiçbir ayar Ekle](media/data-factory-monitor-oms/monitor-oms-image1.png)

   Data Factory 'de mevcut ayarlar varsa, bu veri fabrikasında zaten yapılandırılmış ayarların bir listesini görürsünüz. "Tanılama ayarı Ekle" ye tıklayın

   ![Tanılama ayarını - var olan ayarları Ekle](media/data-factory-monitor-oms/add-diagnostic-setting.png)

4. Ayarınızı bir ad verin ve **Log Analytics gönder**kutusunu işaretleyin ve ardından bir Log Analytics çalışma alanı seçin.

    ![Monitor-OMS-image2. png](media/data-factory-monitor-oms/monitor-oms-image2.png)

5. **Kaydet**’e tıklayın.

Birkaç dakika sonra, yeni ayar bu veri fabrikasının ayarları listenizde görünür ve tanılama günlükleri, yeni olay verileri oluşturulduktan hemen sonra bu çalışma alanına akışla kaydedilir. Bir olayın ne zaman yayıldığını ve Log Analytics görünme süresi arasında 15 dakika sürebilir.

> [!NOTE]
> Belirli bir Azure günlük tablosu 500 ' den fazla sütuna sahip olmayan açık bir sınır nedeniyle **kaynağa özgü mod kullanılması önemle önerilir**. Daha fazla bilgi için bkz. [Log Analytics bilinen sınırlamalar](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-stream-log-store#known-limitation-column-limit-in-azurediagnostics).

### <a name="install-azure-data-factory-analytics-from-azure-marketplace"></a>Azure Marketi 'nden Azure Data Factory Analytics 'i yükler

![Monitor-OMS-image3. png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![Monitor-OMS-image4. png](media/data-factory-monitor-oms/monitor-oms-image4.png)

**Oluştur** ' a tıklayın ve çalışma alanı ve çalışma alanı ayarlarını seçin.

![Monitor-OMS-image5. png](media/data-factory-monitor-oms/monitor-oms-image5.png)

### <a name="monitor-data-factory-metrics"></a>Data Factory ölçümlerini izleme

**Azure Data Factory Analytics** 'i yüklemek, aşağıdaki ölçümleri sağlayan varsayılan bir görünüm kümesi oluşturur:

- ADF çalıştırmaları-1) Data Factory tarafından işlem hattı çalıştırmaları

- ADF çalıştırmalar-2) Data Factory tarafından çalışan etkinlik çalıştırmaları

- ADF çalıştırmalar-3) Data Factory tarafından çalıştırılan tetikler

- ADF hataları-1) Data Factory göre Ilk 10 ardışık düzen hatası

- ADF hataları-2) Data Factory tarafından Ilk 10 etkinlik çalıştırılır

- ADF hataları-3) Ilk 10 Data Factory tarafından hata tetikleyin

- ADF Istatistikleri-1) türe göre etkinlik çalıştırmaları

- ADF Istatistikler-2) türe göre tetikleme çalıştırmaları

- ADF Istatistikleri-3) en fazla işlem hattı çalıştırma süresi

![Monitor-OMS-image6. png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![Monitor-OMS-image7. png](media/data-factory-monitor-oms/monitor-oms-image7.png)

Yukarıdaki ölçümleri görselleştirin, bu ölçümlerin arkasındaki sorgulara bakabilir, sorguları düzenleyebilir, uyarılar oluşturabilir ve benzeri adımları izleyebilirsiniz.

![Monitor-OMS-image8. png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="alerts"></a>Uyarılar

Azure Portal oturum açın ve uyarı oluşturmak için**uyarıları** **İzle** > ' ye tıklayın.

![Portal menüsündeki uyarılar](media/monitor-using-azure-monitor/alerts_image3.png)

### <a name="create-alerts"></a>Uyarı oluşturma

1.  Yeni bir uyarı oluşturmak için **+ Yeni uyarı kuralı** ' na tıklayın.

    ![Yeni uyarı kuralı](media/monitor-using-azure-monitor/alerts_image4.png)

2.  **Uyarı koşulunu**tanımlayın.

    > [!NOTE]
    > **Kaynak türüne göre filtrele**' de **Tümünü** seçtiğinizden emin olun.

    ![Uyarı koşulu, ekran 1/3](media/monitor-using-azure-monitor/alerts_image5.png)

    ![Uyarı koşulu, ekran 2/3](media/monitor-using-azure-monitor/alerts_image6.png)

    ![Uyarı koşulu, ekran 3/3](media/monitor-using-azure-monitor/alerts_image7.png)

3.  **Uyarı ayrıntılarını**tanımlayın.

    ![Uyarı ayrıntıları](media/monitor-using-azure-monitor/alerts_image8.png)

4.  **Eylem grubunu**tanımlayın.

    ![Eylem grubu, ekran 1/4](media/monitor-using-azure-monitor/alerts_image9.png)

    ![Eylem grubu, ekran 2/4](media/monitor-using-azure-monitor/alerts_image10.png)

    ![Eylem grubu, ekran 3/4](media/monitor-using-azure-monitor/alerts_image11.png)

    ![Eylem grubu, ekran 4/4](media/monitor-using-azure-monitor/alerts_image12.png)

## <a name="next-steps"></a>Sonraki adımlar

Kod ile işlem hatlarını izleme ve yönetme hakkında bilgi edinmek için bkz. işlem [hatları programlama yoluyla izleme ve yönetme](monitor-programmatically.md) .
