---
title: Azure Veri Fabrikası'nı kullanarak ftp sunucusundan veri kopyalama
description: Bir Azure Veri Fabrikası ardışık alanında kopyalama etkinliği kullanarak FTP sunucusundan desteklenen bir lavabo veri deposuna verileri nasıl kopyalayabilirsiniz öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/02/2020
ms.author: jingwang
ms.openlocfilehash: b215531fdc1a1bb07b33c427623d5cd4f5f8219a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252473"
---
# <a name="copy-data-from-ftp-server-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak FTP sunucusundan verileri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
>
> * [Sürüm 1](v1/data-factory-ftp-connector.md)
> * [Geçerli sürüm](connector-ftp.md)

Bu makalede FTP sunucusundan verilerin nasıl kopyalanılıp kopyalanılısA bunlar özetlenir. Azure Veri Fabrikası hakkında bilgi edinmek için [giriş makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu FTP bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
- [Etkinliği silme](delete-activity.md)

Özellikle, bu FTP konektörü destekler:

- **Temel** veya **Anonim** kimlik doğrulamasını kullanarak dosyaları kopyalama.
- Dosyaları olduğu gibi kopyalama veya [desteklenen dosya biçimleri ve sıkıştırma codec'leri](supported-file-formats-and-compression-codecs.md)ile dosyaları ayrıştırma.

FTP konektörü pasif modda çalışan FTP sunucusunu destekler. Etkin mod desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>Kullanmaya başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, FTP'ye özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

FTP bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **FtpServer**. | Evet |
| konak | FTP sunucusunun adını veya IP adresini belirtin. | Evet |
| port | FTP sunucusunun dinlediği bağlantı noktasını belirtin.<br/>İzin verilen değerler şunlardır: insa, varsayılan değer **21'dir.** | Hayır |
| sağlarSsl | FTP'yi bir SSL/TLS kanalı üzerinden kullanıp kullanmayacağını belirtin.<br/>İzin verilen değerler şunlardır: **true** (varsayılan), **false**. | Hayır |
| enableServerCertificateValidation | SSL/TLS kanalı üzerinden FTP kullanırken sunucu SSL sertifika doğrulamasını etkinleştirip etkinleştirmeyeceğiniz belirtin.<br/>İzin verilen değerler şunlardır: **true** (varsayılan), **false**. | Hayır |
| authenticationType | Kimlik doğrulama türünü belirtin.<br/>İzin verilen değerler şunlardır: **Temel**, **Anonim** | Evet |
| userName | FTP sunucusuna erişimi olan kullanıcıyı belirtin. | Hayır |
| password | Kullanıcının parolasını belirtin (userName). Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

>[!NOTE]
>FTP konektörü FTP sunucusuna şifreleme veya açık SSL/TLS şifrelemesi olmadan erişmeyi destekler; örtülü SSL/TLS şifrelemeyi desteklemez.

**Örnek 1: Anonim kimlik doğrulaması kullanarak**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: Temel kimlik doğrulamasını kullanarak**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Basic",
            "userName": "<username>",
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

Aşağıdaki özellikler ftp için biçim `location` tabanlı veri kümesindeki ayarlar altında desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesinin `location` altındaki tür özelliği **FtpServerLocation**olarak ayarlanmalıdır. | Evet      |
| folderPath | Klasöre giden yol. Klasöre filtre açmak için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarını belirtin. | Hayır       |
| fileName   | Verilen folderPath altında dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarını belirtin. | Hayır       |

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<FTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "FtpServerLocation",
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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde FTP kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="ftp-as-source"></a>Kaynak olarak FTP

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler ftp için biçim `storeSettings` tabanlı kopyalama kaynağındaki ayarlar altında desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Altındaki `storeSettings` tür özelliği **FtpReadSettings**olarak ayarlanmalıdır. | Evet                                           |
| Özyinelemeli                | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme gerçek olarak ayarlandığında ve lavabo dosya tabanlı bir depo olduğunda, boş bir klasörün veya alt klasörün kopyalanmadığını veya lavaboda oluşturulmadığını unutmayın. İzin verilen değerler **doğru** (varsayılan) ve **yanlıştır.** | Hayır                                            |
| joker KarakterFolderPath       | Kaynak klasörleri filtrelemek için joker karakteriçeren klasör yolu. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. <br>Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Hayır                                            |
| jokerKarakterFileName         | Kaynak dosyaları filtrelemek için verilen folderPath/jokerKarakterIn altında joker karakter içeren dosya adı. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın.  Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Veri `fileName` kümesinde belirtilmemişse evet |
| useBinaryTransfer | İkili aktarım modunu kullanıp kullanmayacağını belirtin. Değerler ikili mod (varsayılan) için ve ASCII için yanlış için doğrudur. |Hayır |
| maxConcurrentConnections | Veri deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromFTP",
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
                    "type": "FtpReadSettings",
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

| folderPath | fileName | Özyinelemeli | Kaynak klasör yapısı ve filtre sonucu **(kalın** renkteki dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (boş, varsayılan kullanın) | yanlış | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*` | (boş, varsayılan kullanın) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*` | `*.csv` | yanlış | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |

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
| folderPath | Klasöre giden yol. Joker karakter filtresi desteklenir, joker `*` karakterlere izin verilir: (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. <br/><br/>Örnekler: rootfolder/subfolder/, Klasör [ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. |Evet |
| fileName | Belirtilen "folderPath" altındaki dosya(lar) için **ad veya joker karakter filtresi.** Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları işaret edir. <br/><br/>Filtre için izin verilen `*` joker karakterler şunlardır: (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>- Örnek 1:`"fileName": "*.csv"`<br/>- Örnek 2:`"fileName": "???20180427.txt"`<br/>Gerçek `^` dosya adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. |Hayır |
| biçim | Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın.<br/><br/>Dosyaları belirli bir biçimde ayrıştmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkquetFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](supported-file-formats-and-compression-codecs-legacy.md#text-format) [Json Formatı,](supported-file-formats-and-compression-codecs-legacy.md#json-format) [Avro Biçimi,](supported-file-formats-and-compression-codecs-legacy.md#avro-format) [Ork Biçimi](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parke Biçimi](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) bölümlerine bakın. |Hayır (yalnızca ikili kopyalama senaryosu için) |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve sıkıştırma codec'lerine](supported-file-formats-and-compression-codecs-legacy.md#compression-support)bakın.<br/>Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**.<br/>Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** |Hayır |
| useBinaryTransfer | İkili aktarım modunu kullanıp kullanmayacağını belirtin. Değerler ikili mod (varsayılan) için ve ASCII için yanlış için doğrudur. |Hayır |

>[!TIP]
>Tüm dosyaları bir klasörün altında kopyalamak için **yalnızca folderPath'i** belirtin.<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için **folderPath** klasörünü klasör bölümü ve dosya adı ile **dosya adı** ile belirtin.<br>Bir klasörün altındaki dosyaların alt kümesini kopyalamak için **folderPath** klasörünü klasör bölümüyle ve joker karakter filtresiile **fileName'yi** belirtin.

>[!NOTE]
>Dosya filtresi için "fileFilter" özelliğini kullanıyorsanız, ileriye dönük olarak "fileName"ye eklenen yeni filtre özelliğini kullanmanız önerilirken, bu özellik olduğu gibi desteklenir.

**Örnek:**

```json
{
    "name": "FTPDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<FTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "myfile.csv.gz",
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
        "name": "CopyFromFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<FTP input dataset name>",
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
