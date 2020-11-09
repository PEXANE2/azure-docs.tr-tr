---
title: 'Öğretici: Azure bot hizmeti ile bir SSS bot oluşturma'
description: Bu öğreticide, Soru-Cevap Oluşturma ve Azure bot hizmeti ile hiçbir kod hakkında SSS bot oluşturun.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 08/31/2020
ms.openlocfilehash: aa2530f817bf20af40ab2913df9f16e7d1974ef3
ms.sourcegitcommit: 051908e18ce42b3b5d09822f8cfcac094e1f93c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94375477"
---
# <a name="tutorial-create-an-faq-bot-with-azure-bot-service"></a>Öğretici: Azure bot hizmeti ile bir SSS bot oluşturma
Kod içermeyen Soru-Cevap Oluşturma ve Azure [bot hizmeti](https://azure.microsoft.com/services/bot-service/) Ile bir SSS bot oluşturun.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

<!-- green checkmark -->
> [!div class="checklist"]
> * Soru-Cevap Oluşturma Bilgi Bankası 'nı Azure bot hizmetine bağlama
> * Bot 'ı dağıtma
> * Web sohbetinde bot ile sohbet edin
> * Desteklenen kanallardaki bot 'ı aydınlatın

## <a name="create-and-publish-a-knowledge-base"></a>Bilgi bankası oluşturma ve yayımlama

Bilgi Bankası oluşturmak için [hızlı](../Quickstarts/create-publish-knowledge-base.md) başlangıcı izleyin. Bilgi Bankası başarıyla yayımlandıktan sonra, aşağıdaki sayfaya ulaşırsanız.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

![Başarılı yayımlamanın ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/publish-knowledge-base-to-endpoint.png)

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

![Başarılı yayımlama yönetilmesinin ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/publish-knowledge-base-to-endpoint-managed.png)

---

## <a name="create-a-bot"></a>Bot oluştur

Yayımladıktan sonra **Yayımla** sayfasından bir bot oluşturabilirsiniz:

* Her biri için farklı bölgeler veya fiyatlandırma planları için aynı bilgi tabanına işaret eden her türlü botu hızlı bir şekilde oluşturabilirsiniz.
* Bilgi Bankası için yalnızca bir bot istiyorsanız, geçerli botunuzun bir listesini görüntülemek için **Azure Portal bağlantı çubuğunda tüm botlarınızı görüntüle** ' yi kullanın.

Bilgi bankasında değişiklikler yaptığınızda ve yeniden yayımladığınızda, bot ile daha fazla işlem yapmanız gerekmez. Bilgi Bankası ile çalışmak üzere zaten yapılandırılmıştır ve ileride Bilgi Bankası değişiklikleriyle birlikte çalışır. Bilgi Bankası 'nı her yayımladığınızda, kendisine bağlı olan tüm botlar otomatik olarak güncelleştirilir.

1. Soru-Cevap Oluşturma portalında, **Yayımla** sayfasında, **bot oluştur** ' u seçin. Bu düğme yalnızca Bilgi Bankası 'nı yayımladıktan sonra görünür.

     # <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

    ![Bot oluşturma ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page.png)

    # <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

    ![Bot yönetilen önizlemesi oluşturma ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page-managed.png)

    ---
    

1. Azure bot hizmetinin oluşturma sayfası ile Azure portal için yeni bir tarayıcı sekmesi açılır. Azure bot hizmetini yapılandırın. Bot ve Soru-Cevap Oluşturma Web App Service planını paylaşabilir, ancak Web uygulamasını paylaşamaz. Bu, bot 'ın **uygulama adının** soru-cevap oluşturma hizmeti için uygulama adından farklı olması anlamına gelir.

    * **Gösterme**
        * Bot tanıtıcısını Değiştir-benzersiz değilse.
        * SDK dilini seçin. Bot oluşturulduktan sonra, kodu yerel geliştirme ortamınıza indirebilir ve geliştirme sürecine devam edebilirsiniz.
    * **Yapmayın**
        * Bot oluştururken Azure portal aşağıdaki ayarları değiştirin. Bunlar, mevcut bilgi tabanınız için önceden doldurulur:
           * QnA auth anahtarı
           * App Service planı ve konumu


1. Bot oluşturulduktan sonra, **bot hizmeti** kaynağını açın.
1. **Bot Management** altında **Web sohbeti ' nda test** ' i seçin.
1. **Iletinizi yazın** sohbet isteminde şunu girin:

    `Azure services?`

    Sohbet bot, bilgi tabanınızdan bir cevap vererek yanıt verir.

    :::image type="content" source="../media/qnamaker-create-publish-knowledge-base/test-web-chat.png" alt-text="Test Web sohbetine bir Kullanıcı sorgusu girin.":::
1. Ek [desteklenen kanallarda](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels?view=azure-bot-service-4.0&preserve-view=true)bot 'ı aydınlatın.
