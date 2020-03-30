---
title: Basit bir sorgu oluşturma
titleSuffix: Azure Cognitive Search
description: Tam metin arama, filtre arama, coğrafi arama, bir Azure Bilişsel Arama dizini karşı yönlü arama için basit sözdizimini temel alarak sorguları çalıştırarak örnek alın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 525c20f5b6e24811ab092a63fa84fe6ebbd7b618
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72808198"
---
# <a name="create-a-simple-query-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da basit bir sorgu oluşturma

Azure Bilişsel Arama'da, [basit sorgu sözdizimi,](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) tam metin arama sorgularını bir dizin karşısında yürütmek için varsayılan sorgu arasını çağırır. Bu ayrıştırıcı hızlıdır ve tam metin arama, filtrelenmiş ve yönlü arama ve coğrafi arama gibi yaygın senaryoları işler. 

Bu makalede, basit sözdizimini göstermek için örnekler kullanırız.

Alternatif bir sorgu sözdizimi [Full Lucene,](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)bulanık ve joker karakter arama gibi daha karmaşık sorgu yapıları destekleyen, işlemek için ek zaman alabilir. Tam sözdizimini gösteren daha fazla bilgi ve örnekler [için](search-query-lucene-examples.md)bkz.

## <a name="formulate-requests-in-postman"></a>Postacı'daki istekleri formüle edin

Aşağıdaki örnekler, [City of New York OpenData](https://nycopendata.socrata.com/) girişimi tarafından sağlanan bir veri kümesine dayalı olarak mevcut işlerden oluşan bir NYC İş arama dizininden yararlanır. Bu veriler geçerli veya tam olarak kabul edilmemelidir. Dizin, Microsoft tarafından sağlanan bir sandbox hizmetindedir, bu da bu sorguları denemek için Azure aboneliğine veya Azure Bilişsel Arama'ya ihtiyacınız olmadığı anlamına gelir.

İhtiyacınız olan postacı veya GET'te HTTP isteği vermek için eşdeğer bir araçtır. Daha fazla bilgi için [Bkz. Quickstart: Postacı kullanarak Azure Bilişsel Arama REST API'sini keşfedin.](search-get-started-postman.md)

### <a name="set-the-request-header"></a>İstek üstbilgisini ayarlama

1. İstek üstbilgisinde **İçerik Türünü** ' `application/json`e ayarlayın.

2. Bir **api tuşu**ekleyin ve bu `252044BE3886FE4A8E3BAA4F595114BB`dize ayarlayın: . Bu, NYC Jobs dizinini barındıran sandbox arama hizmeti için bir sorgu anahtarıdır.

İstek üstbilgisini belirttikten sonra, bu makaledeki tüm sorgular için yeniden kullanabilirsiniz ve yalnızca **search=** string'i değiştirebilirsiniz. 

  ![Postman isteği üst bilgisi](media/search-query-lucene-examples/postman-header.png)

### <a name="set-the-request-url"></a>İstek URL'sini ayarlama

İstek, Azure Bilişsel Arama bitiş noktası ve arama dizesini içeren bir URL ile eşleştirilmiş bir GET komutudur.

  ![Postman isteği üst bilgisi](media/search-query-lucene-examples/postman-basic-url-request-elements.png)

URL kompozisyonu aşağıdaki öğelere sahiptir:

+ **`https://azs-playground.search.windows.net/`** Azure Bilişsel Arama geliştirme ekibi tarafından sürdürülen bir kum havuzu arama hizmetidir. 
+ **`indexes/nycjobs/`** bu hizmetin dizinkoleksiyonundaki NYC İşler dizinidir. İstek üzerine hem hizmet adı hem de dizin gereklidir.
+ **`docs`** tüm aranabilir içeriği içeren belge koleksiyonudur. İstek üstbilgisinde sağlanan sorgu api anahtarı yalnızca belge toplamayı hedefleyen okuma işlemlerinde çalışır.
+ **`api-version=2019-05-06`** her istekte gerekli bir parametre olan api sürümünü ayarlar.
+ **`search=*`** ilk sorguda null olan sorgu dizesi, ilk 50 sonuçları döndürerek (varsayılan olarak).

