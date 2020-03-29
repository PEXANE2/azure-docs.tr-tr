---
title: Çevirmen Metin API v2.0
titleSuffix: Azure Cognitive Services
description: Çevirmen Metin API v2.0 için referans belgeleri.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/15/2018
ms.author: swmachan
ms.openlocfilehash: f111169558118a80602bcb2136bc63ce54c9e0d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72242494"
---
# <a name="translator-text-api-v20"></a>Çevirmen Metin API v2.0

> [!IMPORTANT]
> Çevirmen Metin API'sinin bu sürümü küçümsenir. [Çevirmen Metin API sürümü 3 için belgeleri görüntüleyin.](v3-0-reference.md)

Çevirmen Metin API'sinin Sürüm 2 sürümü, çok dilli kullanıcı deneyimleri sağlamak için uygulamalarınız, web sitelerine, araçlarınıza veya diğer çözümlerinize sorunsuz bir şekilde entegre edilebilir. Herhangi bir donanım platformunda ve herhangi bir işletim sisteminde, endüstri standartlarına göre metin dili algılama ve metinden konuşmaya metin gibi dil çevirisi ve dille ilgili diğer görevleri gerçekleştirmek için kullanabilirsiniz. Daha fazla bilgi için [Çevirmen Metin API'si'ne](../translator-info-overview.md)bakın.

## <a name="getting-started"></a>Başlarken
Çevirmen Metin API'sine erişmek [için Microsoft Azure'a kaydolmanız](../translator-text-how-to-signup.md)gerekir.

## <a name="authentication"></a>Kimlik doğrulaması 
Çevirmen Metin API'sine yapılan tüm aramalarda kimlik doğrulaması için bir abonelik anahtarı gerekir. API üç kimlik doğrulama yöntemini destekler:

