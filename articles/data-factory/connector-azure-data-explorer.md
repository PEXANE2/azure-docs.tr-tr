---
title: Azure Veri Gezgini veri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak verileri Azure Veri Gezgini kopyalama hakkında bilgi edinin.
services: data-factory
ms.author: orspodek
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/18/2020
ms.openlocfilehash: 12bf807f5866567508b644105f377cfde1494250
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81410771"
---
# <a name="copy-data-to-or-from-azure-data-explorer-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Veri Gezgini veri kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, [Azure Veri Gezgini](/azure/data-explorer/data-explorer-overview)veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı açıklanır. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

>[!TIP]
>Azure Data Factory ve Azure Veri Gezgini tümleştirmesi için, [azure Azure Data Factory Veri Gezgini tümleştirme](/azure/data-explorer/data-factory-integration)hakkında daha fazla bilgi edinin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Veri Gezgini Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Desteklenen herhangi bir kaynak veri deposundan Azure Veri Gezgini veri kopyalayabilirsiniz. Ayrıca, Azure Veri Gezgini verileri desteklenen herhangi bir havuz veri deposuna kopyalayabilirsiniz. Kopyalama etkinliğinin kaynak veya havuz olarak desteklediği veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

>[!NOTE]
>Şirket içinde barındırılan tümleştirme çalışma zamanını kullanarak şirket içi veri deposu aracılığıyla veya Azure Veri Gezgini 'den veri kopyalama, sürüm 3,14 ve sonrasında desteklenir.

Azure Veri Gezgini Bağlayıcısı ile şunları yapabilirsiniz:

* Bir **hizmet sorumlusu**ile Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulamasını kullanarak verileri kopyalayın.
* Kaynak olarak, bir KQL (kusto) sorgusu kullanarak verileri alın.
* Havuz olarak, hedef tabloya veri ekleyin.

## <a name="getting-started"></a>Başlarken

>[!TIP]
>Azure Veri Gezgini Bağlayıcısı hakkında yönergeler için bkz. [bir veritabanından azure Veri Gezgini Azure Data Factory ve toplu kopyalama](/azure/data-explorer/data-factory-template) [kullanarak Azure Veri Gezgini verileri kopyalama](/azure/data-explorer/data-factory-load-data)

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Azure Veri Gezgini Connector 'a özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Azure Veri Gezgini Bağlayıcısı hizmet sorumlusu kimlik doğrulamasını kullanır. Hizmet sorumlusu almak ve izin vermek için aşağıdaki adımları izleyin:

1. Uygulamanızı [bir Azure AD kiracısıyla kaydetme](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)bölümündeki adımları izleyerek Azure Active Directory bir uygulama varlığını kaydedin. Bağlı hizmeti tanımlamak için kullandığınız aşağıdaki değerleri unutmayın:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Hizmet sorumlusuna Azure Veri Gezgini 'de doğru izinleri verin. Roller ve izinler ve izinleri yönetme hakkında ayrıntılı bilgi için bkz. [Azure Veri Gezgini veritabanını yönetme izinleri](/azure/data-explorer/manage-database-permissions) . Genel olarak şunları yapmanız gerekir:

    - **Kaynak olarak**, veritabanınıza en azından **veritabanı Görüntüleyicisi** rolü verin
    - **Havuz olarak**, veritabanınıza en azından **veritabanı** alma rolünü verin

>[!NOTE]
>Yazmak için Data Factory Kullanıcı arabirimini kullandığınızda, oturum açma Kullanıcı hesabınız Azure Veri Gezgini kümelerini, veritabanlarını ve tablolarını listelemek için kullanılır. Bu işlemler için izniniz yoksa el ile adı girin.

Azure Veri Gezgini bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Type** özelliği **AzureDataExplorer**olarak ayarlanmalıdır. | Yes |
| endpoint | Biçiminde olacak şekilde `https://<clusterName>.<regionName>.kusto.windows.net`Azure Veri Gezgini kümesinin uç nokta URL 'si. | Yes |
| database | Veritabanının adı. | Yes |
| Kiracı | Uygulamanızın altında bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı KIMLIĞI) belirtin. Bu, [kusto bağlantı dizesinde](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)"Authority ID" olarak bilinir. Azure portal, fare işaretçisini, sağ üst köşesine getirerek alın. | Yes |
| Serviceprincipalıd | Uygulamanın istemci KIMLIĞINI belirtin. Bu, [kusto bağlantı dizesinde](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)"AAD uygulama istemci kimliği" olarak bilinir. | Yes |
| Servicesprincipalkey | Uygulamanın anahtarını belirtin. Bu, [kusto bağlantı dizesinde](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)"AAD uygulama anahtarı" olarak bilinir. Data Factory güvenli bir şekilde depolamak için bu alanı **SecureString** olarak işaretleyin veya [Azure Key Vault depolanan güvenli verilere başvurun](store-credentials-in-key-vault.md). | Yes |

**Bağlı hizmet özellikleri örneği:**

