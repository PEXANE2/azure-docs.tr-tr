---
title: Portal ile Event Grid Azure Media Services olaylarını izleme
description: Bu makalede, Azure Media Services olaylarını izlemek için Event Grid nasıl abone olunacağı gösterilmektedir.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, akış, yayın, canlı, çevrimdışı
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 01/21/2020
ms.author: juliako
ms.openlocfilehash: 18503e64dc6f38daab61599153cd0e0fb6fadb20
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76509232"
---
# <a name="create-and-monitor-media-services-events-with-event-grid-using-the-azure-portal"></a>Azure portalını kullanarak Event Grid ile Media Services olayları oluşturma ve izleme

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu hizmet, olay iletilerini abonelere yönlendirmek için [olay abonelikleri](../../event-grid/concepts.md#event-subscriptions) kullanır. Media Services olaylar, verilerdeki değişikliklere yanıt vermek için gereken tüm bilgileri içerir. EventType özelliği "Microsoft. Media." ile başladığı için bir Media Services olayı tanımlayabilirsiniz. Daha fazla bilgi için bkz. [Media Services olay şemaları](media-services-event-schemas.md).

Bu makalede, Azure Media Services hesabınıza yönelik olaylara abone olmak için Azure portal kullanırsınız. Ardından, sonucu görüntülemek için olayları tetiklersiniz. Normalde olayları, olay verilerini işleyen ve eylemler gerçekleştiren bir uç noktaya gönderirsiniz. Makalesinde, iletileri toplayan ve görüntüleyen bir Web uygulamasına olayları göndereceğiz.

İşiniz bittiğinde, olay verilerinin web uygulamasına gönderildiğini görürsünüz.

## <a name="prerequisites"></a>Ön koşullar 

* Etkin bir Azure aboneliğiniz olmalıdır.
* [Bu hızlı başlangıçta](create-account-cli-quickstart.md) açıklandığı gibi yeni bir Azure Media Services hesabı oluşturun.

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Media Services hesabı için olaylara abone olmadan önce olay iletisi için uç noktayı oluşturalım. Normalde, olay verileri temelinde uç nokta eylemleri gerçekleştirir. Bu makalede, olay iletilerini görüntüleyen [önceden oluşturulmuş bir Web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtırsınız. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

1. Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

1. Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

"Azure Event Grid Görüntüleyici" sitesine geçerseniz, henüz hiç olayına sahip olduğunu görürsünüz.
   
[!INCLUDE [event-grid-register-provider-portal.md](../../../includes/event-grid-register-provider-portal.md)]

## <a name="subscribe-to-media-services-events"></a>Media Services olaylarına abone olma

Event Grid’e hangi olayları izlemek istediğinizi ve olayların nereye gönderileceğini bildirmek için bir konuya abone olursunuz.

1. Portalda Media Services hesabınızı seçin ve **Olaylar**' ı seçin.
1. Olayları görüntüleyici uygulamanıza göndermek için uç noktada bir web kancası kullanın. 

   ![Web kancasını seçme](./media/monitor-events-portal/select-web-hook.png)

1. Olay aboneliği, Media Services hesabınızın değerleriyle önceden doldurulmuştur. 
1. **Uç nokta türü**Için ' Web kancası ' seçeneğini belirleyin.
1. Bu konu başlığında, **tüm olay türlerine abone ol** seçeneğini işaretlenmiş olarak bırakıyoruz. Ancak, işaretini kaldırın ve belirli olay türleri için filtre uygulayabilirsiniz. 
1. **Uç nokta seçin** bağlantısına tıklayın.

    Web kancası uç noktası için web uygulamanızın URL'sini girin ve ana sayfa URL'sine `api/updates` ekleyin. 

1. **Seçimi Onayla**' ya basın.
1. **Oluştur**’a basın.
1. Aboneliğinize bir ad verin.

   ![Günlükleri seçme](./media/monitor-events-portal/create-subscription.png)

1. Web uygulamanızı yeniden görüntüleyin ve buna bir abonelik doğrulama olayının gönderildiğine dikkat edin. 

    Uç noktanın olay verilerini almak istediğini doğrulayabilmesi için Event Grid doğrulama olayını gönderir. Uç noktanın `validationResponse` olarak ayarlanması `validationCode` . Daha fazla bilgi için bkz. [güvenlik ve kimlik doğrulaması Event Grid](../../event-grid/security-authentication.md). Aboneliği nasıl doğrulayacağını görmek için Web uygulaması kodunu görüntüleyebilirsiniz.

Şimdi, Event Grid iletiyi uç noktanıza nasıl dağıttığını görmek için olayları tetikleyelim.

## <a name="send-an-event-to-your-endpoint"></a>Uç noktanıza olay gönderme

Bir kodlama işi çalıştırarak Media Services hesabı için olayları tetikleyebilirsiniz. Bir dosya kodlamak ve olay göndermeye başlamak için [Bu hızlı](stream-files-dotnet-quickstart.md) başlangıcı izleyebilirsiniz. Tüm olaylara abone değilseniz aşağıdakine benzer bir ekran görürsünüz:

> [!TIP]
> Göz simgesini seçerek olay verilerini genişletin. Tüm olayları görüntülemek istiyorsanız, sayfayı yenilemeyin.

![Abonelik olayını görüntüleme](./media/monitor-events-portal/view-subscription-event.png)

## <a name="next-steps"></a>Sonraki adımlar

[Karşıya yükleme, kodlama ve akışla aktarma](stream-files-tutorial-with-api.md)
