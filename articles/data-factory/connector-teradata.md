---
title: Azure Data Factory kullanarak Teradata 'dan veri kopyalama | Microsoft Docs
description: Data Factory hizmetinin Teradata Bağlayıcısı, Teradata veritabanındaki verileri, havuz olarak Data Factory desteklenen veri depolarına kopyalamanızı sağlar.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/23/2019
ms.author: jingwang
ms.openlocfilehash: bec1c0c3523e6d9cfb0b2fdbc7a093ffe0637743
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232503"
---
# <a name="copy-data-from-teradata-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Teradata 'dan veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
>
> * [Sürüm 1](v1/data-factory-onprem-teradata-connector.md)
> * [Geçerli sürüm](connector-teradata.md)

Bu makalede, bir Teradata veritabanından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)sayfasında oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bir Teradata veritabanından, desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Teradata Bağlayıcısı şunları destekler:

- Teradata **sürüm 14,10, 15,0, 15,10, 16,0, 16,10 ve 16,20**.
- **Temel** veya **Windows** kimlik doğrulamasını kullanarak verileri kopyalama.
- Teradata kaynağından paralel kopyalama. Ayrıntılar için [Teradata 'Dan paralel kopyalama](#parallel-copy-from-teradata) bölümüne bakın.

> [!NOTE]
>
> Şirket içinde barındırılan Integration Runtime v 3.18 yayımlandıktan sonra, Teradata bağlayıcısını Azure Data Factory yükselttiniz. Önceki Teradata bağlayıcısını kullanan tüm mevcut iş yükleri hala desteklenmektedir. Bununla birlikte, yeni iş yükleri için yenisini kullanmak iyi bir fikirdir. Yeni yolun farklı bir bağlı hizmet, veri kümesi ve kopyalama kaynağı kümesi gerektirdiğini unutmayın. Yapılandırma ayrıntıları için aşağıdaki ilgili bölümlere bakın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

Integration Runtime, 3,18 sürümünden başlayarak yerleşik bir Teradata sürücüsü sağlar. Herhangi bir sürücüyü el ile yüklemeniz gerekmez. Sürücü, şirket içinde barındırılan C++ tümleştirme çalışma zamanı makinesinde "Visual Redistributable 2012 güncelleştirme 4" gerektirir. Henüz yüklemediyseniz, [buradan](https://www.microsoft.com/en-sg/download/details.aspx?id=30679)indirin.

3,18 ' den önceki şirket içinde barındırılan tümleştirme çalışma zamanı sürümü için, tümleştirme çalışma zamanı makinesine Teradata, sürüm 14 veya üzeri [için .net veri sağlayıcısı](https://go.microsoft.com/fwlink/?LinkId=278886)sürümünü yüklemelisiniz. 

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Teradata bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Teradata bağlı hizmeti aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği **Teradata**olarak ayarlanmalıdır. | Evet |
| connectionString | Teradata veritabanı örneğine bağlanmak için gereken bilgileri belirtir. Aşağıdaki örneklere bakın.<br/>Ayrıca Azure Key Vault bir parola yerleştirebilir ve `password` yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Daha ayrıntılı bilgi için [Azure Key Vault 'de mağaza kimlik bilgilerini](store-credentials-in-key-vault.md) inceleyin. | Evet |
| kullanıcı adı | Teradata veritabanına bağlanmak için bir Kullanıcı adı belirtin. Windows kimlik doğrulaması kullanırken geçerlidir. | Hayır |
| password | Kullanıcı adı için belirttiğiniz kullanıcı hesabı için bir parola belirtin. Ayrıca, [Azure Key Vault depolanan bir gizli dizi başvurusunu](store-credentials-in-key-vault.md)da seçebilirsiniz. <br>Windows kimlik doğrulaması kullandığınızda veya temel kimlik doğrulaması için Key Vault bir parolaya başvurulduğunda geçerlidir. | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Evet |

**Temel kimlik doğrulaması kullanan örnek**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>;Uid=<username>;Pwd=<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Windows kimlik doğrulamasını kullanan örnek**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>",
            "username": "<username>",
            "password": "<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

> [!NOTE]
>
> Aşağıdaki yük hala destekleniyor. Bununla birlikte, yeni bir tane kullanmanız gerekir.

**Önceki yük:**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<Basic/Windows>",
            "username": "<username>",
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

Bu bölüm, Teradata veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar. Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md).

Teradata 'tan veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği olarak `TeradataTable`ayarlanmalıdır. | Evet |
| database | Teradata veritabanının adı. | Hayır (etkinlik kaynağı "query" belirtilmişse) |
| table | Teradata veritabanındaki tablonun adı. | Hayır (etkinlik kaynağı "query" belirtilmişse) |

**Örnek:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "TeradataTable",
        "typeProperties": {},
        "schema": [],        
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

> [!NOTE]
>
> `RelationalTable`tür veri kümesi hala destekleniyor. Ancak, yeni veri kümesini kullanmanızı öneririz.

**Önceki yük:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölüm, Teradata kaynağı tarafından desteklenen özelliklerin bir listesini sağlar. Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md). 

### <a name="teradata-as-source"></a>Kaynak olarak Teradata

>[!TIP]
>Veri bölümleme kullanarak Teradata 'tan verileri verimli bir şekilde yüklemek için [Teradata 'Den paralel kopyadan](#parallel-copy-from-teradata) daha fazla bilgi edinin.

Teradata 'tan veri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği olarak `TeradataSource`ayarlanmalıdır. | Evet |
| query | Verileri okumak için özel bir SQL sorgusu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir.<br>Bölümlenmiş yükü etkinleştirdiğinizde, sorgunuza karşılık gelen yerleşik bölüm parametrelerini de eklemeniz gerekir. Örnekler için, [Teradata 'Den paralel kopyalama](#parallel-copy-from-teradata) bölümüne bakın. | Hayır (veri kümesindeki tablo belirtilmişse) |
| partitionOptions | Teradata 'tan veri yüklemek için kullanılan veri bölümleme seçeneklerini belirtir. <br>İzin verme değerleri şunlardır: **Hiçbiri** (varsayılan), **karma** ve **DynamicRange**.<br>Bir bölüm seçeneği etkinleştirildiğinde (değil `None`), bir Teradata veritabanından eşzamanlı olarak veri yükleme derecesi, kopyalama etkinliğindeki [`parallelCopies`](copy-activity-performance.md#parallel-copy) ayar tarafından denetlenir. | Hayır |
| partitionSettings | Veri bölümleme için ayarların grubunu belirtin. <br>Bölüm seçeneği `None`olmadığında Uygula. | Hayır |
| partitionColumnName | Paralel kopya için Aralık bölümü veya karma bölümü tarafından kullanılacak kaynak sütunun adını belirtin. Belirtilmemişse, tablonun birincil dizini otomatik olarak algılanır ve bölüm sütunu olarak kullanılır. <br>Bölüm seçeneği `Hash` veya `DynamicRange`olduğunda geçerlidir. Kaynak verileri, Hook `?AdfHashPartitionCondition` veya `?AdfRangePartitionColumnName` WHERE yan tümcesini almak için bir sorgu kullanırsanız. [Teradata 'Den paralel kopyalama](#parallel-copy-from-teradata) bölümündeki örneğe bakın. | Hayır |
| partitionüstsınırı | Verilerin kopyalanacağı bölüm sütununun en büyük değeri. <br>Bölüm seçeneği `DynamicRange`olduğunda Uygula. Kaynak verileri almak için sorgu kullanırsanız WHERE yan tümcesinde kanca `?AdfRangePartitionUpbound` . Bir örnek için, [Teradata 'Den paralel kopyalama](#parallel-copy-from-teradata) bölümüne bakın. | Hayır |
| Partitionalme sınırı | Verilerin kopyalanacağı bölüm sütununun en küçük değeri. <br>Bölüm seçeneği `DynamicRange`olduğunda uygulayın. Kaynak verileri almak için bir sorgu kullanırsanız, WHERE yan tümcesinde kanca `?AdfRangePartitionLowbound` . Bir örnek için, [Teradata 'Den paralel kopyalama](#parallel-copy-from-teradata) bölümüne bakın. | Hayır |

> [!NOTE]
>
> `RelationalSource`tür kopyalama kaynağı hala destekleniyor, ancak Teradata 'den yeni yerleşik paralel yükü (Bölüm seçenekleri) desteklemiyor. Ancak, yeni veri kümesini kullanmanızı öneririz.

**Örnek: bölüm olmadan temel sorgu kullanarak veri kopyalama**

```json
"activities":[
    {
        "name": "CopyFromTeradata",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Teradata input dataset name>",
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
                "type": "TeradataSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="parallel-copy-from-teradata"></a>Teradata 'dan paralel kopya

Data Factory Teradata Bağlayıcısı, Teradata 'den paralel olarak veri kopyalamak için yerleşik veri bölümlendirme sağlar. Kopyalama etkinliğinin **kaynak** tablosunda veri bölümleme seçeneklerini bulabilirsiniz.

![Bölüm seçeneklerinin ekran görüntüsü](./media/connector-teradata/connector-teradata-partition-options.png)

Bölümlenmiş kopyayı etkinleştirdiğinizde Data Factory, verileri bölümlere göre yüklemek için Teradata kaynağınıza karşı paralel sorgular çalıştırır. Paralel derece kopyalama etkinliğindeki [`parallelCopies`](copy-activity-performance.md#parallel-copy) ayar tarafından denetlenir. Örneğin, dört olarak ayarlarsanız `parallelCopies` , Data Factory aynı anda, belirtilen bölüm seçeneğiniz ve ayarlarınıza göre dört sorgu üretir ve çalışır ve her sorgu, Teradata veritabanınızdaki verilerin bir kısmını alır.

Teradata veritabanından büyük miktarda veri yüklediğinizde özellikle veri bölümleme ile paralel kopyayı etkinleştirmeniz önerilir. Farklı senaryolar için önerilen yapılandırma aşağıda verilmiştir. Dosya tabanlı veri deposuna veri kopyalarken, bir klasöre birden çok dosya (yalnızca klasör adını belirt) olarak yazma işlemi geri çağrılır, bu durumda performans tek bir dosyaya yazılmasından daha iyidir.

| Senaryo                                                     | Önerilen ayarlar                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Büyük tablodan tam yük.                                   | **Bölüm seçeneği**: Yla. <br><br/>Yürütme sırasında, Data Factory otomatik olarak PK sütununu algılar, buna karşı bir karma uygular ve verileri bölümlere göre kopyalar. |
| Özel bir sorgu kullanarak büyük miktarda veriyi yükleyin.                 | **Bölüm seçeneği**: Yla.<br>**Sorgu**: `SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>`.<br>**Bölüm sütunu**: Karma bölüm uygulamak için kullanılan sütunu belirtin. Belirtilmezse, Data Factory Teradata veri kümesinde belirttiğiniz tablonun PK sütununu otomatik olarak algılar.<br><br>Yürütme sırasında Data Factory, karma `?AdfHashPartitionCondition` bölüm mantığı ile değiştirilir ve Teradata 'a gönderir. |
| Aralık bölümlendirme için eşit olarak dağıtılmış değere sahip bir tamsayı sütunu olan özel bir sorgu kullanarak büyük miktarda veri yükleyin. | **Bölüm seçenekleri**: Dinamik Aralık bölümü.<br>**Sorgu**: `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Bölüm sütunu**: Verileri bölümlemek için kullanılan sütunu belirtin. Tamsayı veri türünde bir sütuna göre bölümleyebilirsiniz.<br>**Bölüm üst sınırı** ve **bölüm alt sınırı**: Yalnızca alt ve üst Aralık arasında veri almak için bölüm sütununa karşı filtreleme yapmak istediğinizi belirtin.<br><br>Yürütme sırasında Data Factory, `?AdfRangePartitionColumnName` `?AdfRangePartitionUpbound`,, ve `?AdfRangePartitionLowbound` her bölüm için gerçek sütun adı ve değer aralıklarıyla değiştirilir ve Teradata 'a gönderir. <br>Örneğin, "ID" adlı bölüm sütununuzu alt sınır 1 ve üst sınır 80 olarak ayarlarsanız, paralel kopya 4 olarak ayarlandıysa Data Factory verileri 4 bölümden alır. Kimlikleri sırasıyla [1, 20], [21, 40], [41, 60] ve [61, 80] arasındadır. |

**Örnek: karma bölüm ile sorgulama**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "Hash",
    "partitionSettings": {
        "partitionColumnName": "<hash_partition_column_name>"
    }
}
```

**Örnek: Dinamik Aralık bölümü ile sorgulama**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<dynamic_range_partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column>",
        "partitionLowerBound": "<lower_value_of_partition_column>"
    }
}
```

## <a name="data-type-mapping-for-teradata"></a>Teradata için veri türü eşlemesi

Teradata 'tan veri kopyaladığınızda aşağıdaki eşlemeler geçerlidir. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Teradata veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Byte[] |
| Byte |Byte[] |
| Byteınt |Int16 |
| Char |Dize |
| CLOB |Dize |
| Date |DateTime |
| Decimal |Decimal |
| Double |Double |
| Graphic |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Integer |Int32 |
| Interval Day |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Day To Hour |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Day To Minute |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Day To Second |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Hour |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Hour To Minute |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Hour To Second |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Minute |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Minute To Second |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Month |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Second |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Year |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Interval Year To Month |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Number |Double |
| Süre (Tarih) |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Süre (saat) |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Süre (saat dilimiyle birlikte) |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Süre (zaman damgası) |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Süre (saat dilimiyle zaman damgası) |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Smallint |Int16 |
| Time |TimeSpan |
| Time With Time Zone |TimeSpan |
| Timestamp |DateTime |
| Timestamp With Time Zone |DateTime |
| VarByte |Byte[] |
| VarChar |Dize |
| VarGraphic |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |
| Xml |Desteklenmiyor. Kaynak sorgusunda açık tür dönüştürme uygulayın. |


## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