```json
{
    "name": "AzureDataExplorerLinkedService",
    "properties": {
        "type": "AzureDataExplorer",
        "typeProperties": {
            "endpoint": "https://<clusterName>.<regionName>.kusto.windows.net ",
            "database": "<database name>",
            "tenant": "<tenant name/id e.g. microsoft.onmicrosoft.com>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [Azure Data Factory veri kümeleri](concepts-datasets-linked-services.md). Bu bölümde, Azure Veri Gezgini veri kümesinin desteklediği özellikler listelenmiştir.

Verileri Azure Veri Gezgini kopyalamak için, veri kümesinin Type özelliğini **AzureDataExplorerTable**olarak ayarlayın.

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Type** özelliği **AzureDataExplorerTable**olarak ayarlanmalıdır. | Yes |
| tablo | Bağlı hizmetin başvurduğu tablonun adı. | Havuz için Evet; Kaynak için Hayır |

**Veri kümesi özellikleri örneği:**

```json
{
   "name": "AzureDataExplorerDataset",
    "properties": {
        "type": "AzureDataExplorerTable",
        "typeProperties": {
            "table": "<table name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Azure Data Explorer linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için, bkz. Azure Data Factory işlem [hatları ve etkinlikleri](concepts-pipelines-activities.md). Bu bölümde Azure Veri Gezgini kaynakları ve havuzları desteğinin özelliklerinin bir listesi verilmiştir.

### <a name="azure-data-explorer-as-source"></a>Kaynak olarak Azure Veri Gezgini

Azure Veri Gezgini 'den veri kopyalamak için, kopyalama etkinliği kaynağındaki **Type** özelliğini **AzureDataExplorerSource**olarak ayarlayın. Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının **Type** özelliği: **AzureDataExplorerSource** olarak ayarlanmalıdır | Yes |
| sorgu | Bir [KQL biçiminde](/azure/kusto/query/)verilen salt okunurdur bir istek. Özel KQL sorgusunu başvuru olarak kullanın. | Yes |
| queryTimeout | Sorgu isteğinin zaman aşımına uğramadan önce beklenecek bekleme süresi. Varsayılan değer 10 dakikadır (00:10:00); izin verilen en büyük değer 1 saattir (01:00:00). | Hayır |
| Kesilmesi | Döndürülen sonuç kümesinin kesilme edilip edilmeyeceğini belirtir. Varsayılan olarak, sonuç 500.000 kayıt veya 64 megabayt (MB) sonra kesilir. Etkinliğin doğru davranış sağlamak için kesme kesinlikle önerilir. |Hayır |

>[!NOTE]
>Varsayılan olarak, Azure Veri Gezgini kaynağı 500.000 kayıt veya 64 MB boyutunda bir boyut sınırına sahiptir. Tüm kayıtları kesme olmadan almak için sorgunuzun başlangıcında belirtebilirsiniz `set notruncation;` . Daha fazla bilgi için bkz. [sorgu sınırları](https://docs.microsoft.com/azure/kusto/concepts/querylimits).

**Örneğinde**

```json
"activities":[
    {
        "name": "CopyFromAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataExplorerSource",
                "query": "TestTable1 | take 10",
                "queryTimeout": "00:10:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Explorer input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

### <a name="azure-data-explorer-as-sink"></a>Havuz olarak Azure Veri Gezgini

Verileri Azure Veri Gezgini kopyalamak için kopyalama etkinliği havuzundan tür özelliğini **AzureDataExplorerSink**olarak ayarlayın. Aşağıdaki özellikler, kopyalama etkinliği **havuzu** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun **Type** özelliği: **AzureDataExplorerSink**olarak ayarlanmalıdır. | Yes |
| ınestionmappingname | Kusto tablosundaki önceden oluşturulmuş [eşlemenin](/azure/kusto/management/mappings#csv-mapping) adı. Sütunları kaynaktan Azure Veri Gezgini eşlemek için (CSV/JSON/avro biçimleri dahil olmak üzere [tüm desteklenen kaynak depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats)için geçerlidir), kopyalama etkinliği [sütun eşlemesini](copy-activity-schema-and-type-mapping.md) (örtük olarak ada veya açıkça yapılandırılmış olarak) ve/veya Azure Veri Gezgini eşlemelerini kullanabilirsiniz. | Hayır |
| additionalProperties | Azure Veri Gezgini havuzu tarafından henüz ayarlanmayan alma özelliklerinden herhangi birini belirtmek için kullanılabilecek bir özellik paketi. Özel olarak, Alım etiketlerini belirtmek yararlı olabilir. Azure verilerinden daha fazla bilgi edinin [verileri araştırma belgesi](https://kusto.azurewebsites.net/docs/management/data-ingestion/index.html). | Hayır |

**Örneğinde**

```json
"activities":[
    {
        "name": "CopyToAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataExplorerSink",
                "ingestionMappingName": "<optional Azure Data Explorer mapping name>",
                "additionalProperties": {<additional settings for data ingestion>}
            }
        },
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Explorer output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özellikler hakkında daha fazla bilgi için bkz. [arama etkinliği](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Sonraki adımlar

* Azure Data Factory içindeki kopyalama etkinliğinin kaynak ve havuz olarak desteklediği veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).

* [Azure Data Factory verileri Azure Veri Gezgini kopyalama](/azure/data-explorer/data-factory-load-data)hakkında daha fazla bilgi edinin.
