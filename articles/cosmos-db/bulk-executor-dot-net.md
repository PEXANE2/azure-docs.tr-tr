---
title: Azure Cosmos DB 'da toplu alma ve güncelleştirme işlemleri gerçekleştirmek için toplu yürütücü .NET kitaplığı kullanma
description: Toplu yürütücü .NET kitaplığı kullanarak Azure Cosmos DB belgelerini toplu olarak içeri aktarın ve güncelleştirin.
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 09/01/2019
ms.author: ramkris
ms.reviewer: sngun
ms.openlocfilehash: 26e4c4edbcfc282fffd0d43673d3585cfe973848
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70241275"
---
# <a name="use-bulk-executor-net-library-to-perform-bulk-operations-in-azure-cosmos-db"></a>Toplu yürütücü .NET kitaplığı 'nı kullanarak Azure Cosmos DB toplu işlemleri gerçekleştirin

Bu öğretici, Azure Cosmos kapsayıcısına belge aktarmak ve güncelleştirmek için Azure Cosmos DB toplu yürütücü .NET kitaplığını kullanma hakkında yönergeler sağlar. Toplu yürütücü Kitaplığı hakkında bilgi edinmek ve büyük/veya depolama özelliğinden yararlanarak nasıl yardım alabileceğinizi öğrenmek için bkz. [toplu yürütücü kitaplığı genel bakış](bulk-executor-overview.md) makalesi. Bu öğreticide, rastgele oluşturulan belgelerin bir Azure Cosmos kapsayıcısına toplu olarak içe aktardığı örnek bir .NET uygulaması görürsünüz. İçeri aktardıktan sonra, belirli belge alanlarında gerçekleştirilecek işlemler olarak düzeltme eklerini belirterek içeri aktarılan verileri nasıl toplu olarak güncelleşkullanabileceğinizi gösterir. 

Şu anda toplu Yürütücü kitaplığı, Gremlin API hesapları yalnızca Azure Cosmos DB SQL API ile desteklenir. Bu makalede, SQL API hesaplarıyla toplu yürütücü .NET kitaplığı 'nın nasıl kullanılacağı açıklanır. Toplu Yürütücü .NET kitaplığı ile Gremlin API kullanımı hakkında bilgi edinmek için [Azure Cosmos DB Gremlin API'SİNDE toplu işlemler gerçekleştirme](bulk-executor-graph-dotnet.md). 

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio 2019 ' ü henüz yüklemediyseniz [Visual studio 2019 Community Edition](https://www.visualstudio.com/downloads/)' ı indirip kullanabilirsiniz. Visual Studio Kurulumu sırasında Azure geliştirmeyi etkinleştirdiğinizden emin olun.

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun. 

