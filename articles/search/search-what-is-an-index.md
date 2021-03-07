---
title: Dizin oluşturma
titleSuffix: Azure Cognitive Search
description: ", Şema tanımları ve fiziksel veri yapısı dahil olmak üzere Azure Bilişsel Arama 'de dizin oluşturma kavramlarını ve araçları sunar."
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/05/2021
ms.openlocfilehash: 96594d573c308727217f537e5421dcb79f02c2ff
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102433803"
---
# <a name="creating-search-indexes-in-azure-cognitive-search"></a>Azure Bilişsel Arama 'de arama dizinleri oluşturma

Bilişsel Arama, bir *arama dizininde* tam metin ve filtrelenmiş sorgular için kullanılan aranabilir içeriği depolar. Bir dizin, bir şema tarafından tanımlanır ve ikinci bir adım olarak verileri içeri aktarma ile hizmete kaydedilir. 

Dizinler *arama belgelerini* içerir. Kavramsal olarak, bir belge, dizininizdeki aranabilir verilerin tek bir birimidir. Bir perakendeciden her ürün için bir belge, bir haber kuruluşunda her bir makale için bir belge olabilir ve bu şekilde devam eder. Bu kavramları daha tanıdık veritabanı eşdeğerlerine eşleme: bir *arama dizini* bir *tabloya* karşılık gelir ve *Belgeler* bir tablodaki *satırlara* kabaca eşdeğerdir.

## <a name="whats-an-index-schema"></a>Dizin şeması nedir?

Bir dizinin fiziksel yapısı, şema tarafından belirlenir. ' Fields ' koleksiyonu genellikle bir dizinin en büyük bölümüdür; burada her alan adlandırılır, bir [veri türü](/rest/api/searchservice/Supported-data-types)atanır ve nasıl kullanıldığını belirten izin verilen davranışlar ile belirlenir.

```json
{
  "name": "name_of_index, unique across the service",
  "fields": [
    {
      "name": "name_of_field",
      "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
      "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
      "filterable": true (default) | false,
      "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
      "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
      "key": true | false (default, only Edm.String fields can be keys),
      "retrievable": true (default) | false,
      "analyzer": "name_of_analyzer_for_search_and_indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
      "searchAnalyzer": "name_of_search_analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
      "indexAnalyzer": "name_of_indexing_analyzer", (only if 'searchAnalyzer' is set and 'analyzer' is not set)
      "synonymMaps": [ "name_of_synonym_map" ] (optional, only one synonym map per field is currently supported)
    }
  ],
  "suggesters": [ ],
  "scoringProfiles": [ ],
  "analyzers":(optional)[ ... ],
  "charFilters":(optional)[ ... ],
  "tokenizers":(optional)[ ... ],
  "tokenFilters":(optional)[ ... ],
  "defaultScoringProfile": (optional) "...",
  "corsOptions": (optional) { },
  "encryptionKey":(optional){ }
  }
}
```

