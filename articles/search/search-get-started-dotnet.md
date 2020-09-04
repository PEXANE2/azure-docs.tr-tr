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
ms.date: 08/05/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: ffcb2c741838b1292eaf0793cd625c99d252068c
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89462080"
---
# <a name="quickstart-create-a-search-index-using-the-azuresearchdocuments-client-library"></a>Hızlı başlangıç: Azure.Search.Documstalar istemci kitaplığını kullanarak arama dizini oluşturma

C# ' de bir arama dizini oluşturan, yükleyen ve sorgulayan bir .NET Core konsol uygulaması oluşturmak için yeni [Azure.Search.Documstalar (sürüm 11) istemci kitaplığını](/dotnet/api/overview/azure/search.documents-readme?view=azure-dotnet) kullanın.

Tamamlanmış bir projeden başlamak için [kaynak kodunu indirin](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/quickstart/v11) veya kendi kendinize oluşturmak için bu makaledeki adımları izleyin.

> [!NOTE]
> Daha önceki bir sürüm mi arıyorsunuz? Bunun yerine [Microsoft. Azure. Search ile v10 arasındaki kullanarak arama dizini oluşturma](search-get-started-dotnet-v10.md) konusuna bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdaki araçlara ve hizmetlere sahip olursunuz:

+ Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/).

