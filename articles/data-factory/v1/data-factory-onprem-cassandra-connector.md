---
title: Veri Fabrikasını kullanarak Verileri Cassandra'dan taşıma
description: Azure Veri Fabrikası'nı kullanarak şirket içi Cassandra veritabanından verileri nasıl taşıyabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 085cc312-42ca-4f43-aa35-535b35a102d5
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 05cee60fb1f4d43d1b4ce371aa9f22650b4782da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281307"
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri şirket içi Cassandra veritabanından taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-onprem-cassandra-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-cassandra.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Cassandra konektörüne](../connector-cassandra.md)bakın.

Bu makalede, verileri şirket içi Cassandra veritabanından taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

Şirket içi Cassandra veri deposundaki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Veri fabrikası şu anda yalnızca bir Cassandra veri deposundan diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından Cassandra veri deposuna taşımak için değildir.

## <a name="supported-versions"></a>Desteklenen sürümler
Cassandra konektörü Cassandra'nın aşağıdaki sürümlerini destekler: 2.x ve 3.x. Kendi kendine barındırılan Tümleştirme Runtime'da çalışan etkinlik için Cassandra 3.x, IR sürüm 3.7 ve üzeri sürümden beri desteklenir.

## <a name="prerequisites"></a>Ön koşullar
Azure Veri Fabrikası hizmetinin şirket içi Cassandra veritabanınıza bağlanabilmesi için, veritabanıyla kaynak rekabetini önlemek için veritabanını barındıran aynı makineye veya ayrı bir makineye bir Veri Yönetimi Ağ Geçidi yüklemeniz gerekir. Veri Yönetimi Ağ Geçidi, şirket içi veri kaynaklarını bulut hizmetlerine güvenli ve yönetilen bir şekilde bağlayan bir bileşendir. Veri Yönetimi Ağ Geçidi hakkında ayrıntılar için [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) makalesine bakın. Bkz. Verileri taşımak için ağ geçidini ayarlamayla ilgili adım adım talimatlar için [verileri şirket içinde bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesine taşıyın.

Veritabanı bulutta barındırılan olsa bile cassandra veritabanına bağlanmak için ağ geçidini kullanmanız gerekir, örneğin bir Azure IaaS VM'de. Y Ağ geçidi veritabanına bağlanabildiği sürece veritabanını barındıran aynı VM'de veya ayrı bir VM'de ağ geçidine sahip olabilirsiniz.

Ağ geçidini yüklediğinizde, Cassandra veritabanına bağlanmak için kullanılan bir Microsoft Cassandra ODBC sürücüsüotomatik olarak yükler. Bu nedenle, Cassandra veritabanından veri kopyalarken ağ geçidi makinesine herhangi bir sürücüyü el ile yüklemeniz gerekmez.

> [!NOTE]
> Sorun giderme bağlantısı/ağ geçidi yle ilgili sorunlarla ilgili ipuçları için [sorun giderme ağ geçidi sorunlarına](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bakın.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak şirket içi Cassandra veri deposundan veri aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

- Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)
- Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.**

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. Şirket içi Cassandra veri deposundaki verileri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: Verileri Cassandra'dan Azure Blob bölümüne kopyalayın.](#json-example-copy-data-from-cassandra-to-azure-blob)

Aşağıdaki bölümler, Cassandra veri deposuna özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, Cassandra bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **OnPremisesCassandra** |Evet |
| konak |Cassandra sunucularının bir veya daha fazla IP adresi veya ana bilgisayar adları.<br/><br/>Tüm sunuculara aynı anda bağlanmak için virgülle ayrılmış IP adresleri veya ana bilgisayar adlarının listesini belirtin. |Evet |
| port |Cassandra sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. |Hayır, varsayılan değer: 9042 |
| authenticationType |Temel veya Anonim |Evet |
| kullanıcı adı |Kullanıcı hesabı için kullanıcı adını belirtin. |Evet, kimlik doğrulamaTürü Temel olarak ayarlanmışsa. |
| password |Kullanıcı hesabı için parola belirtin. |Evet, kimlik doğrulamaTürü Temel olarak ayarlanmışsa. |
| ağ geçidiAdı |Şirket içi Cassandra veritabanına bağlanmak için kullanılan ağ geçidinin adı. |Evet |
| şifreli Credential |Kimlik bilgileri ağ geçidi tarafından şifrelenir. |Hayır |

>[!NOTE]
>Şu anda SSL kullanarak Cassandra bağlantısı desteklenmez.

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **CassandraTable** tipi veri kümesi için typeProperties bölümü aşağıdaki özelliklere sahiptir

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| anahtar boşluğu |Cassandra veritabanındaki boşluk veya şema adı. |Evet **(CassandraSource** **sorgusu** tanımlanmamışsa). |
| tableName |Cassandra veritabanındaki tablonun adı. |Evet **(CassandraSource** **sorgusu** tanımlanmamışsa). |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

Oysa, etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Kaynak **CassandraSource**türünde olduğunda, typeProperties bölümünde aşağıdaki özellikler mevcuttur:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL-92 sorgusu veya CQL sorgusu. [Bkz. CQL başvurusu.](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html) <br/><br/>SQL sorgusunu kullanırken, sorgulamak istediğiniz tabloyu temsil etmek için **keyspace name.table adını** belirtin. |Hayır (dataset'teki tablo Adı ve anahtar alanı tanımlanırsa). |
| tutarlılıkSeviye |Tutarlılık düzeyi, istemci uygulamasına veri döndürmeden önce okuma isteğine kaç yinelemenin yanıt vermesi gerektiğini belirtir. Cassandra, okundu isteğini karşılamak için veri için belirtilen çoğaltma sayısını denetler. |Bir, IKI, ÜÇ, ÇOĞUNLUK, HEPSI, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE. Ayrıntılar için [veri tutarlılığını yapılandırma](https://docs.datastax.com/en/cassandra/2.1/cassandra/dml/dml_config_consistency_c.html) ya da yapılandırma ya bak. |Hayır. Varsayılan değer ONE'dır. |

## <a name="json-example-copy-data-from-cassandra-to-azure-blob"></a>JSON örneği: Cassandra'dan Azure Blob'a veri kopyalama
Bu örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Şirket içi Cassandra veritabanından Azure Blob Depolama'ya verilerin nasıl kopyalanır şekilde kopyalanırhale geldiğini gösterir. Ancak, veriler Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) belirtilen lavabolardan herhangi biri için kopyalanabilir.

> [!IMPORTANT]
> Bu örnek JSON parçacıkları sağlar. Veri fabrikası oluşturmak için adım adım yönergeler içermez. Adım adım yönergeler için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında hareketli veri bakın.

Örnekte aşağıdaki veri fabrikası varlıkları vardır:

* [OnPremisesCassandra](#linked-service-properties)türüne bağlı bir hizmet.
* [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
* [CassandraTable](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
* [CassandraSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity ile bir [boru hattı.](data-factory-create-pipelines.md)

**Cassandra bağlantılı hizmet:**

Bu örnek, **Cassandra** bağlantılı hizmeti kullanır. Bu bağlantılı hizmet tarafından desteklenen özellikler için [Cassandra bağlantılı hizmet](#linked-service-properties) bölümüne bakın.

```json
{
    "name": "CassandraLinkedService",
    "properties":
    {
        "type": "OnPremisesCassandra",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "host": "mycassandraserver",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

**Azure Depolama bağlantılı hizmet:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

**Cassandra giriş veri seti:**

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "mykeyspace"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**True'nun** **dışında** ayar, Veri Fabrikası hizmetine veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

**Azure Blob çıktı veri seti:**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/fromcassandra"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Cassandra kaynak ve Blob lavabo ile bir boru hattı etkinliği kopyalama:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **CassandraSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır.

İlişkisel Kaynak tarafından desteklenen özellikler listesi için [RelationalSource türü özelliklerine](#copy-activity-properties) bakın.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[
        {
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra to an Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "CassandraInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"

                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }
        ]
    }
}
```

### <a name="type-mapping-for-cassandra"></a>Cassandra için tür eşleme
| Cassandra Tipi | .NET Tabanlı Tip |
| --- | --- |
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
>

## <a name="work-with-collections-using-virtual-table"></a>Sanal tabloyu kullanarak koleksiyonlarla çalışma
Azure Veri Fabrikası, Cassandra veritabanınızdaki verilere bağlanmak ve kopyalamak için yerleşik bir ODBC sürücüsü kullanır. Harita, ayar ve liste gibi toplama türleri için sürücü verileri karşılık gelen sanal tablolara yeniden normalleştirir. Özellikle, bir tablo herhangi bir koleksiyon sütunu içeriyorsa, sürücü aşağıdaki sanal tabloları oluşturur:

* Toplama sütunları dışında gerçek tabloyla aynı verileri içeren **bir taban tablo.** Taban tablo, temsil ettiği gerçek tabloyla aynı adı kullanır.
* İç içe geçen verileri genişleten her koleksiyon sütunu için sanal bir **tablo.** Koleksiyonları temsil eden sanal tablolar, gerçek tablonun adı, ayırıcı "*vt*" ve sütunun adı kullanılarak adlandırılır.

Sanal tablolar, sürücünün normalden arındırılmış verilere erişmesini sağlayarak gerçek tablodaki verilere başvurur. Ayrıntılar için Örnek bölümüne bakın. Cassandra koleksiyonlarının içeriğine sanal tabloları sorgulayarak ve katılarak erişebilirsiniz.

Sanal tablolar da dahil olmak üzere Cassandra veritabanındaki tabloların listesini sezgisel olarak görüntülemek ve içindeki verileri önizlemek için [Kopya Sihirbazı'nı](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) kullanabilirsiniz. Ayrıca Kopya Sihirbazı'nda bir sorgu oluşturup sonucu görmek için doğrulayabilirsiniz.

### <a name="example"></a>Örnek
Örneğin, aşağıdaki "ExampleTable", "pk_int" adlı bir tamsayı birincil anahtar sütunu, değer adlı bir metin sütunu, liste sütunu, bir harita sütunu ve set sütunu ("StringSet" olarak adlandırılır) içeren bir Cassandra veritabanı tablosudur.

| pk_int | Değer | Liste | Eşleme | StringSet |
| --- | --- | --- | --- | --- |
| 1 |"örnek değer 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"örnek değer 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Sürücü, bu tek tabloyu temsil edecek birden çok sanal tablo oluşturur. Sanal tablolardaki yabancı anahtar sütunları, gerçek tablodaki birincil anahtar sütunlarına başvurur ve sanal tablo satırının hangi gerçek tablo satırına karşılık olduğunu gösterir.

İlk sanal tablo aşağıdaki tabloda "ExampleTable" adlı taban tablosudur. Temel tablo, bu tablodan atlanan ve diğer sanal tablolarda genişletilen koleksiyonlar dışında özgün veritabanı tablosuyla aynı verileri içerir.

| pk_int | Değer |
| --- | --- |
| 1 |"örnek değer 1" |
| 3 |"örnek değer 3" |

Aşağıdaki tablolar, Liste, Harita ve StringSet sütunlarından verileri yeniden normalleştiren sanal tabloları gösterir. "_index" veya "_key" ile biten adlara sahip sütunlar, orijinal liste veya haritadaki verilerin konumunu gösterir. "_value" ile biten adlara sahip sütunlar, koleksiyondaki genişletilmiş verileri içerir.

#### <a name="table-exampletable_vt_list"></a>Tablo "ExampleTable_vt_List":
| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1 |
| 1 |1 |2 |
| 1 |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

#### <a name="table-exampletable_vt_map"></a>Tablo "ExampleTable_vt_Map":
| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |A |
| 1 |S2 |b |
| 3 |S1 |t |

#### <a name="table-exampletable_vt_stringset"></a>Tablo "ExampleTable_vt_StringSet":
| pk_int | StringSet_value |
| --- | --- |
| 1 |A |
| 1 |B |
| 1 |C |
| 3 |A |
| 3 |E |

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara eşleme hakkında bilgi edinmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir. Bkz. [İlişkisel kaynaklardan tekrarlanabilir okuma.](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
