---
title: Azure Veri Fabrikasını kullanarak PostgreSQL'den veri kopyalama
description: Bir Azure Veri Fabrikası ardışık alanında kopyalama etkinliği kullanarak PostgreSQL'den desteklenen lavabo veri depolarına verileri nasıl kopyalayatıyorum öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: jingwang
ms.openlocfilehash: 6d10e7b9b24817eb738172bd0f2d2c3e7f8f2cbf
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416759"
---
# <a name="copy-data-from-postgresql-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak PostgreSQL'deki verileri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-postgresql-connector.md)
> * [Geçerli sürüm](connector-postgresql.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]


Bu makalede, Bir PostgreSQL veritabanından veri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu PostgreSQL konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

PostgreSQL veritabanından desteklenen herhangi bir lavabo veri deposuna verileri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu PostgreSQL bağlayıcı PostgreSQL **sürüm 7.4 ve üzeri**destekler.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

Tümleştirme Runtime sürüm 3.7'den başlayarak yerleşik bir PostgreSQL sürücüsü sağlar, bu nedenle herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, PostgreSQL bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi verilmiştir.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

PostgreSQL bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **PostgreSql** | Evet |
| Connectionstring | PostgreSQL için Azure Veritabanı'na bağlanmak için bir ODBC bağlantı dizesi. <br/>Parolayı Azure Key Vault'a koyabilir `password` ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Azure Key Vault [makalesinde](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren aşağıdaki örneklere ve Mağaza kimlik bilgilerine bakın. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

Tipik bir bağlantı `Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`dizesi. Durumunuza göre ayarlayabildiğiniz daha fazla özellik:

| Özellik | Açıklama | Seçenekler | Gerekli |
|:--- |:--- |:--- |:--- |
| Şifreleme Yöntemi (EM)| Sürücünün sürücü ve veritabanı sunucusu arasında gönderilen verileri şifrelemek için kullandığı yöntem. Örneğin.`EncryptionMethod=<0/1/6>;`| 0 (Şifreleme Yok) **(Varsayılan)** / 1 (SSL) / 6 (RequestssL) | Hayır |
| Sunucu Sertifikasını Doğrulama (VSC) | SSL şifrelemesi etkinleştirildiğinde sürücünün veritabanı sunucusu tarafından gönderilen sertifikayı doğrulayıp doğrulamadığını belirler (Şifreleme Yöntemi=1). Örneğin.`ValidateServerCertificate=<0/1>;`| 0 (Devre Dışı) **(Varsayılan)** / 1 (Etkin) | Hayır |

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

**Örnek: parolayı Azure Key Vault'ta depolama**

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

PostgreSQL bağlantılı hizmeti aşağıdaki yükle kullanıyorsanız, ileriye dönük yeni hizmetini kullanmanız önerilirken, yine de olduğu gibi desteklenir.

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde PostgreSQL dataset tarafından desteklenen özelliklerin bir listesini sağlar.

PostgreSQL'deki verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği ayarlanmalıdır: **PostgreSqlTable** | Evet |
| Şema | Şema adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tablo | Masanın adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tableName | Şema ile tablonun adı. Bu özellik geriye dönük uyumluluk için desteklenir. Kullanın `schema` `table` ve yeni iş yükü için. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

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

Dakti-zimd veri kümesi kullanıyorsanız, `RelationalTable` ileriye dönük yenisini kullanmanız önerilirken, bu hala olduğu gibi desteklenir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde PostgreSQL kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="postgresql-as-source"></a>Kaynak olarak PostgreSQL

PostgreSQL'deki verileri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **PostgreSqlSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"query": "SELECT * FROM \"MySchema\".\"MyTable\""`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

> [!NOTE]
> Şema ve masa adları büyük/küçük harf duyarlıdır. Sorguda `""` (çift tırnak) bunları içine alayın.

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

Dakti-yazılı `RelationalSource` kaynak kullanıyorsanız, ileriye dönük yeni bir kaynak kullanmanız önerilirken, yine de olduğu gibi desteklenir.

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
