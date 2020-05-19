---
title: Translator v 2.0
titleSuffix: Azure Cognitive Services
description: Translator v 2.0 için başvuru belgeleri.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/15/2018
ms.author: swmachan
ms.openlocfilehash: 7fa148579e7525933d388b8a93c9a3476f473cb6
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588624"
---
# <a name="translator-v20"></a>Translator v 2.0

> [!IMPORTANT]
> Çeviricisinin bu sürümü kullanımdan kaldırılmıştır. [Çeviricisinin 3. sürümü için belgeleri görüntüleyin](v3-0-reference.md).

Çeviricisinin 2. sürümü, çok dilli kullanıcı deneyimleri sağlamak için uygulamalarınız, Web siteleriniz, araçlarınızla veya diğer çözümlerle sorunsuzca tümleştirilebilir. Endüstri standartlarına göre, dil çevirisini ve metin okuma ve metin okuma gibi diğer dille ilgili görevleri gerçekleştirmek için herhangi bir donanım platformunda ve herhangi bir işletim sistemiyle kullanabilirsiniz. Daha fazla bilgi için bkz. [Translator](../translator-info-overview.md).

## <a name="getting-started"></a>Başlarken
Çevirmene erişmek için [Microsoft Azure kaydolmanız](../translator-text-how-to-signup.md)gerekir.

## <a name="authentication"></a>Kimlik Doğrulaması 
Çevirmene yapılan tüm çağrılar, kimlik doğrulaması için bir abonelik anahtarı gerektirir. API üç kimlik doğrulama yöntemini destekler:

