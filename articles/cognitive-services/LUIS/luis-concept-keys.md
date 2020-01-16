---
title: LUSıS ile yazma ve çalışma zamanı anahtarlarını kullanma
titleSuffix: Azure Cognitive Services
description: LUSıS iki anahtar kullanır, modelinizi oluşturmak için yazma anahtarı ve tahmin uç noktasını Kullanıcı utsları ile sorgulamak için çalışma zamanı anahtarı kullanılır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 9d213c8fa03ad2ca5e5fd7e620e52aa502749be2
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75969333"
---
# <a name="authoring-and-runtime-keys"></a>Yazma ve çalışma zamanı anahtarları

Language Understanding (LUA) iki hizmet ve API kümesine sahiptir: 

* Yazma (daha önce _programlı_olarak biliniyordu)
* Tahmin çalışma zamanı

Birlikte çalışmak istediğiniz hizmete ve bununla nasıl çalışmak istediğinize bağlı olarak birkaç anahtar türü vardır.

## <a name="non-azure-resources-for-luis"></a>LUSıS için Azure dışı kaynaklar

### <a name="starter-key"></a>Başlangıç anahtarı

LUSıS kullanmaya ilk kez başladığınızda, sizin için bir **Başlangıç anahtarı** oluşturulur. Bu kaynak şunları sağlar:

* LUıS portalı veya API 'Ler (SDK 'Lar dahil) aracılığıyla ücretsiz yazma hizmeti istekleri
* bir tarayıcı, API veya SDK aracılığıyla ayda ücretsiz 1.000 tahmin uç noktası isteği

## <a name="azure-resources-for-luis"></a>LUSıS için Azure kaynakları

<a name="programmatic-key" ></a>
<a name="endpoint-key"></a>
<a name="authoring-key"></a>

LUSıS üç tür Azure kaynağına izin verir: 
 
