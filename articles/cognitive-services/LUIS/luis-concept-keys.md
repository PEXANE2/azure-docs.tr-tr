---
title: LUIS ile yazma ve çalışma zamanı tuşları nasıl kullanılır?
titleSuffix: Azure Cognitive Services
description: LUIS, modelinizi oluşturmak için yazma anahtarı ve tahmin bitiş noktasını kullanıcı sözcükleriyle sorgulamak için çalışma zamanı anahtarı olmak üzere iki anahtar kullanır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220968"
---
# <a name="authoring-and-runtime-keys"></a>Yazma ve çalışma zamanı anahtarları

Dil Anlama (LUIS) iki hizmet ve API setleri vardır: 

* Yazma (daha önce _programlı_olarak bilinir)
* Tahmin çalışma süresi

Hangi hizmetle çalışmak istediğinize ve bu hizmetle nasıl çalışmak istediğinize bağlı olarak birkaç anahtar türü vardır.

## <a name="non-azure-resources-for-luis"></a>LUIS için Azure dışı kaynaklar

### <a name="starter-key"></a>Başlangıç tuşu

LUIS'i ilk kullanmaya başladığınızda, sizin için bir **başlangıç anahtarı** oluşturulur. Bu kaynak şunları sağlar:

* LUIS portalı veya API'ler aracılığıyla ücretsiz yetkilendirme hizmeti istekleri (SDK'lar dahil)
* tarayıcı, API veya SDK'lar aracılığıyla ayda ücretsiz 1.000 tahmin bitiş noktası isteği

## <a name="azure-resources-for-luis"></a>LUIS için azure kaynakları

<a name="programmatic-key" ></a>
<a name="endpoint-key"></a>
<a name="authoring-key"></a>

LUIS üç tür Azure kaynağına izin verir: 
 
