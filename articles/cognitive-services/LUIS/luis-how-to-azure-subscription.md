---
title: Yazma ve çalışma zamanı tuşları nasıl kullanılır - LUIS
titleSuffix: Azure Cognitive Services
description: Dil Anlama (LUIS) 'yi ilk kullandığınızda, bir yazma anahtarı oluşturmanız gerekmez. Uygulamayı yayımlamak ve ardından çalışma zamanı bitiş noktanızı kullanmayı planladığınızda, uygulamanın çalışma saati anahtarını oluşturmanız ve atamanız gerekir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: diberry
ms.openlocfilehash: a5140da32eb6fce03131a42bfa90e71e64552431
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219978"
---
# <a name="using-authoring-and-runtime-resource-keys"></a>Yazma ve çalışma zamanı kaynak anahtarlarını kullanma

Yazma ve çalışma zamanı kaynakları, LUIS uygulamanıza ve tahmin bitiş noktanıza kimlik doğrulama sağlar.

<a name="create-luis-service"></a>
<a name="create-language-understanding-endpoint-key-in-the-azure-portal"></a>

LUIS portalında oturum açışı yaptığınızda, aşağıdakilerle devam etmeyi seçebilirsiniz:

* ücretsiz bir [deneme anahtarı](#trial-key) - yazma ve birkaç tahmin bitiş noktası sorguları sağlar.
* bir Azure [LUIS yazma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) kaynağı. 


<a name="starter-key"></a>

## <a name="sign-in-to-luis-portal-and-begin-authoring"></a>LUIS portalında oturum açın ve yazmaya başlayın

1. [LUIS portalında](https://www.luis.ai) oturum açın ve kullanım koşullarını kabul edin.
1. Hangi TÜR LUIS yazma anahtarı kullanmak istediğinizi seçerek LUIS uygulamanıza başlayın: ücretsiz deneme anahtarı veya yeni Azure LUIS yazma anahtarı. 

    ![Bir dil anlama türü yazma kaynağı seçin](./media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Kaynak seçim işleminiz bittiğinde, [yeni bir uygulama oluşturun.](luis-how-to-start-new-app.md#create-new-app-in-luis) 

## <a name="trial-key"></a>Deneme anahtarı

Deneme (başlangıç) anahtarı sizin için sağlanmıştır. Tahmin bitiş noktası çalışma saatini, ayda en fazla 1000 sorguyu sorgulamak için kimlik doğrulama anahtarınız olarak kullanılır. 

Hem **Kullanıcı Ayarları** sayfasında hem de LUIS portalındaki Azure kaynaklarını **yönet ->** sayfalarında görülebilir. 

Tahmin bitiş noktanızı yayımlamaya hazır olduğunuzda, başlangıç anahtarı işlevini değiştirmek için yazma ve tahmin çalışma zamanı anahtarları oluşturun ve atayın. 

## <a name="create-resources-in-the-azure-portal"></a>Azure portalında kaynak oluşturma

1. Kaynak oluşturma için Azure portalını açmak için [bu bağlantıyı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) kullanın.
1. Yazarlık ve tahmin bitiş noktası çalışma zamanı anahtarı oluşturmak için **Her ikisi'yi** seçin. 
1. Kaynağı oluşturmak için gereken bilgileri girin ve işlemi tamamlamak için **Oluştur'u** seçin.

    ![Dil anlama kaynağını oluşturma](./media/luis-how-to-azure-subscription/create-resource-in-azure.png)

    |Adı|Amaç|
    |--|--|
    |Kaynak adı| Yazma ve tahmin bitiş noktası sorgularınız için URL'nin bir parçası olarak kullanılan özel bir ad.|
    |Abonelik adı| kaynak için faturalandırılacak abonelik.|
    |Kaynak grubu| Seçtiğiniz veya oluşturduğunuz özel bir kaynak grubu adı. Kaynak grupları, aynı bölgede erişim ve yönetim için Azure kaynaklarını gruplandırmanıza olanak tanır.|
    |Yazma konumu|Modelinizle ilişkili bölge.|
    |Fiyatlandırma katmanı yazma|Fiyatlandırma katmanı saniye ve ay başına maksimum hareketi belirler.|
    |Çalışma zamanı konumu|Yayımlanmış tahmin bitiş noktası çalışma saatinizle ilişkili bölge.|
    |Runtime fiyatlandırma katmanı|Fiyatlandırma katmanı saniye ve ay başına maksimum hareketi belirler.|

    Her iki kaynak oluşturulduktan sonra, LUIS portalındaki kaynakları atayın.

## <a name="create-resources-in-azure-cli"></a>Azure CLI'de kaynak oluşturma

Her kaynağı ayrı ayrı oluşturmak için [Azure CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) kullanın. 

Kaynak `kind`:

* Yazma:`LUIS.Authoring`
* Tahmin:`LUIS` 

1. Azure CLI'de oturum açın:

    ```azurecli
    az login
    ```

    Bu, doğru hesabı seçmenize ve kimlik doğrulamasını sağlamanıza olanak tanıyan bir tarayıcı açar.

1. `westus` Bölge için `my-resource-group` adlandırılmış _varolan_ kaynak `my-luis-authoring-resource` grubunda adı geçen bir **LUIS yazma kaynağı**oluşturun. `LUIS.Authoring` 

    ```azurecli
    az cognitiveservices account create -n my-luis-authoring-resource -g my-resource-group --kind LUIS.Authoring --sku F0 -l westus --yes
    ```

1. `westus` Bölge için adlandırılmış `my-resource-group` _varolan_ `LUIS`kaynak `my-luis-prediction-resource` grubunda adı geçen bir **LUIS tahmini uç nokta kaynağı**oluşturun. Serbest katmandan daha yüksek bir iş `F0` elde `S0`etmek istiyorsanız, 'ye değiştirin. [Fiyatlandırma katmanları ve iş fazlası](luis-boundaries.md#key-limits)hakkında daha fazla bilgi edinin.

    ```azurecli
    az cognitiveservices account create -n my-luis-prediction-resource -g my-resource-group --kind LUIS --sku F0 -l westus --yes
    ```

    > [!Note] 
    > Bu anahtarlar, **Azure'u Yönet-> kaynaklarındaki**LUIS portalına atanana kadar LUIS portalı tarafından **kullanılmaz.**

## <a name="assign-an-authoring-resource-in-the-luis-portal-for-all-apps"></a>Tüm uygulamalar için LUIS portalında bir yazar lık kaynağı atama

Tek bir uygulama veya LUIS'teki tüm uygulamalar için bir yazma kaynağı atayabilirsiniz. Aşağıdaki yordam, tüm uygulamaları tek bir yazar kaynağa atar.

1. [LUIS portalında](https://www.luis.ai)oturum açın.
1. En üstteki gezinme çubuğunda, en sağda, kullanıcı hesabınızı seçin ve ardından **Ayarlar'ı**seçin.
1. Kullanıcı **Ayarları** sayfasında, **yazarlık kaynağı ekle'yi** seçin ve ardından varolan bir yazar kaynağı seçin. **Kaydet'i**seçin. 

## <a name="assign-a-resource-to-an-app"></a>Bir uygulamaya kaynak atama

Aşağıdaki yordamı içeren bir uygulamaya tek bir kaynak, yazma veya tahmin bitiş noktası çalışma zamanı atayabilirsiniz.

1. [LUIS portalında](https://www.luis.ai)oturum açın ve ardından **uygulamalar listemden** bir uygulama seçin.
1. **Azure kaynaklarını yönet ->** sayfasına gidin.

    ![Uygulamaya kaynak atamak için LUIS portalındaki > Azure kaynaklarını yönet'i seçin.](./media/luis-how-to-azure-subscription/manage-azure-resources-prediction.png)

1. Tahmin veya Yazma kaynak sekmesini seçin ve ardından **tahmin kaynağı ekle** veya kaynak **ekle** düğmesini seçin. 
1. Doğru kaynağı bulmak için formdaki alanları seçin ve ardından **Kaydet'i**seçin.  

### <a name="assign-runtime-resource-without-using-luis-portal"></a>LUIS portalKullanmadan çalışma zamanı kaynağı atama

CI/CD ardışık sistem adı gibi otomasyon amaçları için, LUIS çalışma zamanı kaynağının bir LUIS uygulamasına atamasını otomatikleştirmek isteyebilirsiniz. Bunu yapmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Bu [web sitesinden](https://resources.azure.com/api/token?plaintext=true)bir Azure Kaynak Yöneticisi belirteci alın. Bu belirteç, bu nedenle hemen kullanmak süresi yok. İstek bir Azure Kaynak Yöneticisi belirteci döndürür.

    ![Azure Kaynak Yöneticisi belirteci isteyin ve Azure Kaynak Yöneticisi belirteci alın](./media/luis-manage-keys/get-arm-token.png)

1. Kullanıcı hesabınızın erişediği [LUIS azure hesapları API'sinden](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c)abonelikler arasında LUIS çalışma zamanı kaynaklarını istemek için belirteci kullanın. 

    Bu POST API aşağıdaki ayarları gerektirir:

    |Üst bilgi|Değer|
    |--|--|
    |`Authorization`|`Authorization` Değeri. `Bearer {token}` Belirteç değerinin sözcük `Bearer` ve boşluköncesinde olması gerektiğine dikkat edin.| 
    |`Ocp-Apim-Subscription-Key`|Yazaranahtarın.|

    Bu API, hesap adı olarak döndürülen abonelik kimliği, kaynak grubu ve kaynak adı da dahil olmak üzere LUIS aboneliklerinizin bir dizi JSON nesnesini döndürür. Luis uygulamasına atamak için LUIS kaynağı olan dizideki tek öğeyi bulun. 

1. Bir uygulama API'sine LUIS [azure hesapları atayarak](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515) belirteci LUIS kaynağına atayın. 

    Bu POST API aşağıdaki ayarları gerektirir:

    |Tür|Ayar|Değer|
    |--|--|--|
    |Üst bilgi|`Authorization`|`Authorization` Değeri. `Bearer {token}` Belirteç değerinin sözcük `Bearer` ve boşluköncesinde olması gerektiğine dikkat edin.|
    |Üst bilgi|`Ocp-Apim-Subscription-Key`|Yazaranahtarın.|
    |Üst bilgi|`Content-type`|`application/json`|
    |Querystring|`appid`|LUIS app kimliği. 
    |Gövde||{"AzureSubscriptionId":"ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup-2",<br>"Hesap Adı": "luis-uswest-S0-2"}|

    Bu API başarılı olduğunda, 201 - oluşturulan durumu döndürür. 

## <a name="unassign-resource"></a>Kaynağı atama

1. [LUIS portalında](https://www.luis.ai)oturum açın ve ardından **uygulamalar listemden** bir uygulama seçin.
1. **Azure kaynaklarını yönet ->** sayfasına gidin.
1. Kaynak için Öngörü veya Yazma sekmesini seçin ve ardından kaynak için **Kaynak Atama** düğmesini seçin. 

Bir kaynağı atadığınızda, kaynak Azure'dan silinmez. Sadece LUIS'ten bağlantısı yok. 

## <a name="reset-authoring-key"></a>Yazma anahtarını sıfırlama

**Kaynak geçirilen uygulamaları [yazarken:](luis-migration-authoring.md) **yazar anahtarınız tehlikeye girdiyse, bu yazar kaynağın **Anahtarları** sayfasındaki Azure portalındaki anahtarı sıfırlayın. 

**Henüz geçiş yapmış uygulamalar için**anahtar, LUIS portalındaki tüm uygulamalarınızda sıfırlanır. Uygulamalarınızı yazma API'leri aracılığıyla yazarsanız, Ocp-Apim-Abonelik-Anahtar değerini yeni anahtarla değiştirmeniz gerekir.

## <a name="regenerate-azure-key"></a>Azure anahtarını yeniden oluşturma

Azure portalındaki Azure **anahtarlarını, Anahtarlar** sayfasında yeniden oluşturun.

## <a name="delete-account"></a>Hesabı silme

Hesabınızı sildiğinizde hangi verilerin silindiği hakkında bilgi için [Veri depolama ve kaldırma](luis-concept-data-storage.md#accounts) işlemine bakın.

## <a name="change-pricing-tier"></a>Fiyatlandırma katmanını değiştirme

1.  [Azure'da,](https://portal.azure.com)LUIS aboneliğinizi bulun. LUIS aboneliğini seçin.
    ![LUIS aboneliğinizi bulun](./media/luis-usage-tiers/find.png)
1.  Kullanılabilir fiyatlandırma katmanlarını görmek için **Fiyatlandırma katmanını** seçin. 
    ![Fiyatlandırma katmanlarını görüntüleme](./media/luis-usage-tiers/subscription.png)
1.  Fiyatlandırma katmanını seçin ve değişikliğinizi kaydetmek için **Seç'i** seçin. 
    ![LUIS ödeme seviyenizi değiştirme](./media/luis-usage-tiers/plans.png)
1.  Fiyatlandırma değişikliği tamamlandığında, açılır pencere yeni fiyatlandırma katmanını doğrular. 
    ![LUIS ödeme seviyenizi doğrulayın](./media/luis-usage-tiers/updated.png)
1. Bu [bitiş noktası anahtarını](#assign-a-resource-to-an-app) **Yayımla** sayfasında atamayı ve tüm uç nokta sorgularında kullanmayı unutmayın. 

## <a name="viewing-azure-resource-metrics"></a>Azure kaynak ölçümlerini görüntüleme

### <a name="viewing-azure-resource-summary-usage"></a>Azure kaynak özeti kullanımını görüntüleme
LUIS kullanım bilgilerini Azure'da görüntüleyebilirsiniz. **Genel Bakış** sayfası aramalar ve hatalar da dahil olmak üzere en son özet bilgileri gösterir. Bir LUIS bitiş noktası isteği nde bulunursanız, hemen **Genel Bakış sayfasını**izleyin, kullanım için beş dakikaya kadar bekleyin.

![Özet kullanımını görüntüleme](./media/luis-usage-tiers/overview.png)

### <a name="customizing-azure-resource-usage-charts"></a>Azure kaynak kullanım grafiklerini özelleştirme
Ölçümler verilere daha ayrıntılı bir görünüm sağlar.

![Varsayılan ölçümler](./media/luis-usage-tiers/metrics-default.png)

Ölçüm grafiklerinizi dönem ve metrik türüne göre yapılandırabilirsiniz. 

![Özel ölçümler](./media/luis-usage-tiers/metrics-custom.png)

### <a name="total-transactions-threshold-alert"></a>Toplam hareketler eşik uyarısı
Belirli bir işlem eşiğine (örneğin 10.000 işlem) ne zaman ulaştığınızı öğrenmek istiyorsanız, bir uyarı oluşturabilirsiniz. 

![Varsayılan uyarılar](./media/luis-usage-tiers/alert-default.png)

Belirli bir süre için **toplam arama** ölçümü için metrik uyarı ekleyin. Uyarıyı alması gereken tüm kişilerin e-posta adreslerini ekleyin. Uyarı alması gereken tüm sistemler için webhooks ekleyin. Uyarı tetiklendiğinde bir mantık uygulaması da çalıştırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* Uygulama yaşam döngünüzün denetimini sağlamak için [sürümleri nasıl kullanacağınızı](luis-how-to-manage-versions.md) öğrenin.
* [Yazarlık kaynağı](luis-concept-keys.md#authoring-key) ve bu kaynaktaki [katkıda bulunanlar](luis-concept-keys.md#contributions-from-other-authors) gibi kavramları anlayın.
* Yazarlık ve çalışma zamanı kaynaklarını [nasıl oluşturup oluşturabilirsiniz](luis-how-to-azure-subscription.md) öğrenin
* Yeni [yazma kaynağına](luis-migration-authoring.md) geçirin 
