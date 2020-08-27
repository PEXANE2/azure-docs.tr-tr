---
title: C# öğreticisi birden çok Azure veri kaynağını dizinleyen
titleSuffix: Azure Cognitive Search
description: Çoklu veri kaynaklarından verileri Dizin oluşturucular kullanarak tek bir Azure Bilişsel Arama dizinine aktarmayı öğrenin. Bu öğretici ve örnek kod C# ' de bulunur.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 06/20/2020
ms.openlocfilehash: 815709611c0d7985a97ce52584b9e223c6d79aa2
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88924121"
---
# <a name="tutorial-index-from-multiple-data-sources-using-the-net-sdk"></a>Öğretici: .NET SDK kullanarak birden çok veri kaynağından Dizin

Azure Bilişsel Arama, birden çok veri kaynağından verileri içeri aktarabilir, çözümleyebilir ve tek bir birleştirme arama dizinine dizinedebilir. Bu, yapılandırılmış verilerin metin, HTML veya JSON belgeleri gibi diğer kaynaklardan daha az yapılandırılmış veya hatta düz metin verileriyle toplanmış olduğu durumları destekler.

Bu öğreticide, otel verilerinin bir Azure Cosmos DB veri kaynağından nasıl indeksedileceği ve Azure Blob depolama belgelerinden alınan otel odası ayrıntılarıyla birleştirilebileceğiniz açıklanır. Sonuç, karmaşık veri türleri içeren bir birleştirilmiş otel arama dizini olacaktır.

Bu öğretici C# ve [.NET SDK](/dotnet/api/overview/azure/search)kullanır. Bu öğreticide, aşağıdaki görevleri gerçekleştirirsiniz:

> [!div class="checklist"]
> * Örnek verileri karşıya yükleme ve veri kaynaklarını oluşturma
> * Belge anahtarını tanımla
> * Dizini tanımlama ve oluşturma
> * Azure Cosmos DB otel verilerinin dizinini oluştur
> * BLOB depolama alanından otel odası verilerini birleştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

