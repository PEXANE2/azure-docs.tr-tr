---
title: Dış AKıŞ oluşturma (Transact-SQL)-Azure SQL Edge (Önizleme)
description: Azure SQL Edge 'de dış AKıŞ oluşturma açıklaması hakkında bilgi edinin (Önizleme)
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 9e1f672a62ee7687fec9cea96ca03240c893ba95
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2020
ms.locfileid: "84233326"
---
# <a name="create-external-stream-transact-sql"></a>Dış AKıŞ oluşturma (Transact-SQL)

Dış AKıŞ nesnesi hem giriş hem de çıkış için iki amaca sahiptir. Azure Event veya IoT Hub 'ları gibi olay alma hizmetlerinden gelen akış verilerini sorgulamak veya bir akış sorgusundan sonuçların nerede ve nasıl depolanacağını belirtmek için çıkış olarak kullanılan bir giriş olarak kullanılabilir.

Ayrıca, bir dış AKıŞ, Olay Hub 'ı veya blob depolama gibi hizmetler için hem çıkış hem de giriş olarak belirtilebilir ve oluşturulabilir. Bu, akış sorgusunun çıkış olarak dış akışa ve girdi olarak aynı dış akıştan okuduğu başka bir akış sorgusuna neden olduğu zincirleme senaryolarında oluşur. 


## <a name="syntax"></a>Söz dizimi

```sql
CREATE EXTERNAL STREAM {external_stream_name}  
(column definition [,column definitions ] * ) --Used for input - optional 
WITH  
(  
  DATA_SOURCE = <data_source_name>, 
  LOCATION = <location_name>, 
  EXTERNAL_FILE_FORMAT = <external_file_format_name>, --Used for input - optional 
   
  INPUT_OPTIONS =  
    ‘CONSUMER_GROUP=<consumer_group_name>; 
    ‘TIME_POLICY=<time_policy>; 
    LATE_EVENT_TOLERANCE=<late_event_tolerance_value>; 
    OUT_OF_ORDER_EVENT_TOLERANCE=<out_of_order_tolerance_value> 
     
    /* Edge options */ 
  , 
  OUTPUT_OPTIONS =  
    ‘REJECT_POLICY=<reject_policy>; 
    MINIMUM_ROWS=<row_value>; 
    MAXIMUM_TIME=<time_value_minutes>; 
    PARTITION_KEY_COLUMN=<partition_key_column_name>; 
    PROPERTY_COLUMNS=(); 
    SYSTEM_PROPERTY_COLUMNS=(); 
    PARTITION_KEY=<partition_key_name>; 
    ROW_KEY=<row_key_name>; 
    BATCH_SIZE=<batch_size_value>; 
    MAXIMUM_BATCH_COUNT=<batch_value>;  
    STAGING_AREA=<blob_data_source>’ 
     
    /* Edge options */ TAGS=<tag_column_value> 
); 
```


## <a name="arguments"></a>Bağımsız değişkenler


- [DıŞ VERI KAYNAĞı](/sql/t-sql/statements/create-external-data-source-transact-sql/)
- [DıŞ DOSYA BIÇIMI](/sql/t-sql/statements/create-external-file-format-transact-sql/)
- **Konum**: veri kaynağındaki gerçek verilerin veya konumun adını belirtir. Sınır hub 'ı veya Kafka Stream nesnesi söz konusu olduğunda konum, okuma veya yazma için uç hub veya Kafka konusunun adını belirtir.
- **INPUT_OPTIONS**:, akış sorgularının girdileri olan olay ve IoT Hub 'ları gibi hizmetler için anahtar-değer çiftleri olarak seçenekleri belirtin
    - CONSUMER_GROUP: olay ve IoT Hub 'Ları, bir tüketici grubundaki okuyucu sayısını sınırlar (5 ' e kadar). Bu alanı boş bırakmak, ' $Default ' tüketici grubunu kullanır.
      - Olay ve ıOT hub 'Ları için geçerlidir
    - TIME_POLICY: geç olaylar veya sipariş dışı olaylar tolerans değerlerini geçilişinde olayların mi yoksa olay saatinin mi ayarlanacağını açıklar.
      - Olay ve ıOT hub 'Ları için geçerlidir
    - LATE_EVENT_TOLERANCE: kabul edilebilir en fazla gecikme süresi. Gecikme, olayın zaman damgası ve sistem saati arasındaki farkı temsil eder.
      - Olay ve ıOT hub 'Ları için geçerlidir
    - OUT_OF_ORDER_EVENT_TOLERANCE: olaylar, akış sorgusuna yapılan yolculuğa kadar bir sıra sonra gelebilir. Bu olaylar olduğu gibi kabul edilebilir veya bir süre sonra yeniden sıralamak üzere bir ayarlama dönemi için duraklamayı tercih edebilirsiniz.
      - Olay ve ıOT hub 'Ları için geçerlidir
