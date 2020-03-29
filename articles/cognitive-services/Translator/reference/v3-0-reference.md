---
title: Çevirmen Metin API V3.0 Referans
titleSuffix: Azure Cognitive Services
description: Çevirmen Metin API V3.0 için referans belgeleri. Çevirmen Metin API Sürümü 3 modern bir JSON tabanlı Web API sağlar.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 3/13/2020
ms.author: swmachan
ms.openlocfilehash: 4180dc6127fb2d31465400b1b25fb7e2d68f4754
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79369174"
---
# <a name="translator-text-api-v30"></a>Çevirmen Metin API v3.0

## <a name="whats-new"></a>Yenilikler

Çevirmen Metin API Sürümü 3 modern bir JSON tabanlı Web API sağlar. Varolan özellikleri daha az işlemde birleştirerek kullanılabilirliği ve performansı artırır ve yeni özellikler sağlar.

 * Bir dildeki metni bir komut dosyasından başka bir komut dosyasına dönüştürmek için çeviri.
 * Tek bir istekte birden çok dile çeviri.
 * Tek bir istekte dil algılama, çeviri ve çeviri.
 * Sözlük, bir terimin alternatif çevirilerini aramak, geri çevirileri ve bağlam içinde kullanılan terimleri gösteren örnekleri bulmak için kullanılır.
 * Daha bilgilendirici dil algılama sonuçları.

## <a name="base-urls"></a>Taban URL'ler

