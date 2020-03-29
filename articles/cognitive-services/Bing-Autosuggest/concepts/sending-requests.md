---
title: Bing Autosuggest API'sine istek gönderme
titleSuffix: Azure Cognitive Services
description: Bing Autosuggest API, arama kutusundaki kısmi sorgu dizesini temel alan önerilen sorguların listesini döndürür. İstek gönderme hakkında daha fazla bilgi edinin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: scottwhi
ms.openlocfilehash: d479548e682e814345e13d9416d08ec453f90304
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74072856"
---
# <a name="sending-requests-to-the-bing-autosuggest-api"></a>Bing Autosuggest API'sine istek gönderme.

Uygulamanız Bing Arama API'lerinden herhangi biri için sorgu gönderirse, kullanıcılarınızın arama deneyimini geliştirmek için Bing Otomatik Öner API'sini kullanabilirsiniz. Bing Autosuggest API, arama kutusundaki kısmi sorgu dizesini temel alan önerilen sorguların listesini döndürür. Karakterler uygulamanızdaki bir arama kutusuna girilirken, önerileri açılır listede görüntüleyebilirsiniz. Bu API'ye istek gönderme hakkında daha fazla bilgi edinmek için bu makaleyi kullanın. 

## <a name="bing-autosuggest-api-endpoint"></a>Bing Autosuggest API Bitiş Noktası

**Bing Autosuggest API,** kısmi bir arama teriminden önerilen sorguların listesini döndüren bir bitiş noktası içerir.

Bing API'sını kullanarak önerilen sorguları `GET` almak için aşağıdaki bitiş noktasına bir istek gönderin. Diğer belirtimleri tanımlamak için üstbilgi ve URL parametrelerini kullanın.

**Bitiş Noktası:** Kullanıcıtarafından tanımlanan girdiyle alakalı JSON sonuçları olarak arama `?q=""`önerilerini döndürür.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Üstbilgiler, parametreler, piyasa kodları, yanıt nesneleri, hatalar, vb. hakkında ayrıntılı bilgi için [Bing Autosuggest API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference) referansına bakın.

**Bing** API'leri, sonuçları türüne göre döndüren arama eylemlerini destekler.Tüm arama bitiş noktaları JSON yanıt nesneleri olarak sonuçları döndürer.
Tüm uç noktalar, boylam, enlem ve arama yarıçapına göre belirli bir dili ve/veya konumu döndüren sorguları destekler.

Her bitiş noktası tarafından desteklenen parametreler hakkında tam bilgi için her tür için başvuru sayfalarına bakın.
Otomatik Öner API'sini kullanarak temel istek örnekleri için Bkz. [Otomatik Başlatmalar' ı.](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest)

## <a name="bing-autosuggest-api-requests"></a>Bing API isteklerini otomatik leştir

> [!NOTE]
> * Bing Autosuggest API'sine yapılan istekler HTTPS protokolünü kullanmalıdır.

Tüm isteklerin bir sunucudan gönderilmesini öneririz. Anahtarı istemci uygulamasının bir parçası olarak dağıtmak, kötü amaçlı üçüncü taraf erişimi için daha fazla fırsat sağlar. Ayrıca, bir sunucudan arama yapmak gelecekteki güncelleştirmeler için tek bir yükseltme noktası sağlar.

