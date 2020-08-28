---
title: .NET SDK sürüm 11 ' e yükseltme
titleSuffix: Azure Cognitive Search
description: Kodu eski sürümlerden Azure Bilişsel Arama .NET SDK sürüm 11 ' e geçirin. Nelerin yeni olduğunu ve hangi kod değişikliklerinin gerekli olduğunu öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 08/20/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 5a72f864d16ff89636b77709e439dd04134b64b7
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89002734"
---
# <a name="upgrade-to-azure-cognitive-search-net-sdk-version-11"></a>Azure Bilişsel Arama .NET SDK sürüm 11 ' e yükseltme

[.NET SDK 'sının](/dotnet/api/overview/azure/search)10,0 veya daha eski bir sürümünü kullanıyorsanız, bu makale 11 sürümüne yükseltmenize yardımcı olur.

Sürüm 11, Azure SDK geliştirme ekibi tarafından yayınlanan, tamamen yeniden tasarlanan bir istemci kitaplığıdır (önceki sürümler Azure Bilişsel Arama geliştirme ekibi tarafından üretildi). Kitaplık, diğer Azure istemci kitaplıklarıyla daha fazla tutarlılık için yeniden tasarlanmıştır, [Azure](/dotnet/api/azure.core) üzerinde bir bağımlılık gerçekleştirerek ve [ üzerindeSystem.Text.Js](/dotnet/api/system.text.json)ve ortak görevler için tanıdık yaklaşımlar uygulama.

Yeni sürümde fark ettiğiniz bazı önemli farklılıklar şunlardır:

+ Bir paket ve kitaplığı birden çok farklı
+ Yeni bir paket adı: `Azure.Search.Documents` yerine `Microsoft.Azure.Search` .
+ İki yerine üç istemci: `SearchClient` , `SearchIndexClient` , `SearchIndexerClient`
+ Bazı görevleri basitleştirecek bir dizi API ve küçük yapısal farklılık içindeki adlandırma farklılıkları

> [!NOTE]
> .NET SDK sürüm 11 ' de listelenen değişikliklerin listesi için [**değişiklik günlüğünü**](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md) gözden geçirin.

## <a name="package-and-library-consolidation"></a>Paket ve kitaplık birleştirme

Sürüm 11 birden çok paketi ve kitaplığı tek bir birleştirir. Geçiş sonrası, yönetilecek daha az kitaplık olacaktır.

+ [Azure.Search.Documtalar paketi](https://www.nuget.org/packages/Azure.Search.Documents/)

+ [İstemci kitaplığı için API başvurusu](/dotnet/api/overview/azure/search.documents-readme?view=azure-dotnet)

## <a name="client-differences"></a>İstemci farkları

Uygun olduğunda, aşağıdaki tablo iki sürüm arasındaki istemci kitaplıklarını eşler.

| İşlem kapsamı | Microsoft. Azure. Search &nbsp; (ile v10 arasındaki) | Azure.Search.Documstalar &nbsp; (v11) |
|---------------------|------------------------------|------------------------------|
| Sorgular için kullanılan istemci ve bir dizini doldurma. | [SearchIndexClient](/dotnet/api/azure.search.documents.indexes.searchindexclient) | [SearchClient](/dotnet/api/azure.search.documents.searchclient) |
| Dizinler, çözümleyiciler, eş anlamlı haritalar için kullanılan istemci | [SearchServiceClient](/dotnet/api/microsoft.azure.search.searchserviceclient) | [SearchIndexClient](/dotnet/api/azure.search.documents.indexes.searchindexclient) |
| Dizin oluşturucular, veri kaynakları, becerileri için kullanılan istemci | [SearchServiceClient](/dotnet/api/microsoft.azure.search.searchserviceclient) | [SearchIndexerClient (**Yeni**)](/dotnet/api/azure.search.documents.indexes.searchindexerclient) |

> [!Important]
> `SearchIndexClient` Her iki sürümde de bulunur, ancak farklı işlemleri destekler. Sürüm 10 ' da `SearchIndexClient` Dizinler ve diğer nesneler oluşturun. Sürüm 11 ' de `SearchIndexClient` var olan dizinlerle birlikte kullanılır. Kodu güncelleştirirken karışıklık oluşmasını önlemek için, istemci başvurularının güncelleştirildiği sırayı en az bir hale getirin. [Yükseltme adımlarında](#UpgradeSteps) aşağıdaki sıra, herhangi bir dize değiştirme sorununu azaltmaya yardımcı olur.

<a name="naming-differences"></a>

## <a name="naming-and-other-api-differences"></a>Adlandırma ve diğer API farklılıkları

İstemci farklılıklarının yanı sıra (daha önce belirtildiği gibi), diğer birçok API yeniden adlandırıldı ve bazı durumlarda yeniden tasarlanmıştır. Sınıf adı farklılıkları aşağıda özetlenmiştir. Bu liste ayrıntılı değildir, ancak belirli kod bloklarındaki düzeltmeler için faydalı olabilecek API değişikliklerini göreve göre gruplayın. Bir API güncelleştirmeleri listesi için bkz. GitHub 'da için [değişiklik günlüğü](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md) `Azure.Search.Documents` .

### <a name="authentication-and-encryption"></a>Kimlik doğrulama ve şifreleme

| Sürüm 10 | Sürüm 11 eşdeğeri |
|------------|-----------------------|
| [SearchCredentials](/dotnet/api/microsoft.azure.search.searchcredentials) | [AzureKeyCredential](/dotnet/api/azure.azurekeycredential) |
| `EncryptionKey` ( [Önizleme SDK 'sında](https://www.nuget.org/packages/Microsoft.Azure.Search/8.0.0-preview) genel kullanıma açık bir özellik olarak var) | [SearchResourceEncryptionKey](/dotnet/api/azure.search.documents.indexes.models.searchresourceencryptionkey) |

### <a name="indexes-analyzers-synonym-maps"></a>Dizinler, çözümleyiciler, eş anlamlı haritalar

| Sürüm 10 | Sürüm 11 eşdeğeri |
|------------|-----------------------|
| [Dizin oluşturma](/dotnet/api/microsoft.azure.documents.index) | [Searchındex](/dotnet/api/azure.search.documents.indexes.models.searchindex) |
| [Alan](/dotnet/api/microsoft.azure.search.models.field) | [SearchField](/dotnet/api/azure.search.documents.indexes.models.searchfield) |
| [X](/dotnet/api/microsoft.azure.search.models.datatype) | [SearchFieldDataType](/dotnet/api/azure.search.documents.indexes.models.searchfielddatatype) |
| [Itemerror](/dotnet/api/microsoft.azure.search.models.itemerror) | [SearchIndexerError](/dotnet/api/azure.search.documents.indexes.models.searchindexererror) |
| [Analyzer](/dotnet/api/microsoft.azure.search.models.analyzer) | [LexicalAnalyzer](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzer) (Ayrıca, `AnalyzerName` için `LexicalAnalyzerName` ) |
| [Analiz Zerequest](/dotnet/api/microsoft.azure.search.models.analyzerequest) | [Analiz Zetextoptions](/dotnet/api/azure.search.documents.indexes.models.analyzetextoptions) |
| [Standartçözümleyici](/dotnet/api/microsoft.azure.search.models.standardanalyzer) | [LuceneStandardAnalyzer](//dotnet/api/azure.search.documents.indexes.models.lucenestandardanalyzer) |
| [StandardTokenizer](/dotnet/api/microsoft.azure.search.models.standardtokenizer) | [LuceneStandardTokenizer](/dotnet/api/azure.search.documents.indexes.models.lucenestandardtokenizer) (Ayrıca, `StandardTokenizerV2` için `LuceneStandardTokenizerV2` ) |
| [TokenInfo](/dotnet/api/microsoft.azure.search.models.tokeninfo) | [Çözümleyicileri Edtokenınfo](/dotnet/api/azure.search.documents.indexes.models.analyzedtokeninfo) |
| [Belirteç ayırıcı](/dotnet/api/microsoft.azure.search.models.tokenizer) | [Lexicaltokenizer](/dotnet/api/azure.search.documents.indexes.models.lexicaltokenizer) (Ayrıca, `TokenizerName` için `LexicalTokenizerName` ) |
| [Eş anlamlı. biçim](/dotnet/api/microsoft.azure.search.models.synonymmap.format) | Yok. Başvuruları kaldırın `Format` . |

Alan tanımları basitleştirilmiştir: [Searchablefield](/dotnet/api/azure.search.documents.indexes.models.searchablefield), [simplefield](/dotnet/api/azure.search.documents.indexes.models.simplefield), [complexfield](/dotnet/api/azure.search.documents.indexes.models.complexfield) , alan tanımları oluşturmaya yönelik yeni API 'lardır.

### <a name="indexers-datasources-skillsets"></a>Dizin oluşturucular, veri kaynakları, becerileri

| Sürüm 10 | Sürüm 11 eşdeğeri |
|------------|-----------------------|
| [Dizin Oluşturucu](/dotnet/api/microsoft.azure.search.models.indexer) | [SearchIndexer](/dotnet/api/azure.search.documents.indexes.models.searchindexer) |
| [DataSource](/dotnet/api/microsoft.azure.search.models.datasource) | [SearchIndexerDataSourceConnection](//dotnet/api/azure.search.documents.indexes.models.searchindexerdatasourceconnection) |
| [İmde](/dotnet/api/microsoft.azure.search.models.skill) | [SearchIndexerSkill](/dotnet/api/azure.search.documents.indexes.models.searchindexerskill) |
| [Beceri](/dotnet/api/microsoft.azure.search.models.skillset) | [SearchIndexerSkillset](/dotnet/api/azure.search.documents.indexes.models.searchindexerskill) |
| [DataSourceType](/dotnet/api/microsoft.azure.search.models.datasourcetype) | [SearchIndexerDataSourceType](/dotnet/api/azure.search.documents.indexes.models.searchindexerdatasourcetype) |

### <a name="data-import"></a>Veri içeri aktarma

| Sürüm 10 | Sürüm 11 eşdeğeri |
|------------|-----------------------|
| [Indexaction](/dotnet/api/microsoft.azure.search.models.indexaction) | [Indexdocumentsaction](/dotnet/api/azure.search.documents.models.indexdocumentsaction) |
| [Indexbatch](/dotnet/api/microsoft.azure.search.models.indexbatch) | [Indexdocumentsbatch](/dotnet/api/azure.search.documents.models.indexdocumentsbatch) |

### <a name="query-definitions-and-results"></a>Sorgu tanımları ve sonuçları

| Sürüm 10 | Sürüm 11 eşdeğeri |
|------------|-----------------------|
| [DocumentSearchResult](/dotnet/api/microsoft.azure.search.models.documentsearchresult-1) | Sonucun tek bir belge veya birden çok olmasına bağlı olarak [SearchResult](/dotnet/api/azure.search.documents.models.searchresult-1) veya [SearchResults](/dotnet/api/azure.search.documents.models.searchresults-1). |
| [DocumentSuggestResult](/dotnet/api/microsoft.azure.search.models.documentsuggestresult-1) | [SuggestResults](/dotnet/api/azure.search.documents.models.suggestresults-1) |
| [Kullanılması](/dotnet/api/microsoft.azure.search.models.searchparameters) |  [SearchOptions](/dotnet/api/azure.search.documents.searchoptions)  |

<a name="WhatsNew"></a>

## <a name="whats-in-version-11"></a>Sürüm 11 ' de neler

Azure Bilişsel Arama istemci kitaplığı 'nın her sürümü, REST API karşılık gelen bir sürümünü hedefler. REST API, tek tek SDK 'ların bir REST API sürümünü sarmaladığı, hizmete bağlı olarak değerlendirilir. Bir .NET geliştiricisi olarak, belirli nesneler veya işlemlerde daha fazla arka plan istiyorsanız [REST API belgelerini](/rest/api/searchservice/) gözden geçirmeniz yararlı olabilir.

[2020-06-30 arama hizmeti](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/search/data-plane/Azure.Search/preview/2020-06-30/searchservice.json), sürüm 11 ' i hedefler. Sürüm 11 de sıfırdan oluşturulan yeni bir istemci kitaplığı olduğundan, çoğu geliştirme çabasında denkliği, sürüm 10 ' a odaklanılmış ve bazı REST API özellik desteği hala bekliyor.

Sürüm 11,0, aşağıdaki nesneleri ve işlemleri tam olarak destekler:

+ Dizin oluşturma ve yönetme
+ Eş anlamlı eşleme oluşturma ve yönetimi
+ Tüm sorgu türleri ve sözdizimi (coğrafi uzamsal filtreler hariç)
+ Veri kaynakları ve becerileri dahil olmak üzere Azure veri kaynaklarını dizine alma için Dizin Oluşturucu nesneler ve işlemler

Sürüm 11,1 şunları ekler:

+ [FieldBuilder](/dotnet/api/azure.search.documents.indexes.fieldbuilder) (11,1 'e eklendi)
+ [Seri hale getirici özelliği](/dotnet/api/azure.search.documents.searchclientoptions.serializer) (11,1 ' de eklenmiştir) özel serileştirme desteği

### <a name="pending-features"></a>Bekleyen özellikler

Aşağıdaki sürüm 10 özellikleri henüz 11. sürümde kullanılamamaktadır. Bu özelliklere ihtiyaç duyuyorsanız, desteklenene kadar geçiş üzerinde tutun.

+ Jeo-uzamsal türler
+ [Bilgi deposu](knowledge-store-concept-intro.md)

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları

Aşağıdaki adımlar, özellikle de istemci başvurularına bağlı olarak, gerekli görevlerin ilk kümesinden yürüyerek bir kod geçişi üzerinde başlamanızı sağlar.

1. Proje başvurularınızı sağ tıklayıp "NuGet Paketlerini Yönet..." seçeneğini belirleyerek [Azure.Search.Documstaları paketini](https://www.nuget.org/packages/Azure.Search.Documents/) yükleyeceksiniz. Visual Studio 'da.

1. Microsoft. Azure. Search için aşağıdaki yönergeleri kullanarak değiştirin:

   ```csharp
   using Azure;
   using Azure.Search.Documents;
   using Azure.Search.Documents.Indexes;
   using Azure.Search.Documents.Indexes.Models;
   using Azure.Search.Documents.Models;
   ```

1. JSON serileştirme gerektiren sınıflar için `using Newtonsoft.Json` ile değiştirin `using System.Text.Json.Serialization` .

1. İstemci kimlik doğrulama kodunu gözden geçirin. Önceki sürümlerde, API anahtarını (örneğin, [SearchServiceClient. Credentials](/dotnet/api/microsoft.azure.search.searchserviceclient.credentials) özelliği) ayarlamak için istemci nesnesindeki özellikleri kullanırsınız. Geçerli sürümde, anahtarı kimlik bilgileri olarak geçirmek için [AzureKeyCredential](/dotnet/api/azure.azurekeycredential) sınıfını kullanın, böylece gerekırse, API anahtarını yeni istemci nesneleri oluşturmadan güncelleştirebilirsiniz.

   İstemci özellikleri yalnızca `Endpoint` , `ServiceName` , ve `IndexName` (uygun olduğunda) için kolaylaştırılmıştır. Aşağıdaki örnek, anahtar değerini okumak için uç noktayı ve [ortam](/dotnet/api/system.environment) sınıfını sağlamak Için sistem [URI](/dotnet/api/system.uri) sınıfını kullanır:

   ```csharp
   Uri endpoint = new Uri(Environment.GetEnvironmentVariable("SEARCH_ENDPOINT"));
   AzureKeyCredential credential = new AzureKeyCredential(
      Environment.GetEnvironmentVariable("SEARCH_API_KEY"));
   SearchIndexClient indexClient = new SearchIndexClient(endpoint, credential);
   ```

1. Dizin oluşturucudan ilgili nesneler için yeni istemci başvuruları ekleyin. Dizin oluşturucular, veri kaynakları veya becerileri kullanıyorsanız, istemci başvurularını [SearchIndexerClient](/dotnet/api/azure.search.documents.indexes.searchindexerclient)olarak değiştirin. Bu istemci, sürüm 11 ' de yenidir ve öncül değildir.

1. Sorgular ve veri içeri aktarma için istemci başvurularını güncelleştirin. [Searchındexclient](/dotnet/api/microsoft.azure.search.searchindexclient) örnekleri [searchclient](/dotnet/api/azure.search.documents.searchclient)olarak değiştirilmelidir. Ad karışıklığı önlemek için, bir sonraki adıma geçmeden önce tüm örnekleri yakalediğinizden emin olun.

1. Dizin, Dizin Oluşturucu, eş anlamlı eşleme ve çözümleyici nesneleri için istemci başvurularını güncelleştirin. [SearchServiceClient](/dotnet/api/microsoft.azure.search.searchserviceclient) örnekleri [searchındexclient](/dotnet/api/microsoft.azure.search.searchindexclient)olarak değiştirilmelidir. 

1. Mümkün olduğunca, yeni kitaplığın API 'Lerini kullanmak için sınıfları, yöntemleri ve özellikleri güncelleştirin. [Adlandırma farkları](#naming-differences) bölümünün başlaması için bir yer vardır ancak [değişiklik günlüğünü](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md)de gözden geçirebilirsiniz.

   Eşdeğer API 'Leri bulmakta sorun yaşıyorsanız, [https://github.com/MicrosoftDocs/azure-docs/issues](https://github.com/MicrosoftDocs/azure-docs/issues) belgeleri iyileştirebilmemiz veya sorunu araştırabilmeniz için bir sorunu günlüğe kaydetmeyi öneririz.

1. Çözümü yeniden derleyin. Derleme hatalarını veya uyarılarını düzelttikten sonra, [Yeni işlevlerden](#WhatsNew)yararlanmak için uygulamanızda ek değişiklikler yapabilirsiniz.

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-11"></a>Sürüm 11 ' deki Son değişiklikler

Kitaplıklarda ve API 'Lerde yapılan değişiklikler verildiğinde, sürüm 11 ' e yükseltme önemsiz değildir ve kodunuzun artık sürüm 10 ve öncesiyle geriye dönük olarak uyumlu olmaması açısından önemli bir değişiklik oluşturur. Farkları kapsamlı bir şekilde gözden geçirmek için bkz. için [değişiklik günlüğü](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md) `Azure.Search.Documents` .

Sürüm 11 ' deki kod değişikliklerinin var olan işlevlerle (ve yalnızca API 'lerin yeniden düzenlemesi değil) ilgisi olan hizmet sürümü güncelleştirmeleri açısından, aşağıdaki davranış değişikliklerini bulacaksınız:

+ [BM25 derecelendirme algoritması](index-ranking-similarity.md) önceki derecelendirme algoritmasından daha yeni teknolojinin yerini alır. Yeni hizmetler, bu algoritmayı otomatik olarak kullanacaktır. Mevcut hizmetler için, parametreleri yeni algoritmayı kullanacak şekilde ayarlamanız gerekir.

+ Null değerler için [sıralanmış sonuçlar](search-query-odata-orderby.md) bu sürümde değiştirilmiştir, sıralama ve sıralama ise son ' a göre null değerleri ilk görünür `asc` `desc` . Null değerlerin nasıl sıralanacağını işlemek için kod aldıysanız, artık gerekmiyorsa bu kodu gözden geçirmeniz ve potansiyel olarak kaldırmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure.Search.Documtalar paketi](https://www.nuget.org/packages/Azure.Search.Documents/)
+ [GitHub’daki örnekler](https://github.com/azure/azure-sdk-for-net/tree/Azure.Search.Documents_11.0.0/sdk/search/Azure.Search.Documents/samples)
+ [Azure.Search.Document API başvurusu](/dotnet/api/overview/azure/search.documents-readme?view=azure-dotnet)