- **OUTPUT_OPTIONS**:, akış sorgularına çıkış yapan desteklenen hizmetler için anahtar-değer çiftleri olarak seçenekleri belirtin 
  - REJECT_POLICY: BıRAK | Veri dönüştürme hataları oluştuğunda veri hatası işleme ilkeleri türler yeniden deneyin. 
    - Desteklenen tüm çıktılar için geçerlidir 
  - MINIMUM_ROWS:  
    Bir çıkışa yazılan toplu iş başına gereken minimum satır. Parquet için her Batch yeni bir dosya oluşturur. 
    - Desteklenen tüm çıktılar için geçerlidir 
  - MAXIMUM_TIME:  
    Toplu iş başına en fazla bekleme süresi. Bu süreden sonra, en düşük satır gereksinimi karşılanmasa bile toplu iş çıktıya yazılır. 
    - Desteklenen tüm çıktılar için geçerlidir 
  - PARTITION_KEY_COLUMN:  
    Bölüm anahtarı için kullanılan sütun. 
    - Olay Hub 'ı ve Service Bus konusu için geçerlidir 
  - PROPERTY_COLUMNS:  
    İstenirse, iletilere özel özellikler olarak eklenecek çıkış sütunlarının adlarının virgülle ayrılmış bir listesi.  
    - Olay Hub 'ı ve Service Bus konu ve kuyruk için geçerlidir 
  - SYSTEM_PROPERTY_COLUMNS:  
    Service Bus iletilerde doldurulacak sistem özellik adlarının ve çıktı sütunlarının ad/değer çiftlerinin bir JSON biçimli koleksiyonu. örn. {"MessageID": "Sütun1", "PartitionKey": "Sütun2"} 
    - Service Bus konu ve kuyruk için geçerlidir 
  - PARTITION_KEY:  
    Bölüm anahtarını içeren çıkış sütununun adı. Bölüm anahtarı, bir varlığın birincil anahtarının ilk kısmını oluşturan belirli bir tablo içindeki bölüm için benzersiz bir tanımlayıcıdır. Boyutu 1 KB 'a kadar olabilecek bir dize değeridir. 
    - Tablo depolama ve Azure Işlevi için geçerlidir 
  - ROW_KEY:  
    Satır anahtarını içeren çıkış sütununun adı. Satır anahtarı, belirli bir bölüm içindeki bir varlık için benzersiz bir tanımlayıcıdır. Bir varlığın birincil anahtarının ikinci bölümünü oluşturur. Satır anahtarı, boyutu 1 KB 'a kadar olabilecek bir dize değeridir. 
    - Tablo depolama ve Azure Işlevi için geçerlidir 
  - BATCH_SIZE:  
    Bu, en fazla 100 kayda gidebileceği tablo depolaması için işlem sayısını temsil eder. Azure Işlevleri için bu, çağrı başına işleve gönderilen bayt cinsinden toplu iş boyutunu temsil eder-varsayılan değer 256 kB 'dir. 
    - Tablo depolama ve Azure Işlevi için geçerlidir 
  - MAXIMUM_BATCH_COUNT:  
    Azure işlevi için çağrı başına işleve gönderilen en fazla olay sayısı-varsayılan 100 ' dir. SQL veritabanı için, bu, her toplu ekleme işlemi ile gönderilen en fazla kayıt sayısını temsil eder-varsayılan değer 10.000 ' dir. 
    - SQL veritabanı ve Azure işlevi için geçerlidir 
  - STAGING_AREA: dış VERI kaynağı nesnesi, BLOB depolama alanına SQL veri ambarı 'na veri alımı için yüksek aktarım hızı verileri alma 
    - SQL veri ambarı için geçerlidir


