---
title: Amazon Simple Storage Service (S3) öğesinden veri kopyalama
description: Desteklenen bir havuz veri depolarına Amazon Basit Depolama hizmetinden (S3 için) Azure Data Factory kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/24/2019
ms.openlocfilehash: 7a51082bfbcd0790daff7911e79c7424f872d37b
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74913524"
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Amazon basit depolama hizmeti Azure Data Factory kullanarak veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
>
> * [Sürüm 1](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Geçerli sürüm](connector-amazon-simple-storage-service.md)

Bu makalede, Amazon Simple Storage Service 'ten (Amazon S3) verilerin nasıl kopyalanacağı özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

>[!TIP]
>Amazon S3 'den Azure Storage 'a veri geçişi senaryosu için, [Amazon S3 'Ten Azure Storage 'a veri geçirmek üzere Azure Data Factory kullanma](data-migration-guidance-s3-azure-storage.md)hakkında daha fazla bilgi edinin.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Amazon S3 Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)

Özellikle, bu Amazon S3 bağlayıcı kopyalama dosyaları gibi destekler- ya da dosyaları ayrıştırma [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md). S3 'e yönelik isteklerin kimliğini doğrulamak için [AWS Imza sürüm 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) ' ü kullanır.

>[!TIP]
>Bu Amazon S3 bağlayıcısını, **S3 ile uyumlu herhangi bir depolama sağlayıcısından** (örn. [Google bulut depolaması](connector-google-cloud-storage.md)) veri kopyalamak için kullanabilirsiniz. Bağlı hizmet yapılandırmasında karşılık gelen hizmet URL 'sini belirtin.

## <a name="required-permissions"></a>Gerekli izinler

Verileri Amazon S3'ten kopyalamak için aşağıdaki izinleri verilmiş olan emin olun:

- **Kopyalama etkinliği yürütme için:** : `s3:GetObject` ve `s3:GetObjectVersion` Amazon S3 nesnesinin işlemleri için.
- **Data Factory GUI yazma**: `s3:ListAllMyBuckets` ve `s3:ListBucket` / `s3:GetBucketLocation` bağlantıyı test et ve göz atma ve Git gibi işlemler dosya yolları için Amazon S3 Demetini işlemleri için izinleri ayrıca gereklidir. Bu izni vermek istemiyorsanız, bağlantılı hizmet oluşturma sayfasındaki test bağlantısını atlayın ve yolu doğrudan veri kümesi ayarlarında belirtin.

