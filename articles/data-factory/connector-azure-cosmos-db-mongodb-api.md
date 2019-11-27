---
title: Data Factory kullanarak MongoDB için Azure Cosmos DB API 'sine veri kopyalama
description: Data Factory kullanarak desteklenen kaynak veri depolarından veya MongoDB için Azure Cosmos DB API 'sine olan verileri desteklenen havuz depolarına kopyalama hakkında bilgi edinin.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: jingwang
ms.openlocfilehash: 0da3452964a7c4bb7d2a22ce4cd5164ad8c1e3fb
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280702"
---
# <a name="copy-data-to-or-from-azure-cosmos-dbs-api-for-mongodb-by-using-azure-data-factory"></a>Azure Data Factory kullanarak MongoDB için Azure Cosmos DB API 'sine veri kopyalama

Bu makalede, MongoDB için Azure Cosmos DB API 'sinden ve ' den veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Makale, kopyalama etkinliğine genel bir bakış sunan [Azure Data Factory kopyalama etkinliği](copy-activity-overview.md)üzerinde oluşturulur.

>[!NOTE]
>Bu bağlayıcı yalnızca MongoDB için Azure Cosmos DB API 'sine veri kopyalamayı destekler. SQL API 'SI için [Cosmos db SQL API Bağlayıcısı](connector-azure-cosmos-db.md)' na bakın. Diğer API türleri şu anda desteklenmiyor.

## <a name="supported-capabilities"></a>Desteklenen özellikler

MongoDB için Azure Cosmos DB API 'sindeki verileri desteklenen herhangi bir havuz veri deposuna kopyalayabilir veya desteklenen herhangi bir kaynak veri deposundan verileri MongoDB için Azure Cosmos DB API 'sine kopyalayabilirsiniz. Kopyalama etkinliğinin kaynak ve havuz olarak desteklediği veri depolarının bir listesi için bkz. [desteklenen veri depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats).

MongoDB Bağlayıcısı için Azure Cosmos DB API 'sini kullanarak şunları yapabilirsiniz:

- Ve [Azure Cosmos DB MongoDB IÇIN API](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction)'sine verileri kopyalayın.
- **Insert** veya **upsert**olarak Azure Cosmos DB yazın.
- İçeri aktarma ve JSON belgeleri olarak dışarı aktarma- ya da ya da tablolu bir veri kümesine veri kopyalayın. SQL veritabanı ve bir CSV dosyası verilebilir. Belgeleri JSON dosyalarından ya da başka bir Azure Cosmos DB koleksiyonuna veya farklı bir koleksiyon olarak kopyalamak için bkz. JSON belgelerini Içeri veya dışarı aktarma.

