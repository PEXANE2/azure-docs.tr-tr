---
title: Dizinde arama sonuçlarının kapsamını belirleme filtreleri Azure Search
description: Microsoft Azure üzerinde barındırılan bir bulut arama hizmeti olan Azure Search sorgulardaki arama sonuçlarını azaltmak için Kullanıcı güvenlik kimliğine, dile, coğrafi konuma veya sayısal değerlere göre filtreleyin.
author: HeidiSteen
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 49af6f1f535df098aa45cccd7e2d629ff6ccef50
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69649840"
---
# <a name="filters-in-azure-search"></a>Azure Search'te filtreler 

*Filtre* , bir Azure Search sorgusunda kullanılan belgeleri seçme ölçütlerini sağlar. Filtrelenmemiş arama dizindeki tüm belgeleri içerir. Filtre, bir arama sorgusunun bir belge alt kümesiyle kapsamını sorgular. Örneğin, bir filtre tam metin aramasını yalnızca belirli bir marka veya renge sahip ürünlerle, belirli bir eşiğin üzerindeki fiyat noktalarında kısıtlayabilir.

Bazı arama deneyimleri, uygulamanın bir parçası olarak filtre gereksinimleri uygular, ancak *değer tabanlı* ölçütler kullanarak aramayı kısıtlamak istediğiniz zaman, "tarafından yayımlanamayan" Kategori için "Kitaplar" türünde Simon & Schuster ").

Bunun yerine, hedefiniz belirli veri *yapılarında* (bir müşteri gözden geçirmeler alanında arama kapsamını belirleme) arama işlemi gerçekleştirdiyse, aşağıda açıklanan alternatif yöntemler vardır.

## <a name="when-to-use-a-filter"></a>Bir filtrenin ne zaman kullanılacağı

Filtreler, "yakın beni bul", çok yönlü gezinme ve yalnızca bir kullanıcının görebileceği belgeleri gösteren güvenlik filtreleri gibi çeşitli arama deneyimlerine göre belirlenir. Bu deneyimlerden herhangi birini uygularsanız, bir filtre gereklidir. Bu, coğrafi konum koordinatlarını, Kullanıcı tarafından seçilen model kategorisini veya istek sahibinin Güvenlik KIMLIĞINI sağlayan arama sorgusuna iliştirilen filtredir.

Örnek senaryolar aşağıdakileri içerir:

1. Dizininizi dizindeki veri değerlerine göre dilimlemek için bir filtre kullanın. Şehir, muhafazası türü ve değişiklik içeren bir şema verildiğinde, kriterlerinizi karşılayan belgeleri açıkça seçmek için bir filtre oluşturabilirsiniz (Seattle, Condos, su ön). 

   Aynı girişlerle tam metin araması genellikle benzer sonuçlar üretir, ancak bir filtre, dizininizdeki içeriğe karşı filtre terimiyle tam bir eşleşme gerektirdiğinden daha kesin hale gelir. 

2. Arama deneyimi bir filtre gereksinimiyle birlikte geliyorsa bir filtre kullanın:

   * Çok [yönlü gezinme](search-faceted-navigation.md) , Kullanıcı tarafından seçilen model kategorisini geri geçirmek için bir filtre kullanır.
   * Coğrafi arama "yakın beni bul" uygulamalarında geçerli konumun koordinatlarını geçirmek için bir filtre kullanır. 
   * Güvenlik filtreleri güvenlik tanımlayıcılarını bir filtre ölçütü olarak iletir, burada dizindeki bir eşleşme, belgeye erişim hakları için bir proxy görevi görür.

3. Sayısal bir alanda arama ölçütünüz istiyorsanız filtre kullanın. 

   Sayısal alanlar belgeye alınabilir ve arama sonuçlarında görünebilir, ancak aranabilir değildir (tam metin aramasına tabi değildir). Sayısal verilere dayalı seçim ölçütlerine ihtiyacınız varsa bir filtre kullanın.

### <a name="alternative-methods-for-reducing-scope"></a>Kapsam azaltma için alternatif Yöntemler

