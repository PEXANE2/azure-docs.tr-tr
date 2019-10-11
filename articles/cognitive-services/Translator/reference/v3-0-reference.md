---
title: Translator Metin Çevirisi API'si V 3.0 başvurusu
titleSuffix: Azure Cognitive Services
description: Translator Metin Çevirisi API'si V 3.0 için başvuru belgeleri.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: swmachan
ms.openlocfilehash: a441ca83230a1c715aadda79683964aaab6d6213
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72252980"
---
# <a name="translator-text-api-v30"></a>Translator Metin Çevirisi API'si v 3.0

## <a name="whats-new"></a>Yenilikler

Translator Metin Çevirisi API'si sürüm 3, modern bir JSON tabanlı Web API 'SI sağlar. Mevcut özellikleri daha az işlem halinde birleştirerek kullanılabilirliği ve performansı artırır ve yeni özellikler sağlar.

 * Tek bir dildeki metni bir betikten başka bir betiğe dönüştürmek için alfabe alfabmına dönüştürme.
 * Tek bir istekte birden çok dile çeviri.
 * Tek bir istekte dil algılama, çeviri ve alfabede iyileştirme.
 * Bir terime ait alternatif çevirileri aramak için sözlük, bağlamda kullanılan terimleri gösteren geri çevirileri ve örnekleri bulun.
 * Daha bilgilendirici dil algılama sonuçları.

## <a name="base-urls"></a>Temel URL 'Ler

