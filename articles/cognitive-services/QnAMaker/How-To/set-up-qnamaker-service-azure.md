---
title: Soru-Cevap Oluşturma Hizmeti ayarlama-Soru-Cevap Oluşturma
description: Herhangi bir Soru-Cevap Oluşturma bilgi tabanı oluşturabilmeniz için önce Azure 'da bir Soru-Cevap Oluşturma Hizmeti ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturmak için yetkilendirmeye sahip olan herkes, Soru-Cevap Oluşturma bir hizmet ayarlayabilir.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: b6ab131c0fa81609b956de53f2b15d445e8979dd
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102219276"
---
# <a name="manage-qna-maker-resources"></a>Soru-Cevap Oluşturma kaynaklarını yönetme

Herhangi bir Soru-Cevap Oluşturma bilgi tabanı oluşturabilmeniz için önce Azure 'da bir Soru-Cevap Oluşturma Hizmeti ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturmak için yetkilendirmeye sahip olan herkes, Soru-Cevap Oluşturma bir hizmet ayarlayabilir.

Kaynağı oluşturmadan önce aşağıdaki kavramların bir katı şekilde anlaşılması yararlı olur:

* [Soru-Cevap Oluşturma kaynakları](../Concepts/azure-resources.md)
* [Yazma ve yayımlama anahtarları](../Concepts/azure-resources.md#keys-in-qna-maker)

## <a name="create-a-new-qna-maker-service"></a>Yeni bir Soru-Cevap Oluşturma hizmeti oluşturun

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

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
    * **Kaynak grubu konumu** seçin.
    * Azure Bilişsel Arama hizmetinin **Ara fiyatlandırma katmanını** seçin. Ücretsiz katman seçeneği kullanılamıyorsa (soluk görünürse), aboneliğiniz aracılığıyla dağıtılan ücretsiz bir hizmetiniz zaten var demektir. Bu durumda, temel katmanla başlamanız gerekir. Bkz. [Azure bilişsel arama fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/).
    * Azure Bilişsel Arama dizinlerinin dağıtılmasını istediğiniz **Arama konumunu** seçin. Müşteri verilerinin depolanması gereken kısıtlamalar, Azure Bilişsel Arama için seçtiğiniz konumu belirlemenize yardımcı olur.
    * **Uygulama adı** alanına Azure App Service örneğiniz için bir ad girin.
    * Varsayılan olarak, varsayılan olarak standart (S1) katmanına App Service. Oluşturulduktan sonra planı değiştirebilirsiniz. [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/)hakkında daha fazla bilgi edinin.
    * App Service dağıtılacağı **Web sitesi konumunu** seçin.

        > [!NOTE]
        > **Arama konumu** , **Web sitesi konumundan** farklı olabilir.

    * **Application Insights** etkinleştirmek isteyip istemediğinizi seçin. **Application Insights** etkinleştirilirse, soru-cevap oluşturma trafik, sohbet günlükleri ve hatalar üzerinde telemetri toplar.
    * Application Insights kaynağın dağıtılacağı **App Insights konumunu** seçin.
    * Maliyet tasarrufu ölçüleri için, Soru-Cevap Oluşturma için oluşturulan tüm Azure kaynaklarını [paylaşabilirsiniz](configure-QnA-Maker-resources.md#configure-qna-maker-to-use-different-cognitive-search-resource) .

1. Tüm alanlar doğrulandıktan sonra **Oluştur**' u seçin. İşlemin tamamlanması birkaç dakika sürebilir.

1. Dağıtım tamamlandıktan sonra, aboneliğinizde aşağıdaki kaynakların oluşturulduğunu görürsünüz:

   ![Kaynak yeni bir Soru-Cevap Oluşturma Hizmeti oluşturdu](../media/qnamaker-how-to-setup-service/resources-created.png)

    Bilişsel _Hizmetler_ türündeki kaynağın _abonelik_ anahtarları vardır.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Bu yordam, Bilgi Bankası içeriğini yönetmek için gereken Azure kaynaklarını oluşturur. Bu adımları tamamladıktan sonra, Azure portal kaynak için **anahtarlar** sayfasında *abonelik* anahtarlarını bulabilirsiniz.

1. Azure portal oturum açın ve [bir soru-cevap oluşturma kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) .

1. Hüküm ve koşulları okuduktan sonra **Oluştur** ' u seçin:

    ![Yeni bir Soru-Cevap Oluşturma hizmeti oluşturun](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. **Soru-cevap oluşturma**, yönetilen (Önizleme) onay kutusunu işaretleyin ve uygun katmanları ve bölgeleri seçin:

    ![Yeni Soru-Cevap Oluşturma yönetilen hizmet oluşturma-fiyatlandırma katmanı ve bölgeler](../media/qnamaker-how-to-setup-service/enter-qnamaker-v2-info.png)

    * Soru-Cevap Oluşturma kaynağın dağıtılacağı **aboneliği** seçin.
    * Yeni bir **kaynak grubu** oluşturun (önerilir) veya bu soru-cevap oluşturma yönetilen (Önizleme) kaynağını dağıtmak için mevcut bir tane kullanın. Soru-Cevap Oluşturma yönetilen (Önizleme) birkaç Azure kaynağı oluşturur. Bu kaynakları barındıracak bir kaynak grubu oluşturduğunuzda, bu kaynakları kaynak grubu adına göre kolayca bulabilir, yönetebilir ve silebilirsiniz.
    * **Ad** alanına, bu soru-cevap oluşturma yönetilen (Önizleme) hizmeti tanımlamak için benzersiz bir ad girin. 
    * Soru-Cevap Oluşturma yönetilen (Önizleme) hizmetinin dağıtılmasını istediğiniz **konumu** seçin. Yönetim API 'Leri ve hizmet uç noktası bu konumda barınacaktır. 
    * Soru-Cevap Oluşturma yönetilen (Önizleme) hizmeti için **fiyatlandırma katmanını** seçin (Önizleme için ücretsiz). [SKU fiyatlandırması hakkında daha fazla ayrıntı](https://aka.ms/qnamaker-pricing)için bkz..
    * Azure Bilişsel Arama dizinlerinin dağıtılmasını istediğiniz **Arama konumunu** seçin. Müşteri verilerinin depolanması gereken kısıtlamalar, Azure Bilişsel Arama için seçtiğiniz konumu belirlemenize yardımcı olur.
    * Azure Bilişsel Arama hizmetinin **Ara fiyatlandırma katmanını** seçin. Ücretsiz katman seçeneği kullanılamıyorsa (soluk görünürse), aboneliğiniz aracılığıyla dağıtılan ücretsiz bir hizmetiniz zaten var demektir. Bu durumda, temel katmanla başlamanız gerekir. Bkz. [Azure bilişsel arama fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/).

1. Tüm alanlar doğrulandıktan sonra, **gözden geçir + oluştur**' u seçin. İşlemin tamamlanması birkaç dakika sürebilir.

1. Dağıtım tamamlandıktan sonra, aboneliğinizde aşağıdaki kaynakların oluşturulduğunu görürsünüz:

    ![Kaynak yeni bir Soru-Cevap Oluşturma yönetilen (Önizleme) hizmeti oluşturdu](../media/qnamaker-how-to-setup-service/resources-created-v2.png)

    Bilişsel _Hizmetler_ türündeki kaynağın _abonelik_ anahtarları vardır.

---

## <a name="upgrade-azure-resources"></a>Azure kaynaklarını yükseltme

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

### <a name="upgrade-qna-maker-sku"></a>Soru-Cevap Oluşturma SKU 'YU yükselt

Bilgi tabanınızda, geçerli katmanınızın ötesinde daha fazla soru ve yanıt almak istediğinizde Soru-Cevap Oluşturma Hizmeti fiyatlandırma katmanınızı yükseltin.

Soru-Cevap Oluşturma yönetim SKU 'sunu yükseltmek için:

1. Azure portal Soru-Cevap Oluşturma kaynağına gidin ve **fiyatlandırma katmanı**' nı seçin.

    ![Soru-Cevap Oluşturma kaynağı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. Uygun SKU 'yu seçin ve **Seç**' e basın.

    ![Soru-Cevap Oluşturma fiyatlandırması](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)
    
### <a name="upgrade-app-service"></a>App Service yükselt

Bilgi tabanınızın istemci uygulamanızdan daha fazla istek sunması gerektiğinde App Service fiyatlandırma katmanınızı yükseltin.

App Service [ölçeklendirebilir](../../../app-service/manage-scale-up.md) veya ölçeklendirebilirsiniz.

Azure portal App Service kaynağına gidin ve gereken kadar **ölçeği yukarı** veya **genişletme** seçeneğini belirleyin.

![Soru-Cevap Oluşturma App Service ölçeği](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Azure Bilişsel Arama hizmetini yükseltme

Birden fazla bilgi bankanız olmasını planlıyorsanız Azure Bilişsel Arama hizmeti fiyatlandırma katmanınızı yükseltin.

Şu anda Azure Search SKU 'sunun yerinde yükseltmesini gerçekleştiremezsiniz. Ancak, istenen SKU ile yeni bir Azure Search kaynağı oluşturabilir, verileri yeni kaynağa geri yükleyebilir ve sonra Soru-Cevap Oluşturma yığınına bağlayabilirsiniz. Bunu yapmak için şu adımları uygulayın:

1. Azure portal yeni bir Azure Search kaynağı oluşturun ve istediğiniz SKU 'YU seçin.

    ![Azure Search kaynağı Soru-Cevap Oluşturma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Dizinleri özgün Azure Search kaynağından yeni bir kaynağa geri yükleyin. [Yedekleme geri yükleme örnek kodu](https://github.com/pchoudhari/QnAMakerBackupRestore) konusuna bakın.

1. Veriler geri yüklendikten sonra, yeni Azure Arama kaynağınız ' ne gidin, **anahtarlar**' ı seçin ve **adı** ve **yönetici anahtarını** yazın:

    ![Azure Arama anahtarları Soru-Cevap Oluşturma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

1. Yeni Azure Search kaynağını Soru-Cevap Oluşturma yığınına bağlamak için, Soru-Cevap Oluşturma App Service örneğine gidin.

    ![Soru-Cevap Oluşturma App Service örneği](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

1. **Uygulama ayarları**'nı seçin, ardından 3. adımdaki **AzureSearchName** ve **AzureSearchAdminKey** alanlarındaki ayarları değiştirin.

    ![Soru-Cevap Oluşturma App Service ayarı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

1. App Service örneğini yeniden başlatın.

    ![Soru-Cevap Oluşturma App Service örneğinin yeniden başlatılması](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)
    
### <a name="inactivity-policy-for-free-search-resources"></a>Ücretsiz arama kaynakları için eylemsizlik ilkesi

Bir QnA Oluşturucu kaynağı kullanmıyorsanız, tüm kaynakları kaldırmalısınız. Kullanılmayan kaynakları kaldırmazsanız, ücretsiz bir arama kaynağı oluşturduysanız Bilgi Bankası çalışmanız çalışmayı durdurur.

Ücretsiz arama kaynakları, API çağrısı almadan 90 gün sonra silinir.
    
# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Azure Bilişsel Arama hizmetini yükseltme

Birden fazla bilgi bankanız olmasını planlıyorsanız Azure Bilişsel Arama hizmeti fiyatlandırma katmanınızı yükseltin.

Şu anda Azure Search SKU 'sunun yerinde yükseltmesini gerçekleştiremezsiniz. Ancak, istenen SKU ile yeni bir Azure Search kaynağı oluşturabilir, verileri yeni kaynağa geri yükleyebilir ve sonra Soru-Cevap Oluşturma yığınına bağlayabilirsiniz. Bunu yapmak için şu adımları uygulayın:

1. Azure portal yeni bir Azure Search kaynağı oluşturun ve istediğiniz SKU 'YU seçin.

    ![Azure Search kaynağı Soru-Cevap Oluşturma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Dizinleri özgün Azure Search kaynağından yeni bir kaynağa geri yükleyin. [Yedekleme geri yükleme örnek kodu](https://github.com/pchoudhari/QnAMakerBackupRestore) konusuna bakın.

1. Yeni Azure Search kaynağını Soru-Cevap Oluşturma yönetilen (Önizleme) hizmetine bağlamak için aşağıdaki konuya bakın.

### <a name="inactivity-policy-for-free-search-resources"></a>Ücretsiz arama kaynakları için eylemsizlik ilkesi

Bir QnA Oluşturucu kaynağı kullanmıyorsanız, tüm kaynakları kaldırmalısınız. Kullanılmayan kaynakları kaldırmazsanız, ücretsiz bir arama kaynağı oluşturduysanız Bilgi Bankası çalışmanız çalışmayı durdurur.

Ücretsiz arama kaynakları, API çağrısı almadan 90 gün sonra silinir.

---

## <a name="delete-azure-resources"></a>Azure kaynaklarını silme

Soru-Cevap Oluşturma bilgi tabanlarınız için kullanılan Azure kaynaklarından herhangi birini silerseniz, Bilgi Bankası esaları artık çalışmaz. Herhangi bir kaynağı silmeden önce, bilgi temellerinizi **Ayarlar** sayfasından dışarı aktardığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama hizmeti](../../../app-service/index.yml) ve [Arama hizmeti](../../../search/index.yml)hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Başkalarıyla nasıl yazarla ilgili bilgi edinin](../index.yml)
