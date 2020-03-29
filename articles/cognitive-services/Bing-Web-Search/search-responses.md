---
title: Bing Web Arama API yanıt yapısı ve yanıt türleri
titleSuffix: Azure Cognitive Services
description: Bing Web Arama'yı bir arama isteği `SearchResponse` gönderdiğinde, yanıt gövdesindeki bir nesneyi döndürür.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 95ebfaef863a1fa05e8a5d3b46fca9659c61f6b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74110624"
---
# <a name="bing-web-search-api-response-structure-and-answer-types"></a>Bing Web Arama API yanıt yapısı ve yanıt türleri  

Bing Web Arama'yı bir arama isteği [`SearchResponse`](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#searchresponse) gönderdiğinde, yanıt gövdesindeki bir nesneyi döndürür. Nesne, Bing'in sorguyla alakalı olduğunu belirlediği her yanıt için bir alan içerir. Bing tüm yanıtları döndürürse, bu örnekte bir yanıt nesnesi gösteriş verilmiştir:

```json
{
    "_type": "SearchResponse",
    "queryContext": {...},
    "webPages": {...},
    "images": {...},
    "relatedSearches": {...},
    "videos": {...},
    "news": {...},
    "spellSuggestion": {...},
    "computation": {...},
    "timeZone": {...},
    "rankingResponse": {...}
}, ...
```

