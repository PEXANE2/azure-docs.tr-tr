---
title: 'Öğretici: Azure SQL veritabanlarından C# veri dizini oluştur'
titleSuffix: Azure Cognitive Search
description: Bu C# ÖĞRETICIDE Azure SQL veritabanı 'na bağlanın, aranabilir verileri ayıklayın ve Azure bilişsel arama dizinine yükleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/26/2020
ms.openlocfilehash: 978587b68e719b79db31ff25adaf2b38d2235095
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650119"
---
# <a name="tutorial-index-azure-sql-data-in-c-using-azure-cognitive-search-indexers"></a>Öğretici: Azure Bilişsel Arama Dizinleyicileri C# kullanarak Azure SQL verilerinin dizinini oluştur

Kullanarak C#, Azure SQL veritabanı 'nda aranabilir verileri çıkaran ve bir arama dizinine gönderen bir [Dizin Oluşturucu](search-indexer-overview.md) yapılandırın. Bu öğretici, aşağıdaki görevleri gerçekleştirmek için [Azure bilişsel arama .NET istemci kitaplıklarını](https://aka.ms/search-sdk) ve bir .NET Core konsol uygulamasını kullanır:

> [!div class="checklist"]
> * Azure SQL veritabanı 'na bağlanan bir veri kaynağı oluşturma
> * Dizin Oluşturucu yapılandırma
> * Dizine veri yüklemek için Dizin Oluşturucu çalıştırma
> * Bir dizini doğrulama adımı olarak sorgulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

