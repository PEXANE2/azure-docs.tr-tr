---
title: Azure Data Factory kullanarak IBM Informix kaynaklarından veri kopyalama | Microsoft Docs
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliği kullanarak IBM Informix kaynaklarından desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: jingwang
ms.openlocfilehash: 66ae9fa1ff4038a77ecc0b7046434d4d606f5015
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68842575"
---
# <a name="copy-data-from-and-to-ibm-informix-data-stores-using-azure-data-factory"></a>Azure Data Factory kullanarak IBM Informix veri depolarından veri kopyalama

Bu makalede, bir IBM Informix veri deposundan veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Informix kaynağından, desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

## <a name="prerequisites"></a>Önkoşullar

Bu Informix bağlayıcısını kullanmak için şunları yapmanız gerekir:

- Şirket içinde barındırılan bir Integration Runtime ayarlayın. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.
- Integration Runtime makinesindeki veri deposu için Informix ODBC sürücüsünü yükler. Örneğin, "IBM ıNFORMIX Informix sürücü (64-bit)" sürücüsünü kullanabilirsiniz.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Informix bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Informix bağlantılı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği şu şekilde ayarlanmalıdır: **Informix** | Evet |
| connectionString | Kimlik bilgisi bölümünü dışlayarak ODBC bağlantı dizesi. Bağlantı dizesini belirtebilir veya Integration Runtime makinesinde ayarladığınız Sistem DSN 'sini (veri kaynağı adı) kullanabilirsiniz (yine de bağlı hizmette kimlik bilgisi bölümünü de belirtmeniz gerekir).<br>Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md).| Evet |
| authenticationType | Informix veri deposuna bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değerler şunlardır: **Temel** ve **anonim**. | Evet |
| userName | Temel kimlik doğrulaması kullanıyorsanız Kullanıcı adını belirtin. | Hayır |
| password | Kullanıcı adı için belirttiğiniz kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |
| credential | Sürücüye özgü özellik-değer biçiminde belirtilen bağlantı dizesinin erişim kimlik bilgisi kısmı. Bu alanı SecureString olarak işaretleyin. | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşul](#prerequisites)bölümünde belirtildiği gibi, kendinden konak Integration Runtime gereklidir. |Evet |

**Örnek:**

```json
{
    "name": "InformixLinkedService",
    "properties": {
        "type": "Informix",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<Informix connection string or DSN>"
            },
            "authenticationType": "Basic",
            "userName": "<username>",
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölüm, Informix veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Informix verilerini kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği şu şekilde ayarlanmalıdır: **Informixtable** | Evet |
| tableName | Informix içindeki tablonun adı. | Kaynak için Hayır (etkinlik kaynağı içinde "sorgu" belirtilmişse);<br/>Havuz için Evet |

**Örnek**

```json
{
    "name": "InformixDataset",
    "properties": {
        "type": "InformixTable",
        "linkedServiceName": {
            "referenceName": "<Informix linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm Informix kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="informix-as-source"></a>Kaynak olarak Informix

Informix verilerini kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği şu şekilde ayarlanmalıdır: **Informixsource** | Evet |
| query | Verileri okumak için özel sorguyu kullanın. Örneğin: `"SELECT * FROM MyTable"` | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromInformix",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Informix input dataset name>",
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
                "type": "InformixSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
