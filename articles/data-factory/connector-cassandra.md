---
title: Azure Data Factory kullanarak Cassandra 'dan veri kopyalama | Microsoft Docs
description: Azure Data Factory işlem hattındaki kopyalama etkinliğini kullanarak Cassandra 'dan desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: jingwang
ms.openlocfilehash: d0e8881607fe4dc84a7d533855dc2b9c48e5366d
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68726178"
---
# <a name="copy-data-from-cassandra-using-azure-data-factory"></a>Azure Data Factory kullanarak Cassandra 'dan veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-cassandra-connector.md)
> * [Geçerli sürüm](connector-cassandra.md)

Bu makalede, Cassandra veritabanından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Cassandra veritabanından desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu Cassandra Bağlayıcısı şunları destekler:

- Cassandra **sürümleri 2. x ve 3. x**.
- **Temel** veya **anonim** kimlik doğrulaması kullanarak verileri kopyalama.

>[!NOTE]
>Şirket içinde barındırılan Integration Runtime çalışan etkinlik için Cassandra 3. x, IR sürüm 3,7 ve üzeri sürümlerde desteklenir.

## <a name="prerequisites"></a>Önkoşullar

Herkese açık olmayan bir Cassandra veritabanından veri kopyalamak için, kendinden konak Integration Runtime ayarlamanız gerekir. Ayrıntıları öğrenmek için [Şirket içinde barındırılan Integration Runtime](create-self-hosted-integration-runtime.md) makalesine bakın. Integration Runtime, yerleşik bir Cassandra sürücüsü sağlar. bu nedenle,/sürümünden Cassandra 'dan veri kopyalarken herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Cassandra bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Cassandra bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü |Type özelliği şu şekilde ayarlanmalıdır: **Cassandra** |Evet |
| host |Cassandra sunucularının bir veya daha fazla IP adresi veya ana bilgisayar adı.<br/>Aynı anda tüm sunuculara bağlanmak için IP adreslerinin veya ana bilgisayar adlarının virgülle ayrılmış bir listesini belirtin. |Evet |
| port |Cassandra sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. |Hayır (varsayılan değer 9042) |
| authenticationType | Cassandra veritabanına bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değerler şunlardır: **Temel**ve **anonim**. |Evet |
| username |Kullanıcı hesabı için Kullanıcı adını belirtin. |Evet, authenticationType temel olarak ayarlandıysa. |
| password |Kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet, authenticationType temel olarak ayarlandıysa. |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz genel olarak erişilebilir değilse), şirket içinde barındırılan tümleştirme çalışma zamanı veya Azure Integration Runtime kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>Şu anda Cassandra ile SSL kullanarak bağlantı desteklenmiyor.

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölüm, Cassandra veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Cassandra 'dan veri kopyalamak için veri kümesinin Type özelliğini **Cassandratable**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Veri kümesinin Type özelliği şu şekilde ayarlanmalıdır: **CassandraTable** | Evet |
| keySpace |Cassandra veritabanında anahtar alanının veya şemanın adı. |Hayır ("CassandraSource" için "sorgu" belirtilmişse) |
| tableName |Cassandra veritabanındaki tablonun adı. |Hayır ("CassandraSource" için "sorgu" belirtilmişse) |

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


Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Cassandra kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="cassandra-as-source"></a>Kaynak olarak Cassandra

