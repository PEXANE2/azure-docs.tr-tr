---
title: Basit Lucene sorgu söz dizimini kullan
titleSuffix: Azure Cognitive Search
description: Bir Azure Bilişsel Arama dizininde tam metin araması, filtre arama, coğrafi arama ve çok yönlü arama için basit söz dizimi temelinde sorgu çalıştırarak örneği öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/12/2020
ms.openlocfilehash: 51d36211c7ffa0507a186c9a1e1f2b52d478fe4e
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2020
ms.locfileid: "97369111"
---
# <a name="use-the-simple-search-syntax-in-azure-cognitive-search"></a>Azure 'da "basit" arama sözdizimini kullanın Bilişsel Arama

Azure Bilişsel Arama 'de, [basit sorgu söz dizimi](query-simple-syntax.md) tam metin araması için varsayılan sorgu ayrıştırıcısını çağırır. Ayrıştırıcı hızlıdır ve tam metin araması, filtrelenmiş ve çok yönlü arama ve ön ek arama gibi yaygın senaryoları işler. Bu makalede, bir [arama belgeleri (REST API)](/rest/api/searchservice/search-documents) isteğindeki basit sözdizimi kullanımını göstermek için örnekler kullanılmaktadır.

> [!NOTE]
> Alternatif bir sorgu söz dizimi, benzer ve joker karakter arama gibi daha karmaşık sorgu yapılarını destekleyen [tam bir Lucene](query-lucene-syntax.md). Daha fazla bilgi ve örnek için bkz. [tam Lucene sözdizimini kullanma](search-query-lucene-examples.md).

## <a name="nyc-jobs-examples"></a>NYC Işleri örnekleri

