---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: trbye
ms.openlocfilehash: a41c9dc888e85baf021712e3302da7ae6106db47
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81422324"
---
Amaç tanıma hızlı başlangıcı ' nı tamamlayabilmeniz için, LUSıS önizleme portalını kullanarak bir LUO hesabı ve proje oluşturmanız gerekir. Bu hızlı başlangıçta yalnızca bir LUSıS aboneliği gerekir. Konuşma hizmeti aboneliği gerekli *değildir* .

Yapmanız gereken ilk şey, LUSıS önizleme portalını kullanarak bir LUO hesabı ve uygulaması oluşturmaktır. Oluşturduğunuz LUU uygulaması, giriş otomasyonu için, amaçları, varlıkları ve örnek utbotları sağlayan önceden oluşturulmuş bir etki alanı kullanacaktır. İşiniz bittiğinde, bulutta çalışan ve konuşma SDK 'sını kullanarak çağırabileceğinizi bir LUO uç noktası olur. 

LUSıS uygulamanızı oluşturmak için bu yönergeleri izleyin:

* <a href="https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app" target="_blank">Hızlı başlangıç: önceden oluşturulmuş etki alanı uygulaması oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>

İşiniz bittiğinde dört şeyin olması gerekir:

* Konuşma ile birlikte yeniden yayımlama **özelliği** açıldı
* LUSıS **birincil anahtarınız**
* LUSıS **konumunuz**
* LUSıS **Uygulama Kimliğiniz**

Burada, bu bilgileri [lusıs Önizleme portalında](https://preview.luis.ai/)bulabilirsiniz:

1. LUSıS önizleme portalından uygulamanızı seçin ve ardından **Yayınla** düğmesini seçin.

2. **Konuşma primi** seçeneğini **Açık** konuma Dönüştür ' ü `en-US` kullanıyorsanız **Üretim** yuvasını seçin. Ardından **Yayınla** düğmesini seçin.

    > [!IMPORTANT]
    > Konuşma tanıma doğruluğunu iyileştirecek kadar **konuşma Prime** kesinlikle önerilir.

    > [!div class="mx-imgBorder"]
    > ![LUSıS 'yi uç noktaya Yayımla](../../../media/luis/publish-app-popup.png)

3. LUSıS önizleme portalından **Yönet**' i ve ardından **Azure kaynakları**' nı seçin. Bu sayfada, LUSıS anahtarınızı ve konumunuzu (bazen _bölge_olarak adlandırılır) bulacaksınız.

   > [!div class="mx-imgBorder"]
   > ![LUSıS anahtar ve konumu](../../../media/luis/luis-key-region.png)

4. Anahtarınızı ve konumunuzu aldıktan sonra uygulama KIMLIĞI gerekir. **Uygulama ayarlarını** seçin--uygulama kimliğiniz bu sayfada bulunur.

   > [!div class="mx-imgBorder"]
   > ![LUSıS uygulama KIMLIĞI](../../../media/luis/luis-app-id.png)
