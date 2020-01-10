---
title: 'C#Öğretici: birden çok veri kaynağını Dizin'
titleSuffix: Azure Cognitive Search
description: Çoklu veri kaynaklarından verileri Dizin oluşturucular kullanarak tek bir Azure Bilişsel Arama dizinine aktarmayı öğrenin. Bu öğretici ve örnek kod ' de C#bulunur.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/23/2019
ms.openlocfilehash: aac5dc300009ec682ef1599ad654415f5c4ad190
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/26/2019
ms.locfileid: "75495066"
---
# <a name="c-tutorial-combine-data-from-multiple-data-sources-in-one-azure-cognitive-search-index"></a>C#Öğretici: tek bir Azure Bilişsel Arama dizininde bulunan birden çok veri kaynağından verileri birleştirme

Azure Bilişsel Arama, birden çok veri kaynağından alınan verileri tek bir Birleşik arama dizininde içeri aktarabilir, çözümleyebilir ve dizine alabilir. Bu, yapılandırılmış verilerin metin, HTML veya JSON belgeleri gibi diğer kaynaklardan daha az yapılandırılmış veya hatta düz metin verileriyle toplanmış olduğu durumları destekler.

Bu öğreticide, otel verilerinin bir Azure Cosmos DB veri kaynağından nasıl indeksedileceği ve Azure Blob depolama belgelerinden alınan otel odası ayrıntılarıyla birleştirilebileceğiniz açıklanır. Sonuç, karmaşık veri türleri içeren bir birleştirilmiş otel arama dizini olacaktır.

Bu öğreticide C#, Azure BILIŞSEL arama .NET SDK ve aşağıdaki görevleri yapmak için Azure Portal kullanılmaktadır:

> [!div class="checklist"]
> * Örnek verileri karşıya yükleme ve veri kaynaklarını oluşturma
> * Belge anahtarını tanımla
> * Dizini tanımlama ve oluşturma
> * Azure Cosmos DB otel verilerinin dizinini oluştur
> * BLOB depolama alanından otel odası verilerini birleştirme

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki hizmetler, Araçlar ve veriler bu hızlı başlangıçta kullanılır. 

