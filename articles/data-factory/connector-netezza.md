---
title: Azure Data Factory kullanarak Netezza'dan verileri kopyalama | Microsoft Docs
description: Desteklenen bir havuz veri depolarına Netezza'dan bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: jingwang
ms.openlocfilehash: 5d0c09c9cff2fefcc2eee20b9fd2f93dd375115f
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71090011"
---
# <a name="copy-data-from-netezza-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Netezza'dan verileri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de Netezza'dan verileri kopyalamak için nasıl kullanılacağını özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

>[!TIP]
>Netezza 'ten Azure 'a veri geçiş senaryosu için, Şirket [Içi Netezza Server 'Dan Azure 'a veri geçirmek üzere Azure Data Factory kullanma](data-migration-guidance-netezza-azure-sqldw.md)hakkında daha fazla bilgi edinin.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Netezza Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)


Tüm desteklenen havuz veri deposuna Netezza'dan verileri kopyalayabilirsiniz. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Netezza Bağlayıcısı kaynaktan paralel kopyalamayı destekler. Ayrıntılar için [Netezza adresinden paralel kopyalama](#parallel-copy-from-netezza) bölümüne bakın.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar. Bu bağlayıcıyı kullanmak için herhangi bir sürücüsü el ile yüklemeniz gerekmez.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>başlarken

.NET SDK, Python SDK'sı, Azure PowerShell, REST API veya bir Azure Resource Manager şablonu kullanarak kopyalama etkinliği kullanan bir işlem hattı oluşturabilirsiniz. Bkz: [kopyalama etkinliği Öğreticisi](quickstart-create-data-factory-dot-net.md) kopyalama etkinliği içeren işlem hattı oluşturma konusunda adım adım yönergeler için.

Aşağıdaki bölümler Netezza Bağlayıcısı özel olan Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler Netezza bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** özelliği ayarlanmalıdır **Netezza**. | Evet |
| connectionString | Netezza'ya bağlanma bir ODBC bağlantı dizesi. <br/>Bu alanı, Data Factory güvenli bir şekilde depolamak için SecureString olarak işaretleyin. Ayrıca, Azure Key Vault parolayı yerleştirebilir ve `pwd` yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Daha ayrıntılı bilgi için aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault makalesine depolayın](store-credentials-in-key-vault.md) . | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

Bir bağlantı dizesi olan `Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>`. Aşağıdaki tabloda ayarlayabilirsiniz daha fazla özellik açıklanmaktadır:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| SecurityLevel | Veri deposuna bağlantı için sürücüyü kullanır (SSL/TLS) güvenlik düzeyi. Örnek: `SecurityLevel=preferredSecured`. Desteklenen değerler şunlardır:<br/>- **Yalnızca güvenli olmayan** (**yalnızca güvenli olmayan**): Sürücü SSL kullanmaz.<br/>- **Tercih edilen güvenli olmayan (Preferredgüvensiz) (varsayılan)** : Sunucu bir seçenek sağlıyorsa, sürücü SSL kullanmaz. <br/>- **Tercih edilen güvenli (Preferredgüvenli)** : Sunucu bir seçenek sağlıyorsa, sürücü SSL kullanır. <br/>- **Yalnızca güvenli (yalnızca güvenli)** : SSL bağlantısı yoksa sürücü bağlanamaz. | Hayır |
| CaCertFile | Sunucu tarafından kullanılan SSL sertifikasının tam yolu. Örnek: `CaCertFile=<cert path>;`| SSL etkinleştirilmişse, Evet |

**Örnek**

```json
{
    "name": "NetezzaLinkedService",
    "properties": {
        "type": "Netezza",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Azure Key Vault parola depola**

```json
{
    "name": "NetezzaLinkedService",
    "properties": {
        "type": "Netezza",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>;Port=<port>;Database=<database>;UID=<user name>;"
            },
            "pwd": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
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

Bu bölümde Netezza veri kümesini destekleyen özelliklerin bir listesini sağlar.

Bölümleri ve veri kümeleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md).

