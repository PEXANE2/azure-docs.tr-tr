---
title: QnA Maker hizmeti ayarlama - QnA Maker
description: Herhangi bir QnA Maker bilgi tabanı oluşturmadan önce Azure'da bir QnA Maker hizmeti ayarlamanız gerekir. Abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir QnA Maker hizmeti ayarlayabilir.
ms.topic: conceptual
ms.date: 03/19/2020
ms.openlocfilehash: 8ec57f441ba58227e45398c35c7931dc75fa658f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131727"
---
# <a name="manage-qna-maker-resources"></a>QnA Maker kaynaklarını yönetme

Herhangi bir QnA Maker bilgi tabanı oluşturmadan önce Azure'da bir QnA Maker hizmeti ayarlamanız gerekir. Abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir QnA Maker hizmeti ayarlayabilir.

Aşağıdaki kavramların sağlam bir şekilde anlaşılması, kaynağınızı oluşturmadan önce yararlıdır:

* [QnA Maker kaynakları](../Concepts/azure-resources.md)
* [Anahtar yazma ve yayımlama](../Concepts/azure-resources.md#keys-in-qna-maker)

## <a name="create-a-new-qna-maker-service"></a>Yeni bir QnA Maker hizmeti oluşturun

Bu yordam, bilgi tabanı içeriğini yönetmek için gereken Azure kaynaklarını oluşturur. Bu adımları tamamladıktan sonra, Azure portalındaki kaynağın **Anahtarları** sayfasında _abonelik_ anahtarlarını bulabilirsiniz.

1. Azure portalında oturum açın ve [bir QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) kaynağı oluşturun.

1. Hüküm ve koşulları okuduktan sonra **Oluştur'u** seçin:

    ![Yeni bir QnA Maker hizmeti oluşturun](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. **QnA Maker'da**uygun katmanları ve bölgeleri seçin:

    ![Yeni bir QnA Maker hizmeti oluşturun - fiyatlandırma katmanı ve bölgeleri](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * **Ad** alanına, bu QnA Maker hizmetini tanımlamak için benzersiz bir ad girin. Bu ad, bilgi temellerinizin ilişkilendirileceği QnA Maker bitiş noktasını da tanımlar.
    * QnA Maker kaynağının dağıtılacayacağı **Aboneliyi** seçin.
    * QnA Maker yönetim hizmetleri (portal ve yönetim API'leri) için **Fiyatlandırma katmanını** seçin. [SKU fiyatlandırması hakkında daha fazla bilgi](https://aka.ms/qnamaker-pricing)için.
    * Yeni bir **Kaynak grubu** (önerilir) oluşturun veya bu QnA Maker kaynağını dağıtmak için varolan bir grup kullanın. QnA Maker birkaç Azure kaynağı oluşturur. Bu kaynakları tutmak için bir kaynak grubu oluşturduğunuzda, bu kaynakları kaynak grubu adına göre kolayca bulabilir, yönetebilir ve silebilirsiniz.
    * Kaynak **grubu konumunu**seçin.
    * Azure Bilişsel Arama hizmetinin **Arama fiyatlandırma katmanını** seçin. Ücretsiz katman seçeneği kullanılamıyorsa (soluk görünüyorsa), aboneliğiniz aracılığıyla zaten ücretsiz bir hizmetin olduğu anlamına gelir. Bu durumda, Temel katmanla başlamanız gerekir. Bkz. [Azure Bilişsel Arama fiyatlandırma ayrıntıları.](https://azure.microsoft.com/pricing/details/search/)
    * Azure Bilişsel Arama dizinlerinin dağıtılmasını istediğiniz **Arama konumunu** seçin. Müşteri verilerinin depolanması gereken kısıtlamalar, Azure Bilişsel Arama için seçtiğiniz konumu belirlemenize yardımcı olur.
    * Uygulama **adı** alanına Azure Uygulama Hizmeti örneğiniz için bir ad girin.
    * Varsayılan olarak, Uygulama Hizmeti varsayılan olarak standart (S1) katmanına uygun olur. Oluşturulduktan sonra planı değiştirebilirsiniz. [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/)hakkında daha fazla bilgi edinin.
    * Uygulama Hizmetinin dağıtılanacağı **Web Sitesi konumunu** seçin.

        > [!NOTE]
        > **Arama Konumu,** Web **Sitesi Konumundan**farklı olabilir.

    * **Uygulama Öngörülerini**etkinleştirmek isteyip istemediğinizi seçin. **Uygulama Öngörüleri** etkinse, QnA Maker trafikte, sohbet günlüklerinde ve hatalarda telemetri toplar.
    * Uygulama **Öngörüleri** kaynağının dağıtılacayacağı App insights konumunu seçin.
    * Maliyet tasarrufu önlemleri için, QnA Maker için oluşturulan tüm Azure kaynaklarını [paylaşabilirsiniz.](#configure-qna-maker-to-use-different-cognitive-search-resource)

1. Tüm alanlar doğrulandıktan sonra **Oluştur'u**seçin. İşlemin tamamlanması birkaç dakika sürebilir.

1. Dağıtım tamamlandıktan sonra aboneliğinizde oluşturulan aşağıdaki kaynakları görürsünüz:

   ![Kaynak yeni bir QnA Maker hizmeti oluşturdu](../media/qnamaker-how-to-setup-service/resources-created.png)

    _Bilişsel Hizmetler_ türüne sahip kaynak _abonelik_ anahtarlarınız vardır.

## <a name="find-subscription-keys-in-the-azure-portal"></a>Azure portalında abonelik anahtarlarını bulma

QnA Maker kaynağını oluşturduğunuz Azure portalından abonelik anahtarlarınızı görüntüleyebilir ve sıfırlayabilirsiniz.

1. Azure portalındaki QnA Maker kaynağına gidin ve _Bilişsel Hizmetler_ türüne sahip kaynağı seçin:

    ![QnA Maker kaynak listesi](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. **Tuşlara**Git :

    ![Abonelik anahtarı](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="find-endpoint-keys-in-the-qna-maker-portal"></a>QnA Maker portalında uç nokta tuşlarını bulun

Bitiş noktası anahtarları bilgi tabanına arama yapmak için kullanıldığından, bitiş noktası kaynakla aynı bölgededir.

Uç nokta tuşları [QnA Maker portalından](https://qnamaker.ai)yönetilebilir.

1. [QnA Maker portalında](https://qnamaker.ai)oturum açın, profilinize gidin ve ardından **Servis ayarlarını**seçin:

    ![Bitiş noktası tuşu](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Anahtarlarınızı görüntüleyin veya sıfırlanın:

    > [!div class="mx-imgBorder"]
    > ![Bitiş noktası anahtar yöneticisi](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Anahtarlarınızı açığa çıktığını düşünüyorsanız yenileyin. Bu, istemci uygulamanız veya bot kodunuzda ilgili değişiklikler gerektirebilir.

### <a name="upgrade-qna-maker-sku"></a>QnA Maker SKU yükseltme

Bilgi tabanınızda, mevcut seviyenizin ötesinde daha fazla soru ve yanıt almak istediğinizde, QnA Maker hizmet fiyatlandırma katmanınızı yükseltin.

QnA Maker yönetimi SKU yükseltmek için:

1. Azure portalındaki QnA Maker kaynağınıza gidin ve **Fiyatlandırma katmanını**seçin.

    ![QnA Maker kaynağı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. Uygun SKU'yu seçin ve **Seç**tuşuna basın.

    ![QnA Maker fiyatlandırma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

### <a name="upgrade-app-service"></a>Uygulama Hizmetini Yükseltme

 Bilgi tabanınızın müşteri uygulamanızdan daha fazla istek sunması gerektiğinde, Uygulama Hizmeti fiyatlandırma katmanınızı yükseltin.

App Service'i [büyütebilir](https://docs.microsoft.com/azure/app-service/manage-scale-up) veya ölçeklendirebilirsiniz.

Azure portalındaki Uygulama Hizmeti kaynağına gidin ve gerektiğinde **Ölçeklendir** veya **Ölçeklendir seçeneğini belirleyin.**

![QnA Maker Uygulama Hizmeti ölçeği](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Azure Bilişsel Arama hizmetini yükseltme

Çok sayıda bilgi tabanına sahip olmayı planlıyorsanız, Azure Bilişsel Arama hizmeti fiyatlandırma katmanınızı yükseltin.

Şu anda, Azure arama SKU'nun yerinde yükseltmesini gerçekleştiremezsiniz. Ancak, istenilen SKU ile yeni bir Azure arama kaynağı oluşturabilir, verileri yeni kaynağa geri yükleyebilir ve ardından QnA Maker yığınına bağlayabilirsiniz. Bunu yapmak için şu adımları uygulayın:

1. Azure portalında yeni bir Azure arama kaynağı oluşturun ve istediğiniz SKU'yu seçin.

    ![QnA Maker Azure arama kaynağı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Orijinal Azure arama kaynağınızdaki dizinleri yenisine geri yükleyin. Yedek [geri yükleme örnek koduna](https://github.com/pchoudhari/QnAMakerBackupRestore)bakın.

1. Veriler geri yüklendikten sonra yeni Azure arama kaynağınıza gidin, **Keys'i**seçin ve **Ad** ve **Yönetici anahtarını**yazın:

    ![QnA Maker Azure arama tuşları](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

1. Yeni Azure arama kaynağını QnA Maker yığınına bağlamak için QnA Maker App Service örneğine gidin.

    ![QnA Maker Uygulama Hizmeti örneği](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

1. **Uygulama ayarlarını** seçin ve **AzureSearchName** ve **AzureSearchAdminKey** alanlarındaki ayarları adım 3'ten değiştirin.

    ![QnA Maker Uygulama Hizmeti ayarı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

1. Uygulama Hizmeti örneğini yeniden başlatın.

    ![QnA Maker Uygulama Hizmeti örneğinin yeniden başlatılması](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="get-the-latest-runtime-updates"></a>En son çalışma zamanı güncelleştirmelerini alın

QnAMaker çalışma süresi, Azure portalında [bir QnAMaker hizmeti oluşturduğunuzda](./set-up-qnamaker-service-azure.md) dağıtılan Azure Uygulama Hizmeti örneğinin bir parçasıdır. Güncelleştirmeler çalışma saatinde düzenli aralıklarla yapılır. QnA Maker App Service örneği, Nisan 2019 site uzantısı sürümünden (sürüm 5+) sonra otomatik güncelleştirme modundadır. Bu güncelleştirme, yükseltmeler sırasında SIFIR kapalı kalma süresini dikkate almak üzere tasarlanmıştır.

Geçerli sürümünüzü https://www.qnamaker.ai/UserSettings'den kontrol edebilirsiniz. Sürümünüz sürüm 5.x'ten eskiyse, en son güncelleştirmeleri uygulamak için Uygulama Hizmetini yeniden başlatmanız gerekir:

1. [Azure portalındaki](https://portal.azure.com)QnAMaker hizmetinize (kaynak grubu) gidin.

    > [!div class="mx-imgBorder"]
    > ![QnAMaker Azure kaynak grubu](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. Uygulama Hizmeti örneğini seçin ve **Genel Bakış** bölümünü açın.

    > [!div class="mx-imgBorder"]
    > ![QnAMaker Uygulama Hizmeti örneği](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)


1. Uygulama Hizmetini Yeniden Başlatın. Güncelleştirme işlemi birkaç saniye içinde bitmelidir. Bu QnAMaker hizmetini kullanan bağımlı uygulamalar veya botlar bu yeniden başlatma döneminde son kullanıcılar tarafından kullanılamaz.

    ![QnAMaker Uygulama Hizmeti örneğinin yeniden başlatılması](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="configure-qna-maker-to-use-different-cognitive-search-resource"></a>QnA Maker'ı farklı Bilişsel Arama kaynağını kullanacak şekilde yapılandırın

Portal üzerinden bir QnA hizmeti ve onun bağımlılıklarını (Arama gibi) oluşturursanız, sizin için bir Arama hizmeti oluşturulur ve QnA Maker hizmetine bağlanır. Bu kaynaklar oluşturulduktan sonra, önceden varolan bir Arama hizmetini kullanmak ve yeni oluşturduğunuz hizmeti kaldırmak için Uygulama Hizmeti ayarını güncelleştirebilirsiniz.

QnA Maker'ın **Uygulama Hizmeti** kaynağı Bilişsel Arama kaynağını kullanır. QnA Maker tarafından kullanılan Bilişsel Arama kaynağını değiştirmek için Azure portalındaki ayarı değiştirmeniz gerekir.

1. QnA Maker'ın kullanmasını istediğiniz Bilişsel Arama kaynağının **Yönetici anahtarını** ve **Adını** alın.

1. [Azure portalında](https://portal.azure.com) oturum açın ve QnA Maker kaynağınızla ilişkili **Uygulama Hizmeti'ni** bulun. İkisi de aynı isme sahip.

1. **Ayarlar'ı**seçin, ardından **Yapılandırma'yı**. Bu, QnA Maker'ın Uygulama Hizmeti için varolan tüm ayarları görüntüler.

    > [!div class="mx-imgBorder"]
    > ![Uygulama Hizmeti yapılandırma ayarlarını gösteren Azure portalının ekran görüntüsü](../media/qnamaker-how-to-upgrade-qnamaker/change-search-service-app-service-configuration.png)

1. Aşağıdaki anahtarların değerlerini değiştirin:

    * **AzureSearchAdminKey**
    * **AzureSearchName**

1. Yeni ayarları kullanmak için Uygulama hizmetini yeniden başlatmanız gerekir. **Genel Bakış'ı**seçin, ardından **Yeniden Başlat'ı**seçin.

    > [!div class="mx-imgBorder"]
    > ![Yapılandırma ayarları değiştikten sonra Azure portalının Uygulama Hizmetini yeniden başlatma sının ekran görüntüsü](../media/qnamaker-how-to-upgrade-qnamaker/screenshot-azure-portal-restart-app-service.png)

Azure Kaynak Yöneticisi şablonları aracılığıyla bir QnA hizmeti oluşturursanız, tüm kaynakları oluşturabilir ve varolan bir Arama hizmetini kullanmak için Uygulama Hizmeti oluşturmayı denetleyebilirsiniz.

Uygulama Hizmeti [Uygulaması ayarlarını](../../../app-service/configure-common.md#configure-app-settings)yapılandırma hakkında daha fazla bilgi edinin.

## <a name="configure-app-service-idle-setting-to-avoid-timeout"></a>Zaman ayarı önlemek için Uygulama hizmetini boşta ayarlama

Yayınlanan bir bilgi tabanı için QnA Maker tahmin çalışma süresine hizmet veren uygulama hizmeti, hizmet boştaysa otomatik olarak zaman acısı için varsayılan bir zaman acısı yapılandırmasına sahiptir. QnA Maker için bu, tahmin çalışma sürenizin yanıt API'sini zaman zaman trafik siz dönemlerden sonra zaman zaman dışarı ürettiği anlamına gelir.

Trafik olmadığında bile tahmin bitiş noktası uygulamasını yüklü tutmak için boşta kalan uygulamayı her zaman adabına ayarlayın.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. QnA Maker kaynağınızın uygulama hizmetini arayın ve seçin. QnA Maker kaynağıyla aynı ada sahip olacak, ancak farklı bir Uygulama Hizmeti **türüne** sahip olacaktır.
1. **Ayarları** bul ardından **Yapılandırma'yı**seçin.
1. Yapılandırma bölmesinde **Genel ayarları**seçin, ardından **Her Zaman'ı**bulun ve değer olarak **Açık'ı** seçin.

    > [!div class="mx-imgBorder"]
    > ![Yapılandırma bölmesinde **Genel ayarları**' seçeneğini belirleyin, ardından **Always always**'yi bulun ve değer olarak **Açık** seçeneğini belirleyin.](../media/qnamaker-how-to-upgrade-qnamaker/configure-app-service-idle-timeout.png)

1. Yapılandırmayı kaydetmek için **Kaydet'i** seçin.
1. Yeni ayarı kullanmak için uygulamayı yeniden başlatmak isteyip istemediğiniz sorulur. **Devam**'ı seçin.

Uygulama Hizmeti [Genel ayarlarını](../../../app-service/configure-common.md#configure-general-settings)yapılandırma hakkında daha fazla bilgi edinin.

## <a name="delete-azure-resources"></a>Azure kaynaklarını silme

QnA Maker bilgi tabanlarınız için kullanılan Azure kaynaklarından herhangi birini silerseniz, bilgi tabanları artık çalışmaz. Kaynakları silmeden önce, bilgi temellerinizi **Ayarlar** sayfasından dışa aktardığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama hizmeti](../../../app-service/index.yml) ve Arama [hizmeti](../../../search/index.yml)hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma ve yayımlama](../Quickstarts/create-publish-knowledge-base.md)