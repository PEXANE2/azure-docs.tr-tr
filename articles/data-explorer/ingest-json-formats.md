---
title: Azure Veri Gezgini JSON biçimli verileri alma
description: JSON biçimli verileri Azure Veri Gezgini 'a alma hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: kerend
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/23/2020
ms.openlocfilehash: ef5c7de782d833aad96516d3e5357a0ed575a781
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722882"
---
# <a name="ingest-json-formatted-sample-data-into-azure-data-explorer"></a>Azure Veri Gezgini 'de JSON biçimli örnek verileri alma

Bu makalede, JSON biçimli verileri bir Azure Veri Gezgini veritabanına alma işlemi gösterilmektedir. Ham ve eşlenmiş JSON 'ın basit örnekleri ile başlayacaksınız, çok çizgili JSON 'a devam eder ve daha sonra diziler ve sözlükler içeren daha karmaşık JSON şemaları oluşur. 

## <a name="prerequisites"></a>Ön koşullar

[Test kümesi ve veritabanı](create-cluster-database-portal.md)

## <a name="the-json-format"></a>JSON biçimi

Azure Veri Gezgini iki JSON dosya biçimini destekler:
* `json`: satıra ayrılan JSON. Giriş verilerinde bulunan her satırda tam olarak bir JSON kaydı vardır.
* `multijson`: çok çizgili JSON. Ayrıştırıcı, satır ayırıcıları yoksayar ve önceki konumdan geçerli bir JSON sonuna bir kayıt okur.

### <a name="ingest-and-map-json-formatted-data"></a>JSON biçimli verileri alma ve eşleme

