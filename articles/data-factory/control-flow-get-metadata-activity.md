---
title: Azure Veri Fabrikası'nda Meta veri etkinliği alın
description: Veri Fabrikası ardışık etki alanında Meta veri al etkinliğini nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: ''
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: jingwang
ms.openlocfilehash: 344ad8e106c119c1de59570d1ec4e3df5e1cc8af
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417118"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Meta veri etkinliği alın
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Veri Fabrikası'ndaki herhangi bir verinin meta verilerini almak için Meta Veri Al etkinliğini kullanabilirsiniz. Bu etkinliği aşağıdaki senaryolarda kullanabilirsiniz:

- Herhangi bir verinin meta verilerini doğrulayın.
- Veriler hazır/kullanılabilir olduğunda bir ardışık hattı tetikle.

Aşağıdaki işlevsellik denetim akışında kullanılabilir:

- Doğrulama gerçekleştirmek için Meta veri al etkinliğinden çıktıyı koşullu ifadelerde kullanabilirsiniz.
- Bir koşul Do Until döngüsü aracılığıyla karşılandığında bir ardışık hattı tetikleyebilirsiniz.

## <a name="capabilities"></a>Özellikler

Meta veri al etkinliği bir veri kümesini giriş olarak alır ve meta veri bilgilerini çıktı olarak döndürür. Şu anda, aşağıdaki bağlayıcılar ve ilgili alınabilen meta veriler desteklenir. Döndürülen meta verilerin maksimum boyutu 2 MB'dır.

>[!NOTE]
>Meta verilerini kendi kendine barındırılan bir tümleştirme çalışma zamanında çalıştırırsanız, en son özellikler sürüm 3.6 veya sonraki sürümde desteklenir.

### <a name="supported-connectors"></a>Desteklenen konektörler

**Dosya depolama**

