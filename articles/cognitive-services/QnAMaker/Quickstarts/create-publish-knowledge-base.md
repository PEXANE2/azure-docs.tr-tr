---
title: 'Hızlı Başlangıç: Bilgi Bankası oluşturma, eğitme ve yayımlama-Soru-Cevap Oluşturma'
titleSuffix: Azure Cognitive Services
description: SSS sayfaları veya ürün kılavuzları gibi sahip olduğunuz içerikleri kullanarak bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturabilirsiniz. Bu örnekteki Soru-Cevap Oluşturma Bilgi Bankası, BitLocker anahtar kurtarma hakkındaki sorulara yanıt vermek için basit bir SSS Web sayfasından oluşturulmuştur.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 09/04/2019
ms.author: diberry
ms.openlocfilehash: 229956b41f7f26637e6a6aa62e341c4b06b8a429
ms.sourcegitcommit: aebe5a10fa828733bbfb95296d400f4bc579533c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70376373"
---
# <a name="quickstart-create-train-and-publish-your-qna-maker-knowledge-base"></a>Hızlı Başlangıç: Soru-Cevap Oluşturma bilgi bankanızı oluşturma, eğitme ve yayımlama

SSS sayfaları veya ürün kılavuzları gibi sahip olduğunuz içerikleri kullanarak bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturabilirsiniz. Bu makale, BitLocker anahtar kurtarma ile ilgili soruları yanıtlamak için basit bir SSS Web sayfasından Soru-Cevap Oluşturma bilgi tabanı oluşturma hakkında bir örnek içerir.

## <a name="prerequisite"></a>Önkoşul

> [!div class="checklist"]
> * Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-qna-maker-knowledge-base"></a>Soru-Cevap Oluşturma bilgi bankası oluşturma

