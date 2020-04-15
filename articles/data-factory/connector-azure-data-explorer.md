---
title: Azure Veri Gezgini'ne veya Azure Veri Gezgini'nden veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak verileri Azure Veri Gezgini'ne veya Azure Veri Gezgini'nden nasıl kopyalayabilirsiniz öğrenin.
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
ms.openlocfilehash: 5f0eada5184f2c8e36b0deb5a9d27a8a9230776d
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382756"
---
# <a name="copy-data-to-or-from-azure-data-explorer-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Azure Veri Gezgini'ne veya Azure Veri Gezgini'nden kopyalama

Bu makalede, Azure Veri Gezgini'ne veya [Azure Veri Gezgini'nden](/azure/data-explorer/data-explorer-overview)veri kopyalamak için Azure Veri Fabrikası'ndaki kopyalama etkinliğinin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğine genel bir bakış sunan [kopya etkinliğine genel bakış](copy-activity-overview.md) makalesiüzerine kuruludur.

>[!TIP]
>Azure Veri Fabrikası ve Genel Olarak Azure Veri Gezgini tümleştirmesi için Azure [Veri Gezgini'ni Azure Veri Fabrikası ile tümleştir'den](/azure/data-explorer/data-factory-integration)daha fazla bilgi edinin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Veri Gezgini bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Desteklenen herhangi bir kaynak veri deposundan verileri Azure Veri Gezgini'ne kopyalayabilirsiniz. Verileri Azure Veri Gezgini'nden desteklenen herhangi bir lavabo veri deposuna da kopyalayabilirsiniz. Kopyalama etkinliğinin kaynak veya lavabo olarak desteklediği veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

>[!NOTE]
>Kendi kendine barındırılan tümleştirme çalışma zamanı kullanılarak şirket içi bir veri deposu aracılığıyla Azure Veri Gezgini'ne veya Azure Veri Gezgini'nden veri kopyalama, sürüm 3.14 ve sonraki sürümlerde desteklenir.

Azure Veri Gezgini bağlayıcısı ile aşağıdakileri yapabilirsiniz:

* Azure Active Directory (Azure AD) uygulama belirteç kimlik doğrulamasını kullanarak bir **hizmet yöneticisiyle**verileri kopyalayın.
* Kaynak olarak, bir KQL (Kusto) sorgusu kullanarak veri alın.
* Lavabo olarak, verileri hedef tabloya ekin.

## <a name="getting-started"></a>Başlarken

>[!TIP]
>Azure Veri Gezgini bağlayıcısının bir bölümü için Azure [Veri Fabrikası'nı kullanarak Azure Veri Gezgini'ne verileri](/azure/data-explorer/data-factory-load-data) kopyala'ya ve [veritabanından Azure Veri Gezgini'ne toplu kopya bölümüne](/azure/data-explorer/data-factory-template)bakın.

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Azure Veri Gezgini bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Azure Veri Gezgini bağlayıcısı hizmet temel kimlik doğrulamasını kullanır. Bir hizmet sorumlusu almak ve izin vermek için aşağıdaki adımları izleyin:

1. [Uygulamanızı Azure AD kiracısıyla kaydettirme](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)adımlarını izleyerek bir uygulama kuruluşunu Azure Etkin Dizini'ne kaydedin. Bağlantılı hizmeti tanımlamak için kullandığınız aşağıdaki değerlere dikkat edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Hizmet sorumlusuna Azure Veri Gezgini'nde doğru izinleri verin. Bkz. Roller ve izinler ve izinleri yönetme hakkında ayrıntılı bilgi için [Azure Veri Gezgini veritabanı izinlerini yönetin.](/azure/data-explorer/manage-database-permissions) Genel olarak şunları belirtmelisiniz:

    - **Kaynak olarak,** veritabanınıza en azından **Veritabanı görüntüleyici** rolünü verir
    - **Lavabo olarak,** veritabanınıza en azından **Veritabanı alma** rolü vermek

>[!NOTE]
>Yazar olarak Veri Fabrikası Kullanıcı UI'sini kullandığınızda, oturum açma kullanıcı hesabınız Azure Veri Gezgini kümelerini, veritabanlarını ve tabloları listelemek için kullanılır. Bu işlemler için izniniz yoksa, adı el ile girin.