Netezza'dan verileri kopyalamak için ayarlanmış **türü** veri kümesine özelliği **NetezzaTable**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği şu şekilde ayarlanmalıdır: **NetezzaTable** | Evet |
| schema | Şemanın adı. |Hayır (etkinlik kaynağı "query" belirtilmişse)  |
| table | Tablonun adı. |Hayır (etkinlik kaynağı "query" belirtilmişse)  |
| tableName | Şemanın bulunduğu tablonun adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni `schema` iş `table` yükü için ve kullanın. | Hayır (etkinlik kaynağı "query" belirtilmişse) |

**Örnek**

```json
{
    "name": "NetezzaDataset",
    "properties": {
        "type": "NetezzaTable",
        "linkedServiceName": {
            "referenceName": "<Netezza linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölümde Netezza kaynağının desteklediği özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md).

### <a name="netezza-as-source"></a>Kaynak olarak Netezza

>[!TIP]
>Veri bölümlemesini kullanarak Netezza 'tan verileri verimli bir şekilde yüklemek için, [Netezza bölümünden paralel kopyadan](#parallel-copy-from-netezza) daha fazla bilgi edinin.

Netezza'dan verileri kopyalamak için ayarlanmış **kaynak** türü için kopyalama etkinliğindeki **NetezzaSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Türü** kopyalama etkinliği kaynak özelliği ayarlanmalıdır **NetezzaSource**. | Evet |
| query | Verileri okumak için özel bir SQL sorgusu kullanın. Örnek: `"SELECT * FROM MyTable"` | Yok (veri kümesinde "tableName" değeri belirtilmişse) |
| partitionOptions | Netezza 'ten veri yüklemek için kullanılan veri bölümleme seçeneklerini belirtir. <br>İzin verme değerleri şunlardır: **Hiçbiri** (varsayılan), **Dataslice**ve **DynamicRange**.<br>Bir bölüm seçeneği etkinleştirildiğinde (yani, `None`), bir Netezza veritabanından eşzamanlı olarak veri yükleme derecesi, kopyalama etkinliğinde [`parallelCopies`](copy-activity-performance.md#parallel-copy) ayarlanarak denetlenir. | Hayır |
| partitionSettings | Veri bölümleme için ayarların grubunu belirtin. <br>Bölüm seçeneği `None`olmadığında Uygula. | Hayır |
| partitionColumnName | Paralel kopya için Aralık bölümleme tarafından kullanılacak, **tamsayı türünde** kaynak sütunun adını belirtin. Belirtilmemişse, tablonun birincil anahtarı oto algılanır ve bölüm sütunu olarak kullanılır. <br>Bölüm seçeneği `DynamicRange`olduğunda uygulayın. Kaynak verileri almak için bir sorgu kullanırsanız, WHERE yan tümcesinde kanca `?AdfRangePartitionColumnName` . [Netezza from the Parallel Copy](#parallel-copy-from-netezza) bölümüne bakın. | Hayır |
| partitionüstsınırı | Verilerin kopyalanacağı bölüm sütununun en büyük değeri. <br>Bölüm seçeneği `DynamicRange`olduğunda Uygula. Kaynak verileri almak için sorgu kullanırsanız WHERE yan tümcesinde kanca `?AdfRangePartitionUpbound` . Bir örnek için, [Netezza öğesinden paralel kopyalama](#parallel-copy-from-netezza) bölümüne bakın. | Hayır |
| Partitionalme sınırı | Verilerin kopyalanacağı bölüm sütununun en küçük değeri. <br>Bölüm seçeneği `DynamicRange`olduğunda uygulayın. Kaynak verileri almak için bir sorgu kullanırsanız, WHERE yan tümcesinde kanca `?AdfRangePartitionLowbound` . Bir örnek için, [Netezza öğesinden paralel kopyalama](#parallel-copy-from-netezza) bölümüne bakın. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromNetezza",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Netezza input dataset name>",
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
                "type": "NetezzaSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="parallel-copy-from-netezza"></a>Netezza 'den paralel kopya

Data Factory Netezza Bağlayıcısı, verileri Netezza 'den paralel olarak kopyalamak için yerleşik veri bölümlendirme sağlar. Kopyalama etkinliğinin **kaynak** tablosunda veri bölümleme seçeneklerini bulabilirsiniz.

![Bölüm seçeneklerinin ekran görüntüsü](./media/connector-netezza/connector-netezza-partition-options.png)

Bölümlenmiş kopyayı etkinleştirdiğinizde Data Factory, verileri bölümlere göre yüklemek için Netezza kaynağınıza paralel sorgular çalıştırır. Paralel derece kopyalama etkinliğindeki [`parallelCopies`](copy-activity-performance.md#parallel-copy) ayar tarafından denetlenir. Örneğin, dört olarak ayarlarsanız `parallelCopies` , Data Factory aynı anda, belirtilen bölüm seçeneğiniz ve ayarlarınıza göre dört sorgu üretir ve çalışır ve her sorgu, Netezza veritabanınızdaki verilerin bir kısmını alır.

Netezza veritabanından büyük miktarda veri yüklediğinizde, özellikle veri bölümleme ile paralel kopyayı etkinleştirmeniz önerilir. Farklı senaryolar için önerilen yapılandırma aşağıda verilmiştir. Dosya tabanlı veri deposuna veri kopyalarken, bir klasöre birden çok dosya (yalnızca klasör adını belirt) olarak yazma işlemi geri çağrılır, bu durumda performans tek bir dosyaya yazılmasından daha iyidir.

| Senaryo                                                     | Önerilen ayarlar                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Büyük tablodan tam yük.                                   | **Bölüm seçeneği**: Veri dilimi. <br><br/>Yürütme sırasında, verileri [Netezza 'in yerleşik veri dilimlerine](https://www.ibm.com/support/knowledgecenter/en/SSULQD_7.2.1/com.ibm.nz.adm.doc/c_sysadm_data_slices_parts_disks.html)göre otomatik olarak bölümlere ayırarak Data Factory ve verileri bölümlere göre kopyalar. |
| Özel bir sorgu kullanarak büyük miktarda veriyi yükleyin.                 | **Bölüm seçeneği**: Veri dilimi.<br>**Sorgu**: `SELECT * FROM <TABLENAME> WHERE mod(datasliceid, ?AdfPartitionCount) = ?AdfDataSliceCondition AND <your_additional_where_clause>`.<br>Yürütme sırasında, data factory `?AdfPartitionCount` (kopyalama etkinliğinde paralel kopyalama numarası kümesiyle) ve `?AdfDataSliceCondition` veri dilimi bölümü mantığı ile, ve Netezza ' e gönderir. |
| Aralık bölümlendirme için eşit olarak dağıtılmış değere sahip bir tamsayı sütunu olan özel bir sorgu kullanarak büyük miktarda veri yükleyin. | **Bölüm seçenekleri**: Dinamik Aralık bölümü.<br>**Sorgu**: `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Bölüm sütunu**: Verileri bölümlemek için kullanılan sütunu belirtin. Tamsayı veri türünde bir sütuna göre bölümleyebilirsiniz.<br>**Bölüm üst sınırı** ve **bölüm alt sınırı**: Yalnızca alt ve üst Aralık arasında veri almak için bölüm sütununa karşı filtreleme yapmak istediğinizi belirtin.<br><br>Yürütme sırasında Data Factory, `?AdfRangePartitionColumnName` `?AdfRangePartitionUpbound`,, ve `?AdfRangePartitionLowbound` her bölüm için gerçek sütun adı ve değer aralıklarıyla değiştirilir ve Netezza ' e gönderir. <br>Örneğin, "ID" adlı bölüm sütununuzu alt sınır 1 ve üst sınır 80 olarak ayarlarsanız, paralel kopya 4 olarak ayarlandıysa Data Factory verileri 4 bölümden alır. Kimlikleri sırasıyla [1, 20], [21, 40], [41, 60] ve [61, 80] arasındadır. |

**Örnek: veri dilimi bölümüyle sorgulama**

```json
"source": {
    "type": "NetezzaSource",
    "query": "SELECT * FROM <TABLENAME> WHERE mod(datasliceid, ?AdfPartitionCount) = ?AdfDataSliceCondition AND <your_additional_where_clause>",
    "partitionOption": "DataSlice"
}
```

**Örnek: Dinamik Aralık bölümü ile sorgulama**

```json
"source": {
    "type": "NetezzaSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<dynamic_range_partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column>",
        "partitionLowerBound": "<lower_value_of_partition_column>"
    }
}
```

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.


## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).