- Erişim belirteci. Kimlik doğrulama hizmetine bir POST isteği yaparak bir erişim belirteci oluşturmak için abonelik anahtarını kullanın. Ayrıntılar için belirteç hizmeti belgelerine bakın. `Authorization`Üst bilgi veya sorgu parametresini kullanarak erişim belirtecini Translator hizmetine geçirin `access_token` . Erişim belirteci 10 dakika için geçerlidir. 10 dakikada bir yeni bir erişim belirteci alın ve 10 dakika boyunca yinelenen istekler için aynı erişim belirtecini kullanmaya devam edin.
- Doğrudan kullanılan bir abonelik anahtarı. Abonelik anahtarınızı, `Ocp-Apim-Subscription-Key` çevirmene isteğinizle birlikte dahil edilen üst bilgide bir değer olarak geçirin. Abonelik anahtarını doğrudan kullandığınızda, bir erişim belirteci oluşturmak için belirteç kimlik doğrulama hizmetini çağırmanız gerekmez.
- Azure bilişsel [Hizmetler çoklu hizmet aboneliği](https://azure.microsoft.com/pricing/details/cognitive-services/). Bu yöntem, birden fazla hizmete yönelik isteklerin kimliğini doğrulamak için tek bir gizli anahtar kullanmanıza olanak sağlar.
Çoklu hizmet gizli anahtarı kullandığınızda, isteğinize iki kimlik doğrulama üst bilgisi eklemeniz gerekir. İlk üstbilgi gizli anahtarı geçirir. İkinci üstbilgi, aboneliğinizle ilişkili bölgeyi belirtir:
   - `Ocp-Apim-Subscription-Key`
   - `Ocp-Apim-Subscription-Region`

Çoklu hizmet metin API 'SI aboneliği için bölge gereklidir. Seçtiğiniz bölge, çok servis abonelik anahtarını kullandığınızda metin çevirisi için kullanabileceğiniz tek bölgedir. Azure portal, çoklu hizmet aboneliğiniz için kaydolduğunuzda seçtiğiniz bölge ile aynı olması gerekir.

Kullanılabilir bölgeler,,,,,,, `australiaeast` `brazilsouth` `canadacentral` `centralindia` `centraluseuap` `eastasia` `eastus` `eastus2` , `japaneast` , `northeurope` , `southcentralus` , `southeastasia` , `uksouth` , `westcentralus` , `westeurope` , `westus` , ve `westus2` .

Abonelik anahtarınız ve erişim belirteciniz, görünümden gizlenmesi gereken gizli dizlardır.

## <a name="profanity-handling"></a>Küfür işleme
Normalde, çevirmen hizmeti kaynakta bulunan küfür 'ı korur. Küfür derecesi ve küfürlü sözcüklerini yapan bağlam, kültüre göre farklılık gösterir. Bu nedenle, hedef dildeki küfür derecesi artırılabilir veya azaltılabilir.

Kaynak metinde olsa bile küfür 'ın çeviride oluşmasını engellemek isterseniz, onu destekleyen yöntemler için küfür filtreleme seçeneğini kullanabilirsiniz. Seçeneği, uygunsuz küfür 'ın silinip silinmeyeceğini veya uygun etiketlere göre işaretlenip işaretlenmediğini veya hedefte küfür izin verip vermeyeceğinizi seçmenizi sağlar. Kabul edilen değerler `ProfanityAction` `NoAction` (varsayılan), `Marked` ve `Deleted` .


|ProfanityAction    |Eylem |Örnek kaynak (Japonca)  |Örnek çeviri (Ingilizce)  |
|:--|:--|:--|:--|
|NoAction   |Varsayılan. Seçeneği ayarlamaya benzer. Küfür kaynaktan hedefe geçirilecek.        |彼はジャッカスです.     |Bu bir Jackass.   |
|İm     |Küfürlü kelimeler, XML etiketleri \< küfür> ve \< /küfür> çevrelenebilir.       |彼はジャッカスです. |\<Jackass \< /küfür> bir küfür>.  |
|Silindi    |Küfürlü sözcükler, değişiklik yapılmadan çıkışta kaldırılacak.     |彼はジャッカスです. |Bir.   |

    
## <a name="excluding-content-from-translation"></a>İçeriği çeviriye dışlama
HTML () gibi etiketlerle içerik çevirdığınızda `contentType=text/html` , bazı durumlarda belirli içerikleri çevirinin dışında tutmak yararlı olur. Özniteliğini, `class=notranslate` özgün dilinde kalması gereken içeriği belirtmek için kullanabilirsiniz. Aşağıdaki örnekte, ilk `div` öğedeki içerik çevrilmeyecektir, ancak ikinci `div` öğedeki içerik çevrilecek.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>/Translate al

### <a name="implementation-notes"></a>Uygulama notları
Bir metin dizesini bir dilden diğerine çevirir.

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/Translate` .

**Dönüş değeri:** Çevrilmiş metni temsil eden bir dize.

Daha önceden kullandıysanız `AddTranslation` veya `AddTranslationArray` aynı kaynak cümlesi için 5 veya daha yüksek bir dereceye sahip bir çeviri girerseniz, `Translate` yalnızca sisteminiz için kullanılabilir olan en üst seçimi döndürür. "Aynı kaynak cümlesi", bir tümcenin sonundaki büyük harfler, boşluk, etiket değerleri ve noktalama işaretleri dışında tam olarak aynı (%100 eşleşme) anlamına gelir. Bir derecelendirme 5 veya üzeri bir derecelendirmeden depolanmıyorsa, döndürülen sonuç Microsoft Translator tarafından otomatik çeviri olacaktır.

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)

string

Yanıt içerik türü: Application/XML

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama    |Parametre türü|veri türü|
|:--|:--|:--|:--|:--|
|AppID  |olmamalıdır    |Gereklidir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `appid` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .|sorgu|string|
|metin|olmamalıdır   |Gereklidir. Çevrilecek metni temsil eden bir dize. Metin 10.000 karakterden fazlasını içeremez.|sorgu|string|
|Kaynak|olmamalıdır   |İsteğe bağlı. Çevrilen metnin dil kodunu temsil eden bir dize. Örneğin, Ingilizce için en.|sorgu|string|
|-|olmamalıdır |Gereklidir. Metni çevirecek dilin kodunu temsil eden bir dize.|sorgu|string|
|contentType|olmamalıdır    |İsteğe bağlı. Çevrilen metnin biçimi. Desteklenen biçimler `text/plain` (varsayılan) ve `text/html` . Herhangi bir HTML öğesinin düzgün biçimlendirilmiş, tam öğeler olması gerekir.|sorgu|string|
|category|olmamalıdır   |İsteğe bağlı. Çevirinin kategorisini (etki alanı) içeren bir dize. Varsayılan değer: `general`.|sorgu|string|
|Yetkilendirme|olmamalıdır  |Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .|üst bilgi|string|
|Ocp-Apim-Subscription-Key|olmamalıdır  |Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|


### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400    |Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="post-translatearray"></a>POST/TranslateArray

### <a name="implementation-notes"></a>Uygulama notları
Birden çok kaynak metin için çevirileri alır.

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray` .

İstek gövdesinin biçimi şöyledir:

```
<TranslateArrayRequest>
  <AppId />
  <From>language-code</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" />
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</TranslateArrayRequest>
```

Bu öğeler şunlardır `TranslateArrayRequest` :


* `AppId`İstenir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `AppId` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .
* `From`Seçim. Çevrilen metnin dil kodunu temsil eden bir dize. Bu alan boş bırakılırsa, yanıt otomatik dil algılamanın sonucunu dahil eder.
* `Options`Seçim. `Options`Aşağıdaki değerleri içeren bir nesne. Bunların tümü isteğe bağlıdır ve varsayılan olarak en sık kullanılan ayarlara sahiptir. Belirtilen öğelerin alfabetik sırada listelenmesi gerekir.
    - `Category`: Çevirinin kategorisini (etki alanı) içeren bir dize. Varsayılan değer: `general`.
    - `ContentType`: Çevrilmekte olan metnin biçimi. Desteklenen biçimler `text/plain` (varsayılan), `text/xml` ve `text/html` . Herhangi bir HTML öğesinin düzgün biçimlendirilmiş, tam öğeler olması gerekir.
    - `ProfanityAction`: Daha önce açıklandığı gibi profanities nasıl işleneceğini belirtir. Kabul edilen değerler `NoAction` (varsayılan), `Marked` , ve `Deleted` .
    - `State`: İstek ve yanıtı ilişkilendirmenize yardımcı olmak için Kullanıcı durumu. Yanıtta aynı içerik döndürülecek.
    - `Uri`: Sonuçları bu URI 'ye göre filtreleyin. Varsayılan: `all`.
    - `User`: Sonuçları bu kullanıcıya göre filtreleyin. Varsayılan: `all`.
* `Texts`İstenir. Çeviri metnini içeren bir dizi. Tüm dizeler aynı dilde olmalıdır. Çevrilecek tüm metinlerin toplamı 10.000 karakteri aşamaz. Dizi öğesi sayısı üst sınırı 2.000 ' dir.
* `To`İstenir. Metni çevirecek dilin kodunu temsil eden bir dize.

İsteğe bağlı öğeleri atlayabilirsiniz. Doğrudan alt öğeleri olan öğelerin `TranslateArrayRequest` alfabetik sırada listelenmesi gerekir.

`TranslateArray`Yöntemi, `application/xml` veya için kabul eder `text/xml` `Content-Type` .

**Dönüş değeri:** Bir `TranslateArrayResponse` dizi. Her biri `TranslateArrayResponse` Şu öğelere sahiptir:

* `Error`: Bir hata oluşursa hatayı gösterir. Aksi takdirde null olarak ayarlayın.
* `OriginalSentenceLengths`: Kaynak metindeki her tümcenin uzunluğunu gösteren bir tamsayılar dizisi. Dizinin uzunluğu, Tümcelerin sayısını gösterir.
* `TranslatedText`: Çevrilmiş metin.
* `TranslatedSentenceLengths`: Çevrilmiş metindeki her tümcenin uzunluğunu gösteren bir tamsayılar dizisi. Dizinin uzunluğu, Tümcelerin sayısını gösterir.
* `State`: İstek ve yanıtı ilişkilendirmenize yardımcı olmak için Kullanıcı durumu. İstekle aynı içeriği döndürür.

İşte yanıt gövdesinin biçimi:

```
<ArrayOfTranslateArrayResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <TranslateArrayResponse>
    <From>language-code</From>
    <OriginalTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </OriginalTextSentenceLengths>
    <State/>
    <TranslatedText>string-value</TranslatedText>
    <TranslatedTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </TranslatedTextSentenceLengths>
  </TranslateArrayResponse>
</ArrayOfTranslateArrayResponse>
```

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)
Başarılı bir yanıt `TranslateArrayResponse` , daha önce açıklanan biçimdeki dizi dizilerini içerir.