|Anahtar|Amaç|Bilişsel hizmet`kind`|Bilişsel hizmet`type`|
|--|--|--|--|
|[Yazma anahtarı](#programmatic-key)|Yazma, eğitim, yayımlama ve sınama ile uygulama verilerine erişin ve yönetin. Luis uygulamalarını programlı bir şekilde yazarsanız bir LUIS yazma anahtarı oluşturun.<br><br>`LUIS.Authoring` Anahtarın amacı şunları yapmaktır:<br>* eğitim ve yayıncılık da dahil olmak üzere Dil Anlama uygulamaları ve modelleri, programlı yönetmek<br> * katılımcı [rolüne](#contributions-from-other-authors)kişi atayarak yazar kaynağa denetim izinleri .|`LUIS.Authoring`|`Cognitive Services`|
|[Tahmin anahtarı](#prediction-endpoint-runtime-key)| Tahmin bitiş noktası isteklerini sorgula. İstemci uygulamanız başlangıç kaynağı tarafından sağlanan 1.000 istek dışında öngörüler talep etmeden önce bir LUIS tahmin anahtarı oluşturun. |`LUIS`|`Cognitive Services`|
|[Bilişsel Hizmet çok hizmet kaynak anahtarı](../cognitive-services-apis-create-account-cli.md?tabs=windows#create-a-cognitive-services-resource)|LUIS ve diğer desteklenen Bilişsel Hizmetler ile paylaşılan sorgu tahmin bitiş noktası istekleri.|`CognitiveServices`|`Cognitive Services`|

Kaynak oluşturma işlemi tamamlandığında, anahtarı LUIS portalındaki uygulamaya [atayın.](luis-how-to-azure-subscription.md)

Yayımlamak ve sorgulamak istediğiniz [bölgelerde](luis-reference-regions.md#publishing-regions) LUIS uygulamalarını yazarsanız önemlidir.

> [!CAUTION]
> Kolaylık sağlamak için, kotasında birkaç ücretsiz tahmin uç noktası çağrıları sağladığından örneklerin çoğu [Başlangıç anahtarını](#starter-key) [kullanır.](luis-boundaries.md#key-limits)  


### <a name="query-prediction-resources"></a>Sorgu tahmin kaynakları

* Çalışma zamanı tuşu tüm LUIS uygulamalarınız veya belirli LUIS uygulamalarınız için kullanılabilir. 
* LUIS uygulamalarını yazarken çalışma zamanı anahtarını kullanmayın. 

LUIS çalışma süresi bitiş noktası, her ikisi de tahmin bitiş noktası çalışma zamanı anahtarını kullanmak, ancak farklı yerlerde sorgu iki stilleri kabul eder.

Çalışma süresine erişmek için kullanılan bitiş noktası, aşağıdaki `{region}` tabloda belirtilen kaynağınızın bölgesine özgü bir alt etki alanı kullanır. 

## <a name="assignment-of-the-key"></a>Anahtarın atanması

Çalışma zamanı anahtarını [LUIS portalında](https://www.luis.ai) veya ilgili API'ler aracılığıyla [atayabilirsiniz.](luis-how-to-azure-subscription.md) 

## <a name="key-limits"></a>Anahtar limitleri

Abonelik başına bölge başına en fazla 10 yazma anahtarı oluşturabilirsiniz. 

[Bkz. Anahtar Sınırları](luis-boundaries.md#key-limits) ve [Azure bölgeleri.](luis-reference-regions.md) 

Yayımlama bölgeleri yazma bölgelerinden farklıdır. İstemci uygulamanızın bulunmasını istediğiniz yayımlama bölgesine karşılık gelen yazarlık bölgesinde bir uygulama oluşturduğunuzdan emin olun.

## <a name="key-limit-errors"></a>Anahtar sınır hataları
Saniye başına hareket (TPS) kotanızı aşarsanız, bir HTTP 429 hatası alırsınız. Aylık işlem (TPS) kotanızı aşarsanız, bir HTTP 403 hatası alırsınız. 

## <a name="contributions-from-other-authors"></a>Diğer yazarların katkıları

**Kaynak geçirilen uygulamaları [yazarken:](luis-migration-authoring.md) ** _katkıda bulunanlar,_ **Access denetimi (IAM)** sayfasını kullanarak, yazar kaynağı için Azure portalında yönetilir. Ortak çalışanın e-posta adresini ve _katılımcı_ rolünü kullanarak [kullanıcıyı nasıl ekleyeceğinizi](luis-how-to-collaborate.md)öğrenin. 

**Henüz geçiş yapmamış uygulamalar için:** tüm _ortak çalışanlar,_ > **İşbirlikçileri** Yönet sayfasından LUIS portalında yönetilir.

## <a name="move-transfer-or-change-ownership"></a>Taşıma, aktarma veya sahipliği değiştirme

Bir uygulama, sahibinin aboneliği tarafından belirlenen Azure kaynakları tarafından tanımlanır. 

LUIS uygulamanızı taşıyabilirsiniz. Azure portalında veya Azure CLI'de aşağıdaki dokümantasyon kaynaklarını kullanın:

* [Uygulamayı LUIS yazar kaynakları arasında taşıma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Kaynağı yeni kaynak grubuna veya aboneye taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md)
* [Kaynağı aynı abonelik içinde veya abonelikler arasında taşıma](../../azure-resource-manager/management/move-limitations/app-service-move-limitations.md)

Aboneliğinizin [mülkiyetini](../../cost-management-billing/manage/billing-subscription-transfer.md) aktarmak için: 

**Göç eden kullanıcılar için - [kaynak geçirilen](luis-migration-authoring.md) uygulamaları yazar**: Kaynağın sahibi olarak, bir `contributor`.

**Henüz geçiş yapmış kullanıcılar için**: Uygulamanızı JSON dosyası olarak dışa aktarın. Başka bir LUIS kullanıcısı uygulamayı içe aktararak uygulama sahibi haline gelebilir. Yeni uygulama farklı bir uygulama kimliğine sahip olacak.  

## <a name="access-for-private-and-public-apps"></a>Özel ve genel uygulamalara erişim

**Özel** bir uygulama için, işletme zamanı erişimi sahipleri ve katkıda bulunanlar için kullanılabilir. **Genel** bir uygulama için, çalışma zamanı erişimi, kendi Azure [Bilişsel Hizmeti](../cognitive-services-apis-create-account.md) veya [LUIS](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) çalışma zamanı kaynağı olan ve ortak uygulamanın kimliğine sahip olan herkes tarafından kullanılabilir. 

Şu anda, genel uygulamaların bir kataloğu yok.

### <a name="authoring-access"></a>Erişim yazma
Uygulamaya [LUIS](luis-reference-regions.md#luis-website) portalından veya [yazarapilerden](https://go.microsoft.com/fwlink/?linkid=2092087) erişim Azure yazma kaynağı tarafından denetlenir. 

Uygulamanın sahibi ve tüm katkıda bulunanlar uygulamayı yazara erişebilir. 

|Yazma erişimi içerir|Notlar|
|--|--|
|Uç nokta tuşları ekleme veya kaldırma||
|Sürüm dışa aktarma||
|Uç nokta günlüklerini dışa aktarma||
|Sürüm alma||
|Uygulamayı herkese açık hale getirin|Bir uygulama herkese açık olduğunda, yazma veya bitiş noktası anahtarı olan herkes uygulamayı sorgulayabilir.|
|Modeli değiştirme|
|Yayımlama|
|Etkin öğrenme için uç nokta söyleyişlerini gözden [geçirin](luis-how-to-review-endpoint-utterances.md)|
|Eğitim|

<a name="prediction-endpoint-runtime-key"></a>

### <a name="prediction-endpoint-runtime-access"></a>Tahmin bitiş noktası çalışma zamanı erişimi

Tahmin bitiş noktasını sorgulamaya erişim, **Yönet** bölümündeki **Uygulama Bilgileri** sayfasındaki bir ayar tarafından denetlenir. 

|[Özel bitiş noktası](#runtime-security-for-private-apps)|[Genel uç nokta](#runtime-security-for-public-apps)|
|:--|:--|
|Sahibi ve katkıda bulunanlar tarafından kullanılabilir|Uygulama kimliğini bilen kişi, katkıda bulunanlar ve diğer herkes tarafından kullanılabilir|

LUIS çalışma zamanı anahtarınızı sunucudan sunucuya bir ortamda çağırarak kimin göreceğini kontrol edebilirsiniz. Bir bottan LUIS kullanıyorsanız, bot ve LUIS arasındaki bağlantı zaten güvenlidir. LUIS bitiş noktasını doğrudan çağırıyorsanız, kontrollü erişime [(AAD](https://azure.microsoft.com/services/active-directory/)gibi) sahip sunucu tarafında bir API (Azure [işlevi](https://azure.microsoft.com/services/functions/)gibi) oluşturmanız gerekir. Sunucu tarafındaki API çağrıldığında ve kimlik doğrulandığında ve yetkilendirme doğrulandığında, aramayı LUIS'e iletin. Bu strateji ortadaki adam saldırılarını engellemese de, anahtar ve uç nokta URL'nizi kullanıcılarınızdan gizletir, erişimi izlemenize ve bitiş noktası yanıtı günlüğe kaydetmenize olanak tanır [(Uygulama Öngörüleri](https://azure.microsoft.com/services/application-insights/)gibi).

#### <a name="runtime-security-for-private-apps"></a>Özel uygulamalar için çalışma zamanı güvenliği

Özel bir uygulamanın çalışma süresi yalnızca aşağıdakiler tarafından kullanılabilir:

|Anahtar ve kullanıcı|Açıklama|
|--|--|
|Sahibinin yazma anahtarı| 1000 uç nokta vuruşu|
|Ortak çalışan/katkıda bulunan yazar anahtarları| 1000 uç nokta vuruşu|
|Bir yazar veya ortak çalışan/katkıda bulunan kişi tarafından LUIS'e atanan herhangi bir anahtar|Anahtar kullanım katmanına göre|

#### <a name="runtime-security-for-public-apps"></a>Genel uygulamalar için çalışma zamanı güvenliği

Bir uygulama herkese açık olarak yapılandırıldıktan sonra, geçerli _bir_ LUIS yazma anahtarı veya LUIS uç noktası anahtarı, anahtar bitiş noktası kotasının tamamını kullanmadığı sürece uygulamanızı sorgulayabilir.

Sahibi veya katılımcısı olmayan bir kullanıcı, yalnızca uygulama kimliği verilirse ortak bir uygulamanın çalışma süresine erişebilir. LUIS'in halka açık bir uygulamayı aramak için bir halka açık _pazarı_ veya başka bir yolu yoktur.  

Bölge tabanlı LUIS kaynak anahtarına sahip bir kullanıcının, kaynak anahtarıyla ilişkili olduğu bölgede uygulamaya erişebilmeleri için tüm bölgelerde herkese açık bir uygulama yayınlanır.

## <a name="transfer-of-ownership"></a>Mülkiyetin devri

LUIS bir kaynağın mülkiyetini aktarma kavramına sahip değildir. 

## <a name="securing-the-endpoint"></a>Bitiş noktasının güvenliğini sağlama 

LUIS tahmin çalışma zamanı bitiş noktası anahtarınızı sunucudan sunucuya bir ortamda çağırarak kimler görebileceğini kontrol edebilirsiniz. Bir bottan LUIS kullanıyorsanız, bot ve LUIS arasındaki bağlantı zaten güvenlidir. LUIS bitiş noktasını doğrudan çağırıyorsanız, kontrollü erişime [(AAD](https://azure.microsoft.com/services/active-directory/)gibi) sahip sunucu tarafında bir API (Azure [işlevi](https://azure.microsoft.com/services/functions/)gibi) oluşturmanız gerekir. Sunucu tarafındaki API çağrıldığında ve kimlik doğrulama ve yetkilendirme doğrulandığında, aramayı LUIS'e iletin. Bu strateji ortadaki adam saldırılarını engellemese de, uç noktanızı kullanıcılarınızdan gizler, erişimi izlemenize ve bitiş noktası yanıtı günlüğe kaydetmenize olanak tanır [(Uygulama Öngörüleri](https://azure.microsoft.com/services/application-insights/)gibi).  

## <a name="next-steps"></a>Sonraki adımlar

* [Sürüm](luis-concept-version.md) kavramlarını anlayın. 
* [Anahtarları nasıl oluşturabilirsiniz](luis-how-to-azure-subscription.md)öğrenin.
