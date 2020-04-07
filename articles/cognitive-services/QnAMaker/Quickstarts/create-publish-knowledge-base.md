---
title: 'Quickstart: Oluşturma, tren ve bilgi tabanı yayınlamak - QnA Maker'
description: SSS sayfaları veya ürün kılavuzları gibi sahip olduğunuz içerikleri kullanarak bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturabilirsiniz. Bu makalede, soru QnA Maker yanıtlamak için basit bir SSS web sayfasından bir QnA Maker bilgi tabanı oluşturma bir örnek içerir.
ms.topic: conceptual
ms.date: 02/08/2020
ms.openlocfilehash: 5a3cd4a66ae3a7148b517050c3d1a8c6c8e7a712
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756828"
---
# <a name="quickstart-create-train-and-publish-your-qna-maker-knowledge-base"></a>Quickstart: QnA Maker bilgi tabanınızı oluşturun, eğitin ve yayınlayın

SSS sayfaları veya ürün kılavuzları gibi sahip olduğunuz içerikleri kullanarak bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturabilirsiniz. Bu makalede, soru QnA Maker yanıtlamak için basit bir SSS web sayfasından bir QnA Maker bilgi tabanı oluşturma bir örnek içerir.

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.
> * Azure portalında oluşturulan bir QnA Maker [kaynağı.](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) Kaynağı oluşturduğunuzda seçtiğiniz Azure Active Directory ID, Abonelik, QnA kaynak adınızı hatırlayın.

## <a name="create-your-first-qna-maker-knowledge-base"></a>İlk QnA Maker bilgi tabanınızı oluşturun