İstek kullanıcının kısmi arama terimini içeren [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query) sorgu parametresini belirtmelidir. Tercihe bağlı olmakla birlikte, istek, sonuçların gelmesini istediğiniz pazarı tanımlayan [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#mkt) sorgu parametresini de belirtmelidir. İsteğe bağlı sorgu parametrelerinin bir listesi için bkz. [Sorgu Parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query-parameters). Tüm sorgu parametre değerleri URL olarak kodlanmış olmalıdır.

İstek [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#subscriptionkey) üstbilgisini belirtmelidir. İsteğe bağlı olmakla birlikte şu üstbilgileri de belirtmeniz önerilir:

- [Kullanıcı Aracısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#useragent)
- [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientid)
- [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientip)
- [X-Arama Yeri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#location)

İstemci IP'si ve konum üstbilgileri konuma duyarlı içerik döndürmek için önemlidir.

Tüm istek ve yanıt üstbilgilerinin bir listesi için bkz. [Üstbilgiler](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#headers).

> [!NOTE]
> JavaScript'ten Bing Autosuggest API'sini aradiğinizde, tarayıcınızın yerleşik güvenlik özellikleri bu üstbilgideğerlerine erişmenizi engelleyebilir.

Bunu çözmek için, Bing Autosuggest API isteğini bir CORS proxy'si aracılığıyla yapabilirsiniz. Böyle bir proxy'den `Access-Control-Expose-Headers` gelen yanıt, yanıt üstbilgisini beyaz listeye alan ve bunları JavaScript'te kullanılabilir kılan bir üstbilgiye sahiptir.

[Öğretici uygulamamızın](../tutorials/autosuggest.md) isteğe bağlı istemci üstbilgilerine erişmesine izin vermek için BIR CORS proxy'si yüklemek kolaydır. İlk olarak, henüz yüklemediyseniz [Node.js'yi yükleyin](https://nodejs.org/en/download/). Ardından komut istemiyle aşağıdaki komutu girin.

    npm install -g cors-proxy-server

Ardından, HTML dosyasındaki Bing Autosuggest API bitiş noktasını şu şekilde değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Son olarak, aşağıdaki komutla CORS ara sunucusunu başlatın:

    cors-proxy-server

Öğretici uygulamasını kullanırken komut penceresini açık bırakın; pencere kapatılırsa ara sunucu durdurulur. Arama sonuçlarının altındaki genişletilebilir HTTP Üst Bilgileri bölümünde artık `X-MSEdge-ClientID` üst bilgisini (diğerleriyle birlikte) görebilir ve bunun her istekte aynı olduğunu doğrulayabilirsiniz.

İstekler önerilen tüm sorgu parametrelerini ve üstbilgilerini içermelidir. 

Aşağıdaki örnekte, *sail* için önerilen sorgu dizelerini döndüren bir istek gösterilmektedir.

> ```http
> GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE
> X-MSEdge-ClientIP: 999.999.999.999
> X-Search-Location: lat:47.60357;long:-122.3295;re:100
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
> Host: api.cognitive.microsoft.com
> ```

Bing API'lerinden birini ilk kez çağırıyorsanız istemci kimliği üst bilgisini eklemeyin. İstemci kimliği üst bilgisini yalnızca önceden bir Bing API'sini çağırdıysanız ve Bing, kullanıcı ve cihaz birleşimi için bir istemci kimliği döndürdüyse dahil edin.

Aşağıdaki web öneri grubu yukarıdaki isteğe yanıttır. Grup, bir `displayText`, `query`, ve `url` alan da dahil olmak üzere her öneri ile arama sorgusu önerileri bir listesini içerir.

`displayText` alanı, arama kutunuzun açılır listesini doldurmak için kullandığınız önerilen sorguyu içerir. Yanıtın içerdiği tüm önerileri, verilen sırada görüntülemeniz gerekir.  

Kullanıcı açılır listeden bir sorgu seçerse, bing [arama API'lerinden](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/bing-api-comparison?toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fbing-autosuggest%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json) birini aramak ve sonuçları kendiniz görüntülemek için kullanabilir veya kullanıcıyı `url` döndürülen alanı kullanarak Bing sonuçları sayfasına gönderebilirsiniz.

[!INCLUDE [cognitive-services-bing-url-note](../../../../includes/cognitive-services-bing-url-note.md)]

```json
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "Suggestions",
    "queryContext" : {
        "originalQuery" : "sail"
    },
    "suggestionGroups" : [{
        "name" : "Web",
        "searchSuggestions" : [{
            "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
            "displayText" : "sailing lessons seattle",
            "query" : "sailing lessons seattle",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+moon+news&FORM=USBAPI",
            "displayText" : "sailor moon news",
            "query" : "sailor moon news",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+jack%27s+lincoln+city&FORM=USBAPI",
            "displayText" : "sailor jack's lincoln city",
            "query" : "sailor jack's lincoln city",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailing+anarchy&FORM=USBAPI",
            "displayText" : "sailing anarchy",
            "query" : "sailing anarchy",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale&FORM=USBAPI",
            "displayText" : "sailboats for sale",
            "query" : "sailboats for sale",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailstn.mylabsplus.com&FORM=USBAPI",
            "displayText" : "sailstn.mylabsplus.com",
            "query" : "sailstn.mylabsplus.com",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailusfood&FORM=USBAPI",
            "displayText" : "sailusfood",
            "query" : "sailusfood",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale+seattle&FORM=USBAPI",
            "displayText" : "sailboats for sale seattle",
            "query" : "sailboats for sale seattle",
            "searchKind" : "WebSearch"
        }]
    }]
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Bing Otomatik Öneri nedir?](../get-suggested-search-terms.md)
- [Bing Otomatik Öneri API’si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference)
- [Bing Autosuggest API'sinden önerilen arama terimlerini alma](get-suggestions.md)
