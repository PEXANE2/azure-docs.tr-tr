---
title: .NET uygulamasından Azure Search kullanma-Azure Search
description: .NET SDK kullanarak C# .net uygulamasında Azure Search kullanmayı öğrenin. Kod tabanlı görevler, hizmete bağlanma, içerik dizini oluşturma ve bir dizini sorgulama içerir.
author: brjohnstmsft
manager: nitinme
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: brjohnst
ms.openlocfilehash: 3f167ee082b751f6bd686c0543db6a262615e486
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2019
ms.locfileid: "70208250"
---
# <a name="how-to-use-azure-search-from-a-net-application"></a>.NET uygulamasından Azure Search kullanma

Bu makale, [Azure Search .NET SDK 'sı](https://aka.ms/search-sdk)ile çalışmaya başlamanıza yönelik bir yönergedir. Azure Search kullanarak uygulamanızda zengin arama deneyimi uygulamak için .NET SDK 'sını kullanabilirsiniz.

## <a name="whats-in-the-azure-search-sdk"></a>Azure Search SDK 'sındaki Özellikler
SDK, dizinlerinizi, veri kaynaklarınızı, Dizin oluşturucularını ve eş anlamlı haritalarınızı yönetmenizi, Ayrıca, HTTP ve JSON ayrıntıları ile uğraşmak zorunda kalmadan tüm sorguları ve sorguları yürütmeyi sağlayan birkaç istemci kitaplığı içerir. Bu istemci kitaplıklarının hepsi NuGet paketleri olarak dağıtılır.

Ana NuGet paketi `Microsoft.Azure.Search`, diğer tüm paketleri bağımlılık olarak içeren bir meta paketidir. Yeni kullanmaya başladıysanız veya uygulamanızın Azure Search tüm özelliklerine ihtiyacı olacağını biliyorsanız bu paketi kullanın.

SDK 'daki diğer NuGet paketleri şunlardır:
 
  - `Microsoft.Azure.Search.Data`: Azure Search kullanarak bir .NET uygulaması geliştiriyorsanız bu paketi kullanın ve yalnızca dizininizdeki belgeleri sorgulayıp güncelleştirmeniz yeterlidir. Ayrıca dizinleri, eş anlamlı haritaları veya diğer hizmet düzeyi kaynaklarını oluşturmanız ya da güncelleştirmeniz gerekiyorsa, bunun yerine `Microsoft.Azure.Search` paketini kullanın.
  - `Microsoft.Azure.Search.Service`: Azure Search dizinleri, eş anlamlı haritaları, Dizin oluşturucuyu, veri kaynaklarını veya diğer hizmet düzeyi kaynakları yönetmek için .NET 'te Otomasyon geliştiriyorsanız bu paketi kullanın. Dizininizdeki belgeleri sorgulamak veya güncelleştirmek istiyorsanız, bunun yerine `Microsoft.Azure.Search.Data` paketini kullanın. Tüm Azure Search işlevselliğine ihtiyacınız varsa, bunun yerine `Microsoft.Azure.Search` paketini kullanın.
  - `Microsoft.Azure.Search.Common`: Azure Search .NET kitaplıkları için gereken ortak türler. Bu paketi uygulamanızda doğrudan kullanmanız gerekmez. Yalnızca bir bağımlılık olarak kullanılmak üzere tasarlanmıştır.

`Index`Çeşitli istemci kitaplıkları, `Field` `Indexes.Create` vegibi`Documents.Search` işlemleri`SearchIndexClient` ve ve sınıfları gibi sınıfları tanımlar. `SearchServiceClient` `Document` Bu sınıflar aşağıdaki ad alanları halinde düzenlenmiştir:

* [Microsoft. Azure. Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft. Azure. Search. modeller](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

SDK 'nın gelecekteki bir güncelleştirmesiyle ilgili geri bildirim sağlamak isterseniz, [geri bildirim sayfamıza](https://feedback.azure.com/forums/263029-azure-search/) bakın veya [GitHub](https://github.com/azure/azure-sdk-for-net/issues) 'da sorun oluşturun ve sorun başlığında "Azure Search" bahsetmeniz gerekir.

.NET SDK [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/)sürümünü `2019-05-06` destekler. Bu sürüm, Azure Blobları dizin oluştururken [karmaşık türler](search-howto-complex-data-types.md), bilişsel [arama](cognitive-search-concept-intro.md), [otomatik tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete)ve [jsonlines ayrıştırma modu](search-howto-index-json-blobs.md) için destek içerir. 

Bu SDK, arama hizmetleri oluşturma ve ölçekleme ve API anahtarlarını yönetme gibi [yönetim işlemlerini](https://docs.microsoft.com/rest/api/searchmanagement/) desteklemez. Arama kaynaklarınızı bir .NET uygulamasından yönetmeniz gerekiyorsa, [Azure Search .NET Yönetim SDK 'sını](https://aka.ms/search-mgmt-sdk)kullanabilirsiniz.

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>SDK 'nın en son sürümüne yükseltme
Zaten Azure Search .NET SDK 'sının daha eski bir sürümünü kullanıyorsanız ve genel kullanıma sunulan en son sürüme yükseltmek istiyorsanız, [Bu makalede](search-dotnet-sdk-migration-version-9.md) nasıl yapılacağı açıklanmaktadır.

## <a name="requirements-for-the-sdk"></a>SDK gereksinimleri
1. Visual Studio 2017 veya üzeri.
2. Kendi Azure Search hizmetiniz. SDK 'yı kullanmak için hizmetinizin adı ve bir veya daha fazla API anahtarı gerekir. [Portalda bir hizmet oluşturmak](search-create-service-portal.md) , bu adımlarda size yardımcı olur.
3. Visual Studio 'da "NuGet Paketlerini Yönet" i kullanarak Azure Search .NET SDK [NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Search) indirin. Yalnızca NuGet.org üzerinde paket adını `Microsoft.Azure.Search` (veya yalnızca bir işlevin alt kümesine ihtiyacınız varsa yukarıdaki diğer paket adlarından birini) aratın.

Azure Search .NET SDK, .NET Framework 4.5.2 ve üstünü hedefleyen uygulamaları ve .NET Core 2,0 ve üstünü destekler.

## <a name="core-scenarios"></a>Temel senaryolar
Arama uygulamanızda yapmanız gereken birkaç nokta vardır. Bu öğreticide, şu temel senaryoları ele alacağız:

* Dizin oluşturma
* Dizini belgelerle doldurma
* Tam metin arama ve filtreleri kullanarak belge arama

Aşağıdaki örnek kodda bu senaryoların her biri gösterilmektedir. Kod parçacıklarını kendi uygulamanızda kullanmayı ücretsiz olarak kullanabilirsiniz.

### <a name="overview"></a>Genel Bakış
Araştırdığımız örnek uygulama "oteller" adlı yeni bir dizin oluşturur, bunu birkaç belgeyle doldurur, sonra bazı arama sorgularını yürütür. Genel akışı gösteren ana program aşağıda verilmiştir:

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

Bu adım adım adım izlenecek. İlk olarak yeni `SearchServiceClient`bir oluşturma gerekir. Bu nesne, dizinleri yönetmenizi sağlar. Bir tane oluşturmak için, Azure Search hizmet adınızı ve bir yönetici API anahtarını sağlamanız gerekir. Bu bilgileri `appsettings.json` [örnek uygulamanın](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo)dosyasına girebilirsiniz.

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
> Yanlış bir anahtar (örneğin, bir yönetici anahtarının gerekli olduğu bir sorgu anahtarı) `SearchServiceClient` sağlarsanız, `Indexes.Create`üzerinde bir işlem yöntemi ilk kez çağırdığınızda " `CloudException` yasak" hata iletisiyle bir oluşturulur. Bu durumda, API anahtarımızı çift kontrol edin.
> 
> 

Sonraki birkaç satır, "oteller" adlı bir dizin oluşturmak için Yöntemler çağırır, zaten varsa onu siliyor. Bu yöntemlerin biraz daha sonra izlenecek.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteIndexIfExists(indexName, serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateIndex(indexName, serviceClient);
```

Sonra, dizinin doldurulması gerekir. Dizini doldurmak için bir `SearchIndexClient`ile ihtiyacımız olacak. Bir tane elde etmenin iki yolu vardır: oluşturarak veya ' `Indexes.GetClient` `SearchServiceClient`de çağırarak. Daha kolay bir kullanım için kullanırız.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);
```

> [!NOTE]
> Tipik bir arama uygulamasında dizin yönetimi ve popülasyon, arama sorgularından ayrı bir bileşen tarafından işlenebilir. `Indexes.GetClient`, size ek `SearchCredentials`sağlama sorunu kaydettiğinden, bir dizini doldurmak için kullanışlıdır. Yeni `SearchIndexClient` için `SearchServiceClient` oluşturmak üzere kullandığınız yönetici anahtarını geçirerek bunu yapar. Ancak, uygulamanızın sorguları yürüten bölümünde, bir sorgu anahtarı geçirebilmeniz için `SearchIndexClient` doğrudan oluşturulması daha iyidir, bu da bir yönetici anahtarı yerine yalnızca verileri okuyabilmenizi sağlar. Bu, en az ayrıcalık ilkesiyle tutarlıdır ve uygulamanızın daha güvenli olmasına yardımcı olur. Yönetici anahtarları ve sorgu anahtarları hakkında daha fazla bilgiyi [buradan](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization)edinebilirsiniz.
> 
> 

Artık bir `SearchIndexClient`sunduğumuz için dizini doldururuz. Dizin popülasyonu, daha sonra izlenecek bir başka yöntem tarafından yapılır.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Son olarak, birkaç arama sorgusu yürütüyoruz ve sonuçları görüntüyoruz. Bu kez, farklı `SearchIndexClient`bir kullandık:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(indexName, configuration);

RunQueries(indexClientForQueries);
```

Daha sonra `RunQueries` yönteme daha yakından bakacağız. Yeni `SearchIndexClient`bir oluşturma kodu aşağıda verilmiştir:

```csharp
private static SearchIndexClient CreateSearchIndexClient(string indexName, IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, indexName, new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Bu süre, dizine yazma erişimi gerektirmeyen bir sorgu anahtarı kullanıyoruz. Bu bilgileri `appsettings.json` [örnek uygulamanın](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo)dosyasına girebilirsiniz.

Bu uygulamayı geçerli bir hizmet adı ve API anahtarları ile çalıştırırsanız, çıkış aşağıdaki örnekteki gibi görünmelidir: (Bazı konsol çıktıları "..." ile değiştirilmiştir çizim amaçları için.)

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

Uygulamanın tam kaynak kodu, bu makalenin sonunda verilmiştir.

Sonra, tarafından `Main`çağrılan yöntemlerin her birine daha yakından bakacağız.

### <a name="creating-an-index"></a>Dizin oluşturma
Oluşturduktan sonra `Main` , zaten varsa "oteller" dizinini siler. `SearchServiceClient` Bu silme işlemi aşağıdaki yöntem tarafından yapılır:

```csharp
private static void DeleteIndexIfExists(string indexName, SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists(indexName))
    {
        serviceClient.Indexes.Delete(indexName);
    }
}
```

Bu yöntem, dizinin mevcut `SearchServiceClient` olup olmadığını denetlemek için verilen öğesini kullanır ve varsa silin.

> [!NOTE]
> Bu makaledeki örnek kod, kolaylık amacıyla Azure Search .NET SDK'sının zaman uyumlu yöntemlerini kullanır. Kendi uygulamalarınızda zaman uyumsuz yöntemler kullanarak bunları ölçeklenebilir ve esnek tutmanızı öneririz. Örneğin, `ExistsAsync` yukarıdaki yönteminde `Exists` ve `DeleteAsync` yerinekullanabilirsiniz.`Delete`
> 
> 

Sonra, `Main` bu yöntemi çağırarak yeni bir "oteller" dizini oluşturur:

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

Bu yöntem, yeni dizinin `Index` şemasını tanımlayan `Field` nesnelerin listesini içeren yeni bir nesne oluşturur. Her alan, arama davranışını tanımlayan bir ad, veri türü ve birkaç özniteliğe sahiptir. Sınıfı, `Field` belirtilen`Hotel` model sınıfının ortak özelliklerini ve özniteliklerini inceleyerek, dizin için bir nesne listesi oluşturmak üzere yansıma kullanır. `FieldBuilder` Daha sonra `Hotel` sınıfa daha yakından bakacağız.

> [!NOTE]
> Gerektiğinde, her zaman `Field` nesne listesini doğrudan `FieldBuilder` oluşturabilirsiniz. Örneğin, bir model sınıfı kullanmak istemeyebilirsiniz veya öznitelikler ekleyerek değiştirmek istemediğiniz mevcut bir model sınıfını kullanmanız gerekebilir.
>
> 

Alanlara ek olarak, dizine Puanlama profilleri, öneri araçları veya CORS seçenekleri de ekleyebilirsiniz (Bu parametreler, breçekimi için örnekten çıkarılır). Dizin nesnesi hakkında daha fazla bilgi ve [SDK başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index)içindeki bileşen parçaları ve [Azure Search REST API başvurusu](https://docs.microsoft.com/rest/api/searchservice/)hakkında daha fazla bilgi edinebilirsiniz.

### <a name="populating-the-index"></a>Dizin dolduruluyor
İçindeki `Main` bir sonraki adım, yeni oluşturulan dizini doldurur. Bu dizin oluşturma işlemi aşağıdaki yöntemde yapılır: (Bazı kodlar "..." ile değiştirilmiştir illüstrasyon amaçlıdır.  Tam veri popülasyon kodu için tam örnek çözümüne bakın.)

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

Bu yöntemin dört bölümü vardır. İlki, dizine yüklemek üzere giriş verilerimizde kullanılacak 3 `Hotel` `Room` nesne ile her biri 3 nesneden oluşan bir dizi oluşturur. Bu veriler kolaylık sağlaması için sabit olarak kodlanır. Kendi uygulamanızda, verileriniz büyük olasılıkla SQL veritabanı gibi bir dış veri kaynağından gelmiş olacaktır.

İkinci bölüm, belgeleri içeren `IndexBatch` bir oluşturur. Bu durumda, çağırarak `IndexBatch.Upload`, toplu işe uygulamak istediğiniz işlemi bu durumda belirtirsiniz. Batch daha sonra `Documents.Index` yöntemi tarafından Azure Search dizine yüklenir.

> [!NOTE]
> Bu örnekte, belgeleri karşıya yüklüyoruz. Değişiklikleri varolan belgelere birleştirmek veya belgeleri silmek isterseniz,, veya `IndexBatch.Merge` `IndexBatch.Delete` çağırarak `IndexBatch.MergeOrUpload`toplu işlemler oluşturabilirsiniz. Tek bir toplu işte farklı işlemleri, her biri bir belge üzerinde `IndexBatch.New`belirli bir işlemi gerçekleştirmek Azure Search bildiren `IndexAction` bir nesne koleksiyonu alan çağırarak de karıştırabilirsiniz. ,, Vb. `IndexAction` `IndexAction.Upload`gibi karşılık gelen yöntemi çağırarak, her biri kendi işlemi ile oluşturabilirsiniz. `IndexAction.Merge`
> 
> 

Bu yöntemin üçüncü bölümü, dizin oluşturma için önemli bir hata durumunu işleyen bir catch bloğudur. Azure Search hizmetiniz toplu işlemdeki belgelerin bazılarına dizin oluşturmada başarısız olursa `Documents.Index` tarafından bir `IndexBatchException` oluşturulur. Bu özel durum, hizmetiniz ağır yük altındayken belge dizinleniyorsa gerçekleşebilir. **Bu durumu, kodunuzda açık şekilde işlemenizi kesinlikle öneririz.** Başarısız olan belgelere dizin oluşturmayı geciktirip sonra yeniden deneyebilir veya günlük tutup örneğin devam ettiği şekilde devam edebilir veya uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak başka bir şey yapabilirsiniz.

> [!NOTE]
> [`FindFailedActionsToRetry`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception.findfailedactionstoretry) Yöntemini yalnızca önceki `Index`çağrısında başarısız olan eylemleri içeren yeni bir toplu iş oluşturmak için kullanabilirsiniz. [StackOverflow üzerinde](https://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry)düzgün bir şekilde nasıl kullanılacağına ilişkin bir tartışma vardır.
>
>

Son olarak, `UploadDocuments` yöntem iki saniye gecikdi. Azure Search hizmetinizde dizin oluşturma uyumsuz şekilde meydana gelir; bu nedenle belgelerin aramada kullanılabilir olduğundan emin olmak için örnek uygulamanızın kısa bir süre beklemesi gerekir. Bu gibi gecikmeler genellikle yalnızca gösterilerde, testlerde ve örnek uygulamalarda gereklidir.

<a name="how-dotnet-handles-documents"></a>

#### <a name="how-the-net-sdk-handles-documents"></a>.NET SDK belgeleri nasıl işler?
Azure Search .NET SDK'sının `Hotel` gibi kullanıcı tanımlı bir sınıfın örneklerini dizine nasıl yükleyebildiğini merak ediyor olabilirsiniz. Bu sorunun yanıtlanmasına yardımcı olmak için şu `Hotel` sınıfa göz atalım:

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

İlk dikkat edilecek şey, `Hotel` sınıftaki her bir ortak özelliğin adının Dizin tanımında aynı ada sahip olan bir alana eşlendiğine ilişkin bir şeydir. Her bir alanın küçük harfle başlamasını istiyorsanız ("Camel Case"), SDK 'ya özellik adlarını, sınıftaki `[SerializePropertyNamesAsCamelCase]` özniteliğiyle otomatik olarak bir şekilde eşlemenizi söyleyebilirsiniz. Bu senaryo, .NET 'teki "Pascal case" adlandırma kılavuzlarını ihlal etmek zorunda kalmadan, hedef şemanın uygulama geliştiricisinin denetimi dışında olduğu veri bağlamayı gerçekleştiren .NET uygulamalarında yaygındır.

> [!NOTE]
> Azure Search .NET SDK'sı, özel model nesnelerinizi JSON'a ve JSON'dan seri hale getirmek ve seri durumdan çıkarmak için [NewtonSoft JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) kitaplığını kullanır. Gerekirse bu seri hale getirmeyi özelleştirebilirsiniz. Daha fazla bilgi için bkz. [JSON.net Ile özel serileştirme](#JsonDotNet).
> 
> 

`IsFilterable`Dikkat edilecek ikinci şey `IsSearchable` `Analyzer`, her bir özellik, ,vegibiözniteliklerletasarlanmalıdır.`Key` Bu öznitelikler, [bir Azure Search dizinindeki karşılık gelen alan özniteliklerine](https://docs.microsoft.com/rest/api/searchservice/create-index#request)doğrudan eşlenir. `FieldBuilder` Sınıfı, dizin için alan tanımları oluşturmak üzere bu özellikleri kullanır.

`Hotel` Sınıfına ilişkin üçüncü önemli şey, genel özelliklerin veri türleridir. Bu özelliklerin .NET türleri, dizin tanımında eşdeğer alan türleriyle eşlenir. Örneğin, `Category` dize özelliği `Edm.String` türündeki `category` alanına eşlenir. `bool?`, ,,`Edm.Boolean`Vb .arasındabenzertüreşlemelerivardır.`DateTimeOffset?` `Edm.DateTimeOffset` Tür eşlemesine yönelik belirli kurallar, [Azure Search .NET SDK başvurusundaki](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.get) `Documents.Get` yönteminde belirtilmiştir. `FieldBuilder` Sınıfı sizin için bu eşlemeyi üstlenir, ancak serileştirme sorunlarını gidermeniz gerektiğinde anlaşılması faydalı olabilir.

`SmokingAllowed` Özelliği fark etmeniz mı istiyordunuz?

```csharp
[JsonIgnore]
public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;
```

Bu özelliğindeki `FieldBuilder` öznitelik, öğesine onu bir alan olarak dizine serileştirmeyeceğini söyler. `JsonIgnore`  Bu, uygulamanızda yardımcılar olarak kullanabileceğiniz istemci tarafı hesaplanmış Özellikler oluşturmanın harika bir yoludur.  Bu durumda, `SmokingAllowed` özelliği `Rooms` koleksiyonda herhangi birinin `Room` smome izin verip etmediğini yansıtır.  Tümü yanlışsa, tüm otelin smome izin vermiyor olduğunu gösterir.

`Address` Ve`Rooms` gibi bazı özellikler .net sınıflarının örnekleridir.  Bu özellikler daha karmaşık veri yapılarını temsil eder ve sonuç olarak dizinde [karmaşık veri türüne](https://docs.microsoft.com/azure/search/search-howto-complex-data-types) sahip alanlar gerektirir.

Özelliği, `Address` sınıfında aşağıda tanımlanan birden çok değer kümesini temsil eder: `Address`

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

Bu sınıf, Birleşik Devletler veya Kanada 'daki adresleri tanımlamakta kullanılan standart değerleri içerir. Mantıksal alanları dizinde gruplamak için bu gibi türleri kullanabilirsiniz.

Özelliği bir `Room` nesne dizisini temsil eder: `Rooms`

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

.NET 'teki veri modeliniz ve ilgili dizin şeması, son kullanıcıya vermek istediğiniz arama deneyimini destekleyecek şekilde tasarlanmalıdır. .NET 'teki her üst düzey nesne, dizinde bulunan IE belgesi, Kullanıcı arabiriminizdeki mevcut bir arama sonucuna karşılık gelir. Örneğin, bir otel arama uygulamasında son kullanıcılarınızın otel adına, otel özelliklerine veya belirli bir odanın özelliklerine göre arama yapmak isteyebilirsiniz. Biraz daha sonra bazı sorgu örnekleri ele alınacaktır.

Dizindeki belgelerle etkileşim kurmak için kendi sınıflarınızı kullanma özelliği her iki yönde de işe yarar; Ayrıca, bir sonraki bölümde göreceğiniz gibi, arama sonuçlarını alabilir ve SDK 'nın bunları dilediğiniz bir tür için otomatik olarak seri durumdan çıkarabilmeniz gerekir.

> [!NOTE]
> Azure Search .NET SDK'sı, alan adlarını alan değerlerine bir anahtar/değer eşlemesi olan `Document` sınıfını kullanarak dinamik tür belirtilmiş belgeleri de destekler. Bu durum, tasarım sırasında dizin şemasını bilmediğiniz veya belirli model sınıflarına bağlamanın kullanışlı olmayacağı senaryolarda kullanışlıdır. SDK'da belgelerle ilgili tüm yöntemler, `Document` sınıfıyla çalışan aşırı yüklerin yanı sıra genel türde bir parametre alan kesin tür belirtilmiş aşırı yüklere de sahiptir. Bu öğreticideki örnek kodda yalnızca ikincisi kullanılır. Sınıfı öğesinden devralır`Dictionary<string, object>`. [ `Document` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document)
> 
>

**Neden boş değer atanabilir türleri kullanmalısınız?**

Bir Azure Search dizinine eşlemek için yeni model sınıflarınızı tasarlarken, `bool` ve `int` gibi değer türü özelliklerinin boş değer atanabilir (örneğin, `bool` yerine `bool?`) şeklinde bildirilmesini öneririz. Boş değer atanamayan bir özellik kullanırsanız buna karşılık gelen alan için dizininizdeki hiçbir belgenin boş bir değer içermediğini **garanti etmeniz** gerekir. Bunu zorlamanıza ne SDK ne de Azure Search hizmeti yardımcı olur.

Bu yalnızca bir kuramsal sorun değildir: Varolan bir dizine `Edm.Int32`yeni bir alanı eklediğiniz bir senaryoyu düşünün. Dizin tanımını güncelleştirdikten sonra, tüm belgelerin bu yeni alan için boş bir değeri olur (bunun nedeni, Azure Search'te tüm türlerin boş değer atanabilir olmasıdır). Ardından bu alan için boş değer atanamayan bir `int` özelliğiyle bir model sınıfı kullanırsanız belgeleri almaya çalışırken bunun gibi bir `JsonSerializationException` alırsınız:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Bu nedenle, en iyi uygulama olarak model sınıflarınızda boş değer atanabilir türler kullanmanızı öneririz.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>JSON.NET ile özel serileştirme
SDK, belgeleri serileştirmek ve seri durumdan çıkarmak için JSON.NET kullanır. Kendi `JsonConverter` veya`IContractResolver`' i tanımlayarak gerekirse serileştirme ve serisini kaldırabilirsiniz. Daha fazla bilgi için [JSON.net belgelerine](https://www.newtonsoft.com/json/help/html/Introduction.htm)bakın. Bu, bir mevcut model sınıfını Azure Search ve diğer gelişmiş senaryolar ile kullanmak üzere uygulamanız için uyarlamak istediğinizde yararlı olabilir. Örneğin, özel serileştirme ile şunları yapabilirsiniz:

* Model sınıfınızın belirli özelliklerini belge alanları olarak depolanmak üzere dahil edin veya hariç tutun.
* Kodunuzda kodunuzun ve alan adlarınızın Özellik adları arasında eşleme yapın.
* Belge alanlarına özellikleri eşlemek için kullanılabilecek özel öznitelikler oluşturun.

GitHub üzerinde Azure Search .NET SDK için birim testlerinde özel serileştirme uygulama örneklerini bulabilirsiniz. [Bu klasör](https://github.com/Azure/azure-sdk-for-net/tree/4f6f4e4c90200c1b0621c4cead302a91e89f2aba/sdk/search/Microsoft.Azure.Search/tests/Tests/Models)iyi bir başlangıç noktasıdır. Özel serileştirme testleri tarafından kullanılan sınıfları içerir.

### <a name="searching-for-documents-in-the-index"></a>Dizinde belge arama
Örnek uygulamadaki son adım, dizinde bazı belgeleri aramak içindir:

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

Sorgu her yürütüldüğünde, bu yöntem ilk olarak yeni `SearchParameters` bir nesne oluşturur. Bu nesne, sorgu için sıralama, filtreleme, sayfalama ve Faks oluşturma gibi ek seçenekleri belirtmek için kullanılır. Bu yöntemde `Filter`,,, ve `Select` `Top` özelliğini farklı sorgular için `OrderBy`ayarlıyoruz. Tüm özellikler burada belgelenmiştir. [](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters) `SearchParameters`

Sonraki adım aslında arama sorgusunu yürütmekte. Aramayı çalıştırmak `Documents.Search` yöntemi kullanılarak yapılır. Her sorgu için, arama metnini dize olarak kullanılacak şekilde (veya `"*"` arama metni yoksa) ve daha önce oluşturulan arama parametrelerini geçiririz. Ayrıca, SDK `Hotel` 'nın arama sonuçlarındaki belgelerin türünü `Documents.Search`türündeki `Hotel`nesnelere serisini oluşturmasını söyleyen için tür parametresi olarak da belirttik.

> [!NOTE]
> Arama sorgusu ifade sözdizimi hakkında daha fazla bilgiyi [burada](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search)bulabilirsiniz.
> 
> 

Son olarak, her bir sorgudan bu yöntem her bir belgeyi konsola yazdırarak arama sonuçlarındaki tüm eşleştirmelerle yinelenir:

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

Her sorguya sırayla daha yakından göz atalım. İlk sorguyu yürütmek için kod aşağıda verilmiştir:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "HotelName" }
    };

results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

Bu durumda, herhangi bir aranabilir alanda "Motel" sözcüğünün tüm dizinini arıyor ve yalnızca `Select` parametre tarafından belirtilen otel adlarını almak istiyoruz. Sonuçlar şunlardır:

    Name: Secret Point Motel

    Name: Twin Dome Motel

Sonraki sorgu biraz daha ilginç.  Gece ücreti $100 ' den az olan ve yalnızca otel KIMLIĞI ve açıklama döndüren bir salya sahip olan oteller bulmak istiyoruz:

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

Bu sorgu, dizindeki belgeleri `$filter` filtrelemek için `Rooms/any(r: r/BaseRate lt 100)`bir OData ifadesi kullanır. Bu, Oda koleksiyonundaki her öğeye ' BaseRate lt 100 ' öğesini uygulamak için [Any işlecini](https://docs.microsoft.com/azure/search/search-query-odata-collection-operators) kullanır. Azure Search desteklediği OData sözdizimi hakkında daha fazla bilgi edinebilirsiniz. [](https://docs.microsoft.com/azure/search/query-odata-filter-orderby-syntax)

Sorgunun sonuçları aşağıdadır:

    HotelId: 1
    Description: The hotel is ideally located on the main commercial artery of the city in the heart of New York...

    HotelId: 2
    Description: The hotel is situated in a nineteenth century plaza, which has been expanded and renovated to...

Daha sonra, en son yeniden kiralanan en son iki otelyi bulmak istiyoruz ve otel adını ve son yeniden oluşturma tarihini gösterir. Kod aşağıdaki gibidir: 

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

Bu durumda, `OrderBy` parametresini olarak `lastRenovationDate desc`belirtmek için OData söz dizimini kullanırız. Ayrıca yalnızca en `Top` üstteki iki belgeyi aldığınızdan emin olmak için 2 olarak ayarlandık. Daha önce olduğu gibi, `Select` hangi alanların döndürüleceğini belirlemek için ayarlandık.

Sonuçlar şunlardır:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Son olarak, "otel" kelimesiyle eşleşen tüm oteller adlarını bulmak istiyoruz:

```csharp
parameters = new SearchParameters()
{
    SearchFields = new[] { "HotelName" }
};
results = indexClient.Documents.Search<Hotel>("hotel", parameters);

WriteDocuments(results);
```

`Select` Özelliği belirttiğimiz bu yana tüm alanları içeren sonuçlar aşağıda verilmiştir:

    HotelId: 3
    Name: Triple Landscape Hotel
    ...

Bu adım öğreticiyi tamamlar, ancak burada durmayın. \* * Sonraki adımlarda Azure Search hakkında daha fazla bilgi edinmek için ek kaynaklar sağlanır.

## <a name="next-steps"></a>Sonraki adımlar
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) ve [REST API](https://docs.microsoft.com/rest/api/searchservice/) başvurularına göz atın.
* Çeşitli nesneleri adlandırmayla ilgili kuralları öğrenmek için [adlandırma kurallarını](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) gözden geçirin.
* Azure Search [desteklenen veri türlerini](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) gözden geçirin.
