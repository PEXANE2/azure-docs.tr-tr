---
title: Sorgu türleri ve oluşturma
titleSuffix: Azure Cognitive Search
description: Sonuçları filtrelemek, seçmek ve sıralamak için parametreleri kullanarak Azure Bilişsel Arama'da bir arama sorgusu oluşturmanın temel leri.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 902f3628235cc8a4524ddc4dd8a5327592fe47e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282828"
---
# <a name="query-types-and-composition-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da sorgu türleri ve kompozisyon

Azure Bilişsel Arama'da sorgu, gidiş-dönüş işleminin tam bir belirtimidir. İstekteki parametreler, dahil edilecek veya dışlanacağı bir dizinde belge bulmak için eşleşme ölçütleri, motora geçirilen yürütme yönergelerini ve yanıtı şekillendirme yönergelerini sağlar. Belirtilmemiş`search=*`( ), sorgu, tam metin arama işlemi olarak tüm aranabilir alanlara karşı çalışır ve rasgele sırada ayarlanmış puansız bir sonucu döndürer.

Aşağıdaki örnek, [REST API'sinde](https://docs.microsoft.com/rest/api/searchservice/search-documents)oluşturulmuş bir temsilci sorgusudur. Bu örnek, [otellerin demo dizinini](search-get-started-portal.md) hedefler ve ortak parametreleri içerir.

```
{
    "queryType": "simple" 
    "search": "+New York +restaurant",
    "searchFields": "Description, Address/City, Tags",
    "select": "HotelId, HotelName, Description, Rating, Address/City, Tags",
    "top": "10",
    "count": "true",
    "orderby": "Rating desc"
}
```

+ **`queryType`**[varsayılan basit sorgu arayıcısı](search-query-simple-examples.md) (tam metin arama için en uygun) veya normal ifadeler, yakınlık araması, bulanık ve joker karakter araması gibi gelişmiş sorgu yapıları için kullanılan [tam Lucene sorgu aracısını](search-query-lucene-examples.md) birkaç ını adlandırmak için ayrıştırıcıyı ayarlar.

+ **`search`** maç kriterleri, genellikle metin sağlar ama genellikle boolean operatörleri eşliğinde. Tek tek terimler *terim* sorgularıdır. Teklif le kapatılan çok parçalı sorgular *anahtar tümcecik* sorgularıdır. Arama, **`search=*`** olduğu gibi tanımlanmamış olabilir, ancak daha büyük olasılıkla örnekte görünene benzer terimler, tümcecikler ve işleçlerden oluşur.

+ **`searchFields`** sorgu yürütmesini belirli alanlara kısıtlar. Dizin şemasında *aranabilir* olarak atfedilen herhangi bir alan bu parametre için bir adaydır.

Yanıtlar, sorguya eklediğiniz parametrelere göre de şekillenir. Örnekte, sonuç kümesi ekstrede **`select`** listelenen alanlardan oluşur. Yalnızca *alınabilir* olarak işaretlenmiş alanlar $select bir bildirimde kullanılabilir. Ayrıca, bu **`top`** sorguda yalnızca 10 isabet **`count`** döndürülür ve toplam da kaç belgenin eşleşeceğini ve bu da döndürülenlerden daha fazla olabileceğini söyler. Bu sorguda, satırlar azalan sırada Derecelendirmeye göre sıralanır.

Azure Bilişsel Arama'da sorgu yürütme, istekte sağlanan bir api anahtarı kullanılarak kimlik doğrulaması yapılan tek bir dizinle her zaman karşıdır. REST'te, her ikisi de istek üstbilgisinde sağlanır.

### <a name="how-to-run-this-query"></a>Bu sorgu nasıl çalıştırılalır?

Bu sorguyu yürütmek için [Arama gezgini ve oteller demo dizinini](search-get-started-portal.md)kullanın. 

Bu sorgu dizesini gezginin arama çubuğuna yapıştırabilirsiniz:`search=+"New York" +restaurant&searchFields=Description, Address/City, Tags&$select=HotelId, HotelName, Description, Rating, Address/City, Tags&$top=10&$orderby=Rating desc&$count=true`

## <a name="how-query-operations-are-enabled-by-the-index"></a>Sorgu işlemleri dizin tarafından nasıl etkinleştirilir?

Dizin tasarımı ve sorgu tasarımı, Azure Bilişsel Arama'da sıkı bir şekilde birleştirilmiştir. Ön bilmek için önemli bir gerçek, *dizin şema*, her alanda öznitelikleri ile, oluşturabilirsiniz sorgu türünü belirler. 

Bir alandaki dizin öznitelikleri, izin verilen işlemleri ayarlar - bir alanın dizinde *aranabilir* olup olmadığı, sonuçlarda *alınAbilen,* *sıralanabilir,* *filtrelenebilir,* vb. Örnek sorgu dizesinde, `"$orderby": "Rating"` yalnızca Derecelendirme alanı dizin şemasında *sıralanabilir* olarak işaretli olduğundan çalışır. 

![Otel numunesi için dizin tanımı](./media/search-query-overview/hotel-sample-index-definition.png "Otel numunesi için dizin tanımı")

Yukarıdaki ekran görüntüsü, otel numunesi için dizin özniteliklerinin kısmi bir listesidir. Portaldaki tüm dizin şeasını görüntüleyebilirsiniz. Dizin öznitelikleri hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/create-index)

