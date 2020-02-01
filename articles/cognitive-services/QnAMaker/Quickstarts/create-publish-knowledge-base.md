---
title: 'Hızlı başlangıç: Bilgi Bankası oluşturma, eğitme ve yayımlama-Soru-Cevap Oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, kendi içerikinizden (örneğin, SSS veya ürün el kitabı) bir Soru-Cevap Oluşturma Bilgi Bankası (KB) oluşturma gösterilmektedir. Bu örnekteki Soru-Cevap Oluşturma Bilgi Bankası, BitLocker anahtar kurtarma hakkındaki sorulara yanıt vermek için basit bir SSS Web sayfasından oluşturulmuştur.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 01/29/2020
ms.author: diberry
ms.openlocfilehash: a3bdc118be96630ebcf3bf63a2948976dc9b4261
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76901681"
---
# <a name="quickstart-create-train-and-publish-your-qna-maker-knowledge-base"></a>Hızlı başlangıç: Soru-Cevap Oluşturma bilgi bankasını oluşturma, eğitme ve yayımlama

SSS sayfaları veya ürün kılavuzları gibi sahip olduğunuz içerikleri kullanarak bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturabilirsiniz. Bu makale, BitLocker anahtar kurtarma ile ilgili soruları yanıtlamak için basit bir SSS Web sayfasından Soru-Cevap Oluşturma bilgi tabanı oluşturma hakkında bir örnek içerir.

Bilgilerinizi kullanıcılarınız hakkında daha ilgi çekici hale getirmek için bir CHIT-chat kişilik ekleyin.

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisite"></a>Önkoşul

> [!div class="checklist"]
> * Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-new-qna-maker-knowledge-base"></a>Yeni bir Soru-Cevap Oluşturma Bilgi Bankası oluşturun

