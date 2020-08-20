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
ms.openlocfilehash: 83208ec792f40661861dd558ac2c1a1521c1d7fb
ms.sourcegitcommit: d18a59b2efff67934650f6ad3a2e1fe9f8269f21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88660978"
---
# <a name="upgrade-to-azure-cognitive-search-net-sdk-version-11"></a>Azure Bilişsel Arama .NET SDK sürüm 11 ' e yükseltme

[.NET SDK 'sının](https://docs.microsoft.com/dotnet/api/overview/azure/search)10,0 veya daha eski bir sürümünü kullanıyorsanız, bu makale 11 sürümüne yükseltmenize yardımcı olur.

Sürüm 11, Azure SDK geliştirme ekibi tarafından yayınlanan, tamamen yeniden tasarlanan bir istemci kitaplığıdır (önceki sürümler Azure Bilişsel Arama geliştirme ekibi tarafından üretildi). Kitaplık, diğer Azure istemci kitaplıklarıyla daha fazla tutarlılık için yeniden tasarlanmıştır, [Azure](https://docs.microsoft.com/dotnet/api/azure.core) üzerinde bir bağımlılık gerçekleştirerek ve [ üzerindeSystem.Text.Js](https://docs.microsoft.com/dotnet/api/system.text.json)ve ortak görevler için tanıdık yaklaşımlar uygulama.

Yeni sürümde fark ettiğiniz bazı önemli farklılıklar şunlardır:

+ Bir paket ve kitaplığı birden çok farklı
+ Yeni bir paket adı: `Azure.Search.Documents` yerine `Microsoft.Azure.Search` .
+ İki yerine üç istemci: `SearchClient` , `SearchIndexClient` , `SearchIndexerClient`
+ Bazı görevleri basitleştirecek bir dizi API ve küçük yapısal farklılık içindeki adlandırma farklılıkları

## <a name="package-and-library-consolidation"></a>Paket ve kitaplık birleştirme

Sürüm 11 birden çok paketi ve kitaplığı tek bir birleştirir. Geçiş sonrası, yönetilecek daha az kitaplık olacaktır.

+ [Azure.Search.Documtalar paketi](https://www.nuget.org/packages/Azure.Search.Documents/)

+ [İstemci kitaplığı için API başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/search.documents-readme?view=azure-dotnet)

## <a name="client-differences"></a>İstemci farkları

Uygun olduğunda, aşağıdaki tablo iki sürüm arasındaki istemci kitaplıklarını eşler.

| İşlem kapsamı | Microsoft. Azure. Search &nbsp; (ile v10 arasındaki) | Azure.Search.Documstalar &nbsp; (v11) |
|---------------------|------------------------------|------------------------------|
| Sorgular için kullanılan istemci ve bir dizini doldurma. | [SearchIndexClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexclient) | [SearchClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchclient) |
| Dizinler, çözümleyiciler, eş anlamlı haritalar için kullanılan istemci | [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient) | [SearchIndexClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexclient) |
| Dizin oluşturucular, veri kaynakları, becerileri için kullanılan istemci | [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient) | [SearchIndexerClient (**Yeni**)](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexerclient) |

> [!Important]
> `SearchIndexClient` Her iki sürümde de bulunur, ancak farklı işlemleri destekler. Sürüm 10 ' da `SearchIndexClient` Dizinler ve diğer nesneler oluşturun. Sürüm 11 ' de `SearchIndexClient` var olan dizinlerle birlikte kullanılır. Kodu güncelleştirirken karışıklık oluşmasını önlemek için, istemci başvurularının güncelleştirildiği sırayı en az bir hale getirin. [Yükseltme adımlarında](#UpgradeSteps) aşağıdaki sıra, herhangi bir dize değiştirme sorununu azaltmaya yardımcı olur.

<a name="naming-differences"></a>

## <a name="naming-and-other-api-differences"></a>Adlandırma ve diğer API farklılıkları

İstemci farklılıklarının yanı sıra (daha önce belirtildiği gibi), diğer birçok API yeniden adlandırıldı ve bazı durumlarda yeniden tasarlanmıştır. Sınıf adı farklılıkları aşağıda özetlenmiştir. Bu liste ayrıntılı değildir, ancak belirli kod bloklarındaki düzeltmeler için faydalı olabilecek API değişikliklerini göreve göre gruplayın. Bir API güncelleştirmeleri listesi için bkz. GitHub 'da için [değişiklik günlüğü](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md) `Azure.Search.Documents` .

### <a name="authentication-and-encryption"></a>Kimlik doğrulama ve şifreleme

| Sürüm 10 | Sürüm 11 eşdeğeri |
|------------|-----------------------|
| [SearchCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchcredentials) | [AzureKeyCredential](https://docs.microsoft.com/dotnet/api/azure.azurekeycredential) |
| `EncryptionKey` ( [Önizleme SDK 'sında](https://www.nuget.org/packages/Microsoft.Azure.Search/8.0.0-preview) genel kullanıma açık bir özellik olarak var) | [SearchResourceEncryptionKey](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchresourceencryptionkey) |

### <a name="indexes-analyzers-synonym-maps"></a>Dizinler, çözümleyiciler, eş anlamlı haritalar

| Sürüm 10 | Sürüm 11 eşdeğeri |
|------------|-----------------------|
| [Dizin oluşturma](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.index) | [Searchındex](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindex) |
| [Alan](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field) | [SearchField](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchfield) |
| [X](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datatype) | [SearchFieldDataType](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchfielddatatype) |
| [Itemerror](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.itemerror) | [SearchIndexerError](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindexererror) |
| [Analyzer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzer) | [LexicalAnalyzer](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzer) (Ayrıca, `AnalyzerName` için `LexicalAnalyzerName` ) |
| [Analiz Zerequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzerequest) | [Analiz Zetextoptions](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.analyzetextoptions) |
| [Standartçözümleyici](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.standardanalyzer) | [LuceneStandardAnalyzer](https://docs.microsoft.com//dotnet/api/azure.search.documents.indexes.models.lucenestandardanalyzer) |
| [StandardTokenizer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.standardtokenizer) | [LuceneStandardTokenizer](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.lucenestandardtokenizer) (Ayrıca, `StandardTokenizerV2` için `LuceneStandardTokenizerV2` ) |
| [TokenInfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.tokeninfo) | [Çözümleyicileri Edtokenınfo](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.analyzedtokeninfo) |
| [Belirteç ayırıcı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.tokenizer) | [Lexicaltokenizer](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.lexicaltokenizer) (Ayrıca, `TokenizerName` için `LexicalTokenizerName` ) |
| [Eş anlamlı. biçim](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.synonymmap.format) | Yok. Başvuruları kaldırın `Format` . |

Alan tanımları basitleştirilmiştir: [Searchablefield](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchablefield), [simplefield](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.simplefield), [complexfield](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.complexfield) , alan tanımları oluşturmaya yönelik yeni API 'lardır.

### <a name="indexers-datasources-skillsets"></a>Dizin oluşturucular, veri kaynakları, becerileri

| Sürüm 10 | Sürüm 11 eşdeğeri |
|------------|-----------------------|
| [Dizin Oluşturucu](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) | [SearchIndexer](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindexer) |
| [DataSource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource) | [SearchIndexerDataSourceConnection](https://docs.microsoft.com//dotnet/api/azure.search.documents.indexes.models.searchindexerdatasourceconnection) |
| [İmde](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.skill) | [SearchIndexerSkill](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindexerskill) |
| [Beceri](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.skillset) | [SearchIndexerSkillset](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindexerskill) |
| [DataSourceType](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype) | [SearchIndexerDataSourceType](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindexerdatasourcetype) |

### <a name="data-import"></a>Veri içeri aktarma

| Sürüm 10 | Sürüm 11 eşdeğeri |
|------------|-----------------------|
| [Indexaction](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction) | [Indexdocumentsaction](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.indexdocumentsaction) |
| [Indexbatch](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch) | [Indexdocumentsbatch](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.indexdocumentsbatch) |

### <a name="query-definitions-and-results"></a>Sorgu tanımları ve sonuçları

| Sürüm 10 | Sürüm 11 eşdeğeri |
|------------|-----------------------|
| [DocumentSearchResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult-1) | Sonucun tek bir belge veya birden çok olmasına bağlı olarak [SearchResult](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.searchresult-1) veya [SearchResults](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.searchresults-1). |
| [DocumentSuggestResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsuggestresult-1) | [SuggestResults](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.suggestresults-1) |
| [Kullanılması](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters) |  [SearchOptions](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchoptions)  |

<a name="WhatsNew"></a>

## <a name="whats-in-version-11"></a>Sürüm 11 ' de neler

Azure Bilişsel Arama istemci kitaplığı 'nın her sürümü, REST API karşılık gelen bir sürümünü hedefler. REST API, tek tek SDK 'ların bir REST API sürümünü sarmaladığı, hizmete bağlı olarak değerlendirilir. Bir .NET geliştiricisi olarak, belirli nesneler veya işlemlerde daha fazla arka plan istiyorsanız [REST API belgelerini](https://docs.microsoft.com/rest/api/searchservice/) gözden geçirmeniz yararlı olabilir.

[2020-06-30 arama hizmeti](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/search/data-plane/Azure.Search/preview/2020-06-30/searchservice.json), sürüm 11 ' i hedefler. Sürüm 11 de sıfırdan oluşturulan yeni bir istemci kitaplığı olduğundan, çoğu geliştirme çabasında denkliği, sürüm 10 ' a odaklanılmış ve bazı REST API özellik desteği hala bekliyor.

Sürüm 11 aşağıdaki nesneleri ve işlemleri tam olarak destekler:

+ Dizin oluşturma ve yönetme
+ Eş anlamlı eşleme oluşturma ve yönetimi
+ Tüm sorgu türleri ve sözdizimi (coğrafi uzamsal filtreler hariç)
+ Veri kaynakları ve becerileri dahil olmak üzere Azure veri kaynaklarını dizine alma için Dizin Oluşturucu nesneler ve işlemler

### <a name="pending-features"></a>Bekleyen özellikler

Aşağıdaki sürüm 10 özellikleri henüz 11. sürümde kullanılamamaktadır. Bu özellikleri kullanırsanız, desteklenene kadar geçiş üzerinde tutun.

+ Jeo-uzamsal türler
+ [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder) ( [Bu geçici çözümü](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/tests/Samples/FieldBuilder/FieldBuilder.cs)kullanabilseniz de kullanabilirsiniz).
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

1. İstemci kimlik doğrulama kodunu gözden geçirin. Önceki sürümlerde, API anahtarını (örneğin, [SearchServiceClient. Credentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.credentials) özelliği) ayarlamak için istemci nesnesindeki özellikleri kullanırsınız. Geçerli sürümde, anahtarı kimlik bilgileri olarak geçirmek için [AzureKeyCredential](https://docs.microsoft.com/dotnet/api/azure.azurekeycredential) sınıfını kullanın, böylece gerekırse, API anahtarını yeni istemci nesneleri oluşturmadan güncelleştirebilirsiniz.

   İstemci özellikleri yalnızca `Endpoint` , `ServiceName` , ve `IndexName` (uygun olduğunda) için kolaylaştırılmıştır. Aşağıdaki örnek, anahtar değerini okumak için uç noktayı ve [ortam](https://docs.microsoft.com//dotnet/api/system.environment) sınıfını sağlamak Için sistem [URI](https://docs.microsoft.com/dotnet/api/system.uri) sınıfını kullanır:

   ```csharp
   Uri endpoint = new Uri(Environment.GetEnvironmentVariable("SEARCH_ENDPOINT"));
   AzureKeyCredential credential = new AzureKeyCredential(
      Environment.GetEnvironmentVariable("SEARCH_API_KEY"));
   SearchIndexClient indexClient = new SearchIndexClient(endpoint, credential);
   ```

1. Dizin oluşturucudan ilgili nesneler için yeni istemci başvuruları ekleyin. Dizin oluşturucular, veri kaynakları veya becerileri kullanıyorsanız, istemci başvurularını [SearchIndexerClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexerclient)olarak değiştirin. Bu istemci, sürüm 11 ' de yenidir ve öncül değildir.

1. Sorgular ve veri içeri aktarma için istemci başvurularını güncelleştirin. [Searchındexclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) örnekleri [searchclient](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchclient)olarak değiştirilmelidir. Ad karışıklığı önlemek için, bir sonraki adıma geçmeden önce tüm örnekleri yakalediğinizden emin olun.

1. Dizin, Dizin Oluşturucu, eş anlamlı eşleme ve çözümleyici nesneleri için istemci başvurularını güncelleştirin. [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient) örnekleri [searchındexclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient)olarak değiştirilmelidir. 

1. Mümkün olduğunca, yeni kitaplığın API 'Lerini kullanmak için sınıfları, yöntemleri ve özellikleri güncelleştirin. [Adlandırma farkları](#naming-differences) bölümünün başlaması için bir yer vardır ancak [değişiklik günlüğünü](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md)de gözden geçirebilirsiniz.

   Eşdeğer API 'Leri bulmakta sorun yaşıyorsanız, [https://github.com/MicrosoftDocs/azure-docs/issues](https://github.com/MicrosoftDocs/azure-docs/issues) belgeleri iyileştirebilmemiz veya sorunu araştırabilmeniz için bir sorunu günlüğe kaydetmeyi öneririz.

1. Çözümü yeniden derleyin. Derleme hatalarını veya uyarılarını düzelttikten sonra, [Yeni işlevlerden](#WhatsNew)yararlanmak için uygulamanızda ek değişiklikler yapabilirsiniz.

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-11"></a>Sürüm 11 ' deki Son değişiklikler

Kitaplıklarda ve API 'Lerde yapılan değişiklikler verildiğinde, sürüm 11 ' e yükseltme önemsiz değildir ve kodunuzun artık sürüm 10 ve öncesiyle geriye dönük olarak uyumlu olmaması açısından önemli bir değişiklik oluşturur. Farkları kapsamlı bir şekilde gözden geçirmek için bkz. için [değişiklik günlüğü](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md) `Azure.Search.Documents` .

Hizmet sürümleri açısından, 10 ' dan 11 ' e geçmek aşağıdaki davranış değişikliklerini sunar: 

+ [BM25 derecelendirme algoritması](index-ranking-similarity.md) önceki derecelendirme algoritmasından daha yeni teknolojinin yerini alır. Yeni hizmetler, bu algoritmayı otomatik olarak kullanacaktır. Mevcut hizmetler için, parametreleri yeni algoritmayı kullanacak şekilde ayarlamanız gerekir.

+ Null değerler için [sıralanmış sonuçlar](search-query-odata-orderby.md) bu sürümde değiştirilmiştir, sıralama ve sıralama ise son ' a göre null değerleri ilk görünür `asc` `desc` . Null değerlerin nasıl sıralanacağını işlemek için kod aldıysanız, artık gerekmiyorsa bu kodu gözden geçirmeniz ve potansiyel olarak kaldırmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure.Search.Documtalar paketi](https://www.nuget.org/packages/Azure.Search.Documents/)
+ [GitHub’daki örnekler](https://github.com/azure/azure-sdk-for-net/tree/Azure.Search.Documents_11.0.0/sdk/search/Azure.Search.Documents/samples)
+ [Azure.Search.Document API başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/search.documents-readme?view=azure-dotnet)