## <a name="get-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, MongoDB için Azure Cosmos DB API 'sine özgü Data Factory varlıkları tanımlamak için kullanabileceğiniz özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler, Azure Cosmos DB MongoDB bağlı hizmeti için API 'SI için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Type** özelliği **Cosmosdbmongodbapı**olarak ayarlanmalıdır. | Yes |
| connectionString |MongoDB için Azure Cosmos DB API 'niz için bağlantı dizesi belirtin. Azure portal > Cosmos DB dikey pencere > birincil veya ikincil bağlantı dizeniz `mongodb://<cosmosdb-name>:<password>@<cosmosdb-name>.documents.azure.com:10255/?ssl=true&replicaSet=globaldb`düzeniyle bulabilirsiniz. <br/><br />Data Factory güvenli bir şekilde depolamak için bu alanı **SecureString** türü olarak işaretleyin. Ayrıca, [Azure Key Vault depolanan bir gizli](store-credentials-in-key-vault.md)dizi için de başvurabilirsiniz. |Yes |
| database | Erişmek istediğiniz veritabanının adı. | Yes |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Integration Runtime](concepts-integration-runtime.md) . (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Bu özellik belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek**

```json
{
    "name": "CosmosDbMongoDBAPILinkedService",
    "properties": {
        "type": "CosmosDbMongoDbApi",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "mongodb://<cosmosdb-name>:<password>@<cosmosdb-name>.documents.azure.com:10255/?ssl=true&replicaSet=globaldb"
            },
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

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). Aşağıdaki özellikler Azure Cosmos DB MongoDB veri kümesi için API 'SI için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin **Type** özelliği **Cosmosdbmongodbapicollection**olarak ayarlanmalıdır. |Yes |
| collectionName |Azure Cosmos DB koleksiyonunun adı. |Yes |

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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölüm, Azure Cosmos DB MongoDB kaynak ve havuz desteği için API 'sinin özelliklerinin bir listesini sağlar.

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md).

### <a name="azure-cosmos-dbs-api-for-mongodb-as-source"></a>Kaynak olarak MongoDB için API Azure Cosmos DB

Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının **Type** özelliği **Cosmosdbmongodbapısource**olarak ayarlanmalıdır. |Yes |
| filter | Sorgu işleçlerini kullanarak seçim filtresini belirtir. Bir koleksiyondaki tüm belgeleri döndürmek için, bu parametreyi atlayın veya boş bir belge ({}) geçirin. | Hayır |
| cursorMethods. Project | Projeksiyon için belgelere döndürülecek alanları belirtir. Eşleşen belgelerdeki tüm alanları döndürmek için bu parametreyi atlayın. | Hayır |
| cursorMethods. Sort | Sorgunun eşleşen belgeleri döndürdüğü sırayı belirtir. [İmleç. Sort ()](https://docs.mongodb.com/manual/reference/method/cursor.sort/#cursor.sort)öğesine bakın. | Hayır |
| cursorMethods. limit | Sunucunun döndürdüğü en fazla belge sayısını belirtir. [İmleç. limit ()](https://docs.mongodb.com/manual/reference/method/cursor.limit/#cursor.limit)öğesine bakın.  | Hayır | 
| cursorMethods. Skip | Atlanacak belge sayısını ve MongoDB 'nin sonuçları döndürmek için başladığı yeri belirtir. [İmleç. Skip ()](https://docs.mongodb.com/manual/reference/method/cursor.skip/#cursor.skip)öğesine başvurun. | Hayır |
| batchSize | MongoDB örneğinden alınan yanıtın her bir toplu işinde döndürülecek belge sayısını belirtir. Çoğu durumda, toplu iş boyutunu değiştirmek Kullanıcı veya uygulamayı etkilemez. Cosmos DB sınırlar, her toplu işlem, belge boyutunun batchSize toplam sayısı olan 40MB 'ı aşamaz, bu nedenle belge boyutunuz büyükse bu değeri azaltın. | Hayır<br/>(varsayılan değer **100**' dir) |

>[!TIP]
>ADF desteği BSON belgeyi **katı modda**kullanıyor. Filtre sorgunuzun kabuk modu yerine katı modda olduğundan emin olun. [MongoDB el ile](https://docs.mongodb.com/manual/reference/mongodb-extended-json/index.html)daha fazla açıklama bulabilirsiniz.

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

### <a name="azure-cosmos-dbs-api-for-mongodb-as-sink"></a>Havuz olarak MongoDB için API Azure Cosmos DB

Aşağıdaki özellikler, kopyalama etkinliği **havuzu** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun **Type** özelliği **CosmosDbMongoDbApiSink**olarak ayarlanmalıdır. |Yes |
| writeBehavior |Azure Cosmos DB'ye veri yazmak açıklar. İzin verilen değerler: **Insert** ve **upsert**.<br/><br/>Aynı `_id` bir belge zaten mevcutsa, **upsert** davranışı belgeyi değiştirmek için kullanılır. Aksi takdirde, belgeyi ekleyin.<br /><br />**Note**: özgün belgede veya sütun eşlemesinde bir `_id` belirtilmediyse, Data Factory otomatik olarak bir belge `_id` oluşturur. Yani, **büyük** bir şekilde çalışması için BELGENIZDE bir kimliğe sahip olduğundan emin olmanız gerekir. |Hayır<br />(varsayılan değer **Insert**'dir) |
| writeBatchSize | **Writebatchsize** özelliği, her toplu işte yazılacak belgelerin boyutunu denetler. Performansı artırmak ve belge boyutunuz büyükse değeri azaltmak için **Writebatchsize** değerini artırmayı deneyebilirsiniz. |Hayır<br />(varsayılan değer **10.000**' dir) |
| writeBatchTimeout | Toplu iş ekleme işleminin, zaman aşımına uğramadan önce tamamlaması için bekleme süresi. İzin verilen değer TimeSpan değeridir. | Hayır<br/>(varsayılan değer **00:30:00** -30 dakikadır) |

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

>[!TIP]
>JSON belgelerini olduğu gibi içeri aktarmak için [JSON belgelerini içeri veya dışarı aktarma](#import-or-export-json-documents) bölümüne bakın; tablosal şekillendirilmiş verilerden kopyalamak için [şema eşlemesine](#schema-mapping)bakın.

## <a name="import-or-export-json-documents"></a>JSON belgelerini içeri veya dışarı aktarma

Bu Azure Cosmos DB Bağlayıcısı için kolayca kullanabilirsiniz:

* JSON belgeleri için Azure Cosmos DB, Azure Blob Depolama, Azure Data Lake Store ve Azure Data Factory destekleyen diğer dosya tabanlı depoları da dahil olmak üzere çeşitli kaynaklardan içeri aktarın.
* JSON belgeleri bir Azure Cosmos DB koleksiyonundan çeşitli dosya tabanlı depoları dışarı aktarın.
* Belgeler iki Azure Cosmos DB koleksiyonları arasında kopyalama-olduğu.

Bu tür şemadan bağımsız bir kopya elde etmek için, kopyalama etkinliğinde veri kümesi ve şema eşleme ' de "yapı" (kısaca *şema*) bölümünü atlayın.

## <a name="schema-mapping"></a>Şema eşleme

MongoDB için Azure Cosmos DB API 'sinden tablo havuzuna veya ters çevrilmesine veri kopyalamak için [şema eşlemesine](copy-activity-schema-and-type-mapping.md#schema-mapping)bakın.

Özellikle Cosmos DB yazmak için, Cosmos DB kaynak verilerinizde doğru nesne KIMLIĞIYLE doldurduğunuzdan emin olmak için, SQL veritabanı tablosunda bir "kimlik" sütunumuz ve INSERT/upsert için MongoDB içindeki belge KIMLIĞI olarak bu değeri kullanmak istiyorsunuz , aşağıdaki gibi MongoDB katı mod tanımına (`_id.$oid`) göre uygun şema eşlemesini ayarlamanız gerekir:

![MongoDB havuzunda eşleme KIMLIĞI](./media/connector-azure-cosmos-db-mongodb-api/map-id-in-mongodb-sink.png)

Kopyalama etkinliği yürütmeden sonra, BSON ObjectID 'nin aşağıdaki havuzda oluşturulması gerekir:

```json
{
    "_id": ObjectId("592e07800000000000000000")
}
``` 

## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliğinin Azure Data Factory kaynak ve havuz olarak desteklediği veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
