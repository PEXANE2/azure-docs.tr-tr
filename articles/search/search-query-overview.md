---
title: Sorgu türleri ve oluşturma
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de arama sorgusu oluşturmaya yönelik temel bilgiler, sonuçları filtrelemek, seçmek ve sıralamak için parametreler kullanmaktır.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/22/2020
ms.openlocfilehash: 018c3fb08c7fa0ad35fa567bffbeae48b6fbbce9
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88928845"
---
# <a name="query-types-and-composition-in-azure-cognitive-search"></a>Azure Bilişsel Arama 'de sorgu türleri ve bileşimi

Azure Bilişsel Arama, bir sorgu gidiş dönüş işleminin tam belirtimidir. İstekte, altyapıya yönelik yürütme yönergelerini ve geri gelen yanıtı şekillendirieden parametreleri sağlayan parametreler vardır. Belirtilmemiş ( `search=*` ), eşleşme ölçütü olmadan ve null ya da varsayılan parametreleri kullanarak, bir sorgu tüm aranabilir alanlara tam metin arama işlemi olarak yürütülür ve bu da rastgele bir sonuç kümesi döndürür.

Aşağıdaki örnek, [REST API](/rest/api/searchservice/search-documents)oluşturulan temsili bir sorgudur. Bu örnek, [otel demo dizinini](search-get-started-portal.md) hedefler ve bir sorgunun nasıl göründüğünü fikir alabilmek için ortak parametreleri içerir.

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

+ **`queryType`**[varsayılan basit sorgu ayrıştırıcı](search-query-simple-examples.md) (tam metin araması için en uygun) veya normal ifadeler, yakınlık araması, belirsiz ve joker karakter arama gibi gelişmiş sorgu yapıları için kullanılan [tam Lucene sorgu ayrıştırıcısının](search-query-lucene-examples.md) , birkaç kez ad vermek üzere Ayrıştırıcıyı ayarlar.

+ **`search`** eşleşme ölçütlerini, genellikle tüm terimleri veya tümceleri (genellikle Boolean işleçleriyle birlikte) sağlar. Tek başına terimler, *terim* sorgulardır. Tırnak içine alınmış çok parçalı sorgular *tümcecik* sorgulardır. Arama, içinde olduğu gibi tanımsız olabilir, **`search=*`** ancak eşleşme ölçütü olmadan, sonuç kümesi rastgele seçilmiş belgelerden oluşur.

+ **`searchFields`** sorgu yürütmesini belirli alanlara kısıtlar. Dizin şemasında *aranabilir* olarak öznitelikli tüm alanlar, bu parametre için bir adaydır.

Yanıtlar Ayrıca sorguya dahil ettiğiniz parametrelere göre şekillendirilir:

+ **`select`** yanıtta döndürülecek alanları belirtir. Yalnızca dizinde *alınabilir* olarak işaretlenen alanlar, Select ifadesinde kullanılabilir.

+ **`top`** belirtilen en iyi eşleşen belge sayısını döndürür. Bu örnekte, yalnızca 10 isabetler döndürülür. Sonuçları göstermek için top ve Skip (gösterilmez) ' i kullanabilirsiniz.

+ **`count`** Tüm dizindeki birçok belge, döndürülenden daha fazla olabilecek şekilde tamamen eşleşir. 

+ **`orderby`** sonuçları derecelendirme veya konum gibi bir değere göre sıralamak istiyorsanız kullanılır. Aksi takdirde, varsayılan değer, sonuçları derecelendirmek için yakınlık puanı kullanmaktır.

Azure Bilişsel Arama 'de sorgu yürütme her zaman bir dizine göre yapılır ve istekte belirtilen bir API anahtarı kullanılarak kimlik doğrulaması yapılır. REST 'de, her ikisi de istek üst bilgilerinde sağlanır.

### <a name="how-to-run-this-query"></a>Bu sorgu nasıl çalıştırılır

Herhangi bir kod yazmadan önce, sözdizimini öğrenmek ve farklı parametrelerle denemek için sorgu araçları 'nı kullanabilirsiniz. En hızlı yaklaşım, [Arama Gezgini](search-explorer.md)olan yerleşik Portal aracıdır.

