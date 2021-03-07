---
title: Anlamsal sorgu oluşturma
titleSuffix: Azure Cognitive Search
description: Derin öğrenme modellerini, arama derecesi ve ilgisi kapsamında bir amaç ve bağlamı göstermek üzere sorgu işlemeye eklemek için anlamsal bir sorgu türü ayarlayın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/05/2021
ms.openlocfilehash: 8fdb6a53ed0fd64953b75238c3ba3df62c4b644e
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102432953"
---
# <a name="create-a-semantic-query-in-cognitive-search"></a>Bilişsel Arama anlam sorgusu oluşturma

> [!IMPORTANT]
> Anlam sorgu türü, önizleme REST API ve Azure portal aracılığıyla kullanılabilen genel önizlemede bulunur. Önizleme özellikleri, olduğu gibi, [ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)altında sunulur. İlk önizleme başlatma sırasında, semantik arama için ücret alınmaz. Daha fazla bilgi için bkz. [kullanılabilirlik ve fiyatlandırma](semantic-search-overview.md#availability-and-pricing).

Bu makalede, anlam derecelendirmesi kullanan bir arama isteğini nasıl formülleyeceğinizi ve anlam başlıkları ve yanıtları üretir.

Anlamsal sorgular, PDF 'Ler veya büyük parçalar içeren belgeler gibi metin açısından ağır içerikten oluşturulan arama dizinlerinde en iyi şekilde çalışır.

## <a name="prerequisites"></a>Önkoşullar

+ Şu bölgelerden birinde bulunan standart katmanda (S1, S2, S3) bir arama hizmeti: Orta Kuzey ABD, Batı ABD, Batı ABD 2, Doğu ABD 2, Kuzey Avrupa, Batı Avrupa. Bu bölgelerden birinde mevcut bir S1 veya daha fazla hizmetiniz varsa, yeni bir hizmet oluşturmak zorunda kalmadan erişim isteyebilirsiniz.

+ Anlamsal arama önizlemesine erişim: [kaydolma](https://aka.ms/SemanticSearchPreviewSignup)

+ Ingilizce içerik içeren mevcut bir arama dizini

+ Sorgu göndermek için bir arama istemcisi

  Arama istemcisinin, sorgu isteğinde önizleme REST API 'Leri desteklemesi gerekir. Önizleme API 'Lerine GERI çağrı yapmak için değiştirdiğiniz [Postman](search-get-started-rest.md), [Visual Studio Code](search-get-started-vs-code.md)veya kodu kullanabilirsiniz. Bir anlamsal sorgu göndermek için Azure portal [Arama Gezgini](search-explorer.md) 'ni de kullanabilirsiniz.

+ Bir [Arama belgesi](/rest/api/searchservice/preview-api/search-documents) , bu makalede açıklanan anlam seçeneği ve diğer parametrelerle istek ister.

## <a name="whats-a-semantic-query"></a>Anlamsal sorgu nedir?

Bilişsel Arama bir sorgu, sorgu işlemeyi ve yanıtın şeklini belirleyen parametreli bir istek olur. Bir *anlam sorgusu* , eşleşen sonuçların kapsamını ve anlamını değerlendirebilen, daha fazla ilgili eşleştirmeyi en üst düzeye yükseltebilen ve anlam yanıtlarını ve açıklamalı alt yazıları döndüren anlam yeniden yönlendirme modelini çağıran parametreler ekler.

Aşağıdaki istek, temel bir anlam sorgusunun temsilcisidir (yanıt olmadan).

```http
POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=2020-06-30-Preview      
{    
    "search": " Where was Alan Turing born?",    
    "queryType": "semantic",  
    "searchFields": "title,url,body",  
    "queryLanguage": "en-us"  
}
```

Bilişsel Arama tüm sorgularda olduğu gibi, istek tek bir dizinin belgeler koleksiyonunu hedefler. Ayrıca, semantik bir sorgu, semantik olmayan bir sorgu olarak aynı ayrıştırma, analiz ve tarama sırasıyla aynı sırada gider. Fark, ilgiye göre hesaplanır. Bu önizleme sürümünde tanımlandığı gibi, anlam sorgusu, varsayılan benzerlik derecelendirmesi algoritması tarafından atanan puanlar yerine anlam derecelendirmesiyle ilgili en iyi sonucu veren bir yol sağlayarak, gelişmiş algoritmalar kullanılarak *sonuçlar* yeniden işlenir. 

Başlangıçtaki sonuçlardan yalnızca ilk 50 eşleşme anlam olarak derecelendirilir ve yanıtta tüm başlıklar dahil edilebilir. İsteğe bağlı olarak, olası bir **`answer`** yanıtı ayıklamak için istekte bir parametre belirtebilirsiniz. Bu model, arama sayfasının en üstünde işlemeyi seçebileceğiniz, sorguya yönelik beş olası yanıtı ifade edebilir.

## <a name="query-using-rest-apis"></a>REST API 'Leri kullanarak sorgulama

REST API tam belirtimi, [Arama belgelerinde (REST önizlemesi)](/rest/api/searchservice/preview-api/search-documents)bulunabilir.

Anlamsal sorgular, otomatik olarak açıklamalı alt yazılar ve vurgulama sağlar. Yanıtın yanıt içermesini istiyorsanız, isteğe isteğe bağlı bir **`answer`** parametre ekleyebilirsiniz. Bu parametre ve sorgu dizesinin oluşturulması, yanıtta bir yanıt oluşturacak.

Aşağıdaki örnek, semantik yanıtlar ve açıklamalı alt yazılar ile anlamsal bir sorgu isteği oluşturmak için oteller-Sample-Index ' i kullanır:

```http
POST https://[service name].search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview      
{
    "search": "newer hotel near the water with a great restaurant",
    "queryType": "semantic",
    "queryLanguage": "en-us",
    "searchFields": "HotelName,Category,Description",
    "speller": "lexicon",
    "answers": "extractive|count-3",
    "highlightPreTag": "<strong>",
    "highlightPostTag": "</strong>",
    "select": "HotelId,HotelName,Description,Category",
    "count": true
}
```

### <a name="formulate-the-request"></a>İsteği formül altına yaz

Bu bölüm, semantik arama için gerekli olan sorgu parametrelerinin adımlarını izleyin.

#### <a name="step-1-set-querytype-and-querylanguage"></a>1. Adım: queryType ve queryLanguage ayarlama

Rest 'e aşağıdaki parametreleri ekleyin. Her iki parametre de gereklidir.

```json
"queryType": "semantic",
"queryLanguage": "en-us",
```

QueryLanguage, dizin şemasında alan tanımlarına atanan [dil Çözümleyicileri](index-add-language-analyzers.md) ile tutarlı olmalıdır. QueryLanguage "en-US" ise, herhangi bir dil Çözümleyicileri de Ingilizce bir varyant ("en. Microsoft" veya "en. Lucene") olmalıdır. Anahtar sözcük veya basit gibi dilden bağımsız çözümleyiciler, queryLanguage değerleriyle çelişmez.

Bir sorgu isteğinde, [Yazım denetimi](speller-how-to-add.md)de kullanıyorsanız, ayarladığınız QueryLanguage, yazım denetleyicisi, yanıtlar ve açıklamalı alt yazılar için de geçerlidir. Ayrı parçalar için geçersiz kılma yoktur. 

Bir arama dizinindeki içerik birden çok dilde birleştirileken, sorgu girişi büyük olasılıkla birdir. Arama altyapısı, queryLanguage, Language Analyzer ve içeriğin oluştuğu dilin uyumluluğunu kontrol etmez, bu nedenle yanlış sonuçlar üretmemek için sorguları uygun şekilde kapsamdığınızdan emin olun.

<a name="searchfields"></a>

#### <a name="step-2-set-searchfields"></a>2. Adım: searchFields ayarlama

Bu parametre, bu parametreyi dışarıda bıraktığınızda bir hata olmadığı için isteğe bağlıdır, ancak her iki başlık ve yanıt için bir dizi alanın Sıralı bir listesini sağlamak kesinlikle önerilir.

Searchfields parametresi, sorguya "anlamsal benzerlik" için değerlendirilecek olan metinlerin belirlemek için kullanılır. Önizleme için, modelin işlemek için en önemli olan alanlar için bir ipucu gerektirdiğinden, searchFields 'in boş bırakılması önerilmez.

SearchFields sırası kritik öneme sahiptir. Zaten var olan basit veya tam Lucene sorgularında searchFields kullanıyorsanız, anlamsal bir sorgu türüne geçiş yaparken bu parametreyi geri ziyaret ettiğinizden emin olun.

İki veya daha fazla searchFields belirtildiğinde en iyi sonuçları sağlamak için bu yönergeleri izleyin:

+ Koleksiyonlardaki yalnızca dize alanlarını ve en üst düzey dize alanlarını dahil edin. Bir koleksiyonda dize olmayan alanları veya alt düzey alanları dahil etmek için bir hata yoktur, ancak bu alanlar semantik derecelendirmeden kullanılmaz.

+ İlk alan, ideal olarak 25 kelimeyle, her zaman kısa (bir başlık veya ad gibi) olmalıdır.

+ Dizinde metin olan bir URL alanı (örneğin, gibi `www.domain.com/name-of-the-document-and-other-details` makine odaklı gibi okunabilir `www.domain.com/?id=23463&param=eis` ) varsa, bunu bir yere yerleştirin (veya bir kısa başlık alanı yoksa önce).

+ Bir belgenin ana içeriği gibi anlam sorgularının yanıtının bulunabileceği açıklayıcı alanlarla bu alanları izleyin.

Yalnızca bir alan belirtilmişse, anlam sorgularının yanıtının bulunduğu bir belgenin ana içeriği gibi açıklayıcı bir alan kullanın. Yeterli içerik sağlayan bir alan seçin.

#### <a name="step-3-remove-orderby-clauses"></a>3. Adım: orderBy yan tümcelerini kaldırma

Mevcut bir istekte varsa, herhangi bir orderBy yan tümcesini kaldırın. Anlamsal puan sonuçları sıralamak için kullanılır ve açık sıralama mantığı eklerseniz bir HTTP 400 hatası döndürülür.

#### <a name="step-4-add-answers"></a>4. Adım: Yanıt ekleme

İsteğe bağlı olarak, yanıt sağlayan ek işlemleri eklemek istiyorsanız "yanıtlar" ı ekleyin. Yanıtlar (ve açıklamalı alt yazılar), searchfields bölümünde listelenen alanlarda bulunan metinlerin 'lerden alınmıştır. Bir yanıtta en iyi yanıtları ve açıklamalı alt yazıları almak için, searchFields içeriğine zengin içerik alanları eklediğinizden emin olun.

Yanıtları üreten açık ve örtük koşullar vardır. 

+ Açık koşullara "cevaplar = extractive" eklenmesi dahildir. Ayrıca, genel yanıtta döndürülen yanıt sayısını belirtmek için, "Count" sözcüğünü ve ardından bir sayıyı ekleyin: `"answers=extractive|count=3"` .  Varsayılan değer bir. En fazla beş.

+ Örtülü koşullar bir yanıt için kendisini hedefleyen bir sorgu dizesi oluşturmayı içerir. ' Hangi otel 'nin yeşil odasının bulunduğu bir sorgu daha büyük olasılıkla "Cevaplanan iç" gibi bir deyimden oluşan bir sorgudan "yanıtlanır". Bekleneceğiniz gibi sorgu belirtilmemiş veya null olamaz.

Dikkat edilmesi gereken önemli nokta, sorgu bir soru gibi görünmüyorsa, "yanıtlar" parametresi ayarlanmış olsa bile yanıt işleme atlanır.

#### <a name="step-5-add-other-parameters"></a>5. Adım: başka parametreler ekleme

İstekte istediğiniz diğer parametreleri ayarlayın. [Yazım denetleyicisi](speller-how-to-add.md), [seçme](search-query-odata-select.md)ve sayı gibi parametreler, yanıtın kalitesini ve okunabilirliğini artırır.

İsteğe bağlı olarak, açıklamalı alt yazıların uygulanan vurgu stilini özelleştirebilirsiniz. Açıklamalı alt yazılar, yanıtı özetleyen belgedeki anahtar paslar üzerinde vurgu biçimlendirme uygular. Varsayılan değer: `<em>`. Biçimlendirme türünü (örneğin, sarı arka plan) belirtmek istiyorsanız, highlightPreTag ve highlightPostTag ' i ayarlayabilirsiniz.

### <a name="review-the-response"></a>Yanıtı gözden geçirin

Yukarıdaki sorgu için yanıt, en üstteki seçim olarak aşağıdaki eşleşmeyi döndürür. Açıklamalı alt yazılar, düz metin ve vurgulanmış sürümlerle otomatik olarak döndürülür. Anlam yanıtları hakkında daha fazla bilgi için bkz. [anlam derecelendirmesi ve yanıtlar](semantic-how-to-query-response.md).

```json
"@odata.count": 29,
"value": [
    {
        "@search.score": 1.8920634,
        "@search.rerankerScore": 1.1091284966096282,
        "@search.captions": [
            {
                "text": "Oceanside Resort. Budget. New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
                "highlights": "<strong>Oceanside Resort.</strong> Budget. New Luxury Hotel. Be the first to stay.<strong> Bay views</strong> from every room, location near the pier, rooftop pool, waterfront dining & more."
            }
        ],
        "HotelId": "18",
        "HotelName": "Oceanside Resort",
        "Description": "New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
        "Category": "Budget"
    },
```

### <a name="parameters-used-in-a-semantic-query"></a>Anlam sorgusunda kullanılan parametreler

Aşağıdaki tablo, bir anlamsal sorgu için kullanılan sorgu parametrelerini özetler, böylece onları holistik olarak görebilirsiniz. Tüm parametrelerin listesi için bkz. [arama belgeleri (REST önizlemesi)](/rest/api/searchservice/preview-api/search-documents)

| Parametre | Tür | Açıklama |
|-----------|-------|-------------|
| queryType | Dize | Geçerli değerler basit, tam ve anlam içerir. Anlam sorguları için "anlam" değeri gereklidir. |
| Sorgu dili | Dize | Anlam sorguları için gereklidir. Şu anda yalnızca "en-US" uygulandı. |
| searchFields | Dize | Aranabilir alanların virgülle ayrılmış listesi. İsteğe bağlı ancak önerilir. Anlam derecelendirmenin gerçekleştiği alanları belirtir. </br></br>Basit ve tam sorgu türlerinin aksine, alanların listelenme sırası öncelik belirler.|
| acağınız |Dize | Anlam yanıtlarının sonuca dahil edilip edilmeyeceğini belirtmek için isteğe bağlı alan. Şu anda yalnızca "extractive" uygulandı. Yanıtlar en fazla beş olacak şekilde yapılandırılabilir. Bu örnek "extractive|count3 "', üç yanıt sayısını gösterir. Varsayılan değer 1'dir.|

## <a name="query-with-search-explorer"></a>Arama gezgini ile sorgulama

Aşağıdaki sorgu yerleşik oteller örnek dizinini, API sürüm 2020-06-30-önizleme kullanarak ve arama Gezgini 'nde çalışır durumda hedefler. `$select`Yan tümce, sonuçları yalnızca birkaç alana kısıtlar ve bu, arama Gezgini 'nde AYRıNTıLı JSON 'da taramayı kolaylaştırır.

### <a name="with-querytypesemantic"></a>QueryType = anlam ile

```json
search=I want a nice hotel on the water with a great restaurant&$select=HotelId,HotelName,Description,Tags&queryType=semantic&queryLanguage=english&searchFields=Description,Tags
```

İlk birkaç sonuç aşağıdaki gibidir.

```json
{
    "@search.score": 0.38330218,
    "@search.rerankerScore": 0.9754053303040564,
    "HotelId": "18",
    "HotelName": "Oceanside Resort",
    "Description": "New Luxury Hotel. Be the first to stay. Bay views from every room, location near the pier, rooftop pool, waterfront dining & more.",
    "Tags": [
        "view",
        "laundry service",
        "air conditioning"
    ]
},
{
    "@search.score": 1.8920634,
    "@search.rerankerScore": 0.8829904259182513,
    "HotelId": "36",
    "HotelName": "Pelham Hotel",
    "Description": "Stunning Downtown Hotel with indoor Pool. Ideally located close to theatres, museums and the convention center. Indoor Pool and Sauna and fitness centre. Popular Bar & Restaurant",
    "Tags": [
        "view",
        "pool",
        "24-hour front desk service"
    ]
},
{
    "@search.score": 0.95706713,
    "@search.rerankerScore": 0.8538530203513801,
    "HotelId": "22",
    "HotelName": "Stone Lion Inn",
    "Description": "Full breakfast buffet for 2 for only $1.  Excited to show off our room upgrades, faster high speed WiFi, updated corridors & meeting space. Come relax and enjoy your stay.",
    "Tags": [
        "laundry service",
        "air conditioning",
        "restaurant"
    ]
},
```

### <a name="with-querytype-default"></a>QueryType ile (varsayılan)

Karşılaştırma için, öğesini kaldırarak aynı sorguyu çalıştırın `&queryType=semantic&queryLanguage=english&searchFields=Description,Tags` . `"@search.rerankerScore"`Bu sonuçlarda Hayır olduğuna ve farklı otelların ilk üç konumda göründüğünü unutmayın.

```json
{
    "@search.score": 8.633856,
    "HotelId": "3",
    "HotelName": "Triple Landscape Hotel",
    "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
    "Tags": [
        "air conditioning",
        "bar",
        "continental breakfast"
    ]
},
{
    "@search.score": 6.407289,
    "HotelId": "40",
    "HotelName": "Trails End Motel",
    "Description": "Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
    "Tags": [
        "continental breakfast",
        "view",
        "view"
    ]
},
{
    "@search.score": 5.843788,
    "HotelId": "14",
    "HotelName": "Twin Vertex Hotel",
    "Description": "New experience in the Making.  Be the first to experience the luxury of the Twin Vertex. Reserve one of our newly-renovated guest rooms today.",
    "Tags": [
        "bar",
        "restaurant",
        "air conditioning"
    ]
},
```

## <a name="next-steps"></a>Sonraki adımlar

Anlam derecelendirmenin ve yanıtlarının bir ilk sonuç kümesi üzerinden oluşturulup oluşturulabileceğini hatırlayın. İlk sonuçların kalitesini artıran herhangi bir mantık, anlam aramasına ileri doğru ilerlecektir. Sonraki adım olarak, dizelerin nasıl simgeleştirilmiş olduğunu etkileyen çözümleyiciler, sonuçları ayarlayabilen Puanlama profilleri ve varsayılan ilgi algoritması gibi ilk sonuçlara katkıda bulunan özellikleri gözden geçirin.

+ [Metin işleme için çözümleyiciler](search-analyzers.md)
+ [Bilişsel Arama benzerlik ve Puanlama](index-similarity-and-scoring.md)
+ [Puanlama profili ekleme](index-add-scoring-profiles.md)
+ [Anlamsal aramaya genel bakış](semantic-search-overview.md)
+ [Sorgu koşullarına yazım denetimi Ekle](speller-how-to-add.md)
