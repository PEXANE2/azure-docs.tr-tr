---
title: Desenleri ve özel karakterleri Eşleştir
titleSuffix: Azure Cognitive Search
description: Bir Azure Bilişsel Arama sorgu isteğindeki tüm veya kısmi terimlerle eşleştirmek için joker karakter ve ön ek sorguları kullanın. Özel karakterler içeren donanımdan eşleşme desenleri, tam sorgu sözdizimi ve özel çözümleyiciler kullanılarak çözülebilir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/14/2020
ms.openlocfilehash: ec1422d03cce78bdd8206f6687a78b63ddf989dc
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75989624"
---
# <a name="match-on-patterns-and-special-characters-dashes"></a>Desenlerde ve özel karakterlerle Eşleştir (Tireler)

Özel karakterler (`-, *, (, ), /, \, =`) veya daha büyük bir dönem içindeki kısmi koşullara göre sorgu desenleri içeren sorgular için, dizinin beklenen içeriği doğru biçimde içerdiğinden emin olmak için ek yapılandırma adımları genellikle gerekir. 

Varsayılan olarak, `+1 (425) 703-6214` gibi bir telefon numarası `"1"`, `"425"`, `"703"`, `"6214"`olarak simgeleştirilir. Imagine de `"3-62"`arama, tire içeren kısmi koşullar, içerik gerçekten dizinde bulunmadığından başarısız olur. 

Kısmi dizeler veya özel karakterler üzerinde arama yapmanız gerektiğinde, varsayılan çözümleyici 'yi daha basit simgeleştirme kuralları altında çalışan özel bir çözümleyici ile geçersiz kılabilirsiniz ve sorgu dizeleri bir terimin parçalarını veya özel olarak dahil edildiğinde, tüm terimleri korur karakterle. Bir adım geri alınarak yaklaşım şuna benzer:

+ Önceden tanımlanmış bir çözümleyici seçin veya istenen çıktıyı üreten özel bir çözümleyici tanımlayın
+ Çözümleyici 'yi alana atama
+ Dizin ve test oluşturma

Bu makalede, bu görevlerde size kılavuzluk eder. Burada açıklanan yaklaşım diğer senaryolarda faydalıdır: joker karakter ve normal ifade sorguları Ayrıca, model eşleştirme için temel olarak tüm koşullara ihtiyaç duyar. 

