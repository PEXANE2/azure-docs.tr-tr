---
title: "Öğretici: C'deki Azure SQL veritabanlarından gelen dizin verileri # "
titleSuffix: Azure Cognitive Search
description: Bu C# öğreticisinde, Azure SQL veritabanına bağlanın, aranabilir verileri ayıklayın ve azure bilişsel arama dizinine yükleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/28/2020
ms.openlocfilehash: 7660c89032ea3ef8371655b94b75c1f60603ee32
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78193977"
---
# <a name="tutorial-use-c-to-index-data-from-sql-databases-in-azure-cognitive-search"></a>Öğretici: Azure Bilişsel Arama'da SQL veritabanlarındaki verileri dizine almak için C# kullanın

Bir [dizin leyiciyi](search-indexer-overview.md) Azure SQL veritabanından aranabilir verileri ayıklamak için yapılandırın ve Azure Bilişsel Arama'da bir arama dizinine gönderin. 

Bu öğretici, aşağıdaki görevleri gerçekleştirmek için C# ve [.NET SDK'yı](https://aka.ms/search-sdk) kullanır:

> [!div class="checklist"]
> * Azure SQL Veritabanı'na bağlanan bir veri kaynağı oluşturma
> * Dizin oluşturucu oluşturma
> * Verileri bir dizine yüklemek için bir dizin oluşturma
> * Bir dizini doğrulama adımı olarak sorgula

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

