---
title: Tam Lucene sorgu söz dizimini kullan
titleSuffix: Azure Cognitive Search
description: Bir Azure Bilişsel Arama hizmetinde, benzer arama, yakınlık araması, terim artırma, normal ifade arama ve joker karakter aramaları için Lucene sorgu söz dizimi.
manager: nitinme
author: HeidiSteen
ms.author: heidist
tags: Lucene query analyzer syntax
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/05/2020
ms.openlocfilehash: 406233fd93ca76a683cf9f9a9e857de9099705ef
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2020
ms.locfileid: "97368554"
---
# <a name="use-the-full-lucene-search-syntax-advanced-queries-in-azure-cognitive-search"></a>"Full" Lucene arama sözdizimini kullanın (Azure Bilişsel Arama Gelişmiş sorgular)

Azure Bilişsel Arama için sorgular oluştururken, özelleştirilmiş ve Gelişmiş sorgu tanımlarını formülleştirmek için varsayılan [basit sorgu ayrıştırıcısını](query-simple-syntax.md) Azure bilişsel arama daha güçlü bir [Lucene sorgu ayrıştırıcısıyla](query-lucene-syntax.md) değiştirebilirsiniz. 

Lucene ayrıştırıcısı alan kapsamlı sorgular, belirsiz arama, indüzeltilme ve sonek joker karakter araması, yakınlık araması, terim artırma ve normal ifade arama gibi karmaşık sorgu yapılarını destekler. Ek güç ek işlem gereksinimleriyle birlikte gelir, böylece biraz daha uzun bir yürütme süresi beklemelisiniz. Bu makalede, tam söz dizimi temelinde sorgu işlemlerini gösteren örneklerde ileredebilirsiniz.

