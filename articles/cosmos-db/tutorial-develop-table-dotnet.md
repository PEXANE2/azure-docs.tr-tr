---
title: .NET Standart SDK kullanarak Azure Cosmos DB Tablo API
description: Azure Cosmos DB Tablo API hesabında yapılandırılmış verileri nasıl depolayıp sorgulayacağım
author: sakash279
ms.author: akshanka
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: sample
ms.date: 12/03/2019
ms.openlocfilehash: f4d6e1bb0d5db0dbfc30e14abc58321bce8d0baf
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238457"
---
# <a name="get-started-with-azure-cosmos-db-table-api-and-azure-table-storage-using-the-net-sdk"></a>.NET SDK'sı ile Azure Cosmos DB Tablo API'si'ni ve Azure Tablo depolama alanını kullanmaya başlama

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

Yapılandırılmış NoSQL verilerini bulutta depolamak için Azure Cosmos DB Tablo API'sini veya Azure Tablo depolama alanını kullanabilirsiniz ve şema daha az tasarıma sahip bir anahtar/öznitelik deposu sağlayabilirsiniz. Azure Cosmos DB Tablo API'si ve Table depolama alanı şema olmadığından, uygulamanızın gereksinimleri geliştikçe verilerinizi uyarlamak kolaydır. Azure Cosmos DB Tablo API'sini veya Tablo depolama alanını kullanarak web uygulamaları, adres defterleri, aygıt bilgileri veya hizmetinizin gerektirdiği diğer meta veri türleri için kullanıcı verileri gibi esnek veri kümelerini depolayabilirsiniz. 

