---
title: C++ ile Azure Tablo Depolama ve Azure Cosmos DB Tablo API'yi kullanma
description: Azure Tablo Depolama veya Azure Cosmos DB Tablo API’sini kullanarak yapılandırılmış verileri bulutta depolayın.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: cpp
ms.topic: sample
ms.date: 10/07/2019
author: sakash279
ms.author: akshanka
ms.openlocfilehash: e6d61e329ba91f53b11ace4d258b35950e188dcb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76771211"
---
# <a name="how-to-use-azure-table-storage-and-azure-cosmos-db-table-api-with-c"></a>Azure Tablo Depolama ve Azure Cosmos DB Tablo API’sini C++ ile kullanma

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

Bu kılavuz, Azure Tablo depolama hizmetini veya Azure Cosmos DB Tablo API'sini kullanarak size yaygın senaryoları gösterir. Örnekler C++ dilinde yazılmıştır ve [C++ için Azure Depolama İstemci Kitaplığı](https://github.com/Azure/azure-storage-cpp/blob/master/README.md)’nı kullanır. Bu makalede, aşağıdaki senaryoları kapsar:

* Tablo oluşturma ve silme
* Tablo varlıklarıyla çalışma

> [!NOTE]
> Bu kılavuz C++ için Azure Depolama İstemci Kitaplığı sürüm 1.0.0 ve üzerini hedefler. Önerilen [sürüm, NuGet](https://www.nuget.org/packages/wastorage) veya [GitHub](https://github.com/Azure/azure-storage-cpp/)kullanılarak kullanılabilen Depolama İstemci Kitaplığı 2.2.0'dır.
>

## <a name="create-accounts"></a>Hesap oluşturma

### <a name="create-an-azure-service-account"></a>Azure hizmet hesabı oluşturma

[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma

[!INCLUDE [cosmos-db-create-storage-account](../../includes/cosmos-db-create-storage-account.md)]

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Azure Cosmos DB Tablo API’si hesabı oluşturma

[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="create-a-c-application"></a>C++ uygulaması oluşturma

Bu kılavuzda, bir C++ uygulamasının depolama özelliklerini kullanırsınız. Bunu yapmak için C++ için Azure Depolama İstemci Kitaplığını yükleyin.

C++ için Azure Depolama İstemci Kitaplığını yüklemek için aşağıdaki yöntemleri kullanın:

* **Linux:** [C++ README için Azure Depolama İstemci Kitaplığı'nda verilen yönergeleri izleyin: Linux sayfasından başlarken.](https://github.com/Azure/azure-storage-cpp#getting-started-on-linux)
* **Windows:** Windows'da, bağımlılık yöneticisi olarak [vcpkg](https://github.com/microsoft/vcpkg) kullanın. Vcpkg'ı başlatmak için [hızlı başlat'ı](https://github.com/microsoft/vcpkg#quick-start) izleyin. Ardından, kitaplığı yüklemek için aşağıdaki komutu kullanın:

```powershell
.\vcpkg.exe install azure-storage-cpp
```

[README](https://github.com/Azure/azure-storage-cpp#download--install) dosyasında kaynak kodunun nasıl oluşturup Nuget'e aktarılacağı yla ilgili bir kılavuz bulabilirsiniz.

### <a name="configure-access-to-the-table-client-library"></a>Tablo istemci kitaplığına erişimi yapılandırma

Tablolara erişmek için Azure depolama API'lerini kullanmak için C++ dosyasının üst bölümüne aşağıdaki `include` ifadeleri ekleyin:

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

Azure Depolama istemcisi veya Cosmos DB istemcisi, veri yönetimi hizmetlerine erişmek üzere uç noktaları ve kimlik bilgilerini depolamak için bir bağlantı dizesi kullanır. Bir istemci uygulaması çalıştırdığınızda, depolama bağlantı dizesini veya Azure Cosmos DB bağlantı dizesini uygun biçimde sağlamanız gerekir.

### <a name="set-up-an-azure-storage-connection-string"></a>Azure Depolama bağlantı dizesini ayarlama

Bu örnek, Azure Depolama bağlantı dizesini tutmak için statik bir alanın nasıl bildirilen:  

```cpp
// Define the Storage connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=<your_storage_account>;AccountKey=<your_storage_account_key>"));
```

Depolama hesabınızın adını `<your_storage_account>`kullanın. <your_storage_account_key> için [Azure portalında](https://portal.azure.com)listelenen Depolama hesabının erişim anahtarını kullanın. Depolama hesapları ve erişim anahtarları hakkında bilgi için [bkz.](../storage/common/storage-create-storage-account.md)

### <a name="set-up-an-azure-cosmos-db-connection-string"></a>Azure Cosmos DB bağlantı dizesini ayarlama

Bu örnek, Azure Cosmos DB bağlantı dizesini tutmak için statik bir alanın nasıl bildirilen:

```cpp
// Define the Azure Cosmos DB connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=<your_cosmos_db_account>;AccountKey=<your_cosmos_db_account_key>;TableEndpoint=<your_cosmos_db_endpoint>"));
```

Azure Cosmos DB hesabınızın adını `<your_cosmos_db_account>`kullanın. Birincil anahtarınızı `<your_cosmos_db_account_key>`girin. [Azure portalında](https://portal.azure.com) listelenen bitiş noktasını `<your_cosmos_db_endpoint>`girin.

Uygulamanızı yerel Windows tabanlı bilgisayarınızda test etmek için, [Azure SDK](https://azure.microsoft.com/downloads/) ile birlikte yüklenen Azure depolama öykünücüsünü kullanabilirsiniz. Depolama öykünücüsü, yerel geliştirme makinenizde mevcut olan Azure Blob, Kuyruk ve Tablo hizmetlerini benzeten bir yardımcı programdır. Aşağıdaki örnek, bağlantı dizesini yerel depolama emülatörünüzüze tutmak için statik bir alanın nasıl bildirilen:  

```cpp
// Define the connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Windows masaüstünüzden Azure depolama emülatörü başlatmak için **Başlat** düğmesini veya Windows anahtarını seçin. *Microsoft Azure Depolama Emülatörü'ne*girin ve çalıştırın. Daha fazla bilgi için bkz. Geliştirme [ve sınama için Azure depolama emülatörü kullanın.](../storage/common/storage-use-emulator.md)

### <a name="retrieve-your-connection-string"></a>Bağlantı dizenizi alma

`cloud_storage_account` Sınıfı, depolama hesabı bilgilerinizi temsil etmek için kullanabilirsiniz. Depolama hesabı bilgilerinizi depolama bağlantı dizesinden `parse` almak için yöntemi kullanın.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Sonra, bir `cloud_table_client` sınıf için bir referans almak. Bu sınıf, Tablo depolama hizmetinde depolanan tablolar ve varlıklar için başvuru nesneleri almanızı sağlar. Aşağıdaki kod, daha `cloud_table_client` önce aldığınız depolama hesabı nesnesini kullanarak bir nesne oluşturur:  

```cpp
// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-and-add-entities-to-a-table"></a>Tablooluşturma ve tabloya varlık ekleme

### <a name="create-a-table"></a>Bir tablo oluşturma

Nesne, `cloud_table_client` tablolar ve varlıklar için başvuru nesnelerini almanızı sağlar. Aşağıdaki kod bir `cloud_table_client` nesne oluşturur ve yeni bir tablo oluşturmak için kullanır.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference to a table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table if it doesn't exist.
table.create_if_not_exists();  
```

### <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme

Tabloya varlık eklemek için yeni `table_entity` bir nesne oluşturun `table_operation::insert_entity`ve 'ye geçirin Aşağıdaki kod, satır anahtarı olarak müşterinin adını, bölüm anahtarı olarak soyadını kullanır. Birlikte, bir varlığın bölüm ve sıra anahtarı varlığı tabloda benzersiz şekilde tanımlar. Aynı bölüm tuşuna sahip varlıklar, farklı bölüm tuşlarına sahip varlıklardan daha hızlı sorgulanabilir. Farklı bölüm tuşlarını kullanmak daha fazla paralel işlem ölçeklenebilirliği sağlar. Daha fazla bilgi için bkz. [Microsoft Azure depolama performansı ve ölçeklenebilirliği yapılacaklar listesi](../storage/common/storage-performance-checklist.md).

Aşağıdaki kod, depolamak için `table_entity` bazı müşteri verileri ile yeni bir örnek oluşturur. Bir varlığı `table_operation::insert_entity` tabloya `table_operation` eklemek için bir nesne oluşturmak için sonraki kod çağırır ve yeni tablo varlığını onunla ilişkilendirir. Son olarak, kod `execute` `cloud_table` nesne üzerinde yöntem çağırır. Yeni, `table_operation` yeni müşteri varlığını `people` tabloya eklemek için Tablo hizmetine bir istek gönderir.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference to a table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table if it doesn't exist.
table.create_if_not_exists();

// Create a new customer entity.
azure::storage::table_entity customer1(U("Harp"), U("Walter"));

azure::storage::table_entity::properties_type& properties = customer1.properties();
properties.reserve(2);
properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

// Create the table operation that inserts the customer entity.
azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

// Execute the insert operation.
azure::storage::table_result insert_result = table.execute(insert_operation);
```

### <a name="insert-a-batch-of-entities"></a>Toplu işlem varlık yerleştirme

Tablo hizmetine tek bir yazma işlemiyle çok sayıda varlık ekleyebilirsiniz. Aşağıdaki kod bir `table_batch_operation` nesne oluşturur ve sonra üç ekleme işlemleri ekler. Her ekleme işlemi, yeni bir varlık nesnesi oluşturarak, `insert` değerlerini ayarlayarak ve sonra varlığı yeni bir ekleme işlemiyle ilişkilendirmek için `table_batch_operation` nesne üzerindeki yöntemi çağırarak eklenir. Ardından, kod `cloud_table.execute` işlemi çalıştırmak için çağırır.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define a batch operation.
azure::storage::table_batch_operation batch_operation;

// Create a customer entity and add it to the table.
azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

azure::storage::table_entity::properties_type& properties1 = customer1.properties();
properties1.reserve(2);
properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

// Create another customer entity and add it to the table.
azure::storage::table_entity customer2(U("Smith"), U("Ben"));

azure::storage::table_entity::properties_type& properties2 = customer2.properties();
properties2.reserve(2);
properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

// Create a third customer entity to add to the table.
azure::storage::table_entity customer3(U("Smith"), U("Denise"));

azure::storage::table_entity::properties_type& properties3 = customer3.properties();
properties3.reserve(2);
properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

// Add customer entities to the batch insert operation.
batch_operation.insert_or_replace_entity(customer1);
batch_operation.insert_or_replace_entity(customer2);
batch_operation.insert_or_replace_entity(customer3);

// Execute the batch operation.
std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);
```

Toplu işlemlerde dikkat edilecek bazı noktalar:

* Tek bir toplu iş `insert`halinde `delete` `merge`100 , , `replace`, `insert-or-merge`, ve `insert-or-replace` işlemleri herhangi bir kombinasyon da yapabilirsiniz.  
* Toplu işlem, toplu işlemdeki tek işlemse, bir geri alma işlemi olabilir.  
* Tek bir toplu işlemdeki tüm varlıkların bölüm anahtarları aynı olmalıdır.  
* Toplu işlem 4 MB veri yükü ile sınırlıdır.  

## <a name="query-and-modify-entities"></a>Varlıkları sorgula ve değiştir

### <a name="retrieve-all-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma

Bir bölümdeki tüm varlıklar için tabloyu `table_query` sorgulamak için bir nesne kullanın. Aşağıdaki kod örneği, bölüm anahtarının bulunduğu `Smith` varlıklar için bir filtre belirtir. Bu örnek sorgu sonuçlarındaki her varlığın alanlarını konsola yazdırır.  

> [!NOTE]
> Bu yöntemler şu anda Azure Cosmos DB’de C++ için desteklenmemektedir.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Construct the query operation for all customer entities where PartitionKey="Smith".
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Print the fields for each customer.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

Bu örnekteki sorgu, filtre ölçütleri ile eşleşen tüm varlıkları döndürür. Büyük tablolarınız varsa ve tablo varlıklarını sık indirmeniz gerekiyorsa, verilerinizi bunun yerine Azure depolama bloblarında depolamanız önerilir.

### <a name="retrieve-a-range-of-entities-in-a-partition"></a>Bir bölüme bir grup varlık alma

Bir bölümdeki tüm varlıkları sorgulamak istemiyorsanız, bir aralık belirtebilirsiniz. Bölme tuşu filtresini bir satır tuşu filtresiyle birleştirin. Aşağıdaki kod örneği, satır anahtarının (ilk `Smith` ad) `E` alfabedekinden daha önceki bir harfle başladığı ve sorgu sonuçlarını yazdırdığı tüm varlıkları bölüme almak için iki filtre kullanır.  

> [!NOTE]
> Bu yöntemler şu anda Azure Cosmos DB’de C++ için desteklenmemektedir.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table query.
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
    azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
    azure::storage::query_comparison_operator::equal, U("Smith")),
    azure::storage::query_logical_operator::op_and,
    azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Loop through the results, displaying information about the entity.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

### <a name="retrieve-a-single-entity"></a>Tek bir varlık alma

Tek, belirli bir varlığı almak üzere bir sorgu yazabilirsiniz. Aşağıdaki kod `table_operation::retrieve_entity` müşteriyi `Jeff Smith`belirtmek için kullanır. Bu yöntem, bir koleksiyon yerine yalnızca bir varlık döndürür ve döndürülen değer . `table_result` Bir sorguda hem bölüm hem de satır anahtarını belirtmek Tablo hizmetinden tek bir varlık almanın en hızlı yoludur.  

```cpp
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Retrieve the entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Output the entity.
azure::storage::table_entity entity = retrieve_result.entity();
const azure::storage::table_entity::properties_type& properties = entity.properties();

std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
    << U(", Property1: ") << properties.at(U("Email")).string_value()
    << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
```

### <a name="replace-an-entity"></a>Bir varlığı değiştirme

Bir varlığı değiştirmek için Tablo hizmetinden alın, varlık nesnesini değiştirin ve değişiklikleri Tablo hizmetine geri kaydedin. Aşağıdaki kod mevcut bir müşterinin telefon numarasını ve e-posta adresini değiştirir. Aramak `table_operation::insert_entity`yerine, bu `table_operation::replace_entity`kod kullanır. Bu yaklaşım, sunucudaki varlık alındığı tarihten itibaren değişmediği sürece varlığın sunucuda tamamen değiştirilmesine neden olur. Değiştirildiyse, işlem başarısız olur. Bu hata, uygulamanızın başka bir bileşen tarafından alma ve güncelleştirme arasında yapılan bir değişikliği üzerine yazmasını engeller. Bu hatanın doğru işleme varlığı yeniden almak, değişikliklerinizi yapmak, hala geçerli `table_operation::replace_entity` yse ve sonra başka bir işlem yapmaktır.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Replace an entity.
azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
properties_to_replace.reserve(2);

// Specify a new phone number.
properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

// Specify a new email address.
properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

// Create an operation to replace the entity.
azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

// Submit the operation to the Table service.
azure::storage::table_result replace_result = table.execute(replace_operation);
```

### <a name="insert-or-replace-an-entity"></a>Bir varlık ekleme veya değiştirme

`table_operation::replace_entity`varlık sunucudan alındığı için değiştirildiyse işlemler başarısız olur. Ayrıca, başarılı olmak için `table_operation::replace_entity` önce sunucudan varlığı almanız gerekir. Bazen, varlığın sunucuda var olup olmadığını bilemezsin. Güncelleştirmeniz in hepsini üzerine yazması gerektiğinden, içinde depolanan geçerli değerler önemsizdir. Bu sonucu elde etmek `table_operation::insert_or_replace_entity` için bir işlem kullanın. Bu işlem, yoksa varlığı ekler. İşlem varsa varlığın yerini alır. Aşağıdaki kod örneğinde, müşteri `Jeff Smith` varlığı hala alınır, ancak daha sonra kullanarak `table_operation::insert_or_replace_entity`sunucuya geri kaydedilir. Varlığa alma ve güncelleştirme işlemleri arasında yapılan tüm güncelleştirmelerin üzerine yazılacaktır.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Insert or replace an entity.
azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

properties_to_insert_or_replace.reserve(2);

// Specify a phone number.
properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

// Specify an email address.
properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

// Create an operation to insert or replace the entity.
azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

// Submit the operation to the Table service.
azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);
```

### <a name="query-a-subset-of-entity-properties"></a>Giriş özellikleri alt kümesi sorgulama

Tabloya gönderilen bir sorgu, bir varlıktan yalnızca birkaç özellik alabilir. Aşağıdaki koddaki sorgu, `table_query::set_select_columns` yalnızca tablodaki varlıkların e-posta adreslerini döndürmek için yöntemi kullanır.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define the query, and select only the Email property.
azure::storage::table_query query;
std::vector<utility::string_t> columns;

columns.push_back(U("Email"));
query.set_select_columns(columns);

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Display the results.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

    const azure::storage::table_entity::properties_type& properties = it->properties();
    for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
    {
        std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
    }

    std::wcout << std::endl;
}
```

> [!NOTE]
> Bir varlıktaki birkaç özelliğin sorgulanması, tüm özelliklerin alınmasından daha verimli bir işlemdir.
>

## <a name="delete-content"></a>İçeriği silme

### <a name="delete-an-entity"></a>Bir varlığı silme

Bir varlığı aldıktan sonra silebilirsiniz. Bir varlığı aldıktan sonra, silmek için varlığı arayın. `table_operation::delete_entity` O zaman `cloud_table.execute` yöntemi ara. Aşağıdaki kod, bölüm tuşu `Smith` ve `Jeff`satır tuşu ile bir varlığı alır ve siler.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation to delete the entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit the delete operation to the Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);  
```

### <a name="delete-a-table"></a>Bir tablo silme

Son olarak aşağıdaki kod örneği bir depolama hesabından bir tablo siler. Silinmiş bir tablo silinmeden sonra bir süre yeniden oluşturulacak şekilde kullanılamaz.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Delete the table if it exists
if (table.delete_table_if_exists())
{
    std::cout << "Table deleted!";
}
else
{
    std::cout << "Table didn't exist";
}
```

## <a name="troubleshooting"></a>Sorun giderme

Visual Studio Community Edition için, projeniz *storage_account.h* ve *table.h*dosyaları nedeniyle yapı hataları alıyorsa, **/izin verilen** derleyici anahtarını kaldırın:

1. **Çözüm Gezgini**’nde projenize sağ tıklayın ve **Özellikler**’i seçin.
1. **Özellik Sayfaları** iletişim kutusunda **Yapılandırma Özellikleri**’ni, **C/C++**’yi genişletin ve **Dil**’i seçin.
1. **Uyumluluk modu**’nu **Hayır** olarak ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.

Azure Depolama ve Azure Cosmos DB Tablo API’si hakkında daha fazla bilgi almak için şu bağlantıları izleyin:

* [Tablo API’sine giriş](table-introduction.md)
* [Azure Depolama Kaynaklarını C++ dilinde listeleme](../storage/common/storage-c-plus-plus-enumeration.md)
* [C++ başvurusu için Depolama İstemci Kitaplığı](https://azure.github.io/azure-storage-cpp)
* [Azure Depolama belgeleri](https://azure.microsoft.com/documentation/services/storage/)
