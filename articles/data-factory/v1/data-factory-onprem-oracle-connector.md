---
title: Veri Fabrikası'nı kullanarak verileri Oracle'a veya Oracle'dan kopyalama
description: Azure Veri Fabrikası'nı kullanarak şirket içi Oracle veritabanına veya şirket içi Oracle veritabanından verileri nasıl kopyalayarak kopyalayabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 066e32d5ab21f88b170498173606043c54fec586
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265863"
---
# <a name="copy-data-to-or-from-oracle-on-premises-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Oracle'a veya şirket içinde kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-onprem-oracle-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-oracle.md)

> [!NOTE]
> Bu makale, Azure Data Factory’nin 1. sürümü için geçerlidir. Azure Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Oracle bağlayıcısı bölümüne](../connector-oracle.md)bakın.


Bu makalede, verileri şirket içi Oracle veritabanına veya şirket içi Oracle veritabanına taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Makale, Kopyalama Etkinliği'ni kullanarak veri hareketine genel bir bakış sunan [Veri hareketi etkinlikleri](data-factory-data-movement-activities.md)üzerine inşa edin.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Verileri Bir *Oracle veritabanından* aşağıdaki veri depolarına kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından oracle *veritabanına*veri kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Ön koşullar

Veri Fabrikası, Veri Yönetimi Ağ Geçidi'ni kullanarak şirket içi Oracle kaynaklarına bağlanmayı destekler. Veri Yönetimi Ağ Geçidi hakkında daha fazla bilgi edinmek için [Veri Yönetimi Ağ Geçidi'ne](data-factory-data-management-gateway.md) bakın. Verileri taşımak için veri ardışık geçidin nasıl ayarlanaacağına ilişkin adım adım talimatlar için [bkz.](data-factory-move-data-between-onprem-and-cloud.md)

Oracle bir hizmet (IaaS) VM olarak bir Azure altyapısında barındırılan olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabildiği sürece ağ geçidini veri deposuyla aynı IaaS VM'ye veya farklı bir VM'ye yükleyebilirsiniz.

