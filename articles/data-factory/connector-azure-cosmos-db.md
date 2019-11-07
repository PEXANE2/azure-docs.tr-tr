---
title: Data Factory kullanarak Azure Cosmos DB (SQL API) veri kopyalama
description: Data Factory kullanarak desteklenen kaynak veri depolarından veya Azure Cosmos DB (SQL API) tarafından desteklenen havuz depolarına veri kopyalamayı öğrenin.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: jingwang
ms.openlocfilehash: 74bec61cbb69b17afddeb5abc1df16d8c1140234
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73681312"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-sql-api-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Cosmos DB (SQL API) veri kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-documentdb-connector.md)
> * [Geçerli sürüm](connector-azure-cosmos-db.md)

Bu makalede, verileri (SQL API) ' den ve Azure Cosmos DB kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Makale, kopyalama etkinliğine genel bir bakış sunan [Azure Data Factory kopyalama etkinliği](copy-activity-overview.md)üzerinde oluşturulur.

>[!NOTE]
>Bu bağlayıcı yalnızca Cosmos DB SQL API 'sine/kaynağından veri kopyalamayı destekler. MongoDB API 'SI için, [MongoDB için Azure Cosmos DB API 'sine yönelik bağlayıcıya](connector-azure-cosmos-db-mongodb-api.md)başvurun. Diğer API türleri şu anda desteklenmiyor.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Cosmos DB (SQL API) Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Azure Cosmos DB (SQL API) ' den desteklenen herhangi bir havuz veri deposuna veri kopyalayabilir veya desteklenen herhangi bir kaynak veri deposundan verileri Azure Cosmos DB (SQL API) kopyalayabilirsiniz. Kopyalama etkinliğinin kaynak ve havuz olarak desteklediği veri depolarının bir listesi için bkz. [desteklenen veri depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Cosmos DB (SQL API) bağlayıcısını kullanarak şunları yapabilirsiniz:

- Ve Azure Cosmos DB [SQL API](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction)'sine verileri kopyalayın.
- **Insert** veya **upsert**olarak Azure Cosmos DB yazın.
- JSON belgelerini olduğu gibi içeri aktarın veya dışarı aktarın ya da tablo veri kümesindeki verileri veya veri kümesini kopyalayın. Örnek olarak bir SQL veritabanı ve bir CSV dosyası bulunur. Belgeleri JSON dosyalarından ya da başka bir Azure Cosmos DB koleksiyonuna veya farklı bir koleksiyon olarak kopyalamak için bkz. JSON belgelerini Içeri veya dışarı aktarma.

Data Factory, Azure Cosmos DB yazarken en iyi performansı sağlamak için [Azure Cosmos DB toplu yürütücü kitaplığıyla](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) tümleştirilir.

> [!TIP]
> [Veri geçiş videosu](https://youtu.be/5-SRNiC_qOU) , verileri Azure Blob depolamadan Azure Cosmos DB kopyalama adımlarında size yol gösterir. Video Ayrıca verileri genel olarak Azure Cosmos DB almak için performans ayarlama konularını açıklar.

## <a name="get-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Azure Cosmos DB (SQL API) özgü Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikler hakkında ayrıntılı bilgiler sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Azure Cosmos DB (SQL API) bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Type** özelliği **cosmosdb**olarak ayarlanmalıdır. | Evet |
| connectionString |Azure Cosmos DB veritabanına bağlanmak için gereken bilgileri belirtin.<br />**Note**: bağlantı dizesinde, izleyen örneklerde gösterildiği gibi veritabanı bilgilerini belirtmeniz gerekir. <br/>Bu alanı, Data Factory güvenli bir şekilde depolamak için SecureString olarak işaretleyin. Ayrıca hesap anahtarını Azure Key Vault yerleştirebilir ve `accountKey` yapılandırmasını bağlantı dizesinden dışarı çekebilirsiniz. Daha ayrıntılı bilgi için aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault makalesine depolayın](store-credentials-in-key-vault.md) . |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Integration Runtime](concepts-integration-runtime.md) . Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanını (veri depolubir özel ağda bulunuyorsa) kullanabilirsiniz. Bu özellik belirtilmezse, varsayılan Azure Integration Runtime kullanılır. |Hayır |

**Örnek**

```json
{
    "name": "CosmosDbSQLAPILinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Azure Key Vault hesap anahtarını depolayın**

```json
{
    "name": "CosmosDbSQLAPILinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;Database=<Database>"
            },
            "accountKey": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bu bölüm Azure Cosmos DB (SQL API) veri kümesinin desteklediği özelliklerin bir listesini sağlar. 

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). 

