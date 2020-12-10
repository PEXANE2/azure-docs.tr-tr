---
title: Sorgu türleri
titleSuffix: Azure Cognitive Search
description: Serbest metin, filtre, otomatik tamamlama ve öneriler, coğrafi arama, sistem sorguları ve belge arama gibi Bilişsel Arama desteklenen sorgu türleri hakkında bilgi edinin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/09/2020
ms.openlocfilehash: d1ea2d0ba8ed5850e5d4cd9c06a0b016c4059ca7
ms.sourcegitcommit: 273c04022b0145aeab68eb6695b99944ac923465
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97007866"
---
# <a name="query-types-in-azure-cognitive-search"></a>Azure Bilişsel Arama 'de sorgu türleri

Azure Bilişsel Arama, bir sorgu, sorgu yürütmeyi yöneten parametreler ve geri gelen yanıtı şekillendirip parametreleri içeren bir gidiş dönüş işleminin tam belirtimidir.

## <a name="elements-of-a-request"></a>Bir isteğin öğeleri

Aşağıdaki örnek, [REST API arama belgeleri](/rest/api/searchservice/search-documents)kullanılarak oluşturulan temsili bir sorgudur. Bu örnek, [otel demo dizinini](search-get-started-portal.md) hedefler ve bir sorgunun nasıl göründüğünü fikir alabilmek için ortak parametreleri içerir.

```http
POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
{
    "queryType": "simple" 
    "search": "`New York` +restaurant",
    "searchFields": "Description, Address/City, Tags",
    "select": "HotelId, HotelName, Description, Rating, Address/City, Tags",
    "top": "10",
    "count": "true",
    "orderby": "Rating desc"
}
```

Sorgular her zaman tek bir dizinin belgeler koleksiyonuna yönlendirilir. Bir sorgu hedefi olarak dizinlere katılamaz veya özel veya geçici veri yapıları oluşturamazsınız.

+ **`queryType`**[varsayılan basit sorgu ayrıştırıcı](search-query-simple-examples.md) (tam metin araması için en uygun) veya normal ifadeler, yakınlık araması, belirsiz ve joker karakter arama gibi gelişmiş sorgu yapıları için kullanılan [tam Lucene sorgu ayrıştırıcısının](search-query-lucene-examples.md) , birkaç kez ad vermek üzere Ayrıştırıcıyı ayarlar.

+ **`search`** eşleşme ölçütlerini, genellikle tüm terimleri veya tümceleri (genellikle Boolean işleçleriyle birlikte) sağlar. Tek başına terimler, *terim* sorgulardır. Tırnak içine alınmış çok parçalı sorgular *tümcecik* sorgulardır. Arama, içinde olduğu gibi tanımsız olabilir, **`search=*`** ancak eşleşme ölçütü olmadan, sonuç kümesi rastgele seçilmiş belgelerden oluşur.

+ **`searchFields`** sorgu yürütmesini belirli alanlara kısıtlar. Dizin şemasında *aranabilir* olarak öznitelikli tüm alanlar, bu parametre için bir adaydır.

Yanıtlar Ayrıca sorguya dahil ettiğiniz parametrelere göre şekillendirilir:

+ **`select`** yanıtta döndürülecek alanları belirtir. Yalnızca dizinde *alınabilir* olarak işaretlenen alanlar, Select ifadesinde kullanılabilir.

+ **`top`** belirtilen en iyi eşleşen belge sayısını döndürür. Bu örnekte, yalnızca 10 isabetler döndürülür. Sonuçları göstermek için top ve Skip (gösterilmez) ' i kullanabilirsiniz.

+ **`count`** Tüm dizindeki birçok belge, döndürülenden daha fazla olabilecek şekilde tamamen eşleşir. 

+ **`orderby`** sonuçları derecelendirme veya konum gibi bir değere göre sıralamak istiyorsanız kullanılır. Aksi takdirde, varsayılan değer, sonuçları derecelendirmek için yakınlık puanı kullanmaktır.

> [!Tip]
> Herhangi bir kod yazmadan önce, sözdizimini öğrenmek ve farklı parametrelerle denemek için sorgu araçları 'nı kullanabilirsiniz. En hızlı yaklaşım, [Arama Gezgini](search-explorer.md)olan yerleşik Portal aracıdır.
>
> Bu [Hızlı başlangıcı otel demo dizini oluşturmak için](search-get-started-portal.md)izlediyseniz, ilk sorgunuzu çalıştırmak için bu sorgu dizesini gezgin 'in arama çubuğuna yapıştırabilirsiniz: `search=+"New York" +restaurant&searchFields=Description, Address/City, Tags&$select=HotelId, HotelName, Description, Rating, Address/City, Tags&$top=10&$orderby=Rating desc&$count=true`

### <a name="how-field-attributes-in-an-index-determine-query-behaviors"></a>Dizindeki alan özniteliklerinin sorgu davranışlarını belirleme

Dizin tasarımı ve sorgu tasarımı Azure Bilişsel Arama sıkı bir şekilde bağlanmış. En baştan haberdar olmak için önemli bir olgu, *dizin şemasının* her bir alanda bulunan özniteliklerle, oluşturabileceğiniz sorgu türünü belirler. 

Bir alandaki dizin öznitelikleri, izin verilen işlemleri ayarlar; bir alanın dizinde *aranabilir* olup olmadığı, sonuçlarda *alınabilir* , *sıralanabilir*, *filtrelenebilir* ve benzeri. Örnek sorgu dizesinde, `"$orderby": "Rating"` yalnızca derecelendirme alanı dizin şemasında *sıralanabilir* olarak işaretlendiğinden geçerlidir. 

