---
title: Soru-Cevap Oluşturma Hizmeti ayarlama-Soru-Cevap Oluşturma
description: Herhangi bir Soru-Cevap Oluşturma bilgi tabanı oluşturabilmeniz için önce Azure 'da bir Soru-Cevap Oluşturma Hizmeti ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturmak için yetkilendirmeye sahip olan herkes, Soru-Cevap Oluşturma bir hizmet ayarlayabilir.
ms.topic: conceptual
ms.date: 07/13/2020
ms.openlocfilehash: 7ba8134f58a4f0e4e26a3246a44574df295e3c20
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87040374"
---
# <a name="manage-qna-maker-resources"></a>Soru-Cevap Oluşturma kaynaklarını yönetme

Herhangi bir Soru-Cevap Oluşturma bilgi tabanı oluşturabilmeniz için önce Azure 'da bir Soru-Cevap Oluşturma Hizmeti ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturmak için yetkilendirmeye sahip olan herkes, Soru-Cevap Oluşturma bir hizmet ayarlayabilir.

Kaynağı oluşturmadan önce aşağıdaki kavramların bir katı şekilde anlaşılması yararlı olur:

* [Soru-Cevap Oluşturma kaynakları](../Concepts/azure-resources.md)
* [Yazma ve yayımlama anahtarları](../Concepts/azure-resources.md#keys-in-qna-maker)

## <a name="create-a-new-qna-maker-service"></a>Yeni bir Soru-Cevap Oluşturma hizmeti oluşturun

Bu yordam, Bilgi Bankası içeriğini yönetmek için gereken Azure kaynaklarını oluşturur. Bu adımları tamamladıktan sonra, Azure portal kaynak için **anahtarlar** sayfasında _abonelik_ anahtarlarını bulabilirsiniz.

1. Azure portal oturum açın ve [bir soru-cevap oluşturma kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) .

1. Hüküm ve koşulları okuduktan sonra **Oluştur** ' u seçin:

    ![Yeni bir Soru-Cevap Oluşturma hizmeti oluşturun](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. **Soru-cevap oluşturma**, uygun katmanları ve bölgeleri seçin:

    ![Yeni bir Soru-Cevap Oluşturma hizmeti oluşturma-fiyatlandırma katmanı ve bölgeler](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * **Ad** alanına bu soru-cevap oluşturma hizmetini tanımlamak için benzersiz bir ad girin. Bu ad ayrıca, bilgi tabanlarınızın ilişkilendirileceği Soru-Cevap Oluşturma uç noktasını tanımlar.
    * Soru-Cevap Oluşturma kaynağın dağıtılacağı **aboneliği** seçin.
    * Soru-Cevap Oluşturma Management Services (portal ve yönetim API 'Leri) için **fiyatlandırma katmanını** seçin. [SKU fiyatlandırması hakkında daha fazla ayrıntı](https://aka.ms/qnamaker-pricing)için bkz..
    * Yeni bir **kaynak grubu** oluşturun (önerilir) veya bu soru-cevap oluşturma kaynağını dağıtmak için mevcut bir tane kullanın. Soru-Cevap Oluşturma çeşitli Azure kaynakları oluşturur. Bu kaynakları barındıracak bir kaynak grubu oluşturduğunuzda, bu kaynakları kaynak grubu adına göre kolayca bulabilir, yönetebilir ve silebilirsiniz.
    * **Kaynak grubu konumu**seçin.
    * Azure Bilişsel Arama hizmetinin **Ara fiyatlandırma katmanını** seçin. Ücretsiz katman seçeneği kullanılamıyorsa (soluk görünürse), aboneliğiniz aracılığıyla dağıtılan ücretsiz bir hizmetiniz zaten var demektir. Bu durumda, temel katmanla başlamanız gerekir. Bkz. [Azure bilişsel arama fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/).
    * Azure Bilişsel Arama dizinlerinin dağıtılmasını istediğiniz **Arama konumunu** seçin. Müşteri verilerinin depolanması gereken kısıtlamalar, Azure Bilişsel Arama için seçtiğiniz konumu belirlemenize yardımcı olur.
    * **Uygulama adı** alanına Azure App Service örneğiniz için bir ad girin.
    * Varsayılan olarak, varsayılan olarak standart (S1) katmanına App Service. Oluşturulduktan sonra planı değiştirebilirsiniz. [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/)hakkında daha fazla bilgi edinin.
    * App Service dağıtılacağı **Web sitesi konumunu** seçin.

        > [!NOTE]
        > **Arama konumu** , **Web sitesi konumundan**farklı olabilir.

    * **Application Insights**etkinleştirmek isteyip istemediğinizi seçin. **Application Insights** etkinleştirilirse, soru-cevap oluşturma trafik, sohbet günlükleri ve hatalar üzerinde telemetri toplar.
    * Application Insights kaynağın dağıtılacağı **App Insights konumunu** seçin.
    * Maliyet tasarrufu ölçüleri için, Soru-Cevap Oluşturma için oluşturulan tüm Azure kaynaklarını [paylaşabilirsiniz](#configure-qna-maker-to-use-different-cognitive-search-resource) .

1. Tüm alanlar doğrulandıktan sonra **Oluştur**' u seçin. İşlemin tamamlanması birkaç dakika sürebilir.

1. Dağıtım tamamlandıktan sonra, aboneliğinizde aşağıdaki kaynakların oluşturulduğunu görürsünüz:

   ![Kaynak yeni bir Soru-Cevap Oluşturma Hizmeti oluşturdu](../media/qnamaker-how-to-setup-service/resources-created.png)

    Bilişsel _Hizmetler_ türündeki kaynağın _abonelik_ anahtarları vardır.


## <a name="find-authoring-keys-in-the-azure-portal"></a>Azure portal yazma anahtarlarını bulma

Yazma anahtarlarınızı, Soru-Cevap Oluşturma kaynağı oluşturduğunuz Azure portal görüntüleyebilir ve sıfırlayabilirsiniz. Bu anahtarlara abonelik anahtarları denir.

1. Azure portal Soru-Cevap Oluşturma kaynağına gidin ve bilişsel _Hizmetler_ türünün bulunduğu kaynağı seçin:

    ![Soru-Cevap Oluşturma kaynak listesi](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. **Anahtarlara**git:

    ![Abonelik anahtarı](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="find-query-endpoint-keys-in-the-qna-maker-portal"></a>Soru-Cevap Oluşturma portalında sorgu uç noktası anahtarlarını bulma

Uç nokta anahtarları bilgi tabanına çağrı yapmak için kullanıldığından, uç nokta kaynakla aynı bölgededir.

Uç nokta anahtarları [soru-cevap oluşturma portalından](https://qnamaker.ai)yönetilebilir.

1. [Soru-cevap oluşturma portalında](https://qnamaker.ai)oturum açın, profilinize gidin ve ardından **hizmet ayarları**' nı seçin:

    ![Uç nokta anahtarı](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Anahtarlarınızı görüntüleyin veya sıfırlayın:

    > [!div class="mx-imgBorder"]
    > ![Uç nokta anahtar Yöneticisi](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Tehlikede olduğunu düşünüyorsanız, anahtarlarınızı yenileyin. Bu, istemci uygulamanızda veya bot kodunuzda ilgili değişiklikleri gerektirebilir.

### <a name="update-the-resources"></a>Kaynakları güncelleştirme

Bilgi tabanınız tarafından kullanılan kaynakları nasıl yükselteceğinizi öğrenin.

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

Şu anda Azure Search SKU 'sunun yerinde yükseltmesini gerçekleştiremezsiniz. Ancak, istenen SKU ile yeni bir Azure Search kaynağı oluşturabilir, verileri yeni kaynağa geri yükleyebilir ve sonra Soru-Cevap Oluşturma yığınına bağlayabilirsiniz. Bunu yapmak için şu adımları izleyin:

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

QnAMaker çalışma zamanı, Azure portal [bir qnaoluşturucu hizmeti oluşturduğunuzda](./set-up-qnamaker-service-azure.md) dağıtılan Azure App Service örneğinin bir parçasıdır. Güncelleştirmeler çalışma zamanında düzenli olarak yapılır. Soru-Cevap Oluşturma App Service örneği, Nisan 2019 site uzantısı sürümünden (sürüm 5 +) sonra otomatik güncelleştirme modunda. Bu güncelleştirme, yükseltmeler sırasında sıfır kesinti olması için tasarlanmıştır.

Geçerli sürümünüzü adresinde denetleyebilirsiniz https://www.qnamaker.ai/UserSettings . Sürümünüz 5. x sürümünden eskiyse, en son güncelleştirmeleri uygulamak için App Service yeniden başlatmanız gerekir:

1. [Azure Portal](https://portal.azure.com)QnAMaker hizmetinize (kaynak grubu) gidin.

    > [!div class="mx-imgBorder"]
    > ![QnAMaker Azure Kaynak grubu](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. App Service örneğini seçin ve **genel bakış** bölümünü açın.

    > [!div class="mx-imgBorder"]
    > ![QnAMaker App Service örneği](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)


1. App Service yeniden başlatın. Güncelleştirme işlemi birkaç saniye içinde bitmelidir. Bu QnAMaker hizmetini kullanan bağımlı uygulamalar veya botlar, bu yeniden başlatma döneminde son kullanıcılar için kullanılamaz.

    ![QnAMaker App Service örneğini yeniden başlatma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="cognitive-search-consideration"></a>Bilişsel Arama göz önünde bulundurun

Ayrı bir kaynak olarak Bilişsel Arama bilmeniz gereken bazı farklı yapılandırmalara sahiptir.

### <a name="configure-qna-maker-to-use-different-cognitive-search-resource"></a>Soru-Cevap Oluşturma farklı Bilişsel Arama kaynağı kullanacak şekilde yapılandırma

Portal üzerinden bir QnA hizmeti ve bağımlılıklarını (arama gibi) oluşturursanız, sizin için bir arama hizmeti oluşturulur ve Soru-Cevap Oluşturma hizmetine bağlanır. Bu kaynaklar oluşturulduktan sonra, önceden var olan bir arama hizmetini kullanmak için App Service ayarını güncelleştirebilir ve yeni oluşturduğunuz birini kaldırabilirsiniz.

Soru-Cevap Oluşturma **App Service** kaynağı bilişsel arama kaynağını kullanır. Soru-Cevap Oluşturma tarafından kullanılan Bilişsel Arama kaynağını değiştirmek için Azure portal ayarı değiştirmeniz gerekir.

1. Soru-Cevap Oluşturma kullanmak istediğiniz Bilişsel Arama kaynağın **yönetici anahtarını** ve **adını** alın.

1. [Azure Portal](https://portal.azure.com) oturum açın ve soru-cevap oluşturma kaynağınız ile ilişkili **App Service** bulun. Her ikisi de aynı ada sahip.

1. **Ayarlar**' ı ve ardından **yapılandırma**' yı seçin. Bu, Soru-Cevap Oluşturma App Service tüm mevcut ayarlarını görüntüler.

    > [!div class="mx-imgBorder"]
    > ![App Service yapılandırma ayarlarını gösteren Azure portal ekran görüntüsü](../media/qnamaker-how-to-upgrade-qnamaker/change-search-service-app-service-configuration.png)

1. Aşağıdaki anahtarlar için değerleri değiştirin:

    * **AzureSearchAdminKey**
    * **AzureSearchName**

1. Yeni ayarları kullanmak için App Service 'i yeniden başlatmanız gerekir. **Genel bakış**' ı ve ardından **Yeniden Başlat**' ı seçin

    > [!div class="mx-imgBorder"]
    > ![Yapılandırma ayarları değişikliğinden sonra App Service yeniden başlatma Azure portal ekran görüntüsü](../media/qnamaker-how-to-upgrade-qnamaker/screenshot-azure-portal-restart-app-service.png)

Azure Resource Manager şablonları aracılığıyla bir QnA hizmeti oluşturursanız, tüm kaynakları oluşturabilir ve App Service oluşturmayı, var olan bir arama hizmetini kullanacak şekilde denetleyebilirsiniz.

App Service [uygulama ayarlarını](../../../app-service/configure-common.md#configure-app-settings)yapılandırma hakkında daha fazla bilgi edinin.

### <a name="configuring-cognitive-search-as-a-private-endpoint-inside-a-vnet"></a>SANAL ağ içinde özel uç nokta olarak Bilişsel Arama yapılandırma

Bir Soru-Cevap Oluşturma kaynağı oluşturma sırasında bir arama örneği oluşturulduğunda, Bilişsel Arama bir müşterinin sanal ağı içinde oluşturulan özel bir uç nokta yapılandırmasını desteklemeye zorlayabilirsiniz.

Özel bir uç nokta kullanmak için tüm kaynakların aynı bölgede oluşturulması gerekir.

* Soru-Cevap Oluşturma kaynağı
* Yeni Bilişsel Arama kaynağı
* Yeni sanal ağ kaynağı

[Azure Portal](https://portal.azure.com)aşağıdaki adımları uygulayın:

1. [Soru-cevap oluşturma kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker)oluşturun.
1. Uç nokta bağlantısı (veri) ile _özel_olarak ayarlanmış yeni bir bilişsel arama kaynağı oluşturun. Kaynağı, 1. adımda oluşturulan Soru-Cevap Oluşturma kaynağıyla aynı bölgede oluşturun. [Bilişsel arama kaynağı oluşturma](../../../search/search-create-service-portal.md)hakkında daha fazla bilgi edinin ve bu bağlantıyı kullanarak doğrudan [kaynağın oluşturma sayfasına](https://ms.portal.azure.com/#create/Microsoft.Search)gidebilirsiniz.
1. Yeni bir [sanal ağ kaynağı](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)oluşturun.
1. Bu yordamın 1. adımında oluşturulan App Service kaynağında VNET 'i yapılandırın.
    1. 2. adımda oluşturulan yeni Bilişsel Arama kaynağı için VNET 'te yeni bir DNS girişi oluşturun. Bilişsel Arama IP adresine gidin.
1. App Service 'i adım 2 ' de oluşturulan [yeni bilişsel arama kaynağıyla ilişkilendirin](#configure-qna-maker-to-use-different-cognitive-search-resource) . Ardından, 1. adımda oluşturulan özgün Bilişsel Arama kaynağını silebilirsiniz.

[Soru-cevap oluşturma portalında](https://www.qnamaker.ai/)ilk bilgi tabanınızı oluşturun.


### <a name="inactivity-policy-for-free-search-resources"></a>Ücretsiz arama kaynakları için eylemsizlik ilkesi

Bir QnA Oluşturucu kaynağı kullanmıyorsanız, tüm kaynakları kaldırmalısınız. Kullanılmayan kaynakları kaldırmazsanız, ücretsiz bir arama kaynağı oluşturduysanız Bilgi Bankası çalışmanız çalışmayı durdurur.

Ücretsiz arama kaynakları, API çağrısı almadan 90 gün sonra silinir.

## <a name="configure-app-service-idle-setting-to-avoid-timeout"></a>Zaman aşımını önlemek için App Service boşta ayarını yapılandırın

Yayımlanmış bir bilgi tabanı için Soru-Cevap Oluşturma tahmin çalışma zamanına hizmet eden App Service, boşta kalma zaman aşımı yapılandırmasına sahiptir ve bu hizmet boşta kalırsa otomatik olarak zaman aşımına uğrar. Soru-Cevap Oluşturma için bu, tahmin çalışma zamanı generateAnswer API 'nizin zaman zaman trafik olmadan sonra zaman aşımına uğraymasına yol gösterir.

Tahmin uç noktası uygulamasının trafik olmadığında bile yüklenmesini sağlamak için boşta seçeneğini her zaman açık olarak ayarlayın.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Soru-Cevap Oluşturma kaynağınızın App Service 'i arayın ve seçin. Soru-Cevap Oluşturma kaynağıyla aynı ada sahip olur, ancak farklı **türde** App Service olacaktır.
1. **Ayarları** bulun ve **yapılandırma**' yı seçin.
1. Yapılandırma bölmesinde **Genel ayarlar**' ı seçin, **her zaman açık**' i bulun ve değer olarak **Açık** ' ı seçin.

    > [!div class="mx-imgBorder"]
    > ![Yapılandırma bölmesinde, * * Genel Ayarlar * * öğesini seçin, ardından * * Always on * * öğesini bulun ve değer olarak * * seçeneğini belirleyin.](../media/qnamaker-how-to-upgrade-qnamaker/configure-app-service-idle-timeout.png)

1. Yapılandırmayı kaydetmek için **Kaydet** ' i seçin.
1. Yeni ayarı kullanmak için uygulamayı yeniden başlatmak isteyip istemediğiniz sorulur. **Devam**’ı seçin.

App Service [genel ayarlarını](../../../app-service/configure-common.md#configure-general-settings)yapılandırma hakkında daha fazla bilgi edinin.

## <a name="configure-app-service-environment-to-host-qna-maker-app-service"></a>App Service Ortamı Soru-Cevap Oluşturma barındırmak için yapılandırın App Service
App Service Ortamı, Soru-Cevap Oluşturma App Service 'i barındırmak için kullanılabilir. App Service Ortamı iç ise aşağıdaki adımları izlemeniz gerekir:
1. Bir App Service ve bir Azure Search hizmeti oluşturun.
2. App Service 'i kullanıma sunun ve kullanılabilirlik Soru-Cevap Oluşturma izin ver:
    * Herkese açık-varsayılan
    * DNS hizmeti etiketi:
        * `CognitiveServicesManagement`
    * Soru-Cevap Oluşturma ilişkili IP 'Ler şunlardır:
        * 13.91.138.229
        * 40.88.22.25
        * 13.86.184.142
        * 20.185.105.28
        * 13.86.178.10
1. Azure Resource Manager kullanarak Soru-Cevap Oluşturma bilişsel hizmet örneği (Microsoft. Biliveservices/hesaplar) oluşturun; burada Soru-Cevap Oluşturma uç nokta App Service Ortamı olarak ayarlanmalıdır.

## <a name="business-continuity-with-traffic-manager"></a>Traffic Manager ile iş sürekliliği

İş sürekliliği planının birincil amacı dayanıklı bir bilgi tabanı uç noktası oluşturmaktır, bu, bot veya onu kullanan uygulama için zaman kaybı olmamasını sağlar.

> [!div class="mx-imgBorder"]
> ![Soru-Cevap Oluşturma bcp planı](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Yukarıda gösterilen üst düzey fikir aşağıdaki gibidir:

1. [Azure eşlenmiş bölgelerde](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)iki paralel [soru-cevap oluşturma hizmeti](set-up-qnamaker-service-azure.md) ayarlayın.

1. Birincil Soru-Cevap Oluşturma App Service 'i [yedekleyin](../../../app-service/manage-backup.md) ve ikincil kuruluma [geri yükleyin](../../../app-service/web-sites-restore.md) . Bu, her iki kurulum 'un aynı ana bilgisayar adı ve anahtarlarla çalışmasını sağlayacaktır.

1. Birincil ve ikincil Azure arama dizinlerini eşitlenmiş halde tutun. Azure dizinlerini yedekleme ve geri yükleme işlemlerinin nasıl yapılacağını görmek için [burada](https://github.com/pchoudhari/QnAMakerBackupRestore) GitHub örneğini kullanın.

1. Application Insights [sürekli dışarı aktarma](../../../application-insights/app-insights-export-telemetry.md)kullanarak yedekleyin.

1. Birincil ve ikincil yığınlar kurulduktan sonra, iki uç noktayı yapılandırmak ve bir yönlendirme yöntemi ayarlamak için [Traffic Manager](../../../traffic-manager/traffic-manager-overview.md) 'ı kullanın.

1. Traffic Manager uç noktanız için önceden Güvenli Yuva Katmanı (SSL) olarak bilinen bir Aktarım Katmanı Güvenliği (TLS) oluşturmanız gerekir. Uygulama hizmetlerinize [TLS/SSL sertifikası bağlayın](../../../app-service/configure-ssl-bindings.md) .

1. Son olarak, bot veya uygulamanızdaki Traffic Manager uç noktasını kullanın.

## <a name="delete-azure-resources"></a>Azure kaynaklarını silme

Soru-Cevap Oluşturma bilgi tabanlarınız için kullanılan Azure kaynaklarından herhangi birini silerseniz, Bilgi Bankası esaları artık çalışmaz. Herhangi bir kaynağı silmeden önce, bilgi temellerinizi **Ayarlar** sayfasından dışarı aktardığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama hizmeti](../../../app-service/index.yml) ve [Arama hizmeti](../../../search/index.yml)hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Başkalarıyla nasıl yazarla ilgili bilgi edinin](../how-to/collaborate-knowledge-base.md)
