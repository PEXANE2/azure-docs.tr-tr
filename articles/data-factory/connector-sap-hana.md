---
title: SAP HANA'dan veri kopyalama
description: Bir Azure Veri Fabrikası ardışık hattında bir kopyalama etkinliği kullanarak SAP HANA'daki verileri desteklenen lavabo veri depolarına nasıl kopyalaylayamanızı öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/17/2020
ms.openlocfilehash: e1a3ff32956e8a8530684ba7f300f06d0c032227
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421125"
---
# <a name="copy-data-from-sap-hana-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak SAP HANA'daki verileri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-sap-hana-connector.md)
> * [Geçerli sürüm](connector-sap-hana.md)

Bu makalede, BIR SAP HANA veritabanındaki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

>[!TIP]
>Sap veri tümleştirme senaryosunda ADF'nin genel desteğini öğrenmek için, ayrıntılı giriş, karşılaştırma ve kılavuzlu [Azure Veri Fabrikası teknik incelemesini kullanarak SAP veri tümleştirmesine](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) bakın.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu SAP HANA bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

SAP HANA veritabanından desteklenen herhangi bir lavabo veri deposuna verileri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu SAP HANA konektörü destekler:

- SAP HANA veritabanının herhangi bir sürümünden veri kopyalama.
- **HANA bilgi modellerinden** (Analitik ve Hesaplama görünümleri gibi) ve **Satır/Sütun tablolarından**veri kopyalama.
- **Temel** veya **Windows** kimlik doğrulamasını kullanarak verileri kopyalama.
- SAP HANA kaynağından paralel kopyalama. Ayrıntılar için [SAP HANA bölümünden Paralel kopyaya](#parallel-copy-from-sap-hana) bakın.

> [!TIP]
> Verileri SAP HANA veri **deposuna** kopyalamak için genel ODBC bağlayıcısını kullanın. Ayrıntılarla [SAP HANA lavabosu](connector-odbc.md#sap-hana-sink) bakın. SAP HANA konektörü ve ODBC konektörü için bağlantılı hizmetlerin farklı türde olduğunu, bu nedenle yeniden kullanılamayacağını unutmayın.

## <a name="prerequisites"></a>Ön koşullar

Bu SAP HANA konektörünü kullanmak için şunları yapmanız gerekir:

- Kendi kendine barındırılan Tümleştirme Çalışma Zamanı'nı ayarlayın. Ayrıntılar için [Kendi barındırılan Tümleştirme Çalışma Zamanı](create-self-hosted-integration-runtime.md) makalesine bakın.
- SAP HANA ODBC sürücüsünü Tümleştirme Runtime makinesine yükleyin. SAP HANA ODBC sürücüsünü [SAP Software Download Center](https://support.sap.com/swdc) sayfasından indirebilirsiniz. **Windows için SAP HANA CLIENT**anahtar kelimesi ile arama yapın.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, SAP HANA bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Sap HANA bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **SapHana** | Evet |
| Connectionstring | **Temel kimlik doğrulaması** veya **Windows kimlik doğrulaması**kullanarak SAP HANA'ya bağlanmak için gereken bilgileri belirtin. Aşağıdaki örneklere bakın.<br>Bağlantı dizesinde sunucu/bağlantı noktası zorunludur (varsayılan bağlantı noktası 30015'tir) ve temel kimlik doğrulamayı kullanırken kullanıcı adı ve parola zorunludur. Ek gelişmiş ayarlar için [SAP HANA ODBC Bağlantı Özellikleri'ne](<https://help.sap.com/viewer/0eec0d68141541d1b07893a39944924e/2.0.02/en-US/7cab593774474f2f8db335710b2f5c50.html>) bakın<br/>Parolayı Azure Key Vault'a koyabilir ve parola yapılandırmasını bağlantı dizesinin dışına çekebilirsiniz. Azure [Key Vault makalesinde](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren Mağaza kimlik bilgilerine bakın. | Evet |
| userName | Windows kimlik doğrulamasını kullanırken kullanıcı adını belirtin. Örnek: `user@domain.com` | Hayır |
| password | Kullanıcı hesabı için parola belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullarda](#prerequisites)belirtildiği gibi Kendi kendine barındırılan Tümleştirme Çalışma Süresi gereklidir. |Evet |

**Örnek: temel kimlik doğrulamasını kullanma**

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "connectionString": "SERVERNODE=<server>:<port (optional)>;UID=<userName>;PWD=<Password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Windows kimlik doğrulamasını kullanma**

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "connectionString": "SERVERNODE=<server>:<port (optional)>;",
            "userName": "<username>", 
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

SAP HANA bağlantılı hizmeti aşağıdaki taşıma yüküyle kullanıyorsanız, ileriye dönük yeni hizmetini kullanmanız önerilirken, bu hizmet yine de olduğu gibi desteklenir.

**Örnek:**

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server>:<port (optional)>",
            "authenticationType": "Basic",
            "userName": "<username>",
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

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde SAP HANA veri kümesi tarafından desteklenen özelliklerin bir listesi sağlar.

SAP HANA'daki verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **SapHanaTable** | Evet |
| Şema | SAP HANA veritabanındaki şema adı. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |
| tablo | SAP HANA veritabanındaki tablonun adı. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek:**

```json
{
    "name": "SAPHANADataset",
    "properties": {
        "type": "SapHanaTable",
        "typeProperties": {
            "schema": "<schema name>",
            "table": "<table name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP HANA linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Dakti-zimd veri kümesi kullanıyorsanız, `RelationalTable` ileriye dönük yenisini kullanmanız önerilirken, yine de olduğu gibi desteklenir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde SAP HANA kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-hana-as-source"></a>KAYNAK OLARAK SAP HANA

>[!TIP]
>Veri bölümleme kullanarak SAP HANA'dan gelen verileri verimli bir şekilde yutmak için SAP [HANA bölümünden Paralel kopyadan](#parallel-copy-from-sap-hana) daha fazla bilgi edinin.

SAP HANA'daki verileri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **SapHanaSource** | Evet |
| sorgu | SAP HANA örneğindeki verileri okumak için SQL sorgusunu belirtir. | Evet |
| partitionOptions | SAP HANA'dan veri almak için kullanılan veri bölümleme seçeneklerini belirtir. [SAP HANA bölümünden Paralel kopyadan](#parallel-copy-from-sap-hana) daha fazla bilgi edinin.<br>İzin değerleri şunlardır: **Yok** (varsayılan), **PhysicalPartitionsOfTable**, **SapHanaDynamicRange**. [SAP HANA bölümünden Paralel kopyadan](#parallel-copy-from-sap-hana) daha fazla bilgi edinin. `PhysicalPartitionsOfTable`yalnızca bir tablodan veri kopyalanırken kullanılabilir, ancak sorgulanmaz. <br>Bir bölüm seçeneği etkinleştirildiğinde (yani `None`değil), SAP HANA'dan aynı anda yüklenen verileri [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) yüklemek için paralellik derecesi kopyalama etkinliği ayarı tarafından denetlenir. | False |
| partitionAyarlar | Veri bölümleme için ayarlar grubunu belirtin.<br>Bölüm seçeneği . `SapHanaDynamicRange` | False |
| partitionColumnName | Paralel kopya için bölüm tarafından kullanılacak kaynak sütunun adını belirtin. Belirtilmemişse, dizin veya tablonun birincil anahtarı otomatik olarak algılanır ve bölüm sütunu olarak kullanılır.<br>Bölüm seçeneği . `SapHanaDynamicRange` Kaynak verileri almak için bir sorgu `?AdfHanaDynamicRangePartitionCondition` kullanıyorsanız, WHERE yan tümcesini bağla. [SAP HANA bölümünden Paralel kopyadaki](#parallel-copy-from-sap-hana) örneğe bakın. | Evet bölüm `SapHanaDynamicRange` kullanırken. |
| paketBoyut | Verileri birden çok bloka bölmek için ağ paket boyutunu (Kilobaytolarak) belirtir. Kopyalamanız gereken büyük miktarda veri varsa, paket boyutunu artırmak çoğu durumda SAP HANA'nın okuma hızını artırabilir. Paket boyutunu ayarlarken performans testi önerilir. | Hayır.<br>Varsayılan değer 2048 (2MB) olur. |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPHANA",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP HANA input dataset name>",
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
                "type": "SapHanaSource",
                "query": "<SQL query for SAP HANA>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Dakti-yazılı `RelationalSource` kopya kaynağını kullanıyorsanız, ileriye dönük yeni sini kullanmanız önerilirken, bu kaynak olduğu gibi desteklenir.

## <a name="parallel-copy-from-sap-hana"></a>SAP HANA'dan paralel kopya

Veri Fabrikası SAP HANA bağlayıcısı, SAP HANA'daki verileri paralel olarak kopyalamak için yerleşik veri bölümleme sağlar. Veri bölümleme seçeneklerini kopyalama etkinliğinin **Kaynak** tablosunda bulabilirsiniz.

![Bölüm seçeneklerinin ekran görüntüsü](./media/connector-sap-hana/connector-sap-hana-partition-options.png)

Bölümlenmiş kopyalamayı etkinleştirdiğinizde, Veri Fabrikası bölümlere göre veri almak için SAP HANA kaynağınıza paralel sorgular çalıştırAr. Paralel derece, kopyalama [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) etkinliği üzerindeki ayar tarafından denetlenir. Örneğin, dörde `parallelCopies` ayarlarsanız, Veri Fabrikası aynı anda belirttiğiniz bölüm seçeneğiniz ve ayarlarınızı temel alarak dört sorgu oluşturur ve çalıştırAr ve her sorgu SAP HANA'nızdan verilerin bir kısmını alır.

Özellikle SAP HANA'nızdan büyük miktarda veri yuttuğunuzda veri bölümleme ile paralel kopyalamayı etkinleştirmeniz önerilir. Aşağıda farklı senaryolar için önerilen yapılandırmalar vardır. Verileri dosya tabanlı veri deposuna kopyalarken, bir klasöre birden çok dosya olarak yazman önerilir (yalnızca klasör adını belirtin), bu durumda performans tek bir dosyaya yazmaktan daha iyidir.

| Senaryo                                           | Önerilen ayarlar                                           |
| -------------------------------------------------- | ------------------------------------------------------------ |
| Büyük tablodan tam yük.                        | **Bölüm seçeneği**: Tablonun fiziksel bölümleri. <br><br/>Yürütme sırasında, Veri Fabrikası belirtilen SAP HANA tablosunun fiziksel bölüm türünü otomatik olarak algılar ve ilgili bölüm stratejisini seçer:<br>- **Aralık Bölümleme**: Tablo için tanımlanan bölüm sütunu ve bölüm aralıklarını alın, ardından verileri aralığına göre kopyalayın. <br>- **Karma Bölümleme**: Karma bölümleme tuşunu bölüm sütunu olarak kullanın, ardından ADF hesaplanan aralıkları temel alınarak verileri bölümleme ve kopyalama. <br>- **Round-Robin Partitioning** or **No Partition**: Birincil anahtarı bölüm sütunu olarak kullanın, ardından ADF hesaplanan aralıkları temel alınarak verileri bölümleyip kopyalayın. |
| Özel bir sorgu kullanarak büyük miktarda veri yükleyin. | **Bölüm seçeneği**: Dinamik aralık bölümü.<br>**Sorgu** `SELECT * FROM <TABLENAME> WHERE ?AdfHanaDynamicRangePartitionCondition AND <your_additional_where_clause>`: .<br>**Partition column**: Dinamik aralık bölümü uygulamak için kullanılan sütunu belirtin. <br><br>Yürütme sırasında, Veri Fabrikası öncelikle belirtilen bölüm sütununun değer aralıklarını hesaplar, satırları farklı bölüm sütun değerleri ve ADF paralel kopya ayarına göre bir `?AdfHanaDynamicRangePartitionCondition` dizi kovada eşit olarak dağıtarak, sonra her bölüm için bölüm sütunu değer aralığını filtreleyerek değiştirir ve SAP HANA'ya gönderir.<br><br>Birden çok sütunu bölüm sütunu olarak kullanmak istiyorsanız, her sütunun değerlerini sorguda bir sütun olarak eşleyebilir `SELECT * FROM (SELECT *, CONCAT(<KeyColumn1>, <KeyColumn2>) AS PARTITIONCOLUMN FROM <TABLENAME>) WHERE ?AdfHanaDynamicRangePartitionCondition`ve ADF'de bölüm sütunu olarak belirtebilirsiniz. |

**Örnek: tablonun fiziksel bölümleri ile sorgu**

```json
"source": {
    "type": "SapHanaSource",
    "partitionOption": "PhysicalPartitionsOfTable"
}
```

**Örnek: dinamik aralık bölümü ile sorgu**

```json
"source": {
    "type": "SapHanaSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfHanaDynamicRangePartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "SapHanaDynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<Partition_column_name>"
    }
}
```

## <a name="data-type-mapping-for-sap-hana"></a>SAP HANA için veri türü eşleme

SAP HANA'daki verileri kopyalarken, SAP HANA veri türlerinden Azure Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopya etkinliği kaynak şemasını ve veri türünü lavaboyla nasıl eşler hakkında bilgi edinmek için Şema ve [veri türü eşlemelerine](copy-activity-schema-and-type-mapping.md) bakın.

| SAP HANA veri türü | Veri fabrikası geçici veri türü |
| ------------------ | ------------------------------ |
| ALFANÜM           | Dize                         |
| Bigint             | Int64                          |
| Ikili             | Bayt[]                         |
| BINTEXT            | Dize                         |
| Blob               | Bayt[]                         |
| Bool               | Bayt                           |
| Clob               | Dize                         |
| DATE               | DateTime                       |
| On -da -lık            | Ondalık                        |
| Çift             | Çift                         |
| Float              | Çift                         |
| TAMSAYI            | Int32                          |
| Nclob              | Dize                         |
| Nvarchar           | Dize                         |
| GERÇEK SAYI               | Tek                         |
| İkinciTARIH         | DateTime                       |
| KıSA METIN          | Dize                         |
| KÜÇÜKDELIK       | Ondalık                        |
| Smallint           | Int16                          |
| Stgeometrytype     | Bayt[]                         |
| STPOINTTYPE        | Bayt[]                         |
| TEXT               | Dize                         |
| TIME               | TimeSpan                       |
| Tinyint            | Bayt                           |
| Varchar            | Dize                         |
| Zaman damgası          | DateTime                       |
| Varbinary          | Bayt[]                         |

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
