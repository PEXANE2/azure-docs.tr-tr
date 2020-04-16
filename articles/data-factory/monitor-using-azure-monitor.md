---
title: Azure Monitör'ü kullanarak veri fabrikalarını izleme
description: Veri Fabrikası'ndan gelen bilgilerle tanı lama günlüklerini etkinleştirerek /Azure Veri Fabrikası ardışık hatlarını izlemek için Azure Monitor'u nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/11/2018
ms.openlocfilehash: 5753336eeef115038de4eb0b5ade0651b1fa293e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419468"
---
# <a name="alert-and-monitor-data-factories-by-using-azure-monitor"></a>Azure Monitör'ü kullanarak veri fabrikalarını uyarın ve izleyin

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bulut uygulamaları karmaşıktır ve birçok hareketli parçaya sahiptir. İzleyiciler, uygulamalarınızın sağlıklı bir durumda çalışır durumda kalmasını sağlamaya yardımcı olacak veriler sağlar. Monitörler ayrıca olası sorunları önlemenize ve geçmiş sorunları gidermenize de yardımcı olur.

Uygulamalarınız hakkında derin bilgiler edinmek için izleme verilerini kullanabilirsiniz. Bu bilgi, uygulama performansını ve bakımı geliştirmenize yardımcı olur. Ayrıca, aksi takdirde el ile müdahale gerektiren eylemleri otomatikleştirmenize de yardımcı olur.

Azure Monitor, çoğu Azure hizmeti için taban düzey altyapı ölçümleri ve günlükleri sağlar. Azure tanı günlükleri bir kaynak tarafından yayılır ve bu kaynağın çalışması hakkında zengin ve sık veri sağlar. Azure Veri Fabrikası da Monitor'da tanılama günlükleri yazar.

