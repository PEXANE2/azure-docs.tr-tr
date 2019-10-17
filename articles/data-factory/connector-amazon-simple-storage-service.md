---
title: Azure Data Factory kullanarak Amazon Simple Storage Service (S3) verilerini kopyalama | Microsoft Docs
description: Azure Data Factory kullanarak Amazon Simple Storage Service (S3) kaynağından desteklenen havuz veri depolarına veri kopyalama hakkında bilgi edinin.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: jingwang
ms.openlocfilehash: f8f7c33abda8d31d39051a024b9cc381c9f6b192
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387942"
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Azure Data Factory kullanarak Amazon Simple Storage Service 'ten veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
>
> * [Sürüm 1](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Geçerli sürüm](connector-amazon-simple-storage-service.md)

Bu makalede, Amazon Simple Storage Service 'ten (Amazon S3) verilerin nasıl kopyalanacağı özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [tanıtım makalesini](introduction.md)okuyun.

>[!TIP]
>Amazon S3 'den Azure Storage 'a veri geçişi senaryosu için, [Amazon S3 'Ten Azure Storage 'a veri geçirmek üzere Azure Data Factory kullanma](data-migration-guidance-s3-azure-storage.md)hakkında daha fazla bilgi edinin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Amazon S3 Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)

Özellikle, bu Amazon S3 Bağlayıcısı dosyaları olduğu gibi kopyalamayı veya [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleriyle](supported-file-formats-and-compression-codecs.md)dosya ayrıştırmayı destekler. S3 'e yönelik isteklerin kimliğini doğrulamak için [AWS Imza sürüm 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) ' ü kullanır.

>[!TIP]
>Bu Amazon S3 bağlayıcısını, **S3 ile uyumlu herhangi bir depolama sağlayıcısından** (örn. [Google bulut depolaması](connector-google-cloud-storage.md)) veri kopyalamak için kullanabilirsiniz. Bağlı hizmet yapılandırmasında karşılık gelen hizmet URL 'sini belirtin.

## <a name="required-permissions"></a>Gerekli izinler

Amazon S3 öğesinden veri kopyalamak için aşağıdaki izinlerin verildiğinden emin olun:

- **Kopyalama etkinliği yürütmesi için:** : `s3:GetObject` ve Amazon S3 nesne işlemleri için `s3:GetObjectVersion`.
- **Data Factory GUI yazma için**: `s3:ListAllMyBuckets` ve `s3:ListBucket` @ no__t-3 @ no__t-4 ' ü Amazon S3 demet işlemleri izinleri, test bağlantısı gibi işlemler için de gereklidir ve dosya yollarına gözatıp/gezinilir. Bu izni vermek istemiyorsanız, bağlantılı hizmet oluşturma sayfasındaki test bağlantısını atlayın ve yolu doğrudan veri kümesi ayarlarında belirtin.