+ [Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/)
+ [Visual Studio](https://visualstudio.microsoft.com/downloads/)
+ [Varolan bir arama hizmeti](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) [oluşturma](search-create-service-portal.md) veya bulma 

> [!Note]
> Bu öğretici için ücretsiz hizmeti kullanabilirsiniz. Ücretsiz bir arama hizmeti sizi üç dizin, üç dizin leyici ve üç veri kaynağıyla sınırlar. Bu öğreticide hepsinden birer tane oluşturulur. Başlamadan önce, yeni kaynakları kabul etmek için hizmetinizde yer olduğundan emin olun.

## <a name="download-files"></a>Dosyaları indirme

Bu öğreticinin kaynak [kodu, Azure Örnekleri/search-dotnet-getting-started](https://github.com/Azure-Samples/search-dotnet-getting-started) GitHub deposundaki [DotNetHowToIndexer](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToIndexers) klasöründe yer almaktadır.

## <a name="1---create-services"></a>1 - Hizmet oluşturma

Bu öğretici, dizin oluşturma ve sorgular için Azure Bilişsel Arama'yı ve dış veri kaynağı olarak Azure SQL Veritabanı'nı kullanır. Mümkünse, yakınlık ve yönetilebilirlik için aynı bölgede hem de kaynak grubunda hem hizmetleri oluşturun. Uygulamada, Azure SQL Veritabanı herhangi bir bölgede olabilir.

### <a name="start-with-azure-sql-database"></a>Azure SQL Veritabanı ile başlayın

Bu adımda, Azure SQL Veritabanı'nda bir dizin oluşturucunun tarayabileceği bir dış veri kaynağı oluşturun. Azure SQL Veritabanı'nda veri kümesini oluşturmak için örnek indirmeden Azure portalını ve *hotels.sql* dosyasını kullanabilirsiniz. Azure Bilişsel Arama, görünüm veya sorgudan oluşturulan sıra kümeleri gibi düzleştirilmiş satır kümelerini tüketir. Örnek çözümdeki SQL dosyası tek bir tablo oluşturur ve doldurur.

Varolan bir Azure SQL Veritabanı kaynağınız varsa, 4.

1. [Azure portalında oturum açın.](https://portal.azure.com/)

1. Bir SQL **Veritabanı**bulun veya oluşturun. Varsayılan değerleri ve en düşük düzey fiyatlandırma katmanını kullanabilirsiniz. Sunucu oluşturmanın bir avantajı, yönetici kullanıcı adı ve parolası belirtebilmenizdir; çünkü sonraki adımda tabloları oluşturmak ve yüklemek için bunlar gerekecektir.

   ![Yeni veritabanı sayfası](./media/search-indexer-tutorial/indexer-new-sqldb.png "Yeni veritabanı sayfası")

1. Yeni **sunucuyu** ve veritabanını dağıtmak için Gözden Geçir + oluştur'u tıklatın. Sunucu ve veritabanının dağıtılmasını bekleyin.

1. Gezinti bölmesine Sorgu **düzenleyicisini (önizleme)** tıklatın ve sunucu yöneticisinin kullanıcı adını ve parolasını girin. 

   Erişim reddedilirse, istemci IP adresini hata iletisinden kopyalayın ve ardından istemci bilgisayarınızdan erişime izin veren bir kural eklemek için sunucu **güvenlik duvarı** bağlantısını tıklatın. Kuralın etkili olması birkaç dakika sürebilir.

1. Sorgu düzenleyicisinde **Sorguyu Aç'ı** tıklatın ve yerel bilgisayarınızdaki *hotels.sql* dosyasının konumuna gidin. 

1. Dosyayı seçin ve **Aç**'a tıklayın. Betik aşağıdaki ekran görüntüsüne benzer görünmelidir:

   ![SQL betiği](./media/search-indexer-tutorial/sql-script.png "SQL betiği")

1. Sorguyu yürütmek için **Çalıştır**'a tıklayın. Sonuçlar bölmesinde, 3 satır için sorgu başarılı oldu iletisini görmeniz gerekir.

1. Bu tablodan bir satır kümesi döndürmek için, doğrulama adımı olarak aşağıdaki sorguyu yürütebilirsiniz:

    ```sql
    SELECT * FROM Hotels
    ```

1. Veritabanı için ADO.NET bağlantı dizesini kopyalayın. **Ayarlar** > **Bağlantı Dizeleri**altında, aşağıdaki örneğe benzer şekilde ADO.NET bağlantı dizesini kopyalayın.

    ```sql
    Server=tcp:{your_dbname}.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
    ```

Bir sonraki alıştırmada ortamınızı ayarlayarak bu bağlantı dizesini alabilirsiniz.

### <a name="azure-cognitive-search"></a>Azure Bilişsel Arama

Bir sonraki bileşen, [portalda oluşturabileceğiniz](search-create-service-portal.md)Azure Bilişsel Arama'dır. Bu izbiyi tamamlamak için Ücretsiz katmanı kullanabilirsiniz. 

### <a name="get-an-admin-api-key-and-url-for-azure-cognitive-search"></a>Azure Bilişsel Arama için yönetici api anahtarı ve URL'si alın

API aramaları için servis URL'si ve erişim anahtarı gerekir. Her ikisiyle de bir arama hizmeti oluşturulur, bu nedenle aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **Tuşları'nda,** hizmetteki tüm haklar için bir yönetici anahtarı alın. İki değiştirilebilir yönetici anahtarları, bir üzerinde rulo gerekir durumda iş sürekliliği için sağlanan vardır. Nesneleri ekleme, değiştirme ve silme isteklerinde birincil veya ikincil anahtarı kullanabilirsiniz.

   ![Http bitiş noktası ve erişim anahtarı alın](media/search-get-started-postman/get-url-key.png "Http bitiş noktası ve erişim anahtarı alın")

## <a name="2---set-up-your-environment"></a>2 - Ortamınızı ayarlayın

1. Visual Studio'u başlatın ve **DotNetHowToIndexers.sln'yi**açın.

1. Solution Explorer'da bağlantı bilgilerini sağlamak için **appsettings.json'ı** açın.

1. Çünkü, `searchServiceName`tam URL "https://my-demo-service.search.windows.net" ise, sağkılmak üzere hizmet adı "benim demo-hizmetim"dir.

1. Için, `AzureSqlConnectionString`dize biçimi buna benzer:`"Server=tcp:{your_dbname}.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"`

    ```json
    {
      "SearchServiceName": "<placeholder-Azure-Search-service-name>",
      "SearchServiceAdminApiKey": "<placeholder-admin-key-for-Azure-Search>",
      "AzureSqlConnectionString": "<placeholder-ADO.NET-connection-string",
    }
    ```

1. Bağlantı dizesi, bağlantı dizesinin geçerli bir parola içerdiğinden emin olun. Veritabanı ve kullanıcı adları kopyalansa da, parola el ile girilmelidir.

## <a name="3---create-the-pipeline"></a>3 - Boru hattını oluşturun

Dizinleyiciler bir veri kaynağı nesnesi ve dizin gerektirir. İlgili kod iki dosyada bulunur:

  + **hotel.cs**, dizini tanımlayan bir şema içeren
  + **Program.cs**, hizmetinizde yapılar oluşturmak ve yönetmek için işlevler içeren

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

Ana program, istemci, dizin, veri kaynağı ve dizin oluşturucu oluşturmak için mantık içerir. Kod, bu programı birden çok kez çalıştırabileceğiniz varsayımıyla, aynı adı taşıyan kaynakları denetler ve siler.

Veri kaynağı nesnesi, Azure SQL'in yerleşik [değişiklik algılama özelliklerinden](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) yararlanmak için kısmi veya [artımlı dizin oluşturma](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) da dahil olmak üzere Azure SQL veritabanı kaynaklarına özgü ayarlarla yapılandırılır. Azure SQL'deki demo oteller veritabanında **IsDeleted**adlı bir "yumuşak silme" sütunu vardır. Bu sütun veritabanında doğru olarak ayarlandığında, dizinleyici ilgili belgeyi Azure Bilişsel Arama dizininden kaldırır.

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

Dizinleyici nesnesi, kaynakne bakılmaksızın yapılandırmanın, zamanlamanın ve çağırmanın aynı olduğu platform-agnostiktir. Bu örnek dizin oluşturucu, dizin oluşturucu geçmişini temizleyen bir sıfırlama seçeneği ve dizin oluşturup hemen çalıştırmak için bir yöntem çağırır.

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

## <a name="4---build-the-solution"></a>4 - Çözümü oluşturun

Çözümü oluşturmak ve çalıştırmak için F5 tuşuna basın. Program hata ayıklama modunda yürütülür. Konsol penceresinde her işlemin durumu bildirilir.

   ![Konsol çıktısı](./media/search-indexer-tutorial/console-output.png "Konsol çıktısı")

Kodunuz Visual Studio'da yerel olarak çalışır ve Azure'daki arama hizmetinize bağlanır ve bu da Azure SQL Veritabanı'na bağlanır ve veri kümesini alır. Bu kadar çok işlemde, birkaç olası hata noktası vardır. Bir hata alırsanız, önce aşağıdaki koşulları kontrol edin:

+ Sağladığınız arama hizmeti bağlantı bilgileri, bu öğreticide hizmet adıyla sınırlıdır. Tam URL girdiyseniz, dizin oluşturma aşamasında bağlantı hatasıyla işlemler durdurulur.

+ **Appsettings.json** dosyasındaki veritabanı bağlantı bilgileri. Bu, portaldan alınmış ve veritabanınız için geçerli kullanıcı adı ve parolayı içerecek şekilde değiştirilmiş ADO.NET bağlantı dizesi olmalıdır. Kullanıcı hesabının verileri alma izni olmalıdır. Yerel istemci IP adresinize erişim izni verilmelidir.

+ Kaynak sınırları. Serbest katmanın 3 dizin, dizin leyici ve veri kaynağı sınırı olduğunu hatırlayın. Sınırına dayanmış bir hizmet yeni nesneler oluşturamaz.

## <a name="5---search"></a>5 - Arama

Nesne oluşturmayı doğrulamak için Azure portalını kullanın ve dizini sorgulamak için **Arama gezginini** kullanın.

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda, nesnenin oluşturulduğunu doğrulamak için sırayla her listeyi açın. **Dizinler,** **Dizinleyiciler**ve **Veri Kaynakları** sırasıyla "oteller", "azure-sql-indexer" ve "azure-sql" içerir.

   ![Dizin oluşturucu ve veri kaynağı kutucukları](./media/search-indexer-tutorial/tiles-portal.png)

1. Oteller dizinini seçin. Oteller sayfasında, **Arama gezgini** ilk sekmedir. 

1. Boş bir sorgu vermek için **Ara'yı** tıklatın. 

   Dizininizdeki üç girdi, JSON belgeleri olarak döndürülür. Yapının tamamını görebilmeniz için arama gezgini belgeleri JSON olarak döndürür.

   ![Dizin sorgusu](./media/search-indexer-tutorial/portal-search.png "Dizin sorgusu")
   
1. Ardından, bir arama dizesi girin: `search=river&$count=true`. 

   Bu sorgu, `river` terimi için tam metin aramasını çağırır ve sonuç eşleşen belgelerin sayısını içerir. Binlerce veya milyonlarca belge içeren çok büyük bir dizininiz olduğunda, test senaryolarında eşleşen belge sayısının döndürülmesi yararlı olur. Bizim örneğimizde, sorguyla eşleşen tek bir belge vardır.

1. Son olarak, JSON çıkışını ilgilendiğiniz alanlarla sınırlandıran bir arama dizesi girin: `search=river&$count=true&$select=hotelId, baseRate, description`. 

   Sorgu yanıtı seçili alanlara daraltılır ve sonuçta daha kısa bir çıkış elde edilir.

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

Geliştirmenin ilk deneysel aşamalarında, tasarım yinelemesi için en pratik yaklaşım, nesneleri Azure Bilişsel Arama'dan silmek ve kodunuzu yeniden oluşturmasına izin vermektir. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Bu öğreticinin örnek kodu varolan nesneleri denetler ve kodunuzu yeniden çalıştırmak için bunları siler.

Portalı dizinleri, dizin leyicileri ve veri kaynaklarını silmek için de kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda, artık ihtiyacınız olmayan kaynakları kaldırmak iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki Tüm kaynaklar veya Kaynak grupları bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık SQL Veritabanı dizinoluşturmanın temellerini bildiğinize göre, dizinleyici yapılandırmasına daha yakından bakalım.

> [!div class="nextstepaction"]
> [Azure SQL veritabanı dizini yapılandırma](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)