---
title: Veri Fabrikası'nı kullanarak Verileri Amazon Basit Depolama Hizmeti'nden taşıma
description: Azure Veri Fabrikası'nı kullanarak Verileri Amazon Basit Depolama Hizmeti'nden (S3) nasıl taşıyacağım hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: b23be9901df7ca435f412d9f49e1a7ad88382ade
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281658"
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Amazon Basit Depolama Hizmeti'nden taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-amazon-simple-storage-service-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-amazon-simple-storage-service.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Amazon S3 konektörüne](../connector-amazon-simple-storage-service.md)bakın.

Bu makalede, Verileri Amazon Basit Depolama Hizmeti'nden (S3) taşımak için Azure Veri Fabrikası'ndaki kopyalama etkinliğinin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanıyor.

Amazon S3'ün verilerini desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Veri Fabrikası şu anda sadece Amazon S3 diğer veri depolarına veri taşıma destekler, ancak Amazon S3 diğer veri mağazalarından veri taşıma değil.

## <a name="required-permissions"></a>Gerekli izinler
Amazon S3'teki verileri kopyalamak için, size aşağıdaki izinlerin verildiğinden emin olun:

* `s3:GetObject`ve `s3:GetObjectVersion` Amazon S3 Nesne İşlemleri için.
* `s3:ListBucket`Amazon S3 Bucket İşlemleri için. Veri Fabrikası Kopyalama Sihirbazı kullanıyorsanız, `s3:ListAllMyBuckets` ayrıca gereklidir.

Amazon S3 izinlerinin tam listesi hakkında ayrıntılı bilgi [için](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html)bkz.

## <a name="getting-started"></a>Başlarken
Farklı araçlar veya API'ler kullanarak verileri bir Amazon S3 kaynağından hareket ettiren bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Hızlı bir geçiş için [Bkz. Öğretici: Kopya Sihirbazı'nı kullanarak bir ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeler için [Kopyalama etkinliği öğreticisine](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.**

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçlar veya API'ler (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. Bir Amazon S3 veri deposundan veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek için [JSON örneğine bakın: Verileri Amazon S3'ten Azure Blob bölümüne](#json-example-copy-data-from-amazon-s3-to-azure-blob-storage) kopyalayın.

> [!NOTE]
> Kopyalama etkinliği için desteklenen dosya ve sıkıştırma biçimleri hakkında ayrıntılar için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md)bakın.

Aşağıdaki bölümlerde, Amazon S3'e özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi verilmiştir.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Bağlantılı bir hizmet, bir veri deposuna veri fabrikasına bağlanır. Amazon S3 veri deponuzu veri fabrikanıza bağlamak için **AwsAccessKey** türünden bağlantılı bir hizmet oluşturursunuz. Aşağıdaki tablo, Amazon S3 (AwsAccessKey) bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| accessKeyID |Gizli erişim anahtarının kimliği. |string |Evet |
| secretAccessKey |Gizli erişim anahtarının kendisi. |Şifreli gizli dize |Evet |

>[!NOTE]
>Bu bağlayıcı, Amazon S3'ten gelen verileri kopyalamak için IAM hesabının erişim anahtarlarını gerektirir. [Geçici Güvenlik Kimlik Bilgisi](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) desteklenmez.
>

Örnek aşağıda verilmiştir:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Azure Blob depolamasında giriş verilerini temsil edecek bir veri kümesi belirtmek için, veri kümesinin tür özelliğini **AmazonS3**olarak ayarlayın. Veri kümesinin **linkedServiceName** özelliğini Amazon S3 bağlantılı hizmetin adına ayarlayın. Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](data-factory-create-datasets.md) 

