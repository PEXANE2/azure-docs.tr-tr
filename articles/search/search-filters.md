---
title: Arama sonuçlarına filtre uygulayın
titleSuffix: Azure Cognitive Search
description: Microsoft Azure'da barındırılan bir bulut arama hizmeti olan Azure Bilişsel Arama'daki sorgularda arama sonuçlarını azaltmak için kullanıcı güvenlik kimliğine, dile, coğrafi konuma veya sayısal değerlere göre filtre uygulayın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 03333e853a2ab7606ebe60cc3f68bcb5facfbdb4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77191014"
---
# <a name="filters-in-azure-cognitive-search"></a>Azure Bilişsel Arama'daki Filtreler 

*Filtre,* Azure Bilişsel Arama sorgusunda kullanılan belgeleri seçmek için ölçütler sağlar. Filtre uygulanmamış arama, dizindeki tüm belgeleri içerir. Filtre, arama sorgusunun bir belge alt kümesine kadar genişbir kapsama alanıdır. Örneğin, bir filtre tam metin aramasını yalnızca belirli bir marka veya renge sahip ürünlerle, belirli bir eşiğin üzerindeki fiyat noktalarında kısıtlayabilir.

Bazı arama deneyimleri uygulamanın bir parçası olarak filtre gereksinimleri empoze eder, ancak *filtreleri değer tabanlı* ölçütler kullanarak aramayı kısıtlamak istediğinizde kullanabilirsiniz ("Simon & Schuster" tarafından yayınlanan "kurgusal olmayan" kategorisi için ürün türü "kitaplara" arama yıkabilirsiniz).

Bunun yerine hedefiniz belirli veri *yapılarında* (müşteri incelemeleri alanına arama) hedeflenmişse, aşağıda açıklanan alternatif yöntemler vardır.

## <a name="when-to-use-a-filter"></a>Filtre ne zaman kullanılır?

Filtreler, "yakınımda bulun" ve yalnızca kullanıcının görmesine izin verilen belgeleri gösteren güvenlik filtreleri gibi çeşitli arama deneyimlerine temel oluşturur. Bu deneyimlerden herhangi birini uygularsanız, bir filtre gerekir. Coğrafi konum koordinatlarını, kullanıcı tarafından seçilen fason kategorisini veya istekte bulunduranın güvenlik kimliğini sağlayan arama sorgusuna bağlı filtredir.

Örnek senaryolar şunlardır:

1. Dizindeki veri değerlerini temel almak için dizininizi dilimlemek için bir filtre kullanın. Şehir, konut türü ve olanaklariçeren bir şema göz önüne alındığında, (Seattle, kınamak, rıhtım) kriterlerinizi karşılayan belgeleri açıkça seçmek için bir filtre oluşturabilirsiniz. 

   Aynı girdilere sahip tam metin araması genellikle benzer sonuçlar üretir, ancak bir filtre, filtre teriminin dizininizdeki içerikle tam bir eşleşmesini gerektirdiği için daha kesindir. 

2. Arama deneyimi bir filtre gereksinimiyle birlikte geliyorsa filtre kullanın:

   * [Yönlü gezinme,](search-faceted-navigation.md) kullanıcı tarafından seçilen fason kategorisini geri geçirmek için bir filtre kullanır.
   * Coğrafi arama, "yakınımda bulun" uygulamalarında geçerli konumun koordinatlarını geçirmek için bir filtre kullanır. 
   * Güvenlik filtreleri, dizinteki bir eşleşmenin belgeye erişim hakları için proxy olarak hizmet ettiği güvenlik tanımlayıcılarını filtre ölçütleri olarak geçirir.

3. Sayısal bir alanda arama ölçütleri istiyorsanız filtre kullanın. 

   Sayısal alanlar belgede alınabilir ve arama sonuçlarında görünebilir, ancak tek tek aranabilir (tam metin aramaya tabi) değildir. Sayısal verilere dayalı seçim ölçütlerine ihtiyacınız varsa, bir filtre kullanın.

### <a name="alternative-methods-for-reducing-scope"></a>Kapsamı azaltmak için alternatif yöntemler

Arama sonuçlarınızda daraltma efekti istiyorsanız, filtreler tek seçeneğiniz değildir. Bu alternatifler, amacınıza bağlı olarak daha uygun olabilir:

 + `searchFields`sorgu parametresi belirli alanlara arama pegs. Örneğin, dizininiz İngilizce ve İspanyolca açıklamalar için ayrı alanlar sağlıyorsa, tam metin aramaiçin hangi alanların kullanılacağını hedeflemek için arama Alanlarını kullanabilirsiniz. 

