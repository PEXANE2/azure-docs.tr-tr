---
title: Abonelik anahtarları-LUSıS
titleSuffix: Azure Cognitive Services
description: Ücretsiz ilk 1000 uç nokta sorgularını kullanmak için abonelik anahtarları oluşturmanız gerekmez. Bir HTTP 403 veya 429 biçiminde bir hatalı _Kota_ hatası alırsanız, anahtar oluşturmanız ve uygulamanıza atamanız gerekir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/10/2019
ms.author: diberry
ms.openlocfilehash: c7e23c78b5d03b834d593bd2b53958c3379c51f2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560523"
---
# <a name="using-subscription-keys-with-your-luis-app"></a>LUIS uygulamanız ile abonelik anahtarlarını kullanma

Language Understanding (LUU) ilk kez kullandığınızda, abonelik anahtarları oluşturmanız gerekmez. İle başlamak için 1000 uç nokta sorgusu vermiş olursunuz. 

Test ve yalnızca prototip için ücretsiz katman (F0) kullanın. Üretim sistemleri için bir [Ücretli](https://aka.ms/luis-price-tier) katmanı. Kullanmayın [anahtar yazma](luis-concept-keys.md#authoring-key) için üretim uç noktası sorgular.


<a name="create-luis-service"></a>
<a name="create-language-understanding-endpoint-key-in-the-azure-portal"/>

## <a name="create-prediction-endpoint-runtime-resource-in-the-azure-portal"></a>Azure portal tahmin uç noktası çalışma zamanı kaynağı oluşturma

[Tahmin uç noktası kaynağını](get-started-portal-deploy-app.md#create-the-endpoint-resource) Azure Portal oluşturursunuz. Bu kaynak yalnızca Endpoint tahmin sorguları için kullanılmalıdır. Uygulamada değişiklik yazmak için bu kaynağı kullanmayın.

Bir Language Understanding kaynağı veya bilişsel hizmetler kaynağı oluşturabilirsiniz. Bir Language Understanding kaynağı oluşturuyorsanız, kaynak türünü Kaynak adına erteetmeniz iyi bir uygulamadır. 

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
<a name="key-concepts"></a>
<a name="authoring-key"></a>
<a name="create-and-use-an-endpoint-key"></a>
<a name="assign-endpoint-key"></a>
<a name="assign-resource"></a>

### <a name="using-resource-from-luis-portal"></a>LUSıS portalından kaynak kullanma

Kaynağı LUO portalından kullanıyorsanız, anahtarınızı ve konumunuzu bilmeniz gerekmez. Bunun yerine, kaynak kiracınızı, aboneliğinizi ve kaynak adınızı bilmeniz gerekir.

Bir kaynağı LUU portalında LUSıS uygulamanıza [atadıktan](#assign-resource-key-to-luis-app-in-luis-portal) sonra, anahtar ve konum, bölümün **anahtarlarını ve uç nokta ayarlarını** Yönet sayfasında sorgu tahmini uç nokta URL 'sinin bir parçası olarak sağlanır.
 
### <a name="using-resource-from-rest-api-or-sdk"></a>REST API veya SDK 'dan kaynak kullanma

Kaynağı REST API veya SDK 'dan kullanıyorsanız, anahtarınızı ve konumunuzu bilmeniz gerekir. Bu bilgiler, kaynağın genel bakış ve anahtarlar sayfalarındaki Azure portal **ve son nokta ayarlarını** yönetme sayfasındaki sorgu tahmini uç nokta URL 'sinin bir parçası olarak sağlanır.

## <a name="assign-resource-key-to-luis-app-in-luis-portal"></a>LUSıS portalında LUSıS uygulamasına kaynak anahtarı atama

LUHER yeni kaynak oluşturduğunuzda, [kaynağı Luo uygulamasına atamanız](get-started-portal-deploy-app.md#assign-the-resource-key-to-the-luis-app-in-the-luis-portal)gerekir. Atandıktan sonra, yeni bir kaynak oluşturmadığınız takdirde bu adımı tekrar yapmanız gerekmez. Uygulamanızın bölgelerini genişletmek veya daha yüksek sayıda tahmin sorgusu desteklemek için yeni bir kaynak oluşturabilirsiniz.

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

### <a name="unassign-resource"></a>Kaynak atamasını Kaldır
Uç nokta ataması, Azure'dan silinmez. Yalnızca LUIS bağlantısı kesilebilir. 

Uç noktası anahtarı atanmamış veya uygulamaya atanmamış herhangi için uç nokta URL'sini, bir hata döndürür istek: `401 This application cannot be accessed with the current subscription`. 

### <a name="include-all-predicted-intent-scores"></a>Amaç tüm tahmin edilen puanları içerir
**INCLUDE tüm hedefi puanları tahmin** onay kutusu her amacını tahmin puanını dahil etmek uç nokta sorgu yanıt verir. 

Bu ayar, sohbet botu veya döndürülen ıntents puanları temel alarak programlı karar vermek için arama LUIS uygulamanızı sağlar. Genellikle üst iki amacı en ilginç var. En çok puan hiçbiri hedefi yok hedefi ve diğer yüksek puanlı amacını arasında kesin bir seçim yapar izleme bir soru sorun, sohbet botu seçebilirsiniz ise. 

Hedefleri ve puanlarını Ayrıca, uç nokta günlükleri dahil. Yapabilecekleriniz [dışarı](luis-how-to-start-new-app.md#export-app) Bu günlükleri ve puanları analiz edin. 

```JSON
{
  "query": "book a flight to Cairo",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.5223427
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.5223427
    },
    {
      "intent": "BookFlight",
      "score": 0.372391433
    }
  ],
  "entities": []
}
```

### <a name="enable-bing-spell-checker"></a>Bing yazım denetimi etkinleştir 
İçinde **uç nokta URL'si ayarları**, **Bing yazım denetleyicisi** geçiş sözcüklerin tahmin önce düzeltmek LUIS sağlar. Oluşturma bir  **[Bing yazım denetimi anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)** . 

**SpellCheck = true** QueryString parametresini ve **Bing-yazım denetimi-abonelik-Key = {YOUR_BING_KEY_HERE}** öğesini ekleyin. Değiştirin `{YOUR_BING_KEY_HERE}` Bing yazım denetleyicisi anahtarınızı.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```

### <a name="publishing-regions"></a>Yayımlama bölgeleri

Yayımlama hakkında daha fazla bilgi [bölgeleri](luis-reference-regions.md) yayımlama çalıştırmasına dahil olmak üzere [Avrupa](luis-reference-regions.md#publishing-to-europe), ve [Avustralya](luis-reference-regions.md#publishing-to-australia). Yayımlama bölgeler bölge geliştirme farklıdır. Yazma bölgesinde sorgu uç nokta için istediğiniz yayımlama bölgeyi karşılık gelen bir uygulama oluşturun.

## <a name="assign-resource-without-luis-portal"></a>LUIS portalı olmadan kaynak atayın

Bir CI/CD işlem hattı gibi Otomasyon amacıyla bir LUIS uygulaması LUIS kaynağa atamasını otomatik hale getirmek isteyebilirsiniz. Bunu yapmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Azure Resource Manager bu belirteci alma [Web sitesi](https://resources.azure.com/api/token?plaintext=true). Bu belirtecin süresi dolacak şekilde hemen kullanın. İstek, bir Azure Resource Manager belirtecini döndürür.

    ![Azure Resource Manager belirteci iste ve Azure Resource Manager belirteci al](./media/luis-manage-keys/get-arm-token.png)

1. LUIS kaynakları abonelikler arasında istek için belirteci kullanın [azure alma LUIS API'si hesapları](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), kullanıcı hesabınızın erişebilir. 

    Bu POST API'sini aşağıdaki ayarları gerektirir:

    |Üst bilgi|Değer|
    |--|--|
    |`Authorization`|Değerini `Authorization` olduğu `Bearer {token}`. Belirteç değeri sözcük gelmelidir fark `Bearer` ve boşluk.| 
    |`Ocp-Apim-Subscription-Key`|[Anahtar yazma](luis-how-to-account-settings.md).|

    Bu API bir LUIS aboneliğiniz, abonelik kimliği, kaynak grubu ve kaynak adı, hesap adı olarak döndürülen dahil olmak üzere JSON nesne dizisi döndürür. LUIS uygulaması için atama LUIS kaynak dizideki bir öğe bulur. 

1. LUIS kaynak belirteci atamak [bir LUIS azure hesapları bir uygulamaya atama](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515) API. 

    Bu POST API'sini aşağıdaki ayarları gerektirir:

    |Tür|Ayar|Değer|
    |--|--|--|
    |Üst bilgi|`Authorization`|Değerini `Authorization` olduğu `Bearer {token}`. Belirteç değeri sözcük gelmelidir fark `Bearer` ve boşluk.|
    |Üst bilgi|`Ocp-Apim-Subscription-Key`|[Anahtar yazma](luis-how-to-account-settings.md).|
    |Üst bilgi|`Content-type`|`application/json`|
    |Sorgu dizesi|`appid`|LUIS app kimliği. 
    |Gövde||{"Azuresubscriptionıd": "ddda2925-af7f-4b05-9ba1-2155c5fe8a8e"<br>"ResourceGroup": "resourcegroup-2"<br>"AccountName": "luıs-uswest-S0-2"}|

    Bu API, başarılı olduğunda, 201 - oluşturuldu durumuna döndürür. 

## <a name="change-pricing-tier"></a>Fiyatlandırma katmanını değiştir

1.  İçinde [Azure](https://portal.azure.com), LUIS aboneliğinizi bulun. LUSıS aboneliğini seçin.
    ![LUIS aboneliğinizi bulun](./media/luis-usage-tiers/find.png)
1.  Kullanılabilir fiyatlandırma katmanlarını görmek için **fiyatlandırma katmanını** seçin. 
    ![Fiyatlandırma katmanları görüntüleyin](./media/luis-usage-tiers/subscription.png)
1.  Fiyatlandırma katmanını seçin ve Değişiklerinizi kaydetmek için **Seç** ' i seçin. 
    ![LUIS ödeme katmanınızı değiştirin](./media/luis-usage-tiers/plans.png)
1.  Fiyat değişikliği tamamlandıktan sonra bir açılır pencere yeni fiyatlandırma katmanına doğrular. 
    ![LUIS ödeme katmanınızı doğrulayın](./media/luis-usage-tiers/updated.png)
1. Unutmayın [Bu uç noktası anahtarı atama](#assign-endpoint-key) üzerinde **Yayımla** sayfasında ve tüm uç nokta sorguları kullanın. 

## <a name="fix-http-status-code-403-and-429"></a>HTTP durum kodunu onarma 403 ve 429

Fiyatlandırma katmanınız için saniye başına işlemleri veya aylık işlem sayısını aşarsanız 403 ve 429 hata durum kodları alırsınız.

### <a name="when-you-receive-an-http-403-error-status-code"></a>HTTP 403 hata durum kodu aldığınızda

Tüm bu ücretsiz 1000 uç nokta sorgularını kullandığınızda veya fiyatlandırma katmanınızın aylık işlem kotasını aşarsanız, bir HTTP 403 hata durum kodu alırsınız. 

Bu hatayı onarmak için [fiyatlandırma katmanınızı](luis-how-to-azure-subscription.md#change-pricing-tier) daha yüksek bir katmana değiştirmeniz veya [Yeni bir kaynak oluşturup](get-started-portal-deploy-app.md#create-the-endpoint-resource) [uygulamanıza atamanız](get-started-portal-deploy-app.md#assign-the-resource-key-to-the-luis-app-in-the-luis-portal)gerekir.

Bu hata için çözümler şunlardır:

* [Azure Portal](https://portal.azure.com), Language Understanding kaynağınızın **kaynak yönetimi-> fiyatlandırma katmanında**, fiyatlandırma katmanınızı daha yüksek bir TPS katmanına değiştirin. Kaynağınız Language Understanding uygulamanıza zaten atanmışsa Language Understanding portalında herhangi bir şey yapmanız gerekmez.
*  Kullanımınız en yüksek fiyatlandırma katmanını aşarsa, önde gelen yük dengeleyiciye daha fazla Language Understanding kaynağı ekleyin. Kubernetes veya Docker Compose ile [Language Understanding kapsayıcısı](luis-container-howto.md) bu konuda yardımcı olabilir.

### <a name="when-you-receive-an-http-429-error-status-code"></a>HTTP 429 hata durum kodu aldığınızda

Bu durum kodu, işlemleriniz fiyatlandırma katmanınızı aştığında döndürülür.  

Çözümler şunlardır:

* En yüksek katmanda değilseniz [fiyatlandırma katmanınızı artırabilirsiniz](#change-pricing-tier).
* Kullanımınız en yüksek fiyatlandırma katmanını aşarsa, önde gelen yük dengeleyiciye daha fazla Language Understanding kaynağı ekleyin. Kubernetes veya Docker Compose ile [Language Understanding kapsayıcısı](luis-container-howto.md) bu konuda yardımcı olabilir.
* Bu durum kodunu aldığınızda, istemci uygulama isteklerinizi sizin uyguladığınız bir [yeniden deneme ilkesiyle](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults#general-guidelines) geçit olarak kullanabilirsiniz. 

## <a name="viewing-summary-usage"></a>Özet kullanımı görüntüleme
Azure'da LUIS kullanım bilgilerini görüntüleyebilirsiniz. **Genel bakış** sayfası çağrı ve hata dahil olmak üzere son Özet bilgilerini gösterir. Bir LUIS uç nokta isteği yapıyorsa, hemen izleyin **genel bakış sayfasında**, gösterilecek kullanım beş dakika bekleyin.

![Özet kullanımı görüntüleme](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Kullanım grafikleri özelleştirme
Ölçümleri verilerine daha ayrıntılı bir görünüm sağlar.

![Varsayılan ölçümleri](./media/luis-usage-tiers/metrics-default.png)

Zaman dilimi ve ölçü türü için ölçüm grafikleri yapılandırabilirsiniz. 

![Özel ölçümler](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Toplam işlem eşiği Uyarısı
Belirli bir işlem eşiği, örneğin 10.000 işlem ulaştığınız zaman bilmek istiyorsanız, bir uyarı oluşturabilirsiniz. 

![Varsayılan Uyarıları](./media/luis-usage-tiers/alert-default.png)

İçin ölçüm uyarısı Ekle **toplam çağrıları** belirli bir süre ölçümü. Uyarı alması gereken tüm kişilerin e-posta adreslerini ekleyin. Uyarı alması gereken tüm sistemleri için Web kancaları ekleyin. Uyarı tetiklendiğinde bir mantıksal uygulama da çalıştırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Nasıl kullanacağınızı öğrenin [sürümleri](luis-how-to-manage-versions.md) değişiklikleri LUIS uygulamanızı yönetme.
