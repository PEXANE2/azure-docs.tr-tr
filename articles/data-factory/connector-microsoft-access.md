---
title: Microsoft Access kaynaklarından veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopya etkinliği kullanarak desteklenen lavabo veri depolarına Microsoft Access kaynaklarından gelen verileri nasıl kopyalaylayamamayı öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/27/2019
ms.openlocfilehash: d6110065e28b8f179cd1d113107fb3508e1c3e44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75892540"
---
# <a name="copy-data-from-and-to-microsoft-access-data-stores-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Microsoft Access veri depolarından kopyalayın

Bu makalede, Microsoft Access veri deposundaki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Microsoft Access bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Microsoft Access kaynağından desteklenen herhangi bir lavabo veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu Microsoft Access bağlayıcısını kullanmak için şunları yapmanız gerekir:

- Kendi kendine barındırılan Tümleştirme Çalışma Zamanı'nı ayarlayın. Ayrıntılar için [Kendi barındırılan Tümleştirme Çalışma Zamanı](create-self-hosted-integration-runtime.md) makalesine bakın.
- Tümleştirme Runtime makinesinde veri deposu için Microsoft Access ODBC sürücüsünü yükleyin.

>[!NOTE]
>ODBC sürücüsünün Microsoft Access 2016 sürümü bu bağlayıcıyla çalışmaz. Bunun yerine sürücü sürümü 2013 veya 2010'u kullanın.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Microsoft Access bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Microsoft Access bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **MicrosoftAccess** | Evet |
| Connectionstring | Kimlik bilgileri bölümünü hariç alan ODBC bağlantı dizesi. Bağlantı dizesini belirtebilir veya Tümleştirme Runtime makinesinde ayarladığınız Sistem DSN'yi (Veri Kaynak Adı) kullanabilirsiniz (yine de bağlantılı hizmetteki kimlik bölümünü buna göre belirtmeniz gerekir).<br> Ayrıca Azure Key Vault'a bir parola `password` koyabilir ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz.Azure [Anahtar Kasası'ndaki](store-credentials-in-key-vault.md) Mağaza kimlik bilgilerine daha fazla ayrıntı yla bakın.| Evet |
| authenticationType | Microsoft Access veri deposuna bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler şunlardır: **Temel** ve **Anonim**. | Evet |
| userName | Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adını belirtin. | Hayır |
| password | Kullanıcı Adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| kimlik bilgisi | Bağlantı dizesinin sürücüye özgü özellik değeri biçiminde belirtilen erişim kimlik bilgileri bölümü. Bu alanı SecureString olarak işaretleyin. | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullarda](#prerequisites)belirtildiği gibi Kendi kendine barındırılan Tümleştirme Çalışma Süresi gereklidir. |Evet |

**Örnek:**

```json
{
    "name": "MicrosoftAccessLinkedService",
    "properties": {
        "type": "Microsoft Access",
        "typeProperties": {
            "connectionString": "Driver={Microsoft Access Driver (*.mdb, *.accdb)};Dbq=<path to your DB file e.g. C:\\mydatabase.accdb>;",
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Microsoft Access veri kümesi tarafından desteklenen özelliklerin bir listesi bulunmaktadır.

Microsoft Access'ten veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **MicrosoftAccessTable** | Evet |
| tableName | Microsoft Access'teki tablonun adı. | Kaynak için hayır (etkinlik kaynağında "sorgu" belirtilirse);<br/>Lavabo için evet |

**Örnek**

```json
{
    "name": "MicrosoftAccessDataset",
    "properties": {
        "type": "MicrosoftAccessTable",
        "linkedServiceName": {
            "referenceName": "<Microsoft Access linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Microsoft Access kaynağı tarafından desteklenen özelliklerin bir listesi sağlanır.

### <a name="microsoft-access-as-source"></a>Kaynak olarak Microsoft Access

Microsoft Access uyumlu veri deposundan veri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının tür özelliği şu şekilde ayarlanmalıdır: **MicrosoftAccessSource** | Evet |
| sorgu | Verileri okumak için özel sorguyu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromMicrosoftAccess",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Microsoft Access input dataset name>",
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
                "type": "MicrosoftAccessSource",
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