|Anahtar|Amaç|Bilişsel hizmet `kind`|Bilişsel hizmet `type`|
|--|--|--|--|
|[Anahtar yazma](#programmatic-key)|Yazma, eğitim, yayımlama ve test etme ile uygulama verilerine erişin ve bunları yönetin. LUSıS uygulamalarını programlı olarak yazmak istiyorsanız bir LUSıS yazma anahtarı oluşturun.<br><br>`LUIS.Authoring` anahtarının amacı şunları yapmanıza izin versağlamaktır:<br>* Eğitim ve yayımlama dahil Language Understanding uygulamaları ve modelleri program aracılığıyla yönetme<br> * [katkıda bulunan rolüne](#contributions-from-other-authors)kişi atayarak yazma kaynağına yönelik izinleri denetleyin.|`LUIS.Authoring`|`Cognitive Services`|
|[Tahmin anahtarı](#prediction-endpoint-runtime-key)| Sorgu tahmini uç noktası istekleri. İstemci uygulamanızın, başlatıcı kaynak tarafından belirtilen 1.000 isteklerin ötesinde tahmin etmek için bir LUSıS tahmin anahtarı oluşturun. |`LUIS`|`Cognitive Services`|
|[Bilişsel hizmet hizmeti çoklu hizmet kaynak anahtarı](../cognitive-services-apis-create-account-cli.md?tabs=windows#create-a-cognitive-services-resource)|LUVE diğer desteklenen bilişsel hizmetler ile paylaşılan sorgu tahmini uç noktası istekleri.|`CognitiveServices`|`Cognitive Services`|

Kaynak oluşturma işlemi tamamlandığında, anahtarı HALU portalında uygulamaya [atayın](luis-how-to-azure-subscription.md) .

LUSıS uygulamalarını yayımlamak ve sorgulamak istediğiniz [bölgelerde](luis-reference-regions.md#publishing-regions) yazmak önemlidir.

> [!CAUTION]
> Kolaylık olması için, çoğu örnek, [kotasında](luis-boundaries.md#key-limits)birkaç ücretsiz tahmin uç noktası çağrısı sağladığından [Başlangıç anahtarını](#starter-key) kullanır.  


### <a name="query-prediction-resources"></a>Sorgu tahmini kaynakları

* Çalışma zamanı anahtarı tüm LUSıS uygulamalarınız veya belirli bir LUL Apps için kullanılabilir. 
* LUSıS uygulamalarını yazmak için çalışma zamanı anahtarını kullanmayın. 

LUıN çalışma zamanı uç noktası iki sorgu stilini kabul eder, her ikisi de tahmin uç noktası çalışma zamanı anahtarını kullanır, ancak farklı yerlerde.

Çalışma zamanına erişmek için kullanılan uç nokta, aşağıdaki tabloda `{region}` belirtilen kaynak bölgesi için benzersiz olan bir alt etki alanı kullanır. 

## <a name="assignment-of-the-key"></a>Anahtar ataması

Çalışma zamanı anahtarını [LUIS portalında](https://www.luis.ai) veya Ilgili API 'ler aracılığıyla [atayabilirsiniz](luis-how-to-azure-subscription.md) . 

## <a name="key-limits"></a>Anahtar sınırları

Her abonelik için bölge başına en fazla 10 yazma anahtarı oluşturabilirsiniz. 

Bkz. [anahtar sınırları](luis-boundaries.md#key-limits) ve [Azure bölgeleri](luis-reference-regions.md). 

Yayımlama bölgeler bölge geliştirme farklıdır. İstemci uygulamanızın bulunmasını istediğiniz yayımlama bölgesine karşılık gelen yazma bölgesinde bir uygulama oluşturduğunuzdan emin olun.

## <a name="key-limit-errors"></a>Anahtar sınırı hataları
İşlem/saniye (TPS) kotasını aşarsanız bir HTTP 429 hatası alırsınız. İşlem/saat (TPS) kotasını aşarsanız bir HTTP 403 hatası alırsınız. 

## <a name="contributions-from-other-authors"></a>Diğer yazarlardan katkı

**[Kaynak geçişi](luis-migration-authoring.md) yapılan uygulamalar yazmak için**: _katkıda bulunanlar_ , **Access Control (IAM)** sayfasını kullanarak yazma kaynağı için Azure Portal yönetilir. Ortak çalışan e-posta adresini ve _katkıda_ bulunan rolünü kullanarak [bir Kullanıcı eklemeyi](luis-how-to-collaborate.md)öğrenin. 

**Henüz geçirilmeyen uygulamalar için**: tüm _ortak çalışanlar_ , **Manage-> ortak çalışanlar** sayfasından Luo portalında yönetilir.

## <a name="move-transfer-or-change-ownership"></a>Taşıma, aktarma veya sahipliği değiştirme

Bir uygulama, Azure kaynakları tarafından tanımlanır ve bu, sahibin aboneliğine göre belirlenir. 

LUSıS uygulamanızı taşıyabilirsiniz. Azure portal veya Azure CLı 'de aşağıdaki belge kaynaklarını kullanın:

* [Uygulamayı LUSıS yazma kaynakları arasında taşıma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Kaynağı yeni kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md)
* [Kaynağı aynı abonelik içinde veya abonelikler arasında taşıma](../../azure-resource-manager/management/move-limitations/app-service-move-limitations.md)

Aboneliğinizin [sahipliğini](../../cost-management-billing/manage/billing-subscription-transfer.md) aktarmak için: 

**Geçirilen [kaynak geçişi](luis-migration-authoring.md) olan uygulamalar için**: kaynak sahibi olarak, `contributor`ekleyebilirsiniz.

**Henüz geçirilmeyen kullanıcılar için**: UYGULAMANıZı bir JSON dosyası olarak dışarı aktarın. Başka bir LUO kullanıcısı uygulamayı içeri aktarabilir, böylece uygulama sahibi olur. Yeni uygulamanın farklı bir uygulama KIMLIĞI olacaktır.  

## <a name="access-for-private-and-public-apps"></a>Özel ve genel uygulamalara erişim

**Özel** bir uygulama için çalışma zamanı erişimi, sahipler ve katkıda bulunanlar için kullanılabilir. **Ortak** bir uygulama için, çalışma zamanı erişimi kendi Azure bilişsel [hizmeti](../cognitive-services-apis-create-account.md) veya [lusıs](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) çalışma zamanı kaynağı olan herkese açıktır ve ortak uygulamanın kimliğine sahiptir. 

Şu anda, ortak uygulamaların bir kataloğu yoktur.

### <a name="authoring-access"></a>Yazma erişimi
[LUIS](luis-reference-regions.md#luis-website) portalından uygulamaya erişim veya [yazma API 'leri](https://go.microsoft.com/fwlink/?linkid=2092087) , Azure yazma kaynağı tarafından denetlenir. 

Sahibi ve tüm katkıda bulunanlar, uygulamayı yazmak için erişime sahiptir. 

|Yazma erişimi içerir|Notlar|
|--|--|
|Uç nokta anahtarları Ekle Kaldır||
|Sürüm dışarı aktarma||
|Uç nokta günlükleri Dışarı Aktar||
|Sürümü içeri aktarılıyor||
|Uygulama genel yap|Uygulama genel olduğunda, herhangi bir yazma ya da uç noktası anahtarı ile uygulama sorgulayabilirsiniz.|
|Modeli Değiştir|
|Yayımlama|
|Gözden geçirme için uç nokta konuşma [etkin öğrenme](luis-how-to-review-endpoint-utterances.md)|
|Eğitin|

<a name="prediction-endpoint-runtime-key"></a>

### <a name="prediction-endpoint-runtime-access"></a>Tahmin uç noktası çalışma zamanı erişimi

Tahmin uç noktası sorgulama erişimi, **Yönet** bölümündeki **uygulama bilgileri** sayfasında bir ayarla denetlenir. 

|[Özel uç nokta](#runtime-security-for-private-apps)|[Genel bir uç nokta](#runtime-security-for-public-apps)|
|:--|:--|
|Sahip ve katkıda bulunanlar için kullanılabilir|Uygulama KIMLIĞINI bilen sahip, katkıda bulunanlar ve diğer kişiler için kullanılabilir|

Bir sunucudan sunucuya bir ortama çağırarak, LUSıS çalışma zamanı anahtarınızı kimlerin göreceğini denetleyebilirsiniz. LUIS ile bot arasındaki bağlantı zaten bir robotun LUIS kullanıyorsanız, güvenlidir. LUIS uç noktası doğrudan çağırıyorsanız, sunucu tarafı API oluşturmanız gerekir (bir Azure gibi [işlevi](https://azure.microsoft.com/services/functions/)) denetimli erişim (gibi [AAD](https://azure.microsoft.com/services/active-directory/)). Sunucu tarafı API 'SI çağrıldığında ve kimlik doğrulaması yapıldığında ve yetkilendirme doğrulandığında, çağrısı HALSıS 'e geçirin. Bu strateji, ortadaki adam saldırıları engellemez, ancak kullanıcılarınızın anahtar ve uç nokta URL 'nizi, kullanıcılarınızı izlemenizi sağlar ve uç nokta yanıt günlüğü (örneğin, [Application Insights](https://azure.microsoft.com/services/application-insights/)) eklemenize olanak tanır.

#### <a name="runtime-security-for-private-apps"></a>Özel uygulamalar için çalışma zamanı güvenliği

Özel bir uygulamanın çalışma zamanı yalnızca şunlar için kullanılabilir:

|Anahtar ve kullanıcı|Açıklama|
|--|--|
|Sahibin yazma anahtarı| En fazla 1000 uç noktası İsabeti|
|Ortak çalışan/katkıda bulunan yazma anahtarları| En fazla 1000 uç noktası İsabeti|
|LUŞUNA atanan herhangi bir anahtar, bir yazar veya ortak çalışan/katkıda bulunan|Anahtar kullanımı katmanını temel alan|

#### <a name="runtime-security-for-public-apps"></a>Ortak uygulamalar için çalışma zamanı güvenliği

Genel, bir uygulama yapılandırıldıktan sonra _herhangi_ anahtarı tüm uç nokta kota kullanmamış sürece yazma anahtarı veya LUIS uç noktası anahtarı geçerli LUIS uygulamanızı sorgulayabilir.

Sahibi veya katkıda bulunan olmayan bir Kullanıcı, uygulama KIMLIĞI verildiğinde yalnızca ortak uygulamanın çalışma zamanına erişebilir. Genel LUIS yoksa _Pazar_ veya diğer yolu genel bir uygulamayı arayın.  

LUIS kaynak bölge tabanlı anahtarı bir kullanıcıyla uygulama hangi bölge kaynak anahtarla ilişkilendirilen erişebilmesi için tüm bölgelerde genel bir uygulama yayımlanır.

## <a name="transfer-of-ownership"></a>Sahipliğin aktarılması

LUSıS 'in bir kaynağın sahipliğini aktarma kavramı yoktur. 

## <a name="securing-the-endpoint"></a>Uç noktanın güvenliğini sağlama 

Bir sunucudan sunucuya bir ortama çağırarak, LUSıS tahmin çalışma zamanı uç noktası anahtarınızı kimlerin görebileceğini denetleyebilirsiniz. LUIS ile bot arasındaki bağlantı zaten bir robotun LUIS kullanıyorsanız, güvenlidir. LUIS uç noktası doğrudan çağırıyorsanız, sunucu tarafı API oluşturmanız gerekir (bir Azure gibi [işlevi](https://azure.microsoft.com/services/functions/)) denetimli erişim (gibi [AAD](https://azure.microsoft.com/services/active-directory/)). Sunucu tarafı API olarak adlandırılır ve kimlik doğrulama ve yetkilendirme doğrulandıktan sonra LUIS açın çağrı geçirin. Bu strateji adam-de-adam saldırılarına engellemez, ancak erişimi izlemenize olanak tanır, kullanıcılarınızın uç noktanızı bilgisayardan farklı gösterir ve uç nokta yanıt günlük eklemenize izin verir (gibi [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="next-steps"></a>Sonraki adımlar

* Anlamak [sürüm](luis-concept-version.md) kavramları. 
* [Anahtar oluşturmayı](luis-how-to-azure-subscription.md)öğrenin.
