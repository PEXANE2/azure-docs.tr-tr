---
title: Azure Veri Fabrikası'nı kullanarak verileri Sybase'den taşıma
description: Azure Veri Fabrikası'nı kullanarak Sybase Veritabanı'ndan verileri nasıl taşıyabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: b379ee10-0ff5-4974-8c87-c95f82f1c5c6
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/02/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: cefa0c15dd50f95780034dcb63f888a2e1c6b65e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281255"
---
# <a name="move-data-from-sybase-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Sybase'den taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-onprem-sybase-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-sybase.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Sybase konektörüne](../connector-sybase.md)bakın.

Bu makalede, verileri şirket içi Sybase veritabanından taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

Verileri şirket içi Sybase veri deposundan desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Veri fabrikası şu anda yalnızca bir Sybase veri deposundan diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından Sybase veri deposuna taşımak için değildir. 

## <a name="prerequisites"></a>Ön koşullar
Veri Fabrikası hizmeti, Veri Yönetimi Ağ Geçidi'ni kullanarak şirket içi Sybase kaynaklarına bağlanmayı destekler. Veri Yönetimi Ağ Geçidi ve ağ geçidini ayarlama yla ilgili adım adım talimatlar hakkında bilgi edinmek için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında taşınan verileri görün.

Sybase veritabanı bir Azure IaaS VM'de barındırılan olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabildiği sürece ağ geçidini veri deposuyla aynı IaaS VM'ye veya farklı bir VM'ye yükleyebilirsiniz.

> [!NOTE]
> Sorun giderme bağlantısı/ağ geçidi yle ilgili sorunlarla ilgili ipuçları için [sorun giderme ağ geçidi sorunlarına](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bakın.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümler ve kurulum
Veri Yönetimi Ağ Geçidi'nin Sybase Veritabanı'na bağlanması için, [Sybase iAnywhere.Data.SQLAnywhere](https://go.microsoft.com/fwlink/?linkid=324846) 16 veya üzeri veri sağlayıcısını Veri Yönetimi Ağ Geçidi ile aynı sisteme yüklemeniz gerekir. 

SAP Sybase SQL Anywhere (ASA) sürüm 16 ve üzeri desteklenir; IQ ve ASE desteklenmez.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak şirket içi Cassandra veri deposundan veri aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz. 

- Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md) 
- Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın. 

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. 
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** 

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız.  Şirket içi Bir Sybase veri deposundan verileri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: Verileri Sybase'den Azure Blob bölümüne](#json-example-copy-data-from-sybase-to-azure-blob) kopyalayın. 

Aşağıdaki bölümler, Bir Sybase veri deposuna özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, Sybase bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **OnPremisesSybase** |Evet |
| sunucu |Sybase sunucusunun adı. |Evet |
| database |Sybase veritabanının adı. |Evet |
| Şema |Veritabanındaki şemanın adı. |Hayır |
| authenticationType |Sybase veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, Temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulaması kullanıyorsanız kullanıcı adını belirtin. |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Hayır |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi Sybase veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

typeProperties bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **Tür İlişkisel Tablo** (Sybase dataset içeren) veri kümesi için **typeProperties** bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin başlettiği Sybase Veritabanı örneğindeki tablonun adı. |Hayır **(RelationalSource** **sorgusu** belirtilirse) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi için [bkz.](data-factory-create-pipelines.md) Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

Oysa, etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Kaynak **RelationalSource** türünden (Sybase'i içerir) olduğunda, **typeProperties** bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: MyTable'dan * seçin. |Hayır **(veri kümesinin** **tablo adı** belirtilirse) |


## <a name="json-example-copy-data-from-sybase-to-azure-blob"></a>JSON örneği: Verileri Sybase'den Azure Blob'a kopyalama
Aşağıdaki örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Sybase veritabanından Azure Blob Depolama'ya verilerin nasıl kopyalanır olduğunu gösterirler. Ancak, veriler Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) belirtilen lavabolardan herhangi biri için kopyalanabilir.   

Örnekte aşağıdaki veri fabrikası varlıkları vardır:

1. [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties)türüne bağlı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türünde beğenilen bir hizmet.
3. [İlişkisel Tablo](data-factory-onprem-sybase-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity ile [boru hattı.](data-factory-create-pipelines.md)

Örnek, Sybase veritabanındaki bir sorgu sonucundaki verileri her saat başı bir blob'a kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidini kur. Yönergeler, [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasındaki hareketli verilerde dir.

**Sybase bağlantılı hizmet:**

```JSON
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
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

**Azure Blob depolama bağlantılı hizmet:**

```JSON
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

**Sybase giriş veri kümesi:**

Örnek, Sybase'de "MyTable" tablosu oluşturduğunuzu varsayar ve zaman serisi verileri için "zaman damgası" adı verilen bir sütun içerir.

"Harici" ayarı: True, Veri Fabrikası hizmetine bu veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir. Bağlı hizmet in **türünün** şu şekilde ayarlıolduğuna dikkat edin: **RelationalTable**.

```JSON
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
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

**Azure Blob çıktı veri seti:**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```JSON
{
    "name": "AzureBlobSybaseDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopyalama etkinliği olan Boru Hattı:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve saatlik çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **RelationalSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **Sorgu** özelliği için belirtilen SQL sorgusu DBA'daki verileri seçer. Veritabanındaki sipariş tablosu.

```JSON
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from DBA.Orders"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "SybaseDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobSybaseDataSet"
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
                "name": "SybaseToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-sybase"></a>Sybase için tür eşleme
[Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama etkinliği aşağıdaki 2 adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

Sybase, T-SQL ve T-SQL türlerini destekler. SQL türlerinden .NET türüne eşleme tablosu için [Azure SQL Bağlayıcısı](data-factory-azure-sql-connector.md) makalesine bakın.

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara eşleme hakkında bilgi edinmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir. Bkz. [İlişkisel kaynaklardan tekrarlanabilir okuma.](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