Bu öğreticide, Azure Cosmos DB Tablo API ve Azure Tablo depolama senaryolarıyla [.NET için Microsoft Azure Cosmos DB Tablo Kitaplığı'nı](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) nasıl kullanacağınızı gösteren bir örnek açıklanmaktadır. Azure hizmetine özgü bağlantıyı kullanmanız gerekir. Bu senaryolar, tabloların nasıl oluşturulup, veri eklemenin/ güncelleştirilen, verileri sorgulanın ve tabloların nasıl silineceklerini gösteren C# örnekleri kullanılarak incelenir.

## <a name="prerequisites"></a>Ön koşullar

Bu örneği başarıyla tamamlamak için aşağıdakiler gerekir:

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

* [.NET için Microsoft Azure CosmosDB Tablo Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) - Bu kitaplık şu anda .NET Standard ve .NET çerçevesi için kullanılabilir. 

* [Azure Cosmos DB Tablo API hesabı.](create-table-dotnet.md#create-a-database-account)

## <a name="create-an-azure-cosmos-db-table-api-account"></a>Azure Cosmos DB Tablo API’si hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="create-a-net-console-project"></a>Bir .NET konsol projesi oluşturma

Visual Studio'da yeni bir .NET konsol uygulaması oluşturun. Aşağıdaki adımlar Visual Studio 2019'da nasıl bir konsol uygulaması oluşturabileceğinizi gösterir. Azure Cosmos DB Tablo Kitaplığını, Azure bulut hizmeti veya web uygulaması ve masaüstü ve mobil uygulamalar da dahil olmak üzere her tür .NET uygulamasında kullanabilirsiniz. Bu kılavuzda, sadeleştirmek için konsol uygulaması kullanmaktayız.

1. **Dosya** > **Yeni** > **Proje'yi**seçin.

1. **Konsol Uygulaması (.NET Core)** seçeneğini belirleyin ve ardından **İleri'yi**seçin.

1. Proje **adı** alanına **CosmosTableSamples**gibi uygulamanız için bir ad girin. (Gerektiğinde farklı bir ad sağlayabilirsiniz.)

1. **Oluştur'u**seçin.

Bu örnekteki tüm kod örnekleri konsol uygulamanızın **Program.cs** dosyasının Ana() yöntemine eklenebilir.

## <a name="install-the-required-nuget-package"></a>Gerekli NuGet paketini yükleyin

NuGet paketini almak için aşağıdaki adımları izleyin:

1. **Çözüm Gezgini**'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.

1. Microsoft Azure [`Microsoft.Azure.Cosmos.Table`](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) [`Microsoft.Extensions.Configuration`](https://www.nuget.org/packages/Microsoft.Extensions.Configuration)Cosmos DB Tablo Kitaplığını yüklemek için [`Microsoft.Extensions.Configuration.Json`](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json)çevrimiçi arama yapın [`Microsoft.Extensions.Configuration.Binder`](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder) ve **Yükle'yi** seçin.

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma

1. Azure [portalından](https://portal.azure.com/)Azure Cosmos hesabınıza veya Tablo Depolama hesabınıza gidin. 

1. Bağlantı **Dizesi** veya **Erişim anahtarları** bölmesini açın. Pencerenin sağ tarafındaki kopyala düğmesini kullanarak **PRIMARY CONNECTION STRING**'i kopyalayın.

   ![Bağlantı Dizesi bölmesindeki PRIMARY CONNECTION STRING’i görüntüleyin ve kopyalayın](./media/create-table-dotnet/connection-string.png)
   
1. Bağlantı dizenizi yapılandırmak için visual studio'dan **cosmosTableSamples**projenize sağ tıklayın.

1. **Ekle'yi** ve ardından **Yeni Öğe'yi**seçin. **TypeScript JSON Configuration** File olarak dosya türüne sahip yeni bir dosya **Settings.json** oluşturun. 

1. Settings.json dosyasındaki kodu aşağıdaki kodla değiştirin ve birincil bağlantı dizenizi atayın:

   ```csharp
   {
   "StorageConnectionString": <Primary connection string of your Azure Cosmos DB account>
   }
   ```

1. **Projenizdeki CosmosTableSamples**sağ tıklayın. **Ekle**, **Yeni Öğe'yi** seçin ve **AppSettings.cs**adlı sınıf ekleyin.

1. AppSettings.cs dosyasına aşağıdaki kodu ekleyin. Bu dosya Ayarlar.json dosyasındaki bağlantı dizesini okur ve yapılandırma parametresine atar:

   ```csharp
   namespace CosmosTableSamples
   {
    using Microsoft.Extensions.Configuration;
    public class AppSettings
    {
        public string StorageConnectionString { get; set; }
        public static AppSettings LoadAppSettings()
        {
            IConfigurationRoot configRoot = new ConfigurationBuilder()
                .AddJsonFile("Settings.json")
                .Build();
            AppSettings appSettings = configRoot.Get<AppSettings>();
            return appSettings;
        }
    }
   }
   ```

## <a name="parse-and-validate-the-connection-details"></a>Bağlantı ayrıntılarını ayrıştın ve doğrulayın 

1. **Projenizdeki CosmosTableSamples**sağ tıklayın. **Ekle**, **Yeni Öğe'yi** seçin ve **Common.cs**adlı sınıf ekleyin. Bağlantı ayrıntılarını doğrulamak ve bu sınıf içinde bir tablo oluşturmak için kod yazarsınız.

1. Aşağıda gösterildiği `CreateStorageAccountFromConnectionString` gibi bir yöntem tanımlayın. Bu yöntem, bağlantı dize ayrıntılarını ayrıştıracak ve "Settings.json" dosyasında sağlanan hesap adı ve hesap anahtarı ayrıntılarının geçerli olduğunu doğrular. 

 ```csharp
using System;

namespace CosmosTableSamples
{
    using System.Threading.Tasks;
    using Microsoft.Azure.Cosmos.Table;
    using Microsoft.Azure.Documents;

    public class Common
    {
        public static CloudStorageAccount CreateStorageAccountFromConnectionString(string storageConnectionString)
        {
            CloudStorageAccount storageAccount;
            try
            {
                storageAccount = CloudStorageAccount.Parse(storageConnectionString);
            }
            catch (FormatException)
            {
                Console.WriteLine("Invalid storage account information provided. Please confirm the AccountName and AccountKey are valid in the app.config file - then restart the application.");
                throw;
            }
            catch (ArgumentException)
            {
                Console.WriteLine("Invalid storage account information provided. Please confirm the AccountName and AccountKey are valid in the app.config file - then restart the sample.");
                Console.ReadLine();
                throw;
            }

            return storageAccount;
        }
    }
}
   ```


## <a name="create-a-table"></a>Tablo Oluşturma 

[CloudTableClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.cloudtableclient) sınıfı, Tablo Depolamada depolanan tabloları ve varlıkları almanızı sağlar. Cosmos DB Tablo API hesabında tablo olmadığından, tablo oluşturmak için `CreateTableAsync` yöntemi **Common.cs** sınıfına ekleyelim:

```csharp
public static async Task<CloudTable> CreateTableAsync(string tableName)
  {
    string storageConnectionString = AppSettings.LoadAppSettings().StorageConnectionString;

    // Retrieve storage account information from connection string.
    CloudStorageAccount storageAccount = CreateStorageAccountFromConnectionString(storageConnectionString);

    // Create a table client for interacting with the table service
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient(new TableClientConfiguration());

    Console.WriteLine("Create a Table for the demo");

    // Create a table client for interacting with the table service 
    CloudTable table = tableClient.GetTableReference(tableName);
    if (await table.CreateIfNotExistsAsync())
    {
      Console.WriteLine("Created Table named: {0}", tableName);
    }
    else
    {
      Console.WriteLine("Table {0} already exists", tableName);
    }

    Console.WriteLine();
    return table;
}
```

"503 hizmet kullanılamayan özel durum" hatası alırsanız, bağlantı modu için gerekli bağlantı noktaları nın bir güvenlik duvarı tarafından engellenmesi mümkündür. Bu sorunu gidermek için, gerekli bağlantı noktalarını açın veya aşağıdaki kodda gösterildiği gibi ağ geçidi modu bağlantısını kullanın:

```csharp
tableClient.TableClientConfiguration.UseRestExecutorForCosmosEndpoint = true;
```

## <a name="define-the-entity"></a>Varlığı tanımlama 

Varlıklar, [TableEntity](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.tableentity)’den oluşturulan özel bir sınıf kullanarak C# nesneleriyle eşlenir. Tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun.

**Projenizdeki CosmosTableSamples**sağ tıklayın. **Ekle**, **Yeni Klasör'ü** seçin ve **model**olarak adlandırın. Model klasöründe **CustomerEntity.cs** adlı bir sınıf ekleyin ve buna aşağıdaki kodu ekleyin.

```csharp
namespace CosmosTableSamples.Model
{
    using Microsoft.Azure.Cosmos.Table;
    public class CustomerEntity : TableEntity
    {
        public CustomerEntity()
        {
        }

        public CustomerEntity(string lastName, string firstName)
        {
            PartitionKey = lastName;
            RowKey = firstName;
        }

        public string Email { get; set; }
        public string PhoneNumber { get; set; }
    }
}
```

Bu kod, satır anahtarı ve son ad olarak satır anahtarı olarak müşterinin ilk adını kullanan bir varlık sınıfını tanımlar. Birlikte, bir varlığın bölüm ve sıra anahtarı varlığı tabloda benzersiz şekilde tanımlar. Aynı bölüm tuşuna sahip varlıklar farklı bölüm tuşlarına sahip varlıklardan daha hızlı sorgulanabilir, ancak farklı bölüm tuşları kullanılarak paralel işlemlerin daha fazla ölçeklenebilirliği sağlar. Tablolarda depolanacak varlıklar, [TableEntity](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.tableentity) sınıfından türetilen türler gibi desteklenen bir türde olmalıdır. Bir tabloda depolamak istediğiniz varlık özellikleri, türün genel özellikleri olmalı ve değerleri hem almayı hem de ayarlamayı desteklemelidir. Bununla birlikte varlık türü parametresiz bir oluşturucu olmalıdır.

## <a name="insert-or-merge-an-entity"></a>Bir varlık ekleme veya birleştirme

Aşağıdaki kod örneği bir varlık nesnesi oluşturur ve tabloya ekler. [TableOperation](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.tableoperation) sınıfı içindeki Ekleme Veya Birleştirme yöntemi, bir varlığı eklemek veya birleştirmek için kullanılır. [CloudTable.ExecuteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.cloudtable.executeasync?view=azure-dotnet) yöntemi işlemi yürütmek için çağrılır. 

**Projenizdeki CosmosTableSamples**sağ tıklayın. **Ekle**, **Yeni Öğe'yi** seçin ve **SamplesUtils.cs**adlı sınıf ekleyin. Bu sınıf, varlıklar üzerinde CRUD işlemleri gerçekleştirmek için gereken tüm kodu depolar. 

```csharp
 public static async Task<CustomerEntity> InsertOrMergeEntityAsync(CloudTable table, CustomerEntity entity)
 {
     if (entity == null)
     {
         throw new ArgumentNullException("entity");
     }
     try
     {
         // Create the InsertOrReplace table operation
         TableOperation insertOrMergeOperation = TableOperation.InsertOrMerge(entity);

         // Execute the operation.
         TableResult result = await table.ExecuteAsync(insertOrMergeOperation);
         CustomerEntity insertedCustomer = result.Result as CustomerEntity;

         // Get the request units consumed by the current operation. RequestCharge of a TableResult is only applied to Azure Cosmos DB
         if (result.RequestCharge.HasValue)
         {
             Console.WriteLine("Request Charge of InsertOrMerge Operation: " + result.RequestCharge);
         }

         return insertedCustomer;
     }
     catch (StorageException e)
     {
         Console.WriteLine(e.Message);
         Console.ReadLine();
         throw;
     }
 }
```

### <a name="get-an-entity-from-a-partition"></a>Bir bölümden varlık alma

[TableOperation](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.tableoperation) sınıfı altında Retrieve yöntemini kullanarak bir bölümden varlık elde edebilirsiniz. Aşağıdaki kod örneği, bir müşteri kuruluşunun bölüm anahtarı anahtar anahtarını, e-postasını ve telefon numarasını alır. Bu örnek, varlık için sorgu için tüketilen istek birimlerini de yazdırır. Bir varlığı sorgulamak için, aşağıdaki kodu **SamplesUtils.cs** dosyaya ekleyin: 

```csharp
public static async Task<CustomerEntity> RetrieveEntityUsingPointQueryAsync(CloudTable table, string partitionKey, string rowKey)
    {
      try
      {
        TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>(partitionKey, rowKey);
        TableResult result = await table.ExecuteAsync(retrieveOperation);
        CustomerEntity customer = result.Result as CustomerEntity;
        if (customer != null)
        {
          Console.WriteLine("\t{0}\t{1}\t{2}\t{3}", customer.PartitionKey, customer.RowKey, customer.Email, customer.PhoneNumber);
        }

        // Get the request units consumed by the current operation. RequestCharge of a TableResult is only applied to Azure CosmoS DB 
        if (result.RequestCharge.HasValue)
        {
           Console.WriteLine("Request Charge of Retrieve Operation: " + result.RequestCharge);
        }

        return customer;
        }
        catch (StorageException e)
        {
           Console.WriteLine(e.Message);
           Console.ReadLine();
           throw;
        }
    }
```

## <a name="delete-an-entity"></a>Bir varlığı silme

Bir varlığı güncelleştirmek için gösterilen aynı yöntemi kullanarak, bir varlığı aldıktan sonra kolayca silebilirsiniz. Aşağıdaki kod bir müşteri girişini alır ve siler. Bir varlığı silmek için, aşağıdaki kodu **SamplesUtils.cs** dosyaya eklayın: 

```csharp
public static async Task DeleteEntityAsync(CloudTable table, CustomerEntity deleteEntity)
   {
     try
     {
        if (deleteEntity == null)
     {
        throw new ArgumentNullException("deleteEntity");
     }

    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
    TableResult result = await table.ExecuteAsync(deleteOperation);

    // Get the request units consumed by the current operation. RequestCharge of a TableResult is only applied to Azure CosmoS DB 
    if (result.RequestCharge.HasValue)
    {
       Console.WriteLine("Request Charge of Delete Operation: " + result.RequestCharge);
    }

    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="execute-the-crud-operations-on-sample-data"></a>Örnek veriler de CRUD işlemlerini yürütme

Tablo oluşturma, ekleme veya birleştirme varlıklarını oluşturma yöntemlerini tanımladıktan sonra, bu yöntemleri örnek veriler üzerinde çalıştırın. Bunu yapmak **için, projecosmosTableSamples**sağ tıklayın. **Yeni Öğe** **Ekle'yi**seçin ve **BasicSamples.cs** adlı bir sınıf ekleyin ve buna aşağıdaki kodu ekleyin. Bu kod bir tablo oluşturur, ona varlıklar ekler. Projenin sonundaki varlığı ve tabloyu silmek isterseniz, aşağıdaki `table.DeleteIfExistsAsync()` `SamplesUtils.DeleteEntityAsync(table, customer)` koddan açıklamaları ve yöntemleri kaldırın:

```csharp
using System;
namespace CosmosTableSamples
{
    using System.Threading.Tasks;
    using Microsoft.Azure.Cosmos.Table;
    using Model;

    class BasicSamples
    {
        public async Task RunSamples()
        {
            Console.WriteLine("Azure Cosmos DB Table - Basic Samples\n");
            Console.WriteLine();

            string tableName = "demo" + Guid.NewGuid().ToString().Substring(0, 5);

            // Create or reference an existing table
            CloudTable table = await Common.CreateTableAsync(tableName);

            try
            {
                // Demonstrate basic CRUD functionality 
                await BasicDataOperationsAsync(table);
            }
            finally
            {
                // Delete the table
                // await table.DeleteIfExistsAsync();
            }
        }

        private static async Task BasicDataOperationsAsync(CloudTable table)
        {
            // Create an instance of a customer entity. See the Model\CustomerEntity.cs for a description of the entity.
            CustomerEntity customer = new CustomerEntity("Harp", "Walter")
            {
                Email = "Walter@contoso.com",
                PhoneNumber = "425-555-0101"
            };

            // Demonstrate how to insert the entity
            Console.WriteLine("Insert an Entity.");
            customer = await SamplesUtils.InsertOrMergeEntityAsync(table, customer);

            // Demonstrate how to Update the entity by changing the phone number
            Console.WriteLine("Update an existing Entity using the InsertOrMerge Upsert Operation.");
            customer.PhoneNumber = "425-555-0105";
            await SamplesUtils.InsertOrMergeEntityAsync(table, customer);
            Console.WriteLine();

            // Demonstrate how to Read the updated entity using a point query 
            Console.WriteLine("Reading the updated Entity.");
            customer = await SamplesUtils.RetrieveEntityUsingPointQueryAsync(table, "Harp", "Walter");
            Console.WriteLine();

            // Demonstrate how to Delete an entity
            //Console.WriteLine("Delete the entity. ");
            //await SamplesUtils.DeleteEntityAsync(table, customer);
            //Console.WriteLine();
        }
    }
}
```

Önceki kod "demo" ile başlayan bir tablo oluşturur ve oluşturulan GUID tablo adına eklenir. Daha sonra adı ve soyadı "Harp Walter" olan bir müşteri varlığı ekler ve daha sonra bu kullanıcının telefon numarasını güncelleştirir. 

Bu öğreticide, Tablo API hesabında depolanan veriler üzerinde temel CRUD işlemleri gerçekleştirmek için kod oluşturabilirsiniz. Ayrıca , toplu veri ekleme, bir bölüm içindeki tüm verileri sorgula, bir bölüm içindeki veri aralığını sorgula, hesapta adları belirtilen önek ile başlayan tabloları listeler gibi gelişmiş işlemler de gerçekleştirebilirsiniz. [Azure-cosmos-table-dotnet-core-getting-started](https://github.com/Azure-Samples/azure-cosmos-table-dotnet-core-getting-started) GitHub deposunun tam örnek formunu indirebilirsiniz. [AdvancedSamples.cs](https://github.com/Azure-Samples/azure-cosmos-table-dotnet-core-getting-started/blob/master/CosmosTableSamples/AdvancedSamples.cs) sınıfının veriler üzerinde gerçekleştirebileceğiniz daha fazla işlemi vardır.  

## <a name="run-the-project"></a>Projeyi çalıştırma

Projenizden **CosmosTableSamples**. **Program.cs** adlı sınıfı açın ve proje çalıştığında Temel Örnekler'i aramak için aşağıdaki kodu ekleyin.

```csharp
using System;

namespace CosmosTableSamples
{
    class Program
    {
        public static void Main(string[] args)
        {
            Console.WriteLine("Azure Cosmos Table Samples");
            BasicSamples basicSamples = new BasicSamples();
            basicSamples.RunSamples().Wait();
           
            Console.WriteLine();
            Console.WriteLine("Press any key to exit");
            Console.Read();
        }
    }
}
```

Şimdi çözümü oluşturun ve projeyi çalıştırmak için F5 tuşuna basın. Proje çalıştırıldığında, komut isteminde aşağıdaki çıktıyı görürsünüz:

![Komut isteminden çıkış](./media/tutorial-develop-table-standard/output-from-sample.png)

Projeyi çalıştırırken Ayarlar.json dosyasının bulunamamasını belirten bir hata alırsanız, proje ayarlarına aşağıdaki XML girişini ekleyerek sorunu çözebilirsiniz. CosmosTableSamples'e sağ tıklayın, CosmosTableSamples.csproj'u edin ve aşağıdaki itemGroup'u ekleyin: 

```csharp
  <ItemGroup>
    <None Update="Settings.json">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </None>
  </ItemGroup>
```
Artık Azure portalında oturum açabilir ve verilerin tabloda olduğunu doğrulayabilirsiniz. 

![Portalsonuçları](./media/tutorial-develop-table-standard/results-in-portal.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık bir sonraki öğreticiye devam edebilir ve verileri Azure Cosmos DB Table API hesabına nasıl geçirebileceğinizi öğrenebilirsiniz. 

> [!div class="nextstepaction"]
>[Verileri sorgulama](../cosmos-db/table-import.md)
