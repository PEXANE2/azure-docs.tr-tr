---
title: Azure platform günlüğünü bir etkinlik merkezine aktarın
description: Üçüncü taraf SIEM'ler ve diğer günlük analiz çözümleri gibi harici sistemlere veri göndermek için Azure kaynak günlüklerini bir etkinlik merkezine nasıl aktartınız öğrenin.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/15/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 72341b6da0068ba4b7e3f53b08e6015cafb70f09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658923"
---
# <a name="stream-azure-platform-logs-to-azure-event-hubs"></a>Azure platform günlüklerini Azure Etkinlik Hub'larına aktarın
Azure Etkinliği günlüğü ve kaynak günlükleri de dahil olmak üzere Azure'daki [platform günlükleri,](platform-logs-overview.md) Azure kaynakları ve bağlı oldukları Azure platformu için ayrıntılı tanılama ve denetleme bilgileri sağlar.  Bu makalede, üçüncü taraf SIEM'ler ve diğer günlük analiz çözümleri gibi dış sistemlere veri göndermek için olay merkezlerine akış platformu günlükleri açıklanmaktadır.


## <a name="what-you-can-do-with-platform-logs-sent-to-an-event-hub"></a>Etkinlik merkezine gönderilen platform günlükleriyle yapabileceklerin
Aşağıdaki işlevselliği sağlamak için platform oturumlarını etkinlik merkezlerine aktarın:

* **Üçüncü taraf günlük ve telemetri sistemlerine akış günlükleri** – Tüm platform günlüklerinizi tek bir etkinlik hub'ına aktarın ve günlük verilerini üçüncü taraf bir SIEM veya günlük analiz aracına aktarın.
  
* **Özel bir telemetri ve günlük platformu oluşturun** – Etkinlik hub'larının yüksek ölçeklenebilir yayımlama-abone yapısı, platform günlüklerini özel bir teletry platformuna esnek bir şekilde yutmanızı sağlar. Ayrıntılar [için Azure Etkinlik Hub'larında Küresel Ölçekli Bir Telemetri Platformu Tasarlama ve Boyutlandırma](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/) bölümüne bakın.

* Tanılama verilerinizi Azure hizmetlerinizle ilgili neredeyse gerçek zamanlı öngörülere dönüştürmek için Etkinlik Hub'larını, Akış Analizlerini ve Power BI'yi kullanarak **veri akışı sağlayarak hizmet durumunu görüntüleyin.** Bkz. Akış Analizi ve Power BI: Bu çözümle ilgili ayrıntılar [için veri akışı için gerçek zamanlı analiz panosu.](../../stream-analytics/stream-analytics-power-bi-dashboard.md)

    Aşağıdaki SQL kodu, tüm günlük verilerini Power BI tablosunda ayrıştırmak için kullanabileceğiniz örnek bir Akış Analizi sorgusudur:
    
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
Zaten bir olay hub'ınyoksa [bir olay hub'ı oluşturmanız](../../event-hubs/event-hubs-create.md) gerekir. Bu Olay Hub'ları ad alanını kullanarak zaten bir tanılama ayarın varsa, bu olay hub'ı yeniden kullanılır.

Ad alanının paylaşılan erişim ilkesi, akış mekanizmasının sahip olduğu izinleri tanımlar. Olay Hub'larına akış için Yönetim, Gönderme ve Dinleme izinleri gerektirir. Azure portalında, Etkinlik Hub'larınız ad alanınız için Yapılandırma sekmesi altında paylaşılan erişim ilkeleri oluşturabilir veya değiştirebilirsiniz.

Tanılama ayarını akış içerecek şekilde güncelleştirmek için, bu Olay Hub'ları yetkilendirme kuralında ListKey iznine sahip olmalısınız. Etkinlik Hub'ları ad alanı, ayarı yapılandıran kullanıcının her iki aboneye de uygun RBAC erişimine sahip olması ve her iki aboneliğin de aynı AAD kiracısında olması kaydıyla, günlükleri yayan abonelikle aynı abonelikte olması gerekmez.

## <a name="create-a-diagnostic-setting"></a>Tanılama ayarı oluşturma
Azure kaynağı için tanılama ayarı oluşturarak platform günlüklerini bir etkinlik merkezine ve diğer hedeflere gönderin. Ayrıntılar [için Azure'da günlükleri ve ölçümleri toplamak için tanı lama ayarını oluştur'a](diagnostic-settings.md) bakın.

## <a name="collect-data-from-compute-resources"></a>Bilgi işlem kaynaklarından veri toplama
Tanılama ayarları, diğer kaynaklar gibi Azure bilgi işlem kaynakları için kaynak günlükleri toplar, ancak konuk işletim sistemleri veya iş yükleri toplanmaz. Bu verileri toplamak için [Log Analytics aracısını](log-analytics-agent.md)yükleyin. 


## <a name="consuming-log-data-from-event-hubs"></a>Olay merkezlerinden günlük verilerini tüketme
Etkinlik merkezlerinden gelen platform günlükleri aşağıdaki tablodaki öğelerle JSON formatında tüketilir.

| Öğe Adı | Açıklama |
| --- | --- |
| Kayıt |Bu yükteki tüm günlük olayları dizisi. |
| time |Olayın meydana geldiği saat. |
| category |Bu etkinlik için günlük kategorisi. |
| resourceId |Bu olayı oluşturan kaynağın kaynak kimliği. |
| operationName |Operasyonun adı. |
| düzey |İsteğe bağlı. Günlük olay düzeyini gösterir. |
| properties |Olayın özellikleri. Bunlar, ''de [ ]()açıklandığı gibi her Azure hizmeti için değişir. |


Aşağıda, kaynak günlüğü için Olay Hub'larından örnek çıktı verileri vereme

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

* [Kaynak günlükleri hakkında daha fazla bilgi edinin.](platform-logs-overview.md)
* [Azure'da günlükleri ve ölçümleri toplamak için tanılama ayarı oluşturun.](diagnostic-settings.md)
* [Azure Monitor ile Azure Etkin Dizin günlüklerini akışla](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md)aktarın.
* [Etkinlik Hub'ları ile başlayın.](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