Ayrıntılar için [Azure Monitor'a genel bakış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor)bilgisine bakın.

## <a name="keeping-azure-data-factory-data"></a>Azure Veri Fabrikası verilerini tutma

Veri Fabrikası, boru hattı tarafından işletilen verileri yalnızca 45 gün boyunca depolar. Bu verileri daha uzun süre tutmak istiyorsanız Monitör'ü kullanın. Monitor ile tanılama günlüklerini çözümleme için yönlendirebilirsiniz. Seçtiğiniz süre için fabrika bilgilerine sahip olmak için bunları bir depolama hesabında da saklayabilirsiniz.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

* Tanılama günlüklerinizi denetim veya el ile denetim için bir depolama hesabına kaydedin. Gün içinde bekletme süresini belirtmek için tanılama ayarlarını kullanabilirsiniz.
* Günlükleri Azure Etkinlik Hub'larına aktarın. Günlükler bir iş ortağı hizmetine veya Power BI gibi özel bir analiz çözümüne giriş olur.
* Log Analytics ile günlükleri analiz edin.

Günlükleri yayan kaynağın aboneliğinde olmayan bir depolama hesabı veya olay merkezi ad alanı kullanabilirsiniz. Ayarı yapılandıran kullanıcının her iki aboneye de uygun rol tabanlı erişim denetimine (RBAC) erişimi olmalıdır.

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama

### <a name="diagnostic-settings"></a>Tanılama ayarları

İşlem e-yapı dışı kaynaklar için tanılama günlüklerini yapılandırmak için tanılama ayarlarını kullanın. Kaynak denetimi ayarları aşağıdaki özelliklere sahiptir:

* Tanılama günlüklerinin nereye gönderildiğini belirtirler. Bunlara örnek olarak bir Azure depolama hesabı, bir Azure etkinlik merkezi veya Monitör günlükleri verilebilir.
* Hangi günlük kategorilerinin gönderildiğini belirtirler.
* Her günlük kategorisinin bir depolama hesabında ne kadar süreyle tutulması gerektiğini belirtirler.
* Sıfır gün saklama, günlüklerin sınırsız süreyle tutulacağı anlamına gelir. Aksi takdirde, değer 1 ile 2.147.483.647 arasında herhangi bir gün olabilir.
* Bekletme ilkeleri ayarlanır, ancak günlükleri bir depolama hesabında depolama devre dışı bırakılırsa, bekletme ilkelerinin hiçbir etkisi yoktur. Örneğin, bu durum yalnızca Olay Hub'ları veya Monitör günlükleri seçenekleri seçildiğinde gerçekleşebilir.
* Bekletme ilkeleri günde uygulanır. Günler arasındaki sınır gece yarısı Eşgüdümlü Evrensel Saat (UTC) oluşur. Günün sonunda, bekletme ilkesinin ötesindeki günlerin günlükleri silinir. Örneğin, bir günlük bir bekletme ilkeniz varsa, bugün başında dünden önceki günlükler silinir.

### <a name="enable-diagnostic-logs-via-the-azure-monitor-rest-api"></a>Azure Monitor REST API üzerinden tanılama günlüklerini etkinleştirme

#### <a name="create-or-update-a-diagnostics-setting-in-the-monitor-rest-api"></a>Monitor REST API'sinde tanılama ayarı oluşturma veya güncelleştirme

##### <a name="request"></a>İstek

```
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

##### <a name="headers"></a>Üst bilgiler

* `{api-version}` yerine `2016-09-01` yazın.
* Tanılama ayarlarını değiştirmek istediğiniz kaynağın kimliğiyle değiştirin. `{resource-id}` Daha fazla bilgi için Azure [kaynaklarınızı yönetmek için Kaynak gruplarını kullanma'ya](../azure-resource-manager/management/manage-resource-groups-portal.md)bakın.
* Üstbilgiyi `Content-Type` `application/json`.
* Yetkilendirme üstbilgisini Azure Etkin Dizini'nden (Azure AD) aldığınız JSON web belirteciolarak ayarlayın. Daha fazla bilgi [için, kimlik doğrulama isteklerine](../active-directory/develop/authentication-scenarios.md)bakın.

##### <a name="body"></a>Gövde

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
| **depolamaAccountId** |Dize | Tanılama günlükleri göndermek istediğiniz depolama hesabının kaynak kimliği. |
| **serviceBusRuleId** |Dize | Olay Hub'larının akış tanılama günlükleri için oluşturulmasını istediğiniz servis veri günü ad alanının servis-veri günü kuralı kimliği. Kural kimliği biçimine `{service bus resource ID}/authorizationrules/{key name}`sahiptir.|
| **workspaceId** | Karmaşık Tür | Bir dizi metrik zaman taneleri ve bunların bekletme ilkeleri. Bu mülkün değeri boş. |
|**Ölçüm**| Çağrılan ardışık boru hattına geçirilecek boru hattının parametre değerleri| Parametre adlarını bağımsız değişken değerleriyle eşleyen bir JSON nesnesi. |
| **Günlük**| Karmaşık Tür| Kaynak türü için tanılama günlüğü kategorisinin adı. Bir kaynak için tanı-günlük kategorileri listesini almak için, bir GET tanılama ayarları işlemi gerçekleştirin. |
| **Kategori**| Dize| Bir dizi günlük kategorisi ve bunların bekletme ilkeleri. |
| **timeGrain** | Dize | ISO 8601 süre biçiminde yakalanan ölçümlerin parçalılığı. Özellik `PT1M`değeri, bir dakika belirtir olmalıdır. |
| **Etkin**| Boole | Bu kaynak için metrik veya günlük kategorisinin toplanmasının etkin olup olmadığını belirtir. |
| **Retentionpolicy**| Karmaşık Tür| Bir metrik veya günlük kategorisi için bekletme ilkesini açıklar. Bu özellik yalnızca depolama hesapları için kullanılır. |
|**Gün**| int| Ölçümleri veya günlükleri tutmak için gün sayısı. Özellik değeri 0 ise, günlükleri sonsuza kadar tutulur. Bu özellik yalnızca depolama hesapları için kullanılır. |

##### <a name="response"></a>Yanıt

200 tamam.


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

#### <a name="get-information-about-diagnostics-settings-in-the-monitor-rest-api"></a>Monitor REST API'deki tanılama ayarları hakkında bilgi alın

##### <a name="request"></a>İstek

```
GET
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

##### <a name="headers"></a>Üst bilgiler

* `{api-version}` yerine `2016-09-01` yazın.
* Tanılama ayarlarını değiştirmek istediğiniz kaynağın kimliğiyle değiştirin. `{resource-id}` Daha fazla bilgi için Azure [kaynaklarınızı yönetmek için Kaynak gruplarını kullanma'ya](../azure-resource-manager/management/manage-resource-groups-portal.md)bakın.
* Üstbilgiyi `Content-Type` `application/json`.
* Yetkilendirme üstbilgisini Azure AD'den aldığınız Bir JSON web belirteci olarak ayarlayın. Daha fazla bilgi [için, kimlik doğrulama isteklerine](../active-directory/develop/authentication-scenarios.md)bakın.

##### <a name="response"></a>Yanıt

200 tamam.

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
Daha fazla bilgi için [Tanılama Ayarları'na](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)bakın.

## <a name="schema-of-logs-and-events"></a>Günlüklerin ve olayların şeması

### <a name="monitor-schema"></a>Şema izle

#### <a name="activity-run-log-attributes"></a>Etkinlik çalıştıran günlük öznitelikleri

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
| **Düzey** |Dize | Tanılama günlüklerinin düzeyi. Etkinlik çalıştırılanan günlükler için özellik değerini 4 olarak ayarlayın. | `4` |
| **correlationId** |Dize | Belirli bir isteği izlemek için benzersiz kimlik. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | Dize | Zaman dilimi UTC formatında `YYYY-MM-DDTHH:MM:SS.00000Z`olayın saati. | `2017-06-28T21:00:27.3534352Z` |
|**aktiviteRunId**| Dize| Etkinlik çalıştırın kimliği çalıştırın. | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|**boru hattıRunId**| Dize| Boru hattının kimliği. | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|**Resourceıd**| Dize | Veri fabrikası kaynağıyla ilişkili kimlik. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**Kategori**| Dize | Tanılama günlüklerinin kategorisi. Özellik değerini ' `ActivityRuns`ye ayarlayın | `ActivityRuns` |
|**Düzey**| Dize | Tanılama günlüklerinin düzeyi. Özellik değerini ' `Informational`ye ayarlayın | `Informational` |
|**operationName**| Dize | Durumu olan etkinliğin adı. Etkinlik başlangıç sinyali ise, özellik `MyActivity -`değeri. Etkinlik son sinyal ise, özellik `MyActivity - Succeeded`değeri. | `MyActivity - Succeeded` |
|**pipelineName**| Dize | Boru hattının adı. | `MyPipeline` |
|**activityName**| Dize | Etkinliğin adı. | `MyActivity` |
|**Başlatmak**| Dize | Etkinliğin başlangıç saati zaman dilimi UTC biçiminde çalışır. | `2017-06-26T20:55:29.5007959Z`|
|**Son -unda**| Dize | Etkinliğin bitiş saati zaman sonu UTC biçiminde çalışır. Tanılama günlüğü bir etkinliğin başlatılamadığını ancak henüz sona `1601-01-01T00:00:00Z`ermediğini gösteriyorsa, özellik değeri . | `2017-06-26T20:55:29.5007959Z` |

#### <a name="pipeline-run-log-attributes"></a>Ardışık hatlar tarafından çalıştırılan günlük öznitelikleri

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
| **Düzey** |Dize | Tanılama günlüklerinin düzeyi. Etkinlik çalıştırılanan günlükler için özellik değerini 4 olarak ayarlayın. | `4` |
| **correlationId** |Dize | Belirli bir isteği izlemek için benzersiz kimlik. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | Dize | Zaman dilimi UTC formatında `YYYY-MM-DDTHH:MM:SS.00000Z`olayın saati. | `2017-06-28T21:00:27.3534352Z` |
|**runId**| Dize| Boru hattının kimliği. | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|**Resourceıd**| Dize | Veri fabrikası kaynağıyla ilişkili kimlik. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**Kategori**| Dize | Tanılama günlüklerinin kategorisi. Özellik değerini ' `PipelineRuns`ye ayarlayın | `PipelineRuns` |
|**Düzey**| Dize | Tanılama günlüklerinin düzeyi. Özellik değerini ' `Informational`ye ayarlayın | `Informational` |
|**operationName**| Dize | Boru hattının adı ve durumu. Ardışık hatlar çalışması tamamlandıktan `Pipeline - Succeeded`sonra, özellik değeri. | `MyPipeline - Succeeded`. |
|**pipelineName**| Dize | Boru hattının adı. | `MyPipeline` |
|**Başlatmak**| Dize | Etkinliğin başlangıç saati zaman dilimi UTC biçiminde çalışır. | `2017-06-26T20:55:29.5007959Z`. |
|**Son -unda**| Dize | Etkinliğin bitiş saati zaman sonu UTC biçiminde çalışır. Tanılama günlüğü bir etkinliğin başlatılamadığını ancak henüz `1601-01-01T00:00:00Z`sona ermediğini gösteriyorsa, özellik değeri .  | `2017-06-26T20:55:29.5007959Z` |
|**Durum**| Dize | Boru hattı nın son durumu. Olası özellik `Succeeded` değerleri `Failed`ve . | `Succeeded`|

#### <a name="trigger-run-log-attributes"></a>Tetikleyici çalıştır günlük öznitelikleri

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
| **Düzey** |Dize | Tanılama günlüklerinin düzeyi. Etkinlik çalıştırılanan günlükler için özellik değerini 4 olarak ayarlayın. | `4` |
| **correlationId** |Dize | Belirli bir isteği izlemek için benzersiz kimlik. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | Dize | Zaman dilimi UTC formatında `YYYY-MM-DDTHH:MM:SS.00000Z`olayın saati. | `2017-06-28T21:00:27.3534352Z` |
|**triggerId**| Dize| Tetikleme nin kimliği çalıştırın. | `08587023010602533858661257311` |
|**Resourceıd**| Dize | Veri fabrikası kaynağıyla ilişkili kimlik. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**Kategori**| Dize | Tanılama günlüklerinin kategorisi. Özellik değerini ' `PipelineRuns`ye ayarlayın | `PipelineRuns` |
|**Düzey**| Dize | Tanılama günlüklerinin düzeyi. Özellik değerini ' `Informational`ye ayarlayın | `Informational` |
|**operationName**| Dize | Tetikleyicinin son durumu yla birlikte tetikleyicinin adı, tetikleyicinin başarılı bir şekilde çalışıp atMadığını gösterir. Sinyal başarılı olduysa, özellik `MyTrigger - Succeeded`değeri. | `MyTrigger - Succeeded` |
|**triggerName**| Dize | Tetikleyicinin adı. | `MyTrigger` |
|**triggerType**| Dize | Tetikleyicinin türü. Olası özellik `Manual Trigger` değerleri `Schedule Trigger`ve . | `ScheduleTrigger` |
|**triggerOlay**| Dize | Tetikleyici olayı. | `ScheduleTime - 2017-07-06T01:50:25Z` |
|**Başlatmak**| Dize | Zaman dilimi UTC formatında tetiklemenin başlama saati. | `2017-06-26T20:55:29.5007959Z`|
|**Durum**| Dize | Tetikleyicinin başarılı bir şekilde ateşlenip ateşlenmediğini gösteren son durum. Olası özellik `Succeeded` değerleri `Failed`ve . | `Succeeded`|

### <a name="log-analytics-schema"></a>Günlük Analiz şeması

Log Analytics şeayı Aşağıdaki istisnalar dışında Monitör'den devralır:

* Her sütun adındaki ilk harf büyük harfle yazılır. Örneğin, Monitor'daki "correlationId" sütun adı Log Analytics'te "correlationId"dir.
* "Seviye" sütunu yok.
* Dinamik "özellikler" sütunu aşağıdaki dinamik JSON blob türü olarak korunur.

    | Azure Monitör sütunu | Günlük Analizi sütunu | Tür |
    | --- | --- | --- |
    | $.özellikleri. Kullanıcı Özellikleri | Kullanıcı Özellikleri | Dinamik |
    | $.özellikleri. Ek açıklama -ları | Ek Açıklamalar | Dinamik |
    | $.özellikleri. Giriş | Girdi | Dinamik |
    | $.özellikleri. Çıkış | Çıktı | Dinamik |
    | $.özellikleri. Error.errorCode | ErrorCode | int |
    | $.özellikleri. Hata.ileti | Hata İletisi | string |
    | $.özellikleri. Hata | Hata | Dinamik |
    | $.özellikleri. Öncüller | Öncüller | Dinamik |
    | $.özellikleri. Parametre | Parametreler | Dinamik |
    | $.özellikleri. Systemparameters | Systemparameters | Dinamik |
    | $.özellikleri. Etiketler | Etiketler | Dinamik |
    
## <a name="metrics"></a>Ölçümler

Monitör ile Azure iş yüklerinizin performansı ve sistem durumu hakkında görünürlük elde edebilirsiniz. En önemli Monitör veri türü, performans sayacı olarak da adlandırılan metriktir. Ölçümler çoğu Azure kaynağı tarafından yayımlanır. Monitor, izleme ve sorun giderme için bu ölçümleri yapılandırmak ve tüketmek için çeşitli yollar sağlar.

Azure Veri Fabrikası sürüm 2 aşağıdaki ölçümleri yayır.

| **Ölçüm**           | **Metrik görüntü adı**         | **Birim** | **Toplama türü** | **Açıklama**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| PipelineSucceededRuns | Başarılı ardışık ardışık hatlar ölçümleri çalışır | Sayı    | Toplam                | Bir dakika içinde başarılı olan toplam boru hattı çalıştırma sayısı. |
| Boru HattıFailedRuns   | Başarısız ardışık hatlar ölçümleri çalıştırıyor    | Sayı    | Toplam                | Bir dakika içinde başarısız olan toplam ardışık hat lar çalışır.    |
| ActivitySucceededRuns | Başarılı etkinlik ölçümleri çalıştırıyor | Sayı    | Toplam                | Bir dakika içinde başarılı olan toplam etkinlik çalışır.  |
| EtkinlikFailedRuns   | Başarısız etkinlik ölçümleri çalıştırıyor    | Sayı    | Toplam                | Bir dakika içinde başarısız olan toplam etkinlik çalışır.     |
| TriggerSucceededRuns | Başarılı tetikleyici ölçümleri çalıştırıyor  | Sayı    | Toplam                | Bir dakika içinde başarılı olan tetikleyici çalıştırmalarının toplam sayısı.   |
| TriggerFailedRuns    | Başarısız tetikleyici ölçümleri çalıştırıyor     | Sayı    | Toplam                | Bir dakika içinde başarısız olan tetikleyici çalıştırmalarının toplam sayısı.      |

Ölçümlere erişmek için [Azure Monitor veri platformundaki](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics)yönergeleri tamamlayın.

> [!NOTE]
> Yalnızca tamamlanmış, tetiklenen etkinlik ve ardışık hatlar dizisi yayılır. Devam eden ve sandbox/hata ayıklama çalıştırmaları **yayılmıyor.** 

## <a name="monitor-data-factory-metrics-with-azure-monitor"></a>Azure Monitor ile Veri Fabrikası ölçümlerini izleyin

Verileri Monitor'a yönlendirmek için Monitor ile Veri Fabrikası tümleştirmesini kullanabilirsiniz. Bu tümleştirme aşağıdaki senaryolarda yararlıdır:

* Veri Fabrikası tarafından izlenecek şekilde yayınlanan zengin bir ölçüm kümesine karmaşık sorgular yazmak istiyorsunuz. Monitor aracılığıyla bu sorgularda özel uyarılar oluşturabilirsiniz.

* Veri fabrikaları arasında izlemek istiyorsunuz. Verileri birden çok veri fabrikasından tek bir Monitör çalışma alanına yönlendirebilirsiniz.

Yedi dakikalık bir tanıtım ve bu özelliğin tanıtımı için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

### <a name="configure-diagnostic-settings-and-workspace"></a>Tanılama ayarlarını ve çalışma alanını yapılandırma

Veri fabrikanız için tanılama ayarları oluşturun veya ekleyin.

1. Portalda Monitor'a git. **Ayarlar** > **Tanı ayarlarını**seçin.

1. Tanılama ayarını ayarlamak istediğiniz veri fabrikasını seçin.

1. Seçili veri fabrikasında ayar yoksa, bir ayar oluşturmanız istenir. **Tanılamayı Aç'ı**seçin.

   ![Ayar yoksa tanılama ayarı oluşturma](media/data-factory-monitor-oms/monitor-oms-image1.png)

   Veri fabrikasında varolan ayarlar varsa, veri fabrikasında zaten yapılandırılmış ayarların listesini görürsünüz. **Tanıayar ayarı ekle'yi**seçin.

   ![Ayarlar varsa tanılama ayarı ekleme](media/data-factory-monitor-oms/add-diagnostic-setting.png)

1. Ayarınıza bir ad verin, **Log Analytics'e Gönder'i**seçin ve ardından **Log Analytics Çalışma Alanı'ndan**bir çalışma alanı seçin.

    ![Ayarlarınızı adlandırın ve bir günlük analizi çalışma alanı seçin](media/data-factory-monitor-oms/monitor-oms-image2.png)

1. **Kaydet**’i seçin.

Birkaç dakika sonra, yeni ayar bu veri fabrikasının ayarlar listenizde görünür. Tanılama günlükleri, yeni olay verileri oluşturulur oluşturulmaz bu çalışma alanına akışı sağlar. Bir olayın yayımlandığında ve Log Analytics'te görüntülendiğinde 15 dakikaya kadar zaman alabilir.

* _Kaynağa Özel_ modda, Azure Veri Fabrikası'ndan tanı günlükleri _ADFPipelineRun_, _ADFTriggerRun_ve _ADFActivityRun_ tablolarına akar
* _Azure Tanılama_ modunda tanılama günlükleri _AzureDiagnostics_ tablosuna akıtılır

> [!NOTE]
> Azure günlük tablosunda 500'den fazla sütun olmadığından, Kaynağa Özel modu seçmenizi şiddetle tavsiye ediyoruz. Daha fazla bilgi için [bkz.](../azure-monitor/platform/resource-logs-collect-workspace.md#column-limit-in-azurediagnostics)

### <a name="install-azure-data-factory-analytics-from-azure-marketplace"></a>Azure Marketinden Azure Veri Fabrikası Analitiği Yükleme

!["Azure Marketi"ne gidin, "Analytics filtresi"ni girin ve "Azure Veri Fabrikası Analitiği (Önizleme"yi seçin](media/data-factory-monitor-oms/monitor-oms-image3.png)

!["Azure Veri Fabrikası Analitiği (Önizleme)" ile ilgili ayrıntılar](media/data-factory-monitor-oms/monitor-oms-image4.png)

**Oluştur'u** seçin ve ardından **OMS Çalışma Alanı** ve **OMS Çalışma Alanı ayarlarını**seçin.

![Yeni bir çözüm oluşturma](media/data-factory-monitor-oms/monitor-oms-image5.png)

### <a name="monitor-data-factory-metrics"></a>Veri Fabrikası ölçümlerini izleyin

Azure Veri Fabrikası Analitiği'ni yüklemek, aşağıdaki ölçümlerin etkinhale gelmesi için varsayılan bir görünüm kümesi oluşturur:

- ADF Çalışır - 1) Veri Fabrikası tarafından Boru Hattı Çalışır
 
- ADF Çalışır- 2) Veri Fabrikası tarafından Faaliyet Çalışır

- ADF Çalışır - 3) Tetik Veri Fabrikası tarafından Çalışır

