---
title: Azure Veri Fabrikası'nı kullanarak verileri DB2'den taşıma
description: Azure Veri Fabrikası Kopyalama Etkinliği'ni kullanarak şirket içi DB2 veritabanındaki verileri nasıl taşıyarak verileri nasıl taşıyarak nasıl taşıyarak öğrenin
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: c1644e17-4560-46bb-bf3c-b923126671f1
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: e5d2c6b0460c3a7566adb17601aceb57e57f4d0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931779"
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a>Azure Veri Fabrikası Kopyalama Etkinliği'ni kullanarak verileri DB2'den taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-onprem-db2-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-db2.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki DB2 konektörüne](../connector-db2.md)bakın.


Bu makalede, şirket içi DB2 veritabanından bir veri deposuna veri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'ni nasıl kullanabileceğiniz açıklanmaktadır. [Verileri, Veri Fabrikası veri hareketi etkinlikleri](data-factory-data-movement-activities.md#supported-data-stores-and-formats) makalesinde desteklenen lavabo olarak listelenen herhangi bir depoya kopyalayabilirsiniz. Bu konu, Kopyalama Etkinliği'ni kullanarak veri hareketine genel bir bakış sunan ve desteklenen veri deposu birleşimlerini listeleyen Veri Fabrikası makalesine dayanmaktadır. 