![Otel örneği için Dizin tanımı](./media/search-query-overview/hotel-sample-index-definition.png "Otel örneği için Dizin tanımı")

Yukarıdaki ekran görüntüsü, otel örneği için dizin özniteliklerinin kısmi bir listesidir. Tüm Dizin şemasını portalda görüntüleyebilirsiniz. Dizin öznitelikleri hakkında daha fazla bilgi için bkz. [Create ındex REST API](/rest/api/searchservice/create-index).

> [!Note]
> Bazı sorgu işlevleri, her alan temelinde değil, Dizin genelinde etkindir. Bu yetenekler şunlardır: [eş anlamlı haritalar](search-synonyms.md), [özel çözümleyiciler](index-add-custom-analyzers.md), [öneri aracı yapıları (otomatik tamamlama ve önerilen sorgular için)](index-add-suggesters.md), [sıralama sonuçları için Puanlama mantığı](index-add-scoring-profiles.md).

## <a name="choose-a-parser-simple--full"></a>Ayrıştırıcı seçin: basit | tümünü

Azure Bilişsel Arama, tipik ve özelleştirilmiş sorguları işlemek için iki sorgu ayrıştırıcıları arasında seçim sağlar. Basit ayrıştırıcı kullanan istekler [basit sorgu söz dizimi](query-simple-syntax.md)kullanılarak formüle sahiptir ve ücretsiz form metin sorgularında kendi hızı ve verimliliği için varsayılan olarak seçilidir. Bu sözdizimi, AND, OR, NOT, tümcecik, sonek ve öncelik işleçlerini içeren bir dizi yaygın arama işlecini destekler.

İsteğe eklerken etkinleştirilen [tam Lucene sorgu söz dizimi](query-Lucene-syntax.md#bkmk_syntax), `queryType=full` [Apache Lucene](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)'in bir parçası olarak geliştirilen, yaygın olarak benimsenen ve ifade eden sorgu dilini kullanıma sunar. Tam sözdizimi basit sözdizimini genişletir. Basit sözdizimi için yazdığınız herhangi bir sorgu, tam Lucene ayrıştırıcısı altında çalışır. 

Aşağıdaki örneklerde nokta gösterilmektedir: aynı sorgu, ancak farklı queryType ayarları ile farklı sonuçlar elde edin. İlk sorguda, `^3` sonrasında `historic` arama teriminin bir parçası olarak işlenir. Bu sorgu için en üst dereceli sonuç, açıklamasında *okyanus* olan "Marquis plaza & paketleriniz" dır.

```http
queryType=simple&search=ocean historic^3&searchFields=Description, Tags&$select=HotelId, HotelName, Tags, Description&$count=true
```

Tam Lucene ayrıştırıcısı kullanılarak aynı sorgu, `^3` alan terimi rampa olarak yorumlar. Çözümleyicileri değiştirmek, en üste *geçen dönemi içeren* sonuçlarla birlikte derecelendirmeyi değiştirir.

```http
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
| [Otomatik tamamlama veya önerilen sonuçlar](search-autocomplete-tutorial.md) | Otomatik tamamlama veya öneri parametresi | Bir arama türü deneyiminde kısmi dizelere göre yürütülen alternatif bir sorgu formu. Otomatik tamamlama ve önerileri birlikte veya ayrı bir şekilde kullanabilirsiniz. |
| [benzer arama](query-lucene-syntax.md#bkmk_fuzzy) | Arama parametresi ve tam ayrıştırıcı | Benzer bir yapım veya yazım denetimi olan koşullara göre eşleşir. <br/>[Benzer arama örneği](search-query-lucene-examples.md#example-3-fuzzy-search) |
| [yakınlık araması](query-lucene-syntax.md#bkmk_proximity) | Arama parametresi ve tam ayrıştırıcı | Belgedeki birbirini yakın terimleri bulur. <br/>[Yakınlık arama örneği](search-query-lucene-examples.md#example-4-proximity-search) |
| [terim artırma](query-lucene-syntax.md#bkmk_termboost) | Arama parametresi ve tam ayrıştırıcı | Bir belgeyi, daha fazla olmayan diğerlerine göre, daha yüksek bir dönem içeriyorsa, daha yüksek bir şekilde derecelendirir. <br/>[Terim artırma örneği](search-query-lucene-examples.md#example-5-term-boosting) |
| [normal ifade arama](query-lucene-syntax.md#bkmk_regex) | Arama parametresi ve tam ayrıştırıcı | Normal bir ifadenin içeriğine göre eşleşir. <br/>[Normal ifade örneği](search-query-lucene-examples.md#example-6-regex) |
|  [joker karakter veya ön ek arama](query-lucene-syntax.md#bkmk_wildcard) | Arama parametresi ve tam ayrıştırıcı | Bir önek ve tilde ( `~` ) veya tek karakter () temelinde eşleşir `?` . <br/>[Joker karakter arama örneği](search-query-lucene-examples.md#example-7-wildcard-search) |

## <a name="next-steps"></a>Sonraki adımlar

Daha ayrıntılı bir şekilde incelemek için portalı veya Postman veya Visual Studio Code gibi başka bir aracı veya SDK 'Lardan birini kullanın. Aşağıdaki bağlantılar başlamanızı sağlayacak.

+ [Arama Gezgini](search-explorer.md)
+ [.NET 'te sorgulama](./search-get-started-dotnet.md)
+ [REST 'te sorgulama](./search-get-started-powershell.md)