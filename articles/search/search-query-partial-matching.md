---
title: Desenleri ve özel karakterleri eşleştirme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgusu nda tam veya kısmi terimlerle eşleşmek için joker karakter ve önek sorgularını kullanın. Özel karakterler içeren eşleşmesi zor desenler tam sorgu sözdizimi ve özel çözümleyiciler kullanılarak çözülebilir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/14/2020
ms.openlocfilehash: f78ba5b351a3da46d7b8b3780cf00772c4f3b2ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289320"
---
# <a name="match-on-patterns-and-special-characters-dashes"></a>Desenlerve özel karakterler (tireler) ile eşleştirme

Özel karakterler (),`-, *, (, ), /, \, =`veya daha büyük bir terim deki kısmi terimleri temel alan sorgu desenleri içeren sorgular için, dizinin beklenen içeriği doğru biçimde içerdiğinden emin olmak için genellikle ek yapılandırma adımları gerekir. 

Varsayılan `+1 (425) 703-6214` olarak, gibi bir telefon numarası `"1"` `"425"`, `"703"` `"6214"`, , . Tahmin edebileceğiniz gibi, `"3-62"`arama , bir çizgi içeren kısmi terimler, bu içerik aslında dizin yok çünkü başarısız olacaktır. 

Kısmi dizeleri veya özel karakterler üzerinde arama yapmanız gerektiğinde, varsayılan çözümleyiciyi daha basit belirteç verme kuralları altında çalışan özel bir çözümleyiciyle geçersiz kılabilir, sorgu dizeleri bir terimin veya özel dizeleri içerdiğinde gerekli tüm terimleri koruyarak Karakter. Bir adım geri alarak, yaklaşım şu na benzer:

+ Önceden tanımlanmış bir çözümleyici seçin veya istenen çıktıyı üreten özel bir çözümleyici tanımlayın
+ Çözümleyiciyi alana atama
+ Dizin oluşturma ve test

Bu makalede, bu görevler boyunca size yol. Burada açıklanan yaklaşım diğer senaryolarda yararlıdır: joker karakter ve normal ifade sorguları da desen eşleştirme için temel olarak tüm terimler ilerler. 

