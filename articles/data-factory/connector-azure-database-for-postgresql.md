---
title: Azure Data Factory kullanarak PostgreSQL için Azure veritabanı 'na veri kopyalama Microsoft Docs
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak PostgreSQL için Azure veritabanı 'na ve veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: jingwang
ms.openlocfilehash: d6b860f43abae2283b4889bff0913bac25c821f5
ms.sourcegitcommit: 71db032bd5680c9287a7867b923bf6471ba8f6be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71017774"
---
# <a name="copy-data-to-and-from-azure-database-for-postgresql-using-azure-data-factory"></a>Azure Data Factory kullanarak PostgreSQL için Azure veritabanı 'na veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de PostgreSQL için Azure veritabanı'ndan veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

Bu bağlayıcı [PostgreSQL Için Azure veritabanı hizmetine](../postgresql/overview.md)yöneliktir. Şirket içinde veya bulutta bulunan genel PostgreSQL veritabanından veri kopyalamak için [PostgreSQL bağlayıcısını](connector-postgresql.md)kullanın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

PostgreSQL için Azure Veritabanı Bağlayıcısı, aşağıdaki etkinlikler için desteklenir:

- [Etkinliği](copy-activity-overview.md) [Desteklenen kaynak matrisi](copy-activity-overview.md) ile Kopyala
- [Arama etkinliği](control-flow-lookup-activity.md)

Tüm desteklenen havuz veri deposuna PostgreSQL için Azure veritabanı'ndan veri kopyalayabilirsiniz. Ya da desteklenen herhangi bir kaynak veri deposundan verileri PostgreSQL için Azure veritabanı 'na kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli PostgreSQL bağlayıcı için Azure veritabanı'na tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler için Azure veritabanı, PostgreSQL bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği şu şekilde ayarlanmalıdır: **AzurePostgreSql** | Evet |
| connectionString | PostgreSQL için Azure veritabanı'na bağlanmak için bir ODBC bağlantı dizesi.<br/>Bu alanı, Data Factory güvenli bir şekilde depolamak için SecureString olarak işaretleyin. Ayrıca, Azure Key Vault parolayı yerleştirebilir ve `password` yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Daha ayrıntılı bilgi için aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault makalesine depolayın](store-credentials-in-key-vault.md) . | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel ağında bulunuyorsa), Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

Bir bağlantı dizesi olan `Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`. Daha fazla özellik durumunuz ayarlayabilirsiniz:

| Özellik | Açıklama | Seçenekler | Gerekli |
|:--- |:--- |:--- |:--- |
| EncryptionMethod (EM)| Sürücü yöntemi, sürücü ve veritabanı sunucusu arasında gönderilen verileri şifrelemek için kullanır. Ör.,`EncryptionMethod=<0/1/6>;`| 0 (şifreleme) **(varsayılan)** / 1 (SSL) / 6 (RequestSSL) | Hayır |
| ValidateServerCertificate (VSC) | Sürücü SSL şifrelemesi etkin olduğunda veritabanı sunucusu tarafından gönderilen sertifikayı doğrulayıp doğrulamadığını belirler (şifreleme yöntemini = 1). Ör.,`ValidateServerCertificate=<0/1>;`| 0 (devre dışı) **(varsayılan)** / 1 (etkin) | Hayır |

**Örnek:**

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
            }
        }
    }
}
```

**Örnek: Azure Key Vault parola depola**

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, PostgreSQL veri kümesi için Azure veritabanı tarafından desteklenen özelliklerin bir listesini sağlar.

PostgreSQL için Azure veritabanı'ndan veri kopyalamak için dataset öğesinin type özelliği ayarlamak **AzurePostgreSqlTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği şu şekilde ayarlanmalıdır: **AzurePostgreSqlTable** | Evet |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı "query" belirtilmişse) |

**Örnek**

```json
{
    "name": "AzurePostgreSqlDataset",
    "properties": {
        "type": "AzurePostgreSqlTable",
        "linkedServiceName": {
            "referenceName": "<AzurePostgreSql linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, PostgreSQL kaynağı için Azure veritabanı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-database-for-postgresql-as-source"></a>Kaynak olarak PostgreSql için Azure veritabanı

PostgreSQL için Azure veritabanı'ndan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **AzurePostgreSqlSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği şu şekilde ayarlanmalıdır: **AzurePostgreSqlSource** | Evet |
| query | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Yok (veri kümesinde "tableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromAzurePostgreSql",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<AzurePostgreSql input dataset name>",
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
                "type": "AzurePostgreSqlSource",
                "query": "<custom query e.g. SELECT * FROM MyTable>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-database-for-postgresql-as-sink"></a>Havuz olarak PostgreSQL için Azure veritabanı

PostgreSQL için Azure veritabanı 'na veri kopyalamak için, etkinlik **havuzunu** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun Type özelliği şu şekilde ayarlanmalıdır: **AzurePostgreSQLSink** | Evet |
| preCopyScript | Her çalıştırmada PostgreSQL için Azure veritabanı 'na veri yazmadan önce yürütülecek kopyalama etkinliği için bir SQL sorgusu belirtin. Bu özelliği, önceden yüklenmiş verileri temizlemek için kullanabilirsiniz. | Hayır |
| writeBatchSize | Arabellek boyutu writeBatchSize ulaştığında, PostgreSQL için Azure veritabanı tablosuna veri ekler.<br>İzin verilen değer, satır sayısını temsil eden bir tamsayıdır. | Hayır (varsayılan değer 10.000) |
| writeBatchTimeout | Toplu ekleme işleminin, zaman aşımına uğramadan önce tamamlaması için bekleme süresi.<br>İzin verilen değerler TimeSpan. Örnek olarak 00:30:00 (30 dakika). | Hayır (varsayılan değer 00:00:30) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToAzureDatabaseForPostgreSQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure PostgreSQL output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzurePostgreSQLSink",
                "preCopyScript": "<custom SQL script>",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