## <a name="send-your-first-query"></a>İlk sorgunuzu gönderme

Doğrulama adımı olarak, aşağıdaki isteği GET'e yapıştırın ve **Gönder'i**tıklatın. Sonuçlar ayrıntılı JSON belgeleri olarak döndürülür. Tüm alanları ve tüm değerleri görmenizi sağlayan tüm belgeler döndürülür.

Doğrulama adımı olarak bu URL'yi bir REST istemcisine yapıştırın ve belge yapısını görüntüleyin.

  ```http
  https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&search=*
  ```

Sorgu dizesi, **`search=*`** null veya boş arama eşdeğer belirtilmemiş bir aramadır. Özellikle yararlı değildir, ancak yapabileceğiniz en basit aramadır.

İsteğe bağlı olarak, arama ölçütleriyle eşleşen belgelerin sayısını döndürmek için URL'ye ekleyebilirsiniz. **`$count=true`** Boş bir arama dizesi üzerinde, bu dizin (NYC İşler durumunda yaklaşık 2800) tüm belgelerdir.

## <a name="how-to-invoke-simple-query-parsing"></a>Basit sorgu ayrıştırma nasıl çağrılabilir

Etkileşimli sorgular için hiçbir şey belirtmeniz gerekmez: basit varsayılandır. Kodda, daha önce **queryType=full'u** tam sorgu sözdizimi için çağırırsanız, varsayılanı **queryType=simple**ile sıfırlayabilirsiniz.

## <a name="example-1-field-scoped-query"></a>Örnek 1: Alan kapsamı sorgusu

Bu ilk örnek ayrıştırıcıya özgü değildir, ancak ilk temel sorgu kavramını tanıtmak için onunla birlikte yol gösteririz: çevreleme. Bu örnek, sorgu yürütme ve sadece birkaç belirli alanlara yanıt kapsamı. Aracınız Postacı veya Arama gezgini olduğunda okunabilir bir JSON yanıtını nasıl yapılandırılabildiğini bilmek önemlidir. 

Kısalık için, sorgu yalnızca *business_title* alanını hedefler ve yalnızca işletme başlıklarının döndürüldişini belirtir. Sözdizimi, **searchFields** sorgu yürütmesini yalnızca business_title alanıyla sınırlamak ve yanıtta hangi alanların dahil edildiğini belirtmek için **alanlar** aranır.

### <a name="partial-query-string"></a>Kısmi sorgu dizesi

```http
searchFields=business_title&$select=business_title&search=*
```

Burada virgülle sınırlandırılmış bir listede birden çok alan içeren aynı sorgu ve bu sorgu vardır.

```http
search=*&searchFields=business_title, posting_type&$select=business_title, posting_type
```

### <a name="full-url"></a>Tam URL

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&searchFields=business_title&$select=business_title&search=*
```

Bu sorguiçin yanıt aşağıdaki ekran görüntüsüne benzer olmalıdır.

  ![Postacı örnek yanıt](media/search-query-lucene-examples/postman-sample-results.png)

Yanıttaki arama puanını fark etmiş olabilirsiniz. Tek tip 1 puanları, sıralama olmadığında, arama tam metin arama sıyrık olmadığından veya ölçüt uygulanmadığından oluşur. Ölçütsüz null arama için satırlar rasgele sırayla geri gelir. Gerçek ölçütleri eklediğinizde, arama puanlarının anlamlı değerlere dönüştüğünü görürsünüz.

## <a name="example-2-look-up-by-id"></a>Örnek 2: Id'ye göre bak

Bu örnek biraz atipiktir, ancak arama davranışlarını değerlendirirken, neden dahil edildiğini veya sonuçlardan dışlandığını anlamak için belirli bir belgenin tüm içeriğini incelemek isteyebilirsiniz. Tek bir belgeyi bütünüyle döndürmek için, belge kimliğini geçirmek için bir [Arama çalışması](https://docs.microsoft.com/rest/api/searchservice/lookup-document) kullanın.

Tüm belgelerin benzersiz bir tanımlayıcısı var. Arama sorgusunun sözdizimini denemek için, kullanmak için bir tane bulabilmeniz için önce belge kimliklerinin listesini döndürün. NYC Jobs için tanımlayıcılar `id` sahada depolanır.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&searchFields=id&$select=id&search=*
```

