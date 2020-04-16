---
title: Azure Veri Fabrikası'nı kullanarak OData kaynaklarından veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına OData kaynaklarından gelen verileri nasıl kopyalaylayamamayı öğrenin.
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
ms.openlocfilehash: c2fe6b6cc7b52dda9f2beffa444f1965723ea92a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416921"
---
# <a name="copy-data-from-an-odata-source-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Bir OData kaynağından kopyalama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-odata-connector.md)
> * [Geçerli sürüm](connector-odata.md)

Bu makalede, Bir OData kaynağından veri kopyalamak için Azure Veri Fabrikası'nda Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Makale, Azure [Veri Fabrikası'ndaki Kopyalama Etkinliği'ne](copy-activity-overview.md)dayanmaktadır ve bu da Kopyalama Etkinliğine genel bir genel bakış sunar.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu OData bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Verileri Bir OData kaynağından desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama Etkinliği'nin kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına ve biçimlere](copy-activity-overview.md#supported-data-stores-and-formats)bakın.

Özellikle, bu OData bağlayıcısı destekler:

- OData sürüm 3.0 ve 4.0.
- Aşağıdaki kimlik doğrulamalarından birini kullanarak veri kopyalama: **Anonim**, **Temel**, **Windows**, ve **AAD hizmet ilkesi**.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, OData bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanabileceğiniz özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler, OData bağlantılı bir hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Tür** özelliği **OData**olarak ayarlanmalıdır. |Evet |
| url | OData hizmetinin kök URL'si. |Evet |
| authenticationType | OData kaynağına bağlanmak için kullanılan kimlik doğrulama türü. İzin verilen değerler **Anonim,** **Temel**, **Windows**ve **AadServicePrincipal'dır.** Kullanıcı tabanlı OAuth desteklenmez. | Evet |
| userName | Temel veya Windows kimlik doğrulaması kullanıyorsanız **kullanıcı Adı** belirtin. | Hayır |
| password | Kullanıcı Adı için belirlediğiniz kullanıcı hesabının **parolasını** **belirtin.** Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** türü olarak işaretleyin. Ayrıca [Azure Key Vault'ta depolanan bir gizli ye de başvuruda](store-credentials-in-key-vault.md)bulunabilirsiniz. | Hayır |
| hizmetPrincipalId | Azure Etkin Dizin uygulamasının istemci kimliğini belirtin. | Hayır |
| aadServicePrincipalCredentialType | Hizmet asıl kimlik doğrulaması için kullanılacak kimlik bilgisi türünü belirtin. İzin verilen `ServicePrincipalKey` değerler `ServicePrincipalCert`şunlardır: veya . | Hayır |
| servicePrincipalKey | Azure Active Directory uygulamasının anahtarını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| hizmetPrincipalEmbeddedCert | Azure Active Directory'de kayıtlı uygulamanızın base64 kodlanmış sertifikasını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| hizmetPrincipalEmbeddedCertPassword | Sertifikanız bir parolayla korunuyorsa sertifikanızın parolasını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md)  | Hayır|
| Kiracı | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek alın. | Hayır |
| aadResourceId | Yetkilendirme için talep ettiğiniz AAD kaynağını belirtin.| Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresi kullanılır. |Hayır |

**Örnek 1: Anonim kimlik doğrulamayı kullanma**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: Temel kimlik doğrulamasını kullanma**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "Basic",
            "userName": "<user name>",
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

**Örnek 3: Windows kimlik doğrulamasını kullanma**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "Windows",
            "userName": "<domain>\\<user>",
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

**Örnek 4: Hizmet temel anahtar kimlik doğrulamasını kullanma**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "AadServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "aadServicePrincipalCredentialType": "ServicePrincipalKey",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource URL>"
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

**Örnek 5: Hizmet temel sertifika kimlik doğrulamasını kullanma**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "AadServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "aadServicePrincipalCredentialType": "ServicePrincipalCert",
            "servicePrincipalEmbeddedCert": { 
                "type": "SecureString", 
                "value": "<base64 encoded string of (.pfx) certificate data>"
            },
            "servicePrincipalEmbeddedCertPassword": { 
                "type": "SecureString", 
                "value": "<password of your certificate>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource e.g. https://tenant.sharepoint.com>"
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bu bölümde, OData veri kümesinin desteklediği özelliklerin bir listesi sağlar.

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Bkz. Veri Kümeleri ve bağlantılı hizmetler.](concepts-datasets-linked-services.md) 

OData'daki verileri kopyalamak için, veri kümesinin **tür** özelliğini **ODataResource**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin **tür** özelliği **ODataResource**olarak ayarlanmalıdır. | Evet |
| yol | OData kaynağına giden yol. | Evet |

**Örnek**

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<OData linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties":
        {
            "path": "Products"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama

Bu bölümde, OData kaynağının desteklediği özelliklerin bir listesi sağlar.

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md) 

### <a name="odata-as-source"></a>Kaynak olarak OData

OData'daki verileri kopyalamak için, Aşağıdaki özellikler Kopyalama Etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama Etkinliği kaynağının **tür** özelliği **ODataSource**olarak ayarlanmalıdır. | Evet |
| sorgu | Verileri filtreleme için OData sorgu seçenekleri. Örnek: `"$select=Name,Description&$top=5"`.<br/><br/>**Not**: OData bağlayıcısı verileri birleştirilmiş `[URL specified in linked service]/[path specified in dataset]?[query specified in copy activity source]`URL'den kopyalar: . Daha fazla bilgi için Bkz. [OData URL bileşenleri.](https://www.odata.org/documentation/odata-version-3-0/url-conventions/) | Hayır |

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromOData",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<OData input dataset name>",
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
                "type": "ODataSource",
                "query": "$select=Name,Description&$top=5"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Dakti-yazılı `RelationalSource` kaynak kullanıyorsanız, ileriye dönük yeni bir kaynak kullanmanız önerilirken, yine de olduğu gibi desteklenir.

## <a name="data-type-mapping-for-odata"></a>OData için veri türü eşleme

OData'daki verileri kopyaladiğinizde, OData veri türleri ile Azure Veri Fabrikası geçici veri türleri arasında aşağıdaki eşlemeler kullanılır. Copy Activity'in kaynak şemasını ve veri türünü lavaboyla nasıl eşleşdirdisini öğrenmek için [Bkz. Şema ve veri türü eşlemeleri.](copy-activity-schema-and-type-mapping.md)

| OData veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Edm.İkili | Bayt[] |
| Edm.Boolean | Bool |
| Edm.Bayt | Bayt[] |
| Edm.DateTime | DateTime |
| Edm.Ondalık | Ondalık |
| Edm.Double | Çift |
| Edm.Single | Tek |
| Edm.Guid | Guid |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Dize |
| Edm.Zaman | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> OData karmaşık veri türleri **(Object**gibi) desteklenmez.


## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası'nda Copy Activity'in kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına bakın.](copy-activity-overview.md#supported-data-stores-and-formats)