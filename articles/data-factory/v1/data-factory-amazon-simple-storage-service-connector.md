---
title: Data Factory kullanarak Amazon Simple Storage Service 'ten veri taşıma | Microsoft Docs
description: Azure Data Factory kullanarak Amazon Simple Storage Service 'ten (S3) veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: ec44ae7956669ee4e16d2c6ca00794c566272037
ms.sourcegitcommit: a3a40ad60b8ecd8dbaf7f756091a419b1fe3208e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69892002"
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Amazon Simple Storage Service 'ten veri taşıma
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](data-factory-amazon-simple-storage-service-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-amazon-simple-storage-service.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız, bkz. [v2 'de Amazon S3 Bağlayıcısı](../connector-amazon-simple-storage-service.md).

Bu makalede, Amazon Simple Storage Service (S3) ' den verileri taşımak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğiyle veri hareketine genel bir bakış sunan [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesinde oluşturulur.

Amazon S3 'ten desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Data Factory Şu anda yalnızca Amazon S3 'ten diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından Amazon S3 'e taşımamaktadır.

## <a name="required-permissions"></a>Gerekli izinler
Verileri Amazon S3'ten kopyalamak için aşağıdaki izinleri verilmiş olan emin olun:

* `s3:GetObject`Amazon S3 nesne işlemleri için.`s3:GetObjectVersion`
* `s3:ListBucket`Amazon S3 demet Işlemleri için. Data Factory kopyalama Sihirbazı ' nı `s3:ListAllMyBuckets` kullanıyorsanız de gereklidir.

Amazon S3 izinlerin tam listesi hakkında daha fazla ayrıntı için bkz: [izinleri belirten bir ilke](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Başlarken
Farklı araçları veya API 'Leri kullanarak bir Amazon S3 kaynağından veri taşıyan kopyalama etkinliğiyle bir işlem hattı oluşturabilirsiniz.

İşlem hattı oluşturmanın en kolay yolu **Kopyalama Sihirbazı**' nı kullanmaktır. Hızlı bir anlatım için bkz [. Öğretici: Kopyalama Sihirbazı 'Nı](data-factory-copy-data-wizard-tutorial.md)kullanarak bir işlem hattı oluşturun.

İşlem hattı oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği ile işlem hattı oluşturmaya yönelik adım adım yönergeler için, [kopyalama etkinliği öğreticisine](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)bakın.

Araçlar veya API 'Ler kullanmanıza bakılmaksızın, verileri bir kaynak veri deposundan havuz veri deposuna taşınan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıkış veri depolarını veri fabrikanıza bağlamak için **bağlı hizmetler** oluşturun.
2. Kopyalama işlemi için girdi ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Bir veri kümesini girdi olarak ve bir veri kümesini çıkış olarak alan kopyalama etkinliği ile bir işlem **hattı** oluşturun.

Sihirbazı kullandığınızda, bu Data Factory varlıkların JSON tanımları (bağlı hizmetler, veri kümeleri ve işlem hattı) sizin için otomatik olarak oluşturulur. Araçları veya API 'Leri (.NET API hariç) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlarsınız. Bir Amazon S3 veri deposundan veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımlarına sahip bir örnek için, bkz [. JSON örneği: Bu makalenin, Amazon S3 ' dan Azure](#json-example-copy-data-from-amazon-s3-to-azure-blob-storage) blob 'a veri kopyalama bölümüne geçin.

> [!NOTE]
> Kopyalama etkinliği için desteklenen dosya ve sıkıştırma biçimleri hakkında daha fazla bilgi için, bkz. [Azure Data Factory dosya ve sıkıştırma biçimleri](data-factory-supported-file-and-compression-formats.md).

Aşağıdaki bölümler, Amazon S3 'e özgü Data Factory varlıkları tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Bağlı hizmet bir veri deposunu veri fabrikasına bağlar. Amazon S3 veri deponuzu veri fabrikanıza bağlamak için **Awsaccesskey** türünde bağlı bir hizmet oluşturursunuz. Aşağıdaki tabloda, Amazon S3 (AwsAccessKey) bağlı hizmetine özgü JSON öğeleri için açıklama verilmiştir.

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Accesskeyıd |Gizli erişim anahtarı kimliği. |dize |Evet |
| secretAccessKey |Gizli erişim anahtarı kendisi. |Şifrelenmiş gizli dizi |Evet |

>[!NOTE]
>Bu bağlayıcı, Amazon S3'ten veri kopyalamak IAM hesabının erişim anahtarlarını gerektirir. [Geçici güvenlik kimlik bilgisi](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) desteklenmiyor.
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
Azure Blob depolamada giriş verilerini temsil edecek bir veri kümesi belirtmek için, veri kümesinin Type özelliğini **AmazonS3**olarak ayarlayın. Veri kümesinin **Linkedservicename** özelliğini, Amazon S3 bağlı hizmetinin adı olarak ayarlayın. Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri oluşturma](data-factory-create-datasets.md). 

Yapı, kullanılabilirlik ve ilke gibi bölümler tüm veri kümesi türleri (SQL veritabanı, Azure Blob ve Azure tablosu gibi) için benzerdir. **Typeproperties** bölümü her bir veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **AmazonS3** türünde bir veri kümesi Için **typeproperties** bölümü (Amazon S3 veri kümesini içerir) aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| bucketName |S3 demetini adı. |Dize |Evet |
| key |S3 nesne anahtarı. |Dize |Hayır |
| prefix |S3 nesnesinin anahtarı için önek. Seçili bir nesne anahtarları bu öneki ile başlayın. Yalnızca anahtar boş olduğunda geçerlidir. |Dize |Hayır |
| version |S3 sürümü oluşturma etkinse, S3 nesnesinin sürümü. |Dize |Hayır |
| format | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **jsonformat**, **avroformat**, **orcformat**, **parquetformat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [JSON biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquet biçimi ](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında olduğu gibi kopyalamak istiyorsanız, hem giriş hem de çıkış veri kümesi tanımlarının biçim bölümünü atlayın. | |Hayır |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **Gzip**, **söndür**, **bzip2**ve **zipsöndür**. Desteklenen düzeyler şunlardır: **En iyi** ve **en hızlı**. Daha fazla bilgi için bkz. [Azure Data Factory dosya ve sıkıştırma biçimleri](data-factory-supported-file-and-compression-formats.md#compression-support). | |Hayır |


> [!NOTE]
> **bucketName + Key** , S3 nesnesinin konumunu belirtir; burada Bucket, S3 nesnelerinin kök kapsayıcısıdır ve anahtar S3 nesnesinin tam yoludur.

### <a name="sample-dataset-with-prefix"></a>Ön eke sahip örnek veri kümesi

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
### <a name="sample-dataset-with-version"></a>Örnek veri kümesi (sürümle birlikte)

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
Yukarıdaki örnek, Amazon S3 veri kümesindeki **anahtar** ve **bucketName** özellikleri için sabit değerler kullanır.

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

Bu özellikleri, çalışma zamanında dinamik olarak, örneğin, daBaşlat gibi sistem değişkenlerini kullanarak hesaplamanız Data Factory sağlayabilirsiniz.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Bir Amazon S3 veri kümesi **önek** özelliği için aynısını yapabilirsiniz. Desteklenen işlevlerin ve değişkenlerin listesi için bkz. [Data Factory işlevleri ve sistem değişkenleri](data-factory-functions-variables.md).

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları oluşturma](data-factory-create-pipelines.md). Ad, açıklama, giriş ve çıkış tabloları ve ilkeler gibi özellikler, tüm etkinlik türleri için kullanılabilir. Etkinliğin **typeproperties** bölümünde kullanılabilen özellikler her etkinlik türüyle farklılık gösterir. Kopyalama etkinliği için özellikler, kaynak ve havuz türlerine göre farklılık gösterir. Kopyalama etkinliğindeki bir kaynak **Filesystemsource** türünde (Amazon S3 içeren), aşağıdaki özellik **typeproperties** bölümünde mevcuttur:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| recursive |S3 nesnelerinin dizin altında yinelemeli olarak kaydedilip edilmeyeceğini belirtir. |doğru/yanlış |Hayır |

## <a name="json-example-copy-data-from-amazon-s3-to-azure-blob-storage"></a>JSON örneği: Amazon S3 'ten Azure Blob depolama alanına veri kopyalama
Bu örnek, Amazon S3 ' den bir Azure Blob depolama alanına nasıl veri kopyalanacağını gösterir. Ancak, veriler, Data Factory kopyalama etkinliği kullanılarak [desteklenen herhangi bir](data-factory-data-movement-activities.md#supported-data-stores-and-formats) havuza doğrudan kopyalanabilir.

Örnek, aşağıdaki Data Factory varlıkları için JSON tanımları sağlar. Bu tanımları, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak Amazon S3 'ten blob depolamaya veri kopyalamak üzere bir işlem hattı oluşturmak için kullanabilirsiniz.   

* [Awsaccesskey](#linked-service-properties)türünde bağlı bir hizmet.
* [Azurestorage](data-factory-azure-blob-connector.md#linked-service-properties)türünde bağlı bir hizmet.
* [AmazonS3](#dataset-properties)türünde bir giriş [veri kümesi](data-factory-create-datasets.md) .
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünde bir çıkış [veri kümesi](data-factory-create-datasets.md) .
* [Filesystemsource](#copy-activity-properties) ve [blobsink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan kopyalama etkinliğine sahip bir işlem [hattı](data-factory-create-pipelines.md) .

Örnek, her saat için Amazon S3 'den bir Azure blobuna verileri kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri takip eden bölümlerde açıklanmıştır.

### <a name="amazon-s3-linked-service"></a>Amazon S3 bağlı hizmeti

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

### <a name="amazon-s3-input-dataset"></a>Amazon S3 giriş veri kümesi

**"Dış" ayarı: doğru** , Data Factory hizmetine veri kümesinin veri fabrikası dışında olduğunu bildirir. Bu özelliği, işlem hattındaki bir etkinlik tarafından üretilmeyen bir giriş veri kümesinde true olarak ayarlayın.

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

Veriler her saat yeni bir bloba yazılır (sıklık: saat, Aralık: 1). Blob 'un klasör yolu, işlenmekte olan dilimin başlangıç zamanına göre dinamik olarak değerlendirilir. Klasör yolu başlangıç zamanının yıl, ay, gün ve saat kısımlarını kullanır.

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


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a>Bir Amazon S3 kaynağına ve bir blob havuzuna sahip bir işlem hattındaki etkinliği kopyalama

İşlem hattı, giriş ve çıkış veri kümelerini kullanmak üzere yapılandırılmış bir kopyalama etkinliği içerir ve her saat çalışacak şekilde zamanlanır. Ardışık düzen JSON tanımında **kaynak** türü **filesystemsource**olarak ayarlanır ve **Havuz** türü **blobsink**olarak ayarlanır.

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
> Bir kaynak veri kümesindeki sütunları bir havuz veri kümesinden sütunlara eşlemek için, bkz. [Azure Data Factory veri kümesi sütunlarını eşleme](data-factory-map-columns.md).


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

* Veri taşıma (kopyalama etkinliği) performansını Data Factory ve en iyileştirmek için çeşitli yollarla etkileyen temel faktörlerle ilgili bilgi edinmek için bkz. [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).

* Kopyalama etkinliği ile işlem hattı oluşturmaya yönelik adım adım yönergeler için, [kopyalama etkinliği öğreticisine](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)bakın.
