---
title: Azure Veri Fabrikası'nı kullanarak DB2'deki verileri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına DB2'deki verileri nasıl kopyalaylayamamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: jingwang
ms.openlocfilehash: 22ecac12e049e58e533cdde0078f4a25f6bb2aa6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77423836"
---
# <a name="copy-data-from-db2-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak DB2'deki verileri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-db2-connector.md)
> * [Geçerli sürüm](connector-db2.md)

Bu makalede, Bir DB2 veritabanından verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu DB2 veritabanı bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

DB2 veritabanından desteklenen herhangi bir lavabo veri deposuna verileri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu DB2 bağlayıcısı aşağıdaki IBM DB2 platformlarını ve Distributed İlişkisel Veritabanı Mimarisi (DRDA) SQL Access Manager (SQLAM) sürüm 9, 10 ve 11 sürümlerini destekler:

* z/OS 12.1 için IBM DB2
* z/OS 11.1 için IBM DB2
* z/OS 10.1 için IBM DB2
* i 7.3 için IBM DB2
* i 7.2 için IBM DB2
* i 7.1 için IBM DB2
* LUW 11 için IBM DB2
* LUW 10.5 için IBM DB2
* LUW 10.1 için IBM DB2

>[!TIP]
>DB2 konektörü, DB2 için Microsoft OLE DB Sağlayıcısı'nın üzerine inşa edilmiştir. DB2 bağlayıcı hatalarını gidermek için [Veri Sağlayıcısı Hata Kodları'na](https://docs.microsoft.com/host-integration-server/db2oledbv/data-provider-error-codes#drda-protocol-errors)bakın.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

Tümleştirme Runtime yerleşik bir DB2 sürücüsü sağlar, bu nedenle DB2 verileri kopyalarken herhangi bir sürücü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde DB2 bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler DB2 bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **Db2** | Evet |
| sunucu |DB2 sunucusunun adı. Sunucu adını izleyerek bağlantı noktası numarasını belirtebilirsiniz. `server:port` |Evet |
| database |DB2 veritabanının adı. |Evet |
| authenticationType |DB2 veritabanına bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değer: **Temel**. |Evet |
| kullanıcı adı |DB2 veritabanına bağlanmak için kullanıcı adını belirtin. |Evet |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) |Evet |
| paketKoleksiyon | Veritabanı nı sorgularken gerekli paketlerin ADF tarafından otomatik olarak nerede oluşturulduğunu aşağıda belirtin. | Hayır |
| sertifikaCommonName | Güvenli Soketkatmanı (SSL) veya Aktarım Katmanı Güvenliği (TLS) şifrelemesi kullandığınızda, Sertifika ortak adı için bir değer girmeniz gerekir. | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

> [!TIP]
> Bir hata iletisi `The package corresponding to an SQL statement execution request was not found. SQLSTATE=51002 SQLCODE=-805`alırsanız, nedeni kullanıcı için gerekli bir paket oluşturulmaz. Varsayılan olarak, ADF, DB2'ye bağlanmak için kullandığınız kullanıcı olarak adlandırılan koleksiyon altında bir paket oluşturmaya çalışır. Veritabanını sorgularken ADF'nin gerekli paketleri nerede oluşturmasını istediğinizi belirtmek için paket koleksiyonu özelliğini belirtin.

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde DB2 veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

DB2'deki verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği ayarlanmalıdır: **Db2Table** | Evet |
| Şema | Şema adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tablo | Masanın adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tableName | Şema ile tablonun adı. Bu özellik geriye dönük uyumluluk için desteklenir. Kullanın `schema` `table` ve yeni iş yükü için. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

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

Dakti-zimd veri kümesi kullanıyorsanız, `RelationalTable` ileriye dönük yenisini kullanmanız önerilirken, yine de olduğu gibi desteklenir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde DB2 kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="db2-as-source"></a>Kaynak olarak DB2

DB2'deki verileri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **Db2Source** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

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

Dakti-yazılı `RelationalSource` kaynak kullanıyorsanız, ileriye dönük yeni bir kaynak kullanmanız önerilirken, yine de olduğu gibi desteklenir.

## <a name="data-type-mapping-for-db2"></a>DB2 için veri türü eşleme

DB2'den veri kopyalanırken, DB2 veri türlerinden Azure Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopya etkinliği kaynak şemasını ve veri türünü lavaboyla nasıl eşler hakkında bilgi edinmek için Şema ve [veri türü eşlemelerine](copy-activity-schema-and-type-mapping.md) bakın.

| DB2 Veritabanı türü | Veri fabrikası geçici veri türü |
|:--- |:--- |
| Bigint |Int64 |
| İkili |Bayt[] |
| Blob |Bayt[] |
| Char |Dize |
| Clob |Dize |
| Tarih |Tarih saat |
| DB2Dynarray |Dize |
| DbClob |Dize |
| Ondalık |Ondalık |
| Ondalık Float |Ondalık |
| Çift |Çift |
| Kayan |Çift |
| Grafik |Dize |
| Tamsayı |Int32 |
| LongVarBinary |Bayt[] |
| Longvarchar |Dize |
| Longvargraphic |Dize |
| Sayısal |Ondalık |
| Gerçek |Tek |
| Smallint |Int16 |
| Zaman |TimeSpan |
| Zaman damgası |DateTime |
| Varbinary |Bayt[] |
| Varchar |Dize |
| Vargraphıc |Dize |
| Xml |Bayt[] |

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