string

Yanıt içerik türü: Application/XML

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme|olmamalıdır  |Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .|üst bilgi|string|
|Ocp-Apim-Subscription-Key|olmamalıdır|Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu   |Nedeni|
|:--|:--|
|400    |Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin. Yaygın hatalar şunlardır: <ul><li>Dizi öğesi boş olamaz.</li><li>Geçersiz kategori.</li><li>Dil, geçersiz.</li><li>Dil geçersiz.</li><li>İstek çok fazla öğe içeriyor.</li><li>Kimden dili desteklenmiyor.</li><li>To dili desteklenmiyor.</li><li>Çeviri Isteğinde çok fazla veri yok.</li><li>HTML doğru biçimde değil.</li><li>Çeviri Isteğine çok fazla sayıda dize geçirildi.</li></ul>|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="post-getlanguagenames"></a>POST/GetLanguageNames

### <a name="implementation-notes"></a>Uygulama notları
Geçirilen dile yerelleştirilmiş, parametresi olarak geçirilen dillerin kolay adlarını alır `languageCodes` `locale` .

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames` .

İstek gövdesi, kolay adların alınacağı ISO 639-1 dil kodlarını temsil eden bir dize dizisi içerir. İşte bir örnek:

```
<ArrayOfstring xmlns:i="https://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**Dönüş değeri:** Çeviri hizmeti tarafından desteklenen dil adlarını içeren, istenen dile yerelleştirilmiş bir dize dizisi.

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)
İstenen dile yerelleştirilmiş, çevirmen hizmeti tarafından desteklenen dil adlarını içeren bir dize dizisi.

string

Yanıt içerik türü: Application/XML
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|AppID|olmamalıdır|Gereklidir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `appid` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .|sorgu|string|
|locale|olmamalıdır |Gereklidir. Dil adlarını yerelleştirmek için kullanılan aşağıdakilerden birini temsil eden bir dize: <ul><li>ISO 639 2-bir dille ilişkili küçük harfli bir kültür kodu ve ISO 3166 2-Letter büyük harf alt kültür kodu birleşimi. <li>Bir ISO 639 küçük harfli kültür kodu.|sorgu|string|
|Yetkilendirme|olmamalıdır  |Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .|üst bilgi|string|
|Ocp-Apim-Subscription-Key|olmamalıdır  |Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400    |Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="get-getlanguagesfortranslate"></a>/GetLanguagesForTranslate al

### <a name="implementation-notes"></a>Uygulama notları
Çeviri hizmeti tarafından desteklenen dilleri temsil eden dil kodlarının bir listesini alır.  `Translate`ve `TranslateArray` Bu dillerin ikisi arasında çeviri yapabilir.

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate` .

**Dönüş değeri:** Translator hizmeti tarafından desteklenen dil kodlarını içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)
Translator hizmeti tarafından desteklenen dil kodlarını içeren bir dize dizisi.

string

Yanıt içerik türü: Application/XML
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|AppID|olmamalıdır|Gereklidir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `appid` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .|sorgu|string|
|Yetkilendirme|olmamalıdır  |Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .|üst bilgi|string|
|Ocp-Apim-Subscription-Key|olmamalıdır|Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400    |Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="get-getlanguagesforspeak"></a>/GetLanguagesForSpeak al

### <a name="implementation-notes"></a>Uygulama notları
Konuşma birleştirme için kullanılabilen dilleri alır.

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak` .

