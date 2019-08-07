---
title: Azure Data Factory meta verileri Al etkinliği | Microsoft Docs
description: GetMetadata etkinliğini bir Data Factory işlem hattında nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: ''
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: jingwang
ms.openlocfilehash: b819a990b9f607aaf70bf2e16a5857de3f7306cc
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68827501"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Azure Data Factory meta veri Al etkinliği

GetMetadata etkinliği, Azure Data Factory tüm verilerin **meta verilerini** almak için kullanılabilir. Bu etkinlik aşağıdaki senaryolarda kullanılabilir:

- Tüm verilerin meta veri bilgilerini doğrulama
- Veri hazır/kullanılabilir olduğunda bir işlem hattı tetikleyin

Aşağıdaki işlev Denetim akışında mevcuttur:

- GetMetadata etkinliğinin çıktısı, doğrulama gerçekleştirmek için koşullu ifadelerde kullanılabilir.
- İşlem-Until döngü aracılığıyla koşul karşılandığında bir işlem hattı tetiklenebilir

## <a name="supported-capabilities"></a>Desteklenen özellikler

GetMetadata etkinliği, bir veri kümesini gerekli giriş olarak alır ve etkinlik çıkışı olarak kullanılabilir meta veri bilgileri verir. Şu anda, karşılık gelen alınabilir meta veriler ile aşağıdaki bağlayıcılar desteklenir ve desteklenen en büyük meta veri boyutu **1 MB**'a kadar olur.

>[!NOTE]
>Şirket içinde barındırılan bir Integration Runtime GetMetadata etkinliğini çalıştırırsanız en son yetenek sürüm 3,6 veya üzeri sürümlerde desteklenir. 

### <a name="supported-connectors"></a>Desteklenen bağlayıcılar

**Dosya depolama alanı:**

