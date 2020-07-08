---
title: Azure portal kullanarak Azure hizmet bildirimlerinde etkinlik günlüğü uyarıları alma
description: Azure hizmeti gerçekleştiğinde SMS, e-posta veya Web Kancası aracılığıyla bildirim alın.
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: a8723698cddfb519687525820475517b93219a4a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85569007"
---
# <a name="create-activity-log-alerts-on-service-notifications-using-the-azure-portal"></a>Azure portal kullanarak hizmet bildirimlerinde etkinlik günlüğü uyarıları oluşturma
## <a name="overview"></a>Genel Bakış

Bu makalede, Azure portal kullanarak hizmet durumu bildirimlerine yönelik etkinlik günlüğü uyarılarını ayarlamak için Azure portal nasıl kullanılacağı gösterilmektedir.  

Hizmet durumu bildirimleri, etkinlik günlüğünde depolanan büyük miktarda bilgi hacmi verilen [Azure etkinlik günlüğünde](../azure-monitor/platform/platform-logs-overview.md) depolanır, hizmet durumu bildirimlerinde uyarıları görüntülemeyi ve ayarlamayı kolaylaştırmak için ayrı bir kullanıcı arabirimi vardır. 

Azure, Azure aboneliğinize hizmet durumu bildirimleri gönderdiğinde bir uyarı alabilirsiniz. Uyarıyı temel alarak yapılandırabilirsiniz:

- Hizmet durumu bildirimi sınıfı (hizmet sorunları, planlı bakım, sağlık danışma belgeleri).
- Abonelik etkilendi.
- Hizmet (ler) i etkilendi.
- Bu bölgeler etkilendi.

> [!NOTE]
> Hizmet durumu bildirimleri, kaynak sistem durumu olaylarıyla ilgili bir uyarı göndermez.

Ayrıca, uyarının kime gönderileceğini de yapılandırabilirsiniz:

- Mevcut bir eylem grubu seçin.
- Yeni bir eylem grubu oluşturun (gelecekteki uyarılar için kullanılabilir).

Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md).

Azure Resource Manager şablonları kullanarak hizmet durumu bildirim uyarılarını yapılandırma hakkında daha fazla bilgi için, bkz. [Kaynak Yöneticisi Templates](../azure-monitor/platform/alerts-activity-log.md).

### <a name="watch-a-video-on-setting-up-your-first-azure-service-health-alert"></a>İlk Azure Service Health uyarısını ayarlama hakkında bir video izleyin

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OaXt]