Microsoft Translator, birden çok veri merkezi konumu dışında sunulur. Şu anda 10 [Azure coğrafyasında](https://azure.microsoft.com/global-infrastructure/regions)bulunmaktadır:

* **Amerika:** Doğu ABD, Güney Orta ABD, Batı Orta ABD ve Batı ABD 2 
* **Asya Pasifik:** Kore Güney, Japonya Doğu, Güneydoğu Asya ve Avustralya Doğu
* **Avrupa:** Kuzey Avrupa ve Batı Avrupa

Microsoft Translator Text API'ye yapılan istekler çoğu durumda, isteğin geldiği yere en yakın veri merkezi tarafından işlenir. Veri merkezi hatası olması durumunda, istek Azure coğrafyasının dışına yönlendirilebilir.

İsteğin belirli bir Azure coğrafyası tarafından ele alınmasına zorlamak için, API isteğindeki Genel bitiş noktasını istenen bölgesel bitiş noktasıyla değiştirin:

|Açıklama|Azure coğrafyası|Temel URL|
|:--|:--|:--|
|Azure|Küresel (bölgesel olmayan)|   api.cognitive.microsofttranslator.com|
|Azure|Amerika Birleşik Devletleri|   api-nam.cognitive.microsofttranslator.com|
|Azure|Avrupa|  api-eur.cognitive.microsofttranslator.com|
|Azure|Asya Pasifik|    api-apc.cognitive.microsofttranslator.com|

## <a name="authentication"></a>Kimlik doğrulaması

Azure Bilişsel Hizmetler'de Çevirmen Metin API'sine veya [Bilişsel Hizmetler çoklu hizmetine](https://azure.microsoft.com/pricing/details/cognitive-services/) abone olun ve kimlik doğrulaması için abonelik anahtarınızı (Azure portalında kullanılabilir) kullanın. 

Aboneliğinizi doğrulamak için kullanabileceğiniz üç üstbilgi vardır. Bu tablo, her birinin nasıl kullanıldığını açıklar:

|Üst bilgiler|Açıklama|
|:----|:----|
|Ocp-Apim-Subscription-Key|*Gizli anahtarınızı geçiyorsanız Bilişsel Hizmetler aboneliği ile kullanın.*<br/>Bu değer, Çevirmen Metin API aboneliğiniz için Azure gizli anahtarıdır.|
|Yetkilendirme|*Kimlik doğrulama belirteci geçiyorsanız, Bilişsel Hizmetler aboneliğini kullanın.*<br/>Değeri Taşıyıcı belirteci: `Bearer <token>`.|
|Ocp-Apim-Abonelik-Bölge|*Bilişsel Hizmetler çok hizmetli ve bölgesel çevirmen kaynağı ile kullanın.*<br/>Değer, çok hizmetli veya bölgesel çevirmen kaynağının bulunduğu bölgedir. Bu değer, genel bir çevirmen kaynağı kullanırken isteğe bağlıdır.|

###  <a name="secret-key"></a>Gizli anahtar
İlk seçenek `Ocp-Apim-Subscription-Key` üstbilgi kullanarak kimlik doğrulaması etmektir. Üstbilgi `Ocp-Apim-Subscription-Key: <YOUR_SECRET_KEY>` isteğinize ekleyin.

#### <a name="authenticating-with-a-global-resource"></a>Genel bir kaynakla kimlik doğrulama

Genel bir [çevirmen kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation)kullandığınızda, çevirmen API'sini aramak için bir üstbilgi eklemeniz gerekir.

|Üst bilgiler|Açıklama|
|:-----|:----|
|Ocp-Apim-Subscription-Key| Bu değer, Çevirmen Metin API aboneliğiniz için Azure gizli anahtarıdır.|

Burada, genel çevirmen kaynağını kullanarak Çevirmen API'sini aramak için örnek bir istek

```curl
// Pass secret key using headers
curl -X POST "https://api.cognitive.microsoft.com/translate?api-version=3.0&to=es" \
     -H "Ocp-Apim-Subscription-Key:<your-key>" \
     -H "Content-Type: application/json" \
     -d "[{'Text':'Hello, what is your name?'}]"
```

#### <a name="authenticating-with-a-regional-resource"></a>Bölgesel bir kaynakla kimlik doğrulama

Bölgesel bir [çevirmen kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation)kullandığınızda.
Çevirmen API'yi aramanız gereken 2 başlık vardır.

|Üst bilgiler|Açıklama|
|:-----|:----|
|Ocp-Apim-Subscription-Key| Bu değer, Çevirmen Metin API aboneliğiniz için Azure gizli anahtarıdır.|
|Ocp-Apim-Abonelik-Bölge| Değer, çevirmen kaynağının bölgesidir. |

Burada, bölgesel çevirmen kaynağını kullanarak Çevirmen API'sini aramak için örnek bir istek

```curl
// Pass secret key and region using headers
curl -X POST "https://api.cognitive.microsoft.com/translate?api-version=3.0&to=es" \
     -H "Ocp-Apim-Subscription-Key:<your-key>" \
     -H "Ocp-Apim-Subscription-Region:<your-region>" \
     -H "Content-Type: application/json" \
     -d "[{'Text':'Hello, what is your name?'}]"
```

#### <a name="authenticating-with-a-multi-service-resource"></a>Çok hizmet kaynağıyla kimlik doğrulaması

Bir Bilişsel Hizmet'in çok hizmet kaynağını kullandığınızda. Bu, birden çok hizmet için istekleri doğrulamak için tek bir gizli anahtar kullanmanıza olanak sağlar. 

Çok hizmetli gizli bir anahtar kullandığınızda, isteğiniz ile birlikte iki kimlik doğrulama üstadı eklemeniz gerekir. Çevirmen API'yi aramanız gereken 2 başlık vardır.

|Üst bilgiler|Açıklama|
|:-----|:----|
|Ocp-Apim-Subscription-Key| Değer, çok hizmet kaynağınızın Azure gizli anahtarıdır.|
|Ocp-Apim-Abonelik-Bölge| Değer, çok hizmet kaynağının bölgesidir. |

Çok hizmetli Metin API aboneliği için bölge gereklidir. Seçtiğiniz bölge, çok hizmetli abonelik anahtarını kullanırken metin çevirisi için kullanabileceğiniz tek bölgedir ve Azure portalı üzerinden çok hizmetli aboneliğinize kaydolurken seçtiğiniz bölge olmalıdır.

`australiaeast`Mevcut bölgeler, `brazilsouth` `canadacentral`, `centralindia` `centralus`, `centraluseuap` `eastasia` `eastus` `eastus2` `southafricanorth`, , , , , , , , , , , , , , , ve . `francecentral` `japaneast` `japanwest` `koreacentral` `northcentralus` `northeurope` `southcentralus` `southeastasia` `uksouth` `westcentralus` `westeurope` `westus` `westus2`

Parametre `Subscription-Key`ile sorgu dizesi gizli anahtarı geçerseniz, o zaman sorgu `Subscription-Region`parametresi ile bölge belirtmeniz gerekir.

### <a name="authenticating-with-an-access-token"></a>Erişim belirteciyle kimlik doğrulaması
Alternatif olarak, gizli anahtarınızı bir erişim jetonuyla değiştirebilirsiniz. Bu belirteç, `Authorization` üstbilgi olarak her istekle birlikte verilir. Yetkilendirme jetonu almak için `POST` aşağıdaki URL'ye istekte bulunun:

| Kaynak türü     | Kimlik doğrulama hizmeti URL'si                                |
|-----------------|-----------------------------------------------------------|
| Genel          | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |
| Bölgesel veya Çok Hizmetli | `https://<your-region>.api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Gizli bir anahtar verilen bir belirteç elde etmek için örnek istekleri şunlardır:

```curl
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'

// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Başarılı bir istek, kodlanmış erişim belirteci'ni yanıt gövdesinde düz metin olarak döndürür. Geçerli belirteç, Yetkilendirme'de taşıyıcı belirteci olarak Çevirmen hizmetine iletilir.

```http
Authorization: Bearer <Base64-access_token>
```

Kimlik doğrulama belirteci 10 dakika süreyle geçerlidir. Belirteç, Çevirmen API'lerine birden çok arama yapılırken yeniden kullanılmalıdır. Ancak, programınız uzun bir süre boyunca Çevirmen API'sine istekte bulunuyorsa, programınız düzenli aralıklarla (örneğin, her 8 dakikada bir) yeni bir erişim belirteci talep etmelidir.

## <a name="virtual-network-support"></a>Sanal Ağ desteği

Çevirmen hizmeti artık sınırlı bölgelerde Sanal Ağ`WestUS2`özellikleri `EastUS` `SouthCentralUS`ile `WestUS` `Central US EUAP`kullanılabilir `global`( , , , , , . Sanal Ağı etkinleştirmek için lütfen [Azure Bilişsel Hizmetleri Sanal Ağyapılandırma'ya](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-virtual-networks?tabs=portal)bakın. 

Bu özelliği açtıktan sonra, Çevirmen API'sini aramak için özel bitiş noktasını kullanmanız gerekir. Genel çevirmen bitiş noktasını ("api.cognitive.microsofttranslator.com") kullanamazsınız ve erişim belirteciyle kimlik doğrulaması yapamazsınız.

[Çevirmen kaynağını](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation)oluşturduktan sonra özel bitiş noktasını bulabilirsiniz.

|Üst bilgiler|Açıklama|
|:-----|:----|
|Ocp-Apim-Subscription-Key| Bu değer, Çevirmen Metin API aboneliğiniz için Azure gizli anahtarıdır.|
|Ocp-Apim-Abonelik-Bölge| Değer, çevirmen kaynağının bölgesidir. Kaynak isteğe bağlı`global`|

Burada özel bitiş noktasını kullanarak Çevirmen API'sini aramak için örnek bir istek

```curl
// Pass secret key and region using headers
curl -X POST "https://<your-custom-domain>.cognitiveservices.azure.com/translator/text/v3.0/translate?api-version=3.0&to=es" \
     -H "Ocp-Apim-Subscription-Key:<your-key>" \
     -H "Ocp-Apim-Subscription-Region:<your-region>" \
     -H "Content-Type: application/json" \
     -d "[{'Text':'Hello, what is your name?'}]"
```

## <a name="errors"></a>Hatalar

Standart hata yanıtı, ad/değer çifti adlı `error`bir JSON nesnesidir. Değer aynı zamanda özellikleri olan bir JSON nesnesidir:

  * `code`: Sunucu tanımlı hata kodu.
  * `message`: Hatanın insan tarafından okunabilir bir temsilini veren bir dize.

Örneğin, ücretsiz deneme aboneliği olan bir müşteri, ücretsiz kota tükendiğinde aşağıdaki hatayı alır:

```json
{
  "error": {
    "code":403001,
    "message":"The operation is not allowed because the subscription has exceeded its free quota."
    }
}
```
Hata kodu, hatayı daha fazla kategorilere ayırmak için 3 basamaklı HTTP durum kodunu ve ardından 3 basamaklı bir sayıyı birleştiren 6 basamaklı bir sayıdır. Yaygın hata kodları şunlardır:

| Kod | Açıklama |
|:----|:-----|
| 400000| İstek girişlerinden biri geçerli değildir.|
| 400001| "Kapsam" parametresi geçersizdir.|
| 400002| "Kategori" parametresi geçersizdir.|
| 400003| Bir dil belirtici eksik veya geçersiz.|
| 400004| Hedef komut dosyası belirtici ("Komut dosyasına") eksik veya geçersiz.|
| 400005| Giriş metni eksik veya geçersiz.|
| 400006| Dil ve komut dosyasının birleşimi geçerli değildir.|
| 400018| Kaynak komut dosyası belirtici ("Komut dosyasından") eksik veya geçersiz.|
| 400019| Belirtilen dillerden biri desteklenmez.|
| 400020| Giriş metni dizisindeki öğelerden biri geçerli değildir.|
| 400021| API sürüm parametresi eksik veya geçersiz.|
| 400023| Belirtilen dil çiftinden biri geçerli değil.|
| 400035| Kaynak dil ("From" alanı) geçerli değildir.|
| 400036| Hedef dil ("To" alanı) eksik veya geçersiz.|
| 400042| Belirtilen seçeneklerden biri ("Seçenekler" alanı) geçerli değildir.|
| 400043| İstemci izleme kimliği (ClientTraceId alanı veya X-ClientTranceId üstbilgi) eksik veya geçersiz.|
| 400050| Giriş metni çok uzun. [İstek sınırlarını görüntüleyin.](../request-limits.md)|
| 400064| "Çeviri" parametresi eksik veya geçersiz.|
| 400070| Hedef komut dosyası sayısı (ToScript parametresi) hedef dil sayısıyla eşleşmiyor (Parametreye).|
| 400071| Değer TextType için geçerli değildir.|
| 400072| Giriş metni dizisi çok fazla öğeye sahiptir.|
| 400073| Komut dosyası parametresi geçerli değil.|
| 400074| İsteğin gövdesi JSON geçerli değildir.|
| 400075| Dil çifti ve kategori birleşimi geçerli değildir.|
| 400077| Maksimum istek boyutu aşıldı. [İstek sınırlarını görüntüleyin.](../request-limits.md)|
| 400079| Dil arasında ve dil arasında çeviri için istenen özel sistem yok.|
| 400080| Çeviri dil veya komut dosyası için desteklenmez.|
| 401000| Kimlik bilgileri eksik veya geçersiz olduğundan istek yetkilendirilmeyecek.|
| 401015| "Sağlanan kimlik bilgileri Konuşma API'si içindir. Bu istek, Metin API'si için kimlik bilgileri gerektirir. Çevirmen Metin API aboneliği kullanın."|
| 403000| İşleme izin verilmiyor.|
| 403001| Abonelik ücretsiz kotasını aştığı için işlem izin verilmez.|
| 405000| İsteme yöntemi istenen kaynak için desteklenmez.|
| 408001| Talep edilen çeviri sistemi hazırlanıyor. Lütfen birkaç dakika içinde tekrar deneyin.|
| 408002| İstek gelen akışı beklerken zaman doldu. İstemci, sunucunun beklemeye hazır olduğu süre içinde bir istek oluşturmadı. İstemci daha sonra herhangi bir değişiklik yapmadan isteği yineleyebilir.|
| 415000| İçerik Türü üstbilgi eksik veya geçersiz.|
| 429000, 429001, 429002| İstemci istek sınırlarını aştığı için sunucu isteği reddetti.|
| 500000| Beklenmeyen bir hata oluştu. Hata devam ederse, hata tarihi/saati ile bildirin, yanıt üstbilgisi X-RequestId'den tanımlayıcı isteyin ve istek üstbilgisi X-ClientTraceId'den istemci tanımlayıcısı isteyin.|
| 503000| Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin. Hata devam ederse, hata tarihi/saati ile bildirin, yanıt üstbilgisi X-RequestId'den tanımlayıcı isteyin ve istek üstbilgisi X-ClientTraceId'den istemci tanımlayıcısı isteyin.|

## <a name="metrics"></a>Ölçümler 
Ölçümler, aşağıdaki ekran görüntüsünde gösterildiği gibi, Azure portalında çevirmen kullanımı ve kullanılabilirlik bilgilerini ölçümler bölümü altında görüntülemenize olanak tanır. Daha fazla bilgi için [Bkz. Veri ve platform ölçümleri.](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics)

![Çevirmen Ölçümleri](../media/translatormetrics.png)

Bu tablo, çeviri API çağrılarını izlemek için nasıl kullanıldıklarına eşleyen kullanılabilir ölçümleri listeler.

| Ölçümler | Açıklama |
|:----|:-----|
| Toplam Aramalar| Toplam API arama sayısı.|
| ToplamTokenAramalar| Kimlik doğrulama belirteci kullanarak belirteç hizmeti aracılığıyla toplam API çağrısı sayısı.|
| Başarılı Aramalar| Başarılı aramaların sayısı.|
| Toplam Hatalar| Hata yanıtı içeren arama ların sayısı.|
| Engellenen Aramalar| Oran veya kota sınırını aşan arama ların sayısı.|
| Sunucu Hataları| Sunucu iç hatası olan arama ların sayısı(5XX).|
| İstemci Hataları| İstemci tarafı hatası olan arama ların sayısı(4XX).|
| Gecikme süresi| İsteğin milisaniye cinsinden tamamlanması için süre.|
| KarakterlerÇevrilen| Gelen metin isteğindeki toplam karakter sayısı.|
