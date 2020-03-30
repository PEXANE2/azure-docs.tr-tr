---
title: MongoDB'den veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına Mongo DB'deki verileri nasıl kopyalaylayamamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
ms.author: jingwang
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 08/12/2019
ms.openlocfilehash: a7bb74c09b45429a160a3ec481c23073575cfe3c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251719"
---
# <a name="copy-data-from-mongodb-using-azure-data-factory"></a>Azure Veri Fabrikasını kullanarak MongoDB'deki verileri kopyalama

Bu makalede, MongoDB veritabanındaki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

>[!IMPORTANT]
>ADF daha iyi yerli MongoDB desteği sağlayan MongoDB konektörü bu yeni sürümünü bırakın. Geriye dönük uyumluluk için olduğu gibi desteklenen çözümünüzde önceki MongoDB konektörünü kullanıyorsanız, [MongoDB bağlayıcısı (eski)](connector-mongodb-legacy.md) makalesine bakın.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

MongoDB veritabanındaki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu MongoDB konektörü **3.4'e kadar olan sürümleri**destekler.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, MongoDB bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

MongoDB bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Tür özelliği ayarlanmalıdır: **MongoDbV2** |Evet |
| Connectionstring |MongoDB bağlantı dizesini belirtin, `mongodb://[username:password@]host[:port][/[database][?options]]`örneğin. Daha fazla bilgi için [bağlantı dizesi mongoDB kılavuzuna](https://docs.mongodb.com/manual/reference/connection-string/) bakın. <br/><br /> Ayrıca Azure Key Vault'a bir parola `password` koyabilir ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Azure [Anahtar Kasası'ndaki Mağaza kimlik bilgilerine](store-credentials-in-key-vault.md) daha fazla ayrıntı yla bakın. |Evet |
| database | Erişmek istediğiniz veritabanının adı. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "MongoDBLinkedService",
    "properties": {
        "type": "MongoDbV2",
        "typeProperties": {
            "connectionString": "mongodb://[username:password@]host[:port][/[database][?options]]",
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Bkz. Veri Kümeleri ve bağlantılı hizmetler.](concepts-datasets-linked-services.md) Aşağıdaki özellikler MongoDB veri kümesi için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği ayarlanmalıdır: **MongoDbV2Collection** | Evet |
| Toplamaadı |MongoDB veritabanındaki koleksiyonun adı. |Evet |

**Örnek:**

```json
{
    "name": "MongoDbDataset",
    "properties": {
        "type": "MongoDbV2Collection",
        "typeProperties": {
            "collectionName": "<Collection name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<MongoDB linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde MongoDB kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="mongodb-as-source"></a>Kaynak olarak MongoDB

Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **MongoDbV2Source** | Evet |
| filtre | Sorgu işleçlerini kullanarak seçim filtresini belirtir. Koleksiyondaki tüm belgeleri döndürmek için, bu parametreyi atla{}veya boş bir belgeyi geçirin ( ). | Hayır |
| imleçMethods.project | Projeksiyon için belgelerde döndürülecek alanları belirtir. Eşleşen belgelerdeki tüm alanları döndürmek için bu parametreyi atla. | Hayır |
| imleçMethods.sort | Sorgunun eşleşen belgeleri döndürme sırasını belirtir. [Cursor.sort()](https://docs.mongodb.com/manual/reference/method/cursor.sort/#cursor.sort)bakın. | Hayır |
| imleçMethods.limit | Sunucunun döndürdğü en fazla belge sayısını belirtir. [Cursor.limit()](https://docs.mongodb.com/manual/reference/method/cursor.limit/#cursor.limit)bakın.  | Hayır |
| imleçMethods.skip | Atlasılaş acak belge sayısını ve MongoDB'nin sonuçları döndürmeye başladığı yeri belirtir. [Cursor.skip()](https://docs.mongodb.com/manual/reference/method/cursor.skip/#cursor.skip)bakın. | Hayır |
| batchSize | MongoDB örneğinden yanıtın her bir toplu halinde döndürülecek belge sayısını belirtir. Çoğu durumda, toplu iş boyutunu değiştirmek kullanıcıyı veya uygulamayı etkilemez. Cosmos DB sınırları her toplu iş boyutu 40MB geçemez, hangi belgelerin toplu Boyut sayısının toplamıdır, bu nedenle belge boyutu büyük ise bu değeri azaltmak. | Hayır<br/>(varsayılan **değer 100'dür)** |

>[!TIP]
>ADF, BSON belgesini **Sıkı modda**tüketen desteği destekler. Filtre sorgunuzun Shell modu yerine Katı modda olduğundan emin olun. Daha fazla açıklama [MongoDB kılavuzunda](https://docs.mongodb.com/manual/reference/mongodb-extended-json/index.html)bulunabilir.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromMongoDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<MongoDB input dataset name>",
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
                "type": "MongoDbV2Source",
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

## <a name="export-json-documents-as-is"></a>JSON belgelerini olduğu gibi dışa aktarma

Bu MongoDB bağlayıcısını, MongoDB koleksiyonundan çeşitli dosya tabanlı mağazalara veya Azure Cosmos DB'ye json belgelerini olduğu gibi dışa aktarmak için kullanabilirsiniz. Bu tür şema-agnostik kopya elde etmek için, veri kümesindeki "yapı" *(şema*olarak da adlandırılır) bölümünü ve kopyalama etkinliğinde şema eşleciliğini atlayın.

## <a name="schema-mapping"></a>Şema haritalama

MongoDB'deki verileri tabular lavaboya kopyalamak için [şema eşleciliği'ne](copy-activity-schema-and-type-mapping.md#schema-mapping)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
