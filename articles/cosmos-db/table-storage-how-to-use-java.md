---
title: Java 'dan Azure Tablo Depolamayı veya Azure Cosmos DB Tablo API'si kullanma
description: Azure Tablo Depolama veya Azure Cosmos DB Tablo API’sini kullanarak yapılandırılmış verileri bulutta depolayın.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: Java
ms.topic: sample
ms.date: 07/23/2020
author: sakash279
ms.author: akshanka
ms.custom: devx-track-java
ms.openlocfilehash: e28770bae9f845ae8f5edd3b67bc55175392052a
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056678"
---
# <a name="how-to-use-azure-table-storage-or-azure-cosmos-db-table-api-from-java"></a>Java’dan Azure Tablo depolama veya Azure Cosmos DB Tablo API'sini kullanma

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

Bu makalede, tablolar oluşturma, verilerinizi depolama ve veri üzerinde CRUD işlemleri gerçekleştirme işlemleri gösterilir. Azure Tablo hizmeti 'ni veya Azure Cosmos DB Tablo API'si seçin. Örnekler Java dilinde yazılır ve [Java için Azure Depolama SDK’sı][Azure Storage SDK for Java] kullanır. Tablo **oluşturma**, **listeleme** ve **silme** işlemlerinin yanı sıra bir tablodaki varlıkları **ekleme**, **sorgulama**, **değiştirme** ve **silme** işlemleri ele alınmaktadır. Tablolar hakkında daha fazla bilgi için [Sonraki adımlar](#next-steps)’a bakın.

> [!NOTE]
> Android cihazlarda Azure Depolama kullanan geliştiriciler için bir SDK mevcuttur. Daha fazla bilgi için bkz. [Android için Azure Depolama SDK’sı][Azure Storage SDK for Android].
>

## <a name="create-an-azure-service-account"></a>Azure hizmet hesabı oluşturma

[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

**Azure depolama hesabı oluşturma**

[!INCLUDE [cosmos-db-create-storage-account](../../includes/cosmos-db-create-storage-account.md)]

**Azure Cosmos DB hesabı oluşturma**

[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="create-a-java-application"></a>Java uygulaması oluşturma

Bu kılavuzda bir Java uygulamasında yerel olarak veya Azure’daki bir web rolü ya da çalışan rolünde çalışan kod içinde çalıştırabileceğiniz depolama özelliklerini kullanacaksınız.

Bu makaledeki örnekleri kullanmak için Java Geliştirme Seti’ni (JDK) yükleyin, sonra Azure aboneliğinde bir Azure depolama hesabı veya Azure Cosmos DB hesabı oluşturun. Bunu yaptıktan sonra, geliştirme sisteminizin en düşük gereksinimleri ve GitHub’daki [Java için Azure Depolama SDK'sı][Azure Storage SDK for Java] deposunda listelenen bağımlılıkları karşıladığını doğrulayın. Sisteminiz bu gereksinimleri karşılıyorsa, bu depo için Azure Depolama Kitaplıklarını indirip sisteminize yükleme yönergelerini izleyebilirsiniz. Bu görevleri tamamladıktan sonra bu makaledeki örnekleri kullanan bir Java uygulaması oluşturabilirsiniz.

## <a name="configure-your-application-to-access-table-storage"></a>Uygulamanızı tablo depolama alanına erişecek şekilde yapılandırma

Tablolara erişmek üzere Azure depolama API’lerini veya Azure Cosmos DB Tablo API’sini kullanmak istediğiniz Java dosyasının üstüne aşağıdaki içeri aktarma deyimlerini ekleyin:

```java
// Include the following imports to use table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="add-your-connection-string"></a>Bağlantı dizenizi ekleyin

Azure depolama hesabına veya Azure Cosmos DB Tablo API'si hesabına bağlanabilirsiniz. Bağlantı dizesini, kullanmakta olduğunuz hesap türüne göre alır.

### <a name="add-an-azure-storage-connection-string"></a>Azure depolama bağlantı dizesi ekleme

Azure depolama istemcisi, veri yönetimi hizmetlerine erişmek üzere uç noktaları ve kimlik bilgilerini depolamak için bir depolama bağlantı dizesi kullanır. Bir istemci uygulamada çalışırken, **AccountName** ve **AccountKey** için [Azure portalında](https://portal.azure.com) listelenen depolama hesabınızın adını ve depolama hesabı Birincil erişim anahtarını kullanarak depolama bağlantı dizesini aşağıdaki biçimde sağlamanız gerekir. 

Bu örnekte bağlantı dizesini tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

### <a name="add-an-azure-cosmos-db-table-api-connection-string"></a>Azure Cosmos DB Tablo API’si bağlantı dizesi ekleme

Azure Cosmos DB hesabı, tablo uç noktasını ve kimlik bilgilerinizi depolamak için bir bağlantı dizesi kullanır. Bir istemci uygulamada çalışırken, **AccountName** ve **AccountKey** için [Azure portalında](https://portal.azure.com) listelenen hesabın adını ve Azure Cosmos DB hesabı birincil erişim anahtarını kullanarak Azure Cosmos DB bağlantı dizesini aşağıdaki biçimde sağlamanız gerekir. 

Bu örnekte Azure Cosmos DB bağlantı dizesini tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=https;" + 
    "AccountName=your_cosmosdb_account;" + 
    "AccountKey=your_account_key;" + 
    "TableEndpoint=https://your_endpoint;" ;
```

Azure’daki bir rol içinde çalışan uygulamada bu dizeyi *ServiceConfiguration.cscfg* adlı hizmet yapılandırma dosyasında depolayabilirsiniz ve **RoleEnvironment.getConfigurationSettings** yöntemine bir çağrı ile bu dizeye erişebilirsiniz. Yapılandırma dosyasındaki *StorageConnectionString* adlı bir **Ayar** öğesinden bağlantı dizesini alma örneği aşağıda verilmiştir:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

Bağlantı dizenizi projenin config.properties dosyasında da depolayabilirsiniz:

```java
StorageConnectionString = DefaultEndpointsProtocol=https;AccountName=your_account;AccountKey=your_account_key;TableEndpoint=https://your_table_endpoint/
```

Aşağıdaki örnekler, depolama bağlantı dizesini almak için bu yöntemlerden birini kullandığınızı varsayar.

## <a name="create-a-table"></a>Bir tablo oluşturma

Bir `CloudTableClient` nesnesi, tablolar ve varlıklar için başvuru nesneleri almanızı sağlar. Aşağıdaki kod bir nesnesi oluşturur `CloudTableClient` ve bunu `CloudTable` , "kişiler" adlı bir tabloyu temsil eden yeni bir nesne oluşturmak için kullanır. 

> [!NOTE]
> Nesne oluşturmanın başka yolları vardır `CloudStorageAccount` ; daha fazla bilgi için bkz `CloudStorageAccount` . [Azure Storage Client SDK başvurusu].
>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create the table if it doesn't exist.
    String tableName = "people";
    CloudTable cloudTable = tableClient.getTableReference(tableName);
    cloudTable.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="list-the-tables"></a>Tabloları listeleme

Tabloların listesini edinmek için, **CloudTableClient.listTables()** yöntemini çağırarak tablo adlarının yinelenebilir bir listesini alın.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Loop through the collection of table names.
    for (String table : tableClient.listTables())
    {
        // Output each table name.
        System.out.println(table);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme

Varlıklar, uygulayan özel bir sınıf kullanarak Java nesnelerine eşlenir `TableEntity` . Kolaylık olması için, `TableServiceEntity` sınıfı Özellikler `TableEntity` için adlı alıcı ve ayarlayıcı yöntemleriyle özellikleri eşlemek için yansıma uygular ve kullanır. Tabloya bir varlık eklemek için öncelikle varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Aşağıdaki kod, sıra anahtarı olarak müşterinin adını, bölüm anahtarı olarak soyadını kullanan bir varlık sınıfı tanımlar. Birlikte, bir varlığın bölüm ve sıra anahtarı varlığı tabloda benzersiz şekilde tanımlar. Aynı bölüm anahtarına sahip varlıklar, farklı bölüm anahtarlarına sahip varlıklardan daha hızlı sorgulanabilir.

```java
public class CustomerEntity extends TableServiceEntity {
    public CustomerEntity(String lastName, String firstName) {
        this.partitionKey = lastName;
        this.rowKey = firstName;
    }

    public CustomerEntity() { }

    String email;
    String phoneNumber;

    public String getEmail() {
        return this.email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPhoneNumber() {
        return this.phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }
}
```

Varlıklar içeren tablo işlemleri bir `TableOperation` nesne gerektirir. Bu nesne bir varlık üzerinde gerçekleştirilecek işlemi tanımlar ve bir nesneyle yürütülenebilir `CloudTable` . Aşağıdaki kod, `CustomerEntity` bazı müşteri verilerinin depolanacağı sınıfının yeni bir örneğini oluşturur. Sonraki kod `TableOperation` . ınsertorreplace `TableOperation` , tabloya bir varlık eklemek için bir nesne oluşturmak ve yenisini bununla ilişkilendirir `CustomerEntity` . Son olarak, kod, `execute` nesne üzerinde yöntemini çağırarak `CloudTable` , "kişiler" tablosunu ve yeni, yeni `TableOperation` Müşteri varlığını "kişiler" tablosuna eklemek üzere depolama hizmetine bir istek gönderir veya varlık zaten varsa onu değiştirir.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.setEmail("Walter@contoso.com");
    customer1.setPhoneNumber("425-555-0101");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer1);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="insert-a-batch-of-entities"></a>Toplu işlem varlık yerleştirme

Tablo hizmetine tek bir yazma işlemiyle çok sayıda varlık ekleyebilirsiniz. Aşağıdaki kod, bir `TableBatchOperation` nesnesi oluşturur ve buna üç ekleme işlemi ekler. Her ekleme işlemi, yeni bir varlık nesnesi oluşturularak, değerlerini ayarlayarak ve ardından `insert` `TableBatchOperation` varlığı yeni bir ekleme işlemi ile ilişkilendirmek için nesnesi üzerinde çağırarak eklenir. Sonra, `execute` `CloudTable` "kişiler" tablosunu ve `TableBatchOperation` tablo işlemlerinin toplu işlemini tek bir istekte depolama hizmetine gönderen nesnesini belirterek, nesne üzerinde kod çağırır.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Define a batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a customer entity to add to the table.
    CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
    customer.setEmail("Jeff@contoso.com");
    customer.setPhoneNumber("425-555-0104");
    batchOperation.insertOrReplace(customer);

    // Create another customer entity to add to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.setEmail("Ben@contoso.com");
    customer2.setPhoneNumber("425-555-0102");
    batchOperation.insertOrReplace(customer2);

    // Create a third customer entity to add to the table.
    CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
    customer3.setEmail("Denise@contoso.com");
    customer3.setPhoneNumber("425-555-0103");
    batchOperation.insertOrReplace(customer3);

    // Execute the batch of operations on the "people" table.
    cloudTable.execute(batchOperation);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

Toplu işlemlerde dikkat edilecek bazı noktalar:

* Tek bir toplu işlemdeki herhangi bir birleşimde en fazla 100 insert, delete, merge, replace, insert veya merge ve insert ya da replace işlemi gerçekleştirebilirsiniz.
* Bir toplu işlem toplu iş içindeki tek işlemse bir retrieve işlemi içerebilir.
* Tek bir toplu işlemdeki tüm varlıkların bölüm anahtarları aynı olmalıdır.
* Toplu işlem 4 MB veri yükü ile sınırlıdır.

## <a name="retrieve-all-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma

Bir bölümdeki varlıklar için bir tabloyu sorgulamak üzere bir kullanabilirsiniz `TableQuery` . `TableQuery.from`Belirtilen bir sonuç türünü döndüren belirli bir tabloda sorgu oluşturmak için çağırın. Aşağıdaki kod, ‘Smith’in bölüm anahtarı olduğu varlıklar için bir filtre belirtir. `TableQuery.generateFilterCondition`sorgular için filtre oluşturmak için yardımcı bir yöntemdir. `where` `TableQuery.from` Filtreyi sorguya uygulamak için metodu tarafından döndürülen başvuruya çağrı yapın. Sorgu, nesne üzerinde bir çağrısıyla yürütüldüğünde `execute` `CloudTable` , `Iterator` `CustomerEntity` belirtilen sonuç türü ile bir döndürür. Ardından, `Iterator` sonuçları kullanmak için döndürülen "foreach" döngüsünde geri dönüş kullanabilirsiniz. Bu kod, sorgu sonuçlarındaki her varlığın alanlarını konsola yazdırır.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Specify a partition query, using "Smith" as the partition key filter.
    TableQuery<CustomerEntity> partitionQuery =
        TableQuery.from(CustomerEntity.class)
        .where(partitionFilter);

    // Loop through the results, displaying information about the entity.
    for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Bir bölüme bir grup varlık alma

Bir bölümdeki tüm varlıkları sorgulamak istemiyorsanız, bir filtredeki karşılaştırma işleçlerini kullanarak aralık belirtebilirsiniz. Aşağıdaki kod, 'Smith' bölümünde, satır anahtarı (ad) alfabede 'E' harfinden önce gelen bir harfle başlayan tüm varlıkları almak için iki filtre kullanır. Ardından sorgu sonuçlarını yazdırır. Bu kılavuzun toplu iş ekleme bölümündeki tabloya eklenmiş varlıkları kullanırsanız, bu kez yalnızca iki varlık (Ben ve Denise Smith) döndürülür; Jeff Smith dahil edilmez.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Create a filter condition where the row key is less than the letter "E".
    String rowFilter = TableQuery.generateFilterCondition(
        ROW_KEY,
        QueryComparisons.LESS_THAN,
        "E");

    // Combine the two conditions into a filter expression.
    String combinedFilter = TableQuery.combineFilters(partitionFilter,
        Operators.AND, rowFilter);

    // Specify a range query, using "Smith" as the partition key,
    // with the row key being up to the letter "E".
    TableQuery<CustomerEntity> rangeQuery =
        TableQuery.from(CustomerEntity.class)
        .where(combinedFilter);

    // Loop through the results, displaying information about the entity
    for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="retrieve-a-single-entity"></a>Tek bir varlık alma

Tek, belirli bir varlığı almak üzere bir sorgu yazabilirsiniz. Aşağıdaki kod, `TableOperation.retrieve` "Jeff Smith" adlı müşteriyi belirtmek için bölüm anahtarı ve satır anahtarı parametreleri ile çağrı `Table Query` yapar ve aynı şeyi yapmak için bir filtre kullanın. Yürütüldüğünde, retrieve işlemi bir koleksiyon yerine yalnızca bir varlık döndürür. `getResultAsType`Yöntemi, sonucu, bir nesnesi olan atama hedefinin türüne yayınlar `CustomerEntity` . Bu tür sorgu için belirtilen tür ile uyumlu değilse, bir özel durum oluşturulur. Hiçbir varlık tam bir bölüm ve satır anahtarı eşleşmesine sahip değilse bir null değer döndürülür. Bir sorguda hem bölüm hem de satır anahtarını belirtmek Tablo hizmetinden tek bir varlık almanın en hızlı yoludur.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Output the entity.
    if (specificEntity != null)
    {
        System.out.println(specificEntity.getPartitionKey() +
            " " + specificEntity.getRowKey() +
            "\t" + specificEntity.getEmail() +
            "\t" + specificEntity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="modify-an-entity"></a>Varlığı değiştirme

Bir varlığı değiştirmek için tablo hizmetinden alın, varlık nesnesinde değişiklikler yapın ve değişiklikleri bir replace veya merge işlemi ile tablo hizmetine geri kaydedin. Aşağıdaki kod mevcut bir müşterinin telefon numarasını değiştirir. Bu kod, ekleme sırasında yaptığımız gibi **TableOperation.insert** yöntemini çağırmak yerine **TableOperation.replace** yöntemini çağırır. **CloudTable.execute** yöntemi, tablo hizmetini çağırır ve bu uygulama tarafından alınmasından sonra başka bir uygulama tarafından değiştirilmedikçe varlık değiştirilir. Bu durum gerçekleştiğinde bir özel durum oluşturulur ve varlığın yeniden alınması, değiştirilmesi ve kaydedilmesi gerekir. Bu iyimser eşzamanlılık yeniden deneme modeli, dağıtılmış bir depolama sisteminde yaygındır.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Specify a new phone number.
    specificEntity.setPhoneNumber("425-555-0105");

    // Create an operation to replace the entity.
    TableOperation replaceEntity = TableOperation.replace(specificEntity);

    // Submit the operation to the table service.
    cloudTable.execute(replaceEntity);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="query-a-subset-of-entity-properties"></a>Giriş özellikleri alt kümesi sorgulama

Tabloya gönderilen bir sorgu, bir varlıktan yalnızca birkaç özellik alabilir. Projeksiyon olarak adlandırılan bu yöntem bant genişliğini azaltır ve özellikle büyük varlıklar için sorgu performansını iyileştirebilir. Aşağıdaki kodda bulunan sorgu, `select` yalnızca tablodaki varlıkların e-posta adreslerini döndürmek için yöntemini kullanır. Sonuçlar `String` `Entity Resolver` , sunucusundan döndürülen varlıklarda tür dönüştürmeyi belirleyen bir öğesinin yardımıyla bir koleksiyonuna yansıtılır. [Azure tabloları: upsert ve sorgu projeksiyonu tanıtımı] [Azure tabloları: upsert ve sorgu projeksiyonu tanıtma] içinde projeksiyon hakkında daha fazla bilgi edinebilirsiniz. Projeksiyon yerel depolama öykünücüsünde desteklenmez, bu nedenle bu kod yalnızca tablo hizmetinde bir hesap kullanılırken çalışır.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Define a projection query that retrieves only the Email property
    TableQuery<CustomerEntity> projectionQuery =
        TableQuery.from(CustomerEntity.class)
        .select(new String[] {"Email"});

    // Define an Entity resolver to project the entity to the Email value.
    EntityResolver<String> emailResolver = new EntityResolver<String>() {
        @Override
        public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
            return properties.get("Email").getValueAsString();
        }
    };

    // Loop through the results, displaying the Email values.
    for (String projectedString :
        cloudTable.execute(projectionQuery, emailResolver)) {
            System.out.println(projectedString);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="insert-or-replace-an-entity"></a>Varlık Ekleme veya Değiştirme

Genellikle tabloda zaten mevcut olup olmadığını bilmeden tabloya varlık eklemek istersiniz. Bir INSERT veya-Replace işlemi, varlık yoksa varlığı ekleyecek veya varsa var olan birini değiştirecek tek bir istek yapmanıza olanak sağlar. Önceki örneklere ek olarak, aşağıdaki kod "Walter Harp" varlığını ekler veya değiştirir. Yeni bir varlık oluşturulduktan sonra bu kod **TableOperation.insertOrReplace** yöntemini çağırır. Bu kod daha sonra, parametre olarak tablo ve ekleme veya değiştirme tablosu ile **bulut tablo** nesnesinde **yürütme** işlemini çağırır. Bir varlığın yalnızca bir kısmını güncelleştirmek için, bunun yerine **TableOperation.insertOrMerge** yöntemi kullanılabilir. INSERT-veya-Replace yerel depolama öykünücüsünde desteklenmez, bu nedenle bu kod yalnızca tablo hizmetinde bir hesap kullanılırken çalışır. INSERT-or-Replace ve INSERT-or-Merge hakkında daha fazla bilgi edinmek için bu [Azure tabloları: upsert ve sorgu projeksiyonunu tanıtma] [Azure tabloları: upsert ve sorgu projeksiyonu tanıtma].

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
    customer5.setEmail("Walter@contoso.com");
    customer5.setPhoneNumber("425-555-0106");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer5);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-an-entity"></a>Bir varlığı silme

Bir varlığı aldıktan sonra kolayca silebilirsiniz. Varlık alındıktan sonra, `TableOperation.delete` Silinecek varlıkla çağırın. Sonra `execute` `CloudTable` nesneyi çağırın. Aşağıdaki kod bir müşteri girişini alır ve siler.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    CustomerEntity entitySmithJeff =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Create an operation to delete the entity.
    TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

    // Submit the delete operation to the table service.
    cloudTable.execute(deleteSmithJeff);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-table"></a>Bir tablo silme

Son olarak, aşağıdaki kod bir depolama hesabından bir tabloyu siler. Bir tabloyu sildikten sonra 40 saniye içinde, yeniden oluşturamazsınız. 

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Delete the table and all its data if it exists.
    CloudTable cloudTable = tableClient.getTableReference("people");
    cloudTable.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Java’da Azure Tablo Hizmetini Kullanmaya Başlama](https://github.com/Azure-Samples/storage-table-java-getting-started)
* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
* [Java için Azure Depolama SDK'sı][Azure Storage SDK for Java]
* [Azure Depolama İstemcisi SDK Başvurusu][Azure Storage Client SDK Reference]
* [Azure Depolama REST API][Azure Storage REST API]
* [Azure Depolama Ekibi Blogu][Azure Storage Team Blog]

Daha fazla bilgi için bkz. [Java geliştiricileri için Azure](/java/azure).

[Azure SDK for Java]: https://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Depolama İstemcisi SDK Başvurusu]: https://azure.github.io/azure-storage-java/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: https://blogs.msdn.microsoft.com/windowsazurestorage/
