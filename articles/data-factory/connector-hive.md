---
title: Azure Veri Fabrikası'nı kullanarak Kovan'dan veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak Hive'dan desteklenen lavabo veri depolarına verileri nasıl kopyalayatılayısı öğrenin.
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
ms.openlocfilehash: 3720d917d71fa4e8c5a14bb60fdc7c405be4bfdb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81410449"
---
# <a name="copy-data-from-hive-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Kovan'dan veri kopyalama 
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Hive'daki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Kovan konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Hive'daki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Azure Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Kovan konektörüne özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Hive bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **Kovan** | Evet |
| konak | Birden çok ana bilgisayar için ';' ile ayrılan Hive sunucusunun IP adresi veya ana bilgisayar adı (yalnızca serviceDiscoveryMode etkinleştirildiğinde).  | Evet |
| port | Hive sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. Azure HDInsights'a bağlanırsanız, bağlantı noktasını 443 olarak belirtin. | Evet |
| Servertype | Hive sunucusu türü. <br/>İzin verilen değerler şunlardır: **HiveServer1**, **HiveServer2**, **HiveThriftServer** | Hayır |
| tasarrufTransportProtocol | Thrift katmanında kullanılacak taşıma protokolü. <br/>İzin verilen değerler şunlardır: **İkili**, **SASL**, **HTTP** | Hayır |
| authenticationType | Hive sunucusuna erişmek için kullanılan kimlik doğrulama yöntemi. <br/>İzin verilen değerler şunlardır: **Anonim**, **Kullanıcı Adı**, **Kullanıcı AdıAndPassword**, **WindowsAzureHDInsightService** | Evet |
| serviceDiscoveryMode | ZooKeeper hizmeti kullanarak göstermek için doğru, yanlış değil.  | Hayır |
| zooKeeperNameSpace | ZooKeeper'ın altında Hive Server 2 düğümlerinin eklendiği ad alanı.  | Hayır |
| useNativeQuery | Sürücünün yerel HiveQL sorgularını kullanıp kullanmadığını veya hiveql'de eşdeğer bir forma dönüştürüp dönüştürmediğini belirtir.  | Hayır |
| kullanıcı adı | Hive Server'a erişmek için kullandığınız kullanıcı adı.  | Hayır |
| password | Kullanıcıya karşılık gelen parola. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| httpPath | Hive sunucusuna karşılık gelen kısmi URL.  | Hayır |
| sağlarSsl | Sunucuya bağlantıların TLS kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer false'tur.  | Hayır |
| güvenilirCertPath | TLS üzerinden bağlanırken sunucuyu doğrulamak için güvenilir CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik yalnızca kendi barındırılan IR'de TLS kullanırken ayarlanabilir. Varsayılan değer, IR ile yüklü cacerts.pem dosyasıdır.  | Hayır |
| useSystemTrustStore | Sistem güven deposundan veya belirli bir PEM dosyasından CA sertifikası kullanıp kullanılmayacağını belirtir. Varsayılan değer false'tur.  | Hayır |
| allowHostNameCNMismatch | TLS üzerinden bağlanırken sunucunun ana bilgisayar adı ile eşleşecek şekilde CA tarafından verilmiş bir TLS/SSL sertifika adı gerekip gerekmediğini belirtir. Varsayılan değer false'tur.  | Hayır |
| izinSelfSignedServerCert | Sunucudan kendi imzalı sertifikalara izin verip vermemeyi belirtir. Varsayılan değer false'tur.  | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "HiveLinkedService",
    "properties": {
        "type": "Hive",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.net",
            "port" : "<port>",
            "authenticationType" : "WindowsAzureHDInsightService",
            "username" : "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde Hive dataset tarafından desteklenen özelliklerin bir listesini sağlar.

Hive'dan veri kopyalamak için, veri kümesinin tür özelliğini **HiveObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **HiveObject** | Evet |
| Şema | Şema adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tablo | Masanın adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tableName | Şema kısmını içeren tablonun adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü `schema` `table`için, kullanım ve . | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**

```json
{
    "name": "HiveDataset",
    "properties": {
        "type": "HiveObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Hive linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Hive kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="hivesource-as-source"></a>Kaynak olarak HiveSource

Hive'dan veri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **HiveSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **HiveSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromHive",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Hive input dataset name>",
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
                "type": "HiveSource",
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
