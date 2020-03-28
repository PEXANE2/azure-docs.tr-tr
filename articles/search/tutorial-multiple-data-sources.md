---
title: 'C# Tutorial: Birden çok veri kaynağını dizine dizinle'
titleSuffix: Azure Cognitive Search
description: Dizin leyicileri kullanarak birden çok veri kaynağından verileri tek bir Azure Bilişsel Arama dizinine nasıl aktarılacın öğrenin. Bu öğretici ve örnek kod C# bulunmaktadır.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/28/2020
ms.openlocfilehash: 8e75d9de45c64813ac75de635371d2435fb9261f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78271476"
---
# <a name="tutorial-index-data-from-multiple-data-sources-in-c"></a>Öğretici: C'deki birden fazla veri kaynağından gelen verileri dizin #

Azure Bilişsel Arama, birden çok veri kaynağından gelen verileri tek bir leştirilmiş arama dizinine aktarabilir, analiz edebilir ve dizine ekleyebilir. Bu, yapılandırılmış verilerin metin, HTML veya JSON belgeleri gibi diğer kaynaklardan gelen daha az yapılandırılmış ve hatta düz metin verileriyle toplandığı durumları destekler.

Bu öğretici, bir Azure Cosmos DB veri kaynağından otel verilerini nasıl dizinlendirilen ve azure Blob Depolama belgelerinden alınan otel odası ayrıntılarıyla nasıl birleştiriliz açıklanır. Sonuç, karmaşık veri türlerini içeren birleşik bir otel arama dizini olacaktır.

Bu öğretici C# ve [.NET SDK](https://aka.ms/search-sdk)kullanır. Bu öğreticide, aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Örnek verileri yükleme ve veri kaynakları oluşturma
> * Belge anahtarını tanımlama
> * Dizin tanımlama ve oluşturma
> * Azure Cosmos DB'den otel verilerini dizinle
> * Blob depolamaotel odası verilerini birleştirme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