JSON biçimli verilerin alımı, [alımı özelliğini](/azure/kusto/management/data-ingestion/index#ingestion-properties)kullanarak *biçimi* belirtmenizi gerektirir. JSON verilerinin alımı, bir JSON kaynak girişini hedef sütunuyla eşleyen [eşleme](/azure/kusto/management/mappings)gerektirir. Verileri içeri aktardığınızda, önceden tanımlı `jsonMappingReference` alma özelliğini kullanın veya `jsonMapping`alımı özelliğini belirtin. Bu makale, alma işlemi için kullanılan tabloda önceden tanımlanmış olan `jsonMappingReference` alımı özelliğini kullanır. Aşağıdaki örneklerde, JSON kayıtlarını tek sütunlu bir tabloya ham veri olarak kaydederek başlayacağız. Daha sonra, eşlenen sütununa her bir özelliği almak için eşlemeyi kullanacağız. 

### <a name="simple-json-example"></a>Basit JSON örneği

Aşağıdaki örnek, düz bir yapıda basit bir JSON örneğidir. Veriler, çeşitli cihazlar tarafından toplanan sıcaklık ve nem bilgilerine sahiptir. Her kayıt bir KIMLIK ve zaman damgasıyla işaretlenir.

```json
{
    "timestamp": "2019-05-02 15:23:50.0369439",
    "deviceId": "2945c8aa-f13e-4c48-4473-b81440bb5ca2",
    "messageId": "7f316225-839a-4593-92b5-1812949279b3",
    "temperature": 31.0301639051317,
    "humidity": 62.0791099602725
}
```

## <a name="ingest-raw-json-records"></a>Alma ham JSON kayıtları 

Bu örnekte, JSON kayıtlarını tek sütunlu bir tabloya ham veri olarak alırsınız. Veri işleme, sorguları kullanma ve güncelleştirme ilkesi verileri alındıktan sonra yapılır.

# <a name="kqltabkusto-query-language"></a>[KQL](#tab/kusto-query-language)

Ham JSON biçimindeki verileri almak için kusto sorgu dilini kullanın.

1. [https://dataexplorer.azure.com](https://dataexplorer.azure.com) adresinde oturum açın.

1. **Küme ekle**'yi seçin.

1. **Küme Ekle** iletişim kutusunda, `https://<ClusterName>.<Region>.kusto.windows.net/`form URL 'nizi girin ve ardından **Ekle**' yi seçin.

1. Aşağıdaki komutu yapıştırın ve tabloyu oluşturmak için **Çalıştır** ' ı seçin.

    ```Kusto
    .create table RawEvents (Event: dynamic)
    ```

    Bu sorgu, [dinamik](/azure/kusto/query/scalar-data-types/dynamic) veri türünde tek bir `Event` sütunu olan bir tablo oluşturur.

1. JSON eşlemesini oluşturun.

    ```Kusto
    .create table RawEvents ingestion json mapping 'RawEventMapping' '[{"column":"Event","path":"$"}]'
    ```

    Bu komut bir eşleme oluşturur ve `$` JSON kök yolunu `Event` sütunuyla eşler.

1. Verileri `RawEvents` tablosuna alma.

    ```Kusto
    .ingest into table RawEvents h'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/simple.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (format=json, jsonMappingReference=RawEventMapping)
    ```

    > [!NOTE]
    > Bu, doğrudan motor uç noktasına yürütülen `ingest` denetim komutlarını gösterir. Üretim senaryolarında, alım, istemci kitaplıkları veya veri bağlantıları kullanılarak Veri Yönetimi hizmetine yürütülür. [Azure Veri Gezgini Python kitaplığını kullanarak](/azure/data-explorer/python-ingest-data) verileri alma ve bu istemci kitaplıklarına veri alma hakkında bir adım adım yönergeler için [Azure Veri Gezgini .NET Standard SDK 'sını kullanarak](/azure/data-explorer/net-standard-ingest-data) verileri alma hakkında bilgi edinin.

# <a name="ctabc-sharp"></a>[C#](#tab/c-sharp)

Ham C# JSON biçimindeki verileri almak için kullanın.

1. `RawEvents` tablosunu oluşturun.

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

1. JSON eşlemesini oluşturun.
    
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
    Bu komut bir eşleme oluşturur ve `$` JSON kök yolunu `Event` sütunuyla eşler.

1. Verileri `RawEvents` tablosuna alma.

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
> Veriler, [toplu işleme ilkesine](/azure/kusto/concepts/batchingpolicy)göre toplanır ve birkaç dakika gecikme süresine neden olur.

# <a name="pythontabpython"></a>[Python](#tab/python)

Verileri ham JSON biçiminde almak için Python kullanın.

1. `RawEvents` tablosunu oluşturun.

    ```Python
    KUSTO_URI = "https://<ClusterName>.<Region>.kusto.windows.net:443/"
    KCSB_DATA = KustoConnectionStringBuilder.with_aad_device_authentication(KUSTO_URI, AAD_TENANT_ID)
    KUSTO_CLIENT = KustoClient(KCSB_DATA)
    TABLE = "RawEvents"

    CREATE_TABLE_COMMAND = ".create table " + TABLE + " (Events: dynamic)"
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_TABLE_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. JSON eşlemesini oluşturun.

    ```Python
    MAPPING = "RawEventMapping"
    CREATE_MAPPING_COMMAND = ".create table " + TABLE + " ingestion json mapping '" + MAPPING + """' '[{"column":"Event","path":"$"}]'"""
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_MAPPING_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. Verileri `RawEvents` tablosuna alma.

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
    > Veriler, [toplu işleme ilkesine](/azure/kusto/concepts/batchingpolicy)göre toplanır ve birkaç dakika gecikme süresine neden olur.

---

## <a name="ingest-mapped-json-records"></a>Eşlenen JSON kayıtlarını alma

Bu örnekte, JSON kayıtları verilerini alırsınız. Her JSON özelliği, tablodaki tek bir sütunla eşleştirilir. 

# <a name="kqltabkusto-query-language"></a>[KQL](#tab/kusto-query-language)

1. JSON giriş verilerinde benzer bir şemaya sahip yeni bir tablo oluşturun. Aşağıdaki tüm örnekler ve alma komutları için bu tabloyu kullanacağız. 

    ```Kusto
    .create table Events (Time: datetime, Device: string, MessageId: string, Temperature: double, Humidity: double)
    ```

1. JSON eşlemesini oluşturun.

    ```Kusto
    .create table Events ingestion json mapping 'FlatEventMapping' '[{"column":"Time","path":"$.timestamp"},{"column":"Device","path":"$.deviceId"},{"column":"MessageId","path":"$.messageId"},{"column":"Temperature","path":"$.temperature"},{"column":"Humidity","path":"$.humidity"}]'
    ```

    Bu eşlemede, tablo şeması tarafından tanımlandığı gibi, `timestamp` girdileri `datetime` veri türleri olarak `Time` sütuna alınır.

1. Verileri `Events` tablosuna alma.

    ```Kusto
    .ingest into table Events h'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/simple.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (format=json, jsonMappingReference=FlatEventMapping)
    ```

    ' Simple. json ' dosyasında birkaç satır ayrılmış JSON kaydı vardır. Biçim `json`ve Al komutunda kullanılan eşleme, oluşturduğunuz `FlatEventMapping`.

# <a name="ctabc-sharp"></a>[C#](#tab/c-sharp)

1. JSON giriş verilerinde benzer bir şemaya sahip yeni bir tablo oluşturun. Aşağıdaki tüm örnekler ve alma komutları için bu tabloyu kullanacağız. 

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

1. JSON eşlemesini oluşturun.

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

    Bu eşlemede, tablo şeması tarafından tanımlandığı gibi, `timestamp` girdileri `datetime` veri türleri olarak `Time` sütuna alınır.    

1. Verileri `Events` tablosuna alma.

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

    ' Simple. json ' dosyasında birkaç satır ayrılmış JSON kaydı vardır. Biçim `json`ve Al komutunda kullanılan eşleme, oluşturduğunuz `FlatEventMapping`.

# <a name="pythontabpython"></a>[Python](#tab/python)

1. JSON giriş verilerinde benzer bir şemaya sahip yeni bir tablo oluşturun. Aşağıdaki tüm örnekler ve alma komutları için bu tabloyu kullanacağız. 

    ```Python
    TABLE = "RawEvents"
    CREATE_TABLE_COMMAND = ".create table " + TABLE + " (Time: datetime, Device: string, MessageId: string, Temperature: double, Humidity: double)"
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_TABLE_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. JSON eşlemesini oluşturun.

    ```Python
    MAPPING = "FlatEventMapping"
    CREATE_MAPPING_COMMAND = ".create table Events ingestion json mapping '" + MAPPING + """' '[{"column":"Time","path":"$.timestamp"},{"column":"Device","path":"$.deviceId"},{"column":"MessageId","path":"$.messageId"},{"column":"Temperature","path":"$.temperature"},{"column":"Humidity","path":"$.humidity"}]'""" 
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_MAPPING_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. Verileri `Events` tablosuna alma.

    ```Python
    BLOB_PATH = 'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/simple.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D'
    
    INGESTION_PROPERTIES = IngestionProperties(database=DATABASE, table=TABLE, dataFormat=DataFormat.json, mappingReference=MAPPING)
    BLOB_DESCRIPTOR = BlobDescriptor(BLOB_PATH, FILE_SIZE)
    INGESTION_CLIENT.ingest_from_blob(
        BLOB_DESCRIPTOR, ingestion_properties=INGESTION_PROPERTIES)
    ```

    ' Simple. json ' dosyasında birkaç satır ayrılmış JSON kaydı vardır. Biçim `json`ve Al komutunda kullanılan eşleme, oluşturduğunuz `FlatEventMapping`.    
---

## <a name="ingest-multi-lined-json-records"></a>Çok çizgili JSON kayıtlarını alma

Bu örnekte, çok çizgili JSON kayıtlarını alırsınız. Her JSON özelliği, tablodaki tek bir sütunla eşleştirilir. ' Multilined. json ' dosyasında birkaç girintili JSON kaydı vardır. Biçim `multijson`, altyapıya JSON yapısına göre kayıtları okumasını söyler.

# <a name="kqltabkusto-query-language"></a>[KQL](#tab/kusto-query-language)

Verileri `Events` tablosuna alma.

```Kusto
.ingest into table Events h'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/multilined.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (format=multijson, jsonMappingReference=FlatEventMapping)
```

# <a name="ctabc-sharp"></a>[C#](#tab/c-sharp)

Verileri `Events` tablosuna alma.

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

# <a name="pythontabpython"></a>[Python](#tab/python)

Verileri `Events` tablosuna alma.

```Python
MAPPING = "FlatEventMapping"
BLOB_PATH = 'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/multilined.JSON?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D'
INGESTION_PROPERTIES = IngestionProperties(database=DATABASE, table=TABLE, dataFormat=DataFormat.multijson, mappingReference=MAPPING)
BLOB_DESCRIPTOR = BlobDescriptor(BLOB_PATH, FILE_SIZE)
INGESTION_CLIENT.ingest_from_blob(
    BLOB_DESCRIPTOR, ingestion_properties=INGESTION_PROPERTIES)
```

---

## <a name="ingest-json-records-containing-arrays"></a>Dizileri içeren JSON kayıtları alma

Dizi veri türleri sıralı değerler koleksiyonudur. JSON dizisinin alımı, bir [güncelleştirme ilkesi](/azure/kusto/management/update-policy)tarafından yapılır. JSON, bir ara tabloya göre yapılır. Bir güncelleştirme ilkesi `RawEvents` tabloda önceden tanımlanmış bir işlevi çalıştırır ve sonuçları hedef tabloya geri alır. Aşağıdaki yapıyla verileri alacak:

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

# <a name="kqltabkusto-query-language"></a>[KQL](#tab/kusto-query-language)

1. Koleksiyondaki her değerin `mv-expand` işlecini kullanarak ayrı bir satır alması için `records` koleksiyonunu genişleten bir `update policy` işlevi oluşturun. Tablo `RawEvents` kaynak tablo olarak ve hedef tablo olarak `Events` kullanacağız.

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

1. İşlevin aldığı şemanın hedef tablonun şemasıyla eşleşmesi gerekir. Şemayı gözden geçirmek için `getschema` işlecini kullanın.

    ```Kusto
    EventRecordsExpand() | getschema
    ```

1. Güncelleştirme ilkesini hedef tabloya ekleyin. Bu ilke, sorgu `RawEvents` ara tablodaki yeni verileri otomatik olarak çalıştırır ve sonuçları `Events` tablosuna alır. Ara tabloyu kalıcı hale getirmeyi önlemek için sıfır bekletme ilkesi tanımlayın.

    ```Kusto
    .alter table Events policy update @'[{"Source": "RawEvents", "Query": "EventRecordsExpand()", "IsEnabled": "True"}]'
    ```

1. Verileri `RawEvents` tablosuna alma.

    ```Kusto
    .ingest into table Events h'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/array.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (format=multijson, jsonMappingReference=RawEventMapping)
    ```

1. `Events` tablosundaki verileri gözden geçirin.

    ```Kusto
    Events
    ```

# <a name="ctabc-sharp"></a>[C#](#tab/c-sharp)

1. Koleksiyondaki her değerin `mv-expand` işlecini kullanarak ayrı bir satır alması için `records` koleksiyonunu genişleten bir güncelleştirme işlevi oluşturun. Tablo `RawEvents` kaynak tablo olarak ve hedef tablo olarak `Events` kullanacağız.   

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
    > İşlevin aldığı şemanın hedef tablonun şemasıyla eşleşmesi gerekir.

1. Güncelleştirme ilkesini hedef tabloya ekleyin. Bu ilke, sorguyu `RawEvents` ara tablodaki yeni verileri otomatik olarak çalıştırır ve sonuçlarını `Events` tablosuna alır. Ara tabloyu kalıcı hale getirmeyi önlemek için sıfır bekletme ilkesi tanımlayın.

    ```C#
    var command =
        ".alter table Events policy update @'[{'Source': 'RawEvents', 'Query': 'EventRecordsExpand()', 'IsEnabled': 'True'}]";

    kustoClient.ExecuteControlCommand(command);
    ```

1. Verileri `RawEvents` tablosuna alma.

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
    
1. `Events` tablosundaki verileri gözden geçirin.

# <a name="pythontabpython"></a>[Python](#tab/python)

1. Koleksiyondaki her değerin `mv-expand` işlecini kullanarak ayrı bir satır alması için `records` koleksiyonunu genişleten bir güncelleştirme işlevi oluşturun. Tablo `RawEvents` kaynak tablo olarak ve hedef tablo olarak `Events` kullanacağız.   

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
    > İşlevin aldığı şemanın, hedef tablonun şemasıyla eşleşmesi vardır.

1. Güncelleştirme ilkesini hedef tabloya ekleyin. Bu ilke, sorguyu `RawEvents` ara tablodaki yeni verileri otomatik olarak çalıştırır ve sonuçlarını `Events` tablosuna alır. Ara tabloyu kalıcı hale getirmeyi önlemek için sıfır bekletme ilkesi tanımlayın.

    ```Python
    CREATE_UPDATE_POLICY_COMMAND = 
        """.alter table Events policy update @'[{'Source': 'RawEvents', 'Query': 'EventRecordsExpand()', 'IsEnabled': 'True'}]"""
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_UPDATE_POLICY_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. Verileri `RawEvents` tablosuna alma.

    ```Python
    TABLE = "RawEvents"
    MAPPING = "RawEventMapping"
    BLOB_PATH = 'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/array.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D'
    INGESTION_PROPERTIES = IngestionProperties(database=DATABASE, table=TABLE, dataFormat=DataFormat.multijson, mappingReference=MAPPING)
    BLOB_DESCRIPTOR = BlobDescriptor(BLOB_PATH, FILE_SIZE)
    INGESTION_CLIENT.ingest_from_blob(
        BLOB_DESCRIPTOR, ingestion_properties=INGESTION_PROPERTIES)
    ```

1. `Events` tablosundaki verileri gözden geçirin.

---    

## <a name="ingest-json-records-containing-dictionaries"></a>Sözlükleri içeren JSON kayıtları alma

Sözlük yapılandırılmış JSON anahtar-değer çiftleri içeriyor. JSON kayıtları, `JsonPath`mantıksal ifade kullanan eşleme eşlemesini ihlal ediyor. Verileri aşağıdaki yapıyla alabilirsiniz:

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

# <a name="kqltabkusto-query-language"></a>[KQL](#tab/kusto-query-language)

1. JSON eşlemesi oluşturun.

    ```Kusto
    .create table Events ingestion json mapping 'KeyValueEventMapping' '[{"column":"Time","path":"$.event[?(@.Key == 'timestamp')]"},{"column":"Device","path":"$.event[?(@.Key == 'deviceId')]"},{"column":"MessageId","path":"$.event[?(@.Key == 'messageId')]"},{"column":"Temperature","path":"$.event[?(@.Key == 'temperature')]"},{"column":"Humidity","path":"$.event[?(@.Key == 'humidity')]"}]'
    ```

1. Verileri `Events` tablosuna alma.

    ```Kusto
    .ingest into table Events h'https://kustosamplefiles.blob.core.windows.net/jsonsamplefiles/dictionary.json?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (format=multijson, jsonMappingReference=KeyValueEventMapping)
    ```

# <a name="ctabc-sharp"></a>[C#](#tab/c-sharp)

1. JSON eşlemesi oluşturun.

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

1. Verileri `Events` tablosuna alma.

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

# <a name="pythontabpython"></a>[Python](#tab/python)

1. JSON eşlemesi oluşturun.

    ```Python
    MAPPING = "KeyValueEventMapping"
    CREATE_MAPPING_COMMAND = ".create table Events ingestion json mapping '" + MAPPING + """' '[{"column":"Time","path":"$.event[?(@.Key == 'timestamp')]"},{"column":"Device","path":"$.event[?(@.Key == 'deviceId')]"},{"column":"MessageId","path":"$.event[?(@.Key == 'messageId')]"},{"column":"Temperature","path":"$.event[?(@.Key == 'temperature')]"},{"column":"Humidity","path":"$.event[?(@.Key == 'humidity')]"}]'""" 
    RESPONSE = KUSTO_CLIENT.execute_mgmt(DATABASE, CREATE_MAPPING_COMMAND)
    dataframe_from_result_table(RESPONSE.primary_results[0])
    ```

1. Verileri `Events` tablosuna alma.

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

* [Veri alımı genel bakış](ingest-data-overview.md)
* [Sorgu yazma](write-queries.md)