Arama sonuçlarınızda bir daraltma efekti istiyorsanız, filtreler tek seçiminiz değildir. Bu alternatifler amacınıza bağlı olarak daha iyi bir uyum sağlayabilir:

 + `searchFields`sorgu parametresi Pegs aramasını belirli alanlara ekler. Örneğin, dizininiz Ingilizce ve Ispanyolca açıklamaları için ayrı alanlar sağlıyorsa, tam metin aramasında kullanılacak alanları hedeflemek için searchFields ' i kullanabilirsiniz. 

+ `$select`parametresi, bir sonuç kümesinde hangi alanların ekleneceğini belirlemek için kullanılır, yanıtı çağıran uygulamaya göndermeden önce etkin bir şekilde kırpmaz. Bu parametre sorguyu iyileştirmez veya belge koleksiyonunu azaltır, ancak daha küçük bir yanıt hedefiniz ise, bu parametre dikkate alınması gereken bir seçenektir. 

Her iki parametre hakkında daha fazla bilgi için bkz. [arama belgeleri > istek > sorgu parametreleri](https://docs.microsoft.com/rest/api/searchservice/search-documents#request).


## <a name="how-filters-are-executed"></a>Filtrelerin nasıl yürütüldüğü

Sorgu zamanında, bir filtre ayrıştırıcısı ölçütü girdi olarak kabul eder, ifadeyi bir ağaç olarak temsil edilen atomik Boole ifadelerine dönüştürür ve sonra filtre ağacını bir dizin içindeki filtrelenebilir alanlar üzerinde değerlendirir.

Filtreleme, arama ile birlikte meydana gelir ve belge alımı ve ilgi Puanlama için aşağı akış işleme dahil edilecek belgeleri niteleyen şekilde kapsar. Bir arama dizesiyle eşlendiğinde, filtre, sonraki arama işleminin geri çağırma kümesini etkin bir şekilde azaltır. Tek başına kullanıldığında (örneğin, sorgu dizesi nerede `search=*`boşsa), filtre ölçütü tek giriştir. 

## <a name="defining-filters"></a>Filtreleri tanımlama

Filtreler OData ifadeleridir, [Azure Search içinde desteklenen OData v4 sözdizimi alt kümesi](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search)kullanılarak ifade edilir. 

Her **arama** işlemi için bir filtre belirtebilirsiniz, ancak filtrenin kendisi birden çok alan, birden çok ölçüt içerebilir ve bir **IMatch** işlevi kullanırsanız birden çok tam metin arama ifadesi olabilir. Çok parçalı bir filtre ifadesinde, koşulları herhangi bir sırada belirtebilirsiniz (işleç önceliği kurallarına tabidir). Belirli bir dizideki koşulları yeniden düzenlemenize çalışırsanız, performansın hiçbir şekilde ele geçirmesine izin verilmez.

Filtre ifadesindeki sınırlardan biri, isteğin en büyük boyut sınırıdır. Filtrenin tamamında tüm istek, POST için en fazla 16 MB veya GET için 8 KB olabilir. Ayrıca, filtre ifadenizde yan tümce sayısında bir sınır vardır. Güzel bir Thumb kuralı, yüzlerce yan tümce varsa, bu sınıra çalışma riski altında olursunuz. Uygulamanızı, sınırsız boyut filtresi oluşturmamak üzere tasarlaması önerilir.

Aşağıdaki örneklerde, birkaç API 'de Prototipik filtre tanımları temsil etmektedir.

```http
# Option 1:  Use $filter for GET
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2019-05-06

# Option 2: Use filter for POST and pass it in the request body
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2019-05-06
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

```csharp
    parameters =
        new SearchParameters()
        {
            Filter = "baseRate lt 150",
            Select = new[] { "hotelId", "description" }
        };

    var results = searchIndexClient.Documents.Search("*", parameters);
```

## <a name="filter-usage-patterns"></a>Kullanım düzenlerini filtrele

Aşağıdaki örneklerde, filtre senaryoları için çeşitli kullanım desenleri gösterilmektedir. Daha fazla fikir için bkz. [OData Expression sözdizimi > örnekleri](https://docs.microsoft.com/azure/search/search-query-odata-filter#examples).

+ Bir sorgu dizesi olmadan tek başına **$Filter**, filtre ifadesi ilgilendiğiniz belgeleri tamamen niteleyebiliyorlarsa yararlı olur. Sorgu dizesi olmadan, sözcük temelli veya dil analizi, Puanlama yok ve derecelendirme yoktur. Arama dizesinin yalnızca bir yıldız işareti olduğunu, yani "tüm belgeleri Eşleştir" anlamına geldiğini unutmayın.

   ```
   search=*&$filter=(baseRate ge 60 and baseRate lt 300) and accommodation eq 'Hotel' and city eq 'Nogales'
   ```

+ Filtrenin alt kümesini oluşturduğu sorgu dizesi ve **$Filter**birleşimi ve sorgu dizesi filtrelenmiş alt küme üzerinde tam metin araması için terim girişi sağlar. Sorgu dizesiyle bir filtre kullanılması en yaygın kullanım modelidir.

   ```
   search=hotels ocean$filter=(baseRate ge 60 and baseRate lt 300) and city eq 'Los Angeles'
   ```

+ "Veya" ile ayrılmış, her biri kendi filtre ölçütlerine sahip (örneğin, ' köpek ' veya ' Siamese ', ' Cat ' içinde ' Beagles ') bileşik sorgular. İle `or` birleştirilmiş ifadeler, yanıtta geri gönderilen her ifadeyle eşleşen belgelerin birleşimi ile ayrı ayrı değerlendirilir. Bu kullanım deseninin `search.ismatchscoring` işlevi aracılığıyla elde edilir. Puanlama olmayan sürümü `search.ismatch`de kullanabilirsiniz.

   ```
   # Match on hostels rated higher than 4 OR 5-star motels.
   $filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5

   # Match on 'luxury' or 'high-end' in the description field OR on category exactly equal to 'Luxury'.
   $filter=search.ismatchscoring('luxury | high-end', 'description') or category eq 'Luxury'
   ```

  Bunun `search.ismatchscoring` yerine `$filter` `search` kullanarak `and`tam metin aramasını filtreler ile birleştirmek de mümkündür ,ancakbu,biraramaisteğindekiveparametrelerikullanılarakişlevselolarakeşdeğerdir.`or` Örneğin, aşağıdaki iki sorgu aynı sonucu üretir:

  ```
  $filter=search.ismatchscoring('pool') and rating ge 4

  search=pool&$filter=rating ge 4
  ```

Belirli kullanım durumlarında kapsamlı yönergeler için bu makalelerle takip edin:

+ [Model filtreleri](search-filters-facets.md)
+ [Dil filtreleri](search-filters-language.md)
+ [Güvenlik kırpma](search-security-trimming-for-azure-search.md) 

## <a name="field-requirements-for-filtering"></a>Filtreleme için alan gereksinimleri

REST API, filtrelenebilir, basit alanlar için varsayılan olarak *Açık* olur. Filtrelenebilir alanlar dizin boyutunu artırır; Filtre içinde gerçekten kullanmayı `"filterable": false` planlamadığınız alanlar için ayarladığınızdan emin olun. Alan tanımlarının ayarları hakkında daha fazla bilgi için bkz. [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index).

.NET SDK 'sında filtrelenebilir varsayılan olarak *kapalıdır* . İlgili [alan](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field?view=azure-dotnet) nesnesinin `true` [ısfilterable özelliğini](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field.isfilterable?view=azure-dotnet) olarak ayarlayarak bir alanı filtrelenebilir hale getirebilirsiniz. Bunu, [ısfilterable özniteliğini](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.isfilterableattribute)kullanarak bildirimli olarak da yapabilirsiniz. Aşağıdaki örnekte, özniteliği Dizin tanımıyla eşleşen bir model sınıfının `BaseRate` özelliği üzerinde ayarlanır.

```csharp
    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }
