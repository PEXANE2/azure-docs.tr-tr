---
title: Azure Arama .NET SDK sürümü 1.1'e yükseltme
titleSuffix: Azure Cognitive Search
description: Kodu eski API sürümlerinden Azure Arama .NET SDK sürüm 1.1'e geçirin. Yenilikleri ve hangi kod değişikliklerinin gerekli olduğunu öğrenin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 159aaa8424c3d7a711b587464b80696929f02186
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72792392"
---
# <a name="upgrade-to-azure-search-net-sdk-version-11"></a>Azure Arama .NET SDK sürümü 1.1'e yükseltme

[Azure Arama .NET SDK](https://aka.ms/search-sdk)sürümü 1.0.2-önizleme veya daha eski kullanıyorsanız, bu makale, sürüm 1.1'i kullanmak üzere uygulamanızı yükseltmenize yardımcı olur.

Örnekler de dahil olmak üzere SDK'nın daha genel bir gözden geçirme için [,.NET Uygulamasından Azure Arama'nın nasıl kullanılacağına](search-howto-dotnet-sdk.md)bakın.

> [!NOTE]
> Sürüm 1.1'e yükselttikten veya 1.1 ile 2.0 önizleme dahil bir sürümünü zaten kullanıyorsanız, sürüm 3'e yükseltmeniz gerekir. Talimatlar için [Azure Arama .NET SDK sürüm 3'e yükseltme](search-dotnet-sdk-migration.md) ye bakın.
>

İlk olarak, NuGet `Microsoft.Azure.Search` Paket Yöneticisi Konsolunu kullanmak veya proje referanslarınıza sağ tıklayarak ve "NuGet Paketlerini Yönet"i seçerek NuGet başvurunuzu güncelleyin. Visual Studio'da.

NuGet yeni paketleri ve bunların bağımlılıklarını indirdikten sonra projenizi yeniden oluşturun.

Daha önce sürüm 1.0.0-preview, 1.0.1-preview veya 1.0.2-preview kullanıyorsanız, yapı başarılı olmalı ve gitmeye hazırsınız!

Daha önce sürüm 0.13.0-preview veya daha büyük kullanıyorsanız, aşağıdaki gibi yapı hataları görmeniz gerekir:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Bir sonraki adım, yapı hatalarını tek tek düzeltmektir. Çoğu, SDK'da yeniden adlandırılabilen bazı sınıf ve yöntem adlarının değiştirilmesini gerektirir. [Sürüm 1.1'deki son dakika değişikliklerinin](#ListOfChangesV1) listesi, bu ad değişikliklerinin bir listesini içerir.

Belgelerinizi modellemek için özel sınıflar kullanıyorsanız ve bu sınıfların boş verilemez ilkel `int` `bool` türler (örneğin veya C#' da) özellikleri varsa, SDK'nın 1.1 sürümünde farkında olmanızı gerektiren bir hata düzeltmesi vardır. Daha fazla ayrıntı için [sürüm 1.1'deki Hata düzeltmeleri'ne](#BugFixesV1) bakın.

Son olarak, herhangi bir yapı hatasını düzelttikte, isterseniz yeni işlevselliklerden yararlanmak için uygulamanızda değişiklikler yapabilirsiniz.

<a name="ListOfChangesV1"></a>

## <a name="list-of-breaking-changes-in-version-11"></a>Sürüm 1.1'deki son dakika değişiklikleri listesi
Aşağıdaki liste, değişikliğin uygulama kodunuzu etkileme olasılığına göre sıralanır.

### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch ve IndexAction değişiklikleri
`IndexBatch.Create`olarak yeniden adlandırılmıştır `IndexBatch.New` ve artık `params` bir bağımsız değişkeni yoktur. Farklı eylem `IndexBatch.New` türlerini (birleştirmeler, silmeler, vb.) karıştıran toplu iş ler için kullanabilirsiniz. Buna ek olarak, tüm eylemlerin aynı olduğu toplu iş oluşturmak `Delete` `Merge`için `MergeOrUpload`yeni `Upload`statik yöntemler vardır: , , , ve .

`IndexAction`artık kamu yapıcılar ve özellikleri artık değişmez vardır. Farklı amaçlar için eylemler oluşturmak için yeni `Delete`statik `Merge` `MergeOrUpload`yöntemleri `Upload`kullanmalısınız: , , ve . `IndexAction.Create`Kaldırıldı. Yalnızca bir belge gerektiren aşırı yüklemeyi kullandıysanız, `Upload` bunun yerine kullandığınızdan emin olun.

#### <a name="example"></a>Örnek
Kodunuz şuna benziyorsa:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Herhangi bir yapı hatasını düzeltmek için bunu şu şekilde değiştirebilirsiniz:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

İsterseniz, bunu daha da basitleştirebilirsiniz:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

### <a name="indexbatchexception-changes"></a>IndexBatchException değişiklikleri
Özellik `IndexBatchException.IndexResponse` ,' olarak `IndexingResults`yeniden adlandırıldı ve `IList<IndexingResult>`türü şimdi .

#### <a name="example"></a>Örnek
Kodunuz şuna benziyorsa:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Herhangi bir yapı hatasını düzeltmek için bunu şu şekilde değiştirebilirsiniz:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

### <a name="operation-method-changes"></a>İşlem yöntemi değişiklikleri
Azure Arama .NET SDK'daki her işlem, senkron ve eşzamanlı arayanlar için aşırı yükleme yöntemi kümesi olarak ortaya çıkarır. Bu yöntemin imzaları ve faktoringi sürüm 1.1'de aşırı yüklenmiştir.

Örneğin, SDK'nın eski sürümlerindeki "Dizin İstatistikleri Al" işlemi şu imzaları ortaya çıkardı:

`IIndexOperations` içinde:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

`IndexOperationsExtensions` içinde:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Sürüm 1.1'de aynı işlemin yöntem imzaları aşağıdaki gibi görünür:

`IIndexesOperations` içinde:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

`IndexesOperationsExtensions` içinde:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Azure Search .NET SDK, sürüm 1.1 ile başlayarak işlem yöntemlerini farklı şekilde düzenler:

* İsteğe bağlı parametreler artık ek yöntem aşırı yüklemeleri yerine varsayılan parametreler olarak modellenmiştir. Bu yöntem aşırı, bazen önemli ölçüde sayısını azaltır.
* Uzantı yöntemleri artık arayandan HTTP'nin gereksiz ayrıntılarını gizler. Örneğin, SDK'nın eski sürümleri, işlem yöntemleri hata gösteren herhangi bir durum kodu için atadığından, `CloudException` genellikle denetlemeniz gerekmeyen, HTTP durum koduna sahip bir yanıt nesnesi döndürür. Yeni uzantı yöntemleri, model nesnelerini döndürerek, bunları kodunuzda açma zahmetinden tasarruf sağlar.
* Tersine, çekirdek arabirimleri artık ihtiyacınız olduğunda HTTP düzeyinde daha fazla denetim sağlayan yöntemleri ortaya çıkarır. Artık isteklere dahil edilecek özel HTTP üstbilgilerini geçirebilirsiniz `AzureOperationResponse<T>` ve yeni iade türü `HttpRequestMessage` `HttpResponseMessage` size operasyona ve operasyona doğrudan erişim sağlar. `AzureOperationResponse``Microsoft.Rest.Azure` ad alanında tanımlanır ve `Hyak.Common.OperationResponse`değiştirilir.

### <a name="scoringparameters-changes"></a>Puanlama Parametreleri değişiklikleri
Bir arama `ScoringParameter` sorgusundaki puanlama profillerine parametreler inanmasını kolaylaştırmak için en son SDK'ya yeni bir sınıf eklendi. Daha önce `ScoringProfiles` `SearchParameters` sınıfın özelliği olarak `IList<string>`yazıldı; Şimdi olarak `IList<ScoringParameter>`yazılır .

#### <a name="example"></a>Örnek
Kodunuz şuna benziyorsa:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Herhangi bir yapı hatasını düzeltmek için bunu şu şekilde değiştirebilirsiniz: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

### <a name="model-class-changes"></a>Model sınıfı değişiklikleri
[İşlem yöntemi değişikliklerinde](#OperationMethodChanges)açıklanan imza değişiklikleri nedeniyle, `Microsoft.Azure.Search.Models` ad alanındaki birçok sınıfın adı yeniden adlandırıldı veya kaldırıldı. Örnek:

* `IndexDefinitionResponse`tarafından değiştirilmiştir`AzureOperationResponse<Index>`
* `DocumentSearchResponse`, `DocumentSearchResult` olarak yeniden adlandırıldı
* `IndexResult`, `IndexingResult` olarak yeniden adlandırıldı
* `Documents.Count()`şimdi bir `long` belge sayısı yerine bir döndürür`DocumentCountResponse`
* `IndexGetStatisticsResponse`, `IndexGetStatisticsResult` olarak yeniden adlandırıldı
* `IndexListResponse`, `IndexListResult` olarak yeniden adlandırıldı

Özetlemek `OperationResponse`gerekirse, yalnızca bir model nesnesini sarmak için var olan -türetilmiş sınıflar kaldırıldı. Kalan sınıfların sonekleri 'den `Response` `Result`'e değiştirildi.

#### <a name="example"></a>Örnek
Kodunuz şuna benziyorsa:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Herhangi bir yapı hatasını düzeltmek için bunu şu şekilde değiştirebilirsiniz:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

#### <a name="response-classes-and-ienumerable"></a>Yanıt sınıfları ve ImEnumerable
Kodunuzu etkileyebilecek ek bir değişiklik, koleksiyonları tutan yanıt `IEnumerable<T>`sınıflarının artık uygulamaz olmasıdır. Bunun yerine, toplama özelliğine doğrudan erişebilirsiniz. Örneğin, kodunuz şu şekilde görünüyorsa:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Herhangi bir yapı hatasını düzeltmek için bunu şu şekilde değiştirebilirsiniz:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

#### <a name="special-case-for-web-applications"></a>Web uygulamaları için özel durum
Arama sonuçlarını tarayıcıya `DocumentSearchResponse` göndermek için doğrudan seri hale getiren bir web uygulamanız varsa, kodunuzu değiştirmeniz gerekir veya sonuçlar doğru seriye dönüşmez. Örneğin, kodunuz şu şekilde görünüyorsa:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Arama sonucu oluşturmayı `.Results` düzeltmek için arama yanıtının özelliğini alarak değiştirebilirsiniz:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Bu tür durumları kodunuzda kendiniz aramak zorunda kalabilirsiniz; Derleyici türü `object`nden olduğu `JsonResult.Data` **için sizi uyarmayacak.**

### <a name="cloudexception-changes"></a>CloudException değişiklikleri
Sınıf `CloudException` ad alanından `Hyak.Common` `Microsoft.Rest.Azure` ad alanına taşındı. Ayrıca, `Error` özelliği `Body`.

### <a name="searchserviceclient-and-searchindexclient-changes"></a>SearchServiceClient ve SearchIndexClient değişiklikleri
Özelliğin `Credentials` türü taban `SearchCredentials` sınıfına değişti. `ServiceClientCredentials` Eğer `SearchCredentials` bir `SearchIndexClient` veya `SearchServiceClient`erişmeniz gerekiyorsa, lütfen `SearchCredentials` yeni özelliği kullanın.

SDK eski sürümlerinde, `SearchServiceClient` `SearchIndexClient` ve bir `HttpClient` parametre aldı yapıcılar vardı. Bunlar, bir `HttpClientHandler` ve bir dizi `DelegatingHandler` nesne alan yapıcılarla değiştirilmiştir. Bu, gerekirse HTTP isteklerini önceden işlemek için özel işleyicileri yüklemeyi kolaylaştırır.

Son olarak, bir `Uri` aldı ve `SearchCredentials` değişti yapıcılar. Örneğin, şuna benzeyen bir kodunuz varsa:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Herhangi bir yapı hatasını düzeltmek için bunu şu şekilde değiştirebilirsiniz:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Ayrıca kimlik bilgileri parametresi türü ' `ServiceClientCredentials`nin . Bu, `ServiceClientCredentials`türetilmiş olduğundan `SearchCredentials` kodunuzu etkilemesi olası değildir.

### <a name="passing-a-request-id"></a>İstek kimliğini geçirme
SDK'nın eski sürümlerinde, bir istek kimliği `SearchServiceClient` `SearchIndexClient` ayarlayabilirsiniz veya REST API'ye yapılan her isteğe dahil edilir. Bu, desteğe başvurmanız gerekiyorsa arama hizmetinizdeki sorun giderme sorunları için yararlıdır. Ancak, tüm işlemler için aynı kimliği kullanmak yerine her işlem için benzersiz bir istek kimliği ayarlamak daha yararlıdır. Bu nedenle, `SetClientRequestId` yöntemleri `SearchServiceClient` ve `SearchIndexClient` kaldırılmıştır. Bunun yerine, isteğe bağlı `SearchRequestOptions` parametre aracılığıyla her işlem yöntemine bir istek kimliği geçirebilirsiniz.

> [!NOTE]
> SDK'nın gelecekteki sürümünde, istemci nesnelerinde diğer Azure SDK'ları tarafından kullanılan yaklaşımla tutarlı bir istek kimliği ayarlamak için yeni bir mekanizma ekleyeceğiz.
> 
> 

### <a name="example"></a>Örnek
Şuna benzeyen bir kodunuz varsa:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Herhangi bir yapı hatasını düzeltmek için bunu şu şekilde değiştirebilirsiniz:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

### <a name="interface-name-changes"></a>Arabirim adı değişiklikleri
İşlem grubu arabirimi adlarının tümü, karşılık gelen özellik adlarıyla tutarlı olacak şekilde değiştirildi:

* Türü `ISearchServiceClient.Indexes` ' den `IIndexOperations` ' e `IIndexesOperations`yeniden adlandırıldı
* Türü `ISearchServiceClient.Indexers` ' den `IIndexerOperations` ' e `IIndexersOperations`yeniden adlandırıldı
* Türü `ISearchServiceClient.DataSources` ' den `IDataSourceOperations` ' e `IDataSourcesOperations`yeniden adlandırıldı
* Türü `ISearchIndexClient.Documents` ' den `IDocumentOperations` ' e `IDocumentsOperations`yeniden adlandırıldı

Bu arabirimlerle test amacıyla alay lar oluşturmadığınız sürece, bu değişikliğin kodunuzu etkilemesi olası değildir.

<a name="BugFixesV1"></a>

## <a name="bug-fixes-in-version-11"></a>Sürüm 1.1'deki hata düzeltmeleri
Azure Search .NET SDK'nın eski sürümlerinde özel model sınıflarının serileştirilmesiyle ilgili bir hata vardı. Geçersiz bir değer türü özelliğine sahip özel bir model sınıfı oluşturduysanız hata oluşabilir.

### <a name="steps-to-reproduce"></a>Çoğaltma adımları
Nullable değer türü özelliği ile özel bir model sınıfı oluşturun. Örneğin, `int?`'' `UnitCount` yerine bir `int` tür ortak özellik ekleyin.

Bu tür varsayılan değeri olan bir belgeyi dizine `int`dizine alırsanız (örneğin, 0 için), alan Azure Arama'da geçersiz olur. Daha sonra bu belgeyi ararsanız, `Search` `JsonSerializationException` arama, `null` `int`'ye dönüştürülemez şikayet atar.

Ayrıca, amaçlanan değer yerine dizin için null yazıldığından filtreler beklendiği gibi çalışmayabilir.

### <a name="fix-details"></a>Ayrıntıları düzeltme
Bu sorunu SDK'nın 1.1 sürümünde düzelttik. Şimdi, eğer böyle bir model sınıfı varsa:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

ve 0 `IntValue` olarak ayarlayın, bu değer artık doğru tel 0 olarak serileştirilmiş ve dizin 0 olarak saklanır. Yuvarlak tripping de beklendiği gibi çalışır.

Bu yaklaşımla dikkat edilmesi gereken bir olası sorun vardır: Nullable özelliği olmayan bir model türü kullanıyorsanız, dizininizdeki hiçbir belgenin ilgili alan için null değeri içermediğini **garanti** etmeniz gerekir. Ne SDK ne de Azure Arama REST API'si bunu uygulamanıza yardımcı olmayacaktır.

Bu yalnızca kuramsal bir sorun değildir: Var olan `Edm.Int32` türünde bir dizine yeni bir alan eklediğiniz bir senaryoyu düşünün. Dizin tanımını güncelleştirdikten sonra, tüm belgelerin bu yeni alan için boş bir değeri olur (bunun nedeni, Azure Search'te tüm türlerin boş değer atanabilir olmasıdır). Ardından bu alan için boş değer atanamayan bir `int` özelliğiyle bir model sınıfı kullanırsanız belgeleri almaya çalışırken bunun gibi bir `JsonSerializationException` alırsınız:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Bu nedenle, model sınıflarınızda geçersiz türleri en iyi yöntem olarak kullanmanızı öneririz.

Bu hata ve düzeltme hakkında daha fazla bilgi için lütfen [GitHub'da bu soruna](https://github.com/Azure/azure-sdk-for-net/issues/1063)bakın.

