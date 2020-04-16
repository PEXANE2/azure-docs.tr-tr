---
title: PostgreSQL için Azure Veritabanı'na veri kopyalama
description: Bir Azure Veri Fabrikası ardışık alanında kopyalama etkinliği kullanarak PostgreSQL için Azure Veritabanı'na veri kopyalamayı öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/16/2019
ms.openlocfilehash: b85e72ae6698cd9fa018c940e158bfcf25279ed5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81410473"
---
# <a name="copy-data-to-and-from-azure-database-for-postgresql-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak PostgreSQL için Azure Veritabanı'na veri kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, PostgreSQL için Azure Veritabanı'ndaki verileri kopyalamak için Azure Veri Fabrikası'ndaki kopyalama etkinliği özelliğinin nasıl kullanılacağı açıklanmaktadır. [Azure Veri Fabrikası makalesinde](copy-activity-overview.md) kopyalama etkinliğine genel bir bakış sunan Kopyalama etkinliğini temel alar.

Bu bağlayıcı, [PostgreSQL hizmeti için Azure Veritabanı için](../postgresql/overview.md)özelleştirilmiştir. Şirket içinde veya bulutta bulunan genel bir PostgreSQL veritabanından verileri kopyalamak için [PostgreSQL bağlayıcısını](connector-postgresql.md)kullanın.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

PostgreSQL bağlayıcısı için bu Azure Veritabanı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen](copy-activity-overview.md) bir [kaynak/lavabo matrisi](copy-activity-overview.md) ile etkinliği kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

PostgreSQL için Azure Veritabanı'ndan desteklenen herhangi bir lavabo veri deposuna veri kopyalayabilirsiniz. Veya, desteklenen herhangi bir kaynak veri deposundan verileri PostgreSQL için Azure Veritabanı'na kopyalayabilirsiniz. Kopyalama etkinliğinin kaynak ve lavabo olarak desteklediği veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Azure Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar. Bu nedenle, bu bağlayıcıyı kullanmak için herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, PostgreSQL bağlayıcısı için Azure Veritabanına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sunulmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler PostgreSQL bağlantılı hizmet için Azure Veritabanı için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **AzurePostgreSql**. | Evet |
| Connectionstring | PostgreSQL için Azure Veritabanı'na bağlanmak için bir ODBC bağlantı dizesi.<br/>Ayrıca Azure Key Vault'a bir parola `password` koyabilir ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Daha fazla ayrıntı için Aşağıdaki örnekleri ve [Mağaza kimlik bilgilerini Azure Anahtar Kasası'nda](store-credentials-in-key-vault.md) görün. | Evet |
| connectVia | Bu özellik, veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanını](concepts-integration-runtime.md) temsil eder. Azure Tümleştirme Çalışma Süresi'ni veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

Tipik bir bağlantı `Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`dizesi. Servis talebinize göre ayarlayabileceğiniz daha fazla özellik şunlardır:

| Özellik | Açıklama | Seçenekler | Gerekli |
|:--- |:--- |:--- |:--- |
| Şifreleme Yöntemi (EM)| Sürücünün sürücü ve veritabanı sunucusu arasında gönderilen verileri şifrelemek için kullandığı yöntem. Örneğin,`EncryptionMethod=<0/1/6>;`| 0 (Şifreleme Yok) **(Varsayılan)** / 1 (SSL) / 6 (RequestssL) | Hayır |
| Sunucu Sertifikasını Doğrulama (VSC) | SSL şifrelemesi etkinleştirildiğinde sürücünün veritabanı sunucusu tarafından gönderilen sertifikayı doğrulayıp doğrulamadığını belirler (Şifreleme Yöntemi=1). Örneğin,`ValidateServerCertificate=<0/1>;`| 0 (Devre Dışı) **(Varsayılan)** / 1 (Etkin) | Hayır |

**Örnek**:

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
        }
    }
}
```

**Örnek**:

***Parolayı Azure Anahtar Kasası'nda saklayın***

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;",
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Azure Veri Fabrikası'ndaki Veri Kümeleri'ne](concepts-datasets-linked-services.md)bakın. Bu bölümde, PostgreSQL için Azure Veritabanı'nın veri kümelerinde desteklediği özelliklerin bir listesi sağlanmaktadır.

PostgreSQL için Azure Veritabanı'ndaki verileri kopyalamak için, veri kümesinin tür özelliğini **AzurePostgreSqlTable**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği **AzurePostgreSqlTable** olarak ayarlanmalıdır | Evet |
| tableName | Tablonun adı | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**:

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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için Azure [Veri Fabrikası'ndaki Boru Hatları ve etkinliklerine](concepts-pipelines-activities.md)bakın. Bu bölümde, PostgreSQL kaynağı için bir Azure Veritabanı tarafından desteklenen özelliklerin bir listesi sağlanmaktadır.

### <a name="azure-database-for-postgresql-as-source"></a>Kaynak olarak PostgreSql için Azure Veritabanı

PostgreSQL için Azure Veritabanı'ndaki verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **AzurePostgreSqlSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının tür özelliği **AzurePostgreSqlSource** olarak ayarlanmalıdır | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin, `"SELECT * FROM MyTable"` | Hayır (veri kümesindeki tabloAdı özelliği belirtilmişse) |

**Örnek**:

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

### <a name="azure-database-for-postgresql-as-sink"></a>PostgreSQL için Azure Veritabanı

PostgreSQL için Verileri Azure Veritabanı'na kopyalamak için, kopyalama etkinliği **lavabo** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabonun türü özelliği **AzurePostgreSQLSink**olarak ayarlanmalıdır. | Evet |
| preCopyScript | Her çalıştırmada PostgreSQL için Azure Veritabanı'na veri yazmadan önce yürütülecek kopyalama etkinliği için bir SQL sorgusu belirtin. Önceden yüklenmiş verileri temizlemek için bu özelliği kullanabilirsiniz. | Hayır |
| yazmaBatchSize | Arabellek boyutu writeBatchSize'a ulaştığında PostgreSQL için Azure Veritabanı'na veri ekler.<br>İzin verilen değer, satır sayısını temsil eden bir karşıcıdır. | Hayır (varsayılan değer 10.000'dir) |
| yazmaBatchTimeout | Toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresi.<br>İzin verilen değerler Timespan dizeleridir. Bir örnek 00:30:00 (30 dakika). | Hayır (varsayılan değer 00:00:30'dur) |

**Örnek**:

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

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi için [Azure Veri Fabrikası'ndaki Arama etkinliği'ne](control-flow-lookup-activity.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için desteklenen [veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