**Dönüş değeri:** Konuşma sen, çeviri hizmeti tarafından desteklenen dil kodlarını içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)
Konuşma sen, çeviri hizmeti tarafından desteklenen dil kodlarını içeren bir dize dizisi.

string

Yanıt içerik türü: Application/XML

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|AppID|olmamalıdır|Gereklidir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `appid` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .|sorgu|string|
|Yetkilendirme|olmamalıdır|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .|üst bilgi|string|
|Ocp-Apim-Subscription-Key|olmamalıdır|Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|
 
### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400|Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401|Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="get-speak"></a>/Konuş al

### <a name="implementation-notes"></a>Uygulama notları
İstenen dilde konuşulan, geçirilen metnin bir WAV veya MP3 akışını döndürür.

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/Speak` .

**Dönüş değeri:** İstenen dilde konuşulan, geçirilen metnin bir WAV veya MP3 akışı.

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)

ikili

Yanıt içerik türü: Application/XML

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|AppID|olmamalıdır|Gereklidir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `appid` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .|sorgu|string|
|metin|olmamalıdır   |Gereklidir. Belirtilen dilde akış için söylenen bir veya daha fazla cümle içeren bir dize. Metnin 2.000 karakteri aşmaması gerekir.|sorgu|string|
|language|olmamalıdır   |Gereklidir. Metnin konuşmasını istediğiniz dilin desteklenen dil kodunu temsil eden bir dize. Kod, yöntemi tarafından döndürülen kodlardan biri olmalıdır `GetLanguagesForSpeak` .|sorgu|string|
|biçim|olmamalıdır|İsteğe bağlı. Content-Type KIMLIĞINI belirten bir dize. Şu anda `audio/wav` ve `audio/mp3` kullanılabilir. Varsayılan değer: `audio/wav`.|sorgu|string|
|seçenekler|olmamalıdır    |İsteğe bağlı. Sentezlenmiş konuşmanın özelliklerini belirten bir dize:<ul><li>`MaxQuality`ve `MinSize` ses sinyalinin kalitesini belirtin. `MaxQuality`en yüksek kaliteyi sağlar. `MinSize`en küçük dosya boyutunu sağlar. Varsayılan değer `MinSize` .</li><li>`female`ve `male` sesin istenen cinsiyetini belirtin. Varsayılan değer: `female`. <code>\|</code>Birden çok seçenek dahil etmek için dikey çubuğu () kullanın. Örneğin, `MaxQuality|Male`.</li></li></ul>  |sorgu|string|
|Yetkilendirme|olmamalıdır|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .|üst bilgi|string|
|Ocp-Apim-Subscription-Key|olmamalıdır  |Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400    |Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="get-detect"></a>/Detect al

### <a name="implementation-notes"></a>Uygulama notları
Metnin bir bölümünün dilini tanımlar.

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/Detect` .

**Dönüş değeri:** Metin için iki karakterli bir dil kodu içeren bir dize.

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)

string

Yanıt içerik türü: Application/XML

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|AppID|olmamalıdır  |Gereklidir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `appid` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .|sorgu|string|
|metin|olmamalıdır|Gereklidir. Dili tanımlanabilecek metni içeren bir dize. Metnin 10.000 karakteri aşmaması gerekir.|sorgu|  string|
|Yetkilendirme|olmamalıdır|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .|üst bilgi|string|
|Ocp-Apim-Subscription-Key  |olmamalıdır    |Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400|Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|


## <a name="post-detectarray"></a>POST/DetectArray

### <a name="implementation-notes"></a>Uygulama notları

Dizeler dizisindeki dilleri tanımlar. Bağımsız olarak her bir dizi öğesinin dilini algılar ve dizinin her satırı için bir sonuç döndürür.

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/DetectArray` .

İstek gövdesinin biçimi şöyledir:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

Metin 10.000 karakterden uzun olamaz.

**Dönüş değeri:** Giriş dizisindeki her satır için iki karakterli bir dil kodu içeren bir dize dizisi.

İşte yanıt gövdesinin biçimi:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)
`DetectArray`başarılı oldu. Giriş dizisinin her satırı için iki karakterli bir dil kodu içeren bir dize dizisi döndürür.

string

Yanıt içerik türü: Application/XML
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|AppID|olmamalıdır|Gereklidir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `appid` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .|sorgu|string|
|Yetkilendirme|olmamalıdır|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .|üst bilgi|string|
|Ocp-Apim-Subscription-Key|olmamalıdır|Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400    |Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="get-addtranslation"></a>/AddTranslation 'i al

### <a name="implementation-notes"></a>Uygulama notları

> [!IMPORTANT]
> **Kullanımdan kaldırma notumu:** Bu yöntem 31 Ocak 2018 ' den sonra yeni tümce gönderilerini kabul etmez. Bir hata iletisi alırsınız. Lütfen Işbirliğine dayalı çeviri çerçevesindeki (CTF) değişiklikler hakkında duyurusuna bakın.

Çeviri belleğine bir çeviri ekler.

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation` .

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)

string