## <a name="examples"></a>Örnekler

### <a name="example-1---edgehub"></a>Örnek 1-EdgeHub

Tür: giriş veya çıkış<br>
Parametreler:
- Giriş veya çıkış
  - Diğer ad 
  - Olay serileştirme biçimi 
  - Encoding 
- Yalnızca giriş: 
  - Olay sıkıştırma türü 

Söz dizimi:

```sql
CREATE EXTERNAL DATA SOURCE MyEdgeHub 
WITH  
(      
  LOCATION = 'edgehub://'       
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat  
WITH (  
   FORMAT_TYPE = 'JSON', 
); 
 
CREATE EXTERNAL STREAM Stream_A  
WITH    
(   
   DATA_SOURCE = MyEdgeHub, 
   EXTERNAL_FILE_FORMAT = myFileFormat, 
   LOCATION = ‘<mytopicname>’, 
   OUTPUT_OPTIONS =   
     ‘REJECT_TYPE: Drop’ 
);
```


### <a name="example-2---azure-sql-database-azure-sql-edge-sql-server"></a>Örnek 2-Azure SQL veritabanı, Azure SQL Edge, SQL Server

Tür: çıkış<br>
Parametreler:
- Çıktı diğer adı  
- Veritabanı (SQL veritabanı için gereklidir) 
- Sunucu (SQL veritabanı için gereklidir) 
- Kullanıcı adı (SQL veritabanı için gereklidir) 
- Parola (SQL veritabanı için gereklidir) 
- Tablo 
- Tüm giriş bölümlerini tek bir yazma veya önceki sorgu adımının veya girişinin bölüm düzeninde birleştirme (SQL veritabanı için gereklidir) 
- En fazla toplu iş sayısı 

Söz dizimi:

```sql
CREATE DATABASE SCOPED CREDENTIAL SQLCredName 
WITH IDENTITY = '<user>’, 
SECRET = '<password>'; 
 
-- Azure SQL Database 
CREATE EXTERNAL DATA SOURCE MyTargetSQLTabl 
WITH 
(     
  LOCATION = ' <my_server_name>.database.windows.net’, 
  CREDENTIAL = SQLCredName 
); 
 
--SQL Server or Azure SQL Edge
CREATE EXTERNAL DATA SOURCE MyTargetSQLTabl 
WITH 
(     
  LOCATION = ' <sqlserver://<ipaddress>,<port>’, 
  CREDENTIAL = SQLCredName 
); 
 
--SQL Database/Edge 
CREATE EXTERNAL STREAM Stream_A 
WITH   
(  
    DATA_SOURCE = MyTargetSQLTable, 
    LOCATION = ‘<DatabaseName>.<SchemaName>.<TableName>’ 
   --Note: If table is container in the database, <TableName> should be sufficient 
   --Note: Do not need external file format in this case 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    OUTPUT_OPTIONS =  
      ‘REJECT_TYPE: Drop 
); 
```

### <a name="example-3---kafka"></a>Örnek 3-Kafka

Tür: giriş<br>
Parametreler:

- Kafka önyükleme sunucusu 
- Kafka konu adı 
- Kaynak bölüm sayısı 

Söz dizimi:

```sql
CREATE EXTERNAL DATA SOURCE MyKafka_tweets 
WITH 
( 
  --The location maps to KafkaBootstrapServer 
  LOCATION = ' kafka://<kafkaserver>:<ipaddress>’, 
  CREDENTIAL = kafkaCredName 
 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’, 
    DELIMITER = ‘|’ 
); 
 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyKafka_tweets, 
    LOCATION = ‘<KafkaTopicName>’, 
   --JSON: Format, CSV: Delimiter and Encoding, AVRO: None 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    INPUT_OPTIONS =  
      ‘PARTITIONS: 5’ 
); 
```

