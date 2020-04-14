---
title: Tam Lucene sorgu sözdizimini kullanma
titleSuffix: Azure Cognitive Search
description: Bulanık arama, yakınlık araması, terim artırma, düzenli ifade araması ve bir Azure Bilişsel Arama hizmetinde joker karakter aramaları için Lucene sorgu sözdizimi.
manager: nitinme
author: HeidiSteen
ms.author: heidist
tags: Lucene query analyzer syntax
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: bc691299f38d562aee5c08a89e10372331663f8e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81262817"
---
# <a name="use-the-full-lucene-search-syntax-advanced-queries-in-azure-cognitive-search"></a>"Tam" Lucene arama sözdizimini (Azure Bilişsel Arama'da gelişmiş sorgular) kullanın

Azure Bilişsel Arama için sorguoluştururken, özel ve gelişmiş sorgu tanımlarını formüle etmek için varsayılan [basit sorgu arasını](query-simple-syntax.md) Azure Bilişsel Arama'daki daha geniş [Lucene Query Parser](query-lucene-syntax.md) ile değiştirebilirsiniz. 

Lucene arayıcı, alan kapsamı sorguları, bulanık arama, infix ve soneki joker karakter arama, yakınlık araması, terim artırma ve düzenli ifade araması gibi karmaşık sorgu yapılarını destekler. Ek güç, biraz daha uzun yürütme süresi beklemeniz için ek işlem gereksinimleriyle birlikte gelir. Bu makalede, tam sözdizimini kullanırken kullanılabilir sorgu işlemlerini gösteren örnekler arasında adım atabilirsiniz.

