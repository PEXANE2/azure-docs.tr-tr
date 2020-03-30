---
title: PagerDuty ile Azure hizmeti sistem durumu uyarıları gönderme
description: Hizmet sağlığı olayları yla ilgili kişiselleştirilmiş bildirimleri PagerDuty örneğinize alın.
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: bb449a5279f3cea55e6aec2f72edfd11fb26227a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77654078"
---
# <a name="send-azure-service-health-alerts-with-pagerduty-using-webhooks"></a>Web hooks kullanarak PagerDuty ile Azure hizmet durumu uyarıları gönderme

Bu makalede, bir webhook kullanarak PagerDuty aracılığıyla Azure hizmet sistem durumu bildirimlerini nasıl ayarlayabileceğiniz gösterilmektedir. [PagerDuty'nin](https://www.pagerduty.com/)özel Microsoft Azure tümleştirme türünü kullanarak, yeni veya mevcut PagerDuty hizmetlerinize Hizmet Durumu uyarıları zahmetsizce ekleyebilirsiniz.

## <a name="creating-a-service-health-integration-url-in-pagerduty"></a>PagerDuty'de hizmet durumu tümleştirme URL'si oluşturma
1.  [PagerDuty](https://www.pagerduty.com/) hesabınıza kaydolduğunuzdan ve hesabınızda oturum açtığınızdan emin olun.

1.  PagerDuty'deki **Hizmetler** bölümüne gidin.

    ![PagerDuty'deki "Hizmetler" bölümü](./media/webhook-alerts/pagerduty-services-section.png)

1.  **Yeni Hizmet Ekle'yi** seçin veya ayarlamış olduğunuz mevcut bir hizmeti açın.

1.  **Tümleştirme Ayarları'nda**aşağıdakileri seçin:

    a. **Tümleştirme Türü**: Microsoft Azure

    b. **Entegrasyon**Adı \<: Adı\>

    ![PagerDuty'de "Entegrasyon Ayarları"](./media/webhook-alerts/pagerduty-integration-settings.png)

1.  Gerekli diğer alanları doldurun ve **Ekle'yi**seçin.

1.  Bu yeni tümleştirmeyi açın ve **Tümleştirme URL'sini**kopyalayın ve kaydedin.

    ![PagerDuty'de "Entegrasyon URL'si"](./media/webhook-alerts/pagerduty-integration-url.png)

## <a name="create-an-alert-using-pagerduty-in-the-azure-portal"></a>Azure portalında PagerDuty kullanarak bir uyarı oluşturma
### <a name="for-a-new-action-group"></a>Yeni bir eylem grubu için:
1. [Azure portalını kullanarak yeni bir eylem grubu için hizmet durumu bildiriminde uyarı oluşturma'da](../azure-monitor/platform/alerts-activity-log-service-notifications.md)1'den 8'e kadar olan adımları izleyin.

1. **Eylemler**listesinde tanımlayın:

    a. **Eylem Türü:** *Webhook*

    b. **Ayrıntılar:** Daha önce kaydettiğiniz PagerDuty **Tümleştirme** URL'si.

    c. **Adı:** Webhook'un adı, takma adı veya tanımlayıcısı.

1. Uyarıyı oluşturmak için yapıldığında **Kaydet'i** seçin.

### <a name="for-an-existing-action-group"></a>Varolan bir eylem grubu için:
1. Azure [portalında,](https://portal.azure.com/) **Monitör'ü**seçin.

1. **Ayarlar** bölümünde Eylem **gruplarını**seçin.

1. Yeniden yapmak istediğiniz eylem grubunu bulun ve seçin.

1. **Eylemler**listesine ekle:

    a. **Eylem Türü:** *Webhook*

    b. **Ayrıntılar:** Daha önce kaydettiğiniz PagerDuty **Tümleştirme** URL'si.

    c. **Adı:** Webhook'un adı, takma adı veya tanımlayıcısı.

1. Eylem grubunu güncelleştirmek için yapıldığında **Kaydet'i** seçin.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Http POST isteği ile webhook tümleştirmenizi test etme
1. Göndermek istediğiniz hizmet durumu yükü oluşturun. [Azure etkinlik günlüğü uyarıları için Webhooks'ta](../azure-monitor/platform/activity-log-alerts-webhook.md)örnek bir hizmet durumu webhook yükü bulabilirsiniz.

1. Aşağıdaki gibi bir HTTP POST isteği oluşturun:

    ```
    POST        https://events.pagerduty.com/integration/<IntegrationKey>/enqueue

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. "Olay kimliğinizi" içeren bir ileti almalısınız. `202 Accepted`

1. Tümleştirmenizin başarıyla ayarlandığını doğrulamak için [PagerDuty'e](https://www.pagerduty.com/) gidin.

## <a name="next-steps"></a>Sonraki adımlar
- [Mevcut sorun yönetim sistemleri için webhook bildirimlerini nasıl yapılandırıştırılamayı](service-health-alert-webhook-guide.md)öğrenin.
- Etkinlik [günlüğü uyarısı webhook şema](../azure-monitor/platform/activity-log-alerts-webhook.md)gözden geçirin. 
- Hizmet [durumu bildirimleri](../azure-monitor/platform/service-notifications.md)hakkında bilgi edinin.
- [Eylem grupları](../azure-monitor/platform/action-groups.md)hakkında daha fazla bilgi edinin.