---
title: SAP HANA verileri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliği kullanarak SAP HANA verileri desteklenen havuz verileri depolarına kopyalamayı öğrenin.
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
ms.openlocfilehash: fa165c21622110bb18476efdebf3264a11e26ad7
ms.sourcegitcommit: dfa543fad47cb2df5a574931ba57d40d6a47daef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77431120"
---
# <a name="copy-data-from-sap-hana-using-azure-data-factory"></a>Azure Data Factory kullanarak SAP HANA verileri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-sap-hana-connector.md)
> * [Geçerli sürüm](connector-sap-hana.md)

Bu makalede, SAP HANA veritabanından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

>[!TIP]
>ADF 'nin SAP veri tümleştirme senaryosunda genel desteğini öğrenmek için ayrıntılı giriş, comparme ve kılavuzla [Azure Data Factory Teknik İnceleme kullanarak SAP veri tümleştirme](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) konusuna bakın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu SAP HANA Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

SAP HANA veritabanından, desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu SAP HANA Bağlayıcısı şunları destekler:

- SAP HANA veritabanının herhangi bir sürümünden veri kopyalanıyor.
- **Hana bilgi modellerinden** (analitik ve hesaplama görünümleri gibi) ve **satır/sütun tablolarının**verilerini kopyalama.
- **Temel** veya **Windows** kimlik doğrulaması kullanarak verileri kopyalama.
- SAP HANA kaynağından paralel kopyalama. Ayrıntılar için [SAP HANA bölümüne paralel kopyalama](#parallel-copy-from-sap-hana) konusuna bakın.

> [!TIP]
> Verileri **SAP HANA veri** deposuna kopyalamak IÇIN Genel ODBC Bağlayıcısı ' nı kullanın. Ayrıntılara [SAP HANA havuza](connector-odbc.md#sap-hana-sink) bakın. SAP HANA Bağlayıcısı ve ODBC Bağlayıcısı için bağlı hizmetlerin farklı türde olduğunu ve bu nedenle yeniden kullanılmamasını aklınızda olun.

## <a name="prerequisites"></a>Önkoşullar

Bu SAP HANA bağlayıcısını kullanmak için şunları yapmanız gerekir:

- Şirket içinde barındırılan bir Integration Runtime ayarlayın. Ayrıntılar için bkz. [Şirket içinde barındırılan Integration Runtime](create-self-hosted-integration-runtime.md) makalesi.
- SAP HANA ODBC sürücüsünü Integration Runtime makinesine yükler. SAP HANA ODBC sürücüsünü [SAP Software Download Center](https://support.sap.com/swdc)'dan indirebilirsiniz. **Windows için SAP HANA**anahtar sözcüğünü kullanarak arama yapın.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler SAP HANA bağlayıcıya özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

SAP HANA bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği: **Saphana** olarak ayarlanmalıdır | Yes |
| connectionString | **Temel kimlik doğrulaması** veya **Windows kimlik doğrulaması**kullanarak SAP HANA bağlanmak için gereken bilgileri belirtin. Aşağıdaki örneklere bakın.<br>Bağlantı dizesinde, sunucu/bağlantı noktası zorunludur (varsayılan bağlantı noktası 30015 ' dir), temel kimlik doğrulaması kullanılırken Kullanıcı adı ve parola zorunludur. Ek Gelişmiş ayarlar için [SAP HANA ODBC bağlantı özellikleri](<https://help.sap.com/viewer/0eec0d68141541d1b07893a39944924e/2.0.02/en-US/7cab593774474f2f8db335710b2f5c50.html>) ' ne bakın.<br/>Parolayı Azure Key Vault de yerleştirebilir ve parola yapılandırmasını bağlantı dizesinin dışına çekebilirsiniz. Daha ayrıntılı bilgi için [Azure Key Vault makalesinde mağaza kimlik bilgilerini](store-credentials-in-key-vault.md) inceleyin. | Yes |
| userName adı | Windows kimlik doğrulaması kullanırken kullanıcı adını belirtin. Örnek: `user@domain.com` | Hayır |
| password | Kullanıcı hesabı için parola belirtin. Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Integration Runtime](concepts-integration-runtime.md) . [Önkoşul](#prerequisites)bölümünde belirtildiği gibi, kendinden konak Integration Runtime gereklidir. |Yes |

**Örnek: temel kimlik doğrulaması kullanma**

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

Aşağıdaki yük ile SAP HANA bağlı hizmeti kullanıyorsanız, hala olduğu gibi desteklenirken, ileri ' yi kullanmanız önerilir.

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

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm SAP HANA veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

SAP HANA verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği: **Saphanatable** olarak ayarlanmalıdır | Yes |
| schema | SAP HANA veritabanındaki şemanın adı. | Hayır (etkinlik kaynağı "query" belirtilmişse) |
| table | SAP HANA veritabanındaki tablonun adı. | Hayır (etkinlik kaynağı "query" belirtilmişse) |

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

Türü belirlenmiş `RelationalTable` veri kümesini kullanıyorsanız, bunun olduğu gibi hala desteklenmektedir, ileri ' yi kullanmaya devam etmeniz önerilir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm SAP HANA kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-hana-as-source"></a>Kaynak olarak SAP HANA

>[!TIP]
>Veri bölümleme kullanarak SAP HANA verileri verimli bir şekilde almak için [SAP HANA bölümünden paralel kopyadan](#parallel-copy-from-sap-hana) daha fazla bilgi edinin.

SAP HANA verileri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği: **Saphanasource** olarak ayarlanmalıdır | Yes |
| sorgu | SAP HANA örneğinden verileri okumak için SQL sorgusunu belirtir. | Yes |
| partitionOptions | SAP HANA verileri almak için kullanılan veri bölümleme seçeneklerini belirtir. [SAP HANA bölümünden paralel kopyadan](#parallel-copy-from-sap-hana) daha fazla bilgi edinin.<br>Değerlere izin ver: **None** (default), **Physicalpartitionsoftable**, **SapHanaDynamicRange**. [SAP HANA bölümünden paralel kopyadan](#parallel-copy-from-sap-hana) daha fazla bilgi edinin. `PhysicalPartitionsOfTable`, yalnızca bir tablodan veri kopyalanırken kullanılabilir ancak sorgu kullanılamaz. <br>Bir bölüm seçeneği etkinleştirildiğinde (`None`değil), SAP HANA eşzamanlı olarak veri yükleme ile paralellik derecesi kopyalama etkinliğindeki [`parallelCopies`](copy-activity-performance.md#parallel-copy) ayarı tarafından denetlenir. | False |
| partitionSettings | Veri bölümleme için ayarların grubunu belirtin.<br>Bölüm seçeneği `SapHanaDynamicRange`olduğunda Uygula. | False |
| partitionColumnName | Paralel kopya için bölüm tarafından kullanılacak kaynak sütunun adını belirtin. Belirtilmemişse, tablonun dizini veya birincil anahtarı otomatik olarak algılanır ve bölüm sütunu olarak kullanılır.<br>Bölüm seçeneği `SapHanaDynamicRange`olduğunda geçerlidir. Kaynak verileri almak için bir sorgu kullanırsanız, `?AdfHanaDynamicRangePartitionCondition` WHERE yan tümcesinde kanca. [SAP HANA bölümünde paralel kopyalama](#parallel-copy-from-sap-hana) örneğine bakın. | `SapHanaDynamicRange` bölümü kullanılırken Evet. |
| packetSize | Verilerin birden çok bloğuyla bölüneceği ağ paketi boyutunu (kilobayt olarak) belirtir. Kopyalanacak büyük miktarda veriniz varsa, paket boyutunu artırmak çoğu durumda SAP HANA okuma hızını artırabilir. Paket boyutu ayarlanırken performans testi önerilir. | Hayır.<br>Varsayılan değer 2048 ' dir (2MB). |

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

Türü belirtilmiş `RelationalSource` kopyalama kaynağı kullanıyorsanız, yeni bir tane olduğu gibi, ileri ' yi kullanmanız önerilir.

## <a name="parallel-copy-from-sap-hana"></a>SAP HANA paralel kopyası

Data Factory SAP HANA Bağlayıcısı, verileri SAP HANA paralel olarak kopyalamak için yerleşik veri bölümlemesini sağlar. Kopyalama etkinliğinin **kaynak** tablosunda veri bölümleme seçeneklerini bulabilirsiniz.

![Bölüm seçeneklerinin ekran görüntüsü](./media/connector-sap-hana/connector-sap-hana-partition-options.png)

Bölümlenmiş kopyayı etkinleştirdiğinizde Data Factory, verileri bölümlere göre almak için SAP HANA kaynağınıza paralel sorgular çalıştırır. Paralel derece kopyalama etkinliğinde [`parallelCopies`](copy-activity-performance.md#parallel-copy) ayarıyla denetlenir. Örneğin, `parallelCopies` dört olarak ayarlarsanız, Data Factory aynı anda, belirtilen bölüm seçeneğiniz ve ayarlarınıza göre dört sorgu üretir ve çalışır ve her sorgu, SAP HANA verilerin bir kısmını alır.

SAP HANA, özellikle de büyük miktarda veri aldığınızda, verilerin bölümlenmesi ile paralel kopyayı etkinleştirmeniz önerilir. Farklı senaryolar için önerilen yapılandırma aşağıda verilmiştir. Dosya tabanlı veri deposuna veri kopyalarken, bir klasöre birden çok dosya (yalnızca klasör adı belirtin) yazılması önerilir, bu durumda performans tek bir dosyaya yazılmasından daha iyidir.

| Senaryo                                           | Önerilen ayarlar                                           |
| -------------------------------------------------- | ------------------------------------------------------------ |
| Büyük tablodan tam yük.                        | **Bölüm seçeneği**: tablonun fiziksel bölümleri. <br><br/>Yürütme sırasında Data Factory, belirtilen SAP HANA tablosunun fiziksel bölüm türünü otomatik olarak algılar ve ilgili bölüm stratejisini seçer:<br>**Aralık bölümleme**- : tablo için tanımlanan bölüm sütununu ve bölüm aralıklarını alın, sonra verileri aralığa göre kopyalayın. <br>- **karma bölümleme**: karma bölüm anahtarını bölüm sütunu olarak kullanın, ardından verileri BÖLÜMLEYIP ADF hesaplanan aralıklar temelinde kopyalayın. <br>- hepsini bir **kez deneme bölümlendirme** veya **bölüm yok**: birincil anahtarı bölüm sütunu olarak kullanın, ardından verileri bölümleyip ADF hesaplanan aralıklar temelinde kopyalayın. |
| Özel bir sorgu kullanarak büyük miktarda veriyi yükleyin. | **Bölüm seçeneği**: Dinamik Aralık bölümü.<br>**Sorgu**: `SELECT * FROM <TABLENAME> WHERE ?AdfHanaDynamicRangePartitionCondition AND <your_additional_where_clause>`.<br>**Bölüm sütunu**: Dinamik Aralık bölümünü uygulamak için kullanılan sütunu belirtin. <br><br>Yürütme sırasında Data Factory, belirtilen bölüm sütununun değer aralıklarını eşit bir şekilde dağıtarak, satırları ayrı bölüm sütun değerleri ve ADF paralel kopyalama ayarı sayısına göre eşit bir şekilde dağıtır ve ardından `?AdfHanaDynamicRangePartitionCondition`, her bölüm için bölüm sütunu değer aralığını filtreleyerek ve SAP HANA gönderir.<br><br>Birden çok sütunu bölüm sütunu olarak kullanmak istiyorsanız, her sütunun değerini sorguda tek bir sütun olarak birleştirebilir ve ADF 'de `SELECT * FROM (SELECT *, CONCAT(<KeyColumn1>, <KeyColumn2>) AS PARTITIONCOLUMN FROM <TABLENAME>) WHERE ?AdfHanaDynamicRangePartitionCondition`gibi bir bölüm sütunu olarak belirtebilirsiniz. |

**Örnek: bir tablonun fiziksel bölümleri ile sorgulama**

```json
"source": {
    "type": "SapHanaSource",
    "partitionOption": "PhysicalPartitionsOfTable"
}
```

**Örnek: Dinamik Aralık bölümü ile sorgulama**

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

## <a name="data-type-mapping-for-sap-hana"></a>SAP HANA için veri türü eşlemesi

SAP HANA verileri kopyalarken, SAP HANA veri türlerinden aşağıdaki eşlemeler Azure Data Factory geçici veri türlerini kullanır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) .

| SAP HANA veri türü | Veri Fabrikası geçici veri türü |
| ------------------ | ------------------------------ |
| ALPHANUM           | Dize                         |
| BIGıNT             | Int64                          |
| BINARY             | Byte[]                         |
| BINTEXT            | Dize                         |
| BLOB               | Byte[]                         |
| BOOL               | Bayt                           |
| CLOB               | Dize                         |
| DATE               | DateTime                       |
| DECIMAL            | Ondalık                        |
| DOUBLE             | çift                         |
| FLOAT              | çift                         |
| INTEGER            | Int32                          |
| NCLOB              | Dize                         |
| NVARCHAR           | Dize                         |
| REAL               | Tek                         |
| SECONDDATE         | DateTime                       |
| SHORTTEXT          | Dize                         |
| SMALLDECıMAL       | Ondalık                        |
| SMALLINT           | Int16                          |
| STGEOMETRYTYPE     | Byte[]                         |
| STPOINTTYPE        | Byte[]                         |
| TEXT               | Dize                         |
| TIME               | TimeSpan                       |
| TINYINT            | Bayt                           |
| VARCHAR            | Dize                         |
| TIMESTAMP          | DateTime                       |
| VARBINARY          | Byte[]                         |

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