+ Bir Azure Bilişsel Arama hizmeti. [Bir hizmet oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

+ [Visual Studio](https://visualstudio.microsoft.com/downloads/), tüm sürüm. Örnek kod, Visual Studio 2019 ' un ücretsiz topluluk sürümünde test edilmiştir.

<a name="get-service-info"></a>

## <a name="get-a-key-and-endpoint"></a>Anahtar ve uç nokta al

Hizmete yapılan çağrılar, her istekte bir URL uç noktası ve erişim anahtarı gerektirir. Her ikisiyle de bir arama hizmeti oluşturulur. bu nedenle, aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

2. **Ayarlar**  >  **anahtarlarında**, hizmet üzerinde tam haklar için bir yönetici anahtarı alın, nesne oluşturuyorsanız veya silerseniz gereklidir. İki adet değiştirilebilir birincil ve ikincil anahtar vardır. Bunlardan birini kullanabilirsiniz.

   ![HTTP uç noktası ve erişim anahtarı al](media/search-get-started-postman/get-url-key.png "HTTP uç noktası ve erişim anahtarı al")

Tüm istekler hizmetinize gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="set-up-your-project"></a>Projenizi ayarlama

Visual Studio 'Yu başlatın ve .NET Core üzerinde çalışabilen yeni bir konsol uygulaması projesi oluşturun. 

### <a name="install-the-nuget-package"></a>NuGet paketini yükler

Proje oluşturulduktan sonra, istemci kitaplığını ekleyin. [Azure.Search.Documstalar paketi](https://www.nuget.org/packages/Azure.Search.Documents/) , .net 'teki bir arama hizmetiyle çalışmak için kullanılan tüm API 'leri sağlayan bir istemci kitaplığından oluşur.

1. **Araçlar**  >  **NuGet Paket Yöneticisi**' nde **çözüm için NuGet Paketlerini Yönet...** seçeneğini belirleyin. 

1. **Gözat**’a tıklayın.

1. `Azure.Search.Documents`Sürüm 11.0.0 bulun ve seçin.

1. Derlemeyi projenize ve çözümünüze eklemek için **sağdaki aç '** a tıklayın.

### <a name="create-a-search-client"></a>Arama İstemcisi Oluşturma

1. **Program.cs**içinde, ad alanını olarak değiştirin `AzureSearch.SDK.Quickstart.v11` ve ardından aşağıdaki yönergeleri ekleyin `using` .

   ```csharp
   using Azure;
   using Azure.Search.Documents;
   using Azure.Search.Documents.Indexes;
   using Azure.Search.Documents.Indexes.Models;
   using Azure.Search.Documents.Models;
   ```

1. İki istemci oluşturun: [Searchındexclient](/dotnet/api/azure.search.documents.indexes.searchindexclient) dizini oluşturur ve [searchclient](/dotnet/api/azure.search.documents.searchclient) , var olan bir dizinle birlikte çalışabilir. Her iki hizmet uç noktası ve oluşturma/silme haklarıyla kimlik doğrulaması için bir yönetici API anahtarı gereklidir.

   ```csharp
   static void Main(string[] args)
   {
       string serviceName = "<YOUR-SERVICE-NAME>";
       string indexName = "hotels-quickstart-v11";
       string apiKey = "<YOUR-ADMIN-API-KEY>";

       // Create a SearchIndexClient to send create/delete index commands
       Uri serviceEndpoint = new Uri($"https://{serviceName}.search.windows.net/");
       AzureKeyCredential credential = new AzureKeyCredential(apiKey);
       SearchIndexClient idxclient = new SearchIndexClient(serviceEndpoint, credential);

       // Create a SearchClient to load and query documents
       SearchClient qryclient = new SearchClient(serviceEndpoint, indexName, credential);
    ```

## <a name="1---create-an-index"></a>1 - Dizin oluşturma

Bu hızlı başlangıç, otel verileri ile yükleyecek ve sorguları üzerinde çalıştıracak bir oteller dizini oluşturur. Bu adımda, dizindeki alanları tanımlayın. Her alan tanımı, alanın nasıl kullanıldığını tespit eden bir ad, veri türü ve öznitelikler içerir.

Bu örnekte, Azure.Search.Documstalar kitaplığının zaman uyumlu yöntemleri kolaylık ve okunabilirlik için kullanılır. Ancak, üretim senaryolarında uygulamanızın ölçeklenebilir ve yanıt vermesini sağlamak için zaman uyumsuz yöntemler kullanmanız gerekir. Örneğin, [CreateIndex](/dotnet/api/azure.search.documents.indexes.searchindexclient.createindex)yerine [Createındexasync](/dotnet/api/azure.search.documents.indexes.searchindexclient.createindexasync) ' i kullanacaksınız.

1. Projenize boş bir sınıf tanımı ekleyin: **Hotel.cs**

1. **Hotel.cs**' de, otel belgesi yapısını tanımlayın.

    ```csharp
    using System;
    using System.Text.Json.Serialization;

    namespace AzureSearch.SDK.Quickstart.v11
    {
        public class Hotel
        {
            [JsonPropertyName("hotelId")]
            public string Id { get; set; }

            [JsonPropertyName("hotelName")]
            public string Name { get; set; }

            [JsonPropertyName("hotelCategory")]
            public string Category { get; set; }

            [JsonPropertyName("baseRate")]
            public Int32 Rate { get; set; }

            [JsonPropertyName("lastRenovationDate")]
            public DateTime Updated { get; set; }
        }
    }
    ```

1. **Program.cs**' de, alanları ve öznitelikleri belirtin. [Searchındex](/dotnet/api/azure.search.documents.indexes.models.searchindex) ve [CreateIndex](/dotnet/api/azure.search.documents.indexes.searchindexclient.createindex) bir dizin oluşturmak için kullanılır.

   ```csharp
    // Define an index schema using SearchIndex
    // Create the index using SearchIndexClient
    SearchIndex index = new SearchIndex(indexName)
    {
        Fields =
            {
                new SimpleField("hotelId", SearchFieldDataType.String) { IsKey = true, IsFilterable = true, IsSortable = true },
                new SearchableField("hotelName") { IsFilterable = true, IsSortable = true },
                new SearchableField("hotelCategory") { IsFilterable = true, IsSortable = true },
                new SimpleField("baseRate", SearchFieldDataType.Int32) { IsFilterable = true, IsSortable = true },
                new SimpleField("lastRenovationDate", SearchFieldDataType.DateTimeOffset) { IsFilterable = true, IsSortable = true }
            }
    };

    Console.WriteLine("{0}", "Creating index...\n");
    idxclient.CreateIndex(index);
   ```

Alanındaki öznitelikler bir uygulamada nasıl kullanıldığını belirleme. Örneğin, `IsFilterable` özniteliği bir filtre ifadesini destekleyen her alana atanmalıdır.

NET SDK 'nın aranabilir dize alanlarında [aranabilir](/dotnet/api/microsoft.azure.search.models.field.issearchable) olması gereken önceki sürümlerinin aksine, alan tanımlarını kolaylaştırmak Için [searchablefield](/dotnet/api/azure.search.documents.indexes.models.searchablefield) ve [simplefield](/dotnet/api/azure.search.documents.indexes.models.simplefield) kullanabilirsiniz.

Önceki sürümlere benzer şekilde, diğer özniteliklerin de tanımda olması gerekir. Örneğin, yukarıdaki örnekte gösterildiği gibi [ısfilterable](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfilterable), [ıssıralanabilir](/dotnet/api/azure.search.documents.indexes.models.searchfield.issortable)ve [ıbıı tablosu](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfacetable) açıkça adlandırılmalıdır. 

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2-belge yükleme

Azure Bilişsel Arama, hizmette depolanan içerikleri arar. Bu adımda, az önce oluşturduğunuz otel dizinine uygun JSON belgelerini yükleyeceksiniz.

Azure Bilişsel Arama 'de belgeler, sorguların dizin oluşturma ve çıkışlara yönelik giriş olan veri yapılarıdır. Bir dış veri kaynağından elde edilen belge girişleri bir veritabanındaki satırlar, blob depolamada Bloblar veya diskteki JSON belgeleri olabilir. Bu örnekte, bir kısayol sunuyoruz ve kodun kendisinde beş otel için JSON belgelerini katıştırıyoruz. 

Belgeler karşıya yüklenirken [ındexdocumentsbatch](/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1) nesnesini kullanmanız gerekir. Indexdocumentsbatch, her biri bir belge ve Azure 'a ([karşıya yükleme, birleştirme, silme ve mergeOrUpload](search-what-is-data-import.md#indexing-actions)) bilişsel arama bir özellik Içeren bir [eylem](/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1.actions)koleksiyonu içerir.

1. **Program.cs**' de, bir dizi belge ve Dizin eylemi oluşturun ve ardından diziyi `ndexDocumentsBatch` Otel Sınıfı tarafından tanımlanan oteller-QuickStart-v11 dizinine uygun şekilde geçirin.

    ```csharp
    // Load documents (using a subset of fields for brevity)
    IndexDocumentsBatch<Hotel> batch = IndexDocumentsBatch.Create(
        IndexDocumentsAction.Upload(new Hotel { Id = "78", Name = "Upload Inn", Category = "hotel", Rate = 279, Updated = new DateTime(2018, 3, 1, 7, 0, 0) }),
        IndexDocumentsAction.Upload(new Hotel { Id = "54", Name = "Breakpoint by the Sea", Category = "motel", Rate = 162, Updated = new DateTime(2015, 9, 12, 7, 0, 0) }),
        IndexDocumentsAction.Upload(new Hotel { Id = "39", Name = "Debug Motel", Category = "motel", Rate = 159, Updated = new DateTime(2016, 11, 11, 7, 0, 0) }),
        IndexDocumentsAction.Upload(new Hotel { Id = "48", Name = "NuGet Hotel", Category = "hotel", Rate = 238, Updated = new DateTime(2016, 5, 30, 7, 0, 0) }),
        IndexDocumentsAction.Upload(new Hotel { Id = "12", Name = "Renovated Ranch", Category = "motel", Rate = 149, Updated = new DateTime(2020, 1, 24, 7, 0, 0) }));

    IndexDocumentsOptions idxoptions = new IndexDocumentsOptions { ThrowOnAnyError = true };

    Console.WriteLine("{0}", "Loading index...\n");
    qryclient.IndexDocuments(batch, idxoptions);
    ```

    [Indexdocumentsbatch](/dotnet/api/azure.search.documents.models.indexdocumentsbatch-1) nesnesini başlattığınızda, [searchclient](/dotnet/api/azure.search.documents.searchclient) nesneniz üzerindeki [ındexdocuments](/dotnet/api/azure.search.documents.searchclient.indexdocuments) ' ı çağırarak dizine gönderebilirsiniz.

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

1. **Program.cs**içinde, arama sonuçlarını konsola yazdıran bir writedocuments yöntemi oluşturun.

    ```csharp
    private static void WriteDocuments(SearchResults<Hotel> searchResults)
    {
        foreach (SearchResult<Hotel> response in searchResults.GetResults())
        {
            Hotel doc = response.Document;
            var score = response.Score;
            Console.WriteLine($"Name: {doc.Name}, Type: {doc.Category}, Rate: {doc.Rate}, Last-update: {doc.Updated}, Score: {score}");
        }

        Console.WriteLine();
    }
    ```

1. Sorguları yürütmek ve sonuçları döndürmek için bir RunQueries yöntemi oluşturun. Sonuçlar otel nesneleridir.

    ```csharp
    private static void RunQueries(SearchClient qryclient)
    {
        SearchOptions options;
        SearchResults<Hotel> response;

        Console.WriteLine("Query #1: Search on the term 'motel' and list the relevance score for each match...\n");

        options = new SearchOptions()
        {
            Filter = "",
            OrderBy = { "" }
        };

        response = qryclient.Search<Hotel>("motel", options);
        WriteDocuments(response);

        Console.WriteLine("Query #2: Find hotels where 'type' equals hotel...\n");

        options = new SearchOptions()
        {
            Filter = "hotelCategory eq 'hotel'",
        };

        response = qryclient.Search<Hotel>("*", options);
        WriteDocuments(response);

        Console.WriteLine("Query #3: Filter on rates less than $200 and sort by when the hotel was last updated...\n");

        options = new SearchOptions()
        {
            Filter = "baseRate lt 200",
            OrderBy = { "lastRenovationDate desc" }
        };

        response = qryclient.Search<Hotel>("*", options);
        WriteDocuments(response);
    }
    ```

Bu örnek, [bir sorgudaki koşulların iki yolunu](search-query-overview.md#types-of-queries)gösterir: tam metin araması ve filtreler:

+ Dizininizdeki aranabilir alanlarda bir veya daha fazla terim için tam metin arama sorguları. İlk sorgu tam metin aramadır. Tam metin arama, sonuçları derecelendirmek için kullanılan uygunluk puanlarını üretir.

+ Filtre, bir dizindeki [ısfilterable](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfilterable) alanları üzerinde değerlendirilen bir Boole deyimidir. Filtre sorguları, değerleri dahil etme veya hariç tutma. Bu nedenle, bir filtre sorgusuyla ilişkili bir ilgi puanı yoktur. Son iki sorgu filtre aramasını gösterir.

Tam metin arama ve filtreleri birlikte veya ayrı olarak kullanabilirsiniz.

Arama ve filtrelerin her ikisi de [Searchclient. Search](/dotnet/api/azure.search.documents.searchclient.search) yöntemi kullanılarak gerçekleştirilir. Bir arama sorgusu, bir `searchText` filtre ifadesi, [SearchOptions](/dotnet/api/azure.search.documents.searchoptions) sınıfının [Filter](/dotnet/api/azure.search.documents.searchoptions.filter) özelliğine geçirilekarşın, dizeye geçirilebilir. Aramasız filtrelemek için, `"*"` `searchText` [arama](/dotnet/api/azure.search.documents.searchclient.search) yönteminin parametresi için geçirin. Filtrelemeden aramak için, `Filter` özelliği unset olarak bırakın veya bir örneği geçirmeyin `SearchOptions` .

## <a name="run-the-program"></a>Programı çalıştırma

Uygulamayı yeniden derlemek ve programı tamamen çalıştırmak için F5 tuşuna basın. 

Çıktı, sorgu bilgilerinin ve sonuçların eklenmesiyle birlikte [Console. WriteLIne](/dotnet/api/system.console.writeline)içindeki iletileri içerir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu C# hızlı başlangıçta, bir dizin oluşturmak, belgeler ile yüklemek ve sorguları çalıştırmak için bir dizi görev üzerinden çalıştık. Farklı aşamalarda, okunabilirliği ve kavrama için kodu basitleştirecek kısayollar sunuyoruz. Temel kavramlara rahat bir şekilde yararlandıysanız, bilginizi borçlandırmaya yönelik alternatif yaklaşımların ve kavramların araştırmasını sağlayan bir sonraki makaleye önerilir. 

> [!div class="nextstepaction"]
> [.NET ' te geliştirme](search-howto-dotnet-sdk.md)

Bulut harcamalarınızı iyileştirmek ve kaydetmek istiyor musunuz?

> [!div class="nextstepaction"]
> [Maliyet yönetimi ile maliyetleri çözümlemeye başlayın](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)