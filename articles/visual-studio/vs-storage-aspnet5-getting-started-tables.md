---
title: Visual Studio (ASP.NET Core) kullanarak tablo depolama ile başlayın
description: Visual Studio'ya bağlı hizmetleri kullanarak bir depolama hesabına bağlandıktan sonra Visual Studio'daki ASP.NET Core projesinde Azure Tablo depolamasına nasıl başlarsınız?
services: storage
author: ghogen
manager: jillfra
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d209f8117b1e061877daf2f8d316bd01ed4f84cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72298822"
---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Azure Tablo depolama ve Visual Studio bağlantılı hizmetlerle nasıl başlarsınız?

[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

Bu makalede, Visual Studio **Connected Services** özelliğini kullanarak bir ASP.NET Core projesinde bir Azure depolama hesabı oluşturduktan veya başvurulan sonra Visual Studio'da Azure Tablo depolama alanını kullanmaya nasıl başlanırsınız. **Bağlı Hizmetler** işlemi, projenizdeki Azure depolama alanına erişmek için uygun NuGet paketlerini yükler ve depolama hesabının bağlantı dizesini proje yapılandırma dosyalarınıza ekler. (Azure Depolama hakkında genel bilgi için [Depolama belgelerine](https://azure.microsoft.com/documentation/services/storage/) bakın.)

Azure Tablo depolama hizmeti, büyük miktarda yapılandırılmış veri depolamanıza olanak tanır. Hizmet, Azure bulutu içinden ve dışından kimlik doğrulamalı aramaları kabul eden bir NoSQL veri deposudur. Azure tabloları, yapılandırılmış ve ilişkisel olmayan verilerin depolanması için idealdir. Azure Tablo depolama alanını kullanma hakkında daha genel bilgi için [bkz.](../storage/storage-dotnet-how-to-use-tables.md)

Başlamak için önce depolama hesabınızda bir tablo oluşturun. Bu makalede, C# tablosunun nasıl oluşturulup gerçekleştirilebildiğiniz ve tablo girişlerini ekleme, değiştirme, okuma ve kaldırma gibi temel tablo işlemlerinin nasıl gerçekleştirildirilen gösterilmektedir.  Kod, .NET için Azure Depolama İstemci Kitaplığı'nı kullanır. ASP.NET hakkında daha fazla bilgi için [ASP.NET.](https://www.asp.net)

Azure Depolama API'lerinden bazıları eşzamanlıdır ve bu makaledeki kod, async yöntemlerinin kullanıldığını varsayar. Daha fazla bilgi için [Asynchronous programlamaya](https://docs.microsoft.com/dotnet/csharp/async) bakın.

## <a name="access-tables-in-code"></a>Koddaki erişim tabloları

ASP.NET Core projelerindeki tablolara erişmek için, Azure tablo depolama alanına erişen c# kaynak dosyalarına aşağıdaki öğeleri eklemeniz gerekir.

1. Gerekli `using` ifadeleri ekleyin:

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Table;
    using System.Threading.Tasks;
    ```

1. Depolama `CloudStorageAccount` hesabı bilgilerinizi temsil eden bir nesne alın. AppSettings.json'daki depolama bağlantı dizesinde bulabileceğiniz depolama hesabınızın adını ve hesap anahtarını kullanarak aşağıdaki kodu kullanın:

    ```csharp
        CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
                "<name>", "<account-key>"), true);
    ```

1. Depolama `CloudTableClient` hesabınızdaki tablo nesnelerine başvurmak için bir nesne alın:

    ```csharp
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Belirli `CloudTable` bir tabloya ve varlıklara başvurmak için bir başvuru nesnesi alın:

    ```csharp
    // Get a reference to a table named "peopleTable"
    CloudTable peopleTable = tableClient.GetTableReference("peopleTable");
    ```

## <a name="create-a-table-in-code"></a>Kodda tablo oluşturma

Azure tablosunu oluşturmak için, bir async yöntemi `CreateIfNotExistsAsync()`oluşturun ve içinde şu ları arayın:

```csharp
async void CreatePeopleTableAsync()
{
    // Create the CloudTable if it does not exist
    await peopleTable.CreateIfNotExistsAsync();
}
```
    
## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme

Tabloya varlık eklemek için, varlığınızın özelliklerini tanımlayan bir sınıf oluşturursunuz. Aşağıdaki kod, satır anahtarı `CustomerEntity` ve son ad olarak satır anahtarı olarak müşterinin ilk adını kullanan bir varlık sınıfı tanımlar.

```csharp
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
```

Varlıkları içeren tablo `CloudTable` [işlemleri, Access tablolarında](#access-tables-in-code)daha önce oluşturduğunuz nesneyi kodda kullanır. Nesne `TableOperation` yapılacak işlemi temsil eder. Aşağıdaki kod örneği, bir `CloudTable` nesne nin `CustomerEntity` ve nesnenin nasıl oluşturulup oluşturulabildiğini gösterir. İşlemi hazırlamak için, müşteri varlığını tabloya eklemek için bir `TableOperation` oluşturma yapılır. Son olarak, işlem çağırArak `CloudTable.ExecuteAsync`yürütülür.

```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
await peopleTable.ExecuteAsync(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Toplu işlem varlık yerleştirme

Tek bir yazma işleminde birden çok varlığı tabloya ekleyebilirsiniz. Aşağıdaki kod örneği iki varlık nesnesi ("Jeff Smith" ve "Ben `TableBatchOperation` Smith") oluşturur, `Insert` yöntemi kullanarak `CloudTable.ExecuteBatchAsync`bir nesneye ekler ve sonra çağırarak işlemi başlatır.

```csharp
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
```

## <a name="get-all-of-the-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma

Bir bölümdeki tüm varlıklar için tabloyu sorgulamak `TableQuery` için bir nesne kullanın. Aşağıdaki kod örneği, ‘Smith’in bölüm anahtarı olduğu varlıklar için bir filtre belirtir. Bu örnek sorgu sonuçlarındaki her varlığın alanlarını konsola yazdırır.

```csharp
// Construct the query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

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
```

## <a name="get-a-single-entity"></a>Tek bir varlık alma

Tek bir, belirli bir varlık almak için bir sorgu yazabilirsiniz. Aşağıdaki kod, `TableOperation` 'Ben Smith' adlı bir müşteriyi belirtmek için bir nesne kullanır. Yöntem, koleksiyon yerine yalnızca bir varlık döndürür `TableResult.Result` ve `CustomerEntity` döndürülen değer bir nesnedir. Sorguda hem bölüm hem de satır anahtarlarını belirtmek, `Table` hizmetten tek bir varlığı almanın en hızlı yoludur.

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

// Print the phone number of the result.
if (retrievedResult.Result != null)
   Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
else
   Console.WriteLine("The phone number could not be retrieved.");
```

## <a name="delete-an-entity"></a>Bir varlığı silme

Bir varlığı bulduktan sonra silebilirsiniz. Aşağıdaki kod "Ben Smith" adlı bir müşteri varlığını arar ve siler:

```csharp
// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

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
```

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