Yanıt içerik türü: uygulama: XML
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü   |
|:--|:--|:--|:--|:--|
|AppID|olmamalıdır|Gereklidir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `appid` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .|sorgu|string|
|originalText|olmamalıdır|Gereklidir. Çevrilecek metni içeren bir dize. Dizenin uzunluk üst sınırı 1.000 karakterdir.|sorgu|string|
|translatedText|olmamalıdır |Gereklidir. Hedef dile çevrilmiş metin içeren bir dize. Dizenin uzunluk üst sınırı 2.000 karakterdir.|sorgu|string|
|Kaynak|olmamalıdır   |Gereklidir. Metnin özgün dilinin dil kodunu temsil eden bir dize. Örneğin, en Ingilizce ve Almanca için de.|sorgu|string|
|-|olmamalıdır|Gereklidir. Metni çevirecek dilin dil kodunu temsil eden bir dize.|sorgu|string|
|rating|olmamalıdır |İsteğe bağlı. Dize için kalite derecesini temsil eden bir tamsayı. Değer-10 ile 10 arasındadır. Varsayılan değer 1'dir.|sorgu|integer|
|contentType|olmamalıdır    |İsteğe bağlı. Çevrilen metnin biçimi. Desteklenen biçimler `text/plain` ve ' dir `text/html` . Herhangi bir HTML öğesinin düzgün biçimlendirilmiş, tam öğeler olması gerekir.    |sorgu|string|
|category|olmamalıdır|İsteğe bağlı. Çevirinin kategorisini (etki alanı) içeren bir dize. Varsayılan değer: `general`.|sorgu|string|
|kullanıcı|olmamalıdır|Gereklidir. Gönderimi kaynağını izlemek için kullanılan bir dize.|sorgu|string|
|kullanılmamışsa|olmamalıdır|İsteğe bağlı. Çevirinin içerik konumunu içeren bir dize.|sorgu|string|
|Yetkilendirme|olmamalıdır|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .  |üst bilgi|string|
|Ocp-Apim-Subscription-Key|olmamalıdır|Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400    |Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|410|`AddTranslation`artık desteklenmiyor.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="post-addtranslationarray"></a>/AddTranslationArray SONRASı

### <a name="implementation-notes"></a>Uygulama notları

> [!IMPORTANT]
> **Kullanımdan kaldırma notumu:** Bu yöntem 31 Ocak 2018 ' den sonra yeni tümce gönderilerini kabul etmez. Bir hata iletisi alırsınız. Lütfen Işbirliğine dayalı çeviri çerçevesindeki (CTF) değişiklikler hakkında duyurusuna bakın.

Çeviri belleğine bir çeviri dizisi ekler. Bu yöntem, öğesinin bir dizi sürümüdür `AddTranslation` .

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray` .

İstek gövdesinin biçimi şöyledir:

```
<AddtranslationsRequest>
  <AppId></AppId>
  <From>A string containing the language code of the source language</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
  </Options>
  <To>A string containing the language code of the target language</To>
  <Translations>
    <Translation xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
      <OriginalText>string-value</OriginalText>
      <Rating>int-value</Rating>
      <Sequence>int-value</Sequence>
      <TranslatedText>string-value</TranslatedText>
    </Translation>
  </Translations>
</AddtranslationsRequest>
```

Bu öğeler şunlardır `AddtranslationsRequest` :

* `AppId`İstenir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `AppId` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .
* `From`İstenir. Kaynak dilin dil kodunu içeren bir dize. Yöntemi tarafından döndürülen dillerden biri olmalıdır `GetLanguagesForTranslate` .
* `To`İstenir. Hedef dilin dil kodunu içeren bir dize. Yöntemi tarafından döndürülen dillerden biri olmalıdır `GetLanguagesForTranslate` .
* `Translations`İstenir. Çeviri belleğine eklenecek bir çeviri dizisi. Her çeviri,, `OriginalText` `TranslatedText` ve içermelidir `Rating` . Her birinin en büyük boyutu `OriginalText` `TranslatedText` 1.000 karakterdir. Tüm `OriginalText` ve `TranslatedText` öğelerinin toplamı 10.000 karakteri aşamaz. Dizi öğesi sayısı üst sınırı 100 ' dir.
* `Options`İstenir. ,, Ve dahil olmak üzere seçenekler kümesi `Category` `ContentType` `Uri` `User` . `User` gereklidir. `Category`, `ContentType` ve `Uri` isteğe bağlıdır. Belirtilen öğelerin alfabetik sırada listelenmesi gerekir.

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)
`AddTranslationArray`Yöntem başarılı oldu. 

31 Ocak 2018 ' den sonra cümle gönderimleri kabul edilmez. Hizmet 410 hata koduyla yanıt verir.

string

Yanıt içerik türü: Application/XML
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme|olmamalıdır|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .|üst bilgi|string|
|Ocp-Apim-Subscription-Key|olmamalıdır|Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400    |Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|410    |`AddTranslation`artık desteklenmiyor.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="get-breaksentences"></a>/Breakcümleler al

### <a name="implementation-notes"></a>Uygulama notları
Metnin bir bölümünü cümlelere böler ve her tümcenin uzunluklarını içeren bir dizi döndürür.

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences` .

**Dönüş değeri:** Tümcelerin uzunluklarının temsil eden tamsayılar dizisi. Dizinin uzunluğu, Tümcelerin sayısını temsil eder. Değerler her tümcenin uzunluğunu temsil eder.

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)
Tümcelerin uzunluklarının temsil eden tamsayılar dizisi. Dizinin uzunluğu, Tümcelerin sayısını temsil eder. Değerler her tümcenin uzunluğunu temsil eder.