Sonraki örnek, önceki yanıtta ilk olarak görünen `id` "9E1E3AF9-0660-4E00-AF51-9B654925A2D5" tabanlı belirli bir belgeyi döndüren bir arama sorgusudur. Aşağıdaki sorgu, yalnızca seçili alanların değil, belgenin tamamını döndürür. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs/9E1E3AF9-0660-4E00-AF51-9B654925A2D5?api-version=2019-05-06&$count=true&search=*
```

## <a name="example-3-filter-queries"></a>Örnek 3: Filtre sorguları

[Filtre sözdizimi,](https://docs.microsoft.com/azure/search/search-query-odata-filter) **aramayla** veya kendi başına kullanabileceğiniz bir OData ifadesidir. Arama parametresi olmayan bağımsız bir filtre, filtre ifadesi ilgi çekici belgeleri tam olarak nitelendirebildiği nde yararlıdır. Sorgu dizesi olmadan, sözlü veya dilsel analiz, puanlama (tüm puanlar 1'dir) ve sıralama yoktur. Arama dizesinin boş olduğuna dikkat edin.

```http
POST /indexes/nycjobs/docs/search?api-version=2019-05-06
    {
      "search": "",
      "filter": "salary_frequency eq 'Annual' and salary_range_from gt 90000",
      "select": "job_id, business_title, agency, salary_range_from",
      "count": "true"
    }
```

Birlikte kullanıldığında, filtre önce tüm dizine uygulanır ve ardından arama filtrenin sonuçlarına yapılır. Filtreler arama sorgusunun işlemesi gereken belge kümesini azalttığından, sorgu performansını iyileştirmeye yönelik kullanışlı bir teknik olabilir.

  ![Filtre sorgusu yanıtı](media/search-query-simple-examples/filtered-query.png)

Get'i kullanarak Postacı'da bunu denemek istiyorsanız, bu dizeyi yapıştırabilirsiniz:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,business_title,agency,salary_range_from&search=&$filter=salary_frequency eq 'Annual' and salary_range_from gt 90000
```

Filtre ve aramayı birleştirmenin başka **`search.ismatch*()`** bir güçlü yolu da, filtre içinde bir arama sorgusu kullanabileceğiniz bir filtre ifadesidir. Bu filtre ifadesi, plan, planlayıcı, planlama vb. dahil olmak üzere business_title seçmek için *plan* üzerinde bir joker karakter kullanır.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,business_title,agency&search=&$filter=search.ismatch('plan*', 'business_title', 'full', 'any')
```

İşlev hakkında daha fazla bilgi için [search.ismatch in "Filtre örnekleri"ne](https://docs.microsoft.com/azure/search/search-query-odata-full-text-search-functions#examples)bakın.

## <a name="example-4-range-filters"></a>Örnek 4: Aralık filtreleri

Aralık filtreleme, herhangi **`$filter`** bir veri türü için ifadeler aracılığıyla desteklenir. Aşağıdaki örnekler sayısal ve dize alanları üzerinde arama. 

Veri türleri aralık filtrelerinde önemlidir ve sayısal veriler sayısal alanlarda ve dize verileri dize alanlarında olduğunda en iyi şekilde çalışır. Sayısal dizeleri Azure Bilişsel Arama'da karşılaştırılabilir olmadığından, dize alanlarındaki sayısal veriler aralıklar için uygun değildir. 

Aşağıdaki örnekler okunabilirlik için POST biçimindedir (sayısal aralık, ardından metin aralığı):

```http
POST /indexes/nycjobs/docs/search?api-version=2019-05-06
    {
      "search": "",
      "filter": "num_of_positions ge 5 and num_of_positions lt 10",
      "select": "job_id, business_title, num_of_positions, agency",
      "orderby": "agency",
      "count": "true"
    }
