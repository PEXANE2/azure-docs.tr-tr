---
title: Web kancalarını kullanarak OpsGenie ile Azure hizmet durumu uyarıları gönderme
description: OpsGenie örneğiniz için hizmet sistem durumu olayları hakkında kişiselleştirilmiş bildirimler alın.
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: def12d5e7b1b93b8370cd7be61538fca53531ae1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77654146"
---
# <a name="send-azure-service-health-alerts-with-opsgenie-using-webhooks"></a>Web kancalarını kullanarak OpsGenie ile Azure hizmet durumu uyarıları gönderme

Bu makalede, bir Web kancası kullanarak OpsGenie ile Azure hizmet durumu uyarılarını ayarlama işlemi gösterilmektedir. [OpsGenie](https://www.opsgenie.com/)'Nin Azure hizmet durumu tümleştirmesini kullanarak, Azure hizmet durumu uyarılarını OpsGenie 'e iletebilirsiniz. OpsGenie, arama zamanlamalarını, e-posta, kısa mesaj (SMS), telefon çağrılarını, iOS & Android anında iletme bildirimlerini kullanarak bildirimde bulunan ve uyarı onaylanana veya kapalı olana kadar uyarıları yürüyen doğru kişileri belirleyebilir.

## <a name="creating-a-service-health-integration-url-in-opsgenie"></a>OpsGenie 'de hizmet durumu tümleştirme URL 'SI oluşturma
1.  Kaydolduğunuzdan ve [OpsGenie](https://www.opsgenie.com/) hesabınızda oturum açtığınızdan emin olun.

1.  OpsGenie içindeki **tümleştirmeler** bölümüne gidin.

    ![OpsGenie 'teki "tümleştirmeler" bölümü](./media/webhook-alerts/opsgenie-integrations-section.png)

1.  **Azure hizmet durumu** tümleştirme düğmesini seçin.

    ![OpsGenie içindeki "Azure hizmet durumu düğmesi"](./media/webhook-alerts/opsgenie-azureservicehealth-button.png)

1.  Uyarınızı **adlandırın** ve **atanan takım** alanını belirtin.

1.  **Alıcılar**, **etkin**ve **bildirimleri gösterme**gibi diğer alanları doldurun.

1.  Hala sonuna `apiKey` eklenmiş olan **tümleştirme URL 'sini**kopyalayıp kaydedin.

    ![OpsGenie içindeki "tümleştirme URL 'SI"](./media/webhook-alerts/opsgenie-integration-url.png)

1.  **Tümleştirmeyi kaydet** ' i seçin

## <a name="create-an-alert-using-opsgenie-in-the-azure-portal"></a>Azure portal OpsGenie kullanarak uyarı oluşturma
### <a name="for-a-new-action-group"></a>Yeni bir eylem grubu için:
1. [Azure Portal kullanarak yeni bir eylem grubu için hizmet durumu bildiriminde uyarı oluşturma](../azure-monitor/platform/alerts-activity-log-service-notifications.md)bölümünde 1 ile 8 arasındaki adımları izleyin.

1. **Eylem**listesinde tanımlayın:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** Daha önce kaydettiğiniz OpsGenie **tümleştirme URL 'si** .

    c. **Ad:** Web kancası adı, diğer ad veya tanımlayıcı.

1. Uyarıyı oluşturmak için işiniz bittiğinde **Kaydet** ' i seçin.

### <a name="for-an-existing-action-group"></a>Mevcut bir eylem grubu için:
1. [Azure Portal](https://portal.azure.com/), **İzle**' yi seçin.

1. **Ayarlar** bölümünde **eylem grupları**' nı seçin.

1. Düzenlemek istediğiniz eylem grubunu bulun ve seçin.

1. **Eylem**listesine ekle:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** Daha önce kaydettiğiniz OpsGenie **tümleştirme URL 'si** .

    c. **Ad:** Web kancası adı, diğer ad veya tanımlayıcı.

1. Eylem grubunu güncelleştirmek için işiniz bittiğinde **Kaydet** ' i seçin.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Web kancası tümleştirmenizi bir HTTP POST isteği aracılığıyla test etme
1. Göndermek istediğiniz hizmet durumu yükünü oluşturun. [Azure etkinlik günlüğü uyarıları Için Web kancalarında](../azure-monitor/platform/activity-log-alerts-webhook.md)örnek bir hizmet durumu Web kancası yükü bulabilirsiniz.

1. Aşağıdaki gibi bir HTTP POST isteği oluşturun:

    ```
    POST        https://api.opsgenie.com/v1/json/azureservicehealth?apiKey=<APIKEY>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. "Başarılı" durumunun `200 OK` iletisiyle bir yanıt almalısınız.

1. Tümleştirmenin başarıyla ayarlandığını doğrulamak için [OpsGenie](https://www.opsgenie.com/) 'ye gidin.

## <a name="next-steps"></a>Sonraki adımlar
- [Mevcut sorun yönetimi sistemleri için Web kancası bildirimlerinin nasıl yapılandırılacağını](service-health-alert-webhook-guide.md)öğrenin.
- [Etkinlik günlüğü uyarısı Web kancası şemasını](../azure-monitor/platform/activity-log-alerts-webhook.md)gözden geçirin. 
- [Hizmet durumu bildirimleri](../azure-monitor/platform/service-notifications.md)hakkında bilgi edinin.
- [Eylem grupları](../azure-monitor/platform/action-groups.md)hakkında daha fazla bilgi edinin.