Diğer öğeler, kısaltma için daraltılır, ancak aşağıdaki bağlantılar, dil kurallarına veya çözümleyici tarafından desteklenen diğer özelliklere ve [çapraz kaynak uzaktan betik (CORS)](#corsoptions) ayarlarına göre belirteçlerde dizeleri işlemek için kullanılan ayrıntıları sağlar: [Öneri araçları](index-add-suggesters.md), [Puanlama profilleri](index-add-scoring-profiles.md), [çözümleyiciler](search-analyzers.md) .

## <a name="choose-a-client"></a>İstemci seçin

Arama dizini oluşturmanın birkaç yolu vardır. Erken geliştirme ve kavram kanıtı testi için Azure portal veya SDK 'Ları öneririz.

Geliştirme sırasında, sık sık yeniden deryi planlayın. Hizmette fiziksel yapılar oluşturulduğundan, var olan bir alan tanımında çoğu değişiklik için [dizinleri bırakma ve](search-howto-reindex.md) yeniden oluşturma gereklidir. Yeniden oluşturma işlemini daha hızlı hale getirmek için verilerinizin bir alt kümesiyle çalışmayı düşünebilirsiniz.

### <a name="permissions"></a>İzinler

GET istekleri de dahil olmak üzere bir arama diziniyle ilgili tüm işlemler, istekte bir [yönetici API anahtarı](search-security-api-keys.md) gerektirir.

### <a name="limits"></a>Sınırlar

Tüm [hizmet katmanları](search-limits-quotas-capacity.md#index-limits) , oluşturabileceğiniz nesne sayısını sınırlar. Ücretsiz katman üzerinde denemeler yapıyorsanız, belirli bir zamanda yalnızca 3 dizin olabilir.

### <a name="use-azure-portal-to-create-a-search-index"></a>Arama dizini oluşturmak için Azure portal kullanma

Portal, arama dizini oluşturmak için iki seçenek sunar: [**veri alma Sihirbazı**](search-import-data-portal.md) ve Dizin şeması belirtmek için alanlar sağlayan **dizin ekleme** . Sihirbaz, ayrıca bir Dizin Oluşturucu, veri kaynağı ve yükleme verileri oluşturarak ek işlemlerde de paket oluşturur. İstediğiniz kadar fazla ise, yalnızca **Add Index** veya başka bir yaklaşım kullanmanız gerekir.

Aşağıdaki ekran görüntüsünde, portalda **Dizin Ekle** ' nin nerede bulunacağı gösterilmektedir. **Içeri aktarma verileri** , sonraki kapıdır.

  :::image type="content" source="media/search-what-is-an-index/add-index.png" alt-text="Dizin komutu Ekle" border="true":::

> [!Tip]
> Portal üzerinden dizin tasarımı, sayısal alanlarda tam metin arama özelliklerine izin vermeme gibi belirli veri türleri için gereksinimleri ve şema kurallarını zorunlu kılar. Yeniden kullanılabilir bir dizininizden sonra, JSON 'u portaldan kopyalayabilir ve çözümünüze ekleyebilirsiniz.

### <a name="use-a-rest-client"></a>REST istemcisi kullanma

Hem Postman hem de Visual Studio Code (Azure Bilişsel Arama Uzantısı ile), arama dizini istemcisi olarak çalışabilir. İki aracı kullanarak, arama hizmetinize bağlanabilir ve [Dizin oluşturma (REST)](/rest/api/searchservice/create-index) istekleri gönderebilirsiniz. Nesneleri oluşturmak için REST istemcilerini gösteren çok sayıda öğretici ve örnek vardır. 

Her istemci hakkında bilgi edinmek için şu makalelerden birini başlatın:

+ [REST ve Postman kullanarak arama dizini oluşturma](search-get-started-rest.md)
+ [Visual Studio Code ve Azure Bilişsel Arama kullanmaya başlama](search-get-started-vs-code.md)

Dizin isteklerinin formüllenmesini sağlamaya yönelik yardım için [Dizin işlemlerine (REST)](/rest/api/searchservice/index-operations) bakın.

### <a name="use-an-sdk"></a>SDK kullanma

Bilişsel Arama için Azure SDK 'Ları, genel olarak kullanılabilen özellikleri uygular. Bu nedenle, bir arama dizini oluşturmak için SDK 'Lardan birini kullanabilirsiniz. Bunların hepsi, dizin oluşturma ve güncelleştirme yöntemlerine sahip bir **Searchındexclient** sağlar.

| Azure SDK | İstemci | Örnekler |
|-----------|--------|----------|
| .NET | [SearchIndexClient](/dotnet/api/azure.search.documents.indexes.searchindexclient) | [Azure-Search-DotNet-Samples/QuickStart/v11/](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/quickstart/v11) |
| Java | [SearchIndexClient](/java/api/com.azure.search.documents.indexes.searchindexclient) | [Createındexexample. Java](https://github.com/Azure/azure-sdk-for-java/blob/azure-search-documents_11.1.3/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/indexes/CreateIndexExample.java) |
| JavaScript | [SearchIndexClient](/javascript/api/@azure/search-documents/searchindexclient) | [Dizinler](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/search/search-documents/samples/javascript/src/indexes) |
| Python | [SearchIndexClient](/python/api/azure-search-documents/azure.search.documents.indexes.searchindexclient) | [sample_index_crud_operations. Kopyala](https://github.com/Azure/azure-sdk-for-python/blob/7cd31ac01fed9c790cec71de438af9c45cb45821/sdk/search/azure-search-documents/samples/sample_index_crud_operations.py) |

## <a name="define-fields"></a>Alanları tanımlama

Bir arama belgesi, koleksiyon tarafından tanımlanır `fields` . Sorgular ve anahtarlar için alanlara ihtiyacınız olacak. Ayrıca muhtemelen filtreleri, modelleri ve sıralamayı desteklemek için alanlara ihtiyacınız olacaktır. Ayrıca, bir kullanıcının hiç görmemesi gereken veriler için alanlar da gerekebilir. Örneğin, arama sırasını değiştirmek için kullanabileceğiniz kar marjları veya pazarlama promosyonları için alanlar isteyebilirsiniz.

EDM. String türünde bir alanın belge anahtarı olarak belirlenmesi gerekir. Her arama belgesini benzersiz bir şekilde tanımlamak için kullanılır ve büyük/küçük harfe duyarlıdır. Ayrıntılar sayfasını doldurmak için bir belgeyi anahtarına göre alabilirsiniz.

Gelen veriler doğası halinde hiyerarşik ise, iç içe yapıları temsil etmek için [karmaşık tür](search-howto-complex-data-types.md) veri türünü atayın. Yerleşik örnek veri kümesi olan oteller, her bir otelle bire bir ilişkiye sahip olan ve her bir otel ile birden çok oda ilişkilendirildiği bir oda karmaşık koleksiyonu olan bir adres (birden çok alt alan içerir) kullanan karmaşık türleri gösterir. 

Dizin oluşturulmadan önce dize alanlarına çözümleyiciler atayın. Belirli alanlarda otomatik tamamlamayı etkinleştirmek istiyorsanız, öneri araçları için aynısını yapın.

<a name="index-attributes"></a>

### <a name="attributes"></a>Öznitelikler

Alan öznitelikleri, bir alanın nasıl kullanıldığını; örneğin, tam metin alanında mı, çok yönlü gezinmede mi, sıralama işlemlerinde vb.’de mi kullanılıp kullanılmayacağı belirler. 

Dize alanları genellikle "aranabilir" ve "alınabilir" olarak işaretlenir. Arama sonuçlarını daraltmak için kullanılan alanlar arasında "sıralanabilir", "filtrelenebilir" ve "çok yönlü tablo" bulunur.

|Öznitelik|Açıklama|  
|---------------|-----------------|  
|aranamaz |Tam metin aranabilir, dizin oluşturma sırasında sözcüklere bölme gibi sözcük temelli analize tabidir. Aranabilir bir alanı, "güneşli gün" gibi bir değere ayarlarsanız, dahili olarak bu "güneşli" ve "gün" belirteçlerine bölünür. Ayrıntılar için bkz. [Tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)|  
|filtrelenebilir |$filter sorgularında başvurulur. `Edm.String` veya `Collection(Edm.String)` türünde filtrelenebilir alanlara sözcüklere bölme işlemi uygulanmaz, bu nedenle karşılaştırmalar yalnızca tam eşleşmeler içindir. Örneğin, böyle bir alanı "güneşli gün" olarak ayarlarsanız `$filter=f eq 'sunny'` herhangi bir eşleşme bulmaz, ancak `$filter=f eq 'sunny day'` bulur. |  
|amayan |Varsayılan olarak sistem sonuçları puana göre sıralar, ancak belgelerdeki alanlara göre sıralamayı yapılandırabilirsiniz. Türündeki alanlar `Collection(Edm.String)` "sıralanabilir" olamaz. |  
|modellenebilir |Genellikle kategoriye göre (örneğin, belirli bir şehirdeki oteller) isabet sayısını içeren bir arama sonuçları sunumunda kullanılır. Bu seçenek, `Edm.GeographyPoint` türünde alanlarla kullanılamaz. `Edm.String`Filtrelenebilir, "sıralanabilir" veya "çok yönlü tablo" türündeki alanlar en fazla 32 kilobayt uzunluğunda olabilir. Ayrıntılar için bkz. [Dizin Oluşturma (REST API’si)](/rest/api/searchservice/create-index).|  
|anahtar |Dizin içindeki belgeler için benzersiz tanımlayıcı. Anahtar alan olarak tam olarak bir alan seçilmeli ve `Edm.String` türünde olmalıdır.|  
|defterinden |Alanın bir arama sonucunda döndürülüp döndürülemeyeceğini belirler. Filtre, sıralama veya puanlama mekanizması olarak bir alanı (örn. *kâr marjı*) kullanmak istediğinizde, ancak alanın son kullanıcıya görünür olmasını istemediğinizde bu faydalıdır. Bu öznitelik, `key` alanları için `true` olmalıdır.|  

İstediğiniz zaman yeni alanlar ekleyebilseniz de, dizinin ömrü boyunca mevcut alan tanımları kilitlenir. Bu nedenle geliştiriciler genellikle basit dizinler oluşturmak, fikirleri test etmek veya portal sayfalarını kullanarak bir ayarı bulmak için portalı kullanır. Dizini kolayca yeniden derleyebilmeniz için kod tabanlı bir yaklaşım izlerseniz, bir dizin tasarımı çerçevesinde sık sık yapılan yineleme daha verimli olur.

> [!NOTE]
> Bir dizin oluşturmak için kullandığınız API 'Ler, farklı varsayılan davranışlara sahiptir. [REST API 'leri](/rest/api/searchservice/Create-Index)için çoğu öznitelik varsayılan olarak etkindir (örneğin, "aranabilir" ve "alınabilir" dize alanları için geçerlidir) ve genellikle bunları kapatmak istiyorsanız ayarlamanız gerekir. .NET SDK için, tersi doğrudur. Açıkça ayarlamadığınız herhangi bir özellikte, özel olarak etkinleştirmediğiniz takdirde, karşılık gelen arama davranışının devre dışı bırakılması varsayılan olur.

<a name="index-size"></a>

## <a name="attributes-and-index-size-storage-implications"></a>Öznitelikler ve dizin boyutu (depolama etkileri)

Bir dizinin boyutu, karşıya yüklediğiniz belgelerin boyutuna göre belirlenir ve öneri araçları dahil edilip edilmeyeceğini ve ayrı alanlarda öznitelikleri nasıl ayarlayacağınıza ilişkin Dizin yapılandırması vardır. 

Aşağıdaki ekran görüntüsünde, çeşitli öznitelik birleşimlerinden kaynaklanan dizin depolama desenleri gösterilmektedir. Dizin, verileri Içeri aktarma Sihirbazı 'nı kullanarak kolayca oluşturabileceğiniz **gerçek emlak örnek dizinine** dayalıdır. Dizin şemaları gösterilmese de, dizin adına göre öznitelikleri çıkarsyükleyebilirsiniz. Örneğin, *reatastate ile aranabilir* Dizin "aranabilir" özniteliğine sahiptir ve başka hiçbir şey yoktur, *reatastate-alınabilir* Dizin, "alınabilir" özniteliği seçilir ve başka hiçbir şey yapmaz.

![Öznitelik seçimine dayalı Dizin boyutu](./media/search-what-is-an-index/realestate-index-size.png "Öznitelik seçimine dayalı Dizin boyutu")

Bu dizin çeşitleri yapay olsa da, özniteliklerin depolamayı nasıl etkilediği hakkında geniş karşılaştırmalar için bunlara başvurabiliyoruz. "Alınabilir" olarak ayarlandığında Dizin boyutu artar mi? Hayır. **Öneri aracı** bir dizin boyutunu artırmak için alan ekleme Evet. 

Filtrelenen veya sıralanabilir alanları, karakter sıralarının tam olarak eşleştirileceği şekilde belirteç olmadığından, filtrelenebilir veya sıralanabilir bir alan oluşturmak depolama tüketimine de eklenir.

Ayrıca, yukarıdaki tabloya yansıtılmayan [çözümleyiciler, çözümleyicilerin](search-analyzers.md)etkisidir. Tam karakter dizilerini (a, AB, ABC, abcd) depolamak için edgeNgram Simgeleştirici kullanıyorsanız, dizinin boyutu standart bir çözümleyici kullandıysanız daha büyük olur.

> [!Note]
> Depolama mimarisi Azure Bilişsel Arama uygulama ayrıntısı olarak değerlendirilir ve bildirimde bulunulmadan değiştirilebilir. Geçerli davranışın gelecekte devam edeceğini garanti vermez.

<a name="corsoptions"></a>

## <a name="about-corsoptions"></a>Bilgi `corsOptions`

Dizin şemaları, ayar için bir bölüm içerir `corsOptions` . Tarayıcı tüm çapraz kaynak isteklerini önleyediğinden, istemci tarafı JavaScript varsayılan olarak herhangi bir API 'yi çağıramaz. Dizininizdeki çapraz kaynak sorgularına izin vermek için **Corsoptions** ÖZNITELIĞINI ayarlayarak CORS 'yi etkinleştirin (çıkış noktaları arası kaynak paylaşımı). Güvenlik nedenleriyle, yalnızca sorgu API 'Leri CORS 'yi destekler. 

CORS için aşağıdaki seçenekler ayarlanabilir:

+ **allowederlerin** (gerekli): Bu, dizininiz için erişim verilecek olan kaynakları içeren bir listesidir. Bu, bu kaynaklardan sunulan tüm JavaScript kodunun dizininizi sorgulayabileceği anlamına gelir (doğru api anahtarını sağladığını varsayarak). Her kaynak genellikle form olur, `protocol://<fully-qualified-domain-name>:<port>` ancak `<port>` genellikle atlanacaktır. Daha fazla ayrıntı için bkz. [çıkış noktaları arası kaynak paylaşımı (Vikipedi)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) .

  Tüm kaynaklardan erişime izin vermek istiyorsanız, `*` **allowedkaynakları** dizisine tek bir öğe olarak dahil edin. *Bu, üretim arama hizmetleri için önerilen bir uygulamadır,* ancak geliştirme ve hata ayıklama için genellikle yararlıdır.

+ **Maxageınseconds** (isteğe bağlı): TARAYıCıLAR, CORS ön denetim yanıtlarını önbelleğe almak için süreyi (saniye cinsinden) belirlemede bu değeri kullanır. Bu negatif olmayan bir tamsayı olmalıdır. Bu değer arttıkça, daha iyi performans olacaktır, ancak CORS ilke değişikliklerinin etkili olması için daha uzun sürer. Ayarlanmamışsa, varsayılan 5 dakikalık bir süre kullanılacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bilişsel Arama için neredeyse her örnek veya İzlenecek yol kullanarak bir dizin oluşturma deneyimlerine ulaşabilirsiniz. Başlangıçlarda, içerik tablosundan hızlı başlangıçlardan herhangi birini seçebilirsiniz.

Ancak, verileri içeren bir dizin yüklemek için yöntemler konusunda da bilgi sahibi olmak isteyeceksiniz. Dizin tanımı ve veri içeri aktarma stratejileri art arda tanımlanmıştır. Aşağıdaki makalelerde bir dizin yükleme hakkında daha fazla bilgi sağlanmaktadır.

+ [Verileri içeri aktarmaya genel bakış](search-what-is-data-import.md)

+ [Belge ekleme, güncelleştirme veya silme (REST)](/rest/api/searchservice/addupdate-or-delete-documents) 