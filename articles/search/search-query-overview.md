---
title: Sorgu türleri ve birleşim-Azure Search
description: Sonuçları filtrelemek, seçmek ve sıralamak için parametreleri kullanarak Azure Search bir arama sorgusu oluşturmaya yönelik temel bilgiler.
author: HeidiSteen
manager: nitinme
ms.author: heidist
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/13/2019
ms.custom: seodec2018
ms.openlocfilehash: 30c3b233a1454d04fb281e049376b2b3aafe1879
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647965"
---
# <a name="how-to-compose-a-query-in-azure-search"></a>Azure Search bir sorgu oluşturma

Azure Search, bir sorgu gidiş dönüş işleminin tam belirtimidir. İstekteki parametreler, dizinde belge bulmak için eşleşme ölçütlerini, altyapıya yönelik yürütme yönergelerini ve yanıtı şekillendirmeye yönelik yönergeleri sağlar. 

Sorgu isteği, hangi alanların kapsam içinde olduğunu, nasıl aranacağını, hangi alanların dönemeyeceğini, sıralama veya filtreleme yapıp yapamayacağını belirten zengin bir yapıdır. Belirtilmemişse, bir sorgu tam metin arama işlemi olarak aranabilir tüm alanlara karşı çalışır ve rastgele bir sonuç kümesi rastgele sırada döner.

## <a name="apis-and-tools-for-testing"></a>Test için API 'Ler ve araçlar

Aşağıdaki tabloda sorguları göndermek için API 'Ler ve araç tabanlı yaklaşımlar listelenmektedir.