+ [Azure Cosmos DB](../cosmos-db/create-cosmosdb-resources-portal.md)
+ [Azure Depolama](../storage/common/storage-account-create.md)
+ [Visual Studio 2019](https://visualstudio.microsoft.com/)
+ [Mevcut bir arama hizmeti](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) [oluşturun](search-create-service-portal.md) veya bulun 

> [!Note]
> Bu öğretici için ücretsiz hizmeti kullanabilirsiniz. Ücretsiz arama hizmeti, sizi üç Dizin, üç Dizin Oluşturucu ve üç veri kaynağı ile sınırlandırır. Bu öğreticide hepsinden birer tane oluşturulur. Başlamadan önce, hizmetinize yeni kaynakları kabul etmek için yeriniz olduğundan emin olun.

## <a name="download-files"></a>Dosyaları indirme

Bu öğreticinin kaynak kodu, [birden çok veri kaynakları](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/multiple-data-sources) klasöründe [Azure-Search-DotNet-Samples](https://github.com/Azure-Samples/azure-search-dotnet-samples) GitHub deposunda bulunur.

## <a name="1---create-services"></a>1-hizmet oluşturma

Bu öğretici, dizin oluşturma ve sorgular için Azure Bilişsel Arama, bir veri kümesi için Azure Cosmos DB ve ikinci veri kümesi için Azure Blob depolama kullanır. 

Mümkünse, yakınlık ve yönetilebilirlik için aynı bölgedeki ve kaynak grubundaki tüm hizmetleri oluşturun. Uygulamada, hizmetleriniz herhangi bir bölgede olabilir.

Bu örnek, yedi kurgusal oteli tanımlayan iki küçük veri kümesini kullanır. Bir küme, otelleri tanımlar ve bir Azure Cosmos DB veritabanına yüklenir. Diğer küme, otel odası ayrıntılarını içerir ve Azure Blob depolamaya yüklenecek yedi ayrı JSON dosyası olarak sağlanır.

### <a name="start-with-cosmos-db"></a>Cosmos DB kullanmaya başlayın

1. [Azure Portal](https://portal.azure.com)oturum açın ve Azure Cosmos DB hesabınıza genel bakış sayfasına gidin.

1. **Veri Gezgini** ' yi seçin ve ardından **Yeni veritabanı**' nı seçin.

   ![Yeni veritabanı oluşturma](media/tutorial-multiple-data-sources/cosmos-newdb.png "Yeni veritabanı oluşturma")

1. **Otel-odalar-DB**adını girin. Geri kalan ayarlar için varsayılan değerleri kabul edin.

   ![Veritabanını yapılandır](media/tutorial-multiple-data-sources/cosmos-dbname.png "Veritabanını yapılandır")

1. Yeni bir kapsayıcı oluşturun. Yeni oluşturduğunuz mevcut veritabanını kullanın. Kapsayıcı adı için **oteller** girin ve bölüm anahtarı için **/Hotelıd** kullanın.

   ![Kapsayıcı ekleme](media/tutorial-multiple-data-sources/cosmos-add-container.png "Kapsayıcı ekleme")

1. **Oteller**altındaki **öğeleri** seçin ve ardından komut çubuğunda **öğeyi karşıya yükle** ' ye tıklayın. Öğesine gidin ve proje klasöründe **cosmosdb/HotelsDataSubset_CosmosDb.js** dosyasını seçin.

   ![Azure Cosmos DB koleksiyonuna yükle](media/tutorial-multiple-data-sources/cosmos-upload.png "Cosmos DB koleksiyonuna yükle")

1. Oteller koleksiyonundaki öğelerin görünümünü yenilemek için Yenile düğmesini kullanın. Yedi yeni veritabanı belgesi listelendiğini görmeniz gerekir.

### <a name="azure-blob-storage"></a>Azure Blob depolama

1. [Azure Portal](https://portal.azure.com)oturum açın, Azure depolama hesabınıza gidin, **Bloblar**' a tıklayın ve ardından **+ Container**' a tıklayın.

1. Örnek otel odası JSON dosyalarını depolamak için **otel odaları** adlı [bir blob kapsayıcısı oluşturun](../storage/blobs/storage-quickstart-blobs-portal.md) . Ortak erişim düzeyini geçerli değerlerinden herhangi birine ayarlayabilirsiniz.

   ![Blob kapsayıcısı oluşturma](media/tutorial-multiple-data-sources/blob-add-container.png "Blob kapsayıcısı oluşturma")

1. Kapsayıcı oluşturulduktan sonra açın ve komut çubuğunda **karşıya yükle** ' yi seçin. Örnek dosyaları içeren klasöre gidin. Tümünü seçip **karşıya yükle**' ye tıklayın.

   ![Dosyaları karşıya yükleme](media/tutorial-multiple-data-sources/blob-upload.png "Dosyaları karşıya yükleme")

Karşıya yükleme tamamlandıktan sonra dosyalar veri kapsayıcısının listesinde görünmelidir.

### <a name="azure-cognitive-search"></a>Azure Bilişsel Arama

Üçüncü bileşen, [portalda oluşturabileceğiniz](search-create-service-portal.md)Azure bilişsel arama. Bu izlenecek yolu tamamlamak için ücretsiz katmanı kullanabilirsiniz. 

### <a name="get-an-admin-api-key-and-url-for-azure-cognitive-search"></a>Azure Bilişsel Arama yönelik bir yönetici API anahtarı ve URL 'SI alın

Azure Bilişsel Arama hizmetiyle etkileşim kurmak için hizmet URL 'SI ve erişim anahtarı gerekir. Her ikisiyle de bir arama hizmeti oluşturulur. bu nedenle, aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar**  >  **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

   Sorgu anahtarını da alın. Salt okuma erişimiyle sorgu istekleri vermek en iyi uygulamadır.

   ![Hizmet adı ve yönetici ve sorgu anahtarlarını alın](media/search-get-started-nodejs/service-name-and-keys.png)

İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="2---set-up-your-environment"></a>2-ortamınızı ayarlama

1. Visual Studio 2019 ' u başlatın ve **Araçlar** menüsünde **NuGet Paket Yöneticisi** ' ni seçin ve ardından **çözüm için NuGet paketlerini yönetin...**. 

1. **Araştır** sekmesinde, **Microsoft. Azure. Search** (sürüm 9.0.1 veya üzeri) ' i bulup daha sonra yükler. Yüklemeyi tamamlaması için ek iletişim kutularına tıklamacaksınız.

    ![Azure kitaplıklarını eklemek için NuGet kullanma](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

1. NuGet paketindeki **Microsoft.Extensions.Configuration.Js** arayın ve bu paketi de yüklenir.

1. **AzureSearchMultipleDataSources. sln**çözüm dosyasını açın.

1. Çözüm Gezgini, bağlantı bilgilerini eklemek için dosya **appsettings.js** düzenleyin.  

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

İlk iki giriş, Azure Bilişsel Arama hizmetiniz için URL ve yönetici anahtarlarını kullanır. İçin bir uç nokta verildiğinde `https://mydemo.search.windows.net` , örneğin, sağlanacak hizmet adı `mydemo` .

Sonraki girişler, Azure Blob depolama ve Azure Cosmos DB veri kaynakları için hesap adlarını ve bağlantı dizesi bilgilerini belirtir.

## <a name="3---map-key-fields"></a>3-anahtar alanlarını eşleme

İçerik birleştirme, her iki veri akışının de arama dizininde aynı belgeleri hedeflemesini gerektirir. 

Azure Bilişsel Arama 'de, anahtar alanı her belgeyi benzersiz bir şekilde tanımlar. Her arama dizininin türünde tam olarak bir anahtar alanı olmalıdır `Edm.String` . Bu anahtar alanı, dizine eklenen bir veri kaynağındaki her belge için mevcut olmalıdır. (Aslında, tek gerekli alandır.)

Birden çok veri kaynağından veri dizinlerken, her gelen satır veya belgenin, iki fiziksel farklı kaynak belgesinden verileri Birleşik dizindeki yeni bir arama belgesiyle birleştirmek için ortak bir belge anahtarı içerdiğinden emin olun. 

Genellikle dizininiz için anlamlı bir belge anahtarı belirlemek için bazı önde bir planlama gerektirir ve her iki veri kaynağında de bulunduğundan emin olun. Bu gösteride, `HotelId` Cosmos DB içindeki her bir otelin anahtarı blob depolamada yer aldığı odalar JSON Bloblarında de bulunur.

Azure Bilişsel Arama Dizinleyicileri, dizin oluşturma işlemi sırasında veri alanlarını yeniden adlandırmak ve hatta yeniden biçimlendirmek için alan eşlemelerini kullanarak kaynak verilerin doğru Dizin alanına yönlendirilebilmesi için kullanılabilir. Örneğin, Cosmos DB, otel tanımlayıcısı çağırılır **`HotelId`** . Ancak, otel odalarının JSON blob dosyalarında, otel tanımlayıcısı olarak adlandırılır **`Id`** . Program bunu, **`Id`** alanı bloblardan **`HotelId`** dizindeki anahtar alanı ile eşleyerek işler.

> [!NOTE]
> Çoğu durumda, bazı Dizin oluşturucular tarafından varsayılan olarak oluşturulanlar gibi otomatik olarak oluşturulan belge anahtarları, birleştirilmiş dizinler için iyi belge anahtarları oluşturmazlar. Genel olarak, içinde zaten bulunan veya veri kaynaklarınıza kolayca eklenebilen anlamlı, benzersiz bir anahtar değeri kullanmak isteyeceksiniz.

## <a name="4---explore-the-code"></a>4-kodu keşfet

Veriler ve yapılandırma ayarları olduktan sonra, **AzureSearchMultipleDataSources. sln** dosyasındaki örnek program, derleme ve çalıştırmaya hazırlanmalıdır.

Bu basit C#/.NET konsol uygulaması aşağıdaki görevleri gerçekleştirir:

* C# otel sınıfının veri yapısına dayalı yeni bir dizin oluşturur (Ayrıca adres ve oda sınıflarına de başvurur).
* Yeni bir veri kaynağı ve dizin alanları Azure Cosmos DB verileri eşleyen bir dizin oluşturucu oluşturur. Bunlar her iki nesne de Azure Bilişsel Arama.
* Cosmos DB ' dan otel verilerini yüklemek için Dizin oluşturucuyu çalıştırır.
* İkinci bir veri kaynağı ve JSON blob verilerini Dizin alanlarıyla eşleyen bir dizin oluşturucu oluşturur.
* Blob depolamadan Oda verilerini yüklemek için ikinci dizin oluşturucuyu çalıştırır.

 Programı çalıştırmadan önce, bu örneğe ilişkin kodu ve dizin ve Dizin Oluşturucu tanımlarını incelemek için bir dakikanızı alın. İlgili kod iki dosyada yer alır:

  + **Hotel.cs** , dizini tanımlayan şemayı içerir
  + **Program.cs** , Azure bilişsel arama dizinini, veri kaynaklarını ve Dizin oluşturucuyu oluşturan ve Birleşik sonuçları dizine yükleyen işlevleri içerir.

### <a name="create-an-index"></a>Dizin oluşturma

Bu örnek program, bir Azure Bilişsel Arama dizini tanımlamak ve oluşturmak için .NET SDK 'sını kullanır. Bir C# veri modeli sınıfından dizin yapısı oluşturmak için [FieldBuilder](/dotnet/api/microsoft.azure.search.fieldbuilder) sınıfından yararlanır.

Veri modeli, Otel Sınıfı tarafından tanımlanır ve bu da adres ve oda sınıflarına başvurular içerir. FieldBuilder, dizin için karmaşık bir veri yapısı oluşturmak üzere birden fazla sınıf tanımı aracılığıyla ayrıntıya gider. Meta veri etiketleri, her alanın, aranabilir veya sıralanabilir olup olmadığı gibi özniteliklerini tanımlamak için kullanılır.

**Hotel.cs** dosyasındaki aşağıdaki kod parçacıkları, tek bir alanın ve başka bir veri modeli sınıfına yapılan başvurunun nasıl belirtime olduğunu gösterir.

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
public Room[] Rooms { get; set; }
. . .
```

**Program.cs** dosyasında Dizin, yöntemi tarafından oluşturulan bir ad ve alan koleksiyonuyla tanımlanır `FieldBuilder.BuildForType<Hotel>()` ve sonra aşağıdaki gibi oluşturulur:

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

### <a name="create-azure-cosmos-db-data-source-and-indexer"></a>Azure Cosmos DB veri kaynağı ve Dizin Oluşturucu oluşturma

Ardından ana program, otel verileri için Azure Cosmos DB veri kaynağı oluşturma mantığını içerir.

İlk olarak, Azure Cosmos DB veritabanı adını bağlantı dizesine ekler. Daha sonra veri kaynağı nesnesini tanımlar, örneğin, [useChangeDetection] özelliği gibi Azure Cosmos DB kaynaklarına özgü ayarlar.

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

Veri kaynağı oluşturulduktan sonra program, **otel-odalar-Cosmos-Indexer**adlı bir Azure Cosmos DB Dizin Oluşturucu ayarlar.

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
Program, bu örneği birden çok kez çalıştırmak istemeniz durumunda yenisini oluşturmadan önce aynı ada sahip mevcut dizin oluşturucularının silinmesine izin vermez.

Bu örnek, Dizin Oluşturucu için bir zamanlama tanımlar, böylece günde bir kez çalıştırılır. Dizin oluşturucunun daha sonra otomatik olarak yeniden çalışmasını istemiyorsanız bu çağrıdan Schedule özelliğini kaldırabilirsiniz.

### <a name="index-azure-cosmos-db-data"></a>Azure Cosmos DB verileri dizini

Veri kaynağı ve Dizin Oluşturucu oluşturulduktan sonra, Dizin oluşturucuyu çalıştıran kod kısa olur:

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

Bu örnek, yürütme sırasında ortaya çıkabilecek hataları raporlamak için basit bir try-catch bloğu içerir.

Azure Cosmos DB Dizin Oluşturucu çalıştıktan sonra, arama dizini bir örnek otel belgeleri kümesi içerir. Ancak, Azure Cosmos DB veri kaynağında hiçbir Oda Ayrıntısı bulunmadığından, her otel için Odalar alanı boş bir dizi olacaktır. Daha sonra program, Oda verilerini yüklemek ve birleştirmek için BLOB depolama alanından çekmesini sağlar.

### <a name="create-blob-storage-data-source-and-indexer"></a>BLOB depolama veri kaynağı ve Dizin Oluşturucu oluştur

Oda ayrıntılarını almak için, program ilk olarak tek bir JSON blob dosyası kümesine başvurmak üzere bir BLOB depolama veri kaynağı ayarlar.

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

Veri kaynağı oluşturulduktan sonra program, **otel-odalar-blob-Indexer**adlı bir blob Dizin Oluşturucu ayarlar.

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

JSON blob 'ları yerine adında bir anahtar alanı içerir **`Id`** **`HotelId`** . Kod, Dizin `FieldMapping` oluşturucudan **`Id`** alan değerini **`HotelId`** dizindeki belge anahtarına yönlendirecek şekilde söylemek için sınıfını kullanır.

BLOB depolama Dizin oluşturucular, kullanılacak ayrıştırma modunu tanımlayan parametreleri kullanabilir. Ayrıştırma modu, tek bir belgeyi veya aynı blob içindeki birden çok belgeyi temsil eden Bloblar için farklılık gösterir. Bu örnekte, her blob tek bir dizin belgesini temsil ettiğinden, kod `IndexingParameters.ParseJson()` parametresini kullanır.

JSON Blobları için Dizin Oluşturucu ayrıştırma parametreleri hakkında daha fazla bilgi için bkz. [DIZIN JSON blob 'ları](search-howto-index-json-blobs.md). .NET SDK kullanarak bu parametreleri belirtme hakkında daha fazla bilgi için bkz. [IndexerParametersExtension](/dotnet/api/microsoft.azure.search.models.indexingparametersextensions) sınıfı.

Program, bu örneği birden çok kez çalıştırmak istemeniz durumunda yenisini oluşturmadan önce aynı ada sahip mevcut dizin oluşturucularının silinmesine izin vermez.

Bu örnek, Dizin Oluşturucu için bir zamanlama tanımlar, böylece günde bir kez çalıştırılır. Dizin oluşturucunun daha sonra otomatik olarak yeniden çalışmasını istemiyorsanız bu çağrıdan Schedule özelliğini kaldırabilirsiniz.

### <a name="index-blob-data"></a>Dizin blobu verileri

BLOB depolama veri kaynağı ve Dizin Oluşturucu oluşturulduktan sonra, Dizin oluşturucuyu çalıştıran kod basittir:

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

Dizin Azure Cosmos DB veritabanından otel verileriyle doldurulduğundan, blob Indexer dizindeki mevcut belgeleri güncelleştirir ve Oda ayrıntılarını ekler.

> [!NOTE]
> Her iki veri kaynağınız de aynı anahtar olmayan alanlara sahipseniz ve bu alanlardaki veriler eşleşmiyorsa, dizin, dizin oluşturucunun en son çalıştırıldığı değerleri içerir. Örneğimizde, her iki veri kaynağı da bir **Hotelname** alanı içerir. Bazı nedenlerle bu alandaki veriler farklıysa, aynı anahtar değerine sahip belgeler için, en son dizinlenen veri kaynağından gelen **barındırma adı** verileri dizinde depolanan değer olacaktır.

## <a name="5---search"></a>5-arama

Program çalıştırıldıktan sonra, portalda [**Arama Gezgini**](search-explorer.md) 'ni kullanarak doldurulmuş arama dizinini inceleyebilirsiniz.

Azure portal ' de, arama hizmeti **genel bakış** sayfasını açın ve **dizinler** listesinde **otel-odalar-Sample** dizinini bulun.

  ![Azure Bilişsel Arama dizinlerinin listesi](media/tutorial-multiple-data-sources/index-list.png "Azure Bilişsel Arama dizinlerinin listesi")

Listedeki otel-odalar-örnek dizinine tıklayın. Dizin için bir arama Gezgini arabirimi görürsünüz. "Merkezlerini" gibi bir terim için bir sorgu girin. Sonuçlarda en az bir belge görmeniz gerekir ve bu belge, Oda dizisindeki bir oda nesnelerinin bir listesini göstermelidir.

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

Geliştirmede erken deneysel aşamalarda, tasarım yinelemesi için en pratik yaklaşım, nesneleri Azure Bilişsel Arama silmek ve kodunuzun bunları yeniden oluşturması için izin verir. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Bu öğreticinin örnek kodu mevcut nesneleri denetler ve kodunuzu yeniden çalıştırabilmeniz için onları siler.

Ayrıca, dizinleri, Dizin oluşturucuyu ve veri kaynaklarını silmek için portalını de kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir projenin sonunda kendi aboneliğinizde çalışırken, artık ihtiyaç duyulmadığınızda kaynakları kaldırmak iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki tüm kaynaklar veya kaynak grupları bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık birden çok kaynaktan veri alma kavramı hakkında bilgi sahibi olduğunuza göre, Cosmos DB başlayarak Dizin Oluşturucu yapılandırmasına daha yakından bakalım.

> [!div class="nextstepaction"]
> [Azure Cosmos DB Dizin Oluşturucu yapılandırma](search-howto-index-cosmosdb.md)