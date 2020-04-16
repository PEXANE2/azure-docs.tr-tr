---
title: Azure Veri Fabrikası'nı kullanarak Cassandra'dan veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak Cassandra'daki verileri desteklenen lavabo veri depolarına nasıl kopyalaylayamamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: 4b7fd2de0762de147ad3ceae0d562a1c78b33dc2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417466"
---
# <a name="copy-data-from-cassandra-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Cassandra'dan veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-cassandra-connector.md)
> * [Geçerli sürüm](connector-cassandra.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Cassandra veritabanındaki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Cassandra konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Cassandra veritabanından desteklenen herhangi bir lavabo veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Cassandra konektörü destekler:

- Cassandra **sürümleri 2.x ve 3.x**.
- **Temel** veya **Anonim** kimlik doğrulamasını kullanarak verileri kopyalama.

>[!NOTE]
>Kendi kendine barındırılan Tümleştirme Runtime'da çalışan etkinlik için Cassandra 3.x, IR sürüm 3.7 ve üzeri sürümden beri desteklenir.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

Tümleştirme Runtime yerleşik bir Cassandra sürücüsü sağlar, bu nedenle cassandra/to Cassandra'dan veri kopyalarken herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Cassandra bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Cassandra bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Tür özelliği ayarlanmalıdır: **Cassandra** |Evet |
| konak |Cassandra sunucularının bir veya daha fazla IP adresi veya ana bilgisayar adları.<br/>Tüm sunuculara aynı anda bağlanmak için virgülle ayrılmış IP adresleri veya ana bilgisayar adlarının listesini belirtin. |Evet |
| port |Cassandra sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. |Hayır (varsayılan değer 9042'dir) |
| authenticationType | Cassandra veritabanına bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler şunlardır: **Temel**ve **Anonim**. |Evet |
| kullanıcı adı |Kullanıcı hesabı için kullanıcı adını belirtin. |Evet, kimlik doğrulamaTürü Temel olarak ayarlanmışsa. |
| password |Kullanıcı hesabı için parola belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) |Evet, kimlik doğrulamaTürü Temel olarak ayarlanmışsa. |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

>[!NOTE]
>Şu anda TLS kullanarak Cassandra bağlantısı desteklenmez.

**Örnek:**

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "Cassandra",
        "typeProperties": {
            "host": "<host>",
            "authenticationType": "Basic",
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde Cassandra dataset tarafından desteklenen özelliklerin bir listesini sağlar.

Cassandra'dan gelen verileri kopyalamak için, veri kümesinin tür özelliğini **CassandraTable**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **CassandraTable** | Evet |
| anahtar boşluğu |Cassandra veritabanındaki boşluk veya şema adı. |Hayır (CassandraSource için "sorgu" belirtilirse) |
| tableName |Cassandra veritabanındaki tablonun adı. |Hayır (CassandraSource için "sorgu" belirtilirse) |

**Örnek:**

```json
{
    "name": "CassandraDataset",
    "properties": {
        "type": "CassandraTable",
        "typeProperties": {
            "keySpace": "<keyspace name>",
            "tableName": "<table name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Cassandra linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri


Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Cassandra kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="cassandra-as-source"></a>Kaynak olarak Cassandra

Cassandra'dan gelen verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **CassandraSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **CassandraSource** | Evet |
| sorgu |Verileri okumak için özel sorguyu kullanın. SQL-92 sorgusu veya CQL sorgusu. [Bkz. CQL başvurusu.](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html) <br/><br/>SQL sorgusunu kullanırken, sorgulamak istediğiniz tabloyu temsil etmek için **keyspace name.table adını** belirtin. |Hayır (veri kümesinde "tablo Adı" ve "keyspace" belirtilirse). |
| tutarlılıkSeviye |Tutarlılık düzeyi, istemci uygulamasına veri döndürmeden önce okuma isteğine kaç yinelemenin yanıt vermesi gerektiğini belirtir. Cassandra, okundu isteğini karşılamak için veri için belirtilen çoğaltma sayısını denetler. Ayrıntılar için [veri tutarlılığını yapılandırma](https://docs.datastax.com/en/cassandra/2.1/cassandra/dml/dml_config_consistency_c.html) ya da yapılandırma ya bak.<br/><br/>İzin verilen değerler şunlardır: **Bİr**, **İkİ**, **ÜÇ**, **ÇOĞUNLUK**, **TÜM**, **LOCAL_QUORUM**, **EACH_QUORUM**, ve **LOCAL_ONE**. |Hayır (varsayılan) `ONE` |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromCassandra",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Cassandra input dataset name>",
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
                "type": "CassandraSource",
                "query": "select id, firstname, lastname from mykeyspace.mytable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-cassandra"></a>Cassandra için veri türü eşleme

Cassandra'dan veri kopyalanırken, Aşağıdaki eşlemeler Cassandra veri türlerinden Azure Veri Fabrikası geçici veri türlerine kadar kullanılır. Kopya etkinliği kaynak şemasını ve veri türünü lavaboyla nasıl eşler hakkında bilgi edinmek için Şema ve [veri türü eşlemelerine](copy-activity-schema-and-type-mapping.md) bakın.

| Cassandra veri türü | Veri fabrikası geçici veri türü |
|:--- |:--- |
| ASCII |Dize |
| Bigint |Int64 |
| Blob |Bayt[] |
| Boolean |Boole |
| On -da -lık |Ondalık |
| Çift |Çift |
| Float |Tek |
| ınet |Dize |
| INT |Int32 |
| TEXT |Dize |
| Zaman damgası |DateTime |
| TIMEUUID |Guid |
| Uuıd |Guid |
| Varchar |Dize |
| VARINT |Ondalık |

> [!NOTE]
> Toplama türleri (harita, set, liste, vb.) için, sanal tablo bölümünü [kullanarak Cassandra ile Çalışma toplama türlerine](#work-with-collections-using-virtual-table) bakın.
>
> Kullanıcı tanımlı türler desteklenmez.
>
> İkili Sütun ve String Sütun uzunluklarının uzunluğu 4000'den büyük olamaz.
>

## <a name="work-with-collections-using-virtual-table"></a>Sanal tabloyu kullanarak koleksiyonlarla çalışma

Azure Veri Fabrikası, Cassandra veritabanınızdaki verilere bağlanmak ve kopyalamak için yerleşik bir ODBC sürücüsü kullanır. Harita, ayar ve liste gibi toplama türleri için sürücü verileri karşılık gelen sanal tablolara yeniden normalleştirir. Özellikle, bir tablo herhangi bir koleksiyon sütunu içeriyorsa, sürücü aşağıdaki sanal tabloları oluşturur:

* Toplama sütunları dışında gerçek tabloyla aynı verileri içeren **bir taban tablo.** Taban tablo, temsil ettiği gerçek tabloyla aynı adı kullanır.
* İç içe geçen verileri genişleten her koleksiyon sütunu için sanal bir **tablo.** Koleksiyonları temsil eden sanal tablolar, gerçek tablonun adı, ayırıcı "*vt*" ve sütunun adı kullanılarak adlandırılır.

Sanal tablolar, sürücünün normalden arındırılmış verilere erişmesini sağlayarak gerçek tablodaki verilere başvurur. Ayrıntılar için Örnek bölümüne bakın. Cassandra koleksiyonlarının içeriğine sanal tabloları sorgulayarak ve katılarak erişebilirsiniz.

### <a name="example"></a>Örnek

Örneğin, aşağıdaki "ExampleTable", "pk_int" adlı bir tamsayı birincil anahtar sütunu, değer adlı bir metin sütunu, liste sütunu, bir harita sütunu ve set sütunu ("StringSet" olarak adlandırılır) içeren bir Cassandra veritabanı tablosudur.

| pk_int | Değer | Liste | Eşleme | StringSet |
| --- | --- | --- | --- | --- |
| 1 |"örnek değer 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"örnek değer 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Sürücü, bu tek tabloyu temsil edecek birden çok sanal tablo oluşturur. Sanal tablolardaki yabancı anahtar sütunları, gerçek tablodaki birincil anahtar sütunlarına başvurur ve sanal tablo satırının hangi gerçek tablo satırına karşılık olduğunu gösterir.

İlk sanal tablo aşağıdaki tabloda gösterilen "ExampleTable" adlı taban tablosudur: 

| pk_int | Değer |
| --- | --- |
| 1 |"örnek değer 1" |
| 3 |"örnek değer 3" |

Temel tablo, bu tablodan atlanan ve diğer sanal tablolarda genişletilen koleksiyonlar dışında özgün veritabanı tablosuyla aynı verileri içerir.

Aşağıdaki tablolar, Liste, Harita ve StringSet sütunlarından verileri yeniden normalleştiren sanal tabloları gösterir. "_index" veya "_key" ile biten adlara sahip sütunlar, orijinal liste veya haritadaki verilerin konumunu gösterir. "_value" ile biten adlara sahip sütunlar, koleksiyondaki genişletilmiş verileri içerir.

**Tablo "ExampleTable_vt_List":**

| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1 |
| 1 |1 |2 |
| 1 |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

**Tablo "ExampleTable_vt_Map":**

| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |A |
| 1 |S2 |b |
| 3 |S1 |t |

**Tablo "ExampleTable_vt_StringSet":**

| pk_int | StringSet_value |
| --- | --- |
| 1 |A |
| 1 |B |
| 1 |C |
| 3 |A |
| 3 |E |

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