+ [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal)
+ [Azure Depolama](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
+ [Visual Studio 2019](https://visualstudio.microsoft.com/)
+ [Varolan bir arama hizmeti](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) [oluşturma](search-create-service-portal.md) veya bulma 

> [!Note]
> Bu öğretici için ücretsiz hizmeti kullanabilirsiniz. Ücretsiz bir arama hizmeti sizi üç dizin, üç dizin leyici ve üç veri kaynağıyla sınırlar. Bu öğreticide hepsinden birer tane oluşturulur. Başlamadan önce, yeni kaynakları kabul etmek için hizmetinizde yer olduğundan emin olun.

## <a name="download-files"></a>Dosyaları indirme

Bu öğreticinin kaynak kodu, [birden çok veri kaynağı](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/multiple-data-sources) klasöründe, [azure-search-dotnet-örnekleri](https://github.com/Azure-Samples/azure-search-dotnet-samples) GitHub deposundadır.

## <a name="1---create-services"></a>1 - Hizmet oluşturma

Bu öğretici, dizin oluşturma ve sorgular için Azure Bilişsel Arama, bir veri kümesi için Azure Cosmos DB ve ikinci veri kümesi için Azure Blob depolamasını kullanır. 

Mümkünse, yakınlık ve yönetilebilirlik için aynı bölgedeki ve kaynak grubundaki tüm hizmetleri oluşturun. Uygulamada, hizmetleriniz herhangi bir bölgede olabilir.

Bu örnek, yedi kurgusal oteli tanımlayan iki küçük veri kümesi kullanır. Bir set otellerin kendilerini tanımlar ve Azure Cosmos DB veritabanına yüklenir. Diğer set otel odası ayrıntılarını içerir ve Azure Blob Depolama'ya yüklenecek yedi ayrı JSON dosyası olarak sağlanır.

### <a name="start-with-cosmos-db"></a>Cosmos DB ile başlayın

1. [Azure portalında](https://portal.azure.com)oturum açın ve ardından Azure Cosmos DB hesabına Genel Bakış sayfanızda gezinin.

1. **Veri Gezgini'ni** seçin ve ardından **Yeni Veritabanı'nı**seçin.

   ![Yeni veritabanı oluşturma](media/tutorial-multiple-data-sources/cosmos-newdb.png "Yeni veritabanı oluşturma")

1. **Adını otel-odalar-db**girin. Kalan ayarlar için varsayılan değerleri kabul edin.

   ![Veritabanını yapılandırma](media/tutorial-multiple-data-sources/cosmos-dbname.png "Veritabanını yapılandırma")

1. Yeni bir kapsayıcı oluşturun. Oluşturduğunuz varolan veritabanını kullanın. Konteyner adı için **otellergirin** ve Bölüm anahtarı için **/HotelId'yi** kullanın.

   ![Kapsayıcı ekleme](media/tutorial-multiple-data-sources/cosmos-add-container.png "Kapsayıcı ekleme")

1. **Otellerin**altındaki **Öğeler'i** seçin ve ardından komut çubuğundaki **Öğeyi Yükle'yi** tıklatın. Proje klasöründe **cosmosdb/HotelsDataSubset_CosmosDb.json** dosyasına gidin ve ardından seçin.

   ![Azure Cosmos DB koleksiyonuna yükleme](media/tutorial-multiple-data-sources/cosmos-upload.png "Cosmos DB koleksiyonuna yükleyin")

1. Otel koleksiyonundaki öğelerin görünümünü yenilemek için Yenile düğmesini kullanın. Listelenen yedi yeni veritabanı belgesini görmelisiniz.

### <a name="azure-blob-storage"></a>Azure Blob depolama

1. [Azure portalında](https://portal.azure.com)oturum açın, Azure depolama hesabınıza gidin, **Blobs'ı**tıklatın ve sonra **+ Kapsayıcı'yı**tıklatın.

1. Örnek **otel odası** JSON dosyalarını saklamak için otel odaları adlı bir [blob konteyner oluşturun.](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) Genel Erişim Düzeyini geçerli değerlerinden herhangi biri için ayarlayabilirsiniz.

   ![Blob kapsayıcısı oluşturma](media/tutorial-multiple-data-sources/blob-add-container.png "Blob kapsayıcısı oluşturma")

1. Kapsayıcı oluşturulduktan sonra açın ve komut çubuğunda **Yükle'yi** seçin. Örnek dosyaları içeren klasöre gidin. Hepsini seçin ve sonra **Yükle'yi**tıklatın.

   ![Dosyaları karşıya yükleme](media/tutorial-multiple-data-sources/blob-upload.png "Dosyaları karşıya yükleme")

Yükleme tamamlandıktan sonra, dosyalar veri kapsayıcısı listesinde görünmelidir.

### <a name="azure-cognitive-search"></a>Azure Bilişsel Arama

Üçüncü bileşen, [portalda oluşturabileceğiniz](search-create-service-portal.md)Azure Bilişsel Arama'dır. Bu izbiyi tamamlamak için Ücretsiz katmanı kullanabilirsiniz. 

### <a name="get-an-admin-api-key-and-url-for-azure-cognitive-search"></a>Azure Bilişsel Arama için yönetici api anahtarı ve URL'si alın

Azure Bilişsel Arama hizmetinizle etkileşimde kalmak için hizmet URL'sine ve erişim anahtarına ihtiyacınız olacaktır. Her ikisiyle de bir arama hizmeti oluşturulur, bu nedenle aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **Tuşları'nda,** hizmetteki tüm haklar için bir yönetici anahtarı alın. İki değiştirilebilir yönetici anahtarları, bir üzerinde rulo gerekir durumda iş sürekliliği için sağlanan vardır. Nesneleri ekleme, değiştirme ve silme isteklerinde birincil veya ikincil anahtarı kullanabilirsiniz.

   Sorgu anahtarını da alın. Salt okunur erişimle sorgu isteklerini vermek en iyi yöntemdir.

   ![Hizmet adını ve yöneticive sorgu anahtarlarını alın](media/search-get-started-nodejs/service-name-and-keys.png)

İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="2---set-up-your-environment"></a>2 - Ortamınızı ayarlayın

1. Visual Studio 2019'u başlatın ve **Araçlar** menüsünde **NuGet Paket Yöneticisi'ni** seçin ve **ardından Çözüm Için NuGet Paketlerini Yönetin...** 

1. **Gözat** **sekmesinde, Microsoft.Azure.Search'ü** (sürüm 9.0.1 veya daha sonra) bulun ve yükleyin. Yüklemeyi tamamlamak için ek iletişim günlüklerini tıklatmanız gerekir.

    ![Azure kitaplıkları eklemek için NuGet'i kullanma](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

1. **Microsoft.Extensions.Configuration.Json** NuGet paketini arayın ve yükleyin.

1. **AzureSearchMultipleDataSources.sln**çözüm dosyasını açın.

1. Solution Explorer'da bağlantı bilgileri eklemek için **appsettings.json** dosyasını edin.  

    ```json
    {
      "SearchServiceName": "Put your search service name here",
      "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
      "BlobStorageAccountName": "Put your Azure Storage account name here",
      "BlobStorageConnectionString": "Put your Azure Blob Storage connection string here",
      "CosmosDBConnectionString": "Put your Cosmos DB connection string here",
      "CosmosDBDatabaseName": "hotel-rooms-db"
    }
    ```

İlk iki giriş, Azure Bilişsel Arama hizmetinizin URL ve yönetici anahtarlarını kullanır. Bir bitiş noktası `https://mydemo.search.windows.net`göz önüne alındığında , örneğin, sağlamak için hizmet adı `mydemo`.

Sonraki girişler, Azure Blob Depolama ve Azure Cosmos DB veri kaynakları için hesap adlarını ve bağlantı dize bilgilerini belirtir.

## <a name="3---map-key-fields"></a>3 - Harita anahtar alanları

İçeriği birleştirme, her iki veri akışının da arama dizinindeki aynı belgeleri hedeflemesini gerektirir. 

Azure Bilişsel Arama'da, anahtar alan her belgeyi benzersiz olarak tanımlar. Her arama dizininin tam `Edm.String`olarak bir anahtar türü olmalıdır. Bu anahtar alanı, dizin ekleyen bir veri kaynağında her belge için bulunmalıdır. (Aslında, gerekli tek alandır.)

Birden çok veri kaynağından gelen verileri dizine dönüştürürken, her gelen satır veya belgenin, fiziksel olarak farklı iki kaynak belgedeki verileri birleştirilmiş dizinde yeni bir arama belgesiyle birleştirmek için ortak bir belge anahtarı içerdiğinden emin olun. 

Diziniçin anlamlı bir belge anahtarını tanımlamak ve her iki veri kaynağında da var olduğundan emin olmak için genellikle bazı ön planlama gerektirir. Bu demoda, `HotelId` Cosmos DB'deki her otelin anahtarı Blob depolama odasındaki JSON lekeleri de mevcuttur.

Azure Bilişsel Arama dizinleyicileri, kaynak verilerin doğru dizin alanına yönlendirilebilmeleri için dizin oluşturma işlemi sırasında veri alanlarını yeniden adlandırmak ve hatta yeniden biçimlendirmek için alan eşlemelerini kullanabilir. Örneğin, Cosmos DB'de otel tanımlayıcısı . **`HotelId`** Ama otel odaları için JSON blob dosyaları, otel tanımlayıcısı **`Id`** adlandırılır . Program, lekelerden dizindeki **`Id`** **`HotelId`** anahtar alana alanı eşleyerek bunu işler.

> [!NOTE]
> Çoğu durumda, bazı dizin oluşturi kişiler tarafından varsayılan olarak oluşturulanlar gibi otomatik olarak oluşturulan belge anahtarları, birleştirilmiş dizinler için iyi belge anahtarları yapmaz. Genel olarak, veri kaynaklarınızda zaten var olan veya kolayca eklenebilen anlamlı, benzersiz bir anahtar değeri kullanmak isteyeceksiniz.

## <a name="4---explore-the-code"></a>4 - Kodu keşfedin

Veri ve yapılandırma ayarları yerleştirildikten sonra, **AzureSearchMultipleDataSources.sln'deki** örnek program oluşturmaya ve çalıştırmaya hazır olmalıdır.

Bu basit C#/.NET konsol uygulaması aşağıdaki görevleri yerine getirir:

* C# Hotel sınıfının veri yapısına dayalı yeni bir dizin oluşturur (adres ve oda sınıflarına da başvurur).
* Yeni bir veri kaynağı ve Azure Cosmos DB verilerini dizin alanlarıyla eşleyen bir dizin oluşturucu oluşturur. Bunlar Azure Bilişsel Arama'daki nesnelerdir.
* Cosmos DB'den Otel verilerini yüklemek için dizinleyiciyi çalıştırır.
* JSON blob verilerini dizin alanlarıyla eşleyen ikinci bir veri kaynağı ve dizin oluşturucu oluşturur.
* Odalar verilerini Blob depolamadan yüklemek için ikinci dizinleyiciyi çalıştırır.

 Programı çalıştırmadan önce, bu örnek için kodu ve dizin ve dizin oluşturma tanımlarını incelemek için bir dakika ayırın. İlgili kod iki dosyada yer alır:

  + **Hotel.cs** dizini tanımlayan şema içerir
  + **Program.cs,** Azure Bilişsel Arama dizini, veri kaynakları ve dizin oluşturup birleştirilmiş sonuçları dizin içine yükleyen işlevler içerir.

### <a name="create-an-index"></a>Dizin oluşturma

Bu örnek program, bir Azure Bilişsel Arama dizini tanımlamak ve oluşturmak için .NET SDK'yı kullanır. C# veri modeli sınıfından bir dizin yapısı oluşturmak için [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder) sınıfından yararlanır.

Veri modeli, Adres ve Oda sınıflarına atıfta bulunan Otel sınıfı tarafından tanımlanır. FieldBuilder dizin için karmaşık bir veri yapısı oluşturmak için birden çok sınıf tanımları aracılığıyla aşağı delinme. Meta veri etiketleri, aranabilir veya sıralanabilir olup olmadığı gibi her alanın özniteliklerini tanımlamak için kullanılır.

**Hotel.cs** dosyasından aşağıdaki parçacıklar, tek bir alanın ve başka bir veri modeli sınıfına yapılan başvurunun nasıl belirtilebileceğini gösterir.

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
public Room[] Rooms { get; set; }
. . .
```

**Program.cs** dosyasında dizin, `FieldBuilder.BuildForType<Hotel>()` yöntem tarafından oluşturulan bir ad ve alan koleksiyonuyla tanımlanır ve ardından aşağıdaki gibi oluşturulur:

```csharp
private static async Task CreateIndex(string indexName, SearchServiceClient searchService)
{
    // Create a new search index structure that matches the properties of the Hotel class.
    // The Address and Room classes are referenced from the Hotel class. The FieldBuilder
    // will enumerate these to create a complex data structure for the index.
    var definition = new Index()
    {
        Name = indexName,
        Fields = FieldBuilder.BuildForType<Hotel>()
    };
    await searchService.Indexes.CreateAsync(definition);
}
```

### <a name="create-azure-cosmos-db-data-source-and-indexer"></a>Azure Cosmos DB veri kaynağı ve dizin oluştur

Sonraki ana program, otel verileri için Azure Cosmos DB veri kaynağını oluşturmak için mantık içerir.

Önce Azure Cosmos DB veritabanı adını bağlantı dizesine bağlar. Ardından, [useChangeDetection] özelliği gibi Azure Cosmos DB kaynaklarına özgü ayarlar da dahil olmak üzere veri kaynağı nesnesini tanımlar.

  ```csharp
private static async Task CreateAndRunCosmosDbIndexer(string indexName, SearchServiceClient searchService)
{
    // Append the database name to the connection string
    string cosmosConnectString = 
        configuration["CosmosDBConnectionString"]
        + ";Database=" 
        + configuration["CosmosDBDatabaseName"];

    DataSource cosmosDbDataSource = DataSource.CosmosDb(
        name: configuration["CosmosDBDatabaseName"], 
        cosmosDbConnectionString: cosmosConnectString,
        collectionName: "hotels",
        useChangeDetection: true);

    // The Azure Cosmos DB data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(cosmosDbDataSource);
  ```

Veri kaynağı oluşturulduktan sonra, program **otel odaları-cosmos-indexer**adlı bir Azure Cosmos DB dizinleyici ayarlar.

```csharp
    Indexer cosmosDbIndexer = new Indexer(
        name: "hotel-rooms-cosmos-indexer",
        dataSourceName: cosmosDbDataSource.Name,
        targetIndexName: indexName,
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));
    
    // Indexers keep metadata about how much they have already indexed.
    // If we already ran this sample, the indexer will remember that it already
    // indexed the sample data and not run again.
    // To avoid this, reset the indexer if it exists.
    bool exists = await searchService.Indexers.ExistsAsync(cosmosDbIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(cosmosDbIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(cosmosDbIndexer);
```
Program, bu örneği birden fazla kez çalıştırmak istiyorsanız, yenisini oluşturmadan önce aynı ada sahip varolan dizin oluşturlayıcıları siler.

Bu örnek, dizinleyici için bir zamanlama tanımlar, böylece günde bir kez çalışacaktır. Dizinleyicinin gelecekte otomatik olarak yeniden çalışmasını istemiyorsanız zamanlama özelliğini bu çağrıdan kaldırabilirsiniz.

### <a name="index-azure-cosmos-db-data"></a>Azure Cosmos DB verilerini dizinle

Veri kaynağı ve dizinleyici oluşturulduktan sonra, dizinleyiciyi çalıştıran kod kısadır:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Bu örnek, yürütme sırasında oluşabilecek hataları bildirmek için basit bir try-catch bloğu içerir.

Azure Cosmos DB dizinleyicisi çalıştırıldıktan sonra, arama dizini tam bir örnek otel belgesi kümesi içerir. Ancak Azure Cosmos DB veri kaynağı oda ayrıntıları içermediğinden, her otelin oda alanı boş bir dizi olacaktır. Daha sonra, program oda verilerini yüklemek ve birleştirmek için Blob depolama dan çekecektir.

### <a name="create-blob-storage-data-source-and-indexer"></a>Blob depolama veri kaynağı ve dizin oluştur

Oda ayrıntılarını almak için program ilk bireysel JSON blob dosyaları bir dizi başvuru için bir Blob depolama veri kaynağı kurar.

```csharp
private static async Task CreateAndRunBlobIndexer(string indexName, SearchServiceClient searchService)
{
    DataSource blobDataSource = DataSource.AzureBlobStorage(
        name: configuration["BlobStorageAccountName"],
        storageConnectionString: configuration["BlobStorageConnectionString"],
        containerName: "hotel-rooms");

    // The blob data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(blobDataSource);
```

Veri kaynağı oluşturulduktan sonra, program **otel odaları-blob-indexer**adlı bir blob dizinleyici ayarlar.

```csharp
    // Add a field mapping to match the Id field in the documents to 
    // the HotelId key field in the index
    List<FieldMapping> map = new List<FieldMapping> {
        new FieldMapping("Id", "HotelId")
    };

    Indexer blobIndexer = new Indexer(
        name: "hotel-rooms-blob-indexer",
        dataSourceName: blobDataSource.Name,
        targetIndexName: indexName,
        fieldMappings: map,
        parameters: new IndexingParameters().ParseJson(),
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));

    // Reset the indexer if it already exists
    bool exists = await searchService.Indexers.ExistsAsync(blobIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(blobIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(blobIndexer);
```

JSON lekeleri yerine '' **`Id`** adı **`HotelId`** verilen bir anahtar alanı içerir. Kod, dizindeki `FieldMapping` **`Id`** **`HotelId`** belge anahtarına alan değerini yönlendirmesini dizinleyiciye söylemek için sınıfı kullanır.

Blob depolama dizinleyicileri kullanılacak ayrışma modunu tanımlayan parametreleri kullanabilir. Ayrıştma modu, tek bir belgeyi temsil eden lekeler veya aynı blob içindeki birden çok belge için farklılık gösterir. Bu örnekte, her blob tek bir dizin `IndexingParameters.ParseJson()` belgesini temsil eder, bu nedenle kod parametreyi kullanır.

JSON lekeleri için dizinleyici ayrıştma parametreleri hakkında daha fazla bilgi için [Bkz.](search-howto-index-json-blobs.md) .NET SDK kullanarak bu parametreleri belirtme hakkında daha fazla bilgi için [IndexerParametersExtension](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingparametersextensions) sınıfına bakın.

Program, bu örneği birden fazla kez çalıştırmak istiyorsanız, yenisini oluşturmadan önce aynı ada sahip varolan dizin oluşturlayıcıları siler.

Bu örnek, dizinleyici için bir zamanlama tanımlar, böylece günde bir kez çalışacaktır. Dizinleyicinin gelecekte otomatik olarak yeniden çalışmasını istemiyorsanız zamanlama özelliğini bu çağrıdan kaldırabilirsiniz.

### <a name="index-blob-data"></a>Dizin blob verileri

Blob depolama veri kaynağı ve dizinleyici oluşturulduktan sonra, dizinoluşturucuyu çalıştıran kod basittir:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Dizin zaten Azure Cosmos DB veritabanındaki otel verileriyle doldurulmuş olduğundan, blob dizinleyici dizindeki varolan belgeleri güncelleştirir ve oda ayrıntılarını ekler.

> [!NOTE]
> Her iki veri kaynağınızda da aynı anahtar olmayan alanlara sahipseniz ve bu alanlardaki veriler eşleşmiyorsa, dizin en son çalıştırılan dizin değerleri içerir. Örneğimizde, her iki veri kaynağı da **HotelName** alanı içerir. Herhangi bir nedenle bu alandaki veriler farklıysa, aynı anahtar değerine sahip belgeler için, en son dizine eklenen veri kaynağından **gelen HotelName** verileri dizinde depolanan değer olacaktır.

## <a name="5---search"></a>5 - Arama

Portaldaki [**Arama gezginini**](search-explorer.md) kullanarak program çalıştırıldıktan sonra doldurulan arama dizinini keşfedebilirsiniz.

Azure portalında, arama hizmeti **genel bakış** sayfasını açın ve **Indeksler** listesinde **otel odaları örnek** dizinini bulun.

  ![Azure Bilişsel Arama dizinleri listesi](media/tutorial-multiple-data-sources/index-list.png "Azure Bilişsel Arama dizinleri listesi")

Listedeki otel odaları-örnek endeksine tıklayın. Dizin için bir Arama Gezgini arabirimi görürsünüz. "Lüks" gibi bir terim için sorgu girin. Sonuçlarda en az bir belge görmeniz gerekir ve bu belge oda dizisinde oda nesnelerinin listesini göstermelidir.

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

Geliştirmenin ilk deneysel aşamalarında, tasarım yinelemesi için en pratik yaklaşım, nesneleri Azure Bilişsel Arama'dan silmek ve kodunuzu yeniden oluşturmasına izin vermektir. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Bu öğreticinin örnek kodu varolan nesneleri denetler ve kodunuzu yeniden çalıştırmak için bunları siler.

Portalı dizinleri, dizin leyicileri ve veri kaynaklarını silmek için de kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda, artık ihtiyacınız olmayan kaynakları kaldırmak iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki Tüm kaynaklar veya Kaynak grupları bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık birden çok kaynaktan veri alma kavramına aşina olduğunuza göre, Cosmos DB ile başlayan dizinleyici yapılandırmasına daha yakından bakalım.

> [!div class="nextstepaction"]
> [Azure Cosmos DB dizinleyicisi yapılandırma](search-howto-index-cosmosdb.md)