---
title: Azure Hizmet Durumu bildirimlerini webhooks üzerinden gönderme
description: Mevcut sorun yönetim sisteminize hizmet sağlığı olayları hakkında kişiselleştirilmiş bildirimler gönderin.
ms.topic: conceptual
ms.service: service-health
ms.date: 3/27/2018
ms.openlocfilehash: 2609a267bd151354f83482ab16c4b9345aa88cc4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062848"
---
# <a name="use-a-webhook-to-configure-health-notifications-for-problem-management-systems"></a>Sorun yönetim sistemleri için sistem bildirimlerini yapılandırmak için bir webhook kullanın

Bu makalede, mevcut bildirim sisteminize web hooks üzerinden veri göndermek için Azure Hizmet Durumu uyarılarını nasıl yapılandırabileceğiniz gösterilmektedir.

Bir Azure hizmeti olayı sizi etkilediğinde, Hizmet Durumu uyarılarını kısa mesaj veya e-posta ile bildirmek üzere Hizmet Durumu uyarılarını yapılandırabilirsiniz.

Ancak, kullanmayı tercih ettiğiniz varolan bir harici bildirim sisteminiz olabilir. Bu makalede, webhook yükü en önemli bölümlerini tanımlar. Ayrıca, ilgili hizmet sorunları oluştuğunda sizi bilgilendirmek için özel uyarıların nasıl oluşturulacak olduğunu açıklar.

Önceden yapılandırılmış bir tümleştirme kullanmak istiyorsanız, bkz:
* [ServiceNow ile uyarıları yapılandırma](service-health-alert-webhook-servicenow.md)
* [PagerDuty ile uyarıları yapılandırma](service-health-alert-webhook-pagerduty.md)
* [OpsGenie ile uyarıları yapılandırma](service-health-alert-webhook-opsgenie.md)

**Tanıtım videosunu izleyin:**

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUV]

## <a name="configure-a-custom-notification-by-using-the-service-health-webhook-payload"></a>Service Health webhook yükünü kullanarak özel bir bildirim yapılat
Kendi özel webhook tümleştirmenizi ayarlamak için, Hizmet Durumu bildirimi yoluyla gönderilen JSON yükünü ayrışdırman gerekir.

[Örnek](../azure-monitor/platform/activity-log-alerts-webhook.md) `ServiceHealth` bir webhook yüküne bakın.

Bir hizmet sağlık uyarısı olduğunu `context.eventSource == "ServiceHealth"`doğrulayabilirsiniz. Aşağıdaki özellikler en alakalı özellikleri şunlardır:
- **data.context.activityLog.status**
- **data.context.activityLog.level**
- **data.context.activityLog.subscriptionId**
- **data.context.activityLog.properties.title**
- **data.context.activityLog.properties.impactStartTime**
- **data.context.activityLog.properties.communication**
- **data.context.activityLog.properties.impactedServices**
- **data.context.activityLog.properties.trackingId**

## <a name="create-a-link-to-the-service-health-dashboard-for-an-incident"></a>Bir olay için Hizmet Durumu panosuna bağlantı oluşturma
Özel bir URL oluşturarak, masaüstü veya mobil cihazda Hizmet Durumu panonuza doğrudan bir bağlantı oluşturabilirsiniz. Bu biçimde *trackingId'yi* ve *aboneliğinizin* ilk üç ve son üç hanesini kullanın:

<i></i>https://app.azure.com/h/*&lt;trackingId&gt;*/*&lt;ilk üç ve son&gt; üç basamaklı subscriptionId*

Örneğin, *trackingId* aboneliğinizbba14129-e895-429b-8809-278e836ecdb3 ise ve izleme Kimliğiniz 0DET-URB ise, Hizmet Durumunuz URL'si: *subscriptionId*

https://app.azure.com/h/0DET-URB/bbadb3<i></i>

## <a name="use-the-level-to-detect-the-severity-of-the-issue"></a>Sorunun önem derecesini algılamak için düzeyi kullanın
En düşükten en yüksek öneme kadar, yükteki **düzey** özelliği *Bilgi,* *Uyarı,* *Hata*veya *Kritik*olabilir.

## <a name="parse-the-impacted-services-to-determine-the-incident-scope"></a>Olay kapsamını belirlemek için etkilenen hizmetleri ayrıştın
Hizmet Durumu uyarıları, birden çok bölge ve hizmetarasında sorunlar hakkında sizi bilgilendirebilir. Tam bilgi almak için, değerini ayrışdırmak `impactedServices`gerekir.

İçindeki içerik, kaçmış bir [JSON](https://json.org/) dizesidir ve bu dize, kaçmış olduğunda, düzenli olarak ayrıştırılabilen başka bir JSON nesnesi içerir. Örnek:

```json
{"data.context.activityLog.properties.impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"},{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"Alerts & Metrics\"},{\"ImpactedRegions\":[{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"App Service\"}]"}
```

Olur:

```json
[
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia East"
         },
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"Alerts & Metrics"
   },
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"App Service"
   }
]
```

Bu örnekte, sorunlar gösterilmektedir:
- Avustralya Doğu ve Avustralya Güneydoğu "Uyarılar & Ölçümleri"
- Avustralya Güneydoğu 'da "App Service".

## <a name="test-your-webhook-integration-via-an-http-post-request"></a>Webhook tümleştirmenizi http POST isteği yle test edin

Şu adımları uygulayın:

1. Göndermek istediğiniz hizmet durumu yükünü oluşturun. Azure etkinlik günlüğü uyarıları için [Webhooks'ta](../azure-monitor/platform/activity-log-alerts-webhook.md)örnek bir hizmet durumu webhook yüküne bakın.

1. Aşağıdaki gibi bir HTTP POST isteği oluşturun:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
   Bir "2XX - Başarılı" yanıtı almalısınız.

1. Tümleştirmenizin başarıyla ayarlandığını doğrulamak için [PagerDuty'e](https://www.pagerduty.com/) gidin.

## <a name="next-steps"></a>Sonraki adımlar
- Etkinlik [günlüğü uyarısı webhook şema](../azure-monitor/platform/activity-log-alerts-webhook.md)gözden geçirin. 
- Hizmet [durumu bildirimleri](../azure-monitor/platform/service-notifications.md)hakkında bilgi edinin.
- [Eylem grupları](../azure-monitor/platform/action-groups.md)hakkında daha fazla bilgi edinin.