> [!Note]
> Tam Lucene sorgu söz dizimi aracılığıyla etkinleştirilen özelleştirilmiş sorgu kurulumlarını birçoğu [metin çözümlenmez](search-lucene-query-architecture.md#stage-2-lexical-analysis)ve bu da, sözcük olarak ayırmayı veya kasaymayı düşünüyorsanız ortaya çıkmış olabilir. Sözcük temelli analiz yalnızca tüm koşullarda (bir terim sorgusu veya tümcecik sorgusu) gerçekleştirilir. Eksik koşullara sahip sorgu türleri (ön ek sorgusu, joker karakter sorgusu, Regex sorgusu, benzer sorgu) doğrudan sorgu ağacına eklenir, analiz aşaması atlanıyor. Kısmi sorgu koşullarında gerçekleştirilen tek dönüşüm küçük harfe göre yapılır. 
>

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

1. URL 'YI ayarla **`https://azs-playground.search.windows.net/indexes/nycjobs/docs/search=*&api-version=2020-06-30&queryType=full`**

   + Dizindeki belgeler koleksiyonu tüm aranabilir içeriği içerir. İstek üstbilgisinde sağlanmış bir sorgu API anahtarı yalnızca belge koleksiyonunu hedefleyen okuma işlemleri için geçerlidir.

   + **`$count=true`** arama ölçütleriyle eşleşen belgelerin sayısını döndürür. Boş bir arama dizesinde, sayı dizindeki tüm belgeler olacaktır (NYC Işleri durumunda 2558 hakkında).

   + **`search=*`** belirtilmemiş bir sorgu, null veya boş aramaya eşdeğerdir. Özellikle faydalı değildir, ancak yapabileceğiniz en basit aramadır ve tüm değerleri olan dizindeki tüm alınabilir alanları gösterir.

   + **`queryType=full`** tam Lucene Çözümleyicisi 'ni çağırır.

1. Doğrulama adımı olarak, aşağıdaki isteği al öğesine yapıştırın ve **Gönder**' e tıklayın. Sonuçlar ayrıntılı JSON belgeleri olarak döndürülür.

   ```http
   https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30&$count=true&search=*&queryType=full
   ```

### <a name="how-to-invoke-full-lucene-parsing"></a>Tam Lucene ayrıştırma çağırma

**`queryType=full`** Varsayılan basit sorgu söz dizimini geçersiz kılarak tam sorgu söz dizimini çağırmak için ekleyin. Bu makaledeki örneklerin hepsi **`queryType=full`** , tam sözdiziminin Lucene sorgu ayrıştırıcısı tarafından işlendiğini belirten arama parametresini belirtir. 

```http
POST /indexes/nycjobs/docs/search?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "*"
}
```

## <a name="example-1-query-scoped-to-a-list-of-fields"></a>Örnek 1: bir alan listesi kapsamındaki sorgu

Bu ilk örnek, ayrıştırmaya özgü değildir, ancak ilk temel sorgu kavramını tanıtmaya neden olacak: kapsama. Bu örnek hem sorgu yürütmeyi hem de yanıtı yalnızca birkaç belirli alana kısıtlar. Araç Postman veya arama Gezgini olduğunda okunabilir bir JSON yanıtının nasıl ayarlanacağını bilmek önemlidir. 

Bu sorgu yalnızca ' de *business_title* hedefler **`searchFields`** , **`select`** yanıtta aynı alanı belirtin.

```http
POST https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "*",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Bu sorgu için yanıt aşağıdaki ekran görüntüsüne benzer görünmelidir.

  ![Puanlar ile Postman örnek yanıtı](media/search-query-lucene-examples/postman-sample-results.png)

Yanıtta arama puanı olduğunu fark etmiş olabilirsiniz. Arama tam metin araması olmadığından veya hiçbir ölçüt sağlanmadığından, tek bir derecelendirme olmadığında **1** Tekdüzen puanları oluşur. Boş bir arama için satırlar rastgele sırayla geri gelir. Gerçek ölçütleri dahil ettiğinizde, arama puanları anlamlı değerlere geliştikçe görüntülenir.

## <a name="example-2-fielded-search"></a>Örnek 2: Alanlu arama

Full Lucene sözdizimi, tek tek arama ifadelerinin belirli bir alana kapsamını destekler. Bu örnek, iş unvanlarını arar, ancak Junior değildir. VE kullanarak birden çok alan belirtebilirsiniz.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:(senior NOT junior) AND posting_type:external",
    "searchFields": "business_title, posting_type",
    "select": "business_title, posting_type"
}
```

Bu sorgu için yanıt aşağıdaki ekran görüntüsüne benzer görünmelidir (posting_type gösterilmez).

  :::image type="content" source="media/search-query-lucene-examples/intrafieldfilter.png" alt-text="Postman örnek yanıtı arama ifadesi" border="false":::

Arama ifadesi, isteğe bağlı olarak Boolean işleçleriyle tek bir sözcük veya tümcecik ya da parantez içinde daha karmaşık bir ifade olabilir. Bazı örnekler şunlardır:

+ `business_title:(senior NOT junior)`
+ `state:("New York" OR "New Jersey")`
+ `business_title:(senior NOT junior) AND posting_type:external`

Her iki dizenin de tek bir varlık olarak değerlendirilmesini istiyorsanız birden çok dizeyi tırnak içine aldığınızdan emin olun. Bu örnekte, alanda iki ayrı konum arama yapın `state` . Araca bağlı olarak, tırnak işaretleri () arasında kaçış yapmanız gerekebilir `\` . 

**FieldName: searchExpression** 'da belirtilen alan aranabilir bir alan olmalıdır. Dizin özniteliklerinin alan tanımlarında nasıl kullanıldığına ilişkin ayrıntılar için bkz. [Dizin oluşturma (Azure Bilişsel Arama REST API)](/rest/api/searchservice/create-index) .

> [!NOTE]
> Yukarıdaki örnekte, **`searchFields`** sorgunun her bölümü açıkça belirtilmiş bir alan adına sahip olduğundan parametresi atlanır. Ancak, **`searchFields`** sorguda birden çok bölüm varsa (ve deyimlerini kullanarak) yine de kullanabilirsiniz. Örneğin, sorgu `search=business_title:(senior NOT junior) AND external&searchFields=posting_type` `senior NOT junior` yalnızca `business_title` alanla eşleşir, ancak alanla birlikte "External" eşleşecekti `posting_type` . ' De belirtilen alan adı `fieldName:searchExpression` her zaman öncelikli **`searchFields`** olur, bu örnekte bu nedenle, öğesinden geçebiliriz `business_title` **`searchFields`** .

## <a name="example-3-fuzzy-search"></a>Örnek 3: benzer arama

Tam Lucene sözdizimi, benzer bir yapıya sahip koşullarla eşleşen belirsiz aramayı da destekler. Benzer bir arama yapmak için, `~` tek bir sözcüğün sonuna, düzenleme uzaklığını belirten, 0 ile 2 arasında bir değer olan, bir, isteğe bağlı bir parametre ile birlikte tilde sembolünü ekleyin. Örneğin, `blue~` veya `blue~1` mavi, maves ve tutkalla döndürür.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:asosiate~",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Tümcecikler doğrudan desteklenmez, ancak gibi çok parçalı bir tümceciğin her bir teriminde benzer bir eşleşme belirtebilirsiniz `search=business_title:asosiate~ AND comm~` .  Aşağıdaki ekran görüntüsünde, yanıtta *topluluk ilişkilendir*' de bir eşleşme bulunur.

  :::image type="content" source="media/search-query-lucene-examples/fuzzysearch.png" alt-text="Benzer arama yanıtı" border="false":::

> [!Note]
> Benzer sorgular [çözümlenmez](search-lucene-query-architecture.md#stage-2-lexical-analysis). Eksik koşullara sahip sorgu türleri (ön ek sorgusu, joker karakter sorgusu, Regex sorgusu, benzer sorgu) doğrudan sorgu ağacına eklenir, analiz aşaması atlanıyor. Kısmi sorgu koşullarında gerçekleştirilen tek dönüşüm küçük harfe göre yapılır.
>

## <a name="example-4-proximity-search"></a>Örnek 4: yakınlık araması

Yakınlık aramaları, bir belgedeki birbirini yakın terimleri bulmak için kullanılır. Bir ifadenin sonuna, ardından yakınlık sınırını oluşturan sözcüklerin sayısını içeren bir tilde "~" simgesi ekleyin. Örneğin, "otel Havaalanı" ~ 5, bir belgede her birinin 5 sözcüklerinde otel ve Havaalanı terimlerini bulur.

Bu sorgu, her dönemin birden fazla sözcükten ayrıldığı "kıdemler" ve "analist" terimlerini arar ve bu `\"` tümceciği korumak için tırnak işaretleri () kullanılır:

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:\"senior analyst\"~1",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Bu sorgu için yanıt aşağıdaki ekran görüntüsüne benzer görünmelidir 

  :::image type="content" source="media/search-query-lucene-examples/proximity-before.png" alt-text="Yakınlık sorgusu" border="false":::

`~0`"Baş analist" terimleri arasındaki mesafeyi () ortadan kaldırarak yeniden deneyin. Bu sorgu için, önceki sorgu için 10 ' dan farklı olarak 8 belge döndürüldüğünden emin olun.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:\"senior analyst\"~0",
    "searchFields": "business_title",
    "select": "business_title"
}
```

## <a name="example-5-term-boosting"></a>Örnek 5: terim artırma

Terim artırma, bir belgeyi, süresi içermeyen belgelere göre, daha yüksek bir dönem içeriyorsa bir belgenin derecelendirdiğini ifade eder. Bir dönemi artırmak için, `^` arama yaptığınız terimin sonundaki bir artırma faktörü (bir sayı) ile birlikte şapka simgesini kullanın.

Bu "önce" sorgusunda, *bilgisayar analistine* sahip işleri arayın ve hem *bilgisayar* hem de *analist* ile bir sonuç olmadığını, ancak *bilgisayar* işlerinin sonuçların en üstünde olduğunu unutmayın.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:computer analyst",
    "searchFields": "business_title",
    "select": "business_title"
}
```

