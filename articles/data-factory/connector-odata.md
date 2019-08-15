---
title: Azure Data Factory kullanarak OData kaynaklarından veri kopyalama | Microsoft Docs
description: Azure Data Factory işlem hattındaki kopyalama etkinliğini kullanarak OData kaynaklarından desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: 30bad3dd519d622d7e224da7bd53e7c6625014f6
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68966471"
---
# <a name="copy-data-from-an-odata-source-by-using-azure-data-factory"></a>Azure Data Factory kullanarak OData kaynağından veri kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-odata-connector.md)
> * [Geçerli sürüm](connector-odata.md)

Bu makalede, bir OData kaynağından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

## <a name="supported-capabilities"></a>Desteklenen özellikler

OData kaynağından desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Özellikle, bu OData Bağlayıcısı şunları destekler:

- OData sürüm 3,0 ve 4,0.
- Aşağıdaki kimlik doğrulamalarından birini kullanarak verileri kopyalama: **Azure kaynakları Için** **anonim**, **temel**, **Windows**, **AAD hizmet sorumlusu**ve yönetilen kimlikler.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, bir OData bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanabileceğiniz özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

OData bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | **Type** özelliği **OData**olarak ayarlanmalıdır. |Evet |
| url | OData hizmetinin kök URL 'SI. |Evet |
| authenticationType | OData kaynağına bağlanmak için kullanılan kimlik doğrulaması türü. İzin verilen değerler **anonim**, **temel**, **Windows**, **Aadserviceprincipal**ve **managedserviceıdentity**. Kullanıcı tabanlı OAuth desteklenmez. | Evet |
| userName | Belirtin **userName** temel veya Windows kimlik doğrulamasını kullanır. | Hayır |
| password | Belirtin **password** , belirtilen kullanıcı için hesap **userName**. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca, [Azure Key Vault depolanan bir gizli](store-credentials-in-key-vault.md)dizi için de başvurabilirsiniz. | Hayır |
| servicePrincipalId | Azure Active Directory uygulamasının istemci KIMLIĞINI belirtin. | Hayır |
| aadServicePrincipalCredentialType | Hizmet sorumlusu kimlik doğrulaması için kullanılacak kimlik bilgisi türünü belirtin. İzin verilen değerler: `ServicePrincipalKey` veya `ServicePrincipalCert`. | Hayır |
| servicePrincipalKey | Azure Active Directory uygulamasının anahtarını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |
| Serviceprincıpalimon Beddedcert | Azure Active Directory kayıtlı olan uygulamanızın Base64 olarak kodlanmış sertifikasını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |
| Serviceprincıpalimon Beddedcertpassword | Sertifikanızın bir parolayla güvenliği varsa sertifikanızın parolasını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md).  | Hayır|
| tenant | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, Azure portalının sağ üst köşedeki fare gelerek alın. | Hayır |
| Aadresourceıd | Yetkilendirme için istediğiniz AAD kaynağını belirtin.| Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek 1: Anonim kimlik doğrulaması kullanma**

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

**Örnek 2: Temel kimlik doğrulaması kullanma**

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

**Örnek 4: Hizmet sorumlusu anahtar kimlik doğrulamasını kullanma**

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

**Örnek 5: Hizmet sorumlusu sertifika kimlik doğrulamasını kullanma**

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

Bu bölüm, OData veri kümesinin desteklediği özelliklerin bir listesini sağlar.

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). 

OData 'ten veri kopyalamak için, veri kümesinin **Type** özelliğini **ODataResource**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Veri kümesinin **Type** özelliği **ODataResource**olarak ayarlanmalıdır. | Evet |
| path | OData kaynağının yolu. | Evet |

**Örnek**

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölüm, OData kaynağının desteklediği özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). 

### <a name="odata-as-source"></a>Kaynak olarak OData

OData 'ten veri kopyalamak için kopyalama etkinliğindeki **kaynak** türünü **relationalsource**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Kopyalama etkinliği kaynağının **Type** özelliği, **relationalsource**olarak ayarlanmalıdır. | Evet |
| query | Verileri filtrelemek için OData sorgu seçenekleri. Örnek: `"$select=Name,Description&$top=5"`.<br/><br/>**Not**: OData Bağlayıcısı Birleşik URL 'den verileri kopyalar: `[URL specified in linked service]/[path specified in dataset]?[query specified in copy activity source]`. Daha fazla bilgi için bkz. [OData URL bileşenleri](https://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Hayır |

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
                "type": "RelationalSource",
                "query": "$select=Name,Description&$top=5"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-odata"></a>OData için veri türü eşlemesi

OData 'ten veri kopyaladığınızda, OData veri türleri ve Azure Data Factory geçici veri türleri arasında aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| OData veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| EDM. Binary | Byte[] |
| Edm.Boolean | Bool |
| EDM. Byte | Byte[] |
| Edm.DateTime | DateTime |
| EDM. Decimal | Decimal |
| Edm.Double | Double |
| EDM. Single | Single |
| EDM. Guid | Guid |
| EDM. Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| EDM. SByte | Int16 |
| Edm.String | String |
| EDM. Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> OData karmaşık veri türleri (örneğin, **nesne**) desteklenmez.


## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md##supported-data-stores-and-formats).