> [!NOTE]
> Bağlantı ve ağ geçidiyle ilgili sorun giderme sorunlarıyla ilgili ipuçları için, [Sorun Giderme ağ geçidi sorunlarına](data-factory-data-management-gateway.md#troubleshooting-gateway-issues)bakın.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümler ve kurulum

Bu Oracle bağlayıcısı sürücülerin iki versiyonunu destekler:

- **Oracle için Microsoft sürücüsü (önerilir)**: Veri Yönetimi Ağ Geçidi sürüm 2.7'den başlayarak, Oracle için bir Microsoft sürücüsü ağ geçidine otomatik olarak yüklenir. Oracle'a bağlantı kurmak için sürücüyü yüklemeniz veya güncelleştirmeniz gerekmez. Ayrıca bu sürücüyü kullanarak daha iyi kopyalama performansı yaşayabilirsiniz. Oracle veritabanlarının bu sürümleri desteklenir:
  - Oracle 12c R1 (12.1)
  - Oracle 11g R1, R2 (11.1, 11.2)
  - Oracle 10g R1, R2 (10.1, 10.2)
  - Oracle 9i R1, R2 (9.0.1, 9.2)
  - Oracle 8i R3 (8.1.7)

    > [!NOTE]
    > Oracle proxy sunucusu desteklenmez.

    > [!IMPORTANT]
    > Şu anda, Oracle için Microsoft sürücüsü yalnızca Oracle'dan veri kopyalamayı destekler. Sürücü Oracle'a yazmayı desteklemiyor. Veri Yönetimi Ağ Geçidi **Tanılama** sekmesindeki test bağlantısı özelliği bu sürücüyü desteklemez. Alternatif olarak, bağlantı doğrulamak için Kopyasihirbazı kullanabilirsiniz.
    >

- **.NET için Oracle Veri Sağlayıcısı**: Oracle'dan veya Oracle'a veri kopyalamak için Oracle Veri Sağlayıcısı'nı kullanabilirsiniz. Bu [bileşen, Windows için Oracle Veri Erişim Bileşenleri'ne](https://www.oracle.com/technetwork/topics/dotnet/downloads/)dahildir. Ağ geçidinin yüklü olduğu makineye ilgili sürümü (32 bit veya 64 bit) yükleyin. [Oracle Veri Sağlayıcısı .NET 12.1](https://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) Oracle Database 10g Release 2 ve sonraki sürümlerine erişebilir.

    **XCopy Yükleme'yi**seçerseniz, readme.htm dosyasında açıklanan adımları tamamlayın. UI'ye (XCopy yükleyicisi değil) sahip olan yükleyiciyi seçmenizi öneririz.

    Sağlayıcıyı yükledikten sonra, Hizmetler applet veya Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'ni kullanarak makinenizdeki Veri Yönetimi Ağ Geçidi ana bilgisayarı hizmetini yeniden başlatın.

Kopya ardışık ardışık lığı yazmak için Kopya sihirbazını kullanırsanız, sürücü türü otomatik olarak belirlenir. Ağ geçidi sürümünüz sürüm 2.7'den önce olmadığı veya lavabo olarak Oracle'ı seçmediğiniz sürece Microsoft sürücüsü varsayılan olarak kullanılır.

## <a name="get-started"></a>Kullanmaya başlayın

Kopyalama etkinliği olan bir ardışık hatlar oluşturabilirsiniz. Ardışık iş, farklı araçlar veya API'ler kullanarak verileri şirket içi Oracle veritabanına veya şirket içi Oracle veritabanına taşır.

Bir ardışık lık oluşturmanın en kolay yolu Kopya sihirbazını kullanmaktır. Bkz. Öğretici: Veri Kopyala sihirbazını kullanarak bir ardışık yol oluşturma da hızlı bir geçiş için [Kopyala sihirbazını kullanarak bir ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçlardan birini de kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, Azure Kaynak Yöneticisi **şablonu,** **.NET API**veya **REST API.** Kopyalama etkinliği olan bir ardışık yapının nasıl oluşturulacağına ilişkin adım adım yönergeler için [Kopyalama Etkinliği öğreticisine](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık hatlar oluşturmak için aşağıdaki adımları tamamlayın:

1. Bir **veri fabrikası**oluşturun. Bir veri fabrikası bir veya daha fazla ardışık hat içerebilir.
2. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun. Örneğin, bir Oracle veritabanından Azure Blob depolamasına veri kopyalıyorsanız, Oracle veritabanınızı ve Azure depolama hesabınızı veri fabrikanıza bağlamak için iki bağlantılı hizmet oluşturun. Oracle'a özgü bağlantılı hizmet özellikleri için [Bkz. Bağlantılı hizmet özellikleri.](#linked-service-properties)
3. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. Önceki adımdaki örnekte, Oracle veritabanınızda giriş verilerini içeren tabloyu belirtmek için bir veri kümesi oluşturursunuz. Blob kapsayıcısını ve Oracle veritabanından kopyalanan verileri tutan klasörü belirtmek için başka bir veri kümesi oluşturursunuz. Oracle'a özgü veri kümesi özellikleri için [Bkz. Dataset özellikleri.](#dataset-properties)
4. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği olan bir **ardışık işlem** oluştur. Önceki örnekte, kaynak olarak **OracleSource** ve kopyalama etkinliği için bir lavabo olarak **BlobSink** kullanın. Benzer şekilde, Azure Blob depolama alanından oracle veritabanına kopyalıyorsanız, kopyalama etkinliğinde **BlobSource** ve **OracleSink'i** kullanırsınız. Oracle veritabanına özgü Kopyalama Etkinliği özellikleri için [Bkz.](#copy-activity-properties) Kaynak veya lavabo olarak veri deposunun nasıl kullanılacağı hakkında ayrıntılı bilgi için, önceki bölümdeki veri deponuzun bağlantısını seçin.

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları için JSON tanımları sizin için otomatik olarak oluşturulur: bağlantılı hizmetler, veri kümeleri ve ardışık iş. Araçlar veya API'ler (.NET API hariç) kullandığınızda, bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. Şirket içi Oracle veritabanına veya şirket içi Oracle veritabanından veri kopyalamak için kullandığınız Veri Fabrikası varlıkları için JSON tanımlarına sahip örnekler için JSON örneklerine bakın.

Aşağıdaki bölümlerde Veri Fabrikası varlıklarını tanımlamak için kullandığınız JSON özellikleri hakkında ayrıntılı bilgi sağlanıyor.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki tabloda Oracle bağlantılı hizmete özgü JSON öğeleri açıklanmaktadır:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |**Tür** özelliği **OnPremisesOracle**olarak ayarlanmalıdır. |Evet |
| driverType | Oracle veritabanından veya Oracle veritabanından verileri kopyalamak için hangi sürücünün kullanılacağını belirtin. İzin verilen değerler **Microsoft** ve **ODP** 'dir (varsayılan). Sürücü ayrıntıları için [Desteklenen sürüm ve yüklemeye](#supported-versions-and-installation) bakın. | Hayır |
| Connectionstring | **ConnectionString** özelliği için Oracle veritabanı örneğine bağlanmak için gereken bilgileri belirtin. | Evet |
| ağ geçidiAdı | Şirket içi Oracle sunucusuna bağlanmak için kullanılan ağ geçidinin adı. |Evet |

**Örnek: Microsoft sürücüsünü kullanma**

> [!TIP]
> "ORA-01025: UPI parametresi kapsama alanı dışında" yazan bir hata görürseniz `WireProtocolMode=1` ve Oracle'ınız sürüm 8i ise, bağlantı dizenize ekleyin ve yeniden deneyin:

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<service ID>;User Id=<user name>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Örnek: ODP sürücüsünü kullanma**

İzin verilen biçimler hakkında bilgi edinmek [için .NET ODP için Oracle veri sağlayıcısına](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/)bakın.

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<host name>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<service ID>))); User Id=<user name>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](data-factory-create-datasets.md)

