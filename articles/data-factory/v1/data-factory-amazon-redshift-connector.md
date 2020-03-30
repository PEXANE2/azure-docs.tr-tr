---
title: Azure Veri Fabrikası'nı kullanarak Verileri Amazon Redshift'ten taşıma
description: Azure Veri Fabrikası Kopyalama Etkinliği'ni kullanarak verileri Amazon Redshift'ten nasıl taşıyarak nasıl taşıyarak edinebilirsiniz.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: c2e2394bbcee5294bfb752a0af2969457ffff0ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260533"
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Azure Veri Fabrikasını kullanarak Verileri Amazon Redshift'ten Taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-amazon-redshift-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-amazon-redshift.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Amazon Redshift konektörüne](../connector-amazon-redshift.md)bakın.

Bu makalede, Verileri Amazon Redshift'ten taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Makale, kopyalama etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesi ne dayanıyor?

Veri Fabrikası şu anda yalnızca Amazon Redshift'ten desteklenen bir [lavabo veri deposuna](data-factory-data-movement-activities.md#supported-data-stores-and-formats)veri taşımayı desteklemektedir. Diğer veri depolarından Amazon Redshift'e veri taşınması desteklenmez.

> [!TIP]
> Amazon Redshift'ten büyük miktarda veriyi kopyalarken en iyi performansı elde etmek için, Yerleşik Redshift **UNLOAD** komutunu Amazon Basit Depolama Hizmeti (Amazon S3) aracılığıyla kullanmayı düşünün. Ayrıntılar için Amazon [Redshift'ten verileri kopyalamak için UNLOAD kullan'a](#use-unload-to-copy-data-from-amazon-redshift)bakın.

