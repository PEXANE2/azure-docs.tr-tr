---
title: Azure Veri Fabrikası'nı kullanarak VERILERI SAP İş Ambarından taşıma
description: Azure Veri Fabrikası'nı kullanarak SAP İş Ambarı'ndaki verileri nasıl taşıyabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 108bdf057cd375e28b10a6838ec5c8c6f57749a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281060"
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a>Azure Veri Fabrikasını kullanarak verileri SAP İş Ambarından Taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-sap-business-warehouse-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-sap-business-warehouse.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki SAP İş Ambarı konektörüne](../connector-sap-business-warehouse.md)bakın.


Bu makalede, verileri şirket içi SAP İş Ambarı'ndan (BW) taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

Verileri şirket içi SAP İş Ambarı veri deposundan desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Veri fabrikası şu anda yalnızca bir SAP İş Ambarından diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından SAP İş Ambarına taşımak için değildir. 

## <a name="supported-versions-and-installation"></a>Desteklenen sürümler ve kurulum
Bu bağlayıcı SAP İş Ambarı sürüm 7.x'i destekler. MDX sorgularını kullanarak InfoCubes ve QueryCubes (BEx sorguları dahil) verilerinin kopyalanması destekler.

SAP BW örneğine bağlantıyı etkinleştirmek için aşağıdaki bileşenleri yükleyin:
- **Veri Yönetimi Ağ Geçidi**: Veri Fabrikası hizmeti, Veri Yönetimi Ağ Geçidi adı verilen bir bileşeni kullanarak şirket içi veri depolarına (SAP İş Ambarı dahil) bağlanmayı destekler. Veri Yönetimi Ağ Geçidi ve ağ geçidini ayarlamayla ilgili adım adım yönergeler hakkında bilgi edinmek [için, verileri şirket içi veri deposu arasında bulut veri deposu makalesine taşıma](data-factory-move-data-between-onprem-and-cloud.md) başlıklı bakın. SAP İş Ambarı bir Azure IaaS sanal makinede (VM) barındırılan olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabildiği sürece ağ geçidini veri deposuyla aynı VM'ye veya farklı bir VM'ye yükleyebilirsiniz.
- Ağ geçidi makinesinde **SAP NetWeaver kitaplığı.** SAP Netweaver kitaplığını SAP yöneticinizden veya doğrudan [SAP Yazılım İndirme Merkezi'nden](https://support.sap.com/swdc)alabilirsiniz. En son sürümün indirme konumunu almak için **SAP Not #1025361'ni** arayın. SAP NetWeaver kitaplığı (32 bit veya 64 bit) mimarisinin ağ geçidi yüklemenizle eşleştiğinden emin olun. Ardından SAP Not'a göre SAP NetWeaver RFC SDK'da bulunan tüm dosyaları yükleyin. SAP NetWeaver kitaplığı, SAP İstemci Araçları yüklemesinde de yer alır.

> [!TIP]
> NetWeaver RFC SDK'dan çıkarılan dll'leri system32 klasörüne koyun.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak şirket içi Cassandra veri deposundan veri aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz. 

- Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md) 
- Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın. 

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. 
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** 

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız.  Şirket içi SAP İş Ambarı'ndaki verileri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: Verileri SAP İş Ambarı'ndan Azure Blob bölümüne](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) kopyalayın. 

Aşağıdaki bölümler, SAP BW veri deposuna özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, SAP İş Ambarı (BW) bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
sunucu | SAP BW örneğinin bulunduğu sunucunun adı. | string | Evet
systemNumber | SAP BW sisteminin sistem numarası. | Dize olarak temsil edilen iki basamaklı ondalık sayı. | Evet
clientId | SAP W sistemindeki istemci kimliği. | Dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet
kullanıcı adı | SAP sunucusuna erişimi olan kullanıcının adı | string | Evet
password | Kullanıcının parolası. | string | Evet
ağ geçidiAdı | Veri Fabrikası hizmetinin şirket içi SAP BW örneğine bağlanmak için kullanması gereken ağ geçidinin adı. | string | Evet
şifreli Credential | Şifrelenmiş kimlik bilgisi dizesi. | string | Hayır

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **İlişkisel Tablo**türü SAP BW veri kümesi için desteklenen türe özgü özellikler yoktur. 


## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için ilkeler mevcuttur.

Oysa, etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Kopyalama etkinliğindeki kaynak **RelationalSource** türünden (SAP BW içerir) olduğunda, typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu | SAP BW örneğindeki verileri okumak için MDX sorgusunu belirtir. | MDX sorgusu. | Evet |


