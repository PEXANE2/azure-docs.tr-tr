---
title: SAP ECC'den veri kopyalama
description: Bir Azure Veri Fabrikası ardışık alanında kopyalama etkinliği kullanarak SAP ECC'deki verileri desteklenen lavabo veri depolarına nasıl kopyalaylaydestekleyeceğinizi öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/02/2019
ms.openlocfilehash: ad26fca94527864af10bb0051336c372ea65b3e0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81413791"
---
# <a name="copy-data-from-sap-ecc-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak SAP ECC'deki verileri kopyalama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, SAP Enterprise Central Component (ECC) verilerini kopyalamak için Azure Veri Fabrikası'ndaki kopyalama etkinliğinin nasıl kullanılacağı açıklanmaktadır. Daha fazla bilgi için [bkz.](copy-activity-overview.md)

>[!TIP]
>Sap veri tümleştirme senaryosunda ADF'nin genel desteğini öğrenmek için, ayrıntılı giriş, karşılaştırma ve kılavuzlu [Azure Veri Fabrikası teknik incelemesini kullanarak SAP veri tümleştirmesine](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) bakın.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu SAP ECC bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

SAP ECC'deki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu SAP ECC bağlayıcısı destekler:

- SAP NetWeaver sürüm 7.0 ve sonraki sürümde SAP ECC'den gelen verileri kopyalama.
- SAP ECC OData hizmetleri tarafından maruz kalan nesnelerden veri kopyalama:

  - SAP tabloları veya görünümleri.
  - İş Uygulama Programlama Arabirimi [BAPI] nesneleri.
  - Veri çıkarıcılar.
  - Sap İşlem Entegrasyonu'na (PI) gönderilen ve göreli bağdaştırıcılar aracılığıyla OData olarak alınabilen veri veya ara belgeler (IDOCs).

- Temel kimlik doğrulamasını kullanarak verileri kopyalama.

>[!TIP]
>SAP ECC'deki verileri sap tablosu veya görünümü üzerinden kopyalamak için, daha hızlı ve daha ölçeklenebilir SAP [tablo](connector-sap-table.md) bağlayıcısını kullanın.

## <a name="prerequisites"></a>Ön koşullar

Genellikle SAP ECC, SAP Ağ Geçidi aracılığıyla OData hizmetleri aracılığıyla varlıkları ortaya çıkarır. Bu SAP ECC konektörünü kullanmak için şunları yapmanız gerekir:

- **SAP Ağ Geçidi'ni ayarlayın.** SAP NetWeaver sürümleri 7,4'ten sonra olan sunucular için SAP Ağ Geçidi zaten yüklenmiş tir. Önceki sürümler için, SAP ECC verilerini OData hizmetleri aracılığıyla açığa çıkarmadan önce gömülü SAP Ağ Geçidi'ni veya SAP Ağ Geçidi hub sistemini yüklemeniz gerekir. SAP Ağ Geçidi'ni ayarlamak için [yükleme kılavuzuna](https://help.sap.com/saphelp_gateway20sp12/helpdata/en/c3/424a2657aa4cf58df949578a56ba80/frameset.htm)bakın.

- **SAP OData hizmetini etkinleştirin ve yapılandırın.** OData hizmetini TCODE SICF üzerinden saniyeler içinde etkinleştirebilirsiniz. Ayrıca, hangi nesnelerin açığa alınması gerektiğini de yapılandırabilirsiniz. Daha fazla bilgi için [adım adım kılavuza](https://blogs.sap.com/2012/10/26/step-by-step-guide-to-build-an-odata-service-based-on-rfcs-part-1/)bakın.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, SAP ECC bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

SAP ECC bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `type` | `SapEcc`Özellik' `type` in . | Evet |
| `url` | SAP ECC OData hizmetinin URL'si. | Evet |
| `username` | SAP ECC'ye bağlanmak için kullanılan kullanıcı adı. | Hayır |
| `password` | SAP ECC'ye bağlanmak için kullanılan düz metin parolası. | Hayır |
| `connectVia` | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Bir çalışma zamanı belirtmezseniz, varsayılan Azure tümleştirme çalışma zamanı kullanılır. | Hayır |

### <a name="example"></a>Örnek

```json
{
    "name": "SapECCLinkedService",
    "properties": {
        "type": "SapEcc",
        "typeProperties": {
            "url": "<SAP ECC OData URL, e.g., http://eccsvrname:8000/sap/opu/odata/sap/zgw100_dd02l_so_srv/>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        }
    },
    "connectVia": {
        "referenceName": "<name of integration runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-datasets-linked-services.md) Aşağıdaki bölümde SAP ECC veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

SAP ECC'deki verileri kopyalamak için `type` `SapEccResource`veri kümesinin özelliğini .

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `path` | SAP ECC OData varlığının yolu. | Evet |

### <a name="example"></a>Örnek

```json
{
    "name": "SapEccDataset",
    "properties": {
        "type": "SapEccResource",
        "typeProperties": {
            "path": "<entity path, e.g., dd04tentitySet>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP ECC linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md) Aşağıdaki bölümde SAP ECC kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-ecc-as-a-source"></a>Kaynak olarak SAP ECC

SAP ECC'deki verileri kopyalamak için, `type` `source` `SapEccSource`kopyalama etkinliği bölümündeki özelliği ' ye ayarlar.

Aşağıdaki özellikler kopyalama etkinliğinin `source` bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `type` | Kopyalama `type` etkinliği `source` bölümünün özelliği `SapEccSource`' ye göre ayarlanmalıdır. | Evet |
| `query` | Verileri filtrelemek için OData sorgu seçenekleri. Örneğin:<br/><br/>`"$select=Name,Description&$top=10"`<br/><br/>SAP ECC bağlayıcısı verileri birleştirilmiş URL'den kopyalar:<br/><br/>`<URL specified in the linked service>/<path specified in the dataset>?<query specified in the copy activity's source section>`<br/><br/>Daha fazla bilgi için Bkz. [OData URL bileşenleri.](https://www.odata.org/documentation/odata-version-3-0/url-conventions/) | Hayır |

### <a name="example"></a>Örnek

```json
"activities":[
    {
        "name": "CopyFromSAPECC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP ECC input dataset name>",
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
                "type": "SapEccSource",
                "query": "$top=10"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mappings-for-sap-ecc"></a>SAP ECC için veri türü eşlemeleri

SAP ECC'den veri kopyalarken, SAP ECC verileri için OData veri türlerinden Azure Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemasını ve veri türünü lavaboyla nasıl eşlenebildiğini öğrenmek için Bkz. [Şema ve veri türü eşlemeleri.](copy-activity-schema-and-type-mapping.md)

| OData veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| `Edm.Binary` | `String` |
| `Edm.Boolean` | `Bool` |
| `Edm.Byte` | `String` |
| `Edm.DateTime` | `DateTime` |
| `Edm.Decimal` | `Decimal` |
| `Edm.Double` | `Double` |
| `Edm.Single` | `Single` |
| `Edm.Guid` | `String` |
| `Edm.Int16` | `Int16` |
| `Edm.Int32` | `Int32` |
| `Edm.Int64` | `Int64` |
| `Edm.SByte` | `Int16` |
| `Edm.String` | `String` |
| `Edm.Time` | `TimeSpan` |
| `Edm.DateTimeOffset` | `DateTimeOffset` |

> [!NOTE]
> Karmaşık veri türleri şu anda desteklenmez.

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için desteklenen [veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
