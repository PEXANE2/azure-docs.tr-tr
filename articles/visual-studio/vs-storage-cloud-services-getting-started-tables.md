---
title: Visual Studio 'u (bulut hizmetleri) kullanarak tablo depolamaya başlayın
description: Visual Studio'ya bağlı hizmetleri kullanarak bir depolama hesabına bağlandıktan sonra Visual Studio'daki bir bulut hizmeti projesinde Azure Tablo depolamasını kullanmaya nasıl başlarsınız?
services: storage
author: ghogen
manager: jillfra
ms.assetid: a3a11ed8-ba7f-4193-912b-e555f5b72184
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 5c42d65b5e2c46fcdbe1b0725f2ebce881722db3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72299997"
---
# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure tablo depolama ve Visual Studio’ya bağlı hizmetleri kullanmaya başlama (bulut hizmeti projeleri)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, Visual Studio **Bağlantılı Hizmetler Ekle** iletişim kutusunu kullanarak bulut hizmetleri projesinde bir Azure depolama hesabı oluşturduktan veya başvurulan sonra Visual Studio'da Azure tablo depolama sını kullanmaya nasıl başlanırsınız. **Bağlı Hizmetleri Ekle** işlemi, projenizdeki Azure depolama alanına erişmek için uygun NuGet paketlerini yükler ve depolama hesabının bağlantı dizesini proje yapılandırma dosyalarınıza ekler.

Azure Tablo depolama hizmeti, büyük miktarda yapılandırılmış veri depolamanıza olanak tanır. Hizmet, Azure bulutu içinden ve dışından kimlik doğrulamalı aramaları kabul eden bir NoSQL veri deposudur. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir.

Başlamak için öncelikle depolama hesabınızda bir tablo oluşturmanız gerekir. Size kod da bir Azure tablosunun nasıl oluşturulacağını ve tablo varlıklarını ekleme, değiştirme, okuma ve okuma gibi temel tablo ve varlık işlemlerinin nasıl gerçekleştirileceğini gösteririz. Örnekler C\# koduyla yazılır ve [.NET için Microsoft Azure Depolama istemci kitaplığını](https://msdn.microsoft.com/library/azure/dn261237.aspx)kullanır.

**NOT:** Azure depolamasına çağrı yapan API'lerden bazıları eşzamanlıdır. Daha fazla bilgi için [Async ve Await ile Asynchronous programlamaya](https://msdn.microsoft.com/library/hh191443.aspx) bakın. Aşağıdaki kod, async programlama yöntemlerinin kullanıldığını varsayar.

* Bkz. Programlı olarak tabloları manipüle etme hakkında daha fazla bilgi için [.NET'i kullanarak Azure Tablo depolama](../storage/storage-dotnet-how-to-use-tables.md) alanına başlayın.
* Azure Depolama hakkında genel bilgiler için [Depolama belgelerine](https://azure.microsoft.com/documentation/services/storage/) bakın.
* Azure bulut hizmetleri hakkında genel bilgi için [Bulut Hizmetleri belgelerine](https://azure.microsoft.com/documentation/services/cloud-services/) bakın.
* Programlama ASP.NET uygulamaları hakkında daha fazla bilgi için [ASP.NET](https://www.asp.net) bakın.

## <a name="access-tables-in-code"></a>Koddaki erişim tabloları
Bulut hizmeti projelerindeki tablolara erişmek için, Azure tablo depolama alanına erişen c# kaynak dosyalarına aşağıdaki öğeleri eklemeniz gerekir.

1. C# dosyasının üst kısmındaki ad alanı bildirimlerinin bunları **kullanarak** ifadeler içerdiğinden emin olun.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantısı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
   > [!NOTE]
   > Yukarıdaki kodun tümlerini aşağıdaki örneklerde kodun önünde kullanın.
   > 
   > 
3. Depolama hesabınızdaki tablo nesnelerine başvurmak için bir **CloudTableClient** nesnesi alın.
   
         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Belirli bir tabloya ve varlıklara başvurmak için bir **CloudTable** başvuru nesnesi alın.
   
        // Get a reference to a table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Kodda tablo oluşturma
Azure tablosunu oluşturmak için, "Koddaki Erişim tabloları" bölümünde açıklandığı gibi bir **CloudTable** nesnesi aldıktan sonra **CreateIfNotExistsAsync'e** bir çağrı eklemeniz gerekir.

    // Create the CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Tabloya bir varlık eklemek için varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Aşağıdaki kod, müşterinin ilk adını satır anahtarı ve son adı bölüm anahtarı olarak kullanan **CustomerEntity** adlı bir varlık sınıfını tanımlar.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Varlıkları içeren tablo işlemleri, daha önce oluşturduğunuz **CloudTable** nesnesi kullanılarak yapılır. **TableOperation** nesnesi yapılacak işlemi temsil eder. Aşağıdaki kod örneği, **CloudTable** nesnesinin ve **CustomerEntity** nesnesinin nasıl oluşturulup oluşturulabildiğini gösterir. İşlemi hazırlamak için, müşteri varlığını tabloya eklemek için bir **Tablo Çalışması** oluşturulur. Son olarak, işlem **CloudTable.ExecuteAsync**çağırArak yürütülür.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a>Toplu işlem varlık yerleştirme
Tek bir yazma işleminde birden çok varlığı tabloya ekleyebilirsiniz. Aşağıdaki kod örneği iki varlık nesnesi ("Jeff Smith" ve "Ben Smith") oluşturur, bunları Ekle yöntemini kullanarak **tableBatchOperation** nesnesine ekler ve **cloudTable.ExecuteBatchAsync'i**arayarak işlemi başlatır.

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma
Bir bölümdeki tüm varlıklar için tabloyu sorgulamak için **TableQuery** nesnesi kullanın. Aşağıdaki kod örneği, ‘Smith’in bölüm anahtarı olduğu varlıklar için bir filtre belirtir. Bu örnek sorgu sonuçlarındaki her varlığın alanlarını konsola yazdırır.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

    return View();


## <a name="get-a-single-entity"></a>Tek bir varlık alma
Tek bir, belirli bir varlık almak için bir sorgu yazabilirsiniz. Aşağıdaki kod, 'Ben Smith' adlı bir müşteriyi belirtmek için bir **TableOperation** nesnesi kullanır. Bu yöntem, bir koleksiyon yerine yalnızca bir varlık döndürür ve **TableResult.Result'da** döndürülen değer bir **CustomerEntity** nesnesidir. Sorguda hem bölüm hem de satır anahtarlarını belirtmek, **Tablo** hizmetinden tek bir varlığı almanın en hızlı yoludur.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlığı bulduktan sonra silebilirsiniz. Aşağıdaki kod "Ben Smith" adlı bir müşteri varlığını arar ve onu bulursa, onu siler.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

