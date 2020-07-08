---
title: Azure Data Factory kullanarak Google bulut depolamadan veri kopyalama
description: Azure Data Factory kullanarak Google bulut depolamadan desteklenen havuz veri depolarına veri kopyalama hakkında bilgi edinin.
services: data-factory
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/12/2020
ms.author: jingwang
ms.openlocfilehash: 9ecb703f8c8f75939d8d796bdd5f687795145f74
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85101043"
---
# <a name="copy-data-from-google-cloud-storage-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Google bulut depolamadan veri kopyalama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Google bulut depolamadan (GCS) verilerin nasıl kopyalanacağı özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [tanıtım makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Google Cloud Storage Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)

Özellikle, bu Google Cloud Storage Bağlayıcısı, [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs.md)ile dosya kopyalamayı veya dosya ayrıştırmayı destekler. Bu, GCS 'nin S3 ile uyumlu birlikte çalışabilirliğinin avantajlarından yararlanır.

## <a name="prerequisites"></a>Ön koşullar

Google Cloud Storage hesabınızda aşağıdaki kurulum gereklidir:

1. Google Cloud Storage hesabınız için birlikte çalışabilirliği etkinleştirin
2. Hedef GCS demetini kopyalamak istediğiniz verileri içeren varsayılan projeyi ayarlayın.
3. GCP üzerinde Cloud ıAM kullanarak bir hizmet hesabı oluşturun ve doğru izin düzeylerini tanımlayın. 
4. Bu hizmet hesabı için erişim anahtarları oluşturun.

![Google Cloud Storage için erişim anahtarını alma](media/connector-google-cloud-storage/google-storage-cloud-settings.png)

## <a name="required-permissions"></a>Gerekli izinler

Google Cloud Storage 'dan veri kopyalamak için gerekli izinleri vermiş olduğunuzdan emin olun. Hizmet hesabında tanımlanan izinler `storage.buckets.get` ,, `storage.buckets.list` veya `storage.objects.get` nesne işlemleri için içerebilir.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

