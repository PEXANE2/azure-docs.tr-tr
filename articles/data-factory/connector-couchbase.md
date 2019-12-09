---
title: Couchbase, Azure Data Factory (Önizleme) kullanarak verileri kopyalama
description: Desteklenen bir havuz veri depolarına Couchbase bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: 19496d88c1e77a6f6b18ae6f73c289f8617a3c29
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74929563"
---
# <a name="copy-data-from-couchbase-using-azure-data-factory-preview"></a>Couchbase, Azure Data Factory (Önizleme) kullanarak verileri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de Couchbase verileri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

> [!IMPORTANT]
> Bu bağlayıcı, şu anda Önizleme aşamasındadır. Deneyin ve geri bildirimde bulunun. Çözümünüzde bir önizleme bağlayıcısı bağımlılığı olmasını istiyorsanız lütfen [Azure desteğine](https://azure.microsoft.com/support/) başvurun.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Couşbase Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Couchbase tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücü el ile yüklemeniz gerekmez.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Couchbase bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Couchbase bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Couchbase** | Yes |
| connectionString | Couchbase için bağlanmak için bir ODBC bağlantı dizesi. <br/>Bu alanı, Data Factory güvenli bir şekilde depolamak için SecureString olarak işaretleyin. Ayrıca kimlik bilgisi dizesi Azure Key Vault yerleştirebilir ve `credString` yapılandırmasını bağlantı dizesinden dışarı çekebilirsiniz. Daha ayrıntılı bilgi için aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault makalesine depolayın](store-credentials-in-key-vault.md) . | Yes |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "CouchbaseLinkedService",
    "properties": {
        "type": "Couchbase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<server>; Port=<port>;AuthMech=1;CredString=[{\"user\": \"JSmith\", \"pass\":\"access123\"}, {\"user\": \"Admin\", \"pass\":\"simba123\"}];"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Azure Key Vault içinde depolama kimlik bilgisi dizesi**

```json
{
    "name": "CouchbaseLinkedService",
    "properties": {
        "type": "Couchbase",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>; Port=<port>;AuthMech=1;"
            },
            "credString": { 
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Couchbase veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Couchbase verileri kopyalamak için dataset öğesinin type özelliği ayarlamak **CouchbaseTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **CouchbaseTable** | Yes |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı "query" belirtilmişse) |


**Örnek**

```json
{
    "name": "CouchbaseDataset",
    "properties": {
        "type": "CouchbaseTable",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Couchbase linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Couchbase kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="couchbasesource-as-source"></a>Kaynak olarak CouchbaseSource

Couchbase verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **CouchbaseSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **source** bölümü:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **CouchbaseSource** | Yes |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Yok (veri kümesinde "tableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromCouchbase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Couchbase input dataset name>",
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
                "type": "CouchbaseSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
