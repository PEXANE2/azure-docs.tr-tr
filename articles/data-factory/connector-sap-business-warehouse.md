---
title: Azure Data Factory kullanarak SAP BW verileri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak SAP Business Warehouse 'tan desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/04/2019
ms.openlocfilehash: 26693219f44d16c8bc20ee94ae0590414ba88f73
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896342"
---
# <a name="copy-data-from-sap-business-warehouse-using-azure-data-factory"></a>Azure Data Factory kullanarak SAP Business Warehouse 'tan veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-sap-business-warehouse-connector.md)
> * [Geçerli sürüm](connector-sap-business-warehouse.md)

Bu makalede, bir SAP Business Warehouse 'tan (siyah-beyaz) veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

>[!TIP]
>ADF 'nin SAP veri tümleştirme senaryosunda genel desteğini öğrenmek için ayrıntılı giriş, comparme ve kılavuzla [Azure Data Factory Teknik İnceleme kullanarak SAP veri tümleştirme](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) konusuna bakın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu SAP Business Warehouse Connector, aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

SAP Business Warehouse 'tan verileri desteklenen herhangi bir havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu SAP Business Warehouse Connector şunları destekler:

- SAP Business Warehouse **sürüm 7. x**.
- MDX sorgularını kullanarak **InfoCubes ve Queryküplerinden** (Bex sorguları dahil) verileri kopyalama.
- Temel kimlik doğrulaması kullanarak verileri kopyalama.

## <a name="prerequisites"></a>Önkoşullar

Bu SAP Business Warehouse bağlayıcısını kullanmak için şunları yapmanız gerekir:

- Şirket içinde barındırılan bir Integration Runtime ayarlayın. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.
- Integration Runtime makinesine **SAP NetWeaver kitaplığını** yüklersiniz. SAP NetWeaver kitaplığını SAP yöneticinizden alabilir veya doğrudan [SAP Software Download Center](https://support.sap.com/swdc)' dan edinebilirsiniz. En son sürüme ait indirme konumunu almak için **SAP Note #1025361** aratın. Integration Runtime yüklemenize uyan **64 bitlik** SAP NetWeaver kitaplığını seçtiğinizden emin olun. Daha sonra SAP NetWeaver RFC SDK 'sına dahil edilen tüm dosyaları SAP notuna göre yükler. SAP NetWeaver kitaplığı, SAP Istemci araçları yüklemesine de dahildir.

>[!TIP]
>SAP BW bağlantı sorununu gidermek için, şunları yaptığınızdan emin olun:
>- NetWeaver RFC SDK 'dan ayıklanan tüm bağımlılık kitaplıkları%Windir%\System32 klasöründe bulunur. Genellikle icudt34. dll, icuin34. dll, icuuc34. dll, libıuıudecnumber. dll, librfc32. dll, libsapucum. dll, sapşifre. dll, sapcryto_old. dll, sapnwrfc. dll ' dir.
>- SAP sunucusuna bağlanmak için kullanılan bağlantı noktaları, genellikle bağlantı noktası 3300 ve 3201 olan şirket içinde barındırılan IR makinesinde etkinleştirilir.

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, SAP Business Warehouse Connector 'a özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

SAP Business Warehouse (bant genişliği) bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği: **Sapbeyaz** olarak ayarlanmalıdır | Yes |
| sunucu | SAP BW örneğinin bulunduğu sunucunun adı. | Yes |
| systemNumber | SAP BW sisteminin sistem numarası.<br/>İzin verilen değer: dize olarak temsil edilen iki basamaklı ondalık sayı. | Yes |
| clientID | SAP W sistemindeki istemcinin istemci KIMLIĞI.<br/>İzin verilen değer: dize olarak temsil edilen üç basamaklı ondalık sayı. | Yes |
| userName adı | SAP sunucusuna erişimi olan kullanıcının adı. | Yes |
| password | Kullanıcının parolası. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Yes |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşul](#prerequisites)bölümünde belirtildiği gibi, kendinden konak Integration Runtime gereklidir. |Yes |

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm SAP BW veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

SAP BW verileri kopyalamak için, veri kümesinin Type özelliğini **Sapbwcube**olarak ayarlayın. RelationalTable türünde SAP BW veri kümesi için desteklenen türe özgü özellik yok.

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

Türü belirlenmiş `RelationalTable` veri kümesini kullanıyorsanız, bunun olduğu gibi hala desteklenmektedir, ileri ' yi kullanmaya devam etmeniz önerilir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm SAP BW kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-bw-as-source"></a>Kaynak olarak SAP BW

SAP BW verileri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği: **Sapbwsource** olarak ayarlanmalıdır | Yes |
| sorgu | SAP BW örneğinden verileri okumak için MDX sorgusunu belirtir. | Yes |

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

Türü belirlenmiş `RelationalSource` kaynak kullanıyorsanız, hala olduğu gibi desteklenirken, ileri ' yi kullanmanız önerilir.

## <a name="data-type-mapping-for-sap-bw"></a>SAP BW için veri türü eşlemesi

SAP BW verileri kopyalarken, SAP BW veri türlerinden aşağıdaki eşlemeler Azure Data Factory geçici veri türlerini kullanır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| SAP BW veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| ACCP | Int |
| CHAR | Dize |
| CLNT | Dize |
| CURR | Decimal |
| CUKY | Dize |
| DEC | Decimal |
| FLTP | Double |
| INT1 | Bayt |
| INT2 | Int16 |
| INT4 | Int |
| LANG | Dize |
| LCHR | Dize |
| LRAW | Byte[] |
| PREC | Int16 |
| QUA | Decimal |
| RAW | Byte[] |
| RAWSTRING | Byte[] |
| STRING | Dize |
| BİRİM | Dize |
| KATALAR | Dize |
| NUMC | Dize |
| TIMS | Dize |


## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