> [!Note]
> Tam Lucene sorgu sözdizimi aracılığıyla etkinleştirilen özel sorgu yapılarının çoğu [metin çözümlenmez,](search-lucene-query-architecture.md#stage-2-lexical-analysis)bu da köklendirme veya lemmatizasyon beklerseniz şaşırtıcı olabilir. Sözlü çözümleme yalnızca tam terimler (dönem sorgusu veya tümcecik sorgusu) üzerinde gerçekleştirilir. Eksik terimleri olan sorgu türleri (önek sorgusu, joker karakter sorgusu, regex sorgusu, bulanık sorgu) doğrudan sorgu ağacına eklenir ve çözümleme aşamasını atlar. Kısmi sorgu terimlerinde gerçekleştirilen tek dönüşüm, düşürmedir. 
>

## <a name="formulate-requests-in-postman"></a>Postacı'daki istekleri formüle edin

Aşağıdaki örnekler, [City of New York OpenData](https://opendata.cityofnewyork.us/) girişimi tarafından sağlanan bir veri kümesine dayalı olarak mevcut işlerden oluşan bir NYC İş arama dizininden yararlanır. Bu veriler geçerli veya tam olarak kabul edilmemelidir. Dizin, Microsoft tarafından sağlanan bir sandbox hizmetindedir, bu da bu sorguları denemek için Azure aboneliğine veya Azure Bilişsel Arama'ya ihtiyacınız olmadığı anlamına gelir.

İhtiyacınız olan postacı veya GET'te HTTP isteği vermek için eşdeğer bir araçtır. Daha fazla bilgi için REST [istemcileriyle keşfet'e](search-get-started-postman.md)bakın.

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

Sorgu dizesi, **`search=*`** null veya boş arama eşdeğer belirtilmemiş bir aramadır. Bu yapabileceğiniz en basit arama.

İsteğe bağlı olarak, arama ölçütleriyle eşleşen belgelerin sayısını döndürmek için URL'ye ekleyebilirsiniz. **`$count=true`** Boş bir arama dizesi üzerinde, bu dizin (NYC İşler durumunda yaklaşık 2800) tüm belgelerdir.

## <a name="how-to-invoke-full-lucene-parsing"></a>Nasıl tam Lucene ayrıştırma çağırmak için

Varsayılan basit sorgu sözdizimini geçersiz kılarak tam sorgu sözdizimini çağırmak için **queryType=full** ekleyin. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&queryType=full&search=*
```

Bu makaledeki tüm örnekler, tam sözdizimilucene Query Parser tarafından işlenir belirten **queryType=tam** arama parametresi belirtin. 

## <a name="example-1-query-scoped-to-a-list-of-fields"></a>Örnek 1: Alanlar listesine giren sorgu

Bu ilk örnek Lucene'ye özgü değildir, ancak ilk temel sorgu kavramını tanıtmak için bu ayrıntıya gireriz: alan kapsamı. Bu örnek, tüm sorguyu ve yanıtı sadece birkaç özel alana kapsamıdır. Aracınız Postacı veya Arama gezgini olduğunda okunabilir bir JSON yanıtını nasıl yapılandırılabildiğini bilmek önemlidir. 

Kısalık için, sorgu yalnızca *business_title* alanını hedefler ve yalnızca işletme başlıklarının döndürüldişini belirtir. **SearchFields** parametresi sorgu yürütmesini yalnızca business_title alanıyla sınırlandırıyor ve **yanıtta** hangi alanların dahil edildiğini belirtir.

### <a name="search-expression"></a>Arama ifadesi

```http
&search=*&searchFields=business_title&$select=business_title
```

Burada virgülle sınırlandırılmış bir listede birden çok alan içeren aynı sorgu ve bu sorgu vardır.

```http
search=*&searchFields=business_title, posting_type&$select=business_title, posting_type
```

Virgülden sonraki boşluklar isteğe bağlıdır.

> [!Tip]
> Uygulama kodunuzdaki REST API'sini kullanırken, URL kodlama parametrelerini `$select` ve . `searchFields`

### <a name="full-url"></a>Tam URL

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&queryType=full&$count=true&search=*&searchFields=business_title&$select=business_title
```

Bu sorguiçin yanıt aşağıdaki ekran görüntüsüne benzer olmalıdır.

  ![Postacı örnek yanıt](media/search-query-lucene-examples/postman-sample-results.png)

Yanıttaki arama puanını fark etmiş olabilirsiniz. Tek tip 1 puanları, sıralama olmadığında, arama tam metin arama sıyrık olmadığından veya ölçüt uygulanmadığından oluşur. Ölçütsüz null arama için satırlar rasgele sırayla geri gelir. Gerçek arama ölçütlerini eklediğinizde, arama puanlarının anlamlı değerlere dönüştüğünü görürsünüz.

## <a name="example-2-fielded-search"></a>Örnek 2: Alanlı arama

Tam Lucene sözdizimi, tek tek arama ifadelerinin belirli bir alana dahil olmasını destekler. Bu örnek, üst düzey terimi olan iş unvanlarını arar, ancak küçük olmayan terimi arar.

### <a name="search-expression"></a>Arama ifadesi

```http
$select=business_title&search=business_title:(senior NOT junior)
```

Burada birden çok alan ile aynı sorgu.

```http
$select=business_title, posting_type&search=business_title:(senior NOT junior) AND posting_type:external
```

### <a name="full-url"></a>Tam URL

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&queryType=full&$count=true&$select=business_title&search=business_title:(senior NOT junior)
```

  ![Postacı örnek yanıt](media/search-query-lucene-examples/intrafieldfilter.png)

**Alanadı:searchExpression** sözdizimi ile, arama ifadesinin tek bir sözcük veya tümcecik olabileceği veya parantez içinde isteğe bağlı olarak Boolean işleçleriyle daha karmaşık bir ifade olabileceği alanlı bir arama işlemi tanımlayabilirsiniz. Bazı örnekler şunlardır:

- `business_title:(senior NOT junior)`
- `state:("New York" OR "New Jersey")`
- `business_title:(senior NOT junior) AND posting_type:external`

Her iki dizenin de tek bir varlık olarak değerlendirilmesini istiyorsanız, bu durumda `state` alandaki iki farklı konumu ararken, tırnak işaretlerine birden çok dize koyduğunuzdan emin olun. Ayrıca, işlecinot ve AND ile gördüğünüz gibi büyük harfle olduğundan emin olun.

**AlanAdı:searchExpression'da** belirtilen alan aranabilir bir alan olmalıdır. Alan tanımlarında dizin özniteliklerinin nasıl kullanıldığına ilişkin ayrıntılar için [Create Index (Azure Bilişsel Arama REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) bakın.

> [!NOTE]
> Yukarıdaki örnekte, sorgunun `searchFields` her parçası açıkça belirtilen bir alan adı olduğundan parametreyi kullanmamız gerekmezdi. Ancak, bazı parçaların `searchFields` belirli bir alana kapsamının bulunduğu ve geri kalanının çeşitli alanlara uygulanabileceği bir sorgu çalıştırmak istiyorsanız parametreyi kullanmaya devam edebilirsiniz. Örneğin, sorgu `search=business_title:(senior NOT junior) AND external&searchFields=posting_type` yalnızca `senior NOT junior` `business_title` alanla eşleşirken, alanla "dış" `posting_type` eşleşmesi de geçerlidir. **FieldName:searchExpression'da** sağlanan alan adı her `searchFields` zaman parametreden önceliklidir, bu nedenle bu `business_title` örnekte `searchFields` parametreye eklememiz gerekmez.

## <a name="example-3-fuzzy-search"></a>Örnek 3: Bulanık arama

Tam Lucene sözdizimi de bulanık arama destekler, benzer bir yapıya sahip terimlereşleşen. Bulanık bir arama yapmak için, tek `~` bir sözcüğün sonundaki tilde simgesini, 0 ile 2 arasında bir değer olan ve düzenlenen mesafeyi belirten isteğe bağlı bir parametreyle tamamla. Örneğin, `blue~` ya `blue~1` da mavi, blues ve tutkal döndürecek.

### <a name="search-expression"></a>Arama ifadesi

```http
searchFields=business_title&$select=business_title&search=business_title:asosiate~
```

Tümcecikler doğrudan desteklenmez, ancak bir tümceciliğin bileşen bölümlerinde bulanık bir eşleşme belirtebilirsiniz.

```http
searchFields=business_title&$select=business_title&search=business_title:asosiate~ AND comm~ 
```


### <a name="full-url"></a>Tam URL

Bu sorguda "associate" (kasıtlı olarak yanlış yazılmış) terimi olan işler arar:

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:asosiate~
```
  ![Bulanık arama yanıtı](media/search-query-lucene-examples/fuzzysearch.png)


> [!Note]
> Bulanık sorgular [çözümlenmez.](search-lucene-query-architecture.md#stage-2-lexical-analysis) Eksik terimleri olan sorgu türleri (önek sorgusu, joker karakter sorgusu, regex sorgusu, bulanık sorgu) doğrudan sorgu ağacına eklenir ve çözümleme aşamasını atlar. Tamamlanmamış sorgu terimlerinde gerçekleştirilen tek dönüşüm, düşürmedir.
>

## <a name="example-4-proximity-search"></a>Örnek 4: Yakınlık araması
Yakınlık aramaları, belgede birbirine yakın terimleri bulmak için kullanılır. Yakınlık sınırını oluşturan sözcük sayısını izleyen bir tümceciğin sonuna bir tilde "~" simgesi ekleyin. Örneğin, "otel havaalanı"~5 bir belgede birbirinden 5 kelime içinde otel ve havaalanı terimlerini bulur.

### <a name="search-expression"></a>Arama ifadesi

```http
searchFields=business_title&$select=business_title&search=business_title:%22senior%20analyst%22~1
```

### <a name="full-url"></a>Tam URL

Bu sorguda, "kıdemli analist" terimine sahip işler için birden fazla sözcük ile ayrılır:

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:%22senior%20analyst%22~1
```
  ![Yakınlık sorgusu](media/search-query-lucene-examples/proximity-before.png)

"Kıdemli analist" terimi arasındaki kelimeleri kaldırarak tekrar deneyin. Önceki sorgu için 10 yerine bu sorgu için 8 belge döndürülür dikkat edin.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:%22senior%20analyst%22~0
```

## <a name="example-5-term-boosting"></a>Örnek 5: Terim artırma
Terim artırma, bir belgeyi, terimi içermeyen belgelere göre artırılmış terimi içeriyorsa daha yüksek sıralamaanlamına gelir. Bir terimi artırmak için, aradığınız dönemin sonunda bir artırma faktörü (bir sayı) içeren "^" simgesi olan "^" simgesini kullanın. 

### <a name="full-urls"></a>Tam URL'ler

Bu "önce" sorguda, dönem *bilgisayar analisti* ile iş aramak ve hem kelime *bilgisayar* ve *analist*ile hiçbir sonuç olduğunu fark , henüz *bilgisayar* işleri sonuçların üstünde dir.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:computer%20analyst
```
  ![Önce dönem artırma](media/search-query-lucene-examples/termboostingbefore.png)

"Sonra" sorgusunda, aramayı tekrarlayın, her iki sözcük de yoksa, bu kez terim *analisti* ile terimi *analisti* ile sonuçları artırın. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:computer%20analyst%5e2
```
Yukarıdaki sorgunun daha insan tarafından `search=business_title:computer analyst^2`okunabilir bir sürümüdür. Uygulanabilir bir sorgu `^2` için, görülmesi `%5E2`daha zor olan olarak kodlanır.

  ![Sonra dönem artırma](media/search-query-lucene-examples/termboostingafter.png)

Terim artırma, puanlama profillerindeki puanlama profillerinden belirli terimler yerine belirli alanları güçlendirmesinden farklıdır. Aşağıdaki örnek, farklılıkların ortaya önüne gibi yardımcı olur.

Musicstoreindex örneğindeki **tür** gibi belirli bir alandaki eşleşmeleri artıran bir puanlama profili düşünün. Terim artırma, belirli arama terimlerini diğerlerinden daha yüksek bir şekilde artırmak için kullanılabilir. Örneğin, "rock^2 electronic", **tür** alanındaki arama terimlerini içeren belgeleri dizindeki diğer aranabilir alanlardan daha yüksek artırır. Ayrıca, "kaya" arama terimini içeren belgeler, artırma değeri terimi sonucunda diğer arama terimi "elektronik"ten daha yüksek sırada yer alacaktır (2).

Faktör düzeyini ayarlarken, artırma faktörü ne kadar yüksekse, terim diğer arama terimlerine göre o kadar alakalı olur. Varsayılan olarak, artırma faktörü 1'dir. Artırma faktörü pozitif olsa da, 1'den küçük olabilir (örneğin, 0,2).


## <a name="example-6-regex"></a>Örnek 6: Regex

Normal bir ifade [araması, RegExp sınıfında](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html)belgelenen "/" ileri eğik çizgileri arasındaki içeriği temel alan bir eşleşme bulur.

### <a name="search-expression"></a>Arama ifadesi

```http
searchFields=business_title&$select=business_title&search=business_title:/(Sen|Jun)ior/
```

### <a name="full-url"></a>Tam URL

Bu sorguda, Kıdemli veya Junior terimine `search=business_title:/(Sen|Jun)ior/`sahip işleri arayın: .

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:/(Sen|Jun)ior/
```

  ![Regex sorgusu](media/search-query-lucene-examples/regex.png)

> [!Note]
> Regex sorguları [çözümlenmez.](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis) Tamamlanmamış sorgu terimlerinde gerçekleştirilen tek dönüşüm, düşürmedir.
>

## <a name="example-7-wildcard-search"></a>Örnek 7: Joker karakter arama
Birden çok ( ) veya tek\*(?) karakter joker karakter aramaları için genel olarak tanınan sözdizimini kullanabilirsiniz. Lucene sorgu arayıcının bu sembollerin bir tümcecikle değil, tek bir terimle kullanımını desteklediğini unutmayın.

### <a name="search-expression"></a>Arama ifadesi

```http
searchFields=business_title&$select=business_title&search=business_title:prog*
```

### <a name="full-url"></a>Tam URL

Bu sorguda, programlama ve programcı terimlerini içeren iş başlıklarını içeren 'prog' önekini içeren işleri arayın. Bir * veya kullanamazsınız? bir aramanın ilk karakteri olarak sembol.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2019-05-06&queryType=full&$count=true&searchFields=business_title&$select=business_title&search=business_title:prog*
```
  ![Joker karakter sorgusu](media/search-query-lucene-examples/wildcard.png)

> [!Note]
> Joker karakter sorguları [çözümlenmez.](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis) Tamamlanmamış sorgu terimlerinde gerçekleştirilen tek dönüşüm, düşürmedir.
>

## <a name="next-steps"></a>Sonraki adımlar
Kodunuzda Lucene Query Parser'ı belirtmeyi deneyin. Aşağıdaki bağlantılar, hem .NET hem de REST API için arama sorgularının nasıl ayarlanış edilebildiğini açıklar. Bağlantılar varsayılan basit sözdizimini kullanır, bu nedenle **queryType'ı**belirtmek için bu makaleden öğrendiklerinizi uygulamanız gerekir.

* [.NET SDK'yı kullanarak dizinisorgula](search-query-dotnet.md)
* [REST API'yi kullanarak dizinisorgula](search-create-index-rest-api.md)

Ek sözdizimi başvurusu, sorgu mimarisi ve örnekler aşağıdaki bağlantılarda bulunabilir:

+ [Basit sözdizimi sorgu örnekleri](search-query-simple-examples.md)
+ [Azure Bilişsel Arama’da tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)
+ [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Tam Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)