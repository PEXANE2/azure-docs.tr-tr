---
title: Yazma ve çalışma zamanı anahtarları-LUSıS
titleSuffix: Azure Cognitive Services
description: LUSıS iki anahtar kullanır, modelinizi oluşturmak için yazma anahtarı ve tahmin uç noktasını Kullanıcı utsları ile sorgulamak için çalışma zamanı anahtarı kullanılır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 70e58077fa40ce685324cd24b447886ec3411034
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703191"
---
# <a name="authoring-and-runtime-keys"></a>Yazma ve çalışma zamanı anahtarları


>[!NOTE]
>Devam etmeden önce lütfen Azure yazma kaynağını kullanmayan tüm uygulamaları [geçirin](luis-migration-authoring.md) .

LUSıS iki tür Azure kaynağı kullanır, her tür anahtara sahiptir: 
 
* Amaç, varlık ve etiket oluşturma, eğitim ve yayımlama oluşturmak için [yazma](#programmatic-key) . LUSıS uygulamanızı yayımlamaya hazır olduğunuzda, uygulamaya atanan [çalışma zamanı için bir tahmin uç noktası anahtarına](luis-how-to-azure-subscription.md) ihtiyacınız vardır.
* [Çalışma zamanı Için tahmin uç noktası anahtarı](#prediction-endpoint-runtime-key). Bir sohbet bot gibi istemci uygulamalar, bu anahtar aracılığıyla çalışma zamanının **sorgu tahmini uç noktasına** erişmesi gerekir. 

|Anahtar|Amaç|Bilişsel hizmet `kind`|Bilişsel hizmet `type`|
|--|--|--|--|
|[Yazma anahtarı](#programmatic-key)|Yazma, eğitim, yayımlama, test etme.|`LUIS.Authoring`|`Cognitive Services`|
|[Tahmin uç noktası çalışma zamanı anahtarı](#prediction-endpoint-runtime-key)| Hedefleri ve varlıkları tespit etmek için bir kullanıcı ile birlikte tahmin uç noktası çalışma zamanını sorgulayın.|`LUIS`|`Cognitive Services`|

LUDA, aylık tahmin uç noktası kotası 1000 işlem içeren bir [Başlangıç anahtarı](luis-how-to-azure-subscription.md#starter-key) sağlar. 

Her iki anahtarı da aynı anda oluşturmanız gerekmiyorsa, bunu yaparsanız çok daha kolay olur.

LUSıS uygulamalarını yayımlamak ve sorgulamak istediğiniz [bölgelerde](luis-reference-regions.md#publishing-regions) yazmak önemlidir.

<a name="programmatic-key" ></a>

## <a name="authoring-key"></a>Yazma anahtarı

Bir LUVE hesabı oluşturduğunuzda bir yazma anahtarı otomatik olarak oluşturulur ve ücretsizdir. Luo ile başladığınızda, her yazma [bölgesi](luis-reference-regions.md)IÇIN tüm lusıs uygulamalarınız genelinde bir başlangıç anahtarınız olur. Yazma anahtarının amacı, LUSıS uygulamanızı yönetmek veya tahmin uç nokta sorgularını test etmek için kimlik doğrulaması sağlamaktır. 

Azure portal yazma anahtarları oluşturma, kişileri [katkıda bulunan rolüne](#contributions-from-other-authors)atayarak yazma kaynağı izinlerini denetlemenize olanak tanır. Katkıda bulunanlar eklemek için Azure abonelik düzeyinde izninizin olması gerekir. 

Yazma anahtarını bulmak için, [Luo](luis-reference-regions.md#luis-website) 'da oturum açın ve sağ üst gezinti çubuğundaki hesap adına tıklayarak **Hesap ayarlarını**açın.

![yazma anahtarı](./media/luis-concept-keys/authoring-key.png)

**Çalışma zamanı sorguları**yapmak Istediğinizde, Azure [Luo kaynağını](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/)oluşturun. 

> [!CAUTION]
> Kolaylık olması için, çoğu örnek, [kotasında](luis-boundaries.md#key-limits)birkaç ücretsiz tahmin uç noktası çağrısı sağladığından [Başlangıç anahtarını](#starter-prediction-endpoint-runtime-key) kullanır.  

<a name="endpoint-key"></a>

## <a name="prediction-endpoint-runtime-key"></a>Tahmin uç noktası çalışma zamanı anahtarı 

**Çalışma zamanı uç noktası sorgularına**ihtiyacınız olduğunda, bir Language UNDERSTANDING (lusıs) kaynağı OLUŞTURUP bunu Luo uygulamasına atayın. 

[!INCLUDE [Azure runtime resource creation for Language Understanding and Cognitive Service resources](../../../includes/cognitive-services-luis-azure-resource-instructions.md)]

Kaynak oluşturma işlemi tamamlandığında, [anahtarı uygulamaya atayın](luis-how-to-azure-subscription.md) . 

* Çalışma zamanı (sorgu tahmin uç noktası) anahtarı, çalışma zamanı anahtarı oluştururken belirlediğiniz kullanım planına bağlı olarak uç nokta isabetlerinin kotasına izin verir. Fiyatlandırma bilgileri için bkz. bilişsel [Hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) .

* Çalışma zamanı anahtarı tüm LUSıS uygulamalarınız veya belirli bir LUL Apps için kullanılabilir. 
* LUSıS uygulamalarını yazmak için çalışma zamanı anahtarını kullanmayın. 

### <a name="starter-prediction-endpoint-runtime-key"></a>Başlatıcı tahmin uç noktası çalışma zamanı anahtarı

**Başlangıç** tahmin uç noktası anahtarı ücretsiz sağlar ve 1000 tahmin uç nokta sorgularını içerir. Bu sorgular kullanıldıktan sonra, Language Understanding için kendi tahmin uç noktası kaynağını oluşturmanız gerekir.  

Bu, sizin için oluşturulan özel bir kaynaktır. Geçici bir başlangıç anahtarı olarak hazırlandığından, Azure kaynakları listenizde görünmez. 

<a name="use-endpoint-key-in-query"></a>

### <a name="use-runtime-key-in-query"></a>Sorguda çalışma zamanı anahtarını kullan
LUıN çalışma zamanı uç noktası iki sorgu stilini kabul eder, her ikisi de tahmin uç noktası çalışma zamanı anahtarını kullanır, ancak farklı yerlerde.

Çalışma zamanına erişmek için kullanılan uç nokta, aşağıdaki tabloda `{region}` ile belirtilen, kaynağınızın bölgesi için benzersiz olan bir alt etki alanı kullanır. 


#### <a name="v2-prediction-endpointtabv2"></a>[V2 tahmin uç noktası](#tab/V2)

|Ü|Örnek URL ve anahtar konumu|
|--|--|
|[Al](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?runtime-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`|
|[Yayınla](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`|

#### <a name="v3-prediction-endpointtabv3"></a>[V3 tahmin uç noktası](#tab/V3)

|Ü|Örnek URL ve anahtar konumu|
|--|--|
|[Al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/luis-endpoint-api-v3-0-preview/operations/5cb0a91e54c9db63d589f433)|`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?runtime-key=your-endpoint-key-here&query=turn%20on%20the%20lights`|
|[Yayınla](https://westcentralus.dev.cognitive.microsoft.com/docs/services/luis-endpoint-api-v3-0-preview/operations/5cb0a5830f741b27cd03a061)| `https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict`| 

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * * 

**Get**: lusıs bitiş noktası anahtar kota oranını kullanmak için, `runtime-key` ' den yazma (başlangıç) anahtarından bitiş noktası sorgu değerini yeni uç nokta anahtarına değiştirin. Anahtarı oluşturur ve anahtarı atar ancak `runtime-key` için uç nokta sorgu değerini değiştirmeyin, uç nokta anahtar kotayı kullanmayın.

**Gönderi**: `Ocp-Apim-Subscription-Key` için üst bilgi değerini değiştirme<br>Çalışma zamanı anahtarını oluşturur ve çalışma zamanı anahtarını atarsanız ancak `Ocp-Apim-Subscription-Key` için uç nokta sorgu değerini değiştirmeyin, çalışma zamanı anahtarınızı kullanmayın.

Önceki URL 'Lerde kullanılan uygulama KIMLIĞI, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, [etkileşimli Gösterim](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/)için kullanılan genel IoT uygulamasıdır. 

## <a name="assignment-of-the-runtime-key"></a>Çalışma zamanı anahtarının atanması

Çalışma zamanı anahtarını [LUIS portalında](https://www.luis.ai) veya Ilgili API 'ler aracılığıyla [atayabilirsiniz](luis-how-to-azure-subscription.md) . 

## <a name="key-limits"></a>Anahtar sınırları

Her abonelik için bölge başına en fazla 10 yazma anahtarı oluşturabilirsiniz. 

Bkz. [anahtar sınırları](luis-boundaries.md#key-limits) ve [Azure bölgeleri](luis-reference-regions.md). 

Yayımlama bölgeleri, yazma bölgelerinden farklıdır. İstemci uygulamanızın bulunmasını istediğiniz yayımlama bölgesine karşılık gelen yazma bölgesinde bir uygulama oluşturduğunuzdan emin olun.

## <a name="key-limit-errors"></a>Anahtar sınırı hataları
İşlem/saniye (TPS) kotasını aşarsanız bir HTTP 429 hatası alırsınız. İşlem/saat (TPS) kotasını aşarsanız bir HTTP 403 hatası alırsınız. 

## <a name="contributions-from-other-authors"></a>Diğer yazarlardan katkı



Ortak çalışanların katkılarının yönetimi, uygulamanın geçerli durumuna bağlıdır.

**[Kaynak geçişi](luis-migration-authoring.md) yapılan uygulamalar yazmak için**: _katkıda bulunanlar_ , **Access Control (IAM)** sayfasını kullanarak yazma kaynağı için Azure Portal yönetilir. Ortak çalışan e-posta adresini ve _katkıda_ bulunan rolünü kullanarak [bir Kullanıcı eklemeyi](luis-how-to-collaborate.md)öğrenin. 

**Henüz geçirilmeyen uygulamalar için**: tüm _ortak çalışanlar_ , **Manage-> ortak çalışanlar** sayfasından Luo portalında yönetilir.

### <a name="contributor-roles-vs-entity-roles"></a>Katkıda bulunan rolleri vs varlık rolleri

[Varlık rolleri](luis-concept-roles.md) , lusıs uygulamasının veri modeli için geçerlidir. Ortak çalışan/katkıda bulunan rolleri, yazma erişimi düzeyleri için geçerlidir. 

## <a name="moving-or-changing-ownership"></a>Sahipliği taşıma veya değiştirme

Bir uygulama, Azure kaynakları tarafından tanımlanır ve bu, sahibin aboneliğine göre belirlenir. 

LUSıS uygulamanızı taşıyabilirsiniz. Azure portal veya Azure CLı 'de aşağıdaki belge kaynaklarını kullanın:

* [Uygulamayı LUSıS yazma kaynakları arasında taşıma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Kaynağı yeni kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/resource-group-move-resources.md)
* [Kaynağı aynı abonelik içinde veya abonelikler arasında taşıma](../../azure-resource-manager/move-limitations/app-service-move-limitations.md)
* Aboneliğinizin [sahipliğini aktarma](../../billing/billing-subscription-transfer.md) 

## <a name="access-for-private-and-public-apps"></a>Özel ve genel uygulamalara erişim

**Özel** bir uygulama için çalışma zamanı erişimi, sahipler ve katkıda bulunanlar için kullanılabilir. **Ortak** bir uygulama için, çalışma zamanı erişimi kendi Azure bilişsel [hizmeti](../cognitive-services-apis-create-account.md) veya [lusıs](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) çalışma zamanı kaynağı olan herkese açıktır ve ortak uygulamanın kimliğine sahiptir. 

Şu anda, ortak uygulamaların bir kataloğu yoktur.

### <a name="authoring-access"></a>Yazma erişimi
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
|yayınlamanız|
|[Etkin öğrenme](luis-how-to-review-endpoint-utterances.md) için uç nokta utslerini gözden geçirme|
|Eğit|

### <a name="prediction-endpoint-runtime-access"></a>Tahmin uç noktası çalışma zamanı erişimi

Tahmin uç noktası sorgulama erişimi, **Yönet** bölümündeki **uygulama bilgileri** sayfasında bir ayarla denetlenir. 

![Uygulamayı herkese açık olarak ayarla](./media/luis-concept-security/set-application-as-public.png)

|[Özel uç nokta](#runtime-security-for-private-apps)|[Ortak uç nokta](#runtime-security-for-public-apps)|
|:--|:--|
|Sahip ve katkıda bulunanlar için kullanılabilir|Uygulama KIMLIĞINI bilen sahip, katkıda bulunanlar ve diğer kişiler için kullanılabilir|

Bir sunucudan sunucuya bir ortama çağırarak, LUSıS çalışma zamanı anahtarınızı kimlerin göreceğini denetleyebilirsiniz. Bir bot 'tan LUSıS kullanıyorsanız, bot ve LUYA arasındaki bağlantı zaten güvenlidir. LUU uç noktasını doğrudan arıyorsanız, denetimli erişime ( [AAD](https://azure.microsoft.com/services/active-directory/)gibi) sahip bir sunucu tarafı API (Azure [işlevi](https://azure.microsoft.com/services/functions/)gibi) oluşturmanız gerekir. Sunucu tarafı API 'SI çağrıldığında ve kimlik doğrulaması yapıldığında ve yetkilendirme doğrulandığında, çağrısı HALSıS 'e geçirin. Bu strateji, ortadaki adam saldırıları engellemez, ancak kullanıcılarınızın anahtar ve uç nokta URL 'nizi, kullanıcılarınızı izlemenizi sağlar ve uç nokta yanıt günlüğü (örneğin, [Application Insights](https://azure.microsoft.com/services/application-insights/)) eklemenize olanak tanır.

#### <a name="runtime-security-for-private-apps"></a>Özel uygulamalar için çalışma zamanı güvenliği

Özel bir uygulamanın çalışma zamanı yalnızca şunlar için kullanılabilir:

|Anahtar ve Kullanıcı|Açıklama|
|--|--|
|Sahibin yazma anahtarı| En fazla 1000 uç nokta okuması|
|Ortak çalışan/katkıda bulunan yazma anahtarları| En fazla 1000 uç nokta okuması|
|LUŞUNA atanan herhangi bir anahtar, bir yazar veya ortak çalışan/katkıda bulunan|Anahtar kullanımı katmanına göre|

#### <a name="runtime-security-for-public-apps"></a>Ortak uygulamalar için çalışma zamanı güvenliği

Bir uygulama ortak olarak yapılandırıldıktan sonra, anahtarın _tüm uç_ nokta kotasını kullanmadığı sürece, GEÇERLI bir luya yazma anahtarı veya Luo uç nokta anahtarı uygulamanızı sorgulayabilir.

Sahibi veya katkıda bulunan olmayan bir Kullanıcı, uygulama KIMLIĞI verildiğinde yalnızca ortak uygulamanın çalışma zamanına erişebilir. LUYA genel bir uygulama aramak için genel bir _Pazar_ veya başka bir yol yoktur.  

Ortak bir uygulama tüm bölgelerde yayımlanır, böylece bölge tabanlı bir LUTO kaynak anahtarı olan bir Kullanıcı, kaynak anahtarıyla ilişkili bölgede bulunan uygulamaya erişebilir.

## <a name="transfer-of-ownership"></a>Sahiplik aktarımı

**[Kaynak geçişi](luis-migration-authoring.md) yapılan uygulamalar yazmak için**: kaynağın sahibi olarak, `contributor` ekleyebilirsiniz.

**Henüz geçirilmeyen uygulamalar için**: UYGULAMANıZı bir JSON dosyası olarak dışarı aktarın. Başka bir LUO kullanıcısı uygulamayı içeri aktarabilir, böylece uygulama sahibi olur. Yeni uygulamanın farklı bir uygulama KIMLIĞI olacaktır.  

## <a name="securing-the-endpoint"></a>Uç noktanın güvenliğini sağlama 

Bir sunucudan sunucuya bir ortama çağırarak, LUSıS tahmin çalışma zamanı uç noktası anahtarınızı kimlerin görebileceğini denetleyebilirsiniz. Bir bot 'tan LUSıS kullanıyorsanız, bot ve LUYA arasındaki bağlantı zaten güvenlidir. LUU uç noktasını doğrudan arıyorsanız, denetimli erişime ( [AAD](https://azure.microsoft.com/services/active-directory/)gibi) sahip bir sunucu tarafı API (Azure [işlevi](https://azure.microsoft.com/services/functions/)gibi) oluşturmanız gerekir. Sunucu tarafı API 'SI çağrıldığında ve kimlik doğrulaması ve yetkilendirme doğrulandıktan sonra, çağrıyı HALSıS 'e geçirin. Bu strateji, ortadaki adam saldırıları engellemez, ancak uç noktanızı kullanıcılarınıza göre gizleme, erişimi izlemenize ve uç nokta yanıt günlüğü (örneğin [Application Insights](https://azure.microsoft.com/services/application-insights/)) eklemenize olanak tanır.  

## <a name="next-steps"></a>Sonraki adımlar

* [Sürüm oluşturma](luis-concept-version.md) kavramlarını anlayın. 
* [Anahtar oluşturmayı](luis-how-to-azure-subscription.md)öğrenin.
