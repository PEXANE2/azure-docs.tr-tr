---
title: Toplu alma ve güncelleştirme işlemleri için Azure Cosmos DB'deki toplu yürütme .NET kitaplığını kullanma
description: Toplu alma ve toplu yürütme .NET kitaplığı kullanarak Azure Cosmos DB belgeleri güncelleştirme.
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: ramkris
ms.reviewer: sngun
ms.openlocfilehash: 40ef05107f20a3396f6710f894a2dbad2d7fa6c9
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478853"
---
# <a name="use-the-bulk-executor-net-library-to-perform-bulk-operations-in-azure-cosmos-db"></a>Azure Cosmos DB'de toplu işlemler gerçekleştirmek için toplu yürütme .NET kitaplığını kullanın

> [!NOTE]
> Bu makalede açıklanan bu toplu yürütme kitaplığı .NET SDK 2.x sürümü kullanılarak uygulamalar için korunur. Yeni uygulamalar [için,.NET SDK sürüm 3.x](tutorial-sql-api-dotnet-bulk-import.md) ile doğrudan kullanılabilen **toplu desteği** kullanabilirsiniz ve herhangi bir dış kitaplık gerektirmez. 

> Şu anda toplu yürütücü kitaplığını kullanıyorsanız ve yeni SDK'da toplu desteğe geçiş yapmayı planlıyorsanız, uygulamanızı geçirmek için [Geçiş kılavuzundaki](how-to-migrate-from-bulk-executor-library.md) adımları kullanın.

Bu öğretici, belgeleri bir Azure Cosmos kapsayıcısına almak ve güncelleştirmek için toplu yürütme .NET kitaplığını kullanma yla ilgili yönergeler sağlar. Toplu yürütme kitaplığı ve büyük iş artışı ve depolamadan yararlanmanıza nasıl yardımcı olduğu hakkında bilgi edinmek [için, toplu yürütme alanı genel bakış](bulk-executor-overview.md) makalesine bakın. Bu öğreticide, rasgele oluşturulan belgeleri bir Azure Cosmos kapsayıcısına toplu olarak içe aktarmayı sağlayan bir örnek .NET uygulaması görürsünüz. İçe aktarmadan sonra, belirli belge alanlarında gerçekleştirecek işlemler olarak yamaları belirterek içe aktarılan verileri nasıl toplu olarak güncelleştirebileceğinizi gösterir.