- Erişim jetonu. Kimlik doğrulama hizmetine BIR POST isteği nde bulunarak bir erişim jetonu oluşturmak için abonelik anahtarını kullanın. Ayrıntılar için belirteç hizmeti belgelerine bakın. Üstbilgi veya sorgu parametresini kullanarak `Authorization` erişim belirteci'ni Çevirmen hizmetine `access_token` iletin. Erişim belirteci 10 dakika geçerlidir. Her 10 dakikada bir yeni bir erişim belirteci edinin ve 10 dakika boyunca tekrarlanan istekler için aynı erişim belirteci kullanmaya devam edin.
- Doğrudan kullanılan bir abonelik anahtarı. İstekle birlikte çevirmen metin `Ocp-Apim-Subscription-Key` API'sine ekteki başlıkta bir değer olarak abonelik anahtarınızı geçirin. Abonelik anahtarını doğrudan kullandığınızda, erişim jetonu oluşturmak için belirteç kimlik doğrulama hizmetini aramanız gerekmez.
- [Azure Bilişsel Hizmetler çok hizmetli abonelik.](https://azure.microsoft.com/pricing/details/cognitive-services/) Bu yöntem, birden çok hizmet için istekleri doğrulamak için tek bir gizli anahtar kullanmanıza olanak sağlar.
Çok hizmetli gizli bir anahtar kullandığınızda, isteğiniz ile birlikte iki kimlik doğrulama üstadı eklemeniz gerekir. İlk başlık gizli anahtarı geçer. İkinci üstbilgi, aboneliğinizle ilişkili bölgeyi belirtir:
   - `Ocp-Apim-Subscription-Key`
   - `Ocp-Apim-Subscription-Region`

Bölge, çok hizmetli Metin API aboneliği için gereklidir. Seçtiğiniz bölge, çok hizmetli abonelik anahtarını kullandığınızda metin çevirisi için kullanabileceğiniz tek bölgedir. Azure portalında çoklu hizmet aboneliğinize kaydolurken seçtiğiniz bölge olması gerekir.

Mevcut bölgeler `australiaeast`, `brazilsouth` `canadacentral`, `centralindia` `centraluseuap` `eastasia` `eastus` `eastus2` `westeurope` `westus` `westus2`, , , , , , , , , , , , ve . `japaneast` `northeurope` `southcentralus` `southeastasia` `uksouth` `westcentralus`

Abonelik anahtarınız ve erişim belirteciniz görünümden gizlenmesi gereken sırlardır.

## <a name="profanity-handling"></a>Küfür işleme
Normalde, Çevirmen hizmeti kaynakta bulunan küfürleri saklar. Küfür derecesi ve kelimeleri küfür yapan bağlam kültüre göre farklılık gösterir. Yani hedef dildeki küfür derecesi artırılabilir veya azaltılabilir.

Kaynak metinde olsa bile çeviride küfür oluşmasını önlemek istiyorsanız, onu destekleyen yöntemler için küfür filtreleme seçeneğini kullanabilirsiniz. Bu seçenek, küfürlerin silindiğini veya uygun etiketlerle işaretlendiğini veya hedefteki küfüre izin vermek isteyip istemediğinizi seçmenize olanak tanır. `ProfanityAction` Kabul edilen değerler `NoAction` (varsayılan) `Marked`ve `Deleted`.


|KüfürEylem    |Eylem |Örnek kaynak (Japonca)  |Örnek çeviri (İngilizce)  |
|:--|:--|:--|:--|
|Eylem Yok   |Varsayılan. Seçeneği ayarlamamak la aynı. Küfür kaynaktan hedefe geçer.        |Bu nedenle, bu tür bir oyun     |O bir ahmak.   |
|Işaretlenmiş     |Küfürlü kelimeler xml etiketleri \<küfür> ve \</ küfür> ile çevrili olacaktır.       |Bu nedenle, bu tür bir oyun |O bir \<küfür>ahmak\</ küfür>.  |
|Silme    |Saygısız kelimeler değiştirilmeden çıktıdan çıkarılır.     |Bu nedenle, bu tür bir oyun |O bir.   |

    
## <a name="excluding-content-from-translation"></a>İçeriği çeviriden hariç t
Html gibi`contentType=text/html`etiketlerle içerik çevirdiğinizde, bazen belirli içeriği çeviriden hariç tutmak yararlıdır. Özniteliği, `class=notranslate` özgün dilinde kalması gereken içeriği belirtmek için kullanabilirsiniz. Aşağıdaki örnekte, ilk `div` öğedeki içerik çevrilmez, ancak ikinci `div` öğedeki içerik çevrilecektir.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>GET / Çevir

### <a name="implementation-notes"></a>Uygulama notları
Metin dizesini bir dilden diğerine çevirir.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/Translate`olduğunu .

**İade değeri:** Çevrilen metni temsil eden bir dize.

Daha önce aynı `AddTranslation` `AddTranslationArray` kaynak cümle için 5 veya daha yüksek bir derecelendirmeye sahip bir çeviri kullandıysanız veya girmek için, `Translate` yalnızca sisteminizde kullanılabilen en iyi seçeneği döndürür. "Aynı kaynak tümce" büyük harf, beyaz boşluk, etiket değerleri ve cümlenin sonundaki noktalama işaretleri dışında tam olarak aynı (%100 eşleştirme) anlamına gelir. Hiçbir derecelendirme 5 veya üzerinde bir derecelendirme ile depolanırsa, döndürülen sonuç Microsoft Translator tarafından otomatik çeviri olacaktır.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

string

Yanıt içeriği türü: uygulama/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama    |Parametre türü|veri türü|
|:--|:--|:--|:--|:--|
|Appıd  |(boş)    |Gereklidir. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `appid` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.|sorgu|string|
|metin|(boş)   |Gereklidir. Çevrilecek metni temsil eden bir dize. Metin 10.000'den fazla karakter içeremez.|sorgu|string|
|Kaynak|(boş)   |İsteğe bağlı. Çevrilen metnin dil kodunu temsil eden dize. Örneğin, En İngilizce için.|sorgu|string|
|-|(boş) |Gereklidir. Metni çevirmek için dilin kodunu temsil eden bir dize.|sorgu|string|
|Contenttype|(boş)    |İsteğe bağlı. Çevrilen metnin biçimi. Desteklenen biçimler `text/plain` (varsayılan) `text/html`ve . Herhangi bir HTML öğesi iyi biçimlendirilmiş, tam elemanlar olmalıdır.|sorgu|string|
|category|(boş)   |İsteğe bağlı. Çevirinin kategorisini (etki alanını) içeren dize. Varsayılan değer: `general`.|sorgu|string|
|Yetkilendirme|(boş)  |Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)  |Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|


### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503    |Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-translatearray"></a>POST /TranslateArray

### <a name="implementation-notes"></a>Uygulama notları
Birden çok kaynak metin için çevirileri alır.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`olduğunu .

İstek gövdesinin biçimi aşağıda veda edin:

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

Bu unsurlar `TranslateArrayRequest`şunlardır:


* `AppId`: Gerekli. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `AppId` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.
* `From`: İsteğe bağlı. Çevrilen metnin dil kodunu temsil eden dize. Bu alan boş bırakılırsa, yanıt otomatik dil algılama sonucunu içerir.
* `Options`: İsteğe bağlı. Aşağıdaki `Options` değerleri içeren bir nesne. Bunların tümü isteğe bağlıdır ve en yaygın ayarlar için varsayılandır. Belirtilen öğeler alfabetik sırada listelenmelidir.
    - `Category`: Çevirinin kategorisini (etki alanını) içeren dize. Varsayılan değer: `general`.
    - `ContentType`: Çevrilen metnin biçimi. Desteklenen biçimler `text/plain` (varsayılan) `text/xml`ve `text/html`. Herhangi bir HTML öğesi iyi biçimlendirilmiş, tam elemanlar olmalıdır.
    - `ProfanityAction`: Daha önce açıklandığı gibi küfürlerin nasıl ele alındığını belirtir. Kabul edilen `NoAction` değerler `Marked`(varsayılan) `Deleted`ve .
    - `State`: İstek ve yanıtı ilişkilendirmeye yardımcı olacak kullanıcı durumu. Aynı içerik yanıtta döndürülür.
    - `Uri`: Bu URI tarafından filtresonuçları. Varsayılan: `all`.
    - `User`: Sonuçları bu kullanıcı tarafından filtreleyin. Varsayılan: `all`.
* `Texts`: Gerekli. Çeviri metnini içeren bir dizi. Tüm dizeleri aynı dilde olmalıdır. Çevrilecek tüm metnin toplamı 10.000 karakteri geçemez. En fazla dizi öğesi sayısı 2.000'dir.
* `To`: Gerekli. Metni çevirmek için dilin kodunu temsil eden bir dize.

İsteğe bağlı öğeleri atlayabilirsiniz. Doğrudan çocukları olan `TranslateArrayRequest` öğeler alfabetik sırada listelenmelidir.

Yöntem `TranslateArray` kabul `application/xml` eder `text/xml` `Content-Type`veya için .

**İade değeri:** Bir `TranslateArrayResponse` dizi. Her `TranslateArrayResponse` biri şu öğelere sahiptir:

* `Error`: Biri oluşursa bir hata gösterir. Aksi takdirde null ayarlayın.
* `OriginalSentenceLengths`: Kaynak metindeki her cümlenin uzunluğunu gösteren bir aramet dizisi. Dizinin uzunluğu cümle sayısını gösterir.
* `TranslatedText`: Çevrilmiş metin.
* `TranslatedSentenceLengths`: Çevrilen metindeki her cümlenin uzunluğunu gösteren bir tamsayı dizisi. Dizinin uzunluğu cümle sayısını gösterir.
* `State`: İstek ve yanıtı ilişkilendirmeye yardımcı olacak kullanıcı durumu. İstekle aynı içeriği verir.

Yanıt gövdesinin biçimi aşağıda veda edinebilirsiniz:

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

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Başarılı bir yanıt, `TranslateArrayResponse` daha önce açıklanan biçimde bir dizi dizi dizi içerir.

string

Yanıt içeriği türü: uygulama/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme|(boş)  |Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu   |Neden|
|:--|:--|
|400    |Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin. Sık karşılaşılan hatalar şunlardır: <ul><li>Dizi öğesi boş olamaz.</li><li>Geçersiz kategori.</li><li>Dilden geçersizdir.</li><li>Dil geçersizdir.</li><li>İstek çok fazla öğe içerir.</li><li>From dili desteklenmez.</li><li>To dili desteklenmez.</li><li>Çeviri İsteği çok fazla veriye sahiptir.</li><li>HTML doğru biçimde değildir.</li><li>Çeviri İsteği'nde çok fazla dize geçti.</li></ul>|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503    |Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Uygulama notları
Parametre `languageCodes`olarak geçirilen dillerin kolay adlarını alır, geçirilen `locale` dile yerelleştirilmiş.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`olduğunu .

İstek gövdesi, dost adları almak için ISO 639-1 dil kodlarını temsil eden bir dize dizisi içerir. Bir örneği aşağıda verilmiştir:

```
<ArrayOfstring xmlns:i="https://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**İade değeri:** Çevirmen hizmeti tarafından desteklenen ve istenen dile yerelleştirilmiş dil adlarını içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Çevirmen hizmeti tarafından desteklenen ve istenen dile yerelleştirilmiş dil adlarını içeren bir dize dizisi.

string

Yanıt içeriği türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Appıd|(boş)|Gereklidir. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `appid` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.|sorgu|string|
|yerel ayar|(boş) |Gereklidir. Dil adlarını yerelleştirmek için kullanılan aşağıdakilerden birini temsil eden bir dize: <ul><li>Bir dil ile ilişkili bir ISO 639 iki harfli küçük kültür kodu ve bir ISO 3166 iki harfli büyük harfli alt kültür kodu kombinasyonu. <li>Bir ISO 639 küçük kültür kodu tek başına.|sorgu|string|
|Yetkilendirme|(boş)  |Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)  |Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503    |Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-getlanguagesfortranslate"></a>GET /GetLanguagesForTranslate

