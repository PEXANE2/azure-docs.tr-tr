---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: dapine
ms.openlocfilehash: 58122066e65fbcb02b6b4333985785b219d3dbbd
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76900406"
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

2. ' I kullanıyorsanız, **konuşma primi** seçeneğini **açık** konuma değiştirmek `en-US` **Üretim** yuvasını seçin. Ardından **Yayınla** düğmesini seçin.

    > [!IMPORTANT]
    > Konuşma tanıma doğruluğunu iyileştirecek kadar **konuşma Prime** kesinlikle önerilir.

    > [!div class="mx-imgBorder"]
    > LUSıS 'yi uç noktaya Yayımla ![](../../../media/luis/publish-app-popup.png)

3. LUSıS önizleme portalından **Yönet**' i ve ardından **Azure kaynakları**' nı seçin. Bu sayfada, LUSıS anahtarınızı ve konumunuzu (bazen _bölge_olarak adlandırılır) bulacaksınız.

   > [!div class="mx-imgBorder"]
   > ![LUSıS anahtar ve konumu](../../../media/luis/luis-key-region.png)

4. Anahtarınızı ve konumunuzu aldıktan sonra uygulama KIMLIĞI gerekir. **Uygulama ayarlarını** seçin--uygulama kimliğiniz bu sayfada bulunur.

   > [!div class="mx-imgBorder"]
   > ![LUSıS uygulama KIMLIĞI](../../../media/luis/luis-app-id.png)
