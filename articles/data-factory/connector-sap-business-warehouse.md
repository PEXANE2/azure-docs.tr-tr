---
title: SAP BW'den veri kopyalama
description: Bir Azure Veri Fabrikası ardışık alanında kopyalama etkinliği kullanarak SAP İş Ambarı'ndan desteklenen lavabo veri depolarına verileri nasıl kopyalayatılayış edin.
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
ms.date: 09/04/2019
ms.openlocfilehash: 2f8406038be10ba3bdc207bf447fecb86a376fe8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418074"
---
# <a name="copy-data-from-sap-business-warehouse-using-azure-data-factory"></a>Azure Veri Fabrikasını kullanarak SAP İş Ambarından veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-sap-business-warehouse-connector.md)
> * [Geçerli sürüm](connector-sap-business-warehouse.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, bir SAP İş Ambarı'ndan (BW) veri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

>[!TIP]
>Sap veri tümleştirme senaryosunda ADF'nin genel desteğini öğrenmek için, ayrıntılı giriş, karşılaştırma ve kılavuzlu [Azure Veri Fabrikası teknik incelemesini kullanarak SAP veri tümleştirmesine](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) bakın.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu SAP İş Ambarı bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

SAP İş Ambarı'ndaki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu SAP İş Ambarı bağlayıcısı destekler:

- SAP İş Ambarı **sürüm 7.x**.
- MDX sorgularını kullanarak **InfoCubes ve QueryCubes(BEx** sorguları dahil) verilerinin kopyalanması.
- Temel kimlik doğrulamasını kullanarak verileri kopyalama.

## <a name="prerequisites"></a>Ön koşullar

Bu SAP İş Ambarı konektörünü kullanmak için şunları yapmanız gerekir:

- Kendi kendine barındırılan Tümleştirme Çalışma Zamanı'nı ayarlayın. Ayrıntılar için [Kendi barındırılan Tümleştirme Çalışma Zamanı](create-self-hosted-integration-runtime.md) makalesine bakın.
- SAP **NetWeaver kitaplığını** Tümleştirme Runtime makinesine yükleyin. SAP Netweaver kitaplığını SAP yöneticinizden veya doğrudan [SAP Yazılım İndirme Merkezi'nden](https://support.sap.com/swdc)alabilirsiniz. En son sürümün indirme konumunu almak için **SAP Not #1025361'ni** arayın. Tümleştirme Çalışma Zamanı yüklemenizle eşleşen **64 bit** SAP NetWeaver kitaplığını seçtiğinizden emin olun. Ardından SAP Not'a göre SAP NetWeaver RFC SDK'da bulunan tüm dosyaları yükleyin. SAP NetWeaver kitaplığı, SAP İstemci Araçları yüklemesinde de yer alır.

>[!TIP]
>SAP BW'ye bağlantı sorununu gidermek için şunları emin olun:
>- NetWeaver RFC SDK'dan çıkarılan tüm bağımlılık kitaplıkları %windir%\system32 klasöründe yer almaktadır. Genellikle icudt34.dll, icuin34.dll, icuuc34.dll, libicudecnumber.dll, librfc32.dll, libsapucu.dll, sapcrypto.dll, sapcryto_old.dll, sapnwrfc.dll vardır.
>- SAP Server'a bağlanmak için kullanılan gerekli bağlantı noktaları, genellikle 3300 ve 3201 bağlantı noktası olan Self barındırılan IR makinesinde etkinleştirilir.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, SAP İş Ambarı bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Sap İş Ambarı (BW) bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **SapBw** | Evet |
| sunucu | SAP BW örneğinin bulunduğu sunucunun adı. | Evet |
| systemNumber | SAP BW sisteminin sistem numarası.<br/>İzin verilen değer: dize olarak temsil edilen iki basamaklı ondalık sayı. | Evet |
| clientId | SAP W sistemindeki istemci kimliği.<br/>İzin verilen değer: dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet |
| userName | SAP sunucusuna erişimi olan kullanıcının adı. | Evet |
| password | Kullanıcının parolası. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullarda](#prerequisites)belirtildiği gibi Kendi kendine barındırılan Tümleştirme Çalışma Süresi gereklidir. |Evet |

**Örnek:**

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde SAP BW veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

SAP BW'deki verileri kopyalamak için, veri kümesinin tür özelliğini **SapBwCube**olarak ayarlayın. Tür İlişkisel Tablo SAP BW veri kümesi için desteklenen türe özgü özellikleri olsa da.

**Örnek:**

```json
{
    "name": "SAPBWDataset",
    "properties": {
        "type": "SapBwCube",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP BW linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Dakti-zimd veri kümesi kullanıyorsanız, `RelationalTable` ileriye dönük yenisini kullanmanız önerilirken, yine de olduğu gibi desteklenir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde SAP BW kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-bw-as-source"></a>KAYNAK OLARAK SAP BW

SAP BW'deki verileri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği şu şekilde ayarlanmalıdır: **SapBwSource** | Evet |
| sorgu | SAP BW örneğindeki verileri okumak için MDX sorgusunu belirtir. | Evet |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPBW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP BW input dataset name>",
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
                "type": "SapBwSource",
                "query": "<MDX query for SAP BW>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Dakti-yazılı `RelationalSource` kaynak kullanıyorsanız, ileriye dönük yeni bir kaynak kullanmanız önerilirken, yine de olduğu gibi desteklenir.

## <a name="data-type-mapping-for-sap-bw"></a>SAP BW için veri türü eşleme

SAP BW'den veri kopyalanırken, SAP BW veri türlerinden Azure Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopya etkinliği kaynak şemasını ve veri türünü lavaboyla nasıl eşler hakkında bilgi edinmek için Şema ve [veri türü eşlemelerine](copy-activity-schema-and-type-mapping.md) bakın.

| SAP BW veri türü | Veri fabrikası geçici veri türü |
|:--- |:--- |
| ACCP | int |
| Char | Dize |
| CLNT | Dize |
| CURR | Ondalık |
| CUKY | Dize |
| Aralık | Ondalık |
| FLTP | Çift |
| INT1 | Bayt |
| INT2 | Int16 |
| INT4 | int |
| Lang | Dize |
| LCHR | Dize |
| LRAW | Bayt[] |
| PREC | Int16 |
| Quan | Ondalık |
| Ham | Bayt[] |
| RAWSTRING | Bayt[] |
| Dize | Dize |
| Birim | Dize |
| DATS | Dize |
| NUMC | Dize |
| TİmS | Dize |


## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
