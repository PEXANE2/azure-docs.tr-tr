---
title: Dize alanlarına özel çözümleyiciler ekleme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama tam metin arama sorgularında kullanılan metin belirteçlerini ve karakter filtrelerini yapılandırın.
manager: nitinme
author: Yahnoosh
ms.author: jlembicz
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 9bf0fb1a33a98031a78155a3956ac6d6abe33029
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113621"
---
# <a name="add-custom-analyzers-to-string-fields-in-an-azure-cognitive-search-index"></a>Azure Bilişsel Arama dizinindeki dize alanlarına özel çözümleyiciler ekleme

*Özel çözümleyici,* varolan belirteçve isteğe bağlı filtrelerin kullanıcı tanımlı bir birleşiminden oluşan belirli bir [metin çözümleyici](search-analyzers.md) türüdür. Belirteci ve filtreleri yeni yollarla birleştirerek, belirli sonuçlar elde etmek için arama motorundaki metin işlemeyi özelleştirebilirsiniz. Örneğin, metin girişleri belirtilmeden önce HTML biçimlendirmesini kaldırmak için *char filtresi* olan özel bir çözümleyici oluşturabilirsiniz.

 Filtrelerin birleşimini değiştirmek için birden çok özel çözümleyici tanımlayabilirsiniz, ancak her alan dizine çözümleme çözümlemesi için yalnızca bir çözümleyici ve arama çözümlemesi için bir tane kullanabilir. Müşteri çözümleyicisinin nasıl göründüğünün bir örneği için Özel [çözümleyici örneğine](search-analyzers.md#Custom-analyzer-example)bakın.

## <a name="overview"></a>Genel Bakış

 Tam metin [arama motorunun](search-lucene-query-architecture.md)rolü, basit bir ifadeyle, belgeleri verimli sorgulama ve alma sağlayacak şekilde işlemek ve depolamaktır. Yüksek düzeyde, tüm belgelerden önemli sözcükleri ayıklamak, bir dizine koymak ve ardından belirli bir sorgunun sözcükleri ile eşleşen belgeleri bulmak için dizini kullanmak için aşağı gelir. Belgelerden ve arama sorgularından sözcükler çıkarma işlemine *sözlü çözümleme*denir. Sözlü çözümleme gerçekleştiren bileşenlere *çözümleyici*denir.

 Azure Bilişsel Arama'da, [Çözümleyiciler](#AnalyzerTable) tablosunda önceden tanımlanmış dil-agnostik çözümleyiciler kümesi veya [Azure Bilişsel Arama hizmeti REST API&#41;&#40;Dil çözümleyicilerinde ](index-add-language-analyzers.md)listelenen dile özgü analizörler arasından seçim yapabilirsiniz. Ayrıca kendi özel çözümleyicileri tanımlamak için bir seçeneğiniz vardır.  

 Özel bir çözümleyici, metni dizinlenebilir ve aranabilir jetonlara dönüştürme işlemi üzerinde denetim edinmenizi sağlar. Önceden tanımlanmış tek bir belirteç filtresi, bir veya daha fazla belirteç filtresi ve bir veya daha fazla char filtresinden oluşan kullanıcı tanımlı bir yapılandırmadır. Belirteçleri belirteçleri içine metin kırma sorumludur ve belirteçleri belirteçleri tarafından yayılan değiştirin için belirteç filtreleri. Char filtreleri, giriş metnini belirteç tarafından işlenmeden önce hazırlamak için uygulanır. Örneğin, char filtresi belirli karakterlerin veya sembollerin yerini alabilir.

 Özel çözümleyiciler tarafından etkinleştirilen popüler senaryolar şunlardır:  

- Fonetik arama. Bir sözcüğün nasıl yazıldığına değil, nasıl geldiğine göre aramayı etkinleştirmek için fonetik filtre ekleyin.  

- Sözlü analizi devre dışı. Çözümlenmemiş aranabilir alanlar oluşturmak için Anahtar Kelime çözümleyicisini kullanın.  

- Hızlı önek/sonek arama. Hızlı önek eşleştirmesini etkinleştirmek için sözcüklerin öneklerini dizine Kenar N-gram belirteç filtresini ekleyin. Sonek eşleştirmesi yapmak için ters belirteç filtresi ile birleştirin.  

- Özel belirteçleştirme. Örneğin, beyaz alan delimiter olarak kullanarak cümleleri belirteçlere ayırmak için Beyaz Uzay belirteçleri kullanın  

- ASCII katlama. Arama terimlerinde ö veya ê gibi akseyni normalleştirmek için Standart ASCII katlanır filtresini ekleyin.  

  Bu sayfa desteklenen çözümleyicilerin, belirteçlerin, belirteç filtrelerinin ve char filtrelerinin bir listesini sağlar. Ayrıca, dizin tanımındaki değişikliklerin açıklamasını bir kullanım örneğiyle de bulabilirsiniz. Azure Bilişsel Arama uygulamasında yararlanılandan yararlanılandan temel teknoloji hakkında daha fazla bilgi için [Çözüm paketi özetine (Lucene)](https://lucene.apache.org/core/6_0_0/core/org/apache/lucene/codecs/lucene60/package-summary.html)bakın. Çözümleyici yapılandırmaları örnekleri için [bkz.](search-analyzers.md#examples)

## <a name="validation-rules"></a>Doğrulama kuralları  
 Çözümleyicilerin, belirteç lerin, belirteç filtrelerinin ve char filtrelerinin adları benzersiz olmalıdır ve önceden tanımlanmış çözümleyiciler, belirteçler, belirteç filtreleri veya char filtreleri ile aynı olamaz. Zaten kullanılmakta olan adlar için [Özellik Başvurusu'na](#PropertyReference) bakın.

## <a name="create-custom-analyzers"></a>Özel çözümleyiciler oluşturma
 Dizin oluşturma zamanında özel çözümleyiciler tanımlayabilirsiniz. Özel bir çözümleyici belirtmek için sözdizimi bu bölümde açıklanmıştır. [Ayrıca Azure Bilişsel Arama'da çözümleyiciekle'deki](search-analyzers.md#examples)örnek tanımları inceleyerek sözdizimine aşina olabilirsiniz.  

 Çözümleyici tanımı, belirteç sonrası işleme için bir ad, bir tür, bir veya daha fazla char filtresi, en fazla bir belirteç ve bir veya daha fazla belirteç filtresi içerir. Char filerler belirteçleştirme önce uygulanır. Belirteç filtreleri ve char filtreleri soldan sağa uygulanır.

 `tokenizer_name` Belirteç filtrelerinin `token_filter_name_1` adıdır ve `token_filter_name_2` belirteç filtrelerinin `char_filter_name_1` `char_filter_name_2` adlarıdır ve char filtrelerinin adlarıdır (geçerli değerler için [Belirteç](#Tokenizers)filtreleri, [Belirteç filtreleri](#TokenFilters) ve Char filtreleri tablolarına bakın).

Çözümleyici tanımı büyük dizinin bir parçasıdır. Dizinin geri kalanı hakkında bilgi için [Dizin API Oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index) bölümüne bakın.

```
"analyzers":(optional)[
   {
      "name":"name of analyzer",
      "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
      "charFilters":[
         "char_filter_name_1",
         "char_filter_name_2"
      ],
      "tokenizer":"tokenizer_name",
      "tokenFilters":[
         "token_filter_name_1",
         "token_filter_name_2"
      ]
   },
   {
      "name":"name of analyzer",
      "@odata.type":"#analyzer_type",
      "option1":value1,
      "option2":value2,
      ...
   }
],
"charFilters":(optional)[
   {
      "name":"char_filter_name",
      "@odata.type":"#char_filter_type",
      "option1":value1,
      "option2":value2,
      ...
   }
],
"tokenizers":(optional)[
   {
      "name":"tokenizer_name",
      "@odata.type":"#tokenizer_type",
      "option1":value1,
      "option2":value2,
      ...
   }
],
"tokenFilters":(optional)[
   {
      "name":"token_filter_name",
      "@odata.type":"#token_filter_type",
      "option1":value1,
      "option2":value2,
      ...
   }
]
```

> [!NOTE]  
>  Oluşturduğunuz özel çözümleyiciler Azure portalında açıklanmaz. Özel bir çözümleyici eklemenin tek yolu, bir dizini tanımlarken API'ye çağrı yapan koddur.  

 Dizin tanımı içinde, bu bölümü bir oluşturma dizin isteğinin gövdesindeki herhangi bir yere yerleştirebilirsiniz, ancak genellikle sonunda gider:  

```
{
  "name": "name_of_index",
  "fields": [ ],
  "suggesters": [ ],
  "scoringProfiles": [ ],
  "defaultScoringProfile": (optional) "...",
  "corsOptions": (optional) { },
  "analyzers":(optional)[ ],
  "charFilters":(optional)[ ],
  "tokenizers":(optional)[ ],
  "tokenFilters":(optional)[ ]
}
```

Yalnızca özel seçenekler ayarlıyorsanız, karakter filtreleri, belirteçleri ve belirteç filtreleri için tanımlar dizine eklenir. Varolan bir filtreyi veya belirteçleyiciyi olduğu gibi kullanmak için çözümleyici tanımında ada göre belirtin.

<a name="Testing custom analyzers"></a>

## <a name="test-custom-analyzers"></a>Özel çözümleyicileri test edin

Çözümleyicinin verilen metni belirteçlere nasıl ayırdığını görmek için [REST API'deki](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) **Test Çözümleyici işlemini** kullanabilirsiniz.

**İstek**
```
  POST https://[search service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
  Content-Type: application/json
    api-key: [admin key]

  {
     "analyzer":"my_analyzer",
     "text": "Vis-à-vis means Opposite"
  }
```
**Yanıt**
```
  {
    "tokens": [
      {
        "token": "vis_a_vis",
        "startOffset": 0,
        "endOffset": 9,
        "position": 0
      },
      {
        "token": "vis_à_vis",
        "startOffset": 0,
        "endOffset": 9,
        "position": 0
      },
      {
        "token": "means",
        "startOffset": 10,
        "endOffset": 15,
        "position": 1
      },
      {
        "token": "opposite",
        "startOffset": 16,
        "endOffset": 24,
        "position": 2
      }
    ]
  }
```

## <a name="update-custom-analyzers"></a>Özel çözümleyicileri güncelleştirme

Bir çözümleyici, bir belirteç filtresi veya char filtresi tanımlandıktan sonra değiştirilemez. Yalnızca `allowIndexDowntime` dizin güncelleştirme isteğinde bayrak doğru ayarlanmışsa, varolan bir dizine yenileri eklenebilir:

```
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true
```

Bu işlem dizininizi en az birkaç saniye çevrimdışı nasibini alır ve dizin oluşturma ve sorgu isteklerinizin başarısız olmasını sağlar. Dizin performansı ve yazma kullanılabilirliği dizin güncelleştirildikten sonra birkaç dakika veya çok büyük dizinler için daha uzun süre bozulabilir, ancak bu etkiler geçicidir ve sonunda kendi başlarına çözülebilir.

 <a name="ReferenceIndexAttributes"></a>

## <a name="analyzer-reference"></a>Çözümleyici başvurusu

Aşağıdaki tablolarda çözümleyiciler, belirteçler, belirteç filtreleri ve bir dizin tanımının char filtresi bölümü için yapılandırma özellikleri listeleilmektedir. Dizininizdeki bir çözümleyici, belirteç veya filtrenin yapısı bu özniteliklerden oluşur. Değer atama sı bilgileri için [Özellik Başvurusu'na](#PropertyReference)bakın.

### <a name="analyzers"></a>Çözümleyiciler

Çözümleyiciler için dizin öznitelikleri, önceden tanımlanmış veya özel çözümleyiciler kullanıp kullanmadığınıza bağlı olarak değişir.

#### <a name="predefined-analyzers"></a>Önceden Tanımlanmış Analizörler

|||  
|-|-|  
|Adı|Yalnızca harfler, basamaklar, boşluklar, tireler veya alt çizgiler içermelidir, yalnızca alfasayısal karakterlerle başlayıp bitirebilir ve 128 karakterle sınırlıdır.|  
|Tür|Desteklenen çözümleyiciler listesinden çözümleyici türü. Aşağıdaki [Çözümleyiciler](#AnalyzerTable) tablosundaki **analyzer_type** sütununa bakın.|  
|Seçenekler|Aşağıdaki [Çözümleyiciler](#AnalyzerTable) tablosunda listelenen önceden tanımlanmış bir çözümleyicinin geçerli seçenekleri olmalıdır.|  

#### <a name="custom-analyzers"></a>Özel Analizörler

|||  
|-|-|  
|Adı|Yalnızca harfler, basamaklar, boşluklar, tireler veya alt çizgiler içermelidir, yalnızca alfasayısal karakterlerle başlayıp bitirebilir ve 128 karakterle sınırlıdır.|  
|Tür|"#Microsoft.Azure.Search.CustomAnalyzer" olmalıdır.|  
|CharFiltreler|[Char Filtreler](#char-filters-reference) tablosunda listelenen önceden tanımlanmış char filtrelerinden birine veya dizin tanımında belirtilen özel bir char filtresine ayarlayın.|  
|Tokenizer|Gereklidir. Aşağıdaki [Tokenizers](#Tokenizers) tablosunda listelenen önceden tanımlanmış belirteçlerden birine veya dizin tanımında belirtilen özel bir belirteç olarak ayarlayın.|  
|TokenFiltreler|[Token filtreleri](#TokenFilters) tablosunda listelenen önceden tanımlanmış belirteç filtrelerinden birine veya dizin tanımında belirtilen özel bir belirteç filtresine ayarlayın.|  

> [!NOTE]
> Özel çözümleyicinizi 300 karakterden uzun belirteçler üretmeyecek şekilde yapılandırmanız gerekir. Dizin oluşturma, bu tür belirteçleri olan belgeler için başarısız olur. Bunları kırpmak veya yok saymak için sırasıyla **TruncateTokenFilter** ve **LengthTokenFilter'i** kullanın.  Başvuru için [**Belirteç filtrelerini**](#TokenFilters) kontrol edin.

<a name="CharFilter"></a>

### <a name="char-filters"></a>Char Filtreler

 Bir char filtresi, giriş metnini belirteç tarafından işlenmeden önce hazırlamak için kullanılır. Örneğin, belirli karakterleri veya sembolleri değiştirebilirler. Özel bir çözümleyicide birden çok char filtresi olabilir. Char filtreleri listelendikleri sırada çalışır.  

|||  
|-|-|  
|Adı|Yalnızca harfler, basamaklar, boşluklar, tireler veya alt çizgiler içermelidir, yalnızca alfasayısal karakterlerle başlayıp bitirebilir ve 128 karakterle sınırlıdır.|  
|Tür|Desteklenen char filtreleri listesinden Char filtre türü. Aşağıdaki [Char Filtreleri](#char-filters-reference) tablosundaki **char_filter_type** sütuna bakın.|  
|Seçenekler|Belirli bir [Char Filtreleri](#char-filters-reference) türünün geçerli seçenekleri olmalıdır.|  

### <a name="tokenizers"></a>Tokenizers

 Belirteç, sürekli metni, bir cümleyi sözcüklere bölme gibi bir belirteç dizisine böler.  

 Özel çözümleyici başına tam olarak bir belirteç belirtebilirsiniz. Birden fazla belirteç gerekir, birden çok özel çözümleyicioluşturmak ve bunları dizin şemanızda alan bazında atayabilirsiniz.  
Özel bir çözümleyici, varsayılan veya özelleştirilmiş seçenekleri olan önceden tanımlanmış bir belirteç kullanabilir.  

|||  
|-|-|  
|Adı|Yalnızca harfler, basamaklar, boşluklar, tireler veya alt çizgiler içermelidir, yalnızca alfasayısal karakterlerle başlayıp bitirebilir ve 128 karakterle sınırlıdır.|  
|Tür|Desteklenen tokenizler listesinden Tokenizer adı. Aşağıdaki [Tokenizers](#Tokenizers) tablosundaki **tokenizer_type** sütuna bakın.|  
|Seçenekler|Aşağıdaki [Tokenizers](#Tokenizers) tablosunda listelenen belirli bir belirteç türü geçerli seçenekler olmalıdır.|  

### <a name="token-filters"></a>Belirteç filtreleri

 Belirteç filtresi, bir belirteççi tarafından oluşturulan belirteçleri filtrelemek veya değiştirmek için kullanılır. Örneğin, tüm karakterleri küçük harfe dönüştüren küçük harf filtresi belirtebilirsiniz.   
Özel bir çözümleyicide birden çok belirteç filtresi olabilir. Belirteç filtreleri listelendikleri sırada çalışır.  

|||  
|-|-|  
|Adı|Yalnızca harfler, basamaklar, boşluklar, tireler veya alt çizgiler içermelidir, yalnızca alfasayısal karakterlerle başlayıp bitirebilir ve 128 karakterle sınırlıdır.|  
|Tür|Desteklenen belirteç filtreleri listesinden belirteç filtresi adı. Aşağıdaki [Token filtreleri](#TokenFilters) tablosundaki **token_filter_type** sütununa bakın.|  
|Seçenekler|Belirli bir belirteç filtresi türünün [Belirteç filtreleri](#TokenFilters) olmalıdır.|  

<a name="PropertyReference"></a>  

## <a name="property-reference"></a>Özellik başvurusu

Bu bölümde, dizininizde özel çözümleyici, belirteç, char filtresi veya belirteç filtresi tanımında belirtilen öznitelikler için geçerli değerler sağlar. Apache Lucene kullanılarak uygulanan analizörler, belirteçler ve filtreler, Lucene API belgelerine bağlantılara sahiptir.

<a name="AnalyzerTable"></a>

###  <a name="predefined-analyzers-reference"></a>Önceden Tanımlanmış Analizörler Referans

|**analyzer_name**|**analyzer_type**  <sup>1</sup>|**Açıklama ve Seçenekler**|  
|-|-|-|  
|[Anahtar kelime](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html)| (tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir) |Bir alanın tüm içeriğini tek bir belirteç olarak ele adatır. Bu, posta kodları, d'ler ve bazı ürün adları gibi veriler için yararlıdır.|  
|[Desen](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/PatternAnalyzer.html)|DesenAnalizörü|Esnek bir şekilde normal bir ifade deseni aracılığıyla metni terimlere ayırır.<br /><br /> **Seçenekler**<br /><br /> küçük harf (tür: bool) - Terimlerin küçük olup olmadığını belirler. Varsayılan değer doğrudur.<br /><br /> [desen](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html?is-external=true) (tür: string) - Belirteç ayırıcıları eşleştirmek için normal bir ifade deseni. Varsayılan değer \w+'dır.<br /><br /> [bayraklar](https://docs.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html#field_summary) (tür: dize) - Normal ifade bayrakları. Varsayılan, boş bir dizedir. İzin verilen değerler: CANON_EQ, CASE_INSENSITIVE, YORUMLAR, DOTALL, LITERAL, MULTILINE, UNICODE_CASE, UNIX_LINES<br /><br /> stopwords (türü: string array) - Stopwords listesi. Varsayılan, boş bir listedir.|  
|[Basit](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/SimpleAnalyzer.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir) |Metni harf olmayan harflerle böler ve bunları küçük harfe dönüştürür. |  
|[Standart](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html) <br />(Standard.lucene olarak da adlandırılır)|StandardAnalyzer|Standart belirteç, küçük filtre ve durdurma filtresinden oluşan standart Lucene çözümleyicisi.<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (türü: int) - Maksimum belirteç uzunluğu. Varsayılan değer 255'tir. Maksimum uzunluktan uzun belirteçler bölünür. Kullanılabilecek maksimum belirteç uzunluğu 300 karakterdir.<br /><br /> stopwords (türü: string array) - Stopwords listesi. Varsayılan, boş bir listedir.|  
|standardasciifolding.lucene|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir) |Ascii katlanır filtreli standart analizör. |  
|[Durdurmak](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/StopAnalyzer.html)|StopAnalyzer|Metni harf olmayan olarak böler, küçük harf ve stopword belirteç filtrelerini uygular.<br /><br /> **Seçenekler**<br /><br /> stopwords (türü: string array) - Stopwords listesi. Varsayılan, İngilizce için önceden tanımlanmış bir listedir. |  
|[Boşluk](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir) |Beyazuzay belirteci kullanan bir çözümleyici. 255 karakterden uzun belirteçler bölünür.|  

 <sup>1</sup> Çözümleyici Türleri her zaman "#Microsoft.Azure.Search" ile kod olarak önceden belirlenmiştir, "PatternAnalyzer" aslında "#Microsoft.Azure.Search.PatternAnalyzer" olarak belirtilir. Kısaltma için öneki kaldırdık, ancak önek kodunuzda gereklidir. 
 
analyzer_type yalnızca özelleştirilebilen çözümleyiciler için sağlanır. Anahtar kelime çözümleyicisi ile olduğu gibi seçenek yoksa, ilişkili #Microsoft.Azure.Search türü yoktur.


<a name="CharFilter"></a>

###  <a name="char-filters-reference"></a>Char Filtreler Başvuru

Aşağıdaki tabloda, Apache Lucene kullanılarak uygulanan karakter filtreleri Lucene API belgelerine bağlanır.

|**char_filter_name**|**char_filter_type** <sup>1</sup>|**Açıklama ve Seçenekler**|  
|-|-|-|
|[html_strip](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/charfilter/HTMLStripCharFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |HTML yapılarını sökmeye çalışan bir char filtresi.|  
|[eşleme](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/charfilter/MappingCharFilter.html)|MappingCharFilter|Eşlemeler seçeneğiyle tanımlanan eşlemeleri uygulayan bir char filtresi. Eşleştirme açgözlüdür (belirli bir noktada eşleşen en uzun desen kazanır). Değiştirme boş dize olmak için izin verilir.<br /><br /> **Seçenekler**<br /><br /> eşlemeler (tür: dize dizisi) - Aşağıdaki biçimdeki eşlemelerin listesi: "a=>b" ("a" karakterinin tüm oluşumları "b" karakteriyle değiştirilir). Gereklidir.|  
|[pattern_replace](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/pattern/PatternReplaceCharFilter.html)|DesenYerine CharFilter|Giriş dizesinde karakterlerin yerini alan bir char filtresi. Korumak için karakter dizilerini tanımlamak için normal bir ifade ve değiştirilecek karakterleri tanımlamak için değiştirme deseni kullanır. Örneğin, giriş metni = "aa bb aa bb", pattern="(aa)\\\s+(bb)" değiştirme="$1#$2", sonuç = "aa#bb aa#bb".<br /><br /> **Seçenekler**<br /><br /> desen (türü: string) - Gerekli.<br /><br /> değiştirme (tip: string) - Gerekli.|  

 <sup>1</sup> Char Filtre Türleri her zaman "#Microsoft.Azure.Search" ile kod olarak önceden belirlenmiştir, bu nedenle "MappingCharFilter" aslında "#Microsoft.Azure.Search.MappingCharFilter" olarak belirtilir. Tablonun genişliğini azaltmak için önek kaldırıldı, ancak lütfen kodunuza eklemeyi unutmayın. char_filter_type yalnızca özelleştirilebilen filtreler için sağlandığına dikkat edin. html_strip'da olduğu gibi seçenek yoksa, ilişkili #Microsoft.Azure.Search türü yoktur.

<a name="Tokenizers"></a>

###  <a name="tokenizers-reference"></a>Tokenizers Referans

Aşağıdaki tabloda, Apache Lucene kullanılarak uygulanan belirteçler Lucene API belgelerine bağlıdır.

|**tokenizer_name**|**tokenizer_type** <sup>1</sup>|**Açıklama ve Seçenekler**|  
|-|-|-|  
|[Klasik](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/standard/ClassicTokenizer.html)|ClassicTokenizer|Avrupa dilli belgelerin çoğu işlemeye uygun dilbilgisi tabanlı belirteç.<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (türü: int) - Maksimum belirteç uzunluğu. Varsayılan: 255, maksimum: 300. Maksimum uzunluktan uzun belirteçler bölünür.|  
|[edgeNGram](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenizer.html)|KenarNGramTokenizer|Bir kenardan giriyi verilen boyut(lar)n gramına dönüştürür.<br /><br /> **Seçenekler**<br /><br /> minGram (türü: int) - Varsayılan: 1, maksimum: 300.<br /><br /> maxGram (türü: int) - Varsayılan: 2, maksimum: 300. minGram'dan daha büyük olmalı.<br /><br /> tokenChars (türü: string array) - Belirteçleri tutmak için karakter sınıfları. İzin verilen değerler: <br />"harf", "basamak", "boşluk", "noktalama", "sembol". Boş bir dizi varsayılan - tüm karakterleri tutar. |  
|[keyword_v2](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html)|Anahtar KelimeTokenizerV2|Tüm girişi tek bir belirteç olarak yayır.<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (türü: int) - Maksimum belirteç uzunluğu. Varsayılan: 256, maksimum: 300. Maksimum uzunluktan uzun belirteçler bölünür.|  
|[Mektup](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LetterTokenizer.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Metni harf olmayan harflerle böler. 255 karakterden uzun belirteçler bölünür.|  
|[Küçük](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LowerCaseTokenizer.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Metni harf olmayan harflerle böler ve bunları küçük harfe dönüştürür. 255 karakterden uzun belirteçler bölünür.|  
| microsoft_language_tokenizer| MicrosoftLanguageTokenizer| Dile özgü kuralları kullanarak metni böler.<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (türü: int) - Maksimum belirteç uzunluğu, varsayılan: 255, maksimum: 300. Maksimum uzunluktan uzun belirteçler bölünür. 300 karakterden uzun belirteçler önce 300 uzunluğundaki belirteçlere bölünür ve ardından bu belirteçlerin her biri maxTokenLength kümesine göre bölünür.<br /><br />isSearchTokenizer (türü: bool) - Arama belirteci olarak kullanılırsa doğru ayarlayın, dizin oluşturma belirteçleyici olarak kullanıldığında false olarak ayarlanır. <br /><br /> dil (tür: string) - Kullanılacak dil, varsayılan "İngilizce". İzin verilen değerler şunlardır:<br />"bangla", "bulgarca", "katalanca", "çinceBasitleştirilmiş", "chineseTraditional", "croatian", "czech", "danish", "felemenkçe", "İngilizce", "Fransızca", "almanca", "yunanca", "gujarati", "hintçe", "İzlandaca", "endonezyaca", "İtalyanca", "japonca", "kannada", "kannada", " korece", "malay", "malayalam", "marathi", "norwegianBokmaal", "lehçe", "portekizce", "portekizceBrezilya", "punjabi", "romence", "rusça", "sırpçaKiril", "sırpça", "sloven", "İspanyolca", "İsveççe", "tamil", "telugu", "thai", " Ukraynaca", "urduca", "vietnamca" |
| microsoft_language_stemming_tokenizer | MicrosoftLanguageStemmingTokenizer| Dile özgü kuralları kullanarak metni böler ve sözcükleri temel biçimlerine küçültür<br /><br /> **Seçenekler**<br /><br />maxTokenLength (türü: int) - Maksimum belirteç uzunluğu, varsayılan: 255, maksimum: 300. Maksimum uzunluktan uzun belirteçler bölünür. 300 karakterden uzun belirteçler önce 300 uzunluğundaki belirteçlere bölünür ve ardından bu belirteçlerin her biri maxTokenLength kümesine göre bölünür.<br /><br /> isSearchTokenizer (türü: bool) - Arama belirteci olarak kullanılırsa doğru ayarlayın, dizin oluşturma belirteçleyici olarak kullanıldığında false olarak ayarlanır.<br /><br /> dil (tür: string) - Kullanılacak dil, varsayılan "İngilizce". İzin verilen değerler şunlardır:<br />"arapça", "bangla", "bulgarca", "katalanca", "hırvatça", "çekçe", "danca", "felemenkçe", "İngilizce", "estonya", "fince", "fransızca", "almanca", "yunanca", "gujarati", "İbranice", "hintçe", "macarca", "İzlandaca", "endonezyaca", "İtalyanca", "kannada", " letonyaca", "litvanyaca", "malay", "malayalam", "marathi", "norwegianBokmaal", "lehçe", "portekizce", "portekizce", "punjabi", "romence", "rusça", "serbianCyrillic", "serbianLatince", "slovak", "sloven", "İspanyolca", "İsveççe", "İsveççe", " tamil", "telugu", "türk", "ukraynaca", "urdu" |
|[nGram](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ngram/NGramTokenizer.html)|NGramTokenizer|Verilen boyutun (lar) n-gram içine giriş tokenizes.<br /><br /> **Seçenekler**<br /><br /> minGram (türü: int) - Varsayılan: 1, maksimum: 300.<br /><br /> maxGram (türü: int) - Varsayılan: 2, maksimum: 300. minGram'dan daha büyük olmalı. <br /><br /> tokenChars (türü: string array) - Belirteçleri tutmak için karakter sınıfları. İzin verilen değerler: "harf", "basamak", "boşluk", "noktalama", "sembol". Boş bir dizi varsayılan - tüm karakterleri tutar. |  
|[path_hierarchy_v2](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/path/PathHierarchyTokenizer.html)|Yol HiyerarşisiTokenizerV2|Yol benzeri hiyerarşiler için tokenizer.<br /><br /> **Seçenekler**<br /><br /> delimiter (türü: string) - Varsayılan: '/.<br /><br /> değiştirme (tür: string) - Eğer ayarlanırsa, sınır dışı layıcı karakteri değiştirir. Varsayılan değer delimiter değeri ile aynıdır.<br /><br /> maxTokenLength (türü: int) - Maksimum belirteç uzunluğu. Varsayılan: 300, maksimum: 300. MaksimumTokenLength'dan daha uzun yollar yoksayılır.<br /><br /> ters (türü: bool) - Doğruysa, ters sırada belirteç oluşturur. Varsayılan: false.<br /><br /> atlamak (türü: bool) - Atlamak için ilk belirteçleri. Varsayılan değer, 0'dur.|  
|[Desen](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/pattern/PatternTokenizer.html)|DesenTokenizer|Bu belirteçleri farklı belirteçleri oluşturmak için regex desen eşleştirme kullanır.<br /><br /> **Seçenekler**<br /><br /> [desen](https://docs.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html) (tür: string) - Normal ifade deseni. Varsayılan değer \W+'dır. <br /><br /> [bayraklar](https://docs.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html#field_summary) (tür: dize) - Normal ifade bayrakları. Varsayılan, boş bir dizedir. İzin verilen değerler: CANON_EQ, CASE_INSENSITIVE, YORUMLAR, DOTALL, LITERAL, MULTILINE, UNICODE_CASE, UNIX_LINES<br /><br /> grup (türü: int) - Hangi grup belirteçleri içine ayıklamak için. Varsayılan değer -1 (split).|
|[standard_v2](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardTokenizer.html)|StandartTokenizerV2|[Unicode Metin Bölümlendirme kurallarını](https://unicode.org/reports/tr29/)izleyerek metni tatili.<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (türü: int) - Maksimum belirteç uzunluğu. Varsayılan: 255, maksimum: 300. Maksimum uzunluktan uzun belirteçler bölünür.|  
|[uax_url_email](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/standard/UAX29URLEmailTokenizer.html)|UaxUrlEmailTokenizer|Url'leri ve e-postaları tek bir belirteç olarak belirtin.<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (türü: int) - Maksimum belirteç uzunluğu. Varsayılan: 255, maksimum: 300. Maksimum uzunluktan uzun belirteçler bölünür.|  
|[Boşluk](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/WhitespaceTokenizer.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir) |Metni beyazboşlukta böler. 255 karakterden uzun belirteçler bölünür.|  

 <sup>1</sup> Tokenizer Türleri her zaman "#Microsoft.Azure.Search" ile kod olarak önceden belirlenmiştir, "ClassicTokenizer" aslında "#Microsoft.Azure.Search.ClassicTokenizer" olarak belirtilir. Tablonun genişliğini azaltmak için önek kaldırıldı, ancak lütfen kodunuza eklemeyi unutmayın. tokenizer_type yalnızca özelleştirilebilen belirteçler için sağlandığına dikkat edin. Mektup belirtecinde olduğu gibi seçenek yoksa, ilişkili #Microsoft.Azure.Search türü yoktur.

<a name="TokenFilters"></a>

###  <a name="token-filters-reference"></a>Belirteç FiltrelerI Başvuru

Aşağıdaki tabloda, Apache Lucene kullanılarak uygulanan belirteç filtreleri Lucene API belgelerine bağlanır.

|**token_filter_name**|**token_filter_type** <sup>1</sup>|**Açıklama ve Seçenekler**|  
|-|-|-|  
|[arabic_normalization](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ar/ArabicNormalizationFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Ortografiyi normalleştirmek için Arapça normalleştiriciyi uygulayan bir belirteç filtresi.|  
|[Kesme işareti](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/tr/ApostropheFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Bir keseli sıçandan sonra tüm karakterleri şeritler (apostrophe kendisi dahil). |  
|[asciifolding](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html)|AsciiFoldingTokenFilter|Alfabetik, sayısal ve sembolik Unicode karakterlerini ilk 127 ASCII karakterinde ("Temel Latince" Unicode bloğu) varsa ASCII eşdeğerlerine dönüştürür.<br /><br /> **Seçenekler**<br /><br /> preserveOriginal (türü: bool) - Doğruysa, orijinal belirteç tutulur. Varsayılan değer false.|  
|[cjk_bigram](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/cjk/CJKBigramFilter.html)|CjkBigramTokenFiltre|StandardTokenizer'dan oluşturulan CJK terimlerinin bigrams oluşturur.<br /><br /> **Seçenekler**<br /><br /> ignoreScripts (türü: string dizisi) - Komut dosyaları yoksaymak için. İzin verilen değerler şunlardır: "han", "hiragana", "katakana", "hangul". Varsayılan, boş bir listedir.<br /><br /> outputUnigrams (türü: bool) - Her zaman hem unigrams ve bigrams çıkış istiyorsanız doğru ayarlayın. Varsayılan değer false.|  
|[cjk_width](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/cjk/CJKWidthFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |CJK genişlik farklarını normalleştirir. Tam genişlikTEKI ASCII varyantlarını eşdeğer temel latince ve yarım genişlikteki Katakana varyantlarını eşdeğer kana'ya katlar. |  
|[Klasik](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/standard/ClassicFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |İngilizce iyelikleri ve noktalarını kısaltmalardan kaldırır. |  
|[common_grams](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/commongrams/CommonGramsFilter.html)|CommonGramTokenFilter|Dizin oluşturma sırasında sık sık oluşan terimler için bigrams oluştur. Tek terimler hala bigrams overlaid ile, çok endeksli.<br /><br /> **Seçenekler**<br /><br /> commonWords (tür: string array) - Ortak sözcükler kümesi. Varsayılan, boş bir listedir. Gereklidir.<br /><br /> ignoreCase (türü: bool) - Doğruysa, eşleştirme büyük/küçük harf duyarsızdır. Varsayılan değer false.<br /><br /> queryMode (türü: bool) - Bigrams oluşturur sonra ortak bir kelime ardından ortak sözcükleri ve tek terimleri kaldırır. Varsayılan değer false.|  
|[dictionary_decompounder](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/compound/DictionaryCompoundWordTokenFilter.html)|SözlükDecompounderTokenFilter|Birçok Germen disinde bulunan bileşik sözcükleri ayrıştırır.<br /><br /> **Seçenekler**<br /><br /> wordList (türü: string array) - Karşı maç için kelimelerin listesi. Varsayılan, boş bir listedir. Gereklidir.<br /><br /> minWordSize (türü: int) - Sadece bu işlem daha uzun kelimeler. Varsayılan değer 5'tir.<br /><br /> minSubwordSize (türü: int) - Sadece alt sözcükler bu çıktı daha uzun. Varsayılan değer 2'dir.<br /><br /> maxSubwordSize (türü: int) - Sadece alt sözcükler bu çıktı daha kısa. Varsayılan değer 15’tir.<br /><br /> onlyLongestMatch (türü: bool) - Çıktı için yalnızca en uzun eşleşen alt sözcüğü ekleyin. Varsayılan değer false.|  
|[edgeNGram_v2](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenFilter.html)|KenarNGramTokenFilterV2|Bir giriş belirtecinin önünden veya arkasından başlayan verilen boyutun (ler) n-gramını oluşturur.<br /><br /> **Seçenekler**<br /><br /> minGram (türü: int) - Varsayılan: 1, maksimum: 300.<br /><br /> maxGram (türü: int) - Varsayılan: 2, maksimum 300. minGram'dan daha büyük olmalı.<br /><br /> yan (tip: string) - N-gram girişinin hangi tarafından oluşturulması gerektiğini belirtir. İzin verilen değerler: "ön", "arka" |  
|[elision](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/util/ElisionFilter.html)|ElisionTokenFiltre|Elisions kaldırır. Örneğin, "l'avion" (uçak) "avion" (düzlem) dönüştürülür.<br /><br /> **Seçenekler**<br /><br /> makaleler (türü: string array) - Kaldırılacak bir makale kümesi. Varsayılan, boş bir listedir. Ayarlanmış makalelerin listesi yoksa, varsayılan olarak tüm Fransızca makaleler kaldırılır.|  
|[german_normalization](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/de/GermanNormalizationFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |[Alman2 kartopu algoritmasının](https://snowballstem.org/algorithms/german2/stemmer.html) buluşsallarına göre Almanca karakterleri normalleştirir.|  
|[hindi_normalization](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/hi/HindiNormalizationFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Yazım varyasyonlarında bazı farklılıkları kaldırmak için Hintçe metni normalleştirir. |  
|[indic_normalization](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/in/IndicNormalizationFilter.html)|IndicNormalizationTokenFilter|Hint dillerinde metnin Unicode temsilini normalleştirir.
|[Tutmak](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/KeepWordFilter.html)|KeepTokenFilter|Yalnızca belirtilen sözcük listesinde bulunan metiniçeren belirteçleri tutan bir belirteç filtresi.<br /><br /> **Seçenekler**<br /><br /> keepWords (tür: string array) - Tutulması gereken kelimelerin listesi. Varsayılan, boş bir listedir. Gereklidir.<br /><br /> keepWordsCase (türü: bool) - Eğer doğruysa, küçük harf tüm kelimeler önce. Varsayılan değer false.|  
|[keyword_marker](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/KeywordMarkerFilter.html)|Anahtar KelimeMarkerTokenFilter|Terimleri anahtar kelime olarak işaretler.<br /><br /> **Seçenekler**<br /><br /> anahtar kelimeler (tür: string array) - Anahtar kelime olarak işaretlenen sözcüklerin listesi. Varsayılan, boş bir listedir. Gereklidir.<br /><br /> ignoreCase (türü: bool) - Eğer doğruysa, küçük harf tüm kelimeler ilk. Varsayılan değer false.|  
|[keyword_repeat](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/KeywordRepeatFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Her gelen belirteci anahtar kelime olarak iki kez ve anahtar kelime olmayan olarak bir kez yakar. |  
|[kstem](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/en/KStemFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |İngilizce için yüksek performanslı kstem filtresi. |  
|[Uzun -luğu](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/LengthFilter.html)|LengthTokenFilter|Çok uzun veya çok kısa sözcükleri kaldırır.<br /><br /> **Seçenekler**<br /><br /> min (türü: int) - Minimum sayı. Varsayılan: 0, maksimum: 300.<br /><br /> max (türü: int) - Maksimum sayı. Varsayılan: 300, maksimum: 300.|  
|[Sınırı](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/LimitTokenCountFilter.html)|Microsoft.Azure.Search.LimitTokenFiltresi|Dizinleme sırasında belirteçlerin sayısını sınırlar.<br /><br /> **Seçenekler**<br /><br /> maxTokenCount (türü: int) - Üretecek en yüksek belirteç sayısı. Varsayılan değer 1'dir.<br /><br /> consumeAllTokens (türü: bool) - MaxTokenCount ulaşılsa bile girişteki tüm belirteçlerin tüketilmesi gerekip gerekmediği. Varsayılan değer false.|  
|[Küçük](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Belirteç metnini küçük harfe normalleştirir. |  
|[nGram_v2](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ngram/NGramTokenFilter.html)|NGramTokenFilterV2|Verilen boyutun (lar) n-gramını oluşturur.<br /><br /> **Seçenekler**<br /><br /> minGram (türü: int) - Varsayılan: 1, maksimum: 300.<br /><br /> maxGram (türü: int) - Varsayılan: 2, maksimum 300. minGram'dan daha büyük olmalı.|  
|[pattern_capture](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/pattern/PatternCaptureGroupTokenFilter.html)|DesenYakalamaTokenFiltre|Birden çok belirteç, bir veya daha fazla desen her yakalama grubu için bir yatmak için Java regexes kullanır.<br /><br /> **Seçenekler**<br /><br /> desenler (tür: dize dizisi) - Her belirteç ile eşleşen desenler listesi. Gereklidir.<br /><br /> preserveOriginal (türü: bool) - Desenlerden biri eşleşse bile orijinal belirteci döndürmek için doğru olarak ayarlayın, varsayılan: true |  
|[pattern_replace](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/pattern/PatternReplaceFilter.html)|DesenYerineTokenFiltre|Akıştaki her belirteç için bir desen uygulayan ve eşleşme oluşumlarını belirtilen değiştirme dizesiyle değiştiren bir belirteç filtresi.<br /><br /> **Seçenekler**<br /><br /> desen (türü: string) - Gerekli.<br /><br /> değiştirme (tip: string) - Gerekli.|  
|[persian_normalization](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/fa/PersianNormalizationFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir) |Farsça için normalleştirme uygular. |  
|[Fonetik](https://lucene.apache.org/core/6_6_1/analyzers-phonetic/org/apache/lucene/analysis/phonetic/package-tree.html)|FonetikTokenFiltresi|Fonetik eşleşmeler için belirteçler oluşturun.<br /><br /> **Seçenekler**<br /><br /> kodlayıcı (türü: string) - Kullanılacak fonetik kodlayıcı. İzin verilen değerler şunlardır: "metafon", "doubleMetaphone", "soundex", "rafineSoundex", "caverphone1", "caverphone2", "kolonya", "nysiis", "koelnerPhonetik", "haasePhonetik", "beiderMorse". Varsayılan: "metafon". Varsayılan metafon.<br /><br /> Daha fazla bilgi için [kodlayıcıya](https://commons.apache.org/proper/commons-codec/archives/1.10/apidocs/org/apache/commons/codec/language/package-frame.html) bakın.<br /><br /> replace (türü: bool) - Kodlanmış belirteçler orijinal belirteçleri değiştirmeliyse doğrudur, eşanlamlı olarak eklenmelidirse yanlış. Varsayılan değer doğrudur.|  
|[porter_stem](https://lucene.apache.org/core/6_6_1/analyzers-common/org/tartarus/snowball/ext/PorterStemmer.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |[Porter kökleme algoritmasına](https://tartarus.org/~martin/PorterStemmer/)göre belirteç akışını dönüştürür. |  
|[Ters](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/reverse/ReverseStringFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Belirteç dizesini tersine çevirir. |  
|[scandinavian_normalization](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/ScandinavianNormalizationFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Değiştirilebilir İskandinav karakterlerinin kullanımını normalleştirir. |  
|[scandinavian_folding](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/ScandinavianFoldingFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Folds İskandinav karakterleri ååäæÄæ->a ve öÖøØ->o. Ayrıca çift sesli harf kullanımına karşı ayrımcılık aa, ae, ao, oe ve oo, sadece bir ilk bırakarak. |  
|[Shingle](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/shingle/ShingleFilter.html)|ShingleTokenFiltre|Tek bir belirteç olarak belirteçlerin birleşimlerini oluşturur.<br /><br /> **Seçenekler**<br /><br /> maxShingleSize (türü: int) - Varsayılan 2.<br /><br /> minShingleSize (türü: int) - Varsayılan 2.<br /><br /> outputUnigrams (türü: bool) - doğruysa, çıkış akışı giriş belirteçleri (unigrams) yanı sıra zona içerir. Varsayılan değer doğrudur.<br /><br /> outputUnigramsIfNoShingles (tip: bool) - Doğruysa, outputUnigrams davranışını geçersiz kılın==zona olmadığı zamanlar için yanlış. Varsayılan değer false.<br /><br /> tokenSeparator (türü: string) - Bir shingle oluşturmak için bitişik belirteçleri birleştirerek kullanılacak dize. Varsayılan değer " "dir.<br /><br /> filterToken (türü: string) - Belirteç bulunmayan her pozisyon için eklenecek dize. Varsayılan "_" olur.|  
|[Kartopu](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/snowball/SnowballFilter.html)|KartopuTokenFiltresi|Kartopu Belirteç Filtresi.<br /><br /> **Seçenekler**<br /><br /> dil (tip: string) - İzin verilen değerler şunlardır: "ermenice", "bask", "katalanca", "danca", "felemenkçe", "İngilizce", "fince", "fransızca", "almanca", "almanca2", "macarca", "İtalyanca", "kp", "lovins", "norveççe", "porter", "portekizce", "romence", "romence", " Rusça", "İspanyolca", "İsveççe", "Türkçe"|  
|[sorani_normalization](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ckb/SoraniNormalizationFilter.html)|SoraniNormalizationTokenFilter|Sorani metninin Unicode temsilini normalleştirir.<br /><br /> **Seçenekler**<br /><br /> Yok.|  
|Stemmer|KökmerTokenFiltresi|Dile özgü kökleme filtresi.<br /><br /> **Seçenekler**<br /><br /> dil (tür: string) - İzin verilen değerler şunlardır: <br /> -   ["arapça"](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ar/ArabicStemmer.html)<br />-   ["ermeni"](https://snowballstem.org/algorithms/armenian/stemmer.html)<br />-   ["Bask"](https://snowballstem.org/algorithms/basque/stemmer.html)<br />-   ["brezilyalı"](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/br/BrazilianStemmer.html)<br />- "bulgar"<br />-   ["Katalanca"](https://snowballstem.org/algorithms/catalan/stemmer.html)<br />-   ["çek"](https://portal.acm.org/citation.cfm?id=1598600)<br />-   ["danca"](https://snowballstem.org/algorithms/danish/stemmer.html)<br />-   ["hollandaca"](https://snowballstem.org/algorithms/dutch/stemmer.html)<br />-   ["dutchKp"](https://snowballstem.org/algorithms/kraaij_pohlmann/stemmer.html)<br />-   ["İngilizce"](https://snowballstem.org/algorithms/porter/stemmer.html)<br />-   ["lightİngilizce"](https://ciir.cs.umass.edu/pubfiles/ir-35.pdf)<br />-   ["minimalİngilizce"](https://www.researchgate.net/publication/220433848_How_effective_is_suffixing)<br />-   ["possessiveEnglish"](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/en/EnglishPossessiveFilter.html)<br />-   ["hamal2"](https://snowballstem.org/algorithms/english/stemmer.html)<br />-   ["lovins"](https://snowballstem.org/algorithms/lovins/stemmer.html)<br />-   ["fince"](https://snowballstem.org/algorithms/finnish/stemmer.html)<br />- "lightFinnish"<br />-   ["Fransızca"](https://snowballstem.org/algorithms/french/stemmer.html)<br />-   ["lightFrench"](https://dl.acm.org/citation.cfm?id=1141523)<br />-   ["minimalFransızca"](https://dl.acm.org/citation.cfm?id=318984)<br />- "galiçyaca"<br />- "minimalGaliçyaca"<br />-   ["Almanca"](https://snowballstem.org/algorithms/german/stemmer.html)<br />-   ["german2"](https://snowballstem.org/algorithms/german2/stemmer.html)<br />-   ["lightGerman"](https://dl.acm.org/citation.cfm?id=1141523)<br />- "minimalAlmanca"<br />-   ["yunanca"](https://sais.se/mthprize/2007/ntais2007.pdf)<br />- "hintçe"<br />-   ["macar"](https://snowballstem.org/algorithms/hungarian/stemmer.html)<br />-   ["lightMacar"](https://dl.acm.org/citation.cfm?id=1141523&dl=ACM&coll=DL&CFID=179095584&CFTOKEN=80067181)<br />-   ["endonezyalı"](https://www.illc.uva.nl/Publications/ResearchReports/MoL-2003-02.text.pdf)<br />-   ["İrlandalı"](https://snowballstem.org/otherapps/oregan/)<br />-   ["İtalyanca"](https://snowballstem.org/algorithms/italian/stemmer.html)<br />-   ["lightItalian"](https://www.ercim.eu/publication/ws-proceedings/CLEF2/savoy.pdf)<br />-   ["sorani"](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ckb/SoraniStemmer.html)<br />-   ["letonya"](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/lv/LatvianStemmer.html)<br />-   ["norveççe"](https://snowballstem.org/algorithms/norwegian/stemmer.html)<br />-   ["lightNorwegian"](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/no/NorwegianLightStemmer.html)<br />-   ["minimalNorveç"](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/no/NorwegianMinimalStemmer.html)<br />-   ["lightNynorsk"](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/no/NorwegianLightStemmer.html)<br />-   ["minimalNynorsk"](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/no/NorwegianMinimalStemmer.html)<br />-   ["portekizce"](https://snowballstem.org/algorithms/portuguese/stemmer.html)<br />-   ["lightPortuguese"](https://dl.acm.org/citation.cfm?id=1141523&dl=ACM&coll=DL&CFID=179095584&CFTOKEN=80067181)<br />-   ["minimalPortekizce"](https://www.inf.ufrgs.br/~buriol/papers/Orengo_CLEF07.pdf)<br />-   ["portugueseRslp"](https://www.inf.ufrgs.br//~viviane/rslp/index.htm)<br />-   ["romen"](https://snowballstem.org/otherapps/romanian/)<br />-   ["Rusça"](https://snowballstem.org/algorithms/russian/stemmer.html)<br />-   ["lightRussian"](https://doc.rero.ch/lm.php?url=1000%2C43%2C4%2C20091209094227-CA%2FDolamic_Ljiljana_-_Indexing_and_Searching_Strategies_for_the_Russian_20091209.pdf)<br />-   ["İspanyolca"](https://snowballstem.org/algorithms/spanish/stemmer.html)<br />-   ["lightSpanish"](https://www.ercim.eu/publication/ws-proceedings/CLEF2/savoy.pdf)<br />-   ["İsveççe"](https://snowballstem.org/algorithms/swedish/stemmer.html)<br />- "lightSwedish"<br />-   ["türk"](https://snowballstem.org/algorithms/turkish/stemmer.html)|  
|[stemmer_override](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/StemmerOverrideFilter.html)|Kök MerdanOverrideTokenFilter|Herhangi bir sözlük-Stemmed terimleri, zincirin köksültini engelleyen anahtar kelimeler olarak işaretlenir. Herhangi bir kökleme filtresiönce yerleştirilmelidir.<br /><br /> **Seçenekler**<br /><br /> kurallar (tür: dize dizisi) - Aşağıdaki biçimde "word => stem" örneğin "ran => çalıştır" kurallarını saplama. Varsayılan, boş bir listedir.  Gereklidir.|  
|[stopwords](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/StopFilter.html)|StopwordsTokenFilter|Belirteç akışındaki sözcükleri durdurur. Varsayılan olarak, filtre İngilizce için önceden tanımlanmış bir stop word listesi kullanır.<br /><br /> **Seçenekler**<br /><br /> stopwords (türü: string array) - Stopwords listesi. StopwordsList belirtilirse belirtilemez.<br /><br /> stopwordsList (türü: string) - Stopwords önceden tanımlanmış bir liste. Stopwords belirtilirse belirtilemez. İzin verilen değerler şunlardır:"arapça", "ermenice", "bask", "brezilyaca", "bulgarca", "katalanca", "çek", "danca", "felce", "İngilizce", "fince", "fransızca", "galiçyaca", "almanca", "yunanca", "", "macarca", "endonezyaca", "İrlandalı", "İtalyanca", "letonyaca", " ", "norveççe", "farsça", "portekizce", "romence", "rusça", "sorani", "İspanyolca", "İsveççe", "tay", "türkçe", varsayılan: "İngilizce". Stopwords belirtilirse belirtilemez. <br /><br /> ignoreCase (türü: bool) - Doğruysa, tüm sözcükler önce küçük harfle olur. Varsayılan değer false.<br /><br /> removeTrailing (türü: bool) - Doğruysa, bir stop kelimesi yse son arama terimini yoksayın. Varsayılan değer doğrudur.
|[Eşanlamlı](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/synonym/SynonymFilter.html)|Eş anlamlıtokenfiltre|Bir belirteç akışındaki tek veya çok sözcük eşanlamlısı yla eşleşir.<br /><br /> **Seçenekler**<br /><br /> eşanlamlılar (tür: dize dizisi) - Gerekli. Aşağıdaki iki biçimden birinde eşanlamlılistesi:<br /><br /> -inanılmaz, inanılmaz, muhteşem => inanılmaz - => sembolünün sol tarafındaki tüm terimler sağ tarafındaki tüm terimlerle değiştirilir.<br /><br /> -inanılmaz, inanılmaz, inanılmaz, inanılmaz - eşdeğer kelimelerin virgülayrılmış bir listesi. Bu listenin yorumlandığını değiştirmek için genişletme seçeneğini ayarlayın.<br /><br /> ignoreCase (türü: bool) - Eşleştirme için büyük/küçük harf girişi katlar. Varsayılan değer false.<br /><br /> genişletmek (türü: bool) - Doğruysa, eşanlamlılar listesindeki tüm sözcükler (if => gösterimi kullanılmaz) eşlemleri birbirine göre yapılır. <br />Aşağıdaki liste: inanılmaz, inanılmaz, muhteşem, inanılmaz eşdeğerdir: inanılmaz, inanılmaz, muhteşem, inanılmaz => inanılmaz, inanılmaz, muhteşem, inanılmaz<br /><br />- Eğer yanlış, aşağıdaki liste: inanılmaz, inanılmaz, muhteşem, şaşırtıcı eşdeğerdir: inanılmaz, inanılmaz, inanılmaz, inanılmaz , inanılmaz => inanılmaz.|  
|[Döşeme](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/TrimFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Belirteçlerden beyaz boşluğu niçin önde ve takip eden kırpar. |  
|[Truncate](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/TruncateTokenFilter.html)|BulaletFiltresi|Terimleri belirli bir uzunluğa doğru truncates.<br /><br /> **Seçenekler**<br /><br /> uzunluk (tür: int) - Varsayılan: 300, maksimum: 300. Gereklidir.|  
|[Benzer -siz](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/RemoveDuplicatesTokenFilter.html)|BenzersizTokenFiltresi|Belirteçleri önceki belirteçle aynı metinle filtreler.<br /><br /> **Seçenekler**<br /><br /> onlyOnSamePosition (türü: bool) - Ayarlanırsa, yinelemeleri yalnızca aynı konumda kaldırın. Varsayılan değer doğrudur.|  
|[Büyük](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/UpperCaseFilter.html)|(tür yalnızca seçenekler kullanılabilir olduğunda geçerlidir)  |Büyük harfe göre belirteç metnini normalleştirir. |  
|[word_delimiter](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/miscellaneous/WordDelimiterFilter.html)|WordDelimiterTokenFiltre|Sözcükleri alt sözcüklere böler ve alt sözcük gruplarında isteğe bağlı dönüşümler gerçekleştirir.<br /><br /> **Seçenekler**<br /><br /> generateWordParts (tür: bool) - Sözcüklerin bazı bölümlerinin oluşturulmasına neden olur, örneğin "AzureSearch" "Azure" "Arama" olur. Varsayılan değer doğrudur.<br /><br /> generateNumberParts (türü: bool) - Sayı alt sözcüklerinin oluşturulmasına neden olur. Varsayılan değer doğrudur.<br /><br /> catenateWords (tür: bool) - "Azure-Arama" "Azure Arama" gibi sözcük parçalarının maksimum çalışır şekilde çalıştırılmasına neden olur. Varsayılan değer false.<br /><br /> catenateNumbers (türü: bool) - Örneğin "1-2" "12" olur, catenated edilecek sayı parçalarının maksimum çalışır neden olur. Varsayılan değer false.<br /><br /> catenateAll (tür: bool) - Tüm alt sözcük parçalarının katenated edilmesine neden olur, örneğin "Azure-Search-1" "AzureSearch1" olur. Varsayılan değer false.<br /><br /> splitOnCaseChange (türü: bool) - Doğruysa, "AzureSearch" "Azure" "Arama" olur gibi, caseChange'deki sözcükleri böler. Varsayılan değer doğrudur.<br /><br /> preserveOriginal - Orijinal sözcüklerin korunmasına ve alt sözcük listesine eklenmesine neden olur. Varsayılan değer false.<br /><br /> splitOnNumerics (türü: bool) - Doğruysa, sayılarda bölünür, örneğin "Azure1Search" "Azure" "1" "Arama" olur. Varsayılan değer doğrudur.<br /><br /> stemEnglishPossessive (türü: bool) - Her alt kelime için kaldırılacak "'s" izleyerek neden olur. Varsayılan değer doğrudur.<br /><br /> protectedWords (türü: string dizisi) - Belirteçleri sınırlı olmaktan korumak için. Varsayılan, boş bir listedir.|  

 <sup>1</sup> Belirteç Filtresi Türleri her zaman "#Microsoft.Azure.Search" ile kod olarak önceden belirlenmiştir, "ArabicNormalizationTokenFilter" aslında "#Microsoft.Azure.Search.ArabicNormalizationTokenFilter" olarak belirtilir.  Tablonun genişliğini azaltmak için önek kaldırıldı, ancak lütfen kodunuza eklemeyi unutmayın.  


## <a name="see-also"></a>Ayrıca bkz.  
 [Azure Bilişsel Arama REST API'leri](https://docs.microsoft.com/rest/api/searchservice/)   
 [Azure Bilişsel Arama > Örneklerinde Çözümleyiciler](search-analyzers.md#examples)    
 [Azure Bilişsel Arama REST API &#40;Dizin oluşturma&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)  
