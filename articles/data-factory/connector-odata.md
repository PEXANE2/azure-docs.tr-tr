---
title: Azure Data Factory kullanarak OData kaynaklarından veri kopyalama
description: Azure Data Factory işlem hattındaki kopyalama etkinliğini kullanarak OData kaynaklarından desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
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
ms.openlocfilehash: e70cd6d7745b2754bdda6992c587c5c72643869b
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74912503"
---
# <a name="copy-data-from-an-odata-source-by-using-azure-data-factory"></a>Azure Data Factory kullanarak OData kaynağından veri kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-odata-connector.md)
> * [Geçerli sürüm](connector-odata.md)

Bu makalede, bir OData kaynağından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu OData Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

OData kaynağından desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Özellikle, bu OData Bağlayıcısı şunları destekler:

- OData sürüm 3,0 ve 4,0.
- Aşağıdaki kimlik doğrulamalarından birini kullanarak verileri kopyalama: **anonim**, **temel**, **Windows**, **AAD hizmet sorumlusu**ve **Azure kaynakları için Yönetilen kimlikler**.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>Kullanmaya Başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, bir OData bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanabileceğiniz özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

OData bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | **Type** özelliği **OData**olarak ayarlanmalıdır. |Yes |
| url | OData hizmetinin kök URL 'SI. |Yes |
| authenticationType | OData kaynağına bağlanmak için kullanılan kimlik doğrulaması türü. İzin verilen değerler **anonim**, **temel**, **Windows**, **Aadserviceprincipal**ve **managedserviceıdentity**. Kullanıcı tabanlı OAuth desteklenmez. | Yes |
| userName adı | Belirtin **userName** temel veya Windows kimlik doğrulamasını kullanır. | Hayır |
| password | Belirtin **password** , belirtilen kullanıcı için hesap **userName**. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca, [Azure Key Vault depolanan bir gizli](store-credentials-in-key-vault.md)dizi için de başvurabilirsiniz. | Hayır |
| servicePrincipalId | Azure Active Directory uygulamasının istemci KIMLIĞINI belirtin. | Hayır |
| aadServicePrincipalCredentialType | Hizmet sorumlusu kimlik doğrulaması için kullanılacak kimlik bilgisi türünü belirtin. İzin verilen değerler: `ServicePrincipalKey` veya `ServicePrincipalCert`. | Hayır |
| servicePrincipalKey | Azure Active Directory uygulamasının anahtarını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |
| Serviceprincıpalimon Beddedcert | Azure Active Directory kayıtlı olan uygulamanızın Base64 olarak kodlanmış sertifikasını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |
| Serviceprincıpalimon Beddedcertpassword | Sertifikanızın bir parolayla güvenliği varsa sertifikanızın parolasını belirtin. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md).  | Hayır|
| tenant | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, Azure portalının sağ üst köşedeki fare gelerek alın. | Hayır |
| Aadresourceıd | Yetkilendirme için istediğiniz AAD kaynağını belirtin.| Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek 1: anonim kimlik doğrulaması kullanma**

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

**Örnek 2: temel kimlik doğrulaması kullanma**

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

**Örnek 4: hizmet sorumlusu anahtar kimlik doğrulamasını kullanma**

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

**Örnek 5: hizmet sorumlusu sertifika kimlik doğrulamasını kullanma**

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

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Veri kümesinin **Type** özelliği **ODataResource**olarak ayarlanmalıdır. | Yes |
| yol | OData kaynağının yolu. | Yes |

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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölüm, OData kaynağının desteklediği özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). 

### <a name="odata-as-source"></a>Kaynak olarak OData

OData 'ten veri kopyalamak için aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının **Type** özelliği **odatasource**olarak ayarlanmalıdır. | Yes |
| sorgu | Verileri filtrelemek için OData sorgu seçenekleri. Örnek: `"$select=Name,Description&$top=5"`.<br/><br/>**Note**: OData BAĞLAYıCıSı Birleşik URL 'den verileri kopyalar: `[URL specified in linked service]/[path specified in dataset]?[query specified in copy activity source]`. Daha fazla bilgi için bkz. [OData URL bileşenleri](https://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Hayır |

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

Türü belirlenmiş `RelationalSource` kaynak kullanıyorsanız, hala olduğu gibi desteklenirken, ileri ' yi kullanmanız önerilir.

## <a name="data-type-mapping-for-odata"></a>OData için veri türü eşlemesi

OData 'ten veri kopyaladığınızda, OData veri türleri ve Azure Data Factory geçici veri türleri arasında aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| OData veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| EDM.Binary | Byte[] |
| Edm.Boolean | Bool |
| EDM.Byte | Byte[] |
| Edm.DateTime | Tarih Saat |
| EDM.Decimal | Decimal |
| Edm.Double | Double |
| EDM.Single | Tek |
| EDM.Guid | Guid |
| EDM.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Dize |
| EDM.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> OData karmaşık veri türleri (örneğin, **nesne**) desteklenmez.


## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md##supported-data-stores-and-formats).