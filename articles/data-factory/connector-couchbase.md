---
title: Azure Veri Fabrikasını kullanarak Couchbase'den veri kopyalama (Önizleme)
description: Bir Azure Veri Fabrikası ardışık hattında bir kopyalama etkinliği kullanarak Couchbase'den desteklenen lavabo veri depolarına verileri nasıl kopyalayatılayıdersiniz öğrenin.
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
ms.openlocfilehash: d78d533bc4a863a0a70b1dbb47bdfa85d539884f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417434"
---
# <a name="copy-data-from-couchbase-using-azure-data-factory-preview"></a>Azure Veri Fabrikasını kullanarak Couchbase'den veri kopyalama (Önizleme)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]
Bu makalede, Couchbase'deki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

> [!IMPORTANT]
> Bu bağlayıcı şu anda önizlemede. Bunu deneyebilir ve bize geribildirim verebilirsiniz. Çözümünüzde bir önizleme bağlayıcısı bağımlılığı olmasını istiyorsanız lütfen [Azure desteğine](https://azure.microsoft.com/support/) başvurun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Couchbase konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Couchbase'deki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Azure Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Couchbase konektörüne özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Couchbase bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **Couchbase** | Evet |
| Connectionstring | Couchbase'e bağlanmak için bir ODBC bağlantı dizesi. <br/>Ayrıca Azure Key Vault'a kimlik bilgisi dizesini koyabilir ve yapılandırmayı `credString` bağlantı dizesinin dışına çekebilirsiniz. Azure Key Vault [makalesinde](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren aşağıdaki örneklere ve Mağaza kimlik bilgilerine bakın. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "CouchbaseLinkedService",
    "properties": {
        "type": "Couchbase",
        "typeProperties": {
            "connectionString": "Server=<server>; Port=<port>;AuthMech=1;CredString=[{\"user\": \"JSmith\", \"pass\":\"access123\"}, {\"user\": \"Admin\", \"pass\":\"simba123\"}];"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: kimlik belgesi dizelerini Azure Key Vault'ta depolama**

```json
{
    "name": "CouchbaseLinkedService",
    "properties": {
        "type": "Couchbase",
        "typeProperties": {
            "connectionString": "Server=<server>; Port=<port>;AuthMech=1;",
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde Couchbase dataset tarafından desteklenen özelliklerin bir listesini sağlar.

Couchbase'deki verileri kopyalamak için, veri kümesinin tür özelliğini **CouchbaseTable**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **CouchbaseTable** | Evet |
| tableName | Masanın adı. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |


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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Couchbase kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="couchbasesource-as-source"></a>Kaynak olarak CouchbaseSource

Couchbase'deki verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **CouchbaseSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **CouchbaseSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

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

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