"Sonra" sorgusunda, bu kez, her iki kelime de yoksa, aramayı tekrarlayarak *bilgisayar* terimi üzerinde *analist* terimini elde edin. Sorgunun okunabilir bir sürümü `search=business_title:computer analyst^2` . Postman 'daki daha takılabilir bir sorgu için `^2` olarak kodlanır `%5E2` .

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:computer analyst%5e2",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Bu sorgu için yanıt aşağıdaki ekran görüntüsüne benzer görünmelidir.

  :::image type="content" source="media/search-query-lucene-examples/termboostingafter.png" alt-text="Sonrasında dönem artırma" border="false":::

Kullanım süresi, Puanlama profillerindeki Puanlama profillerinden farklıdır ve belirli koşullar yerine belirli alanları artırır. Aşağıdaki örnek, farkları göstermeye yardımcı olur.

Musicstoreındex örneğinde **tarz** gibi belirli bir alanda eşleşen bir Puanlama profili düşünün. Belirli arama terimlerini diğerlerinden daha fazla artırmak için kullanım süresi kullanılabilir. Örneğin, "Rock ^ 2 elektronik", **tarzdaki** arama terimlerini, dizinde bulunan diğer aranabilir alanlardan daha yüksek olan belgeyi artırır. Ayrıca, "Rock" arama terimini içeren belgeler, yükseltme değeri (2) sonucunda "elektronik" diğer arama teriminden daha yüksek olarak derecelendirilir.