Genellikle Bing Web Arama, yanıtların bir alt kümesini döndürür. Örneğin, sorgu terimi *yelken dinghies*ise, yanıt `webPages` `images`içerebilir `rankingResponse`, , ve . [YanıtFiltresi'i](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#responsefilter) web sayfalarını filtrelemek için kullanmadığınız `webpages` sürece, yanıt her zaman yanıtları içerir ve `rankingResponse` yanıtlar içerir.

[!INCLUDE [cognitive-services-bing-url-note](../../../includes/cognitive-services-bing-url-note.md)]

## <a name="webpages-answer"></a>Web sayfaları yanıtlama

[WebSayfaları](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#webanswer) yanıtı, Bing Web Araması'nın sorguyla alakalı olduğunu belirlediği web sayfalarına bağlantılar listesi içerir. Listedeki her [web sayfası](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#webpage) şunlardır: sayfanın adı, url'si, ekran URL'si, içeriğin kısa bir açıklaması ve Bing'in içeriği bulduğu tarih.

```json
{
    "id": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
    "name": "Dinghy sailing",
    "url": "https:\/\/www.bing.com\/cr?IG=3A43CA5...",
    "displayUrl": "https:\/\/en.contoso.com\/wiki\/Dinghy_sailing",
    "snippet": "Dinghy sailing is the activity of sailing small boats...",
    "dateLastCrawled": "2017-04-05T16:25:00"
}, ...
```

`name` Kullanıcıyı `url` web sayfasına götüren bir köprü kullanın ve oluşturun.

<!-- Remove until this can be replaced with a sanitized version.
The following shows an example of how you might display the webpage in a search results page.

![Rendered webpage example](./media/cognitive-services-bing-web-api/bing-rendered-webpage-example.PNG)
-->

## <a name="images-answer"></a>Görüntüler yanıt

[Görüntülerin](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) yanıtı, Bing'in sorguyla ilgili olduğunu düşündüğü görüntülerin bir listesini içerir. Listedeki her [resim,](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#image) resmin URL'sini, boyutunu, boyutlarını ve kodlama biçimini içerir. Görüntü nesnesi aynı zamanda görüntü küçük resminin URL'sini ve küçük resmin boyutlarını içerir.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=3A43CA5CA64...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP....",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/upload.contoso.com\/sailing\/...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=3A43CA5CA6464....",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "http:\/\/en.contoso.com\/wiki\/File...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    }
}, ...
```

Kullanıcının cihazına bağlı olarak, genellikle küçük resimlerin bir alt kümesini görüntülersiniz ve kullanıcının kalan resimler [arasında sayfa](paging-webpages.md) ekleme seçeneği vardır.

<!-- Remove until this can be replaced with a sanitized version.
![List of thumbnail images](./media/cognitive-services-bing-web-api/bing-web-image-thumbnails.PNG)
-->

Küçük resimlerin, kullanıcı imleci üzerine getirdiğinde büyütülmesini de sağlayabilirsiniz. Büyütmeniz halinde görüntüye bağlam sağlamayı unutmayın. Örneğin, ana bilgisayarı ayıklayarak `hostPageDisplayUrl` ve görüntünün altında görüntüleyerek. Küçük resmi yeniden boyutlandırma hakkında bilgi için bkz. [Küçük Resimleri Yeniden Boyutlandırma ve Kırpma](./resize-and-crop-thumbnails.md).

<!-- Remove until this can be replaced with a sanitized version.
![Expanded view of thumbnail image](./media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Kullanıcı küçük resmi tıklatıyorsa, kullanıcıyı resimlerin kolajını içeren resimler için Bing'in arama sonuçları sayfasına götürmek için kullanın. `webSearchUrl`

Resim yanıtı ve resimler hakkında ayrıntılı bilgi için [Resim Arama API'si'ne](../bing-image-search/search-the-web.md)bakın.

## <a name="related-searches-answer"></a>İlgili aramalar cevap

[İlgili Aramalar](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#searchresponse-relatedsearches) yanıtı, diğer kullanıcılar tarafından yapılan en popüler ilgili sorguların listesini içerir. Listedeki her [sorgu,](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#query_obj) bing'in arama sonuçları sayfasına isabet`displayText`vurgulama karakterleri (`webSearchUrl`), içeren bir sorgu dizesi`text`( ), bir sorgu dizesi ve bir URL ( ) içerir.

```json
{
    "text": "dinghy racing teams",
    "displayText": "dinghy racing teams",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=96C4CF214A0..."
}, ...
```

Kullanıcıyı `displayText` ilgili sorgu `webSearchUrl` için Bing arama sonuçları sayfasına götüren bir köprü oluşturmak için sorgu dizesini ve URL'yi kullanın. Ayrıca kendi Web `text` Arama API sorgusunda sorgu dizesini kullanabilir ve sonuçları kendiniz görüntüleyebilirsiniz.

Vurgulama işaretçilerini `displayText`nasıl işleyeceğiniz hakkında bilgi için Bkz. [Hit Vurgulama.](../bing-web-search/hit-highlighting.md)

Aşağıda, Bing.com ilgili sorguların bir örneği gösterilmektedir.

![Bing'de ilgili aramalar örneği](./media/cognitive-services-bing-web-api/bing-web-rendered-relatedsearches.GIF)

## <a name="videos-answer"></a>Videolar cevap

[Video](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videos) ların yanıtı, Bing'in sorguyla alakalı olduğunu düşündüğü videoların bir listesini içerir. Listedeki her [video,](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#video) videonun URL'sini, süresini, boyutlarını ve kodlama biçimini içerir. Video nesnesi aynı zamanda video küçük resminin URL'sini ve küçük resmin boyutlarını içerir.

```json
{
    "name": "Sailing dinghy",
    "description": "Northwind Traders is a 12 foot gunter rigged...",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=1CAE739681D84...",
    "thumbnailUrl": "https:\/\/tse2.mm.bing.net\/th?id=OVP.wsKiL...",
    "datePublished": "2013-11-06T01:56:28",
    "publisher": [{
        "name": "Fabrikam"
    }],
    "contentUrl": "https:\/\/www.fabrikam.com\/watch?v=MrVBWZpJjX",
    "hostPageUrl": "https:\/\/www.bing.com\/cr?IG=1CAE739681D8400DB...",
    "encodingFormat": "mp4",
    "hostPageDisplayUrl": "https:\/\/www.fabrikam.com\/watch?v=MrBWZpJjXo",
    "width": 1280,
    "height": 720,
    "duration": "PT3M47S",
    "motionThumbnailUrl": "https:\/\/tse2.mm.bing.net\/th?id=OM.oa...",
    "embedHtml": "<iframe width=\"1280\" height=\"720\" src=\"http:\/\/www....><\/iframe>",
    "allowHttpsEmbed": true,
    "viewCount": 19089,
    "thumbnail": {
        "width": 300,
        "height": 168
    },
    "allowMobileEmbed": true,
    "isSuperfresh": false
}, ...
```

Kullanıcının cihazına bağlı olarak, genellikle kullanıcının kalan videoları görüntüleme seçeneğiyle videoların bir alt kümesini görüntülersiniz. Videonun küçük resmini videonun uzunluğu, açıklaması (adı) ve atıf (yayımcı) ile görüntülersiniz.

<!-- Remove until this can be replaced with a sanitized version.
![List of video thumbnails](./media/cognitive-services-bing-web-api/bing-web-video-thumbnails.PNG)
-->

Kullanıcı küçük resmin üzerinde gezinirken videonun küçük resim sürümünü oynatmak için kullanabilirsiniz. `motionThumbnailUrl` Hareket küçük resmini görüntülediğinizde öznitelik belirlediğinizden emin olun.

<!-- Remove until this can be replaced with a sanitized version.
![Motion thumbnail of a video](./media/cognitive-services-bing-web-api/bing-web-video-motion-thumbnail.PNG)
-->

Kullanıcı küçük resme tıklarsa, aşağıdaki video görüntüleme seçenekleri sunulur:

- Videoyu `hostPageUrl` ev sahibi web sitesinde görüntülemek için kullanın (örneğin, YouTube)
- Bing `webSearchUrl` video tarayıcısında videoyu görüntülemek için kullanın
- Videoyu `embedHtml` kendi deneyimlerinize katıştırmak için kullanın

Video yanıtı ve videoları hakkında ayrıntılı bilgi için [Video Arama API'si'ne](../bing-video-search/search-the-web.md)bakın.

## <a name="news-answer"></a>Haber yanıtı

[Haber](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#news) yanıtı, Bing'in sorguyla alakalı olduğunu düşündüğü haber makalelerinin bir listesini içerir. Listedeki her [haber makalesi](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle), makalenin adı, açıklaması ve konağın web sitesindeki makalenin URL’sini içerir. Makale bir görüntü içeriyorsa, nesne görüntünün küçük resmini içerir.

```json
{
    "name": "WC Sailing Qualifies for America Trophy with...",
    "url": "http:\/\/www.bing.com\/cr?IG=3445EEF15DAF4FFFBF7...",
    "image": {
        "contentUrl": "http:\/\/www.contoso.com\/sports\/sail...",
        "thumbnail": {
            "contentUrl": "https:\/\/www.bing.com\/th?id=ON.1...",
            "width": 400,
            "height": 272
        }
    },
    "description": "The WC sailing team qualified for a...",
    "provider": [{
        "_type": "Organization",
        "name": "contoso.com"
    }],
    "datePublished": "2017-04-16T21:56:00"
}, ...
```

Kullanıcının cihazına bağlı olarak, kullanıcının kalan makaleleri görüntüleme seçeneği içeren haber makalelerinin bir alt kümesini görüntülersiniz. Kullanıcıyı konağın sitesindeki haber makalesine götüren bir köprü bağlantı oluşturmak için `name` ve `url` kullanın. Makalede bir resim varsa, kullanarak resmi `url`tıklanabilir hale getirin. Makaleyi ilişkilendirmek için `provider` kullandığınızdan emin olun.

<!-- Remove until this can be replaced with a sanitized version.
The following shows an example of how you might display articles in a search results page.

![List of news articles](./media/cognitive-services-bing-web-api/bing-web-news-list.PNG)
-->

Haber yanıtı ve haber makaleleri hakkında ayrıntılı bilgi için [Haber Arama API'sına](../bing-news-search/search-the-web.md)bakın.

## <a name="computation-answer"></a>Hesaplama yanıtı

Kullanıcı matematiksel bir ifade veya birim dönüştürme sorgusu girerse, yanıt bir [Hesaplama](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#computation) yanıtı içerebilir. Yanıt `computation` normalleştirilmiş ifade ve sonucu içerir.

Birim dönüştürme sorgusu, bir birimi diğerine dönüştüren bir sorgudur. Örneğin, *10 metrede kaç metre mi?* *How many tablespoons in a 1/4 cup?*

Aşağıdaki 10 `computation` metre kaç metre için cevap *gösterir?*

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "10 meters",
    "value": "32.808399 feet"
}, ...
```

Aşağıda matematiksel sorgu örnekleri ve bunların `computation` karşılık gelen cevapları gösterilmektedir.

```
Query: (5+3)(10/2)+8
Encoded query: %285%2B3%29%2810%2F2%29%2B8
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "((5+3)*(10\/2))+8",
    "value": "48"
}
```

```
Query: sqrt(4^2+8^2)
Encoded query: sqrt%284^2%2B8^2%29
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "sqrt((4^2)+(8^2))",
    "value": "8.94427191"
}
```

```
Query: 30 6/8 - 18 8/16
Encoded query: 30%206%2F8%20-%2018%208%2F16
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#WolframAlpha",
    "expression": "30 6\/8-18 8\/16",
    "value": "12.25"
}
```

```
Query: 8^2+11^2-2*8*11*cos(37)
Encoded query: 8^2%2B11^2-2*8*11*cos%2837%29
```

```json
"computation": {
        "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
        "expression": "(8^2)+(11^2)-(2*8*11*cos(37))",
        "value": "44.4401502"
}
```

Matematiksel bir ifade aşağıdaki sembolleri içerebilir:

|Sembol|Açıklama|
|------------|-----------------|
|+|Toplama|
|-|Çıkarma|
|/|Bölme|
|*|Çarpma|
|^|Üs|
|!|Faktöriyel|
|.|Ondalık|
|()|Öncelik gruplandırma|
|[]|İşlev|

Matematiksel bir ifade aşağıdaki sabitleri içerebilir:

|Sembol|Açıklama|
|------------|-----------------|
|Pi|3.14159...|
|Derece|Derece|
|ı|Hayali numara|
|e|e, 2.71828...|
|Altın Oran|Altın oran, 1.61803...|

Matematiksel bir ifade aşağıdaki işlevleri içerebilir:

|Sembol|Açıklama|
|------------|-----------------|
|Sırala|Karekök|
|Günah[x], Cos[x], Tan[x]<br />Csc[x], Sec[x], Karyola[x]|Trigonometrik fonksiyonlar (radyanlarda bağımsız değişkenler ile)|
|ArcSin[x], ArcCos[x], ArcTan[x]<br />ArcCsc[x], ArcSec[x], ArcCot[x]|Ters trigonometrik fonksiyonlar (radyanlarda sonuç verme)|
|Exp[x], E^x|Üstel fonksiyon|
|Günlük[x]|Doğal logaritma|
|Sinh[x], Cosh[x], Tanh[x]<br />Csch[x], Sech[x], Coth[x]|Hiperbolik fonksiyonlar|
|ArcSinh[x], ArcCosh[x], ArcTanh[x]<br />ArcCsch[x], ArcSech[x], ArcCoth[x]|Ters hiperbolik fonksiyonlar|

Değişkenleri içeren matematiksel ifadeler (örneğin, x'in değişken olduğu 4x+6=18) desteklenmez.

## <a name="timezone-answer"></a>TimeZone yanıtı

Kullanıcı bir saat veya tarih sorgusu girerse, yanıt bir [TimeZone](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#timezone) yanıtı içerebilir. Bu yanıt örtülü veya açık sorguları destekler. *Saat ne*gibi örtülü bir sorgu, kullanıcının konumuna bağlı olarak yerel saati döndürür. *Seattle'da saat ne zaman*olduğu gibi açık bir sorgu, Seattle, WA için yerel saati döndürür.

Yanıt, `timeZone` konumun adını, belirtilen konumdaki geçerli UTC tarih ve saatini ve UTC mahsupını sağlar. Konumun sınırı birden çok saat dilimi içindeyse, yanıt sınır içindeki tüm saat dilimlerinin geçerli UTC tarih ve saatini içerir. Örneğin, Florida Eyaleti iki saat dilimi içinde yer aldığı için, yanıt her iki saat diliminin yerel tarih ve saatini içerir.  

Sorgu bir eyaletin veya ülkenin/bölgenin saatini isterse, Bing konumun coğrafi sınırları içindeki birincil şehri `primaryCityTime` belirler ve onu alanında döndürür. Sınır birden çok saat dilimi içeriyorsa, kalan `otherCityTimes` saat dilimleri alanda döndürülür.

Aşağıdaki `timeZone` yanıtı döndüren örnek sorguları gösterir.

```
Query: What time is it?

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Redmond, Washington, United States",
        "time": "2015-10-27T08:38:12.1189231Z",
        "utcOffset": "UTC-7"
    }
}

Query: What time is it in the Pacific time zone?

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Pacific Time Zone",
        "time": "2015-10-23T12:33:19.0728146Z",
        "utcOffset": "UTC-7"
    }
}

Query: Time in Florida?

"timeZone": {
        "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
        "primaryCityTime": {
            "location": "Tallahassee, Florida, United States",
            "time": "2015-10-23T13:04:56.6774389Z",
            "utcOffset": "UTC-4"
        },
        "otherCityTimes": [{
            "location": "Pensacola",
            "time": "2015-10-23T12:04:56.6664294Z",
            "utcOffset": "UTC-5"
        }]
}

Query: What time is it in the U.S.

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Washington, D.C., United States",
        "time": "2015-10-23T15:27:59.8892745Z",
        "utcOffset": "UTC-4"
    },
    "otherCityTimes": [{
        "location": "Honolulu",
        "time": "2015-10-23T09:27:59.8892745Z",
        "utcOffset": "UTC-10"
    },
    {
        "location": "Anchorage",
        "time": "2015-10-23T11:27:59.8892745Z",
        "utcOffset": "UTC-8"
    },
    {
        "location": "Phoenix",
        "time": "2015-10-23T12:27:59.8892745Z",
        "utcOffset": "UTC-7"
    },
    {
        "location": "Los Angeles",
        "time": "2015-10-23T12:27:59.8942788Z",
        "utcOffset": "UTC-7"
    },
    {
        "location": "Denver",
        "time": "2015-10-23T13:27:59.8812681Z",
        "utcOffset": "UTC-6"
    },
    {
        "location": "Chicago",
        "time": "2015-10-23T14:27:59.8892745Z",
        "utcOffset": "UTC-5"
    }]
}
```

## <a name="spellsuggestion-answer"></a>SpellSuggestion yanıtı

Bing, kullanıcının farklı bir şey aramayı planladığını belirlerse, yanıt bir [Yazım Önerileri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#spellsuggestions) nesnesi içerir. Örneğin, kullanıcı *carlos kalemi*ararsa Bing, kullanıcının carlos Pena'yı arama yı planladığını *(carlos pen'in*diğer kişiler tarafından yapılan geçmiş aramalarına göre) belirleyebilir. Aşağıdaki bir örnek yazım yanıtı gösterir.

```json
"spellSuggestions": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#SpellSuggestions",
    "value": [{
        "text": "carlos pena",
        "displayText": "carlos pena"
    }]
}, ...
```

## <a name="response-headers"></a>Yanıt üst bilgileri

Bing Web Arama API'sinden gelen yanıtlar aşağıdaki üstbilgileri içerebilir:

|||
|-|-|
|`X-MSEdge-ClientID`|Bing'in kullanıcıya atadığı benzersiz kimlik|
|`BingAPIs-Market`|Talebi yerine getirmek için kullanılan pazar|
|`BingAPIs-TraceId`|Bu istek için Bing API sunucusundaki günlük girişi (destek için)|

İstemci kimliğini sürdürmek ve sonraki isteklerle iade etmek özellikle önemlidir. Bunu yaptığınızda, arama, sıralama arama sonuçlarında geçmiş bağlamı kullanır ve tutarlı bir kullanıcı deneyimi sağlar.

Ancak, JavaScript'ten Bing Web Arama API'sini aradığınızda, tarayıcınızın yerleşik güvenlik özellikleri (CORS) bu üstbilgi değerlerine erişmenizi engelleyebilir.

Üstbilgiye erişmek için, Bing Web Arama API isteğini bir CORS proxy'si aracılığıyla yapabilirsiniz. Böyle bir ara sunucudan gelen yanıtta, yanıt üst bilgilerini beyaz listeye alan ve JavaScript’in kullanımına sunan `Access-Control-Expose-Headers` üst bilgisi bulunur.

[Öğretici uygulamamızın](tutorial-bing-web-search-single-page-app.md) isteğe bağlı istemci üstbilgilerine erişmesine izin vermek için BIR CORS proxy'si yüklemek kolaydır. İlk olarak, henüz yüklemediyseniz [Node.js'yi yükleyin](https://nodejs.org/en/download/). Ardından komut istemiyle aşağıdaki komutu girin.

    npm install -g cors-proxy-server

Ardından, HTML dosyasındaki Bing Web Arama API bitiş noktasını şu şekilde değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Son olarak, aşağıdaki komutla CORS ara sunucusunu başlatın:

    cors-proxy-server

Öğretici uygulamasını kullanırken komut penceresini açık bırakın; pencere kapatılırsa ara sunucu durdurulur. Arama sonuçlarının altındaki genişletilebilir HTTP Üst Bilgileri bölümünde artık `X-MSEdge-ClientID` üst bilgisini (diğerleriyle birlikte) görebilir ve bunun her istekte aynı olduğunu doğrulayabilirsiniz.

## <a name="response-headers-in-production"></a>Üretimdeki yanıt başlıkları

Önceki yanıtta açıklanan CORS proxy yaklaşımı geliştirme, test ve öğrenme için uygundur.

Üretim ortamında, Bing Web Arama API'sını kullanan Web sayfasıyla aynı etki alanında sunucu tarafında bir komut dosyası barındırmanız gerekir. Bu komut dosyası, Web sayfası JavaScript'ten istek üzerine API çağrıları yapmalı ve üstbilgi de dahil olmak üzere tüm sonuçları istemciye geri aktarmalıdır. İki kaynak (sayfa ve komut dosyası) bir kaynağı paylaştığından, CORS kullanılmaz ve özel üstbilgiler Web sayfasındaki JavaScript'e erişebilir.

Bu yaklaşım, API anahtarınızı yalnızca sunucu tarafındaki komut dosyasının buna ihtiyacı olduğundan, herkese açık hale gelmekten de korur. Komut dosyası, isteğin yetkilendirildiğinden emin olmak için başlık bir yöntem kullanabilir.

Aşağıda Bing'in yazım önerisini nasıl kullandığı gösterilmektedir.

![Bing yazım önerisi örneği](./media/cognitive-services-bing-web-api/bing-web-spellingsuggestion.GIF)  

## <a name="next-steps"></a>Sonraki adımlar  

* İstek azaltma belgelerini gözden [geçirin.](throttling-requests.md)  

## <a name="see-also"></a>Ayrıca bkz.  

* [Bing Web Arama API başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference)
