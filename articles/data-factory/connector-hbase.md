---
title: Azure Veri Fabrikası'nı kullanarak HBase'ten veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopya etkinliği kullanarak HBase'den desteklenen lavabo veri depolarına verileri nasıl kopyalayatılayış edin.
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
ms.openlocfilehash: 2dfb2a7766ddbda5dd27d5b4fd6745836ad1dc75
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74929368"
---
# <a name="copy-data-from-hbase-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak HBase'ten veri kopyalama 

Bu makalede, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin HBase'ten verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu HBase konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

HBase'deki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Azure Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, HBase bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler HBase bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **HBase** | Evet |
| konak | HBase sunucusunun IP adresi veya ana bilgisayar adı. (yani.  `[clustername].azurehdinsight.net`, `192.168.222.160`( ) ( )  | Evet |
| port | HBase örneğinin istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. Varsayılan değer 9090'dır. Azure HDInsights'a bağlanırsanız, bağlantı noktasını 443 olarak belirtin. | Hayır |
| httpPath | HdInsights kümesini `/hbaserest0` kullanırken, Örneğin HBase sunucusuna karşılık gelen kısmi URL. | Hayır |
| authenticationType | HBase sunucusuna bağlanmak için kullanılacak kimlik doğrulama mekanizması. <br/>İzin verilen değerler şunlardır: **Anonim**, **Temel** | Evet |
| kullanıcı adı | HBase örneğine bağlanmak için kullanılan kullanıcı adı.  | Hayır |
| password | Kullanıcı adına karşılık gelen parola. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| sağlarSsl | Sunucuya bağlantıların SSL kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer false'tur.  | Hayır |
| güvenilirCertPath | SSL üzerinden bağlanırken sunucuyu doğrulamak için güvenilir CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik yalnızca kendi barındırılan IR'de SSL kullanırken ayarlanabilir. Varsayılan değer, IR ile yüklü cacerts.pem dosyasıdır.  | Hayır |
| allowHostNameCNMismatch | SSL üzerinden bağlanırken sunucunun ana bilgisayar adı ile eşleşecek şekilde CA tarafından verilmiş bir SSL sertifika adı gerekip gerekmediğini belirtir. Varsayılan değer false'tur.  | Hayır |
| izinSelfSignedServerCert | Sunucudan kendi imzalı sertifikalara izin verip vermemeyi belirtir. Varsayılan değer false'tur.  | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

>[!NOTE]
>Kümeniz HDInsight gibi yapışkan oturumu desteklemiyorsa, http yol ayarına açıkça düğüm dizini ekleyin, örneğin `/hbaserest0` bunun `/hbaserest`yerine belirtin.

**HDInsights HBase için örnek:**

```json
{
    "name": "HBaseLinkedService",
    "properties": {
        "type": "HBase",
        "typeProperties": {
            "host" : "<cluster name>.azurehdinsight.net",
            "port" : "443",
            "httpPath" : "/hbaserest0",
            "authenticationType" : "Basic",
            "username" : "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "enableSsl" : true
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Genel HBase için örnek:**

```json
{
    "name": "HBaseLinkedService",
    "properties": {
        "type": "HBase",
        "typeProperties": {
            "host" : "<host e.g. 192.168.222.160>",
            "port" : "<port>",
            "httpPath" : "<e.g. /gateway/sandbox/hbase/version>",
            "authenticationType" : "Basic",
            "username" : "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "enableSsl" : true,
            "trustedCertPath" : "<trustedCertPath>",
            "allowHostNameCNMismatch" : true,
            "allowSelfSignedServerCert" : true
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, HBase veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

HBase'den veri kopyalamak için, veri kümesinin tür özelliğini **HBaseObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **HBaseObject** | Evet |
| tableName | Masanın adı. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**

```json
{
    "name": "HBaseDataset",
    "properties": {
        "type": "HBaseObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<HBase linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, HBase kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="hbasesource-as-source"></a>Kaynak olarak HBaseSource

HBase'den veri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **HBaseSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği şu şekilde ayarlanmalıdır: **HBaseSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromHBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<HBase input dataset name>",
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
                "type": "HBaseSource",
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