## <a name="prerequisites"></a>Ön koşullar
* Verileri şirket içi veri deposuna taşıyorsanız, şirket içi bir makineye [Veri Yönetimi Ağ Geçidi'ni](data-factory-data-management-gateway.md) yükleyin. Şirket içi makine IP adresini kullanarak Amazon Redshift kümesine erişim izni ver. Yönergeler için [bkz.](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html)
* Verileri bir Azure veri deposuna taşımak için [Microsoft Azure Veri Merkezleri tarafından kullanılan İşlem IP adresi ve SQL aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)bakın.

## <a name="getting-started"></a>Başlarken
Farklı araçlar ve API'ler kullanarak verileri Bir Amazon Redshift kaynağından taşımak için bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

Bir ardışık iş tonu oluşturmanın en kolay yolu Azure Veri Fabrikası Kopyalama Sihirbazı'nı kullanmaktır. Kopya Sihirbazı'nı kullanarak bir ardışık kaynak oluşturma konusunda hızlı bir geçiş [için, Bkz. Öğretici: Kopya Sihirbazı'nı kullanarak bir ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Visual Studio, Azure PowerShell veya diğer araçları kullanarak bir boru hattı da oluşturabilirsiniz. Azure Kaynak Yöneticisi şablonları, .NET API veya REST API de ardışık kaynak oluşturmak için kullanılabilir. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeler için [Etkinlik Kopyala öğreticisine](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için bağlantılı hizmetler oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek veri kümeleri oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir ardışık işlem oluşturma.

Kopyalama Sihirbazı'nı kullandığınızda, bu Veri Fabrikası varlıkları için JSON tanımları otomatik olarak oluşturulur. Araçlar veya API'ler (.NET API hariç) kullandığınızda, JSON biçimini kullanarak Veri Fabrikası varlıklarını tanımlarsınız. JSON örneği: Amazon Redshift'ten Azure Blob depolamasına kadar olan verileri kopyalamak, Bir Amazon Redshift veri deposundan veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlarını gösterir.

Aşağıdaki bölümlerde Amazon Redshift için Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri açıklanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki tablo, Bir Amazon Redshift bağlantılı hizmete özgü JSON öğeleri için açıklamalar sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **Türü** |Bu özellik **AmazonRedshift**olarak ayarlanmalıdır. |Evet |
| **sunucu** |Amazon Redshift sunucusunun IP adresi veya ana bilgisayar adı. |Evet |
| **Bağlantı noktası** |Amazon Redshift sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktasının sayısı. |Hayır (varsayılan değer 5439'dur) |
| **Veritabanı** |Amazon Redshift veritabanının adı. |Evet |
| **Username** |Veritabanına erişimi olan kullanıcının adı. |Evet |
| **parola** |Kullanıcı hesabının şifresi. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. **Yapı,** **kullanılabilirlik**ve **ilke** bölümleri tüm veri kümesi türleri için benzerdir. Veri kümesi türlerine örnek olarak Azure SQL, Azure Blob depolama ve Azure Tablo depolama sayılabilir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve depodaki verilerin konumu hakkında bilgi sağlar. Amazon Redshift veri kümesini içeren **RelationalTable**türünden bir veri kümesinin **typeProperties** bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **Tablename** |Bağlı hizmetin atıfta bulunduğu Amazon Redshift veritabanındaki tablonun adı. |Hayır **(RelationalSource** türündeki bir kopya etkinliğinin **sorgu** özelliği belirtilmişse) |

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. **Ad,** **açıklama,** **giriş** tablosu, **çıktılar** tablosu ve **ilke** özellikleri tüm etkinlik türleri için kullanılabilir. **TypeProperties** bölümünde bulunan özellikler her etkinlik türü için değişir. Kopyalama Etkinliği için özellikler veri kaynakları ve lavabo türlerine bağlı olarak değişir.

Copy Activity için, kaynak **AmazonRedshiftSource**türünde olduğunda, aşağıdaki özellikler **typeProperties** bölümünde mevcuttur:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **Sorgu** | Verileri okumak için özel sorguyu kullanın. |Hayır (bir veri kümesinin **tabloAdı** özelliği belirtilirse) |
| **redshiftUnloadAyarlar** | Redshift **UNLOAD** komutunu kullanırken özellik grubunu içerir. | Hayır |
| **s3LinkedServiceName** | Amazon S3 geçici bir mağaza olarak kullanmak. Bağlantılı hizmet, **AwsAccessKey**türünden bir Azure Veri Fabrikası adı kullanılarak belirtilir. | **RedshiftUnloadSettings** özelliğini kullanırken gereklidir |
| **bucketName** | Ara verileri depolamak için kullanılacak Amazon S3 kovasını gösterir. Bu özellik sağlanamazsa, Kopyalama Etkinliği otomatik olarak bir kova oluşturur. | **RedshiftUnloadSettings** özelliğini kullanırken gereklidir |

Alternatif olarak, Amazon Redshift içeren **RelationalSource** türünü **typeProperties** bölümünde aşağıdaki özelliği ile kullanabilirsiniz. Bu kaynak türünün Redshift **UNLOAD** komutunu desteklemediğini unutmayın.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **Sorgu** |Verileri okumak için özel sorguyu kullanın. | Hayır (bir veri kümesinin **tabloAdı** özelliği belirtilirse) |

## <a name="use-unload-to-copy-data-from-amazon-redshift"></a>Amazon Redshift'ten verileri kopyalamak için UNLOAD'ı kullanın

Amazon Redshift [**UNLOAD**](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) komutu, bir sorgunun sonuçlarını Amazon S3'teki bir veya daha fazla dosyaya boşaltır. Bu komut, Redshift'ten büyük veri kümelerini kopyalamak için Amazon tarafından önerilir.

**Örnek: Verileri Amazon Redshift'ten Azure SQL Veri Ambarı'na kopyalama**

Bu örnek, Amazon Redshift'ten Azure SQL Veri Ambarı'na kadar olan verileri kopyalar. Örnek, Redshift **UNLOAD** komutunu, aşamalı kopya verilerini ve Microsoft PolyBase'i kullanır.

Bu örnek kullanım örneği için, Copy Activity önce **redshiftUnloadSettings** seçeneğinde yapılandırıldığı gibi Amazon Redshift'ten Amazon S3'e verileri boşaltır. Ardından, **veriler, evreleme Ayarlar** seçeneğinde belirtildiği şekilde Amazon S3'ten Azure Blob depolamasına kopyalanır. Son olarak, PolyBase verileri SQL Veri Ambarı'na yükler. Tüm ara biçimleri Copy Activity tarafından işlenir.

![Amazon Redshift'ten SQL Veri Ambarı'na iş akışını kopyala](media/data-factory-amazon-redshift-connector/redshift-to-sql-dw-copy-workflow.png)

```json
{
    "name": "CopyFromRedshiftToSQLDW",
    "type": "Copy",
    "typeProperties": {
        "source": {
            "type": "AmazonRedshiftSource",
            "query": "select * from MyTable",
            "redshiftUnloadSettings": {
                "s3LinkedServiceName":"MyAmazonS3StorageLinkedService",
                "bucketName": "bucketForUnload"
            }
        },
        "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyAzureStorageLinkedService",
            "path": "adfstagingcopydata"
        },
        "cloudDataMovementUnits": 32
        .....
    }
}
```

## <a name="json-example-copy-data-from-amazon-redshift-to-azure-blob-storage"></a>JSON örneği: Verileri Amazon Redshift'ten Azure Blob depolamaalanına kopyalama
Bu örnek, bir Amazon Redshift veritabanından Azure Blob Depolama'ya verilerin nasıl kopyalanır olduğunu gösterir. Veriler, Kopyalama Etkinliği kullanılarak desteklenen herhangi bir [lavaboya](data-factory-data-movement-activities.md#supported-data-stores-and-formats) doğrudan kopyalanabilir.

Örnekte aşağıdaki veri fabrikası varlıkları vardır:

* [AmazonRedshift](#linked-service-properties) türü ne bağlı bir hizmet
* [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
* [Tür İlişkisel Tablo'nun](#dataset-properties) giriş [veri kümesi](data-factory-create-datasets.md)
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties) türünden bir çıktı [veri kümesi](data-factory-create-datasets.md)
* [İlişkisel Kaynak](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) özelliklerini kullanan bir kopyalama etkinliği olan bir [ardışık hat](data-factory-create-pipelines.md)

Örnek, amazon redshift'inden bir Azure blob'una saatlik olarak bir sorgu sonucundaki verileri kopyalar. Örnekte kullanılan JSON özellikleri varlık tanımlarını izleyen bölümlerde açıklanmıştır.

**Amazon Redshift bağlantılı hizmet**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< The IP address or host name of the Amazon Redshift server >",
            "port": "<The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>",
            "database": "<The database name of the Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

**Azure Blob depolama bağlantılı hizmet**

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
**Amazon Redshift giriş dataset**

**Dış** özellik, Veri Fabrikası hizmetine veri kümesinin veri fabrikasının dışında olduğunu bildirmek için "true" olarak ayarlanır. Bu özellik ayarı, veri kümesinin veri fabrikasındaki bir etkinlik tarafından üretilmediğini gösterir. El üstündeki bir etkinlik tarafından üretilmeyen bir giriş veri kümesinde özelliği doğru şekilde ayarlayın.

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

**Azure Blob çıktı veri kümesi**

Veri, **frekans** özelliğini "Saat" ve **aralık** özelliği 1 olarak ayarlayarak her saat yeni bir blob'a yazılır. Blob için **folderPath** özelliği dinamik olarak değerlendirilir. Özellik değeri, işlenen dilimin başlangıç saatini temel adatır. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Azure Redshift kaynağı (türü İlişkisel Kaynak) ve Azure Blob lavabosu içeren bir ardışık ardışık ardışık ardışık ardışık alanda etkinliği kopyalama**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan bir kopyalama etkinliği içerir. Boru hattının her saat başı çalışması planlanıyor. Boru hattı için JSON tanımında, **kaynak** türü **RelationalSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **Sorgu** özelliği için belirtilen SQL sorgusu, son bir saat içinde kopyalanacak verileri seçer.

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "AmazonRedshiftSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)",
                        "redshiftUnloadSettings": {
                            "s3LinkedServiceName":"myS3Storage",
                            "bucketName": "bucketForUnload"
                        }
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    },
                    "cloudDataMovementUnits": 32
                },
                "inputs": [
                    {
                        "name": "AmazonRedshiftInputDataset"
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
                "name": "AmazonRedshiftToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-amazon-redshift"></a>Amazon Redshift için tip haritalama
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama Etkinliği kaynak türünden lavabo türüne otomatik tür dönüştürmeleri gerçekleştirir. Türleri iki adımlı bir yaklaşım kullanılarak dönüştürülür:

1. Yerel kaynak türünden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

Kopyalama Etkinliği verileri Amazon Redshift türünden bir .NET türüne dönüştürdüğünde aşağıdaki eşlemeler kullanılır:

| Amazon Redshift tipi | .NET türü |
| --- | --- |
| Smallint |Int16 |
| TAMSAYI |Int32 |
| Bigint |Int64 |
| On -da -lık |Ondalık |
| GERÇEK SAYI |Tek |
| ÇİFT HASSASLIK |Çift |
| Boolean |Dize |
| Char |Dize |
| Varchar |Dize |
| DATE |DateTime |
| Zaman damgası |DateTime |
| TEXT |Dize |

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara nasıl eşlendireceklerini öğrenmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-reads-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okumalar
İlişkisel bir veri deposundan veri kopyaladığınızda, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme **ilkesini** de yapılandırabilirsiniz. Dilim kaç kez yeniden çalıştırılırsa çalıştırılırsa okunsun, aynı verilerin okunduğundan emin olun. Ayrıca, dilimi nasıl yeniden çalıştırdığınıza bakılmaksızın aynı verilerin okunduğundan emin olun. Daha fazla bilgi için bkz: [İlişkisel kaynaklardan tekrarlanabilir okumalar.](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve ayar
Kopyalama Etkinliği performansını etkileyen önemli etkenler ve Kopyalama [Etkinliği Performansı ve Tuning Kılavuzu'ndaki](data-factory-copy-activity-performance.md)performansı optimize etmenin yolları hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama Etkinliği ile bir ardışık hatlar oluşturmak için adım adım yönergeler için, [Etkinliği Kopyala öğreticiye](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)bakın.