- Geçerli aboneliğinizde [bir Azure bilişsel arama hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz.

- Örnek otel verilerini depolamak için [bir Azure Cosmos DB hesabı oluşturun](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal) .

- Örnek oda verilerini depolamak için [bir Azure depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) .

- IDE olarak kullanmak için [Visual Studio 2019](https://visualstudio.microsoft.com/) ' i yükler.

### <a name="install-the-project-from-github"></a>Projeyi GitHub 'dan yükler

1. GitHub 'da örnek depoyu bulun: [Azure-Search-DotNet-Samples](https://github.com/Azure-Samples/azure-search-dotnet-samples).
1. **Kopyala veya indir** ' i seçin ve deponun özel yerel kopyasını yapın.
1. Visual Studio 2019 ' i açın ve henüz yüklenmemişse Bilişsel Arama NuGet paketini Microsoft Azure. **Araçlar** menüsünde **NuGet Paket Yöneticisi** ' ni ve ardından **çözüm için NuGet Paketlerini Yönet...** ' i seçin. **Araştır** sekmesinde, **Microsoft. Azure. Search** (sürüm 9.0.1 veya üzeri) ' i bulup daha sonra yükler. Yüklemeyi tamamlaması için ek iletişim kutularına tıklamacaksınız.

    ![Azure kitaplıklarını eklemek için NuGet kullanma](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

1. Visual Studio 'yu kullanarak yerel deponuza gidin ve **AzureSearchMultipleDataSources. sln**çözüm dosyasını açın.

## <a name="get-a-key-and-url"></a>Anahtar ve URL al

Azure Bilişsel Arama hizmetinize etkileşimde bulunmak için hizmet URL 'SI ve erişim anahtarı gerekir. Her ikisiyle de bir arama hizmeti oluşturulur. bu nedenle, aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com/)oturum açın ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

![HTTP uç noktası ve erişim anahtarı al](media/search-get-started-postman/get-url-key.png "HTTP uç noktası ve erişim anahtarı al")

Tüm istekler hizmetinize gönderilen her istekte bir API anahtarı gerektirir. Geçerli bir anahtar, istek başına, isteği gönderen uygulama ve onu işleyen hizmet arasında güven oluşturur.

## <a name="prepare-sample-azure-cosmos-db-data"></a>Örnek Azure Cosmos DB verileri hazırlama

Bu örnek, yedi kurgusal oteli tanımlayan iki küçük veri kümesini kullanır. Bir küme, otelleri tanımlar ve bir Azure Cosmos DB veritabanına yüklenir. Diğer küme, otel odası ayrıntılarını içerir ve Azure Blob depolamaya yüklenecek yedi ayrı JSON dosyası olarak sağlanır.

1. [Azure Portal](https://portal.azure.com)oturum açın ve Azure Cosmos DB hesabınıza genel bakış sayfasına gidin.

1. **Veri Gezgini** ' yi seçin ve ardından **Yeni veritabanı**' nı seçin.

   ![Yeni veritabanı oluştur](media/tutorial-multiple-data-sources/cosmos-newdb.png "Yeni veritabanı oluşturma")

1. **Otel-odalar-DB**adını girin. Geri kalan ayarlar için varsayılan değerleri kabul edin.

   ![Veritabanını yapılandır](media/tutorial-multiple-data-sources/cosmos-dbname.png "Veritabanını yapılandır")

1. Yeni bir kapsayıcı oluşturun. Yeni oluşturduğunuz mevcut veritabanını kullanın. Kapsayıcı adı için **oteller** girin ve bölüm anahtarı için **/Hotelıd** kullanın.

   ![Kapsayıcı Ekle](media/tutorial-multiple-data-sources/cosmos-add-container.png "Kapsayıcı ekleme")

1. **Oteller**altındaki **öğeleri** seçin ve ardından komut çubuğunda **öğeyi karşıya yükle** ' ye tıklayın. Öğesine gidin ve proje klasöründe **cosmosdb/HotelsDataSubset_CosmosDb. JSON** dosyasını seçin.

   ![Azure Cosmos DB koleksiyonuna yükle](media/tutorial-multiple-data-sources/cosmos-upload.png "Cosmos DB koleksiyonuna yükle")

1. Oteller koleksiyonundaki öğelerin görünümünü yenilemek için Yenile düğmesini kullanın. Yedi yeni veritabanı belgesi listelendiğini görmeniz gerekir.

## <a name="prepare-sample-blob-data"></a>Örnek blob verilerini hazırlama

1. [Azure Portal](https://portal.azure.com)oturum açın, Azure depolama hesabınıza gidin, **Bloblar**' a tıklayın ve ardından **+ Container**' a tıklayın.

1. Örnek otel odası JSON dosyalarını depolamak için **otel odaları** adlı [bir blob kapsayıcısı oluşturun](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) . Ortak erişim düzeyini geçerli değerlerinden herhangi birine ayarlayabilirsiniz.

   ![Blob kapsayıcısı oluşturma](media/tutorial-multiple-data-sources/blob-add-container.png "Blob kapsayıcısı oluşturma")

1. Kapsayıcı oluşturulduktan sonra açın ve komut çubuğunda **karşıya yükle** ' yi seçin. Örnek dosyaları içeren klasöre gidin. Tümünü seçip **karşıya yükle**' ye tıklayın.

   ![Dosyaları karşıya yükleme](media/tutorial-multiple-data-sources/blob-upload.png "Dosyaları karşıya yükleme")

Karşıya yükleme tamamlandıktan sonra dosyalar veri kapsayıcısının listesinde görünmelidir.

## <a name="set-up-connections"></a>Bağlantıları ayarlama

Arama hizmeti ve veri kaynakları için bağlantı bilgileri, çözümdeki **appSettings. JSON** dosyasında belirtilir. 

1. Visual Studio 'da **AzureSearchMultipleDataSources. sln** dosyasını açın.

1. Çözüm Gezgini, **appSettings. JSON** dosyasını düzenleyin.  

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

İlk iki giriş, Azure Bilişsel Arama hizmetiniz için URL ve yönetici anahtarlarını kullanır. Örneğin, sağlanacak hizmet adı `mydemo``https://mydemo.search.windows.net`uç noktası verilir.

Sonraki girişler, Azure Blob depolama ve Azure Cosmos DB veri kaynakları için hesap adlarını ve bağlantı dizesi bilgilerini belirtir.

### <a name="identify-the-document-key"></a>Belge anahtarını tanımla

Azure Bilişsel Arama 'de, anahtar alanı dizindeki her belgeyi benzersiz bir şekilde tanımlar. Her arama dizininin `Edm.String`türünde tam olarak bir anahtar alanı olmalıdır. Bu anahtar alanı, dizine eklenen bir veri kaynağındaki her belge için mevcut olmalıdır. (Aslında, tek gerekli alandır.)

Birden çok veri kaynağından veri dizinlerken, iki fiziksel ayrı kaynak belgesinden verileri Birleşik dizindeki yeni bir arama belgesiyle birleştirmek için ortak bir belge anahtarı kullanın. Genellikle dizininiz için anlamlı bir belge anahtarı belirlemek için bazı önde bir planlama gerektirir ve her iki veri kaynağında de bulunduğundan emin olun. Bu gösteride, Cosmos DB içindeki her otel için Hotelıd anahtarı, blob depolamada yer aldığı odalar JSON Bloblarında de bulunur.

Azure Bilişsel Arama Dizinleyicileri, dizin oluşturma işlemi sırasında veri alanlarını yeniden adlandırmak ve hatta yeniden biçimlendirmek için alan eşlemelerini kullanarak kaynak verilerin doğru Dizin alanına yönlendirilebilmesi için kullanılabilir.

Örneğin, örnek Azure Cosmos DB verilerimizde, otel tanımlayıcısı **`HotelId`** olarak adlandırılır. Ancak, otel odalarına yönelik JSON blob dosyalarında, otel tanımlayıcısı **`Id`** olarak adlandırılır. Program bunu, **`Id`** alanını bloblardan dizindeki **`HotelId`** anahtar alanına eşleyerek işler.

> [!NOTE]
> Çoğu durumda, varsayılan olarak bazı Dizin oluşturucular tarafından oluşturulan belgeler gibi otomatik olarak oluşturulmuş belge anahtarları, birleştirilmiş dizinler için iyi belge anahtarları oluşturmazlar. Genel olarak, içinde zaten bulunan veya veri kaynaklarınıza kolayca eklenebilen anlamlı, benzersiz bir anahtar değeri kullanmak isteyeceksiniz.

## <a name="understand-the-code"></a>Kodu anlama

Veriler ve yapılandırma ayarları olduktan sonra, **AzureSearchMultipleDataSources. sln** dosyasındaki örnek program, derleme ve çalıştırmaya hazırlanmalıdır.

Bu basit C#/.NET konsol uygulaması aşağıdaki görevleri gerçekleştirir:
* C# Otel sınıfının veri yapısına dayalı yeni bir dizin oluşturur (Ayrıca adres ve oda sınıflarına de başvurur).
* Yeni bir veri kaynağı ve dizin alanları Azure Cosmos DB verileri eşleyen bir dizin oluşturucu oluşturur. Bunlar her iki nesne de Azure Bilişsel Arama.
* Cosmos DB ' dan otel verilerini yüklemek için Dizin oluşturucuyu çalıştırır.
* İkinci bir veri kaynağı ve JSON blob verilerini Dizin alanlarıyla eşleyen bir dizin oluşturucu oluşturur.
* Blob depolamadan Oda verilerini yüklemek için ikinci dizin oluşturucuyu çalıştırır.

 Programı çalıştırmadan önce, bu örneğe ilişkin kodu ve dizin ve Dizin Oluşturucu tanımlarını incelemek için bir dakikanızı alın. İlgili kod iki dosyada yer alır:

  + **Hotel.cs** , dizini tanımlayan şemayı içerir
  + **Program.cs** , Azure bilişsel arama dizinini, veri kaynaklarını ve Dizin oluşturucuyu oluşturan ve Birleşik sonuçları dizine yükleyen işlevleri içerir.

### <a name="define-the-index"></a>Dizini tanımlama

Bu örnek program, bir Azure Bilişsel Arama dizini tanımlamak ve oluşturmak için .NET SDK 'sını kullanır. Bir C# veri modeli sınıfından dizin yapısı oluşturmak Için [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder) sınıfından yararlanır.

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

**Program.cs** dosyasında, Dizin `FieldBuilder.BuildForType<Hotel>()` yöntemi tarafından oluşturulan bir ad ve alan koleksiyonuyla tanımlanır ve sonra aşağıdaki gibi oluşturulur:

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

JSON blob 'ları **`HotelId`** yerine **`Id`** adlı bir anahtar alanı içerir. Kod, Dizin oluşturucudan **`Id`** alanı değerini dizindeki **`HotelId`** belge anahtarına yönlendirmiş olduğunu söylemek için `FieldMapping` sınıfını kullanır.

BLOB depolama Dizin oluşturucular, kullanılacak ayrıştırma modunu tanımlayan parametreleri kullanabilir. Ayrıştırma modu, tek bir belgeyi veya aynı blob içindeki birden çok belgeyi temsil eden Bloblar için farklılık gösterir. Bu örnekte, her blob tek bir dizin belgesini temsil ettiğinden kod `IndexingParameters.ParseJson()` parametresini kullanır.

JSON Blobları için Dizin Oluşturucu ayrıştırma parametreleri hakkında daha fazla bilgi için bkz. [DIZIN JSON blob 'ları](search-howto-index-json-blobs.md). .NET SDK kullanarak bu parametreleri belirtme hakkında daha fazla bilgi için bkz. [IndexerParametersExtension](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingparametersextensions) sınıfı.

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

## <a name="search-your-json-files"></a>JSON dosyalarınızı arama

Program çalıştırıldıktan sonra, portalda [**Arama Gezgini**](search-explorer.md) 'ni kullanarak doldurulmuş arama dizinini inceleyebilirsiniz.

Azure portal ' de, arama hizmeti **genel bakış** sayfasını açın ve **dizinler** listesinde **otel-odalar-Sample** dizinini bulun.

  ![Azure Bilişsel Arama dizinlerinin listesi](media/tutorial-multiple-data-sources/index-list.png "Azure Bilişsel Arama dizinlerinin listesi")

Listedeki otel-odalar-örnek dizinine tıklayın. Dizin için bir arama Gezgini arabirimi görürsünüz. "Merkezlerini" gibi bir terim için bir sorgu girin. Sonuçlarda en az bir belge görmeniz gerekir ve bu belge, Oda dizisindeki bir oda nesnelerinin bir listesini göstermelidir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiden sonra temizlemenin en hızlı yolu, Azure Bilişsel Arama hizmetini içeren kaynak grubunu silmelidir. Kaynak grubunu silerek içindeki her şeyi kalıcı olarak silebilirsiniz. Portalda, kaynak grubu adı Azure Bilişsel Arama hizmetinin genel bakış sayfasında bulunur.

## <a name="next-steps"></a>Sonraki adımlar

JSON bloblarını dizine almanın çeşitli yaklaşımları ve birden çok seçeneği vardır. Kaynak verileriniz JSON içeriği içeriyorsa, senaryonuza en uygun olanı görmek için bu seçenekleri inceleyebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama blob Indexer kullanarak JSON bloblarını dizin oluşturma](search-howto-index-json-blobs.md)
