---
title: Azure Data Factory kullanarak PostgreSQL 'Ten veri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak PostgreSQL 'ten desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
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
ms.openlocfilehash: fa5835b287a2fd39671ea5ac0c3bc849378645c5
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75891927"
---
# <a name="copy-data-from-postgresql-by-using-azure-data-factory"></a>Azure Data Factory kullanarak PostgreSQL 'ten veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-postgresql-connector.md)
> * [Geçerli sürüm](connector-postgresql.md)

Bu makalede, bir PostgreSQL veritabanından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu PostgreSQL Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

PostgreSQL veritabanından desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu PostgreSQL Bağlayıcısı PostgreSQL **sürüm 7,4 ve üstünü**destekler.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

3,7 'den daha düşük olan şirket içinde barındırılan IR sürümü için Integration Runtime makinesindeki 2.0.12 ve 3.1.9 arasında sürümü olan [PostgreSQL Için Ngpsql veri sağlayıcısını](https://go.microsoft.com/fwlink/?linkid=282716) yüklemeniz gerekir.

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, PostgreSQL bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

PostgreSQL bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği: **PostgreSQL** olarak ayarlanmalıdır | Evet |
| connectionString | PostgreSQL için Azure veritabanı'na bağlanmak için bir ODBC bağlantı dizesi. <br/>Ayrıca, Azure Key Vault parola yerleştirebilir ve `password` yapılandırmasını bağlantı dizesinden dışarı çekebilirsiniz. Daha ayrıntılı bilgi için aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault makalesine depolayın](store-credentials-in-key-vault.md) . | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

Bir bağlantı dizesi olan `Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`. Daha fazla özellik durumunuz ayarlayabilirsiniz:

| Özellik | Açıklama | Seçenekler | Gereklidir |
|:--- |:--- |:--- |:--- |
| EncryptionMethod (EM)| Sürücü yöntemi, sürücü ve veritabanı sunucusu arasında gönderilen verileri şifrelemek için kullanır. Örn. `EncryptionMethod=<0/1/6>;`| 0 (şifreleme) **(varsayılan)** / 1 (SSL) / 6 (RequestSSL) | Hayır |
| ValidateServerCertificate (VSC) | Sürücü SSL şifrelemesi etkin olduğunda veritabanı sunucusu tarafından gönderilen sertifikayı doğrulayıp doğrulamadığını belirler (şifreleme yöntemini = 1). Örn. `ValidateServerCertificate=<0/1>;`| 0 (devre dışı) **(varsayılan)** / 1 (etkin) | Hayır |

**Örnek:**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Azure Key Vault parola depola**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>;Database=<database>;Port=<port>;UID=<username>;",
            "password": { 
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

Aşağıdaki yük ile PostgreSQL bağlı hizmetini kullanıyorsanız, yeni bir tane olduğu gibi, ileri ' yi kullanmanız önerilir.

**Önceki yük:**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, PostgreSQL veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

PostgreSQL 'ten veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | DataSet 'in Type özelliği: **Postgrescollection tablosuna** ayarlanmalıdır | Evet |
| schema | Şemanın adı. |Hayır (etkinlik kaynağı "query" belirtilmişse)  |
| table | Tablonun adı. |Hayır (etkinlik kaynağı "query" belirtilmişse)  |
| tableName | Şemanın bulunduğu tablonun adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü için `schema` ve `table` kullanın. | Hayır (etkinlik kaynağı "query" belirtilmişse) |

**Örnek**

```json
{
    "name": "PostgreSQLDataset",
    "properties":
    {
        "type": "PostgreSqlTable",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<PostgreSQL linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Türü belirlenmiş `RelationalTable` veri kümesini kullanıyorsanız, bunun olduğu gibi hala desteklenmektedir, ileri ' yi kullanmaya devam etmeniz önerilir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, PostgreSQL kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="postgresql-as-source"></a>Kaynak olarak PostgreSQL

PostgreSQL 'ten veri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği: **Postgrescollection kaynağına** ayarlanmalıdır | Evet |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"query": "SELECT * FROM \"MySchema\".\"MyTable\""`. | Yok (veri kümesinde "tableName" değeri belirtilmişse) |

> [!NOTE]
> Şema ve tablo adları büyük/küçük harfe duyarlıdır. Onları sorguya `""` (çift tırnak) içine alın.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromPostgreSQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<PostgreSQL input dataset name>",
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
                "type": "PostgreSqlSource",
                "query": "SELECT * FROM \"MySchema\".\"MyTable\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Türü belirlenmiş `RelationalSource` kaynak kullanıyorsanız, hala olduğu gibi desteklenirken, ileri ' yi kullanmanız önerilir.

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
