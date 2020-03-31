---
title: Webhooks kullanarak OpsGenie ile Azure hizmet durumu uyarıları gönderme
description: OpsGenie örneğinize hizmet sağlığı olayları yla ilgili kişiselleştirilmiş bildirimler alın.
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: def12d5e7b1b93b8370cd7be61538fca53531ae1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77654146"
---
# <a name="send-azure-service-health-alerts-with-opsgenie-using-webhooks"></a>Webhooks kullanarak OpsGenie ile Azure hizmet durumu uyarıları gönderme

Bu makalede, bir webhook kullanarak OpsGenie ile Azure hizmet durumu uyarıları nasıl ayarlayabileceğinizi gösterir. [OpsGenie'nin](https://www.opsgenie.com/)Azure Hizmet Durumu Tümleştirmesini kullanarak, Azure Hizmet Durumu uyarılarını OpsGenie'ye iletebilirsiniz. OpsGenie, e-posta, kısa mesaj (SMS), telefon görüşmeleri, iOS & Android anında iletme bildirimleri ve uyarı onaylanana veya kapatılıcaya kadar uyarıları artırarak, arama zamanlamalarına göre bildirimde bulunmak için doğru kişileri belirleyebilir.

## <a name="creating-a-service-health-integration-url-in-opsgenie"></a>OpsGenie'de hizmet durumu tümleştirme URL'si oluşturma
1.  [OpsGenie](https://www.opsgenie.com/) hesabınıza kaydolduğunuzdan ve hesabınızda oturum açtığınızdan emin olun.

1.  OpsGenie'deki **Tümleştirmeler** bölümüne gidin.

    ![OpsGenie'deki "Entegrasyonlar" bölümü](./media/webhook-alerts/opsgenie-integrations-section.png)

1.  Azure **Hizmet Durumu** tümleştirme düğmesini seçin.

    ![OpsGenie'deki "Azure Hizmet Durumu düğmesi"](./media/webhook-alerts/opsgenie-azureservicehealth-button.png)

1.  Uyarınızı **adlandırın** ve **Atanan Takım** alanını belirtin.

1.  **Alıcılar**, **Etkin**ve **Bildirimleri Bastır**gibi diğer alanları doldurun.

1.  Sonuna eklenen kitabınızı `apiKey` zaten içermesi gereken **Tümleştirme URL'sini**kopyalayın ve kaydedin.

    ![OpsGenie'deki "Entegrasyon URL'si"](./media/webhook-alerts/opsgenie-integration-url.png)

1.  **Tümleştirmeyi Kaydet'i** seçin

## <a name="create-an-alert-using-opsgenie-in-the-azure-portal"></a>Azure portalında OpsGenie'yi kullanarak bir uyarı oluşturma
### <a name="for-a-new-action-group"></a>Yeni bir eylem grubu için:
1. [Azure portalını kullanarak yeni bir eylem grubu için hizmet durumu bildiriminde uyarı oluşturma'da](../azure-monitor/platform/alerts-activity-log-service-notifications.md)1'den 8'e kadar olan adımları izleyin.

1. **Eylemler**listesinde tanımlayın:

    a. **Eylem Türü:** *Webhook*

    b. **Ayrıntılar:** Daha önce kaydettiğiniz OpsGenie **Tümleştirme** URL'si.

    c. **Adı:** Webhook'un adı, takma adı veya tanımlayıcısı.

1. Uyarıyı oluşturmak için yapıldığında **Kaydet'i** seçin.

### <a name="for-an-existing-action-group"></a>Varolan bir eylem grubu için:
1. Azure [portalında,](https://portal.azure.com/) **Monitör'ü**seçin.

1. **Ayarlar** bölümünde Eylem **gruplarını**seçin.

1. Yeniden yapmak istediğiniz eylem grubunu bulun ve seçin.

1. **Eylemler**listesine ekle:

    a. **Eylem Türü:** *Webhook*

    b. **Ayrıntılar:** Daha önce kaydettiğiniz OpsGenie **Tümleştirme** URL'si.

    c. **Adı:** Webhook'un adı, takma adı veya tanımlayıcısı.

1. Eylem grubunu güncelleştirmek için yapıldığında **Kaydet'i** seçin.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Http POST isteği ile webhook tümleştirmenizi test etme
1. Göndermek istediğiniz hizmet durumu yükü oluşturun. [Azure etkinlik günlüğü uyarıları için Webhooks'ta](../azure-monitor/platform/activity-log-alerts-webhook.md)örnek bir hizmet durumu webhook yükü bulabilirsiniz.

1. Aşağıdaki gibi bir HTTP POST isteği oluşturun:

    ```
    POST        https://api.opsgenie.com/v1/json/azureservicehealth?apiKey=<APIKEY>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. Durum "başarılı" iletisi ile bir `200 OK` yanıt almalısınız.

1. Tümleştirmenizin başarılı bir şekilde ayarlandığını doğrulamak için [OpsGenie'ye](https://www.opsgenie.com/) gidin.

## <a name="next-steps"></a>Sonraki adımlar
- [Mevcut sorun yönetim sistemleri için webhook bildirimlerini nasıl yapılandırıştırılamayı](service-health-alert-webhook-guide.md)öğrenin.
- Etkinlik [günlüğü uyarısı webhook şema](../azure-monitor/platform/activity-log-alerts-webhook.md)gözden geçirin. 
- Hizmet [durumu bildirimleri](../azure-monitor/platform/service-notifications.md)hakkında bilgi edinin.
- [Eylem grupları](../azure-monitor/platform/action-groups.md)hakkında daha fazla bilgi edinin.