1. Azure kimlik bilgilerinizle [QnAMaker.ai](https://QnAMaker.ai) portalında oturum açın.

1. QnA Maker portalında **bilgi tabanı oluştur'u**seçin.

1. **Oluştur** sayfasında, QnA Maker kaynağınız zaten varsa **Adım 1'i** atlayın.

    Kaynağı henüz oluşturmadıysanız, Bir **QnA hizmeti oluştur'u**seçin. Aboneliğinizde bir Soru-Cevap Oluşturma hizmeti ayarlamak için [Azure portala](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) yönlendirilirsiniz. Kaynağı oluşturduğunuzda seçtiğiniz Azure Active Directory ID, Abonelik, QnA kaynak adınızı hatırlayın.

    Azure portalında kaynağı oluşturmayı bitirdiğinizde, QnA Maker portalına dönün, tarayıcı sayfasını yenileyin ve **Adım 2'ye**devam edin.

1. **Adım**3'te, Etkin dizininizi, aboneliğinizi, hizmetinizi (kaynağı) ve hizmette oluşturulan tüm bilgi tabanlarının dilini seçin.

   ![QnA Maker hizmet bilgi tabanı seçme ekran görüntüsü](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

1. **Adım 3**, bilgi tabanı benim **Örnek QnA KB**adı .

1. **Adım**4'te, ayarları aşağıdaki tabloyla yapılandırın:

    |Ayar|Değer|
    |--|--|
    |**URL'lerden, .pdf veya .docx dosyalarından çoklu dönüş çıkarma olanağı sağlar.**|İşaretli|
    |**Varsayılan yanıt metni**| `Quickstart - default answer not found.`|
    |**+ URL ekle**|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/troubleshooting`|
    |**Chit-sohbet**|**Profesyonel'i** Seçin|

1. **Adım 5**, **KB oluştur'u**seçin.

    Ayıklama işleminin belgeyi okuması ve soruları ve yanıtları tanımlaması birkaç dakika sürer.

    QnA Maker bilgi tabanını başarıyla oluşturduktan sonra **Bilgi bankası** sayfası açılır. Bu sayfadaki bilgi tabanının içeriğini edinebilirsiniz.

## <a name="add-a-new-question-and-answer-set"></a>Yeni bir soru ve yanıt kümesi ekleme

1. QnA Maker portalında, **Düzenleme** sayfasında, bağlam araç çubuğundan **+ QnA çifti ekle'yi** seçin.
1. Aşağıdaki soruyu ekleyin:

    `How many Azure services are used by a knowledge base?`

1. _İşaretle_biçimlendirilmiş yanıtı ekleyin:

    ` * Azure QnA Maker service\n* Azure Cognitive Search\n* Azure web app\n* Azure app plan`

    ![ Soruyu metin olarak ve yanıtı işaretleme ile biçimlendirilmiş olarak ekleyin.](../media/qnamaker-create-publish-knowledge-base/add-question-and-answer.png)

    İşaretle işareti `*`simgesi, madde işaretleri için kullanılır. Yeni `\n` bir satır için kullanılır.

    **Edit** sayfası işaretlemeyi gösterir. **Test** panelini daha sonra kullandığınızda, işaretlemenin düzgün görüntülendiğini görürsünüz.

## <a name="save-and-train"></a>Kaydet ve eğit

Sağ üstte, **Kaydet'i** seçin ve dolandırılarınızı kurtarmak için trene binin ve QnA Maker'ı eğitin. Kaydedilmeyen düzenlemeler silinir.

## <a name="test-the-knowledge-base"></a>Bilgi tabanını test edin

1. QnA Maker portalında, sağ üstte yaptığınız değişikliklerin etkili olduğunu test etmek için **Test'i** seçin.
1. Textbox'a örnek bir kullanıcı sorgusu girin.

    `How many Azure services are used by a knowledge base?`

    ![ Textbox'a örnek bir kullanıcı sorgusu girin. ](../media/qnamaker-create-publish-knowledge-base/test-panel-in-qna-maker.png)

1. Yanıtı daha ayrıntılı bir şekilde incelemek için **Inspect** (Denetle) öğesini seçin. Test penceresi, bilgi tabanınızı yayımlamadan önce bilgi tabanındaki değişikliklerinizi sınamak için kullanılır.

1. **Test** panelini kapatmak için tekrar **Test'i** seçin.

## <a name="publish-the-knowledge-base"></a>Bilgi bankasını yayımlama

Bir bilgi tabanı yayımladığınızda, bilgi tabanınızın içeriği `test` Azure `prod` aramasında dizinden dizin olarak hareket eder.

![Bilgi tabanınızın içeriğini taşıma ekran görüntüsü](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

1. QnA Maker portalında **Yayımla'yı**seçin. Ardından onaylamak için sayfadaki **Publish** (Yayımla) öğesini seçin.

    Soru-Cevap Oluşturma hizmeti başarıyla yayımlandı. Uç noktayı uygulamanızda veya bot kodunuzda kullanabilirsiniz.

    ![Başarılı yayımlamanın ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/publish-knowledge-base-to-endpoint.png)

## <a name="create-a-bot"></a>Bir bot oluşturma

Yayımladıktan sonra **Yayımla** sayfasından bir bot oluşturabilirsiniz:

* Tüm farklı bölgeler veya bireysel botlar için fiyatlandırma planları için aynı bilgi tabanına işaret, hızlı bir şekilde birkaç botlar oluşturabilirsiniz.
* Bilgi tabanı için yalnızca bir bot istiyorsanız, geçerli botlarınızın listesini görüntülemek için **Azure portalı bağlantısındaki tüm botlarınızı** görüntüle'yi kullanın.

Bilgi tabanında değişiklik yaptığınızda ve yeniden yayımladığınızda, botla daha fazla işlem yapmanız gerekmez. Zaten bilgi tabanı ile çalışmak için yapılandırılmıştır ve bilgi tabanında gelecekteki tüm değişiklikler ile çalışır. Bir bilgi tabanını her yayımladığınızda, ona bağlı tüm botlar otomatik olarak güncellenir.

1. QnA Maker portalında, **Yayımla** **sayfasında, bot oluştur'u**seçin. Bu düğme yalnızca bilgi tabanını yayımladıktan sonra görüntülenir.

    ![Bir bot oluşturma ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page.png)

1. Azure Bot Hizmeti'nin oluşturma sayfasıyla birlikte Azure portalı için yeni bir tarayıcı sekmesi açılır. Azure bot hizmetini yapılandırın. Bot ve QnA Maker web uygulaması hizmet planını paylaşabilir, ancak web uygulamasını paylaşamaz. Bu, botun **uygulama adının** QnA Maker hizmetinin uygulama adından farklı olması gerektiği anlamına gelir.

    * **Yap**
        * Bot tutamacını değiştirin - benzersiz değilse.
        * SDK dilini seçin. Bot oluşturulduktan sonra, kodu yerel geliştirme ortamınıza indirebilir ve geliştirme sürecine devam edebilirsiniz.
    * **Yapmayın**
        * botu oluştururken Azure portalında aşağıdaki ayarları değiştirin. Mevcut bilgi tabanınız için önceden doldurulur:
           * QnA Auth Anahtar
           * Uygulama hizmet planı ve konumu


1. Bot oluşturulduktan sonra **Bot hizmet** kaynağını açın.
1. **Bot Yönetimi**altında, Web **Sohbet Test**seçin.
1. **İletinizi yazın'ın**sohbet isteminde, girin:

    `Azure services?`

    Sohbet botu bilgi tabanınızdan bir cevap ile yanıt verir.

    ![Test web sohbetine bir kullanıcı sorgusu girin.](../media/qnamaker-create-publish-knowledge-base/test-web-chat.png)

## <a name="what-did-you-accomplish"></a>Ne başardın?

Yeni bir bilgi tabanı oluşturdunuz, bilgi tabanına genel bir URL eklediniz, kendi QnA çiftinize eklediniz, eğitildiniz, test edindin ve bilgi tabanını yayınladınız.

Bilgi tabanını yayımladıktan sonra, bir bot oluşturdunuz ve botu test ettin.

Bu tüm herhangi bir kod yazmak veya içerik temizlemek zorunda kalmadan birkaç dakika içinde tamamlandı.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki hızlı başlatmaya devam etmiyorsanız, Azure portalındaki QnA Maker ve Bot çerçeve kaynaklarını silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Meta verilerle soru ekleme](add-question-metadata-portal.md)

Daha fazla bilgi için:

* [Yanıtlarda Işaretleme biçimi](../reference-markdown-format.md)
* QnA Maker [veri kaynakları](../concepts/knowledge-base.md).


