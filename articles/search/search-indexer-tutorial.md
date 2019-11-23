---
title: 'Tutorial: Index data in C# from Azure SQL databases'
titleSuffix: Azure Cognitive Search
description: In this C# tutorial, connect to Azure SQL database, extract searchable data, and load it into an Azure Cognitive Search index.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 36215403f99cc86ab4fb111ce95a6b3190063d7b
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74406705"
---
# <a name="tutorial-import-azure-sql-database-in-c-using-azure-cognitive-search-indexers"></a>Tutorial: Import Azure SQL database in C# using Azure Cognitive Search indexers

Learn how to configure an indexer for extracting searchable data from a sample Azure SQL database. [Indexers](search-indexer-overview.md) are a component of Azure Cognitive Search that crawl external data sources, populating a [search index](search-what-is-an-index.md) with content. Of all indexers, the indexer for Azure SQL Database is the most widely used. 

Dizin oluşturucu yapılandırmasında yeterlilik kazanmak yararlı olur çünkü yazmanız ve korumanız gereken kod miktarını azaltır. Şemayla uyumlu bir JSON veri kümesi hazırlayıp göndermek yerine, veri kaynağına bir dizin oluşturucu ekleyebilir, bu dizin oluşturucunun verileri ayıklamasını ve dizine eklemesini sağlayabilir ve isteğe bağlı olarak temel kaynaktaki değişiklikleri almak için dizin oluşturucuyu yinelenen bir zamanlamada çalıştırabilirsiniz.

In this tutorial, use the [Azure Cognitive Search .NET client libraries](https://aka.ms/search-sdk) and a .NET Core console application to perform the following tasks:

> [!div class="checklist"]
> * Uygulama ayarlarına arama hizmeti bilgilerini ekleme
> * Azure SQL veritabanında bir dış veri kümesi hazırlama 
> * Örnek kodda dizinin ve dizin oluşturucunun tanımlarını gözden geçirme
> * Dizin oluşturucu kodunu çalıştırarak verileri içeri aktarma
> * Dizinde arama yapma
> * Portalda dizin oluşturucu yapılandırmasını görüntüleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

The following services, tools, and data are used in this quickstart. 

[Create an Azure Cognitive Search service](search-create-service-portal.md) or [find an existing service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) under your current subscription. You can use a free service for this tutorial.

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) stores the external data source used by an indexer. Örnek çözümde, tablo oluşturmak için bir SQL veri dosyası sağlanır. Steps for creating the service and database are provided in this tutorial.

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), any edition, can be used to run the sample solution. Sample code and instructions were tested on the free Community edition.