| Bağlayıcı/meta veriler | ItemName<br>(dosya/klasör) | ItemType<br>(dosya/klasör) | size<br>dosyasýný | oluşturuldu<br>(dosya/klasör) | lastModified<br>(dosya/klasör) |childItems<br>klasörde |contentMD5<br>dosyasýný | structure<br/>dosyasýný | columnCount<br>dosyasýný | bulunur<br>(dosya/klasör) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| [Amazon S3](connector-amazon-simple-storage-service.md) | √/√ | √/√ | √ | x/x | √/√ * | √ | x | √ | √ | √/√ * |
| [Google bulut depolaması](connector-google-cloud-storage.md) | √/√ | √/√ | √ | x/x | √/√ * | √ | x | √ | √ | √/√ * |
| [Azure blobu](connector-azure-blob-storage.md) | √/√ | √/√ | √ | x/x | √/√ * | √ | √ | √ | √ | √/√ |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure Dosya Depolama](connector-azure-file-storage.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [Dosya Sistemi](connector-file-system.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [SFTP](connector-sftp.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [FTP](connector-ftp.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |

- Amazon S3 ve Google Cloud Storage `lastModified` için, demet ve anahtar için geçerlidir; ancak sanal klasörü için geçerlidir;; `exists` ve demet ve anahtar için geçerlidir ancak önek veya sanal klasör değildir.
- Azure Blob `lastModified` için, kapsayıcı ve BLOB için geçerlidir ancak sanal klasör değildir.

**İlişkisel veritabanı:**

| Bağlayıcı/meta veriler | structure | columnCount | bulunur |
|:--- |:--- |:--- |:--- |
| [Azure SQL Veritabanı](connector-azure-sql-database.md) | √ | √ | √ |
| [Azure SQL veritabanı yönetilen örneği](connector-azure-sql-database-managed-instance.md) | √ | √ | √ |
| [Azure SQL Veri Ambarı](connector-azure-sql-data-warehouse.md) | √ | √ | √ |
| [SQL Server](connector-sql-server.md) | √ | √ | √ |

### <a name="metadata-options"></a>Meta veri seçenekleri

Aşağıdaki meta veri türleri, alınacak GetMetadata etkinlik alanı listesinde belirtilebilir:

| Meta veri türü | Açıklama |
|:--- |:--- |
| ItemName | Dosya veya klasörün adı. |
| ItemType | Dosya veya klasörün türü. Çıkış değeri `File` veya `Folder`. |
| size | Dosyanın bayt cinsinden boyutu. Yalnızca dosya için geçerlidir. |
| oluşturuldu | Dosya veya klasörün DateTime değeri oluşturuldu. |
| lastModified | Dosya veya klasörün son değiştirilme tarihi. |
| childItems | Verilen klasör içindeki alt klasörlerin ve dosyaların listesi. Yalnızca klasör için geçerlidir. Çıkış değeri her bir alt öğenin ad ve türünün bir listesidir. |
| contentMD5 | Dosyanın MD5. Yalnızca dosya için geçerlidir. |
| structure | Dosya veya ilişkisel veritabanı tablosu içindeki veri yapısı. Çıkış değeri, sütun adı ve sütun türünün bir listesidir. |
| columnCount | Dosya veya ilişkisel tablo içindeki sütun sayısı. |
| bulunur| Bir dosya/klasör/tablonun var olup olmadığı. "GetaMetadata alan listesinde" Exists "belirtilirse, öğe (dosya/klasör/tablo) mevcut olmadığında bile etkinlik başarısız olmaz; Bunun yerine çıktıyı geri `exists: false` döndürür. |

>[!TIP]
>Bir dosya/klasör/tablo varsa doğrulamak istediğinizde, GetMetadata etkinlik alanı listesinde belirtin `exists` , ardından etkinlik çıktısından `exists: true/false` elde edilen sonucu kontrol edebilirsiniz. `exists` Alan listesinde yapılandırılmamışsa, nesne bulunamadığında GetMetadata etkinliği başarısız olur.

>[!NOTE]
>Dosya mağazalarından meta verileri aldığınızda ve/veya `modifiedDatetimeStart` yapılandırıp/veya `modifiedDatetimeEnd`yapılandırdığınızda, `childItems` içindeki çıktı yalnızca, Aralık arasındaki son değiştirme zamanına sahip, ancak alt klasörler olmadan verilen yolun altındaki dosyaları döndürür.

## <a name="syntax"></a>Sözdizimi

**GetMetadata etkinliği:**

```json
{
    "name": "MyActivity",
    "type": "GetMetadata",
    "typeProperties": {
        "fieldList" : ["size", "lastModified", "structure"],
        "dataset": {
            "referenceName": "MyDataset",
            "type": "DatasetReference"
        }
    }
}
```

**Veri kümesi**

```json
{
    "name": "MyDataset",
    "properties": {
    "type": "AzureBlob",
        "linkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath":"container/folder",
            "filename": "file.json",
            "format":{
                "type":"JsonFormat"
            }
        }
    }
}
```

## <a name="type-properties"></a>Tür özellikleri

Şu anda GetMetadata etkinliği aşağıdaki tür meta veri bilgilerini alabilir.

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
fieldList | Gerekli meta veri bilgisi türlerini listeler. Desteklenen meta verilerde [meta veri seçenekleri](#metadata-options) bölümünde ayrıntılara bakın. | Evet 
veri kümesi | Meta veri etkinliği GetMetadata etkinliği tarafından alınacak olan başvuru veri kümesi. Desteklenen bağlayıcılar bölümünde [desteklenen yetenekler](#supported-capabilities) bölümüne bakın ve veri kümesi sözdizimi ayrıntılarında bağlayıcı konusuna başvurun. | Evet
formatSettings | Biçim türü veri kümesi (Parquet, DelimitedText) kullanılırken geçerlidir. | Hayır
storeSettings | Biçim türü veri kümesi (Parquet, DelimitedText) kullanılırken geçerlidir. | Hayır

## <a name="sample-output"></a>Örnek çıktı

GetMetadata sonucu etkinlik çıkışında gösterilir. Aşağıda, alan listesinde başvuru olarak seçilen ayrıntılı meta veri seçenekleriyle iki örnek verilmiştir. Sonraki etkinlikteki sonucu kullanmak için, öğesinin `@{activity('MyGetMetadataActivity').output.itemName}`stilini kullanın.

### <a name="get-a-files-metadata"></a>Bir dosyanın meta verilerini al

```json
{
  "exists": true,
  "itemName": "test.csv",
  "itemType": "File",
  "size": 104857600,
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "contentMD5": "cMauY+Kz5zDm3eWa9VpoyQ==",
  "structure": [
    {
        "name": "id",
        "type": "Int64"
    },
    {
        "name": "name",
        "type": "String"
    }
  ],
  "columnCount": 2
}
```

### <a name="get-a-folders-metadata"></a>Klasörün meta verilerini alın

```json
{
  "exists": true,
  "itemName": "testFolder",
  "itemType": "Folder",
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "childItems": [
    {
      "name": "test.avro",
      "type": "File"
    },
    {
      "name": "folder hello",
      "type": "Folder"
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinliklerini görün: 

- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
