---
title: Azure Veri Fabrikası'nı kullanarak Teradata Vantage'daki verileri kopyalama
description: Veri Fabrikası hizmetinin Teradata Bağlayıcısı, Veri Fabrikası tarafından desteklenen veri depolarına teradata vantage'daki verileri lavabo olarak kopyalamanızı sağlar.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: jingwang
ms.openlocfilehash: 4eed79210e3e39f82b892ac0681e161ebb59597e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418040"
---
# <a name="copy-data-from-teradata-vantage-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Teradata Vantage'daki verileri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
>
> * [Sürüm 1](v1/data-factory-onprem-teradata-connector.md)
> * [Geçerli sürüm](connector-teradata.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Teradata Vantage'dan verileri kopyalamak için Azure Veri Fabrikası'ndaki kopyalama etkinliğinin nasıl kullanılacağı açıklanmaktadır. [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Teradata bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Teradata Vantage'daki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Teradata bağlayıcıdestekler:

- Teradata **sürüm 14.10, 15.0, 15.10, 16.0, 16.10 ve 16.20**.
- **Temel** veya **Windows** kimlik doğrulamasını kullanarak verileri kopyalama.
- Teradata kaynağından paralel kopyalama. Ayrıntılar için [Teradata bölümünden Paralel kopyaya](#parallel-copy-from-teradata) bakın.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

Kendi kendine barındırılan Tümleştirme Runtime kullanıyorsanız, sürüm 3.18'den başlayarak yerleşik bir Teradata sürücüsü sağladığını unutmayın. Herhangi bir sürücüyü el ile yüklemeniz gerekmez. Sürücü, kendi barındırılan tümleştirme çalışma saatinde "Visual C++ Redistributable 2012 Update 4" gerektirir. Henüz yüklü değilseniz, [buradan](https://www.microsoft.com/en-sg/download/details.aspx?id=30679)indirin.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Teradata bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Teradata bağlantılı hizmet aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **Teradata**olarak ayarlanmalıdır. | Evet |
| Connectionstring | Teradata örneğine bağlanmak için gereken bilgileri belirtir. Aşağıdaki örneklere bakın.<br/>Ayrıca Azure Key Vault'a bir parola `password` koyabilir ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Azure [Anahtar Kasası'ndaki Mağaza kimlik bilgilerine](store-credentials-in-key-vault.md) daha fazla ayrıntı yla bakın. | Evet |
| kullanıcı adı | Teradata'ya bağlanmak için bir kullanıcı adı belirtin. Windows kimlik doğrulamasını kullanırken geçerlidir. | Hayır |
| password | Kullanıcı adı için belirttiğiniz kullanıcı hesabı için bir parola belirtin. [Azure Key Vault'ta depolanan bir gizli ye başvurmayı](store-credentials-in-key-vault.md)da seçebilirsiniz. <br>Windows kimlik doğrulaması kullanırken veya temel kimlik doğrulama için Key Vault'ta bir parolaya başvururken geçerlidir. | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

Servis talebinize göre bağlantı dizesi olarak ayarlayabildiğiniz daha fazla bağlantı özelliği:

| Özellik | Açıklama | Varsayılan değer |
|:--- |:--- |:--- |
| Characterset | Oturum için kullanılacak karakter kümesi. Örneğin, `CharacterSet=UTF16`.<br><br/>Bu değer, kullanıcı tanımlı bir karakter kümesi veya önceden tanımlanmış aşağıdaki karakter kümelerinden biri olabilir: <br/>- ASCII<br/>- UTF8<br/>- UTF16<br/>- LATIN1252_0A<br/>- LATIN9_0A<br/>- LATIN1_0A<br/>- Shift-JIS (Windows, DOS uyumlu, KANJISJIS_0S)<br/>- EUC (Unix uyumlu, KANJIEC_0U)<br/>- IBM Ana Bilgisayar (KANJIEBCDIC5035_0I)<br/>- KANJI932_1S0<br/>- BIG5 (TCHBIG5_1R0)<br/>- GB (SCHGB2312_1T0)<br/>- SCHINESE936_6R0<br/>- TCHINESE950_8R0<br/>- NetworkKorean (HANGULKSC5601_2R4)<br/>- HANGUL949_7R0<br/>- ARABIC1256_6A0<br/>- CYRILLIC1251_2A0<br/>- HEBREW1255_5A0<br/>- LATIN1250_1A0<br/>- LATIN1254_7A0<br/>- LATIN1258_8A0<br/>- THAI874_4A0 | Varsayılan değer. `ASCII` |
| MaxRespSize |Kilobayt (KBs) olarak SQL istekleri için yanıt arabelleği maksimum boyutu. Örneğin, `MaxRespSize=‭10485760‬`.<br/><br/>Teradata Database sürüm 16.00 veya sonraki sürüm için, maksimum değer 7361536'dır. Önceki sürümleri kullanan bağlantılar için en büyük değer 1048576'dır. | Varsayılan değer. `65536` |

**Temel kimlik doğrulamasını kullanma örneği**

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

**Windows kimlik doğrulamasını kullanma örneği**

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
> Aşağıdaki yük hala desteklenir. Ancak ileriye doğru, yenisini kullanmalısınız.

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

Bu bölümde Teradata veri kümesi tarafından desteklenen özelliklerin bir listesi sağlar. Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-datasets-linked-services.md)

Teradata'daki verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği `TeradataTable`' ye ayarlanmalıdır. | Evet |
| database | Teradata örneğinin adı. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |
| tablo | Teradata örneğindeki tablonun adı. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

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
> `RelationalTable`tür veri kümesi hala desteklenir. Ancak, yeni veri kümesini kullanmanızı öneririz.

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

Bu bölümde Teradata kaynağı tarafından desteklenen özelliklerin bir listesini sağlar. Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md) 

### <a name="teradata-as-source"></a>Kaynak olarak Teradata

>[!TIP]
>Veri bölümleme kullanarak Teradata'dan veri yüklemek için [Teradata bölümünden Paralel kopyadan](#parallel-copy-from-teradata) daha fazla bilgi edinin.

Teradata'daki verileri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ' `TeradataSource`ne ayarlanmalıdır. | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir.<br>Bölümlenmiş yükü etkinleştirdiğinizde, sorgunuzda karşılık gelen yerleşik bölüm parametrelerini bağlamanız gerekir. Örneğin, [Teradata bölümünden Paralel kopyaya](#parallel-copy-from-teradata) bakın. | Hayır (veri kümesinde tablo belirtilirse) |
| partitionOptions | Teradata'dan veri yüklemek için kullanılan veri bölümleme seçeneklerini belirtir. <br>İzin değerleri şunlardır: **Yok** (varsayılan), **Karma** ve **DynamicRange**.<br>Bir bölüm seçeneği etkinleştirildiğinde (yani `None`değil), Teradata'dan aynı anda veri yüklemek [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) için paralellik derecesi kopyalama etkinliği ayarı tarafından denetlenir. | Hayır |
| partitionAyarlar | Veri bölümleme için ayarlar grubunu belirtin. <br>Bölüm seçeneği olmadığında `None`uygulayın. | Hayır |
| partitionColumnName | Paralel kopya için aralık bölümü veya Karma bölüm tarafından kullanılacak kaynak sütunun adını belirtin. Belirtilmemişse, tablonun birincil dizini otomatik olarak algılanır ve bölüm sütunu olarak kullanılır. <br>Bölüm seçeneği veya `Hash` `DynamicRange`' olduğunda uygulayın Kaynak verileri almak için bir sorgu `?AdfHashPartitionCondition` kullanırsanız, kanca veya `?AdfRangePartitionColumnName` WHERE yan tümcesi. [Teradata bölümünden Paralel kopyadaki](#parallel-copy-from-teradata) örneğe bakın. | Hayır |
| partitionUpperBound | Verileri kopyalamak için bölüm sütununun maksimum değeri. <br>Bölüm seçeneği . `DynamicRange` Kaynak verileri almak için sorgu `?AdfRangePartitionUpbound` kullanıyorsanız, WHERE yan tümcesini bağla. Örneğin, [Teradata bölümünden Paralel kopyaya](#parallel-copy-from-teradata) bakın. | Hayır |
| partitionLowerBound | Verileri kopyalamak için bölüm sütununun minimum değeri. <br>Bölüm seçeneği . `DynamicRange` Kaynak verileri almak için bir sorgu `?AdfRangePartitionLowbound` kullanıyorsanız, WHERE yan tümcesini bağla. Örneğin, [Teradata bölümünden Paralel kopyaya](#parallel-copy-from-teradata) bakın. | Hayır |

> [!NOTE]
>
> `RelationalSource`tür kopyalama kaynağı hala desteklenir, ancak Teradata'dan gelen yeni yerleşik paralel yükü desteklemez (bölüm seçenekleri). Ancak, yeni veri kümesini kullanmanızı öneririz.

**Örnek: bölüm olmadan temel bir sorgu kullanarak verileri kopyalama**

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

## <a name="parallel-copy-from-teradata"></a>Teradata'dan paralel kopya

Veri Fabrikası Teradata bağlayıcısı, Teradata'daki verileri paralel olarak kopyalamak için yerleşik veri bölümleme sağlar. Veri bölümleme seçeneklerini kopyalama etkinliğinin **Kaynak** tablosunda bulabilirsiniz.

![Bölüm seçeneklerinin ekran görüntüsü](./media/connector-teradata/connector-teradata-partition-options.png)

Bölümlenmiş kopyalamayı etkinleştirdiğinizde, Veri Fabrikası verileri bölümlere göre yüklemek için Teradata kaynağınıza paralel sorgular çalıştırAr. Paralel derece, kopyalama [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) etkinliği üzerindeki ayar tarafından denetlenir. Örneğin, dörde `parallelCopies` ayarlarsanız, Veri Fabrikası aynı anda belirttiğiniz bölüm seçeneğiniz ve ayarlarınızı temel alan dört sorgu oluşturur ve çalıştırZ ve her sorgu Teradata'nızdan verilerin bir kısmını alır.

Özellikle Teradata'nızdan büyük miktarda veri yüklediğinizde, veri bölümleme ile paralel kopyalamayı etkinleştirmeniz önerilir. Aşağıda farklı senaryolar için önerilen yapılandırmalar vardır. Verileri dosya tabanlı veri deposuna kopyalarken, bir klasöre birden çok dosya olarak yazmak (yalnızca klasör adını belirtin) yeniden komut verilir ve bu durumda performans tek bir dosyaya yazmaktan daha iyidir.

| Senaryo                                                     | Önerilen ayarlar                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Büyük tablodan tam yük.                                   | **Bölüm seçeneği**: Karma. <br><br/>Yürütme sırasında, Veri Fabrikası birincil dizin sütununa otomatik olarak algılar, buna karşı bir karma uygular ve verileri bölümlere göre kopyalar. |
| Özel bir sorgu kullanarak büyük miktarda veri yükleyin.                 | **Bölüm seçeneği**: Karma.<br>**Sorgu** `SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>`: .<br>**Partition column**: Karma bölüm uygulamak için kullanılan sütunu belirtin. Belirtilmemişse, Veri Fabrikası Teradata veri kümesinde belirttiğiniz tablonun PK sütununa otomatik olarak algılar.<br><br>Yürütme sırasında, Veri `?AdfHashPartitionCondition` Fabrikası karma bölüm mantığı ile değiştirir ve Teradata gönderir. |
| Aralık bölümleme için eşit olarak dağıtılmış değere sahip bir tamsayı sütununa sahip olarak özel bir sorgu kullanarak büyük miktarda veri yükleyin. | **Bölüm seçenekleri**: Dinamik aralık bölümü.<br>**Sorgu** `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`: .<br>**Partition column**: Verileri bölmek için kullanılan sütunu belirtin. Noter veri türüne sahip sütuna karşı bölümleyebilirsiniz.<br>**Bölüm üst sınır** ve **bölüm alt sınır**: Yalnızca alt ve üst aralık arasında veri almak için bölüm sütununa filtre yapmak isteyip istemediğinizi belirtin.<br><br>Yürütme sırasında, Veri `?AdfRangePartitionColumnName`Fabrikası `?AdfRangePartitionUpbound`, `?AdfRangePartitionLowbound` , ve her bölüm için gerçek sütun adı ve değer aralıkları ile değiştirir ve Teradata gönderir. <br>Örneğin, alt sınır 1 ve üst sınır 80 olarak ayarlanmış bölüm sütununuzun "ID" kümesi, paralel kopya 4 olarak ayarlanmışsa, Veri Fabrikası verileri 4 bölümle alır. Onların kimliklerini [1,20], [21, 40], [41, 60], ve [61, 80], arasındadır. |

**Örnek: karma bölümlü sorgu**

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

**Örnek: dinamik aralık bölümü ile sorgu**

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

## <a name="data-type-mapping-for-teradata"></a>Teradata için veri türü eşleme

Teradata'daki verileri kopyaladiğinizde, aşağıdaki eşlemeler uygulanır. Kopyalama etkinliğinin kaynak şemasını ve veri türünü lavaboyla nasıl eşlenebildiğini öğrenmek için Bkz. [Şema ve veri türü eşlemeleri.](copy-activity-schema-and-type-mapping.md)

| Teradata veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Bigint |Int64 |
| Blob |Bayt[] |
| Bayt |Bayt[] |
| Bayt |Int16 |
| Char |Dize |
| Clob |Dize |
| Tarih |DateTime |
| Ondalık |Ondalık |
| Çift |Çift |
| Grafik |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Tamsayı |Int32 |
| Interval Gün |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Gün-Saat Aralığı |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Dakikadan Dakikaya Aralık |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Interval Gün to Second |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Aralık Saati |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Dakikaya Aralık Saati |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Interval Hour To Second |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Aralık Dakika |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Dakikadan Saniyeye Aralık |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Aralık Ayı |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| İkinci Aralık |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Aralık Yılı |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Yıl başından Aya Aralık |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Sayı |Çift |
| Dönem (Tarih) |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Dönem (Zaman) |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Dönem (Saat Dilimi ile Saat Dilimi) |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Dönem (Zaman Damgası) |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Dönem (Saat Dilimi ile Zaman Damgası) |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Smallint |Int16 |
| Zaman |TimeSpan |
| Saat Dilimi ile Saat Dilimi |TimeSpan |
| Zaman damgası |DateTime |
| Saat Dilimi li Zaman Damgası |DateTime |
| VarBayt |Bayt[] |
| Varchar |Dize |
| Vargraphıc |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |
| Xml |Desteklenmiyor. Kaynak sorgusunda açık döküm uygulayın. |


## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi [için](copy-activity-overview.md#supported-data-stores-and-formats)bkz.
