---
title: Yazma ve çalışma zamanı anahtarlarını kullanma-LUSıS
description: Language Understanding (LUU) ilk kez kullandığınızda, yazma anahtarı oluşturmanız gerekmez. Uygulamayı yayımlamayı ve sonra çalışma zamanı uç noktanızı kullanmayı amaçlıyorsanız, çalışma zamanı anahtarını uygulamaya oluşturup atamanız gerekir.
services: cognitive-services
ms.topic: how-to
ms.date: 06/26/2020
ms.openlocfilehash: 5f6d62a63ea5ae0d3e4ca5913d6e7834ba07692a
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85560437"
---
# <a name="create-luis-resources"></a>LUSıS kaynakları oluşturma

Yazma ve sorgu tahmini çalışma zamanı kaynakları, LUSıS uygulamanız ve tahmin uç noktanıza kimlik doğrulaması sağlar.

<a name="programmatic-key" ></a>
<a name="endpoint-key"></a>
<a name="authoring-key"></a>

## <a name="luis-resources"></a>LUSıS kaynakları

LUSıS üç tür Azure kaynağına ve Azure olmayan bir kaynağa izin verir:

|Anahtar|Amaç|Bilişsel hizmet`kind`|Bilişsel hizmet`type`|
|--|--|--|--|
|Yazma anahtarı|Yazma, eğitim, yayımlama ve test etme ile uygulama verilerine erişin ve bunları yönetin. LUSıS uygulamalarını programlı olarak yazmak istiyorsanız bir LUSıS yazma anahtarı oluşturun.<br><br>Anahtarın amacı şunları yapmanıza `LUIS.Authoring` izin versağlamaktır:<br>* Eğitim ve yayımlama dahil Language Understanding uygulamaları ve modelleri program aracılığıyla yönetme<br> * [katkıda bulunan rolüne](#contributions-from-other-authors)kişi atayarak yazma kaynağına yönelik izinleri denetleyin.|`LUIS.Authoring`|`Cognitive Services`|
|Sorgu tahmin anahtarı| Sorgu tahmini uç noktası istekleri. İstemci uygulamanızın, başlatıcı kaynak tarafından belirtilen 1.000 isteklerin ötesinde tahmin etmek için bir LUSıS tahmin anahtarı oluşturun. |`LUIS`|`Cognitive Services`|
|[Bilişsel hizmet hizmeti çoklu hizmet kaynak anahtarı](../cognitive-services-apis-create-account-cli.md?tabs=windows#create-a-cognitive-services-resource)|LUVE diğer desteklenen bilişsel hizmetler ile paylaşılan sorgu tahmini uç noktası istekleri.|`CognitiveServices`|`Cognitive Services`|
|Başlangıç|LUıS portalı veya API 'Ler (SDK 'lar dahil) aracılığıyla ücretsiz yazma (rol tabanlı erişim denetimi olmadan), bir tarayıcı, API veya SDK aracılığıyla ayda ücretsiz 1.000 tahmin uç noktası istekleri|-|Azure kaynağı değil|

Azure kaynak oluşturma işlemi tamamlandığında, anahtarı HALU portalında uygulamaya [atayın](#assign-a-resource-to-an-app) .

LUSıS uygulamalarını yayımlamak ve sorgulamak istediğiniz [bölgelerde](luis-reference-regions.md#publishing-regions) yazmak önemlidir.

## <a name="resource-ownership"></a>Kaynak sahipliği

LUO gibi bir Azure kaynağı, kaynağı içeren aboneliğe aittir.

Bir kaynağın sahipliğini aktarmak için, OU şunlardan biri olabilir:
* Aboneliğinizin [sahipliğini](../../cost-management-billing/manage/billing-subscription-transfer.md) aktarma
* LUO uygulamasını bir dosya olarak dışarı aktarın ve uygulamayı farklı bir abonelikte içeri aktarın. Dışarı aktarma işlemi, Lua portalındaki **uygulamalarım** sayfasında bulunur.


## <a name="resource-limits"></a>Kaynak sınırları

### <a name="authoring-key-creation-limits"></a>Anahtar oluşturma sınırlarını yazma

Her abonelik için bölge başına en fazla 10 yazma anahtarı oluşturabilirsiniz.

Bkz. [anahtar sınırları](luis-limits.md#key-limits) ve [Azure bölgeleri](luis-reference-regions.md).

Yayımlama bölgeleri, yazma bölgelerinden farklıdır. İstemci uygulamanızın bulunmasını istediğiniz yayımlama bölgesine karşılık gelen yazma bölgesinde bir uygulama oluşturduğunuzdan emin olun.

### <a name="key-usage-limit-errors"></a>Anahtar kullanımı sınırı hataları

Kullanım sınırları, fiyatlandırma katmanını temel alır.

İşlem/saniye (TPS) kotasını aşarsanız bir HTTP 429 hatası alırsınız. İşlem/saat (TPS) kotasını aşarsanız bir HTTP 403 hatası alırsınız.


### <a name="reset-authoring-key"></a>Yazma anahtarını Sıfırla

[Kaynak geçişi](luis-migration-authoring.md) yapılan uygulamalar yazmak için: yazma anahtarınız tehlikeye atılırsa, bu yazma kaynağının **anahtarlar** sayfasındaki Azure Portal anahtarı sıfırlayın.

Henüz geçirilmeyen uygulamalar için: anahtar, LUSıS portalındaki tüm uygulamalarınızda sıfırlanır. Uygulamalarınızı yazma API 'Leri aracılığıyla yazardıysanız, OCP-apim-Subscription-Key değerini yeni anahtar olarak değiştirmeniz gerekir.

### <a name="regenerate-azure-key"></a>Azure anahtarını yeniden üret

**Anahtarlar** sayfasında Azure Portal Azure anahtarlarını yeniden oluşturun.


## <a name="app-ownership-access-and-security"></a>Uygulama sahipliği, erişim ve güvenlik

Bir uygulama, Azure kaynakları tarafından tanımlanır ve bu, sahibin aboneliğine göre belirlenir.

LUSıS uygulamanızı taşıyabilirsiniz. Azure portal veya Azure CLı 'de aşağıdaki belge kaynaklarını kullanın:

* [Uygulamayı LUSıS yazma kaynakları arasında taşıma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Kaynağı yeni kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md)
* [Kaynağı aynı abonelik içinde veya abonelikler arasında taşıma](../../azure-resource-manager/management/move-limitations/app-service-move-limitations.md)


### <a name="contributions-from-other-authors"></a>Diğer yazarlardan katkı

[Kaynak geçişi](luis-migration-authoring.md) yapılan uygulamalar yazmak için: _katkıda bulunanlar_ , **Access Control (IAM)** sayfasını kullanarak yazma kaynağı için Azure Portal yönetilir. Ortak çalışan e-posta adresini ve _katkıda_ bulunan rolünü kullanarak [bir Kullanıcı eklemeyi](luis-how-to-collaborate.md)öğrenin.

Henüz geçirilmeyen uygulamalar için: tüm _ortak çalışanlar_ , **Manage-> ortak çalışanlar** sayfasından Luo portalında yönetilir.

### <a name="query-prediction-access-for-private-and-public-apps"></a>Özel ve genel uygulamalar için sorgu tahmin erişimi

**Özel** bir uygulama için, sorgu tahmini çalışma zamanı erişimi, sahipler ve katkıda bulunanlar için kullanılabilir. **Ortak** bir uygulama için, çalışma zamanı erişimi kendi Azure bilişsel [hizmeti](../cognitive-services-apis-create-account.md) veya [lusıs](#create-resources-in-the-azure-portal) çalışma zamanı kaynağı olan herkese açıktır ve ortak uygulamanın kimliğine sahiptir.

Şu anda, ortak uygulamaların bir kataloğu yoktur.

### <a name="authoring-permissions-and-access"></a>Yazma izinleri ve erişim
[LUIS](luis-reference-regions.md#luis-website) portalından uygulamaya erişim veya [yazma API 'leri](https://go.microsoft.com/fwlink/?linkid=2092087) , Azure yazma kaynağı tarafından denetlenir.

Sahibi ve tüm katkıda bulunanlar, uygulamayı yazmak için erişime sahiptir.

|Yazma erişimi şunları içerir|Notlar|
|--|--|
|Uç nokta anahtarları ekleme veya kaldırma||
|Sürüm dışarı aktarılıyor||
|Uç nokta günlüklerini dışarı aktar||
|Sürüm içeri aktarılıyor||
|Uygulamayı genel yap|Bir uygulama genel olduğunda, yazma veya bitiş noktası anahtarına sahip herkes uygulamayı sorgulayabilir.|
|Modeli değiştir|
|Yayımlama|
|[Etkin öğrenme](luis-how-to-review-endpoint-utterances.md) için uç nokta utslerini gözden geçirme|
|Eğitim|

<a name="prediction-endpoint-runtime-key"></a>

### <a name="prediction-endpoint-runtime-access"></a>Tahmin uç noktası çalışma zamanı erişimi

Tahmin uç noktası sorgulama erişimi, **Yönet** bölümündeki **uygulama bilgileri** sayfasında bir ayarla denetlenir.

|[Özel uç nokta](#runtime-security-for-private-apps)|[Genel uç nokta](#runtime-security-for-public-apps)|
|:--|:--|
|Sahip ve katkıda bulunanlar için kullanılabilir|Uygulama KIMLIĞINI bilen sahip, katkıda bulunanlar ve diğer kişiler için kullanılabilir|

Bir sunucudan sunucuya bir ortama çağırarak, LUSıS çalışma zamanı anahtarınızı kimlerin göreceğini denetleyebilirsiniz. Bir bot 'tan LUSıS kullanıyorsanız, bot ve LUYA arasındaki bağlantı zaten güvenlidir. LUU uç noktasını doğrudan arıyorsanız, denetimli erişime ( [AAD](https://azure.microsoft.com/services/active-directory/)gibi) sahip bir sunucu tarafı API (Azure [işlevi](https://azure.microsoft.com/services/functions/)gibi) oluşturmanız gerekir. Sunucu tarafı API 'SI çağrıldığında ve kimlik doğrulaması yapıldığında ve yetkilendirme doğrulandığında, çağrısı HALSıS 'e geçirin. Bu strateji, ortadaki adam saldırıları engellemez, ancak kullanıcılarınızın anahtar ve uç nokta URL 'nizi, kullanıcılarınızı izlemenizi sağlar ve uç nokta yanıt günlüğü (örneğin, [Application Insights](https://azure.microsoft.com/services/application-insights/)) eklemenize olanak tanır.

### <a name="runtime-security-for-private-apps"></a>Özel uygulamalar için çalışma zamanı güvenliği

Özel bir uygulamanın çalışma zamanı yalnızca şunlar için kullanılabilir:

|Anahtar ve Kullanıcı|Açıklama|
|--|--|
|Sahibin yazma anahtarı| En fazla 1000 uç nokta okuması|
|Ortak çalışan/katkıda bulunan yazma anahtarları| En fazla 1000 uç nokta okuması|
|LUŞUNA atanan herhangi bir anahtar, bir yazar veya ortak çalışan/katkıda bulunan|Anahtar kullanımı katmanına göre|

### <a name="runtime-security-for-public-apps"></a>Ortak uygulamalar için çalışma zamanı güvenliği

Bir uygulama ortak olarak yapılandırıldıktan sonra, anahtarın _tüm uç_ nokta kotasını kullanmadığı sürece, GEÇERLI bir luya yazma anahtarı veya Luo uç nokta anahtarı uygulamanızı sorgulayabilir.

Sahibi veya katkıda bulunan olmayan bir Kullanıcı, uygulama KIMLIĞI verildiğinde yalnızca ortak uygulamanın çalışma zamanına erişebilir. LUYA genel bir uygulama aramak için genel bir _Pazar_ veya başka bir yol yoktur.

Ortak bir uygulama tüm bölgelerde yayımlanır, böylece bölge tabanlı bir LUTO kaynak anahtarı olan bir Kullanıcı, kaynak anahtarıyla ilişkili bölgede bulunan uygulamaya erişebilir.


### <a name="securing-the-query-prediction-endpoint"></a>Sorgu tahmin uç noktasının güvenliğini sağlama

Bir sunucudan sunucuya bir ortama çağırarak, LUSıS tahmin çalışma zamanı uç noktası anahtarınızı kimlerin görebileceğini denetleyebilirsiniz. Bir bot 'tan LUSıS kullanıyorsanız, bot ve LUYA arasındaki bağlantı zaten güvenlidir. LUU uç noktasını doğrudan arıyorsanız, denetimli erişime ( [AAD](https://azure.microsoft.com/services/active-directory/)gibi) sahip bir sunucu tarafı API (Azure [işlevi](https://azure.microsoft.com/services/functions/)gibi) oluşturmanız gerekir. Sunucu tarafı API 'SI çağrıldığında ve kimlik doğrulaması ve yetkilendirme doğrulandıktan sonra, çağrıyı HALSıS 'e geçirin. Bu strateji, ortadaki adam saldırıları engellemez, ancak uç noktanızı kullanıcılarınıza göre gizleme, erişimi izlemenize ve uç nokta yanıt günlüğü (örneğin [Application Insights](https://azure.microsoft.com/services/application-insights/)) eklemenize olanak tanır.

<a name="starter-key"></a>

## <a name="sign-in-to-luis-portal-and-begin-authoring"></a>LUSıS portalında oturum açın ve yazmaya başlayın

1. [Luo portalında](https://www.luis.ai) oturum açın ve kullanım koşullarını kabul edin.
1. Hangi tür bir LUSıS yazma anahtarını kullanmak istediğinizi seçerek LUSıS uygulamanızı başlatın: ücretsiz deneme anahtarı veya yeni Azure LUSıS yazma anahtarı.

    ![Language Understanding yazma kaynağı türünü seçin](./media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Kaynak seçim sürecinizle işiniz bittiğinde [Yeni bir uygulama oluşturun](luis-how-to-start-new-app.md#create-new-app-in-luis).


## <a name="create-azure-resources"></a>Azure kaynakları oluşturma

<a name="create-resources-in-the-azure-portal"></a>

[!INCLUDE [Create LUIS resource in Azure Portal](includes/create-luis-resource.md)]

### <a name="create-resources-in-azure-cli"></a>Azure CLı 'de kaynak oluşturma

Her kaynağı ayrı ayrı oluşturmak için [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 'yi kullanın.

Kaynak `kind` :

* Özgün`LUIS.Authoring`
* Hızlı`LUIS`

1. Azure CLı 'da oturum açın:

    ```azurecli
    az login
    ```

    Bu, doğru hesabı seçmenizi ve kimlik doğrulaması sağlamanıza olanak tanıyan bir tarayıcı açar.

1. **LUIS authoring resource** `LUIS.Authoring` `my-luis-authoring-resource` Bölge için adlı _mevcut_ kaynak grubunda adlı bir lusıs yazma kaynağı oluşturun `my-resource-group` `westus` .

    ```azurecli
    az cognitiveservices account create -n my-luis-authoring-resource -g my-resource-group --kind LUIS.Authoring --sku F0 -l westus --yes
    ```

1. **LUIS prediction endpoint resource** `LUIS` `my-luis-prediction-resource` Bölge için adlı _mevcut_ kaynak grubunda adlı, türü bir lusıs tahmin uç noktası kaynağı oluşturun `my-resource-group` `westus` . Ücretsiz katmandan daha yüksek bir aktarım hızı istiyorsanız `F0` olarak değiştirin `S0` . [Fiyatlandırma katmanları ve verimlilik](luis-limits.md#key-limits)hakkında daha fazla bilgi edinin.

    ```azurecli
    az cognitiveservices account create -n my-luis-prediction-resource -g my-resource-group --kind LUIS --sku F0 -l westus --yes
    ```

    > [!Note]
    > Bu anahtarlar, **Manage-> Azure KAYNAKLARıNDA**Luo portalında atanana kadar Luo portalı **tarafından kullanılmaz.**

<a name="assign-an-authoring-resource-in-the-luis-portal-for-all-apps"></a>

### <a name="assign-resource-in-the-luis-portal"></a>LUSıS portalında kaynak atama

Tek bir uygulama için veya LUSıS 'deki tüm uygulamalar için bir yazma kaynağı atayabilirsiniz. Aşağıdaki yordam, tüm uygulamaları tek bir yazma kaynağına atar.

1. [Lui portalında](https://www.luis.ai)oturum açın.
1. Üst gezinti çubuğunda, en sağdaki kullanıcı hesabınızı seçin ve ardından **Ayarlar**' ı seçin.
1. **Kullanıcı ayarları** sayfasında, **yazma kaynağı Ekle** ' yi seçin ve var olan bir yazma kaynağı seçin. **Kaydet**’i seçin.

## <a name="assign-a-resource-to-an-app"></a>Uygulamaya kaynak atama

Aşağıdaki yordamla bir uygulamaya bir atayabilirsiniz.

1. [Luo portalında](https://www.luis.ai)oturum açın, sonra **uygulamalarım** listesinden bir uygulama seçin.
1. **Yönet-> Azure kaynakları** sayfasına gidin.

    ![Uygulamaya bir kaynak atamak için LUU portalında Manage-> Azure kaynaklarını seçin.](./media/luis-how-to-azure-subscription/manage-azure-resources-prediction.png)

1. Tahmin veya yazma kaynağı sekmesini seçin, ardından **tahmin kaynağı Ekle** veya **yazma kaynağı Ekle** düğmesini seçin.
1. Doğru kaynağı bulmak için formdaki alanları seçin ve ardından **Kaydet**' i seçin.

### <a name="assign-query-prediction-runtime-resource-without-using-luis-portal"></a>LUO portalını kullanmadan sorgu tahmini çalışma zamanı kaynağı atama

CI/CD işlem hattı gibi Otomasyon amaçları için bir lusıs çalışma zamanı kaynağının bir LUU uygulamasına atanmasını otomatikleştirmek isteyebilirsiniz. Bunu yapmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Bu [Web sitesinden](https://resources.azure.com/api/token?plaintext=true)bir Azure Resource Manager belirteci alın. Bu belirtecin kullanım süresini hemen kullanın. İstek bir Azure Resource Manager belirteci döndürüyor.

    ![Azure Resource Manager belirteci iste ve Azure Resource Manager belirteci al](./media/luis-manage-keys/get-arm-token.png)

1. Kullanıcı hesabınızın erişimi olan [Get lusıs Azure hesapları API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c)'sinden, halsıs çalışma zamanı kaynaklarını abonelikler arasında istemek için belirteci kullanın.

    Bu POST API 'SI aşağıdaki ayarları gerektirir:

    |Üst bilgi|Değer|
    |--|--|
    |`Authorization`|Öğesinin değeri `Authorization` `Bearer {token}` . Belirteç değerinin önünde `Bearer` ve bir boşluk olması gerektiğini unutmayın.|
    |`Ocp-Apim-Subscription-Key`|Yazma anahtarınız.|

    Bu API, abonelik KIMLIĞI, kaynak grubu ve kaynak adı gibi, hesap adı olarak döndürülen LUSıS aboneliklerinizin JSON nesnelerinin bir dizisini döndürür. LUO uygulamasına atanacak LUO kaynağı olan dizideki bir öğeyi bulun.

1. [Bir uygulama API 'sine BIR lusıs Azure hesabı atama](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515) ile, belirteci halsıs kaynağına atayın.

    Bu POST API 'SI aşağıdaki ayarları gerektirir:

    |Tür|Ayar|Değer|
    |--|--|--|
    |Üst bilgi|`Authorization`|Öğesinin değeri `Authorization` `Bearer {token}` . Belirteç değerinin önünde `Bearer` ve bir boşluk olması gerektiğini unutmayın.|
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
* Yeni [yazma kaynağına](luis-migration-authoring.md) geçir