Aşağıdaki örnekler, [New York OpenData girişiminin City](https://nycopendata.socrata.com/)tarafından sağlanan bir veri kümesine dayalı olarak bulunan Işleri Içeren [NYC işleri arama dizininden](https://azjobsdemo.azurewebsites.net/) yararlanır. Bu verilerin geçerli veya tamamlanmış olarak kabul edilmemelidir. Dizin, Microsoft tarafından sağlanmış bir korumalı alan hizmetidir ve bu sorguları denemek için bir Azure aboneliğine veya Azure Bilişsel Arama ihtiyaç duymayın.

Ne yapmanız gerekir Postman veya GET veya POST sırasında HTTP isteği vermeye yönelik eşdeğer bir araçtır. Bu araçlarla ilgili bilgi sahibi değilseniz bkz. [hızlı başlangıç: Azure Bilişsel Arama REST API araştırma](search-get-started-rest.md).

## <a name="set-up-the-request"></a>İsteği ayarlama

1. İstek üst bilgileri aşağıdaki değerlere sahip olmalıdır:

   | Anahtar | Değer |
   |-----|-------|
   | İçerik Türü | `application/json`|
   | api anahtarı  | `252044BE3886FE4A8E3BAA4F595114BB` </br> (Bu, NYC Işleri dizinini barındıran korumalı alan arama hizmeti için gerçek sorgu API anahtarıdır) |

1. Fiili öğesini olarak ayarlayın **`GET`** .

1. URL 'YI olarak ayarlayın **`https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30&search=*&$count=true`** . 

   + Dizindeki belgeler koleksiyonu tüm aranabilir içeriği içerir. İstek üstbilgisinde belirtilen sorgu api anahtarı yalnızca belge koleksiyonunu hedefleyen okuma işlemleri için geçerlidir.

   + **`$count=true`** arama ölçütleriyle eşleşen belgelerin sayısını döndürür. Boş bir arama dizesinde, sayı dizindeki tüm belgeler olacaktır (NYC Işleri durumunda 2558 hakkında).

   + **`search=*`** belirtilmemiş bir sorgu, null veya boş aramaya eşdeğerdir. Özellikle faydalı değildir, ancak yapabileceğiniz en basit aramadır ve tüm değerleri olan dizindeki tüm alınabilir alanları gösterir.

1. Doğrulama adımı olarak, aşağıdaki isteği al öğesine yapıştırın ve **Gönder**' e tıklayın. Sonuçlar ayrıntılı JSON belgeleri olarak döndürülür.

   ```http
   https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30&$count=true&search=*&queryType=full
   ```

### <a name="how-to-invoke-simple-query-parsing"></a>Basit sorgu ayrıştırmayı çağırma

Etkileşimli sorgular için, herhangi bir şey belirtmeniz gerekmez: Simple varsayılandır. Kod içinde, daha önce çağrılırsa, **`queryType=full`** Varsayılan olarak ' ı sıfırlayabilirsiniz **`queryType=simple`** .

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
{
    "count": true,
    "queryType": "simple",
    "search": "*"
}
```

## <a name="example-1-full-text-search-on-specific-fields"></a>Örnek 1: belirli alanlarda tam metin arama

Bu ilk örnek, ayrıştırmaya özgü değildir, ancak ilk temel sorgu kavramını tanıtmaya neden olacak: kapsama. Bu örnek hem sorgu yürütmeyi hem de yanıtı yalnızca birkaç belirli alana kısıtlar. Araç Postman veya arama Gezgini olduğunda okunabilir bir JSON yanıtının nasıl ayarlanacağını bilmek önemlidir. 

Bu sorgu yalnızca ' de *business_title* hedefler **`searchFields`** , **`select`** yanıtta aynı alanı belirtin.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
{
    "count": true,
    "queryType": "simple",
    "search": "*",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Bu sorgu için yanıt aşağıdaki ekran görüntüsüne benzer görünmelidir.

  :::image type="content" source="media/search-query-lucene-examples/postman-sample-results.png" alt-text="Postman örnek yanıtı" border="false":::

Yanıtta arama puanı olduğunu fark etmiş olabilirsiniz. Arama tam metin araması olmadığından veya hiçbir ölçüt sağlanmadığından, tek bir derecelendirme olmadığında **1** Tekdüzen puanları oluşur. Boş bir arama için satırlar rastgele sırayla geri gelir. Gerçek ölçütleri dahil ettiğinizde, arama puanları anlamlı değerlere geliştikçe görüntülenir.

## <a name="example-2-look-up-by-id"></a>Örnek 2: KIMLIĞE göre ara

Arama sonuçlarını bir sorguda döndürdüğünüzde, bir sonraki adım belgeden daha fazla alan içeren bir Ayrıntılar sayfası sağlamaktır. Bu örnek, belge KIMLIĞINDE geçiş yapmak için bir [arama işlemi](/rest/api/searchservice/lookup-document) kullanarak tek bir belgeyi döndürmeyi gösterir.

Tüm belgeler benzersiz bir tanımlayıcıya sahiptir. Bir arama sorgusunun sözdizimini denemek için ilk olarak belge kimliklerinin bir listesini döndürün, böylece kullanmak üzere bir tane bulabilirsiniz. NYC Işleri için tanımlayıcılar `id` alanında depolanır.

```http
GET /indexes/nycjobs/docs?api-version=2020-06-30&search=*&$select=id&$count=true
```

Ardından, `id` önceki yanıtta ilk görüntülenen "9E1e3af9-0660-4E00-AF51-9b654925a2d5" temelinde koleksiyondan bir belge alın. Aşağıdaki sorgu tüm belgenin tüm alınabilir alanlarını döndürür.

```http
GET /indexes/nycjobs/docs/9E1E3AF9-0660-4E00-AF51-9B654925A2D5?api-version=2020-06-30
```

## <a name="example-3-filter-queries"></a>Örnek 3: sorguları filtrele

[Filtre sözdizimi](./search-query-odata-filter.md) , kendisi veya ile kullanabileceğiniz bir OData deyimidir **`search`** . Arama parametresi olmayan tek başına bir filtre, filtre ifadesi ilgilendiğiniz belgeleri tamamen niteleyebiliyorlarsa yararlıdır. Sorgu dizesi olmadan, sözcük temelli veya dil analizi yoktur, Puanlama yoktur (Tüm puanlar 1) ve derecelendirme yoktur. Arama dizesinin boş olduğuna dikkat edin.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "salary_frequency eq 'Annual' and salary_range_from gt 90000",
      "select": "job_id, business_title, agency, salary_range_from"
    }
```

Birlikte kullanıldığında, filtre önce tüm dizine uygulanır ve sonra filtrenin sonuçlarında arama yapılır. Filtreler arama sorgusunun işlemesi gereken belge kümesini azalttığından, sorgu performansını iyileştirmeye yönelik kullanışlı bir teknik olabilir.

  :::image type="content" source="media/search-query-simple-examples/filtered-query.png" alt-text="Filtre sorgusu yanıtı" border="false":::

Filtre ve arama 'yı birleştirmenin bir diğer güçlü yolu **`search.ismatch*()`** , filtre içinde bir arama sorgusu kullanabileceğiniz bir filtre ifadesinde kullanmaktır. Bu filtre ifadesi, plan, Planner, planlama ve benzeri dönem dahil business_title seçmek için *planda* bir joker karakter kullanır.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "search.ismatch('plan*', 'business_title', 'full', 'any')",
      "select": "job_id, business_title, agency, salary_range_from"
    }