## <a name="alert-and-new-action-group-using-azure-portal"></a>Azure portal kullanarak uyarı ve yeni eylem grubu
1. [Portalda](https://portal.azure.com) **hizmet durumu**' nu seçin.

    !["Hizmet durumu" hizmeti](media/alerts-activity-log-service-notifications/home-servicehealth.png)

1. **Uyarılar** bölümünde **sistem durumu uyarıları**' nı seçin.

    !["Sistem durumu uyarıları" sekmesi](media/alerts-activity-log-service-notifications/alerts-blades-sh.png)

1. **Hizmet sistem durumu uyarısı oluştur** ' u seçin ve alanları girin.

    !["Hizmet sistem durumu uyarısı oluştur" komutu](media/alerts-activity-log-service-notifications/service-health-alert.png)

1. Uyarı almak istediğiniz **abonelik**, **hizmet**ve **bölgeleri** seçin.

    !["Etkinlik günlüğü uyarısı Ekle" iletişim kutusu](media/alerts-activity-log-service-notifications/activity-log-alert-new-ux.png)

    > [!NOTE]
    > Bu abonelik, etkinlik günlüğü uyarısını kaydetmek için kullanılır. Uyarı kaynağı bu aboneliğe dağıtılır ve etkinlik günlüğündeki olayları izler.

1. Uyarı almak istediğiniz **olay türlerini** seçin: *hizmet sorunu*, *Planlı bakım*ve *sistem durumu Danışma belgeleri* 

1. Uyarı **kuralları adı** ve **açıklaması**girerek uyarı ayrıntılarınızı tanımlayın.

1. Uyarının kaydedilmesini istediğiniz **kaynak grubunu** seçin.

1. **Yeni eylem grubu**' nu seçerek yeni bir eylem grubu oluşturun. **Eylem grubu adı** kutusuna bir ad girin ve **kısa ad** kutusuna bir ad girin. Bu uyarı tetiklendiğinde gönderilen bildirimlerde kısa ada başvurulur.

    ![Yeni bir eylem grubu oluştur](media/alerts-activity-log-service-notifications/action-group-creation.png)

1. Alıcının bir listesini, alıcıların şunları sağlayarak tanımlayın:

    a. **Ad**: alıcının adını, diğer adını veya tanımlayıcıyı girin.

    b. **Eylem türü**: SMS, e-posta, Web kancası, Azure uygulaması ve daha fazlasını seçin.

    c. **Ayrıntılar**: seçilen eylem türüne bağlı olarak bir telefon numarası, e-posta adresi, Web kancası URI 'si vb. girin.

1. Eylem grubunu oluşturmak için **Tamam** ' ı seçin ve sonra uyarılarınızı gerçekleştirmek için **Uyarı kuralı oluşturun** .

Birkaç dakika içinde, uyarı etkin olur ve oluşturma sırasında belirttiğiniz koşullara göre tetiklemeye başlar.

[Mevcut sorun yönetimi sistemleri için Web kancası bildirimlerinin nasıl yapılandırılacağını](service-health-alert-webhook-guide.md)öğrenin. Etkinlik günlüğü uyarıları için Web kancası şeması hakkında bilgi için bkz. [Azure etkinlik günlüğü uyarıları Için Web kancaları](../azure-monitor/platform/activity-log-alerts-webhook.md).

>[!NOTE]
>Bu adımlarda tanımlanan eylem grubu, gelecekteki tüm uyarı tanımları için mevcut bir eylem grubu olarak yeniden kullanılabilir.
>

## <a name="alert-with-existing-action-group-using-azure-portal"></a>Azure portal kullanarak mevcut eylem grubuyla uyar

1. Service Health bildirimini oluşturmak için önceki bölümde bulunan 1 ile 6 arasındaki adımları izleyin. 

1. **Eylem grubunu tanımla**altında **eylem grubu seç** düğmesine tıklayın. Uygun eylem grubunu seçin.

1. Eylem grubunu eklemek için **Ekle** ' yi seçin ve sonra uyarılarınızı tamamlamaya yönelik **Uyarı kuralını oluşturun** .

Birkaç dakika içinde, uyarı etkin olur ve oluşturma sırasında belirttiğiniz koşullara göre tetiklemeye başlar.


## <a name="next-steps"></a>Sonraki adımlar
- [Azure hizmet durumu uyarılarını ayarlamaya yönelik en iyi uygulamalar](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUa)hakkında bilgi edinin.
- [Azure hizmet durumu için mobil anında iletme bildirimleri ayarlamayı](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUw)öğrenin.
- [Mevcut sorun yönetimi sistemleri için Web kancası bildirimlerinin nasıl yapılandırılacağını](service-health-alert-webhook-guide.md)öğrenin.
- [Hizmet durumu bildirimleri](service-notifications.md)hakkında bilgi edinin.
- [Bildirim hızı sınırlaması](../azure-monitor/platform/alerts-rate-limiting.md)hakkında bilgi edinin.
- [Etkinlik günlüğü uyarısı Web kancası şemasını](../azure-monitor/platform/activity-log-alerts-webhook.md)gözden geçirin.
- [Etkinlik günlüğü uyarılarına genel bir bakış](../azure-monitor/platform/alerts-overview.md)elde edin ve uyarıları alma hakkında bilgi edinin.
- [Eylem grupları](../azure-monitor/platform/action-groups.md)hakkında daha fazla bilgi edinin.
