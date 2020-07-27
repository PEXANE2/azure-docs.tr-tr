---
title: Azure Data Factory meta veri Al etkinliği
description: Data Factory ardışık düzeninde meta veri al etkinliğinin nasıl kullanılacağını öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: ''
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/24/2020
ms.author: jingwang
ms.openlocfilehash: a5d203664520aebadefd16c19813d7957dd37fc4
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87171243"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Azure Data Factory meta veri Al etkinliği
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Factory ' deki herhangi bir verinin meta verilerini almak için meta veri al etkinliğini kullanabilirsiniz. Bu etkinliği aşağıdaki senaryolarda kullanabilirsiniz:

- Verilerin meta verilerini doğrulayın.
- Veriler hazır/kullanılabilir olduğunda bir işlem hattı tetikleyin.

Aşağıdaki işlev Denetim akışında mevcuttur:

- Doğrulama gerçekleştirmek için koşullu ifadelerde meta verileri al etkinliğinden çıktıyı kullanabilirsiniz.
- Döngüye göre do aracılığıyla bir koşul karşılandığında bir işlem hattı tetikleyebilirsiniz.

## <a name="capabilities"></a>Özellikler

Meta veri Al etkinliği bir veri kümesini girdi olarak alır ve meta veri bilgilerini çıkış olarak döndürür. Şu anda, aşağıdaki bağlayıcılar ve karşılık gelen alınabilir meta veriler desteklenir. Döndürülen meta verilerin en büyük boyutu 2 MB 'tır.

>[!NOTE]
>Şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde meta veri al etkinliğini çalıştırırsanız, sürüm 3,6 veya sonraki sürümlerde en son yetenekler desteklenir.

### <a name="supported-connectors"></a>Desteklenen bağlayıcılar

**Dosya depolama**