### <a name="implementation-notes"></a>Uygulama notları
Çeviri hizmeti tarafından desteklenen dilleri temsil eden dil kodlarının bir listesini alır.  `Translate`ve `TranslateArray` bu dillerin herhangi ikisi arasında çeviri yapabilirsiniz.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`olduğunu .

**İade değeri:** Çevirmen hizmeti tarafından desteklenen dil kodlarını içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Çevirmen hizmeti tarafından desteklenen dil kodlarını içeren bir dize dizisi.

string

Yanıt içeriği türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Appıd|(boş)|Gereklidir. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `appid` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.|sorgu|string|
|Yetkilendirme|(boş)  |Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503|Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-getlanguagesforspeak"></a>GET /GetLanguagesForSpeak

### <a name="implementation-notes"></a>Uygulama notları
Konuşma sentezi için kullanılabilir dilleri alır.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`olduğunu .

**İade değeri:** Çevirmen hizmeti tarafından konuşma sentezi için desteklenen dil kodlarını içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Çevirmen hizmeti tarafından konuşma sentezi için desteklenen dil kodlarını içeren bir dize dizisi.

string

Yanıt içeriği türü: uygulama/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Appıd|(boş)|Gereklidir. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `appid` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.|sorgu|string|
|Yetkilendirme|(boş)|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|
 
### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400|Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401|Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503    |Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-speak"></a>GET /Speak

### <a name="implementation-notes"></a>Uygulama notları
İstenilen dilde konuşulan geçirilen metnin WAV veya MP3 akışını verir.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/Speak`olduğunu .

**İade değeri:** İstenilen dilde konuşulan, geçirilen metnin WAV veya MP3 akışı.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

ikili

Yanıt içeriği türü: uygulama/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Appıd|(boş)|Gereklidir. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `appid` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.|sorgu|string|
|metin|(boş)   |Gereklidir. Belirtilen dilde akış için konuşulacak bir veya daha fazla cümle içeren bir dize. Metin 2.000 karakteri geçmemelidir.|sorgu|string|
|language|(boş)   |Gereklidir. Metni konuşmak için dilin desteklenen dil kodunu temsil eden dize. Kod, yöntem `GetLanguagesForSpeak`tarafından döndürülen kodlardan biri olmalıdır.|sorgu|string|
|biçim|(boş)|İsteğe bağlı. İçerik türü kimliği belirten bir dize. Şu `audio/wav` anda ve `audio/mp3` kullanılabilir. Varsayılan değer: `audio/wav`.|sorgu|string|
|seçenekler|(boş)    |İsteğe bağlı. Sentezlenen konuşmanın özelliklerini belirten bir dize:<ul><li>`MaxQuality`ve `MinSize` ses sinyalinin kalitesini belirtin. `MaxQuality`en yüksek kaliteyi sağlar. `MinSize`en küçük dosya boyutunu sağlar. Varsayılan değer. `MinSize`</li><li>`female`ve `male` sesin istenilen cinsiyetini belirtin. Varsayılan değer: `female`. Birden çok seçenek<code>\|</code>eklemek için dikey çubuğu () kullanın. Örneğin, `MaxQuality|Male`.</li></li></ul>  |sorgu|string|
|Yetkilendirme|(boş)|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)  |Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503    |Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-detect"></a>GET /Detect

### <a name="implementation-notes"></a>Uygulama notları
Metnin bir bölümünün dilini tanımlar.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/Detect`olduğunu .

**İade değeri:** Metin için iki karakterli bir dil kodu içeren dize.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

string

Yanıt içeriği türü: uygulama/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Appıd|(boş)  |Gereklidir. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `appid` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.|sorgu|string|
|metin|(boş)|Gereklidir. Dili tanımlanacak metin içeren bir dize. Metin 10.000 karakteri geçmemelidir.|sorgu|  string|
|Yetkilendirme|(boş)|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key  |(boş)    |Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400|Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503    |Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|


## <a name="post-detectarray"></a>POST /DetectArray

### <a name="implementation-notes"></a>Uygulama notları

Dilleri bir dizi dizehalinde tanımlar. Bağımsız olarak her bir dizi öğesinin dilini algılar ve dizinin her satırı için bir sonuç döndürür.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`olduğunu .

İstek gövdesinin biçimi aşağıda veda edin:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

Metin 10.000 karakteri geçemez.

**İade değeri:** Giriş dizisindeki her satır için iki karakterli bir dil kodu içeren bir dize dizisi.

Yanıt gövdesinin biçimi aşağıda veda edinebilirsiniz:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
`DetectArray`başarılı oldu. Giriş dizisinin her satırı için iki karakterli bir dil kodu içeren bir dize dizisi döndürür.

string

Yanıt içeriği türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Appıd|(boş)|Gereklidir. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `appid` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.|sorgu|string|
|Yetkilendirme|(boş)|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503    |Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-addtranslation"></a>GET / AddTranslation

### <a name="implementation-notes"></a>Uygulama notları

> [!IMPORTANT]
> **Amortisman notu:** 31 Ocak 2018 tarihinden sonra bu yöntem yeni cümle başvurularını kabul etmeyecek. Bir hata iletisi alırsınız. Lütfen İşbirlikçi Çeviri Çerçevesi'ndeki (CTF) değişiklikler hakkındaki duyuruya bakın.

Çeviri belleğine bir çeviri ekler.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`olduğunu .

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

