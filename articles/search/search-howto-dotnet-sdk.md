---
title: .NET 'te Azure.Search.Documstalar (v11) kullanın
titleSuffix: Azure Cognitive Search
description: C# ve Azure.Search.Documstalar (v11) istemci kitaplığı kullanarak bir .NET uygulamasında arama nesneleri oluşturmayı ve yönetmeyi öğrenin. Kod parçacıkları hizmete bağlanmayı, dizinleri ve sorguları oluşturmayı gösterir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.devlang: dotnet
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/27/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 4b3256591c0aa2536fd42bcdbb2ef339fc1d5c48
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93356816"
---
# <a name="how-to-use-azuresearchdocuments-in-a-c-net-application"></a>C# .NET uygulamasında Azure.Search.Documtaları kullanma

Bu makalede, C# ve [**Azure.Search.Documstalar**](/dotnet/api/overview/azure/search) (sürüm 11) istemci kitaplığı kullanılarak arama nesnelerinin nasıl oluşturulduğu ve yönetileceği açıklanmaktadır.

## <a name="about-version-11"></a>Sürüm 11 hakkında

.NET için Azure SDK, Azure SDK ekibi 'nden [Microsoft. Azure. Search](/dotnet/api/overview/azure/search/client10) istemci kitaplıklarına işlevsel olarak denk olan yeni bir [**Azure.Search.Documstalar**](/dotnet/api/overview/azure/search) istemci kitaplığı ekler, ancak uygun yerlerde yaygın yaklaşımlar ve kuralları kullanır. Bazı örneklere [`AzureKeyCredential`](/dotnet/api/azure.azurekeycredential) anahtar kimlik doğrulaması ve [ üzerindeSystem.Text.Jsdahildir. ](/dotnet/api/system.text.json.serialization) JSON serileştirme için serileştirme.

Önceki sürümlerde olduğu gibi, bu kitaplığı kullanarak şunları yapabilirsiniz:

+ Arama dizinleri, veri kaynakları, Dizin oluşturucular, becerileri ve eş anlamlı haritalar oluşturun ve yönetin
+ Dizinde arama belgelerini yükleme ve yönetme
+ HTTP ve JSON ayrıntıları ile uğraşmak zorunda kalmadan sorguları yürütün

Kitaplık, bir arama hizmetine programlı erişim için kullanılan tüm API 'Leri içeren tek bir [Azure.Search.Document NuGet paketi](https://www.nuget.org/packages/Azure.Search.Documents/)olarak dağıtılır.

İstemci kitaplığı, ve gibi işlemleri ve ve sınıfları gibi sınıfları tanımlar `SearchIndex` `SearchField` `SearchDocument` `SearchIndexClient.CreateIndex` `SearchClient.Search` `SearchIndexClient` `SearchClient` . Bu sınıflar aşağıdaki ad alanları halinde düzenlenmiştir:

+ [`Azure.Search.Documents`](/dotnet/api/azure.search.documents)
+ [`Azure.Search.Documents.Indexes`](/dotnet/api/azure.search.documents.indexes)
+ [`Azure.Search.Documents.Indexes.Models`](/dotnet/api/azure.search.documents.indexes.models)
+ [`Azure.Search.Documents.Models`](/dotnet/api/azure.search.documents.models)

Azure.Search.Docmestalar (sürüm 11) [ `2020-06-30` Azure bilişsel arama REST API 'ın](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/search/data-plane/Azure.Search/preview/2020-06-30)hedeflediği sürüm. 

İstemci kitaplığı, arama hizmetleri oluşturma ve ölçekleme ve API anahtarlarını yönetme gibi [hizmet yönetimi işlemleri](/rest/api/searchmanagement/)sağlamaz. Arama kaynaklarınızı bir .NET uygulamasından yönetmeniz gerekiyorsa, .NET için Azure SDK 'da [Microsoft. Azure. Management. Search](/dotnet/api/overview/azure/search/management) kitaplığını kullanın.

## <a name="upgrade-to-v11"></a>V11 sürümüne yükselt

.NET SDK 'sının önceki sürümünü kullanıyorsanız ve geçerli genel kullanıma sunulan sürüme yükseltmek isterseniz, bkz. [Azure bilişsel arama .NET SDK sürüm 11 ' e yükseltme](search-dotnet-sdk-migration-version-11.md)

## <a name="sdk-requirements"></a>SDK gereksinimleri