* [Azure Cosmos DB’yi ücretsiz olarak](https://azure.microsoft.com/try/cosmosdb/) bir Azure aboneliği olmadan, ücretsiz ve herhangi bir taahhütte bulunmadan deneyebilirsiniz. Ya da [Azure Cosmos DB öykünücüsünü](https://docs.microsoft.com/azure/cosmos-db/local-emulator) `https://localhost:8081` uç noktayla kullanabilirsiniz. Birincil Anahtar, [Kimlik doğrulama istekleri](local-emulator.md#authenticating-requests) bölümünde sağlanır.

* .NET hızlı başlangıç makalesinin [veritabanı hesabı oluşturma](create-sql-api-dotnet.md#create-account) bölümünde açıklanan adımları kullanarak Azure Cosmos DB BIR SQL API hesabı oluşturun. 

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi, bazı örnek .NET uygulamalarını GitHub 'dan indirerek kodla çalışmaya geçiş yapalım. Bu uygulamalar Azure Cosmos DB veri üzerinde toplu işlemler gerçekleştirir. Uygulamaları kopyalamak için, bir komut istemi açın, kopyalamak istediğiniz dizine gidin ve şu komutu çalıştırın:

```
git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started.git
```

Kopyalanmış depo "Bulkımportsample" ve "BulkUpdateSample" olmak üzere iki örnek içerir. Örnek uygulamalardan birini açabilir, App. config dosyasındaki bağlantı dizelerini Azure Cosmos DB hesabınızın bağlantı dizeleriyle güncelleştirebilir, çözümü derleyebilir ve çalıştırabilirsiniz. 

"Bulkımportsample" uygulaması rastgele belgeler oluşturur ve bunları Azure Cosmos DB Toplu içe aktarır. "BulkUpdateSample" uygulaması, belirli belge alanlarında gerçekleştirilecek işlemler olarak düzeltme eklerini belirterek, içeri aktarılan belgeleri toplu olarak güncelleştirir. Sonraki bölümlerde, bu örnek uygulamaların her birinde kodu gözden geçiyapacaksınız.

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Azure Cosmos DB için toplu verileri içeri aktar

1. "Bulkımportsample" klasörüne gidin ve "Bulkımportsample. sln" dosyasını açın.  

2. Azure Cosmos DB bağlantı dizeleri, aşağıdaki kodda gösterildiği gibi App. config dosyasından alınır:  

   ```csharp
   private static readonly string EndpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
   private static readonly string AuthorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];
   private static readonly string DatabaseName = ConfigurationManager.AppSettings["DatabaseName"];
   private static readonly string CollectionName = ConfigurationManager.AppSettings["CollectionName"];
   private static readonly int CollectionThroughput = int.Parse(ConfigurationManager.AppSettings["CollectionThroughput"]);
   ```

   Toplu içe aktarıcı, App. config dosyasında belirtilen veritabanı adı, kapsayıcı adı ve üretilen iş değerleriyle yeni bir veritabanı ve bir kapsayıcı oluşturur. 

3. Daha sonra DocumentClient nesnesi doğrudan TCP bağlantı moduyla başlatılır:  

   ```csharp
   ConnectionPolicy connectionPolicy = new ConnectionPolicy
   {
      ConnectionMode = ConnectionMode.Direct,
      ConnectionProtocol = Protocol.Tcp
   };
   DocumentClient client = new DocumentClient(new Uri(endpointUrl),authorizationKey,
   connectionPolicy)
   ```

4. Bulkyürütücü nesnesi, bekleme süresi ve kısıtlanmış istekler için yüksek yeniden deneme değeriyle başlatılır. Ardından, yaşam süresi boyunca sıkışıklık denetimini Bulkyürütücü 'e geçirmek için 0 olarak ayarlanır.  

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

5. Uygulama, Bulkımportasync API 'sini çağırır. .NET kitaplığı, toplu içeri aktarma API 'sinin, serileştirilmiş JSON belgelerinin bir listesini kabul eden ve diğeri de seri durumdan çıkarılan POCO belgelerinin bir listesini kabul eden iki aşırı yüklemesini sağlar. Bu aşırı yüklenmiş yöntemlerin her birinin tanımları hakkında bilgi edinmek için [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkimportasync?view=azure-dotnet)bakın.

   ```csharp
   BulkImportResponse bulkImportResponse = await bulkExecutor.BulkImportAsync(
     documents: documentsToImportInBatch,
     enableUpsert: true,
     disableAutomaticIdGeneration: true,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```
   **Bulkımportasync yöntemi aşağıdaki parametreleri kabul eder:**
   
   |**Parametre**  |**Açıklama** |
   |---------|---------|
   |enableUpsert    |   Belgelerin üst sürümlerini etkinleştirmek için bir bayrak. Bir belge ile verilen kimliği zaten var, bunu güncelleştirilir. Varsayılan olarak, false olarak ayarlanır.      |
   |disableAutomaticIdGeneration    |    Kimliği otomatik olarak oluşturulmasını devre dışı bırakmak için bayrak Varsayılan olarak ayarlanmış true.     |
   |maxConcurrencyPerPartitionKeyRange    | Bölüm anahtar aralığı başına en fazla eşzamanlılık derecesi, null olarak ayarlanması, kitaplığın varsayılan değer olan 20 kullanmasına neden olur. |
   |Maxınmemorysortingbatchsize     |  Her aşamada API çağrısına geçirilen belge numaralandırıcılarından çekilen en fazla belge sayısı.  Toplu içeri aktarma işleminden önce bellek içi ön işleme için sıralama aşamasında, null olarak ayarlandığında kitaplığın varsayılan değer olan min (Documents. Count, 1000000) kullanılmasına neden olur.       |
   |cancellationToken    |    Toplu içeri aktarmaya düzgün bir şekilde çıkmak için iptal belirteci.     |

   **Toplu içeri aktarma yanıtı nesne tanımı** Toplu içeri aktarma API çağrısının sonucu aşağıdaki öznitelikleri içerir:

   |**Parametre**  |**Açıklama**  |
   |---------|---------|
   |NumberOfDocumentsImported (uzun)   |  Toplu olarak sağlanan belgeleri dışında başarıyla içeri aktarıldı belgelerin toplam sayısı, API çağrısı içeri aktarın.       |
   |Totalrequestunitstüketilen (Double)   |   Toplu tarafından tüketilen toplam istek birimi (RU) API çağrısı içeri aktarın.      |
   |Totaltımetaken (TimeSpan)    |   Toplu olarak içeri aktarma tarafından API çağrısı, yürütme tamamlamak için geçen toplam süre.      |
   |Badınputdocuments (liste\<nesnesi >)   |     API çağrısı başarıyla toplu olarak içeri aktarılamadı, bozuk biçimli belgelerin listesini içeri aktarın. Kullanıcı, döndürülen belgelerin düzeltin ve içeri aktarmayı yeniden deneyin. Hatalı biçimlendirilmiş belgeleri kimliği değeri (null ya da başka herhangi bir veri türü geçersiz olarak kabul edilir) bir dize değil belgeleri içerir.    |

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Azure Cosmos DB'de toplu güncelleştirme verileri

Varolan belgeleri BulkUpdateAsync API'sini kullanarak güncelleştirebilirsiniz. Bu örnekte, ad alanına yeni bir değere ayarlayın ve açıklama alanı mevcut kaldırma. Desteklenen alan güncelleştirme işlemlerinin tam kümesi için [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet)başvurun. 

1. "BulkUpdateSample" klasörüne gidin ve "BulkUpdateSample. sln" dosyasını açın.  

2. Güncelleştirme öğelerini ilgili alan güncelleştirme işlemleriyle birlikte tanımlayın. Bu örnekte ad alanı ve açıklama alanı tüm belgelerden kaldırma UnsetUpdateOperation güncelleştirilecek SetUpdateOperation kullanır. Diğer artış gibi bir belge alan belirli bir değere göre işlemleri, belirli değerleri bir dizi alanındaki anında iletme veya belirli bir değer bir dizi alanından kaldırın. Toplu güncelleştirme API 'SI tarafından sunulan farklı yöntemler hakkında bilgi edinmek için [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet)bakın.

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

3. Uygulama, BulkUpdateAsync API 'sini çağırır. BulkUpdateAsync yönteminin tanımı hakkında bilgi edinmek için [API belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.ibulkexecutor.bulkupdateasync?view=azure-dotnet)bakın.  

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
   |maxConcurrencyPerPartitionKeyRange    |   Bölüm anahtar aralığı başına en fazla eşzamanlılık derecesi, null olarak ayarlandığında, kitaplığın varsayılan değer olan 20 ' yi kullanmasına neden olur.   |
   |Maxınmemorysortingbatchsize    |    Toplu güncelleştirmeden önce bellek içi ön işleme sıralama aşaması için her aşamada API çağrısına geçirilen güncelleştirme öğesi numaralandırıcısından alınan en fazla güncelleştirme öğesi sayısı, null olarak ayarlandığında kitaplığın varsayılan değer olan min (UpdateItems. Count) kullanılmasına neden olur. 1000000).     |
   | cancellationToken|Toplu güncelleştirmeden sorunsuz bir şekilde çıkmak için iptal belirteci. |

   **Toplu güncelleştirme yanıtı nesne tanımı** Toplu güncelleştirme API 'SI çağrısının sonucu aşağıdaki öznitelikleri içerir:

   |**Parametre**  |**Açıklama** |
   |---------|---------|
   |NumberOfDocumentsUpdated (uzun)    |   Toplu güncelleştirme API çağrısı için sağlandıklardan başarıyla güncellenen toplam belge sayısı.      |
   |Totalrequestunitstüketilen (Double)   |    Toplu güncelleştirme API çağrısı tarafından tüketilen toplam istek birimi (RU).    |
   |Totaltımetaken (TimeSpan)   | Yürütme tamamlanması API çağrısı tarafından toplu geçen toplam süreyi güncelleştirin. |
    
## <a name="performance-tips"></a>Performans ipuçları 

Toplu Yürütücü Kitaplığı kullanıldığında daha iyi performans için aşağıdaki noktaları göz önünde bulundurun:

* En iyi performansı elde etmek için, uygulamanızı Cosmos DB hesabınızın yazma bölgesiyle aynı bölgede bulunan bir Azure sanal makinesinden çalıştırın.  

* Belirli bir Cosmos kapsayıcısına karşılık gelen tek bir sanal makinede uygulamanın tamamı için tek bir Bulkyürütücü nesnesi örneği oluşturmanız önerilir.  

* Bu yana tek bir toplu işlem API yürütme istemcinin CPU ve ağ GÇ büyük bir yığın kullanır. Birden çok görev tarafından dahili olarak UNICODE böyle, yürütülen her toplu işlem API çağrıları, uygulama işlemi içinde birden çok eş zamanlı görevleri UNICODE özen göstermektir. Tek bir sanal makinede çalışan tek bir toplu işlem API çağrısı, kapsayıcının aktarım hızını (kapsayıcının verimlilik > 1.000.000 RU/sn) tüketmez, aynı anda yürütülecek ayrı sanal makineler oluşturmak tercih edilir toplu işlem API 'SI çağrıları.  

* Hedef Cosmos kapsayıcı bölümü eşlemini getirmek için bir Bulkyürütücü nesnesi örneği oluşturulduktan sonra ınitialeasync () çağrıldığından emin olun.  

* Uygulamanızın App. config dosyasında, daha iyi performans için **gcServer** etkinleştirildiğinden emin olun
  ```xml  
  <runtime>
    <gcServer enabled="true" />
  </runtime>
  ```
* Kitaplık, bir günlük dosyasında ya da konsolunda toplanabilecek izlemeleri yayar. Her ikisini de etkinleştirmek için uygulamanızın App. config dosyasına aşağıdakini ekleyin.

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
* NuGet paket ayrıntıları ve toplu yürütücü .NET kitaplığı sürüm notları hakkında bilgi edinmek için bkz.[toplu yürütücü SDK ayrıntıları](sql-api-sdk-bulk-executor-dot-net.md). 
