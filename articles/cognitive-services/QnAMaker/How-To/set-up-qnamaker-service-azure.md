---
title: Soru-Cevap Oluşturma Hizmeti ayarlama-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Herhangi bir soru-cevap Oluşturucu bilgi bankalarından oluşturabilmeniz için önce bir soru-cevap Oluşturucu hizmetini azure'da ilk ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir soru-cevap Oluşturucu hizmetini ayarlayabilirsiniz.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: bec154a7e437621f377c503642b186cf166016cc
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195310"
---
# <a name="manage-qna-maker-resources"></a>Soru-Cevap Oluşturma kaynaklarını yönetme

Herhangi bir soru-cevap Oluşturucu bilgi bankalarından oluşturabilmeniz için önce bir soru-cevap Oluşturucu hizmetini azure'da ilk ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir soru-cevap Oluşturucu hizmetini ayarlayabilirsiniz.

## <a name="types-of-keys-in-qna-maker"></a>Soru-Cevap Oluşturma anahtar türleri

Soru-cevap Oluşturucu hizmetinizi anahtarları, iki tür ile ilgilenen **Abonelik anahtarları** ve **uç nokta anahtarları**.

![Anahtar Yönetimi](../media/qnamaker-how-to-key-management/key-management.png)

|Name|Location|Amaç|
|--|--|--|
|Abonelik Anahtarı|[Azure portal](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)|Bu anahtarlar [soru-cevap oluşturma Management Service API 'lerine](https://go.microsoft.com/fwlink/?linkid=2092179)erişmek için kullanılır. Bu API 'Ler, bilgi bankasındaki soruları ve yanıtları düzenlemenize ve bilgi tabanınızı yayımlamanıza olanak sağlar. Yeni bir Soru-Cevap Oluşturma hizmeti oluşturduğunuzda bu anahtarlar oluşturulur.<br><br>Bu anahtarları, **anahtarlar** sayfasında bilişsel **Hizmetler** kaynağında bulabilirsiniz.|
|Uç nokta anahtarı|[Soru-Cevap Oluşturma portalı](http://www.qnamaker.ai)|Bu anahtarlar, bir Kullanıcı sorusuna yanıt almak için yayımlanmış bilgi tabanı uç noktasına erişmek üzere kullanılır. Bu uç noktayı genellikle sohbet bot 'unuza veya Soru-Cevap Oluşturma hizmetine bağlanan istemci uygulaması koduna kullanırsınız. Bu anahtarlar Soru-Cevap Oluşturma bilgi bankasını yayımladığınızda oluşturulur.<br><br>Bu anahtarları **hizmet ayarları** sayfasında bulabilirsiniz. Bu sayfayı üstten, açılan menüdeki bir seçenek olarak sağ kullanıcının menüsünde bulabilirsiniz.|

## <a name="create-a-new-qna-maker-service"></a>Yeni bir soru-cevap Oluşturucu hizmeti oluşturma

Bu yordam, Bilgi Bankası içeriğini yönetmek için gereken Azure kaynaklarını oluşturur. Bu adımları tamamladığınızda, Azure portal kaynak için **anahtarlar** sayfasında _abonelik_ anahtarlarını bulabilirsiniz.

1. Azure portal oturum açın ve [bir soru-cevap oluşturma kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) .

1. Hüküm ve koşulları okuduktan sonra **Oluştur** ' u seçin.

    ![Yeni bir soru-cevap Oluşturucu hizmeti oluşturma](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. İçinde **soru-cevap Oluşturucu**, uygun katmanları ve bölgeleri seçin.

    ![Yeni bir soru-cevap Oluşturucu hizmeti - fiyatlandırma katmanı ve bölgeleri oluşturma](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Dolgu **adı** Bu soru-cevap Oluşturucu hizmetini tanımlamak için benzersiz bir ada sahip. Bu ad Ayrıca, bilgi bankalarından ilişkili edileceği soru-cevap Oluşturucu uç nokta tanımlar.
    * Seçin **abonelik** soru-cevap Oluşturucu kaynağın dağıtılacağı.
    * Soru-Cevap Oluşturma Management Services (portal ve yönetim API 'Leri) için **fiyatlandırma katmanını** seçin. Bkz: [burada](https://aka.ms/qnamaker-pricing) SKU'ları fiyatlandırması hakkında ayrıntılı bilgi için.
    * Yeni bir **kaynak grubu** (önerilir) veya bu soru-cevap Oluşturucu kaynak dağıtacağınız var olanı kullanın. Soru-Cevap Oluşturma birkaç Azure kaynağı oluşturur; Bu kaynakları barındıracak bir kaynak grubu oluşturduğunuzda, bu kaynakları kaynak grubu adına göre kolayca bulabilir, yönetebilir ve silebilirsiniz.
    * **Kaynak grubu konumu**seçin.
    * Seçin **arama fiyatlandırma katmanı** Azure Search hizmeti. Gri ücretsiz katmanı seçeneğini görürseniz, aboneliğinizde bir ücretsiz Azure arama katmanı zaten sahip olduğunuz anlamına gelir. Bu durumda Azure arama temel katman ile başlatmanız gerekir. Azure arama fiyatlandırma ayrıntılarına [burada](https://azure.microsoft.com/pricing/details/search/).
    * Seçin **arama konumu** dağıtılacak Azure Search veri istediğiniz. Müşteri verilerinin nerede depolanacağını gerekir, kısıtlamaları, Azure arama için seçtiğiniz konumu bilgilendirecektir.
    * App service içinde bir ad verin **uygulama adı**.
    * Varsayılan olarak App service için standart (S1) katman varsayılan olarak. Plan oluşturulduktan sonra değiştirebilirsiniz. App service fiyatlandırması hakkında daha fazla ayrıntı görmek [burada](https://azure.microsoft.com/pricing/details/app-service/).
    * Seçin **Web sitesi konumu** App Service dağıtılacağı.

        > [!NOTE]
        > Arama konumu Web sitesi konumundan farklı olabilir.

    * Etkinleştirmek isteyip istemediğinizi seçin **Application Insights** veya yok. Varsa **Application Insights** olan etkin, soru-cevap Oluşturucu telemetri trafik, sohbet günlükleri ve hataları toplar.
    * Seçin **uygulama öngörülerinin konumu** Application Insights kaynağı dağıtılacağı.
    * Maliyet tasarrufu ölçüleri için, Soru-Cevap Oluşturma için oluşturulan tüm Azure kaynaklarını [paylaşabilirsiniz](#share-existing-services-with-qna-maker) . 

1. Tüm alanlar doğrulandıktan sonra **Oluştur**' u seçin. Tamamlanması birkaç dakika sürebilir.

1. Dağıtım tamamlandığında, aboneliğinizde oluşturduğunuz aşağıdaki kaynakları görürsünüz.

    ![Yeni bir soru-cevap Oluşturucu hizmeti kaynağı oluşturuldu](../media/qnamaker-how-to-setup-service/resources-created.png)

    Bilişsel _Hizmetler_ tür kaynağı _abonelik_ Anahtarlarınıza sahiptir.

## <a name="find-subscription-keys-in-azure-portal"></a>Azure portal 'de abonelik anahtarlarını bulma

Soru-Cevap Oluşturma kaynağını oluşturduğunuz Azure portal düzenlediğiniz aboneliğinizi düzenleyebilmeniz için abonelik anahtarlarınızı görüntüleyebilir ve sıfırlayabilirsiniz. 

1. Azure portal Soru-Cevap Oluşturma kaynağına gidin ve bilişsel _Hizmetler_türünde kaynağı seçin.

    ![Soru-cevap Oluşturucu kaynak listesi](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Git **anahtarları**.

    ![Abonelik anahtarı](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="find-endpoint-keys-in-qna-maker-portal"></a>Soru-Cevap Oluşturma portalında uç nokta anahtarlarını bulma

Uç nokta anahtarları alanından yönetilebilir [soru-cevap Oluşturucu portalı](https://qnamaker.ai).

1. [Soru-cevap oluşturma portalında](https://qnamaker.ai)oturum açın, profilinize gidin ve ardından **hizmet ayarları**' na tıklayın.

    ![Uç noktası anahtarı](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Görüntüleyin ya da anahtarlarınızı sıfırlayın.

    ![uç nokta Anahtar Yöneticisi](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Anahtarlarınızı tehlikeye girmiş düşünüyorsanız yenileyin. Bu, istemci uygulamanızda veya bot kodunuzda ilgili değişiklikleri gerektirebilir.

## <a name="share-existing-services-with-qna-maker"></a>Mevcut hizmetleri Soru-Cevap Oluşturma ile paylaşma

Soru-Cevap Oluşturma çeşitli Azure kaynakları oluşturur. Yönetim ve maliyet paylaşımının avantajlarından yararlanmak için aşağıdaki tabloyu kullanarak neleri paylaşabdiklerinizi ve neleri paylaşabileceğinizi öğrenin:

|Hizmet|Paylaş|Reason|
|--|--|--|
|Bilişsel Hizmetler|X|Tasarım tarafından mümkün değil|
|Uygulama hizmeti planı|✔|Bir App Service planı için ayrılan sabit disk alanı. Diğer uygulamalar, aynı App Service planını paylaşıyorsanız, önemli disk alanını kullanın, QnAMaker App Service sorunlar halinde çalışacaktır.|
|App Service|X|Tasarım tarafından mümkün değil|
|Application Insights|✔|Paylaşılabilir|
|Arama hizmeti|✔|1. `testkb` qnamaker hizmeti için ayrılmış bir addır, diğerleri tarafından kullanılamaz.<br>2. Ad `synonym-map` ile eş anlamlı eşleme qnamaker hizmeti için ayrılmıştır.<br>3. Yayınlanan KBs sayısı arama hizmeti katmanıyla sınırlıdır. Kullanılabilir ücretsiz dizinler varsa, diğer hizmetler bunu kullanabilir.|

### <a name="using-a-single-search-service"></a>Tek bir arama hizmeti kullanma

Portal üzerinden bir QnA hizmeti ve bağımlılıklarını (arama gibi) oluşturursanız, sizin için bir arama hizmeti oluşturulur ve Soru-Cevap Oluşturma hizmetine bağlanır. Bu kaynaklar oluşturulduktan sonra, önceden var olan bir arama hizmetini kullanmak için App Service ayarını güncelleştirebilir ve yalnızca oluşturulan arama hizmetini kaldırabilirsiniz.

Azure Resource Manager şablonları aracılığıyla bir QnA hizmeti oluşturursanız, mevcut bir arama hizmetini kullanmak için tüm kaynakları oluşturabilir ve App Service oluşturmayı denetleyebilirsiniz. 

## <a name="upgrade-qna-maker"></a>Soru-Cevap Oluşturma yükselt

|Yükseltme|Reason|
|--|--|
|[Yükseltme](#upgrade-qna-maker-sku) Soru-Cevap Oluşturma Management SKU 'su|Bilgi bankasında daha fazla sorunuz ve yanıt almanız gerekir.|
|[Yükseltme](#upgrade-app-service) App Service SKU 'su|Bilgi tabanınız, bir sohbet bot gibi istemci uygulamanızdan daha fazla istek sunması gerekir.|
|[Yükseltme](#upgrade-azure-search-service) Azure Search hizmeti|Birçok Bilgi Bankası 'na sahip olmanız planlanır.|


### <a name="upgrade-qna-maker-sku"></a>Soru-Cevap Oluşturma SKU 'yu yükselt

Bilgi tabanınızda, geçerli katmanınızın ötesinde daha fazla soru ve yanıt almanız gerektiğinde Soru-Cevap Oluşturma Hizmeti fiyatlandırma katmanınızı yükseltin. 

Soru-Cevap Oluşturma yönetim SKU 'sunu yükseltmek için:

1. Azure portal Soru-Cevap Oluşturma kaynağına gidin ve **fiyatlandırma katmanı**' nı seçin.

    ![Soru-Cevap Oluşturma kaynağı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. Uygun SKU 'yu seçin ve **Seç**' e basın.

    ![Soru-Cevap Oluşturma fiyatlandırması](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

### <a name="upgrade-app-service"></a>App Service 'i yükselt

 Bilgi tabanınızın istemci uygulamanızdan daha fazla istek sunması gerektiğinde, App Service fiyatlandırma katmanınızı yükseltin.

Uygulama hizmetinin [ölçeğini](https://docs.microsoft.com/azure/app-service/manage-scale-up) değiştirebilir veya azaltabilirsiniz.

Azure portal App Service kaynağına gidin ve gerektiğinde **ölçeği yukarı** veya **aşağı genişlet** seçeneğini belirleyin.

![Soru-Cevap Oluşturma App Service ölçeği](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

### <a name="upgrade-azure-search-service"></a>Azure Search hizmetini yükselt

Birçok bilgi tabanınız olduğunu planlarken Azure Search hizmeti fiyatlandırma katmanınızı yükseltin. 

Şu anda Azure Search SKU 'sunda yerinde yükseltme yapmak mümkün değildir. Ancak, istenen SKU ile yeni bir Azure Search kaynağı oluşturabilir, verileri yeni kaynağa geri yükleyebilir ve sonra Soru-Cevap Oluşturma yığınına bağlayabilirsiniz.

1. Azure portal yeni bir Azure Search kaynağı oluşturun ve istediğiniz SKU 'yu seçin.

    ![Azure Search kaynağı Soru-Cevap Oluşturma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Dizinleri özgün Azure Search kaynağından yeni bir kaynağa geri yükleyin. Yedekleme geri yükleme örnek koduna [buradan](https://github.com/pchoudhari/QnAMakerBackupRestore)bakın.

1. Veriler geri yüklendikten sonra, yeni Azure Arama kaynağınız ' ne gidin, **anahtarlar**' ı seçin ve **adı** ve **yönetici anahtarı**' nı aklınızda olun.

    ![Azure Arama anahtarları Soru-Cevap Oluşturma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

1. Yeni Azure Search kaynağını Soru-Cevap Oluşturma yığınına bağlamak için Soru-Cevap Oluşturma App Service 'e gidin.

    ![Soru-Cevap Oluşturma appservice](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

1. **Uygulama ayarları** ' nı seçin ve 3. adımdaki **AzureSearchName** ve **AzureSearchAdminKey** alanlarını değiştirin.

    ![Soru-Cevap Oluşturma appservice ayarı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

1. App service'ı yeniden başlatın.

    ![Appservice 'i Soru-Cevap Oluşturma yeniden başlatma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="get-latest-runtime-updates"></a>En son çalışma zamanı güncelleştirmelerini al

QnAMaker çalışma zamanı, Azure portal [bir qnaoluşturucu hizmeti oluşturduğunuzda](./set-up-qnamaker-service-azure.md) dağıtılan Azure App Service bir parçasıdır. Güncelleştirmeler çalışma zamanı için düzenli aralıklarla hale getirilir. Soru-Cevap Oluşturma App Service, Nisan 2019 site uzantısı sürümünden (sürüm 5 +) sonra otomatik güncelleştirme modunda. Bu, yükseltme sırasında sıfır kesinti olması için zaten tasarlanmıştır. 

Geçerli sürümünüzü adresinde https://www.qnamaker.ai/UserSettings denetleyebilirsiniz. Sürümünüz 5. x sürümünden eskiyse, en son güncelleştirmeleri uygulamak için App Service yeniden başlatmanız gerekir.

1. QnAMaker hizmetinize (kaynak grubu) Git [Azure portalı](https://portal.azure.com)

    ![QnAMaker Azure kaynak grubu](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. App Service üzerinde tıklayın ve genel bakış bölümünü açın

     ![QnAMaker App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

1. App service'ı yeniden başlatın. Bu işlem birkaç saniye içinde tamamlanmalıdır. Bu QnAMaker hizmetini kullanan bağımlı uygulamalar veya botlar, bu yeniden başlatma döneminde son kullanıcılar için kullanılamaz.

    ![QnAMaker appservice yeniden başlatma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="management-service-region"></a>Yönetim hizmeti bölgesi

Yalnızca ilk veri işleme için Soru-Cevap Oluşturma Portal & için kullanılan Soru-Cevap Oluşturma yönetim hizmeti yalnızca Batı ABD içinde kullanılabilir. Bu Batı ABD hizmetinde hiçbir müşteri verisi depolanmaz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oluşturma ve Bilgi Bankası yayımlama](../Quickstarts/create-publish-knowledge-base.md)
