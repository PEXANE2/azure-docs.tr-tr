---
title: Azure Data Factory | kullanarak bir dosya sistemine veri kopyalama Microsoft Docs
description: Azure Data Factory kullanarak, desteklenen kaynak veri depolarından dosya sistemine desteklenen havuz veri depolarına (veya) veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: jingwang
ms.openlocfilehash: b22242ac617f8dca95941a489c38658f5721851a
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387284"
---
# <a name="copy-data-to-or-from-a-file-system-by-using-azure-data-factory"></a>Azure Data Factory kullanarak bir dosya sistemine veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-file-system-connector.md)
> * [Geçerli sürüm](connector-file-system.md)

Bu makalede, dosya sistemine veya dosya sistemine verilerin nasıl kopyalanacağı özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [tanıtım makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu dosya sistemi Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)

Özellikle, bu dosya sistemi Bağlayıcısı şunları destekler:

- Dosyalar/bilgisayardan yerel makineden veya ağ dosya paylaşımından kopyalanıyor. Linux dosya paylaşımının kullanılması için Linux sunucunuza [Samba](https://www.samba.org/) 'yi yüklersiniz.
- **Windows** kimlik doğrulaması kullanılarak dosyalar kopyalanıyor.
- Dosya kopyalama veya [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs.md)ile dosyaları ayrıştırma/oluşturma/oluşturma.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, dosya sistemine özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Dosya sistemi bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği: dosya **sunucusu**olarak ayarlanmalıdır. | Yes |
| Konağının | Kopyalamak istediğiniz klasörün kök yolunu belirtir. Dizedeki özel karakterler için "\" kaçış karakterini kullanın. Örnekler için bkz. [örnek bağlantılı hizmet ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) . | Yes |
| UserID | Sunucuya erişimi olan kullanıcının KIMLIĞINI belirtin. | Yes |
| password | Kullanıcının parolasını belirtin (Kullanıcı kimliği). Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Yes |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Integration Runtime](concepts-integration-runtime.md) . [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. |Hayır |

### <a name="sample-linked-service-and-dataset-definitions"></a>Örnek bağlantılı hizmet ve veri kümesi tanımları

| Senaryo | bağlı hizmet tanımında "ana bilgisayar" | veri kümesi tanımında "folderPath" |
|:--- |:--- |:--- |
| Integration Runtime makinesindeki yerel klasör: <br/><br/>Örnekler: D: \\ @ no__t-1 veya D:\folder\alt klasörü @ no__t-2 @ no__t-3 |JSON içinde: `D:\\`<br/>Kullanıcı arabiriminde: `D:\` |JSON içinde: `.\\` veya `folder\\subfolder`<br>Kullanıcı arabiriminde: `.\` veya `folder\subfolder` |
| Uzak paylaşılan klasör: <br/><br/>Örnekler: \\ @ no__t-1sunucum @ no__t-2share @ no__t-3 @ no__t-4 veya \\ @ no__t-6sunucum @ no__t-7share @ no__t-8folder @ no__t-9alt klasörü @ no__t-10 @ no__t-11 |JSON içinde: `\\\\myserver\\share`<br/>Kullanıcı arabiriminde: `\\myserver\share` |JSON içinde: `.\\` veya `folder\\subfolder`<br/>Kullanıcı arabiriminde: `.\` veya `folder\subfolder` |

>[!NOTE]
>Kullanıcı arabirimi aracılığıyla yazarken, JSON aracılığıyla yaptığınız gibi çıkmak için çift ters eğik çizgi (`\\`) girmeniz gerekmez, tek eğik çizgi belirtin.

**Örnek:**

```json
{
    "name": "FileLinkedService",
    "properties": {
        "type": "FileServer",
        "typeProperties": {
            "host": "<host>",
            "userid": "<domain>\\<user>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

**Parquet, sınırlandırılmış metin, JSON, avro ve ikili biçimine**veri kopyalamak için, biçim tabanlı veri kümesinde ve desteklenen ayarlarda [Parquet biçimine](format-parquet.md), [sınırlandırılmış metin biçimine](format-delimited-text.md), [avro biçimine](format-avro.md) ve [ikili biçim](format-binary.md) makalesine bakın . Dosya sistemi için, biçim tabanlı veri kümesindeki `location` ayarları altında aşağıdaki özellikler desteklenir:

| Özellik   | Açıklama                                                  | Gereklidir |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesindeki `location` ' ın altında bulunan tür özelliği **Fileserverlocation**olarak ayarlanmalıdır. | Yes      |
| folderPath | Klasörün yolu. Klasörü filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirtin. | Hayır       |
| fileName   | Verilen folderPath altındaki dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirtin. | Hayır       |

> [!NOTE]
> Sonraki bölümde belirtilen Parquet/metin biçimine sahip **FileShare** türü veri kümesi, geriye dönük uyumluluk için kopyalama/arama/GetMetadata etkinliğinin olduğu gibi hala desteklenir, ancak eşleme veri akışı ile çalışmaz. Bu yeni modeli ileriye doğru kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi bu yeni türleri oluşturmaya geçti.

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<File system linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "FileServerLocation",
                "folderPath": "root/folder/subfolder"
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

**Orc biçimindeki**dosya sistemine ve bu bilgisayardan veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | DataSet 'in Type özelliği: **FileShare** olarak ayarlanmalıdır |Yes |
| folderPath | Klasörün yolu. Joker karakter filtresi desteklenir, izin verilen joker karakterler şunlardır: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek klasörünüzün adında joker karakter veya bu kaçış karakteri varsa, çıkış için `^` kullanın. <br/><br/>Örnekler: RootFolder/alt klasör/, bkz. [örnek bağlantılı hizmet ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) ve [klasör ve dosya filtresi örnekleri](#folder-and-file-filter-examples). |Hayır |
| fileName | Belirtilen "folderPath" altındaki dosya (ler) için **ad veya joker karakter filtresi** . Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları işaret eder. <br/><br/>Filtre için, izin verilen joker karakterler şunlardır: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Gerçek dosya adınızın joker karakter veya içinde bu kaçış karakteri varsa, çıkış için `^` kullanın.<br/><br/>Bir çıkış veri kümesi için dosya adı belirtilmediğinde ve bir etkinlik havuzunda **Preservehierarchy** belirtilmemişse, Copy etkinliği dosya adını aşağıdaki Düzenle otomatik olarak oluşturur: "*Data. [ Etkinlik çalıştırma KIMLIĞI GUID 'SI]. [DÜZEDEN hiyerarşi varsa GUID]. [yapılandırıldıysa Biçimlendir]. [yapılandırıldıysa sıkıştırma]* ", ör." Data. 0a405f8a-93ff-4c6f-B3BE-f69616f1df7a. txt. gz "; sorgu yerine tablo adını kullanarak tablo kaynağından kopyalama yaparsanız, ad deseninin adı " *[tablo adı]. [ Biçim]. [yapılandırıldıysa sıkıştırma]* ", ör." MyTable. csv ". |Hayır |
| modifiedDatetimeStart | Öznitelikleri temel alan dosya filtresi: son değiştirme. Son değiştirilme zamanı, `modifiedDatetimeStart` ve `modifiedDatetimeEnd` arasındaki zaman aralığı içinde ise bu dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Çok büyük miktarlarda dosyadan dosya filtresi yapmak istediğinizde, bu ayarın etkinleştirilmesi durumunda veri hareketinin genel performansını unutmayın. <br/><br/> Veri kümesine hiçbir dosya özniteliği filtresinin uygulanamadığını gösteren Özellikler NULL olabilir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeEnd` NULL ise, son değiştirilen özniteliği DateTime değeri ile eşit veya daha büyük olan dosyalar seçilir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| modifiedDatetimeEnd | Öznitelikleri temel alan dosya filtresi: son değiştirme. Son değiştirilme zamanı, `modifiedDatetimeStart` ve `modifiedDatetimeEnd` arasındaki zaman aralığı içinde ise bu dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Çok büyük miktarlarda dosyadan dosya filtresi yapmak istediğinizde, bu ayarın etkinleştirilmesi durumunda veri hareketinin genel performansını unutmayın. <br/><br/> Veri kümesine hiçbir dosya özniteliği filtresinin uygulanamadığını gösteren Özellikler NULL olabilir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeEnd` NULL ise, son değiştirilen özniteliği DateTime değeri ile eşit veya daha büyük olan dosyalar seçilir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| formatını | Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıkış veri kümesi tanımlarının biçim bölümünü atlayın.<br/><br/>Belirli bir biçimdeki dosyaları ayrıştırmak veya oluşturmak isterseniz, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **jsonformat**, **avroformat**, **orcformat**, **parquetformat**. Biçim ' in altındaki **Type** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için bkz. [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [JSON biçimi](supported-file-formats-and-compression-codecs.md#json-format), [avro Format](supported-file-formats-and-compression-codecs.md#avro-format), [orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format)ve [Parquet biçim](supported-file-formats-and-compression-codecs.md#parquet-format) bölümleri. |Hayır (yalnızca ikili kopya senaryosu için) |
| masıyla | Verilerin türünü ve sıkıştırma düzeyini belirtin. Daha fazla bilgi için bkz. [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler şunlardır: **gzip**, **söndür**, **bzip2**ve **zipsöndür**.<br/>Desteklenen düzeyler şunlardır: **en iyi** ve **en hızlı**. |Hayır |

>[!TIP]
>Bir klasörün altındaki tüm dosyaları kopyalamak için, yalnızca **FolderPath** ' i belirtin.<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için klasör **bölümü ve dosya adı ile dosya** adı ile **FolderPath** belirtin.<br>Bir klasörün altına bir dosya alt kümesini kopyalamak için klasör bölümü ve **dosya adı** joker karakter filtresi ile **FolderPath** öğesini belirtin.

>[!NOTE]
>Dosya filtresi için "fileFilter" özelliğini kullanıyorsanız, "dosya adı" ' na eklenen yeni filtre özelliğini kullanmanız önerilirken, olduğu gibi hala desteklenmektedir.

**Örnek:**

```json
{
    "name": "FileSystemDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<file system linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, dosya sistemi kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="file-system-as-source"></a>Kaynak olarak dosya sistemi

- **Parquet, sınırlandırılmış metin, JSON, avro ve ikili biçiminden**kopyalamak Için, [Parquet, SıNıRLANDıRıLMıŞ metin, JSON, avro ve ikili biçim kaynağı](#format-based-source) bölümüne bakın.
- **Orc biçimi**gibi diğer biçimlerden kopyalamak için [diğer biçim kaynağı](#other-format-source) bölümüne bakın.

#### <a name="format-based-source"></a>Parquet, sınırlandırılmış metin, JSON, avro ve ikili biçim kaynağı

**Parquet, sınırlandırılmış metin, JSON, avro ve ikili biçimdeki**verileri kopyalamak için, biçim tabanlı kopyalama etkinliği kaynağı ve desteklenmiş olan [Parquet biçimine](format-parquet.md), [sınırlandırılmış metin biçimine](format-delimited-text.md), [avro biçimine](format-avro.md) ve [ikili biçim](format-binary.md) makalesine bakın Ayarlar. Dosya sistemi için, biçim tabanlı kopyalama kaynağında `storeSettings` ayarları altında aşağıdaki özellikler desteklenir:

| Özellik                 | Açıklama                                                  | Gereklidir                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | @No__t-0 altındaki Type özelliği **Fileserverreadsetting**olarak ayarlanmalıdır. | Yes                                           |
| öz                | Verilerin alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak okunup okunmadığını gösterir. Özyinelemeli değeri true olarak ayarlandığında ve havuz dosya tabanlı bir depo olduğunda, havuzda boş bir klasör veya alt klasör kopyalanmadığını veya oluşturulamadığına unutmayın. İzin verilen değerler **true** (varsayılan) ve **false**şeklindedir. | Hayır                                            |
| Yavaya Cardfolderpath       | Kaynak klasörlerin filtreleneceği joker karakter içeren klasör yolu. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek klasörünüzün adında joker karakter veya bu kaçış karakteri varsa, çıkış için `^` kullanın. <br>[Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Hayır                                            |
| Yavaya Cardfilename         | Kaynak dosyalarını filtrelemek için, belirtilen folderPath/, Cardfolderpath altındaki joker karakterlerle dosya adı. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek klasörünüzün adında joker karakter veya bu kaçış karakteri varsa, çıkış için `^` kullanın.  [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Veri kümesinde `fileName` belirtilmemişse Evet |
| modifiedDatetimeStart    | Öznitelikleri temel alan dosya filtresi: son değiştirme. Son değiştirilme zamanı, `modifiedDatetimeStart` ve `modifiedDatetimeEnd` arasındaki zaman aralığı içinde ise bu dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmayacak anlamına gelir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeEnd` NULL ise, son değiştirilen özniteliği DateTime değeri ile eşit veya daha büyük olan dosyalar seçilir.  @No__t-0 ' ın DateTime değeri varsa, ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdaki gibi.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır                                            |

> [!NOTE]
> Parquet/delimited metin biçimi için, sonraki bölümde bahsedilen **filesystemsource** türü kopyalama etkinliği kaynağı, geriye dönük uyumluluk için olduğu gibi desteklenmeye devam etmektedir. Bu yeni modeli ileriye doğru kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi bu yeni türleri oluşturmaya geçti.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromFileSystem",
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
                    "type": "FileServerReadSetting",
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

**Orc biçimindeki**dosya sisteminden veri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği: **Filesystemsource** olarak ayarlanmalıdır |Yes |
| öz | Verilerin alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak okunup okunmadığını gösterir. Özyinelemeli değeri true olarak ayarlandığında ve havuz dosya tabanlı depo ise, boş klasör/alt klasör, havuzda kopyalanmayacak/oluşturulmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<file system input dataset name>",
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

### <a name="file-system-as-sink"></a>Havuz olarak dosya sistemi

- **Parquet, ayrılmış metin, JSON, avro ve ikili biçimi**kopyalamak Için, [Parquet, SıNıRLANDıRıLMıŞ metin, JSON, avro ve ikili biçim havuzu](#format-based-sink) bölümüne bakın.
- **Orc biçimi**gibi diğer biçimlere kopyalamak için [diğer biçim havuzu](#other-format-sink) bölümüne bakın.

#### <a name="format-based-sink"></a>Parquet, sınırlandırılmış metin, JSON, avro ve ikili biçim havuzu

**Parquet, sınırlandırılmış metin, JSON, avro ve ikili biçimdeki**verileri kopyalamak için, biçim tabanlı kopyalama etkinlik havuzunda ve desteklenen [Parquet biçimine](format-parquet.md), [sınırlandırılmış metin biçimine](format-delimited-text.md), [avro biçimine](format-avro.md) ve [ikili biçim](format-binary.md) makalesine bakın Ayarlar. Dosya sistemi için, biçim tabanlı kopya havuzunda `storeSettings` ayarları altında aşağıdaki özellikler desteklenir:

| Özellik                 | Açıklama                                                  | Gereklidir |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | @No__t-0 altındaki Type özelliği **Fileserverwritesetting**olarak ayarlanmalıdır. | Yes      |
| copyBehavior             | Kaynak dosya tabanlı bir veri deposundan dosyalar olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-Preservehierarchy (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu hedef dosyanın hedef klasöre göreli yolu ile aynıdır.<br/><b>-DÜZEDEN hiyerarşi</b>: kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlara sahiptir. <br/><b>-Mergefiles</b>: kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya adı belirtilmişse, birleştirilmiş dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulan bir dosya adıdır. | Hayır       |
| maxConcurrentConnections | Veri deposuna eşzamanlı olarak bağlanmak için bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır       |

> [!NOTE]
> Parquet/delimited metin biçimi için, sonraki bölümde bahsedilen **Filesystemmsink** türü kopyalama etkinliği havuzu, geriye dönük uyumluluk için olduğu gibi desteklenmeye devam etmektedir. Bu yeni modeli ileriye doğru kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi bu yeni türleri oluşturmaya geçti.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "FileServerWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Diğer biçim havuzu

**Orc biçimindeki**dosya sistemine veri kopyalamak için, **Havuz** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun Type özelliği: **Filesystemmsink** olarak ayarlanmalıdır |Yes |
| copyBehavior | Kaynak dosya tabanlı veri deposundan dosyalar olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-Preservehierarchy (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu hedef dosyanın hedef klasöre göreli yolu ile aynıdır.<br/><b>-DÜZEDEN hiyerarşi</b>: kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak üretilen adı var. <br/><b>-Mergefiles</b>: kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya adı belirtilmişse, birleştirilmiş dosya adı belirtilen ad olur; Aksi takdirde, otomatik olarak oluşturulan dosya adı olacaktır. | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<file system output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "FileSystemSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, klasör yolu ve dosya adının joker karakter filtreleriyle elde edilen davranışı açıklanmaktadır.

| folderPath | fileName | öz | Kaynak klasör yapısı ve filtre sonucu ( **kalın** olan dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (boş, Varsayılanı kullan) | yanlış | Klasör a<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3**FILE1. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3**dosya2. JSON**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File3.csv<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File4.json<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp; @ no__t-1 @ no__t-2&nbsp;File6.csv |
| `Folder*` | (boş, Varsayılanı kullan) | doğru | Klasör a<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3**FILE1. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3**dosya2. JSON**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7**File3. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7**File4. JSON**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7**File5. csv**<br/>AnotherFolderB<br/>&nbsp; @ no__t-1 @ no__t-2&nbsp;File6.csv |
| `Folder*` | `*.csv` | yanlış | Klasör a<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3**FILE1. csv**<br/>&nbsp; @ no__t-1 @ no__t-2&nbsp;File2.json<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File3.csv<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File4.json<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp; @ no__t-1 @ no__t-2&nbsp;File6.csv |
| `Folder*` | `*.csv` | doğru | Klasör a<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3**FILE1. csv**<br/>&nbsp; @ no__t-1 @ no__t-2&nbsp;File2.json<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7**File3. csv**<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6&nbsp;File4.json<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7**File5. csv**<br/>AnotherFolderB<br/>&nbsp; @ no__t-1 @ no__t-2&nbsp;File6.csv |

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri

Bu bölümde, özyinelemeli ve copyBehavior değerlerinin farklı birleşimleri için kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| öz | copyBehavior | Kaynak klasör yapısı | Sonuç hedefi |
|:--- |:--- |:--- |:--- |
| doğru |preserveHierarchy | Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File2<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File3<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File4<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File5 | Hedef klasör Klasör1, kaynak ile aynı yapıyla oluşturulur:<br/><br/>Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File2<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File3<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File4<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File5. |
| doğru |DÜZEDEN hiyerarşisi | Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File2<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File3<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File4<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File5 | Hedef Klasör1 aşağıdaki yapıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Otomatik üretilen ad Fıle1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Otomatik üretilen ad dosya2<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Otomatik üretilen ad File3<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Otomatik üretilen ad File4<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Otomatik üretilen ad File5 |
| doğru |mergeFiles | Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File2<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File3<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File4<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File5 | Hedef Klasör1 aşağıdaki yapıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File1 + dosya2 + File3 + File4 + dosya 5 içerikleri, otomatik olarak oluşturulan dosya adı ile tek bir dosyada birleştirilir |
| yanlış |preserveHierarchy | Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File2<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File3<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File4<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File5 | Klasör1 hedef klasörü aşağıdaki yapıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File2<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte alınmaz. |
| yanlış |DÜZEDEN hiyerarşisi | Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File2<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File3<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File4<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File5 | Klasör1 hedef klasörü aşağıdaki yapıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Otomatik üretilen ad Fıle1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Otomatik üretilen ad dosya2<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte alınmaz. |
| yanlış |mergeFiles | Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File2<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3Subfolder1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File3<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File4<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7File5 | Klasör1 hedef klasörü aşağıdaki yapıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3File1 + dosya2 içerikleri, otomatik olarak oluşturulan dosya adı ile tek bir dosyada birleştirilir. Fıle1 için otomatik olarak oluşturulan ad<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte alınmaz. |

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) denetleyin 

## <a name="delete-activity-properties"></a>Etkinlik özelliklerini Sil

Özelliklerle ilgili ayrıntıları öğrenmek için [silme etkinliği](delete-activity.md) onay

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