```
  ![Sayısal aralıklar için aralık filtresi](media/search-query-simple-examples/rangefilternumeric.png)


```http
POST /indexes/nycjobs/docs/search?api-version=2019-05-06
    {
      "search": "",
      "filter": "business_title ge 'A*' and business_title lt 'C*'",
      "select": "job_id, business_title, agency",
      "orderby": "business_title",
      "count": "true"
    }
```

  ![Metin aralıkları için aralık filtresi](media/search-query-simple-examples/rangefiltertext.png)

Ayrıca GET kullanarak Postman bu deneyebilirsiniz:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&search=&$filter=num_of_positions ge 5 and num_of_positions lt 10&$select=job_id, business_title, num_of_positions, agency&$orderby=agency&$count=true
```

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&search=&$filter=business_title ge 'A*' and business_title lt 'C*'&$select=job_id, business_title, agency&$orderby=business_title&$count=true
```

> [!NOTE]
> Değer aralıkları üzerinde yüz yüze ortak bir arama uygulaması gereksinimidir. Fason gezinme yapıları için bina filtreleri hakkında daha fazla bilgi ve örnekler için, [ *"Bir*aralık tabanlı filtre"](search-faceted-navigation.md#filter-based-on-a-range)başlıklı bakın.

## <a name="example-5-geo-search"></a>Örnek 5: Coğrafi arama

Örnek dizini enlem ve boylam koordinatları ile geo_location bir alan içerir. Bu örnek, bir başlangıç noktasının çevresi içindeki belgelere filtre leyen [geo.distance işlevini,](https://docs.microsoft.com/azure/search/search-query-odata-geo-spatial-functions#examples) sağladığınız rasgele bir uzaklık (kilometre olarak) olarak kullanır. Sorgunun yüzey alanını azaltmak veya büyütmek için sorgudaki (4) son değeri ayarlayabilirsiniz.

Aşağıdaki örnek okunabilirlik için POST biçimindedir:

```http
POST /indexes/nycjobs/docs/search?api-version=2019-05-06
    {
      "search": "",
      "filter": "geo.distance(geo_location, geography'POINT(-74.11734 40.634384)') le 4",
      "select": "job_id, business_title, work_location",
      "count": "true"
    }
```
Daha okunabilir sonuçlar için, arama sonuçları bir iş kimliği, iş unvanı ve iş konumunu içerecek şekilde kırpılır. Başlangıç koordinatları dizindeki rasgele bir belgeden elde edildi (bu durumda, Staten adasındaki bir çalışma yeri için.

Ayrıca GET kullanarak Postman bu deneyebilirsiniz:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&search=&$select=job_id, business_title, work_location&$filter=geo.distance(geo_location, geography'POINT(-74.11734 40.634384)') le 4
```

## <a name="example-6-search-precision"></a>Örnek 6: Arama hassasiyeti

Terim sorguları, bağımsız olarak değerlendirilen tek terimlerdir, belki de birçoğu. Tümcecik sorguları tırnak işaretlerine eklenir ve harfi harfine dize olarak değerlendirilir. Maçın hassasiyeti operatörler ve searchMode tarafından kontrol edilir.

Örnek 1: **`&search=fire`** tüm eşleşmelerin belgede bir yerde yangın sözcüğünün bulunduğu 150 sonuç döndürür.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&search=fire
```

Örnek 2: **`&search=fire department`** 2002 sonuçlarını döndürür. Maçlar, yangın veya departman içeren belgeler için döndürülür.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&search=fire department
```