> [!Note]
> Bazı sorgu işlevleri, alan başına değil, dizin genelinde etkinleştirilir. Bu özellikler şunlardır: [eşanlamlı haritalar,](search-synonyms.md) [özel çözümleyiciler,](index-add-custom-analyzers.md) [önerici yapılar (otomatik tamamlama ve önerilen sorgular için)](index-add-suggesters.md), [sıralama sonuçları için mantık puanlama](index-add-scoring-profiles.md).

## <a name="elements-of-a-query-request"></a>Sorgu isteğinin öğeleri

Sorgular her zaman tek bir dizin yönlendirilir. Dizinlere katılamaz veya sorgu hedefi olarak özel veya geçici veri yapıları oluşturamazsınız. 

Sorgu isteğinde gerekli öğeler aşağıdaki bileşenleri içerir:

+ Sabit ve kullanıcı tanımlı bileşenler içeren bir URL olarak ifade edilen hizmet bitiş noktası ve dizin belgeleri koleksiyonu:**`https://<your-service-name>.search.windows.net/indexes/<your-index-name>/docs`**
+ **`api-version`**(Yalnızca REST) gereklidir, çünkü API'nin birden fazla sürümü her zaman kullanılabilir. 
+ **`api-key`**, bir sorgu veya yönetici api anahtarı, hizmetinize istek doğrular.
+ **`queryType`**, dahili varsayılan basit sözdizimini kullanıyorsanız atlanabilir basit veya tam.
+ **`search`** veya **`filter`** boş bir arama yapmak istiyorsanız belirtilmeyen eşleşme ölçütlerini sağlar. Her iki sorgu türü de basit ayrıştırıcı açısından tartışılır, ancak gelişmiş sorgular bile karmaşık sorgu ifadelerini geçirmek için arama parametresi gerektirir.