Bu [Hızlı başlangıcı otel demo dizini oluşturmak için](search-get-started-portal.md)izlediyseniz, ilk sorgunuzu çalıştırmak için bu sorgu dizesini gezgin 'in arama çubuğuna yapıştırabilirsiniz: `search=+"New York" +restaurant&searchFields=Description, Address/City, Tags&$select=HotelId, HotelName, Description, Rating, Address/City, Tags&$top=10&$orderby=Rating desc&$count=true`

## <a name="how-query-operations-are-enabled-by-the-index"></a>Sorgu işlemleri dizin tarafından nasıl etkinleştirilir

Dizin tasarımı ve sorgu tasarımı Azure Bilişsel Arama sıkı bir şekilde bağlanmış. En baştan haberdar olmak için önemli bir olgu, *dizin şemasının*her bir alanda bulunan özniteliklerle, oluşturabileceğiniz sorgu türünü belirler. 

Bir alandaki dizin öznitelikleri, izin verilen işlemleri ayarlar; bir alanın dizinde *aranabilir* olup olmadığı, sonuçlarda *alınabilir* , *sıralanabilir*, *filtrelenebilir*ve benzeri. Örnek sorgu dizesinde, `"$orderby": "Rating"` yalnızca derecelendirme alanı dizin şemasında *sıralanabilir* olarak işaretlendiğinden geçerlidir. 

![Otel örneği için Dizin tanımı](./media/search-query-overview/hotel-sample-index-definition.png "Otel örneği için Dizin tanımı")

Yukarıdaki ekran görüntüsü, otel örneği için dizin özniteliklerinin kısmi bir listesidir. Tüm Dizin şemasını portalda görüntüleyebilirsiniz. Dizin öznitelikleri hakkında daha fazla bilgi için bkz. [Create ındex REST API](/rest/api/searchservice/create-index).

> [!Note]
> Bazı sorgu işlevleri, her alan temelinde değil, Dizin genelinde etkindir. Bu yetenekler şunlardır: [eş anlamlı haritalar](search-synonyms.md), [özel çözümleyiciler](index-add-custom-analyzers.md), [öneri aracı yapıları (otomatik tamamlama ve önerilen sorgular için)](index-add-suggesters.md), [sıralama sonuçları için Puanlama mantığı](index-add-scoring-profiles.md).

## <a name="elements-of-a-query-request"></a>Bir sorgu isteği öğeleri

Sorgular her zaman tek bir dizine yönlendirilir. Bir sorgu hedefi olarak dizinlere katılamaz veya özel veya geçici veri yapıları oluşturamazsınız. 

Bir sorgu isteğindeki gerekli öğeler aşağıdaki bileşenleri içerir:

+ Sabit ve Kullanıcı tanımlı bileşenleri içeren bir URL olarak ifade edilen hizmet uç noktası ve Dizin belgeleri koleksiyonu: **`https://<your-service-name>.search.windows.net/indexes/<your-index-name>/docs`**
+ **`api-version`** (Yalnızca REST), API 'nin birden fazla sürümü her zaman kullanılabilir olduğundan gereklidir. 
+ **`api-key`** ya bir sorgu ya da yönetici API anahtarı, hizmetinize yönelik isteğin kimliğini doğrular.
+ **`queryType`**, basit veya tam, yerleşik varsayılan basit sözdizimi kullanıyorsanız atlanabilir.
+ **`search`** ya da **`filter`** boş bir arama gerçekleştirmek istiyorsanız bu parametre belirtilmeyen bir eşleşme ölçütü sağlar. Her iki sorgu türü de basit ayrıştırıcı açısından ele alınmıştır ancak gelişmiş sorgular bile karmaşık sorgu ifadelerini geçirmek için arama parametresi gerektirir.