```

### <a name="making-an-existing-field-filterable"></a>Varolan bir alanı filtrelenebilir hale getirme

Mevcut alanları filtrelenebilir hale getirmek için değiştiremezsiniz. Bunun yerine, yeni bir alan eklemeniz veya dizini yeniden oluşturmanız gerekir. Bir dizini yeniden oluşturma veya alanları yeniden doldurma hakkında daha fazla bilgi için bkz. [Azure Search dizin oluşturma](search-howto-reindex.md).

## <a name="text-filter-fundamentals"></a>Metin filtresi temelleri

Metin filtreleri, filtre içinde sağladığınız sabit dizeler ile dize alanlarıyla eşleşir. Tam metin aramasının aksine, metin filtreleri için sözcük temelli analiz veya sözcük bölme yoktur, bu nedenle karşılaştırmalar yalnızca tam eşleşmeler için geçerlidir. Örneğin, *f* alanının "Sunny günü" içerdiğini varsayalım, `$filter=f eq 'Sunny'` eşleşmez, ancak `$filter=f eq 'sunny day'` işlem yapar. 

Metin dizeleri büyük/küçük harfe duyarlıdır. Büyük küçük harf olmayan sözcüklerin olmaması: `$filter=f eq 'Sunny day'` "Sunny günü" bulmayacak.

### <a name="approaches-for-filtering-on-text"></a>Metinde filtreleme yaklaşımları

| Yaklaşım | Açıklama | Kullanılması gereken durumlar |
|----------|-------------|-------------|
| [`search.in`](search-query-odata-search-in-function.md) | Ayrılmış bir dize listesine karşı bir alanla eşleşen bir işlev. | Birçok ham metin değerinin bir dize alanı ile eşleştirilmesi gereken, [Güvenlik filtreleri](search-security-trimming-for-azure-search.md) ve tüm filtreler için önerilir. **Search.in** işlevi hız için tasarlanmıştır ve ve `eq` `or`kullanarak alanı her bir dizeye göre açıkça karşılaştırmadan çok daha hızlıdır. | 
| [`search.ismatch`](search-query-odata-full-text-search-functions.md) | Aynı filtre ifadesinde tam metin arama işlemlerini kesin olarak Boolean filtre işlemleriyle karıştırabilmeniz için bir işlev. | Tek bir istekte birden çok arama filtresi kombinasyonu istediğinizde **Search. IsMatch** (veya Puanlama eşdeğerini, **arama. ısmatchpuanlama**) kullanın. Ayrıca, daha büyük bir dizedeki kısmi bir dizeyi filtrelemek için bir *Contains* filtresi için de kullanabilirsiniz. |
| [`$filter=field operator string`](search-query-odata-comparison-operators.md) | Alanlar, işleçler ve değerlerden oluşan Kullanıcı tanımlı bir ifade. | Bir dize alanı ve bir dize değeri arasındaki tam eşleşmeleri bulmak istediğinizde bunu kullanın. |

## <a name="numeric-filter-fundamentals"></a>Sayısal filtre temelleri

Sayısal alanlar tam metin `searchable` araması bağlamında değil. Yalnızca dizeler tam metin aramasına tabidir. Örneğin, bir arama terimi olarak 99,99 girerseniz, öğelerin fiyatı $99,99 ' de alınır. Bunun yerine, belgenin dize alanlarında 99 numaralı öğeyi görürsünüz. Bu nedenle, sayısal verileriniz varsa, Bu varsayımını aralıklar, modeller, gruplar vb. dahil olmak üzere filtreler için kullanmanız gerekir. 

Sayısal alanlar (Fiyat, boyut, SKU, KIMLIK) içeren belgeler, alan işaretlenmişse `retrievable`arama sonuçlarında bu değerleri sağlar. Burada nokta, tam metin aramasının kendisinin sayısal alan türleri için geçerli olmadığı bir alandır.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, **$Filter** parametrelerle sorgu göndermek Için portalda **Arama Gezgini** ' ni deneyin. [Gerçek zamanlı örnek dizin](search-get-started-portal.md) , arama çubuğuna yapıştırdığınızda aşağıdaki filtrelenmiş sorgular için ilginç sonuçlar sağlar:

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

Daha fazla örnekle çalışmak için bkz. [OData filtre Ifadesi söz dizimi > örnekleri](https://docs.microsoft.com/azure/search/search-query-odata-filter#examples).

## <a name="see-also"></a>Ayrıca bkz.

+ [Tam metin aramasının Azure Search nasıl çalıştığı](search-lucene-query-architecture.md)
+ [Belgelerde ara REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents)
+ [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [Desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)
