---
title: Azure Data Factory kullanarak DB2 'den veri kopyalama | Microsoft Docs
description: Azure Data Factory işlem hattındaki kopyalama etkinliğini kullanarak DB2 'den desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: jingwang
ms.openlocfilehash: 2bdec0c70e9f11ca40e0ff9e1aa87898c94e119c
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232995"
---
# <a name="copy-data-from-db2-by-using-azure-data-factory"></a>Azure Data Factory kullanarak DB2 'den veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-db2-connector.md)
> * [Geçerli sürüm](connector-db2.md)

Bu makalede, bir DB2 veritabanından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

DB2 veritabanından desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu DB2 Bağlayıcısı, dağıtılmış Ilişkisel veritabanı mimarisi (DRDA) SQL Access Manager (, 10 ve 11) ile birlikte aşağıdaki IBM DB2 platformunu ve sürümlerini destekler:

* Z/OS 11,1 için IBM DB2
* Z/OS 10,1 için IBM DB2
* I 7,3 için IBM DB2
* I 7,2 için IBM DB2
* I 7,1 için IBM DB2
* LUW 11 için IBM DB2
* LUW 10,5 için IBM DB2
* LUW 10,1 için IBM DB2

> [!TIP]
> "SQL deyimindeki bir yürütme isteğine karşılık gelen paket bulunamadı" iletisini alırsanız bir hata mesajı alırsanız. SQLSTATE = 51002 SQLCODE =-805 ", bu nedenle söz konusu IŞLETIM sisteminde normal kullanıcı için gerekli bir paket oluşturulmaz. Bu yönergeleri DB2 sunucunuzun türüne göre izleyin:
> - I için DB2 (AS400): kopyalama etkinliğini kullanmadan önce, Power User 'ın oturum açma kullanıcısı için koleksiyon oluşturmasına izin verin. Komutundaki`create collection <username>`
> - Z/OS veya LUW için DB2: paket yetkilileri ve bağlama, BINERDD ve BIND, BINERDD ile yüksek ayrıcalıklı bir hesap-Power User veya admin kullanın-kopyalama etkinliğini bir kez çalıştırmak için, gerekli paket kopyalama sırasında otomatik olarak oluşturulur. Daha sonra, sonraki kopya çalışmalarınız için normal kullanıcıya dönebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

Integration Runtime yerleşik bir DB2 sürücüsü sağlar, bu nedenle DB2 'den veri kopyalarken herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler DB2 bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler DB2 bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği şu şekilde ayarlanmalıdır: **DB2** | Evet |
| server |DB2 sunucusunun adı. Sunucu adının sonuna iki nokta üst üste ile ayrılmış bağlantı noktası numarasını belirtebilirsiniz, örneğin `server:port`. |Evet |
| database |DB2 veritabanının adı. |Evet |
| authenticationType |DB2 veritabanına bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değer: **Temel**. |Evet |
| username |DB2 veritabanına bağlanmak için Kullanıcı adını belirtin. |Evet |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölüm, DB2 veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

DB2 'den veri kopyalamak için veri kümesinin Type özelliğini **Relationaltable**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği şu şekilde ayarlanmalıdır: **RelationalTable** | Evet |
| tableName | DB2 veritabanındaki tablonun adı. | Hayır (etkinlik kaynağı "query" belirtilmişse) |

**Örnek**

```json
{
    "name": "DB2Dataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<DB2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, DB2 kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="db2-as-source"></a>Kaynak olarak DB2

DB2 'den veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **Relationalsource**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **source** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği şu şekilde ayarlanmalıdır: **RelationalSource** | Evet |
| query | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""`. | Yok (veri kümesinde "tableName" değeri belirtilmişse) |

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
                "type": "RelationalSource",
                "query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-db2"></a>DB2 için veri türü eşlemesi

DB2 'den veri kopyalarken aşağıdaki eşlemeler DB2 veri türlerinden, geçici veri türlerini Azure Data Factory için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| DB2 veritabanı türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BigInt |Int64 |
| Binary |Byte[] |
| Blob |Byte[] |
| Char |Dize |
| CLOB |Dize |
| Date |Datetime |
| DB2DynArray |Dize |
| DbClob |Dize |
| Decimal |Decimal |
| DecimalFloat |Decimal |
| Double |Double |
| Float |Double |
| Graphic |Dize |
| Integer |Int32 |
| LongVarBinary |Byte[] |
| LongVarChar |Dize |
| LongVarGraphic |Dize |
| Numeric |Decimal |
| Real |Single |
| Smallint |Int16 |
| Time |TimeSpan |
| Timestamp |Datetime |
| Ikili |Byte[] |
| VarChar |Dize |
| VarGraphic |Dize |
| Xml |Byte[] |


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
