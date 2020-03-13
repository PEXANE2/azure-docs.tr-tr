---
title: Soru-Cevap Oluşturma Hizmeti ayarlama-Soru-Cevap Oluşturma
description: Herhangi bir soru-cevap Oluşturucu bilgi bankalarından oluşturabilmeniz için önce bir soru-cevap Oluşturucu hizmetini azure'da ilk ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir soru-cevap Oluşturucu hizmetini ayarlayabilirsiniz.
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: 663cbce0e096c6189d97cf7872d466383d272f06
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79220594"
---
# <a name="manage-qna-maker-resources"></a>Soru-Cevap Oluşturma kaynaklarını yönetme

Herhangi bir soru-cevap Oluşturucu bilgi bankalarından oluşturabilmeniz için önce bir soru-cevap Oluşturucu hizmetini azure'da ilk ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir soru-cevap Oluşturucu hizmetini ayarlayabilirsiniz.

Kaynağı oluşturmadan önce aşağıdaki kavramların bir katı şekilde anlaşılması yararlı olur:

* [Soru-Cevap Oluşturma kaynakları](../Concepts/azure-resources.md)
* [Yazma ve yayımlama anahtarları](../Concepts/azure-resources.md#keys-in-qna-maker)

## <a name="create-a-new-qna-maker-service"></a>Yeni bir soru-cevap Oluşturucu hizmeti oluşturma

Bu yordam, Bilgi Bankası içeriğini yönetmek için gereken Azure kaynaklarını oluşturur. Bu adımları tamamladıktan sonra, Azure portal kaynak için **anahtarlar** sayfasında _abonelik_ anahtarlarını bulabilirsiniz.

1. Azure portal oturum açın ve [bir soru-cevap oluşturma kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) .

1. Hüküm ve koşulları okuduktan sonra **Oluştur** ' u seçin:

    ![Yeni bir soru-cevap Oluşturucu hizmeti oluşturma](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. **Soru-cevap oluşturma**, uygun katmanları ve bölgeleri seçin:

    ![Yeni bir soru-cevap Oluşturucu hizmeti - fiyatlandırma katmanı ve bölgeleri oluşturma](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * **Ad** alanına bu soru-cevap oluşturma hizmetini tanımlamak için benzersiz bir ad girin. Bu ad ayrıca, bilgi tabanlarınızın ilişkilendirileceği Soru-Cevap Oluşturma uç noktasını tanımlar.
    * Soru-Cevap Oluşturma kaynağın dağıtılacağı **aboneliği** seçin.
    * Soru-Cevap Oluşturma Management Services (portal ve yönetim API 'Leri) için **fiyatlandırma katmanını** seçin. [SKU fiyatlandırması hakkında daha fazla ayrıntı](https://aka.ms/qnamaker-pricing)için bkz.
    * Yeni bir **kaynak grubu** oluşturun (önerilir) veya bu soru-cevap oluşturma kaynağını dağıtmak için mevcut bir tane kullanın. Soru-Cevap Oluşturma çeşitli Azure kaynakları oluşturur. Bu kaynakları barındıracak bir kaynak grubu oluşturduğunuzda, bu kaynakları kaynak grubu adına göre kolayca bulabilir, yönetebilir ve silebilirsiniz.
    * **Kaynak grubu konumu**seçin.
    * Azure Bilişsel Arama hizmetinin **Ara fiyatlandırma katmanını** seçin. Ücretsiz katman seçeneği kullanılamıyorsa (soluk görünürse), aboneliğiniz aracılığıyla dağıtılan ücretsiz bir hizmetiniz zaten var demektir. Bu durumda, temel katmanla başlamanız gerekir. Bkz. [Azure bilişsel arama fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/).
    * Azure Bilişsel Arama dizinlerinin dağıtılmasını istediğiniz **Arama konumunu** seçin. Müşteri verilerinin depolanması gereken kısıtlamalar, Azure Bilişsel Arama için seçtiğiniz konumu belirlemenize yardımcı olur.
    * **Uygulama adı** alanına Azure App Service örneğiniz için bir ad girin.
    * Varsayılan olarak, varsayılan olarak standart (S1) katmanına App Service. Plan oluşturulduktan sonra değiştirebilirsiniz. [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/)hakkında daha fazla bilgi edinin.
    * App Service dağıtılacağı **Web sitesi konumunu** seçin.

        > [!NOTE]
        > **Arama konumu** , **Web sitesi konumundan**farklı olabilir.

    * **Application Insights**etkinleştirmek isteyip istemediğinizi seçin. **Application Insights** etkinleştirilirse, soru-cevap oluşturma trafik, sohbet günlükleri ve hatalar üzerinde telemetri toplar.
    * Application Insights kaynağın dağıtılacağı **App Insights konumunu** seçin.
    * Maliyet tasarrufu ölçüleri için, Soru-Cevap Oluşturma için oluşturulan tüm Azure kaynaklarını [paylaşabilirsiniz](#configure-qna-maker-to-use-different-cognitive-search-resource) .

1. Tüm alanlar doğrulandıktan sonra **Oluştur**' u seçin. İşlemin tamamlanması birkaç dakika sürebilir.

1. Dağıtım tamamlandıktan sonra, aboneliğinizde aşağıdaki kaynakların oluşturulduğunu görürsünüz:

   ![Yeni bir soru-cevap Oluşturucu hizmeti kaynağı oluşturuldu](../media/qnamaker-how-to-setup-service/resources-created.png)

    Bilişsel _Hizmetler_ türündeki kaynağın _abonelik_ anahtarları vardır.

## <a name="find-subscription-keys-in-the-azure-portal"></a>Azure portal abonelik anahtarlarını bulma

Abonelik anahtarlarınızı, Soru-Cevap Oluşturma kaynağı oluşturduğunuz Azure portal görüntüleyebilir ve sıfırlayabilirsiniz.

1. Azure portal Soru-Cevap Oluşturma kaynağına gidin ve bilişsel _Hizmetler_ türünün bulunduğu kaynağı seçin:

    ![Soru-cevap Oluşturucu kaynak listesi](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. **Anahtarlara**git:

    ![Abonelik anahtarı](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="find-endpoint-keys-in-the-qna-maker-portal"></a>Soru-Cevap Oluşturma portalında uç nokta anahtarlarını bulma

Uç nokta anahtarları bilgi tabanına çağrı yapmak için kullanıldığından, uç nokta kaynakla aynı bölgededir.

Uç nokta anahtarları [soru-cevap oluşturma portalından](https://qnamaker.ai)yönetilebilir.

1. [Soru-cevap oluşturma portalında](https://qnamaker.ai)oturum açın, profilinize gidin ve ardından **hizmet ayarları**' nı seçin:

    ![Uç noktası anahtarı](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Anahtarlarınızı görüntüleyin veya sıfırlayın:

    > [!div class="mx-imgBorder"]
    > ![uç noktası Anahtar Yöneticisi](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Tehlikede olduğunu düşünüyorsanız, anahtarlarınızı yenileyin. Bu, istemci uygulamanızda veya bot kodunuzda ilgili değişiklikleri gerektirebilir.

### <a name="upgrade-qna-maker-sku"></a>Soru-Cevap Oluşturma SKU 'YU yükselt

Bilgi tabanınızda, geçerli katmanınızın ötesinde daha fazla soru ve yanıt almak istediğinizde Soru-Cevap Oluşturma Hizmeti fiyatlandırma katmanınızı yükseltin.

Soru-Cevap Oluşturma yönetim SKU 'sunu yükseltmek için:

1. Azure portal Soru-Cevap Oluşturma kaynağına gidin ve **fiyatlandırma katmanı**' nı seçin.

    ![Soru-Cevap Oluşturma kaynağı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. Uygun SKU 'yu seçin ve **Seç**' e basın.

    ![Soru-Cevap Oluşturma fiyatlandırması](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

### <a name="upgrade-app-service"></a>App Service yükselt

 Bilgi tabanınızın istemci uygulamanızdan daha fazla istek sunması gerektiğinde App Service fiyatlandırma katmanınızı yükseltin.

App Service [ölçeklendirebilir](https://docs.microsoft.com/azure/app-service/manage-scale-up) veya ölçeklendirebilirsiniz.

Azure portal App Service kaynağına gidin ve gereken kadar **ölçeği yukarı** veya **genişletme** seçeneğini belirleyin.

![Soru-Cevap Oluşturma App Service ölçeği](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Azure Bilişsel Arama hizmetini yükseltme

Birçok bilgi tabanınız varsa, Azure Bilişsel Arama Service fiyatlandırma katmanınızı yükseltin.

Şu anda Azure Search SKU 'sunun yerinde yükseltmesini gerçekleştiremezsiniz. Ancak, istenen SKU ile yeni bir Azure Search kaynağı oluşturabilir, verileri yeni kaynağa geri yükleyebilir ve sonra Soru-Cevap Oluşturma yığınına bağlayabilirsiniz. Bunu yapmak için şu adımları uygulayın:

1. Azure portal yeni bir Azure Search kaynağı oluşturun ve istediğiniz SKU 'YU seçin.

    ![Azure Search kaynağı Soru-Cevap Oluşturma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Dizinleri özgün Azure Search kaynağından yeni bir kaynağa geri yükleyin. Bkz. [yedekleme geri yükleme örnek kodu](https://github.com/pchoudhari/QnAMakerBackupRestore).

1. Veriler geri yüklendikten sonra, yeni Azure Arama kaynağınız ' ne gidin, **anahtarlar**' ı seçin ve **adı** ve **yönetici anahtarını**yazın:

    ![Azure Arama anahtarları Soru-Cevap Oluşturma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

1. Yeni Azure Search kaynağını Soru-Cevap Oluşturma yığınına bağlamak için, Soru-Cevap Oluşturma App Service örneğine gidin.

    ![Soru-Cevap Oluşturma App Service örneği](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

1. **Uygulama ayarları** ' nı seçin ve 3. adımdaki **AzureSearchName** ve **AzureSearchAdminKey** alanlarındaki ayarları değiştirin.

    ![Soru-Cevap Oluşturma App Service ayarı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

1. App Service örneğini yeniden başlatın.

    ![Soru-Cevap Oluşturma App Service örneğinin yeniden başlatılması](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="get-the-latest-runtime-updates"></a>En son çalışma zamanı güncelleştirmelerini al

QnAMaker çalışma zamanı, Azure portal [bir qnaoluşturucu hizmeti oluşturduğunuzda](./set-up-qnamaker-service-azure.md) dağıtılan Azure App Service örneğinin bir parçasıdır. Güncelleştirmeler çalışma zamanı için düzenli aralıklarla hale getirilir. Soru-Cevap Oluşturma App Service örneği, Nisan 2019 site uzantısı sürümünden (sürüm 5 +) sonra otomatik güncelleştirme modunda. Bu güncelleştirme, yükseltmeler sırasında sıfır kesinti olması için tasarlanmıştır.

Güncel sürümünüzü https://www.qnamaker.ai/UserSettingsdenetleyebilirsiniz. Sürümünüz 5. x sürümünden eskiyse, en son güncelleştirmeleri uygulamak için App Service yeniden başlatmanız gerekir:

1. [Azure Portal](https://portal.azure.com)QnAMaker hizmetinize (kaynak grubu) gidin.

    > [!div class="mx-imgBorder"]
    > ![QnAMaker Azure Kaynak grubu](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. App Service örneğini seçin ve **genel bakış** bölümünü açın.

    > [!div class="mx-imgBorder"]
    > ![QnAMaker App Service örneği](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)


1. App Service yeniden başlatın. Güncelleştirme işlemi birkaç saniye içinde bitmelidir. Bu QnAMaker hizmetini kullanan bağımlı uygulamalar veya botlar, bu yeniden başlatma döneminde son kullanıcılar için kullanılamaz.

    ![QnAMaker App Service örneğini yeniden başlatma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="configure-qna-maker-to-use-different-cognitive-search-resource"></a>Soru-Cevap Oluşturma farklı Bilişsel Arama kaynağı kullanacak şekilde yapılandırma

Portal üzerinden bir QnA hizmeti ve bağımlılıklarını (arama gibi) oluşturursanız, sizin için bir arama hizmeti oluşturulur ve Soru-Cevap Oluşturma hizmetine bağlanır. Bu kaynaklar oluşturulduktan sonra, önceden var olan bir arama hizmetini kullanmak için App Service ayarını güncelleştirebilir ve yeni oluşturduğunuz birini kaldırabilirsiniz.

Soru-Cevap Oluşturma **App Service** kaynağı bilişsel arama kaynağını kullanır. Soru-Cevap Oluşturma tarafından kullanılan Bilişsel Arama kaynağını değiştirmek için Azure portal ayarı değiştirmeniz gerekir.

1. Soru-Cevap Oluşturma kullanmak istediğiniz Bilişsel Arama kaynağın **yönetici anahtarını** ve **adını** alın.

1. [Azure Portal](https://portal.azure.com) oturum açın ve soru-cevap oluşturma kaynağınız ile ilişkili **App Service** bulun. Her ikisi de aynı ada sahip.

1. **Ayarlar**' ı ve ardından **yapılandırma**' yı seçin. Bu, Soru-Cevap Oluşturma App Service tüm mevcut ayarlarını görüntüler.

    > [!div class="mx-imgBorder"]
    > App Service yapılandırma ayarlarını gösteren Azure portal ekran görüntüsünü ![](../media/qnamaker-how-to-upgrade-qnamaker/change-search-service-app-service-configuration.png)

1. Aşağıdaki anahtarlar için değerleri değiştirin:

    * **AzureSearchAdminKey**
    * **AzureSearchName**

1. Yeni ayarları kullanmak için App Service 'i yeniden başlatmanız gerekir. **Genel bakış**' ı ve ardından **Yeniden Başlat**' ı seçin

    > [!div class="mx-imgBorder"]
    > yapılandırma ayarları değişikliğinden sonra Azure portal yeniden başlatma App Service ekran görüntüsünü ![](../media/qnamaker-how-to-upgrade-qnamaker/screenshot-azure-portal-restart-app-service.png)

Azure Resource Manager şablonları aracılığıyla bir QnA hizmeti oluşturursanız, tüm kaynakları oluşturabilir ve App Service oluşturmayı, var olan bir arama hizmetini kullanacak şekilde denetleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama hizmeti](../../../app-service/index.yml) ve [Arama hizmeti](../../../search/index.yml)hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Bilgi Bankası oluşturma ve yayımlama](../Quickstarts/create-publish-knowledge-base.md)