[Azure-Samples/search-dotnet-getting-started](https://github.com/Azure-Samples/search-dotnet-getting-started) provides the sample solution, located in the Azure samples GitHub repository. Download and extract the solution. By default, solutions are read-only. Right-click the solution and clear the read-only attribute so that you can modify files.

> [!Note]
> If you are using the free Azure Cognitive Search service, you are limited to three indexes, three indexers, and three data sources. Bu öğreticide hepsinden birer tane oluşturulur. Hizmetinizde yeni kaynakları kabul edecek kadar yer olduğundan emin olun.

## <a name="get-a-key-and-url"></a>Get a key and URL

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. A search service is created with both, so if you added Azure Cognitive Search to your subscription, follow these steps to get the necessary information:

1. [Sign in to the Azure portal](https://portal.azure.com/), and in your search service **Overview** page, get the URL. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. In **Settings** > **Keys**, get an admin key for full rights on the service. There are two interchangeable admin keys, provided for business continuity in case you need to roll one over. You can use either the primary or secondary key on requests for adding, modifying, and deleting objects.

![Get an HTTP endpoint and access key](media/search-get-started-postman/get-url-key.png "Get an HTTP endpoint and access key")

All requests require an api-key on every request sent to your service. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="set-up-connections"></a>Bağlantıları ayarlama
Gerekli hizmetlere bağlantı bilgileri, çözümdeki **appsettings.json** dosyasında belirtilir. 

1. In Visual Studio, open the **DotNetHowToIndexers.sln** file.

1. In Solution Explorer, open **appsettings.json** so that you can populate each setting.  

The first two entries you can fill in right now, using the URL and admin keys for your Azure Cognitive Search service. Given an endpoint of `https://mydemo.search.windows.net`, the service name to provide is `mydemo`.

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "AzureSqlConnectionString": "Put your Azure SQL database connection string here",
}
```

The last entry requires an existing database. You'll create it in the next step.

## <a name="prepare-sample-data"></a>Prepare sample data

Bu adımda, dizin oluşturucunun gezinebileceği bir dış veri kaynağı oluşturun. Azure SQL Veritabanı'nda veri kümesini oluşturmak için Azure Portal'ı ve örnekteki *hotels.sql* dosyasını kullanabilirsiniz. Azure Cognitive Search consumes flattened rowsets, such as one generated from a view or query. Örnek çözümdeki SQL dosyası tek bir tablo oluşturur ve doldurur.

Aşağıdaki alıştırmada, mevcut sunucu veya veritabanı olmadığı varsayılır ve 2. adımda size her ikisini de oluşturma yönergeleri verilir. İsteğe bağlı olarak, kaynağınızın mevcut olması durumunda, oteller tablosunu buna ekleyebilir ve 4. adımdan başlayabilirsiniz.

1. [Sign in to the Azure portal](https://portal.azure.com/). 

2. Find or create an **Azure SQL Database** to create a database, server, and resource group. Varsayılan değerleri ve en düşük düzey fiyatlandırma katmanını kullanabilirsiniz. Sunucu oluşturmanın bir avantajı, yönetici kullanıcı adı ve parolası belirtebilmenizdir; çünkü sonraki adımda tabloları oluşturmak ve yüklemek için bunlar gerekecektir.

   ![Yeni veritabanı sayfası](./media/search-indexer-tutorial/indexer-new-sqldb.png)

3. Yeni sunucuyu ve veritabanını dağıtmak için **Oluştur**'a tıklayın. Sunucu ve veritabanının dağıtılmasını bekleyin.

4. Yeni veritabanınızın SQL Veritabanı sayfasını (zaten açık değilse) açın. Kaynak adı *SQL Server* değil *SQL veritabanı* olmalıdır.

   ![SQL veritabanı sayfası](./media/search-indexer-tutorial/hotels-db.png)

4. On the navigation pane, click **Query editor (preview)** .

5. **Oturum Aç**'a tıklayın ve sunucu yöneticisinin kullanıcı adıyla parolasını girin.

6. **Sorgu aç**'a tıklayın ve *hotels.sql* dosyasının konumuna gidin. 

7. Dosyayı seçin ve **Aç**'a tıklayın. Betik aşağıdaki ekran görüntüsüne benzer görünmelidir:

   ![SQL betiği](./media/search-indexer-tutorial/sql-script.png)

8. Sorguyu yürütmek için **Çalıştır**'a tıklayın. Sonuçlar bölmesinde, 3 satır için sorgu başarılı oldu iletisini görmeniz gerekir.

9. Bu tablodan bir satır kümesi döndürmek için, doğrulama adımı olarak aşağıdaki sorguyu yürütebilirsiniz:

    ```sql
    SELECT HotelId, HotelName, Tags FROM Hotels
    ```
    `SELECT * FROM Hotels` sorgu prototipi Sorgu Düzenleyicisi'nde çalışmaz. Örnek verilerde, Konum alanında coğrafi koordinatlar vardır ve bunlar şu anda düzenleyicide işlenmemektedir. Sorgulanacak diğer sütunların listesi için, şu deyimi yürütebilirsiniz: `SELECT * FROM sys.columns WHERE object_id = OBJECT_ID('dbo.Hotels')`

10. Artık bir dış veri kümeniz olduğuna göre, veritabanının ADO.NET bağlantı dizesini kopyalayın. Veritabanınızın SQL Veritabanı sayfasında **Ayarlar** > **Bağlantı Dizeleri**'ne gidin ve ADO.NET bağlantı dizesini kopyalayın.
 
    ADO.NET bağlantı dizesi aşağıdaki örneğe benzer; geçerli bir veritabanı adı, kullanıcı adı ve parola kullanacak şekilde değiştirilmiştir.

    ```sql
    Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
    ```
11. Bağlantı dizesini, Visual Studio'da **appsettings.json** dosyasındaki üçüncü girdi olarak "AzureSqlConnectionString" içine yapıştırın.

    ```json
    {
      "SearchServiceName": "<placeholder-Azure-Search-service-name>",
      "SearchServiceAdminApiKey": "<placeholder-admin-key-for-Azure-Search>",
      "AzureSqlConnectionString": "Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security  Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
    }
    ```

## <a name="understand-the-code"></a>Kodu anlama

Once the data and configuration settings are in place, the sample program in **DotNetHowToIndexers.sln** is ready to build and run. Bunu yapmadan önce, birkaç dakikanızı ayırıp bu örnekteki dizin ve dizin oluşturucu tanımlarını öğrenin. İlgili kod iki dosyada yer alır:

  + **hotel.cs**, dizini tanımlayan şemayı içerir
  + **Program.cs**, hizmetinizdeki yapıları oluşturmaya ve yönetmeye yönelik işlevleri içerir

### <a name="in-hotelcs"></a>Hotel.cs dosyasında

Dizin şeması, aşağıdaki HotelName alan tanımında gösterildiği gibi alanın tam metin araması yapılabilir, filtrelenebilir veya sıralanabilir olup olmadığı gibi izin verilen işlemleri belirten öznitelikler de dahil olmak üzere alan koleksiyonunu tanımlar. 

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
```

Şema başka öğeler, örneğin arama puanını artırmak için puanlama profilleri, özel çözümleyiciler ve başka yapılar içerebilir. Öte yandan, bizim amaçlarımıza uygun olarak şema yalnızca örnek veri kümelerinde bulunan alanlarla seyrek bir şekilde tanımlanmıştır.

Bu öğreticide, dizin oluşturucu verileri tek bir veri kaynağından çeker. In practice, you can attach multiple indexers to the same index, creating a consolidated searchable index from multiple data sources. Nerede esnekliğe ihtiyacınız olduğuna bağlı olarak, yalnızca veri kaynaklarında değişiklik yapıp aynı dizin oluşturucu-dizin çiftini veya çeşitli dizin oluşturucu ve veri kaynağı bileşimleriyle tek bir dizini kullanabilirsiniz.

### <a name="in-programcs"></a>Program.cs dosyasında

The main program includes logic for creating a client, an index, a data source, and an indexer. Kod, bu programı birden çok kez çalıştırabileceğiniz varsayımıyla, aynı adı taşıyan kaynakları denetler ve siler.

The data source object is configured with settings that are specific to Azure SQL database resources, including [incremental indexing](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) for leveraging the built-in [change detection features](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) of Azure SQL. The demo hotels database in Azure SQL has a "soft delete" column named **IsDeleted**. When this column is set to true in the database, the indexer removes the corresponding document from the Azure Cognitive Search index.

  ```csharp
  Console.WriteLine("Creating data source...");

  DataSource dataSource = DataSource.AzureSql(
      name: "azure-sql",
      sqlConnectionString: configuration["AzureSQLConnectionString"],
      tableOrViewName: "hotels",
      deletionDetectionPolicy: new SoftDeleteColumnDeletionDetectionPolicy(
          softDeleteColumnName: "IsDeleted",
          softDeleteMarkerValue: "true"));
  dataSource.DataChangeDetectionPolicy = new SqlIntegratedChangeTrackingPolicy();

  searchService.DataSources.CreateOrUpdateAsync(dataSource).Wait();
  ```

An indexer object is platform-agnostic, where  configuration, scheduling, and invocation are the same regardless of the source. This example indexer includes a schedule, a reset option that clears indexer history, and calls a method to create and run the indexer immediately.

  ```csharp
  Console.WriteLine("Creating Azure SQL indexer...");
  Indexer indexer = new Indexer(
      name: "azure-sql-indexer",
      dataSourceName: dataSource.Name,
      targetIndexName: index.Name,
      schedule: new IndexingSchedule(TimeSpan.FromDays(1)));
  // Indexers contain metadata about how much they have already indexed
  // If we already ran the sample, the indexer will remember that it already
  // indexed the sample data and not run again
  // To avoid this, reset the indexer if it exists
  exists = await searchService.Indexers.ExistsAsync(indexer.Name);
  if (exists)
  {
      await searchService.Indexers.ResetAsync(indexer.Name);
  }

  await searchService.Indexers.CreateOrUpdateAsync(indexer);

  // We created the indexer with a schedule, but we also
  // want to run it immediately
  Console.WriteLine("Running Azure SQL indexer...");

  try
  {
      await searchService.Indexers.RunAsync(indexer.Name);
  }
  catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
  {
      Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
  }
  ```



## <a name="run-the-indexer"></a>Dizin oluşturucuyu çalıştırma

Bu adımda, programı derleyin ve çalıştırın. 

1. Çözüm Gezgini'nde **DotNetHowToIndexers** öğesine sağ tıklayın ve **Oluştur**'u seçin.
2. Bir kez daha **DotNetHowToIndexers** öğesine sağ tıklayın ve ardından **Hata Ayıkla** > **Yeni örnek başlat**'a tıklayın.

Program hata ayıklama modunda yürütülür. Konsol penceresinde her işlemin durumu bildirilir.

  ![SQL betiği](./media/search-indexer-tutorial/console-output.png)

Kodunuz, Azure'da arama hizmetinize bağlanarak Visual Studio'da yerel olarak çalıştırılır. Arama hizmeti de bağlantı dizesini kullanarak Azure SQL Veritabanı'na bağlanır ve veri kümesini alır. Bu kadar çok işlem olduğunda, bazı olası hata noktaları vardır ama hata alırsanız, önce aşağıdaki koşulları denetleyin:

+ Sağladığınız arama hizmeti bağlantı bilgileri, bu öğreticide hizmet adıyla sınırlıdır. Tam URL girdiyseniz, dizin oluşturma aşamasında bağlantı hatasıyla işlemler durdurulur.

+ **Appsettings.json** dosyasındaki veritabanı bağlantı bilgileri. Bu, portaldan alınmış ve veritabanınız için geçerli kullanıcı adı ve parolayı içerecek şekilde değiştirilmiş ADO.NET bağlantı dizesi olmalıdır. Kullanıcı hesabının verileri alma izni olmalıdır.

+ Kaynak sınırları. Recall that the Free tier has limits of 3 indexes, indexers, and data sources. Sınırına dayanmış bir hizmet yeni nesneler oluşturamaz.

## <a name="search-the-index"></a>Dizinde arama yapma 

Azure Portal'da, arama hizmetinin Genel Bakış sayfasında üst kısımdaki **Arama gezgini**'ne tıklayarak yeni dizine birkaç sorgu gönderin.

1. Üst kısımdaki **Dizini değiştir**'e tıklayarak *hotels* dizinini seçin.

2. Boş bir arama göndermek için **Ara** düğmesine tıklayın. 

   Dizininizdeki üç girdi, JSON belgeleri olarak döndürülür. Yapının tamamını görebilmeniz için arama gezgini belgeleri JSON olarak döndürür.

3. Ardından, bir arama dizesi girin: `search=river&$count=true`. 

   Bu sorgu, `river` terimi için tam metin aramasını çağırır ve sonuç eşleşen belgelerin sayısını içerir. Binlerce veya milyonlarca belge içeren çok büyük bir dizininiz olduğunda, test senaryolarında eşleşen belge sayısının döndürülmesi yararlı olur. Bizim örneğimizde, sorguyla eşleşen tek bir belge vardır.

4. Son olarak, JSON çıkışını ilgilendiğiniz alanlarla sınırlandıran bir arama dizesi girin: `search=river&$count=true&$select=hotelId, baseRate, description`. 

   Sorgu yanıtı seçili alanlara daraltılır ve sonuçta daha kısa bir çıkış elde edilir.

## <a name="view-indexer-configuration"></a>Dizin oluşturucu yapılandırmasını görüntüleme

Az önce programlama yoluyla oluşturduğunuz dizin oluşturucu da dahil olmak üzere tüm dizin oluşturucular portalda listelenir. Dizin oluşturucu tanımını açabilir ve veri kaynağını görüntüleyebilir veya yeni ve değiştirilmiş satırları seçmek için bir yenileme zamanlaması yapılandırabilirsiniz.

1. [Sign in to the Azure portal](https://portal.azure.com/), and in your search service **Overview** page, click the links for **Indexes**, **Indexers**, and **Data Sources**.
3. Select individual objects to view or modify configuration settings.

   ![Dizin oluşturucu ve veri kaynağı kutucukları](./media/search-indexer-tutorial/tiles-portal.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

The fastest way to clean up after a tutorial is by deleting the resource group containing the Azure Cognitive Search service. Kaynak grubunu silerek içindeki her şeyi kalıcı olarak silebilirsiniz. In the portal, the resource group name is on the Overview page of Azure Cognitive Search service.

## <a name="next-steps"></a>Sonraki adımlar

You can attach AI enrichment algorithms to an indexer pipeline. Sonraki adım olarak, aşağıdaki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Blob Depolama’da Belgelerin Dizinini Oluşturma](search-howto-indexing-azure-blob-storage.md)