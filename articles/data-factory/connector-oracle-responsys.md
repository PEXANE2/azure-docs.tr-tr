---
title: Oracle Responsys 'tan veri kopyalama (Önizleme)
description: Desteklenen bir havuz veri depolarına Oracle Responsys bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
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
ms.openlocfilehash: 67c737423b0b2be4c666974feb2f10ca5cdf1111
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74929096"
---
# <a name="copy-data-from-oracle-responsys-using-azure-data-factory-preview"></a>Oracle Responsys Azure Data Factory (Önizleme) kullanarak verileri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de Oracle Responsys verileri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

> [!IMPORTANT]
> Bu bağlayıcı, şu anda Önizleme aşamasındadır. Deneyin ve geri bildirimde bulunun. Çözümünüzde bir önizleme bağlayıcısı bağımlılığı olmasını istiyorsanız lütfen [Azure desteğine](https://azure.microsoft.com/support/) başvurun.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Oracle Responsys Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Oracle Responsys tüm desteklenen havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlangıç

.NET SDK'sı, Python SDK'sı, Azure PowerShell, REST API'si veya Azure Resource Manager şablonu kullanarak kopyalama etkinlikli bir işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği Öğreticisi](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Aşağıdaki bölümler, Data Factory varlıklarını belirli Oracle Responsys bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Oracle Responsys bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Responsys** | Yes |
| endpoint | Respopnsys sunucu uç noktası  | Yes |
| clientID | Responsys uygulama ile ilişkili istemci kimliği.  | Yes |
| clientSecret | Responsys uygulamayla ilişkili gizli. Bu alanı, ADF 'de güvenli bir şekilde depolamak veya Azure Key Vault parolayı depolamak için bir SecureString olarak işaretlemeyi seçebilir ve veri kopyalama işlemini gerçekleştirirken ADF kopyalama etkinliği çekmeye izin verebilir. [Key Vault mağaza kimlik bilgilerinden](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Yes |
| useEncryptedEndpoints | Veri kaynağı uç noktaları HTTPS kullanılarak şifrelenmiş olup olmadığını belirtir. Varsayılan değer true olur.  | Hayır |
| useHostVerification | Ana bilgisayar adı sunucunun sertifikasında SSL üzerinden bağlanırken sunucu ana bilgisayar adıyla eşleşmesi gerekip gerekmediğini belirtir. Varsayılan değer true olur.  | Hayır |
| usePeerVerification | SSL üzerinden bağlanırken sunucu kimliğinin doğrulanıp doğrulanmayacağını belirtir. Varsayılan değer true olur.  | Hayır |

**Örnek:**

```json
{
    "name": "OracleResponsysLinkedService",
    "properties": {
        "type": "Responsys",
        "typeProperties": {
            "endpoint" : "<endpoint>",
            "clientId" : "<clientId>",
            "clientSecret": {
                 "type": "SecureString",
                 "value": "<clientSecret>"
            },
            "useEncryptedEndpoints" : true,
            "useHostVerification" : true,
            "usePeerVerification" : true
        }
    }
}

```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Oracle Responsys veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Oracle Responsys verileri kopyalamak için dataset öğesinin type özelliği ayarlamak **ResponsysObject**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **ResponsysObject** | Yes |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı "query" belirtilmişse) |

**Örnek**

```json
{
    "name": "OracleResponsysDataset",
    "properties": {
        "type": "ResponsysObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Oracle Responsys linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}

```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Oracle Responsys kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="oracle-responsys-as-source"></a>Oracle Responsys kaynağı olarak

Oracle Responsys verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **ResponsysSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **source** bölümü:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **ResponsysSource** | Yes |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Yok (veri kümesinde "tableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromOracleResponsys",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle Responsys input dataset name>",
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
                "type": "ResponsysSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