| Yöntemi | Açıklama |
|-------------|-------------|
| [Arama Gezgini (portal)](search-explorer.md) | Dizin ve API-sürüm seçimleri için bir arama çubuğu ve seçenekler sağlar. Sonuçlar JSON belgeleri olarak döndürülür. <br/>[Daha fazla bilgi edinin.](search-get-started-portal.md#query-index) | 
| [Postman veya Fiddler](search-get-started-postman.md) | Web test araçları, REST çağrılarını formülletmenin çok iyi bir seçimdir. REST API, Azure Search tüm olası işlemleri destekler. Bu makalede, Azure Search istek göndermek için HTTP istek üst bilgisini ve gövdesini ayarlamayı öğrenin.  |
| [Searchındexclient (.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) | Bir Azure Search dizinini sorgulamak için kullanılabilen istemci.  <br/>[Daha fazla bilgi edinin.](search-howto-dotnet-sdk.md#core-scenarios)  |
| [Belgelerde ara (REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) | Ek giriş için sorgu parametrelerini kullanarak bir dizinde GET veya POST yöntemleri.  |

## <a name="a-first-look-at-query-requests"></a>Sorgu isteklerine ilk bakış

Örnekler, yeni kavramlara giriş için faydalıdır. [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents)oluşturulan temsili bir sorgu olarak, bu örnek [gerçek emlak demo dizinini](search-get-started-portal.md) hedefler ve ortak parametreleri içerir.

```
{
    "queryType": "simple" 
    "search": "seattle townhouse* +\"lake\"",
    "searchFields": "description, city",
    "count": "true",
    "select": "listingId, street, status, daysOnMarket, description",
    "top": "10",
    "orderby": "daysOnMarket"
}
```

+ **`queryType`** Azure Search, [varsayılan basit sorgu ayrıştırıcı](search-query-simple-examples.md) (tam metin araması için en uygun) veya normal ifadeler, yakınlık araması, benzer ve joker karakter gibi gelişmiş sorgu yapıları için kullanılan [tam Lucene sorgu ayrıştırıcısının](search-query-lucene-examples.md) kullanılabileceği ayrıştırıcısı ayarlar. birkaç tane olacak şekilde arama yapın.

+ **`search`** eşleşme ölçütlerini genellikle metin olan ancak genellikle Boolean işleçlerle birlikte sağlar. Tek başına terimler, *terim* sorgulardır. Tırnak içine alınmış çok parçalı sorgular *anahtar tümceciği* sorgulardır. Arama, içinde **`search=*`** olduğu gibi tanımsız olabilir, ancak büyük olasılıkla örnekte gösterilene benzer hüküm, tümcecik ve işleçlerden oluşur.

+ **`searchFields`** , sorgu yürütmesini belirli alanlara kısıtlamak için kullanılır.

Yanıtlar Ayrıca sorguya dahil ettiğiniz parametrelere göre şekillendirilir. Örnekte, sonuç kümesi, **`select`** bildiriminde listelenen alanlardan oluşur. Bu sorguda yalnızca ilk 10 eşleşme döndürülür, ancak **`count`** her bir kaç belgenin genel olarak eşleştiğini söyler. Bu sorguda, satırlar daysOnMarket 'e göre sıralanır.

Azure Search, sorgu yürütmesi her zaman bir dizine göre belirlenir ve istekte belirtilen bir API anahtarı kullanılarak kimlik doğrulaması yapılır. REST 'de, her ikisi de istek üst bilgilerinde sağlanır.

### <a name="how-to-run-this-query"></a>Bu sorgu nasıl çalıştırılır

Bu sorguyu yürütmek için [Arama Gezgini 'ni ve gerçek emlak tanıtım dizinini](search-get-started-portal.md)kullanın. 

Bu sorgu dizesini gezgin 'in arama çubuğuna yapıştırabilirsiniz:`search=seattle townhouse +lake&searchFields=description, city&$count=true&$select=listingId, street, status, daysOnMarket, description&$top=10&$orderby=daysOnMarket`

## <a name="how-query-operations-are-enabled-by-the-index"></a>Sorgu işlemleri dizin tarafından nasıl etkinleştirilir

Dizin tasarımı ve sorgu tasarımı Azure Search sıkı bir şekilde bağlanmış. En baştan haberdar olmak için önemli bir olgu, *dizin şemasının*her bir alanda bulunan özniteliklerle, oluşturabileceğiniz sorgu türünü belirler. 

Bir alandaki dizin öznitelikleri, izin verilen işlemleri ayarlar; bir alanın dizinde *aranabilir* olup olmadığı, sonuçlarda *alınabilir* , *sıralanabilir*, *filtrelenebilir*ve benzeri. Örnek sorgu dizesinde, `"$orderby": "daysOnMarket"` yalnızca daysonmarket alanı dizin şemasında *sıralanabilir* olarak işaretlendiğinden geçerlidir. 

![Emlak örneği Için Dizin tanımı](./media/search-query-overview/realestate-sample-index-definition.png "Emlak örneği Için Dizin tanımı")

Yukarıdaki ekran görüntüsü, gerçek emlak örneğine ait dizin özniteliklerinin kısmi bir listesidir. Tüm Dizin şemasını portalda görüntüleyebilirsiniz. Dizin öznitelikleri hakkında daha fazla bilgi için bkz. [Create ındex REST API](https://docs.microsoft.com/rest/api/searchservice/create-index).

> [!Note]
> Bazı sorgu işlevleri, her alan temelinde değil, Dizin genelinde etkindir. Bu yetenekler şunlardır: [eş anlamlı haritalar](search-synonyms.md), [özel çözümleyiciler](index-add-custom-analyzers.md), [öneri aracı yapıları (otomatik tamamlama ve önerilen sorgular için)](index-add-suggesters.md), [sıralama sonuçları için Puanlama mantığı](index-add-scoring-profiles.md).

## <a name="elements-of-a-query-request"></a>Bir sorgu isteği öğeleri

Sorgular her zaman tek bir dizine yönlendirilir. Bir sorgu hedefi olarak dizinlere katılamaz veya özel veya geçici veri yapıları oluşturamazsınız. 

Bir sorgu isteğindeki gerekli öğeler aşağıdaki bileşenleri içerir:

+ Sabit ve Kullanıcı tanımlı bileşenleri içeren bir URL olarak ifade edilen hizmet uç noktası ve Dizin belgeleri koleksiyonu: **`https://<your-service-name>.search.windows.net/indexes/<your-index-name>/docs`**
+ **`api-version`** (Yalnızca REST), API 'nin birden fazla sürümü her zaman kullanılabilir olduğundan gereklidir. 
+ **`api-key`** ya bir sorgu ya da yönetici API anahtarı, hizmetinize yönelik isteğin kimliğini doğrular.
+ **`queryType`** , basit veya tam, yerleşik varsayılan basit sözdizimi kullanıyorsanız atlanabilir.
+ **`search`** ya **`filter`** da boş bir arama gerçekleştirmek istiyorsanız bu parametre belirtilmeyen bir eşleşme ölçütü sağlar. Her iki sorgu türü de basit ayrıştırıcı açısından ele alınmıştır ancak gelişmiş sorgular bile karmaşık sorgu ifadelerini geçirmek için arama parametresi gerektirir.

Diğer tüm arama parametreleri isteğe bağlıdır. Özniteliklerin tam listesi için bkz. [Dizin oluşturma (REST)](https://docs.microsoft.com/rest/api/searchservice/create-index). İşlemler sırasında parametrelerin nasıl kullanıldığına daha yakından bakmak için, bkz. [tam metin aramasının Azure Search nasıl çalıştığı](search-lucene-query-architecture.md).

## <a name="choose-a-parser-simple--full"></a>Ayrıştırıcı seçin: basit | tümünü

Azure Search Apache Lucene üzerinde bulunur ve tipik ve özelleştirilmiş sorguları işlemek için iki sorgu ayrıştırıcıları arasında seçim sağlar. Basit ayrıştırıcı kullanan istekler [basit sorgu söz dizimi](query-simple-syntax.md)kullanılarak formüle sahiptir ve ücretsiz form metin sorgularında kendi hızı ve verimliliği için varsayılan olarak seçilidir. Bu sözdizimi, AND, OR, NOT, tümcecik, sonek ve öncelik işleçlerini içeren bir dizi yaygın arama işlecini destekler.

İsteğe eklerken `queryType=full` etkinleştirilen [tam Lucene sorgu söz dizimi](query-Lucene-syntax.md#bkmk_syntax), [Apache Lucene](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)'in bir parçası olarak geliştirilen, yaygın olarak benimsenen ve ifade eden sorgu dilini kullanıma sunar. Tam sözdizimi basit sözdizimini genişletir. Basit sözdizimi için yazdığınız herhangi bir sorgu, tam Lucene ayrıştırıcısı altında çalışır. 

Aşağıdaki örneklerde nokta gösterilmektedir: aynı sorgu, ancak farklı queryType ayarları ile farklı sonuçlar elde edin. İlk sorguda `^3` , arama teriminin bir parçası olarak kabul edilir.

```
queryType=simple&search=mountain beach garden ranch^3&searchFields=description&$count=true&$select=listingId, street, status, daysOnMarket, description&$top=10&$orderby=daysOnMarket
```

Tam Lucene ayrıştırıcısı kullanılarak aynı sorgu, bu belirli terimi içeren sonuçların arama derecesini artıran "Ranch" üzerinde alan içi artışı yorumlar.

```
queryType=full&search=mountain beach garden ranch^3&searchFields=description&$count=true&$select=listingId, street, status, daysOnMarket, description&$top=10&$orderby=daysOnMarket
```

<a name="types-of-queries"></a>

## <a name="types-of-queries"></a>Sorgu türleri

Azure Search, çok çeşitli sorgu türlerini destekler. 

| Sorgu türü | Kullanım | Örnekler ve daha fazla bilgi |
|------------|--------|-------------------------------|
| Serbest form metin araması | Arama parametresi ve ayrıştırıcı| Tam metin arama, dizininizdeki tüm *aranabilir* alanlarda bir veya daha fazla terimi tarar ve Google veya Bing gibi bir arama altyapısının çalışmasını istediğiniz şekilde çalışır. Giriş bölümündeki örnek tam metin aramadır.<br/><br/>Tam metin araması, standart Lucene Çözümleyicisi (varsayılan olarak), tüm terimleri düşürmek için "The" gibi durdurma sözcüklerini kullanarak metin analizinden daha az gider. Varsayılan ayarı, [İngilizce olmayan çözümleyiciler](index-add-language-analyzers.md#language-analyzer-list) veya metin analizini değiştiren [özel dilden bağımsız çözümleyiciler](index-add-custom-analyzers.md#AnalyzerTable) ile geçersiz kılabilirsiniz. Bir alanın tüm içeriğini tek bir belirteç olarak ele alan bir [anahtar sözcük](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) örneğidir. Bu, ZIP kodları, kimlikler ve bazı ürün adları gibi veriler için yararlıdır. | 
| Filtrelenmiş arama | [OData filtre ifadesi](query-odata-filter-orderby-syntax.md) ve her iki ayrıştırıcı | Filtre sorguları bir dizin içindeki tüm *filtrelenebilir* alanlar üzerinde bir Boole ifadesi değerlendirir. Aramanın aksine, bir filtre sorgusu, dize alanlarında büyük/küçük harf duyarlılığı dahil olmak üzere bir alanın tam içeriğiyle eşleşir. Farklı bir farklılık, filtre sorgularının OData sözdiziminde ifade edildiği bir farktır. <br/>[Filtre ifadesi örneği](search-query-simple-examples.md#example-3-filter-queries) |
| Coğrafi arama | Alan, filtre ifadesi ve herhangi bir ayrıştırıcıda [Edm. Geographon noktası türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) | EDM. Geographi noktası bulunan bir alanda depolanan koordinatlar "yakın ben bul" veya harita tabanlı arama denetimleri için kullanılır. <br/>[Coğrafi arama örneği](search-query-simple-examples.md#example-5-geo-search)|
| Aralık araması | Filtre ifadesi ve basit ayrıştırıcı | Azure Search, Aralık sorguları filtre parametresi kullanılarak oluşturulur. <br/>[Aralık filtresi örneği](search-query-simple-examples.md#example-4-range-filters) | 
| [Parçalı arama](query-lucene-syntax.md#bkmk_fields) | Arama parametresi ve tam ayrıştırıcı | Tek bir alanı hedefleyen bir bileşik sorgu ifadesi oluşturun. <br/>[Parçalı arama örneği](search-query-lucene-examples.md#example-2-fielded-search) |
| [benzer arama](query-lucene-syntax.md#bkmk_fuzzy) | Arama parametresi ve tam ayrıştırıcı | Benzer bir yapım veya yazım denetimi olan koşullara göre eşleşir. <br/>[Benzer arama örneği](search-query-lucene-examples.md#example-3-fuzzy-search) |
| [yakınlık araması](query-lucene-syntax.md#bkmk_proximity) | Arama parametresi ve tam ayrıştırıcı | Belgedeki birbirini yakın terimleri bulur. <br/>[Yakınlık arama örneği](search-query-lucene-examples.md#example-4-proximity-search) |
| [terim artırma](query-lucene-syntax.md#bkmk_termboost) | Arama parametresi ve tam ayrıştırıcı | Bir belgeyi, daha fazla olmayan diğerlerine göre, daha yüksek bir dönem içeriyorsa, daha yüksek bir şekilde derecelendirir. <br/>[Terim artırma örneği](search-query-lucene-examples.md#example-5-term-boosting) |
| [normal ifade arama](query-lucene-syntax.md#bkmk_regex) | Arama parametresi ve tam ayrıştırıcı | Normal bir ifadenin içeriğine göre eşleşir. <br/>[Normal ifade örneği](search-query-lucene-examples.md#example-6-regex) |
|  [joker karakter veya ön ek arama](query-lucene-syntax.md#bkmk_wildcard) | Arama parametresi ve tam ayrıştırıcı | Bir önek ve tilde (`~`) veya tek karakter (`?`) temelinde eşleşir. <br/>[Joker karakter arama örneği](search-query-lucene-examples.md#example-7-wildcard-search) |

## <a name="manage-search-results"></a>Arama sonuçlarını Yönet 

Sorgu sonuçları REST API JSON belgeleri olarak akışlardır, ancak .NET API 'Lerini kullanıyor olsanız da serileştirme yerleşiktir. Sorgudaki parametreleri ayarlayarak, yanıt için belirli alanlar seçerek sonuçları şekillendirebilirsiniz.

Sorgudaki parametreler, sonuç kümesini aşağıdaki yollarla yapılandırmak için kullanılabilir:

+ Sonuçlarda belge sayısını sınırlama veya toplu işleme (varsayılan olarak 50)
+ Sonuçlara dahil edilecek alanları seçme
+ Sıralama düzeni ayarlama
+ Arama sonuçlarının gövdesinde, eşleşen koşullara dikkat çekmek için isabet vurgulamaları ekleme

### <a name="tips-for-unexpected-results"></a>Beklenmeyen sonuçlara yönelik ipuçları

Kimi zaman, sonuçların yapısını değil, her zaman için. Sorgu sonuçları görmeyi beklediğiniz gibi değilse sonuçların iyileştirediğini görmek için bu sorgu değişikliklerini deneyebilirsiniz:

+ Ölçütlerin **`searchMode=any`** herhangi biri yerine tüm **`searchMode=all`** ölçütlerde eşleşme gerektirmek için (varsayılan) öğesini olarak değiştirin. Bu, Boolean işleçleri sorguyu dahil edildiğinde özellikle doğrudur.

+ Metin veya sözcük temelli analiz gerekliyse sorgu tekniğini değiştirin, ancak sorgu türü, dil işlemesini önceden halizler. Tam metin aramasında, yazım hataları, tekil çoğul sözcük formları ve hatta düzensiz fiiller ya da isimler için metin veya sözlü analizler oto düzeltir. Benzer veya joker karakter araması gibi bazı sorgular için, metin Analizi sorgu ayrıştırma işlem hattının bir parçası değildir. Bazı senaryolarda, normal ifadeler geçici bir çözüm olarak kullanılmıştır. 

### <a name="paging-results"></a>Disk belleği sonuçları
Azure Search, arama sonuçlarının sayfalanması uygulamasını kolaylaştırır. **`top`** **Ve`skip`** parametrelerini kullanarak, iyi arama kullanıcı arabirimi uygulamalarını kolayca etkinleştiren, yönetilebilir, sıralı alt kümeler halinde toplam arama sonuçları kümesini almanızı sağlayan arama isteklerini sorunsuz bir şekilde oluşturabilirsiniz. Bu daha küçük sonuç alt kümelerini alırken, tüm arama sonuçları kümesindeki belge sayısını da alabilirsiniz.

[Azure Search'te arama sonuçlarını numaralandırma](search-pagination-page-layout.md) makalesinde arama sonuçlarının numaralanması hakkında daha fazla bilgi alabilirsiniz.

### <a name="ordering-results"></a>Sonuçları sıralama
Bir arama sorgusunun sonuçları alınırken, Azure Search'ün sonuçları belirli bir alandaki değerlere göre sıralayarak sunmasını isteyebilirsiniz. Varsayılan olarak Azure Search, her bir belgenin [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)'den türetilen arama puanı sıralamasını temel alarak arama sonuçlarını sıralar.

Azure Search, arama puanından farklı bir değere göre sıralanmış sonuçlarınızı döndürmesini istiyorsanız **`orderby`** arama parametresini kullanabilirsiniz. Alan adlarını ve jeo-uzamsal değerler **`orderby`** [ **`geo.distance()` işlevine**](query-odata-filter-orderby-syntax.md) çağrıları dahil etmek için parametresinin değerini belirtebilirsiniz. Sonuçların artan sırada istendiğini belirtmek ve `asc` **`desc`** sonuçların azalan sırada istendiğini belirtmek için, her ifadeye sonra gelebilir. Artan sıralama varsayılandır.


### <a name="hit-highlighting"></a>İsabet vurgulama
Azure Search ' de arama sorgusuyla eşleşen arama sonuçlarının tam bölümünü vurgulayarak **`highlight`** , **`highlightPreTag`** , ve **`highlightPostTag`** parametreleri kullanılarak kolayca yapılır. Hangi *aranabilir* alanların eşleşen metninin vurgulanacağının yanı sıra Azure Search'ün döndürdüğü eşleşen metnin başına ve sonuna eklenecek dize etiketlerini tam olarak belirtebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Tam metin aramasının Azure Search nasıl çalıştığı (sorgu ayrıştırma mimarisi)](search-lucene-query-architecture.md)
+ [Arama Gezgini](search-explorer.md)
+ [.NET 'te sorgulama](search-query-dotnet.md)
+ [REST 'te sorgulama](search-create-index-rest-api.md)
