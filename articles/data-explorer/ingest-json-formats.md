---
title: JSON biçimlendirilmiş verileri Azure Veri Gezgini'ne alma
description: JSON biçimlendirilmiş verileri Azure Veri Gezgini'ne nasıl sindirebilirsiniz hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: kerend
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: bcf6a0ccfc04890052f1a4bab19f97ee4e55f87a
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756620"
---
# <a name="ingest-json-formatted-sample-data-into-azure-data-explorer"></a>JSON biçimlendirilmiş örnek verileri Azure Veri Gezgini'ne alma

Bu makalede, JSON biçimlendirilmiş verileri Azure Veri Gezgini veritabanına nasıl sindirilen gösterilmektedir. Ham ve eşlenmiş JSON basit örnekler ile başlayacağız, çok çizgili JSON devam ve sonra dizileri ve sözlükler içeren daha karmaşık JSON şema mücadele.  Örnekler, Kusto sorgu dili (KQL), C#veya Python kullanarak JSON biçimlendirilmiş verileri sindirme işlemini ayrıntılarıyla açıklar. Kusto sorgu `ingest` dil denetim komutları doğrudan motor bitiş noktasına çalıştırılır. Üretim senaryolarında, istemci kitaplıkları veya veri bağlantıları kullanılarak Veri Yönetimi hizmetine aktar edilir. Bu istemci kitaplıkları ile veri yutma ile ilgili bir geçiş için [Azure Veri Gezgini .NET Standart SDK kullanarak](/azure/data-explorer/net-standard-ingest-data) Azure Veri Gezgini Python kitaplığı ve Veri Yutma kullanarak veri yutma [yıkın.](/azure/data-explorer/python-ingest-data)

## <a name="prerequisites"></a>Ön koşullar

[Test kümesi ve veritabanı](create-cluster-database-portal.md)

## <a name="the-json-format"></a>JSON biçimi

Azure Veri Gezgini iki JSON dosya biçimini destekler:
* `json`: Hat ayrılmış JSON. Giriş verilerindeki her satırın tam olarak bir JSON kaydı vardır.
* `multijson`: Çok çizgili JSON. Ayrıştırıcı satır ayırıcılarını yoksa ve önceki konumdan geçerli bir JSON'un sonuna kadar bir kayıt okur.

### <a name="ingest-and-map-json-formatted-data"></a>JSON biçimlendirilmiş verileri yutma ve haritalandırma