Diğer tüm arama parametreleri isteğe bağlıdır. Özniteliklerin tam listesi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/create-index) İşlem sırasında parametrelerin nasıl kullanıldığına daha yakından bakmak için [Azure Bilişsel Arama'da tam metin aramanın nasıl çalıştığını](search-lucene-query-architecture.md)görün.

## <a name="choose-apis-and-tools"></a>API'leri ve araçları seçin

Aşağıdaki tabloda sorgu göndermek için API'ler ve araç tabanlı yaklaşımlar listelenir.

| Yöntem | Açıklama |
|-------------|-------------|
| [Arama gezgini (portal)](search-explorer.md) | Dizin ve api sürümü seçimleri için bir arama çubuğu ve seçenekler sağlar. Sonuçlar JSON belgeleri olarak döndürülür. Arama, test ve doğrulama için önerilir. <br/>[Daha fazla bilgi edinin.](search-get-started-portal.md#query-index) | 
| [Postacı veya diğer REST araçları](search-get-started-postman.md) | Web test araçları REST aramaları formüle etmek için mükemmel bir seçimdir. REST API, Azure Bilişsel Arama'da mümkün olan her işlemi destekler. Bu makalede, Azure Bilişsel Arama'ya istek göndermek için bir HTTP istek üstbilgisini ve gövdesini nasıl ayarlayaradığınızı öğrenin.  |
| [SearchIndexClient (.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) | Azure Bilişsel Arama dizinini sorgulamak için kullanılabilecek istemci.  <br/>[Daha fazla bilgi edinin.](search-howto-dotnet-sdk.md#core-scenarios)  |
| [Arama Belgeleri (REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) | Ek giriş için sorgu parametrelerini kullanarak bir dizin üzerinde GET veya POST yöntemleri.  |

## <a name="choose-a-parser-simple--full"></a>Bir parser seçin: basit | Tam

Azure Bilişsel Arama, Apache Lucene'nin üzerinde yer alan ve tipik ve özel leştirilmiş sorguları işlemek için iki sorgu ayrıştırıcısı arasında seçim olanağı sağlar. Basit arayıcı yı kullanan istekler, serbest biçimmetin sorgularında hızı ve etkinliği için varsayılan olarak seçilen [basit sorgu sözdizimi](query-simple-syntax.md)kullanılarak formüle edilir. Bu sözdizimi, AND, OR, NOT, tümcecik, sonek ve öncelik işleçleri de dahil olmak üzere bir dizi yaygın arama işlecidestekler.

İstek eklediğinizde `queryType=full` etkinleştirilen [tam Lucene sorgu sözdizimi,](query-Lucene-syntax.md#bkmk_syntax) [Apache Lucene'nin](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)bir parçası olarak geliştirilen yaygın olarak benimsenen ve etkileyici sorgu dilini ortaya çıkarır. Tam sözdizimi basit sözdizimini genişletir. Basit sözdizimi için yazdığınız herhangi bir sorgu tam Lucene ayrıştırıcıaltında çalışır. 

Aşağıdaki örnekler noktayı göstermektedir: aynı sorgu, ancak farklı queryType ayarları ile, farklı sonuçlar verir. İlk sorguda, `^3` sonraki `historic` arama teriminin bir parçası olarak kabul edilir. Bu sorgu için üst sırada sonuç "Marquis Plaza & Suites", hangi açıklamasında *okyanus* vardır

```
queryType=simple&search=ocean historic^3&searchFields=Description, Tags&$select=HotelId, HotelName, Tags, Description&$count=true
```

Tam Lucene arayıcısı kullanarak aynı `^3` sorgu alan içi terim güçlendirici olarak yorumlanır. Parsers anahtarlama, *dönem tarihi* üst hareket içeren sonuçlar ile rütbe değiştirir.

```
queryType=full&search=ocean historic^3&searchFields=Description, Tags&$select=HotelId, HotelName, Tags, Description&$count=true
```

<a name="types-of-queries"></a>

## <a name="types-of-queries"></a>Sorgu türleri

Azure Bilişsel Arama, çok çeşitli sorgu türlerini destekler. 

| Sorgu türü | Kullanım | Örnekler ve daha fazla bilgi |
|------------|--------|-------------------------------|
| Ücretsiz form metin arama | Arama parametresi ve ya ayrıştırıcı| Tam metin arama, dizininizdeki tüm *aranabilir* alanlarda bir veya daha fazla terimiçin tarar ve Google veya Bing gibi bir arama motorunun çalışmasını beklediğiniz şekilde çalışır. Girişteki örnek tam metin aramadır.<br/><br/>Tam metin arama, tüm terimleri küçük düşürmek, "the" gibi dur sözcükleri kaldırmak için standart Lucene çözümleyicisini (varsayılan olarak) kullanarak metin çözümlemesi yapılır. Metin çözümlemesi değiştirmek İngilizce [olmayan çözümleyiciler](index-add-language-analyzers.md#language-analyzer-list) veya [özel dil-agnostik çözümleyicileri](index-add-custom-analyzers.md#AnalyzerTable) ile varsayılan geçersiz kılınabilir. Bir örnek, bir alanın tüm içeriğini tek bir belirteç olarak ele alan [anahtar kelimedir.](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) Bu, posta kodları, d'ler ve bazı ürün adları gibi veriler için yararlıdır. | 
| Filtrelenmiş arama | [OData filtre ifadesi](query-odata-filter-orderby-syntax.md) ve ya ayrıştırıcı | Filtre sorguları, bir dizinteki tüm *filtrelenebilir* alanlar üzerinde boolean ifadesini değerlendirir. Aramanın aksine, filtre sorgusu dize alanlarındaki büyük/küçük harf duyarlılığı da dahil olmak üzere bir alanın tam içeriğiyle eşleşir. Diğer bir fark, filtre sorgularının OData sözdiziminde ifade edilmiş olmasıdır. <br/>[Filtre ifadesi örneği](search-query-simple-examples.md#example-3-filter-queries) |
| Coğrafi arama | [Edm.GeographyPoint yazın](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) alan, filtre ifade ve ya parser | Edm.GeographyPoint'e sahip bir alanda depolanan koordinatlar "yakınımda bulun" veya harita tabanlı arama denetimleri için kullanılır. <br/>[Coğrafi arama örneği](search-query-simple-examples.md#example-5-geo-search)|
| Aralık arama | filtre ifadesi ve basit ayrıştırıcı | Azure Bilişsel Arama'da, aralık sorguları filtre parametresi kullanılarak oluşturulur. <br/>[Aralık filtresi örneği](search-query-simple-examples.md#example-4-range-filters) | 
| [Alanlı arama](query-lucene-syntax.md#bkmk_fields) | Arama parametresi ve Tam parser | Tek bir alanı hedefleyen bileşik sorgu ifadesi oluşturun. <br/>[Alanlı arama örneği](search-query-lucene-examples.md#example-2-fielded-search) |
| [bulanık arama](query-lucene-syntax.md#bkmk_fuzzy) | Arama parametresi ve Tam parser | Benzer bir yapı veya yazım alasahip terimlerle eşleşir. <br/>[Bulanık arama örneği](search-query-lucene-examples.md#example-3-fuzzy-search) |
| [yakınlık arama](query-lucene-syntax.md#bkmk_proximity) | Arama parametresi ve Tam parser | Belgede birbirine yakın terimleri bulur. <br/>[Yakınlık arama örneği](search-query-lucene-examples.md#example-4-proximity-search) |
| [dönem artırma](query-lucene-syntax.md#bkmk_termboost) | Arama parametresi ve Tam parser | Artırılmış terimi içeriyorsa, belgeyi, yükseltilen diğer terime göre daha yüksek sıralar. <br/>[Terim artırma örneği](search-query-lucene-examples.md#example-5-term-boosting) |
| [düzenli ifade arama](query-lucene-syntax.md#bkmk_regex) | Arama parametresi ve Tam parser | Normal bir ifadenin içeriğine göre eşleşir. <br/>[Normal ifade örneği](search-query-lucene-examples.md#example-6-regex) |
|  [joker karakter veya önek arama](query-lucene-syntax.md#bkmk_wildcard) | Arama parametresi ve Tam parser | Önek ve tilde (`~`) veya tek`?`karaktere ( ) dayalı eşleşmeler . <br/>[Joker karakter arama örneği](search-query-lucene-examples.md#example-7-wildcard-search) |

## <a name="manage-search-results"></a>Arama sonuçlarını yönetme 

Sorgu sonuçları, .NET API'lerini kullanırsanız, serileştirme yerleşik olsa da, REST API'sinde JSON belgeleri olarak akışlanır. Sorguda parametreleri ayarlayarak, yanıt için belirli alanları seçerek sonuçları şekillendirebilirsiniz.

Sorgudaki parametreler, sonuç kümesini aşağıdaki yollarla yapılandırmak için kullanılabilir:

+ Sonuçlardaki belge sayısını sınırlama veya toplu işleme (varsayılan olarak 50)
+ Sonuçlara dahil etmek için alanları seçme
+ Sıralama sırası ayarlama
+ Arama sonuçlarının gövdesindeki eşleşen terimlere dikkat çekmek için isabet vurguları ekleme

### <a name="tips-for-unexpected-results"></a>Beklenmeyen sonuçlar için ipuçları

Bazen, madde değil, sonuçların yapısı beklenmeyen. Sorgu sonuçları görmeyi beklediğiniz gibi olmadığında, sonuçların iyileşip iyileşmediğini görmek için bu sorgu değişikliklerini deneyebilirsiniz:

+ Herhangi **`searchMode=any`** bir ölçüt yerine tüm ölçütlere eşleşme gerektirecek şekilde değiştirin (varsayılan). **`searchMode=all`** Bu özellikle boolean işleçleri sorgu dahil edildiğinde doğrudur.

+ Metin veya sözlü çözümleme gerekiyorsa sorgu tekniğini değiştirin, ancak sorgu türü dilsel işlemeyi engelliyorsa. Tam metin arama, metin veya sözlü çözümleme yazım hataları, tekil-çoğul sözcük formları ve hatta düzensiz fiiller veya adlar için otomatik düzeltir. Bulanık veya joker karakter araması gibi bazı sorgular için metin çözümlemesi sorgu ayrıştırma ardışık alanının bir parçası değildir. Bazı senaryolar için normal ifadeler geçici çözüm olarak kullanılmıştır. 

### <a name="paging-results"></a>Disk belleği sonuçları
Azure Bilişsel Arama, arama sonuçlarının sayfalatMa uygulamasını kolaylaştırır. Parametreleri **`top`** ve **`skip`** parametreleri kullanarak, iyi arama arabirimi uygulamalarını kolayca etkinleştiren yönetilebilir, sıralı alt kümelerde toplam arama sonuçları kümesini almanıza olanak tanıyan arama isteklerini sorunsuz bir şekilde verebilirsiniz. Bu daha küçük sonuç alt kümelerini alırken, tüm arama sonuçları kümesindeki belge sayısını da alabilirsiniz.

Azure Bilişsel Arama'da arama sonuçlarını [sayfalama makalesinde](search-pagination-page-layout.md)arama sonuçlarını sayfalama hakkında daha fazla bilgi edinebilirsiniz.

### <a name="ordering-results"></a>Sonuçları sıralama
Bir arama sorgusu için sonuç alırken, Azure Bilişsel Arama'nın belirli bir alandaki değerlere göre sıralanan sonuçlara hizmet ettiğini isteyebilirsiniz. Varsayılan olarak, Azure Bilişsel Arama, arama sonuçlarını her belgenin [TF-IDF'den](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)türetilen arama puanının sıralamasına göre sıralar.

Azure Bilişsel Arama'nın sonuçlarınızı arama puanı dışında bir değere göre döndürmesini istiyorsanız, arama parametresini **`orderby`** kullanabilirsiniz. Parametrenin **`orderby`** değerini alan adlarını ve coğrafi değerler için [** `geo.distance()` işleve**](query-odata-filter-orderby-syntax.md) çağrıları eklemek için belirtebilirsiniz. Her ifade, sonuçların `asc` artan sırada istendiğini belirtmek ve **`desc`** sonuçların azalan sırada istendiğini belirtmek için izlenebilir. Artan sıralama varsayılandır.


### <a name="hit-highlighting"></a>İsabet vurgulama
Azure Bilişsel Arama'da, arama sorgusuyla eşleşen arama sonuçlarının tam bölümünü **`highlight`** **`highlightPreTag`** vurgulamak, **`highlightPostTag`** , ve parametrelerkullanılarak kolay hale getirilir. Azure Bilişsel Arama'nın döndürdettiği eşleşen metnin başlangıç ve sonuna eklemek için tam dize etiketlerini belirterek, hangi *aranabilir* alanların eşleştirilmiş metinlerinin vurgulanması gerektiğini belirtebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama'da tam metin arama nasıl çalışır (sorgu ayrıştırma mimarisi)](search-lucene-query-architecture.md)
+ [Arama gezgini](search-explorer.md)
+ [.NET'te sorgulama nasıl](search-query-dotnet.md)
+ [REST'te sorgulama nasıl](search-create-index-rest-api.md)
