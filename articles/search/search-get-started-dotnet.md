---
title: "Hızlı başlatma: .NET kullanarak C#'da arama dizini oluşturma"
titleSuffix: Azure Cognitive Search
description: Bu C# hızlı başlangıcında, Azure Bilişsel Arama .NET SDK'yı kullanarak bir dizin oluşturmayı, veri yüklemeyi ve sorguları çalıştırmayı öğrenin.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 02/10/2020
ms.openlocfilehash: 3d0006a3c77050c1bb21a0da8d6be51e659f933d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77589224"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-in-c-using-the-net-sdk"></a>Hızlı Başlangıç: .NET SDK'yı kullanarak C#'da Azure Bilişsel Arama dizini oluşturma
> [!div class="op_single_selector"]
> * [C #](search-get-started-dotnet.md)
> * [Portal](search-get-started-portal.md)
> * [Powershell](search-create-index-rest-api.md)
> * [Python](search-get-started-python.md)
> * [Postman](search-get-started-postman.md)
>*

Visual Studio ve [Azure Bilişsel Arama .NET SDK'yı](https://aka.ms/search-sdk)kullanarak bir Azure Bilişsel Arama dizini oluşturan, yükleyen ve sorgulayan bir .NET Core C# konsolu uygulaması oluşturun. Bu makalede, uygulama adım adım nasıl oluşturulacak açıklanmaktadır. Alternatif olarak, [tam uygulamayı indirebilir ve çalıştırabilirsiniz.](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/Quickstart)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

> [!NOTE]
> Bu makaledeki demo kodu, basitlik için Azure Bilişsel Arama .NET SDK'nın eşzamanlı yöntemlerini kullanır. Ancak, üretim senaryoları için, ölçeklenebilir ve duyarlı tutmak için kendi uygulamalarınızda eşzamanlı yöntemler kullanmanızı öneririz. Örneğin, kullanabilirsiniz `CreateAsync` ve `DeleteAsync` yerine `Create` `Delete`ve .

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç için aşağıdaki hizmetler ve araçlar gereklidir.

+ [Visual Studio](https://visualstudio.microsoft.com/downloads/), herhangi bir baskı. Örnek kod ve talimatlar ücretsiz Topluluk sürümünde test edildi.

+ [Bir Azure Bilişsel Arama hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğiniz altında [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

<a name="get-service-info"></a>

## <a name="get-a-key-and-url"></a>Bir anahtar ve URL alın

Hizmete yapılan aramalar için her istekte bir URL bitiş noktası ve erişim anahtarı gerekir. Her ikisiyle de bir arama hizmeti oluşturulur, bu nedenle aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

2. **Ayarlar** > **Tuşları'nda,** hizmetteki tüm haklar için bir yönetici anahtarı alın. İki değiştirilebilir yönetici anahtarları, bir üzerinde rulo gerekir durumda iş sürekliliği için sağlanan vardır. Nesneleri ekleme, değiştirme ve silme isteklerinde birincil veya ikincil anahtarı kullanabilirsiniz.

   Sorgu anahtarını da alın. Salt okunur erişimle sorgu isteklerini vermek en iyi yöntemdir.

![Http bitiş noktası ve erişim anahtarı alın](media/search-get-started-postman/get-url-key.png "Http bitiş noktası ve erişim anahtarı alın")

Tüm istekler, hizmetinize gönderilen her istekiçin bir api anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

Visual Studio'yı açarak ve .NET Core'da çalıştırılabilen yeni bir Konsol Uygulaması projesi oluşturarak başlayın.

### <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

[Azure Bilişsel Arama .NET SDK,](https://aka.ms/search-sdk) NuGet paketleri olarak dağıtılan birkaç istemci kitaplığından oluşur.

Bu proje `Microsoft.Azure.Search` için, NuGet paketinin 9 `Microsoft.Extensions.Configuration.Json` sürümünü ve en son NuGet paketini kullanın.

1. **Araçlar** > **NuGet Paket Yöneticisi'nde,** Çözüm için **NuGet Paketlerini Yönet'i seçin...** 

1. **Gözat**’a tıklayın.

1. 9.0.1 veya sonraki sürümleri arayın `Microsoft.Azure.Search` ve seçin.

1. Kurulumunu projenize ve çözümünüze eklemek için sağtarafta **Yükle'yi** tıklatın.

1. Sürüm `Microsoft.Extensions.Configuration.Json`2.2.0 veya sonraki sürüm seçerek için tekrarlayın.


### <a name="add-azure-cognitive-search-service-information"></a>Azure Bilişsel Arama hizmeti bilgileri ekleme

1. Çözüm Gezgini'nde projeye sağ tıklayın ve > **Yeni Öğe Ekle'yi seçin...** seçeneğini belirleyin. **Add** 

1. Yeni Öğe Ekle'de, JSON ile ilgili madde türleri listesini döndürmek için "JSON"u arayın.

1. **JSON Dosyası'nı**seçin, dosyayı "appsettings.json" olarak adlandırın ve **Ekle'yi**tıklatın. 

1. Dosyayı çıktı dizininize ekleyin. Appsettings.json'a sağ tıklayın ve **Özellikler'i**seçin. **Çıktı Dizini Kopyala'da,** **daha yeniyse Kopyala'yı**seçin.

1. Aşağıdaki JSON dosyanızı yeni JSON dosyanıza kopyalayın. Arama hizmeti adını (Sİz-ARAMA-HİzMET-NAME) ve yönetici API anahtarını (YOUR-ADMIN-API-KEY) geçerli değerlerle değiştirin. Hizmet bitiş noktanız `https://mydemo.search.windows.net`ise, hizmet adı "mydemo" olacaktır.

```json
{
  "SearchServiceName": "<YOUR-SEARCH-SERVICE-NAME>",
  "SearchServiceAdminApiKey": "<YOUR-ADMIN-API-KEY>",
  "SearchIndexName": "hotels-quickstart"
}
```

### <a name="add-class-method-files-to-your-project"></a>Sınıf ekle ". Yöntem" dosyaları projenize

Sonuçları konsol penceresine yazdırırken, Otel nesnesinden tek tek alanlar dizeleri olarak döndürülmelidir. Gerekli kodu iki yeni dosyaya kopyalayarak bu görevi gerçekleştirmek için [ToString()](https://docs.microsoft.com/dotnet/api/system.object.tostring?view=netframework-4.8) uygulayabilirsiniz.

1. Projenize iki boş sınıf tanımı ekleyin: Address.Methods.cs Hotel.Methods.cs

1. Address.Methods.cs olarak, varsayılan içeriğin üzerine aşağıdaki kod, [satır 1-32](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/Quickstart/AzureSearchQuickstart/Address.Methods.cs/#L1-L32)ile yazın.

1. Hotel.Methods.cs, kopya [satırları 1-66](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/Quickstart/AzureSearchQuickstart/Hotel.Methods.cs/#L1-L66).


## <a name="1---create-index"></a>1 - Dizin oluşturma

Otel endeksi, basit bir alanın "HotelName" veya "Açıklama" olduğu basit ve karmaşık alanlardan oluşur ve karmaşık alanlar alt alanları veya oda koleksiyonu içeren bir adrestir. Bir dizin karmaşık türleri içeriyorsa, karmaşık alan tanımlarını ayrı sınıflarda yalıtın.

1. Projenize iki boş sınıf tanımı ekleyin: Address.cs Hotel.cs

1. Address.cs, varsayılan içeriği aşağıdaki kodla birlikte üzerine yazın:

    ```csharp
    using System;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Newtonsoft.Json;

    namespace AzureSearchQuickstart
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

1. Hotel.cs olarak, sınıf adres sınıfına başvurular da dahil olmak üzere dizinin genel yapısını tanımlar.

    ```csharp
    namespace AzureSearchQuickstart
    {
        using System;
        using Microsoft.Azure.Search;
        using Microsoft.Azure.Search.Models;
        using Newtonsoft.Json;

        public partial class Hotel
        {
            [System.ComponentModel.DataAnnotations.Key]
            [IsFilterable]
            public string HotelId { get; set; }

            [IsSearchable, IsSortable]
            public string HotelName { get; set; }

            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.EnMicrosoft)]
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

            [IsFilterable, IsSortable, IsFacetable]
            public DateTimeOffset? LastRenovationDate { get; set; }

            [IsFilterable, IsSortable, IsFacetable]
            public double? Rating { get; set; }

            public Address Address { get; set; }
        }
    }
    ```

    Alandaki öznitelikler, uygulamada nasıl kullanılacağını belirler. Örneğin, `IsSearchable` öznitelik tam metin arama dahil edilmesi gereken her alana atanmalıdır. 
    
    > [!NOTE]
    > .NET SDK'da, alanlar açıkça [`IsSearchable`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field.issearchable?view=azure-dotnet)" [`IsFilterable`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field.isfilterable?view=azure-dotnet)ve [`IsSortable`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field.issortable?view=azure-dotnet) [`IsFacetable`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field.isfacetable?view=azure-dotnet). Bu davranış, dolaylı olarak veri türüne dayalı ilişkilendirme sağlayan REST API'si ile zıttır (örneğin, basit dize alanları otomatik olarak aranabilir).

    Türüdizinizdeki `string` tam bir alan, her belgeyi benzersiz bir şekilde tanımlayan *anahtar* alan olmalıdır. Bu şemada, anahtar `HotelId`.

    Bu dizinde, açıklama alanları, [`analyzer`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field.analyzer?view=azure-dotnet) varsayılan standart Lucene çözümleyicisini geçersiz kılmak istediğinizde belirtilen isteğe bağlı özelliği kullanır. Alan `description_fr` Fransızca metin depolar çünkü Fransız Lucene çözümleyici[(FrLucene)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername.frlucene?view=azure-dotnet)kullanıyor. İsteğe `description` bağlı Microsoft dil çözümleyicisi[(EnMicrosoft)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername.enmicrosoft?view=azure-dotnet)kullanıyor.

1. Program.cs, uygulamanın [`SearchServiceClient`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient?view=azure-dotnet) config dosyasında (appsettings.json) depolanan değerleri kullanarak hizmete bağlanmak için sınıfın bir örneğini oluşturun. 

   `SearchServiceClient`Oluşturmak, [`Indexes`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.indexes?view=azure-dotnet) listelemek, güncelleştirmek veya Azure Bilişsel Arama dizinlerini silmek için gereken tüm yöntemleri sağlayan bir özelliğe sahiptir. 

    ```csharp
    using System;
    using System.Linq;
    using System.Threading;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Microsoft.Extensions.Configuration;

    namespace AzureSearchQuickstart
    {
        class Program {
            // Demonstrates index delete, create, load, and query
            // Commented-out code is uncommented in later steps
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

                // Uncomment next 3 lines in "2 - Load documents"
                // ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);
                // Console.WriteLine("{0}", "Uploading documents...\n");
                // UploadDocuments(indexClient);

                // Uncomment next 2 lines in "3 - Search an index"
                // Console.WriteLine("{0}", "Searching index...\n");
                // RunQueries(indexClient);

                Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
                Console.ReadKey();
            }

            // Create the search service client
            private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
            {
                string searchServiceName = configuration["SearchServiceName"];
                string adminApiKey = configuration["SearchServiceAdminApiKey"];

                SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
                return serviceClient;
            }

            // Delete an existing index to reuse its name
            private static void DeleteIndexIfExists(string indexName, SearchServiceClient serviceClient)
            {
                if (serviceClient.Indexes.Exists(indexName))
                {
                    serviceClient.Indexes.Delete(indexName);
                }
            }

            // Create an index whose fields correspond to the properties of the Hotel class.
            // The Address property of Hotel will be modeled as a complex field.
            // The properties of the Address class in turn correspond to sub-fields of the Address complex field.
            // The fields of the index are defined by calling the FieldBuilder.BuildForType() method.
            private static void CreateIndex(string indexName, SearchServiceClient serviceClient)
            {
                var definition = new Index()
                {
                    Name = indexName,
                    Fields = FieldBuilder.BuildForType<Hotel>()
                };

                serviceClient.Indexes.Create(definition);
            }
        }
    }    
    ```

    Mümkünse, çok fazla `SearchServiceClient` bağlantı açılmasını önlemek için uygulamanızda tek bir örnek paylaşın. Sınıf yöntemleri, bu tür paylaşımı etkinleştirmek için iş parçacığı güvenlidir.

   Sınıfın birkaç oluşturucusu vardır. İstediğiniz oluşturucu, arama hizmeti adınızı ve `SearchCredentials` nesnesini parametre olarak alır. `SearchCredentials`, api anahtarınızı sarmalar.

    Dizin tanımında, nesneleri oluşturmanın `Field` en kolay yolu `FieldBuilder.BuildForType` yöntemi çağırmak, tür parametresi için bir model sınıfı geçmektir. Bir model sınıfında dizininizin alanlarıyla eşlenen özellikler mevcuttur. Bu eşleme, arama dizininizdeki belgeleri model sınıfınızın örneklerine bağlamanızı sağlar.

    > [!NOTE]
    > Bir model sınıfı kullanmayı planlamıyorsanız, `Field` nesnelerini doğrudan oluşturarak dizininizi tanımlayabilirsiniz. Oluşturucuya alan adını veri türü (veya dize alanları çözümleyicisi) ile birlikte sağlayabilirsiniz. Birkaç ad vermek için `IsSearchable` `IsFilterable`, , gibi diğer özellikleri de ayarlayabilirsiniz.
    >

1. Uygulamayı oluşturmak ve dizini oluşturmak için F5 tuşuna basın. 

    Proje başarıyla oluşturulursa, dizin siler ve oluşturmak için ekrana durum iletileri yazarak bir konsol penceresi açılır. 

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - Belgeleri yükleyin

Azure Bilişsel Arama'da belgeler, hem dizin oluşturma hem de sorgulardan çıktı girişi olan veri yapılarıdır. Harici bir veri kaynağından elde edilen belge girişleri veritabanındaki satırlar, Blob depolamasındaki blob'lar veya diskteki JSON belgeleri olabilir. Bu örnekte, bir kısayol alıyor ve kodun kendisine dört otel için JSON belgeleri katıştırıyoruz. 

Belgeleri yüklerken bir [`IndexBatch`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) nesne kullanmanız gerekir. Her `IndexBatch` biri Azure [`IndexAction`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction?view=azure-dotnet) Bilişsel Arama'ya hangi eylemin gerçekleştirilip[(yükleme, birleştirme, silme ve birleştirmeOrUpload)](search-what-is-data-import.md#indexing-actions)ne anlama gelir bildiren bir belge ve özellik içeren bir nesne koleksiyonu içerir.

1. Program.cs, bir dizi belge ve dizin eylemi oluşturun `IndexBatch`ve ardından diziyi .'a geçirin. Aşağıdaki belgeler, otel ve adres sınıfları tarafından tanımlandığı gibi otel hızlı başlangıç endeksine uygundur.

    ```csharp
    // Upload documents as a batch
    private static void UploadDocuments(ISearchIndexClient indexClient)
    {
        var actions = new IndexAction<Hotel>[]
        {
            IndexAction.Upload(
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
                }
            ),
            IndexAction.Upload(
                new Hotel()
                {
                    HotelId = "2",
                    HotelName = "Twin Dome Motel",
                    Description = "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
                    DescriptionFr = "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
                    Category = "Boutique",
                    Tags = new[] { "pool", "free wifi", "concierge" },
                    ParkingIncluded = false,
                    LastRenovationDate =  new DateTimeOffset(1979, 2, 18, 0, 0, 0, TimeSpan.Zero),
                    Rating = 3.60,
                    Address = new Address()
                    {
                        StreetAddress = "140 University Town Center Dr",
                        City = "Sarasota",
                        StateProvince = "FL",
                        PostalCode = "34243",
                        Country = "USA"
                    }
                }
            ),
            IndexAction.Upload(
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
                }
            ),
            IndexAction.Upload(
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
                    Rating = 4.6,
                    Address = new Address()
                    {
                        StreetAddress = "7400 San Pedro Ave",
                        City = "San Antonio",
                        StateProvince = "TX",
                        PostalCode = "78216",
                        Country = "USA"
                    }
                }
            ),
        };

        var batch = IndexBatch.New(actions);

        try
        {
            indexClient.Documents.Index(batch);
        }
        catch (IndexBatchException e)
        {
            // When a service is under load, indexing might fail for some documents in the batch. 
            // Depending on your application, you can compensate by delaying and retrying. 
            // For this simple demo, we just log the failed document keys and continue.
            Console.WriteLine(
                "Failed to index some of the documents: {0}",
                String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
        }

        // Wait 2 seconds before starting queries 
        Console.WriteLine("Waiting for indexing...\n");
        Thread.Sleep(2000);
    }
    ```

    Nesneyi baş`IndexBatch` harfe gönderdikten sonra, nesnenizi [`Documents.Index`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.index?view=azure-dotnet) [`SearchIndexClient`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) çağırarak dizin gönderebilirsiniz. `Documents`dizininizde `SearchIndexClient` belge ekleme, değiştirme, silme veya sorgulama yöntemleri sağlayan bir özelliktir.

    Çağrıyı `Index` çevreleyen yöntem, hizmetiniz ağır yük altındaysa meydana gelebilecek dizin oluşturma hatalarını yakalar. `try` / `catch` Üretim kodunda, başarısız olan belgeleri dizine ekleyerek geciktirebilir ve sonra yeniden deneyebilir veya örnek gibi günlüğe kaydedebilir ve devam edebilir veya uygulamanızın veri tutarlılığı gereksinimlerini karşılayan başka bir şekilde işleyebilirsiniz.

    2 saniyelik gecikme, sorgular yürütülmeden önce tüm belgelerin dizine ekilen, eşzamanlı olan dizin oluşturmayı telafi eder. Gecikmede kodlama genellikle yalnızca demolarda, testlerde ve örnek uygulamalarda gereklidir.

1. Program.cs, ana olarak, "2 - Load belgeleri" için satırları yorumsuz. 

    ```csharp
    // Uncomment next 3 lines in "2 - Load documents"
    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);
    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);
    ```
1. Uygulamayı yeniden oluşturmak için F5 tuşuna basın. 

    Proje başarıyla inşa edilirse, bir konsol penceresi açılır, durum iletileri yazma, bu kez belgeleri yükleme hakkında bir mesaj ile. Azure portalında, arama **hizmetine Genel Bakış** sayfasında, otellerin hızlı başlangıç dizininde artık 4 belge olmalıdır.

Belge işleme hakkında daha fazla bilgi için [".NET SDK belgeleri nasıl işler" bilgisine](search-howto-dotnet-sdk.md#how-dotnet-handles-documents)bakın.

## <a name="3---search-an-index"></a>3 - Dizin arama

İlk belge dizine girer girmez sorgu sonuçları alabilirsiniz, ancak dizininizin gerçek testi tüm belgelerin dizine ekinine kadar beklemelidir. 

Bu bölümde iki işlevsellik parçası ekler: sorgu mantığı ve sonuçlar. Sorgular için [`Search`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.search?view=azure-dotnet
) yöntemi kullanın. Bu yöntem, arama metninin yanı sıra diğer [parametreleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters?view=azure-dotnet)de alır. 

Sınıf [`DocumentsSearchResult`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult-1?view=azure-dotnet) sonuçları temsil eder.


1. Program.cs, arama sonuçlarını konsola yazdıran bir WriteDocuments yöntemi oluşturun.

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

1. Sorguları yürütmek ve sonuçları döndürmek için bir RunQueries yöntemi oluşturun. Sonuçlar Otel nesneleridir. Alanları tek tek yüzeye çıkarmak için seçili parametreyi kullanabilirsiniz. Bir alan seçilen parametreye dahil değilse, ilgili Otel mülkü geçersiz olacaktır.

    ```csharp
    private static void RunQueries(ISearchIndexClient indexClient)
    {
        SearchParameters parameters;
        DocumentSearchResult<Hotel> results;

        // Query 1 
        Console.WriteLine("Query 1: Search for term 'Atlanta' with no result trimming");
        parameters = new SearchParameters();
        results = indexClient.Documents.Search<Hotel>("Atlanta", parameters);
        WriteDocuments(results);

        // Query 2
        Console.WriteLine("Query 2: Search on the term 'Atlanta', with trimming");
        Console.WriteLine("Returning only these fields: HotelName, Tags, Address:\n");
        parameters =
            new SearchParameters()
            {
                Select = new[] { "HotelName", "Tags", "Address" },
            };
        results = indexClient.Documents.Search<Hotel>("Atlanta", parameters);
        WriteDocuments(results);

        // Query 3
        Console.WriteLine("Query 3: Search for the terms 'restaurant' and 'wifi'");
        Console.WriteLine("Return only these fields: HotelName, Description, and Tags:\n");
        parameters =
            new SearchParameters()
            {
                Select = new[] { "HotelName", "Description", "Tags" }
            };
        results = indexClient.Documents.Search<Hotel>("restaurant, wifi", parameters);
        WriteDocuments(results);

        // Query 4 -filtered query
        Console.WriteLine("Query 4: Filter on ratings greater than 4");
        Console.WriteLine("Returning only these fields: HotelName, Rating:\n");
        parameters =
            new SearchParameters()
            {
                Filter = "Rating gt 4",
                Select = new[] { "HotelName", "Rating" }
            };
        results = indexClient.Documents.Search<Hotel>("*", parameters);
        WriteDocuments(results);

        // Query 5 - top 2 results
        Console.WriteLine("Query 5: Search on term 'boutique'");
        Console.WriteLine("Sort by rating in descending order, taking the top two results");
        Console.WriteLine("Returning only these fields: HotelId, HotelName, Category, Rating:\n");
        parameters =
            new SearchParameters()
            {
                OrderBy = new[] { "Rating desc" },
                Select = new[] { "HotelId", "HotelName", "Category", "Rating" },
                Top = 2
            };
        results = indexClient.Documents.Search<Hotel>("boutique", parameters);
        WriteDocuments(results);
    }
    ```

    [Bir sorguda terimleri eşleştirmenin](search-query-overview.md#types-of-queries)iki yolu vardır: tam metin arama ve filtreler. Tam metin arama sorgusu, dizininizdeki `IsSearchable` alanlarda bir veya daha fazla terim arar. Filtre, dizindeki alanlar üzerinde `IsFilterable` değerlendirilen bir boolean ifadesidir. Tam metin arama ve filtreleri birlikte veya ayrı ayrı kullanabilirsiniz.

    Arama ve filtrelerin her ikisi de `Documents.Search` yöntemi kullanılarak gerçekleştirilir. Bir arama sorgusu `searchText` parametresinde geçirilebilirken bir filtre ifadesi `SearchParameters` sınıfının `Filter` özelliğinden geçirilebilir. Arama yapmadan filtrelemek üzere `searchText` parametresi için `"*"` geçirmeniz yeterlidir. Filtrelemeden arama yapmak için `Filter` özelliğini ayarlamadan bırakmanız veya bir `SearchParameters` örneği geçirmemeniz yeterlidir.

1. Program.cs, ana olarak, "3 - Arama" için satırları yorumun. 

    ```csharp
    // Uncomment next 2 lines in "3 - Search an index"
    Console.WriteLine("{0}", "Searching documents...\n");
    RunQueries(indexClient);
    ```
1. Çözüm artık bitti. Uygulamayı yeniden oluşturmak ve programı bütünüyle çalıştırmak için F5 tuşuna basın. 

    Çıktı, sorgu bilgileri ve sonuçların eklenmesiyle öncekiyle aynı iletileri içerir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki **Tüm kaynaklar** veya **Kaynak grupları** bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin ve veri kaynağıyla sınırlı olduğunuzu unutmayın. Sınırın altında kalmak için portaldaki tek tek öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu C# hızlı başlangıcında, bir dizin oluşturmak, belgelerle yüklemek ve sorguları çalıştırmak için bir dizi görev üzerinde çalıştınız. Farklı aşamalarda, okunabilirlik ve anlama kodunu basitleştirmek için kısayollar aldık. Eğer temel kavramlar ile rahat iseniz, biz bilginizi derinleştirecek alternatif yaklaşımlar ve kavramlar bir keşif için bir sonraki makale öneririz. 

Örnek kod ve dizin bu genişletilmiş sürümleri. Sonraki örnek bir Oda koleksiyonu ekler, farklı sınıflar ve eylemler kullanır ve işlemenin nasıl çalıştığına daha yakından bakar.

> [!div class="nextstepaction"]
> [.NET'te nasıl geliştirilir?](search-howto-dotnet-sdk.md)