| Bağlayıcı/meta veriler | ItemName<br>(dosya/klasör) | ItemType<br>(dosya/klasör) | size<br>dosyasýný | yaratıl<br>(dosya/klasör) | lastModified<br>(dosya/klasör) |childItems<br>klasörde |contentMD5<br>dosyasýný | yapı<br/>dosyasýný | columnCount<br>dosyasýný | bulunur<br>(dosya/klasör) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| [Amazon S3](connector-amazon-simple-storage-service.md) | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| [Google Cloud Storage](connector-google-cloud-storage.md) | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| [Azure Blob depolama](connector-azure-blob-storage.md) | √/√ | √/√ | √ | x/x | √/√* | √ | √ | √ | √ | √/√ |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure Data Lake Storage 2. Nesil](connector-azure-data-lake-storage.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure Dosyaları](connector-azure-file-storage.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [Dosya sistemi](connector-file-system.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [SFTP](connector-sftp.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [FTP](connector-ftp.md) | √/√ | √/√ | √ | x/x | x/x | √ | x | √ | √ | √/√ |

- Bir klasöre karşı meta veri al etkinliğini kullanırken, verilen klasöre yönelik LıST/EXECUTE izninizin olduğundan emin olun.
- Amazon S3 ve Google Cloud Storage için `lastModified` demet ve anahtar için geçerlidir ancak sanal klasöre uygulanmaz; `exists` demet ve anahtar için geçerlidir ancak önek veya sanal klasöre uygulanmaz.
- Azure Blob depolama için `lastModified` kapsayıcı ve BLOB için geçerlidir ancak sanal klasöre uygulanmaz.
- `lastModified`Filtre Şu anda alt öğeleri filtrelemek için geçerlidir ancak belirtilen klasör/dosyanın kendisi için geçerli değildir.
- Klasörler/dosyalar üzerinde joker karakter filtresi, meta veri Al etkinliği için desteklenmiyor.
- `structure``columnCount`ikili, JSON veya XML dosyalarından meta veriler alınırken desteklenmez.

**İlişkisel veritabanı**

| Bağlayıcı/meta veriler | yapı | columnCount | bulunur |
|:--- |:--- |:--- |:--- |
| [Azure SQL Veritabanı](connector-azure-sql-database.md) | √ | √ | √ |
| [Azure SQL Yönetilen Örnek](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md) | √ | √ | √ |
| [Azure SQL Veri Ambarı](connector-azure-sql-data-warehouse.md) | √ | √ | √ |
| [SQL Server](connector-sql-server.md) | √ | √ | √ |

### <a name="metadata-options"></a>Meta veri seçenekleri

İlgili bilgileri almak için meta verileri al etkinlik alan listesinde aşağıdaki meta veri türlerini belirtebilirsiniz:

| Meta veri türü | Açıklama |
|:--- |:--- |
| ItemName | Dosya veya klasörün adı. |
| ItemType | Dosya veya klasörün türü. Döndürülen değer `File` veya `Folder` . |
| size | Dosyanın bayt cinsinden boyutu. Yalnızca dosyalar için geçerlidir. |
| yaratıl | Dosya veya klasörün DateTime değeri oluşturuldu. |
| lastModified | Dosya veya klasörün son değiştirilme tarihi. |
| childItems | Belirtilen klasördeki alt klasörlerin ve dosyaların listesi. Yalnızca klasörler için geçerlidir. Döndürülen değer her bir alt öğenin adının ve türünün bir listesidir. |
| contentMD5 | Dosyanın MD5. Yalnızca dosyalar için geçerlidir. |
| yapı | Dosya veya ilişkisel veritabanı tablosunun veri yapısı. Döndürülen değer, sütun adlarının ve sütun türlerinin bir listesidir. |
| columnCount | Dosya veya ilişkisel tablodaki sütun sayısı. |
| bulunur| Bir dosya, klasör veya tablo bulunup yok. `exists`Meta verileri al alan listesinde belirtilmişse, dosya, klasör veya tablo mevcut olmasa bile etkinliğin başarısız olacağını unutmayın. Bunun yerine `exists: false` çıktıda döndürülür. |

>[!TIP]
>Bir dosya, klasör veya tablonun var olduğunu doğrulamak istediğinizde `exists` meta verileri al etkinlik alanı listesini belirtin. Ardından, `exists: true/false` etkinlik çıkışında sonucu kontrol edebilirsiniz. `exists`Alan listesinde belirtilmemişse, nesne bulunamazsa meta verileri Al etkinliği başarısız olur.

>[!NOTE]
>Dosya mağazalarından meta veriler alırken ve `modifiedDatetimeStart` veya `modifiedDatetimeEnd` ' ı yapılandırdığınızda, `childItems` yalnızca belirtilen aralıktaki son değiştirme zamanına sahip olan verilen yoldaki dosyaları dahil eder. ' De alt klasörlerdeki öğeler dahil değildir.

## <a name="syntax"></a>Sözdizimi

**Meta veri Al etkinliği**

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

Şu anda meta verileri Al etkinliği şu tür meta veri bilgilerini döndürebilir:

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
fieldList | Gerekli meta veri bilgileri türleri. Desteklenen meta veriler hakkında daha fazla bilgi için bu makalenin [meta veri seçenekleri](#metadata-options) bölümüne bakın. | Yes 
veri kümesi | Meta verileri Al etkinliği tarafından alınacak olan başvuru veri kümesi. Desteklenen bağlayıcılar hakkında bilgi için bkz. [yetenekler](#capabilities) bölümü. Veri kümesi sözdizimi ayrıntıları için ilgili bağlayıcı konularına bakın. | Yes
formatSettings | Biçim türü veri kümesi kullanırken uygulayın. | Hayır
storeSettings | Biçim türü veri kümesi kullanırken uygulayın. | Hayır

## <a name="sample-output"></a>Örnek çıktı

Veri Al sonuçları, etkinlik çıkışında gösterilir. Aşağıda, kapsamlı meta veri seçeneklerini gösteren iki örnek verilmiştir. Sonuçları sonraki bir etkinlikte kullanmak için şu stili kullanın: `@{activity('MyGetMetadataActivity').output.itemName}` .

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
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri hakkında bilgi edinin:

- [İşlem hattı yürütme etkinliği](control-flow-execute-pipeline-activity.md)
- [ForEach etkinliği](control-flow-for-each-activity.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
