---
title: ServiceNow ile Azure hizmet durumu uyarıları gönderme
description: ServiceNow örneğinize hizmet sağlığı olayları yla ilgili kişiselleştirilmiş bildirimler alın.
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 3daae05aabff571010d043cf5602847e95ea29f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77654112"
---
# <a name="send-azure-service-health-alerts-with-servicenow-using-webhooks"></a>Web hooks kullanarak ServiceNow ile Azure hizmet durumu uyarıları gönderme

Bu makalede, bir webhook kullanarak ServiceNow ile Azure hizmet durumu uyarıları nasıl entegre ettiğinizi gösterir. ServiceNow örneğinizle webhook tümleştirmesini ayarladıktan sonra, Azure hizmet sorunları sizi etkilediğinde varolan bildirim altyapınız aracılığıyla uyarılar alırsınız. Bir Azure Hizmet Durumu uyarısı her yangında ServiceNow'un Komut Dosyası REST API'si aracılığıyla bir web hook çağırır.

## <a name="creating-a-scripted-rest-api-in-servicenow"></a>ServiceNow'da komut dosyası yla yazılmış BIR REST API oluşturma

1.  [ServiceNow](https://www.servicenow.com/) hesabınıza kaydolduğunuzdan ve hesabınızda oturum açtığınızdan emin olun.

1.  ServiceNow'daki **Sistem Web Hizmetleri** bölümüne gidin ve Komut Dosyası REST **API'lerini**seçin.

    ![ServiceNow'daki "Komut Dosyası Web Hizmeti" bölümü](./media/webhook-alerts/servicenow-sws-section.png)

1.  Yeni bir Komut Dosyası REST hizmeti oluşturmak için **Yeni'yi** seçin.
 
    ![ServiceNow'da "Yeni Komut Dosyası REST API" düğmesi](./media/webhook-alerts/servicenow-new-button.png)

1.  REST API'nize bir **Ad** ekleyin ve `azureservicehealth` **API Kimliğini** 'ye ayarlayın.

1.  **Gönder**’i seçin.

    ![ServiceNow'daki "REST API Ayarları"](./media/webhook-alerts/servicenow-restapi-settings.png)

1.  Oluşturduğunuz REST API'sini seçin ve **Kaynaklar** sekmesinin altında **Yeni'yi**seçin.

    ![ServiceNow'daki "Kaynak Sekmesi"](./media/webhook-alerts/servicenow-resources-tab.png)

1.  Yeni kaynağınızı `event` **adlandırın** ve `POST`HTTP yöntemini '' olarak **değiştirin.**

1.  **Script** bölümünde, aşağıdaki JavaScript kodunu ekleyin:

    >[!NOTE]
    >Aşağıdaki komut `<secret>`dosyasında`<group>`,, ve `<email>` değeri güncelleştirmeniz gerekir.
    >* `<secret>`bir GUID gibi rasgele bir dize olmalıdır
    >* `<group>`olayı atamak istediğiniz ServiceNow grubu olmalıdır
    >* `<email>`olayı (isteğe bağlı) atamak istediğiniz belirli kişi olmalıdır
    >

    ```javascript
    (function process( /*RESTAPIRequest*/ request, /*RESTAPIResponse*/ response) {
        var apiKey = request.queryParams['apiKey'];
        var secret = '<secret>';
        if (apiKey == secret) {
            var event = request.body.data;
            var responseBody = {};
            if (event.data.context.activityLog.operationName == 'Microsoft.ServiceHealth/incident/action') {
                var inc = new GlideRecord('incident');
                var incidentExists = false;
                inc.addQuery('number', event.data.context.activityLog.properties.trackingId);
                inc.query();
                if (inc.hasNext()) {
                    incidentExists = true;
                    inc.next();
                } else {
                    inc.initialize();
                }
                var short_description = "Azure Service Health";
                if (event.data.context.activityLog.properties.incidentType == "Incident") {
                    short_description += " - Service Issue - ";
                } else if (event.data.context.activityLog.properties.incidentType == "Maintenance") {
                    short_description += " - Planned Maintenance - ";
                } else if (event.data.context.activityLog.properties.incidentType == "Informational" || event.data.context.activityLog.properties.incidentType == "ActionRequired") {
                    short_description += " - Health Advisory - ";
                }
                short_description += event.data.context.activityLog.properties.title;
                inc.short_description = short_description;
                inc.description = event.data.context.activityLog.properties.communication;
                inc.work_notes = "Impacted subscription: " + event.data.context.activityLog.subscriptionId;
                if (incidentExists) {
                    if (event.data.context.activityLog.properties.stage == 'Active') {
                        inc.state = 2;
                    } else if (event.data.context.activityLog.properties.stage == 'Resolved') {
                        inc.state = 6;
                    } else if (event.data.context.activityLog.properties.stage == 'Closed') {
                        inc.state = 7;
                    }
                    inc.update();
                    responseBody.message = "Incident updated.";
                } else {
                    inc.number = event.data.context.activityLog.properties.trackingId;
                    inc.state = 1;
                    inc.impact = 2;
                    inc.urgency = 2;
                    inc.priority = 2;
                    inc.assigned_to = '<email>';
                    inc.assignment_group.setDisplayValue('<group>');
                    var subscriptionId = event.data.context.activityLog.subscriptionId;
                    var comments = "Azure portal Link: https://app.azure.com/h";
                    comments += "/" + event.data.context.activityLog.properties.trackingId;
                    comments += "/" + subscriptionId.substring(0, 3) + subscriptionId.slice(-3);
                    var impactedServices = JSON.parse(event.data.context.activityLog.properties.impactedServices);
                    var impactedServicesFormatted = "";
                    for (var i = 0; i < impactedServices.length; i++) {
                        impactedServicesFormatted += impactedServices[i].ServiceName + ": ";
                        for (var j = 0; j < impactedServices[i].ImpactedRegions.length; j++) {
                            if (j != 0) {
                                impactedServicesFormatted += ", ";
                            }
                            impactedServicesFormatted += impactedServices[i].ImpactedRegions[j].RegionName;
                        }

                        impactedServicesFormatted += "\n";

                    }
                    comments += "\n\nImpacted Services:\n" + impactedServicesFormatted;
                    inc.comments = comments;
                    inc.insert();
                    responseBody.message = "Incident created.";
                }
            } else {
                responseBody.message = "Hello from the other side!";
            }
            response.setBody(responseBody);
        } else {
            var unauthorized = new sn_ws_err.ServiceError();
            unauthorized.setStatus(401);
            unauthorized.setMessage('Invalid apiKey');
            response.setError(unauthorized);
        }
    })(request, response);
    ```

1.  Güvenlik sekmesinde, uncheck **Kimlik Doğrulaması Gerektirir** ve **Gönder'i**seçin. Ayarlanan bu `<secret>` API yerine korur.

    ![ServiceNow'daki "Kimlik Doğrulama Gerektirir" onay kutusu](./media/webhook-alerts/servicenow-resource-settings.png)

1.  Scripted REST API'leri bölümünde, yeni REST API'niz için **Temel API Yolunu** bulmalısınız:

     ![ServiceNow'daki "Temel API Yolu"](./media/webhook-alerts/servicenow-base-api-path.png)

1.  Tam Tümleştirme URL'niz şu şekilde görünür:
        
         https://<yourInstanceName>.service-now.com/<baseApiPath>?apiKey=<secret>


## <a name="create-an-alert-using-servicenow-in-the-azure-portal"></a>Azure portalında ServiceNow'u kullanarak bir uyarı oluşturma
### <a name="for-a-new-action-group"></a>Yeni bir eylem grubu için:
1. Yeni bir eylem grubuyla bir uyarı oluşturmak için [bu makalede](../azure-monitor/platform/alerts-activity-log-service-notifications.md) 1'den 8'e kadar olan adımları izleyin.

1. **Eylemler**listesinde tanımlayın:

    a. **Eylem Türü:** *Webhook*

    b. **Ayrıntılar:** Daha önce kaydettiğiniz ServiceNow **Tümleştirme** URL'si.

    c. **Adı:** Webhook'un adı, takma adı veya tanımlayıcısı.

1. Uyarıyı oluşturmak için yapıldığında **Kaydet'i** seçin.

### <a name="for-an-existing-action-group"></a>Varolan bir eylem grubu için:
1. Azure [portalında,](https://portal.azure.com/) **Monitör'ü**seçin.

1. **Ayarlar** bölümünde Eylem **gruplarını**seçin.

1. Yeniden yapmak istediğiniz eylem grubunu bulun ve seçin.

1. **Eylemler**listesine ekle:

    a. **Eylem Türü:** *Webhook*

    b. **Ayrıntılar:** Daha önce kaydettiğiniz ServiceNow **Tümleştirme** URL'si.

    c. **Adı:** Webhook'un adı, takma adı veya tanımlayıcısı.

1. Eylem grubunu güncelleştirmek için yapıldığında **Kaydet'i** seçin.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Http POST isteği ile webhook tümleştirmenizi test etme
1. Göndermek istediğiniz hizmet durumu yükü oluşturun. [Azure etkinlik günlüğü uyarıları için Webhooks'ta](../azure-monitor/platform/activity-log-alerts-webhook.md)örnek bir hizmet durumu webhook yükü bulabilirsiniz.

1. Aşağıdaki gibi bir HTTP POST isteği oluşturun:

    ```
    POST        https://<yourInstanceName>.service-now.com/<baseApiPath>?apiKey=<secret>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. "Olay oluşturuldu" iletisiyle bir `200 OK` yanıt almalısınız.

1. Tümleştirmenizin başarıyla ayarlandığını doğrulamak için [ServiceNow'a](https://www.servicenow.com/) gidin.

## <a name="next-steps"></a>Sonraki adımlar
- [Mevcut sorun yönetim sistemleri için webhook bildirimlerini nasıl yapılandırıştırılamayı](service-health-alert-webhook-guide.md)öğrenin.
- Etkinlik [günlüğü uyarısı webhook şema](../azure-monitor/platform/activity-log-alerts-webhook.md)gözden geçirin. 
- Hizmet [durumu bildirimleri](../azure-monitor/platform/service-notifications.md)hakkında bilgi edinin.
- [Eylem grupları](../azure-monitor/platform/action-groups.md)hakkında daha fazla bilgi edinin.