Veya Azure Cosmos DB (SQL API) verilerini kopyalamak için, veri kümesinin **Type** özelliğini **documentdbcollection**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin **Type** özelliği **documentdbcollection**olarak ayarlanmalıdır. |Evet |
| Ma |Azure Cosmos DB belge koleksiyonunun adı. |Evet |

**Örnek**

```json
{
    "name": "CosmosDbSQLAPIDataset",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Data Factory şema

Azure Cosmos DB gibi şema içermeyen veri depoları için, kopyalama etkinliği şemayı aşağıdaki listede açıklanan şekilde bir şekilde kaydeder. [JSON belgelerini olduğu gibi içeri veya dışarı aktarmak](#import-or-export-json-documents)istemediğiniz sürece, en iyi yöntem **Yapı** bölümünde verilerin yapısını belirtmektir.

* Veri kümesi tanımında **Yapı** özelliğini kullanarak verilerin yapısını belirtirseniz, Data Factory bu yapıyı şema olarak kabul eder. 

    Bir satır bir sütun için değer içermiyorsa, sütun değeri için null değer sağlanır.
* Veri kümesi tanımında **Yapı** özelliğini kullanarak verilerin yapısını belirtmezseniz, Data Factory hizmeti verilerin ilk satırını kullanarak şemayı algılar. 

    İlk satır tam şemayı içermiyorsa, kopyalama işleminin sonucunda bazı sütunlar eksik olur.

## <a name="copy-activity-properties"></a>Kopyalama etkinliği özellikleri

Bu bölüm Azure Cosmos DB (SQL API) kaynak ve havuz desteğinin özelliklerinin bir listesini sağlar.

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md).

### <a name="azure-cosmos-db-sql-api-as-source"></a>Kaynak olarak Azure Cosmos DB (SQL API)

Azure Cosmos DB (SQL API) ' den veri kopyalamak için kopyalama etkinliğindeki **kaynak** türünü **Documentdbcollectionsource**olarak ayarlayın. 

Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının **Type** özelliği **Documentdbcollectionsource**olarak ayarlanmalıdır. |Evet |
| sorgu |Verileri okumak için Azure Cosmos DB sorgusunu belirtin.<br/><br/>Örnek:<br /> `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Hayır <br/><br/>Belirtilmemişse, bu SQL deyimi yürütülür: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Belgenin iç içe olduğunu ve sonuç kümesini nasıl düzleştirdiğini gösteren özel bir karakter.<br/><br/>Örneğin, Azure Cosmos DB bir sorgu iç içe geçmiş sonuç `"Name": {"First": "John"}`döndürürse,,, **Nestedseparator** değeri olduğunda, kopyalama etkinliği sütun adını "John" değeri ile `Name.First`olarak tanımlar **.** (nokta). |Hayır<br />(varsayılan değer **.** (nokta)) |

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromCosmosDBSQLAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Cosmos DB SQL API input dataset name>",
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
                "type": "DocumentDbCollectionSource",
                "query": "SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-db-sql-api-as-sink"></a>Havuz olarak Azure Cosmos DB (SQL API)

