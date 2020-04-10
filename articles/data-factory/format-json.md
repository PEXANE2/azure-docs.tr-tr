---
title: Azure Veri Fabrikası'nda JSON biçimi
description: Bu konu, Azure Veri Fabrikası'nda JSON biçimiyle nasıl başa çıkılabildiğini açıklar.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/05/2020
ms.author: jingwang
ms.openlocfilehash: b63dcfd6ed293cab9d0107a8b6a35c7ce358d429
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81011693"
---
# <a name="json-format-in-azure-data-factory"></a>Azure Veri Fabrikası'nda JSON biçimi

**JSON dosyalarını ayrışdırmak veya verileri JSON biçimine yazmak**istediğinizde bu makaleyi izleyin. 

JSON biçimi aşağıdaki bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md), [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md), [Azure Dosya Depolama](connector-azure-file-storage.md), Dosya [Sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Bulut Depolama](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md), ve [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde JSON veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin tür özelliği **Json**olarak ayarlanmalıdır. | Evet      |
| location         | Dosya(lar) konum ayarları. Her dosya tabanlı bağlayıcının kendi konum türü ve `location`desteklenen özellikleri vardır. **Bağlayıcı makalesindeki ayrıntılara bakın -> Dataset özellikleri bölümünde.** | Evet      |
| encodingName     | Test dosyalarını okumak/yazmak için kullanılan kodlama türü. <br>Allowed values are as follows: "UTF-8", "UTF-16", "UTF-16BE", "UTF-32", "UTF-32BE", "US-ASCII", "UTF-7", "BIG5", "EUC-JP", "EUC-KR", "GB2312", "GB18030", "JOHAB", "SHIFT-JIS", "CP875", "CP866", "IBM00858", "IBM037", "IBM273", "IBM437", "IBM500", "IBM737", "IBM775", "IBM850", "IBM852", "IBM855", "IBM857", "IBM860", "IBM861", "IBM863", "IBM864", "IBM865", "IBM869", "IBM870", "IBM01140", "IBM01141", "IBM01142", "IBM01143", "IBM01144", "IBM01145", "IBM01146", "IBM01147", "IBM01148", "IBM01149", "ISO-2022-JP", "ISO-2022-KR", "ISO-8859-1", "ISO-8859-2", "ISO-8859-3", "ISO-8859-4", "ISO-8859-5", "ISO-8859-6", "ISO-8859-7", "ISO-8859-8", "ISO-8859-9", "ISO-8859-13" " ISO-8859-15", "WINDOWS-874", "WINDOWS-1250", "WINDOWS-1251", "WINDOWS-1252", "WINDOWS-1253", "WINDOWS-1254", "WINDOWS-1255", "WINDOWS-1257", "WINDOWS-1258".| Hayır       |
| sıkıştırma | Dosya sıkıştırma yapılandırmak için özellikleri grubu. Etkinlik yürütme sırasında sıkıştırma/dekompresyon yapmak istediğinizde bu bölümü yapılandırın. | Hayır |
| type | JSON dosyalarını okumak/yazmak için kullanılan sıkıştırma codec'i. <br>İzin verilen değerler **bzip2,** **gzip**, **deflate**, **ZipDeflate**, **snappy**, veya **lz4**. dosyayı kaydederken kullanmak için. Varsayılan sıkıştırılmış değil.<br>**Not** şu anda Kopyalama etkinliği "snappy" & "lz4" desteklemez ve veri akışını eşleme "ZipDeflate" desteklemez.<br>ZipDeflate dosya(lar) decompress ve dosya tabanlı lavabo veri deposuna yazmak için kopyalama etkinliği kullanırken **not,** dosyalar klasöre ayıklanır: `<path specified in dataset>/<folder named as source zip file>/`. | Hayır.  |
| düzey | Sıkıştırma oranı. <br>İzin verilen değerler **En Uygun** veya **En Hızlı**değerlerdir.<br>- **En hızlı:** Elde edilen dosya en iyi şekilde sıkıştırılmamış olsa bile sıkıştırma işlemi mümkün olan en kısa sürede tamamlanmalıdır.<br>- **Optimal**: Sıkıştırma işlemi, operasyonun tamamlanması daha uzun sürse bile en iyi şekilde sıkıştırılmalıdır. Daha fazla bilgi için [Sıkıştırma Düzeyi](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) konusuna bakın. | Hayır       |

Aşağıda Azure Blob Depolama JSON veri kümesi bir örnektir:

```json
{
    "name": "JSONDataset",
    "properties": {
        "type": "Json",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compression": {
                "type": "gzip"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde JSON kaynağı ve lavabo tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="json-as-source"></a>Kaynak olarak JSON

Aşağıdaki özellikler kopyalama etkinliği *** \*kaynak\* *** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinlik kaynağının türü özelliği **JSONSource**olarak ayarlanmalıdır. | Evet      |
| mağazaAyarları | Veri deposundan verilerin nasıl okunacaklarına ilişkin özellikler grubu. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen okuma ayarları vardır. **Bağlayıcı makaledeki ayrıntılara bakın -> Etkinlik özelliklerini kopyala bölümünden kopyalayın.** | Hayır       |

### <a name="json-as-sink"></a>JSON lavabo olarak

Aşağıdaki özellikler kopyalama etkinliği *** \*lavabo\* *** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinlik kaynağının türü özelliği **JSONSink**olarak ayarlanmalıdır. | Evet      |
| formatAyarlar | Bir grup özellik. Aşağıdaki **JSON yazma ayarları** tablosuna bakın. | Hayır       |
| mağazaAyarları | Veri deposuna nasıl veri yazılailgili bir grup özellik. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen yazma ayarları vardır. **Bağlayıcı makaledeki ayrıntılara bakın -> Etkinlik özelliklerini kopyala bölümünden kopyalayın.** | Hayır       |

Desteklenen `formatSettings` **JSON altında ayarları yazmak:**

| Özellik      | Açıklama                                                  | Gerekli                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| type          | FormatAyarları türü **JsonWriteSettings**olarak ayarlanmalıdır. | Evet                                                   |
| filePattern |Her bir JSON dosyasında depolanan verilerin desenini belirtir. İzin verilen değerler: **setOfObjects** ve **arrayOfObjects**. **Varsayılan** değer **setOfObjects** olarak belirlenmiştir. Bu desenler hakkında ayrıntılı bilgi için bkz. [JSON dosyası desenleri](#json-file-patterns). |Hayır |

### <a name="json-file-patterns"></a>JSON dosyası desenleri

Kopyalama etkinliği, JSON dosyalarının aşağıdaki desenlerini otomatik olarak algılayabilir ve ayrıştırabilir. 

- **1. Tür: setOfObjects**

    Her dosya tek bir nesne veya satırlara ayrılmış/bitiştirilmiş birden fazla nesne içerir. 
    Bu seçenek kopyalama etkinliği lavabosunda seçildiğinde, kopyalama etkinliği satır başına her nesneyle tek bir JSON dosyası üretir (satır sınırlıdır).

    * **tek nesne JSON örneği**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **satırlara ayrılmış JSON örneği**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **bitiştirilmiş JSON örneği**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **2. Tür: arrayOfObjects**

    Her dosya bir nesne dizisi içerir.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

JSON dosya türleri veri akışını eşlemede hem lavabo hem de kaynak olarak kullanılabilir.

### <a name="creating-json-structures-in-a-derived-column"></a>Türetilmiş sütunda JSON yapıları oluşturma

Türemiş sütun ifade oluşturucusu aracılığıyla veri akışınıza karmaşık bir sütun ekleyebilirsiniz. Türetilen sütun dönüşümünde, yeni bir sütun ekleyin ve mavi kutuyu tıklatarak ifade oluşturucuyu açın. Sütun karmaşık hale getirmek için JSON yapısını el ile girebilir veya etkileşimli olarak alt sütun eklemek için UX'yi kullanabilirsiniz.

#### <a name="using-the-expression-builder-ux"></a>İfade oluşturucu UX kullanarak

Çıkış şema yan bölmesinde, bir sütunun üzerine gezin ve artı simgesini tıklatın. Sütunu karmaşık bir tür yapmak için **alt sütun ekle'yi** seçin.

![Alt sütun ekleme](media/data-flow/addsubcolumn.png "Alt Sütun Ekle")

Aynı şekilde ek sütunlar ve alt sütunlar ekleyebilirsiniz. Karmaşık olmayan her alan için, ifade düzenleyicisine sağa bir ifade eklenebilir.

![Karmaşık sütun](media/data-flow/complexcolumn.png "Karmaşık sütun")

#### <a name="entering-the-json-structure-manually"></a>JSON yapısına el ile girme

El ile Bir JSON yapısı eklemek için yeni bir sütun ekleyin ve düzenleyiciye ifadeyi girin. İfade aşağıdaki genel biçimi izler:

```
@(
    field1=0,
    field2=@(
        field1=0
    )
)
```

Bu ifade "complexColumn" adlı bir sütun için girildiyse, lavaboya aşağıdaki JSON olarak yazılır:

```
{
    "complexColumn": {
        "field1": 0,
        "field2": {
            "field1": 0
        }
    }
}
```

#### <a name="sample-manual-script-for-complete-hierarchical-definition"></a>Tam hiyerarşik tanım için örnek el yazısı komut dosyası
```
@(
    title=Title,
    firstName=FirstName,
    middleName=MiddleName,
    lastName=LastName,
    suffix=Suffix,
    contactDetails=@(
        email=EmailAddress,
        phone=Phone
    ),
    address=@(
        line1=AddressLine1,
        line2=AddressLine2,
        city=City,
        state=StateProvince,
        country=CountryRegion,
        postCode=PostalCode
    ),
    ids=[
        toString(CustomerID), toString(AddressID), rowguid
    ]
)
```

### <a name="source-format-options"></a>Kaynak biçimi seçenekleri

Veri akışınızda kaynak olarak JSON veri kümesini kullanmak, beş ek ayar ayarlamanıza olanak tanır. Bu ayarlar **Kaynak Seçenekleri** sekmesinde **JSON ayarları** akordeon altında bulunabilir.  

![JSON Ayarları](media/data-flow/json-settings.png "JSON Ayarları")

#### <a name="default"></a>Varsayılan

Varsayılan olarak, JSON verileri aşağıdaki biçimde okunur.

```
{ "json": "record 1" }
{ "json": "record 2" }
{ "json": "record 3" }
```

#### <a name="single-document"></a>Tek belge

**Tek belge** seçilirse, veri akışlarını eşleme her dosyadan bir JSON belgesi ni okur. 

``` json
File1.json
{
    "json": "record 1"
}
File2.json
{
    "json": "record 2"
}
File3.json
{
    "json": "record 3"
}
```

#### <a name="unquoted-column-names"></a>Tırnak içinde sütun adları

**Tırnak içinde sütun adları** seçilirse, veri akışlarını eşlemede tırnak işaretleriyle çevrili olmayan JSON sütunları yer alıyor. 

```
{ json: "record 1" }
{ json: "record 2" }
{ json: "record 3" }
```

#### <a name="has-comments"></a>Yorumları var

JSON verilerinde C veya C++ stili nde yorum varsa **Var'ın yorumunu** seçin.

``` json
{ "json": /** comment **/ "record 1" }
{ "json": "record 2" }
{ /** comment **/ "json": "record 3" }
```

#### <a name="single-quoted"></a>Tek alıntı

JSON alanları ve değerleri çift tırnak yerine tek tırnak işaretleri kullanıyorsa, **tek teklif** seçin.

```
{ 'json': 'record 1' }
{ 'json': 'record 2' }
{ 'json': 'record 3' }
```

#### <a name="backslash-escaped"></a>Backslash kaçtı

JSON verilerindeki karakterlerden kaçmak için ters eğik çizgiler kullanılıyorsa **Tek teklif'i** seçin.

```
{ "json": "record 1" }
{ "json": "\} \" \' \\ \n \\n record 2" }
{ "json": "record 3" }
```

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
