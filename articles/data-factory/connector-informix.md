---
title: Azure Veri Fabrikası'nı kullanarak IBM Informix kaynaklarından verileri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık bir kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına IBM Informix kaynaklarından gelen verileri nasıl kopyalaylayamamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: jingwang
ms.openlocfilehash: cb4b81f7c5e219c520078ecf34eba3f5e98da684
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75892586"
---
# <a name="copy-data-from-and-to-ibm-informix-data-stores-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri IBM Informix veri depolarından kopyalayın

Bu makalede, bir IBM Informix veri deposundan veri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Informix konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Verileri Informix kaynağından desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu Informix konektörünü kullanmak için şunları yapmanız gerekir:

- Kendi kendine barındırılan Tümleştirme Çalışma Zamanı'nı ayarlayın. Ayrıntılar için [Kendi barındırılan Tümleştirme Çalışma Zamanı](create-self-hosted-integration-runtime.md) makalesine bakın.
- Tümleştirme Runtime makinesindeki veri deposu için Informix ODBC sürücüsünü yükleyin. Örneğin, sürücü "IBM INFORMIX Informix DRIVER (64-bit)" kullanabilirsiniz.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Informix konektörüne özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi verilmiştir.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Informix bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **Informix** | Evet |
| Connectionstring | Kimlik bilgileri bölümünü hariç alan ODBC bağlantı dizesi. Bağlantı dizesini belirtebilir veya Tümleştirme Runtime makinesinde ayarladığınız Sistem DSN'yi (Veri Kaynak Adı) kullanabilirsiniz (yine de bağlantılı hizmetteki kimlik bölümünü buna göre belirtmeniz gerekir). <br> Ayrıca Azure Key Vault'a bir parola `password` koyabilir ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Azure [Anahtar Kasası'ndaki](store-credentials-in-key-vault.md) Mağaza kimlik bilgilerine daha fazla ayrıntı yla bakın.| Evet |
| authenticationType | Informix veri deposuna bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler şunlardır: **Temel** ve **Anonim**. | Evet |
| userName | Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adını belirtin. | Hayır |
| password | Kullanıcı Adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| kimlik bilgisi | Bağlantı dizesinin sürücüye özgü özellik değeri biçiminde belirtilen erişim kimlik bilgileri bölümü. Bu alanı SecureString olarak işaretleyin. | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullarda](#prerequisites)belirtildiği gibi Kendi kendine barındırılan Tümleştirme Çalışma Süresi gereklidir. |Evet |

**Örnek:**

```json
{
    "name": "InformixLinkedService",
    "properties": {
        "type": "Informix",
        "typeProperties": {
            "connectionString": "<Informix connection string or DSN>",
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Informix dataset tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Informix'teki verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **InformixTable** | Evet |
| tableName | Informix'teki tablonun adı. | Kaynak için hayır (etkinlik kaynağında "sorgu" belirtilirse);<br/>Lavabo için evet |

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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Informix kaynağı tarafından desteklenen özelliklerin bir listesi yer almaktadır.

### <a name="informix-as-source"></a>Kaynak olarak Informix

Informix'teki verileri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **InformixSource** | Evet |
| sorgu | Verileri okumak için özel sorguyu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

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

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
