---
title: Azure Veri Fabrikası'nı kullanarak Impala'dan veri kopyalama
description: Impala'dan desteklenen lavabo veri depolarına, veri fabrikası boru hattında ki bir kopyalama etkinliğini kullanarak verileri nasıl kopyalayarak kopyalayın.
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
ms.openlocfilehash: b70db03e03ce914ea1d81d94cd2803a36eccfc88
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418227"
---
# <a name="copy-data-from-impala-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Impala'dan veri kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Impala'daki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama [etkinliğine](copy-activity-overview.md) genel bir genel bakış sunan Kopyalama Etkinliği genel bakış makalesine dayanıyor.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Impala konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Impala'daki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar. Bu nedenle, bu bağlayıcıyı kullanmak için bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Impala bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Impala bağlantılı hizmet için desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **Impala**olarak ayarlanmalıdır. | Evet |
| konak | Impala sunucusunun IP adresi veya ana bilgisayar adı (yani 192.168.222.160).  | Evet |
| port | Impala sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. Varsayılan değer 21050'dir.  | Hayır |
| authenticationType | Kullanılacak kimlik doğrulama türü. <br/>İzin verilen değerler **Anonim**, **SASLUsername**ve **Kullanıcı AdıAndPassword**vardır. | Evet |
| kullanıcı adı | Impala sunucusuna erişmek için kullanılan kullanıcı adı. SASLUsername kullandığınızda varsayılan değer anonimdir.  | Hayır |
| password | UsernameAndPassword'i kullandığınızda kullanıcı adına karşılık gelen parola. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| sağlarSsl | Sunucuya bağlantıların TLS kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer **false** şeklindedir.  | Hayır |
| güvenilirCertPath | TLS üzerinden bağlandığınızda sunucuyu doğrulamak için kullanılan güvenilir CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik yalnızca KENDI barındırılan Tümleştirme Çalışma Süresi'nde TLS kullandığınızda ayarlanabilir. Varsayılan değer, tümleştirme çalışma süresiyle yüklü cacerts.pem dosyasıdır.  | Hayır |
| useSystemTrustStore | Sistem güven deposundan veya belirli bir PEM dosyasından CA sertifikası kullanıp kullanılmayacağını belirtir. Varsayılan değer **false** şeklindedir.  | Hayır |
| allowHostNameCNMismatch | TLS üzerinden bağlandığınızda sunucunun ana bilgisayar adı ile eşleşecek ŞEKILDE CA tarafından verilmiş bir TLS/SSL sertifika adı gerekip gerekmediğini belirtir. Varsayılan değer **false** şeklindedir.  | Hayır |
| izinSelfSignedServerCert | Sunucudan kendi imzalı sertifikalara izin verip vermemeyi belirtir. Varsayılan değer **false** şeklindedir.  | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "ImpalaLinkedService",
    "properties": {
        "type": "Impala",
        "typeProperties": {
            "host" : "<host>",
            "port" : "<port>",
            "authenticationType" : "UsernameAndPassword",
            "username" : "<username>",
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Impala veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Impala'dan veri kopyalamak için, veri kümesinin tür özelliğini **ImpalaObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **ImpalaObject** | Evet |
| Şema | Şema adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tablo | Masanın adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tableName | Şema ile tablonun adı. Bu özellik geriye dönük uyumluluk için desteklenir. Kullanın `schema` `table` ve yeni iş yükü için. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**

```json
{
    "name": "ImpalaDataset",
    "properties": {
        "type": "ImpalaObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Impala linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Impala kaynak türü tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="impala-as-a-source-type"></a>Kaynak türü olarak Impala

Impala'dan gelen verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **ImpalaSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği **ImpalaSource**olarak ayarlanmalıdır. | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromImpala",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Impala input dataset name>",
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
                "type": "ImpalaSource",
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
Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi [için](copy-activity-overview.md#supported-data-stores-and-formats)bkz.