```

İşlev hakkında daha fazla bilgi için, ["filtre örnekleri" içinde Search. IsMatch](./search-query-odata-full-text-search-functions.md#examples)bölümüne bakın.

## <a name="example-4-range-filters"></a>Örnek 4: Aralık filtreleri

Aralık filtreleme, **`$filter`** herhangi bir veri türü için ifadeler aracılığıyla desteklenir. Aşağıdaki örneklerde sayısal ve dize alanları üzerinde arama yapılır. 

Veri türleri, Aralık filtrelerinde önemlidir ve sayısal veriler sayısal alanlarda ve dize alanlarında dize verilerinde en iyi şekilde çalışır. Sayısal dizeler Azure Bilişsel Arama karşılaştırılabilir olmadığından, dize alanlarındaki sayısal veriler aralıklar için uygun değildir.

Aşağıdaki sorgu sayısal bir aralıktır:

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "num_of_positions ge 5 and num_of_positions lt 10",
      "select": "job_id, business_title, num_of_positions, agency",
      "orderby": "agency"
    }
```
Bu sorgu için yanıt aşağıdaki ekran görüntüsüne benzer görünmelidir.

  :::image type="content" source="media/search-query-simple-examples/rangefilternumeric.png" alt-text="Sayısal aralıklar için Aralık filtresi" border="false":::

Bu sorguda, Aralık bir dize alanı (business_title) üzerinde.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "business_title ge 'A*' and business_title lt 'C*'",
      "select": "job_id, business_title, agency",
      "orderby": "business_title"
    }
```

Bu sorgu için yanıt aşağıdaki ekran görüntüsüne benzer görünmelidir.

  :::image type="content" source="media/search-query-simple-examples/rangefiltertext.png" alt-text="Metin aralıkları için Aralık filtresi" border="false":::

> [!NOTE]
> Değer aralıkları üzerinde her zaman, ortak bir arama uygulaması gereksinimidir. Daha fazla bilgi ve örnek için bkz. [model filtresi oluşturma](search-filters-facets.md).

## <a name="example-5-geo-search"></a>Örnek 5: coğrafi arama

Örnek dizin, enlem ve Boylam koordinatlarıyla bir geo_location alanı içerir. Bu örnek, bir başlangıç noktasının çevresi içindeki belgelerde, sağladığınız rastgele bir uzaklığa (kilometre cinsinden) göre filtreleyen [coğrafi. Distance işlevini](search-query-odata-geo-spatial-functions.md#examples) kullanır. Sorgunun yüzey alanını azaltmak veya büyütmek için sorgudaki son değeri ayarlayabilirsiniz (4).

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "",
      "filter": "geo.distance(geo_location, geography'POINT(-74.11734 40.634384)') le 4",
      "select": "business_title, work_location"
    }
```

Daha okunabilir sonuçlar için, arama sonuçları iş başlığını ve çalışma konumunu içerecek şekilde kırpılır. Başlangıç koordinatları dizindeki rastgele bir belgeden (Bu durumda, Staon Adası üzerinde bir iş konumu için) alındı.

  :::image type="content" source="media/search-query-simple-examples/geo-search.png" alt-text="Staon Adası Haritası" border="false":::

## <a name="example-6-search-precision"></a>Örnek 6: arama hassasiyeti

Terim sorguları, bağımsız olarak değerlendirilen ve bağımsız olarak değerlendirilen tek koşullardır. Tümcecik sorguları, tırnak işaretleri içine alınır ve tam bir dize olarak değerlendirilir. Eşleşmenin hassasiyeti, işleçler ve searchMode tarafından denetlenir.

Örnek 1: `search=fire`  tüm eşleşmelerin belgede bir yerde harekete geçen kelimeyi içerdiği 140 sonucu ile eşleşir.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "fire"
    }
```

Örnek 2: `search=fire department` 2002 sonucunu döndürür. Eşleşmeler, yangın veya departmanın bulunduğu belgeler için döndürülür.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "fire department"
    }
```

Örnek 3: `search="fire department"` 77 sonucunu döndürür. Dizeyi tırnak içine alarak, her iki terimden oluşan bir tümcecik araması oluşturulur ve eşleşmeler, Birleşik terimlerden oluşan dizinde bulunan simgeleştirilmiş koşullarda bulunur. Bu, şöyle bir aramanın neden `search=+fire +department` eşit olmadığı açıklanmaktadır. Her iki terim de gereklidir, ancak bağımsız olarak taranarak yapılır. 

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
    "count": true,
    "search": "\"fire department\""
    }
