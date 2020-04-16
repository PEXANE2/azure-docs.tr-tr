---
title: Amazon Basit Depolama Servisi (S3) veri kopyalama
description: Azure Veri Fabrikası'nı kullanarak desteklenen lavabo veri depolarına Amazon Basit Depolama Hizmeti'nden (S3) verileri kopyalama hakkında bilgi edinin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.openlocfilehash: 6975c876bb31e36b7b6268d284f747fd51803d63
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415464"
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Amazon Basit Depolama Hizmeti'nden veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
>
> * [Sürüm 1](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Geçerli sürüm](connector-amazon-simple-storage-service.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Amazon Basit Depolama Servisi (Amazon S3) verileri kopyalamak için nasıl özetliyor. Azure Veri Fabrikası hakkında bilgi edinmek için [giriş makalesini](introduction.md)okuyun.

>[!TIP]
>Amazon S3'ten Azure Depolama'ya veri geçişi senaryosu için, [Verileri Amazon S3'ten Azure Depolama'ya geçirmek için Azure Veri Fabrikası'nı kullanın'dan](data-migration-guidance-s3-azure-storage.md)daha fazla bilgi edinin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Amazon S3 konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
- [Etkinliği silme](delete-activity.md)

Özellikle, bu Amazon S3 bağlayıcısı dosyaları olduğu gibi kopyalamayı veya [desteklenen dosya biçimleri ve sıkıştırma codec'leri](supported-file-formats-and-compression-codecs.md)ile dosyaları ayrıştırma yı destekler. Ayrıca [kopya sırasında dosya meta verilerini korumayı](#preserve-metadata-during-copy)da seçebilirsiniz. Bağlayıcı, S3'teki istekleri doğrulamak için [AWS Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) kullanır.

>[!TIP]
>Bu Amazon S3 konektörünü, [örneğin Google Bulut Depolama](connector-google-cloud-storage.md)gibi **S3 uyumlu depolama sağlayıcılarından** gelen verileri kopyalamak için kullanabilirsiniz. Bağlantılı hizmet yapılandırmasında ilgili hizmet URL'sini belirtin.

## <a name="required-permissions"></a>Gerekli izinler

Amazon S3'teki verileri kopyalamak için, size aşağıdaki izinlerin verildiğinden emin olun:

- **Kopyalama etkinliği yürütme için:**: `s3:GetObject` ve `s3:GetObjectVersion` Amazon S3 Nesne İşlemleri için.
- **Veri Fabrikası GUI**yazma `s3:ListAllMyBuckets` `s3:ListBucket` / `s3:GetBucketLocation` için : ve Amazon S3 Bucket İşlemleri için izinleri ayrıca test bağlantısı ve göz atma/ dosya yollarında gezinme gibi işlemler için gereklidir. Bu izni vermek istemiyorsanız, bağlantılı hizmet oluşturma sayfasındatest bağlantısını atlayın ve yolu doğrudan veri kümesi ayarlarında belirtin.

Amazon S3 izinlerinin tam listesi hakkında ayrıntılı bilgi [için](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html)bkz.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

Aşağıdaki bölümlerde, Amazon S3'e özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi verilmiştir.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Amazon S3 bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **AmazonS3**olarak ayarlanmalıdır. | Evet |
| accessKeyId | Gizli erişim anahtarının kimliği. |Evet |
| secretAccessKey | Gizli erişim anahtarının kendisi. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) |Evet |
| serviceUrl | Resmi Amazon S3 hizmeti dışındaki S3 uyumlu bir depolama sağlayıcısından veri kopyalıyorsanız, özel S3 bitiş noktasını belirtin. Örneğin, Google Bulut Depolama'dan verileri `https://storage.googleapis.com`kopyalamak için belirtin. | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Süresi'ni veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

>[!TIP]
>Resmi Amazon S3 hizmeti dışında S3 uyumlu bir depodan veri kopyalıyorsanız özel S3 hizmet URL'sini belirtin.

>[!NOTE]
>Bu bağlayıcı, Amazon S3'ten gelen verileri kopyalamak için IAM hesabının erişim anahtarlarını gerektirir. [Geçici Güvenlik Kimlik Bilgisi](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) desteklenmez.
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı veri `location` kümesindeki ayarlar altında Amazon S3 için desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesinin `location` altındaki tür özelliği **AmazonS3Location**olarak ayarlanmalıdır. | Evet      |
| bucketName | S3 kova adı.                                          | Evet      |
| folderPath | Verilen kovanın altındaki klasöre giden yol. Klasöre filtre açmak için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarını belirtin. | Hayır       |
| fileName   | Verilen kova + folderPath altında dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarını belirtin. | Hayır       |
| version | S3 sürümü etkinse, S3 nesnesinin sürümü. Belirtilmemişse, en son sürüm getirilir. |Hayır |

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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Amazon S3 kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="amazon-s3-as-source"></a>Kaynak olarak Amazon S3

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama `storeSettings` kaynağındaki ayarlar altında Amazon S3 için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                                    |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------------------------- |
| type                     | Altındaki `storeSettings` tür özelliği **AmazonS3ReadSettings**olarak ayarlanmalıdır. | Evet                                                         |
| Özyinelemeli                | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme gerçek olarak ayarlandığında ve lavabo dosya tabanlı bir depo olduğunda, boş bir klasörün veya alt klasörün kopyalanmadığını veya lavaboda oluşturulmadığını unutmayın. İzin verilen değerler **doğru** (varsayılan) ve **yanlıştır.** | Hayır                                                          |
| Önek                   | Kaynak nesneleri filtrelemek için veri kümesinde yapılandırılan verilen kovanın altındaki S3 nesne anahtarı için önek. Anahtarları bu önek ile başlayan nesneler seçilir. <br>Yalnızca özellikler `wildcardFolderPath` belirtilmediğinde geçerlidir. `wildcardFileName` | Hayır                                                          |
| joker KarakterFolderPath       | Verilen kovanın altındaki joker karakteriçeren klasör yolu, kaynak klasörleri filtrelemek için veri kümesinde yapılandırılır. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. <br>Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Hayır                                                          |
| jokerKarakterFileName         | Kaynak dosyaları filtrelemek için verilen kova + folderPath/jokerKarakterIn altında joker karakter içeren dosya adı. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın.  Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Veri `fileName` kümesinde yse `prefix` ve belirtilmemişse evet |
| modifiyeDatetimeBaşlat    | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanamayacağı anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir. | Hayır                                                          |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                                          |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır                                                          |

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
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AmazonS3ReadSettings",
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

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, klasör yolunun ve dosya adının joker karakter filtreleriyle ortaya çıkan davranışı açıklanmaktadır.

| Kova | anahtar | Özyinelemeli | Kaynak klasör yapısı ve filtre sonucu (kalın renkteki dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| Kova | `Folder*/*` | yanlış | Kova<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| Kova | `Folder*/*` | true | Kova<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| Kova | `Folder*/*.csv` | yanlış | Kova<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| Kova | `Folder*/*.csv` | true | Kova<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |

## <a name="preserve-metadata-during-copy"></a>Kopyalama sırasında meta verileri koruma

Amazon S3'ten Azure Veri Gölü Depolama Gen2/Azure Blob'a kadar dosyaları kopyalarken, dosya meta verilerini verilerle birlikte korumayı seçebilirsiniz. [Meta verileri koru](copy-activity-preserve-metadata.md#preserve-metadata)adresinden daha fazla bilgi edinin.

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="getmetadata-activity-properties"></a>Metaveri etkinlik özelliklerini alın

Özellikler hakkında daha fazla bilgi edinmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) kontrol edin 

## <a name="delete-activity-properties"></a>Etkinlik özelliklerini silme

Özellikler hakkında ayrıntılı bilgi edinmek için [Sil etkinliğini denetleyin](delete-activity.md)

## <a name="legacy-models"></a>Eski modeller

>[!NOTE]
>Aşağıdaki modeller hala geriye dönük uyumluluk için olduğu gibi desteklenir. İleriye dönük yukarıdaki bölümlerde belirtilen yeni modeli kullanmanız önerilir ve ADF yazarlı UI yeni modeli oluşturmaya geçti.

### <a name="legacy-dataset-model"></a>Eski dataset modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği ayarlanmalıdır: **AmazonS3Object** |Evet |
| bucketName | S3 kova adı. Joker karakter filtresi desteklenmez. |Kopyalama/Arama etkinliği için Evet, GetMetadata etkinliği için Hayır |
| anahtar | Belirtilen kovanın altındaki S3 nesne anahtarının **adı veya joker filtresi.** Yalnızca "önek" özelliği belirtilmediğinde geçerlidir. <br/><br/>Joker karakter filtresi hem klasör bölümü hem de dosya adı bölümü için desteklenir. İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir).<br/>- Örnek 1:`"key": "rootfolder/subfolder/*.csv"`<br/>- Örnek 2:`"key": "rootfolder/subfolder/???20180427.txt"`<br/>Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. Gerçek `^` klasör /dosya adı joker veya bu kaçış char içinde varsa kaçmak için kullanın. |Hayır |
| Önek | S3 nesne anahtarı için önek. Anahtarları bu önek ile başlayan nesneler seçilir. Yalnızca "anahtar" özelliği belirtilmediğinde geçerlidir. |Hayır |
| version | S3 sürümü etkinse, S3 nesnesinin sürümü. Belirtilmemişse, en son sürüm getirilir. |Hayır |
| modifiyeDatetimeBaşlat | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek veri hareketinin genel performansının etkileneceğini unutmayın. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanamayacağı anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek veri hareketinin genel performansının etkileneceğini unutmayın. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanamayacağı anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir.| Hayır |
| biçim | Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın.<br/><br/>Belirli bir biçimde dosyaları ayrışdırmak veya oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](supported-file-formats-and-compression-codecs-legacy.md#text-format) [Json Formatı,](supported-file-formats-and-compression-codecs-legacy.md#json-format) [Avro Biçimi,](supported-file-formats-and-compression-codecs-legacy.md#avro-format) [Ork Biçimi](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parke Biçimi](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) bölümlerine bakın. |Hayır (yalnızca ikili kopyalama senaryosu için) |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve sıkıştırma codec'lerine](supported-file-formats-and-compression-codecs-legacy.md#compression-support)bakın.<br/>Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**.<br/>Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** |Hayır |

>[!TIP]
>Tüm dosyaları bir klasörün altında kopyalamak için kova için **bucketName** ve klasör bölümü için **önek belirtin.**<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için, kova için **bucketName** ve klasör bölümü artı dosya adı için **anahtar** belirtin.<br>Bir klasörün altındaki dosyaların alt kümesini kopyalamak için, klasör bölümü artı joker karakter filtresi için kova ve **anahtar** için **bucketName'yi** belirtin.

**Örnek: önek kullanarak**

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

### <a name="legacy-copy-activity-source-model"></a>Eski kopyalama etkinliği kaynak modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **FileSystemSource** |Evet |
| Özyinelemeli | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme doğru ayarlandığında ve lavabo dosya tabanlı depolandığında, boş klasör/alt klasör lavaboda kopyalanmaz/oluşturulmaz.<br/>İzin verilen değerler şunlardır: **true** (varsayılan), **yanlış** | Hayır |
| maxConcurrentConnections | Veri deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

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

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
