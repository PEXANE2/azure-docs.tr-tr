---
title: Dynamics AX'ten veri kopyalama
description: Bir Azure Veri Fabrikası ardışık hattında bir kopyalama etkinliği kullanarak Dynamics AX'tan desteklenen lavabo veri depolarına nasıl veri kopyalayarak nasıl kopyalaylaydestekleyeceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/01/2019
ms.openlocfilehash: 4dd82eea0a80ef81a0f972d1964a62e6c17a80c0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417376"
---
# <a name="copy-data-from-dynamics-ax-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Dynamics AX'ten verileri kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Dynamics AX kaynağından verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Makale, Azure [Veri Fabrikası'ndaki Kopyalama Etkinliği'ne](copy-activity-overview.md)dayanmaktadır ve bu da Kopyalama Etkinliğine genel bir genel bakış sunar.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Dynamics AX konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Dynamics AX'ten desteklenen herhangi bir lavabo veri deposuna veri kopyalayabilirsiniz. Kopyalama Etkinliği'nin kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına ve biçimlere](copy-activity-overview.md#supported-data-stores-and-formats)bakın.

Özellikle, bu Dynamics AX **bağlayıcısı, Service Principal kimlik doğrulaması**ile **OData protokolünü** kullanarak Dynamics AX'ten veri kopyalamayı destekler.

>[!TIP]
>Dynamics **365 Finans ve Operasyon**verilerini kopyalamak için bu bağlayıcıyı da kullanabilirsiniz. Dynamics 365'in [OData desteği](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/data-entities/odata) ve [Kimlik Doğrulama yöntemine](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/data-entities/services-home-page#authentication)bakın.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Dynamics AX bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanabileceğiniz özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="prerequisites"></a>Ön koşullar

Hizmet temel kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin:

1. Uygulamanızı [bir Azure AD kiracısıyla kaydederek](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)bir uygulama kuruluşunu Azure Etkin Dizin'e (Azure AD) kaydedin. Bağlantılı hizmeti tanımlamak için kullandığınız aşağıdaki değerlere dikkat edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Dynamics AX'e gidin ve bu hizmet müdürüne Dynamics AX'ınıza erişmek için uygun izni tanıyın.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Dynamics AX bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Tür** özelliği **DynamicsAX**olarak ayarlanmalıdır. |Evet |
| url | Dynamics AX (veya Dynamics 365 Finans ve Operasyon) örneği OData bitiş noktası. |Evet |
| hizmetPrincipalId | Uygulamanın istemci kimliğini belirtin. | Evet |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| Kiracı | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek alın. | Evet |
| aadResourceId | Yetkilendirme için talep ettiğiniz AAD kaynağını belirtin. Örneğin, Dynamics URL'niz `https://sampledynamics.sandbox.operations.dynamics.com/data/`ise, ilgili AAD `https://sampledynamics.sandbox.operations.dynamics.com`kaynağı genellikle . | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Zamanı'nı veya kendi kendine barındırılan Tümleştirme Çalışma Zamanı'nı (veri mağazanız özel bir ağda bulunuyorsa) seçebilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresi kullanılır. |Hayır |

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

Bu bölümde, Dynamics AX veri kümesinin desteklediği özelliklerin bir listesi sağlanıyor.

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Bkz. Veri Kümeleri ve bağlantılı hizmetler.](concepts-datasets-linked-services.md) 

Dynamics AX'ten gelen verileri kopyalamak için, veri kümesinin **tür** özelliğini **DynamicsAXResource**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin **tür** özelliği **DynamicsAXResource**olarak ayarlanmalıdır. | Evet |
| yol | Dynamics AX OData varlığına giden yol. | Evet |

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

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama

Bu bölümde, Dynamics AX kaynağının desteklediği özelliklerin bir listesi sağlanıyor.

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md) 

### <a name="dynamics-ax-as-source"></a>Kaynak olarak Dynamics AX

Dynamics AX'teki verileri kopyalamak **için,** Kopyalama Etkinliği'ndeki kaynak türünü **DynamicsAXSource**olarak ayarlayın. Aşağıdaki özellikler Kopyalama Etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama Etkinliği kaynağının **türü** özelliği **DynamicsAXSource**olarak ayarlanmalıdır. | Evet |
| sorgu | Verileri filtreleme için OData sorgu seçenekleri. Örnek: `"?$select=Name,Description&$top=5"`.<br/><br/>**Not**: Bağlayıcı verileri birleştirilmiş URL'den kopyalar: `[URL specified in linked service]/[path specified in dataset][query specified in copy activity source]`. Daha fazla bilgi için Bkz. [OData URL bileşenleri.](https://www.odata.org/documentation/odata-version-3-0/url-conventions/) | Hayır |

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


## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası'nda Copy Activity'in kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına bakın.](copy-activity-overview.md#supported-data-stores-and-formats)
