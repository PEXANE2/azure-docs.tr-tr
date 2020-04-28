---
title: Azure Search .NET SDK sürüm 1,1 ' e yükseltin
titleSuffix: Azure Cognitive Search
description: Kodu eski API sürümlerinden Azure Search .NET SDK sürüm 1,1 ' e geçirin. Nelerin yeni olduğunu ve hangi kod değişikliklerinin gerekli olduğunu öğrenin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 159aaa8424c3d7a711b587464b80696929f02186
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "72792392"
---
# <a name="upgrade-to-azure-search-net-sdk-version-11"></a>Azure Search .NET SDK sürüm 1,1 ' e yükseltin

[Azure Search .NET SDK 'sının](https://aka.ms/search-sdk)sürüm 1.0.2-Preview veya daha eski bir sürümünü kullanıyorsanız, bu makale uygulamanızı 1,1 sürümünü kullanacak şekilde yükseltmenize yardımcı olur.

Örnek içeren SDK hakkında daha genel bir anlatım için bkz. [.NET uygulamasından Azure Search kullanma](search-howto-dotnet-sdk.md).

> [!NOTE]
> Sürüm 1,1 ' e yükselttikten sonra veya 1,1 ile 2,0-Önizleme arasında zaten bir sürüm kullanıyorsanız, sürüm 3 ' e yükseltmeniz gerekir. Yönergeler için bkz. [.NET SDK sürüm 3 Azure Search yükseltme](search-dotnet-sdk-migration.md) .
>

İlk olarak, NuGet ' i paket `Microsoft.Azure.Search` Yöneticisi konsolu 'nu kullanarak veya proje başvurularınızı sağ tıklatıp "NuGet Paketlerini Yönet..." seçeneğini belirleyerek NuGet başvurunuz ' ı güncelleştirin. Visual Studio 'da.

NuGet yeni paketleri ve bağımlılıklarını indirdikten sonra projenizi yeniden derleyin.

Daha önce 1.0.0-Preview, 1.0.1-Preview veya 1.0.2-Preview sürümünü kullanıyorsanız, derleme başarılı olur ve başlamaya hazırsınız!

Daha önce 0.13.0-Preview veya daha eski sürümü kullanıyorsanız, aşağıdaki gibi derleme hataları görmeniz gerekir:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Sonraki adım derleme hatalarını tek tek düzeltemedi. Çoğu, SDK 'da yeniden adlandırılmış bazı sınıf ve yöntem adlarının değiştirilmesini gerektirir. [Sürüm 1,1 ' deki son değişikliklerin listesi](#ListOfChangesV1) , bu ad değişikliklerinin bir listesini içerir.

Belgelerinizi modellemek için özel sınıflar kullanıyorsanız ve bu sınıfların null yapılamayan temel türlerin özellikleri varsa (örneğin, ya `int` `bool` da C# ' de), bilmeniz gereken SDK 'nın 1,1 sürümünde bir hata düzeltildi. Daha fazla bilgi için bkz. [sürüm 1,1 ' de hata düzeltmeleri](#BugFixesV1) .

Son olarak, herhangi bir yapı hatasını düzelttikten sonra, isterseniz yeni işlevlerden yararlanmak için uygulamanızda değişiklikler yapabilirsiniz.

<a name="ListOfChangesV1"></a>

## <a name="list-of-breaking-changes-in-version-11"></a>Sürüm 1,1 ' daki son değişikliklerin listesi
Aşağıdaki liste, değişikliğin uygulama kodunuzu etkileyeceğine göre sıralanır.

### <a name="indexbatch-and-indexaction-changes"></a>Indexbatch ve ındexaction değişiklikleri
`IndexBatch.Create`, olarak `IndexBatch.New` yeniden adlandırıldı ve artık bir `params` bağımsız değişkeni yoktur. Farklı eylem türlerini `IndexBatch.New` (birleştirme, silme, vb.) karıştıraş toplu işler için kullanabilirsiniz. Ayrıca, tüm eylemlerin aynı olduğu durumlarda toplu iş oluşturmak için yeni statik yöntemler vardır `Delete`:, `Merge` `MergeOrUpload`, ve. `Upload`

`IndexAction`Artık ortak oluşturucular yoktur ve özellikleri artık sabit değildir. Farklı amaçlara yönelik eylemler oluşturmak için yeni statik yöntemleri kullanmanız gerekir `Delete`:, `Merge` `MergeOrUpload`, ve. `Upload` `IndexAction.Create`kaldırıldı. Yalnızca bir belge alan aşırı yüklemeyi kullandıysanız, `Upload` bunun yerine kullandığınızdan emin olun.

#### <a name="example"></a>Örnek
Kodunuz şuna benziyorsa:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Herhangi bir derleme hatasını çözebilmeniz için bunu bu şekilde değiştirebilirsiniz:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

İsterseniz bunu daha da kolaylaştırmaya devam edebilirsiniz:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

### <a name="indexbatchexception-changes"></a>Indexbatchexception değişiklikleri
`IndexBatchException.IndexResponse` Özelliği olarak `IndexingResults`yeniden adlandırıldı ve türü artık `IList<IndexingResult>`.

#### <a name="example"></a>Örnek
Kodunuz şuna benziyorsa:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Herhangi bir derleme hatasını çözebilmeniz için bunu bu şekilde değiştirebilirsiniz:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

### <a name="operation-method-changes"></a>İşlem yöntemi değişiklikleri
Azure Search .NET SDK içindeki her işlem, zaman uyumlu ve zaman uyumsuz çağıranlar için bir dizi yöntem aşırı yüklemesi olarak sunulur. Bu yöntem aşırı yüklemelerinin imzaları ve düzenleme sürümü 1,1 sürümünde değiştirilmiştir.

Örneğin, SDK 'nın eski sürümlerindeki "Dizin Istatistiklerini al" işlemi şu imzaları kullanıma sunuldu:

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

Sürüm 1,1 ' de aynı işleme yönelik yöntem imzaları şuna benzer:

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

Sürüm 1,1 ' den başlayarak, .NET SDK Azure Search işlem yöntemlerini farklı şekilde düzenler:

* İsteğe bağlı parametreler artık ek yöntem aşırı yüklemeleri yerine varsayılan parametreler olarak modellenir. Bu, bazen önemli ölçüde önemli ölçüde yöntem aşırı yükleme sayısını azaltır.
* Uzantı yöntemleri artık, çağıranın çok sayıda HTTP ayrıntılarını gizler. Örneğin, SDK 'nın eski sürümleri, genellikle bir hata belirten herhangi bir durum kodu `CloudException` için işlem yöntemleri oluşturduğundan, bir http durum kodu ile bir yanıt nesnesi döndürdü. Yeni uzantı yöntemleri yalnızca model nesnelerini döndürmekte ve bunları kodunuzda sarmalamak zorunda kalmadan yapmanız gereken bir sorun yaşıyor.
* Buna karşılık, çekirdek arabirimler artık ihtiyacınız olduğunda HTTP düzeyinde daha fazla denetim sağlayan yöntemleri kullanıma sunar. Artık isteklere dahil edilecek özel HTTP üstbilgilerini geçirebilir ve yeni `AzureOperationResponse<T>` dönüş türü, `HttpRequestMessage` işlem için ve `HttpResponseMessage` için doğrudan erişim sağlar. `AzureOperationResponse``Microsoft.Rest.Azure` ad alanında tanımlanır ve değiştirilir `Hyak.Common.OperationResponse`.

### <a name="scoringparameters-changes"></a>Değişiklikleri ScoringParameters
Bir arama sorgusunda Puanlama `ScoringParameter` profillerine parametre sağlamayı kolaylaştırmak için en son SDK 'ya adlı yeni bir sınıf eklenmiştir. Daha önce `ScoringProfiles` `SearchParameters` sınıfının özelliği olarak `IList<string>`yazılmış; Artık olarak `IList<ScoringParameter>`yazılmış.

#### <a name="example"></a>Örnek
Kodunuz şuna benziyorsa:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Herhangi bir derleme hatasını çözebilmeniz için bunu bu şekilde değiştirebilirsiniz: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

### <a name="model-class-changes"></a>Model sınıfı değişiklikleri
[İşlem yöntemi değişikliklerinde](#OperationMethodChanges)açıklanan imza değişikliklerinden dolayı, `Microsoft.Azure.Search.Models` ad alanındaki birçok sınıf yeniden adlandırıldı veya kaldırılmıştır. Örneğin:

* `IndexDefinitionResponse`değiştirdi`AzureOperationResponse<Index>`
* `DocumentSearchResponse`, `DocumentSearchResult` olarak yeniden adlandırıldı
* `IndexResult`, `IndexingResult` olarak yeniden adlandırıldı
* `Documents.Count()``long` şimdi bir değil, belge sayısı`DocumentCountResponse`
* `IndexGetStatisticsResponse`, `IndexGetStatisticsResult` olarak yeniden adlandırıldı
* `IndexListResponse`, `IndexListResult` olarak yeniden adlandırıldı

Özetlemek gerekirse, `OperationResponse`yalnızca bir model nesnesini kaydırmak için varolan türetilmiş sınıflar kaldırılmıştır. Kalan sınıfların soneki `Response` olarak `Result`değiştirildi.

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

Herhangi bir derleme hatasını çözebilmeniz için bunu bu şekilde değiştirebilirsiniz:

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

#### <a name="response-classes-and-ienumerable"></a>Yanıt sınıfları ve IEnumerable
Kodunuzu etkileyebilecek ek bir değişiklik, koleksiyonları tutan yanıt sınıflarının artık uygulamalarıdır `IEnumerable<T>`. Bunun yerine, doğrudan koleksiyon özelliğine erişebilirsiniz. Örneğin, kodunuz şuna benziyorsa:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Herhangi bir derleme hatasını çözebilmeniz için bunu bu şekilde değiştirebilirsiniz:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

#### <a name="special-case-for-web-applications"></a>Web uygulamaları için özel durum
`DocumentSearchResponse` Doğrudan tarayıcıya arama sonuçları göndermek için seri hale getirilen bir Web uygulamanız varsa, kodunuzu değiştirmeniz gerekir, aksi takdirde sonuçlar doğru serileştirmez. Örneğin, kodunuz şuna benziyorsa:

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

Arama sonucu işlemesini onarmak için arama yanıtının `.Results` özelliğini alarak bunu değiştirebilirsiniz:

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

Kodunuzda bu tür durumları aramanız gerekecektir; Bir tür `JsonResult.Data` `object`olduğundan **derleyici sizi uyarmaz** .

### <a name="cloudexception-changes"></a>CloudException değişiklikleri
`CloudException` Sınıf `Hyak.Common` ad alanından ad `Microsoft.Rest.Azure` alanına taşındı. Ayrıca, `Error` özelliği olarak `Body`yeniden adlandırıldı.

### <a name="searchserviceclient-and-searchindexclient-changes"></a>SearchServiceClient ve Searchındexclient değişiklikleri
`Credentials` Özelliğin türü, `ServiceClientCredentials`temel sınıfı olan `SearchCredentials` olarak değiştirildi. `SearchCredentials` Bir veya `SearchIndexClient` `SearchServiceClient`' a erişmeniz gerekiyorsa, lütfen yeni `SearchCredentials` özelliği kullanın.

SDK 'nın eski sürümlerinde `SearchServiceClient` ve `SearchIndexClient` bir `HttpClient` parametre alan oluşturucular vardı. Bunlar, bir `HttpClientHandler` dizi `DelegatingHandler` nesne alan oluşturucular ile değiştirilmiştir. Bu, gerekirse HTTP isteklerini önceden işlemek için özel işleyiciler yüklemeyi kolaylaştırır.

Son olarak, `Uri` ve `SearchCredentials` kullanan oluşturucular değişmiştir. Örneğin, aşağıdakine benzer bir kodunuz varsa:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Herhangi bir derleme hatasını çözebilmeniz için bunu bu şekilde değiştirebilirsiniz:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Ayrıca, kimlik bilgileri parametresinin türünün olarak `ServiceClientCredentials`değiştirildiğini unutmayın. Bu, öğesinden `SearchCredentials` `ServiceClientCredentials`türetildiğinden kodunuzun etkilenmemesi olası değildir.

### <a name="passing-a-request-id"></a>İstek KIMLIĞI geçirme
SDK 'nın eski sürümlerinde, `SearchServiceClient` veya `SearchIndexClient` ' de bir istek kimliği ayarlayabilir ve REST API her isteğine dahil edilebilir. Bu, desteğe başvurmanız gerekirse arama hizmetinizdeki sorunları gidermeye yarar. Ancak, tüm işlemler için aynı KIMLIĞI kullanmak yerine her işlem için benzersiz bir istek KIMLIĞI ayarlamak daha yararlıdır. Bu nedenle, `SetClientRequestId` `SearchServiceClient` ve `SearchIndexClient` yöntemleri kaldırılmıştır. Bunun yerine, isteğe bağlı `SearchRequestOptions` parametre aracılığıyla her bir işlem yöntemine BIR istek kimliği geçirebilirsiniz.

> [!NOTE]
> SDK 'nın gelecek bir sürümünde, diğer Azure SDK 'Ları tarafından kullanılan yaklaşımla tutarlı olan istemci nesnelerinde bir istek KIMLIĞINI genel olarak ayarlamaya yönelik yeni bir mekanizma ekleyeceğiz.
> 
> 

### <a name="example"></a>Örnek
Aşağıdakine benzer bir kodunuz varsa:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Herhangi bir derleme hatasını çözebilmeniz için bunu bu şekilde değiştirebilirsiniz:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

### <a name="interface-name-changes"></a>Arabirim adı değişiklikleri
İşlem grubu arabirim adlarının hepsi karşılık gelen özellik adlarıyla tutarlı olacak şekilde değişti:

* Türü `ISearchServiceClient.Indexes` , `IIndexOperations` olarak `IIndexesOperations`yeniden adlandırıldı.
* Türü `ISearchServiceClient.Indexers` , `IIndexerOperations` olarak `IIndexersOperations`yeniden adlandırıldı.
* Türü `ISearchServiceClient.DataSources` , `IDataSourceOperations` olarak `IDataSourcesOperations`yeniden adlandırıldı.
* Türü `ISearchIndexClient.Documents` , `IDocumentOperations` olarak `IDocumentsOperations`yeniden adlandırıldı.

Bu arabirimlerin test amaçları doğrultusunda bu arabirimlerin bir listesini oluşturmadığınız müddetçe bu değişiklik, kodunuzun etkilenmemesi için olası bir olasılıktır.

<a name="BugFixesV1"></a>

## <a name="bug-fixes-in-version-11"></a>Sürüm 1,1 ' de hata düzeltmeleri
Azure Search .NET SDK 'sının özel model sınıflarının serileştirilmesi ile ilgili eski sürümlerinde bir hata oluştu. Null olamayan bir değer türünün özelliği ile özel bir model sınıfı oluşturduysanız hata oluşabilir.

### <a name="steps-to-reproduce"></a>Yeniden oluşturma adımları
Null olamayan değer türünde bir özelliği olan özel bir model sınıfı oluşturun. Örneğin, yerine türünde `UnitCount` `int` bir public özelliği ekleyin. `int?`

Bir belgeyi bu türün varsayılan değeriyle dizinederseniz (örneğin, için `int`0), alan Azure Search null olur. `Search` Bu belgeyi daha sonra arıyorsanız çağrı, dönüştüremeyecek `JsonSerializationException` `null` şikayetler oluşturur. `int`

Ayrıca, istenen değer yerine null dizine yazıldığı için filtreler beklendiği gibi çalışmayabilir.

### <a name="fix-details"></a>Ayrıntıları giderme
Bu sorunu SDK sürüm 1,1 ' de düzelttik. Şimdi şöyle bir model sınıfınız varsa:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

0 olarak ayarlarsanız `IntValue` , bu değer artık kabloda 0 olarak doğru şekilde serileştirilir ve dizinde 0 olarak depolanır. Yuvarlak Daire ayrıca beklendiği gibi da çalışmaktadır.

Bu yaklaşımla farkında olmak için olası bir sorun var: null yapılamayan bir özellik ile model türü kullanırsanız, dizininizdeki hiçbir belgenin karşılık gelen alan için null değer **içermediğinden emin olmanız** gerekir. SDK ne de Azure Search REST API bunu zorunlu etmenize yardımcı olur.

Bu yalnızca kuramsal bir sorun değildir: Var olan `Edm.Int32` türünde bir dizine yeni bir alan eklediğiniz bir senaryoyu düşünün. Dizin tanımını güncelleştirdikten sonra, tüm belgelerin bu yeni alan için boş bir değeri olur (bunun nedeni, Azure Search'te tüm türlerin boş değer atanabilir olmasıdır). Ardından bu alan için boş değer atanamayan bir `int` özelliğiyle bir model sınıfı kullanırsanız belgeleri almaya çalışırken bunun gibi bir `JsonSerializationException` alırsınız:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Bu nedenle, en iyi uygulama olarak model sınıflarınızda null yapılabilir türler kullanmanızı öneririz.

Bu hata ve düzeltmeyle ilgili daha fazla bilgi için lütfen [GitHub 'da Bu soruna](https://github.com/Azure/azure-sdk-for-net/issues/1063)bakın.

