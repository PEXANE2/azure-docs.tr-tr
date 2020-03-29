---
title: Azure Medya Hizmetleri etkinliklerini Portal ile Olay Izgarasıyla izleyin
description: Bu makalede, Azure Medya Hizmetleri olaylarını izlemek için Olay Ağıt'a nasıl abone olunur gösterilmektedir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76509232"
---
# <a name="create-and-monitor-media-services-events-with-event-grid-using-the-azure-portal"></a>Azure portalını kullanarak Event Grid ile Media Services olayları oluşturma ve izleme

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu hizmet, olay iletilerini abonelere yönlendirmek için [olay aboneliklerini](../../event-grid/concepts.md#event-subscriptions) kullanır. Medya Hizmetleri olayları, verilerinizdeki değişikliklere yanıt vermek için gereken tüm bilgileri içerir. EventType özelliği "Microsoft.Media" ile başladığı için bir Medya Hizmetleri olayını tanımlayabilirsiniz. Daha fazla bilgi için Medya [Hizmetleri etkinlik şemalarına](media-services-event-schemas.md)bakın.

Bu makalede, Azure Medya Hizmetleri hesabınızdaki etkinliklere abone olmak için Azure portalını kullanıyorsunuz. Ardından, sonucu görüntülemek için olayları tetiklersiniz. Normalde olayları, olay verilerini işleyen ve eylemler gerçekleştiren bir uç noktaya gönderirsiniz. Makalede, olayları iletileri toplayan ve görüntüleyen bir web uygulamasına göndeririz.

İşiniz bittiğinde, olay verilerinin web uygulamasına gönderildiğini görürsünüz.

## <a name="prerequisites"></a>Ön koşullar 

* Etkin bir Azure aboneliğine sahip olun.
* [Bu hızlı başlangıçta](create-account-cli-quickstart.md) açıklandığı gibi yeni bir Azure Media Services hesabı oluşturun.

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Medya Hizmetleri hesabının olaylarına abone olmadan önce, olay iletisi için bitiş noktasını oluşturalım. Normalde, olay verileri temelinde uç nokta eylemleri gerçekleştirir. Bu makalede, olay iletilerini görüntüleyen önceden oluşturulmuş bir [web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtmış sınız. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

1. Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

1. Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

"Azure Olay Izgara Görüntüleyicisi" sitesine geçerseniz, henüz herhangi bir etkinliği olmadığını görürsünüz.
   
[!INCLUDE [event-grid-register-provider-portal.md](../../../includes/event-grid-register-provider-portal.md)]

## <a name="subscribe-to-media-services-events"></a>Medya Hizmetleri etkinliklerine abone olun

Event Grid’e hangi olayları izlemek istediğinizi ve olayların nereye gönderileceğini bildirmek için bir konuya abone olursunuz.

1. Portalda, Medya Hizmetleri hesabınızı seçin ve **Etkinlikler'i**seçin.
1. Olayları görüntüleyici uygulamanıza göndermek için uç noktada bir web kancası kullanın. 

   ![Web kancasını seçme](./media/monitor-events-portal/select-web-hook.png)

1. Olay aboneliği, Medya Hizmetleri hesabınıziçin değerlerle önceden doldurulmuş. 
1. **Bitiş Noktası Türü**için 'Web Kancası'nı seçin.
1. Bu konuda, aboneyi kontrol edilen **tüm olay türlerine** bırakıyoruz. Ancak, denetiminin karşıdan çekilip belirli olay türleri için filtre uygulayabilirsiniz. 
1. Bitiş Noktası bağlantısını **seç'e** tıklayın.

    Web kancası uç noktası için web uygulamanızın URL'sini girin ve ana sayfa URL'sine `api/updates` ekleyin. 

1. **Seçimi Onayla'ya**basın.
1. **Oluştur**’a basın.
1. Aboneliğinize bir ad verin.

   ![Günlükleri seçme](./media/monitor-events-portal/create-subscription.png)

1. Web uygulamanızı yeniden görüntüleyin ve buna bir abonelik doğrulama olayının gönderildiğine dikkat edin. 

    Uç noktanın olay verilerini almak istediğini doğrulayabilmesi için Event Grid doğrulama olayını gönderir. Bitiş `validationResponse` `validationCode`noktası. Daha fazla bilgi için [Olay Izgara güvenliği ve kimlik doğrulaması'na](../../event-grid/security-authentication.md)bakın. Aboneliği nasıl doğruladığını görmek için web uygulama kodunu görüntüleyebilirsiniz.

Şimdi, Olay Idamı'nın iletiyi bitiş noktanıza nasıl dağıttığını görmek için olayları tetikleyelim.

## <a name="send-an-event-to-your-endpoint"></a>Uç noktanıza olay gönderme

Bir kodlama işi çalıştırarak Medya Hizmetleri hesabı için olayları tetikleyebilirsiniz. Bir dosyayı kodlamak ve olayları göndermeye başlamak için [bu hızlı başlatıizleyin.](stream-files-dotnet-quickstart.md) Tüm etkinliklere abone olduysanız, aşağıdakilere benzer bir ekran görürsünüz:

> [!TIP]
> Göz simgesini seçerek olay verilerini genişletin. Tüm olayları görüntülemek istiyorsanız sayfayı yenilemayın.

![Abonelik olayını görüntüleme](./media/monitor-events-portal/view-subscription-event.png)

## <a name="next-steps"></a>Sonraki adımlar

[Karşıya yükleme, kodlama ve akışla aktarma](stream-files-tutorial-with-api.md)