1. Azure kimlik bilgilerinizle [QnAMaker.ai](https://QnAMaker.ai) portalında oturum açın.

1. Soru-Cevap Oluşturma portalında **Bilgi Bankası oluştur**' u seçin.

   ![Soru-Cevap Oluşturma portalının ekran görüntüsü](../media/qna-maker-create-kb.png)

1. **Create** (Oluştur) sayfasındaki 1. adımda **Create a QnA service** (Soru-Cevap hizmeti oluştur) öğesini seçin. Aboneliğinizde bir Soru-Cevap Oluşturma hizmeti ayarlamak için [Azure portala](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) yönlendirilirsiniz. Azure portal zaman aşımına uğrarsa sitede **Try again** (Yeniden deneyin) öğesini seçin. Bağlantı kurulduktan sonra Azure panonuz görünür.

1. Azure'da yeni bir Soru-Cevap Oluşturma hizmetini başarıyla oluşturduktan sonra qnamaker.ai/create sayfasına geri dönün. 2\. adımdaki açılan listelerden Soru-Cevap Oluşturma hizmetinizi seçin. Yeni bir Soru-Cevap Oluşturma Hizmeti oluşturduysanız, sayfayı yenilediğinizden emin olun.

   ![Soru-Cevap Oluşturma Hizmeti Bilgi Bankası seçme ekran görüntüsü](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

1. Adım 3 ' te bilgi tabanınızı **örnek QnA KB**olarak adlandırın.

1. Bilgi tabanınızı içerik eklemek için üç tür veri kaynağı seçin. 4\. adımdaki **Populate your KB** (KB'nizi doldurun) bölümünde **URL** kutusuna [BitLocker Kurtarma Hakkında SSS](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) sayfasının URL'sini girin.

   ![Veri kaynağı ekleme ekran görüntüsü](../media/qnamaker-quickstart-kb/add-datasources.png)

1. 5\. adımda **Create your KB** (KB'nizi oluşturun) öğesini seçin.

1. Bilgi Bankası Soru-Cevap Oluşturma oluştururken, bir açılır pencere görüntülenir. Ayıklama işleminin HTML sayfasını okuyup soruları ve yanıtları tanımlaması birkaç dakika sürer.

1. Bilgi Bankası Soru-Cevap Oluşturma başarıyla oluşturulduktan sonra **Bilgi Bankası** sayfası açılır. Bilgi Bankası 'nın içeriğini bu sayfada düzenleyebilirsiniz.

## <a name="edit-the-knowledge-base"></a>Bilgi Bankası 'nı düzenleme

1. Bilgi Bankası 'na yeni bir satır eklemek için Soru-Cevap Oluşturma portalında **Düzenle** bölümünde **QNA Pair Ekle** ' yi seçin. **Question** (Soru) bölümüne **Hi** (Merhaba) yazın. **Answer** (Cevap) bölümüne **Hello. Bana BitLocker sorularını sorma.**

    ![Soru-Cevap Oluşturma portalının ekran görüntüsü](../media/qnamaker-quickstart-kb/add-qna-pair.png)

1. Sağ üst köşeden **Save and train** (Kaydet ve eğit) öğesine tıklayarak yaptığınız düzenlemeleri kaydedin ve Soru-Cevap Oluşturma modelinizi eğitin. Kaydedilmeyen düzenlemeler silinir.

## <a name="test-the-knowledge-base"></a>Bilgi Bankası 'nı test etme

1. Soru-Cevap Oluşturma portalında, sağ üst köşedeki **Test** ' i seçerek yaptığınız değişikliklerin etkili olduğunu test edin. Kutuya `hi there` girin ve ENTER ' u seçin. Yanıt olarak oluşturduğunuz cevabı almanız gerekir.

1. Yanıtı daha ayrıntılı bir şekilde incelemek için **Inspect** (Denetle) öğesini seçin. Test penceresi, bilgi bankasından yayımlanmadan önce yaptığınız değişiklikleri test etmek için kullanılır.

    ![Test panelinin ekran görüntüsü](../media/qnamaker-quickstart-kb/inspect.png)

1. **Test**'i tekrar seçerek **Test** penceresini kapatın.

## <a name="publish-the-knowledge-base"></a>Bilgi bankasını yayımlama

Bir Bilgi Bankası yayımladığınızda, bilgi Bankalarınızın sorusu ve yanıt içerikleri, test dizininden Azure Search 'teki bir üretim dizinine gider.

![Bilgi Bankalarınızın içeriğini taşıma ekran görüntüsü](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

1. Soru-Cevap Oluşturma portalında, **Düzenle**' nin yanındaki menüde **Yayımla**' yı seçin. Ardından onaylamak için sayfadaki **Publish** (Yayımla) öğesini seçin.

1. Soru-Cevap Oluşturma hizmeti başarıyla yayımlandı. Uç noktayı uygulamanızda veya bot kodunuzda kullanabilirsiniz.

    ![Başarılı yayımlamanın ekran görüntüsü](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="create-a-bot"></a>Bot oluşturun

Yayımladıktan sonra **Yayımla** sayfasından bir bot oluşturabilirsiniz: 

* Her biri için farklı bölgeler veya fiyatlandırma planları için aynı bilgi tabanına işaret eden her türlü botu hızlı bir şekilde oluşturabilirsiniz. 
* Bilgi Bankası için yalnızca bir bot istiyorsanız, geçerli botunuzun bir listesini görüntülemek için **Azure Portal bağlantı çubuğunda tüm botlarınızı görüntüle** ' yi kullanın. 

Bilgi bankasında değişiklikler yaptığınızda ve yeniden yayımladığınızda, bot ile daha fazla işlem yapmanız gerekmez. Bilgi Bankası ile çalışmak üzere zaten yapılandırılmıştır ve ileride Bilgi Bankası değişiklikleriyle birlikte çalışır. Bilgi Bankası 'nı her yayımladığınızda, kendisine bağlı olan tüm botlar otomatik olarak güncelleştirilir.

1. Soru-Cevap Oluşturma portalında, **Yayımla** sayfasında, **bot oluştur**' u seçin. Bu düğme yalnızca Bilgi Bankası 'nı yayımladıktan sonra görünür.

    ![Bot oluşturma ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page.png)

1. Azure bot hizmetinin oluşturma sayfası ile Azure portal için yeni bir tarayıcı sekmesi açılır. Azure bot hizmetini yapılandırın. Bu yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure bot hizmeti v4 Ile QnA bot oluşturma](../tutorials/create-qna-bot.md).
    
    * Bot oluştururken Azure portal aşağıdaki ayarları değiştirmeyin. Bunlar, mevcut bilgi tabanınız için önceden doldurulur: 
        * QnA auth anahtarı
        * App Service planı ve konumu
        * Azure Storage
    * Bot ve Soru-Cevap Oluşturma Web App Service planını paylaşabilir, ancak Web uygulamasını paylaşamaz. Bu, **uygulama adının** soru-cevap oluşturma hizmeti oluştururken kullandığınız uygulama adından farklı olması gerektiği anlamına gelir. 


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma](../How-To/create-knowledge-base.md)
