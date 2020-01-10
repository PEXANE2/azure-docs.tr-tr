---
title: Azure platformu günlüklerini bir olay hub 'ına akış
description: Üçüncü taraf Sıems ve diğer Log Analytics çözümleri gibi dış sistemlere veri göndermek için Azure Kaynak günlüklerinin bir olay hub 'ına akışını nasıl sağlayacağınızı öğrenin.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/15/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 00dcc1c1a1d823ab0f2497e47641916d391ee37b
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75750348"
---
# <a name="stream-azure-platform-logs-to-azure-event-hubs"></a>Azure platformu günlüklerini Azure Event Hubs akış
Azure etkinlik günlüğü ve kaynak günlükleri dahil olmak üzere Azure 'daki [Platform günlükleri](platform-logs-overview.md) , Azure kaynakları ve bağımlı oldukları Azure platformu için ayrıntılı tanılama ve denetim bilgileri sağlar.  Bu makalede, üçüncü taraf Sıems ve diğer Log Analytics çözümleri gibi dış sistemlere veri göndermek için Olay Hub 'larına yönelik akış platformu günlükleri açıklanır.


## <a name="what-you-can-do-with-platform-logs-sent-to-an-event-hub"></a>Bir olay hub 'ına gönderilen platform günlükleriyle neler yapabilirsiniz?
Aşağıdaki işlevleri sağlamak için Azure 'daki Olay Hub 'larına platform günlüklerini akışla.

* Günlük verilerini üçüncü taraf bir SıEM veya Log Analytics aracına yöneltmek için tüm platform günlüklerinizi tek bir olay hub **'ına akışa alma** .
  
* **Özel bir telemetri ve günlüğe kaydetme platformu oluşturma** – Olay Hub 'larının yüksek düzeyde ölçeklenebilir yayımla-abone olma yapısı, platform günlüklerini özel bir teletry platformunda esnek bir şekilde içe almanıza olanak sağlar. Ayrıntılar için bkz. [Event Hubs Azure 'Da küresel ölçekli telemetri platformunu tasarlama ve boyutlandırma](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/) .

* **Power BI veri akışı yaparak hizmet durumunu görüntüleyin** – tanılama verilerinizi Azure hizmetlerinizden neredeyse gerçek zamanlı içgörüler halinde dönüştürmek için Event Hubs, Stream Analytics ve Power BI kullanın. Bkz. [Stream Analytics ve Power BI: Bu çözümdeki Ayrıntılar için veri akışı için gerçek zamanlı analiz panosu](../../stream-analytics/stream-analytics-power-bi-dashboard.md) .

    Aşağıdaki SQL kodu, ' deki tüm günlük verilerini bir Power BI tablosuna ayrıştırmak için kullanabileceğiniz bir örnek Stream Analytics sorgusudur:
    
    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the Power BI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

## <a name="prerequisites"></a>Ön koşullar
Henüz bir tane yoksa [bir olay hub 'ı oluşturmanız](../../event-hubs/event-hubs-create.md) gerekir. Bu Event Hubs ad alanını kullanan bir tanılama ayarınız zaten varsa, bu olay hub 'ı yeniden kullanılacaktır.

Ad alanı için paylaşılan erişim ilkesi, akış mekanizmanın sahip olduğu izinleri tanımlar. Event Hubs akışının yönetilmesi, gönderilmesi ve dinlemesi izinlerinin olması gerekir. Event Hubs ad alanınız için Yapılandır sekmesinin altındaki Azure portal paylaşılan erişim ilkeleri oluşturabilir veya değiştirebilirsiniz.

Tanılama ayarını akış içerecek şekilde güncelleştirmek için, bu Event Hubs yetkilendirme kuralında ListKey izninizin olması gerekir. Ayarı yapılandıran kullanıcının her iki aboneliğe ve her iki aboneliğe de aynı AAD kiracısına sahip olması koşuluyla, Event Hubs ad alanının, günlükleri yayan abonelikle aynı abonelikte olması gerekmez.

## <a name="create-a-diagnostic-setting"></a>Tanılama ayarı oluştur
Bir Azure kaynağı için tanılama ayarı oluşturarak, platform günlüklerini bir olay hub 'ına ve diğer hedeflere gönderin. Ayrıntılar için bkz. [Azure 'da günlükleri ve ölçümleri toplamak için tanılama ayarı oluşturma](diagnostic-settings.md) .

## <a name="collect-data-from-compute-resources"></a>İşlem kaynaklarından veri toplama
Tanılama ayarları, Azure işlem kaynakları için, Konuk işletim sistemleri veya iş yükleri değil, diğer kaynaklar gibi kaynak günlüklerini toplar. Bu verileri toplamak için [Log Analytics aracısını](log-analytics-agent.md)yüklersiniz. 


## <a name="consuming-log-data-from-event-hubs"></a>Olay Hub 'larından günlük verilerini kullanma
Olay Hub 'larından platform günlükleri, JSON biçiminde aşağıdaki tablodaki öğelerle kullanılır.

| Öğe Adı | Açıklama |
| --- | --- |
| kayıtlar |Bu yükteki tüm günlük olaylarının bir dizisi. |
| time |Olayın gerçekleştiği zaman. |
| category |Bu olay için günlük kategorisi. |
| resourceId |Bu olayı oluşturan kaynağın kaynak KIMLIĞI. |
| operationName |İşlemin adı. |
| level |İsteğe bağlı. Günlük olayı düzeyini gösterir. |
| properties |Etkinliğin özellikleri. Bunlar, bölümünde [ ]()açıklandığı gibi her bir Azure hizmeti için farklılık gösterir. |


Aşağıda bir kaynak günlüğü için Event Hubs örnek çıkış verileri verilmiştir:

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```



## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak günlükleri hakkında daha fazla bilgi edinin](platform-logs-overview.md).
* [Azure 'da günlüklerin ve ölçümlerin toplanması için tanılama ayarı oluşturun](diagnostic-settings.md).
* [Azure izleyici ile Azure Active Directory günlükleri akışı](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md).
* [Event Hubs kullanmaya başlayın](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md).