Bir veri kümesi JSON dosyasının yapı, kullanılabilirlik ve ilke gibi bölümleri tüm veri kümesi türleri için benzerdir (örneğin, Oracle, Azure Blob depolama alanı ve Azure Tablo depolama alanı için).

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **OracleTable** türü veri kümesi için **typeProperties** bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin atıfta bulunduğu Oracle veritabanındaki tablonun adı. |Hayır **(oracleReaderQuery** veya **OracleSource** belirtilirse) |

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](data-factory-create-pipelines.md)

Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

> [!NOTE]
> Kopyalama Etkinliği yalnızca bir giriş alır ve yalnızca bir çıktı üretir.

Etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama Etkinliği özellikleri kaynak ve lavabonun türüne bağlı olarak değişir.

### <a name="oraclesource"></a>OracleSource

Kopyalama Etkinliği'nde, kaynak **OracleSource** türü olduğunda, **typeProperties** bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| oracleReaderQuery |Verileri okumak için özel sorguyu kullanın. |Bir SQL sorgu dizesi. Örneğin, \* **"MyTable'dan**seçin ". <br/><br/>Belirtilmemişse, bu SQL deyimi \* yürütülür: **"MyTable'dan**seçin " |Hayır<br />(tablo veri **kümesinin** **adı** belirtilirse) |

### <a name="oraclesink"></a>OracleSink

**OracleSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| yazmaBatchTimeout |Toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresi. |**Timespan**<br/><br/> Örnek: 00:30:00 (30 dakika) |Hayır |
| yazmaBatchSize |Arabellek boyutu **writeBatchSize**değerine ulaştığında SQL tablosuna veri ekler. |Sonsayı (satır sayısı) |Hayır (varsayılan: 100) |
| sqlWriterCleanupScript |Belirli bir dilimin verilerinin temizlenmesi için Kopyalama Etkinliği'nin yürütülmesi için bir sorgu belirtir. |Sorgu deyimi. |Hayır |
| sliceIdentifierSütun Adı |Otomatik olarak oluşturulmuş bir dilim tanımlayıcısıyla doldurulması için Kopyalama Etkinliği için sütun adını belirtir. **SliceIdentifierColumnName** değeri, yeniden çalıştırıldığında belirli bir dilimin verilerini temizlemek için kullanılır. |**İkili(32)** veri türüne sahip bir sütunun sütun adı. |Hayır |

## <a name="json-examples-for-copying-data-to-and-from-the-oracle-database"></a>Oracle veritabanına ve oracle veritabanından veri kopyalamak için JSON örnekleri

Aşağıdaki örnekler, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Örnekler, oracle veritabanından ve Azure Blob depolamadan verilerin nasıl kopyalanılabildiğini gösterir. Ancak, veriler [Desteklenen veri depolarında ve biçimlerinde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) listelenen lavabolardan herhangi biri için Azure Veri Fabrikası'nda Kopyalama Etkinliği kullanılarak kopyalanabilir.