Aşağıdaki bölümlerde, Google Cloud Storage 'a özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgiler sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Google Cloud Storage bağlı hizmetleri için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | **Type** özelliği **GoogleCloudStorage**olarak ayarlanmalıdır. | Evet |
| Accesskeyıd | Gizli dizi erişim anahtarının KIMLIĞI. Erişim anahtarını ve gizli anahtarı bulmak için bkz. [Önkoşullar](#prerequisites). |Evet |
| secretAccessKey | Gizli dizi erişim anahtarı. Data Factory güvenli bir şekilde depolamak için bu alanı **SecureString** olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. |Evet |
| serviceUrl | Özel GCS uç noktasını olarak belirtin `https://storage.googleapis.com` . | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure tümleştirme çalışma zamanını veya şirket içinde barındırılan tümleştirme çalışma zamanını (veri depolduğunuz özel bir ağda olması halinde) kullanabilirsiniz. Bu özellik belirtilmezse, hizmet varsayılan Azure tümleştirme çalışma zamanını kullanır. |Hayır |

İşte bir örnek:

```json
{
    "name": "GoogleCloudStorageLinkedService",
    "properties": {
        "type": "GoogleCloudStorage",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": {
                "type": "SecureString",
                "value": "<secret access key>"
            },
            "serviceUrl": "https://storage.googleapis.com"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, `location` Biçim tabanlı bir veri kümesindeki ayarlar altında Google bulut depolaması için desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| tür       | **type** `location` Veri kümesinde bulunan tür özelliğinin **GoogleCloudStorageLocation**olarak ayarlanması gerekir. | Evet      |
| bucketName | GCS demet adı.                                          | Evet      |
| folderPath | Verilen demet altındaki klasörün yolu. Klasörü filtrelemek için bir joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirleyin. | Hayır       |
| fileName   | Belirtilen demet ve klasör yolu altındaki dosya adı. Dosyaları filtrelemek için bir joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirleyin. | Hayır       |

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Google Cloud Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "GoogleCloudStorageLocation",
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

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Google bulut depolama kaynağının desteklediği özelliklerin bir listesini sağlar.

### <a name="google-cloud-storage-as-a-source-type"></a>Kaynak türü olarak Google bulut depolama

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, `storeSettings` Biçim tabanlı bir kopyalama kaynağındaki ayarlar altında Google bulut depolaması için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                                    |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------------------------- |
| tür                     | İçindeki **tür** özelliği `storeSettings` **GoogleCloudStorageReadSettings**olarak ayarlanmalıdır. | Evet                                                         |
| ***Kopyalanacak dosyaları bulun:*** |  |  |
| SEÇENEK 1: statik yol<br> | Veri kümesinde belirtilen belirli bir demet veya klasör/dosya yolundan kopyalama. Bir demet veya klasörden tüm dosyaları kopyalamak istiyorsanız, ayrıca olarak öğesini belirtin `wildcardFileName` `*` . |  |
| Seçenek 2: GCS ön eki<br>-önek | Kaynak GCS dosyalarını filtrelemek için veri kümesinde yapılandırılan belirtilen demet altındaki GCS anahtar adının öneki. Adları ile başlayan GCS anahtarları `bucket_in_dataset/this_prefix` seçilidir. Bir joker karakter filtresinden daha iyi performans sağlayan, GCS 'nin hizmet tarafı filtresini kullanır. | Hayır |
| Seçenek 3: joker karakter<br>-Yavaya Cardfolderpath | Kaynak klasörleri filtrelemek için bir veri kümesinde yapılandırılan belirtilen demet altında joker karakter olan klasör yolu. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir). `^`Klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. <br>[Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Hayır                                            |
| Seçenek 3: joker karakter<br>-Yavaya Cardfilename | Kaynak dosyalarını filtrelemek için, belirtilen demet ve klasör yolu (veya joker karakter klasörü yolu) altındaki joker karakterlerle dosya adı. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir). `^`Klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın.  [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Evet |
| Seçenek 3: dosya listesi<br>-fileListPath | Belirli bir dosya kümesinin kopyalanıp ayrılmadığını gösterir. Veri kümesinde yapılandırılan yolun göreli yolu olan, kopyalamak istediğiniz dosyaların listesini içeren bir metin dosyası üzerine gelin.<br/>Bu seçeneği kullandığınızda, veri kümesinde dosya adı belirtmeyin. [Dosya listesi örneklerinde](#file-list-examples)daha fazla örneğe bakın. |Hayır |
| ***Ek ayarlar:*** |  | |
| öz | Verilerin alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak okunup okunmadığını gösterir. **Özyinelemeli** değeri **true** olarak ayarlandığında ve havuz dosya tabanlı bir depo olduğunda, havuzda boş bir klasör veya alt klasör kopyalanmadığını veya oluşturulamadığına unutmayın. <br>İzin verilen değerler **true** (varsayılan) ve **false**şeklindedir.<br>Bu özellik, yapılandırdığınızda uygulanmaz `fileListPath` . |Hayır |
| deleteFilesAfterCompletion | Hedef depoya başarıyla taşıdıktan sonra, ikili dosyaların kaynak depodan silinip silinmeyeceğini gösterir. Dosya silme dosya başına yapılır, bu nedenle kopyalama etkinliği başarısız olduğunda, bazı dosyaların hedefe zaten kopyalanmış ve kaynaktan silindiği görürsünüz, diğerleri ise kaynak deposunda hala kalır. <br/>Bu özellik yalnızca, veri kaynağı depolamadaki blob, ADLS 1., ADLS 2., S3, Google bulut depolama, dosya, Azure dosyası, SFTP veya FTP olan ikili kopyalama senaryosunda geçerlidir. Varsayılan değer: false. |Hayır |
| modifiedDatetimeStart    | Dosyalar şu özniteliğe göre filtrelenmiştir: son değiştirme. <br>Son değiştirilme zamanı ve arasındaki zaman aralığı içinde ise dosyalar seçilir `modifiedDatetimeStart` `modifiedDatetimeEnd` . Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler **null**olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmayacağı anlamına gelir.  Ne zaman `modifiedDatetimeStart` bir tarih saat değeri olduğunda `modifiedDatetimeEnd` , ancak **null**ise, son değiştirilen özniteliği DateTime değerinden büyük veya ona eşit olan dosyalar seçilir.  Ne zaman `modifiedDatetimeEnd` bir tarih saat değeri olduğunda `modifiedDatetimeStart` , ancak **null**ise, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.<br/>Bu özellik, yapılandırdığınızda uygulanmaz `fileListPath` . | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdaki gibi.                                               | Hayır                                                          |
| maxConcurrentConnections | Depolama için eşzamanlı bağlantı sayısı. Yalnızca veri deposuyla eş zamanlı bağlantıları sınırlandırmak istediğinizde belirtin. | Hayır                                                          |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromGoogleCloudStorage",
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
                    "type": "GoogleCloudStorageReadSettings",
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

Bu bölümde, klasör yolu ve dosya adının joker karakter filtreleriyle elde edilen davranışı açıklanmaktadır.

| demet | anahtar | öz | Kaynak klasör yapısı ve filtre sonucu (kalın olan dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| demet | `Folder*/*` | yanlış | demet<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**ÜzerindeFile2.js**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| demet | `Folder*/*` | true | demet<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**ÜzerindeFile2.js**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**ÜzerindeFile4.js**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| demet | `Folder*/*.csv` | yanlış | demet<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile2.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| demet | `Folder*/*.csv` | true | demet<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile2.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="file-list-examples"></a>Dosya listesi örnekleri

Bu bölümde, kopyalama etkinlik kaynağında bir dosya listesi yolu kullanmanın ortaya çıkan davranışı açıklanmaktadır.

Aşağıdaki kaynak klasör yapısına sahip olduğunuz ve dosyaları kalın yazı tipinde kopyalamak istediğinizi varsayalım:

| Örnek kaynak yapısı                                      | FileListToCopy.txt içerik                             | Data Factory Yapılandırması                                            |
| ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------ |
| demet<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile2.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Veriyi<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileListToCopy.txt | File1.csv<br>Subfolder1/File3.csv<br>Subfolder1/File5.csv | **Veri kümesinde:**<br>Demet`bucket`<br>-Klasör yolu:`FolderA`<br><br>**Kopyalama etkinliği kaynağı:**<br>-Dosya listesi yolu:`bucket/Metadata/FileListToCopy.txt` <br><br>Dosya listesi yolu, veri kümesinde yapılandırılan yolun göreli yolu ile, kopyalamak istediğiniz dosyaların bir listesini, her satıra bir dosya içeren bir metin dosyasını gösterir. |

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md)inceleyin. 

## <a name="delete-activity-properties"></a>Etkinlik özelliklerini Sil

Özelliklerle ilgili ayrıntıları öğrenmek için [silme etkinliği](delete-activity.md)' ni işaretleyin.

## <a name="legacy-models"></a>Eski modeller

Google Cloud Storage 'dan veri kopyalamak için bir Amazon S3 Bağlayıcısı kullanıyorsanız, geriye dönük uyumluluk için olduğu gibi hala desteklenmektedir. Daha önce bahsedilen yeni modeli kullanmanızı öneririz. Data Factory yazma Kullanıcı arabirimi yeni modeli oluşturmaya geçti.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliğinin kaynak ve havuz olarak desteklediği veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