Amazon S3 izinlerinin tam listesi hakkında daha fazla bilgi için bkz. [ilke Içinde Izinleri belirtme](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

Aşağıdaki bölümlerde,, Amazon S3 'e özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Amazon S3 bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği **AmazonS3**olarak ayarlanmalıdır. | Yes |
| Accesskeyıd | Gizli dizi erişim anahtarının KIMLIĞI. |Yes |
| secretAccessKey | Gizli dizi erişim anahtarı. Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. |Yes |
| serviceUrl | Resmi Amazon S3 hizmeti dışında S3 ile uyumlu bir depolama sağlayıcısından veri kopyalıyorsanız, özel S3 uç noktasını belirtin. Örneğin, Google Cloud Storage 'dan veri kopyalamak için `https://storage.googleapis.com` belirtin. | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Integration Runtime](concepts-integration-runtime.md) . Azure Integration Runtime veya şirket içinde barındırılan Integration Runtime (veri depolduğunuz özel ağda yer alıyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!TIP]
>Resmi Amazon S3 hizmeti dışında S3 ile uyumlu bir depolama alanından veri kopyalıyorsanız, özel S3 hizmeti URL 'sini belirtin.

>[!NOTE]
>Bu bağlayıcı, veri Amazon S3 ' dan veri kopyalamak için ıAM hesabının erişim anahtarlarını gerektirir. [Geçici güvenlik kimlik bilgileri](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) desteklenmiyor.
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

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. 

- **Parquet, ayrılmış metin, JSON, avro ve ikili biçimi**Için, [Parquet, SıNıRLANDıRıLMıŞ metin, JSON, avro ve ikili biçim veri kümesi](#format-based-dataset) bölümüne bakın.
- **Orc biçimi**gibi diğer biçimler için [diğer biçim veri kümesi](#other-format-dataset) bölümüne bakın.

### <a name="format-based-dataset"></a>Parquet, sınırlandırılmış metin, JSON, avro ve ikili biçim veri kümesi

**Inquet, ayrılmış metin, JSON, avro ve ikili biçimdeki**Amazon S3 öğesinden veri kopyalamak için, biçim tabanlı veri kümesinde [Parquet biçimine](format-parquet.md), [sınırlandırılmış metin biçimine](format-delimited-text.md), [avro biçime](format-avro.md) ve [ikili biçim](format-binary.md) makalesine bakın ve desteklenir Ayarlar. Aşağıdaki özellikler, biçim tabanlı veri kümesindeki `location` ayarları altında Amazon S3 için desteklenir:

| Özellik   | Açıklama                                                  | Gereklidir |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesindeki `location` ' ın altında bulunan tür özelliği **AmazonS3Location**olarak ayarlanmalıdır. | Yes      |
| bucketName | S3 demet adı.                                          | Yes      |
| folderPath | Verilen demet altındaki klasörün yolu. Klasörü filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirtin. | Hayır       |
| fileName   | Belirtilen demet + folderPath altındaki dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirtin. | Hayır       |
| version | S3 sürümü etkinse S3 nesnesinin sürümü. Belirtilmemişse, en son sürüm alınacaktır. |Hayır |

> [!NOTE]
> Sonraki bölümde belirtilen Parquet/metin biçimine sahip **AmazonS3Object** Type veri kümesi, geriye dönük uyumluluk için kopyalama/arama/GetMetadata etkinliğinin olduğu gibi hala desteklenir, ancak eşleme veri akışı ile çalışmaz. Bu yeni modeli ileriye doğru kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi bu yeni türleri oluşturmaya geçti.

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

### <a name="other-format-dataset"></a>Diğer biçim veri kümesi

**Orc biçimindeki**Amazon S3 öğesinden veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | DataSet 'in Type özelliği: **AmazonS3Object** olarak ayarlanmalıdır |Yes |
| bucketName | S3 demet adı. Joker karakter filtresi desteklenmiyor. |Kopyala/ara etkinliği için Evet, GetMetadata etkinliği için Hayır |
| anahtar | Belirtilen demet altındaki S3 nesne anahtarının **adı veya joker karakter filtresi** . Yalnızca "önek" özelliği belirtilmediğinde geçerlidir. <br/><br/>Joker karakter filtresi, hem klasör bölümü hem de dosya adı bölümü için desteklenir. İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>-Örnek 1: `"key": "rootfolder/subfolder/*.csv"`<br/>-Örnek 2: `"key": "rootfolder/subfolder/???20180427.txt"`<br/>[Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örneğe bakın. Gerçek klasörünüz/dosya adınızın joker karakter veya içinde bu kaçış karakteri varsa çıkış için `^` kullanın. |Hayır |
| koy | S3 nesne anahtarı için ön ek. Anahtarları bu önek ile başlayan nesneler seçilidir. Yalnızca "Key" özelliği belirtilmediğinde geçerlidir. |Hayır |
| version | S3 sürümü etkinse S3 nesnesinin sürümü. Belirtilmemişse, en son sürüm alınacaktır. |Hayır |
| modifiedDatetimeStart | Öznitelikleri temel alan dosya filtresi: son değiştirme. Son değiştirilme zamanı, `modifiedDatetimeStart` ve `modifiedDatetimeEnd` arasındaki zaman aralığı içinde ise bu dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Çok büyük miktarlarda dosyadan dosya filtresi yapmak istediğinizde, bu ayarın etkinleştirilmesi durumunda veri hareketinin genel performansını unutmayın. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmayacak anlamına gelir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeEnd` NULL ise, son değiştirilen özniteliği DateTime değeri ile eşit veya daha büyük olan dosyalar seçilir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| modifiedDatetimeEnd | Öznitelikleri temel alan dosya filtresi: son değiştirme. Son değiştirilme zamanı, `modifiedDatetimeStart` ve `modifiedDatetimeEnd` arasındaki zaman aralığı içinde ise bu dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Çok büyük miktarlarda dosyadan dosya filtresi yapmak istediğinizde, bu ayarın etkinleştirilmesi durumunda veri hareketinin genel performansını unutmayın. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmayacak anlamına gelir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeEnd` NULL ise, son değiştirilen özniteliği DateTime değeri ile eşit veya daha büyük olan dosyalar seçilir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| formatını | Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıkış veri kümesi tanımlarının biçim bölümünü atlayın.<br/><br/>Belirli bir biçimdeki dosyaları ayrıştırmak veya oluşturmak isterseniz, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **jsonformat**, **avroformat**, **orcformat**, **parquetformat**. Biçim ' in altındaki **Type** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için bkz. [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [JSON biçimi](supported-file-formats-and-compression-codecs.md#json-format), [avro Format](supported-file-formats-and-compression-codecs.md#avro-format), [orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format)ve [Parquet biçim](supported-file-formats-and-compression-codecs.md#parquet-format) bölümleri. |Hayır (yalnızca ikili kopya senaryosu için) |
| masıyla | Verilerin türünü ve sıkıştırma düzeyini belirtin. Daha fazla bilgi için bkz. [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler şunlardır: **gzip**, **söndür**, **bzip2**ve **zipsöndür**.<br/>Desteklenen düzeyler şunlardır: **en iyi** ve **en hızlı**. |Hayır |

>[!TIP]
>Bir klasörün altındaki tüm dosyaları kopyalamak için, for Bucket için **bucketName** ve klasör bölümü için **önek** belirtin.<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için, Aralık için **bucketName** ve klasör bölümü artı dosya adı için **anahtar** belirtin.<br>Bir klasörün altına bir dosya alt kümesini kopyalamak için, Bucket için **bucketName** ve klasör bölümü artı joker karakter filtresi için **anahtar** belirtin.

**Örnek: Using öneki**

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

**Örnek: anahtar ve sürüm kullanma (isteğe bağlı)**

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

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Amazon S3 kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="amazon-s3-as-source"></a>Amazon S3 kaynak olarak

- **Parquet, sınırlandırılmış metin, JSON, avro ve ikili biçiminden**kopyalamak Için, [Parquet, SıNıRLANDıRıLMıŞ metin, JSON, avro ve ikili biçim kaynağı](#format-based-source) bölümüne bakın.
- **Orc biçimi**gibi diğer biçimlerden kopyalamak için [diğer biçim kaynağı](#other-format-source) bölümüne bakın.

#### <a name="format-based-source"></a>Parquet, sınırlandırılmış metin, JSON, avro ve ikili biçim kaynağı

**Inquet, ayrılmış metin, JSON, avro ve ikili biçimdeki**Amazon S3 ' dan verileri kopyalamak için, biçim tabanlı kopyalama etkinliği kaynağı 'Nda [Parquet biçimine](format-parquet.md), [sınırlandırılmış metin biçimine](format-delimited-text.md), [avro biçimine](format-avro.md) ve [ikili biçim](format-binary.md) makalesine başvurun. desteklenen ayarlar. Aşağıdaki özellikler, `storeSettings` ayarları altında, biçim tabanlı kopyalama kaynağında Amazon S3 için desteklenir:

| Özellik                 | Açıklama                                                  | Gereklidir                                                    |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------------------------- |
| type                     | @No__t-0 altındaki Type özelliği **AmazonS3ReadSetting**olarak ayarlanmalıdır. | Yes                                                         |
| öz                | Verilerin alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak okunup okunmadığını gösterir. Özyinelemeli değeri true olarak ayarlandığında ve havuz dosya tabanlı bir depo olduğunda, havuzda boş bir klasör veya alt klasör kopyalanmadığını veya oluşturulamadığına unutmayın. İzin verilen değerler **true** (varsayılan) ve **false**şeklindedir. | Hayır                                                          |
| koy                   | Kaynak nesneleri filtrelemek için veri kümesinde yapılandırılan belirtilen demet altındaki S3 nesne anahtarının ön eki. Anahtarları bu önek ile başlayan nesneler seçilidir. <br>Yalnızca `wildcardFolderPath` ve `wildcardFileName` özellikleri belirtilmediğinde geçerlidir. | Hayır                                                          |
| Yavaya Cardfolderpath       | Kaynak klasörleri filtrelemek için veri kümesinde yapılandırılan belirtilen demet altında joker karakter olan klasör yolu. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek klasörünüzün adında joker karakter veya bu kaçış karakteri varsa, çıkış için `^` kullanın. <br>[Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Hayır                                                          |
| Yavaya Cardfilename         | Kaynak dosyalarını filtrelemek için, belirtilen demet + folderPath/, Cardfolderpath altındaki joker karakterlerle dosya adı. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek klasörünüzün adında joker karakter veya bu kaçış karakteri varsa, çıkış için `^` kullanın.  [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Veri kümesinde `fileName` ve `prefix` belirtilmemişse Evet |
| modifiedDatetimeStart    | Öznitelikleri temel alan dosya filtresi: son değiştirme. Son değiştirilme zamanı, `modifiedDatetimeStart` ve `modifiedDatetimeEnd` arasındaki zaman aralığı içinde ise bu dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmayacak anlamına gelir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeEnd` NULL ise, son değiştirilen özniteliği DateTime değeri ile eşit veya daha büyük olan dosyalar seçilir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir. | Hayır                                                          |
| modifiedDatetimeEnd      | Yukarıdaki gibi.                                               | Hayır                                                          |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır                                                          |

> [!NOTE]
> Parquet/delimited metin biçimi için, sonraki bölümde bahsedilen **filesystemsource** türü kopyalama etkinliği kaynağı, geriye dönük uyumluluk için olduğu gibi desteklenmeye devam etmektedir. Bu yeni modeli ileriye doğru kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi bu yeni türleri oluşturmaya geçti.

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

#### <a name="other-format-source"></a>Diğer biçim kaynağı

**Orc biçimindeki**Amazon S3 öğesinden veri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği: **Filesystemsource** olarak ayarlanmalıdır |Yes |
| öz | Verilerin alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak okunup okunmadığını gösterir. Özyinelemeli değeri true olarak ayarlandığında ve havuz dosya tabanlı depo ise, boş klasör/alt klasör, havuzda kopyalanmayacak/oluşturulmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |
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

| demet | anahtar | öz | Kaynak klasör yapısı ve filtre sonucu (kalın olan dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| demet | `Folder*/*` | yanlış | demet<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3FolderA<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7**FILE1. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7**dosya2. JSON**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10&nbsp;1File3.csv<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10&nbsp;1File4.json<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10&nbsp;1File5.csv<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3AnotherFolderB<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File6.csv |
| demet | `Folder*/*` | doğru | demet<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3FolderA<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7**FILE1. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7**dosya2. JSON**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10 @ no__t-11**File3. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10 @ no__t-11**File4. JSON**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10 @ no__t-11**File5. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3AnotherFolderB<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File6.csv |
| demet | `Folder*/*.csv` | yanlış | demet<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3FolderA<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7**FILE1. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File2.json<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10&nbsp;1File3.csv<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10&nbsp;1File4.json<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10&nbsp;1File5.csv<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3AnotherFolderB<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File6.csv |
| demet | `Folder*/*.csv` | doğru | demet<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3FolderA<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7**FILE1. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File2.json<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10 @ no__t-11**File3. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10&nbsp;1File4.json<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9 @ no__t-10 @ no__t-11**File5. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3AnotherFolderB<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File6.csv |

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) denetleyin 

## <a name="delete-activity-properties"></a>Etkinlik özelliklerini Sil

Özelliklerle ilgili ayrıntıları öğrenmek için [silme etkinliği](delete-activity.md) onay

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