## <a name="json-example-copy-data-from-sap-business-warehouse-to-azure-blob"></a>JSON örneği: SAP İş Ambarı'ndan Azure Blob'a veri kopyalama
Aşağıdaki örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Bu örnek, şirket içi SAP İş Ambarı'ndan Azure Blob Depolama alanına verilerin nasıl kopyalanır olduğunu gösterir. Ancak veriler, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) belirtilen lavabolardan herhangi biri için **doğrudan** kopyalanabilir.  

> [!IMPORTANT]
> Bu örnek JSON parçacıkları sağlar. Veri fabrikası oluşturmak için adım adım yönergeler içermez. Adım adım yönergeler için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında hareketli veri bakın.

Örnekte aşağıdaki veri fabrikası varlıkları vardır:

1. [SapBw](#linked-service-properties)türünde bağlantılı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [İlişkisel Tablo](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity içeren bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, sap İş ambarı örneğindeki verileri saatlik bir Azure blob'una kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidini kur. Yönergeler, [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasındaki hareketli verilerde dir.

### <a name="sap-business-warehouse-linked-service"></a>SAP Business Warehouse bağlantılı hizmet
Bu bağlantılı hizmet, SAP BW örneğini veri fabrikasına bağlar. Tür özelliği **SapBw**olarak ayarlanır. typeProperties bölümü SAP BW örneği için bağlantı bilgileri sağlar. 

```json
{
    "name": "SapBwLinkedService",
    "properties":
    {
        "type": "SapBw",
        "typeProperties":
        {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti
Bu bağlantılı hizmet, Azure Depolama hesabınızı veri fabrikasına bağlar. Tür özelliği **AzureStorage**olarak ayarlanır. typeProperties bölümü, Azure Depolama hesabı için bağlantı bilgileri sağlar.

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

### <a name="sap-bw-input-dataset"></a>SAP BW giriş veri seti
Bu veri kümesi SAP İş Ambarı veri kümesini tanımlar. Veri Fabrikası veri kümesinin türünü **İlişkisel Tablo**olarak ayarlarsınız. Şu anda, sap BW veri kümesi için türe özgü özellikler belirtmezsiniz. Etkinlik Kopyala tanımındaki sorgu, SAP BW örneğinden hangi verilerin okunacak olduğunu belirtir. 

Dış özelliğin doğru olarak ayarlanması, Veri Fabrikası hizmetine tablonun veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

Sıklık ve aralık özellikleri zamanlamayı tanımlar. Bu durumda, veriler SAP BW örneğinden saatlik olarak okunur. 

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```



### <a name="azure-blob-output-dataset"></a>Azure Blob çıktı veri kümesi
Bu veri kümesi, azure blob veri kümesiçıktısını tanımlar. Tür özelliği AzureBlob olarak ayarlanır. TypeProperties bölümü, SAP BW örneğinden kopyalanan verilerin depolandığı yeri sağlar. Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sapbw/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a>Kopyalama etkinliği olan boru hattı
Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **RelationalSource** (SAP BW kaynak için) olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. Sorgu özelliği için belirtilen **sorgu,** kopyalanacak son saatteki verileri seçer.

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<MDX query for SAP BW>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapBwDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDataSet"
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
                "name": "SapBwToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```



### <a name="type-mapping-for-sap-bw"></a>SAP BW için tür eşleme
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama etkinliği aşağıdaki iki adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

SAP BW'den veri aktarırken, sap BW türlerinden .NET türlerine aşağıdaki eşlemeler kullanılır.

ABAP Sözlüğü'nde veri türü | .NET Veri Türü
-------------------------------- | --------------
ACCP |  int
Char | Dize
CLNT | Dize
CURR | Ondalık
CUKY | Dize
Aralık | Ondalık
FLTP | Çift
INT1 | Bayt
INT2 | Int16
INT4 | int
Lang | Dize
LCHR | Dize
LRAW | Bayt[]
PREC | Int16
Quan | Ondalık
Ham | Bayt[]
RAWSTRING | Bayt[]
Dize | Dize
Birim | Dize
DATS | Dize
NUMC | Dize
TİmS | Dize

> [!NOTE]
> Kaynak veri kümesinden sütunlara kadar sütunları haritalamak için Azure [Veri Fabrikası'nda veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.


## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara eşleme hakkında bilgi edinmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir. Bkz. [İlişkisel kaynaklardan tekrarlanabilir okuma](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