Azure Veri Gezgini bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Tür** özelliği **AzureDataExplorer**olarak ayarlanmalıdır. | Evet |
| endpoint | Azure Veri Gezgini kümesinin bitiş noktası `https://<clusterName>.<regionName>.kusto.windows.net`URL'si, biçimi . | Evet |
| database | Veritabanının adı. | Evet |
| Kiracı | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Bu, [Kusto bağlantı dizesinde](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)"Yetki Kimliği" olarak bilinir. Azure portalının sağ üst köşesindeki fare işaretçisini gezdirerek alın. | Evet |
| hizmetPrincipalId | Uygulamanın istemci kimliğini belirtin. Bu, [Kusto bağlantı dizesinde](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)"AAD uygulama istemci kimliği" olarak bilinir. | Evet |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu, [Kusto bağlantı dizesinde](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)"AAD uygulama anahtarı" olarak bilinir. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** olarak işaretleyin veya [Azure Key Vault'ta depolanan güvenli verilere başvurun.](store-credentials-in-key-vault.md) | Evet |

**Bağlantılı hizmet özellikleri örneği:**

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Azure Veri Fabrikası'ndaki Veri Kümeleri'ne](concepts-datasets-linked-services.md)bakın. Bu bölümde, Azure Veri Gezgini veri kümesinin desteklediği özellikler listeleneb.)

Verileri Azure Veri Gezgini'ne kopyalamak için, veri kümesinin tür özelliğini **AzureDataExplorerTable**olarak ayarlayın.

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Tür** özelliği **AzureDataExplorerTable**olarak ayarlanmalıdır. | Evet |
| tablo | Bağlı hizmetin başvurulması gereken tablonun adı. | Lavabo için evet; Kaynak için hayır |

**Dataset özellikleri örneği:**

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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için Azure [Veri Fabrikası'ndaki Boru Hatları ve etkinliklerine](concepts-pipelines-activities.md)bakın. Bu bölümde, Azure Veri Gezgini kaynaklarının ve lavabolarının desteklediği özelliklerin bir listesi sağlanmaktadır.

### <a name="azure-data-explorer-as-source"></a>Kaynak olarak Azure Veri Gezgini

Azure Veri Gezgini'nden veri kopyalamak için, Kopyalama etkinlik kaynağındaki **tür** özelliğini **AzureDataExplorerSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının **tür** özelliği şu şekilde ayarlanmalıdır: **AzureDataExplorerSource** | Evet |
| sorgu | [KQL formatında](/azure/kusto/query/)verilen salt okunur istek. Başvuru olarak özel KQL sorgusunu kullanın. | Evet |
| Querytimeout | Sorgu isteğinin zaman ları dolmadan önceki bekleme süresi. Varsayılan değer 10 dkdır (00:10:00); izin verilen maksimum değer 1 saattir (01:00:00). | Hayır |
| noTruncation | Döndürülen sonuç kümesini druncate olup olmadığını gösterir. Varsayılan olarak, sonuç 500.000 kayıt veya 64 megabayt (MB) sonra kesilir. Etkinliğin doğru davranışını sağlamak için kesilme şiddetle önerilir. |Hayır |

>[!NOTE]
>Varsayılan olarak, Azure Veri Gezgini kaynağının 500.000 kayıt veya 64 MB'lık bir boyut sınırı vardır. Kesilmeden tüm kayıtları almak için, sorgunuzun başında belirtebilirsiniz. `set notruncation;` Daha fazla bilgi için [Sorgu sınırlarına](https://docs.microsoft.com/azure/kusto/concepts/querylimits)bakın.

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

### <a name="azure-data-explorer-as-sink"></a>Lavabo olarak Azure Veri Gezgini

Verileri Azure Veri Gezgini'ne kopyalamak için, kopyalama etkinliğindeki tür özelliğini **AzureDataExplorerSink**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **lavabo** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabonun **türü** özelliği şu şekilde ayarlanmalıdır: **AzureDataExplorerSink**. | Evet |
| ingestionMappingName | Kusto masasında önceden oluşturulmuş bir [eşlemenin](/azure/kusto/management/mappings#csv-mapping) adı. Sütunları kaynaktan Azure Veri Gezgini'ne (CSV/JSON/Avro biçimleri de dahil olmak üzere [desteklenen tüm kaynak depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats)için geçerlidir) eşlemek için, kopyalama etkinliği [sütuneşlemesi](copy-activity-schema-and-type-mapping.md) (örtülü olarak veya açıkça yapılandırılmış olarak) ve/veya Azure Veri Gezgini eşlemelerini kullanabilirsiniz. | Hayır |
| ek Özellikler | Azure Veri Gezgini Büzerek tarafından zaten ayarlanamamakta olan yutma özelliklerinden herhangi birini belirtmek için kullanılabilen bir özellik çantası. Özellikle, yutma etiketleri belirtmek için yararlı olabilir. [Azure Veri Satmasını Vereme dokümanından](https://kusto.azurewebsites.net/docs/management/data-ingestion/index.html)daha fazla bilgi edinin. | Hayır |

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

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi için [Bkz. Arama etkinliği.](control-flow-lookup-activity.md)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Veri Fabrikası'ndaki kopyalama etkinliğinin kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.

* [Azure Veri Fabrikası'ndan Azure Veri Gezgini'ne kadar verileri](/azure/data-explorer/data-factory-load-data)kopyalama hakkında daha fazla bilgi edinin.