+ Visual Studio 2019 veya üzeri.

+ Kendi Azure Bilişsel Arama hizmetiniz. SDK 'yı kullanmak için hizmetinizin adı ve bir veya daha fazla API anahtarı gerekir. [Portalda bir hizmet oluşturun](search-create-service-portal.md) .

+ **Araçlar** NuGet Paket Yöneticisi, Visual Studio 'da çözüm için NuGet Paketlerini Yönet ' i kullanarak [Azure.Search.Documstaları paketini](https://www.nuget.org/packages/Azure.Search.Documents) indirin  >  **NuGet Package Manager**  >  **Manage NuGet Packages for Solution** . Paket adını arayın `Azure.Search.Documents` .

.NET için Azure SDK, en düşük gereksinimleri .NET Framework 4.6.1 ve .NET Core 2,0 anlamına gelen [.NET Standard 2,0](/dotnet/standard/net-standard#net-implementation-support)' a uyar.

## <a name="example-application"></a>Örnek uygulama

Bu makalede, Azure Bilişsel Arama temel kavramları (özellikle, bir arama dizini oluşturma, yükleme ve sorgulama) göstermek için GitHub 'daki [Dotnethowto](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) Code örneğine bağlı olarak "örnek olarak öğretilir" makalesi.

Bu makalenin geri kalanında, sonuçlarda eşleşen birkaç sorguyla birlikte birkaç belgeyle doldurulmuş "oteller" adlı yeni bir dizin olduğunu varsayalım.

Genel akışın gösterildiği ana program aşağıda verilmiştir:

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchIndexClient indexClient = CreateSearchIndexClient(configuration);

    string indexName = configuration["SearchIndexName"];

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteIndexIfExists(indexName, indexClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateIndex(indexName, indexClient);

    SearchClient searchClient = indexClient.GetSearchClient(indexName);

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(searchClient);

    SearchClient indexClientForQueries = CreateSearchClientForQueries(indexName, configuration);

    Console.WriteLine("{0}", "Run queries...\n");
    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
    Console.ReadKey();
}
```

Sonraki bir deyişle, bu uygulamayı geçerli bir hizmet adı ve API anahtarlarıyla çalıştırdığınız varsayılarak çıktının kısmi bir ekran görüntüsüdür:

:::image type="content" source="media/search-howto-dotnet-sdk/console-output.png" alt-text="Örnek programdan Console. WriteLine çıkışı":::

### <a name="client-types"></a>İstemci türleri

İstemci kitaplığı çeşitli işlemler için üç istemci türü kullanır: [`SearchIndexClient`](/dotnet/api/azure.search.documents.indexes.searchindexclient) Dizin oluşturmak, güncelleştirmek veya silmek, [`SearchClient`](/dotnet/api/azure.search.documents.searchclient) bir dizini yüklemek veya sorgulamak, Dizin [`SearchIndexerClient`](/dotnet/api/azure.search.documents.indexes.searchindexerclient) oluşturucular ve becerileri ile çalışmak için. Bu makale, ilk iki üzerine odaklanır. 

Tüm istemciler, en azından hizmet adı veya uç nokta ve bir API anahtarı gerektirir. Bu bilgileri, `appsettings.json` [Dotnethowto örnek uygulamasının](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo)dosyasında buldıklarınız gibi bir yapılandırma dosyasında sağlamak yaygındır. Yapılandırma dosyasından okumak için, `using Microsoft.Extensions.Configuration;` programınıza ekleyin.

Aşağıdaki ifade, dizinleri oluşturmak, güncelleştirmek veya silmek için kullanılan dizin istemcisini oluşturur. Bir arama uç noktası ve yönetici API anahtarı alır.

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceEndPoint = configuration["SearchServiceEndPoint"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(new Uri(searchServiceEndPoint), new AzureKeyCredential(adminApiKey));
    return indexClient;
}
```

Sonraki ifade, belgeleri yüklemek veya sorguları çalıştırmak için kullanılan arama istemcisini oluşturur. `SearchClient` bir dizin gerektirir. Belge yüklemek için bir yönetici API anahtarınız olması gerekir, ancak sorguları çalıştırmak için bir sorgu API 'SI anahtarı kullanabilirsiniz. 

```csharp
string indexName = configuration["SearchIndexName"];

private static SearchClient CreateSearchClientForQueries(string indexName, IConfigurationRoot configuration)
{
    string searchServiceEndPoint = configuration["SearchServiceEndPoint"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchClient searchClient = new SearchClient(new Uri(searchServiceEndPoint), indexName, new AzureKeyCredential(queryApiKey));
    return searchClient;
}
```

> [!NOTE]
> İçeri aktarma işlemi için geçersiz bir anahtar sağlarsanız (örneğin, bir yönetici anahtarının gerekli olduğu bir sorgu anahtarı), `SearchClient` `CloudException` üzerinde bir işlem yöntemi ilk kez çağırdığınızda "yasak" hata iletisiyle bir oluşturulur. Bu durumda, API anahtarını çift kontrol edin.
>

### <a name="deleting-the-index"></a>Dizin siliniyor

Geliştirmenin erken aşamalarında, [`DeleteIndex`](/dotnet/api/azure.search.documents.indexes.searchindexclient.deleteindex) devam eden bir dizini silmek için bir ifade eklemek isteyebilirsiniz, böylece onu güncelleştirilmiş bir tanım ile yeniden oluşturabilirsiniz. Azure Bilişsel Arama için örnek kod genellikle, örneği yeniden çalıştırabilmeniz için bir silme adımı içerir.

Aşağıdaki satırı çağırır `DeleteIndexIfExists` :

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteIndexIfExists(indexName, indexClient);
```

Bu yöntem, `SearchIndexClient` dizinin mevcut olup olmadığını denetlemek için verilen ' ı kullanır ve varsa, siler:

```csharp
private static void DeleteIndexIfExists(string indexName, SearchIndexClient indexClient)
{
    try
    {
        if (indexClient.GetIndex(indexName) != null)
        {
            indexClient.DeleteIndex(indexName);
        }
    }
    catch (RequestFailedException e) when (e.Status == 404)
    {
        // Throw an exception if the index name isn't found
        Console.WriteLine("The index doesn't exist. No deletion occurred.");
```

> [!NOTE]
> Bu makaledeki örnek kod, basitlik için zaman uyumlu yöntemleri kullanır, ancak bunları ölçeklenebilir ve yanıt verecek şekilde korumak için kendi uygulamalarınızda zaman uyumsuz yöntemleri kullanmanız gerekir. Örneğin, yukarıdaki yönteminde [`DeleteIndexAsync`](/dotnet/api/azure.search.documents.indexes.searchindexclient.deleteindexasync) yerine kullanabilirsiniz [`DeleteIndex`](/dotnet/api/azure.search.documents.indexes.searchindexclient.deleteindex) .
>

## <a name="create-an-index"></a>Dizin oluşturma

[`SearchIndexClient`](/dotnet/api/azure.search.documents.indexes.searchindexclient)Bir dizin oluşturmak için ' i kullanabilirsiniz. 

Aşağıdaki yöntem, [`SearchIndex`](/dotnet/api/azure.search.documents.indexes.models.searchindex) [`SearchField`](/dotnet/api/azure.search.documents.indexes.models.searchfield) Yeni dizinin şemasını tanımlayan nesnelerin listesini içeren yeni bir nesne oluşturur. Her alan, arama davranışını tanımlayan bir ad, veri türü ve birkaç özniteliğe sahiptir. 

Alanlar, kullanarak bir model sınıfından tanımlanabilir [`FieldBuilder`](/dotnet/api/azure.search.documents.indexes.fieldbuilder) . `FieldBuilder`Sınıfı, `SearchField` belirtilen model sınıfının ortak özelliklerini ve özniteliklerini inceleyerek, dizin için bir nesne listesi oluşturmak üzere yansıma kullanır `Hotel` . `Hotel`Daha sonra sınıfa daha yakından bakacağız.

```csharp
private static void CreateIndex(string indexName, SearchIndexClient indexClient)
{
    FieldBuilder fieldBuilder = new FieldBuilder();
    var searchFields = fieldBuilder.Build(typeof(Hotel));

    var definition = new SearchIndex(indexName, searchFields);

    indexClient.CreateOrUpdateIndex(definition);
}
```

Alanların yanı sıra, dizine Puanlama profilleri, öneri araçları veya CORS seçenekleri de ekleyebilirsiniz (Bu parametreler, breçekimi için örnekten çıkarılır). Searchındex nesnesi ve bileşen parçaları hakkında daha fazla bilgiyi [`SearchIndex`](/dotnet/api/azure.search.documents.indexes.models.searchindex) Özellikler listesinde ve [REST API başvurusunda](/rest/api/searchservice/)bulabilirsiniz.

> [!NOTE]
> Gerektiğinde, her zaman `Field` nesne listesini doğrudan oluşturabilirsiniz `FieldBuilder` . Örneğin, bir model sınıfı kullanmak istemeyebilirsiniz veya öznitelikler ekleyerek değiştirmek istemediğiniz mevcut bir model sınıfını kullanmanız gerekebilir.
>

### <a name="call-createindex-in-main"></a>Main () içinde CreateIndex çağrısı

`Main` Yukarıdaki yöntemi çağırarak yeni bir "oteller" dizini oluşturur:

```csharp
Console.WriteLine("{0}", "Creating index...\n");
CreateIndex(indexName, indexClient);
```

## <a name="use-a-model-class-for-data-representation"></a>Veri gösterimi için model sınıfı kullanma

DotNetHowTo örneği, [otel](https://github.com/Azure-Samples/search-dotnet-getting-started/blob/master/DotNetHowTo/DotNetHowTo/Hotel.cs), [Adres](https://github.com/Azure-Samples/search-dotnet-getting-started/blob/master/DotNetHowTo/DotNetHowTo/Address.cs)ve [Oda](https://github.com/Azure-Samples/search-dotnet-getting-started/blob/master/DotNetHowTo/DotNetHowTo/Room.cs) veri yapıları için model sınıfları kullanır. `Hotel` başvurular `Address` , tek düzeyli bir karmaşık tür (çok parçalı bir alan) ve `Room` (çok parçalı alanların koleksiyonu).

Bu türleri kullanarak dizin oluşturabilir ve yükleyebilir ve yanıtı bir sorgudan yapılandırabilirsiniz:

```csharp
// Use-case: <Hotel> in a field definition
FieldBuilder fieldBuilder = new FieldBuilder();
var searchFields = fieldBuilder.Build(typeof(Hotel));

// Use-case: <Hotel> in a response
private static void WriteDocuments(SearchResults<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.GetResults())
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

### <a name="field-definitions"></a>Alan tanımları

.NET 'teki veri modeliniz ve buna karşılık gelen dizin şeması, son kullanıcıya vermek istediğiniz arama deneyimini desteklemelidir. Bir arama dizinindeki arama belgesi gibi, .NET içindeki her üst düzey nesne, Kullanıcı arabiriminizdeki mevcut bir arama sonucuna karşılık gelir. Örneğin, bir otel arama uygulamasında son kullanıcılarınızın otel adına, otel özelliklerine veya belirli bir odanın özelliklerine göre arama yapmak isteyebilirsiniz. 

Her bir sınıfta, bir alan, nasıl kullanıldığını tanımlayan bir veri türü ve öznitelikleri ile tanımlanır. Her sınıftaki her bir ortak özelliğin adı, Dizin tanımında aynı ada sahip bir alana eşlenir. 

Otel sınıfından birkaç alan tanımı çeken aşağıdaki kod parçacığına göz atın. Adres ve odalar 'ın kendi sınıf tanımlarına sahip C# türleri olduğuna dikkat edin (bunları görüntülemek istiyorsanız örnek koda bakın). Her ikisi de karmaşık türlerdir. Daha fazla bilgi için bkz. [karmaşık türleri Modelme](search-howto-complex-data-types.md).

```csharp
public partial class Hotel
{
    [SimpleField(IsKey = true, IsFilterable = true)]
    public string HotelId { get; set; }

    [SearchableField(IsSortable = true)]
    public string HotelName { get; set; }

    [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.EnLucene)]
    public string Description { get; set; }

    [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
    public string Category { get; set; }

    [JsonIgnore]
    public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;

    [SearchableField]
    public Address Address { get; set; }

    public Room[] Rooms { get; set; }
```

#### <a name="choosing-a-field-class"></a>Alan sınıfı seçme

Alanları tanımlarken, temel [`SearchField`](/dotnet/api/azure.search.documents.indexes.models.searchfield) sınıfını kullanabilir veya önceden yapılandırılmış özelliklerle "Şablonlar" olarak görev yapan türev yardımcı modelleri kullanabilirsiniz.

Dizininizdeki tek bir alan, belge anahtarı () olarak kullanılmalıdır `IsKey = true` . Bu bir dize olmalıdır ve her belgeyi benzersiz şekilde tanımlamalıdır. Ayrıca `IsHidden = true` , bu, arama sonuçlarında görünemediği anlamına gelen olması gerekir.

| Alan türü | Açıklama ve kullanım |
|------------|-----------------------|
| [`SearchField`](/dotnet/api/azure.search.documents.indexes.models.searchfield) | Temel sınıf, çoğu özelliği null olarak, hariç tutulan `Name` ve `AnalyzerName` Standart Lucene varsayılan değer olarak ayarlanmıştır. |
| [`SimpleField`](/dotnet/api/azure.search.documents.indexes.models.simplefield) | Yardımcı model. Herhangi bir veri türü olabilir, her zaman aranabilir değildir (tam metin arama sorguları için yok sayılır) ve alınabilir (gizli değildir). Diğer öznitelikler varsayılan olarak kapalıdır, ancak etkinleştirilebilir. `SimpleField`Yalnızca filtrelerde, modellerde veya Puanlama profillerinde kullanılan belge kimlikleri veya alanları için kullanabilirsiniz. Bu durumda, bir belge KIMLIĞI gibi senaryo için gerekli olan tüm öznitelikleri uyguladığınızdan emin olun `IsKey = true` . Daha fazla bilgi için bkz. [SimpleFieldAttribute.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/src/Indexes/SimpleFieldAttribute.cs) in Source Code. |
| [`SearchableField`](/dotnet/api/azure.search.documents.indexes.models.searchablefield) | Yardımcı model. Bir dize olmalı ve her zaman aranabilir ve alınabilir olmalıdır. Diğer öznitelikler varsayılan olarak kapalıdır, ancak etkinleştirilebilir. Bu alan türü aranabilir olduğundan, eş anlamlıları ve çözümleyici özelliklerinin tam olarak tamamlayıcısını destekler. Daha fazla bilgi için bkz. [SearchableFieldAttribute.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/src/Indexes/SearchableFieldAttribute.cs) in Source Code. |

Temel `SearchField` API 'yi veya yardımcı modellerden birini kullanmanıza bakılmaksızın, filtre, model ve sıralama özniteliklerini açıkça etkinleştirmeniz gerekir. Örneğin, yukarıdaki örnekte gösterildiği gibi [ısfilterable](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfilterable), [ıssıralanabilir](/dotnet/api/azure.search.documents.indexes.models.searchfield.issortable)ve [ıbıı tablosu](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfacetable) açıkça adlandırılmalıdır.

#### <a name="adding-field-attributes"></a>Alan öznitelikleri ekleme

Her alanın,, ve gibi özniteliklerle nasıl tasarlandığına dikkat edin `IsFilterable` `IsSortable` `IsKey` `AnalyzerName` . Bu öznitelikler, [bir Azure bilişsel arama dizininde karşılık gelen alan özniteliklerine](/rest/api/searchservice/create-index)doğrudan eşlenir. `FieldBuilder`Sınıfı, dizin için alan tanımları oluşturmak üzere bu özellikleri kullanır.

#### <a name="field-type-mapping"></a>Alan türü eşleme

Özelliklerin .NET türleri, Dizin tanımındaki eşdeğer alan türleriyle eşlenir. Örneğin, `Category` dize özelliği `Edm.String` türündeki `category` alanına eşlenir. ,,, Vb. arasında benzer tür eşlemeleri vardır `bool?` `Edm.Boolean` `DateTimeOffset?` `Edm.DateTimeOffset` . 

Özelliği fark etmeniz mı istiyordunuz `SmokingAllowed` ?

```csharp
[JsonIgnore]
public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;
```

`JsonIgnore`Bu özelliğindeki öznitelik, `FieldBuilder` öğesine onu bir alan olarak dizine serileştirmeyeceğini söyler.  Bu, uygulamanızda yardımcılar olarak kullanabileceğiniz istemci tarafı hesaplanmış Özellikler oluşturmanın harika bir yoludur.  Bu durumda, `SmokingAllowed` özelliği koleksiyonda herhangi birinin `Room` `Rooms` smome izin verip etmediğini yansıtır. Tümü yanlışsa, tüm otelin smome izin vermiyor olduğunu gösterir.

## <a name="load-an-index"></a>Dizin yükleme

İçindeki sonraki adım, `Main` Yeni oluşturulan "oteller" dizinini doldurur. Bu dizin oluşturma işlemi şu yöntemde yapılır: (bazı kodlar "... ile değiştirilmiştir" illüstrasyon amaçlıdır. Tam veri popülasyon kodu için tam örnek çözümüne bakın.)

```csharp
private static void UploadDocuments(SearchClient searchClient)
{
    IndexDocumentsBatch<Hotel> batch = IndexDocumentsBatch.Create(
        IndexDocumentsAction.Upload(
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
            }),
        IndexDocumentsAction.Upload(
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
            }),
        IndexDocumentsAction.Upload(
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

    try
    {
        IndexDocumentsResult result = searchClient.IndexDocuments(batch);
    }
    catch (Exception)
    {
        // Sometimes when your Search service is under load, indexing will fail for some of the documents in
        // the batch. Depending on your application, you can take compensating actions like delaying and
        // retrying. For this simple demo, we just log the failed document keys and continue.
        Console.WriteLine("Failed to index some of the documents: {0}");
    }

    Console.WriteLine("Waiting for documents to be indexed...\n");
    Thread.Sleep(2000);
```

Bu yöntemin dört bölümü vardır. İlki, `Hotel` `Room` dizine yüklemek üzere giriş verilerimizde kullanılacak 3 nesne ile her biri 3 nesneden oluşan bir dizi oluşturur. Bu veriler kolaylık sağlaması için sabit olarak kodlanır. Gerçek bir uygulamada, veriler büyük olasılıkla SQL veritabanı gibi bir dış veri kaynağından gelmiş olacaktır.

İkinci bölüm, [`IndexDocumentsBatch`](/dotnet/api/azure.search.documents.models.indexdocumentsbatch) belgeleri içeren bir oluşturur. Bu durumda, çağırarak, toplu işe uygulamak istediğiniz işlemi bu durumda belirtirsiniz [`IndexDocumentsAction.Upload`](/dotnet/api/azure.search.documents.models.indexdocumentsaction.upload) . Batch daha sonra yöntemi tarafından Azure Bilişsel Arama dizinine yüklenir [`IndexDocuments`](/dotnet/api/azure.search.documents.searchclient.indexdocuments) .

> [!NOTE]
> Bu örnekte, belgeleri karşıya yüklüyoruz. Değişiklikleri varolan belgelere birleştirmek veya belgeleri silmek isterseniz,, veya çağırarak toplu işlemler oluşturabilirsiniz `IndexDocumentsAction.Merge` `IndexDocumentsAction.MergeOrUpload` `IndexDocumentsAction.Delete` . Tek bir toplu işte farklı işlemleri, `IndexBatch.New` `IndexDocumentsAction` her biri Azure bilişsel arama bir belge üzerinde belirli bir işlemi gerçekleştirmesini söyleyen bir nesne koleksiyonu alan çağırarak de karıştırabilirsiniz. ,, Vb `IndexDocumentsAction` . gibi karşılık gelen yöntemi çağırarak, her biri kendi işlemi ile `IndexDocumentsAction.Merge` oluşturabilirsiniz `IndexAction.Upload` .
> 

Bu yöntemin üçüncü bölümü, dizin oluşturma için önemli bir hata durumunu işleyen bir catch bloğudur. Arama hizmetiniz toplu işteki bazı belge dizinini dizinlemezse, bir `IndexBatchException` tarafından oluşturulur `IndexDocuments` . Bu özel durum, hizmetiniz ağır yük altındayken belge dizinleniyorsa gerçekleşebilir. **Bu durumu açık bir şekilde kodunuzda işlemenizi kesinlikle öneririz.** Başarısız olan belgelere dizin oluşturmayı geciktirip sonra yeniden deneyebilir veya günlük tutup örneğin devam ettiği şekilde devam edebilir veya uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak başka bir şey yapabilirsiniz.

Son olarak, `UploadDocuments` Yöntem iki saniye gecikdi. Arama hizmetinizde dizin oluşturma zaman uyumsuz olarak gerçekleştirilir, bu nedenle örnek uygulamanın, belgelerin aranabilmesi için kullanılabilir olduğundan emin olmak için kısa bir süre beklemesi gerekir. Bu gibi gecikmeler genellikle yalnızca gösterilerde, testlerde ve örnek uygulamalarda gereklidir.

### <a name="call-uploaddocuments-in-main"></a>Main () içinde UploadDocuments çağırma

Aşağıdaki kod parçacığı, [`SearchClient`](/dotnet/api/azure.search.documents.searchclient) ındexclient metodunu kullanarak bir örneğini ayarlar [`GetSearchClient`](/dotnet/api/azure.search.documents.indexes.searchindexclient.getsearchclient) . Indexclient, isteklerinde belge yükleme veya yenileme için gereken bir yönetici API anahtarı kullanır.

Alternatif bir yaklaşım doğrudan çağrı yapmak `SearchClient` ve üzerinde bir yönetıcı API anahtarı geçirmektir `AzureKeyCredential` .

```csharp
SearchClient searchClient = indexClient.GetSearchClient(indexName);

Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(searchClient);
```

## <a name="run-queries"></a>Sorgu çalıştırma

İlk olarak, `SearchClient` **üzerindeappsettings.js** arama uç noktasını ve sorgu API anahtarını okuyan bir a ayarlayın:

```csharp
private static SearchClient CreateSearchClientForQueries(string indexName, IConfigurationRoot configuration)
{
    string searchServiceEndPoint = configuration["SearchServiceEndPoint"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchClient searchClient = new SearchClient(new Uri(searchServiceEndPoint), indexName, new AzureKeyCredential(queryApiKey));
    return searchClient;
}
```

İkinci olarak, bir sorgu isteği gönderen bir yöntem tanımlayın. 

Yöntem bir sorguyu her yürüttüğünde, yeni bir [`SearchOptions`](/dotnet/api/azure.search.documents.searchoptions) nesne oluşturur. Bu nesne, sorgu için sıralama, filtreleme, sayfalama ve Faks oluşturma gibi ek seçenekleri belirtmek için kullanılır. Bu yöntemde,, `Filter` `Select` ve `OrderBy` özelliğini farklı sorgular için ayarlıyoruz. Arama sorgusu ifade sözdizimi hakkında daha fazla bilgi için, [basit sorgu söz dizimi](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).

Sonraki adım aslında arama sorgusunu yürütmekte. Aramayı çalıştırmak yöntemi kullanılarak yapılır `SearchClient.Search` . Her sorgu için, arama metnini dize olarak kullanılacak şekilde geçirin (veya `"*"` arama metni yoksa) ve daha önce oluşturulan arama seçeneklerini kullanın. Ayrıca `Hotel` `SearchClient.Search` , SDK 'nın arama sonuçlarındaki belgelerin türünü türündeki nesnelere serisini oluşturmasını söyleyen için tür parametresi olarak da belirttik `Hotel` .

```csharp
private static void RunQueries(SearchClient searchClient)
{
    SearchOptions options;
    SearchResults<Hotel> results;

    Console.WriteLine("Query 1: Search for 'motel'. Return only the HotelName in results:\n");

    options = new SearchOptions();
    options.Select.Add("HotelName");

    results = searchClient.Search<Hotel>("motel", options);

    WriteDocuments(results);

    Console.Write("Query 2: Apply a filter to find hotels with rooms cheaper than $100 per night, ");
    Console.WriteLine("returning the HotelId and Description:\n");

    options = new SearchOptions()
    {
        Filter = "Rooms/any(r: r/BaseRate lt 100)"
    };
    options.Select.Add("HotelId");
    options.Select.Add("Description");

    results = searchClient.Search<Hotel>("*", options);

    WriteDocuments(results);

    Console.Write("Query 3: Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
    Console.WriteLine("lastRenovationDate:\n");

    options =
        new SearchOptions()
        {
            Size = 2
        };
    options.OrderBy.Add("LastRenovationDate desc");
    options.Select.Add("HotelName");
    options.Select.Add("LastRenovationDate");

    results = searchClient.Search<Hotel>("*", options);

    WriteDocuments(results);

    Console.WriteLine("Query 4: Search the HotelName field for the term 'hotel':\n");

    options = new SearchOptions();
    options.SearchFields.Add("HotelName");

    //Adding details to select, because "Location" is not supported yet when deserialize search result to "Hotel"
    options.Select.Add("HotelId");
    options.Select.Add("HotelName");
    options.Select.Add("Description");
    options.Select.Add("Category");
    options.Select.Add("Tags");
    options.Select.Add("ParkingIncluded");
    options.Select.Add("LastRenovationDate");
    options.Select.Add("Rating");
    options.Select.Add("Address");
    options.Select.Add("Rooms");

    results = searchClient.Search<Hotel>("hotel", options);

    WriteDocuments(results);
}
```

Üçüncü olarak, her belgeyi konsola yazdırarak yanıtı yazan bir yöntem tanımlayın:

```csharp
private static void WriteDocuments(SearchResults<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.GetResults())
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

### <a name="call-runqueries-in-main"></a>Main () içinde RunQueries çağrısı yapın

```csharp
SearchClient indexClientForQueries = CreateSearchClientForQueries(indexName, configuration);

Console.WriteLine("{0}", "Running queries...\n");
RunQueries(indexClientForQueries);
```

### <a name="explore-query-constructs"></a>Sorgu yapılarını keşfet

Her sorguya sırayla daha yakından göz atalım. İlk sorguyu yürütmek için kod aşağıda verilmiştir:

```csharp
options = new SearchOptions();
options.Select.Add("HotelName");

results = searchClient.Search<Hotel>("motel", options);

WriteDocuments(results);
```

Bu durumda, herhangi bir aranabilir alanda "Motel" sözcüğünün tüm dizinini arıyor ve yalnızca seçenekte belirtilen otel adlarını almak istiyoruz `Select` . Sonuçlar şunlardır:

```output
Name: Secret Point Motel

Name: Twin Dome Motel
```

İkinci sorguda, gece ücreti $100 ' dan az olan oda seçmek için bir filtre kullanın. Sonuçlarda yalnızca otel KIMLIĞI ve açıklaması Döndür:

```csharp
options = new SearchOptions()
{
    Filter = "Rooms/any(r: r/BaseRate lt 100)"
};
options.Select.Add("HotelId");
options.Select.Add("Description");

results = searchClient.Search<Hotel>("*", options);
```

Yukarıdaki sorgu, `$filter` `Rooms/any(r: r/BaseRate lt 100)` dizinde bulunan belgeleri filtrelemek Için bir OData ifadesi kullanır. Bu, Oda koleksiyonundaki her öğeye ' BaseRate lt 100 ' öğesini uygulamak için [Any işlecini](./search-query-odata-collection-operators.md) kullanır. Daha fazla bilgi için bkz. [OData filtresi sözdizimi](./query-odata-filter-orderby-syntax.md).

Üçüncü sorguda en son yeniden kiralanan en son iki oteller bulun ve otel adı ile son yeniden oluşturma tarihini gösterin. Kod aşağıdaki gibidir: 

```csharp
options =
    new SearchOptions()
    {
        Size = 2
    };
options.OrderBy.Add("LastRenovationDate desc");
options.Select.Add("HotelName");
options.Select.Add("LastRenovationDate");

results = searchClient.Search<Hotel>("*", options);

WriteDocuments(results);
```

Son sorguda, "otel" kelimesiyle eşleşen tüm oteller adlarını bulun:

```csharp
options.Select.Add("HotelId");
options.Select.Add("HotelName");
options.Select.Add("Description");
options.Select.Add("Category");
options.Select.Add("Tags");
options.Select.Add("ParkingIncluded");
options.Select.Add("LastRenovationDate");
options.Select.Add("Rating");
options.Select.Add("Address");
options.Select.Add("Rooms");

results = searchClient.Search<Hotel>("hotel", options);

WriteDocuments(results);
```

Bu bölüm, .NET SDK 'ya giriş sonucuna varır, ancak burada durmayın. Sonraki bölümde, Azure Bilişsel Arama ile programlama hakkında daha fazla bilgi edinmek için ek kaynaklar önerilir.

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure.Search.Documlar](/dotnet/api/azure.search.documents) ve [REST API](/rest/api/searchservice/) için API başvuru belgelerine bakın

+ [Azure-Search-DotNet-Samples](https://github.com/Azure-Samples/azure-search-dotnet-samples) ve [Search-Başlarken-dotnet](https://github.com/Azure-Samples/search-dotnet-getting-started) ' de Azure.Search.Documtları temel alarak diğer kod örneklerine gözatın

+ Çeşitli nesneleri adlandırmayla ilgili kuralları öğrenmek için [adlandırma kurallarını](/rest/api/searchservice/Naming-rules) gözden geçirin

+ [Desteklenen veri türlerini](/rest/api/searchservice/Supported-data-types) gözden geçirin