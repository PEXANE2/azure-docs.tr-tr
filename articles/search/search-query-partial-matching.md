---
title: Kısmi terimler, desenler ve özel karakterler
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgu isteğindeki tüm veya kısmi terimlerle eşleştirmek için joker karakter, Regex ve önek sorguları kullanın. Özel karakterler içeren donanımdan eşleşme desenleri, tam sorgu sözdizimi ve özel çözümleyiciler kullanılarak çözülebilir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: 5a05f2973ac17460250fb3e80eb7bc0da9849940
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81262885"
---
# <a name="partial-term-search-and-patterns-with-special-characters-wildcard-regex-patterns"></a>Kısmi terim arama ve özel karakterlerle desenler (joker karakter, Regex, desenler)

*Kısmi bir terim arama* , dönem parçalarından oluşan sorguları ifade eder; burada, tam bir dönem yerine yalnızca başlangıç, orta veya terim (ön ek, ınfıx veya sonek sorguları olarak adlandırılır) olabilir. Bir *kalıp* , genellikle sorgu dizesinin parçası olan tireler veya eğik çizgiler gibi özel karakterler içeren parçaların bir birleşimidir. Yaygın kullanım örnekleri, telefon numarası, URL, kişi veya ürün kodlarının veya Bileşik sözcüklerin bölümlerinin sorgulanmasını içerir.

