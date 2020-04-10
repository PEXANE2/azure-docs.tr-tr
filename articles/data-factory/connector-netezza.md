---
title: Azure Veri Fabrikası'nı kullanarak Verileri Netezza'dan kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak Netezza'dan desteklenen lavabo veri depolarına verileri nasıl kopyalayacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: jingwang
ms.openlocfilehash: 69e2a0fe63be65a2b5d51f7bd2e0885fcbfc5bbb
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991680"
---
# <a name="copy-data-from-netezza-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Verileri Netezza'dan kopyalama

Bu makalede, Netezza'daki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Makale, Azure [Veri Fabrikası'ndaki Kopyalama Etkinliği'ne](copy-activity-overview.md)dayanmaktadır ve bu da Kopyalama Etkinliğine genel bir genel bakış sunar.

>[!TIP]
>Netezza'dan Azure'a veri geçişi senaryosu için, [şirket içi Netezza sunucusundan Azure'a veri aktarılabilmek için Azure Veri Fabrikası'nı kullanın'dan](data-migration-guidance-netezza-azure-sqldw.md)daha fazla bilgi edinin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Netezza konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)


Verileri Netezza'dan desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama Etkinliği'nin kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına ve biçimlere](copy-activity-overview.md#supported-data-stores-and-formats)bakın.

Netezza konektörü kaynaktan paralel kopyalamayı destekler. Ayrıntılar için [Netezza bölümünden Paralel kopyaya](#parallel-copy-from-netezza) bakın.

Azure Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar. Bu bağlayıcıyı kullanmak için herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>başlarken

.NET SDK, Python SDK, Azure PowerShell, REST API veya Azure Kaynak Yöneticisi şablonunu kullanarak kopyalama etkinliğini kullanan bir ardışık kaynak oluşturabilirsiniz. Kopyalama etkinliği olan bir ardışık yapının nasıl oluşturulacağına ilişkin adım adım yönergeler için [Kopyalama Etkinliği öğreticisine](quickstart-create-data-factory-dot-net.md) bakın.

Aşağıdaki bölümlerde, Netezza bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanabileceğiniz özellikler hakkında ayrıntılı bilgi verilmiştir.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Netezza bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Tür** özelliği **Netezza**olarak ayarlanmalıdır. | Evet |
| Connectionstring | Netezza'ya bağlanmak için bir ODBC bağlantı dizesi. <br/>Parolayı Azure Key Vault'a koyabilir `pwd` ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Azure Key Vault [makalesinde](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren aşağıdaki örneklere ve Mağaza kimlik bilgilerine bakın. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresi kullanılır. |Hayır |

Tipik bir bağlantı `Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>`dizesi. Aşağıdaki tabloda ayarlayabileceğiniz daha fazla özellik açıklanmaktadır:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| Güvenlik Düzeyi | Sürücünün veri deposuna bağlantı için kullandığı güvenlik düzeyi (SSL/TLS). Örnek: `SecurityLevel=preferredSecured`. Desteklenen değerler şunlardır:<br/>- **Yalnızca güvenli olmayan** (**onlyUnSecured**): Sürücü TLS kullanmaz.<br/>- **Tercih edilen güvenli olmayan (tercihsiz Güvenli değil) (varsayılan)**: Sunucu bir seçenek sunuyorsa, sürücü TLS kullanmaz. <br/>- **Tercih edilen güvenli (preferredSecured)**: Sunucu bir seçenek sunuyorsa, sürücü TLS kullanır. <br/>- **Yalnızca güvenli (yalnızca Güvenli)**: TLS bağlantısı olmadığı sürece sürücü bağlanmaz. | Hayır |
| CaCertFile | Sunucu tarafından kullanılan TLS/SSL sertifikasına tam yol. Örnek: `CaCertFile=<cert path>;`| Evet, TLS etkinse |

**Örnek**

```json
{
    "name": "NetezzaLinkedService",
    "properties": {
        "type": "Netezza",
        "typeProperties": {
            "connectionString": "Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: parolayı Azure Key Vault'ta depolama**

```json
{
    "name": "NetezzaLinkedService",
    "properties": {
        "type": "Netezza",
        "typeProperties": {
            "connectionString": "Server=<server>;Port=<port>;Database=<database>;UID=<user name>;",
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

Bu bölümde, Netezza veri kümesinin desteklediği özelliklerin bir listesi yer almaktadır.

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-datasets-linked-services.md)

Netezza'dan gelen verileri kopyalamak için, veri kümesinin **tür** özelliğini **NetezzaTable**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği ayarlanmalıdır: **NetezzaTable** | Evet |
| Şema | Şema adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tablo | Masanın adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tableName | Şema ile tablonun adı. Bu özellik geriye dönük uyumluluk için desteklenir. Kullanın `schema` `table` ve yeni iş yükü için. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

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

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama

Bu bölümde, Netezza kaynağının desteklediği özelliklerin bir listesi sağlar.

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md)

### <a name="netezza-as-source"></a>Kaynak olarak Netezza

>[!TIP]
>Veri bölümleme kullanarak Netezza'dan veri yüklemek için, [Netezza bölümünden Paralel kopyadan](#parallel-copy-from-netezza) daha fazla bilgi edinin.

Netezza'dan gelen verileri kopyalamak **için,** Kopyalama Etkinliği'ndeki kaynak türünü **NetezzaSource**olarak ayarlayın. Aşağıdaki özellikler Kopyalama Etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama Etkinliği kaynağının **türü** özelliği **NetezzaSource**olarak ayarlanmalıdır. | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örnek: `"SELECT * FROM MyTable"` | Hayır (veri kümesinde "tablo Adı" belirtilirse) |
| partitionOptions | Netezza'dan veri yüklemek için kullanılan veri bölümleme seçeneklerini belirtir. <br>İzin değerleri şunlardır: **Yok** (varsayılan), **DataSlice**ve **DynamicRange**.<br>Bir bölme seçeneği etkinleştirildiğinde (yani `None`değil), Netezza veritabanından aynı anda veri yüklemek [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) için paralellik derecesi kopyalama etkinliği ayarlayarak denetlenir. | Hayır |
| partitionAyarlar | Veri bölümleme için ayarlar grubunu belirtin. <br>Bölüm seçeneği olmadığında `None`uygulayın. | Hayır |
| partitionColumnName | Paralel kopya için aralık bölümleme tarafından kullanılacak **tamsayı türünde** kaynak sütunun adını belirtin. Belirtilmemişse, tablonun birincil anahtarı otomatik olarak algılanır ve bölüm sütunu olarak kullanılır. <br>Bölüm seçeneği . `DynamicRange` Kaynak verileri almak için bir sorgu `?AdfRangePartitionColumnName` kullanıyorsanız, WHERE yan tümcesini bağla. [Netezza bölümünden Paralel kopyaörneğine](#parallel-copy-from-netezza) bakın. | Hayır |
| partitionUpperBound | Verileri kopyalamak için bölüm sütununun maksimum değeri. <br>Bölüm seçeneği . `DynamicRange` Kaynak verileri almak için sorgu `?AdfRangePartitionUpbound` kullanıyorsanız, WHERE yan tümcesini bağla. Örneğin, [Netezza bölümünden Paralel kopyaya](#parallel-copy-from-netezza) bakın. | Hayır |
| partitionLowerBound | Verileri kopyalamak için bölüm sütununun minimum değeri. <br>Bölüm seçeneği . `DynamicRange` Kaynak verileri almak için bir sorgu `?AdfRangePartitionLowbound` kullanıyorsanız, WHERE yan tümcesini bağla. Örneğin, [Netezza bölümünden Paralel kopyaya](#parallel-copy-from-netezza) bakın. | Hayır |

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

## <a name="parallel-copy-from-netezza"></a>Netezza'dan paralel kopya

Veri Fabrikası Netezza konektörü, Netezza'dan gelen verileri paralel olarak kopyalamak için yerleşik veri bölümleme sağlar. Veri bölümleme seçeneklerini kopyalama etkinliğinin **Kaynak** tablosunda bulabilirsiniz.

![Bölüm seçeneklerinin ekran görüntüsü](./media/connector-netezza/connector-netezza-partition-options.png)

Bölümlenmiş kopyalamayı etkinleştirdiğinizde, Veri Fabrikası, verileri bölümlere yüklemek için Netezza kaynağınıza paralel sorgular çalıştırAr. Paralel derece, kopyalama [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) etkinliği üzerindeki ayar tarafından denetlenir. Örneğin, dörde `parallelCopies` ayarlarsanız, Veri Fabrikası aynı anda belirttiğiniz bölüm seçeneğiniz ve ayarlarınızı temel alarak dört sorgu oluşturur ve çalıştırZ ve her sorgu Netezza veritabanınızdan verilerin bir kısmını alır.

Özellikle Netezza veritabanınızdan büyük miktarda veri yüklediğinizde, veri bölümleme ile paralel kopyalamayı etkinleştirmeniz önerilir. Aşağıda farklı senaryolar için önerilen yapılandırmalar vardır. Verileri dosya tabanlı veri deposuna kopyalarken, bir klasöre birden çok dosya olarak yazmak (yalnızca klasör adını belirtin) yeniden komut verilir ve bu durumda performans tek bir dosyaya yazmaktan daha iyidir.

| Senaryo                                                     | Önerilen ayarlar                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Büyük tablodan tam yük.                                   | **Bölüm leme seçeneği**: Veri Dilimi. <br><br/>Yürütme sırasında Veri Fabrikası, [Verileri Netezza'nın yerleşik veri dilimlerine](https://www.ibm.com/support/knowledgecenter/en/SSULQD_7.2.1/com.ibm.nz.adm.doc/c_sysadm_data_slices_parts_disks.html)göre otomatik olarak bölümlere ayırır ve verileri bölümlere kopyalar. |
| Özel bir sorgu kullanarak büyük miktarda veri yükleyin.                 | **Bölüm leme seçeneği**: Veri Dilimi.<br>**Sorgu** `SELECT * FROM <TABLENAME> WHERE mod(datasliceid, ?AdfPartitionCount) = ?AdfDataSliceCondition AND <your_additional_where_clause>`: .<br>Yürütme sırasında, Veri `?AdfPartitionCount` Fabrikası (kopyalama etkinliğinde ayarlanmış paralel `?AdfDataSliceCondition` kopya numarasıyla) ve veri dilimi bölümleme mantığıyla değiştirilir ve Netezza'ya gönderir. |
| Aralık bölümleme için eşit olarak dağıtılmış değere sahip bir tamsayı sütununa sahip olarak özel bir sorgu kullanarak büyük miktarda veri yükleyin. | **Bölüm seçenekleri**: Dinamik aralık bölümü.<br>**Sorgu** `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`: .<br>**Partition column**: Verileri bölmek için kullanılan sütunu belirtin. Noter veri türüne sahip sütuna karşı bölümleyebilirsiniz.<br>**Bölüm üst sınır** ve **bölüm alt sınır**: Yalnızca alt ve üst aralık arasında veri almak için bölüm sütununa filtre yapmak isteyip istemediğinizi belirtin.<br><br>Yürütme sırasında, Veri `?AdfRangePartitionColumnName`Fabrikası `?AdfRangePartitionUpbound`, `?AdfRangePartitionLowbound` , ve her bölüm için gerçek sütun adı ve değer aralıkları ile değiştirir ve Netezza gönderir. <br>Örneğin, alt sınır 1 ve üst sınır 80 olarak ayarlanmış bölüm sütununuzun "ID" kümesi, paralel kopya 4 olarak ayarlanmışsa, Veri Fabrikası verileri 4 bölümle alır. Onların kimliklerini [1,20], [21, 40], [41, 60], ve [61, 80], arasındadır. |

**Örnek: veri dilimi bölümü ile sorgu**

```json
"source": {
    "type": "NetezzaSource",
    "query": "SELECT * FROM <TABLENAME> WHERE mod(datasliceid, ?AdfPartitionCount) = ?AdfDataSliceCondition AND <your_additional_where_clause>",
    "partitionOption": "DataSlice"
}
```

**Örnek: dinamik aralık bölümü ile sorgu**

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

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası'nda Copy Activity'in kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına bakın.](copy-activity-overview.md#supported-data-stores-and-formats)
