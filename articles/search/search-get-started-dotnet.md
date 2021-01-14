---
title: "Hızlı başlangıç: .NET 'te arama dizini oluşturma"
titleSuffix: Azure Cognitive Search
description: Bu C# hızlı başlangıçta Dizin oluşturmayı, verileri yüklemeyi ve Azure.Search.Documstalar istemci kitaplığını kullanarak sorguları çalıştırmayı öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 11/20/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: f0d912d5b14932c43d109f8f955d5f16381cf773
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98180107"
---
# <a name="quickstart-create-a-search-index-using-the-azuresearchdocuments-client-library"></a>Hızlı başlangıç: Azure.Search.Documstalar istemci kitaplığını kullanarak arama dizini oluşturma

C# ' de bir arama dizini oluşturan, yükleyen ve sorgulayan bir .NET Core konsol uygulaması oluşturmak için yeni [Azure.Search.Documstalar (sürüm 11) istemci kitaplığını](/dotnet/api/overview/azure/search.documents-readme) kullanın.

Tamamlanmış bir projeden başlamak için [kaynak kodu indirebilir](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/quickstart/v11) veya kendi kendinize oluşturmak için bu makaledeki adımları takip edebilirsiniz.

> [!NOTE]
> Daha önceki bir sürüm mi arıyorsunuz? Bunun yerine [Microsoft. Azure. Search ile v10 arasındaki kullanarak arama dizini oluşturma](search-get-started-dotnet-v10.md) konusuna bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdaki araçlara ve hizmetlere sahip olursunuz:

+ Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/).