Cassandra 'dan veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **Cassandrasource**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Kopyalama etkinliği kaynağının Type özelliği şu şekilde ayarlanmalıdır: **CassandraSource** | Evet |
| query |Verileri okumak için özel sorguyu kullanın. SQL-92 sorgusu veya CQL sorgusu. Bkz. [CQL başvurusu](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>SQL sorgusu kullanırken, sorgulamak istediğiniz tabloyu temsil etmek için **anahtar alanı Name. Table adını** belirtin. |Hayır (veri kümesinde "tableName" ve "keyspace" belirtilmişse). |
| Bu düzey |Tutarlılık düzeyi, istemci uygulamasına veri döndürmeden önce bir okuma isteğine kaç yinelemenin yanıt vereceğini belirtir. Cassandra, okuma isteğini karşılamak üzere verilerin belirtilen sayıda çoğaltmasını denetler. Ayrıntılar için bkz. [veri tutarlılığını yapılandırma](https://docs.datastax.com/en/cassandra/2.1/cassandra/dml/dml_config_consistency_c.html) .<br/><br/>İzin verilen değerler şunlardır: **One**, **iki**, **üç**, **çekirdek**, **All**, **LOCAL_QUORUM**, **EACH_QUORUM**ve **LOCAL_ONE**. |Hayır (varsayılan değer `ONE`) |

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

## <a name="data-type-mapping-for-cassandra"></a>Cassandra için veri türü eşlemesi

Cassandra 'dan veri kopyalarken, Cassandra veri türlerinden aşağıdaki eşlemeler, geçici veri türleri Azure Data Factory için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| Cassandra veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| ASCII |Dize |
| BIGINT |Int64 |
| BUN |Byte[] |
| BOOLEAN |Boole değeri |
| KATEGORI |Decimal |
| DOUBLE |Double |
| FLOAT |Single |
| INET |Dize |
| INT |Int32 |
| TEXT |Dize |
| TIMESTAMP |Datetime |
| TIMEUUID |Guid |
| EDIN |Guid |
| VARCHAR |Dize |
| VARINT |Decimal |

> [!NOTE]
> Koleksiyon türleri için (eşleme, küme, liste, vb.), [sanal tablo kullanarak Cassandra koleksiyon türleriyle çalışma](#work-with-collections-using-virtual-table) bölümüne bakın.
>
> Kullanıcı tanımlı türler desteklenmez.
>
> Ikili sütun ve dize sütun uzunluklarının uzunluğu 4000 ' ten büyük olamaz.
>

## <a name="work-with-collections-using-virtual-table"></a>Sanal tablo kullanarak koleksiyonlarla çalışma

Azure Data Factory, Cassandra veritabanınıza bağlanmak ve veri kopyalamak için yerleşik bir ODBC sürücüsü kullanır. Eşleme, küme ve liste gibi koleksiyon türleri için, sürücü verileri ilgili sanal tablolara yeniden ayırır. Özellikle, bir tablo koleksiyon sütunları içeriyorsa, sürücü aşağıdaki sanal tabloları oluşturur:

* Koleksiyon sütunları hariç gerçek tabloyla aynı verileri içeren bir **temel tablo**. Temel tablo, temsil ettiği gerçek tabloyla aynı adı kullanır.
* İç içe geçmiş verileri genişleten her koleksiyon sütunu için bir **sanal tablo** . Koleksiyonları temsil eden sanal tablolar, gerçek tablonun adı, bir ayırıcı "*VT*" ve sütunun adı kullanılarak adlandırılır.

Sanal tablolar, gerçek tablodaki verilere başvurur ve bu da sürücünün, verilerin yoğun verilere erişmesine olanak tanır. Ayrıntılar için örnek bölümüne bakın. Cassandra koleksiyonlarının içeriğine, sanal tabloları sorgulayarak ve birleştirerek erişebilirsiniz.

### <a name="example"></a>Örnek

Örneğin, aşağıdaki "ExampleTable", "pk_int" adlı bir tamsayı birincil anahtar sütunu, value adlı bir metin sütunu, bir liste sütunu, bir harita sütunu ve bir küme sütunu ("StringSet" adlı) içeren bir Cassandra veritabanı tablosudur.

| pk_int | Value | List | Eşleme | StringSet |
| --- | --- | --- | --- | --- |
| 1\. |"örnek değer 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"örnek değeri 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Bu tek tabloyu temsil eden sürücü birden çok sanal tablo oluşturur. Sanal tablolardaki yabancı anahtar sütunları, gerçek tablodaki birincil anahtar sütunlarına başvurur ve sanal tablo satırının hangi gerçek tablo satırına karşılık geldiğini belirtir.

İlk sanal tablo, "ExampleTable" adlı temel tablodur ve aşağıdaki tabloda gösterilmiştir: 

| pk_int | Value |
| --- | --- |
| 1\. |"örnek değer 1" |
| 3 |"örnek değeri 3" |

Temel tablo, bu tablodan atlanan ve diğer sanal tablolarda genişletilen koleksiyonlar hariç özgün veritabanı tablosuyla aynı verileri içerir.

Aşağıdaki tablolarda, List, Map ve StringSet sütunlarından verileri yeniden izleyen sanal tablolar gösterilmektedir. "_İndex" veya "_Key" ile biten adlara sahip sütunlar, verilerin özgün liste veya harita içindeki konumunu gösterir. "_Value" ile biten adlara sahip sütunlar, koleksiyondaki genişletilmiş verileri içerir.

**"ExampleTable_vt_List" tablosu:**

| pk_int | List_index | List_value |
| --- | --- | --- |
| 1\. |0 |1\. |
| 1\. |1\. |2 |
| 1\. |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

**"ExampleTable_vt_Map" tablosu:**

| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1\. |S1 |A |
| 1\. |S2 |b |
| 3 |S1 |t |

**"ExampleTable_vt_StringSet" tablosu:**

| pk_int | StringSet_value |
| --- | --- |
| 1\. |A |
| 1\. |B |
| 1\. |C |
| 3 |A |
| 3 |E |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
