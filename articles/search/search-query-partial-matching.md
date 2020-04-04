---
title: Kısmi terimler, desenler ve özel karakterler
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgu isteğinde tam veya kısmi terimlerle eşleşecek şekilde joker karakter, regex ve önek sorguları kullanın. Özel karakterler içeren eşleşmesi zor desenler tam sorgu sözdizimi ve özel çözümleyiciler kullanılarak çözülebilir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/02/2020
ms.openlocfilehash: 3e0e0291ff855b4502224466e17696a4fe668c2a
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655996"
---
# <a name="partial-term-search-in-azure-cognitive-search-queries-wildcard-regex-fuzzy-search-patterns"></a>Azure Bilişsel Arama sorgularında kısmi süreli arama (joker karakter, regex, bulanık arama, desenler)

*Kısmi terim araması,* bir dizenin ilk, son veya iç kısımları gibi terim parçalarından oluşan sorguları veya genellikle tire veya kesikler gibi özel karakterlerle ayrılmış parçaların birleşiminden oluşan bir desen anlamına gelir. Yaygın kullanım örnekleri, telefon numarasının, URL'nin, kişilerin veya ürün kodlarının veya bileşik sözcüklerin bölümlerinin sorgulanmasıdır.

Dizin kendisi genellikle kısmi dize ve desen eşleştirme için elverişli bir şekilde terimleri depolamak değil, çünkü kısmi arama sorunlu olabilir. Dizin oluşturmanın metin çözümleme aşamasında, özel karakterler atılır, bileşik ve bileşik dizeleri bölünür ve eşleme bulununca desen sorgularının başarısız olması gerekir. Örneğin, bu içerik `+1 (425) 703-6214`dizinde gerçekte `"1"`bulunmadığından, `"6214"`(belirteç , `"3-62"` , `"425"` `"703"`) gibi bir telefon numarası sorguda gösterilmez. 

Çözüm, kısmi arama senaryolarını destekleyebilmeniz için bu dizelerin bozulmamış sürümlerini dizinde depolamaktır. Bozulmamış bir dize için ek bir alan oluşturma nın yanı sıra içerik koruyucu bir çözümleyici kullanmak çözümün temelidir.

## <a name="what-is-partial-search-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da kısmi arama nedir

Azure Bilişsel Arama'da kısmi arama şu formlarda kullanılabilir:

+ [Önek arama](query-simple-syntax.md#prefix-search), `search=cap*`örneğin , "Cap'n Jack's Waterfront Inn" veya "Gacc Capital" ile eşleşen. Önek araması için basit sorgu sözdizimini kullanabilirsiniz.
+ [Joker karakter araması](query-lucene-syntax.md#bkmk_wildcard) veya soneki de dahil olmak üzere katışılmış bir dize veya parçaları arayan [normal ifadeler.](query-lucene-syntax.md#bkmk_regex) Örneğin, "alfanümerik" terimi göz önüne alındığında, bu`search=/.*numeric.*/`terimdeki sonek sorgusu eşleşmesi için joker karakter araması ( ) kullanırsınız. Joker karakter ve normal ifadeler tam Lucene sözdizimini gerektirir.

İstemci uygulamanızda yukarıdaki sorgu türlerinden herhangi biri gerektiğinde, dizininizde gerekli içeriğin bulunduğundan emin olmak için bu makaledeki adımları izleyin.

## <a name="solving-partial-search-problems"></a>Kısmi arama sorunlarını çözme

Desenlerde veya özel karakterlerde arama yapmanız gerektiğinde, varsayılan çözümleyiciyi daha basit belirteç verme kuralları altında çalışan ve tüm dizeyi koruyan özel bir çözümleyiciyle geçersiz kılabilir. Bir adım geri alarak, yaklaşım şu na benzer:

+ Dize bozulmamış bir sürümünü depolamak için bir alan tanımlayın (çözümlenmiş ve çözümlenmemiş metin istediğinizi varsayarak)
+ Önceden tanımlanmış bir çözümleyici seçin veya sağlam bir dize çıkarmak için özel bir çözümleyici tanımlayın
+ Çözümleyiciyi alana atama
+ Dizin oluşturma ve test edin

> [!TIP]
> Çözümleyicileri değerlendirmek, sık dizin yeniden oluşturmayı gerektiren yinelemeli bir işlemdir. Postacı, [Dizin Oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index), [Dizin Sil](https://docs.microsoft.com/rest/api/searchservice/delete-index),[Belgeleri Yükle](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)ve Arama [Belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents)için REST API'lerini kullanarak bu adımı kolaylaştırabilirsiniz. Yükleme Belgeleri için istek gövdesi, test etmek istediğiniz küçük bir temsili veri kümesi (örneğin, telefon numaraları veya ürün kodları içeren bir alan) içermelidir. Aynı Postacı koleksiyonundaki bu API'lerle, bu adımları hızlı bir şekilde gözden geçirebilirsiniz.

## <a name="duplicate-fields-for-different-scenarios"></a>Farklı senaryolar için yinelenen alanlar

Çözümleyiciler, alan başına olarak atanır, bu da farklı senaryolar için en iyi duruma getirmek için dizininizde alanlar oluşturabileceğiniz anlamına gelir. Özellikle, birinci ve gelişmiş desen eşleşen ilk düzenli tam metin arama desteklemek için "featureCode" ve "featureCodeRegex" tanımlayabilirsiniz.

```json
{
  "name": "featureCode",
  "type": "Edm.String",
  "retrievable": true,
  "searchable": true,
  "analyzer": null
},
{
  "name": "featureCodeRegex",
  "type": "Edm.String",
  "retrievable": true,
  "searchable": true,
  "analyzer": "my_customanalyzer"
},
```

## <a name="choose-an-analyzer"></a>Bir çözümleyici seçin

Tam süreli belirteçler üreten bir çözümleyici seçerken, aşağıdaki çözümleyiciler sık kullanılan seçeneklerdir:

| Analyzer | Davranışlar |
|----------|-----------|
| [Anahtar kelime](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) | Tüm alanın içeriği tek bir terim olarak belirtilir. |
| [Boşluk](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html) | Yalnızca beyaz alanlarda ayırır. Tire veya diğer karakterleri içeren terimler tek bir belirteç olarak kabul edilir. |
| [özel analizör](index-add-custom-analyzers.md) | (önerilir) Özel bir çözümleyici oluşturmak, hem belirteç ve belirteç filtresini belirtmenize olanak tanır. Önceki çözümleyiciler olduğu gibi kullanılmalıdır. Özel bir çözümleyici, hangi belirteçleri ve belirteç filtreleri kullanmak için seçmenizi sağlar. <br><br>Önerilen kombinasyon, [küçük harf belirteç filtresine](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html)sahip anahtar kelime [tokenizeridir.](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html) Kendi başına, önceden tanımlanmış [anahtar kelime çözümleyici](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) sorguları başarısızlığa neden olabilir herhangi bir büyük harfli metin, küçük harf değildir. Özel bir çözümleyici, küçük harf belirteç filtresi eklemek için bir mekanizma sağlar. |

Postacı gibi bir web API test aracı kullanıyorsanız, belirteç çıktısını incelemek için [Test Analyzer REST çağrısını](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) ekleyebilirsiniz.

Çalışmak için varolan bir dizinin olması gerekir. Varolan bir dizin ve tire veya kısmi terimler içeren bir alan göz önüne alındığında, hangi belirteçlerin yayDığını görmek için belirli terimler üzerinde çeşitli çözümleyicileri deneyebilirsiniz.  

1. Terimlerin varsayılan olarak nasıl belirteçleştirilebildiğini görmek için Standart çözümleyicisini denetleyin.

   ```json
   {
   "text": "SVP10-NOR-00",
   "analyzer": "standard"
   }
    ```

1. Metnin dizin içinde nasıl belirteçleştirilebildiğini görmek için yanıtı değerlendirin. Her terimin nasıl küçük harfle ve nasıl ayrıldığına dikkat edin.

    ```json
    {
        "tokens": [
            {
                "token": "svp10",
                "startOffset": 0,
                "endOffset": 5,
                "position": 0
            },
            {
                "token": "nor",
                "startOffset": 6,
                "endOffset": 9,
                "position": 1
            },
            {
                "token": "00",
                "startOffset": 10,
                "endOffset": 12,
                "position": 2
            }
        ]
    }
    ```
1. İsteği veya `whitespace` `keyword` çözümleyiciyi kullanmak için değiştirin:

    ```json
    {
    "text": "SVP10-NOR-00",
    "analyzer": "keyword"
    }
    ```

1. Şimdi yanıt tek bir belirteç oluşur, büyük kasalı, tire bir parçası olarak korunmuş tire ile. Bir desen veya kısmi bir terim üzerinde arama yapmanız gerekiyorsa, sorgu altyapısı artık eşleşme bulmak için temele sahiptir.


    ```json
    {

        "tokens": [
            {
                "token": "SVP10-NOR-00",
                "startOffset": 0,
                "endOffset": 12,
                "position": 0
            }
        ]
    }
    ```
> [!Important]
> Sorgu ağacını kurarken sorgu ayrıştırıcılarının genellikle bir arama ifadesinde küçük harf terimlerini ayrıştırdığını unutmayın. Küçük harfli metin girişleri olmayan bir çözümleyici kullanıyorsanız ve beklenen sonuçları almıyorsanız, bu nedenle olabilir. Çözüm, aşağıdaki "Özel çözümleyiciler kullan" bölümünde açıklandığı gibi küçük harfbelirteç filtresi eklemektir.

## <a name="configure-an-analyzer"></a>Çözümleyiciyi yapılandırma
 
Çözümleyicileri değerlendiriyor veya belirli bir yapılandırmaile ilerliyor olun, alan tanımında çözümleyiciyi belirtmeniz ve yerleşik bir çözümleyici kullanmıyorsanız çözümleyicinin kendisini yapılandırmanız gerekir. Çözümleyicileri değiştirirken, genellikle dizini yeniden oluşturmanız gerekir (bırak, yeniden oluşturma ve yeniden yükleme). 

### <a name="use-built-in-analyzers"></a>Yerleşik çözümleyicileri kullanma

Yerleşik veya önceden tanımlanmış çözümleyiciler, dizinde `analyzer` ek yapılandırma gerektirmeden alan tanımının özelliğinde adlarıyla belirtilebilir. Aşağıdaki örnek, çözümleyiciyi `whitespace` bir alana nasıl ayarlayacağınızı gösterir. Kullanılabilir yerleşik çözümleyiciler hakkında daha fazla bilgi [için, Önceden Tanımlanmış çözümleyiciler listesine](https://docs.microsoft.com/azure/search/index-add-custom-analyzers#predefined-analyzers-reference)bakın. 

```json
    {
      "name": "phoneNumber",
      "type": "Edm.String",
      "key": false,
      "retrievable": true,
      "searchable": true,
      "analyzer": "whitespace"
    }
```

### <a name="use-custom-analyzers"></a>Özel çözümleyiciler kullanma

Özel bir [çözümleyici](index-add-custom-analyzers.md)kullanıyorsanız, dizin içinde kullanıcı tanımlı bir belirteç, belirteç filtresi ve olası yapılandırma ayarları yla tanımlayın. Daha sonra, yerleşik bir çözümleyici gibi, bir alan tanımı üzerinde başvuru.

Amaç tam terim belirteçleştirme olduğunda, **bir anahtar kelime belirteci** ve **küçük harf belirteç filtresinden** oluşan özel bir çözümleyici önerilir.

+ Anahtar kelime belirteci, bir alanın tüm içeriği için tek bir belirteç oluşturur.
+ Küçük harf belirteç filtresi büyük harfleri küçük harfli metne dönüştürür. Sorgu ayrıştırıcıları genellikle herhangi bir büyük metin girişlerini küçük düşürür. Düşük kasa, girdileri belirteçli terimlerle homojenize eder.

Aşağıdaki örnekte, anahtar kelime tokenizeri ve küçük harf belirteç filtresi sağlayan özel bir çözümleyici gösteriş gösterin.

```json
{
"fields": [
  {
  "name": "accountNumber",
  "analyzer":"myCustomAnalyzer",
  "type": "Edm.String",
  "searchable": true,
  "filterable": true,
  "retrievable": true,
  "sortable": false,
  "facetable": false
  }
],

"analyzers": [
  {
  "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
  "name":"myCustomAnalyzer",
  "charFilters":[],
  "tokenizer":"keyword_v2",
  "tokenFilters":["lowercase"]
  }
],
"tokenizers":[],
"charFilters": [],
"tokenFilters": []
```

> [!NOTE]
> Belirteç `keyword_v2` ve `lowercase` belirteç filtresi sistem tarafından bilinen ve varsayılan yapılandırmaları kullanarak, bu nedenle ilk olarak tanımlamak zorunda kalmadan adıyla başvuruyapabilirsiniz.

## <a name="build-and-test"></a>Derleme ve test etme

Senaryonuzu destekleyen çözümleyiciler ve alan tanımları içeren bir dizin tanımladıktan sonra, kısmi dize sorgularını sınayabilmeniz için temsili dizeleri olan belgeleri yükleyin. 

Önceki bölümlerde mantık açıklanmıştır. Bu bölüm, çözümünüzü test ederken aramanız gereken her API'den geçer. Daha önce de belirtildiği gibi, Postman gibi etkileşimli bir web test aracı kullanıyorsanız, bu görevleri hızlı bir şekilde atlayabilirsiniz.

+ [Sil Dizini,](https://docs.microsoft.com/rest/api/searchservice/delete-index) aynı adı niçin yeniden oluşturabileceğinizi varolan bir dizini kaldırır.

+ [Create Index,](https://docs.microsoft.com/rest/api/searchservice/create-index) çözümleyici tanımları ve çözümleyici belirtimi olan alanlar da dahil olmak üzere arama hizmetinizdeki dizin yapısını oluşturur.

+ [Yük Belgeleri,](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) dizininizdeki yapıyla aynı yapıya sahip belgeleri ve aranabilir içeriği içeri yükler. Bu adımdan sonra dizinsorgu veya test etmeye hazırdır.

+ [Test Analyzer](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) bir [çözümleyici seçin](#choose-an-analyzer)tanıtıldı. Terimlerin nasıl belirteçleştirilebildiğini anlamak için çeşitli çözümleyiciler kullanarak dizininizdeki dizelerin bazılarını test edin.

+ [Arama Belgeleri,](https://docs.microsoft.com/rest/api/searchservice/search-documents) joker karakter ve normal ifadeler için [basit sözdizimi](query-simple-syntax.md) veya [tam Lucene sözdizimini](query-lucene-syntax.md) kullanarak bir sorgu isteğinin nasıl oluşturulabildiğini açıklar.

## <a name="tips-and-best-practices"></a>İpuçları ve en iyi yöntemler

### <a name="tune-query-performance"></a>Sorgu performansını ayarlama

keyword_v2 belirteç ve küçük harf belirteç filtresi içeren önerilen yapılandırmayı uygularsanız, dizininizdeki varolan belirteçler üzerinde ek belirteç filtresi işleme nedeniyle sorgu performansında bir düşüş fark edebilirsiniz. 

Aşağıdaki örnek, önek eşleşmeleri daha hızlı yapmak için bir [EdgeNGramTokenFiltresi](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenizer.html) ekler. Karakterleri içeren 2-25 karakter birleşimiiçin ek belirteçler oluşturulur: (sadece MS, MSF, MSFT, MSFT/, MSFT/S, MSFT/SQ, MSFT/SQL değil). Tahmin edebileceğiniz gibi, ek belirteç oluşturma daha büyük bir dizin sonuçlanır.

```json
{
"fields": [
  {
  "name": "accountNumber",
  "analyzer":"myCustomAnalyzer",
  "type": "Edm.String",
  "searchable": true,
  "filterable": true,
  "retrievable": true,
  "sortable": false,
  "facetable": false
  }
],

"analyzers": [
  {
  "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
  "name":"myCustomAnalyzer",
  "charFilters":[],
  "tokenizer":"keyword_v2",
  "tokenFilters":["lowercase", "my_edgeNGram"]
  }
],
"tokenizers":[],
"charFilters": [],
"tokenFilters": [
  {
  "@odata.type":"#Microsoft.Azure.Search.EdgeNGramTokenFilterV2",
  "name":"my_edgeNGram",
  "minGram": 2,
  "maxGram": 25,
  "side": "front"
  }
]
```

### <a name="use-different-analyzers-for-indexing-and-query-processing"></a>Dizin oluşturma ve sorgu işleme için farklı çözümleyiciler kullanın

Çözümleyiciler dizin oluşturma ve sorgu yürütme sırasında çağrılır. Her ikisi için de aynı çözümleyicikullanmak yaygındır, ancak her iş yükü için özel çözümleyicileri yapılandırabilirsiniz. Çözümleyici geçersiz kılmaları bir `analyzers` [bölümdeki dizin tanımında](https://docs.microsoft.com/rest/api/searchservice/create-index) belirtilir ve daha sonra belirli alanlarda başvurulur. 

Yalnızca dizin oluşturma sırasında özel çözümleme gerektiğinde, özel çözümleyiciyi yalnızca dizine uygulayabilir ve sorgular için standart Lucene çözümleyicisini (veya başka bir çözümleyiciyi) kullanmaya devam edebilirsiniz.

Role özgü çözümleme belirtmek için, her biri için alandaki `searchAnalyzer` özellikleri, `analyzer` varsayılan özellik yerine ayarlayabilirsiniz. `indexAnalyzer`

```json
"name": "featureCode",
"indexAnalyzer":"my_customanalyzer",
"searchAnalyzer":"standard",
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, çözümleyicilerin hem sorgu sorunlarına katkıda bulunmaları hem de sorgu sorunlarını nasıl çözdükleri açıklanmaktadır. Bir sonraki adım olarak, çözümleyicinin dizin oluşturma ve sorgu işleme üzerindeki etkisine daha yakından bakın. Özellikle, bir çözümleyicinin dizininiz için tam olarak ne oluşturduğunu görebilmeniz için belirteçleştirilmiş çıktıyı döndürmek için Çözümleme Metni API'sini kullanmayı düşünün.

+ [Dil çözümleyicileri](search-language-support.md)
+ [Azure Bilişsel Arama'da metin işleme için çözümleyiciler](search-analyzers.md)
+ [Metin API'sini Analiz Et (REST)](https://docs.microsoft.com/rest/api/searchservice/test-analyzer)
+ [Tam metin arama nasıl çalışır (sorgu mimarisi)](search-lucene-query-architecture.md)