+ [Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/)
+ [Visual Studio](https://visualstudio.microsoft.com/downloads/)
+ [Mevcut bir arama hizmeti](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) [oluşturun](search-create-service-portal.md) veya bulun 

> [!Note]
> Bu öğretici için ücretsiz hizmeti kullanabilirsiniz. Ücretsiz arama hizmeti, sizi üç Dizin, üç Dizin Oluşturucu ve üç veri kaynağı ile sınırlandırır. Bu öğreticide hepsinden birer tane oluşturulur. Başlamadan önce, hizmetinize yeni kaynakları kabul etmek için yeriniz olduğundan emin olun.

## <a name="download-source-code"></a>Kaynak kodunu indir

Bu öğreticinin kaynak kodu, [Azure-Samples/Search-DotNet-Başlarken](https://github.com/Azure-Samples/search-dotnet-getting-started) GitHub deposundaki [Dotnethowtoındexer](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToIndexers) klasöründedir.

## <a name="get-a-key-and-url"></a>Anahtar ve URL al

API çağrıları, hizmet URL 'SI ve erişim anahtarı gerektirir. Her ikisiyle de bir arama hizmeti oluşturulur. bu nedenle, aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

   ![HTTP uç noktası ve erişim anahtarı al](media/search-get-started-postman/get-url-key.png "HTTP uç noktası ve erişim anahtarı al")

## <a name="set-up-connections"></a>Bağlantıları ayarlama

1. Visual Studio 'Yu başlatın ve **Dotnethowtoındexers. sln**'yi açın.

1. Çözüm Gezgini, **appSettings. JSON** ' u açın ve yer tutucu değerlerini arama hizmetinize bağlantı bilgileriyle değiştirin. Tam URL "https://my-demo-service.search.windows.net" ise, sağlanacak hizmet adı "My-demo-Service" dir.

    ```json
    {
      "SearchServiceName": "Put your search service name here",
      "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
      "AzureSqlConnectionString": "Put your Azure SQL database connection string here",
    }
    ```

Son giriş için mevcut bir veritabanı gerekir. Bunu bir sonraki adımda oluşturacaksınız.

## <a name="prepare-sample-data"></a>Örnek verileri hazırlama

Bu adımda, bir dizin oluşturucunun gezinebileceği Azure SQL veritabanı 'nda bir dış veri kaynağı oluşturun. Azure SQL Veritabanı'nda veri kümesini oluşturmak için Azure Portal'ı ve örnekteki *hotels.sql* dosyasını kullanabilirsiniz. Azure Bilişsel Arama, bir görünümden veya sorgudan oluşturulan bir gibi düzleştirilmiş satır kümeleri kullanır. Örnek çözümdeki SQL dosyası tek bir tablo oluşturur ve doldurur.

Mevcut bir Azure SQL veritabanı kaynağınız varsa, bu, 4. adımdan başlayarak bu tabloya oteller tablosunu ekleyebilirsiniz.

1. [Azure Portal oturum açın](https://portal.azure.com/).

1. Bir **SQL veritabanı**bulun veya oluşturun. Varsayılan değerleri ve en düşük düzey fiyatlandırma katmanını kullanabilirsiniz. Sunucu oluşturmanın bir avantajı, yönetici kullanıcı adı ve parolası belirtebilmenizdir; çünkü sonraki adımda tabloları oluşturmak ve yüklemek için bunlar gerekecektir.

   ![Yeni veritabanı sayfası](./media/search-indexer-tutorial/indexer-new-sqldb.png "Yeni veritabanı sayfası")

1. Yeni sunucuyu ve veritabanını dağıtmak için **gözden geçir + oluştur** ' a tıklayın. Sunucu ve veritabanının dağıtılmasını bekleyin.

1. Gezinti bölmesinde **sorgu Düzenleyicisi (Önizleme)** öğesine tıklayın ve sunucu yöneticisinin Kullanıcı adını ve parolasını girin. 

   Erişim reddedilirse, istemci IP adresini hata iletisinden kopyalayın ve ardından, istemci bilgisayarınızdan aralığa erişim sağlayan bir kural eklemek için **sunucu güvenlik duvarı ayarla** bağlantısına tıklayın. Kuralın etkili olması birkaç dakika sürebilir.

1. Sorgu Düzenleyicisi 'nde **sorgu aç** ' a tıklayın ve yerel bilgisayarınızdaki *oteller. SQL* dosyasının konumuna gidin. 

1. Dosyayı seçin ve **Aç**'a tıklayın. Betik aşağıdaki ekran görüntüsüne benzer görünmelidir:

   ![SQL betiği](./media/search-indexer-tutorial/sql-script.png "SQL betiği")

1. Sorguyu yürütmek için **Çalıştır**'a tıklayın. Sonuçlar bölmesinde, 3 satır için sorgu başarılı oldu iletisini görmeniz gerekir.

1. Bu tablodan bir satır kümesi döndürmek için, doğrulama adımı olarak aşağıdaki sorguyu yürütebilirsiniz:

    ```sql
    SELECT * FROM Hotels
    ```

1. Veritabanı için ADO.NET bağlantı dizesini kopyalayın. **Ayarlar** > **bağlantı dizeleri**altında, aşağıdaki örneğe benzer şekilde ADO.NET bağlantı dizesini kopyalayın.

    ```sql
    Server=tcp:{your_dbname}.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
    ```

1. Bağlantı dizesini, Visual Studio'da **appsettings.json** dosyasındaki üçüncü girdi olarak "AzureSqlConnectionString" içine yapıştırın.

    ```json
    {
      "SearchServiceName": "<placeholder-Azure-Search-service-name>",
      "SearchServiceAdminApiKey": "<placeholder-admin-key-for-Azure-Search>",
      "AzureSqlConnectionString": "Server=tcp:{your_dbname}.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
    }
    ```

1. **AppSettings. JSON** dosyasındaki bağlantı dizesine parolanızı girin. Veritabanı ve Kullanıcı adları bağlantı dizeniz üzerine kopyalanacak, ancak parolanın el ile girilmesi gerekir.

## <a name="build-the-solution"></a>Çözümü derleme

Çözümü derlemek için F5 tuşuna basın. Program hata ayıklama modunda yürütülür. Konsol penceresinde her işlemin durumu bildirilir.

   ![Konsol çıkışı](./media/search-indexer-tutorial/console-output.png "Konsol çıktısı")

Kodunuz Visual Studio 'da yerel olarak çalışarak Azure 'da arama hizmetinize bağlanarak Azure SQL veritabanına bağlanır ve veri kümesini alır. Bu çok işlem sayesinde birkaç olası hata noktası vardır. Bir hata alırsanız, önce aşağıdaki koşulları kontrol edin:

+ Sağladığınız arama hizmeti bağlantı bilgileri, bu öğreticide hizmet adıyla sınırlıdır. Tam URL girdiyseniz, dizin oluşturma aşamasında bağlantı hatasıyla işlemler durdurulur.

+ **Appsettings.json** dosyasındaki veritabanı bağlantı bilgileri. Bu, portaldan alınmış ve veritabanınız için geçerli kullanıcı adı ve parolayı içerecek şekilde değiştirilmiş ADO.NET bağlantı dizesi olmalıdır. Kullanıcı hesabının verileri alma izni olmalıdır. Yerel istemci IP adresiniz izin verilen erişim iznine sahip olmalıdır.

+ Kaynak sınırları. Ücretsiz katmanın 3 dizin, Dizin Oluşturucu ve veri kaynağı sınırlarına sahip olduğunu hatırlayın. Sınırına dayanmış bir hizmet yeni nesneler oluşturamaz.

## <a name="check-results"></a>Sonuçları denetle

Nesne oluşturmayı doğrulamak için Azure portal kullanın ve ardından dizini sorgulamak için **Arama Gezgini** 'ni kullanın.

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında, nesnenin oluşturulduğunu doğrulamak için her listeyi sırayla açın. **Dizinler**, **Dizin oluşturucular**ve **veri kaynakları** sırasıyla "oteller", "Azure-SQL-Indexer" ve "Azure-SQL" olacaktır.

   ![Dizin oluşturucu ve veri kaynağı kutucukları](./media/search-indexer-tutorial/tiles-portal.png)

1. Oteller dizinini seçin. Oteller sayfasında, **Arama Gezgini** ilk sekmedir. 

1. Boş bir sorgu vermek için **Ara** ' ya tıklayın. 

   Dizininizdeki üç girdi, JSON belgeleri olarak döndürülür. Yapının tamamını görebilmeniz için arama gezgini belgeleri JSON olarak döndürür.

   ![Dizin sorgulama](./media/search-indexer-tutorial/portal-search.png "Dizin sorgulama")
   
1. Ardından, bir arama dizesi girin: `search=river&$count=true`. 

   Bu sorgu, `river` terimi için tam metin aramasını çağırır ve sonuç eşleşen belgelerin sayısını içerir. Binlerce veya milyonlarca belge içeren çok büyük bir dizininiz olduğunda, test senaryolarında eşleşen belge sayısının döndürülmesi yararlı olur. Bizim örneğimizde, sorguyla eşleşen tek bir belge vardır.

1. Son olarak, JSON çıkışını ilgilendiğiniz alanlarla sınırlandıran bir arama dizesi girin: `search=river&$count=true&$select=hotelId, baseRate, description`. 

   Sorgu yanıtı seçili alanlara daraltılır ve sonuçta daha kısa bir çıkış elde edilir.

## <a name="explore-the-code"></a>Kodu keşfet

Örnek kodun ne yaptığını anladığınıza göre, kodu gözden geçirmek için çözüme geri dönelim. İlgili kod iki dosyada bulunur:

  + dizini tanımlayan bir şema içeren **Hotel.cs**
  + **Program.cs**, hizmetinizdeki yapıları oluşturma ve yönetme işlevlerini içerir

### <a name="in-hotelcs"></a>Hotel.cs dosyasında

Dizin şeması, aşağıdaki HotelName alan tanımında gösterildiği gibi alanın tam metin araması yapılabilir, filtrelenebilir veya sıralanabilir olup olmadığı gibi izin verilen işlemleri belirten öznitelikler de dahil olmak üzere alan koleksiyonunu tanımlar. 

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
```

Şema başka öğeler, örneğin arama puanını artırmak için puanlama profilleri, özel çözümleyiciler ve başka yapılar içerebilir. Öte yandan, bizim amaçlarımıza uygun olarak şema yalnızca örnek veri kümelerinde bulunan alanlarla seyrek bir şekilde tanımlanmıştır.

### <a name="in-programcs"></a>Program.cs dosyasında

Ana program, bir istemci, dizin, veri kaynağı ve Dizin Oluşturucu oluşturma mantığını içerir. Kod, bu programı birden çok kez çalıştırabileceğiniz varsayımıyla, aynı adı taşıyan kaynakları denetler ve siler.

Veri kaynağı nesnesi, Azure SQL 'in yerleşik [değişiklik algılama özelliklerinden](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) yararlanmak için [kısmi veya artımlı dizin oluşturma](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) da dahIl olmak üzere Azure SQL veritabanı kaynaklarına özgü ayarlarla yapılandırılır. Azure SQL 'deki demo oteller veritabanının **IsDeleted**adlı "geçici silme" sütunu vardır. Bu sütun veritabanında true olarak ayarlandığında, Dizin Oluşturucu ilgili belgeyi Azure Bilişsel Arama dizininden kaldırır.

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

Dizin Oluşturucu nesnesi, yapılandırma, zamanlama ve çağrının kaynağa bakılmaksızın aynı olduğu platformdan bağımsız bir platformdur. Bu örnek Dizin Oluşturucu bir zamanlama, Dizin Oluşturucu geçmişini temizleyen bir sıfırlama seçeneği içerir ve Dizin oluşturucuyu hemen oluşturmak ve çalıştırmak için bir yöntem çağırır.

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

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir projenin sonunda kendi aboneliğinizde çalışırken, artık ihtiyaç duyulmadığınızda kaynakları kaldırmak iyi bir fikirdir. Çalışan kaynaklar sizin için ücret verebilir. Kaynakları tek tek silebilir veya kaynak grubunu silerek tüm kaynak kümesini silebilirsiniz.

Sol gezinti bölmesindeki tüm kaynaklar veya kaynak grupları bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Bilişsel Arama 'de, Dizin oluşturucular birden çok Azure veri kaynağı için kullanılabilir. Sonraki adım olarak, Azure Blob depolama için Dizin oluşturucuyu keşfedebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Blob Depolama’da Belgelerin Dizinini Oluşturma](search-howto-indexing-azure-blob-storage.md)