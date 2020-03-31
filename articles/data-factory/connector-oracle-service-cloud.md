---
title: Oracle Service Cloud'dan veri kopyalama (Önizleme)
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopya etkinliği kullanarak Oracle Service Cloud'dan desteklenen lavabo veri depolarına verileri nasıl kopyalaylayamamayı öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/01/2019
ms.openlocfilehash: c655ddf47318951f01cb86841a72389489941db2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930517"
---
# <a name="copy-data-from-oracle-service-cloud-using-azure-data-factory-preview"></a>Azure Veri Fabrikasını kullanarak Oracle Service Cloud'dan veri kopyalama (Önizleme)

Bu makalede, Oracle Service Cloud'dan verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

> [!IMPORTANT]
> Bu bağlayıcı şu anda önizlemede. Bunu deneyebilir ve geribildirim sağlayabilirsiniz. Çözümünüzde bir önizleme bağlayıcısı bağımlılığı olmasını istiyorsanız lütfen [Azure desteğine](https://azure.microsoft.com/support/) başvurun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Oracle Service Cloud bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Oracle Service Cloud'daki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Azure Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Oracle Service Cloud bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Oracle Service Cloud bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **OracleServiceCloud** | Evet |
| konak | Oracle Service Cloud örneğinin URL'si.  | Evet |
| kullanıcı adı | Oracle Service Cloud sunucusuna erişmek için kullandığınız kullanıcı adı.  | Evet |
| password | Kullanıcı adı anahtarında sağladığınız kullanıcı adına karşılık gelen parola. Bu alanı Güvenli String olarak işaretlemeyi seçip ADF'de güvenli bir şekilde depolamayı veya parolayı Azure Key Vault'ta depolamayı ve ADF'nin veri kopyalama gerçekleştirirken etkinlik çekmesine izin verebilirsiniz - [Key Vault'taki Mağaza kimlik numaralarından](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Evet |
| useEncryptedEndpoints | Veri kaynağı uç noktalarının HTTPS kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer true şeklindedir.  | Hayır |
| useHostVerification | SSL üzerinden bağlanırken sunucunun sertifikasındaki ana bilgisayar adının sunucunun ana bilgisayar adıile eşleşip eşleşmeyeceğini belirtir. Varsayılan değer true şeklindedir.  | Hayır |
| usePeerVerification | SSL üzerinden bağlanırken sunucunun kimliğinin doğrulanıp doğrulamayacağı belirtilir. Varsayılan değer true şeklindedir.  | Hayır |

**Örnek:**

```json
{
    "name": "OracleServiceCloudLinkedService",
    "properties": {
        "type": "OracleServiceCloud",
        "typeProperties": {
            "host" : "<host>",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            },
            "useEncryptedEndpoints" : true,
            "useHostVerification" : true,
            "usePeerVerification" : true,
        }
    }
}

```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Oracle Service Cloud veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Oracle Service Cloud'dan veri kopyalamak için, veri kümesinin tür özelliğini **OracleServiceCloudObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **OracleServiceCloudObject** | Evet |
| tableName | Masanın adı. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**

```json
{
    "name": "OracleServiceCloudDataset",
    "properties": {
        "type": "OracleServiceCloudObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<OracleServiceCloud linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}

```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Oracle Service Cloud kaynağı tarafından desteklenen özelliklerin bir listesi yer almaktadır.

### <a name="oracle-service-cloud-as-source"></a>Kaynak olarak Oracle Service Cloud

Oracle Service Cloud'daki verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **OracleServiceCloudSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının tür özelliği şu şekilde ayarlanmalıdır: **OracleServiceCloudSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromOracleServiceCloud",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<OracleServiceCloud input dataset name>",
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
                "type": "OracleServiceCloudSource",
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