```

> [!Note]
> Bir tümcecik sorgusu tırnak işaretleri aracılığıyla belirtildiğinden, bu örnek `\` söz dizimini korumak için bir kaçış karakteri () ekler.

## <a name="example-7-booleans-with-searchmode"></a>Örnek 7: searchMode ile Boolean

Basit sözdizimi, karakter () biçiminde Boole işleçlerini destekler `+, -, |` . SearchMode parametresi duyarlık ve geri çekme arasındaki avantajları bildirir; **`searchMode=any`** favoring geri çağırma (herhangi bir ölçütle eşleşen bir belgeyi sonuç kümesi için nitelendirir) ve **`searchMode=all`** favoring hassasiyeti (tüm kriterlerin eşleşmesi gerekir). 

Varsayılan olarak, **`searchMode=any`** birden çok işleçle bir sorguyu yığınlama ve daha da dar sonuçlar yerine daha geniş bir değer elde ediyorsanız kafa karıştırıcı olabilecek bir değer. Bu durum özellikle doğru DEĞILDIR; burada sonuçlar, belirli bir terimi "içermeyen" tüm belgeleri içerir.

Varsayılan searchMode (any) kullanıldığında, 2800 belge döndürülür: "yangın departmanı" ifadesini ve "MetroTech Center" ifadesi olmayan tüm belgeleri içerir.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"-\"Metrotech Center\"",
      "searchMode": "any"
    }
```

Bu sorgu için yanıt aşağıdaki ekran görüntüsüne benzer görünmelidir.

  :::image type="content" source="media/search-query-simple-examples/searchmodeany.png" alt-text="Arama modu any" border="false":::

**`searchMode=all`** Ölçütlerde bir toplu efekti zorlar ve daha küçük bir sonuç kümesi-21 belge ("Fire Departmanı" ifadesinin tamamını içeren belgelerden ve bu işlerin tümünü MetroTech Center adresinde içeren belgelerden oluşan şekilde değiştirme.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"-\"Metrotech Center\"",
      "searchMode": "all"
    }
```

  :::image type="content" source="media/search-query-simple-examples/searchmodeall.png" alt-text="Arama modu tümü" border="false":::

## <a name="example-8-structuring-results"></a>Örnek 8: sonuçları yapılandırma

Birçok parametre, arama sonuçlarında hangi alanların olduğunu, her toplu işte döndürülen belge sayısını ve sıralama düzenini denetler. Bu örnek, önceki örneklerden birkaç örnekten daha sonra, sonuçları **`$select`** ifade ve tam arama ölçütlerini kullanarak, 82 eşleşme döndüren belirli alanlarla kısıtlar.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description"
    }
```

Önceki örneğe eklenen başlığa göre sıralayabilirsiniz. Civil_service_title, dizinde *sıralanabilir* olduğundan bu sıralama işe yarar.

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description",
      "orderby": "civil_service_title"
    }
```

Sayfalama sonuçları parametre kullanılarak uygulanır **`$top`** , bu örnekte ilk 5 belge döndürülüyor:

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description",
      "orderby": "civil_service_title",
      "top": "5"
    }
```

Sonraki 5 ' i almak için ilk toplu işi atlayın:

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
    {
      "count": true,
      "search": "\"fire department\"",
      "searchMode": "any",
      "select": "job_id,agency,business_title,civil_service_title,work_location,job_description",
      "orderby": "civil_service_title",
      "top": "5",
      "skip": "5"
    }
```

## <a name="next-steps"></a>Sonraki adımlar

Koddaki sorguları belirtmeyi deneyin. Aşağıdaki bağlantılarda, Azure SDK 'Ları kullanılarak arama sorgularının nasıl ayarlanacağı açıklanmaktadır.

+ [.NET SDK kullanarak dizininizi sorgulama](search-get-started-dotnet.md)
+ [Python SDK 'sını kullanarak dizininizi sorgulama](search-get-started-python.md)
+ [JavaScript SDK 'sını kullanarak dizininizi sorgulama](search-get-started-javascript.md)

Ek sözdizimi başvurusu, sorgu mimarisi ve örnekler aşağıdaki bağlantılarda bulunabilir:

+ [Lucene sözdizimi sorgu gelişmiş sorgular oluşturmaya yönelik örnekler](search-query-lucene-examples.md)
+ [Azure Bilişsel Arama’da tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)
+ [Basit sorgu söz dizimi](query-simple-syntax.md)
+ [Tam Lucene sorgu söz dizimi](query-lucene-syntax.md)
+ [Filtre sözdizimi](search-query-odata-filter.md)