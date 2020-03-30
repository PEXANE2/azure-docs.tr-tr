---
title: Verileri MongoDB'den taşıma
description: Azure Veri Fabrikası'nı kullanarak MongoDB veritabanındaki verileri nasıl taşıyacağım hakkında bilgi edinin.
services: data-factory
author: linda33wj
ms.author: jingwang
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/13/2018
ms.openlocfilehash: edddd100bddab1d642a8169353298a2d20620274
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281346"
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Verileri MongoDB'den taşıma

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-on-premises-mongodb-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-mongodb.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki MongoDB konektörüne](../connector-mongodb.md)bakın.


Bu makalede, verileri şirket içi MongoDB veritabanından taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

Şirket içi MongoDB veri deposundaki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Veri fabrikası şu anda yalnızca MongoDB veri deposundan diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından MongoDB veri deposuna taşımak için değildir.

## <a name="prerequisites"></a>Ön koşullar
Azure Veri Fabrikası hizmetinin şirket içi MongoDB veritabanınıza bağlanabilmesi için aşağıdaki bileşenleri yüklemeniz gerekir:

- Desteklenen MongoDB sürümleri şunlardır: 2.4, 2.6, 3.0, 3.2, 3.4 ve 3.6.
- Veri yönetimi ağ geçidi, veritabanıyla kaynak rekabetini önlemek için veritabanını barındıran aynı makinede veya ayrı bir makinede. Veri Yönetimi Ağ Geçidi, şirket içi veri kaynaklarını bulut hizmetlerine güvenli ve yönetilen bir şekilde bağlayan bir yazılımdır. Veri Yönetimi Ağ Geçidi hakkında ayrıntılar için [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) makalesine bakın. Bkz. Verileri taşımak için ağ geçidini ayarlamayla ilgili adım adım talimatlar için [verileri şirket içinde bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesine taşıyın.

    Ağ geçidini yüklediğinizde, MongoDB'ye bağlanmak için kullanılan bir Microsoft MongoDB ODBC sürücüsünü otomatik olarak yükler.

    > [!NOTE]
    > Azure IaaS VM'lerde barındırılan olsa bile MongoDB'ye bağlanmak için ağ geçidini kullanmanız gerekir. Bulutta barındırılan MongoDB örneğine bağlanmaya çalışıyorsanız, ağ geçidi örneğini IaaS VM'ye de yükleyebilirsiniz.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak şirket içi MongoDB veri deposundan veri aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.**

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız.  Şirket içi MongoDB veri deposundan veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: Verileri MongoDB'den Azure Blob bölümüne](#json-example-copy-data-from-mongodb-to-azure-blob) kopyalayın.

Aşağıdaki bölümler, MongoDB kaynağına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, **OnPremisesMongoDB** bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **OnPremisesMongoDb** |Evet |
| sunucu |MongoDB sunucusunun IP adresi veya ana bilgisayar adı. |Evet |
| port |MongoDB sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. |İsteğe bağlı, varsayılan değer: 27017 |
| authenticationType |Temel veya Anonim. |Evet |
| kullanıcı adı |MongoDB'ye erişmek için kullanıcı hesabı. |Evet (temel kimlik doğrulama kullanılıyorsa). |
| password |Kullanıcının parolası. |Evet (temel kimlik doğrulama kullanılıyorsa). |
| authSource |Kimlik doğrulama için kimlik bilgilerinizi denetlemek için kullanmak istediğiniz MongoDB veritabanının adı. |İsteğe bağlı (temel kimlik doğrulama kullanılıyorsa). varsayılan: yönetici hesabını ve databaseName özelliğini kullanarak belirtilen veritabanını kullanır. |
| Databasename |Erişmek istediğiniz MongoDB veritabanının adı. |Evet |
| ağ geçidiAdı |Veri deposuna erişen ağ geçidinin adı. |Evet |
| şifreli Credential |Kimlik bilgileri ağ geçidi tarafından şifrelenir. |İsteğe bağlı |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **MongoDbCollection** türü veri kümesi için typeProperties bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Toplamaadı |MongoDB veritabanındaki koleksiyonun adı. |Evet |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

Diğer taraftan etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Kaynak **MongoDbSource** türünde olduğunda, typeProperties bölümünde aşağıdaki özellikler mevcuttur:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL-92 sorgu dizesi. Örneğin: MyTable'dan * seçin. |Hayır **(koleksiyonVeri** **kümesinin** adı belirtilirse) |



## <a name="json-example-copy-data-from-mongodb-to-azure-blob"></a>JSON örneği: MongoDB'den Azure Blob'a veri kopyalama
Bu örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Şirket içi Bir MongoDB'den Azure Blob Depolama alanına verilerin nasıl kopyalanır şekilde kopyalanırolduğunu gösterir. Ancak, veriler Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) belirtilen lavabolardan herhangi biri için kopyalanabilir.

Örnekte aşağıdaki veri fabrikası varlıkları vardır:

1. [OnPremisesMongoDb](#linked-service-properties)türüne bağlı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [MongoDbCollection](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [MongoDbSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity ile bir [boru hattı.](data-factory-create-pipelines.md)

Örnek, MongoDB veritabanındaki bir sorgu sonucundaki verileri her saat başı bir blob'a kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

İlk adım olarak, [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) makalesindeki talimatlara göre veri yönetimi ağ geçidini kurun.

**MongoDB bağlantılı hizmet:**

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< The IP address or host name of the MongoDB server >",
            "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< The database that you want to use to check your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
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

**MongoDB giriş veri seti:** "Dış"ı ayarlamak: "true" tablonun veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```json
{
    "name": "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

**Azure Blob çıktı veri seti:**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**MongoDB kaynağı ve Blob lavabosu olan bir boru hattındaki etkinliği kopyalayın:**

Ardışık iş, yukarıdaki giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **MongoDbSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **Sorgu** özelliği için belirtilen SQL sorgusu, kopyalanacak son saatteki verileri seçer.

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "MongoDbSource",
                        "query": "$$Text.Format('select * from MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "MongoDbInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "MongoDBToAzureBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```


## <a name="schema-by-data-factory"></a>Veri Fabrikası'na Göre Şema
Azure Veri Fabrikası hizmeti, koleksiyondaki en son 100 belgeyi kullanarak bir MongoDB koleksiyonundan şema çıkar. Bu 100 belge tam şema içermiyorsa, kopyalama işlemi sırasında bazı sütunlar yoksayılabilir.

## <a name="type-mapping-for-mongodb"></a>MongoDB için tür eşleme
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama etkinliği aşağıdaki 2 adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

Verileri MongoDB'ye aktarırken MongoDB türlerinden .NET türlerine aşağıdaki eşlemeler kullanılır.

| MongoDB tipi | .NET Framework türü |
| --- | --- |
| İkili |Bayt[] |
| Boole |Boole |
| Tarih |DateTime |
| SayıÇift |Çift |
| NumberInt |Int32 |
| Sayı Uzun |Int64 |
| ObjectID |Dize |
| Dize |Dize |
| Uuıd |Guid |
| Nesne |İç içe ayırıcı olarak "_" ile düzleştirilmiş sütunlar halinde yeniden normalleştirildi |

> [!NOTE]
> Sanal tabloları kullanarak dizidesteği hakkında bilgi edinmek [için,](#support-for-complex-types-using-virtual-tables) aşağıdaki sanal tablolar bölümünü kullanarak karmaşık türler için Destek bölümüne bakın.

Şu anda, aşağıdaki MongoDB veri türleri desteklenmez: DBPointer, JavaScript, Max/Min tuşu, Düzenli İfade, Sembol, Zaman Damgası, Tanımlanmamış

## <a name="support-for-complex-types-using-virtual-tables"></a>Sanal tabloları kullanarak karmaşık türleri için destek
Azure Veri Fabrikası, MongoDB veritabanınızdaki verilere bağlanmak ve kopyalamak için yerleşik bir ODBC sürücüsü kullanır. Belgeler arasında farklı türlerde diziler veya nesneler gibi karmaşık türler için sürücü verileri karşılık gelen sanal tablolara yeniden normalleştirir. Özellikle, bir tablo bu tür sütunlar içeriyorsa, sürücü aşağıdaki sanal tabloları oluşturur:

* Karmaşık tür sütunları dışında gerçek tabloyla aynı verileri içeren **bir taban tablosu.** Taban tablo, temsil ettiği gerçek tabloyla aynı adı kullanır.
* İç içe geçen verileri genişleten her karmaşık tür sütunu için sanal bir **tablo.** Sanal tablolar gerçek tablo, bir ayırıcı "_" ve dizi veya nesnenin adı kullanılarak adlandırılır.

Sanal tablolar, sürücünün normalden arındırılmış verilere erişmesini sağlayarak gerçek tablodaki verilere başvurur. Aşağıdaki örnek bölümüne bakın. Sanal tabloları sorgulayarak ve birleştirerek MongoDB dizilerinin içeriğine erişebilirsiniz.

Sanal tablolar da dahil olmak üzere MongoDB veritabanındaki tabloların listesini sezgisel olarak görüntülemek ve içindeki verileri önizlemek için [Kopya Sihirbazı'nı](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) kullanabilirsiniz. Ayrıca Kopya Sihirbazı'nda bir sorgu oluşturup sonucu görmek için doğrulayabilirsiniz.

### <a name="example"></a>Örnek
Örneğin, aşağıdaki "ExampleTable" her hücrede bir dizi Nesne içeren bir sütuna sahip bir MongoDB tablosu – Faturalar ve bir dizi Skaler türü olan bir sütun – Derecelendirmeler.

| _id | Müşteri Adı | Faturalar | Hizmet Düzeyi | Derecelendirmeler |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id:"123", item:"tost makinesi", fiyat:"456", indirimli:"0.2"}, {invoice_id:"124", öğe:"fırın", fiyat: "1235", indirim: "0.2"}] |Silver |[5,6] |
| 2222 |XYZ |[{invoice_id:"135", item:"buzdolabı", fiyat: "12543", indirim: "0.0"}] |Gold |[1,2] |

Sürücü, bu tek tabloyu temsil edecek birden çok sanal tablo oluşturur. İlk sanal tablo aşağıda gösterilen "ExampleTable" adlı temel tablodur. Temel tablo özgün tablonun tüm verilerini içerir, ancak dizilerden gelen veriler atlanmıştır ve sanal tablolarda genişletilir.

| _id | Müşteri Adı | Hizmet Düzeyi |
| --- | --- | --- |
| 1111 |ABC |Silver |
| 2222 |XYZ |Gold |

Aşağıdaki tablolar, örnekteki özgün dizileri temsil eden sanal tabloları gösterir. Bu tablolar aşağıdakileri içerir:

* Özgün dizinin satırına karşılık gelen özgün birincil anahtar sütununa (_id sütun üzerinden) geri gönderme
* Orijinal dizi içindeki verilerin konumunun bir göstergesi
* Dizi içindeki her öğe için genişletilmiş veri

Tablo "ExampleTable_Invoices":

| _id | ExampleTable_Invoices_dim1_idx | invoice_id | öğe | price | İndirim |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |Tost makinesi |456 |0.2 |
| 1111 |1 |124 |Fırın |1235 |0.2 |
| 2222 |0 |135 |Buzdolabı |12543 |0,0 |

Tablo "ExampleTable_Ratings":

| _id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara eşleme hakkında bilgi edinmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir. Bkz. [İlişkisel kaynaklardan tekrarlanabilir okuma.](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.

## <a name="next-steps"></a>Sonraki Adımlar
Bkz. Verileri şirket içi veri deposundan Azure veri deposuna taşıyacak bir veri ardışık alanı oluşturmak için adım adım yönergeler için şirket içi ve bulut makalesi [arasında veri taşıma.](data-factory-move-data-between-onprem-and-cloud.md)