+ `$select`parametre, bir sonuç kümesine hangi alanların dahil edileceğini belirtmek ve yanıtı arama uygulamasına göndermeden önce etkili şedi olarak belirtmek için kullanılır. Bu parametre sorguyu hassaslaştırmaz veya belge koleksiyonunu azaltmaz, ancak hedefiniz daha küçük bir yanıtsa, bu parametre göz önünde bulundurulması gereken bir seçenektir. 

Parametre hakkında daha fazla bilgi için [arama belgeleri > Sorgu > Sorgu parametrelerine](/rest/api/searchservice/search-documents#query-parameters)bakın.


## <a name="how-filters-are-executed"></a>Filtreler nasıl çalıştırılır?

Sorgu zamanında, filtre arayıcısı ölçütleri giriş olarak kabul eder, ifadeyi ağaç olarak temsil edilen atomik Boolean ifadelerine dönüştürür ve filtre ağacını bir dizinteki filtrelenebilir alanlar üzerinden değerlendirir.

Filtreleme, belge alma ve alaka düzeyi puanlaması için alt akış işlemlerine hangi belgelerin dahil edilemeye uygun olarak aramayla birlikte gerçekleşir. Bir arama dizesi ile eşleştirildiğinde, filtre sonraki arama işleminin geri çağırma kümesini etkili bir şekilde azaltır. Tek başına kullanıldığında (örneğin, sorgu dizesi boş olduğunda), `search=*`filtre ölçütleri tek giriştir. 

## <a name="defining-filters"></a>Filtreleri tanımlama
Filtreler, [Azure Bilişsel Arama'da desteklenen OData V4 sözdizimi alt kümesi](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search)kullanılarak ifade edilen OData ifadeleridir. 

Her **arama** işlemi için bir filtre belirtebilirsiniz, ancak filtrenin kendisi birden çok alan, birden çok ölçüt içerebilir ve bir **eşleşme** işlevi kullanırsanız, birden çok tam metin arama ifadesi. Çok parçalı filtre ifadesinde, yüklemleri herhangi bir sırada belirtebilirsiniz (işleç önceliği kurallarına tabidir). Belirli bir sırada yüklemleri yeniden düzenlemeye çalışırsanız, performansta kayda değer bir kazanç yoktur.

Filtre ifadesinin sınırlarından biri, isteğin en büyük boyut sınırıdır. Filtre dahil tüm istek, POST için en fazla 16 MB veya GET için 8 KB olabilir. Filtre ifadenizdeki yan tümce sayısı nın da bir sınırı vardır. Başparmak iyi bir kural, yüzlerce yan tümceniz varsa, sınıra girme riskiniz olduğudur. Uygulamanızı sınırsız boyutta filtreler oluşturacak şekilde tasarlamanızı öneririz.

Aşağıdaki örnekler, çeşitli API'lerde prototipfiltre tanımlarını temsil eder.

```http
# Option 1:  Use $filter for GET
GET https://[service name].search.windows.net/indexes/hotels/docs?api-version=2019-05-06&search=*&$filter=Rooms/any(room: room/BaseRate lt 150.0)&$select=HotelId, HotelName, Rooms/Description, Rooms/BaseRate

# Option 2: Use filter for POST and pass it in the request body
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2019-05-06
{
    "search": "*",
    "filter": "Rooms/any(room: room/BaseRate lt 150.0)",
    "select": "HotelId, HotelName, Rooms/Description, Rooms/BaseRate"
}
```

```csharp
    parameters =
        new SearchParameters()
        {
            Filter = "Rooms/any(room: room/BaseRate lt 150.0)",
            Select = new[] { "HotelId", "HotelName", "Rooms/Description" ,"Rooms/BaseRate"}
        };

    var results = searchIndexClient.Documents.Search("*", parameters);
```

## <a name="filter-usage-patterns"></a>Filtre kullanım desenleri

Aşağıdaki örnekler, filtre senaryoları için çeşitli kullanım desenleri göstermektedir. Daha fazla fikir için, [OData ifade sözdizimi > Örnekler'e](https://docs.microsoft.com/azure/search/search-query-odata-filter#examples)bakın.

+ Bağımsız **$filter**, sorgu dizesi olmadan, filtre ifadesi tamamen ilgi belgeleri nitelemek mümkün olduğunda yararlıdır. Sorgu dizesi olmadan, sözlü veya dilsel analiz, puanlama ve sıralama yoktur. Arama dizesinin "tüm belgeleri eşleştirmek" anlamına gelen bir yıldız işareti olduğuna dikkat edin.

   ```
   search=*&$filter=Rooms/any(room: room/BaseRate ge 60 and room/BaseRate lt 300) and Address/City eq 'Honolulu'
   ```

+ Sorgu dizesi ve **$filter**birleşimi, filtrealt kümesini oluşturur ve sorgu dizesi filtreuygulanmış alt küme üzerinden tam metin araması için terim girişlerini sağlar. Terimlerin eklenmesi (yürüme mesafesi tiyatroları), terimlerle en iyi eşleşen belgelerin daha yüksek sırada yer aldığı sonuçlarda arama puanlarını ortaya çıkarır. Sorgu dizeli bir filtre kullanmak en yaygın kullanım desenidir.

   ```
  search=walking distance theaters&$filter=Rooms/any(room: room/BaseRate ge 60 and room/BaseRate lt 300) and Address/City eq 'Seattle'&$count=true
   ```

+ Bileşik sorgular, her biri kendi filtre ölçütlerine sahip olan "veya" ile ayrılmıştır (örneğin, 'köpek' veya 'kedi'deki 'siyam' 'beagles'). İfadeler birlikte `or` ayrı ayrı değerlendirilir ve yanıtta gönderilen her ifadeyle eşleşen belgelerin birleşimi ile birlikte değerlendirilir. Bu kullanım deseni `search.ismatchscoring` işlev aracılığıyla elde edilir. Ayrıca, puanlama olmayan sürümünü `search.ismatch`kullanabilirsiniz.

   ```
   # Match on hostels rated higher than 4 OR 5-star motels.
   $filter=search.ismatchscoring('hostel') and Rating ge 4 or search.ismatchscoring('motel') and Rating eq 5

   # Match on 'luxury' or 'high-end' in the description field OR on category exactly equal to 'Luxury'.
   $filter=search.ismatchscoring('luxury | high-end', 'Description') or Category eq 'Luxury'&$count=true
   ```

  Tam metin aramasını filtreler `search.ismatchscoring` aracılığıyla `and` , `or`ancak bu işlevsel olarak bir arama isteğinde `search` `$filter` ve parametrelerini kullanmaya eşdeğerdir. Örneğin, aşağıdaki iki sorgu aynı sonucu üretir:

  ```
  $filter=search.ismatchscoring('pool') and Rating ge 4

  search=pool&$filter=Rating ge 4
  ```

Belirli kullanım durumları hakkında kapsamlı rehberlik için bu makaleleri takip edin:

+ [Model filtreleri](search-filters-facets.md)
+ [Dil filtreleri](search-filters-language.md)
+ [Güvenlik kırpma](search-security-trimming-for-azure-search.md) 

## <a name="field-requirements-for-filtering"></a>Filtreleme için alan gereksinimleri

REST API'sinde, filtrelenebilir basit alanlar için varsayılan olarak *açıktır.* Filtrelenebilir alanlar dizin boyutunu artırır; bir filtrede `"filterable": false` kullanmayı planlamadığınız alanlar için ayarladığınızdan emin olun. Alan tanımları ayarları hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/create-index)

.NET SDK'da filtrelenebilir varsayılan olarak *kapalıdır.* İlgili [Alan](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field?view=azure-dotnet) nesnesinin `true` [İzlenebilir özelliğini](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field.isfilterable?view=azure-dotnet) 'ye ayarlayarak bir alanı filtrelenebilir hale getirebilirsiniz. Bunu [IsFilterable özniteliğini](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.isfilterableattribute)kullanarak da bildirimsel olarak yapabilirsiniz. Aşağıdaki örnekte, öznitelik dizin `BaseRate` tanımıyla eşleyen bir model sınıfının özelliğine ayarlanır.

```csharp
    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }
```

### <a name="making-an-existing-field-filterable"></a>Varolan bir alanı filtrelenebilir hale getirme

Varolan alanları filtrelenebilir hale getirmek için değiştiremezsiniz. Bunun yerine, yeni bir alan eklemeniz veya dizini yeniden oluşturmanız gerekir. Bir dizin yeniden oluşturma veya alanları yeniden doldurma hakkında daha fazla bilgi için azure [Bilişsel Arama dizinini nasıl yeniden oluşturursunuz'](search-howto-reindex.md)a bakın.

## <a name="text-filter-fundamentals"></a>Metin filtresi temelleri

Metin filtreleri, dize alanlarını filtrede sağladığınız gerçek dizelerle eşleşir. Tam metin aramanın aksine, metin filtreleri için sözlü çözümleme veya sözcük kırma yoktur, bu nedenle karşılaştırmalar yalnızca tam eşleşmeler içindir. Örneğin, *f* alanının "güneşli gün" `$filter=f eq 'Sunny'` içerdiğini, eşleşmediğini, ancak `$filter=f eq 'sunny day'` içereceğini varsayalım. 

Metin dizeleri büyük/küçük harf duyarlıdır. Büyük harfli kelimelerin daha düşük kasası `$filter=f eq 'Sunny day'` yoktur: "güneşli gün" bulamaz.

### <a name="approaches-for-filtering-on-text"></a>Metin üzerinde filtreleme için yaklaşımlar

| Yaklaşım | Açıklama | Kullanılması gereken durumlar |
|----------|-------------|-------------|
| [`search.in`](search-query-odata-search-in-function.md) | Bir alanı sınırlı dizelistesiyle eşleştirebilen bir işlev. | Güvenlik [filtreleri](search-security-trimming-for-azure-search.md) ve birçok ham metin değerlerinin bir dize alanıyla eşleştirilmiş olması gereken filtreler için önerilir. **search.in** fonksiyonu hız için tasarlanmıştır ve açıkça kullanarak `eq` her dize karşı `or`alanı karşılaştırarak çok daha hızlıdır. | 
| [`search.ismatch`](search-query-odata-full-text-search-functions.md) | Tam metin arama işlemlerini kesinlikle Boolean filtre işlemleriyle aynı filtre ifadesinde karıştırmanızı sağlayan bir işlev. | Tek bir istekte birden çok arama filtresi birleşimini istediğinizde **search.ismatch** (veya puanlama eşdeğeri **search.ismatchscoring)** kullanın. Daha büyük bir dize içinde kısmi bir dize üzerinde filtrelemek için *bir içerir* filtre için de kullanabilirsiniz. |
| [`$filter=field operator string`](search-query-odata-comparison-operators.md) | Alanlar, işleçler ve değerlerden oluşan kullanıcı tanımlı bir ifade. | Dize alanı ile dize değeri arasındaki tam eşleşmeleri bulmak istediğinizde bunu kullanın. |

## <a name="numeric-filter-fundamentals"></a>Sayısal filtre temelleri

Sayısal alanlar tam `searchable` metin arama bağlamında değildir. Yalnızca dizeleri tam metin arama tabidir. Örneğin, bir arama terimi olarak 99,99 girerseniz, 99,99 TL fiyatlandırılan öğeleri geri almazsınız. Bunun yerine, belgenin dize alanlarında 99 sayısına sahip öğeleri görürsünüz. Bu nedenle, sayısal verileriniz varsa, bunları aralıklar, fatlar, gruplar vb. dahil olmak üzere filtreler için kullanacağınız varsayımı vardır. 

Sayısal alanlar (fiyat, boyut, SKU, ID) içeren belgeler, alan işaretlenirse `retrievable`arama sonuçlarında bu değerleri sağlar. Burada nokta tam metin arama kendisi sayısal alan türleri için geçerli değildir.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, **$filter** parametreleri olan sorguları göndermek için portalda **Arama gezginini** deneyin. [Gayrimenkul örnek dizini,](search-get-started-portal.md) bunları arama çubuğuna yapıştırdığınızda aşağıdaki filtreuygulanmış sorgular için ilginç sonuçlar sağlar:

```
# Geo-filter returning documents within 5 kilometers of Redmond, Washington state
# Use $count=true to get a number of hits returned by the query
# Use $select to trim results, showing values for named fields only
# Use search=* for an empty query string. The filter is the sole input

search=*&$count=true&$select=description,city,postCode&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5

# Numeric filters use comparison like greater than (gt), less than (lt), not equal (ne)
# Include "and" to filter on multiple fields (baths and bed)
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=baths gt 3 and beds gt 4

# Text filters can also use comparison operators
# Wrap text in single or double quotes and use the correct case
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=city gt 'Seattle'
```

Daha fazla örnekle çalışmak için, [OData Filtresi İfadesözdizimi > Örnekler'e](https://docs.microsoft.com/azure/search/search-query-odata-filter#examples)bakın.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama’da tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)
+ [Belgelerde Arama REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents)
+ [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [Desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)
