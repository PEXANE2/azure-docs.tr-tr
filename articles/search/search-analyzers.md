---
title: Dilve metin işleme için çözümleyiciler
titleSuffix: Azure Cognitive Search
description: Varsayılan standart Lucene'yi özel, önceden tanımlanmış veya dile özgü alternatiflerle değiştirmek için bir dizindeki aranabilir metin alanlarına çözümleyiciler atayın.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/10/2019
ms.openlocfilehash: 2e4a6ab8825982969ffa4654c2418f7a9d168d2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75460725"
---
# <a name="analyzers-for-text-processing-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da metin işleme için çözümleyiciler

*Çözümleyici,* sorgu dizelerinde ve dizinlenmiş belgelerde metni işlemekten sorumlu [tam metin arama motorunun](search-lucene-query-architecture.md) bir bileşenidir. Farklı çözümleyiciler senaryoya bağlı olarak metni farklı şekillerde manipüle emzler. Dil çözümleyicileri metni arama kalitesini artırmak için dilkurallarını kullanarak işlerken, diğer çözümleyiciler karakterleri küçük harfe dönüştürme gibi daha temel görevleri gerçekleştirirler. 

Dil çözümleyicileri en sık kullanılan dil çözümleyicileridir ve Azure Bilişsel Arama dizinindeki her aranabilir alana atanan varsayılan dil çözümleyicisi vardır. Aşağıdaki dil dönüşümleri metin çözümlemesi sırasında tipiktir:

+ Gerekli olmayan sözcükler (stopwords) ve noktalama işaretleri kaldırılır.
+ Tümcecikler ve tireli sözcükler bileşen bölümlerine ayrılır.
+ Büyük harfli sözcükler küçük harfle ifade edilir.
+ Sözcükler, zamanne bakılmaksızın bir eşleşme nin bulunabilmesi için kök formlara indirgenir.

Dil çözümleyicileri, metin girdilerini bilgi depolama ve alma için verimli olan ilkel veya kök formlara dönüştürür. Dönüşüm dizin oluşturma sırasında, dizin oluşturulur ve dizin okunduğunda arama sırasında tekrar oluşur. Her iki işlem için de aynı çözümleyiciyi kullanırsanız beklediğiniz arama sonuçlarını alma olasılığınız daha yüksektir.

## <a name="default-analyzer"></a>Varsayılan çözümleyici  

