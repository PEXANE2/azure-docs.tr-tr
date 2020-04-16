---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: trbye
ms.openlocfilehash: a41c9dc888e85baf021712e3302da7ae6106db47
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422324"
---
Niyet tanıma hızlı başlatılmasını tamamlamak için, LUIS önizleme portalını kullanarak bir LUIS hesabı ve bir proje oluşturmanız gerekir. Bu hızlı başlatma yalnızca bir LUIS aboneliği gerektirir. Konuşma hizmeti *aboneliği* gerekmez.

Yapmanız gereken ilk şey LUIS önizleme portalını kullanarak bir LUIS hesabı ve uygulaması oluşturmaktır. Oluşturduğunuz LUIS uygulaması, ev otomasyonu için, niyetler, varlıklar ve örnek söz lerle ilgili önceden oluşturulmuş bir etki alanı kullanır. İşi bittiğinde, Konuşma SDK'sını kullanarak arayabilirsiniz bulutta çalışan bir LUIS bitiş noktası nız olur. 

LUIS uygulamanızı oluşturmak için aşağıdaki yönergeleri izleyin:

* <a href="https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app" target="_blank">Quickstart: Önceden oluşturulmuş etki alanı uygulaması oluşturun<span class="docon docon-navigate-external x-hidden-focus"></span></a>

Bitirdiğinde, dört şeye ihtiyacın olacak:

* **Konuşma primi** ile yeniden yayımla
* LUIS **Birincil anahtarınız**
* LUIS **Konumunuz**
* LUIS **Uygulama Kimliğiniz**

Bu bilgileri [LUIS önizleme portalında](https://preview.luis.ai/)bulabileceğiniz yer:

1. LUIS önizleme portalından uygulamanızı seçin ve **ardından Yayındüğmesini** seçin.

2. **Konuşma astarseçeneğini** **Açık** konumuna `en-US` kaydırma seçeneğini kullanıyorsanız, **Üretim** yuvasını seçin. Ardından **Yayımla** düğmesini seçin.

    > [!IMPORTANT]
    > Konuşma tanıma doğruluğunu artıracağı için **konuşma astarı** şiddetle tavsiye edilir.

    > [!div class="mx-imgBorder"]
    > ![Luis'i bitiş noktasına yayımla](../../../media/luis/publish-app-popup.png)

3. LUIS önizleme portalından **Yönet'i**seçin ve ardından **Azure Kaynakları'nı**seçin. Bu sayfada, LUIS anahtarınızı ve konumunuzu (bazen _bölge_olarak da adlandırılır) bulacaksınız.

   > [!div class="mx-imgBorder"]
   > ![LUIS tuşu ve konumu](../../../media/luis/luis-key-region.png)

4. Anahtarınızı ve konumunuzu aldıktan sonra uygulama kimliğine ihtiyacınız olur. **Uygulama Ayarları'nı** seçin -- uygulama kimliğiniz bu sayfada mevcuttur.

   > [!div class="mx-imgBorder"]
   > ![LUIS uygulama kimliği](../../../media/luis/luis-app-id.png)