### <a name="example-4---blob-storage"></a>Örnek 4-BLOB depolama

Tür: giriş veya çıkış<br>
Parametreler:
- Giriş veya çıkış:
  - Diğer ad 
  - Depolama hesabı 
  - Depolama hesabı anahtarı 
  - Kapsayıcı 
  - Yol deseni 
  - Tarih biçimi 
  - Saat biçimi 
  - Olay serileştirme biçimi 
  - Encoding 
- Yalnızca giriş: 
  - Bölümler (giriş) 
  - Olay sıkıştırma türü (giriş) 
- Yalnızca çıkış: 
  - Minimum satırlar (çıkış) 
  - En uzun süre (çıkış) 
  - Kimlik doğrulama modu (çıkış) 

Söz dizimi:

```sql
CREATE DATABASE SCOPED CREDENTIAL StorageAcctCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = 'AccountKey'; 
 
CREATE DATABASE SCOPED CREDENTIAL StorageAcctCredNameMSI 
WITH IDENTITY = 'Managed Identity’; 
 
CREATE EXTERNAL DATA SOURCE MyBlobStorage_tweets 
WITH 
(     
  LOCATION = 'sb://my-sb-namespace.servicebus.windows.net’, 
  CREDENTIAL = eventHubCredName 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', --Event serialization format 
    DATE_FORMAT = 'YYYY/MM/DD HH', --Both Date and Time format 
    ENCODING = ‘UTF-8’ 
); 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyBlobStorage_tweets, 
    LOCATION = ‘<path_pattern>’, 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    INPUT_OPTIONS =  
      ‘PARTITIONS: 1’, 
  
    OUTPUT_OPTIONS =  
      ‘REJECT_TYPE: Drop, 
      PARTITION_KEY_COLUMN: , 
      PROPERTY_COLUMNS: (), 
      MINUMUM_ROWS: 100000, 
      MAXIMUM_TIME: 60’ 
); 
```

### <a name="example-5---event-hub"></a>Örnek 5-Olay Hub 'ı

Tür: giriş veya çıkış<br>
Parametreler:
- Giriş veya çıkış:
  - Diğer ad 
  - Service Bus ad alanı 
  - Olay Hub'ı adı 
  - Olay Hub'ı ilke adı 
  - Olay Hub 'ı ilke anahtarı 
  - Olay serileştirme biçimi 
  - Encoding 
- Yalnızca giriş: 
  - Olay Hub'ı tüketici grubu 
  - Olay sıkıştırma türü 
- Yalnızca çıkış: 
  - Bölüm anahtarı sütunu 
  - Özellik sütunları 

Söz dizimi:

```sql
CREATE DATABASE SCOPED CREDENTIAL eventHubCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = '<policyName>'; 
 
CREATE EXTERNAL DATA SOURCE MyEventHub_tweets 
WITH 
(     
  LOCATION = 'sb://my-sb-namespace.servicebus.windows.net’, 
  CREDENTIAL = eventHubCredName 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’, 
    DELIMITER = ‘|’ 
); 
 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyEventHub_tweets, 
    LOCATION = ‘<topicname>’, 
   --JSON: Format, CSV: Delimiter and Encoding, AVRO: None 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
 
    INPUT_OPTIONS =  
      ‘CONSUMER_GROUP: FirstConsumerGroup’, 
          
    OUTPUT_OPTIONS =  
      ‘REJECT_TYPE: Drop, 
      PARTITION_KEY_COLUMN: , 
      PROPERTY_COLUMNS: ()’ 
);
```

### <a name="example-6---iot-hub"></a>Örnek 6-ıOT hub 'ı

Tür: giriş<br>
Parametreler:

- Girdi diğer adı 
- IoT Hub 
- Uç Nokta 
- Paylaşılan erişim ilkesi adı 
- Paylaşılan erişim ilkesi anahtarı 
- Tüketici grubu 
- Olay serileştirme biçimi 
- Encoding 
- Olay sıkıştırma türü 

Söz dizimi:

```sql
CREATE DATABASE SCOPED CREDENTIAL IoTHubCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = '<policyName>'; 
 
CREATE EXTERNAL DATA SOURCE MyIoTHub_tweets 
WITH 
(     
  LOCATION = ' iot://iot_hub_name.azure-devices.net’, 
  CREDENTIAL = IoTHubCredName 
);  

CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', --Event serialization format 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’ 
); 
 
CREATE EXTERNAL STREAM Stream_A (user_id VARCHAR, tweet VARCHAR) 
WITH   
(  
    DATA_SOURCE = MyIoTHub_tweets, 
    LOCATION = ‘<name>’, 
    EXTERNAL_FILE_FORMAT = myFileFormat,  
    INPUT_OPTIONS =  
      ‘ENDPOINT: Messaging, 
      CONSUMER_GROUP: ‘FirstConsumerGroup’ 
); 
```

### <a name="example-7---azure-synapse-analytics-formerly-sql-data-warehouse"></a>Örnek 7-Azure SYNAPSE Analytics (eski adıyla SQL veri ambarı)

Tür: çıkış<br>
Parametreler:
- Çıktı diğer adı 
- Veritabanı 
- Sunucu 
- Kullanıcı adı 
- Parola 
- Tablo 
- Hazırlama alanı (kopyalama için) 

Söz dizimi:

```sql
CREATE DATABASE SCOPED CREDENTIAL SQLCredName 
WITH IDENTITY = '<user>’, 
SECRET = '<password>'; 
 
CREATE EXTERNAL DATA SOURCE MyTargetSQLTable 
WITH 
(     
  LOCATION = ' <my_server_name>.database.windows.net’, 
  CREDENTIAL = SQLCredName 
); 
 
CREATE EXTERNAL STREAM MySQLTableOutput 
WITH ( 
   DATA_SOURCE = MyTargetSQLTable, 
   LOCATION = ‘<TableName>’ 
   --Note: Do not need external file format in this case 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop, 
     STAGING_AREA: staging_area_data_source’, 
); 
```


### <a name="example-8---table-storage"></a>Örnek 8-tablo depolama

Tür: çıkış<br>
Parametreler:
- Çıktı diğer adı 
- Depolama hesabı 
- Depolama hesabı anahtarı 
- Tablo adı 
- Bölüm anahtarı 
- Satır anahtarı 
- Toplu iş boyutu 

Söz dizimi:

```sql
CREATE DATABASE SCOPED CREDENTIAL TableStorageCredName 
WITH IDENTITY = ‘Storage account Key’, 
SECRET = '<storage_account_key>'; 
 
CREATE EXTERNAL DATA SOURCE MyTargetTableStorage 
WITH 
(     
  LOCATION = 'abfss://<storage_account>.dfs.core.windows.net’, 
  CREDENTIAL = TableStorageCredName 
); 
 
CREATE EXTERNAL STREAM MyTargetTableStorageOutput 
WITH ( 
   DATA_SOURCE = MyTargetTableStorage, 
   LOCATION = ‘<TableName>’, 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop, 
     PARTITION_KEY: <column_partition_key>, 
     ROW_KEY: <column_row_key>, 
     BATCH_SIZE: 100’ 
); 
```


### <a name="example-9---service-bus-topic-same-properties-as-queue"></a>Örnek 9-Service Bus konu (kuyrukla aynı Özellikler)

Tür: çıkış<br>
Parametreler:
- Çıktı diğer adı 
- Service Bus ad alanı 
- Konu adı 
- Konu ilkesi adı 
- Konu İlkesi anahtarı 
- Özellik sütunları 
- Sistem özelliği sütunları 
- Olay serileştirme biçimi 
- Encoding 

Söz dizimi:

```sql
CREATE DATABASE SCOPED CREDENTIAL serviceBusCredName 
WITH IDENTITY = 'Shared Access Signature’, 
SECRET = '<policyName>'; 
 
CREATE EXTERNAL DATA SOURCE MyServiceBus_tweets 
WITH 
(     
  LOCATION = 'sb://my-sb-namespace.servicebus.windows.net’, 
  CREDENTIAL = serviceBusCredName 
); 
 
CREATE EXTERNAL FILE FORMAT myFileFormat 
WITH ( 
    FORMAT_TYPE = 'CSV', --Event serialization format 
    DATA_COMPRESSION = 'GZIP', 
    ENCODING = ‘UTF-8’ 
); 
 
CREATE EXTERNAL STREAM MyServiceBusOutput 
WITH ( 
   DATA_SOURCE = MyServiceBus_tweets, 
   LOCATION = ‘<topic_name>’, 
   EXTERNAL_FILE_FORMAT = myFileFormat 
       OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop, 
     PARTITION_KEY_COLUMN: , 
     PROPERTY_COLUMNS: (), 
     SYSTEM_PROPERTY_COLUMNS: ()’ 
   --JSON: Format, CSV: Delimiter and Encoding, AVRO: None 
           
); 
```


### <a name="example-10---cosmos-db"></a>Örnek 10-Cosmos DB

Tür: çıkış<br>
Parametreler:
- Çıktı diğer adı 
- Hesap Kimliği 
- Hesap anahtarı 
- Veritabanı 
- Kapsayıcı adı 
- Belge KIMLIĞI 


Söz dizimi:

```sql
CREATE DATABASE SCOPED CREDENTIAL cosmosDBCredName 
WITH IDENTITY = ‘Storage Account Key’, 
SECRET = '<accountKey>'; 
 
CREATE EXTERNAL DATA SOURCE MyCosmosDB_tweets 
WITH 
(     
  LOCATION = ' cosmosdb://accountid.documents.azure.com:443/ database’, 
  CREDENTIAL = cosmosDBCredName 
); 
 
CREATE EXTERNAL STREAM MyCosmosDBOutput 
WITH ( 
   DATA_SOURCE = MyCosmosDB_tweets, 
   LOCATION = ‘<container/documentID>’ 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop', 
     --Note: Do not need external file format in this case 
          
);
```



### <a name="example-11---power-bi"></a>Örnek 11-Power BI

Tür: çıkış<br>
Parametreler:
- Çıktı diğer adı 
- Veri kümesi adı 
- Tablo adı 


Söz dizimi:

```sql
CREATE DATABASE SCOPED CREDENTIAL PBIDBCredName 
WITH IDENTITY = ‘Managed Identity’; 
 
CREATE EXTERNAL DATA SOURCE MyPbi_tweets 
WITH 
( 
  LOCATION = 'pbi://dataset/’, 
  CREDENTIAL = PBIDBCredName 
 
); 
 
CREATE EXTERNAL STREAM MyPbiOutput 
WITH ( 
   DATA_SOURCE = MyPbi_tweets, 
   LOCATION = 'tableName', 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: Drop' 
        
);
```

### <a name="example-12---azure-function"></a>Örnek 12-Azure işlevi

Tür: çıkış<br>
Parametreler:
- İşlev uygulaması 
- İşlev 
- Anahtar 
- En fazla toplu iş boyutu 
- En fazla toplu iş sayısı 

Söz dizimi:

```sql
CREATE DATABASE SCOPED CREDENTIAL AzureFunctionCredName 
WITH IDENTITY = ‘Function Key’, 
SECRET = '<function_key>'; 
 
CREATE EXTERNAL DATA SOURCE MyTargetTableStorage 
WITH 
(     
  LOCATION = 'abfss://<storage_account>.dfs.core.windows.net’, 
  CREDENTIAL = TableStorageCredName 
); 
 
CREATE EXTERNAL STREAM MyTargetTableStorageOutput 
WITH ( 
   DATA_SOURCE = MyTargetTableStorage, 
   LOCATION = ‘<TableName>’, 
   OUTPUT_OPTIONS =  
     ‘REJECT_TYPE: 'Drop'      
     PARTITION_KEY: ‘<column_partition_key>, 
     ROW_KEY: <column_row_key>, 
     BATCH_SIZE: 100’ 
); 
```


## <a name="see-also"></a>Ayrıca bkz.

- [Dış akışı DEĞIŞTIRME (Transact-SQL)](alter-external-stream-transact-sql.md) 
- [Dış akışı bırak (Transact-SQL)](drop-external-stream-transact-sql.md) 