Amazon S3 izinlerin tam listesi hakkında daha fazla ayrıntı için bkz: [izinleri belirten bir ilke](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

Aşağıdaki bölümler, Amazon S3 belirli Data Factory varlıkları tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Amazon S3'bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AmazonS3**. | Yes |
| accessKeyId | Gizli erişim anahtarı kimliği. |Yes |
| secretAccessKey | Gizli erişim anahtarı kendisi. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Yes |
| serviceUrl | Resmi Amazon S3 hizmeti dışında S3 ile uyumlu bir depolama sağlayıcısından veri kopyalıyorsanız, özel S3 uç noktasını belirtin. Örneğin, Google Cloud Storage 'dan veri kopyalamak için `https://storage.googleapis.com`belirtin. | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel ağında bulunuyorsa), Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!TIP]
>Resmi Amazon S3 hizmeti dışında S3 ile uyumlu bir depolama alanından veri kopyalıyorsanız, özel S3 hizmeti URL 'sini belirtin.

>[!NOTE]
>Bu bağlayıcı, Amazon S3'ten veri kopyalamak IAM hesabının erişim anahtarlarını gerektirir. [Geçici güvenlik kimlik bilgisi](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) desteklenmiyor.
>

Örnek aşağıda verilmiştir:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AmazonS3",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": {
                "type": "SecureString",
                "value": "<secret access key>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı veri kümesindeki `location` ayarları altında Amazon S3 için desteklenir:

| Özellik   | Açıklama                                                  | Gereklidir |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesindeki `location` öğesinin altında bulunan tür özelliği **AmazonS3Location**olarak ayarlanmalıdır. | Yes      |
| bucketName | S3 demetini adı.                                          | Yes      |
| folderPath | Verilen demet altındaki klasörün yolu. Klasörü filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirtin. | Hayır       |
| fileName   | Belirtilen demet + folderPath altındaki dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirtin. | Hayır       |
| version | S3 sürümü oluşturma etkinse, S3 nesnesinin sürümü. Belirtilmemişse, en son sürüm alınacaktır. |Hayır |

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AmazonS3Location",
                "bucketName": "bucketname",
                "folderPath": "folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="legacy-dataset-model"></a>Eski veri kümesi modeli

>[!NOTE]
>Aşağıdaki veri kümesi modeli, geriye dönük uyumluluk için olduğu gibi hala desteklenmektedir. Yukarıdaki ileri giderek bahsedilen yeni modeli kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi yeni modeli oluşturmaya geçti.

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **AmazonS3Object** |Yes |
| bucketName | S3 demetini adı. Joker karakter filtresi desteklenmez. |GetMetadata etkinliği için Hayır kopyalama/arama etkinliği için Evet |
| anahtar | **Adı veya joker karakter filtresi** altında belirtilen demetini S3 nesnesinin anahtarı. Yalnızca ne zaman "ön eki" özelliği belirtilmedi geçerlidir. <br/><br/>Joker karakter filtresi, hem klasör bölümü hem de dosya adı bölümü için desteklenir. Joker karakterlere izin verilir: `*` (sıfır veya daha fazla karakter ile eşleşir) ve `?` (eşleşen sıfır ya da tek bir karakter).<br/>-Örnek 1: `"key": "rootfolder/subfolder/*.csv"`<br/>-Örnek 2: `"key": "rootfolder/subfolder/???20180427.txt"`<br/>[Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örneğe bakın. Gerçek klasörünüz/dosya adınızın joker karakter veya içinde bu kaçış karakteri varsa çıkmak için `^` kullanın. |Hayır |
| prefix | S3 nesnesinin anahtarı için önek. Seçili bir nesne anahtarları bu öneki ile başlayın. Yalnızca "anahtarını" özelliği belirtilmedi uygulanır. |Hayır |
| version | S3 sürümü oluşturma etkinse, S3 nesnesinin sürümü. Belirtilmemişse, en son sürüm alınacaktır. |Hayır |
| modifiedDatetimeStart | Dosyaları filtre özniteliğine dayanarak: son değişiklik. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Çok büyük miktarlarda dosyadan dosya filtresi yapmak istediğinizde, bu ayarın etkinleştirilmesi durumunda veri hareketinin genel performansını unutmayın. <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosyaları filtre özniteliğine dayanarak: son değişiklik. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br/><br/> Çok büyük miktarlarda dosyadan dosya filtresi yapmak istediğinizde, bu ayarın etkinleştirilmesi durumunda veri hareketinin genel performansını unutmayın. <br/><br/> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir.| Hayır |
| biçim | İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquetbiçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopya senaryosu için) |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyler: **Optimal** ve **en hızlı**. |Hayır |

>[!TIP]
>Bir klasör altındaki tüm dosyaları kopyalamak için belirtin **bucketName** demet için ve **önek** klasör bölümü için.<br>Belirli bir ada sahip tek bir dosya kopyalamak için belirtin **bucketName** demet için ve **anahtar** klasör bölümü artı dosya adı.<br>Bir alt klasörü altında bulunan dosyaları kopyalamak için belirtin **bucketName** demet için ve **anahtar** klasör bölümü artı joker karakter filtresi için.

**Örnek: önek kullanma**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3Object",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "prefix": "testFolder/test",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

**Örnek: anahtarı ve sürüm (isteğe bağlı) kullanma**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "key": "testFolder/testfile.csv.gz",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Amazon S3 kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="amazon-s3-as-source"></a>Amazon S3 kaynağı olarak

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama kaynağında `storeSettings` ayarları altında Amazon S3 için desteklenir:

| Özellik                 | Açıklama                                                  | Gereklidir                                                    |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------------------------- |
| type                     | `storeSettings` altındaki tür özelliği **AmazonS3ReadSetting**olarak ayarlanmalıdır. | Yes                                                         |
| recursive                | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında bir dosya tabanlı depolama, bir boş klasör veya alt klasör olduğunu unutmayın kopyalanır değil veya havuz oluşturulur. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır                                                          |
| prefix                   | Kaynak nesneleri filtrelemek için veri kümesinde yapılandırılan belirtilen demet altındaki S3 nesne anahtarının ön eki. Seçili bir nesne anahtarları bu öneki ile başlayın. <br>Yalnızca `wildcardFolderPath` ve `wildcardFileName` özellikleri belirtilmediğinde geçerlidir. | Hayır                                                          |
| Yavaya Cardfolderpath       | Kaynak klasörleri filtrelemek için veri kümesinde yapılandırılan belirtilen demet altında joker karakter olan klasör yolu. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek klasör adınızın joker karakter veya içinde bu kaçış karakteri varsa çıkmak için `^` kullanın. <br>[Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Hayır                                                          |
| Yavaya Cardfilename         | Kaynak dosyalarını filtrelemek için, belirtilen demet + folderPath/, Cardfolderpath altındaki joker karakterlerle dosya adı. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek klasör adınızın joker karakter veya içinde bu kaçış karakteri varsa çıkmak için `^` kullanın.  [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Veri kümesindeki `fileName` ve `prefix` belirtilmemişse Evet |
| modifiedDatetimeStart    | Dosyaları filtre özniteliğine dayanarak: son değişiklik. Kendi son değiştirilme zamanı zaman aralığı içinde olduğunda dosyaları seçilir `modifiedDatetimeStart` ve `modifiedDatetimeEnd`. Zaman biçimi UTC saat diliminde uygulanan "2018-12-01T05:00:00Z". <br> Özellikler, hiçbir dosya öznitelik filtresi, veri kümesine uygulanacak anlamına NULL olabilir.  Zaman `modifiedDatetimeStart` datetime değerine sahip ancak `modifiedDatetimeEnd` NULL olduğu için daha büyük olan son değiştirilen özniteliği dosyaları geldiğini veya tarih saat değeri ile eşit seçilir.  Zaman `modifiedDatetimeEnd` datetime değerine sahip ancak `modifiedDatetimeStart` NULL ise, son değiştirilen özniteliği, tarih saat değeri seçilir daha az dosya anlamına gelir. | Hayır                                                          |
| modifiedDatetimeEnd      | Yukarıdaki gibi.                                               | Hayır                                                          |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır                                                          |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AmazonS3ReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="legacy-source-model"></a>Eski kaynak modeli

>[!NOTE]
>Aşağıdaki kopya kaynak modeli, geriye dönük uyumluluk için olduğu gibi hala desteklenmektedir. Yukarıdaki ileri giderek bahsedilen yeni modeli kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi yeni modeli oluşturmaya geçti.

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **FileSystemSource** |Yes |
| recursive | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli true ve havuz için ayarlandığında Not dosya tabanlı depolama, boş klasör/alt-folder havuz kopyalanan/oluşturulmuş olmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |
| maxConcurrentConnections | Veri deposuna eşzamanlı olarak bağlanmak için bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Amazon S3 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, klasör yolu ve dosya adının joker karakter filtreleriyle elde edilen davranışı açıklanmaktadır.

| bucket | anahtar | recursive | Kaynak klasör yapısı ve filtre sonucu (kalın olan dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| bucket | `Folder*/*` | yanlış | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**dosya2. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| bucket | `Folder*/*` | doğru | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**dosya2. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| bucket | `Folder*/*.csv` | yanlış | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dosya2. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| bucket | `Folder*/*.csv` | doğru | bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dosya2. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) denetleyin 

## <a name="delete-activity-properties"></a>Etkinlik özelliklerini Sil

Özelliklerle ilgili ayrıntıları öğrenmek için [silme etkinliği](delete-activity.md) onay

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