Azure Bilişsel Arama varsayılan olarak [Apache Lucene Standart çözümleyicisini (standart lucene)](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html) kullanır ve [metni "Unicode Metin Segmentasyonu"](https://unicode.org/reports/tr29/) kurallarını izleyerek öğelere ayırır. Ayrıca, standart çözümleyici tüm karakterleri küçük harf formuna dönüştürür. Hem dizine eklenmiş belgeler hem de arama terimleri dizin oluşturma ve sorgu işleme sırasında çözümleme yoluyla gider.  

Her aranabilir alanda otomatik olarak kullanılır. Varsayılanı alan bazında geçersiz kılabilirsiniz. Alternatif çözümleyiciler bir [dil çözümleyicisi](index-add-language-analyzers.md), [özel çözümleyici](index-add-custom-analyzers.md), ya da [kullanılabilir analizörler listesinden](index-add-custom-analyzers.md#AnalyzerTable)önceden tanımlanmış bir çözümleyici olabilir.


## <a name="types-of-analyzers"></a>Çözümleyici türleri

Aşağıdaki listede, Azure Bilişsel Arama'da hangi çözümleyicilerin kullanılabilir olduğu açıklanmaktadır.

| Kategori | Açıklama |
|----------|-------------|
| [Standart Lucene analizörü](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | Varsayılan. Belirtim veya yapılandırma gerekmez. Bu genel amaçlı çözümleyici çoğu dil ve senaryo için iyi performans gösterir.|
| Önceden tanımlanmış analizörler | Olduğu gibi kullanılmak üzere tasarlanmış bitmiş bir ürün olarak sunulmaktadır. <br/>İki türü vardır: özel ve dil. Onları "önceden tanımlanmış" yapan şey, yapılandırma veya özelleştirme olmadan onları ada göre referans haline getirmiş olmaktır. <br/><br/>Metin girişleri özel işleme veya en az işleme gerektirdiğinde [özelleştirilmiş (dil-agnostik) çözümleyiciler](index-add-custom-analyzers.md#AnalyzerTable) kullanılır. Dil önceden tanımlanmış olmayan analizörler **Asciifolding,** **Anahtar Kelime**, **Desen**, **Basit**, **Stop**, **Whitespace**içerir.<br/><br/>[Dil çözümleyicileri,](index-add-language-analyzers.md) tek tek diller için zengin dil desteğine ihtiyaç duyduğunuzda kullanılır. Azure Bilişsel Arama, 35 Lucene dil çözümleyicisi ve 50 Microsoft doğal dil işleme çözümleyicisi destekler. |
|[Özel çözümleyiciler](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Bir belirteç (gerekli) ve isteğe bağlı filtrelerden (char veya token) oluşan varolan öğelerin birleşiminin kullanıcı tarafından tanımlanan yapılandırmasını ifade eder.|

**Desen** veya **Durdur**gibi önceden tanımlanmış birkaç çözümleyici sınırlı bir yapılandırma seçeneğini destekler. Bu seçenekleri ayarlamak için, önceden tanımlanmış çözümleyiciden ve [Önceden Tanımlı Çözümleyici Başvurusu'nda](index-add-custom-analyzers.md#AnalyzerTable)belgelenen alternatif seçeneklerden oluşan özel bir çözümleyiciyi etkin bir şekilde oluşturursunuz. Herhangi bir özel yapılandırmada olduğu gibi, yeni yapılandırmanızı Lucene Pattern çözümleyicisinden ayırt etmek için *myPatternAnalyzer* gibi bir ad sağlayın.

## <a name="how-to-specify-analyzers"></a>Çözümleyiciler nasıl belirtilir?

1. (yalnızca özel analizörler için) Dizin tanımında adlandırılmış bir **çözümleyici** bölümü oluşturun. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/create-index) [Add custom analyzers](index-add-custom-analyzers.md)

2. Dizindeki bir [alan tanımında,](https://docs.microsoft.com/rest/api/searchservice/create-index) alanın **çözümleyici** özelliğini hedef çözümleyicinin adına ayarlayın `"analyzer" = "keyword"`(örneğin, . Geçerli değerler, dizin şemasında tanımlanan önceden tanımlanmış bir çözümleyicinin, dil çözümleyicisinin veya özel çözümleyicinin adını içerir. Dizin hizmette oluşturulmadan önce dizin tanımı aşamasında çözümleyici atama yı planlayın.

3. İsteğe bağlı olarak, bir **çözümleyici** özelliği yerine, **indexAnalyzer** ve **searchAnalyzer** alan parametrelerini kullanarak dizin oluşturma ve sorgulama için farklı çözümleyiciler ayarlayabilirsiniz. Bu etkinliklerden biri diğeri tarafından gerekli olmayan belirli bir dönüştürme gerektiriyorsa, veri hazırlama ve alma için farklı çözümleyiciler kullanırsınız.

> [!NOTE]
> Dizinleme zamanında, bir alanın sorgu zamanına göre farklı bir [dil çözümleyicisi](index-add-language-analyzers.md) kullanmak mümkün değildir. Bu özellik [özel çözümleyiciler](index-add-custom-analyzers.md)için ayrılmıştır. Bu nedenle, **searchAnalyzer** veya **indexAnalyzer** özelliklerini bir dil çözümleyicisinin adına ayarlamaya çalışırsanız, REST API bir hata yanıtı döndürecektir. Bunun yerine **çözümleyici** özelliğini kullanmanız gerekir.

**Çözümleyici** veya **indexAnalyzer'ı** zaten fiziksel olarak oluşturulmuş bir alana atamak izin verilmez. Bunlardan herhangi biri belirsizse, hangi eylemlerin yeniden oluşturulmasını ve neden gerektirdiğini bir dökümü için aşağıdaki tabloyu gözden geçirin.
 
 | Senaryo | Etki | Adımlar |
 |----------|--------|-------|
 | Yeni alan ekleme | Minimal | Şemada alan henüz yoksa, alan dizininizde henüz fiziksel bir varlık bulunmadığından, yapacak alan düzeltmesi yoktur. Varolan bir dizine yeni bir alan eklemek için [Dizin Güncelleştir'i](https://docs.microsoft.com/rest/api/searchservice/update-index) ve doldurmak için [BirleştirmeOrUpload'u](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) kullanabilirsiniz.|
 | Varolan bir dizinlenmiş alana bir **çözümleyici** veya **indexAnalyzer** ekleyin. | [Yeni -den inşa](search-howto-reindex.md) | Bu alanın ters dizini baştan sona yeniden oluşturulmalı ve bu alanların içeriği yeniden dizine eklenmelidir. <br/> <br/>Etkin geliştirme aşamasındaki dizinler için, yeni alan tanımını almak için dizini [silin](https://docs.microsoft.com/rest/api/searchservice/delete-index) ve [oluşturun.](https://docs.microsoft.com/rest/api/searchservice/create-index) <br/> <br/>Üretimdeki dizinler için, gözden geçirilmiş tanımı sağlamak için yeni bir alan oluşturarak yeniden oluşturmayı erteleyebilir ve eskisinin yerine kullanmaya başlayabilirsiniz. Yeni alanı birleştirmek için [Güncelleştirme Dizini'ni](https://docs.microsoft.com/rest/api/searchservice/update-index) ve doldurmak için [birleştirmeOrUpload'u](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) kullanın. Daha sonra, planlanan dizin hizmetinin bir parçası olarak, eski alanları kaldırmak için dizini temizleyebilirsiniz. |

## <a name="when-to-add-analyzers"></a>Çözümleyiciler ne zaman eklenir?

Çözümleyicileri eklemek ve atamak için en iyi zaman, dizinleri düşürme ve yeniden oluşturma sırasında etkin geliştirme sırasında rutindir.

Dizin tanımı sağlamlaştın, bir dizin için yeni çözümleme yapıları ekleyebilirsiniz, ancak bu hatayı önlemek istiyorsanız, **izin veren IndexDowntime** bayrağını [Güncelleştirme Dizini'ne](https://docs.microsoft.com/rest/api/searchservice/update-index) geçirmeniz gerekir:

*"Dizin güncelleştirmesi, kapalı kalma süresine neden olacağı için izin verilmez. Varolan bir dizin için yeni çözümleyiciler, belirteç filtreleri, belirteç filtreleri veya karakter filtreleri eklemek için, dizin güncelleştirme isteğinde 'allowIndexDowntime' sorgu parametresini 'true' olarak ayarlayın. Bu işlemin diziniçevrimdışı duruma getirecek ve dizin oluşturma ve sorgu isteklerinin başarısız olmasını sağlar. Dizin performansı ve yazma kullanılabilirliği, dizin güncelleştirildikten sonra birkaç dakika veya çok büyük dizinler için daha uzun süre bozulabilir."*

Bir alana çözümleyici atarken de geçerlidir. Çözümleyici, alanın tanımının ayrılmaz bir parçasıdır, bu nedenle yalnızca alan oluşturulduğunda ekleyebilirsiniz. Varolan alanlara çözümleyici eklemek istiyorsanız, dizini [düşürüp yeniden oluşturmanız](search-howto-reindex.md) veya istediğiniz çözümleyiciyle yeni bir alan eklemeniz gerekir.

Belirtildiği gibi, bir istisna **searchAnalyzer** varyantı. Çözümleyicileri **(analyzer**, **indexAnalyzer**, **searchAnalyzer)** belirtmenin üç yolu vardır, sadece **searchAnalyzer** özniteliği varolan bir alanda değiştirilebilir.

## <a name="recommendations-for-working-with-analyzers"></a>Çözümleyicilerle çalışmak için öneriler

Bu bölümde analizörlerile nasıl çalışılabilenler hakkında tavsiyeler sunulmaktadır.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Belirli gereksinimleriniz yoksa okuma-yazma için bir çözümleyici

Azure Bilişsel Arama ek **indexAnalyzer** ve **searchAnalyzer** alan parametreleri üzerinden dizin oluşturma ve arama için farklı analizörler belirtmenizi sağlar. Belirtilmemişse, **çözümleyici** özelliğine sahip çözümleyici kümesi hem dizin oluşturma hem de arama için kullanılır. `analyzer` Belirtilmemişse, varsayılan Standart Lucene çözümleyicisi kullanılır.

Genel bir kural, belirli gereksinimler aksini belirtmediği sürece, hem dizin oluşturma hem de sorgulama için aynı çözümleyiciyi kullanmaktır. Iyice test emin olun. Metin işleme, arama ve dizin oluşturma zamanında farklılık gösterirken, arama ve dizin çözümleyici yapılandırmaları hizalanmadığında sorgu terimleri ile dizinlenmiş terimler arasında uyumsuzluk riski yle karşılanırsınız.

### <a name="test-during-active-development"></a>Aktif geliştirme sırasında test edin

Standart çözümleyici geçersiz kılmak için dizin yeniden oluşturma gerekir. Mümkünse, bir dizini üretime dönüştürmeden önce, etkin geliştirme sırasında hangi analizörlerin kullanılacağına karar verin.

### <a name="inspect-tokenized-terms"></a>Belirteçli terimleri inceleyin

Bir arama beklenen sonuçları döndürmezse, en olası senaryo sorgudaki terim girişleri ile dizindeki belirteç terimleri arasındaki belirteç tutarsızlıklarıdır. Belirteçler aynı değilse, eşleşmeler gerçekleşmez. Belirteci çıktısını incelemek için, [Analiz API'sini](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) bir araştırma aracı olarak kullanmanızı öneririz. Yanıt, belirli bir çözümleyici tarafından oluşturulan belirteçlerden oluşur.

<a name="examples"></a>

## <a name="rest-examples"></a>REST örnekleri

Aşağıdaki örnekler, birkaç önemli senaryo için çözümleyici tanımlarını göstermektedir.

+ [Özel çözümleyici örneği](#Custom-analyzer-example)
+ [Çözümleyicileri bir alan örneğine atama](#Per-field-analyzer-assignment-example)
+ [Dizin oluşturma ve arama için analizörleri karıştırma](#Mixing-analyzers-for-indexing-and-search-operations)
+ [Dil çözümleyicisi örneği](#Language-analyzer-example)

<a name="Custom-analyzer-example"></a>

### <a name="custom-analyzer-example"></a>Özel çözümleyici örneği

Bu örnek, özel seçenekleri olan bir çözümleyici tanımı göstermektedir. Char filtreleri, belirteçleri ve belirteç filtreleri için özel seçenekler, adlandırılmış yapılar olarak ayrı ayrı belirtilir ve çözümleyici tanımında başvurulur. Önceden tanımlanmış elemanlar olduğu gibi kullanılır ve basitçe adıyla başvurulur.

Bu örneküzerinden yürüyüş:

* Çözümleyiciler, aranabilir bir alan için alan sınıfının bir özelliğidir.
* Özel çözümleyici, dizin tanımının bir parçasıdır. Hafifçe özelleştirilebilir (örneğin, tek bir filtrede tek bir seçeneği özelleştirme) veya birden çok yerde özelleştirilebilir.
* Bu durumda, özel çözümleyici sırayla özelleştirilmiş bir standart belirteç "my_standard_tokenizer" ve iki belirteç filtreleri kullanır "my_analyzer": küçük ve özelleştirilmiş assifolding filtre "my_asciifolding".
* Ayrıca 2 özel char filtreleri "map_dash" ve "remove_whitespace" tanımlar. İlki tüm tirelerin yerine alt çizer, ikincisi ise tüm boşlukları kaldırır. Boşlukların eşleme kurallarında UTF-8 kodlanmış olması gerekir. Char filtreleri belirteçleştirmeden önce uygulanır ve ortaya çıkan belirteçleri (standart belirteçleri çizgi ve boşluklarda kırılır, ancak alt çizgide değil) etkiler.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"my_analyzer"
        }
     ],
     "analyzers":[
        {
           "name":"my_analyzer",
           "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
           "charFilters":[
              "map_dash",
              "remove_whitespace"
           ],
           "tokenizer":"my_standard_tokenizer",
           "tokenFilters":[
              "my_asciifolding",
              "lowercase"
           ]
        }
     ],
     "charFilters":[
        {
           "name":"map_dash",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["-=>_"]
        },
        {
           "name":"remove_whitespace",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["\\u0020=>"]
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizerV2",
           "maxTokenLength":20
        }
     ],
     "tokenFilters":[
        {
           "name":"my_asciifolding",
           "@odata.type":"#Microsoft.Azure.Search.AsciiFoldingTokenFilter",
           "preserveOriginal":true
        }
     ]
  }
~~~~

<a name="Per-field-analyzer-assignment-example"></a>

### <a name="per-field-analyzer-assignment-example"></a>Alan başına çözümleyici atama örneği

Standart çözümleyici varsayılandır. Varsayılanı desen çözümleyicisi gibi farklı bir önceden tanımlanmış çözümleyiciyle değiştirmek istediğinizi varsayalım. Özel seçenekler ayarlamıyorsanız, alan tanımında ada göre belirtmeniz gerekir.

"Çözümleyici" öğesi, Standart çözümleyiciyi alan bazında geçersiz kılar. Genel geçersiz kılma yoktur. Bu örnekte, `text1` desen çözümleyicisi kullanır ve `text2`bir çözümleyici belirtmeyen, varsayılan kullanır.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text1",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"pattern"
        },
        {
           "name":"text2",
           "type":"Edm.String",
           "searchable":true
        }
     ]
  }
~~~~

<a name="Mixing-analyzers-for-indexing-and-search-operations"></a>

### <a name="mixing-analyzers-for-indexing-and-search-operations"></a>Dizin oluşturma ve arama işlemleri için analizörleri karıştırma

API'ler, dizin oluşturma ve arama için farklı çözümleyiciler belirtmek için ek dizin öznitelikleri içerir. **SearchAnalyzer** ve **indexAnalyzer** öznitelikleri, tek **çözümleyici** özniteliğiyerine bir çift olarak belirtilmelidir.


~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
     ],
  }
~~~~

<a name="Language-analyzer-example"></a>

### <a name="language-analyzer-example"></a>Dil çözümleyicisi örneği

Farklı dillerde dizeleri içeren alanlar bir dil çözümleyicisi kullanabilirken, diğer alanlar varsayılanı korur (veya önceden tanımlanmış veya özel çözümleyicisi kullanır). Bir dil çözümleyicisi kullanıyorsanız, hem dizin oluşturma hem de arama işlemleri için kullanılmalıdır. Dil çözümleyicisi kullanan alanların dizin oluşturma ve arama için farklı çözümleyicileri olamaz.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
        {
           "name":"text_fr",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"fr.lucene"
        }
     ],
  }
~~~~

## <a name="c-examples"></a>C# örnekleri

.NET SDK kod örneklerini kullanıyorsanız, çözümleyicileri kullanmak veya yapılandırmak için bu örnekleri ekleyebilirsiniz.

+ [Yerleşik bir çözümleyici atama](#Assign-a-language-analyzer)
+ [Çözümleyiciyi yapılandırma](#Define-a-custom-analyzer)

<a name="Assign-a-language-analyzer"></a>

### <a name="assign-a-language-analyzer"></a>Dil çözümleyicisi atama

Yapılandırması olmayan, olduğu gibi kullanılan herhangi bir çözümleyici alan tanımında belirtilir. Çözümleyici yapısı oluşturmak için bir gereklilik yoktur. 

Bu örnek, açıklama alanlarına Microsoft İngilizce ve Fransızca çözümleyiciler atar. [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) örneğinin hotels.cs dosyasında Otel sınıfını kullanarak otel sınıfı kullanılarak yapılan otel dizininin daha büyük bir tanımından alınan bir parçacık.

Çağrı [Çözümleyicisi,](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzer?view=azure-dotnet)Azure Bilişsel Arama'da desteklenen bir metin çözümleyicisi sağlayan [AnalyzerName](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername?view=azure-dotnet) türünü belirterek.

```csharp
    public partial class Hotel
    {
       . . . 

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.EnMicrosoft)]
        [JsonProperty("description")]
        public string Description { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.FrLucene)]
        [JsonProperty("description_fr")]
        public string DescriptionFr { get; set; }

      . . .
    }
```
<a name="Define-a-custom-analyzer"></a>

### <a name="define-a-custom-analyzer"></a>Özel bir çözümleyici tanımlayın

Özelleştirme veya yapılandırma gerektiğinde, bir dizin için bir çözümleyici yapısı eklemeniz gerekir. Bir kez tanımladıktan sonra, önceki örnekte gösterildiği gibi alan tanımı ekleyebilirsiniz.

Bir [CustomAnalyzer nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.customanalyzer?view=azure-dotnet) oluşturun. Daha fazla örnek için [bkz. CustomAnalyzerTests.cs.](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Microsoft.Azure.Search/tests/Tests/CustomAnalyzerTests.cs)

```csharp
{
   var definition = new Index()
   {
         Name = "hotels",
         Fields = FieldBuilder.BuildForType<Hotel>(),
         Analyzers = new[]
            {
               new CustomAnalyzer()
               {
                     Name = "url-analyze",
                     Tokenizer = TokenizerName.UaxUrlEmail,
                     TokenFilters = new[] { TokenFilterName.Lowercase }
               }
            },
   };

   serviceClient.Indexes.Create(definition);
```

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure Bilişsel Arama'da tam metin aramanın nasıl çalıştığına](search-lucene-query-architecture.md)ilişkin kapsamlı açıklamamızı inceleyin. Bu makalede, yüzeyde sezgilere aykırı görünen davranışları açıklamak için örnekler kullanır.

+ [Arama Belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) örnek bölümünden veya portaldaki Arama gezginindeki [Basit sorgu sözdiziminden](query-simple-syntax.md) ek sorgu sözdizimini deneyin.

+ Dile özgü [sözlü analizörlerin](index-add-language-analyzers.md)nasıl uygulanacağı öğrenin.

+ Tek tek alanlarda en az işleme veya özel işleme için [özel çözümleyicileri yapılandırın.](index-add-custom-analyzers.md)

## <a name="see-also"></a>Ayrıca bkz.

 [Belgelerde Arama REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

 [Basit sorgu söz dizimi](query-simple-syntax.md) 

 [Tam Lucene sorgu söz dizimi](query-lucene-syntax.md) 
 
 [Arama sonuçlarını işleme](search-pagination-page-layout.md)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
