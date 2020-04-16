---
title: Azure Veri Fabrikası'nı kullanarak verileri dosya sisteminden/dosya sisteminden kopyalama
description: Azure Veri Fabrikası'nı kullanarak, desteklenen kaynak veri depolarından dosya sisteminden dosya sistemine kadar verileri nasıl kopyalayış edin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: jingwang
ms.openlocfilehash: 21329e3fada62036bc5b8d2bf6cdf6b3be5adad3
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417402"
---
# <a name="copy-data-to-or-from-a-file-system-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri bir dosya sistemine veya dosya sisteminden kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-onprem-file-system-connector.md)
> * [Geçerli sürüm](connector-file-system.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, dosya sistemine ve dosya sisteminden verilerin nasıl kopyalanıp kopyalanılacak olduğu açıklanmıştır. Azure Veri Fabrikası hakkında bilgi edinmek için [giriş makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu dosya sistemi bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
- [Etkinliği silme](delete-activity.md)

Özellikle, bu dosya sistemi bağlayıcısı destekler:

- Dosyaları yerel makineden veya ağ dosyası paylaşımından kopyalama. Bir Linux dosya paylaşımı nı kullanmak için [Samba'yı](https://www.samba.org/) Linux sunucunuza yükleyin.
- **Windows** kimlik doğrulamasını kullanarak dosyaları kopyalama.
- Dosyaları olduğu gibi kopyalama veya desteklenen dosya biçimleri [ve sıkıştırma codec'leri](supported-file-formats-and-compression-codecs.md)ile dosyaları ayrıştırma/oluşturma.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, dosya sistemine özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Dosya sistemi bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **FileServer.** | Evet |
| konak | Kopyalamak istediğiniz klasörün kök yolunu belirtir. Dizedeki özel\" karakterler için " kaçış karakterini kullanın. Örnekler için [Örnek bağlantılı hizmet ve veri kümesi tanımlarına](#sample-linked-service-and-dataset-definitions) bakın. | Evet |
| Userıd | Sunucuya erişimi olan kullanıcının kimliğini belirtin. | Evet |
| password | Kullanıcının parolasını belirtin (userid). Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

### <a name="sample-linked-service-and-dataset-definitions"></a>Örnek bağlantılı hizmet ve veri kümesi tanımları

| Senaryo | Bağlantılı hizmet tanımında "ana bilgisayar" | dataset tanımında "folderPath" |
|:--- |:--- |:--- |
| Tümleştirme Runtime makinesinde yerel klasör: <br/><br/>Örnekler: D:\\ \* veya D:\folder\subfolder\\* |JSON olarak:`D:\\`<br/>UI üzerinde:`D:\` |JSON olarak: `.\\` veya`folder\\subfolder`<br>UI'de: `.\` veya`folder\subfolder` |
| Uzaktan paylaşılan klasör: <br/><br/>Örnekler: \\ \\\\myserver\\ \* \\ \\share\\veya\\\\myserver share folder alt klasörü\\* |JSON olarak:`\\\\myserver\\share`<br/>UI üzerinde:`\\myserver\share` |JSON olarak: `.\\` veya`folder\\subfolder`<br/>UI'de: `.\` veya`folder\subfolder` |

>[!NOTE]
>UI üzerinden yazarken, JSON üzerinden yaptığınız gibi çift`\\`eğik çizgi () giriş yapmanıza gerek yok, tek bir ters eğik çizgi belirtin.

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı veri `location` kümesinde ayarlar altında dosya sistemi için desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesinin `location` altındaki tür özelliği **FileServerLocation**olarak ayarlanmalıdır. | Evet      |
| folderPath | Klasöre giden yol. Klasöre filtre açmak için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarını belirtin. | Hayır       |
| fileName   | Verilen folderPath altında dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarını belirtin. | Hayır       |

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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, dosya sistemi kaynağı ve lavabo tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="file-system-as-source"></a>Kaynak olarak dosya sistemi

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama `storeSettings` kaynağındaki ayarlar altında dosya sistemi için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Altındaki `storeSettings` tür özelliği **FileServerReadSettings**olarak ayarlanmalıdır. | Evet                                           |
| Özyinelemeli                | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme gerçek olarak ayarlandığında ve lavabo dosya tabanlı bir depo olduğunda, boş bir klasörün veya alt klasörün kopyalanmadığını veya lavaboda oluşturulmadığını unutmayın. İzin verilen değerler **doğru** (varsayılan) ve **yanlıştır.** | Hayır                                            |
| joker KarakterFolderPath       | Kaynak klasörleri filtrelemek için joker karakteriçeren klasör yolu. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. <br>Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Hayır                                            |
| jokerKarakterFileName         | Kaynak dosyaları filtrelemek için verilen folderPath/jokerKarakterIn altında joker karakter içeren dosya adı. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın.  Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. [Lütfen bunun dosya Adı ayarınızı geçersiz kacağını unutmayın] | Veri `fileName` kümesinde belirtilmemişse evet |
| modifiyeDatetimeBaşlat    | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanamayacağı anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır                                            |

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
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "FileServerReadSettings",
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

### <a name="file-system-as-sink"></a>Lavabo olarak dosya sistemi

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama `storeSettings` lavabosunda ayarlar altında dosya sistemi için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Altındaki `storeSettings` tür özelliği **FileServerWriteSettings**olarak ayarlanmalıdır. | Evet      |
| copyBehavior             | Kaynak dosya tabanlı bir veri deposundan dosya olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>- Koruma Hiyerarşisi (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasörüne göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><b>- FlattenHiyerarşiy</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulmuş adları vardır. <br/><b>- Birleştirme Dosyaları</b>: Kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya adı belirtilirse, birleştirilen dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulmuş bir dosya adıdır. | Hayır       |
| maxConcurrentConnections | Veri deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır       |

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
                    "type": "FileServerWriteSettings",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, klasör yolunun ve dosya adının joker karakter filtreleriyle ortaya çıkan davranışı açıklanmaktadır.

| folderPath | fileName | Özyinelemeli | Kaynak klasör yapısı ve filtre sonucu **(kalın** renkteki dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (boş, varsayılan kullanın) | yanlış | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*` | (boş, varsayılan kullanın) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*` | `*.csv` | yanlış | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri

Bu bölümde, özyinelemeli ve copyBehavior değerlerinin farklı birleşimleri için Kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| Özyinelemeli | copyBehavior | Kaynak klasör yapısı | Elde çıkan hedef |
|:--- |:--- |:--- |:--- |
| true |korumaHiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef klasör Folder1 kaynakla aynı yapıyla oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5. |
| true |flattenHiyerarşi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |birleştirmeDosyaları | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + Dosya 5 içeriği otomatik oluşturulan dosya adı ile bir dosya da birleştirilir |
| yanlış |korumaHiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |flattenHiyerarşi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulan ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |birleştirmeDosyaları | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 içeriği otomatik olarak oluşturulan dosya adı içeren bir dosyada birleştirilir. File1 için otomatik olarak oluşturulan ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |

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
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **FileShare** |Evet |
| folderPath | Klasöre giden yol. Joker karakter filtresi desteklenir, joker `*` karakterlere izin verilir: (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. <br/><br/>Örnekler: rootfolder/subfolder/, Örnek [bağlantılı hizmet ve dataset tanımları](#sample-linked-service-and-dataset-definitions) ve [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek bakın. |Hayır |
| fileName | Belirtilen "folderPath" altındaki dosya(lar) için **ad veya joker karakter filtresi.** Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları işaret edir. <br/><br/>Filtre için izin verilen `*` joker karakterler şunlardır: (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>- Örnek 1:`"fileName": "*.csv"`<br/>- Örnek 2:`"fileName": "???20180427.txt"`<br/>Gerçek `^` dosya adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde ve **korumaHiyerarşisi** etkinlik lavabosunda belirtilmediğinde, kopyalama etkinliği otomatik olarak aşağıdaki desenle dosya adını oluşturur: "*Data.[ etkinlik çalıştırın ID GUID]. [GUID if FlattenHiyerarşiy]. [yapılandırılırsa biçimlendirme]. [yapılandırılırsa sıkıştırma]*"örneğin "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz"; sorgu yerine tablo adını kullanarak tablo kaynağından kopyalarsanız, ad deseni "*[tablo adı].[ biçimlendirin]. [yapılandırılırsa sıkıştırma]*", örneğin "MyTable.csv". |Hayır |
| modifiyeDatetimeBaşlat | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek veri hareketinin genel performansının etkileneceğini unutmayın. <br/><br/> Özellikler, veri kümesine dosya özniteliği filtresi uygulanmayacak anlamına gelen NULL olabilir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek veri hareketinin genel performansının etkileneceğini unutmayın. <br/><br/> Özellikler, veri kümesine dosya özniteliği filtresi uygulanmayacak anlamına gelen NULL olabilir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir.| Hayır |
| biçim | Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın.<br/><br/>Belirli bir biçimde dosyaları ayrışdırmak veya oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](supported-file-formats-and-compression-codecs-legacy.md#text-format) [Json Formatı,](supported-file-formats-and-compression-codecs-legacy.md#json-format) [Avro Biçimi,](supported-file-formats-and-compression-codecs-legacy.md#avro-format) [Ork Biçimi](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parke Biçimi](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) bölümlerine bakın. |Hayır (yalnızca ikili kopyalama senaryosu için) |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve sıkıştırma codec'lerine](supported-file-formats-and-compression-codecs-legacy.md#compression-support)bakın.<br/>Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**.<br/>Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** |Hayır |

>[!TIP]
>Tüm dosyaları bir klasörün altında kopyalamak için **yalnızca folderPath'i** belirtin.<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için **folderPath** klasörünü klasör bölümü ve dosya adı ile **dosya adı** ile belirtin.<br>Bir klasörün altındaki dosyaların alt kümesini kopyalamak için **folderPath** klasörünü klasör bölümüyle ve joker karakter filtresiile **fileName'yi** belirtin.

>[!NOTE]
>Dosya filtresi için "fileFilter" özelliğini kullanıyorsanız, ileriye dönük olarak "fileName"ye eklenen yeni filtre özelliğini kullanmanız önerilirken, bu özellik olduğu gibi desteklenir.

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

### <a name="legacy-copy-activity-source-model"></a>Eski kopyalama etkinliği kaynak modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **FileSystemSource** |Evet |
| Özyinelemeli | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme doğru ayarlandığında ve lavabo dosya tabanlı depolandığında, boş klasör/alt klasör lavaboda kopyalanmaz/oluşturulmaz.<br/>İzin verilen değerler şunlardır: **true** (varsayılan), **yanlış** | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

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

### <a name="legacy-copy-activity-sink-model"></a>Eski kopyalama etkinliği lavabo modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabonun türü özelliği şu şekilde ayarlanmalıdır: **FileSystemSink** |Evet |
| copyBehavior | Kaynak dosya tabanlı veri deposundan dosyalar olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>- Koruma Hiyerarşisi (varsayılan)</b>: hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasörüne göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><b>- FlattenHiyerarşiy</b>: kaynak klasöründen tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulan adı var. <br/><b>- Birleştirme Dosyaları</b>: kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Birleştirme sırasında kayıt çoğaltma yapılmaz. Dosya Adı belirtilirse, birleştirilen dosya adı belirtilen ad olur; aksi takdirde, otomatik olarak oluşturulan dosya adı olacaktır. | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

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

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
