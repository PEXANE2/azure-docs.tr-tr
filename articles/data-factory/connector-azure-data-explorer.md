---
title: Azure Data Factory kullanarak Azure Veri Gezgini veri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak verileri Azure Veri Gezgini kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: orspodek
ms.openlocfilehash: a7ac0bdc2bd5eed802f6959a628dee4c8141dbd1
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68720795"
---
# <a name="copy-data-to-or-from-azure-data-explorer-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Veri Gezgini veri kopyalama

Bu makalede, [Azure Veri Gezgini](../data-explorer/data-explorer-overview.md)veri kopyalamak Için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Desteklenen herhangi bir kaynak veri deposundan Azure Veri Gezgini veri kopyalayabilirsiniz. Ayrıca, Azure Veri Gezgini verileri desteklenen herhangi bir havuz veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md) tablo.

>[!NOTE]
>Şirket içinde barındırılan Integration Runtime kullanarak Azure Veri Gezgini 'den/'tan şirket içi veri deposuna veri kopyalama 3,14 sürümünden bu yana desteklenir.

Azure Veri Gezgini Bağlayıcısı aşağıdakileri yapmanıza olanak sağlar:

* Bir **hizmet sorumlusu**ile Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulamasını kullanarak verileri kopyalayın.
* Kaynak olarak, bir KQL (kusto) sorgusu kullanarak verileri alın.
* Havuz olarak, hedef tabloya veri ekleyin.

## <a name="getting-started"></a>Başlarken

>[!TIP]
>Azure Veri Gezgini Bağlayıcısı 'nı kullanma hakkında yönergeler için bkz. [Azure Data Factory kullanarak verileri azure Veri Gezgini kopyalama](../data-explorer/data-factory-load-data.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Azure Veri Gezgini Connector 'a özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Veri Gezgini Bağlayıcısı hizmet sorumlusu kimlik doğrulamasını kullanır. Hizmet sorumlusu almak ve izin vermek için aşağıdaki adımları izleyin:

1. Azure Active Directory (Azure AD) uygulama varlık kaydınızı [uygulamanızı Azure AD kiracısı ile kaydetmeniz](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Azure Veri Gezgini 'de hizmet sorumlusu uygun iznini verin. Roller ve izinler hakkında ayrıntılı bilgi ve izinleri yönetmeye yönelik izlenecek yol ile [Azure Veri Gezgini veritabanı Izinlerini yönetme](../data-explorer/manage-database-permissions.md) bölümüne bakın. Genel olarak, şunları yapmanız gerekir

    - **Kaynak olarak**, veritabanınıza en az **veritabanı Görüntüleyicisi** rolü verin.
    - **Havuz olarak**, veritabanınıza en az **veritabanı** alma rolü verin.

>[!NOTE]
>Yazmak için ADF Kullanıcı arabirimini kullanırken, oturum açma Kullanıcı hesabınız Azure Veri Gezgini kümelerini, veritabanlarını ve tabloları listelemek için kullanılır. Bu tür bir işlem için izniniz yoksa, bu adı el ile girin.

Azure Veri Gezgini bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | **Type** özelliği **AzureDataExplorer** olarak ayarlanmalıdır | Evet |
| endpoint | Biçiminde olacak şekilde `https://<clusterName>.<regionName>.kusto.windows.net`Azure Veri Gezgini kümesinin uç nokta URL 'si. | Evet |
| database | Veritabanının adı. | Evet |
| tenant | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Bu, [kusto bağlantı dizesinde](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)normalde "**yetkili kimliği**" olarak bildiğiniz şeydir. Azure portal, fareyi sağ üst köşesinde bulunan fareyle girerek alın. | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. Bu, normal olarak [kusto bağlantı dizesinde](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)"**AAD uygulama istemci kimliği**" olarak bildiğiniz şeydir. | Evet |
| servicePrincipalKey | Uygulama anahtarını belirtin. Bu, normal olarak [kusto bağlantı dizesinde](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)"**AAD uygulama anahtarı**" olarak bildiğiniz şeydir. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Azure Veri Gezgini veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Verileri Azure Veri Gezgini kopyalamak için, veri kümesinin Type özelliğini **AzureDataExplorerTable**olarak ayarlayın.

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | **Type** özelliği **AzureDataExplorerTable** olarak ayarlanmalıdır | Evet |
| table | Bağlı hizmetin başvurduğu tablonun adı. | Havuz için Evet; Kaynak için Hayır |

**DataSet özellikleri örneği**

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

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm Azure Veri Gezgini kaynak ve havuz tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-data-explorer-as-source"></a>Kaynak olarak Azure Veri Gezgini

Azure Veri Gezgini 'den veri kopyalamak için, kopyalama etkinliği kaynağındaki **Type** özelliğini **AzureDataExplorerSource**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Kopyalama etkinliği kaynağının **Type** özelliği şu şekilde ayarlanmalıdır: **AzureDataExplorerSource** | Evet |
| query | Bir [KQL biçiminde](/azure/kusto/query/)verilen salt okunurdur bir istek. Özel KQL sorgusunu başvuru olarak kullanın. | Evet |
| queryTimeout | Sorgu isteğinin zaman aşımına uğramadan önce beklenecek bekleme süresi. Varsayılan değer 10 dakikadır (00:10:00); izin verilen en büyük değer 1 saattir (01:00:00). | Hayır |
| Kesilmesi | Döndürülen sonuç kümesinin kesilme edilip edilmeyeceğini belirtir. Varsayılan olarak, sonuç 500.000 kayıt veya 64MB sonra kesilir. Etkinliğin uygun bir davranışı için kesme kesinlikle önerilir. |Hayır |

>[!NOTE]
>Azure Veri Gezgini kaynağı varsayılan olarak 500.000 kayıt veya 64 MB boyutunda bir boyut sınırına sahiptir. Tüm kayıtları kesme olmadan almak için sorgunuzun başlangıcında belirtebilirsiniz `set notruncation;` . Daha ayrıntılı bilgi için [sorgu sınırlarına](https://docs.microsoft.com/azure/kusto/concepts/querylimits) bakın.

**Örnek:**

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

Verileri Azure Veri Gezgini kopyalamak için kopyalama etkinliği havuzundan tür özelliğini **AzureDataExplorerSink**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Kopyalama etkinliği havuzunun **Type** özelliği şu şekilde ayarlanmalıdır: **AzureDataExplorerSink** | Evet |
| ınestionmappingname | Kusto tablosundaki önceden oluşturulmuş **[eşlemenin](/azure/kusto/management/mappings#csv-mapping)** adı. Sütunları kaynaktan Azure Veri Gezgini eşlemek için CSV/JSON/avro biçimleri de dahil olmak üzere **[tüm desteklenen kaynak depoları/formatları](copy-activity-overview.md#supported-data-stores-and-formats)** için geçerlidir. kopyalama etkinliği [sütun eşlemesini](copy-activity-schema-and-type-mapping.md) (örtülü olarak ada veya açıkça yapılandırılmış olarak) kullanabilirsiniz ve /veya Azure Veri Gezgini eşlemeleri. | Hayır |

**Örnek:**

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
                "ingestionMappingName": "<optional Azure Data Explorer mapping name>"
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

## <a name="next-steps"></a>Sonraki adımlar

* Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).

* [Azure Data Factory verilerini Azure Veri Gezgini 'A kopyalama](/azure/data-explorer/data-factory-load-data)hakkında daha fazla bilgi edinin.