Örnek 3: **`&search="fire department"`** 82 sonuç verir. Dizeyi tırnak işaretlerine dahil etmek, her iki terimde de tam olarak yapılan bir aramadır ve eşleşmeler, birleşik terimlerden oluşan dizindeki belirteçli terimlerde bulunur. Bu, neden benzer **`search=+fire +department`** bir aramanın eşdeğer olmadığını açıklar. Her iki terim de gereklidir, ancak bağımsız olarak taranır. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&search="fire department"
```

## <a name="example-7-booleans-with-searchmode"></a>Örnek 7: searchMode ile Booleans

Basit sözdizimi karakter şeklinde boolean operatörleri`+, -, |`destekler ( ). searchMode parametresi, kesinlik ve geri çağırma `searchMode=any` arasındaki dengeleri, geri çağırmayı (herhangi bir ölçütte eşleşen, sonuç kümesi için bir belgeyi nitelendirir) ve `searchMode=all` hassasiyeti (tüm ölçütlerin eşleştirilmesi) lehine bildirir. Varsayılan, `searchMode=any`birden çok işleç ile bir sorgu istifleme ve daha dar sonuçlar yerine daha geniş oluyorsanız kafa karıştırıcı olabilir. Bu, sonuçların belirli bir terimi "içermeyen" tüm belgeleri içerdiği NOT için özellikle geçerlidir.

Varsayılan searchMode (herhangi bir) kullanılarak, 2800 belge döndürülür: çok parçalı "itfaiye" terimini içerenler ve "Metrotech Center" terimine sahip olmayan tüm belgeler.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&searchMode=any&search="fire department"  -"Metrotech Center"
```

  ![arama modu herhangi](media/search-query-simple-examples/searchmodeany.png)

Arama Modu'nu ölçütler üzerinde kümülatif bir etki yitirmek ve `all` daha küçük bir sonuç kümesini döndürecek şekilde değiştirmek - 21 belge - Metrotech Center adresindeki işler hariç, "itfaiye" tüm ifadesini içeren belgelerden oluşan.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&searchMode=all&search="fire department"  -"Metrotech Center"
```
  ![arama modu tüm](media/search-query-simple-examples/searchmodeall.png)

## <a name="example-8-structuring-results"></a>Örnek 8: Yapı sonuçları

Çeşitli parametreler, arama sonuçlarında hangi alanların olduğunu, her toplu iş partisinde döndürülen belge sayısını ve sıralama sırasını denetler. Bu örnek, önceki örneklerden birkaçını yeniden ortaya çıkararak, **sonuçları $select** deyimi ve tam olarak arama ölçütlerini kullanarak belirli alanlarla sınırlandırarak 82 eşleşme döndürmektedir 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"
```
Önceki örneğe eklenen, başlığa göre sıralayabilirsiniz. Bu sıralama, civil_service_title dizinde *sıralanabilir* olduğundan çalışır.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title
```

Sayfalama sonuçları **$top** parametresi kullanılarak uygulanır ve bu durumda en iyi 5 belge döndürülerek:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=0
```

Sonraki 5'i almak için ilk toplu işlemi atlayın:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=5
```

## <a name="next-steps"></a>Sonraki adımlar
Kodunuzda sorguları belirtmeyi deneyin. Aşağıdaki bağlantılar, varsayılan basit sözdizimini kullanarak hem .NET hem de REST API için arama sorgularının nasıl ayarlanış yapılacağını açıklar.

* [.NET SDK'yı kullanarak dizinisorgula](search-query-dotnet.md)
* [REST API'yi kullanarak dizinisorgula](search-create-index-rest-api.md)

Ek sözdizimi başvurusu, sorgu mimarisi ve örnekler aşağıdaki bağlantılarda bulunabilir:

+ [Gelişmiş sorgular oluşturmak için Lucene sözdizimi sorgu örnekleri](search-query-lucene-examples.md)
+ [Azure Bilişsel Arama’da tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)
+ [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Tam Lucene sorgusu](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [Filtreleme ve Sipariş sözdizimine göre](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search)