integer

Yanıt içerik türü: Application/XML

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|AppID|olmamalıdır  |Gereklidir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `appid` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .|sorgu| string|
|metin|olmamalıdır   |Gereklidir. Cümlelere bölünecek metni temsil eden bir dize. Metnin en büyük boyutu 10.000 karakterdir.|sorgu|string|
|language   |olmamalıdır    |Gereklidir. Giriş metninin dil kodunu temsil eden bir dize.|sorgu|string|
|Yetkilendirme|olmamalıdır|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .   |üst bilgi|string|
|Ocp-Apim-Subscription-Key|olmamalıdır|Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400|Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401|Geçersiz kimlik bilgileri.|
|500|Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="post-gettranslations"></a>POST/GetTranslations

### <a name="implementation-notes"></a>Uygulama notları
Mağazadan ve MT altyapısından belirli bir dil çiftinin çeviri dizisini alır. `GetTranslations``Translate`, tüm kullanılabilir çevirileri döndürdüğünden öğesinden farklıdır.

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations` .

İsteğin gövdesi, `TranslationOptions` bu biçime sahip olan isteğe bağlı nesneyi içerir:

```
<TranslateOptions xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
  <Category>string-value</Category>
  <ContentType>text/plain</ContentType>
  <ReservedFlags></ReservedFlags>
  <State>int-value</State>
  <Uri>string-value</Uri>
  <User>string-value</User>
</TranslateOptions>
```

`TranslateOptions`Nesnesi aşağıdaki listedeki değerleri içerir. Bunların tümü isteğe bağlıdır ve varsayılan olarak en sık kullanılan ayarlara sahiptir. Belirtilen öğelerin alfabetik sırada listelenmesi gerekir.

* `Category`: Çevirinin kategorisini (etki alanı) içeren bir dize. Varsayılan değer: `general`.
* `ContentType`: Desteklenen tek seçenek ve varsayılan, ' dir `text/plain` .
* `IncludeMultipleMTAlternatives`: MT altyapısından birden fazla alternatif döndürülüp döndürülmeyeceğini belirten bir Boole bayrağı. Geçerli değerler `true` ve `false` (büyük/küçük harfe duyarlı). Varsayılan olarak, `false` yalnızca bir alternatif döndürür. Bayrağını, birlikte çalışan `true` çeviri çerçevesiyle (CTF) tamamen tümleşik olan yapay alternatifler oluşturmayı sağlar. Özelliği, kod çözücünün *n*en iyi listesinden yapay alternatifler ekleyerek CTF 'de çevirisi olmayan cümleler için alternatifleri döndürmeyi mümkün.
    - Lendir. Derecelendirmeler aşağıdaki gibi uygulanır: 
         - En iyi otomatik çeviri, 5 derecesine sahiptir.
       - CTF 'nin alternatifleri gözden geçirenin yetkilisini yansıtır. -10 ile + 10 arasında değişir.
       - Otomatik olarak oluşturulan (*n*-en iyi) çeviri alternatifleri, 0 derecelendirmesine ve 100 eşleşme derecesine sahiptir.
    - Alternatifin sayısı. Döndürülen alternatiflere ilişkin sayı ' de belirtilen değer kadar yüksek olabilir `maxTranslations` , ancak daha az olabilir.
    - Dil çiftleri. Bu işlev, Basitleştirilmiş Çince ve geleneksel Çince arasında her iki yönde de çeviri için kullanılamaz. Microsoft Translator tarafından desteklenen tüm diğer dil çiftleri için kullanılabilir.
* `State`: İstek ve yanıtı ilişkilendirmenize yardımcı olmak için Kullanıcı durumu. Yanıtta aynı içerik döndürülecek.
* `Uri`: Sonuçları bu URI 'ye göre filtreleyin. Değer ayarlanmamışsa, varsayılan olur `all` .
* `User`: Sonuçları bu kullanıcıya göre filtreleyin. Değer ayarlanmamışsa, varsayılan olur `all` .

İstek `Content-Type` olmalıdır `text/xml` .

**Dönüş değeri:** Yanıtın biçimi şöyledir:

```
<GetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <From>Two character language code</From>
  <State/>
  <Translations>
    <TranslationMatch>
      <Count>int-value</Count>
      <MatchDegree>int-value</MatchDegree>
      <MatchedOriginalText/>
      <Rating>int value</Rating>
      <TranslatedText>string-value</TranslatedText>
    </TranslationMatch>
  </Translations>