Diğer tüm arama parametreleri isteğe bağlıdır. Özniteliklerin tam listesi için bkz. [Dizin oluşturma (REST)](/rest/api/searchservice/create-index). İşlemler sırasında parametrelerin nasıl kullanıldığına daha yakından bakmak için, [tam metin aramasının Azure bilişsel arama 'de nasıl çalıştığını](search-lucene-query-architecture.md)görün.

## <a name="choose-apis-and-tools"></a>API 'Leri ve araçları seçin

Aşağıdaki tabloda sorguları göndermek için API 'Ler ve araç tabanlı yaklaşımlar listelenmektedir.

| Yöntem | Açıklama |
|-------------|-------------|
| [Arama Gezgini (portal)](search-explorer.md) | Dizin ve API-sürüm seçimleri için bir arama çubuğu ve seçenekler sağlar. Sonuçlar JSON belgeleri olarak döndürülür. Araştırma, test ve doğrulama için önerilir. <br/>[Daha fazla bilgi edinin.](search-get-started-portal.md#query-index) | 
| [Postman veya diğer REST araçları](search-get-started-postman.md) | Web test araçları, REST çağrılarını formülletmenin çok iyi bir seçimdir. REST API Azure Bilişsel Arama tüm olası işlemleri destekler. Bu makalede, Azure Bilişsel Arama istek göndermek için HTTP istek üst bilgisini ve gövdesini ayarlamayı öğrenin.  |
| [Searchındexclient (.NET)](/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) | Azure Bilişsel Arama dizinini sorgulamak için kullanılabilen istemci.  <br/>[Daha fazla bilgi edinin.](search-howto-dotnet-sdk.md#core-scenarios)  |
| [Belgelerde ara (REST API)](/rest/api/searchservice/search-documents) | Ek giriş için sorgu parametrelerini kullanarak bir dizinde GET veya POST yöntemleri.  |

## <a name="choose-a-parser-simple--full"></a>Ayrıştırıcı seçin: basit | tümünü

Azure Bilişsel Arama Apache Lucene üzerinde bulunur ve tipik ve özelleştirilmiş sorguları işlemek için iki sorgu ayrıştırıcıları arasında seçim sağlar. Basit ayrıştırıcı kullanan istekler [basit sorgu söz dizimi](query-simple-syntax.md)kullanılarak formüle sahiptir ve ücretsiz form metin sorgularında kendi hızı ve verimliliği için varsayılan olarak seçilidir. Bu sözdizimi, AND, OR, NOT, tümcecik, sonek ve öncelik işleçlerini içeren bir dizi yaygın arama işlecini destekler.

İsteğe eklerken etkinleştirilen [tam Lucene sorgu söz dizimi](query-Lucene-syntax.md#bkmk_syntax), `queryType=full` [Apache Lucene](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)'in bir parçası olarak geliştirilen, yaygın olarak benimsenen ve ifade eden sorgu dilini kullanıma sunar. Tam sözdizimi basit sözdizimini genişletir. Basit sözdizimi için yazdığınız herhangi bir sorgu, tam Lucene ayrıştırıcısı altında çalışır. 

Aşağıdaki örneklerde nokta gösterilmektedir: aynı sorgu, ancak farklı queryType ayarları ile farklı sonuçlar elde edin. İlk sorguda, `^3` sonrasında `historic` arama teriminin bir parçası olarak işlenir. Bu sorgu için en üst dereceye sahip sonuç, açıklamasında *okyanus* olan "Marquis plaza & paketleriniz" dır

```
queryType=simple&search=ocean historic^3&searchFields=Description, Tags&$select=HotelId, HotelName, Tags, Description&$count=true
```

Tam Lucene ayrıştırıcısı kullanılarak aynı sorgu, `^3` alan terimi rampa olarak yorumlar. Çözümleyicileri değiştirmek, en üste *geçen dönemi içeren* sonuçlarla birlikte derecelendirmeyi değiştirir.

```
queryType=full&search=ocean historic^3&searchFields=Description, Tags&$select=HotelId, HotelName, Tags, Description&$count=true
```

<a name="types-of-queries"></a>

## <a name="types-of-queries"></a>Sorgu türleri

Azure Bilişsel Arama, çok çeşitli sorgu türlerini destekler. 

| Sorgu türü | Kullanım | Örnekler ve daha fazla bilgi |
|------------|--------|-------------------------------|
| Serbest form metin araması | Arama parametresi ve ayrıştırıcı| Tam metin arama, dizininizdeki tüm *aranabilir* alanlarda bir veya daha fazla terimi tarar ve Google veya Bing gibi bir arama altyapısının çalışmasını istediğiniz şekilde çalışır. Giriş bölümündeki örnek tam metin aramadır.<br/><br/>Tam metin araması, standart Lucene Çözümleyicisi (varsayılan olarak), tüm terimleri düşürmek için, "The" gibi durdurma sözcüklerini kaldırmak için standart Lucene çözümleyici (varsayılan olarak) kullanılarak sözlü analizler Varsayılan ayarı, [İngilizce olmayan çözümleyiciler](index-add-language-analyzers.md#language-analyzer-list) veya sözcük temelli analizleri değiştiren [özel dilden bağımsız çözümleyiciler](index-add-custom-analyzers.md#AnalyzerTable) ile geçersiz kılabilirsiniz. Bir alanın tüm içeriğini tek bir belirteç olarak ele alan bir [anahtar sözcük](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) örneğidir. Bu, ZIP kodları, kimlikler ve bazı ürün adları gibi veriler için yararlıdır. | 
| Filtrelenmiş arama | [OData filtre ifadesi](query-odata-filter-orderby-syntax.md) ve her iki ayrıştırıcı | Filtre sorguları bir dizin içindeki tüm *filtrelenebilir* alanlar üzerinde bir Boole ifadesi değerlendirir. Aramanın aksine, bir filtre sorgusu, dize alanlarında büyük/küçük harf duyarlılığı dahil olmak üzere bir alanın tam içeriğiyle eşleşir. Farklı bir farklılık, filtre sorgularının OData sözdiziminde ifade edildiği bir farktır. <br/>[Filtre ifadesi örneği](search-query-simple-examples.md#example-3-filter-queries) |
| Coğrafi arama | Alan, filtre ifadesi ve herhangi bir ayrıştırıcıda [Edm. Geographon noktası türü](/rest/api/searchservice/supported-data-types) | EDM. Geographi noktası bulunan bir alanda depolanan koordinatlar "yakın ben bul" veya harita tabanlı arama denetimleri için kullanılır. <br/>[Coğrafi arama örneği](search-query-simple-examples.md#example-5-geo-search)|
| Aralık araması | Filtre ifadesi ve basit ayrıştırıcı | Azure Bilişsel Arama 'de, Aralık sorguları filtre parametresi kullanılarak oluşturulur. <br/>[Aralık filtresi örneği](search-query-simple-examples.md#example-4-range-filters) | 
| [Parçalı arama](query-lucene-syntax.md#bkmk_fields) | Arama parametresi ve tam ayrıştırıcı | Tek bir alanı hedefleyen bir bileşik sorgu ifadesi oluşturun. <br/>[Parçalı arama örneği](search-query-lucene-examples.md#example-2-fielded-search) |
| [benzer arama](query-lucene-syntax.md#bkmk_fuzzy) | Arama parametresi ve tam ayrıştırıcı | Benzer bir yapım veya yazım denetimi olan koşullara göre eşleşir. <br/>[Benzer arama örneği](search-query-lucene-examples.md#example-3-fuzzy-search) |
| [yakınlık araması](query-lucene-syntax.md#bkmk_proximity) | Arama parametresi ve tam ayrıştırıcı | Belgedeki birbirini yakın terimleri bulur. <br/>[Yakınlık arama örneği](search-query-lucene-examples.md#example-4-proximity-search) |
| [terim artırma](query-lucene-syntax.md#bkmk_termboost) | Arama parametresi ve tam ayrıştırıcı | Bir belgeyi, daha fazla olmayan diğerlerine göre, daha yüksek bir dönem içeriyorsa, daha yüksek bir şekilde derecelendirir. <br/>[Terim artırma örneği](search-query-lucene-examples.md#example-5-term-boosting) |
| [normal ifade arama](query-lucene-syntax.md#bkmk_regex) | Arama parametresi ve tam ayrıştırıcı | Normal bir ifadenin içeriğine göre eşleşir. <br/>[Normal ifade örneği](search-query-lucene-examples.md#example-6-regex) |
|  [joker karakter veya ön ek arama](query-lucene-syntax.md#bkmk_wildcard) | Arama parametresi ve tam ayrıştırıcı | Bir önek ve tilde ( `~` ) veya tek karakter () temelinde eşleşir `?` . <br/>[Joker karakter arama örneği](search-query-lucene-examples.md#example-7-wildcard-search) |

## <a name="manage-search-results"></a>Arama sonuçlarını Yönet 

Sorgu sonuçları REST API JSON belgeleri olarak akışlardır, ancak .NET API 'Lerini kullanıyor olsanız da serileştirme yerleşiktir. Sorgudaki parametreleri ayarlayarak, yanıt için belirli alanlar seçerek sonuçları şekillendirebilirsiniz.

Sorgudaki parametreler, sonuç kümesini aşağıdaki yollarla yapılandırmak için kullanılabilir:

+ Sonuçlarda belge sayısını sınırlama veya toplu işleme (varsayılan olarak 50)
+ Sonuçlara dahil edilecek alanları seçme
+ Sıralama düzeni ayarlama
+ Arama sonuçlarının gövdesinde, eşleşen koşullara dikkat çekmek için isabet vurgulamaları ekleme

### <a name="tips-for-unexpected-results"></a>Beklenmeyen sonuçlara yönelik ipuçları

Kimi zaman, sonuçların yapısını değil, her zaman için. Sorgu sonuçları görmeyi beklediğiniz gibi değilse sonuçların iyileştirediğini görmek için bu sorgu değişikliklerini deneyebilirsiniz:

+ **`searchMode=any`** **`searchMode=all`** Ölçütlerin herhangi biri yerine tüm ölçütlerde eşleşme gerektirmek için (varsayılan) öğesini olarak değiştirin. Bu, Boolean işleçleri sorguyu dahil edildiğinde özellikle doğrudur.

+ Metin veya sözcük temelli analiz gerekliyse sorgu tekniğini değiştirin, ancak sorgu türü, dil işlemesini önceden halizler. Tam metin aramasında, yazım hataları, tekil çoğul sözcük formları ve hatta düzensiz fiiller ya da isimler için metin veya sözlü analizler oto düzeltir. Benzer veya joker karakter araması gibi bazı sorgularda, sözcük temelli analiz sorgu ayrıştırma ardışık düzeninin bir parçası değildir. Bazı senaryolarda, normal ifadeler geçici bir çözüm olarak kullanılmıştır. 

### <a name="paging-results"></a>Disk belleği sonuçları
Azure Bilişsel Arama, arama sonuçlarının sayfalama uygulanmasını kolaylaştırır. **`top`** Ve **`skip`** parametrelerini kullanarak, iyi arama kullanıcı arabirimi uygulamalarını kolayca etkinleştiren, yönetilebilir, sıralı alt kümeler halinde toplam arama sonuçları kümesini almanızı sağlayan arama isteklerini sorunsuz bir şekilde oluşturabilirsiniz. Bu daha küçük sonuç alt kümelerini alırken, tüm arama sonuçları kümesindeki belge sayısını da alabilirsiniz.

Sayfalama arama sonuçları hakkında daha fazla bilgi edinmek [Için bkz. Azure bilişsel arama arama sonuçları sayfası](search-pagination-page-layout.md).

### <a name="ordering-results"></a>Sonuçları sıralama
Bir arama sorgusu için sonuçları alırken, Azure Bilişsel Arama belirli bir alandaki değerlere göre sıralanmış sonuçlara hizmet verebilir. Azure Bilişsel Arama, varsayılan olarak, arama sonuçlarını [tf-ıDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)' den türetilen her bir belgenin arama puanı derecesine göre sıralar.

Azure Bilişsel Arama 'nin sonuçlarınızı arama puanı dışında bir değere göre sıralanmış olarak döndürmesini istiyorsanız **`orderby`** arama parametresini kullanabilirsiniz. **`orderby`** Alan adlarını ve jeo-uzamsal değerler [** `geo.distance()` işlevine**](query-odata-filter-orderby-syntax.md) çağrıları dahil etmek için parametresinin değerini belirtebilirsiniz. `asc`Sonuçların artan sırada istendiğini belirtmek ve **`desc`** sonuçların azalan sırada istendiğini belirtmek için, her ifadeye sonra gelebilir. Artan sıralama varsayılandır.


### <a name="hit-highlighting"></a>İsabet vurgulama
Azure Bilişsel Arama 'de arama sorgusuyla eşleşen arama sonuçlarının tam bölümünü vurgulayarak,, ve parametreleri kullanılarak kolayca yapılır **`highlight`** **`highlightPreTag`** **`highlightPostTag`** . Hangi *aranabilir* alanların eşleşen metnine sahip olması gerektiğini ve Azure bilişsel arama döndürdüğü eşleşen metnin başlangıcına ve sonuna eklemek için tam dize etiketlerini belirterek belirtebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Tam metin aramasının Azure Bilişsel Arama 'da nasıl çalıştığı (sorgu ayrıştırma mimarisi)](search-lucene-query-architecture.md)
+ [Arama Gezgini](search-explorer.md)
+ [.NET 'te sorgulama](./search-get-started-dotnet.md)
+ [REST 'te sorgulama](./search-get-started-powershell.md)