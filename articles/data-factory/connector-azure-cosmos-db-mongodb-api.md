---
title: Azure Cosmos DB'nin MongoDB api'sinden verileri kopyalama
description: Desteklenen kaynak veri depolarından Azure Cosmos DB'nin MongoDB api'sine veri fabrikasını kullanarak desteklenen lavabo depolarına nasıl kopyalaylayamamayı öğrenin.
services: data-factory, cosmosdb
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/20/2019
ms.openlocfilehash: 9b23f46a418f2663531cc121f00b83d00d84e48d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415456"
---
# <a name="copy-data-to-or-from-azure-cosmos-dbs-api-for-mongodb-by-using-azure-data-factory"></a>Azure Data Factory'yi kullanarak MongoDB için Azure Cosmos DB'nin API'sinden API'sine veri kopyalama (SQL API)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makale, MongoDB için Azure Cosmos DB API’sinden veri kopyalamak (veya buna kopyalamak) için Azure Data Factory’deki Kopyalama Etkinliğini kullanmayı özetler. Makale, Azure [Veri Fabrikası'ndaki Kopyalama Etkinliği'ne](copy-activity-overview.md)dayanmaktadır ve bu da Kopyalama Etkinliğine genel bir genel bakış sunar.

>[!NOTE]
>Bu bağlayıcı yalnızca Azure Cosmos DB'nin MongoDB için API'sine kopya verilerini destekler. SQL API için [Cosmos DB SQL API konektörüne](connector-azure-cosmos-db.md)bakın. Diğer API türleri artık desteklenmiyor.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Azure Cosmos DB'nin MongoDB için API'sinden desteklenen herhangi bir lavabo veri deposuna verileri kopyalayabilir veya desteklenen herhangi bir kaynak veri deposundaki verileri Azure Cosmos DB'nin MongoDB için API'sine kopyalayabilirsiniz. Kopyalama Etkinliği'nin kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına ve biçimlere](copy-activity-overview.md#supported-data-stores-and-formats)bakın.

MongoDB konektörü için Azure Cosmos DB'nin API'sini şu şekilde kullanabilirsiniz:

- [Verileri MongoDB için Azure Cosmos DB'nin API'sine](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction)kopyalayın.
- Azure Cosmos DB'ye **ekleme** veya **yükseltme**olarak yazın.
- JSON belgelerini olduğu gibi içe aktarın ve dışa aktarın veya verileri bir tabular veri kümesinden kopyalayın veya kopyalayın. Örnekler arasında bir SQL veritabanı ve csv dosyası yer almaktadır. Belgeleri JSON dosyalarına veya başka bir Azure Cosmos DB koleksiyonundan veya başka bir Azure Cosmos DB koleksiyonundan olduğu gibi kopyalamak için JSON belgelerini içe veya dışa aktarmaya bakın.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Azure Cosmos DB'nin MongoDB için API'sine özgü Veri Fabrikası varlıklarını tanımlamak için kullanabileceğiniz özellikler hakkında ayrıntılı bilgi verilir.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, Azure Cosmos DB'nin MongoDB bağlantılı hizmet için API'si için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Tür** özelliği **CosmosDbMongoDbApi**olarak ayarlanmalıdır. | Evet |
| Connectionstring |Azure Cosmos DB'nizin MongoDB için API'sinin bağlantı dizesini belirtin. Cosmos DB bıçak -> birincil veya ikincil bağlantı dizenizi > Azure portalında `mongodb://<cosmosdb-name>:<password>@<cosmosdb-name>.documents.azure.com:10255/?ssl=true&replicaSet=globaldb`bulabilirsiniz. <br/><br />Ayrıca Azure Key Vault'a bir parola `password` koyabilir ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz.Azure [Anahtar Kasası'ndaki](store-credentials-in-key-vault.md) Mağaza kimlik bilgilerine daha fazla ayrıntı yla bakın.|Evet |
| database | Erişmek istediğiniz veritabanının adı. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Zamanı'nı veya kendi kendine barındırılan tümleştirme çalışma zamanını (veri deponuz özel bir ağda bulunuyorsa) kullanabilirsiniz. Bu özellik belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresi kullanılır. |Hayır |

**Örnek**