string

Yanıt içeriği türü: uygulama: xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü   |
|:--|:--|:--|:--|:--|
|Appıd|(boş)|Gereklidir. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `appid` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.|sorgu|string|
|originalText|(boş)|Gereklidir. Çevrilecek metni içeren bir dize. Dize maksimum uzunluğu 1.000 karakterdir.|sorgu|string|
|çevrilmişMetin|(boş) |Gereklidir. Hedef dile çevrilmiş metin içeren bir dize. Dize maksimum uzunluğu 2.000 karakterdir.|sorgu|string|
|Kaynak|(boş)   |Gereklidir. Metnin özgün dilinin dil kodunu temsil eden dize. Örneğin, En İngilizce için ve almanca için de.|sorgu|string|
|-|(boş)|Gereklidir. Metni çevirmek için dilin dil kodunu temsil eden dize.|sorgu|string|
|rating|(boş) |İsteğe bağlı. Dize için kalite derecelendirmesini temsil eden bir arayıcı. Değer -10 ile 10 arasındadır. Varsayılan değer 1'dir.|sorgu|integer|
|Contenttype|(boş)    |İsteğe bağlı. Çevrilen metnin biçimi. Desteklenen biçimler `text/plain` ve `text/html`. Herhangi bir HTML öğesi iyi biçimlendirilmiş, tam elemanlar olmalıdır.    |sorgu|string|
|category|(boş)|İsteğe bağlı. Çevirinin kategorisini (etki alanını) içeren dize. Varsayılan değer: `general`.|sorgu|string|
|kullanıcı|(boş)|Gereklidir. Gönderinin kaynağını izlemek için kullanılan bir dize.|sorgu|string|
|Urı|(boş)|İsteğe bağlı. Çevirinin içerik konumunu içeren bir dize.|sorgu|string|
|Yetkilendirme|(boş)|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.  |üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|410|`AddTranslation`artık desteklenmeyecek.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503    |Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-addtranslationarray"></a>POST /AddTranslationArray

### <a name="implementation-notes"></a>Uygulama notları

> [!IMPORTANT]
> **Amortisman notu:** 31 Ocak 2018 tarihinden sonra bu yöntem yeni cümle başvurularını kabul etmeyecek. Bir hata iletisi alırsınız. Lütfen İşbirlikçi Çeviri Çerçevesi'ndeki (CTF) değişiklikler hakkındaki duyuruya bakın.

Çeviri belleğine bir dizi çeviri ekler. Bu `AddTranslation`yöntem, bir dizi sürümüdür.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray`olduğunu .

İstek gövdesinin biçimi aşağıda veda edin:

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

Bu unsurlar `AddtranslationsRequest`şunlardır:

* `AppId`: Gerekli. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `AppId` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.
* `From`: Gerekli. Kaynak dilin dil kodunu içeren bir dize. `GetLanguagesForTranslate` Yöntemle döndürülen dillerden biri olmalıdır.
* `To`: Gerekli. Hedef dilin dil kodunu içeren bir dize. `GetLanguagesForTranslate` Yöntemle döndürülen dillerden biri olmalıdır.
* `Translations`: Gerekli. Çeviri belleğieklemek için bir dizi çeviri. Her çeviri `OriginalText`, `TranslatedText`, `Rating`ve . Her `OriginalText` birinin maksimum `TranslatedText` boyutu ve 1.000 karakterdir. Tüm `OriginalText` öğelerin `TranslatedText` ve öğelerin toplamı 10.000 karakteri geçemez. En fazla dizi öğesi sayısı 100'dür.
* `Options`: Gerekli. Bir dizi seçenek, `Category`dahil `ContentType` `Uri`olmak `User`üzere , , , ve . `User` gereklidir. `Category`, `ContentType`ve `Uri` isteğe bağlıdır. Belirtilen öğeler alfabetik sırada listelenmelidir.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
`AddTranslationArray`yöntem başarılı oldu. 

31 Ocak 2018 tarihinden sonra ceza başvuruları kabul edilmeyecek. Hizmet hata kodu 410 ile yanıt verecektir.

string

Yanıt içeriği türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme|(boş)|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|410    |`AddTranslation`artık desteklenmeyecek.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503|Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-breaksentences"></a>GET /BreakSentences

### <a name="implementation-notes"></a>Uygulama notları
Metnin bir bölümünü cümlelere ayırır ve her cümlenin uzunluklarını içeren bir dizi döndürür.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`olduğunu .

