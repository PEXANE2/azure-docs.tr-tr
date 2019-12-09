---
title: Azure Data Factory kullanarak Sybase 'ten veri kopyalama
description: Azure Data Factory işlem hattındaki kopyalama etkinliğini kullanarak Sybase 'ten desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: jingwang
ms.openlocfilehash: 0552cdc50e2b760600ad8c58bd3d1cd4d2dc50a2
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930989"
---
# <a name="copy-data-from-sybase-using-azure-data-factory"></a>Azure Data Factory kullanarak Sybase 'ten veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-sybase-connector.md)
> * [Geçerli sürüm](connector-sybase.md)

Bu makalede, bir Sybase veritabanından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Sybase Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Verileri Sybase veritabanından desteklenen herhangi bir havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Sybase Bağlayıcısı şunları destekler:

- SAP Sybase SQL her yerde (ASA) **sürüm 16 ve üzeri**; IQ ve ATıCı desteklenmez.
- **Temel** veya **Windows** kimlik doğrulaması kullanarak verileri kopyalama.

## <a name="prerequisites"></a>Önkoşullar

Bu Sybase bağlayıcısını kullanmak için şunları yapmanız gerekir:

- Şirket içinde barındırılan bir Integration Runtime ayarlayın. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.
- Integration Runtime makinesinde [Sybase ıheryer. Data. SQLAnywhere 16 veya üzeri için veri sağlayıcısını](https://go.microsoft.com/fwlink/?linkid=324846) yükler.

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Sybase bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler, Sybase bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği: **Sybase** olarak ayarlanmalıdır | Yes |
| sunucu | Sybase sunucusunun adı. |Yes |
| veritabanı | Sybase veritabanının adı. |Yes |
| authenticationType | Sybase veritabanına bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değerler şunlardır: **temel**ve **Windows**. |Yes |
| kullanıcı adı | Sybase veritabanına bağlanmak için Kullanıcı adını belirtin. |Yes |
| password | Kullanıcı adı için belirttiğiniz kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Yes |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşul](#prerequisites)bölümünde belirtildiği gibi, kendinden konak Integration Runtime gereklidir. |Yes |

**Örnek:**

```json
{
    "name": "SybaseLinkedService",
    "properties": {
        "type": "Sybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "authenticationType": "Basic",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Sybase veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Sybase 'ten veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | DataSet 'in Type özelliği: **Sybasetable** olarak ayarlanmalıdır | Yes |
| tableName | Sybase veritabanındaki tablonun adı. | Hayır (etkinlik kaynağı "query" belirtilmişse) |

**Örnek**

```json
{
    "name": "SybaseDataset",
    "properties": {
        "type": "SybaseTable",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Sybase linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Türü belirlenmiş `RelationalTable` veri kümesini kullanıyorsanız, bunun olduğu gibi hala desteklenmektedir, ileri ' yi kullanmaya devam etmeniz önerilir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Sybase kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sybase-as-source"></a>Kaynak olarak Sybase

Verileri Sybase 'ten kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği: **Sybasesource** olarak ayarlanmalıdır | Yes |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Yok (veri kümesinde "tableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSybase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Sybase input dataset name>",
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
                "type": "SybaseSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Türü belirlenmiş `RelationalSource` kaynak kullanıyorsanız, hala olduğu gibi desteklenirken, ileri ' yi kullanmanız önerilir.

## <a name="data-type-mapping-for-sybase"></a>Sybase için veri türü eşlemesi

Verileri Sybase 'ten kopyalarken, veri türleri Azure Data Factory için Sybase veri türlerinden aşağıdaki eşlemeler kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

Sybase T-SQL türlerini destekler. SQL türlerindeki bir eşleme tablosu, ara veri türlerini Azure Data Factory için, bkz. [Azure SQL Veritabanı Bağlayıcısı-veri türü eşleme](connector-azure-sql-database.md#data-type-mapping-for-azure-sql-database) bölümü.

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.



## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
