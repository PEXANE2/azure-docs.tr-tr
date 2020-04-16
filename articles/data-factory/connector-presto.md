---
title: Azure Veri Fabrikasını kullanarak Presto'dan veri kopyalama (Önizleme)
description: Bir Azure Veri Fabrikası ardışık hattında bir kopyalama etkinliği kullanarak Presto'dan desteklenen lavabo veri depolarına verileri nasıl kopyalayacağınızı öğrenin.
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
ms.openlocfilehash: 261bdedee56bb4de2dfbbef27358fae5ae8fdc3e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416735"
---
# <a name="copy-data-from-presto-using-azure-data-factory-preview"></a>Azure Veri Fabrikasını kullanarak Presto'dan veri kopyalama (Önizleme)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Presto'daki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

> [!IMPORTANT]
> Bu bağlayıcı şu anda önizlemede. Bunu deneyebilir ve bize geribildirim verebilirsiniz. Çözümünüzde bir önizleme bağlayıcısı bağımlılığı olmasını istiyorsanız lütfen [Azure desteğine](https://azure.microsoft.com/support/) başvurun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Presto bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Presto'daki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Azure Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Presto bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Presto bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **Presto** | Evet |
| konak | Presto sunucusunun IP adresi veya ana bilgisayar adı. (yani 192.168.222.160)  | Evet |
| Serverversion | Presto sunucusunun sürümü. (yani 0.148-t)  | Evet |
| Katalog | Sunucuya karşı tüm istek için katalog bağlamı.  | Evet |
| port | Presto sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. Varsayılan değer 8080'dir.  | Hayır |
| authenticationType | Presto sunucusuna bağlanmak için kullanılan kimlik doğrulama mekanizması. <br/>İzin verilen değerler şunlardır: **Anonim**, **LDAP** | Evet |
| kullanıcı adı | Presto sunucusuna bağlanmak için kullanılan kullanıcı adı.  | Hayır |
| password | Kullanıcı adına karşılık gelen parola. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| sağlarSsl | Sunucuya bağlantıların TLS kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer false'tur.  | Hayır |
| güvenilirCertPath | TLS üzerinden bağlanırken sunucuyu doğrulamak için güvenilir CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik yalnızca kendi barındırılan IR'de TLS kullanırken ayarlanabilir. Varsayılan değer, IR ile yüklü cacerts.pem dosyasıdır.  | Hayır |
| useSystemTrustStore | Sistem güven deposundan veya belirli bir PEM dosyasından CA sertifikası kullanıp kullanılmayacağını belirtir. Varsayılan değer false'tur.  | Hayır |
| allowHostNameCNMismatch | TLS üzerinden bağlanırken sunucunun ana bilgisayar adı ile eşleşecek şekilde CA tarafından verilmiş bir TLS/SSL sertifika adı gerekip gerekmediğini belirtir. Varsayılan değer false'tur.  | Hayır |
| izinSelfSignedServerCert | Sunucudan kendi imzalı sertifikalara izin verip vermemeyi belirtir. Varsayılan değer false'tur.  | Hayır |
| timeZoneID | Bağlantı tarafından kullanılan yerel saat dilimi. Bu seçenek için geçerli değerler IANA Saat Dilimi Veritabanında belirtilir. Varsayılan değer sistem saat dilimidir.  | Hayır |

**Örnek:**

```json
{
    "name": "PrestoLinkedService",
    "properties": {
        "type": "Presto",
        "typeProperties": {
            "host" : "<host>",
            "serverVersion" : "0.148-t",
            "catalog" : "<catalog>",
            "port" : "<port>",
            "authenticationType" : "LDAP",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            },
            "timeZoneID" : "Europe/Berlin"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Presto dataset tarafından desteklenen özelliklerin bir listesini sağlar.

Presto'dan gelen verileri kopyalamak için, veri kümesinin tür özelliğini **PrestoObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **PrestoObject** | Evet |
| Şema | Şema adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tablo | Masanın adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tableName | Şema ile tablonun adı. Bu özellik geriye dönük uyumluluk için desteklenir. Kullanın `schema` `table` ve yeni iş yükü için. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**

```json
{
    "name": "PrestoDataset",
    "properties": {
        "type": "PrestoObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Presto linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Presto kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="presto-as-source"></a>Kaynak olarak Presto

Presto'daki verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **PrestoSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **PrestoSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromPresto",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Presto input dataset name>",
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
                "type": "PrestoSource",
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