```json
{
    "name": "CosmosDbMongoDBAPILinkedService",
    "properties": {
        "type": "CosmosDbMongoDbApi",
        "typeProperties": {
            "connectionString": "mongodb://<cosmosdb-name>:<password>@<cosmosdb-name>.documents.azure.com:10255/?ssl=true&replicaSet=globaldb",
            "database": "myDatabase"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Bkz. Veri Kümeleri ve bağlantılı hizmetler.](concepts-datasets-linked-services.md) Aşağıdaki özellikler, Azure Cosmos DB'nin MongoDB veri kümesi için API'si için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin **tür** özelliği **CosmosDbMongoDbApiCollection**olarak ayarlanmalıdır. |Evet |
| Toplamaadı |Azure Cosmos DB koleksiyonunun adı. |Evet |

**Örnek**

```json
{
    "name": "CosmosDbMongoDBAPIDataset",
    "properties": {
        "type": "CosmosDbMongoDbApiCollection",
        "typeProperties": {
            "collectionName": "<collection name>"
        },
        "schema": [],
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB's API for MongoDB linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama

Bu bölümde, Azure Cosmos DB'nin MongoDB kaynağı ve lavabo desteği için API'sinin özelliklerinin bir listesi yer almaktadır.

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md)

### <a name="azure-cosmos-dbs-api-for-mongodb-as-source"></a>Kaynak olarak MongoDB için Azure Cosmos DB'nin API'si

Aşağıdaki özellikler Kopyalama Etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının **türü** özelliği **CosmosDbMongoDbApiSource**olarak ayarlanmalıdır. |Evet |
| filtre | Sorgu işleçlerini kullanarak seçim filtresini belirtir. Koleksiyondaki tüm belgeleri döndürmek için, bu parametreyi atla{}veya boş bir belgeyi geçirin ( ). | Hayır |
| imleçMethods.project | Projeksiyon için belgelerde döndürülecek alanları belirtir. Eşleşen belgelerdeki tüm alanları döndürmek için bu parametreyi atla. | Hayır |
| imleçMethods.sort | Sorgunun eşleşen belgeleri döndürme sırasını belirtir. [Cursor.sort()](https://docs.mongodb.com/manual/reference/method/cursor.sort/#cursor.sort)bakın. | Hayır |
| imleçMethods.limit | Sunucunun döndürdğü en fazla belge sayısını belirtir. [Cursor.limit()](https://docs.mongodb.com/manual/reference/method/cursor.limit/#cursor.limit)bakın.  | Hayır | 
| imleçMethods.skip | Atlasılaş acak belge sayısını ve MongoDB'nin sonuçları döndürmeye başladığı yeri belirtir. [Cursor.skip()](https://docs.mongodb.com/manual/reference/method/cursor.skip/#cursor.skip)bakın. | Hayır |
| batchSize | MongoDB örneğinden yanıtın her bir toplu halinde döndürülecek belge sayısını belirtir. Çoğu durumda, toplu iş boyutunu değiştirmek kullanıcıyı veya uygulamayı etkilemez. Cosmos DB sınırları her toplu iş boyutu 40MB geçemez, hangi belgelerin toplu Boyut sayısının toplamıdır, bu nedenle belge boyutu büyük ise bu değeri azaltmak. | Hayır<br/>(varsayılan **değer 100'dür)** |

>[!TIP]
>ADF, BSON belgesini **Sıkı modda**tüketen desteği destekler. Filtre sorgunuzun Shell modu yerine Katı modda olduğundan emin olun. Daha fazla açıklama [MongoDB kılavuzunda](https://docs.mongodb.com/manual/reference/mongodb-extended-json/index.html)bulunabilir.

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromCosmosDBMongoDBAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Cosmos DB's API for MongoDB input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "CosmosDbMongoDbApiSource",
                "filter": "{datetimeData: {$gte: ISODate(\"2018-12-11T00:00:00.000Z\"),$lt: ISODate(\"2018-12-12T00:00:00.000Z\")}, _id: ObjectId(\"5acd7c3d0000000000000000\") }",
                "cursorMethods": {
                    "project": "{ _id : 1, name : 1, age: 1, datetimeData: 1 }",
                    "sort": "{ age : 1 }",
                    "skip": 3,
                    "limit": 3
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-dbs-api-for-mongodb-as-sink"></a>MongoDB için Azure Cosmos DB'nin API'si

Aşağıdaki özellikler Kopyalama Etkinliği **lavabo** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama Etkinliği **lavabonun türü** özelliği **CosmosDbMongoDbApiSink**olarak ayarlanmalıdır. |Evet |
| yazmaDavranışı |Azure Cosmos DB'ye nasıl veri yazılabildiğini açıklar. İzin verilen değerler: ekle ve **yukarı sa.** **insert**<br/><br/>**Upsert'in** davranışı, belgenin aynısına `_id` sahip bir belge varsa belgenin değiştirilmesidir; aksi takdirde, belgeyi ekleyin.<br /><br />**Not**: Veri Fabrikası, `_id` özgün belgede veya `_id` sütun eşlemesinde belirtilmemişse, belge için otomatik olarak bir belge oluşturur. Bu, **yükseltmenin** beklendiği gibi çalışması için belgenizin bir kimliği olduğundan emin olmak gerektiği anlamına gelir. |Hayır<br />(varsayılan **insert)** |
| yazmaBatchSize | **writeBatchSize** özelliği, her toplu iş partisinde yazılabilmek için belgelerin boyutunu denetler. Performansı artırmak için **writeBatchSize** değerini artırmayı ve belge boyutunuz büyükse değeri azaltmayı deneyebilirsiniz. |Hayır<br />(varsayılan **değer 10.000'dir)** |
| yazmaBatchTimeout | Toplu ekleme işleminin zaman dolmadan bitmeden tamamlanması için bekleme süresi. İzin verilen değer zaman asıdır. | Hayır<br/>(varsayılan **değer 00:30:00** - 30 dakikadır) |

>[!TIP]
>JSON belgelerini olduğu gibi almak için, [JSON belgelerini içe aktarma veya dışa aktarma](#import-and-export-json-documents) bölümüne bakın; tabular şekilli verilerden kopyalamak için [Şema eşleciliği'ne](#schema-mapping)bakın.

**Örnek**

```json
"activities":[
    {
        "name": "CopyToCosmosDBMongoDBAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Document DB output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "CosmosDbMongoDbApiSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

## <a name="import-and-export-json-documents"></a>JSON belgelerini alma ve dışa aktarma

Bu Azure Cosmos DB konektörünü kolayca kullanabilirsiniz:

* Belgeleri olduğu gibi iki Azure Cosmos DB koleksiyonu arasında kopyalayın.
* JSON belgelerini MongoDB, Azure Blob depolama, Azure Veri Gölü Deposu ve Azure Veri Fabrikası'nın desteklediği diğer dosya tabanlı mağazalar da dahil olmak üzere çeşitli kaynaklardan Azure Cosmos DB'ye aktarın.
* JSON belgelerini Azure Cosmos DB koleksiyonundan çeşitli dosya tabanlı mağazalara dışa aktarın.

Şema-agnostik kopya elde etmek için:

* Verileri Kopyala aracını kullandığınızda, **JSON dosyalarına veya Cosmos DB koleksiyonu seçeneğine olduğu gibi dışa** aktar'ı seçin.
* Etkinlik yazma yı kullandığınızda, kaynak veya lavabo için ilgili dosya deposuyla JSON biçimini seçin.

## <a name="schema-mapping"></a>Şema haritalama

Azure Cosmos DB'nin MongoDB api'sinden verileri tabular lavaboya veya tersine kopyalamak için [şema eşleciliği'ne](copy-activity-schema-and-type-mapping.md#schema-mapping)bakın.

Cosmos DB'ye yazmak için özellikle, cosmos DB'yi kaynak verilerinizden doğru nesne kimliğiyle doldurabildiğinizden emin olmak için, örneğin, SQL veritabanı tablosunda bir "id" sütununuz var ve bunun değerini MongoDB'deki belge kimliği olarak`_id.$oid`eklemek/eklemek için kullanmak istiyorsanız, MongoDB sıkı mod tanımına göre uygun şema eşlemesini ayarlamanız gerekir ( ) aşağıdaki gibi:

![MongoDB lavaboda Harita kimliği](./media/connector-azure-cosmos-db-mongodb-api/map-id-in-mongodb-sink.png)

Kopyalama etkinliği yürütülmesinden sonra, aşağıdaki BSON ObjectId lavaboda oluşturulur:

```json
{
    "_id": ObjectId("592e07800000000000000000")
}
``` 

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası'nda Copy Activity'in kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
