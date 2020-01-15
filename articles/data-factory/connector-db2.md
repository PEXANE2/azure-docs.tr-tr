---
title: Azure Data Factory kullanarak DB2 'den veri kopyalama
description: Azure Data Factory işlem hattındaki kopyalama etkinliğini kullanarak DB2 'den desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: jingwang
ms.openlocfilehash: 3d3a1704b75de53bf65012329fba5f8522adff3a
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75941761"
---
# <a name="copy-data-from-db2-by-using-azure-data-factory"></a>Azure Data Factory kullanarak DB2 'den veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-db2-connector.md)
> * [Geçerli sürüm](connector-db2.md)

Bu makalede, bir DB2 veritabanından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu DB2 veritabanı Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

DB2 veritabanından desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu DB2 Bağlayıcısı, dağıtılmış Ilişkisel veritabanı mimarisi (DRDA) SQL Access Manager (, 10 ve 11) ile birlikte aşağıdaki IBM DB2 platformunu ve sürümlerini destekler:

* Z/OS 12,1 için IBM DB2
* Z/OS 11,1 için IBM DB2
* Z/OS 10,1 için IBM DB2
* I 7,3 için IBM DB2
* I 7,2 için IBM DB2
* I 7,1 için IBM DB2
* LUW 11 için IBM DB2
* LUW 10,5 için IBM DB2
* LUW 10,1 için IBM DB2

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

Integration Runtime yerleşik bir DB2 sürücüsü sağlar, bu nedenle DB2 'den veri kopyalarken herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler DB2 bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler DB2 bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği: **DB2** olarak ayarlanmalıdır | Evet |
| sunucu |DB2 sunucusunun adı. Sunucu adının sonuna iki nokta (`server:port`) ile ayrılmış bağlantı noktası numarasını belirtebilirsiniz. |Evet |
| veritabanı |DB2 veritabanının adı. |Evet |
| authenticationType |DB2 veritabanına bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değer: **temel**. |Evet |
| kullanıcı adı |DB2 veritabanına bağlanmak için Kullanıcı adını belirtin. |Evet |
| parola |Kullanıcı adı için belirttiğiniz kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| packageCollection | Veritabanı sorgulanırken, gerekli paketlerin ADF tarafından otomatik olarak oluşturulduğu yeri belirtin. | Hayır |
| Certificatecommonadı | Güvenli Yuva Katmanı (SSL) veya Aktarım Katmanı Güvenliği (TLS) şifrelemesini kullandığınızda, sertifika ortak adı için bir değer girmeniz gerekir. | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

> [!TIP]
> `The package corresponding to an SQL statement execution request was not found. SQLSTATE=51002 SQLCODE=-805`bildiren bir hata iletisi alırsanız, bu nedenle Kullanıcı için gerekli bir paket oluşturulmaz. Varsayılan olarak, ADF, DB2 'ye bağlanmak için kullandığınız kullanıcı olarak adlı koleksiyonda bir paket oluşturmaya çalışır. ADF 'nin veritabanını sorgularken gereken paketleri oluşturmasını istediğiniz yeri belirtmek için paket koleksiyonu özelliğini belirtin.

**Örnek:**

```json
{
    "name": "Db2LinkedService",
    "properties": {
        "type": "Db2",
        "typeProperties": {
            "server": "<servername:port>",
            "database": "<dbname>",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, DB2 veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

DB2 'den veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | DataSet 'in Type özelliği: **Db2Table** olarak ayarlanmalıdır | Evet |
| schema | Şemanın adı. |Hayır (etkinlik kaynağı "query" belirtilmişse)  |
| table | Tablonun adı. |Hayır (etkinlik kaynağı "query" belirtilmişse)  |
| tableName | Şemanın bulunduğu tablonun adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü için `schema` ve `table` kullanın. | Hayır (etkinlik kaynağı "query" belirtilmişse) |

**Örnek**

```json
{
    "name": "DB2Dataset",
    "properties":
    {
        "type": "Db2Table",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<DB2 linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Türü belirlenmiş `RelationalTable` veri kümesini kullanıyorsanız, bunun olduğu gibi hala desteklenmektedir, ileri ' yi kullanmaya devam etmeniz önerilir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, DB2 kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="db2-as-source"></a>Kaynak olarak DB2

DB2 'den veri kopyalamak için aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği: **Db2Source** olarak ayarlanmalıdır | Evet |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""`. | Yok (veri kümesinde "tableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromDB2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<DB2 input dataset name>",
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
                "type": "Db2Source",
                "query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Türü belirlenmiş `RelationalSource` kaynak kullanıyorsanız, hala olduğu gibi desteklenirken, ileri ' yi kullanmanız önerilir.

## <a name="data-type-mapping-for-db2"></a>DB2 için veri türü eşlemesi

DB2 'den veri kopyalarken aşağıdaki eşlemeler DB2 veri türlerinden, geçici veri türlerini Azure Data Factory için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| DB2 veritabanı türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BigInt |Int64 |
| Binary |Byte[] |
| Blob |Byte[] |
| char |Dize |
| CLOB |Dize |
| Tarih |Datetime |
| DB2DynArray |Dize |
| DbClob |Dize |
| Decimal |Decimal |
| DecimalFloat |Decimal |
| Double |Double |
| Kayan |Double |
| Graphic |Dize |
| Tamsayı |Int32 |
| LongVarBinary |Byte[] |
| LongVarChar |Dize |
| LongVarGraphic |Dize |
| Sayısal |Decimal |
| Real |Tek |
| Smallint |Int16 |
| Zaman |TimeSpan |
| Zaman damgası |Tarih Saat |
| Ikili |Byte[] |
| VarChar |Dize |
| VarGraphic |Dize |
| Xml |Byte[] |

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