</GetTranslationsResponse>
```

Bu yanıt `GetTranslationsResponse` , aşağıdaki değerleri içeren bir öğesi içerir:

* `Translations`: Nesneler içinde depolanan eşleşmelerin bir dizisi `TranslationMatch` (aşağıdaki bölümde açıklanmıştır). Çeviriler orijinal metnin hafif türevlerini (belirsiz eşleşme) içerebilir. Çeviriler sıralanacaktır: ilk olarak %100 eşleşme, belirsiz eşleşme ileri.
* `From`: Yöntem bir dil belirtmezse `From` , bu değer otomatik dil algılamada gelir. Aksi halde, belirtilen `From` dil olacaktır.
* `State`: İstek ve yanıtı ilişkilendirmenize yardımcı olmak için Kullanıcı durumu. Parametresinde sağlanan değeri içerir `TranslateOptions` .

`TranslationMatch`Nesne şu değerlerden oluşur:

* `Error`: Belirli bir giriş dizesi için bir hata oluşursa hata kodu. Aksi takdirde, bu alan boştur.
* `MatchDegree`: Giriş metninin depoda bulunan özgün metinle ne kadar yakın olduğunu gösterir. Sistem, tam eşleşmeler dahil olmak üzere, depoya karşı giriş cümlelerini eşleştirir. Değer, 0 ile 100 arasında bir değer döndürür; burada 0 benzerlik yoktur ve 100 tam, büyük/küçük harfe duyarlı bir eşleşmedir.
* `MatchedOriginalText`: Bu sonuçla eşleşen özgün metin. Bu değer yalnızca eşleşen özgün metnin giriş metninde farklı olması durumunda döndürülür. Benzer bir eşleşmenin kaynak metnini döndürmek için kullanılır. Bu değer, Microsoft Translator sonuçları için döndürülmez.
* `Rating`: Kalite kararı veren kişinin yetkilisini belirtir. Makine çevirisi sonuçlarında 5 derecelendirmesi vardır. Anonim olarak belirtilen Çeviriler genellikle 1 ile 4 arasında bir derecelendirmesine sahiptir. Yetkili olarak sağlanmış Çeviriler genellikle 6 ile 10 arasında bir derecelendirme olur.
* `Count`: Bu derecelendirmeden bu çevirinin kaç kez seçildiği. Değer, otomatik olarak çevrilmiş yanıt için 0 ' dır.
* `TranslatedText`: Çevrilmiş metin.

### <a name="response-class-status-200"></a>Response sınıfı (durum 200)
`GetTranslationsResponse`Daha önce açıklanan biçimdeki bir nesne.

string

Yanıt içerik türü: Application/XML
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|AppID|olmamalıdır|Gereklidir. `Authorization`Veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanılıyorsa, `appid` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .|sorgu|string|
|metin|olmamalıdır|Gereklidir. Çevrilecek metni temsil eden bir dize. Metnin en büyük boyutu 10.000 karakterdir.|sorgu|string|
|Kaynak|olmamalıdır|Gereklidir. Çevrilen metnin dil kodunu temsil eden bir dize.|sorgu|string|
|- |olmamalıdır    |Gereklidir. Metni çevirecek dilin dil kodunu temsil eden bir dize.|sorgu|string|
|Maxçeviriler|olmamalıdır|Gereklidir. Döndürülecek en fazla çeviri sayısını temsil eden bir tamsayı.|sorgu|integer|
|Yetkilendirme| olmamalıdır|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .|string|  üst bilgi|
|Ocp-Apim-Subscription-Key|olmamalıdır  |Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400    |Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="post-gettranslationsarray"></a>POST/GetTranslationsArray

### <a name="implementation-notes"></a>Uygulama notları
Birden çok kaynak metin için birden çok çeviri adayları alır.

İstek URI 'SI `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray` .

İstek gövdesinin biçimi şöyledir:

```
<GetTranslationsArrayRequest>
  <AppId></AppId>
  <From>language-code</From>
  <MaxTranslations>int-value</MaxTranslations>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"/>
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</GetTranslationsArrayRequest>
```

`GetTranslationsArrayRequest`Şu öğeleri içerir:

* `AppId`İstenir. `Authorization`Üst bilgi kullanılırsa, `AppId` alanı boş bırakın. Aksi takdirde, içeren bir dize ekleyin `"Bearer" + " " + "access_token"` .
* `From`İstenir. Çevrilen metnin dil kodunu temsil eden bir dize.
* `MaxTranslations`İstenir. Döndürülecek en fazla çeviri sayısını temsil eden bir tamsayı.
* `Options`Seçim. `Options`Aşağıdaki değerleri içeren bir nesne. Bunların tümü isteğe bağlıdır ve varsayılan olarak en sık kullanılan ayarlara sahiptir. Belirtilen öğelerin alfabetik sırada listelenmesi gerekir.
    - `Category`: Çevirinin kategorisini (etki alanı) içeren bir dize. Varsayılan değer: `general`.
    - `ContentType`: Desteklenen tek seçenek ve varsayılan, ' dir `text/plain` .
    - `IncludeMultipleMTAlternatives`: MT altyapısından birden fazla alternatif döndürülüp döndürülmeyeceğini belirten bir Boole bayrağı. Geçerli değerler `true` ve `false` (büyük/küçük harfe duyarlı). Varsayılan olarak, `false` yalnızca bir alternatif döndürür. Bayrağını, `true` , birlikte çalışan çeviri çerçevesiyle (CTF) tam olarak tümleştirilmiş, çeviride yapay alternatifler oluşturmayı sağlar. Özelliği, kod çözücüsünün *n*en iyi listesinden yapay alternatifler ekleyerek CTF 'de alternatif olmayan cümleler için alternatifler döndürmeyi mümkün.
        - Derecelendirmeler aşağıdaki gibi uygulanır:
          - En iyi otomatik çeviri, 5 derecesine sahiptir.
          - CTF 'nin alternatifleri gözden geçirenin yetkilisini yansıtır. -10 ile + 10 arasında değişir.
          - Otomatik olarak oluşturulan (*n*-en iyi) çeviri alternatifleri, 0 derecelendirmesine ve 100 eşleşme derecesine sahiptir.
        - Alternatifin sayısı. Döndürülen alternatiflere ilişkin sayı ' de belirtilen değer kadar yüksek olabilir `maxTranslations` , ancak daha az olabilir.
        - Dil çiftleri. Bu işlev, Basitleştirilmiş Çince ve geleneksel Çince arasında her iki yönde de çeviri için kullanılamaz. Microsoft Translator tarafından desteklenen tüm diğer dil çiftleri için kullanılabilir.
* `State`: İstek ve yanıtı ilişkilendirmenize yardımcı olmak için Kullanıcı durumu. Yanıtta aynı içerik döndürülecek.
* `Uri`: Sonuçları bu URI 'ye göre filtreleyin. Değer ayarlanmamışsa, varsayılan olur `all` .
* `User`: Sonuçları bu kullanıcıya göre filtreleyin. Değer ayarlanmamışsa, varsayılan olur `all` .
* `Texts`İstenir. Çeviri metnini içeren bir dizi. Tüm dizeler aynı dilde olmalıdır. Çevrilecek tüm metinlerin toplamı 10.000 karakteri aşamaz. Dizi öğelerinin maksimum sayısı 10 ' dur.
* `To`İstenir. Metni çevirecek dilin dil kodunu temsil eden bir dize.

İsteğe bağlı öğeleri atlayabilirsiniz. Doğrudan alt öğeleri olan öğelerin `GetTranslationsArrayRequest` alfabetik sırada listelenmesi gerekir.

İstek `Content-Type` olmalıdır `text/xml` .

**Dönüş değeri:** Yanıtın biçimi şöyledir:

```
<ArrayOfGetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <GetTranslationsResponse>
    <From>language-code</From>
    <State/>
    <Translations>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText>string-value</MatchedOriginalText>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText/>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
    </Translations>
  </GetTranslationsResponse>