+ Bir Azure Bilişsel Arama hizmeti. [Bir hizmet oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

+ [Visual Studio](https://visualstudio.microsoft.com/downloads/), tüm sürüm. Örnek kod, Visual Studio 2019 ' un ücretsiz topluluk sürümünde test edilmiştir.

Projenizi ayarlarken [Azure.Search.Documstalar NuGet paketini](https://www.nuget.org/packages/Azure.Search.Documents/)indirirsiniz.

.NET için Azure SDK, en düşük gereksinimleri .NET Framework 4.6.1 ve .NET Core 2,0 anlamına gelen [.NET Standard 2,0](/dotnet/standard/net-standard#net-implementation-support)' a uyar.

## <a name="set-up-your-project"></a>Projenizi ayarlama

Hizmet bağlantı bilgilerini toplayın ve ardından Visual Studio 'Yu başlatıp .NET Core üzerinde çalışabilen yeni bir konsol uygulaması projesi oluşturun.

<a name="get-service-info"></a>

### <a name="copy-a-key-and-endpoint"></a>Anahtar ve uç nokta kopyalama

Hizmete yapılan çağrılar, her istekte bir URL uç noktası ve erişim anahtarı gerektirir. İlk adım olarak, projenize eklenecek API anahtarını ve URL 'YI bulun. Daha sonraki bir adımda istemciyi oluştururken her iki değeri de belirtirsiniz.

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

2. **Ayarlar**  >  **anahtarlarında**, hizmet üzerinde tam haklar için bir yönetici anahtarı alın, nesne oluşturuyorsanız veya silerseniz gereklidir. İki adet değiştirilebilir birincil ve ikincil anahtar vardır. Bunlardan birini kullanabilirsiniz.

   ![HTTP uç noktası ve erişim anahtarı al](media/search-get-started-rest/get-url-key.png "HTTP uç noktası ve erişim anahtarı al")

Tüm istekler hizmetinize gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

### <a name="install-the-nuget-package"></a>NuGet paketini yükler

Proje oluşturulduktan sonra, istemci kitaplığını ekleyin. [Azure.Search.Documstalar paketi](https://www.nuget.org/packages/Azure.Search.Documents/) , .net 'teki bir arama hizmetiyle çalışmak için kullanılan tüm API 'leri sağlayan bir istemci kitaplığından oluşur.

1. Visual Studio 'Yu başlatın ve bir .NET Core konsol uygulaması oluşturun.

1. **Araçlar**  >  **NuGet Paket Yöneticisi**' nde **çözüm için NuGet Paketlerini Yönet...** seçeneğini belirleyin. 

1. **Gözat**’a tıklayın.

1. `Azure.Search.Documents`Sürüm 11,0 veya üzerini arayın ve seçin.

1. Derlemeyi projenize ve çözümünüze eklemek için **sağdaki aç '** a tıklayın.

### <a name="create-a-search-client"></a>Arama İstemcisi Oluşturma

1. **Program.cs** içinde, ad alanını olarak değiştirin `AzureSearch.SDK.Quickstart.v11` ve ardından aşağıdaki yönergeleri ekleyin `using` .

   ```csharp
   using Azure;
   using Azure.Search.Documents;
   using Azure.Search.Documents.Indexes;
   using Azure.Search.Documents.Indexes.Models;
   using Azure.Search.Documents.Models;
   ```

1. İki istemci oluşturun: [Searchındexclient](/dotnet/api/azure.search.documents.indexes.searchindexclient) dizini oluşturur ve [searchclient](/dotnet/api/azure.search.documents.searchclient) , var olan bir dizini yükler ve sorgular. Her iki hizmet uç noktası ve oluşturma/silme haklarıyla kimlik doğrulaması için bir yönetici API anahtarı gereklidir.

   ```csharp
   static void Main(string[] args)
   {
       string serviceName = "<YOUR-SERVICE-NAME>";
       string indexName = "hotels-quickstart";
       string apiKey = "<YOUR-ADMIN-API-KEY>";

        // Create a SearchIndexClient to send create/delete index commands
        Uri serviceEndpoint = new Uri($"https://{serviceName}.search.windows.net/");
        AzureKeyCredential credential = new AzureKeyCredential(apiKey);
        SearchIndexClient adminClient = new SearchIndexClient(serviceEndpoint, credential);

        // Create a SearchClient to load and query documents
        SearchClient srchclient = new SearchClient(serviceEndpoint, indexName, credential);
    ```

## <a name="1---create-an-index"></a>1 - Dizin oluşturma

Bu hızlı başlangıç, otel verileriyle birlikte yükleyeceksiniz ve sorguları yürütmek için bir oteller dizini oluşturur. Bu adımda, dizindeki alanları tanımlayın. Her alan tanımı, alanın nasıl kullanıldığını tespit eden bir ad, veri türü ve öznitelikler içerir.

Bu örnekte, Azure.Search.Documstalar kitaplığının zaman uyumlu yöntemleri kolaylık ve okunabilirlik için kullanılır. Ancak, üretim senaryolarında uygulamanızın ölçeklenebilir ve yanıt vermesini sağlamak için zaman uyumsuz yöntemler kullanmanız gerekir. Örneğin, [CreateIndex](/dotnet/api/azure.search.documents.indexes.searchindexclient.createindex)yerine [Createındexasync](/dotnet/api/azure.search.documents.indexes.searchindexclient.createindexasync) ' i kullanacaksınız.

1. Projenize boş bir sınıf tanımı ekleyin: **Hotel.cs**

1. Bir otel belgesinin yapısını tanımlamak için aşağıdaki kodu **Hotel.cs** 'e kopyalayın. Alanındaki öznitelikler bir uygulamada nasıl kullanıldığını belirleme. Örneğin, `IsFilterable` özniteliği bir filtre ifadesini destekleyen her alana atanmalıdır.

    ```csharp
    using System;
    using System.Text.Json.Serialization;
    using Azure.Search.Documents.Indexes;
    using Azure.Search.Documents.Indexes.Models;

    namespace AzureSearch.Quickstart
    {
        public partial class Hotel
        {
            [SimpleField(IsKey = true, IsFilterable = true)]
            public string HotelId { get; set; }

            [SearchableField(IsSortable = true)]
            public string HotelName { get; set; }

            [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.EnLucene)]
            public string Description { get; set; }

            [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.FrLucene)]
            [JsonPropertyName("Description_fr")]
            public string DescriptionFr { get; set; }

            [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public string Category { get; set; }

            [SearchableField(IsFilterable = true, IsFacetable = true)]
            public string[] Tags { get; set; }

            [SimpleField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public bool? ParkingIncluded { get; set; }

            [SimpleField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public DateTimeOffset? LastRenovationDate { get; set; }

            [SimpleField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public double? Rating { get; set; }

            [SearchableField]
            public Address Address { get; set; }
        }
    }
    ```

   Azure.Search.Documstalar istemci kitaplığında, alan tanımlarını kolaylaştırmak için [Searchablefield](/dotnet/api/azure.search.documents.indexes.models.searchablefield) ve [simplefield](/dotnet/api/azure.search.documents.indexes.models.simplefield) kullanabilirsiniz. Her ikisi de bir [Searchfield](/dotnet/api/azure.search.documents.indexes.models.searchfield) 'ın türevleri ve kodunuzu basitleştirecek olabilir:

   + `SimpleField` herhangi bir veri türü olabilir, her zaman aranabilir değildir (tam metin arama sorguları için yok sayılır) ve alınabilir (gizli değildir). Diğer öznitelikler varsayılan olarak kapalıdır, ancak etkinleştirilebilir. `SimpleField`Yalnızca filtrelerde, modellerde veya Puanlama profillerinde kullanılan belge kimlikleri veya alanları için kullanabilirsiniz. Bu durumda, bir belge KIMLIĞI gibi senaryo için gerekli olan tüm öznitelikleri uyguladığınızdan emin olun `IsKey = true` . Daha fazla bilgi için bkz. [SimpleFieldAttribute.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/src/Indexes/SimpleFieldAttribute.cs) in Source Code.

   + `SearchableField` bir dize olmalı ve her zaman aranabilir ve alınabilir olmalıdır. Diğer öznitelikler varsayılan olarak kapalıdır, ancak etkinleştirilebilir. Bu alan türü aranabilir olduğundan, eş anlamlıları ve çözümleyici özelliklerinin tam olarak tamamlayıcısını destekler. Daha fazla bilgi için bkz. [SearchableFieldAttribute.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/src/Indexes/SearchableFieldAttribute.cs) in Source Code.

   Temel `SearchField` API 'yi veya yardımcı modellerden birini kullanmanıza bakılmaksızın, filtre, model ve sıralama özniteliklerini açıkça etkinleştirmeniz gerekir. Örneğin, yukarıdaki örnekte gösterildiği gibi [ısfilterable](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfilterable), [ıssıralanabilir](/dotnet/api/azure.search.documents.indexes.models.searchfield.issortable)ve [ıbıı tablosu](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfacetable) açıkça adlandırılmalıdır. 

1. Projenize ikinci boş bir sınıf tanımı ekleyin: **Address.cs**.  Aşağıdaki kodu sınıfına kopyalayın.

   ```csharp
   using Azure.Search.Documents.Indexes;

    namespace AzureSearch.Quickstart
    {
        public partial class Address
        {
            [SearchableField(IsFilterable = true)]
            public string StreetAddress { get; set; }

            [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public string City { get; set; }

            [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public string StateProvince { get; set; }

            [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public string PostalCode { get; set; }

            [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
            public string Country { get; set; }
        }
    }
   ```

1. İki sınıf daha oluşturun: ToString () için **Hotel.Methods.cs** ve **Address.Methods.cs** geçersiz kılmaları. Bu sınıflar, konsol çıkışında arama sonuçlarını işlemek için kullanılır.  Bu sınıfların içeriği bu makalede sağlanmaz, ancak kodu [GitHub 'daki dosyalardan](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/quickstart/v11/AzureSearchQuickstart-v11)kopyalayabilirsiniz.

1. **Program.cs** içinde, bir [searchındex](/dotnet/api/azure.search.documents.indexes.models.searchindex) nesnesi oluşturun ve ardından, arama hizmetinizde dizini Ifade etmek için [CreateIndex](/dotnet/api/azure.search.documents.indexes.searchindexclient.createindex) yöntemini çağırın. Dizin, belirtilen alanlarda otomatik tamamlamayı etkinleştirmek için de bir [SearchSuggester](/dotnet/api/azure.search.documents.indexes.models.searchsuggester) içerir.

   ```csharp
    // Create hotels-quickstart index
    private static void CreateIndex(string indexName, SearchIndexClient adminClient)
    {
        FieldBuilder fieldBuilder = new FieldBuilder();
        var searchFields = fieldBuilder.Build(typeof(Hotel));

        var definition = new SearchIndex(indexName, searchFields);

        var suggester = new SearchSuggester("sg", new[] { "HotelName", "Category", "Address/City", "Address/StateProvince" });
        definition.Suggesters.Add(suggester);

        adminClient.CreateOrUpdateIndex(definition);
    }
   ```

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2-belge yükleme

Azure Bilişsel Arama, hizmette depolanan içerikleri arar. Bu adımda, az önce oluşturduğunuz otel dizinine uygun JSON belgelerini yükleyeceksiniz.

Azure Bilişsel Arama 'de, arama belgeleri, sorguların dizin oluşturma ve çıkışlara yönelik giriş olan veri yapılarıdır. Bir dış veri kaynağından elde edilen belge girişleri bir veritabanındaki satırlar, blob depolamada Bloblar veya diskteki JSON belgeleri olabilir. Bu örnekte, bir kısayol çekiyoruz ve JSON belgelerini kodun kendisinde dört otel için katıştırıyoruz. 

Belgeler karşıya yüklenirken [ındexdocumentsbatch](/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1) nesnesini kullanmanız gerekir. Bir `IndexDocumentsBatch` nesne, her biri bir belge ve Azure 'a ([karşıya yükleme, birleştirme, silme ve mergeorupload](search-what-is-data-import.md#indexing-actions)) bilişsel arama bir özellik içeren bir [eylem](/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1.actions)koleksiyonu içerir.

1. **Program.cs** içinde bir dizi belge ve Dizin eylemi oluşturun ve sonra diziyi öğesine geçirin `IndexDocumentsBatch` . Aşağıdaki belgeler, Otel Sınıfı tarafından tanımlanan oteller-hızlı başlangıç dizinine uygundur.

    ```csharp
    // Upload documents in a single Upload request.
    private static void UploadDocuments(SearchClient searchClient)
    {
        IndexDocumentsBatch<Hotel> batch = IndexDocumentsBatch.Create(
            IndexDocumentsAction.Upload(
                new Hotel()
                {
                    HotelId = "1",
                    HotelName = "Secret Point Motel",
                    Description = "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
                    DescriptionFr = "L'hôtel est idéalement situé sur la principale artère commerciale de la ville en plein cœur de New York. A quelques minutes se trouve la place du temps et le centre historique de la ville, ainsi que d'autres lieux d'intérêt qui font de New York l'une des villes les plus attractives et cosmopolites de l'Amérique.",
                    Category = "Boutique",
                    Tags = new[] { "pool", "air conditioning", "concierge" },
                    ParkingIncluded = false,
                    LastRenovationDate = new DateTimeOffset(1970, 1, 18, 0, 0, 0, TimeSpan.Zero),
                    Rating = 3.6,
                    Address = new Address()
                    {
                        StreetAddress = "677 5th Ave",
                        City = "New York",
                        StateProvince = "NY",
                        PostalCode = "10022",
                        Country = "USA"
                    }
                }),
            IndexDocumentsAction.Upload(
                new Hotel()
                {
                    HotelId = "2",
                    HotelName = "Twin Dome Motel",
                    Description = "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
                    DescriptionFr = "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
                    Category = "Boutique",
                    Tags = new[] { "pool", "free wifi", "concierge" },
                    ParkingIncluded = false,
                    LastRenovationDate = new DateTimeOffset(1979, 2, 18, 0, 0, 0, TimeSpan.Zero),
                    Rating = 3.60,
                    Address = new Address()
                    {
                        StreetAddress = "140 University Town Center Dr",
                        City = "Sarasota",
                        StateProvince = "FL",
                        PostalCode = "34243",
                        Country = "USA"
                    }
                }),
            IndexDocumentsAction.Upload(
                new Hotel()
                {
                    HotelId = "3",
                    HotelName = "Triple Landscape Hotel",
                    Description = "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
                    DescriptionFr = "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
                    Category = "Resort and Spa",
                    Tags = new[] { "air conditioning", "bar", "continental breakfast" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(2015, 9, 20, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4.80,
                    Address = new Address()
                    {
                        StreetAddress = "3393 Peachtree Rd",
                        City = "Atlanta",
                        StateProvince = "GA",
                        PostalCode = "30326",
                        Country = "USA"
                    }
                }),
            IndexDocumentsAction.Upload(
                new Hotel()
                {
                    HotelId = "4",
                    HotelName = "Sublime Cliff Hotel",
                    Description = "Sublime Cliff Hotel is located in the heart of the historic center of Sublime in an extremely vibrant and lively area within short walking distance to the sites and landmarks of the city and is surrounded by the extraordinary beauty of churches, buildings, shops and monuments. Sublime Cliff is part of a lovingly restored 1800 palace.",
                    DescriptionFr = "Le sublime Cliff Hotel est situé au coeur du centre historique de sublime dans un quartier extrêmement animé et vivant, à courte distance de marche des sites et monuments de la ville et est entouré par l'extraordinaire beauté des églises, des bâtiments, des commerces et Monuments. Sublime Cliff fait partie d'un Palace 1800 restauré avec amour.",
                    Category = "Boutique",
                    Tags = new[] { "concierge", "view", "24-hour front desk service" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1960, 2, 06, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4.60,
                    Address = new Address()
                    {
                        StreetAddress = "7400 San Pedro Ave",
                        City = "San Antonio",
                        StateProvince = "TX",
                        PostalCode = "78216",
                        Country = "USA"
                    }
                })
            );

        try
        {
            IndexDocumentsResult result = searchClient.IndexDocuments(batch);
        }
        catch (Exception)
        {
            // If for some reason any documents are dropped during indexing, you can compensate by delaying and
            // retrying. This simple demo just logs the failed document keys and continues.
            Console.WriteLine("Failed to index some of the documents: {0}");
        }
    }
    ```

    [Indexdocumentsbatch](/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1) nesnesini başlattığınızda, [searchclient](/dotnet/api/azure.search.documents.searchclient) nesneniz üzerindeki [ındexdocuments](/dotnet/api/azure.search.documents.searchclient.indexdocuments) ' ı çağırarak dizine gönderebilirsiniz.

1. Main () öğesine aşağıdaki satırları ekleyin. Belge yükleme, SearchClient kullanılarak yapılır, ancak işlem, genellikle Searchındexclient ile ilişkili olan hizmette yönetici hakları da gerektirir. Bu işlemi ayarlamaya yönelik bir yol, Searchındexclient (Bu örnekte adminClient) aracılığıyla SearchClient 'ı edinmenin bir yoludur.

   ```csharp
    SearchClient ingesterClient = adminClient.GetSearchClient(indexName);

    // Load documents
    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(ingesterClient);
   ```

1. Bu, tüm komutları sıralı olarak çalıştıran bir konsol uygulaması olduğundan, dizin oluşturma ve sorgular arasında 2 saniyelik bir bekleme süresi ekleyin.

    ```csharp
    // Wait 2 seconds for indexing to complete before starting queries (for demo and console-app purposes only)
    Console.WriteLine("Waiting for indexing...\n");
    System.Threading.Thread.Sleep(2000);
    ```

    2 saniyelik gecikme, zaman uyumsuz olan dizin oluşturma için dengelenir, böylece sorgular yürütülmeden önce tüm belgelerin dizini oluşturulabilir. Yalnızca gösteriler, testler ve örnek uygulamalarda yalnızca bir gecikmeden kodlama gereklidir.

## <a name="3---search-an-index"></a>3 - Dizin arama

İlk belge dizine eklendiğinde sorgu sonuçları elde edebilirsiniz, ancak dizininizin gerçek testi tüm belgelerin dizinlenene kadar beklemeniz gerekir.

Bu bölüm iki işlev parçasını ekler: Sorgu mantığı ve sonuçları. Sorgular için [arama](/dotnet/api/azure.search.documents.searchclient.search) yöntemini kullanın. Bu yöntem, arama metnini (sorgu dizesi) ve diğer [seçenekleri](/dotnet/api/azure.search.documents.searchoptions)alır.

[SearchResults](/dotnet/api/azure.search.documents.models.searchresults-1) sınıfı sonuçları temsil eder.

1. **Program.cs** içinde, arama sonuçlarını konsola yazdıran bir **writedocuments** yöntemi oluşturun.

    ```csharp
    // Write search results to console
    private static void WriteDocuments(SearchResults<Hotel> searchResults)
    {
        foreach (SearchResult<Hotel> result in searchResults.GetResults())
        {
            Console.WriteLine(result.Document);
        }

        Console.WriteLine();
    }
    ```

1. Sorguları yürütmek ve sonuçları döndürmek için bir **Runqueries** yöntemi oluşturun. Sonuçlar otel nesneleridir. Bu örnek, yöntem imzasını ve ilk sorguyu gösterir. Bu sorgu, belgedeki seçili alanları kullanarak sonucu oluşturmanıza olanak sağlayan Select parametresini gösterir.

    ```csharp
    // Run queries, use WriteDocuments to print output
    private static void RunQueries(SearchClient srchclient)
    {
        SearchOptions options;
        SearchResults<Hotel> response;

        Console.WriteLine("Query #1: Search on empty term '*' to return all documents, showing a subset of fields...\n");

        options = new SearchOptions()
        {
            IncludeTotalCount = true,
            Filter = "",
            OrderBy = { "" }
        };

        options.Select.Add("HotelId");
        options.Select.Add("HotelName");
        options.Select.Add("Address/City");

        response = srchclient.Search<Hotel>("*", options);
        WriteDocuments(response);
    ```

1. İkinci sorguda bir dönemde arama yapın, derecelendirmenin 4 ' ten büyük olduğu belgeleri seçen bir filtre ekleyin ve sonra derecelendirmeye göre azalan sırada sıralayın. Filtre, bir dizindeki [ısfilterable](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfilterable) alanları üzerinde değerlendirilen bir Boole deyimidir. Filtre sorguları, değerleri dahil etme veya hariç tutma. Bu nedenle, bir filtre sorgusuyla ilişkili bir ilgi puanı yoktur. 

    ```csharp
    Console.WriteLine("Query #2: Search on 'hotels', filter on 'Rating gt 4', sort by Rating in descending order...\n");

    options = new SearchOptions()
    {
        Filter = "Rating gt 4",
        OrderBy = { "Rating desc" }
    };

    options.Select.Add("HotelId");
    options.Select.Add("HotelName");
    options.Select.Add("Rating");

    response = srchclient.Search<Hotel>("hotels", options);
    WriteDocuments(response);
    ```

1. Üçüncü sorgu, tam metin arama işleminin belirli alanlara kapsamını atamak için kullanılan searchFields 'i gösterir.

    ```csharp
    Console.WriteLine("Query #3: Limit search to specific fields (pool in Tags field)...\n");

    options = new SearchOptions()
    {
        SearchFields = { "Tags" }
    };

    options.Select.Add("HotelId");
    options.Select.Add("HotelName");
    options.Select.Add("Tags");

    response = srchclient.Search<Hotel>("pool", options);
    WriteDocuments(response);
    ```

1. Dördüncü sorgu, çok yönlü bir gezinti yapısını yapılandırmak için kullanılabilen modelleri gösterir. 

   ```csharp
    Console.WriteLine("Query #4: Facet on 'Category'...\n");

    options = new SearchOptions()
    {
        Filter = ""
    };

    options.Facets.Add("Category");

    options.Select.Add("HotelId");
    options.Select.Add("HotelName");
    options.Select.Add("Category");

    response = srchclient.Search<Hotel>("*", options);
    WriteDocuments(response);
   ```

1. Beşinci sorguda, belirli bir belgeyi döndürün. Belge arama, bir sonuç kümesinde OnClick olayına tipik bir yanıt olur.

   ```csharp
    Console.WriteLine("Query #5: Look up a specific document...\n");

    Response<Hotel> lookupResponse;
    lookupResponse = srchclient.GetDocument<Hotel>("3");

    Console.WriteLine(lookupResponse.Value.HotelId);
   ```

1. Son sorguda, "sa" adlı kısmi Kullanıcı girişinin, dizinde tanımladığınız öneri aracı ilişkili sourceFields 'teki iki olası eşleştirmelere çözümlenen "sa" kısmi Kullanıcı girişinin benzetimi gösterilmektedir.

   ```csharp
    Console.WriteLine("Query #6: Call Autocomplete on HotelName that starts with 'sa'...\n");

    var autoresponse = srchclient.Autocomplete("sa", "sg");
    WriteDocuments(autoresponse);
   ```

1. Main () öğesine **Runqueries** ekleyin.

    ```csharp
    // Call the RunQueries method to invoke a series of queries
    Console.WriteLine("Starting queries...\n");
    RunQueries(srchclient);

    // End the program
    Console.WriteLine("{0}", "Complete. Press any key to end this program...\n");
    Console.ReadKey();
    ```

Önceki sorgular [bir sorgudaki terimlerle eşleşen](search-query-overview.md#types-of-queries)birden çok yol gösterir: tam metin arama, filtreler ve otomatik tamamlama.

Tam metin araması ve filtreler [Searchclient. Search](/dotnet/api/azure.search.documents.searchclient.search) yöntemi kullanılarak gerçekleştirilir. Bir arama sorgusu, bir `searchText` filtre ifadesi, [SearchOptions](/dotnet/api/azure.search.documents.searchoptions) sınıfının [Filter](/dotnet/api/azure.search.documents.searchoptions.filter) özelliğine geçirilekarşın, dizeye geçirilebilir. Aramasız filtrelemek için, `"*"` `searchText` [arama](/dotnet/api/azure.search.documents.searchclient.search) yönteminin parametresi için geçirin. Filtrelemeden aramak için, `Filter` özelliği unset olarak bırakın veya bir örneği geçirmeyin `SearchOptions` .

## <a name="run-the-program"></a>Programı çalıştırma

Uygulamayı yeniden derlemek ve programı tamamen çalıştırmak için F5 tuşuna basın. 

Çıktı, sorgu bilgilerinin ve sonuçların eklenmesiyle birlikte [Console. WriteLine](/dotnet/api/system.console.writeline)içindeki iletileri içerir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu C# hızlı başlangıçta, bir dizin oluşturmak, belgeler ile yüklemek ve sorguları çalıştırmak için bir görev kümesi üzerinde çalıştık. Farklı aşamalarda, okunabilirliği ve kavrama için kodu basitleştirecek kısayollar sunuyoruz. Temel kavramlara rahat bir şekilde yararlandıysanız, bilginizi borçlandırmaya yönelik alternatif yaklaşımların ve kavramların araştırmasını sağlayan bir sonraki makaleye önerilir. 

> [!div class="nextstepaction"]
> [.NET ' te geliştirme](search-howto-dotnet-sdk.md)

Bulut harcamalarınızı iyileştirmek ve kaydetmek istiyor musunuz?

> [!div class="nextstepaction"]
> [Maliyet yönetimi ile maliyetleri çözümlemeye başlayın](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)