Microsoft Translator, birden çok veri merkezi konumundan kullanıma sunulur. Şu anda, 10 [Azure coğrafi grafları](https://azure.microsoft.com/global-infrastructure/regions)'nda bulunuyor:

* **Kuzey:** Doğu ABD, Orta Güney ABD, Orta Batı ABD ve Batı ABD 2 
* **Asya Pasifik:** Kore Güney, Japonya Doğu, Güneydoğu Asya ve Avustralya Doğu
* **Avrupa:** Kuzey Avrupa ve Batı Avrupa

Microsoft Translator Metin Çevirisi API'si istekleri, isteğin kaynaklandığı yere en yakın veri merkezi tarafından işlenen çoğu durumda bulunur. Bir veri merkezi arızası durumunda, istek Azure Coğrafya dışında yönlendirilebilir.

İsteğin belirli bir Azure Coğrafya tarafından işlenmesine zorlamak için API isteğindeki genel uç noktasını istenen bölgesel uç noktaya değiştirin:

|Açıklama|Azure Coğrafya|Taban URL 'SI|
|:--|:--|:--|
|Azure|Genel (bölgesel olmayan)|   api.cognitive.microsofttranslator.com|
|Azure|Amerika Birleşik Devletleri|   api-nam.cognitive.microsofttranslator.com|
|Azure|'Ya|  api-eur.cognitive.microsofttranslator.com|
|Azure|Asya Pasifik|    api-apc.cognitive.microsofttranslator.com|

## <a name="authentication"></a>Kimlik doğrulaması

Azure bilişsel hizmetler 'de Translator Metin Çevirisi API'si veya bilişsel [Hizmetler çoklu hizmet hizmetine](https://azure.microsoft.com/pricing/details/cognitive-services/) abone olun ve kimlik doğrulaması için abonelik anahtarınızı (Azure Portal kullanılabilir) kullanın. 

Aboneliğinizin kimliğini doğrulamak için kullanabileceğiniz üç üst bilgi vardır. Bu tablo, her birinin nasıl kullanıldığını açıklar:

|Bilgisinde|Açıklama|
|:----|:----|
|OCP-apim-Subscription-Key|*Gizli anahtarınızı geçirmektir bilişsel hizmetler aboneliğiyle kullanın*.<br/>Değer, aboneliğinizin Translator Metin Çevirisi API'si Azure gizli anahtarıdır.|
|Yetkilendirme|*Kimlik doğrulama belirteci geçirçalışıyorsanız bilişsel hizmetler aboneliğiyle kullanın.*<br/>Değer, taşıyıcı belirtecidir: `Bearer <token>`.|
|OCP-apim-Subscription-Region|*Çok sunuculu bir gizli anahtar geçirçalışıyorsanız bilişsel hizmetler çoklu hizmet aboneliği ile birlikte kullanın.*<br/>Değer, çoklu hizmet aboneliğinin bölgesidir. Bu değer, çok hizmet temelli bir abonelik kullanmadığınız zaman isteğe bağlıdır.|

###  <a name="secret-key"></a>Gizli anahtar
İlk seçenek `Ocp-Apim-Subscription-Key` üst bilgisini kullanarak kimlik doğrulaması yapmanız gerekir. @No__t-0 üst bilgisini isteğinize ekleyin.

### <a name="authorization-token"></a>Yetkilendirme belirteci
Alternatif olarak, bir erişim belirteci için gizli anahtarınızı değiş tokuş edebilirsiniz. Bu belirteç her isteğe `Authorization` üstbilgisi olarak dahildir. Yetkilendirme belirteci almak için aşağıdaki URL 'ye `POST` isteği yapın:

| Ortam     | Kimlik doğrulama hizmeti URL 'SI                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Aşağıda gizli anahtar verilen bir belirteç elde etmek için örnek istekler verilmiştir:

```curl
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'

// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Başarılı bir istek, kodlanmış erişim belirtecini yanıt gövdesinde düz metin olarak döndürür. Geçerli belirteç, çevirici hizmetine yetkilendirmede bir taşıyıcı belirteci olarak geçirilir.

```http
Authorization: Bearer <Base64-access_token>
```

Bir kimlik doğrulama belirteci 10 dakika için geçerlidir. Çevirmen API 'Lerine birden çok çağrı yapıldığında belirtecin yeniden kullanılması gerekir. Ancak, programınız Translator API 'sine uzun süre boyunca istek yapıyorsa, programınızın düzenli aralıklarla yeni bir erişim belirteci istemesi gerekir (örneğin, her 8 dakikada bir).

### <a name="multi-service-subscription"></a>Çoklu hizmet aboneliği

Son kimlik doğrulama seçeneği bilişsel hizmetin çoklu hizmet aboneliği kullanmaktır. Bu, birden çok hizmete yönelik isteklerin kimliğini doğrulamak için tek bir gizli anahtar kullanmanıza olanak sağlar. 

Birden çok hizmet gizli anahtarı kullandığınızda, isteğinize iki kimlik doğrulama üst bilgisi eklemeniz gerekir. İlk olarak gizli anahtarı geçirir, ikincisi aboneliğinizle ilişkili bölgeyi belirtir. 
* `Ocp-Apim-Subscription-Key`
* `Ocp-Apim-Subscription-Region`

Çoklu hizmet metin API 'SI aboneliği için bölge gereklidir. Seçtiğiniz bölge, çok sunuculu abonelik anahtarını kullanırken metin çevirisi için kullanabileceğiniz tek bölgedir ve Azure portal aracılığıyla çok hizmet aboneliğiniz için kaydolduğunuzda seçtiğiniz bölge olmalıdır.

Kullanılabilir bölgeler `australiaeast`, `brazilsouth`, `canadacentral`, `centralindia`, `centralus`, `centraluseuap`, `eastasia`, `eastus`, `eastus2`, `francecentral`, 0, 1, 2, 3, 4, 5, 6, 7 t-21 ve 2.

Sorgu dizesinde gizli anahtarı `Subscription-Key` parametresiyle geçirirseniz, `Subscription-Region` sorgu parametresiyle bölgeyi belirtmeniz gerekir.

Bir taşıyıcı belirteci kullanıyorsanız, belirteç uç noktası: `https://<your-region>.api.cognitive.microsoft.com/sts/v1.0/issueToken` ' dan edinmeniz gerekir.


## <a name="errors"></a>Hatalar

Standart hata yanıtı, ad/değer çifti `error` adlı bir JSON nesnesidir. Değer aynı zamanda şu özelliklere sahip bir JSON nesnesidir:

  * `code`: sunucu tanımlı bir hata kodu.
  * `message`: hatanın insan tarafından okunabilen bir temsilini sağlayan bir dize.

Örneğin, ücretsiz deneme aboneliği olan bir müşteri, ücretsiz kota tükendiğinde aşağıdaki hatayı alır:

```json
{
  "error": {
    "code":403001,
    "message":"The operation is not allowed because the subscription has exceeded its free quota."
    }
}
```
Hata kodu 3 basamaklı HTTP durum kodunu birleştiren 6 basamaklı bir sayıdır ve ardından hatayı daha fazla kategorilere ayırarak 3 basamaklı bir sayıdır. Ortak hata kodları şunlardır:

| Kod | Açıklama |
|:----|:-----|
| 400000| İstek girişlerinden biri geçerli değil.|
| 400001| "Scope" parametresi geçersiz.|
| 400002| "Category" parametresi geçersiz.|
| 400003| Bir dil tanımlayıcısı eksik veya geçersiz.|
| 400004| Hedef betik Belirleyicisi ("betiğe") eksik veya geçersiz.|
| 400005| Giriş metni eksik veya geçersiz.|
| 400006| Dil ve betiğin birleşimi geçerli değil.|
| 400018| Kaynak betik Belirleyicisi ("betikten") eksik veya geçersiz.|
| 400019| Belirtilen dillerden biri desteklenmez.|
| 400020| Giriş metni dizisindeki öğelerden biri geçerli değil.|
| 400021| API sürümü parametresi eksik veya geçersiz.|
| 400023| Belirtilen dil çiftinin biri geçerli değil.|
| 400035| Kaynak dili ("Kimden" alanı) geçerli değil.|
| 400036| Hedef dil ("to" alanı) eksik veya geçersiz.|
| 400042| Belirtilen seçeneklerden biri ("Seçenekler" alanı) geçerli değil.|
| 400043| İstemci izleme KIMLIĞI (Clienttraceıd alanı veya X-Clienttranceıd üstbilgisi) eksik veya geçersiz.|
| 400050| Giriş metni çok uzun. [İstek sınırlarını](../request-limits.md)görüntüleyin.|
| 400064| "Translation" parametresi eksik veya geçersiz.|
| 400070| Hedef betiklerin (ToScript parametresi) sayısı, hedef dillerin sayısıyla (parametreye) eşleşmiyor.|
| 400071| Değer TextType için geçerli değil.|
| 400072| Giriş metninin dizisinde çok fazla öğe vardır.|
| 400073| Betik parametresi geçerli değil.|
| 400074| İsteğin gövdesi geçerli bir JSON değil.|
| 400075| Dil çifti ve kategori birleşimi geçerli değil.|
| 400077| İstek boyutu üst sınırı aşıldı. [İstek sınırlarını](../request-limits.md)görüntüleyin.|
| 400079| Ve dilinden dile arasında çeviri için istenen özel sistem yok.|
| 400080| Söz konusu dil veya betik için alfabede desteklenmez.|
| 401000| Kimlik bilgileri eksik veya geçersiz olduğundan istek yetkilendirilmemiş.|
| 401015| "Belirtilen kimlik bilgileri konuşma API 'SI içindir. Bu istek, metin API 'SI için kimlik bilgileri gerektirir. Translator Metin Çevirisi API'si için bir abonelik kullanın. "|
| 403000| İşleme izin verilmiyor.|
| 403001| Abonelik, boş kotasını aştığından işleme izin verilmiyor.|
| 405000| İstek yöntemi istenen kaynak için desteklenmiyor.|
| 408001| İstenen çeviri sistemi hazırlanıyor. Lütfen birkaç dakika sonra yeniden deneyin.|
| 408002| Gelen akış beklenirken istek zaman aşımına uğradı. İstemci, sunucunun beklemeye hazırlandığı süre içinde bir istek üretmedi. İstemci daha sonra değişiklik yapmadan isteği yineleyebilir.|
| 415000| Content-Type üst bilgisi eksik veya geçersiz.|
| 429000, 429001, 429002| İstemci istek sınırlarını aştığından, sunucu isteği reddetti.|
| 500000| Beklenmeyen bir hata oluştu. Hata devam ederse, hatayı tarih/saat ile, X-RequestId yanıt başlığından istek tanımlayıcısını ve X-Clienttraceıd istek başlığından istemci tanımlayıcısını bildirin.|
| 503000| Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin. Hata devam ederse, hatayı tarih/saat ile, X-RequestId yanıt başlığından istek tanımlayıcısını ve X-Clienttraceıd istek başlığından istemci tanımlayıcısını bildirin.|