</ArrayOfGetTranslationsResponse>
```

Her `GetTranslationsResponse` öğe şu değerleri içerir:

* `Translations`: Nesneler içinde depolanan eşleşmelerin bir dizisi `TranslationMatch` (aşağıdaki bölümde açıklanmıştır). Çeviriler orijinal metnin hafif türevlerini (belirsiz eşleşme) içerebilir. Çeviriler sıralanacaktır: ilk olarak %100 eşleşme, belirsiz eşleşme ileri.
* `From`: Yöntem bir dil belirtmezse `From` , bu değer otomatik dil algılamada gelir. Aksi halde, belirtilen `From` dil olacaktır.
* `State`: İstek ve yanıtı ilişkilendirmenize yardımcı olmak için Kullanıcı durumu. Parametresinde sağlanan değeri içerir `TranslateOptions` .

`TranslationMatch`Nesnesi aşağıdaki değerleri içerir:
* `Error`: Belirli bir giriş dizesi için bir hata oluşursa hata kodu. Aksi takdirde, bu alan boştur.
* `MatchDegree`: Giriş metninin depoda bulunan özgün metinle ne kadar yakın olduğunu gösterir. Sistem, tam eşleşmeler dahil olmak üzere, depoya karşı giriş cümlelerini eşleştirir. Değer, 0 ile 100 arasında bir değer döndürür; burada 0 benzerlik yoktur ve 100 tam, büyük/küçük harfe duyarlı bir eşleşmedir.
* `MatchedOriginalText`: Bu sonuçla eşleşen özgün metin. Bu değer yalnızca eşleşen özgün metnin giriş metninde farklı olması durumunda döndürülür. Benzer bir eşleşmenin kaynak metnini döndürmek için kullanılır. Bu değer, Microsoft Translator sonuçları için döndürülmez.
* `Rating`: Kalite kararı veren kişinin yetkilisini belirtir. Makine çevirisi sonuçlarında 5 derecelendirmesi vardır. Anonim olarak belirtilen Çeviriler genellikle 1 ile 4 arasında bir derecelendirmesine sahiptir. Yetkili olarak sağlanmış Çeviriler genellikle 6 ile 10 arasında bir derecelendirmesine sahiptir.
* `Count`: Bu derecelendirmeden bu çevirinin kaç kez seçildiği. Değer, otomatik olarak çevrilmiş yanıt için 0 ' dır.
* `TranslatedText`: Çevrilmiş metin.


### <a name="response-class-status-200"></a>Response sınıfı (durum 200)

string

Yanıt içerik türü: Application/XML
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme  |olmamalıdır    |Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üst bilgi boş bırakılırsa gereklidir.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"` .|üst bilgi|string|
|Ocp-Apim-Subscription-Key|olmamalıdır  |Hem `appid` alan hem de `Authorization` üst bilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Nedeni|
|:--|:--|
|400    |Hatalı istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bilgi verin. Lütfen isteğin yaklaşık Tarih & zamanını ve yanıt üstbilgisine dahil edilen istek KIMLIĞI ile bize bildirin `X-MS-Trans-Info` .|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hatanın devam edip etmediğini bize bildirin.|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Translator v3 'e geçiş](../migrate-to-v3.md)