**Örnek: Oracle'dan Azure Blob depolama alanına veri kopyalama**

Örnekte aşağıdaki Veri Fabrikası varlıkları vardır:

* [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties)türüne bağlı bir hizmet.
* [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
* [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
* Kaynak olarak [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) ve lavabo olarak [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) kullanan bir kopyalama etkinliği ile bir [boru hattı.](data-factory-create-pipelines.md)

Örnek, şirket içi Oracle veritabanındaki bir tablodaki verileri saatlik bir blob'a kopyalar. Örnekte kullanılan çeşitli özellikler hakkında daha fazla bilgi için örnekleri izleyen bölümlere bakın.

**Oracle bağlantılı hizmet**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<service ID>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob depolama bağlantılı hizmet**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

**Oracle giriş veri seti**

Örnek, Oracle'da **MyTable** adında bir tablo oluşturduğunuzu varsayar. Zaman serisi verileri için **zaman damgası sütunu** adı verilen bir sütun içerir.

Ayarı **dış**: **true,** Veri Fabrikası hizmetine veri kümesinin veri fabrikasının dışında olduğunu ve veri kümesinin veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
        },
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

Veriler her saat yeni bir blob yazılır (**frekans**: **saat**, **aralık**: **1**). Blob'un klasör yolu ve dosya adı, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat kısmını kullanır.

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": "\t",
                "rowDelimiter": "\n"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Kopyalama etkinliği içeren işlem hattı**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve saatlik çalışacak şekilde zamanlanan bir kopyalama etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **OracleSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **OracleReaderQuery** özelliğini kullanarak belirttiğiniz SQL sorgusu, kopyalanacak verileri son bir saat içinde seçer.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for a copy activity",
        "activities":[
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

**Örnek: Azure Blob depolamasından Oracle'a veri kopyalama**

Bu örnek, azure blob depolama hesabındaki verileri şirket içi Oracle veritabanına nasıl kopyalaylayiş yapılacağını gösterir. Ancak, Azure Veri Fabrikası'nda Kopyalama Etkinliği'ni kullanarak verileri *doğrudan* [Desteklenen veri depolarında ve biçimlerinde](data-factory-data-movement-activities.md#supported-data-stores-and-formats) listelenen kaynaklardan kopyalayabilirsiniz.

Örnekte aşağıdaki Veri Fabrikası varlıkları vardır:

* [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties)türüne bağlı bir hizmet.
* [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
* [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
* Bdamla kaynak Olarak [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) kullanan bir kopyalama etkinliği olan bir [ardışık kaynak](data-factory-create-pipelines.md) [OracleSink.](data-factory-onprem-oracle-connector.md#copy-activity-properties)

Örnek, verileri her saat başı şirket içi Oracle veritabanındaki bir tabloya kopyalar. Örnekte kullanılan çeşitli özellikler hakkında daha fazla bilgi için örnekleri izleyen bölümlere bakın.

**Oracle bağlantılı hizmet**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<host name>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<service ID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob depolama bağlantılı hizmet**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

**Azure Blob girdi veri kümesi**

Veriler her saat yeni bir damladan alınır (**sıklık**: **saat**, **aralık**: **1**). Blob'un klasör yolu ve dosya adı, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay ve gün kısmını kullanır. Dosya adı, başlangıç saatinin saat kısmını kullanır. Ayarı **dış**: **true,** veri fabrikası hizmetine bu tablonun veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
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

**Oracle çıktı veri seti**

Örnek, Oracle'da **MyTable** adında bir tablo oluşturduğunuzu varsayar. Tabloyu Oracle'da, blob CSV dosyasının içermesini beklediğiniz sütun sayısıyla oluşturun. Her saat başı tabloya yeni satırlar eklenir.

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

**Kopyalama etkinliği içeren işlem hattı**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir kopyalama etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **BlobSource** olarak ayarlanır ve **lavabo** türü **OracleSink**olarak ayarlanır.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with a copy activity",
        "activities":[
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
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


## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

### <a name="problem-1-net-framework-data-provider"></a>Sorun 1: .NET Çerçeve Veri Sağlayıcısı

**Hata iletisi**

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .NET Framework Data Provider. It may not be installed.

**Olası nedenler**

* Oracle için .NET Framework Veri Sağlayıcısı yüklü değildi.
* Oracle için .NET Framework Veri Sağlayıcısı .NET Framework 2.0'a yüklendi ve .NET Framework 4.0 klasörlerinde bulunamadı.

**Çözünürlük**

* Oracle için .NET Sağlayıcısı'nı yüklemediyseniz, [yükleyin](https://www.oracle.com/technetwork/topics/dotnet/downloads/)ve senaryoyu yeniden deneyin.
* Sağlayıcıyı yükledikten sonra bile hata iletisini görürseniz, aşağıdaki adımları tamamlayın:
    1. .NET 2.0 için makine config dosyasını <\>sistem diski klasöründen açın :\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. **.NET için Oracle Veri Sağlayıcısı'nı**arayın. **System.data** > **DbProviderFactorys**altında aşağıdaki örnekte gösterildiği gibi bir giriş bulabilmelisiniz:`<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />`
* Bu girişi machine.config dosyasına aşağıdaki .NET 4.0 klasöründe\>kopyalayın: <sistem diski :\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config. Ardından, sürümü 4.xxx.x.x olarak değiştirin.
* <ODP.NET Yüklü\>Yol \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll'yi genel montaj önbelleğinde (GAC) **gacutil /i [sağlayıcı yolu]** çalıştırarak yükleyin.

### <a name="problem-2-datetime-formatting"></a>Sorun 2: Tarih/saat biçimlendirme

**Hata iletisi**

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Çözünürlük**

Oracle veritabanınızda tarihlerin nasıl yapılandırıldığına bağlı olarak kopyalama etkinliğinizdeki sorgu dizesini ayarlamanız gerekebilir. Aşağıda bir örnek **(to_date** işlevini kullanarak):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a>Oracle için tür eşleme

Veri hareketi [etkinliklerinde](data-factory-data-movement-activities.md)belirtildiği gibi, Kopyalama Etkinliği aşağıdaki iki adımlı yaklaşımı kullanarak kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürün.
2. .NET türünden yerel lavabo türüne dönüştürün.

Verileri Oracle'dan taşıdığınızda, Oracle veri türünden .NET türüne veya tam tersi aşağıdaki eşlemeler kullanılır:

| Oracle veri türü | .NET Framework veri türü |
| --- | --- |
| Bdosya |Bayt[] |
| Blob |Bayt[]<br/>(yalnızca Bir Microsoft sürücüsü kullandığınızda Oracle 10g ve sonraki sürümlerinde desteklenir) |
| Char |Dize |
| Clob |Dize |
| DATE |DateTime |
| Float |Ondalık, String (hassas > 28) |
| TAMSAYI |Ondalık, String (hassas > 28) |
| Aradan YıL AYA |Int32 |
| İKINCI IÇIN ARALıK GÜNÜ |TimeSpan |
| Uzun |Dize |
| UZUN HAM |Bayt[] |
| Nchar |Dize |
| Nclob |Dize |
| Numarası |Ondalık, String (hassas > 28) |
| NVARCHAR2 |Dize |
| Ham |Bayt[] |
| Rowıd |Dize |
| Zaman damgası |DateTime |
| YEREL SAAT DILIMILI ZAMAN DAMGASı |DateTime |
| SAAT DILIMILI ZAMAN DAMGASı |DateTime |
| İmzaSıZ SONDA |Sayı |
| VARCHAR2 |Dize |
| XML |Dize |

> [!NOTE]
> Microsoft sürücüsü kullandığınızda veri türleri **INTERVAL YEAR to MONTH** ve INTERVAL DAY to **SECOND** desteklenmez.

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı

Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara eşleme hakkında daha fazla bilgi edinmek için Bkz. [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme.](data-factory-map-columns.md)

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma

İlişkisel veri depolarından verileri kopyaladığınızda, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim el ile veya yeniden deneme ilkesiyle yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olun. Daha fazla bilgi için, [ilişkisel kaynaklardan tekrarlanabilir okumaya](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)bakın.

## <a name="performance-and-tuning"></a>Performans ve ayar

Azure Veri Fabrikası'ndaki veri hareketinin (Kopyalama Etkinliği) performansını etkileyen önemli etkenler hakkında bilgi edinmek için [Kopyalama Etkinliği performansı ve alıç kılavuzuna](data-factory-copy-activity-performance.md) bakın. Ayrıca optimize etmek için çeşitli yollar hakkında bilgi edinebilirsiniz.
