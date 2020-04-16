---
title: Verileri Azure Dosya Depolama'dan/azure dosya depolamasına kopyalama
description: Desteklenen kaynak veri depolarından Azure Veri Fabrikası'nı kullanarak Azure Dosya Depolama'dan desteklenen lavabo veri depolarına (veya) verileri nasıl kopyalaylayışolarak azure dosya depolamaya kopyalayamamayı öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/10/2019
ms.openlocfilehash: d31ab1d9cb250e0395e8b57342a449a6b9383c64
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415078"
---
# <a name="copy-data-from-or-to-azure-file-storage-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Azure Dosya Depolama'dan veya Azure Dosya Depolamasına kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure Dosya Depolama'ya ve Azure Dosya Depolama'dan verilerin nasıl kopyalanıp kopyalanılacak olduğu açıklanmaktadır. Azure Veri Fabrikası hakkında bilgi edinmek için [giriş makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Dosya Depolama bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
- [Etkinliği silme](delete-activity.md)

Özellikle, bu Azure Dosya Depolama bağlayıcısı, desteklenen dosya biçimleri ve sıkıştırma [codec'leri](supported-file-formats-and-compression-codecs.md)ile dosyaları olduğu gibi kopyalamayı veya ayrıştırma/oluşturmayı destekler.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Azure Dosya Depolamasına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Azure Dosya Depolama bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **AzureFileStorage**. | Evet |
| konak | Azure Dosya Depolama bitiş noktasını aşağıdaki gibi belirtir: <br/>-Kullanıcı UI kullanma: belirtin`\\<storage name>.file.core.windows.net\<file service name>`<br/>- JSON `"host": "\\\\<storage name>.file.core.windows.net\\<file service name>"`kullanma: . | Evet |
| Userıd | Azure Dosya Depolama'sına erişmek için kullanıcıyı şu şekilde belirtin: <br/>-Kullanıcı UI kullanma: belirtin`AZURE\<storage name>`<br/>-JSON kullanma: `"userid": "AZURE\\<storage name>"`. | Evet |
| password | Depolama erişim anahtarını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Süresi'ni veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Kaynak için hayır, lavabo için Evet |

>[!IMPORTANT]
> - Azure Tümleştirme Çalışma Süresi'ni kullanarak verileri Azure Dosya Depolama alanına kopyalamak için, Dosya Depolama nızın konumunu içeren [bir Azure IR'si açıkça oluşturun](create-azure-integration-runtime.md#create-azure-ir) ve bağlantılı hizmeti aşağıdaki örnek olarak ilişkilendirin.
> - Azure dışında kendi kendine barındırılan Tümleştirme Çalışma Süresi'ni kullanarak Azure Dosya Depolama'dan/Azure Dosya Depolama'dan veri kopyalamak için, yerel abunuzlu TCP bağlantı noktası 445'i açmayı unutmayın.

>[!TIP]
>Yazarlık için ADF Kullanıcı Arabirimi'ni kullanırken, altında tür `FileServer` nesnesi oluşturan bağlantılı hizmet oluşturma için "Azure Dosya Depolama" özel girişini bulabilirsiniz.

**Örnek:**

```json
{
    "name": "AzureFileStorageLinkedService",
    "properties": {
        "type": "AzureFileStorage",
        "typeProperties": {
            "host": "\\\\<storage name>.file.core.windows.net\\<file service name>",
            "userid": "AZURE\\<storage name>",
            "password": {
                "type": "SecureString",
                "value": "<storage access key>"
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

Aşağıdaki özellikler, biçim tabanlı veri `location` kümesinde ayarlar altında Azure Dosya Depolama için desteklenir:

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
            "referenceName": "<Azure File Storage linked service name>",
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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Azure Dosya Depolama kaynağı ve lavabo tarafından desteklenen özelliklerin bir listesi bulunmaktadır.

### <a name="azure-file-storage-as-source"></a>Kaynak Olarak Azure Dosya Depolama

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama `storeSettings` kaynağındaki ayarlar altında Azure Dosya Depolama için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Altındaki `storeSettings` tür özelliği **FileServerReadSettings**olarak ayarlanmalıdır. | Evet                                           |
| Özyinelemeli                | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme gerçek olarak ayarlandığında ve lavabo dosya tabanlı bir depo olduğunda, boş bir klasörün veya alt klasörün kopyalanmadığını veya lavaboda oluşturulmadığını unutmayın. İzin verilen değerler **doğru** (varsayılan) ve **yanlıştır.** | Hayır                                            |
| joker KarakterFolderPath       | Kaynak klasörleri filtrelemek için joker karakteriçeren klasör yolu. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. <br>Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Hayır                                            |
| jokerKarakterFileName         | Kaynak dosyaları filtrelemek için verilen folderPath/jokerKarakterIn altında joker karakter içeren dosya adı. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın.  Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Veri `fileName` kümesinde belirtilmemişse evet |
| modifiyeDatetimeBaşlat    | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanamayacağı anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır                                            |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromAzureFileStorage",
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

### <a name="azure-file-storage-as-sink"></a>Lavabo Olarak Azure Dosya Depolama

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama `storeSettings` lavabosunda ayarlar altında Azure Dosya Depolama için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Altındaki `storeSettings` tür özelliği **FileServerWriteSettings**olarak ayarlanmalıdır. | Evet      |
| copyBehavior             | Kaynak dosya tabanlı bir veri deposundan dosya olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>- Koruma Hiyerarşisi (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasörüne göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><b>- FlattenHiyerarşiy</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulmuş adları vardır. <br/><b>- Birleştirme Dosyaları</b>: Kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya adı belirtilirse, birleştirilen dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulmuş bir dosya adıdır. | Hayır       |
| maxConcurrentConnections | Veri deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır       |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToAzureFileStorage",
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
| true |flattenHiyerarşi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File3 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulmuş ad |
| true |birleştirmeDosyaları | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + Dosya 5 içeriği otomatik olarak oluşturulmuş dosya adı içeren bir dosyada birleştirilir |
| yanlış |korumaHiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |flattenHiyerarşi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulmuş ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |birleştirmeDosyaları | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 içeriği otomatik olarak oluşturulan dosya adı içeren bir dosyada birleştirilir. File1 için otomatik olarak oluşturulmuş ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |

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
| folderPath | Klasöre giden yol. <br/><br/>Joker karakter filtresi desteklenir, joker `*` karakterlere izin verilir: (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. <br/><br/>Örnekler: rootfolder/subfolder/, Klasör [ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. |Evet |
| fileName | Belirtilen "folderPath" altındaki dosya(lar) için **ad veya joker karakter filtresi.** Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları işaret edir. <br/><br/>Filtre için izin verilen `*` joker karakterler şunlardır: (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>- Örnek 1:`"fileName": "*.csv"`<br/>- Örnek 2:`"fileName": "???20180427.txt"`<br/>Gerçek `^` dosya adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde ve **korumaHiyerarşisi** etkinlik lavabosunda belirtilmediğinde, kopyalama etkinliği otomatik olarak aşağıdaki desenle dosya adını oluşturur: "*Data.[ etkinlik çalıştırın ID GUID]. [GUID if FlattenHiyerarşiy]. [yapılandırılırsa biçimlendirme]. [yapılandırılırsa sıkıştırma]*"örneğin "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz"; sorgu yerine tablo adını kullanarak tablo kaynağından kopyalarsanız, ad deseni "*[tablo adı].[ biçimlendirin]. [yapılandırılırsa sıkıştırma]*", örneğin "MyTable.csv". |Hayır |
| modifiyeDatetimeBaşlat | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek veri hareketinin genel performansının etkileneceğini unutmayın. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanamayacağı anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek veri hareketinin genel performansının etkileneceğini unutmayın. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanamayacağı anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir.| Hayır |
| biçim | Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın.<br/><br/>Belirli bir biçimde dosyaları ayrışdırmak veya oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](supported-file-formats-and-compression-codecs-legacy.md#text-format) [Json Formatı,](supported-file-formats-and-compression-codecs-legacy.md#json-format) [Avro Biçimi,](supported-file-formats-and-compression-codecs-legacy.md#avro-format) [Ork Biçimi](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parke Biçimi](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) bölümlerine bakın. |Hayır (yalnızca ikili kopyalama senaryosu için) |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve sıkıştırma codec'lerine](supported-file-formats-and-compression-codecs-legacy.md#compression-support)bakın.<br/>Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**.<br/>Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** |Hayır |

>[!TIP]
>Tüm dosyaları bir klasörün altında kopyalamak için **yalnızca folderPath'i** belirtin.<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için **folderPath** klasörünü klasör bölümü ve dosya adı ile **dosya adı** ile belirtin.<br>Bir klasörün altındaki dosyaların alt kümesini kopyalamak için **folderPath** klasörünü klasör bölümüyle ve joker karakter filtresiile **fileName'yi** belirtin.

>[!NOTE]
>Dosya filtresi için "fileFilter" özelliğini kullanıyorsanız, ileriye dönük olarak "fileName"ye eklenen yeni filtre özelliğini kullanmanız önerilirken, bu özellik olduğu gibi desteklenir.

**Örnek:**

```json
{
    "name": "AzureFileStorageDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<Azure File Storage linked service name>",
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
        "name": "CopyFromAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure File Storage input dataset name>",
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
| copyBehavior | Kaynak dosya tabanlı veri deposundan dosyalar olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>- Koruma Hiyerarşisi (varsayılan)</b>: hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasörüne göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><b>- FlattenHiyerarşiy</b>: kaynak klasöründen tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulmuş adı var. <br/><b>- Birleştirme Dosyaları</b>: kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya Adı belirtilirse, birleştirilen dosya adı belirtilen ad olur; aksi takdirde, otomatik olarak oluşturulan dosya adı olacaktır. | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure File Storage output dataset name>",
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