Faktör düzeyi ayarlanırken, yükseltme faktörü arttıkça, bu terim diğer arama koşullarına göre daha uygun olacaktır. Varsayılan olarak, Boost faktörü 1 ' dir. Boost faktörü pozitif olmalıdır, ancak 1 ' den az olabilir (örneğin, 0,2).

## <a name="example-6-regex"></a>Örnek 6: Regex

Normal ifade araması, [RegExp sınıfında](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html)belgelendiği gibi eğik çizgi "/" arasındaki içeriğe dayalı bir eşleşme bulur.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:/(Sen|Jun)ior/",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Bu sorgu için yanıt aşağıdaki ekran görüntüsüne benzer görünmelidir.

  :::image type="content" source="media/search-query-lucene-examples/regex.png" alt-text="Regex sorgusu" border="false":::

> [!Note]
> Regex sorguları [çözümlenmez](./search-lucene-query-architecture.md#stage-2-lexical-analysis). Kısmi sorgu koşullarında gerçekleştirilen tek dönüşüm küçük harfe göre yapılır.
>

## <a name="example-7-wildcard-search"></a>Örnek 7: joker karakter arama

Birden çok ( \* ) veya tek (?) karakterli joker karakter aramaları için genellikle tanınan sözdizimini kullanabilirsiniz. Lucene sorgu ayrıştırıcısının, bu sembollerin tek bir terim ve tümcecik değil, kullanımını desteklediği unutulmamalıdır.

Bu sorguda, programlama ve programlayıcı koşullarına sahip iş başlıklarını içeren ' prog ' önekini içeren işleri arayın. Bir `*` veya `?` sembolünü bir aramanın ilk karakteri olarak kullanamazsınız.

```http
POST /indexes/nycjobs/docs?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "business_title:prog*",
    "searchFields": "business_title",
    "select": "business_title"
}
```

Bu sorgu için yanıt aşağıdaki ekran görüntüsüne benzer görünmelidir.

  :::image type="content" source="media/search-query-lucene-examples/wildcard.png" alt-text="Joker karakter sorgusu" border="false":::

> [!Note]
> Joker karakter sorguları [çözümlenmez](./search-lucene-query-architecture.md#stage-2-lexical-analysis). Kısmi sorgu koşullarında gerçekleştirilen tek dönüşüm küçük harfe göre yapılır.
>

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