Yapı, kullanılabilirlik ve ilke gibi bölümler tüm veri kümesi türleri için benzerdir (SQL veritabanı, Azure blob ve Azure tablosu gibi). **typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **AmazonS3** türünden bir veri kümesi (Amazon S3 veri kümesini içerir) için **typeProperties** bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| bucketName |S3 kova adı. |Dize |Evet |
| anahtar |S3 nesne anahtarı. |Dize |Hayır |
| Önek |S3 nesne anahtarı için önek. Anahtarları bu önek ile başlayan nesneler seçilir. Yalnızca anahtar boşolduğunda geçerlidir. |Dize |Hayır |
| version |S3 sürümü etkinse, S3 nesnesinin sürümü. |Dize |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [JSON biçimi,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro formatı,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork formatı](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke formatında](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlere bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında olduğu gibi kopyalamak istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. | |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeleri şunlardır: **Optimal** ve **En Hızlı**. Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. | |Hayır |


> [!NOTE]
> **bucketName + tuşu,** kovanın S3 nesnelerinin kök kapsayıcısı olduğu S3 nesnesinin konumunu belirtir ve anahtar S3 nesnesine giden tam yoldur.

### <a name="sample-dataset-with-prefix"></a>Önek ile örnek veri kümesi

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "testbucket",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
### <a name="sample-dataset-with-version"></a>Örnek veri seti (sürümile)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "testbucket",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="dynamic-paths-for-s3"></a>S3 için dinamik yollar
Önceki örnek, Amazon S3 veri kümesindeki **anahtar** ve **bucketName** özellikleri için sabit değerler kullanır.

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

SliceStart gibi sistem değişkenlerini kullanarak Veri Fabrikası'nın bu özellikleri çalışma zamanında dinamik olarak hesaplamalarını sağlayabilirsiniz.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Aynı şeyi bir Amazon S3 veri kümesinin **önek** özelliği için de yapabilirsiniz. Desteklenen işlevler ve değişkenlerin listesi için Bkz. [Veri Fabrikası işlevleri ve sistem değişkenleri.](data-factory-functions-variables.md)

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](data-factory-create-pipelines.md) Ad, açıklama, giriş ve çıktı tabloları ve ilkeler gibi özellikler tüm etkinlik türleri için kullanılabilir. Etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için özellikler kaynak ve lavabo türlerine bağlı olarak değişir. Kopyalama etkinliğindeki bir kaynak **FileSystemSource** türünden (Amazon S3'ü içerir) varsa, **typeProperties** bölümünde aşağıdaki özellik kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Dizin altında S3 nesnelerini özyinelemeli olarak listeleyip listelemeyeceğini belirtir. |doğru/yanlış |Hayır |

## <a name="json-example-copy-data-from-amazon-s3-to-azure-blob-storage"></a>JSON örneği: Verileri Amazon S3'ten Azure Blob depolamaalanına kopyalama
Bu örnek, Amazon S3'ten Azure Blob depolama alanına verilerin nasıl kopyalanır olduğunu gösterir. Ancak, veriler Veri Fabrikası'ndaki kopyalama etkinliği kullanılarak [desteklenen lavabolardan herhangi biri](data-factory-data-movement-activities.md#supported-data-stores-and-formats) için doğrudan kopyalanabilir.

Örnek, aşağıdaki Veri Fabrikası varlıkları için JSON tanımları sağlar. Bu tanımları, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [PowerShell'i](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak Amazon S3'ten Blob depolamasına kadar olan verileri kopyalamak için bir boru hattı oluşturmak için kullanabilirsiniz.   

* [AwsAccessKey](#linked-service-properties)türünde bağlantılı bir hizmet.
* [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
* [AmazonS3](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
* [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan kopyalama etkinliği ile bir [boru hattı.](data-factory-create-pipelines.md)

Örnek, Amazon S3'teki verileri her saat başı Azure blob'una kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

### <a name="amazon-s3-linked-service"></a>Amazon S3 bağlantılı hizmet

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti

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

### <a name="amazon-s3-input-dataset"></a>Amazon S3 giriş veri seti

**"Harici" ayarı: True,** Veri Fabrikası hizmetine veri kümesinin veri fabrikasının dışında olduğunu bildirir. Bu özelliği, ardışık işlemdeki bir etkinlik tarafından üretilmeyen bir giriş veri kümesinde doğru olarak ayarlayın.

```json
    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }
```


### <a name="azure-blob-output-dataset"></a>Azure Blob çıktı veri kümesi

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a>Bir Amazon S3 kaynağı ve bir blob lavabo ile bir boru hattı etkinliği kopyalama

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir kopyalama etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **FileSystemSource**olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır.

```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource",
                        "recursive": true
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonS3InputDataset"
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
                "name": "AmazonS3ToBlob"
            }
        ],
        "start": "2014-08-08T18:00:00Z",
        "end": "2014-08-08T19:00:00Z"
    }
}
```
> [!NOTE]
> Bir kaynak veri kümesinden sütunlara sütunları eşlemek için Azure [Veri Fabrikası'nda veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

* Veri Fabrikası'ndaki veri hareketinin performansını (kopyalama etkinliği) etkileyen önemli faktörler ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Kopyalama etkinliği performansı ve atokskılavuzuna](data-factory-copy-activity-performance.md)bakın.

* Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeler için [Kopyalama etkinliği öğreticisine](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)bakın.
