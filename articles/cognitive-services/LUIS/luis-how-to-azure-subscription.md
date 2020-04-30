---
title: Yazma ve çalışma zamanı anahtarlarını kullanma-LUSıS
description: Language Understanding (LUU) ilk kez kullandığınızda, yazma anahtarı oluşturmanız gerekmez. Uygulamayı yayımlamayı ve sonra çalışma zamanı uç noktanızı kullanmayı amaçlıyorsanız, çalışma zamanı anahtarını uygulamaya oluşturup atamanız gerekir.
services: cognitive-services
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: d9235b6ef1c7cddbfbbd36f8382439d781af6d5f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82101034"
---
# <a name="create-luis-resources"></a>LUSıS kaynakları oluşturma

Yazma ve çalışma zamanı kaynakları, LUSıS uygulamanız ve tahmin uç noktanıza kimlik doğrulaması sağlar.

<a name="create-luis-service"></a>
<a name="create-language-understanding-endpoint-key-in-the-azure-portal"></a>

LUU portalında oturum açtığınızda, devam etmeyi seçebilirsiniz:

* ücretsiz [deneme anahtarı](#trial-key) ; yazma ve birkaç tahmin uç nokta sorgusu sağlar.
* Azure [lusıs yazma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) kaynağı.

<a name="starter-key"></a>

## <a name="sign-in-to-luis-portal-and-begin-authoring"></a>LUSıS portalında oturum açın ve yazmaya başlayın

1. [Luo portalında](https://www.luis.ai) oturum açın ve kullanım koşullarını kabul edin.
1. Hangi tür bir LUSıS yazma anahtarını kullanmak istediğinizi seçerek LUSıS uygulamanızı başlatın: ücretsiz deneme anahtarı veya yeni Azure LUSıS yazma anahtarı.

    ![Language Understanding yazma kaynağı türünü seçin](./media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Kaynak seçim sürecinizle işiniz bittiğinde [Yeni bir uygulama oluşturun](luis-how-to-start-new-app.md#create-new-app-in-luis).

## <a name="trial-key"></a>Deneme anahtarı

Deneme (başlangıç) anahtarı sizin için verilmiştir. Tahmin uç noktası çalışma zamanını sorgulamak için, bir ayda 1000 'e kadar sorgu için kimlik doğrulama anahtarınız olarak kullanılır.

Bu, hem **Kullanıcı ayarları** sayfasında hem de Luo portalındaki **Manage-> Azure Resources** sayfalarında görülebilir.

Tahmin uç noktanızı yayımlamaya hazır olduğunuzda, başlangıç anahtar işlevini değiştirmek için yazma ve tahmin çalışma zamanı anahtarlarını [oluşturun](#create-luis-resources) ve [atayın](#assign-a-resource-to-an-app) .

<a name="create-resources-in-the-azure-portal"></a>


[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

## <a name="create-resources-in-azure-cli"></a>Azure CLı 'de kaynak oluşturma

Her kaynağı ayrı ayrı oluşturmak için [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 'yi kullanın.

Kaynak `kind`:

* Özgün`LUIS.Authoring`
* Hızlı`LUIS`

1. Azure CLı 'da oturum açın:

    ```azurecli
    az login
    ```

    Bu, doğru hesabı seçmenizi ve kimlik doğrulaması sağlamanıza olanak tanıyan bir tarayıcı açar.

1. `westus` Bölge için adlı `my-resource-group` _mevcut_ kaynak grubunda adlı `my-luis-authoring-resource` bir **lusıs yazma kaynağı** `LUIS.Authoring`oluşturun.

    ```azurecli
    az cognitiveservices account create -n my-luis-authoring-resource -g my-resource-group --kind LUIS.Authoring --sku F0 -l westus --yes
    ```

1. `westus` Bölge için adlı `my-resource-group` _mevcut_ kaynak grubunda adlı `LUIS` `my-luis-prediction-resource` , türü bir **lusıs tahmin uç noktası kaynağı**oluşturun. Ücretsiz katmandan daha yüksek bir aktarım hızı istiyorsanız olarak `F0` `S0`değiştirin. [Fiyatlandırma katmanları ve verimlilik](luis-limits.md#key-limits)hakkında daha fazla bilgi edinin.

    ```azurecli
    az cognitiveservices account create -n my-luis-prediction-resource -g my-resource-group --kind LUIS --sku F0 -l westus --yes
    ```

    > [!Note]
    > Bu anahtarlar, **Manage-> Azure KAYNAKLARıNDA**Luo portalında atanana kadar Luo portalı **tarafından kullanılmaz.**

## <a name="assign-an-authoring-resource-in-the-luis-portal-for-all-apps"></a>Tüm uygulamalar için LUO portalında bir yazma kaynağı atama

Tek bir uygulama için veya LUSıS 'deki tüm uygulamalar için bir yazma kaynağı atayabilirsiniz. Aşağıdaki yordam, tüm uygulamaları tek bir yazma kaynağına atar.

1. [Lui portalında](https://www.luis.ai)oturum açın.
1. Üst gezinti çubuğunda, en sağdaki kullanıcı hesabınızı seçin ve ardından **Ayarlar**' ı seçin.
1. **Kullanıcı ayarları** sayfasında, **yazma kaynağı Ekle** ' yi seçin ve var olan bir yazma kaynağı seçin. **Kaydet**’i seçin.

## <a name="assign-a-resource-to-an-app"></a>Uygulamaya kaynak atama

Aşağıdaki yordamı kullanarak bir uygulamaya tek bir kaynak, yazma veya tahmin uç noktası çalışma zamanı atayabilirsiniz.

1. [Luo portalında](https://www.luis.ai)oturum açın, sonra **uygulamalarım** listesinden bir uygulama seçin.
1. **Yönet-> Azure kaynakları** sayfasına gidin.

    ![Uygulamaya bir kaynak atamak için LUU portalında Manage-> Azure kaynaklarını seçin.](./media/luis-how-to-azure-subscription/manage-azure-resources-prediction.png)

1. Tahmin veya yazma kaynağı sekmesini seçin, ardından **tahmin kaynağı Ekle** veya **yazma kaynağı Ekle** düğmesini seçin.
1. Doğru kaynağı bulmak için formdaki alanları seçin ve ardından **Kaydet**' i seçin.

### <a name="assign-runtime-resource-without-using-luis-portal"></a>LUO portalını kullanmadan çalışma zamanı kaynağı atama

CI/CD işlem hattı gibi Otomasyon amaçları için bir lusıs çalışma zamanı kaynağının bir LUU uygulamasına atanmasını otomatikleştirmek isteyebilirsiniz. Bunu yapmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Bu [Web sitesinden](https://resources.azure.com/api/token?plaintext=true)bir Azure Resource Manager belirteci alın. Bu belirtecin kullanım süresini hemen kullanın. İstek bir Azure Resource Manager belirteci döndürüyor.

    ![Azure Resource Manager belirteci iste ve Azure Resource Manager belirteci al](./media/luis-manage-keys/get-arm-token.png)

1. Kullanıcı hesabınızın erişimi olan [Get lusıs Azure hesapları API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c)'sinden, halsıs çalışma zamanı kaynaklarını abonelikler arasında istemek için belirteci kullanın.

    Bu POST API 'SI aşağıdaki ayarları gerektirir:

    |Üst bilgi|Değer|
    |--|--|
    |`Authorization`|Öğesinin `Authorization` değeri `Bearer {token}`. Belirteç değerinin önünde `Bearer` ve bir boşluk olması gerektiğini unutmayın.|
    |`Ocp-Apim-Subscription-Key`|Yazma anahtarınız.|

    Bu API, abonelik KIMLIĞI, kaynak grubu ve kaynak adı gibi, hesap adı olarak döndürülen LUSıS aboneliklerinizin JSON nesnelerinin bir dizisini döndürür. LUO uygulamasına atanacak LUO kaynağı olan dizideki bir öğeyi bulun.

1. [Bir uygulama API 'sine BIR lusıs Azure hesabı atama](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515) ile, belirteci halsıs kaynağına atayın.

    Bu POST API 'SI aşağıdaki ayarları gerektirir:

    |Tür|Ayar|Değer|
    |--|--|--|
    |Üst bilgi|`Authorization`|Öğesinin `Authorization` değeri `Bearer {token}`. Belirteç değerinin önünde `Bearer` ve bir boşluk olması gerektiğini unutmayın.|
    |Üst bilgi|`Ocp-Apim-Subscription-Key`|Yazma anahtarınız.|
    |Üst bilgi|`Content-type`|`application/json`|
    |QueryString|`appid`|LUIS app kimliği.
    |Gövde||{"Azuyeniden gönderme Scriptionıd": "ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup-2",<br>"AccountName": "Lua-usbatı-S0-2"}|

    Bu API başarılı olduğunda, 201 tarafından oluşturulan bir durum döndürür.

## <a name="unassign-resource"></a>Kaynak atamasını Kaldır

1. [Luo portalında](https://www.luis.ai)oturum açın, sonra **uygulamalarım** listesinden bir uygulama seçin.
1. **Yönet-> Azure kaynakları** sayfasına gidin.
1. Tahmin veya yazma kaynağı sekmesini seçin ve kaynağın **atamasını Kaldır** düğmesini seçin.

Bir kaynağın atamasını kaldırdığınızda, Azure 'dan silinmez. Yalnızca LUSıS bağlantısı kaldırılır.

## <a name="reset-authoring-key"></a>Yazma anahtarını Sıfırla

**[Kaynak geçişi](luis-migration-authoring.md) yapılan uygulamalar yazmak için**: yazma anahtarınız tehlikeye atılırsa, bu yazma kaynağının **anahtarlar** sayfasındaki Azure Portal anahtarı sıfırlayın.

**Henüz geçirilmeyen uygulamalar için**: anahtar, lusıs portalındaki tüm uygulamalarınızda sıfırlanır. Uygulamalarınızı yazma API 'Leri aracılığıyla yazardıysanız, OCP-apim-Subscription-Key değerini yeni anahtar olarak değiştirmeniz gerekir.

## <a name="regenerate-azure-key"></a>Azure anahtarını yeniden üret

**Anahtarlar** sayfasında Azure Portal Azure anahtarlarını yeniden oluşturun.

## <a name="delete-account"></a>Hesabı silme

Hesabınızı sildiğinizde hangi verilerin silindiği hakkında bilgi için bkz. [veri depolama ve kaldırma](luis-concept-data-storage.md#accounts) .

## <a name="change-pricing-tier"></a>Fiyatlandırma katmanını değiştirme

1.  [Azure](https://portal.azure.com)'DA, lusıs aboneliğinizi bulun. LUSıS aboneliğini seçin.
    ![LUSıS aboneliğinizi bulun](./media/luis-usage-tiers/find.png)
1.  Kullanılabilir fiyatlandırma katmanlarını görmek için **fiyatlandırma katmanını** seçin.
    ![Fiyatlandırma katmanlarını görüntüleme](./media/luis-usage-tiers/subscription.png)
1.  Fiyatlandırma katmanını seçin ve Değişiklerinizi kaydetmek için **Seç** ' i seçin.
    ![LUSıS ödeme katmanınızı değiştirin](./media/luis-usage-tiers/plans.png)
1.  Fiyatlandırma değişikliği tamamlandığında, bir açılır pencere yeni fiyatlandırma katmanını doğrular.
    ![LUSıS ödeme katmanınızı doğrulayın](./media/luis-usage-tiers/updated.png)
1. [Bu uç nokta anahtarını](#assign-a-resource-to-an-app) **Yayımla** sayfasında atamayı ve tüm uç nokta sorgularında kullanmayı unutmayın.

## <a name="viewing-azure-resource-metrics"></a>Azure Kaynak ölçümlerini görüntüleme

### <a name="viewing-azure-resource-summary-usage"></a>Azure Kaynak Özeti kullanımını görüntüleme
Azure 'da LUSıS kullanım bilgilerini görüntüleyebilirsiniz. **Genel bakış** sayfası, çağrılar ve hatalar dahil olmak üzere son özet bilgileri gösterir. Bir LUSıS uç noktası isteği yaparsanız, **genel bakış sayfasını**hemen izleyin ve kullanım için en fazla beş dakika bekleyin.

![Özet kullanımı görüntüleme](./media/luis-usage-tiers/overview.png)

### <a name="customizing-azure-resource-usage-charts"></a>Azure Kaynak kullanımı grafiklerini özelleştirme
Ölçümler, verilere daha ayrıntılı bir görünüm sağlar.

![Varsayılan ölçümler](./media/luis-usage-tiers/metrics-default.png)

Ölçüm grafiklerinizi zaman dönemi ve ölçüm türü için yapılandırabilirsiniz.

![Özel ölçümler](./media/luis-usage-tiers/metrics-custom.png)

### <a name="total-transactions-threshold-alert"></a>Toplam işlem eşiği uyarısı
Belirli bir işlem eşiğine ne zaman ulaşılırsa emin olmak istiyorsanız, örneğin 10.000 işlem için bir uyarı oluşturabilirsiniz.

![Varsayılan uyarılar](./media/luis-usage-tiers/alert-default.png)

Belirli bir döneme ait **Toplam çağrı** ölçümü için bir ölçüm uyarısı ekleyin. Uyarı alması gereken tüm kişilerin e-posta adreslerini ekleyin. Uyarı alması gereken tüm sistemler için Web kancaları ekleyin. Ayrıca, uyarı tetiklendiğinde bir Logic App de çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Sürümlerin uygulama yaşam döngüsünü denetlemek için [nasıl](luis-how-to-manage-versions.md) kullanılacağını öğrenin.
* Kaynak üzerinde [yazma kaynağı](luis-concept-keys.md#authoring-key) ve [katkıda bulunanlar](luis-concept-keys.md#contributions-from-other-authors) dahil olmak üzere kavramları anlayın.
* Yazma ve çalışma zamanı kaynakları [oluşturmayı](luis-how-to-azure-subscription.md) öğrenin
* Yeni [yazma kaynağına](luis-migration-authoring.md) geçir
