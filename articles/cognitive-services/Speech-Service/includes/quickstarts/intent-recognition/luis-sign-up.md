---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: trbye
ms.openlocfilehash: 42dc7eb1a74bf2e376f834ee3198ad6a492226e9
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102444482"
---
Amaç tanıma hızlı başlangıcı ' nı tamamlayabilmeniz için, LUSıS önizleme portalını kullanarak bir LUO hesabı ve proje oluşturmanız gerekir. Bu hızlı başlangıçta yalnızca bir LUSıS aboneliği gerekir. Konuşma hizmeti aboneliği gerekli *değildir* .

Yapmanız gereken ilk şey, LUSıS önizleme portalını kullanarak bir LUO hesabı ve uygulaması oluşturmaktır. Oluşturduğunuz LUU uygulaması, giriş otomasyonu için, amaçları, varlıkları ve örnek utbotları sağlayan önceden oluşturulmuş bir etki alanı kullanacaktır. İşiniz bittiğinde, bulutta çalışan ve konuşma SDK 'sını kullanarak çağırabileceğinizi bir LUO uç noktası olur. 

LUSıS uygulamanızı oluşturmak için bu yönergeleri izleyin:

* <a href="https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app" target="_blank">Hızlı başlangıç: önceden oluşturulmuş etki alanı uygulaması oluşturma </a>

İşiniz bittiğinde dört şeyin olması gerekir:

* Konuşma ile birlikte yeniden yayımlama **özelliği** açıldı
* LUSıS **birincil anahtarınız**
* LUSıS **konumunuz**
* LUSıS **Uygulama Kimliğiniz**

Burada, bu bilgileri [lusıs Önizleme portalında](https://preview.luis.ai/)bulabilirsiniz:

1. LUSıS önizleme portalından uygulamanızı seçin ve ardından **Yayınla** düğmesini seçin.

2.  `en-US` **Konuşma primi** seçeneğini **Açık** konuma Dönüştür ' ü kullanıyorsanız üretim yuvasını seçin. Ardından **Yayınla** düğmesini seçin.

    > [!IMPORTANT]
    > Konuşma tanıma doğruluğunu iyileştirecek kadar **konuşma Prime** kesinlikle önerilir.

    > [!div class="mx-imgBorder"]
    > ![LUSıS 'yi uç noktaya Yayımla](../../../media/luis/publish-app-popup.png)

3. LUSıS önizleme portalından **Yönet**' i ve ardından **Azure kaynakları**' nı seçin. Bu sayfada, LUSıS anahtarınızı ve konumunuzu (bazen _bölge_ olarak adlandırılır) bulacaksınız.

   > [!div class="mx-imgBorder"]
   > ![LUSıS anahtar ve konumu](../../../media/luis/luis-key-region.png)

4. Anahtarınızı ve konumunuzu aldıktan sonra uygulama KIMLIĞI gerekir. **Uygulama ayarlarını** seçin--uygulama kimliğiniz bu sayfada bulunur.

   > [!div class="mx-imgBorder"]
   > ![LUSıS uygulama KIMLIĞI](../../../media/luis/luis-app-id.png)