JSON biçimlendirilmiş verilerin yutulması, [yutma özelliğini](/azure/kusto/management/data-ingestion/index#ingestion-properties)kullanarak *biçimi* belirtmenizi gerektirir. JSON verilerinin alınması, json kaynak girişini hedef sütununa eşleyen [eşleme](/azure/kusto/management/mappings)gerektirir. Verileri sindirirken, önceden tanımlanmış `jsonMappingReference` yutma özelliğini `jsonMapping`kullanın veya yutma özelliğini belirtin. Bu makalede, `jsonMappingReference` yutma için kullanılan tabloda önceden tanımlanmış olan yutma özelliği kullanılacaktır. Aşağıdaki örneklerde, JSON kayıtlarını ham veri olarak tek bir sütun tablosuna silerek başlayacağız. Daha sonra eşlenen sütuna her özelliği yutmak için eşleme kullanacağız. 

### <a name="simple-json-example"></a>Basit JSON örneği

Aşağıdaki örnek, düz bir yapıya sahip basit bir JSON'dur. Veriler, çeşitli cihazlar tarafından toplanan sıcaklık ve nem bilgilerine sahiptir. Her kayıt bir kimlik ve zaman damgası ile işaretlenir.

```json
{
    "timestamp": "2019-05-02 15:23:50.0369439",
    "deviceId": "2945c8aa-f13e-4c48-4473-b81440bb5ca2",
    "messageId": "7f316225-839a-4593-92b5-1812949279b3",
    "temperature": 31.0301639051317,
    "humidity": 62.0791099602725
}
```

## <a name="ingest-raw-json-records"></a>Ham JSON kayıtlarını yutma 

Bu örnekte, JSON kayıtlarını ham veri olarak tek bir sütun tablosuna sindirirsiniz. Veri işleme, sorguları kullanarak ve güncelleştirme ilkesi veriler yutulur sonra yapılır.

# <a name="kql"></a>[KQL](#tab/kusto-query-language)

Ham JSON biçiminde veri yutmak için Kusto sorgu dilini kullanın.

1. Oturum [https://dataexplorer.azure.com](https://dataexplorer.azure.com)aç.

1. **Küme ekle**'yi seçin.

1. Küme **Ekle** iletişim kutusuna, küme URL'nizi `https://<ClusterName>.<Region>.kusto.windows.net/`forma girin , sonra **Ekle'yi**seçin.

1. Aşağıdaki komuta yapıştırın ve tabloyu oluşturmak için **Çalıştır'ı** seçin.

    ```Kusto
    .create table RawEvents (Event: dynamic)
    ```

    Bu sorgu, [dinamik](/azure/kusto/query/scalar-data-types/dynamic) bir `Event` veri türünde tek bir sütuniçeren bir tablo oluşturur.

1. JSON eşlemi oluşturun.

    ```Kusto
    .create table RawEvents ingestion json mapping 'RawEventMapping' '[{"column":"Event","path":"$"}]'
    ```

    Bu komut bir eşleme oluşturur ve Sütuna `Event` JSON kök yolunu `$` eşler.

1. Verileri tabloya `RawEvents` doğru yutun.

    ```Kusto
    .ingest into table RawEvents h'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/simple.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (format=json, jsonMappingReference=RawEventMapping)
    ```

# <a name="c"></a>[C #](#tab/c-sharp)

Ham JSON formatında veri yutmak için C# kullanın.

1. Tabloyu `RawEvents` oluşturun.

    ```C#
    var kustoUri = "https://<ClusterName>.<Region>.kusto.windows.net:443/";
    var kustoConnectionStringBuilder =
        new KustoConnectionStringBuilder(ingestUri)
        {
            FederatedSecurity = true,
            InitialCatalog = database,
            UserID = user,
            Password = password,
            Authority = tenantId
        };
    var kustoClient = KustoClientFactory.CreateCslAdminProvider(kustoConnectionStringBuilder);

    var table = "RawEvents";
    var command =
        CslCommandGenerator.GenerateTableCreateCommand(
            table,
            new[]
            {
                Tuple.Create("Events", "System.Object"),
            });

    kustoClient.ExecuteControlCommand(command);
    ```

1. JSON eşlemi oluşturun.
    
    ```C#
    var tableMapping = "RawEventMapping";
    var command =
        CslCommandGenerator.GenerateTableJsonMappingCreateCommand(
            tableName,
            tableMapping,
            new[]
            {
                new JsonColumnMapping {ColumnName = "Events", JsonPath = "$"},
            });

    kustoClient.ExecuteControlCommand(command);
    ```
    Bu komut bir eşleme oluşturur ve Sütuna `Event` JSON kök yolunu `$` eşler.

1. Verileri tabloya `RawEvents` doğru yutun.

    ```C#
    var ingestUri = "https://ingest-<ClusterName>.<Region>.kusto.windows.net:443/";
    var blobPath = "https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/simple.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
    var ingestConnectionStringBuilder =
        new KustoConnectionStringBuilder(ingestUri)
        {
            FederatedSecurity = true,
            InitialCatalog = database,
            UserID = user,
            Password = password,
            Authority = tenantId
        };
    var ingestClient = KustoIngestFactory.CreateQueuedIngestClient(ingestConnectionStringBuilder);
    
    var properties =
        new KustoQueuedIngestionProperties(database, table)
        {
            Format = DataSourceFormat.json,
            IngestionMappingReference = tableMapping
        };

    ingestClient.IngestFromSingleBlob(blobPath, deleteSourceOnSuccess: false, ingestionProperties: properties);
    ```

> [!NOTE]
> Veriler toplu işleme [ilkesine](/azure/kusto/concepts/batchingpolicy)göre toplanır ve birkaç dakikalık bir gecikmeyle sonuçlanır.

# <a name="python"></a>[Python](#tab/python)

Verileri ham JSON biçiminde yutmak için Python'u kullanın.

1. Tabloyu `RawEvents` oluşturun.

    ```Python
    KUSTO_URI = "https://<ClusterName>.<Region>.kusto.windows.net:443/"
    KCSB_DATA = KustoConnectionStringBuilder.with_aad_device_authentication(KUSTO_URI, AAD_TENANT_ID)
    KUSTO_CLIENT = KustoClient(KCSB_DATA)
    TABLE = "RawEvents"

    CREATE_TABLE_COMMAND = ".create table " + TABLE + " (Events: dynamic)"
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_TABLE_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. JSON eşlemi oluşturun.

    ```Python
    MAPPING = "RawEventMapping"
    CREATE_MAPPING_COMMAND = ".create table " + TABLE + " ingestion json mapping '" + MAPPING + """' '[{"column":"Event","path":"$"}]'"""
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_MAPPING_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. Verileri tabloya `RawEvents` doğru yutun.

    ```Python
    INGEST_URI = "https://ingest-<ClusterName>.<Region>.kusto.windows.net:443/"
    KCSB_INGEST = KustoConnectionStringBuilder.with_aad_device_authentication(INGEST_URI, AAD_TENANT_ID)
    INGESTION_CLIENT = KustoIngestClient(KCSB_INGEST)
    BLOB_PATH = 'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/simple.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D'
    
    INGESTION_PROPERTIES = IngestionProperties(database=DATABASE, table=TABLE, dataFormat=DataFormat.json, mappingReference=MAPPING)
    BLOB_DESCRIPTOR = BlobDescriptor(BLOB_PATH, FILE_SIZE)
    INGESTION_CLIENT.ingest_from_blob(
        BLOB_DESCRIPTOR, ingestion_properties=INGESTION_PROPERTIES)
    ```

    > [!NOTE]
    > Veriler toplu işleme [ilkesine](/azure/kusto/concepts/batchingpolicy)göre toplanır ve birkaç dakikalık bir gecikmeyle sonuçlanır.

---

## <a name="ingest-mapped-json-records"></a>JSON kayıtlarının eşlenemelerini yutma

Bu örnekte, JSON kayıtları verilerini sindirirsiniz. Her JSON özelliği tablodaki tek bir sütuna eşlenir. 

# <a name="kql"></a>[KQL](#tab/kusto-query-language)

1. JSON giriş verilerine benzer bir şemaya sahip yeni bir tablo oluşturun. Bu tabloyu aşağıdaki örneklerin tümü için kullanacağız ve komutları yutacağız. 

    ```Kusto
    .create table Events (Time: datetime, Device: string, MessageId: string, Temperature: double, Humidity: double)
    ```

1. JSON eşlemi oluşturun.

    ```Kusto
    .create table Events ingestion json mapping 'FlatEventMapping' '[{"column":"Time","path":"$.timestamp"},{"column":"Device","path":"$.deviceId"},{"column":"MessageId","path":"$.messageId"},{"column":"Temperature","path":"$.temperature"},{"column":"Humidity","path":"$.humidity"}]'
    ```

    Tablo şemasıtarafından tanımlandığı gibi bu eşlemede, `timestamp` girişler veri türleri `Time` `datetime` olarak sütuna yutulacaktır.

1. Verileri tabloya `Events` doğru yutun.

    ```Kusto
    .ingest into table Events h'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/simple.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (format=json, jsonMappingReference=FlatEventMapping)
    ```

    'Simple.json' dosyasında birkaç satır ayrılmış JSON kaydı var. Biçimi `json`ve yutulkomutu kullanılan eşleme `FlatEventMapping` oluşturduğunuz.

# <a name="c"></a>[C #](#tab/c-sharp)

1. JSON giriş verilerine benzer bir şemaya sahip yeni bir tablo oluşturun. Bu tabloyu aşağıdaki örneklerin tümü için kullanacağız ve komutları yutacağız. 

    ```C#
    var table = "Events";
    var command =
        CslCommandGenerator.GenerateTableCreateCommand(
            table,
            new[]
            {
                Tuple.Create("Time", "System.DateTime"),
                Tuple.Create("Device", "System.String"),
                Tuple.Create("MessageId", "System.String"),
                Tuple.Create("Temperature", "System.Double"),
                Tuple.Create("Humidity", "System.Double"),
            });

    kustoClient.ExecuteControlCommand(command);
    ```

1. JSON eşlemi oluşturun.

    ```C#
    var tableMapping = "FlatEventMapping";
    var command =
        CslCommandGenerator.GenerateTableJsonMappingCreateCommand(
            tableName,
            tableMapping,
            new[]
            {
                        new JsonColumnMapping {ColumnName = "Time", JsonPath = "$.timestamp"},
                        new JsonColumnMapping {ColumnName = "Device", JsonPath = "$.deviceId"},
                        new JsonColumnMapping {ColumnName = "MessageId", JsonPath = "$.messageId"},
                        new JsonColumnMapping {ColumnName = "Temperature", JsonPath = "$.temperature"},
                        new JsonColumnMapping {ColumnName = "Humidity", JsonPath = "$.humidity"},
            });

    kustoClient.ExecuteControlCommand(command);
    ```

    Tablo şemasıtarafından tanımlandığı gibi bu eşlemede, `timestamp` girişler veri türleri `Time` `datetime` olarak sütuna yutulacaktır.    

1. Verileri tabloya `Events` doğru yutun.

    ```C#
    var blobPath = "https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/simple.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
    var properties =
        new KustoQueuedIngestionProperties(database, table)
        {
            Format = DataSourceFormat.json,
            IngestionMappingReference = tableMapping
        };

    ingestClient.IngestFromSingleBlob(blobPath, deleteSourceOnSuccess: false, ingestionProperties: properties);
    ```

    'Simple.json' dosyasında birkaç satır ayrılmış JSON kaydı var. Biçimi `json`ve yutulkomutu kullanılan eşleme `FlatEventMapping` oluşturduğunuz.

# <a name="python"></a>[Python](#tab/python)

1. JSON giriş verilerine benzer bir şemaya sahip yeni bir tablo oluşturun. Bu tabloyu aşağıdaki örneklerin tümü için kullanacağız ve komutları yutacağız. 

    ```Python
    TABLE = "RawEvents"
    CREATE_TABLE_COMMAND = ".create table " + TABLE + " (Time: datetime, Device: string, MessageId: string, Temperature: double, Humidity: double)"
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_TABLE_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. JSON eşlemi oluşturun.

    ```Python
    MAPPING = "FlatEventMapping"
    CREATE_MAPPING_COMMAND = ".create table Events ingestion json mapping '" + MAPPING + """' '[{"column":"Time","path":"$.timestamp"},{"column":"Device","path":"$.deviceId"},{"column":"MessageId","path":"$.messageId"},{"column":"Temperature","path":"$.temperature"},{"column":"Humidity","path":"$.humidity"}]'""" 
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_MAPPING_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. Verileri tabloya `Events` doğru yutun.

    ```Python
    BLOB_PATH = 'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/simple.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D'
    
    INGESTION_PROPERTIES = IngestionProperties(database=DATABASE, table=TABLE, dataFormat=DataFormat.json, mappingReference=MAPPING)
    BLOB_DESCRIPTOR = BlobDescriptor(BLOB_PATH, FILE_SIZE)
    INGESTION_CLIENT.ingest_from_blob(
        BLOB_DESCRIPTOR, ingestion_properties=INGESTION_PROPERTIES)
    ```

    'Simple.json' dosyasıjson kayıtları ayrılmış birkaç satır vardır. Biçimi `json`ve yutulkomutu kullanılan eşleme `FlatEventMapping` oluşturduğunuz.    
---

## <a name="ingest-multi-lined-json-records"></a>Çok çizgili JSON kayıtlarını yutma

Bu örnekte, çok çizgili JSON kayıtlarını sindirirsiniz. Her JSON özelliği tablodaki tek bir sütuna eşlenir. Dosya 'multilined.json' birkaç girinilmiş JSON kayıtları vardır. Biçim, `multijson` motora JSON yapısına göre kayıtları okumasını söyler.

# <a name="kql"></a>[KQL](#tab/kusto-query-language)

Verileri tabloya `Events` doğru yutun.

```Kusto
.ingest into table Events h'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/multilined.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (format=multijson, jsonMappingReference=FlatEventMapping)
```

# <a name="c"></a>[C #](#tab/c-sharp)

Verileri tabloya `Events` doğru yutun.

```C#
var tableMapping = "FlatEventMapping";
var blobPath = "https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/multilined.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
var properties =
    new KustoQueuedIngestionProperties(database, table)
    {
        Format = DataSourceFormat.multijson,
        IngestionMappingReference = tableMapping
    };

ingestClient.IngestFromSingleBlob(blobPath, deleteSourceOnSuccess: false, ingestionProperties: properties);
```

# <a name="python"></a>[Python](#tab/python)

Verileri tabloya `Events` doğru yutun.

```Python
MAPPING = "FlatEventMapping"
BLOB_PATH = 'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/multilined.JSON?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D'
INGESTION_PROPERTIES = IngestionProperties(database=DATABASE, table=TABLE, dataFormat=DataFormat.multijson, mappingReference=MAPPING)
BLOB_DESCRIPTOR = BlobDescriptor(BLOB_PATH, FILE_SIZE)
INGESTION_CLIENT.ingest_from_blob(
    BLOB_DESCRIPTOR, ingestion_properties=INGESTION_PROPERTIES)
```

---

## <a name="ingest-json-records-containing-arrays"></a>Diziler içeren JSON kayıtlarını yutma

Dizi veri türleri sıralı bir değer koleksiyonu. JSON dizisinin yutulması bir [güncelleştirme ilkesi](/azure/kusto/management/update-policy)tarafından yapılır. JSON bir ara tablo yaslanır. Güncelleştirme `RawEvents` ilkesi, sonuçları hedef tabloya yeniden ekleyerek tabloda önceden tanımlanmış bir işlev çalıştırılır. Verileri aşağıdaki yapıyla birlikte alacağız:

```json
{
    "records": 
    [
        {
            "timestamp": "2019-05-02 15:23:50.0000000",
            "deviceId": "ddbc1bf5-096f-42c0-a771-bc3dca77ac71",
            "messageId": "7f316225-839a-4593-92b5-1812949279b3",
            "temperature": 31.0301639051317,
            "humidity": 62.0791099602725
        },
        {
            "timestamp": "2019-05-02 15:23:51.0000000",
            "deviceId": "ddbc1bf5-096f-42c0-a771-bc3dca77ac71",
            "messageId": "57de2821-7581-40e4-861e-ea3bde102364",
            "temperature": 33.7529423105311,
            "humidity": 75.4787976739364
        }
    ]
}
```

# <a name="kql"></a>[KQL](#tab/kusto-query-language)

1. Koleksiyondaki `update policy` her değerin `records` `mv-expand` işleci kullanarak ayrı bir satır alması için koleksiyonu genişleten bir işlev oluşturun. Tabloyu `RawEvents` kaynak tablo ve `Events` hedef tablo olarak kullanacağız.

    ```Kusto
    .create function EventRecordsExpand() {
        RawEvents
        | mv-expand records = Event
        | project
            Time = todatetime(records["timestamp"]),
            Device = tostring(records["deviceId"]),
            MessageId = tostring(records["messageId"]),
            Temperature = todouble(records["temperature"]),
            Humidity = todouble(records["humidity"])
    }
    ```

1. İşlev tarafından alınan şema, hedef tablonun şeasını eşleştirmelidir. Şemayı incelemek için işleci kullanın. `getschema`

    ```Kusto
    EventRecordsExpand() | getschema
    ```

1. Güncelleştirme ilkesini hedef tabloya ekleyin. Bu ilke, ara tabloda `RawEvents` yeni alınan herhangi bir veri üzerinde sorguyu `Events` otomatik olarak çalıştıracak ve sonuçları tabloya yutacaktır. Ara tablonun kalıcı olmasını önlemek için sıfır bekletme ilkesi tanımlayın.

    ```Kusto
    .alter table Events policy update @'[{"Source": "RawEvents", "Query": "EventRecordsExpand()", "IsEnabled": "True"}]'
    ```

1. Verileri tabloya `RawEvents` doğru yutun.

    ```Kusto
    .ingest into table RawEvents h'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/array.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (format=multijson, jsonMappingReference=RawEventMapping)
    ```

1. Tablodaki verileri `Events` gözden geçirin.

    ```Kusto
    Events
    ```

# <a name="c"></a>[C #](#tab/c-sharp)

1. Koleksiyondaki her değerin `records` `mv-expand` işleci kullanarak ayrı bir satır alması için koleksiyonu genişleten bir güncelleştirme işlevi oluşturun. Tabloyu `RawEvents` kaynak tablo ve `Events` hedef tablo olarak kullanacağız.   

    ```C#
    var command =
        CslCommandGenerator.GenerateCreateFunctionCommand(
            "EventRecordsExpand",
            "UpdateFunctions",
            string.Empty,
            null,
            @"RawEvents
                | mv-expand records = Event
                | project
                    Time = todatetime(records['timestamp']),
                    Device = tostring(records['deviceId']),
                    MessageId = tostring(records['messageId']),
                    Temperature = todouble(records['temperature']),
                    Humidity = todouble(records['humidity'])",
            ifNotExists: false);

    kustoClient.ExecuteControlCommand(command);
    ```

    > [!NOTE]
    > İşlev tarafından alınan şema, hedef tablonun şeasını eşleştirmelidir.

1. Güncelleştirme ilkesini hedef tabloya ekleyin. Bu ilke, ara tabloda `RawEvents` yeni alınan herhangi bir veri üzerinde sorguyu `Events` otomatik olarak çalıştıracak ve sonuçlarını tabloya yutacaktır. Ara tablonun kalıcı olmasını önlemek için sıfır bekletme ilkesi tanımlayın.

    ```C#
    var command =
        ".alter table Events policy update @'[{'Source': 'RawEvents', 'Query': 'EventRecordsExpand()', 'IsEnabled': 'True'}]";

    kustoClient.ExecuteControlCommand(command);
    ```

1. Verileri tabloya `RawEvents` doğru yutun.

    ```C#
    var table = "RawEvents";
    var tableMapping = "RawEventMapping";
    var blobPath = "https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/array.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
    var properties =
        new KustoQueuedIngestionProperties(database, table)
        {
            Format = DataSourceFormat.multijson,
            IngestionMappingReference = tableMapping
        };

    ingestClient.IngestFromSingleBlob(blobPath, deleteSourceOnSuccess: false, ingestionProperties: properties);
    ```
    
1. Tablodaki verileri `Events` gözden geçirin.

# <a name="python"></a>[Python](#tab/python)

1. Koleksiyondaki her değerin `records` `mv-expand` işleci kullanarak ayrı bir satır alması için koleksiyonu genişleten bir güncelleştirme işlevi oluşturun. Tabloyu `RawEvents` kaynak tablo ve `Events` hedef tablo olarak kullanacağız.   

    ```Python
    CREATE_FUNCTION_COMMAND = 
        '''.create function EventRecordsExpand() { 
            RawEvents
            | mv-expand records = Event
            | project
                Time = todatetime(records["timestamp"]),
                Device = tostring(records["deviceId"]),
                MessageId = tostring(records["messageId"]),
                Temperature = todouble(records["temperature"]),
                Humidity = todouble(records["humidity"])
            }'''
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_FUNCTION_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

    > [!NOTE]
    > İşlev tarafından alınan şema, hedef tablonun şeasını eşleştirmek zorundadır.

1. Güncelleştirme ilkesini hedef tabloya ekleyin. Bu ilke, ara tabloda `RawEvents` yeni alınan herhangi bir veri üzerinde sorguyu `Events` otomatik olarak çalıştıracak ve sonuçlarını tabloya yutacaktır. Ara tablonun kalıcı olmasını önlemek için sıfır bekletme ilkesi tanımlayın.

    ```Python
    CREATE_UPDATE_POLICY_COMMAND = 
        """.alter table Events policy update @'[{'Source': 'RawEvents', 'Query': 'EventRecordsExpand()', 'IsEnabled': 'True'}]"""
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_UPDATE_POLICY_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. Verileri tabloya `RawEvents` doğru yutun.

    ```Python
    TABLE = "RawEvents"
    MAPPING = "RawEventMapping"
    BLOB_PATH = 'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/array.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D'
    INGESTION_PROPERTIES = IngestionProperties(database=DATABASE, table=TABLE, dataFormat=DataFormat.multijson, mappingReference=MAPPING)
    BLOB_DESCRIPTOR = BlobDescriptor(BLOB_PATH, FILE_SIZE)
    INGESTION_CLIENT.ingest_from_blob(
        BLOB_DESCRIPTOR, ingestion_properties=INGESTION_PROPERTIES)
    ```

1. Tablodaki verileri `Events` gözden geçirin.

---    

## <a name="ingest-json-records-containing-dictionaries"></a>Sözlükler içeren JSON kayıtlarını yutma

Sözlük yapılı JSON anahtar değeri çiftleri içerir. Json kayıtları, mantıksal ifade kullanılarak `JsonPath`yutma eşlemelerinden geçer. Aşağıdaki yapıyla veri yutabilirsiniz:

```json
{
    "event": 
    [
        {
            "Key": "timestamp",
            "Value": "2019-05-02 15:23:50.0000000"
        },
        {
            "Key": "deviceId",
            "Value": "ddbc1bf5-096f-42c0-a771-bc3dca77ac71"
        },
        {
            "Key": "messageId",
            "Value": "7f316225-839a-4593-92b5-1812949279b3"
        },
        {
            "Key": "temperature",
            "Value": 31.0301639051317
        },
        {
            "Key": "humidity",
            "Value": 62.0791099602725
        }
    ]
}
```

# <a name="kql"></a>[KQL](#tab/kusto-query-language)

1. Bir JSON eşleme oluşturun.

    ```Kusto
    .create table Events ingestion json mapping 'KeyValueEventMapping' '[{"column":"Time","path":"$.event[?(@.Key == 'timestamp')]"},{"column":"Device","path":"$.event[?(@.Key == 'deviceId')]"},{"column":"MessageId","path":"$.event[?(@.Key == 'messageId')]"},{"column":"Temperature","path":"$.event[?(@.Key == 'temperature')]"},{"column":"Humidity","path":"$.event[?(@.Key == 'humidity')]"}]'
    ```

1. Verileri tabloya `Events` doğru yutun.

    ```Kusto
    .ingest into table Events h'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/dictionary.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (format=multijson, jsonMappingReference=KeyValueEventMapping)
    ```

# <a name="c"></a>[C #](#tab/c-sharp)

1. Bir JSON eşleme oluşturun.

    ```C#
    var tableName = "Events";
    var tableMapping = "KeyValueEventMapping";
    var command =
        CslCommandGenerator.GenerateTableJsonMappingCreateCommand(
            tableName,
            tableMapping,
            new[]
            {
                        new JsonColumnMapping {ColumnName = "Time", JsonPath = "$.event[?(@.Key == 'timestamp')]"},
                        new JsonColumnMapping {ColumnName = "Device", JsonPath = "$.event[?(@.Key == 'deviceId')]"},
                        new JsonColumnMapping {ColumnName = "MessageId", JsonPath = "$.event[?(@.Key == 'messageId')]"},
                        new JsonColumnMapping {ColumnName = "Temperature", JsonPath = "$.event[?(@.Key == 'temperature')]"},
                        new JsonColumnMapping {ColumnName = "Humidity", JsonPath = "$.event[?(@.Key == 'humidity')]"},
            });

    kustoClient.ExecuteControlCommand(command);
    ```

1. Verileri tabloya `Events` doğru yutun.

    ```C#
    var blobPath = "https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/dictionary.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
    var properties =
        new KustoQueuedIngestionProperties(database, table)
        {
            Format = DataSourceFormat.multijson,
            IngestionMappingReference = tableMapping
        };

    ingestClient.IngestFromSingleBlob(blobPath, deleteSourceOnSuccess: false, ingestionProperties: properties);
    ```

# <a name="python"></a>[Python](#tab/python)

1. Bir JSON eşleme oluşturun.

    ```Python
    MAPPING = "KeyValueEventMapping"
    CREATE_MAPPING_COMMAND = ".create table Events ingestion json mapping '" + MAPPING + """' '[{"column":"Time","path":"$.event[?(@.Key == 'timestamp')]"},{"column":"Device","path":"$.event[?(@.Key == 'deviceId')]"},{"column":"MessageId","path":"$.event[?(@.Key == 'messageId')]"},{"column":"Temperature","path":"$.event[?(@.Key == 'temperature')]"},{"column":"Humidity","path":"$.event[?(@.Key == 'humidity')]"}]'""" 
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_MAPPING_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. Verileri tabloya `Events` doğru yutun.

     ```Python
    MAPPING = "KeyValueEventMapping"
    BLOB_PATH = 'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/dictionary.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D'
    INGESTION_PROPERTIES = IngestionProperties(database=DATABASE, table=TABLE, dataFormat=DataFormat.multijson, mappingReference=MAPPING)u
    BLOB_DESCRIPTOR = BlobDescriptor(BLOB_PATH, FILE_SIZE)
    INGESTION_CLIENT.ingest_from_blob(
        BLOB_DESCRIPTOR, ingestion_properties=INGESTION_PROPERTIES)
    ```

---    

## <a name="next-steps"></a>Sonraki adımlar

* [Veri almaya genel bakış](ingest-data-overview.md)
* [Sorgu yazma](write-queries.md)