**İade değeri:** Cümlelerin uzunluklarını temsil eden bir dizi toplamcı. Dizinin uzunluğu cümle sayısını temsil eder. Değerler her cümlenin uzunluğunu temsil ediyor.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Cümlelerin uzunluklarını temsil eden bir dizi toplamcı. Dizinin uzunluğu cümle sayısını temsil eder. Değerler her cümlenin uzunluğunu temsil ediyor.

integer

Yanıt içeriği türü: uygulama/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Appıd|(boş)  |Gereklidir. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `appid` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.|sorgu| string|
|metin|(boş)   |Gereklidir. Cümlelere bölünecek metni temsil eden dize. Metnin en büyük boyutu 10.000 karakterdir.|sorgu|string|
|language   |(boş)    |Gereklidir. Giriş metninin dil kodunu temsil eden bir dize.|sorgu|string|
|Yetkilendirme|(boş)|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.   |üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400|Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401|Geçersiz kimlik bilgileri.|
|500|Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503|Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Uygulama notları
Mağazadan ve MT motorundan belirli bir dil çifti için bir dizi çeviri alır. `GetTranslations`tüm kullanılabilir `Translate` çevirileri döndürür farklı.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations`olduğunu .

İsteğin gövdesi, bu `TranslationOptions` biçime sahip isteğe bağlı nesneyi içerir:

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

Nesne `TranslateOptions` aşağıdaki listedeki değerleri içerir. Bunların tümü isteğe bağlıdır ve en yaygın ayarlar için varsayılandır. Belirtilen öğeler alfabetik sırada listelenmelidir.

* `Category`: Çevirinin kategorisini (etki alanını) içeren dize. Varsayılan değer: `general`.
* `ContentType`: Desteklenen tek seçenek ve varsayılan `text/plain`seçenek.
* `IncludeMultipleMTAlternatives`: MT motorundan birden fazla alternatifin döndürülüp döndürülmemesi gerektiğini belirten bir Boolean bayrağı. Geçerli değerler `true` `false` ve (büyük/küçük harf duyarlı). Varsayılan, `false`yalnızca bir alternatif döndürür. Bayrağın, `true` İşbirlikçi Çeviri Çerçevesi (CTF) ile tam olarak entegre edilmiş yapay alternatiflerin oluşturulmasını sağlayacak şekilde ayarlanması. Bu özellik, kod çözücünün *n*-en iyi listesinden yapay alternatifler ekleyerek CTF'de çevirisi olmayan cümleler için alternatiflerin döndürülmesini sağlar.
    - Derecelendirme. Derecelendirmeler şu şekilde uygulanır: 
         - En iyi otomatik çeviri 5 puana sahiptir.
       - CTF'nin alternatifleri gözden geçirenin yetkisini yansıtır. -10 ile +10 arasında değişir.
       - Otomatik olarak oluşturulan *(n*-en iyi) çeviri alternatifleri 0 dereceye ve maç derecesi 100'e sahiptir.
    - Alternatif sayısı. Döndürülen alternatiflerin sayısı belirtilen değer kadar `maxTranslations`yüksek olabilir, ancak daha düşük olabilir.
    - Dil çiftleri. Bu işlevsellik, basitleştirilmiş Çince ve Geleneksel Çince arasındaki çeviriler için her iki yönde de kullanılamaz. Microsoft Translator tarafından desteklenen diğer tüm dil çiftleri için kullanılabilir.
* `State`: İstek ve yanıtı ilişkilendirmeye yardımcı olacak kullanıcı durumu. Aynı içerik yanıtta döndürülür.
* `Uri`: Bu URI tarafından filtresonuçları. Değer ayarlı değilse, varsayılan `all`değer.
* `User`: Sonuçları bu kullanıcı tarafından filtreleyin. Değer ayarlı değilse, varsayılan `all`değer.

İstek. `Content-Type` `text/xml`

**İade değeri:** Yanıtın biçimi aşağıda veda edinebilirsiniz:

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

Bu yanıt, `GetTranslationsResponse` aşağıdaki değerleri içeren bir öğe içerir:

* `Translations`: Nesnelerde `TranslationMatch` saklanan bulunan eşleşmelerin bir dizisi (aşağıdaki bölümde açıklanmıştır). Çeviriler, özgün metnin hafif türevlerini (bulanık eşleştirme) içerebilir. Çeviriler sıralanacaktır: %100 önce eşleşir, sonra bulanık eşleşmeler.
* `From`: Yöntembir `From` dil belirtmiyorsa, bu değer otomatik dil algılamadan gelir. Aksi takdirde, belirtilen `From` dil olacaktır.
* `State`: İstek ve yanıtı ilişkilendirmeye yardımcı olacak kullanıcı durumu. `TranslateOptions` Parametrede verilen değeri içerir.

Nesne `TranslationMatch` şu değerlerden oluşur:

* `Error`: Belirli bir giriş dizesi için bir hata oluşursa hata kodu. Aksi takdirde, bu alan boştur.
* `MatchDegree`: Giriş metninin mağazada bulunan orijinal metinle ne kadar yakın eşleştiğini gösterir. Sistem, giriş cümlelerini mağazaya karşı tam olmayan eşleşmeler de dahil olmak üzere eşler. Döndürülen değer 0 ile 100 arasında değişir, 0 benzerlik yoktur ve 100 tam, büyük/küçük harf duyarlı bir eşleşmedir.
* `MatchedOriginalText`: Bu sonuçla eşleşen özgün metin. Bu değer yalnızca eşleşen özgün metin giriş metninden farklıysa döndürülür. Bulanık bir eşleşmenin kaynak metnini döndürmek için kullanılır. Bu değer Microsoft Translator sonuçları için döndürülür.
* `Rating`: Kalite kararını yapan kişinin otoritesini gösterir. Makine Çeviri sonuçları 5 puana sahiptir. Anonim olarak sağlanan çeviriler genellikle 1'den 4'e kadar bir derecelendirmeye sahiptir. Yetkili olarak sağlanan çeviriler genellikle 6'dan 10'a kadar bir derecelendirmeye sahip olacaktır.
* `Count`: Bu derecelendirmeye sahip bu çevirinin kaç kez seçildiği. Otomatik olarak çevrilen yanıt için değer 0'dır.
* `TranslatedText`: Çevrilmiş metin.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Daha `GetTranslationsResponse` önce açıklanan biçimdeki bir nesne.

string

Yanıt içeriği türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Appıd|(boş)|Gereklidir. Üstbilgi `Authorization` `Ocp-Apim-Subscription-Key` veya üstbilgi kullanılırsa, `appid` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.|sorgu|string|
|metin|(boş)|Gereklidir. Çevrilecek metni temsil eden bir dize. Metnin en büyük boyutu 10.000 karakterdir.|sorgu|string|
|Kaynak|(boş)|Gereklidir. Çevrilen metnin dil kodunu temsil eden dize.|sorgu|string|
|- |(boş)    |Gereklidir. Metni çevirmek için dilin dil kodunu temsil eden dize.|sorgu|string|
|maxÇeviriler|(boş)|Gereklidir. Döndürülecek en fazla çeviri sayısını temsil eden bir karşıcı.|sorgu|integer|
|Yetkilendirme| (boş)|Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|string|  üst bilgi|
|Ocp-Apim-Subscription-Key|(boş)  |Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503|Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-gettranslationsarray"></a>POST /GetTranslationsArray

### <a name="implementation-notes"></a>Uygulama notları
Birden çok kaynak metin için birden çok çeviri adayı nı alır.

İstek URI `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray`olduğunu .

İstek gövdesinin biçimi aşağıda veda edin:

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

`GetTranslationsArrayRequest`bu öğeleri içerir:

* `AppId`: Gerekli. `Authorization` Üstbilgi kullanılırsa, `AppId` alanı boş bırakın. Aksi takdirde, içeren `"Bearer" + " " + "access_token"`bir dize içerir.
* `From`: Gerekli. Çevrilen metnin dil kodunu temsil eden dize.
* `MaxTranslations`: Gerekli. Döndürülecek en fazla çeviri sayısını temsil eden bir karşıcı.
* `Options`: İsteğe bağlı. Aşağıdaki `Options` değerleri içeren bir nesne. Bunların tümü isteğe bağlıdır ve en yaygın ayarlar için varsayılandır. Belirtilen öğeler alfabetik sırada listelenmelidir.
    - `Category`: Çevirinin kategorisini (etki alanını) içeren dize. Varsayılan değer: `general`.
    - `ContentType`: Desteklenen tek seçenek ve varsayılan `text/plain`seçenek.
    - `IncludeMultipleMTAlternatives`: MT motorundan birden fazla alternatifin döndürülüp döndürülmemesi gerektiğini belirten bir Boolean bayrağı. Geçerli değerler `true` `false` ve (büyük/küçük harf duyarlı). Varsayılan, `false`yalnızca bir alternatif döndürür. Ortak Çeviriler `true` Çerçevesi (CTF) ile tam olarak entegre edilmiş, çeviride yapay alternatiflerin üretilmesini sağlayacak şekilde bayrağın ayarlanması. Bu özellik, kod çözücünün *n*-en iyi listesinden yapay alternatifler ekleyerek CTF'de alternatifi olmayan cümleler için alternatiflerin döndürülmesini sağlar.
        - Derecelendirme Derecelendirmeler Aşağıdaki gibi uygulanır:
          - En iyi otomatik çeviri 5 puana sahiptir.
          - CTF'nin alternatifleri gözden geçirenin yetkisini yansıtır. -10 ile +10 arasında değişir.
          - Otomatik olarak oluşturulan *(n*-en iyi) çeviri alternatifleri 0 dereceye ve maç derecesi 100'e sahiptir.
        - Alternatif sayısı. Döndürülen alternatiflerin sayısı belirtilen değer kadar `maxTranslations`yüksek olabilir, ancak daha düşük olabilir.
        - Dil çiftleri. Bu işlevsellik, basitleştirilmiş Çince ve Geleneksel Çince arasındaki çeviriler için her iki yönde de kullanılamaz. Microsoft Translator tarafından desteklenen diğer tüm dil çiftleri için kullanılabilir.
* `State`: İstek ve yanıtı ilişkilendirmeye yardımcı olacak kullanıcı durumu. Aynı içerik yanıtta döndürülür.
* `Uri`: Bu URI tarafından filtresonuçları. Değer ayarlı değilse, varsayılan `all`değer.
* `User`: Sonuçları bu kullanıcı tarafından filtreleyin. Değer ayarlı değilse, varsayılan `all`değer.
* `Texts`: Gerekli. Çeviri metnini içeren bir dizi. Tüm dizeleri aynı dilde olmalıdır. Çevrilecek tüm metnin toplamı 10.000 karakteri geçemez. En fazla dizi öğesi sayısı 10'dur.
* `To`: Gerekli. Metni çevirmek için dilin dil kodunu temsil eden dize.

İsteğe bağlı öğeleri atlayabilirsiniz. Doğrudan çocukları olan `GetTranslationsArrayRequest` öğeler alfabetik sırada listelenmelidir.

İstek. `Content-Type` `text/xml`

**İade değeri:** Yanıtın biçimi aşağıda veda edinebilirsiniz:

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

* `Translations`: Nesnelerde `TranslationMatch` saklanan bulunan eşleşmelerin bir dizisi (aşağıdaki bölümde açıklanmıştır). Çeviriler, özgün metnin hafif türevlerini (bulanık eşleştirme) içerebilir. Çeviriler sıralanacaktır: %100 önce eşleşir, sonra bulanık eşleşmeler.
* `From`: Yöntembir `From` dil belirtmiyorsa, bu değer otomatik dil algılamadan gelir. Aksi takdirde, belirtilen `From` dil olacaktır.
* `State`: İstek ve yanıtı ilişkilendirmeye yardımcı olacak kullanıcı durumu. `TranslateOptions` Parametrede verilen değeri içerir.

Nesne `TranslationMatch` aşağıdaki değerleri içerir:
* `Error`: Belirli bir giriş dizesi için bir hata oluşursa hata kodu. Aksi takdirde, bu alan boştur.
* `MatchDegree`: Giriş metninin mağazada bulunan orijinal metinle ne kadar yakın eşleştiğini gösterir. Sistem, giriş cümlelerini mağazaya karşı tam olmayan eşleşmeler de dahil olmak üzere eşler. Döndürülen değer 0 ile 100 arasında değişir, 0 benzerlik yoktur ve 100 tam, büyük/küçük harf duyarlı bir eşleşmedir.
* `MatchedOriginalText`: Bu sonuçla eşleşen özgün metin. Bu değer yalnızca eşleşen özgün metin giriş metninden farklıysa döndürülür. Bulanık bir eşleşmenin kaynak metnini döndürmek için kullanılır. Bu değer Microsoft Translator sonuçları için döndürülür.
* `Rating`: Kalite kararını yapan kişinin otoritesini gösterir. Makine Çeviri sonuçları 5 puana sahiptir. Anonim olarak sağlanan çeviriler genellikle 1'den 4'e kadar bir derecelendirmeye sahiptir. Yetkili olarak sağlanan çeviriler genellikle 6'dan 10'a kadar bir derecelendirmeye sahiptir.
* `Count`: Bu derecelendirmeye sahip bu çevirinin kaç kez seçildiği. Otomatik olarak çevrilen yanıt için değer 0'dır.
* `TranslatedText`: Çevrilmiş metin.


### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

string

Yanıt içeriği türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme  |(boş)    |Hem `appid` alan hem de `Ocp-Apim-Subscription-Key` üstbilgi boş bırakılırsa gereklidir.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)  |Hem `appid` alan hem de `Authorization` üstbilgi boş bırakılırsa gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Kötü istek. Giriş parametrelerini ve ayrıntılı hata yanıtını denetleyin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Hata devam ederse, bize bildirin. Lütfen bize isteğin yaklaşık & saatini ve yanıt başlığında yer alan `X-MS-Trans-Info`istek kimliğini sağlayın.|
|503    |Hizmet geçici olarak kullanılamaz. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çevirmen Metin API v3'e geçir](../migrate-to-v3.md)