Veri Fabrikası şu anda yalnızca DB2 veritabanından desteklenen bir [lavabo veri deposuna](data-factory-data-movement-activities.md#supported-data-stores-and-formats)veri taşımayı destekler. Verileri diğer veri depolarından DB2 veritabanına taşımak desteklenmez.

## <a name="prerequisites"></a>Ön koşullar
Veri Fabrikası, [veri yönetimi ağ geçidini](data-factory-data-management-gateway.md)kullanarak şirket içi DB2 veritabanına bağlanmayı destekler. Verilerinizi taşımak için ağ geçidi veri ardışık hattını ayarlamak için adım adım talimatlar için, [verileri şirket içinde bulut makalesine taşıy'a](data-factory-move-data-between-onprem-and-cloud.md) bakın.

DB2 Azure IaaS VM'de barındırılan olsa bile bir ağ geçidi gereklidir. Ağ geçidini veri deposuyla aynı IaaS VM'ye yükleyebilirsiniz. Ağ geçidi veritabanına bağlanabiliyorsa, ağ geçidini farklı bir VM'ye yükleyebilirsiniz.

Veri yönetimi ağ geçidi yerleşik bir DB2 sürücüsü sağlar, bu nedenle DB2'den verileri kopyalamak için bir sürücüyü el ile yüklemeniz gerekmez.

> [!NOTE]
> Sorun giderme bağlantısı ve ağ geçidi sorunlarıyla ilgili ipuçları [için, Sorun Giderme ağ geçidi sorunları](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) makalesine bakın.


## <a name="supported-versions"></a>Desteklenen sürümler
Veri Fabrikası DB2 bağlayıcısı, Dağıtılmış İlişkisel Veritabanı Mimarisi (DRDA) SQL Access Manager sürümleri 9, 10 ve 11 ile aşağıdaki IBM DB2 platformlarını ve sürümlerini destekler:

* z/OS sürümü 11.1 için IBM DB2
* z/OS sürümü 10.1 için IBM DB2
* IBM DB2 i (AS400) sürüm 7.2 için
* IBM DB2 i (AS400) sürüm 7.1 için
* Linux, UNIX ve Windows (LUW) sürüm 11 için IBM DB2
* LUW sürüm 10.5 için IBM DB2
* LUW sürüm 10.1 için IBM DB2

> [!TIP]
> Hata iletisi alırsanız "Bir SQL deyimi yürütme isteğine karşılık gelen paket bulunamadı. SQLSTATE=51002 SQLCODE=-805" nedeni, işletim sistemi üzerinde normal kullanıcı için gerekli bir paket oluşturulmadı. Bu sorunu gidermek için, DB2 sunucu türünüz için aşağıdaki yönergeleri izleyin:
> - DB2 for i (AS400): Kopyalama Etkinliği'ni çalıştırmadan önce bir güç kullanıcısının normal kullanıcı için koleksiyonu oluşturmasına izin verin. Koleksiyonu oluşturmak için aşağıdaki komutu kullanın:`create collection <username>`
> - z/OS veya LUW için DB2: Kopyayı bir kez çalıştırmak için yüksek bir ayrıcalık hesabı kullanın - paket yetkilileri ve BIND, BINDADD, KAMU IZINLERİnE GRANT EXECUTE'a sahip bir güç kullanıcısı veya yöneticisi. Gerekli paket kopya sırasında otomatik olarak oluşturulur. Daha sonra, sonraki kopya çalıştırmalarınız için normal kullanıcıya geri dönebilirsiniz.

## <a name="getting-started"></a>Başlarken
Farklı araçlar ve API'ler kullanarak şirket içi DB2 veri deposundan veri taşımak için kopyalama etkinliği içeren bir ardışık hatlar oluşturabilirsiniz: 

- Bir ardışık iş tonu oluşturmanın en kolay yolu Azure Veri Fabrikası Kopyalama Sihirbazı'nı kullanmaktır. Kopya Sihirbazı'nı kullanarak bir ardışık kaynak oluşturma konusunda hızlı bir geçiş [için, Bkz. Öğretici: Kopya Sihirbazı'nı kullanarak bir ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md) 
- Visual Studio, Azure PowerShell, Azure Kaynak Yöneticisi şablonu, .NET API ve REST API gibi bir boru hattı oluşturmak için araçları da kullanabilirsiniz. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeler için [Etkinlik Kopyala öğreticisine](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)bakın. 

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için bağlantılı hizmetler oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek veri kümeleri oluşturun. 
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir ardışık işlem oluşturma. 

Kopyalama Sihirbazı'nı kullandığınızda, Veri Fabrikası bağlantılı hizmetler, veri kümeleri ve boru hattı varlıkları için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar veya API'ler (.NET API hariç) kullandığınızda, JSON biçimini kullanarak Veri Fabrikası varlıklarını tanımlarsınız. JSON örneği: DB2'den Azure Blob depolamasına veri kopyalama, şirket içi DB2 veri deposundan veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlarını gösterir.

Aşağıdaki bölümler, DB2 veri deposuna özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="db2-linked-service-properties"></a>DB2 bağlantılı hizmet özellikleri
Aşağıdaki tabloda, DB2 bağlantılı bir hizmete özgü JSON özellikleri listelenir.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **Türü** |Bu özellik **OnPremisesDb2**olarak ayarlanmalıdır. |Evet |
| **sunucu** |DB2 sunucusunun adı. |Evet |
| **Veritabanı** |DB2 veritabanının adı. |Evet |
| **Şema** |DB2 veritabanındaki şema adı. Bu özellik büyük/küçük harf duyarlıdır. |Hayır |
| **authenticationType** |DB2 veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, Temel ve Windows. |Evet |
| **Username** |Temel veya Windows kimlik doğrulaması kullanıyorsanız kullanıcı hesabının adı. |Hayır |
| **parola** |Kullanıcı hesabının şifresi. |Hayır |
| **ağ geçidiAdı** |Veri Fabrikası hizmetinin şirket içi DB2 veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. **Yapı,** **kullanılabilirlik**ve veri kümesi JSON **ilkesi** gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure Blob depolama, Azure Tablo depolama vb.) için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. DB2 veri kümesini içeren **RelationalTable**türünden bir veri kümesinin **typeProperties** bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **Tablename** |Bağlı hizmetin başvurulması gereken DB2 veritabanı örneğindeki tablonun adı. Bu özellik büyük/küçük harf duyarlıdır. |Hayır **(RelationalSource** türündeki bir kopya etkinliğinin **sorgu** özelliği belirtilmişse) |

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama
Kopyalama etkinliklerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin listesi [için, Ardışık Hatlar Oluşturma](data-factory-create-pipelines.md) makalesine bakın. **Ad,** **açıklama,** **giriş** tablosu, **çıktılar** tablosu ve **ilke**gibi Etkinlik özelliklerini kopyalama, tüm etkinlik türleri için kullanılabilir. Etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türü için değişir. Kopyalama Etkinliği için özellikler veri kaynakları ve lavabo türlerine bağlı olarak değişir.

Copy Activity için, kaynak **RelationalSource** türünden (DB2 içerir) olduğunda, **typeProperties** bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| **Sorgu** |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin, `"query": "select * from "MySchema"."MyTable""` |Hayır (bir veri kümesinin **tabloAdı** özelliği belirtilirse) |

> [!NOTE]
> Şema ve masa adları büyük/küçük harf duyarlıdır. Sorgu deyiminde, "" (çift tırnak) kullanarak özellik adlarını içine alın.

## <a name="json-example-copy-data-from-db2-to-azure-blob-storage"></a>JSON örneği: Verileri DB2'den Azure Blob depolama alanına kopyalama
Bu örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hatlar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Örnek, db2 veritabanından Blob depolamasına nasıl veri kopyaladığınızı gösterir. Ancak, veriler Azure Veri Fabrikası Kopyalama Etkinliği kullanılarak [desteklenen veri deposu lavabo türüne](data-factory-data-movement-activities.md#supported-data-stores-and-formats) kopyalanabilir.

Örnekte aşağıdaki Veri Fabrikası varlıkları vardır:

- [OnPremisesDb2](data-factory-onprem-db2-connector.md) türü bir DB2 bağlantılı hizmet
- [AzureDepolama](data-factory-azure-blob-connector.md#linked-service-properties) türüne bağlı bir Azure Blob depolama hizmeti
- [Tür İlişkisel Tablo'nun](data-factory-onprem-db2-connector.md#dataset-properties) giriş [veri kümesi](data-factory-create-datasets.md)
- [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties) türünden bir çıktı [veri kümesi](data-factory-create-datasets.md)
- [İlişkisel Kaynak](data-factory-onprem-db2-connector.md#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) özelliklerini kullanan bir kopyalama etkinliği olan bir [ardışık hat](data-factory-create-pipelines.md)

Örnek, bir sorgu sonucundaki verileri DB2 veritabanından saatlik bir Azure blob'una kopyalar. Örnekte kullanılan JSON özellikleri varlık tanımlarını izleyen bölümlerde açıklanmıştır.

İlk adım olarak, bir veri ağ geçidi yükleyin ve yapılandırır. Yönergeler, şirket içi konumlar ve bulut makalesi [arasındaki hareketli verilerde](data-factory-move-data-between-onprem-and-cloud.md) yer alıyor.

**DB2 bağlantılı hizmet**

```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

**Azure Blob depolama bağlantılı hizmet**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

**DB2 giriş veri seti**

Örnek, DB2'de zaman serisi verileri için "zaman damgası" etiketli bir sütuna sahip "MyTable" adlı bir tablo oluşturduğunuzu varsayar.

**Dış** özellik "doğru" olarak ayarlanır. Bu ayar, Veri Fabrikası hizmetine bu veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir. **Tür** özelliğinin **RelationalTable**olarak ayarlı olduğuna dikkat edin.


```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
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

**Azure Blob çıktı veri kümesi**

Veri, **frekans** özelliğini "Saat" ve **aralık** özelliği 1 olarak ayarlayarak her saat yeni bir blob'a yazılır. Blob için **folderPath** özelliği, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat parçalarını kullanır.

```json
{
    "name": "AzureBlobDb2DataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopyalama etkinliği için boru hattı**

Ardışık iş, belirtilen giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir kopyalama etkinliği içerir. Boru hattı için JSON tanımında, **kaynak** türü **RelationalSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **Sorgu** özelliği için belirtilen SQL sorgusu "Siparişler" tablosundan verileri seçer.

```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for the copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"Orders\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "Db2DataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDb2DataSet"
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
                "name": "Db2ToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-db2"></a>DB2 için tür eşleme
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama Etkinliği aşağıdaki iki adımlı yaklaşımı kullanarak kaynak türünden lavabo türüne otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türünden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

Kopyalama Etkinliği verileri DB2 türünden bir .NET türüne dönüştürdüğünde aşağıdaki eşlemeler kullanılır:

| DB2 veritabanı türü | .NET Framework türü |
| --- | --- |
| Smallint |Int16 |
| Tamsayı |Int32 |
| Bigint |Int64 |
| Gerçek |Tek |
| Çift |Çift |
| Kayan |Çift |
| Ondalık |Ondalık |
| Ondalık Float |Ondalık |
| Sayısal |Ondalık |
| Tarih |DateTime |
| Zaman |TimeSpan |
| Zaman damgası |DateTime |
| Xml |Bayt[] |
| Char |Dize |
| Varchar |Dize |
| Longvarchar |Dize |
| DB2Dynarray |Dize |
| İkili |Bayt[] |
| Varbinary |Bayt[] |
| LongVarBinary |Bayt[] |
| Grafik |Dize |
| Vargraphıc |Dize |
| Longvargraphic |Dize |
| Clob |Dize |
| Blob |Bayt[] |
| DbClob |Dize |
| Smallint |Int16 |
| Tamsayı |Int32 |
| Bigint |Int64 |
| Gerçek |Tek |
| Çift |Çift |
| Kayan |Çift |
| Ondalık |Ondalık |
| Ondalık Float |Ondalık |
| Sayısal |Ondalık |
| Tarih |DateTime |
| Zaman |TimeSpan |
| Zaman damgası |DateTime |
| Xml |Bayt[] |
| Char |Dize |

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara nasıl eşlendireceklerini öğrenmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-reads-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okumalar
İlişkisel bir veri deposundan veri kopyaladığınızda, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme **ilkesi** özelliğini de yapılandırabilirsiniz. Dilim kaç kez yeniden çalıştırılırsa yayınlanın ve dilimin nasıl yeniden çalıştırıldığına bakılmaksızın aynı verilerin okundurun. Daha fazla bilgi için bkz: [İlişkisel kaynaklardan tekrarlanabilir okumalar.](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve ayar
Kopyalama Etkinliği performansını etkileyen önemli etkenler ve Kopyalama [Etkinliği Performansı ve Tuning Kılavuzu'ndaki](data-factory-copy-activity-performance.md)performansı optimize etmenin yolları hakkında bilgi edinin.
