---
title: Azure Data Factory (Önizleme) kullanarak Dynamics AX 'ten veri kopyalama | Microsoft Docs
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak Dynamics AX 'ten desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: jingwang
ms.openlocfilehash: f2b1e8b9829bab56f0e49eafc50b7c56594de96b
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68720811"
---
# <a name="copy-data-from-dynamics-ax-by-using-azure-data-factory-preview"></a>Azure Data Factory kullanarak Dynamics AX 'ten veri kopyalama (Önizleme)

Bu makalede, Dynamics AX kaynağından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Dynamics AX 'ten desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Özellikle, bu Dynamics AX Connector, **hizmet sorumlusu kimlik doğrulamasıyla** **OData protokolünü** kullanarak Dynamics AX 'ten veri kopyalamayı destekler.

>[!TIP]
>Ayrıca, bu bağlayıcıyı **Dynamics 365 finans ve işlemlerinden**veri kopyalamak için de kullanabilirsiniz. Dynamics 365 ' in [OData desteği](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/data-entities/odata) ve [kimlik doğrulama yöntemi](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/data-entities/services-home-page#authentication)' ne bakın.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Dynamics AX bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Hizmet sorumlusu kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. Azure Active Directory (Azure AD) uygulama varlık kaydınızı [uygulamanızı Azure AD kiracısı ile kaydetmeniz](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Dynamics AX 'e gidin ve Dynamics AX 'e erişmek için bu hizmete asıl uygun izni verin.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Dynamics AX bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | **Type** özelliği **DynamicsAx**olarak ayarlanmalıdır. |Evet |
| url | Dynamics AX (veya Dynamics 365 finans ve Operations) örneği OData uç noktası. |Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| servicePrincipalKey | Uygulama anahtarını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| tenant | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, Azure portalının sağ üst köşedeki fare gelerek alın. | Evet |
| Aadresourceıd | Yetkilendirme için istediğiniz AAD kaynağını belirtin. Örneğin, Dynamics URL 'niz ise `https://sampledynamics.sandbox.operations.dynamics.com/data/`, karşılık gelen AAD kaynağı genellikle `https://sampledynamics.sandbox.operations.dynamics.com`olur. | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Azure Integration Runtime veya şirket içinde barındırılan Integration Runtime (veri depolubir özel ağda bulunuyorsa) seçeneğini belirleyebilirsiniz. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek**

```json
{
    "name": "DynamicsAXLinkedService",
    "properties": {
        "type": "DynamicsAX",
        "typeProperties": {
            "url": "<Dynamics AX instance OData endpoint>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource, e.g. https://sampledynamics.sandbox.operations.dynamics.com>"
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}

```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bu bölüm, Dynamics AX veri kümesinin desteklediği özelliklerin bir listesini sağlar.

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). 

Dynamics AX 'ten veri kopyalamak için veri kümesinin **Type** özelliğini **Dynamicsaxresource**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Veri kümesinin **Type** özelliği **Dynamicsaxresource**olarak ayarlanmalıdır. | Evet |
| path | Dynamics AX OData varlığının yolu. | Evet |

**Örnek**

```json
{
    "name": "DynamicsAXResourceDataset",
    "properties": {
        "type": "DynamicsAXResource",
        "typeProperties": {
            "path": "<entity path e.g. dd04tentitySet>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Dynamics AX linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölüm, Dynamics AX kaynağının desteklediği özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). 

### <a name="dynamics-ax-as-source"></a>Kaynak olarak Dynamics AX

Dynamics AX 'ten veri kopyalamak için kopyalama etkinliğindeki **kaynak** türünü **Dynamicsaxsource**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Kopyalama etkinliği kaynağının **Type** özelliği **Dynamicsaxsource**olarak ayarlanmalıdır. | Evet |
| query | Verileri filtrelemek için OData sorgu seçenekleri. Örnek: `"?$select=Name,Description&$top=5"`.<br/><br/>**Not**: Bağlayıcı, verileri birleştirilmiş URL 'den kopyalar: `[URL specified in linked service]/[path specified in dataset][query specified in copy activity source]`. Daha fazla bilgi için bkz. [OData URL bileşenleri](https://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Hayır |

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromDynamicsAX",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics AX input dataset name>",
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
                "type": "DynamicsAXSource",
                "query": "$top=10"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md##supported-data-stores-and-formats).