> [!TIP]
> Çözümleyiciler değerlendirmek, sık sık dizin yeniden oluşturma gerektiren yinelemeli bir işlemdir. Bu adımı Postman, [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index), [Dizin silme](https://docs.microsoft.com/rest/api/searchservice/delete-index),[belge yükleme](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)ve [belge arama](https://docs.microsoft.com/rest/api/searchservice/search-documents)için REST API 'leri kullanarak daha kolay hale getirebilirsiniz. Yükleme belgeleri için, istek gövdesi test etmek istediğiniz küçük bir temsili veri kümesi (örneğin, telefon numarası veya ürün kodu içeren bir alan) içermelidir. Aynı Postman koleksiyonundaki bu API 'lerle, bu adımları hızla geçebilirsiniz.

## <a name="choosing-an-analyzer"></a>Çözümleyici seçme

Tam terim belirteçleri üreten bir çözümleyici seçerken, aşağıdaki çözümleyiciler yaygın seçimlerdir:

| Analyzer | Davranışlar |
|----------|-----------|
| [sözcükle](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) | Tüm alanın içeriği tek bir terim olarak simgeleştirilir. |
| [boşlu](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html) | Yalnızca boşluklardan ayrılır. Tireler veya diğer karakterleri içeren koşullar tek bir belirteç olarak değerlendirilir. |
| [özel çözümleyici](index-add-custom-analyzers.md) | Önerilen Özel bir çözümleyici oluşturma, hem belirteç ayırıcı hem de Token filtresi belirtmenize olanak tanır. Önceki çözümleyiciler olduğu gibi kullanılmalıdır. Özel çözümleyici, hangi simgeleyiciler ve belirteç filtrelerini kullanacağınızı seçmenizi sağlar. <br><br>Önerilen birleşim, [küçük harfli bir belirteç filtresiyle](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html) [Simgeleştirici anahtar kelimedir](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html) . Tek başına, önceden tanımlanmış [anahtar sözcük Çözümleyicisi](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) büyük/küçük harf, sorguların başarısız olmasına neden olabilir. Özel çözümleyici, size küçük harfli belirteç filtresi eklemek için bir mekanizma sağlar. |

Postman gibi bir Web API test aracı kullanıyorsanız, simgeleştirilmiş çıktıyı incelemek için [Test ÇÖZÜMLEYICISI Rest çağrısını](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) ekleyebilirsiniz. Var olan bir dizin ve tire ya da kısmi terim içeren bir alan verildiğinde, hangi belirteçlerin yayınlandığını görmek için belirli koşullara göre çeşitli çözümleyiciler deneyebilirsiniz.  

1. Koşulların varsayılan olarak nasıl simgeleştirilmiş olduğunu görmek için standart çözümleyici 'yi denetleyin.

   ```json
   {
   "text": "SVP10-NOR-00",
   "analyzer": "standard"
   }
    ```

1. Metnin dizin içinde nasıl simgeleştirilmiş olduğunu görmek için yanıtı değerlendirin. Her bir terimin ne kadar küçük ve nasıl bölündüğü hakkında dikkat edin.

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
1. `whitespace` veya `keyword` çözümleyici 'yi kullanmak için isteği değiştirin:

    ```json
    {
    "text": "SVP10-NOR-00",
    "analyzer": "keyword"
    }
    ```

1. Artık yanıt, dizenin bir parçası olarak bir üst üste çizgilerden oluşan tek bir belirteçle oluşur. Bir düzende veya kısmi bir dönemde aramanız gerekiyorsa, sorgu altyapısı artık eşleşme bulma temelini oluşturur.


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
> Sorgunun, sorgu ağacını oluştururken bir arama ifadesinde genellikle küçük harf koşullarına duyarlı olduğunu unutmayın. Büyük/küçük harf metin girişi olmayan bir çözümleyici kullanıyorsanız ve beklenen sonuçları alamıyorsanız, bunun nedeni bu olabilir. Çözüm, bir lwower-Case belirteç filtresi eklemektir.

## <a name="analyzer-definitions"></a>Çözümleyici tanımları
 
Çözümleyici 'yi değerlendiriyor veya belirli bir yapılandırma ile ileriye doğru hareket etmekle birlikte, alan tanımında Çözümleyicisi belirtmeniz ve bir yerleşik çözümleyici kullanmıyorsanız, büyük olasılıkla çözümleyici 'yi yapılandırmanız gerekir. Çözümleyiciler takas edildiğinde, genellikle dizini yeniden oluşturmanız gerekir (drop, yeniden oluştur ve yeniden yükle). 

### <a name="use-built-in-analyzers"></a>Yerleşik çözümleyiciler kullanın

Yerleşik veya önceden tanımlanmış çözümleyiciler, dizinde ek yapılandırma gerekmeden, bir alan tanımının `analyzer` özelliği üzerinde ada göre belirtilebilir. Aşağıdaki örnek, bir alan üzerinde `whitespace` çözümleyicisini nasıl ayarlayabileceğinizi gösterir.

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
Tüm mevcut yerleşik çözümleyiciler hakkında daha fazla bilgi için bkz. [önceden tanımlanmış çözümleyiciler listesi](https://docs.microsoft.com/azure/search/index-add-custom-analyzers#predefined-analyzers-reference). 

### <a name="use-custom-analyzers"></a>Özel çözümleyiciler kullanma

[Özel bir çözümleyici](index-add-custom-analyzers.md)kullanıyorsanız, bu dosyayı bir Kullanıcı tanımlı Simgeleştirici, tokenfilter, olası yapılandırma ayarları ile birlikte dizinde tanımlayın. Daha sonra, bir alan tanımında, tıpkı yerleşik çözümleyici gibi, ona başvurun.

Amaç bir bütün olarak simgeleştirmeye ayarlandığında, **simgeleştirme anahtar sözcüğünden** ve **küçük harfli belirteç filtresinden** oluşan özel bir çözümleyici önerilir.

+ Belirteç ayırıcı anahtar sözcüğü, bir alanın tüm içeriği için tek bir belirteç oluşturur.
+ Küçük harfli belirteç filtresi, büyük harfli harfleri küçük harfli metne dönüştürür. Sorgu Çözümleyicileri genellikle küçük harfli bir metin girişi yapar. Küçük harf, girdileri simgeleştirilmiş koşullara ayırır.

Aşağıdaki örnek, Simgeleştirici ve küçük harfli bir belirteç filtresi anahtar sözcüğünü sağlayan özel bir çözümleyici gösterir.

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
]

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
> `keyword_v2` belirteç ayırıcı ve `lowercase` Token filtresi, sistem tarafından bilinir ve varsayılan yapılandırmalarının kullanılması gerekir. bu nedenle, bunları önce tanımlamak zorunda kalmadan adlarıyla başvurabileceğiniz anlamına gelir.

## <a name="tips-and-best-practices"></a>İpuçları ve en iyi yöntemler

### <a name="tune-query-performance"></a>Sorgu performansını ayarlama

Keyword_v2 belirteç ayırıcı ve küçük harfli belirteç filtresini içeren önerilen yapılandırmayı uygularsanız, dizininizdeki mevcut belirteçler üzerinde ek belirteç filtresi işleme nedeniyle sorgu performansında azalmayı fark edebilirsiniz. 

Aşağıdaki örnek, öneki daha hızlı eşleşmekte olmak için bir [Edgengramtokenfilter](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenizer.html) ekler. Şu karakterleri içeren 2-25 karakter birleşimlerinde ek belirteçler oluşturulur: (yalnızca MS, MSF, MSFT, MSFT/, MSFT/S, MSFT/SQ, MSFT/SQL). Imagine de, ek simgeleştirme daha büyük bir dizin ile sonuçlanır.

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
]

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