| Bağlayıcı/Meta veri | ıtemname<br>(dosya/klasör) | ıtemtype<br>(dosya/klasör) | size<br>(dosya) | Oluşturulan<br>(dosya/klasör) | Lastmodified<br>(dosya/klasör) |Childıtems<br>(klasör) |içerikMD5<br>(dosya) | yapı<br/>(dosya) | Columncount<br>(dosya) | Var<br>(dosya/klasör) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| [Amazon S3](connector-amazon-simple-storage-service.md) | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| [Google Cloud Storage](connector-google-cloud-storage.md) | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| [Azure Blob depolama](connector-azure-blob-storage.md) | √/√ | √/√ | √ | x/x | √/√* | √ | √ | √ | √ | √/√ |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure Dosyalar](connector-azure-file-storage.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [Dosya sistemi](connector-file-system.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [SFTP](connector-sftp.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [FTP](connector-ftp.md) | √/√ | √/√ | √ | x/x | x/x | √ | x | √ | √ | √/√ |

- Meta veri etkinliğini bir klasöre karşı alın'ı kullanırken, verilen klasöre LIST/EXECUTE izni aldığınızdan emin olun.
- Amazon S3 ve Google `lastModified` Cloud Storage için, kova ve anahtar için geçerlidir, ancak sanal klasör için geçerlideğildir ve `exists` kova ve anahtar için geçerlidir, ancak önek veya sanal klasör için geçerli değildir.
- Azure Blob depolama `lastModified` için kapsayıcı ve blob için geçerlidir, ancak sanal klasör için geçerli değildir.
- `lastModified`filtre şu anda alt öğeleri filtrelemek için geçerlidir, ancak belirtilen klasör/dosyanın kendisini filtrelemez.
- Klasörlerdeki/dosyalardaki Joker karakter filtresi Meta veri al etkinliği için desteklenmez.

**İlişkisel veritabanı**

| Bağlayıcı/Meta veri | yapı | Columncount | Var |
|:--- |:--- |:--- |:--- |
| [Azure SQL Veritabanı](connector-azure-sql-database.md) | √ | √ | √ |
| [Azure SQL Veritabanı yönetilen örnek](connector-azure-sql-database-managed-instance.md) | √ | √ | √ |
| [Azure SQL Veri Ambarı](connector-azure-sql-data-warehouse.md) | √ | √ | √ |
| [SQL Server](connector-sql-server.md) | √ | √ | √ |

### <a name="metadata-options"></a>Meta veri seçenekleri

İlgili bilgileri almak için Meta veri alanı al listesinde aşağıdaki meta veri türlerini belirtebilirsiniz:

| Meta veri türü | Açıklama |
|:--- |:--- |
| ıtemname | Dosya nın veya klasörün adı. |
| ıtemtype | Dosya veya klasörün türü. Döndürülen `File` değer `Folder`veya . |
| size | Dosyanın boyutu, baytlar halinde. Yalnızca dosyalara uygulanabilir. |
| Oluşturulan | Dosya veya klasörün datetime oluşturuldu. |
| Lastmodified | Dosya nın veya klasörün son değiştirilmiş datetime'ı. |
| Childıtems | Verilen klasördeki alt klasörlerin ve dosyaların listesi. Yalnızca klasörler için geçerlidir. Döndürülen değer, her alt öğenin adı ve türünün listesidir. |
| içerikMD5 | Dosyanın MD5'i. Yalnızca dosyalara uygulanabilir. |
| yapı | Dosyanın veya ilişkisel veritabanı tablosunun veri yapısı. Döndürülen değer, sütun adları ve sütun türlerinin listesidir. |
| Columncount | Dosyaveya ilişkitablosundaki sütun sayısı. |
| Var| Dosya, klasör veya tablo nun var olup olmadığı. Meta veri `exists` alan listesinde belirtilmişse, dosya, klasör veya tablo yoksa bile etkinliğin başarısız olmayacağını unutmayın. Bunun `exists: false` yerine, çıktı döndürülür. |

>[!TIP]
>Bir dosyanın, klasörün veya tablonun var `exists` olduğunu doğrulamak istediğinizde, Meta verileri al etkinlik alanı listesinde belirtin. Daha sonra etkinlik `exists: true/false` çıktısında sonucu kontrol edebilirsiniz. Alan `exists` listesinde belirtilmemişse, nesne bulunmazsa Meta Veri Al etkinliği başarısız olur.

>[!NOTE]
>Dosya depolarından meta veri aldığınızda `modifiedDatetimeStart` `modifiedDatetimeEnd`ve `childItems` yapılandırıldığınızda veya , in çıktısı yalnızca belirtilen aralıkta son değiştirilen süreye sahip olan yoldaki dosyaları içerir. In alt klasörlerde öğeleri içermez.

## <a name="syntax"></a>Sözdizimi

**Meta veri etkinliği alın**

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

Şu anda, Meta veri al etkinliği aşağıdaki meta veri bilgi türlerini döndürebilir:

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
alanListesi | Gerekli meta veri bilgi türleri. Desteklenen meta verilerle ilgili ayrıntılar için bu makalenin [Meta veri seçenekleri](#metadata-options) bölümüne bakın. | Evet 
Dataset | Meta verileri Get Metadata etkinliği tarafından alınacak olan başvuru veri kümesi. Desteklenen bağlayıcılar hakkında bilgi için [Yetenekler](#capabilities) bölümüne bakın. Veri kümesi sözdizimi ayrıntıları için belirli bağlayıcı konulara bakın. | Evet
formatAyarlar | Biçim türü veri kümesini kullanırken uygulayın. | Hayır
mağazaAyarları | Biçim türü veri kümesini kullanırken uygulayın. | Hayır

## <a name="sample-output"></a>Örnek çıktı

Meta veri al sonuçları etkinlik çıktısında gösterilir. Aşağıda kapsamlı meta veri seçenekleri gösteren iki örnek verilmiştir. Sonuçları sonraki bir etkinlikte kullanmak için şu `@{activity('MyGetMetadataActivity').output.itemName}`deseni kullanın: .

### <a name="get-a-files-metadata"></a>Dosyanın meta verilerini alma

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

### <a name="get-a-folders-metadata"></a>Bir klasörün meta verilerini alma

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
Veri Fabrikası tarafından desteklenen diğer kontrol akışı etkinlikleri hakkında bilgi edinin:

- [Boru Hattı etkinliğini yürütme](control-flow-execute-pipeline-activity.md)
- [ForEach etkinliği](control-flow-for-each-activity.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