1. Azure kimlik bilgilerinizle [QnAMaker.ai](https://QnAMaker.ai) portalında oturum açın.

1. Soru-Cevap Oluşturma portalında **Bilgi Bankası oluştur**' u seçin.

1. **Oluştur** sayfasında, **QNA hizmeti oluştur**' u seçin. Aboneliğinizde bir Soru-Cevap Oluşturma hizmeti ayarlamak için [Azure portala](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) yönlendirilirsiniz.

1. Azure portal, kaynağı oluşturun. Kaynağı oluştururken seçtiğiniz Azure Active Directory KIMLIĞI, aboneliğiniz, QnA kaynak adınızı unutmayın.
1. Soru-Cevap Oluşturma portalına geri dönüp, bilgi bankanızı oluşturmaya devam etmek için portalda Web sayfasını yenileyin. Mevcut bir kiracı, abonelik ve yeni kaynak seçin. Dili seçin. Bu, bu Soru-Cevap Oluşturma hizmetindeki tüm bilgi tabanları için kullanılan dildir.

   ![Soru-Cevap Oluşturma Hizmeti Bilgi Bankası seçme ekran görüntüsü](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

1. Bilgi bankasını **örnek QnA KB**olarak adlandırın.

1. URL olarak örnek bir Word belgesi ekleyin:

    `https://docs.microsoft.com/azure/cognitive-services/qnamaker/troubleshooting`

1. `+ Add URL` öğesini seçin.

1. Bir  **_profesyonel_ ÇıT-** KB 'nize sohbet ekleyin.

1. Seçin **, KB oluşturma**.

    Ayıklama işlemi, belgeyi okuyup soruları ve yanıtları tanımlamak için birkaç dakika sürer.

    Bilgi Bankası Soru-Cevap Oluşturma başarıyla oluşturulduktan sonra **Bilgi Bankası** sayfası açılır. Bilgi Bankası 'nın içeriğini bu sayfada düzenleyebilirsiniz.

## <a name="add-a-new-question-and-answer-set"></a>Yeni soru ve yanıt kümesi ekleme

1. Soru-Cevap Oluşturma portalında, **Düzen** sayfasında, bağlam araç çubuğundan **+ QNA çifti Ekle** ' yi seçin.
1. Aşağıdaki soruyu ekleyin:

    `How many Azure services are used by a knowledge base?`

1. _Markaşağı_ile biçimlendirilen yanıtı ekleyin:

    ` * Azure QnA Maker service\n* Azure Cognitive Search\n* Azure web app\n* Azure app plan`

    ![ Soruyu metin olarak ve markaşağı ile biçimlendirilen yanıtı ekleyin.](../media/qnamaker-create-publish-knowledge-base/add-question-and-answer.png)

    `*`, markaşağı simgesi, madde işareti noktaları için kullanılır. `\n` yeni bir satır için kullanılır.

    **Düzenle** sayfası markın gösterir. Daha sonra **Test** panelini kullandığınızda markaşağı doğru görüntülendiğini görürsünüz.

## <a name="save-and-train"></a>Kaydet ve eğit

Sağ üst köşeden **Save and train** (Kaydet ve eğit) öğesine tıklayarak yaptığınız düzenlemeleri kaydedin ve Soru-Cevap Oluşturma modelinizi eğitin. Kaydedilmeyen düzenlemeler silinir.

## <a name="test-the-knowledge-base"></a>Bilgi Bankası 'nı test etme

1. Soru-Cevap Oluşturma portalında, sağ üst köşedeki **Test** ' i seçerek yaptığınız değişikliklerin etkili olduğunu test edin.
1. Metin kutusuna bir örnek Kullanıcı sorgusu girin.

    `How many Azure services are used by a knowledge base?`

    ![ Metin kutusuna bir örnek Kullanıcı sorgusu girin. ](../media/qnamaker-create-publish-knowledge-base/test-panel-in-qna-maker.png)

1. Yanıtı daha ayrıntılı bir şekilde incelemek için **Inspect** (Denetle) öğesini seçin. Bilgi tabanınızı yayımlamadan önce Bilgi Bankası 'nda yaptığınız değişiklikleri test etmek için test penceresi kullanılır.

1. **Test panelini kapatmak** Için yeniden **Test** ' i seçin.

## <a name="publish-the-knowledge-base"></a>Bilgi bankasını yayımlama

Bir Bilgi Bankası yayımladığınızda, bilgi Bankalarınızın içeriği `test` dizininden Azure Search 'te `prod` dizine gider.

![Bilgi Bankalarınızın içeriğini taşıma ekran görüntüsü](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

1. Soru-Cevap Oluşturma portalında **Yayımla**' yı seçin. Ardından onaylamak için sayfadaki **Publish** (Yayımla) öğesini seçin.

    Soru-Cevap Oluşturma hizmeti başarıyla yayımlandı. Uç noktayı uygulamanızda veya bot kodunuzda kullanabilirsiniz.

    ![Başarılı yayımlamanın ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/publish-knowledge-base-to-endpoint.png)

## <a name="create-a-bot"></a>Bot oluşturun

Yayımladıktan sonra **Yayımla** sayfasından bir bot oluşturabilirsiniz:

* Her biri için farklı bölgeler veya fiyatlandırma planları için aynı bilgi tabanına işaret eden her türlü botu hızlı bir şekilde oluşturabilirsiniz.
* Bilgi Bankası için yalnızca bir bot istiyorsanız, geçerli botunuzun bir listesini görüntülemek için **Azure Portal bağlantı çubuğunda tüm botlarınızı görüntüle** ' yi kullanın.

Bilgi bankasında değişiklikler yaptığınızda ve yeniden yayımladığınızda, bot ile daha fazla işlem yapmanız gerekmez. Bilgi Bankası ile çalışmak üzere zaten yapılandırılmıştır ve ileride Bilgi Bankası değişiklikleriyle birlikte çalışır. Bilgi Bankası 'nı her yayımladığınızda, kendisine bağlı olan tüm botlar otomatik olarak güncelleştirilir.

1. Soru-Cevap Oluşturma portalında, **Yayımla** sayfasında, **bot oluştur**' u seçin. Bu düğme yalnızca Bilgi Bankası 'nı yayımladıktan sonra görünür.

    ![Bot oluşturma ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page.png)

1. Azure bot hizmetinin oluşturma sayfası ile Azure portal için yeni bir tarayıcı sekmesi açılır. Azure bot hizmetini yapılandırın. Bot ve Soru-Cevap Oluşturma Web App Service planını paylaşabilir, ancak Web uygulamasını paylaşamaz. Bu, bot 'ın **uygulama adının** soru-cevap oluşturma hizmeti için uygulama adından farklı olması anlamına gelir.

    * **Gösterme**
        * Bot tanıtıcısını Değiştir-benzersiz değilse.
        * SDK dilini seçin. Bot oluşturulduktan sonra, kodu yerel geliştirme ortamınıza indirebilir ve geliştirme sürecine devam edebilirsiniz.
    * **Yapma**
        * bot oluştururken Azure portal aşağıdaki ayarları değiştirin. Bunlar, mevcut bilgi tabanınız için önceden doldurulur:
           * QnA auth anahtarı
           * App Service planı ve konumu


1. Bot oluşturulduktan sonra, **bot hizmeti** kaynağını açın.
1. **Bot Management**altında **Web sohbeti ' nda test**' i seçin.
1. **Iletinizi yazın**sohbet isteminde şunu girin:

    `Azure services?`

    Sohbet bot, bilgi tabanınızdan bir cevap vererek yanıt verir.

    ![Test Web sohbetine bir Kullanıcı sorgusu girin.](../media/qnamaker-create-publish-knowledge-base/test-web-chat.png)

## <a name="what-did-you-accomplish"></a>Ne başardınız?

Yeni bir Bilgi Bankası oluşturdunuz, Bilgi Bankası 'na genel bir URL eklediniz, kendi QnA kümesini eklediniz, eğitilen, test edildi ve Bilgi Bankası 'nı yayımladınız.

Bilgi Bankası 'nı yayımladıktan sonra, bir bot oluşturdunuz ve bot 'ı test ettikten sonra.

Bu, herhangi bir kod yazmak ve içeriği temizlemek zorunda kalmadan birkaç dakikada gerçekleştirildi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure portal Soru-Cevap Oluşturma ve bot Framework kaynaklarını temizleyin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için:

* [Yanıt olarak markaşağı biçimi](../reference-markdown-format.md)
* [Veri kaynaklarını](../concepts/knowledge-base.md)soru-cevap oluşturma.
* [Bot kaynak yapılandırma ayarları](../tutorials/create-qna-bot.md).

> [!div class="nextstepaction"]
> [Meta verilerle soru ekleme](add-question-metadata-portal.md)
