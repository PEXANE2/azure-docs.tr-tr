---
title: Azure Veri Fabrikası'nı kullanarak SAP HANA'daki verileri taşıma
description: Azure Veri Fabrikası'nı kullanarak SAP HANA'daki verileri nasıl taşıyabildiğini öğrenin.
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
ms.openlocfilehash: 361b98a1cde8ee5dee99a370b46d8fc8e0f5af28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265824"
---
# <a name="move-data-from-sap-hana-using-azure-data-factory"></a>Azure Veri Fabrikasını kullanarak VERILERI SAP HANA'dan taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-sap-hana-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-sap-hana.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki SAP HANA konektörüne](../connector-sap-business-warehouse.md)bakın.

Bu makalede, verileri şirket içi SAP HANA'dan taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

Verileri şirket içi SAP HANA veri deposundan desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Veri fabrikası şu anda yalnızca sap HANA'dan diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından SAP HANA'ya taşımak için değildir.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümler ve kurulum
Bu bağlayıcı SAP HANA veritabanının herhangi bir sürümünü destekler. SQL sorgularını kullanarak HANA bilgi modellerinden (Analitik ve Hesaplama görünümleri gibi) ve Satır/Sütun tablolarından veri kopyalamayı destekler.

SAP HANA örneğine bağlantıyı etkinleştirmek için aşağıdaki bileşenleri yükleyin:
- **Veri Yönetimi Ağ Geçidi**: Veri Fabrikası hizmeti, Veri Yönetimi Ağ Geçidi adı verilen bir bileşeni kullanarak şirket içi veri depolarına (SAP HANA dahil) bağlanmayı destekler. Veri Yönetimi Ağ Geçidi ve ağ geçidini ayarlamayla ilgili adım adım yönergeler hakkında bilgi edinmek [için, verileri şirket içi veri deposu arasında bulut veri deposu makalesine taşıma](data-factory-move-data-between-onprem-and-cloud.md) başlıklı bakın. SAP HANA bir Azure IaaS sanal makinede (VM) barındırılabilse bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabildiği sürece ağ geçidini veri deposuyla aynı VM'ye veya farklı bir VM'ye yükleyebilirsiniz.
- Ağ geçidi makinesinde **SAP HANA ODBC sürücüsü.** SAP HANA ODBC sürücüsünü [SAP Software Download Center](https://support.sap.com/swdc) sayfasından indirebilirsiniz. **Windows için SAP HANA CLIENT**anahtar kelimesi ile arama yapın. 

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak şirket içi SAP HANA veri deposundan veri aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz. 

- Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md) 
- Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın. 

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. 
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** 

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız.  Şirket içi SAP HANA'daki verileri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: Sap HANA'dan Azure Blob bölümüne verileri kopyalayın.](#json-example-copy-data-from-sap-hana-to-azure-blob) 

Aşağıdaki bölümler, SAP HANA veri deposuna özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, SAP HANA bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
sunucu | SAP HANA örneğinin bulunduğu sunucunun adı. Sunucunuz özelleştirilmiş bir bağlantı noktası `server:port`kullanıyorsa, belirtin. | string | Evet
authenticationType | Kimlik doğrulama türü. | kullanabilirsiniz. "Temel" veya "Windows" | Evet 
kullanıcı adı | SAP sunucusuna erişimi olan kullanıcının adı | string | Evet
password | Kullanıcının parolası. | string | Evet
ağ geçidiAdı | Veri Fabrikası hizmetinin şirket içi SAP HANA örneğine bağlanmak için kullanması gereken ağ geçidinin adı. | string | Evet
şifreli Credential | Şifrelenmiş kimlik bilgisi dizesi. | string | Hayır

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **İlişkisel Tablo**türü SAP HANA veri kümesi için desteklenen türe özgü özellikler yoktur. 


## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için ilkeler mevcuttur.

Oysa, etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Kopyalama etkinliğindeki kaynak **RelationalSource** türünden (SAP HANA'yı içerir) olduğunda, typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu | SAP HANA örneğindeki verileri okumak için SQL sorgusunu belirtir. | SQL sorgusu. | Evet |

## <a name="json-example-copy-data-from-sap-hana-to-azure-blob"></a>JSON örneği: SAP HANA'dan Azure Blob'a veri kopyalama
Aşağıdaki örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Bu örnek, şirket içi SAP HANA'daki verilerin Azure Blob Depolama alanına nasıl kopyalanır olduğunu gösterir. Ancak, veriler Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) listelenen lavabolardan herhangi biri için **doğrudan** kopyalanabilir.  

> [!IMPORTANT]
> Bu örnek JSON parçacıkları sağlar. Veri fabrikası oluşturmak için adım adım yönergeler içermez. Adım adım yönergeler için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında hareketli veri bakın.

Örnekte aşağıdaki veri fabrikası varlıkları vardır:

1. [SapHana](#linked-service-properties)türünde bağlantılı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [İlişkisel Tablo](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity içeren bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, verileri SAP HANA örneğinden bir Azure örneğine saatlik olarak kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidini kur. Yönergeler, [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasındaki hareketli verilerde dir.

### <a name="sap-hana-linked-service"></a>SAP HANA bağlantılı hizmet
Bu bağlantılı hizmet, SAP HANA örneğini veri fabrikasına bağlar. Tür özelliği **SapHana**olarak ayarlanır. typeProperties bölümü SAP HANA örneği için bağlantı bilgileri sağlar.

```json
{
    "name": "SapHanaLinkedService",
    "properties":
    {
        "type": "SapHana",
        "typeProperties":
        {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
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

### <a name="sap-hana-input-dataset"></a>SAP HANA giriş veri seti

Bu veri kümesi SAP HANA veri kümesini tanımlar. Veri Fabrikası veri kümesinin türünü **İlişkisel Tablo**olarak ayarlarsınız. Şu anda, sap HANA veri kümesi için türe özgü özellikler belirtmezsiniz. Kopyalama Etkinliği tanımındaki sorgu, SAP HANA örneğinden hangi verilerin okunacak olduğunu belirtir. 

Dış özelliğin doğru olarak ayarlanması, Veri Fabrikası hizmetine tablonun veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

Sıklık ve aralık özellikleri zamanlamayı tanımlar. Bu durumda, veriler SAP HANA örneğinden saatlik olarak okunur. 

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
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
Bu veri kümesi, azure blob veri kümesiçıktısını tanımlar. Tür özelliği AzureBlob olarak ayarlanır. TypeProperties bölümü, SAP HANA örneğinden kopyalanan verilerin depolandığı yeri sağlar. Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/saphana/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **RelationalSource** (SAP HANA kaynak için) olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **Sorgu** özelliği için belirtilen SQL sorgusu, kopyalanacak son saatteki verileri seçer.

```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<SQL Query for HANA>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapHanaDataset"
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
                "name": "SapHanaToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```


### <a name="type-mapping-for-sap-hana"></a>SAP HANA için tür eşleme
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama etkinliği aşağıdaki iki adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

SAP HANA'dan veri aktarırken, sap HANA türlerinden .NET türlerine aşağıdaki eşlemeler kullanılır.

SAP HANA Tipi | .NET Tabanlı Tip
------------- | ---------------
Tinyint | Bayt
Smallint | Int16
INT | Int32
Bigint | Int64
GERÇEK SAYI | Tek
Çift | Tek
On -da -lık | Ondalık
Boolean | Bayt
Varchar | Dize
Nvarchar | Dize
Clob | Bayt[]
ALFANÜM | Dize
Blob | Bayt[]
DATE | DateTime
TIME | TimeSpan
Zaman damgası | DateTime
İkinciTARIH | DateTime

## <a name="known-limitations"></a>Bilinen sınırlamalar
SAP HANA'dan veri kopyalarken bilinen birkaç sınırlama vardır:

- NVARCHAR dizeleri 4000 Unicode karakter olan maksimum uzunluğa göre kısaltılır
- SMALLDECIMAL desteklenmez
- VARBINARY desteklenmez
- Geçerli Tarihler 1899/12/30 ile 9999/12/31 arasıdır

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara eşleme hakkında bilgi edinmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir. Bkz. [İlişkisel kaynaklardan tekrarlanabilir okuma](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