> [!TIP]
> Analyers değerlendirilmesi sık dizin yeniden gerektiren bir yinelemeli bir süreçtir. Postacı, [Dizin Oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index), [Dizin Sil](https://docs.microsoft.com/rest/api/searchservice/delete-index),[Belgeleri Yükle](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)ve Arama [Belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents)için REST API'lerini kullanarak bu adımı kolaylaştırabilirsiniz. Yükleme Belgeleri için istek gövdesi, test etmek istediğiniz küçük bir temsili veri kümesi (örneğin, telefon numaraları veya ürün kodları içeren bir alan) içermelidir. Aynı Postacı koleksiyonundaki bu API'lerle, bu adımları hızlı bir şekilde gözden geçirebilirsiniz.

## <a name="choosing-an-analyzer"></a>Çözümleyici seçme

Tam süreli belirteçler üreten bir çözümleyici seçerken, aşağıdaki çözümleyiciler sık kullanılan seçeneklerdir:

| Analyzer | Davranışlar |
|----------|-----------|
| [Anahtar kelime](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) | Tüm alanın içeriği tek bir terim olarak belirtilir. |
| [Boşluk](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html) | Yalnızca beyaz alanlarda ayırır. Tire veya diğer karakterleri içeren terimler tek bir belirteç olarak kabul edilir. |
| [özel analizör](index-add-custom-analyzers.md) | (önerilir) Özel bir çözümleyici oluşturmak, hem belirteç ve belirteç filtresini belirtmenize olanak tanır. Önceki çözümleyiciler olduğu gibi kullanılmalıdır. Özel bir çözümleyici, hangi belirteçleri ve belirteç filtreleri kullanmak için seçmenizi sağlar. <br><br>Önerilen kombinasyon, [küçük harf belirteç filtresine](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html)sahip anahtar kelime [tokenizeridir.](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html) Kendi başına, önceden tanımlanmış [anahtar kelime çözümleyici](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) sorguları başarısızlığa neden olabilir herhangi bir büyük harfli metin, küçük harf değildir. Özel bir çözümleyici, küçük harf belirteç filtresi eklemek için bir mekanizma sağlar. |

Postacı gibi bir web API test aracı kullanıyorsanız, belirteç çıktısını incelemek için [Test Analyzer REST çağrısını](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) ekleyebilirsiniz. Varolan bir dizin ve tire veya kısmi terimler içeren bir alan göz önüne alındığında, hangi belirteçlerin yayDığını görmek için belirli terimler üzerinde çeşitli çözümleyicileri deneyebilirsiniz.  

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
> Sorgu ağacını kurarken sorgu ayrıştırıcılarının genellikle bir arama ifadesinde küçük harf terimlerini ayrıştırdığını unutmayın. Küçük harfli metin girişleri olmayan bir çözümleyici kullanıyorsanız ve beklenen sonuçları almıyorsanız, bu nedenle olabilir. Çözüm bir lwower-case belirteç filtresi eklemektir.

## <a name="analyzer-definitions"></a>Çözümleyici tanımları
 
Çözümleyicileri değerlendiriyor veya belirli bir yapılandırmaile ilerliyor olun, alan tanımında çözümleyiciyi belirtmeniz ve yerleşik bir çözümleyici kullanmıyorsanız çözümleyicinin kendisini yapılandırmanız gerekir. Çözümleyicileri değiştirirken, genellikle dizini yeniden oluşturmanız gerekir (bırak, yeniden oluşturma ve yeniden yükleme). 

### <a name="use-built-in-analyzers"></a>Yerleşik çözümleyicileri kullanma

Yerleşik veya önceden tanımlanmış çözümleyiciler, dizinde `analyzer` ek yapılandırma gerektirmeden alan tanımının özelliğinde adlarıyla belirtilebilir. Aşağıdaki örnek, çözümleyiciyi `whitespace` bir alana nasıl ayarlayacağınızı gösterir.

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
Tüm kullanılabilir yerleşik çözümleyiciler hakkında daha fazla bilgi için, [Önceden Tanımlanmış çözümleyiciler listesine](https://docs.microsoft.com/azure/search/index-add-custom-analyzers#predefined-analyzers-reference)bakın. 

### <a name="use-custom-analyzers"></a>Özel çözümleyiciler kullanma

Özel bir [çözümleyici](index-add-custom-analyzers.md)kullanıyorsanız, dizin içinde kullanıcı tanımlı bir belirteç, belirteç filtresi ve olası yapılandırma ayarlarıyla tanımlayın. Daha sonra, yerleşik bir çözümleyici gibi, bir alan tanımı üzerinde başvuru.

Amaç tam terim belirteçleştirme olduğunda, **bir anahtar kelime belirteci** ve **küçük harf belirteç filtresinden** oluşan özel bir çözümleyici önerilir.

+ Anahtar kelime belirteci, bir alanın tüm içeriği için tek bir belirteç oluşturur.
+ Küçük harf belirteç filtresi büyük harfleri küçük harfli metne dönüştürür. Sorgu ayrıştırıcıları genellikle herhangi bir büyük metin girişlerini küçük düşürür. Düşükleme, girdileri belirteçli terimlerle homojenize eder.

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

### <a name="duplicate-fields-for-different-scenarios"></a>Farklı senaryolar için yinelenen alanlar

Başka bir seçenek, farklı senaryolar için optimize etmek için alan başına çözümleyici atama yararlanır. Özellikle, birinci ve gelişmiş desen eşleşen ilk düzenli tam metin arama desteklemek için "featureCode" ve "featureCodeRegex" tanımlayabilirsiniz.

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

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, çözümleyicilerin hem sorgu sorunlarına katkıda bulunmaları hem de sorgu sorunlarını nasıl çözdükleri açıklanmaktadır. Bir sonraki adım olarak, çözümleyicinin dizin oluşturma ve sorgu işleme üzerindeki etkisine daha yakından bakın. Özellikle, bir çözümleyicinin dizininiz için tam olarak ne oluşturduğunu görebilmeniz için belirteçleştirilmiş çıktıyı döndürmek için Çözümleme Metni API'sini kullanmayı düşünün.

+ [Dil çözümleyicileri](search-language-support.md)
+ [Azure Bilişsel Arama'da metin işleme için çözümleyiciler](search-analyzers.md)
+ [Metin API'sini Analiz Et (REST)](https://docs.microsoft.com/rest/api/searchservice/test-analyzer)
+ [Tam metin arama nasıl çalışır (sorgu mimarisi)](search-lucene-query-architecture.md)