Şu anda toplu yürütme kitaplığı yalnızca Azure Cosmos DB SQL API ve Gremlin API hesapları tarafından desteklenir. Bu makalede, SQL API hesapları ile toplu yürütme .NET kitaplığı nasıl kullanılacağı açıklanmaktadır. Gremlin API hesaplarıyla toplu yürütme .NET kitaplığını kullanma hakkında bilgi edinmek için Azure [Cosmos DB Gremlin API'de toplu işlemler gerçekleştirin'e](bulk-executor-graph-dotnet.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio 2019 yüklü değilseniz, [Visual Studio 2019 Community Edition'ı](https://www.visualstudio.com/downloads/)indirebilir ve kullanabilirsiniz. Visual Studio kurulumu sırasında "Azure geliştirme"yi etkinleştirdiğinizden emin olun.

* Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) bir hesap oluşturun.

* [Azure Cosmos DB’yi ücretsiz olarak](https://azure.microsoft.com/try/cosmosdb/) bir Azure aboneliği olmadan, ücretsiz ve herhangi bir taahhütte bulunmadan deneyebilirsiniz. Veya, son nokta yla [Azure Cosmos DB emülatörü](https://docs.microsoft.com/azure/cosmos-db/local-emulator) kullanabilirsiniz. `https://localhost:8081` Birincil Anahtar, [Kimlik doğrulama istekleri](local-emulator.md#authenticating-requests) bölümünde sağlanır.

* .NET quickstart makalesinin veritabanı hesabı oluşturma bölümünde açıklanan adımları kullanarak bir Azure Cosmos DB SQL API hesabı [oluşturun.](create-sql-api-dotnet.md#create-account)

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir örnek .NET uygulaması indirerek kodla çalışmaya geçelim. Bu uygulama, Azure Cosmos hesabınızda depolanan veriler üzerinde toplu işlemler gerçekleştirir. Uygulamayı klonlamak için bir komut istemi açın, kopyalamak istediğiniz dizine gidin ve aşağıdaki komutu çalıştırın:

```
git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started.git
```

Klonlanmış depo iki örnek "BulkImportSample" ve "BulkUpdateSample" içerir. Örnek uygulamalardan birini açabilir, App.config dosyasındaki bağlantı dizelerini Azure Cosmos DB hesabınızın bağlantı dizeleriyle güncelleyebilir, çözüm oluşturabilir ve çalıştırabilirsiniz.

"BulkImportSample" uygulaması rasgele belgeler oluşturur ve bunları Azure Cosmos hesabınıza toplu olarak aktarın. "BulkUpdateSample" uygulaması, belirli belge alanlarında gerçekleştirilen işlemler olarak yamaları belirterek, içe aktarılan belgeleri toplu olarak günceller. Sonraki bölümlerde, bu örnek uygulamaların her birinde kodu gözden geçireceksiniz.

## <a name="bulk-import-data-to-an-azure-cosmos-account"></a>Azure Cosmos hesabına toplu veri alma

1. "BulkImportSample" klasörüne gidin ve "BulkImportSample.sln" dosyasını açın.  

2. Azure Cosmos DB'nin bağlantı dizeleri, aşağıdaki kodda gösterildiği gibi App.config dosyasından alınır:  

   ```csharp
   private static readonly string EndpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
   private static readonly string AuthorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];
   private static readonly string DatabaseName = ConfigurationManager.AppSettings["DatabaseName"];
   private static readonly string CollectionName = ConfigurationManager.AppSettings["CollectionName"];
   private static readonly int CollectionThroughput = int.Parse(ConfigurationManager.AppSettings["CollectionThroughput"]);
   ```

   Toplu içe aktarıcı, yeni bir veritabanı ve veritabanı adı, kapsayıcı adı ve App.config dosyasında belirtilen iş gücü değerlerini içeren bir kapsayıcı oluşturur.

3. Sonraki DocumentClient nesnesi Doğrudan TCP bağlantı modu ile başharfe işlenir:  

   ```csharp
   ConnectionPolicy connectionPolicy = new ConnectionPolicy
   {
      ConnectionMode = ConnectionMode.Direct,
      ConnectionProtocol = Protocol.Tcp
   };
   DocumentClient client = new DocumentClient(new Uri(endpointUrl),authorizationKey,
   connectionPolicy)
   ```

4. BulkExecutor nesnesi bekleme süresi ve daraltılmış istekler için yüksek yeniden deneme değeri ile baş harfe olarak verilir. Ve sonra 0 için bulkExecutor için ömür boyu tıkanıklık kontrolü geçmek için ayarlanır.  

   ```csharp
   // Set retry options high during initialization (default values).
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 30;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 9;

   IBulkExecutor bulkExecutor = new BulkExecutor(client, dataCollection);
   await bulkExecutor.InitializeAsync();

   // Set retries to 0 to pass complete control to bulk executor.
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 0;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 0;
   ```

5. Uygulama BulkImportAsync API çağırır. .NET kitaplığı, biri serileştirilmiş JSON belgelerinin listesini kabul eden diğeri de deserialized POCO belgelerinin listesini kabul eden toplu alma API'sinin iki aşırı yüklenir. Bu aşırı yüklü yöntemlerin tanımları hakkında daha fazla bilgi edinmek için [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkimportasync?view=azure-dotnet)bakın.

   ```csharp
   BulkImportResponse bulkImportResponse = await bulkExecutor.BulkImportAsync(
     documents: documentsToImportInBatch,
     enableUpsert: true,
     disableAutomaticIdGeneration: true,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```
   **BulkImportAsync yöntemi aşağıdaki parametreleri kabul eder:**
   
   |**Parametre**  |**Açıklama** |
   |---------|---------|
   |enableUpsert    |   Belgelerde işlemleri etkinleştirmek için bir bayrak. Verilen kimliği olan bir belge zaten varsa, güncelleştirilir. Varsayılan olarak, yanlış olarak ayarlanır.      |
   |devre dışı AutomaticIdGeneration    |    Otomatik kimlik oluşturmayı devre dışı kıvmak için bir bayrak. Varsayılan olarak, doğru olarak ayarlanır.     |
   |maxConcurrencyPerPartitionKeyRange    | Bölüm anahtar aralığı başına maksimum eşzamanlılık derecesi, null ayarı kitaplık 20 varsayılan değeri kullanmasına neden olur. |
   |maxInMemorySortingBatchSize     |  Her aşamada API çağrısına geçirilen belge numaralandırıcısından çekilen maksimum belge sayısı. Toplu içe aktarmadan önce gerçekleşen bellek içi sıralama aşaması için, bu parametreyi null olarak ayarlamak kitaplığın varsayılan minimum değeri kullanmasına neden olur (documents.count, 1000000).       |
   |Cancellationtoken    |    Toplu alma işleminden zarif bir şekilde çıkmak için iptal belirteci.     |

   **Toplu alma yanıtı nesne s›n›r›m›** Toplu alma API çağrısının sonucu aşağıdaki öznitelikleri içerir:

   |**Parametre**  |**Açıklama**  |
   |---------|---------|
   |NumberOfDocumentsİtlese (uzun)   |  Toplu alma API çağrısına sağlanan toplam belgeden başarıyla alınan toplam belge sayısı.       |
   |TotalRequestUnitsConsumed (çift)   |   Toplu alma API çağrısı tarafından tüketilen toplam istek birimleri (RU).      |
   |Toplam Zaman (TimeSpan)    |   Toplu alma API'si tarafından alınan toplam süre yürütmeyi tamamlamak için çağrı.      |
   |BadInputDocuments (Liste\<nesnesi>)   |     Toplu alma API çağrısında başarıyla alınmayan kötü biçimli belgelerin listesi. Döndürülen belgeleri düzeltin ve içe aktarmayı yeniden deneyin. Kötü biçimlendirilmiş belgeler, kimlik değeri dize olmayan belgeleri içerir (null veya başka bir veri türü geçersiz kabul edilir).    |

## <a name="bulk-update-data-in-your-azure-cosmos-account"></a>Azure Cosmos hesabınızdaki toplu güncelleme verileri

Varolan belgeleri BulkUpdateAsync API'sını kullanarak güncelleştirebilirsiniz. Bu örnekte, `Name` alanı yeni bir değere ayarlar `Description` ve alanı varolan belgelerden kaldırırsınız. Desteklenen güncelleştirme işlemlerinin tam kümesi için [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet)bakın.

1. "BulkUpdateSample" klasörüne gidin ve "BulkUpdateSample.sln" dosyasını açın.  

2. Güncelleştirme öğelerini ilgili alan güncelleştirme işlemleriyle birlikte tanımlayın. `SetUpdateOperation` Bu örnekte, `Name` alanı güncelleştirmek ve `UnsetUpdateOperation` `Description` alanı tüm belgelerden kaldırmak için kullanırsınız. Ayrıca, belge alanını belirli bir değerle artırarak, belirli değerleri bir dizi alanına itme veya belirli bir değeri bir dizi alanından kaldırma gibi diğer işlemleri de gerçekleştirebilirsiniz. Toplu güncelleştirme API'si tarafından sağlanan farklı yöntemler hakkında bilgi edinmek için [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet)bakın.

   ```csharp
   SetUpdateOperation<string> nameUpdate = new SetUpdateOperation<string>("Name", "UpdatedDoc");
   UnsetUpdateOperation descriptionUpdate = new UnsetUpdateOperation("description");

   List<UpdateOperation> updateOperations = new List<UpdateOperation>();
   updateOperations.Add(nameUpdate);
   updateOperations.Add(descriptionUpdate);

   List<UpdateItem> updateItems = new List<UpdateItem>();
   for (int i = 0; i < 10; i++)
   {
    updateItems.Add(new UpdateItem(i.ToString(), i.ToString(), updateOperations));
   }
   ```

3. Uygulama BulkUpdateAsync API çağırır. BulkUpdateAsync yönteminin tanımı hakkında bilgi edinmek için [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.ibulkexecutor.bulkupdateasync?view=azure-dotnet)bakın.  

   ```csharp
   BulkUpdateResponse bulkUpdateResponse = await bulkExecutor.BulkUpdateAsync(
     updateItems: updateItems,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```  
   **BulkUpdateAsync yöntemi aşağıdaki parametreleri kabul eder:**

   |**Parametre**  |**Açıklama** |
   |---------|---------|
   |maxConcurrencyPerPartitionKeyRange    |   Bu parametreyi null olarak ayarlayarak, bölüm anahtar aralığı başına maksimum eşzamanlılık derecesi, kitaplığın varsayılan değeri kullanmasını sağlar(20).   |
   |maxInMemorySortingBatchSize    |    Güncelleştirme öğeleri numaralandırıcıdan çıkarılan maksimum güncelleştirme öğesi sayısı her aşamada API çağrısına aktarılır. Toplu güncelleştirmeden önce gerçekleşen bellek içi sıralama aşaması için, bu parametreyi null olarak ayarlamak kitaplığın varsayılan minimum değeri kullanmasına neden olur (updateItems.count, 1000000).     |
   | Cancellationtoken|Toplu güncelleştirme işleminden zarif bir şekilde çıkmak için iptal belirteci. |

   **Toplu güncelleştirme yanıtı nesne tanımı** Toplu güncelleştirme API çağrısının sonucu aşağıdaki öznitelikleri içerir:

   |**Parametre**  |**Açıklama** |
   |---------|---------|
   |NumberOfDocumentsUpdated (uzun)    |   Toplu güncelleştirme API çağrısına sağlanan toplam belgeden başarıyla güncelleştirilen belge sayısı.      |
   |TotalRequestUnitsConsumed (çift)   |    Toplu güncelleştirme API çağrısı tarafından tüketilen toplam istek birimleri (RUS).    |
   |Toplam Zaman (TimeSpan)   | Toplu güncelleştirme API tarafından alınan toplam süre yürütmeyi tamamlamak için çağrı. |
    
## <a name="performance-tips"></a>Performans ipuçları 

Toplu yürütme kitaplığını kullanırken daha iyi performans için aşağıdaki noktaları göz önünde bulundurun:

* En iyi performans için, uygulamanızı Azure Cosmos hesabınızın yazma bölgesiyle aynı bölgede bulunan bir Azure sanal makinesinden çalıştırın.  

* Belirli bir Azure Cosmos kapsayıcısına `BulkExecutor` karşılık gelen tek bir sanal makine içinde tüm uygulama için tek bir nesneyi anında göndermeniz önerilir.  

* Tek bir toplu işlem API yürütme istemci makinenin CPU ve ağ IO büyük bir yığın tüketir beri (Bu dahili olarak birden çok görev yumurtlama tarafından olur). Toplu işlem API çağrılarını yürüten uygulama sürecinizde birden çok eşzamanlı görev inmekten kaçının. Tek bir sanal makinede çalışan tek bir toplu işlem API çağrısı, konteynerin tüm masını tüketemiyorsa (kabınızın iş bolması > 1 milyon RU/s) toplu işlem API çağrılarını aynı anda yürütmek için ayrı sanal makineler oluşturmak tercih edilir.  

* Hedef `InitializeAsync()` Cosmos konteynerinin bölüm eşlemesini almak için bir BulkExecutor nesnesini anında aldıktan sonra yöntemin çağrıldığından emin olun.  

* Uygulamanızın App.Config'inde, **gcServer'ın** daha iyi performans için etkinleştirildiğinden emin olun
  ```xml  
  <runtime>
    <gcServer enabled="true" />
  </runtime>
  ```
* Kitaplık, bir günlük dosyasında veya konsolda toplanabilecek izler yayır. Her ikisini de etkinleştirmek için, uygulamanızın App.Config dosyasına aşağıdaki kodu ekleyin.

  ```xml
  <system.diagnostics>
    <trace autoflush="false" indentsize="4">
      <listeners>
        <add name="logListener" type="System.Diagnostics.TextWriterTraceListener" initializeData="application.log" />
        <add name="consoleListener" type="System.Diagnostics.ConsoleTraceListener" />
      </listeners>
    </trace>
  </system.diagnostics>
  ```

## <a name="next-steps"></a>Sonraki adımlar

* Nuget paket ayrıntıları ve sürüm notları hakkında bilgi edinmek için [toplu uygulayıcı SDK ayrıntılarına](sql-api-sdk-bulk-executor-dot-net.md)bakın.