- ADF Hataları - 1) Veri Fabrikası'na göre En İyi 10 Boru Hattı Hatası

- ADF Hataları - 2) Veri Fabrikası tarafından En İyi 10 Etkinlik Çalışır

- ADF Hataları - 3) Veri Fabrikası'na Göre En İyi 10 Tetikleyici Hatası

- ADF İstatistikleri - 1) Aktivite Türüne Göre Çalışır

- ADF İstatistikleri - 2) Tetik Tipine Göre Çalışır

- ADF İstatistikleri - 3) Max Pipeline Runs Süresi

!["Çalışma Kitapları (Önizleme)" ve "AzureDataFactoryAnalytics" vurgulu pencere](media/data-factory-monitor-oms/monitor-oms-image6.png)

Önceki ölçümleri görselleştirebilir, bu ölçümlerin arkasındaki sorgulara bakabilir, sorguları düzeltebilir, uyarılar oluşturabilir ve diğer eylemleri yapabilirsiniz.

![Veri fabrikası tarafından boru hattı çalışır Grafik gösterimi"](media/data-factory-monitor-oms/monitor-oms-image8.png)

> [!NOTE]
> Azure Veri Fabrikası Analizi (Önizleme), tanılama günlüklerini _Kaynağa özel_ hedef tablolara gönderir. Aşağıdaki tablolara karşı sorgular yazabilirsiniz: _ADFPipelineRun_, _ADFTriggerRun_ve _ADFActivityRun_.

## <a name="alerts"></a>Uyarılar

Azure portalında oturum açın ve uyarılar oluşturmak için**Uyarıları** **İzle'yi** > seçin.

![Portal menüsündeki uyarılar](media/monitor-using-azure-monitor/alerts_image3.png)

### <a name="create-alerts"></a>Uyarılar Oluştur

1. Yeni bir uyarı oluşturmak için **+ Yeni Uyarı kuralını** seçin.

    ![Yeni uyarı kuralı](media/monitor-using-azure-monitor/alerts_image4.png)

1. Uyarı koşulunu tanımlayın.

    > [!NOTE]
    > Kaynak türü açılır listesine **göre Filtredeki** **Tümü'nü** seçtiğinizden emin olun.

    !["Uyarı koşulunu tanımla" > "Hedef seç" bölmesini açan "Hedef seç" ](media/monitor-using-azure-monitor/alerts_image5.png)

    !["Uyarı koşulunu tanımla" >" "Sinyal mantığını yapılandır" bölmesini açan ölçütleri ekle"](media/monitor-using-azure-monitor/alerts_image6.png)

    !["Sinyal türünü yapılandırma" bölmesi](media/monitor-using-azure-monitor/alerts_image7.png)

1. Uyarı ayrıntılarını tanımlayın.

    ![Uyarı ayrıntıları](media/monitor-using-azure-monitor/alerts_image8.png)

1. Eylem grubunu tanımlayın.

    !["Yeni Eylem grubu" vurgulanmış bir kural oluşturma](media/monitor-using-azure-monitor/alerts_image9.png)

    ![Yeni bir eylem grubu oluşturma](media/monitor-using-azure-monitor/alerts_image10.png)

    ![E-posta, SMS, itme ve ses yapılandırma](media/monitor-using-azure-monitor/alerts_image11.png)

    ![Eylem grubunu tanımlama](media/monitor-using-azure-monitor/alerts_image12.png)

## <a name="next-steps"></a>Sonraki adımlar
[Boru hatlarını programlı olarak izleme ve yönetme](monitor-programmatically.md)