### <a name="use-different-analyzers-for-indexing-and-query-processing"></a>Dizin oluşturma ve sorgu işleme için farklı çözümleyiciler kullanma

Çözümleyiciler dizin oluşturma sırasında ve sorgu yürütülürken çağırılır. Her ikisi için de aynı çözümleyici kullanılması yaygındır, ancak her iş yükü için özel Çözümleyicileri yapılandırabilirsiniz. Çözümleyici geçersiz kılmaları `analyzers` bölümünde [Dizin tanımında](https://docs.microsoft.com/rest/api/searchservice/create-index) belirtilir ve sonra belirli alanlarda başvurulur. 

Özel analiz yalnızca dizin oluşturma sırasında gerekliyse, özel çözümleyici 'yi yalnızca dizin oluşturma için uygulayabilir ve sorgular için standart Lucene Analyzer 'ı (veya başka bir Çözümleyicisi) kullanmaya devam edebilirsiniz.

Role özgü analiz belirtmek için, her biri için alanın özelliklerini ayarlayabilir, varsayılan `analyzer` özelliği yerine `indexAnalyzer` ve `searchAnalyzer` belirleyebilirsiniz.

```json
"name": "featureCode",
"indexAnalyzer":"my_customanalyzer",
"searchAnalyzer":"standard",
```

### <a name="duplicate-fields-for-different-scenarios"></a>Farklı senaryolar için yinelenen alanlar

Başka bir seçenek, farklı senaryolar için optimize etmek üzere alan başına çözümleyici atamasını kullanır. Özellikle, ilki üzerinde düzenli tam metin aramasını desteklemek için "featureCode" ve "featureCodeRegex" tanımlayabilir ve ikincide gelişmiş bir düzende eşleştirme yapabilirsiniz.

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

Bu makalede, çözümleyicilerin her ikisi de sorgu sorunlarına katkıda bulunma ve sorgu sorunlarını çözme işlemleri açıklanmaktadır. Bir sonraki adımda, dizin oluşturma ve sorgu işleme konusunda çözümleyici etkisi konusuna daha yakından göz atın. Özellikle, dizininiz için bir çözümleyici 'nin nasıl olduğunu tam olarak görebilmeniz için simgeleştirilmiş çıktıyı döndürmek için metin analizi API 'sini kullanmayı göz önünde bulundurun.

+ [Dil çözümleyicileri](search-language-support.md)
+ [Azure Bilişsel Arama metin işleme için çözümleyiciler](search-analyzers.md)
+ [Metin API 'sini çözümleme (REST)](https://docs.microsoft.com/rest/api/searchservice/test-analyzer)
+ [Tam metin aramasının nasıl çalıştığı (sorgu mimarisi)](search-lucene-query-architecture.md)