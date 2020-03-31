---
title: Azure Bilişsel Arama'yı .NET'te kullanma
titleSuffix: Azure Cognitive Search
description: C# ve .NET SDK'yı kullanarak bir .NET uygulamasında Azure Bilişsel Arama'yı nasıl kullanacağınızı öğrenin. Kod tabanlı görevler, hizmete bağlanmayı, dizin içeriğini ve dizin sorgusunu içerir.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.devlang: dotnet
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: b31a4e40c1e9095499faf265673ab4213ad6bde0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283075"
---
# <a name="how-to-use-azure-cognitive-search-from-a-net-application"></a>Bir .NET Uygulamasından Azure Bilişsel Arama nasıl kullanılır?

Bu makale, [Azure Bilişsel Arama .NET SDK](https://aka.ms/search-sdk)ile çalışmaya devam etmek için bir izdir. Azure Bilişsel Arama'yı kullanarak uygulamanızda zengin bir arama deneyimi uygulamak için .NET SDK'yı kullanabilirsiniz.

## <a name="whats-in-the-azure-cognitive-search-sdk"></a>Azure Bilişsel Arama SDK'sında neler var
SDK, dizinlerinizi, veri kaynaklarınızı, dizin leyicilerinizi ve eşanlamlı haritalarınızı yönetmenize, belgeleri yüklemenize ve yönetmeninize ve sorguları, http ve JSON'un ayrıntılarıyla uğraşmak zorunda kalmadan yürütmenize olanak tanıyan birkaç istemci kitaplığından oluşur. Bu istemci kitaplıklarının tümü NuGet paketleri olarak dağıtılır.

Ana NuGet paketi, bağımlılık olarak tüm diğer paketleri içeren bir meta paketidir. `Microsoft.Azure.Search` Daha yeni başlıyorsanız veya uygulamanızın Azure Bilişsel Arama'nın tüm özelliklerine ihtiyaç dedeceğini biliyorsanız bu paketi kullanın.

SDK'daki diğer NuGet paketleri şunlardır:
 
  - `Microsoft.Azure.Search.Data`: Azure Bilişsel Arama'yı kullanarak bir .NET uygulaması geliştiriyorsanız ve yalnızca dizinlerinizdeki belgeleri sorgulamanız veya güncelleştirmeniz gerekiyorsa bu paketi kullanın. Dizinler, eşanlamlı haritalar veya diğer hizmet düzeyi kaynakları oluşturmanız veya güncelleştirmeniz gerekiyorsa, `Microsoft.Azure.Search` bunun yerine paketi kullanın.
  - `Microsoft.Azure.Search.Service`: Azure Bilişsel Arama dizinlerini, eşanlamlı haritaları, dizin leyicileri, veri kaynaklarını veya diğer hizmet düzeyi kaynakları yönetmek için .NET'te otomasyon geliştiriyorsanız bu paketi kullanın. Yalnızca dizinlerinizdeki belgeleri sorgulamanız veya güncelleştirmeniz `Microsoft.Azure.Search.Data` gerekiyorsa, bunun yerine paketi kullanın. Azure Bilişsel Arama'nın tüm işlevlerine `Microsoft.Azure.Search` ihtiyacınız varsa, bunun yerine paketi kullanın.
  - `Microsoft.Azure.Search.Common`: Azure Bilişsel Arama .NET kitaplıkları tarafından gereken yaygın türler. Bu paketi doğrudan uygulamanızda kullanmanız gerekmez. Sadece bir bağımlılık olarak kullanılmak üzere dir.

Çeşitli istemci kitaplıkları `Index` `Field`gibi `Document`sınıfları tanımlar , `Indexes.Create` `Documents.Search` , `SearchServiceClient` ve `SearchIndexClient` , gibi işlemleri gibi ve ve sınıflar. Bu sınıflar aşağıdaki ad alanlarına düzenlenir:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Modelleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

SDK'nın gelecekteki bir güncellemesi için geri bildirim sağlamak istiyorsanız, [geri bildirim sayfamıza](https://feedback.azure.com/forums/263029-azure-search/) bakın veya [GitHub'da](https://github.com/azure/azure-sdk-for-net/issues) bir sorun oluşturun ve sorun başlığında "Azure Bilişsel Arama"dan bahsedin.

.NET SDK, `2019-05-06` Azure [Bilişsel Arama REST API](https://docs.microsoft.com/rest/api/searchservice/)sürümünü destekler. Bu sürüm, Azure Blobs dizinleme karmaşık [türleri,](search-howto-complex-data-types.md) [AI zenginleştirme,](cognitive-search-concept-intro.md) [otomatik tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete)ve [JsonLines ayrıştırma modu](search-howto-index-json-blobs.md) desteği içerir. 

Bu SDK, Arama hizmetleri oluşturma ve ölçekleme ve API anahtarlarını yönetme gibi [Yönetim Işlemlerini](https://docs.microsoft.com/rest/api/searchmanagement/) desteklemez. Arama kaynaklarınızı bir .NET uygulamasından yönetmeniz gerekiyorsa, [Azure Bilişsel Arama .NET Management SDK'yı](https://aka.ms/search-mgmt-sdk)kullanabilirsiniz.

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>SDK'nın en son sürümüne yükseltme
Azure Bilişsel Arama .NET SDK'nın eski bir sürümünü zaten kullanıyorsanız ve genel olarak kullanılabilen en son sürüme yükseltmek istiyorsanız, [bu makalede](search-dotnet-sdk-migration-version-9.md) nasıl olduğu açıklanmaktadır.

## <a name="requirements-for-the-sdk"></a>SDK için gereksinimler
1. Visual Studio 2017 veya sonrası.
2. Kendi Azure Bilişsel Arama hizmetiniz. SDK'yı kullanabilmek için hizmetinizin adını ve bir veya daha fazla API anahtarına ihtiyacınız olacaktır. [Portalda bir hizmet oluşturmak](search-create-service-portal.md) bu adımları size yardımcı olacaktır.
3. Visual Studio'da "NuGet Paketlerini Yönet" dosyasını kullanarak Azure Bilişsel Arama .NET SDK [NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Search) indirin. Yalnızca işlevselliğin bir `Microsoft.Azure.Search` alt kümesine ihtiyacınız varsa, NuGet.org paket adını (veya yukarıdaki diğer paket adlarından birini) aramanız yeterli.

Azure Bilişsel Arama .NET SDK, .NET Framework 4.5.2 ve üzeri uygulamalarıve .NET Core 2.0 ve üzeri uygulamaları destekler.

## <a name="core-scenarios"></a>Temel senaryolar
Arama uygulamanızda yapmanız gereken birkaç şey vardır. Bu öğreticide, bu temel senaryoları ele alacağız:

* Dizin oluşturma
* Dizini belgelerle doldurma
* Tam metin arama ve filtreleri kullanarak belge arama

Aşağıdaki örnek kod, bu senaryoların her birini gösterir. Kendi uygulamanızda kod parçacıklarını kullanmaktan çekinmeyin.

### <a name="overview"></a>Genel Bakış
Araştıracağım örnek uygulama "oteller" adlı yeni bir dizin oluşturur, birkaç belgeyle doldurur ve bazı arama sorgularını yürütür. Burada genel akışı gösteren ana program:

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    string indexName = configuration["SearchIndexName"];

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteIndexIfExists(indexName, serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateIndex(indexName, serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);

    ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> Bu kılavuzda kullanılan örnek uygulamanın tam kaynak kodunu [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) üzerinde bulabilirsiniz.
> 
>

Bu adım adım yürüyeceğiz. Önce yeni `SearchServiceClient`bir şey yaratmalıyız. Bu nesne dizinleri yönetmenize olanak sağlar. Bir tane oluşturmak için Azure Bilişsel Arama hizmet adınızın yanı sıra yönetici API anahtarı sağlamanız gerekir. Bu bilgileri örnek `appsettings.json` [uygulamanın](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo)dosyasına girebilirsiniz.

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

> [!NOTE]
> Yanlış bir anahtar (örneğin, yönetici anahtarının gerekli olduğu bir `SearchServiceClient` sorgu anahtarı) sağlarsanız, üzerinde işlem yöntemini ilk kez çağırdığınızda `Indexes.Create`"Yasak" hata iletisi ile bir hata iletisi verecektir. `CloudException` Eğer bu sizin de olursanız, API anahtarımızı iki kez kontrol edin.
> 
> 

Sonraki birkaç satır, "oteller" adlı bir dizin oluşturmak için yöntemleri çağırır ve zaten varsa önce siler. Bu yöntemlerden biraz daha geç geçacağız.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteIndexIfExists(indexName, serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateIndex(indexName, serviceClient);
```

Ardından, dizin doldurulması gerekir. Endeksi doldurmak için, bir `SearchIndexClient`. Bir elde etmek için iki yolu vardır: inşa `Indexes.GetClient` ederek, ya da çağırarak `SearchServiceClient`. Biz kolaylık için ikinci kullanın.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);
```

> [!NOTE]
> Tipik bir arama uygulamasında, dizin yönetimi ve popülasyon, arama sorgularından ayrı bir bileşen tarafından işlenebilir. `Indexes.GetClient`ek sağlama zahmetinden tasarruf sağladığından, bir dizin `SearchCredentials`doldurmak için uygundur. Yeni `SearchIndexClient` için `SearchServiceClient` oluşturmak üzere kullandığınız yönetici anahtarını geçirerek bunu yapar. Ancak, uygulamanızın sorguları yürüten bölümünde, `SearchIndexClient` doğrudan oluşturmak daha iyidir, böylece bir yönetici anahtarı yerine yalnızca verileri okumanızı sağlayan bir sorgu anahtarı nı geçirebilirsiniz. Bu, en az ayrıcalık ilkesiyle tutarlıdır ve uygulamanızı daha güvenli hale getirmenize yardımcı olur. Yönetici tuşları ve sorgu [tuşları](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization)hakkında daha fazla bilgi bulabilirsiniz.
> 
> 

`SearchIndexClient`Artık endeksi doldurabiliyoruz. İndeks popülasyonu daha sonra yürüyeceğimiz başka bir yöntemle yapılır.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Son olarak, birkaç arama sorgusu yürütmek ve sonuçları görüntüler. Bu sefer farklı `SearchIndexClient`bir kullanın:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(indexName, configuration);

RunQueries(indexClientForQueries);
```

Yönteme `RunQueries` daha yakından bakacağız. Burada yeni `SearchIndexClient`oluşturmak için kod:

```csharp
private static SearchIndexClient CreateSearchIndexClient(string indexName, IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, indexName, new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Dizin yazma erişimine gerek olmadığı için bu kez bir sorgu anahtarı kullanıyoruz. Bu bilgileri örnek `appsettings.json` [uygulamanın](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo)dosyasına girebilirsiniz.

Bu uygulamayı geçerli bir hizmet adı ve API anahtarlarıyla çalıştırarsanız, çıktı şu örnek gibi görünmelidir: (Bazı konsol çıktısı "..." ile değiştirilmiştir. illüstrasyon amaçlıdır.)

    Deleting index...

    Creating index...

    Uploading documents...

    Waiting for documents to be indexed...

    Search the entire index for the term 'motel' and return only the HotelName field:

    Name: Secret Point Motel

    Name: Twin Dome Motel


    Apply a filter to the index to find hotels with a room cheaper than $100 per night, and return the hotelId and description:

    HotelId: 1
    Description: The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Times Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.

    HotelId: 2
    Description: The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.


    Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

    Name: Triple Landscape Hotel
    Last renovated on: 9/20/2015 12:00:00 AM +00:00

    Name: Twin Dome Motel
    Last renovated on: 2/18/1979 12:00:00 AM +00:00


    Search the hotel names for the term 'hotel':

    HotelId: 3
    Name: Triple Landscape Hotel
    ...

    Complete.  Press any key to end application... 

Uygulamanın tam kaynak kodu bu makalenin sonunda sağlanır.

Daha sonra, tarafından `Main`çağrılan yöntemlerin her birine daha yakından bakacağız.

### <a name="creating-an-index"></a>Dizin oluşturma
Bir `SearchServiceClient`oluşturduktan `Main` sonra, zaten varsa "oteller" dizini siler. Bu silme aşağıdaki yöntemle yapılır:

```csharp
private static void DeleteIndexIfExists(string indexName, SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists(indexName))
    {
        serviceClient.Indexes.Delete(indexName);
    }
}
```

Bu yöntem, `SearchServiceClient` dizin var olup olmadığını denetlemek için verilen kullanır ve eğer öyleyse, silin.

> [!NOTE]
> Bu makaledeki örnek kod, basitlik için Azure Bilişsel Arama .NET SDK'nın eşzamanlı yöntemlerini kullanır. Kendi uygulamalarınızda zaman uyumsuz yöntemler kullanarak bunları ölçeklenebilir ve esnek tutmanızı öneririz. Örneğin, yukarıdaki yöntemde kullanabilirsiniz `ExistsAsync` `DeleteAsync` ve `Exists` yerine `Delete`ve .
> 
> 

Ardından, `Main` bu yöntemi çağırarak yeni bir "oteller" dizini oluşturur:

```csharp
private static void CreateIndex(string indexName, SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = indexName,
        Fields = FieldBuilder.BuildForType<Hotel>()
    };
    
    serviceClient.Indexes.Create(definition);
}
```

Bu yöntem, yeni `Index` dizinin şema `Field` tanımlayan nesnelerin bir listesi ile yeni bir nesne oluşturur. Her alanın bir adı, veri türü ve arama davranışını tanımlayan birkaç özniteliği vardır. Sınıf, `FieldBuilder` verilen `Field` `Hotel` model sınıfının ortak özelliklerini ve özniteliklerini inceleyerek dizin için nesnelerin bir listesini oluşturmak için yansımayı kullanır. Daha sonra sınıfa `Hotel` daha yakından bakarız.

> [!NOTE]
> Gerektiğinde kullanmak `Field` `FieldBuilder` yerine nesnelerin listesini her zaman doğrudan oluşturabilirsiniz. Örneğin, bir model sınıfı kullanmak istemeyebilirsiniz veya öznitelikler ekleyerek değiştirmek istemediğiniz varolan bir model sınıfı kullanmanız gerekebilir.
>
> 

Alanlara ek olarak, Dizin'e puanlama profilleri, önerenler veya CORS seçenekleri de ekleyebilirsiniz (bu parametreler kısalık için örnekten çıkarılır). [SDK referansında](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index)ve [Azure Bilişsel Arama REST API referansında](https://docs.microsoft.com/rest/api/searchservice/)Dizin nesnesi ve kurucu parçaları hakkında daha fazla bilgi bulabilirsiniz.

### <a name="populating-the-index"></a>Dizinin doldurulma
Bir sonraki `Main` adım, yeni oluşturulan dizini doldurulur. Bu dizin popülasyonu aşağıdaki yöntemle yapılır: (Bazı kod "..." ile değiştirilir illüstrasyon amaçlı.  Tam veri popülasyon kodu için tam örnek çözüme bakın.)

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
        new Hotel()
        {
            HotelId = "1",
            HotelName = "Secret Point Motel",
            ...
            Address = new Address()
            {
                StreetAddress = "677 5th Ave",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Budget Room, 1 Queen Bed (Cityside)",
                    ...
                },
                new Room()
                {
                    Description = "Budget Room, 1 King Bed (Mountain View)",
                    ...
                },
                new Room()
                {
                    Description = "Deluxe Room, 2 Double Beds (City View)",
                    ...
                }
            }
        },
        new Hotel()
        {
            HotelId = "2",
            HotelName = "Twin Dome Motel",
            ...
            {
                StreetAddress = "140 University Town Center Dr",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Suite, 2 Double Beds (Mountain View)",
                    ...
                },
                new Room()
                {
                    Description = "Standard Room, 1 Queen Bed (City View)",
                    ...
                },
                new Room()
                {
                    Description = "Budget Room, 1 King Bed (Waterfront View)",
                    ...
                }
            }
        },
        new Hotel()
        {
            HotelId = "3",
            HotelName = "Triple Landscape Hotel",
            ...
            Address = new Address()
            {
                StreetAddress = "3393 Peachtree Rd",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Standard Room, 2 Queen Beds (Amenities)",
                    ...
                },
                new Room ()
                {
                    Description = "Standard Room, 2 Double Beds (Waterfront View)",
                    ...
                },
                new Room()
                {
                    Description = "Deluxe Room, 2 Double Beds (Cityside)",
                    ...
                }
            }
        }
    };

    var batch = IndexBatch.Upload(hotels);

    try
    {
        indexClient.Documents.Index(batch);
    }
    catch (IndexBatchException e)
    {
        // Sometimes when your Search service is under load, indexing will fail for some of the documents in
        // the batch. Depending on your application, you can take compensating actions like delaying and
        // retrying. For this simple demo, we just log the failed document keys and continue.
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

    Console.WriteLine("Waiting for documents to be indexed...\n");
    Thread.Sleep(2000);
}
```

Bu yöntem dört bölümden oluşur. İlk 3 `Hotel` nesnelerin her 3 nesneleri bizim `Room` giriş verileri olarak dizin yüklemek için hizmet verecek bir dizi oluşturur. Bu veriler basitlik için sabit kodlanmış. Kendi uygulamanızda, verileriniz büyük olasılıkla SQL veritabanı gibi harici bir veri kaynağından gelecektir.

İkinci bölüm belgeleri `IndexBatch` içeren bir oluşturur. Oluşturduğunuz sırada toplu iş için uygulamak istediğiniz işlemi belirtin, bu `IndexBatch.Upload`durumda . Toplu iş daha sonra `Documents.Index` yöntemtarafından Azure Bilişsel Arama dizinine yüklenir.

> [!NOTE]
> Bu örnekte, sadece belgeleri yüklüyoruz. Değişiklikleri varolan belgelerde birleştirmek veya belgeleri silmek istiyorsanız, `IndexBatch.Merge`"veya `IndexBatch.MergeOrUpload` `IndexBatch.Delete` bunun yerine" çağırarak toplu iş oluşturabilirsiniz. Ayrıca, her biri Azure Bilişsel Arama'ya bir belgede `IndexAction` belirli bir işlemi gerçekleştirmesini söyleyen nesnelerin bir koleksiyonunu alan aramayı `IndexBatch.New`yaparak farklı işlemleri tek bir toplu iş halinde karıştırabilirsiniz. Her biri, `IndexAction` ", ve benzeri" `IndexAction.Merge` `IndexAction.Upload`gibi ilgili yöntemi çağırarak kendi işlemiyle oluşturabilirsiniz.
> 
> 

Bu yöntemin üçüncü bölümü, dizin oluşturma için önemli bir hata durumu işleyen bir catch bloğudur. Azure Bilişsel Arama hizmetiniz toplu işteki bazı belgeleri `IndexBatchException` dizine `Documents.Index`dizine ekmezse, bir . Bu özel durum, hizmetiniz ağır yük altındayken belgeleri dizine ekiyorsanız gerçekleşebilir. **Bu durumu açık bir şekilde kodunuzda işlemenizi kesinlikle öneririz.** Başarısız olan belgelere dizin oluşturmayı geciktirip sonra yeniden deneyebilir veya günlük tutup örneğin devam ettiği şekilde devam edebilir veya uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak başka bir şey yapabilirsiniz.

> [!NOTE]
> [`FindFailedActionsToRetry`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception.findfailedactionstoretry) Yöntemi, yalnızca önceki bir çağrıda başarısız olan eylemleri içeren yeni `Index`bir toplu iş oluşturmak için kullanabilirsiniz. [StackOverflow üzerinde](https://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry)düzgün bir şekilde nasıl kullanılacağı konusunda bir tartışma vardır.
>
>

Son olarak, `UploadDocuments` yöntem iki saniye geciktirir. Dizin oluşturma, Azure Bilişsel Arama hizmetinizde eş zamanlı olarak gerçekleşir, bu nedenle örnek uygulamanın belgelerin arama için kullanılabilir olduğundan emin olmak için kısa bir süre beklemesi gerekir. Bu gibi gecikmeler genellikle yalnızca gösterilerde, testlerde ve örnek uygulamalarda gereklidir.

<a name="how-dotnet-handles-documents"></a>

#### <a name="how-the-net-sdk-handles-documents"></a>.NET SDK belgeleri nasıl işler?
Azure Bilişsel Arama .NET SDK'nın dizin gibi kullanıcı tanımlı bir `Hotel` sınıfın örneklerini nasıl yükleyebildiğini merak ediyor olabilirsiniz. Bu soruyu cevaplamak için, `Hotel` sınıfa bakalım:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsSearchable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.EnLucene)]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("Description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    // SmokingAllowed reflects whether any room in the hotel allows smoking.
    // The JsonIgnore attribute indicates that a field should not be created 
    // in the index for this property and it will only be used by code in the client.
    [JsonIgnore]
    public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? Rating { get; set; }

    public Address Address { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }

    public Room[] Rooms { get; set; }
}
```

Dikkat edilecek ilk şey, `Hotel` sınıftaki her kamu malının adının dizin tanımında aynı ada sahip bir alana eşleneeceğidir. Her alanın küçük harfle ("deve kılıfı") başlamasını istiyorsanız, SDK'ya özellik adlarını sınıftaki `[SerializePropertyNamesAsCamelCase]` öznitelikile otomatik olarak deve kasasına eşlemini söyleyebilirsiniz. Bu senaryo, .NET'teki "Pascal case" adlandırma yönergelerini ihlal etmek zorunda kalmadan, hedef şema uygulama geliştiricisinin denetimi dışında olduğu durumlarda veri bağlama gerçekleştiren .NET uygulamalarında yaygındır.

> [!NOTE]
> Azure Bilişsel Arama .NET SDK, Özel model nesnelerinizi JSON'a ve JSON'dan seri hale getirmek ve deserialize etmek için [NewtonSoft JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) kitaplığını kullanır. Gerekirse bu seri hale getirmeyi özelleştirebilirsiniz. Daha fazla bilgi için JSON.NET [ile Özel Serileştirme'ye](#JsonDotNet)bakın.
> 
> 

Dikkat edilmesi gereken ikinci şey, her özellik `IsFilterable`gibi `IsSearchable` `Key`nitelikleri `Analyzer`ile dekore edilmiştir , , , ve . Bu öznitelikler, [doğrudan bir Azure Bilişsel Arama dizinindeki ilgili alan öznitelikleriyle](/rest/api/searchservice/create-index)eşleşecek. Sınıf `FieldBuilder` dizin için alan tanımları oluşturmak için bu özellikleri kullanır.

`Hotel` Sınıf la ilgili üçüncü önemli şey, ortak özelliklerin veri türleridir. Bu özelliklerin .NET türleri, dizin tanımında eşdeğer alan türleriyle eşlenir. Örneğin, `Category` dize özelliği `Edm.String` türündeki `category` alanına eşlenir. , `bool?`, `Edm.Boolean` `DateTimeOffset?`, ve `Edm.DateTimeOffset` benzeri arasında benzer tür eşlemeleri vardır. Tür eşleme için özel kurallar Azure `Documents.Get` Bilişsel [Arama .NET SDK başvurusundaki](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.get)yöntemle belgelenmiştir. Sınıf `FieldBuilder` sizin için bu eşleme ilgilenir, ancak yine de herhangi bir serileştirme sorunları gidermek gerekir durumda anlamak için yararlı olabilir.

`SmokingAllowed` Mülkü fark ettin mi?

```csharp
[JsonIgnore]
public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;
```

`JsonIgnore` Bu özellikteki öznitelik, `FieldBuilder` dizin için alan olarak serihale değil söyler.  Bu, uygulamanızda yardımcı olarak kullanabileceğiniz istemci tarafı hesaplanmış özellikler oluşturmak için harika bir yoldur.  Bu durumda, `SmokingAllowed` özellik `Rooms` koleksiyonda `Room` herhangi bir sigara izin verir olup olmadığını yansıtır.  Tüm yanlış ise, tüm otel sigara içmesine izin vermez gösterir.

.NET sınıfları gibi `Address` bazı özellikler `Rooms` verilmiştir.  Bu özellikler daha karmaşık veri yapılarını temsil ve sonuç olarak, dizin karmaşık bir [veri türü](https://docs.microsoft.com/azure/search/search-howto-complex-data-types) olan alanlar gerektirir.

Özellik, `Address` sınıfta aşağıda tanımlanan birden `Address` çok değer kümesini temsil eder:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Newtonsoft.Json;

namespace AzureSearch.SDKHowTo
{
    public partial class Address
    {
        [IsSearchable]
        public string StreetAddress { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string City { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string StateProvince { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string PostalCode { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string Country { get; set; }
    }
}
```

Bu sınıf, ABD veya Kanada'daki adresleri tanımlamak için kullanılan standart değerleri içerir. Mantıksal alanları diziniçinde gruplandırmak için bu gibi türleri kullanabilirsiniz.

Özellik `Rooms` bir dizi `Room` nesneyi temsil eder:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Newtonsoft.Json;

namespace AzureSearch.SDKHowTo
{
    public partial class Room
    {
        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.EnMicrosoft)]
        public string Description { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.FrMicrosoft)]
        [JsonProperty("Description_fr")]
        public string DescriptionFr { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string Type { get; set; }

        [IsFilterable, IsFacetable]
        public double? BaseRate { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string BedOptions { get; set; }

        [IsFilterable, IsFacetable]
        public int SleepsCount { get; set; }

        [IsFilterable, IsFacetable]
        public bool? SmokingAllowed { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string[] Tags { get; set; }
    }
}
```

.NET'teki veri modeliniz ve buna karşılık gelen indeks şemasını, son kullanıcınıza vermek istediğiniz arama deneyimini destekleyecek şekilde tasarlanmalıdır. .NET'teki her üst düzey nesne, yani dizindeki belge, kullanıcı arabiriminizde sunacağınız bir arama sonucuna karşılık gelir. Örneğin, bir otel arama uygulamasında son kullanıcılarınız otel adına, otelin özelliklerine veya belirli bir odanın özelliklerine göre arama yapmak isteyebilir. Bazı sorgu örneklerini biraz daha sonra ele alacağız.

Dizindeki belgelerle etkileşim kurmak için kendi sınıflarınızı kullanma becerisi her iki yönde de çalışır; Ayrıca arama sonuçlarını alabilir ve SDK'nın bir sonraki bölümde göreceğimiz gibi, bunları otomatik olarak seçtiğiniz bir türe çözmesini sağlayabilirsiniz.

> [!NOTE]
> Azure Bilişsel Arama .NET SDK, alan adlarının `Document` alan değerlerine önemli/değer eşlenemi olan sınıfı kullanarak dinamik olarak yazılan belgeleri de destekler. Bu durum, tasarım sırasında dizin şemasını bilmediğiniz veya belirli model sınıflarına bağlamanın kullanışlı olmayacağı senaryolarda kullanışlıdır. SDK'da belgelerle ilgili tüm yöntemler, `Document` sınıfıyla çalışan aşırı yüklerin yanı sıra genel türde bir parametre alan kesin tür belirtilmiş aşırı yüklere de sahiptir. Bu öğreticideki örnek kodda yalnızca ikincisi kullanılır. [ `Document` Sınıf](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document) `Dictionary<string, object>`devralır.
> 
>

**Neden boş değer atanabilir türleri kullanmalısınız?**

Azure Bilişsel Arama dizinine eşlenecek kendi model sınıflarınızı tasarlarken, değer `bool` türlerinin `int` özelliklerini bildirmenizi ve `bool?` geçersiz `bool`saymanızı öneririz (örneğin, bunun yerine). Boş değer atanamayan bir özellik kullanırsanız buna karşılık gelen alan için dizininizdeki hiçbir belgenin boş bir değer içermediğini **garanti etmeniz** gerekir. Ne SDK ne de Azure Bilişsel Arama hizmeti bunu uygulamanıza yardımcı olmayacaktır.

Bu yalnızca kuramsal bir sorun değildir: Var olan `Edm.Int32` türünde bir dizine yeni bir alan eklediğiniz bir senaryoyu düşünün. Dizin tanımını güncelleştirdikten sonra, tüm belgelerin bu yeni alan için boş bir değeri olacaktır (Azure Bilişsel Arama'da tüm türler geçersiz olduğundan). Ardından bu alan için boş değer atanamayan bir `int` özelliğiyle bir model sınıfı kullanırsanız belgeleri almaya çalışırken bunun gibi bir `JsonSerializationException` alırsınız:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Bu nedenle, en iyi uygulama olarak model sınıflarınızda boş değer atanabilir türler kullanmanızı öneririz.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>JSON.NET ile Özel Serileştirme
SDK, belgeleri seri hale getirmek ve deserialize etmek için JSON.NET kullanır. Gerekirse kendi `JsonConverter` veya `IContractResolver`. Daha fazla bilgi için [JSON.NET belgelerine](https://www.newtonsoft.com/json/help/html/Introduction.htm)bakın. Bu, Azure Bilişsel Arama ve diğer daha gelişmiş senaryolarla kullanmak üzere uygulamanızdan varolan bir model sınıfını uyarlamak istediğinizde yararlı olabilir. Örneğin, özel serileştirme ile şunları yapabilirsiniz:

* Model sınıfınızın belirli özelliklerini belge alanı olarak depolanmadan dahil edin veya hariç tolun.
* Kodunuzdaki özellik adları ile dizininizdeki alan adları arasında eşlenin.
* Belge alanlarına özellikleri eşleme için kullanılabilecek özel öznitelikler oluşturun.

GitHub'daki Azure Bilişsel Arama .NET SDK için birim testlerinde özel serileştirme uygulama örnekleri bulabilirsiniz. İyi bir başlangıç noktası [bu klasördür.](https://github.com/Azure/azure-sdk-for-net/tree/4f6f4e4c90200c1b0621c4cead302a91e89f2aba/sdk/search/Microsoft.Azure.Search/tests/Tests/Models) Özel serileştirme testleri tarafından kullanılan sınıfları içerir.

### <a name="searching-for-documents-in-the-index"></a>Dizinde belge arama
Örnek uygulamanın son adımı, dizindeki bazı belgeleri aramaktır:

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
    SearchParameters parameters;
    DocumentSearchResult<Hotel> results;

    Console.WriteLine("Search the entire index for the term 'motel' and return only the HotelName field:\n");

    parameters =
        new SearchParameters()
        {
            Select = new[] { "HotelName" }
        };

    results = indexClient.Documents.Search<Hotel>("motel", parameters);

    WriteDocuments(results);

    Console.Write("Apply a filter to the index to find hotels with a room cheaper than $100 per night, ");
    Console.WriteLine("and return the hotelId and description:\n");

    parameters =
        new SearchParameters()
        {
            Filter = "Rooms/any(r: r/BaseRate lt 100)",
            Select = new[] { "HotelId", "Description" }
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
    Console.WriteLine("lastRenovationDate:\n");

    parameters =
        new SearchParameters()
        {
            OrderBy = new[] { "LastRenovationDate desc" },
            Select = new[] { "HotelName", "LastRenovationDate" },
            Top = 2
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.WriteLine("Search the entire index for the term 'hotel':\n");

    parameters = new SearchParameters();
    results = indexClient.Documents.Search<Hotel>("hotel", parameters);

    WriteDocuments(results);
}
```

Her sorgu yürütürken, bu yöntem önce `SearchParameters` yeni bir nesne oluşturur. Bu nesne, sorgu için sıralama, filtreleme, sayfalama ve yüz leme gibi ek seçenekler belirtmek için kullanılır. Bu yöntemde, farklı sorgular `Select` `OrderBy`için `Top` `Filter`, , ve özellik ayarlıyoruz. Tüm `SearchParameters` özellikleri [burada](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters)belgelenmiştir.

Bir sonraki adım aslında arama sorgusu yürütmektir. Arama yı çalıştırma `Documents.Search` yöntemi kullanılarak yapılır. Her sorgu için, arama metnini dize olarak `"*"` kullanmak üzere (veya arama metni yoksa) ve daha önce oluşturulan arama parametrelerini geçiririz. Ayrıca, `Hotel` SDK'ya arama sonuçlarındaki belgeleri tür `Hotel`deki nesnelere deserialize etmesini söyleyen tür parametresi olarak da belirtiriz. `Documents.Search`

> [!NOTE]
> Burada arama sorgusu [ifade](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search)sözdizimi hakkında daha fazla bilgi bulabilirsiniz.
> 
> 

Son olarak, her sorgudan sonra bu yöntem, arama sonuçlarındaki tüm eşleşmeleri yineler ve her belgeyi konsola yazdırar:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Sırayla sorguların her birine daha yakından bakalım. İlk sorguyu yürütmek için kod aşağıda veda edebilirsiniz:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "HotelName" }
    };

results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

Bu durumda, herhangi bir aranabilir alanda "motel" sözcüğünün tüm dizinini araştırıyoruz ve sadece `Select` parametrede belirtildiği gibi otel adlarını almak istiyoruz. İşte sonuçlar:

    Name: Secret Point Motel

    Name: Twin Dome Motel

Bir sonraki sorgu biraz daha ilginç.  Biz az 100 $ bir gecelik oranı ile bir oda var ve sadece otel kimliği ve açıklama dönmek herhangi bir otel bulmak istiyorum:

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "Rooms/any(r: r/BaseRate lt 100)",
        Select = new[] { "HotelId", "Description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Bu sorgu, `$filter` `Rooms/any(r: r/BaseRate lt 100)`dizinteki belgeleri filtrelemek için bir OData ifadesi kullanır. Bu, Odalar koleksiyonundaki her öğeye 'BaseRate lt 100' uygulamak için [herhangi bir operatör](https://docs.microsoft.com/azure/search/search-query-odata-collection-operators) kullanır. Azure Bilişsel Arama'nın desteklediği OData sözdizimi hakkında daha fazla bilgiyi [burada](https://docs.microsoft.com/azure/search/query-odata-filter-orderby-syntax)bulabilirsiniz.

İşte sorgunun sonuçları şunlardır:

    HotelId: 1
    Description: The hotel is ideally located on the main commercial artery of the city in the heart of New York...

    HotelId: 2
    Description: The hotel is situated in a nineteenth century plaza, which has been expanded and renovated to...

Daha sonra, en son yenilenmiş en iyi iki otel bulmak ve otel adını ve son yenileme tarihini göstermek istiyorum. Kod aşağıdaki gibidir: 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "LastRenovationDate desc" },
        Select = new[] { "HotelName", "LastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Bu durumda, parametreyi `OrderBy` . olarak `lastRenovationDate desc`belirtmek için yine OData sözdizimini kullanırız. Ayrıca sadece `Top` ilk iki belgeyi aldığımızdan emin olmak için 2'ye ayarlandı. Daha önce olduğu `Select` gibi, hangi alanların döndürüleceğini belirtmeyi ayarlayın.

İşte sonuçlar:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Son olarak, "otel" kelimesine uyan tüm otel adlarını bulmak istiyoruz:

```csharp
parameters = new SearchParameters()
{
    SearchFields = new[] { "HotelName" }
};
results = indexClient.Documents.Search<Hotel>("hotel", parameters);

WriteDocuments(results);
```

Ve burada `Select` özellik belirtmedi beri tüm alanları içeren sonuçlar şunlardır:

    HotelId: 3
    Name: Triple Landscape Hotel
    ...

Bu adım öğretici tamamlar, ancak burada durmayın. **Sonraki adımlar Azure Bilişsel Arama hakkında daha fazla bilgi edinmek için ek kaynaklar sağlar.

## <a name="next-steps"></a>Sonraki adımlar
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) ve [REST API](https://docs.microsoft.com/rest/api/searchservice/) başvurularına göz atın.
* Çeşitli nesneleri adlandırma kurallarını öğrenmek için [adlandırma kurallarını](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) gözden geçirin.
* Azure Bilişsel Arama'da [desteklenen veri türlerini](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) gözden geçirin.