Dizin beklenen biçimde şartlar içermiyorsa kısmi ve kalıp arama sorunlu olabilir. Dizin oluşturma işleminin (varsayılan standart çözümleyicisini varsayarak) [sözcük analizi aşamasında](search-lucene-query-architecture.md#stage-2-lexical-analysis) , özel karakterler atılır, bileşik ve Bileşik dizeler bölünür ve boşluk silinir; tümü, hiçbir eşleşme bulunamadığında, model sorgularının başarısız olmasına neden olabilir. Örneğin `+1 (425) 703-6214` , ( `"1"` `"425"` `"703"`Yani,,,, `"6214"`) gibi bir telefon numarası, içerik gerçekten dizinde bulunmadığından bir `"3-62"` sorguda gösterilmez. 

Çözüm, gerekirse boşluklar ve özel karakterler de dahil olmak üzere tüm dizeleri koruyan bir çözümleyici çağırmak ve böylece kısmi hüküm ve desenlerle eşleşemez. Bozulmadan bir dize için ek alan oluşturma ve içerik koruma Çözümleyicisi kullanma, çözümün temelini oluşturur.

> [!TIP]
> Postman ve REST API 'Leri hakkında bilgi sahibi misiniz? Bu makalede açıklanan kısmi terimleri ve özel karakterleri sorgulamak için [sorgu örnekleri koleksiyonunu indirin](https://github.com/Azure-Samples/azure-search-postman-samples/tree/master/full-syntax-examples) .

## <a name="what-is-partial-search-in-azure-cognitive-search"></a>Azure Bilişsel Arama kısmi arama nedir?

Azure Bilişsel Arama 'de, kısmi arama ve desenler şu formlarda kullanılabilir:

+ "Capda jakın `search=cap*`su ön Inn" veya "Gacc sermaye" Ile eşleşen [ön ek arama](query-simple-syntax.md#prefix-search). Ön ek arama için basit sorgu söz dizimini veya tam Lucene sorgu söz dizimini kullanabilirsiniz.

+ Katıştırılmış bir dizenin bir modelini veya parçalarını arayan [joker karakter arama](query-lucene-syntax.md#bkmk_wildcard) veya [normal ifadeler](query-lucene-syntax.md#bkmk_regex) . Joker karakter ve normal ifadeler tam Lucene sözdizimini gerektirir. Sonek ve dizin sorguları, normal bir ifade olarak formüllüyor.

  Kısmi terim aramasına ilişkin bazı örnekler şunlardır. Bir sonek sorgusu için, "alfasayısal" terimi verildiğinde bir eşleştirme bulmak için joker karakter araması (`search=/.*numeric.*/`) kullanırsınız. URL parçası gibi iç karakterler içeren kısmi bir terim için kaçış karakterleri eklemeniz gerekebilir. JSON 'da eğik çizgi `/` , ters eğik `\`çizgiyle çıkış olur. Bu nedenle, `search=/.*microsoft.com\/azure\/.*/` "MICROSOFT.com/Azure/" URL parçasına yönelik sözdizimidir.

Belirtildiği gibi, yukarıdaki tüm, dizinin, standart çözümleyici tarafından sağlamayan bir biçim iletkenine ve model eşleştirmeye yönelik dizeler içermesi gerekir. Bu makaledeki adımları izleyerek, bu senaryoları desteklemek için gerekli içeriğin mevcut olduğundan emin olabilirsiniz.

## <a name="solving-partialpattern-search-problems"></a>Kısmi/kalıp arama sorunlarını çözme

Parçalar veya desenler veya özel karakterler üzerinde arama yapmanız gerektiğinde, varsayılan çözümleyici 'yi daha basit simgeleştirme kuralları altında çalışan özel bir çözümleyici ile geçersiz kılabilirsiniz ve tüm dizeyi korur. Bir adım geri alınarak yaklaşım şuna benzer:

+ Dizenin bozulmadan bir sürümünü depolamak için bir alan tanımlayın (çözümlenme ve çözümlenmemiş metin istediğiniz varsayılarak)
+ Önceden tanımlanmış bir çözümleyici seçin veya çözümlenmemiş olmayan bir dizenin çıktısını almak için özel bir çözümleyici tanımlayın
+ Özel çözümleyici 'yi alana atama
+ Dizin oluşturma ve test etme

> [!TIP]
> Çözümleyiciler değerlendirmek, sık sık dizin yeniden oluşturma gerektiren yinelemeli bir işlemdir. Bu adımı Postman, [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index), [Dizin silme](https://docs.microsoft.com/rest/api/searchservice/delete-index),[belge yükleme](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)ve [belge arama](https://docs.microsoft.com/rest/api/searchservice/search-documents)için REST API 'leri kullanarak daha kolay hale getirebilirsiniz. Yükleme belgeleri için, istek gövdesi test etmek istediğiniz küçük bir temsili veri kümesi (örneğin, telefon numarası veya ürün kodu içeren bir alan) içermelidir. Aynı Postman koleksiyonundaki bu API 'lerle, bu adımları hızla geçebilirsiniz.

## <a name="duplicate-fields-for-different-scenarios"></a>Farklı senaryolar için yinelenen alanlar

Çözümleyiciler alan temelinde atanır. Bu, dizininizdeki alanları farklı senaryolar için iyileştirmek üzere oluşturabileceğiniz anlamına gelir. Özellikle, ilki üzerinde düzenli tam metin aramasını desteklemek için "featureCode" ve "featureCodeRegex" tanımlayabilir ve ikincide gelişmiş bir düzende eşleştirme yapabilirsiniz.

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
  "analyzer": "my_custom_analyzer"
},
```

## <a name="choose-an-analyzer"></a>Çözümleyici seçin

Tam terim belirteçleri üreten bir çözümleyici seçerken, aşağıdaki çözümleyiciler yaygın seçimlerdir:

| Analyzer | Davranışlar |
|----------|-----------|
| [Dil Çözümleyicileri](index-add-language-analyzers.md) | Birleşik kelimeler veya dizelerde, sesli harfler ve fiil formlarında kısa çizgileri korur. Sorgu desenleri tire içeriyorsa, dil Çözümleyicisi kullanmak yeterli olabilir. |
| [sözcükle](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) | Tüm alanın içeriği tek bir terim olarak simgeleştirilir. |
| [boşlu](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html) | Yalnızca boşluklardan ayrılır. Tireler veya diğer karakterleri içeren koşullar tek bir belirteç olarak değerlendirilir. |
| [özel çözümleyici](index-add-custom-analyzers.md) | Önerilen Özel bir çözümleyici oluşturma, hem belirteç ayırıcı hem de Token filtresi belirtmenize olanak tanır. Önceki çözümleyiciler olduğu gibi kullanılmalıdır. Özel çözümleyici, hangi simgeleyiciler ve belirteç filtrelerini kullanacağınızı seçmenizi sağlar. <br><br>Önerilen birleşim, [küçük harfli bir belirteç filtresiyle](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html) [Simgeleştirici anahtar kelimedir](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html) . Tek başına, önceden tanımlanmış [anahtar sözcük Çözümleyicisi](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) büyük/küçük harf, sorguların başarısız olmasına neden olabilir. Özel çözümleyici, size küçük harfli belirteç filtresi eklemek için bir mekanizma sağlar. |

Postman gibi bir Web API test aracı kullanıyorsanız, simgeleştirilmiş çıktıyı incelemek için [Test ÇÖZÜMLEYICISI Rest çağrısını](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) ekleyebilirsiniz.

Birlikte çalışmak için mevcut bir dizininizin olması gerekir. Var olan bir dizin ve tire ya da kısmi terim içeren bir alan verildiğinde, hangi belirteçlerin yayınlandığını görmek için belirli koşullara göre çeşitli çözümleyiciler deneyebilirsiniz.  

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
1. `whitespace` Veya `keyword` çözümleyicisini kullanmak için isteği değiştirin:

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
> Sorgunun, sorgu ağacını oluştururken bir arama ifadesinde genellikle küçük harf koşullarına duyarlı olduğunu unutmayın. Büyük/küçük harf metin girişi olmayan bir çözümleyici kullanıyorsanız ve beklenen sonuçları alamıyorsanız, bunun nedeni bu olabilir. Çözüm, aşağıdaki "özel Çözümleyicileri kullanma" bölümünde açıklandığı gibi küçük harfli bir belirteç filtresi eklemektir.

## <a name="configure-an-analyzer"></a>Çözümleyici yapılandırma
 
Çözümleyici 'yi değerlendiriyor veya belirli bir yapılandırma ile ileriye doğru hareket etmekle birlikte, alan tanımında Çözümleyicisi belirtmeniz ve bir yerleşik çözümleyici kullanmıyorsanız, büyük olasılıkla çözümleyici 'yi yapılandırmanız gerekir. Çözümleyiciler takas edildiğinde, genellikle dizini yeniden oluşturmanız gerekir (drop, yeniden oluştur ve yeniden yükle). 

### <a name="use-built-in-analyzers"></a>Yerleşik çözümleyiciler kullanın

Yerleşik veya önceden tanımlanmış çözümleyiciler, dizinde ek yapılandırma gerekmeden, bir alan tanımının `analyzer` bir özelliğinde ad ile belirtilebilir. Aşağıdaki örnek, bir alanı üzerinde `whitespace` çözümleyiciyi nasıl ayarlayabileceğinizi gösterir. 

Diğer senaryolar ve diğer yerleşik çözümleyiciler hakkında daha fazla bilgi edinmek için bkz. [önceden tanımlanmış çözümleyiciler listesi](https://docs.microsoft.com/azure/search/index-add-custom-analyzers#predefined-analyzers-reference). 

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

[Özel bir çözümleyici](index-add-custom-analyzers.md)kullanıyorsanız, bu dosyayı, Kullanıcı tanımlı Simgeleştirici, belirteç filtresi, olası yapılandırma ayarları ile birlikte dizinde tanımlayın. Daha sonra, bir alan tanımında, tıpkı yerleşik çözümleyici gibi, ona başvurun.

Amaç bir bütün olarak simgeleştirmeye ayarlandığında, **simgeleştirme anahtar sözcüğünden** ve **küçük harfli belirteç filtresinden** oluşan özel bir çözümleyici önerilir.

+ Belirteç ayırıcı anahtar sözcüğü, bir alanın tüm içeriği için tek bir belirteç oluşturur.
+ Küçük harfli belirteç filtresi, büyük harfli harfleri küçük harfli metne dönüştürür. Sorgu Çözümleyicileri genellikle küçük harfli bir metin girişi yapar. Büyük küçük harf, girdileri simgeleştirilmiş koşullara göre sıralar.

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
> `keyword_v2` Belirteç ayırıcı ve `lowercase` Token filtresi, sistem tarafından bilinir ve varsayılan yapılandırmalarının kullanılması gerekir. bu nedenle, bunları önce tanımlamak zorunda kalmadan ada göre başvurabileceğiniz anlamına gelir.

## <a name="build-and-test"></a>Derleme ve test etme

Çözümleyicinizi destekleyen çözümleyiciler ve alan tanımlarına sahip bir dizin tanımladıktan sonra, kısmi dize sorgularını test edebilmeniz için temsili dizeleri olan belgeleri yükleyin. 

Önceki bölümlerde Logic açıklanmaktadır. Bu bölüm, çözümünüzü sınarken çağırmanız gereken her API aracılığıyla yapmanız gereken adımları uygulayın. Daha önce belirtildiği gibi, Postman gibi etkileşimli bir Web testi aracı kullanırsanız, bu görevleri hızlıca izleyebilirsiniz.

+ [Dizini Sil](https://docs.microsoft.com/rest/api/searchservice/delete-index) , yeniden oluşturabilmeniz için aynı ada sahip var olan bir dizini kaldırır.

+ [Create INDEX](https://docs.microsoft.com/rest/api/searchservice/create-index) , çözümleyici tanımları ve çözümleyici belirtimine sahip alanlar da dahil olmak üzere arama hizmetinizde dizin yapısı oluşturur.

+ [Yükleme belgeleri](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) , dizininizdeki ve aranabilir içeriğin yanı sıra aynı yapıya sahip belgeleri içeri aktarır. Bu adımdan sonra, dizininiz sorgu veya test için hazırlanıyor.

+ [Test Çözümleyicisi](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) , [bir çözümleyici seçin](#choose-an-analyzer)bölümünde tanıtılmıştır. Koşulların nasıl simgeleştirilmiş olduğunu anlamak için çeşitli çözümleyiciler kullanarak dizininizdeki dizelerin bazılarını test edin.

+ [Arama belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents) , joker ve normal ifadeler için [basit söz dizimi](query-simple-syntax.md) veya [tam Lucene sözdizimini](query-lucene-syntax.md) kullanarak bir sorgu isteğinin nasıl oluşturulacağını açıklar.

  "+ 1 (425) 703-6214" üzerinde bir eşleşme bulmak için "3-6214" sorgulaması gibi kısmi terim sorguları için, basit söz dizimini kullanabilirsiniz: `search=3-6214&queryType=simple`.

  "Num" veya "" alfasayısal "ile eşleşen bir eşleşme bulmak için sayısal olarak sorgu ve sonek sorguları için, tam Lucene söz dizimini ve bir normal ifadeyi kullanın:`search=/.*num.*/&queryType=full`

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

### <a name="use-different-analyzers-for-indexing-and-query-processing"></a>Dizin oluşturma ve sorgu işleme için farklı çözümleyiciler kullanma

Çözümleyiciler dizin oluşturma sırasında ve sorgu yürütülürken çağırılır. Her ikisi için de aynı çözümleyici kullanılması yaygındır, ancak her iş yükü için özel Çözümleyicileri yapılandırabilirsiniz. Çözümleyici geçersiz kılmaları bir `analyzers` bölümdeki [Dizin tanımında](https://docs.microsoft.com/rest/api/searchservice/create-index) belirtilir ve ardından belirli alanlarda başvurulur. 

Özel analiz yalnızca dizin oluşturma sırasında gerekliyse, özel çözümleyici 'yi yalnızca dizin oluşturma için uygulayabilir ve sorgular için standart Lucene Analyzer 'ı (veya başka bir Çözümleyicisi) kullanmaya devam edebilirsiniz.

Role özgü analiz belirtmek için, her biri için alanın özelliklerini, varsayılan `indexAnalyzer` `searchAnalyzer` `analyzer` Özellik yerine ve ayarlarını belirleyebilirsiniz.

```json
"name": "featureCode",
"indexAnalyzer":"my_customanalyzer",
"searchAnalyzer":"standard",
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, çözümleyicilerin her ikisi de sorgu sorunlarına katkıda bulunma ve sorgu sorunlarını çözme işlemleri açıklanmaktadır. Bir sonraki adımda, dizin oluşturma ve sorgu işleme konusunda çözümleyici etkisi konusuna daha yakından göz atın. Özellikle, dizininiz için bir çözümleyici 'nin nasıl olduğunu tam olarak görebilmeniz için simgeleştirilmiş çıktıyı döndürmek için metin analizi API 'sini kullanmayı göz önünde bulundurun.

+ [Dil çözümleyicileri](search-language-support.md)
+ [Azure Bilişsel Arama metin işleme için çözümleyiciler](search-analyzers.md)
+ [Metin API 'sini çözümleme (REST)](https://docs.microsoft.com/rest/api/searchservice/test-analyzer)
+ [Tam metin aramasının nasıl çalıştığı (sorgu mimarisi)](search-lucene-query-architecture.md)