Azure Cosmos DB (SQL API) ' e veri kopyalamak için kopyalama etkinliğindeki **Havuz** türünü **Documentdbcollectionsink**olarak ayarlayın. 

Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun **Type** özelliği **documentdbcollectionsink**olarak ayarlanmalıdır. |Evet |
| WriteBehavior |Azure Cosmos DB verilerin nasıl yazılacağını açıklar. İzin verilen değerler: **Insert** ve **upsert**.<br/><br/>Aynı KIMLIĞE sahip bir belge zaten mevcutsa, **upsert** 'un davranışı belgeyi değiştirir; Aksi takdirde, belgeyi ekleyin.<br /><br />**Note**: özgün belgede veya sütun EŞLEMESINDE bir kimlik belirtilmemişse, otomatik olarak BIR belge kimliği üretir Data Factory. Yani, **büyük** bir şekilde çalışması için BELGENIZDE bir kimliğe sahip olduğundan emin olmanız gerekir. |Hayır<br />(varsayılan değer **Insert**'dir) |
| writeBatchSize | Data Factory, Azure Cosmos DB verileri yazmak için [Azure Cosmos DB toplu yürütücü kitaplığını](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) kullanır. **Writebatchsize** ÖZELLIĞI, ADF 'nin kitaplığa sağladığı belge boyutunu denetler. Performansı artırmak ve belge boyutunuz büyük olduğunda bu değeri azaltmak için **Writebatchsize** değerini artırmayı deneyebilirsiniz. |Hayır<br />(varsayılan değer **10.000**' dir) |
| nestingSeparator |**Kaynak** sütun adında, iç içe geçmiş bir belgenin gerekli olduğunu belirten özel bir karakter. <br/><br/>Örneğin, çıkış veri kümesi yapısındaki `Name.First`, **Nestedseparator** olduğunda Azure Cosmos DB BELGESINDE aşağıdaki JSON yapısını oluşturur **.** (nokta): `"Name": {"First": "[value maps to this column from source]"}`  |Hayır<br />(varsayılan değer **.** (nokta)) |
| disableMetricsCollection | Data Factory, performansı iyileştirmek ve önerileri kopyalamak için Cosmos DB ru gibi ölçümleri toplar. Bu davranışla ilgileniyorlarsa, kapatmak için `true` belirtin. | Hayır (varsayılan `false`) |

>[!TIP]
>Cosmos DB tek isteğin boyutunu 2MB olarak sınırlandırır. Formül, Istek boyutu = tek belge boyutu * yazma toplu Iş boyutu ' dür. **"İstek boyutu çok büyük"** hatası ile karşılaşırsanız, kopyalama havuzu yapılandırmasındaki **`writeBatchSize` değerini azaltın** .

**Örnek**

```json
"activities":[
    {
        "name": "CopyToCosmosDBSQLAPI",
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
                "type": "DocumentDbCollectionSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```
## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="import-or-export-json-documents"></a>JSON belgelerini içeri veya dışarı aktarma

Bu Azure Cosmos DB (SQL API) bağlayıcısını kolayca kullanabilirsiniz:

* Azure Blob depolama, Azure Data Lake Store ve Azure Data Factory desteklediği diğer dosya tabanlı depolardan da dahil olmak üzere çeşitli kaynaklardan gelen JSON belgelerini Azure Cosmos DB içeri aktarın.
* JSON belgelerini bir Azure Cosmos DB koleksiyonundan çeşitli dosya tabanlı depolarına dışarı aktarın.
* İki Azure Cosmos DB koleksiyon arasında belge kopyalama.

Şema belirsiz kopya elde etmek için:

* Veri Kopyalama aracını kullanırken, **-as JSON dosyalarını veya Cosmos DB koleksiyon** seçeneğini belirleyin.
* Etkinlik yazma kullandığınızda Azure Cosmos DB veri kümesindeki **yapıyı** ( *şema*olarak da bilinir) belirtmeyin. Ayrıca, kopyalama etkinliğinde Azure Cosmos DB kaynak veya havuz ' da **Nestingseparator** özelliğini belirtmeyin. JSON dosyalarından içeri veya dışarı aktarma yaptığınızda, karşılık gelen dosya deposu veri kümesinde, **Biçim** türünü **jsonformat** olarak belirtin ve **filemodel** ' i [JSON biçim](supported-file-formats-and-compression-codecs.md#json-format) bölümünde açıklandığı gibi yapılandırın. Ardından, **Yapı** bölümünü belirtmeyin ve biçim ayarlarının geri kalanını atlayın.

## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliğinin Azure Data Factory kaynak ve havuz olarak desteklediği veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
