---
title: Azure Veri Fabrikası'nda desteklenen dosya biçimleri (eski)
description: Bu konu, Azure Veri Fabrikası'ndaki dosya tabanlı bağlayıcılar tarafından desteklenen dosya biçimlerini ve sıkıştırma kodlarını açıklar.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: jingwang
ms.openlocfilehash: 423706c391e8d8c2c609798d9f50e5a22f5c39bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260689"
---
# <a name="supported-file-formats-and-compression-codecs-in-azure-data-factory-legacy"></a>Azure Veri Fabrikası'nda desteklenen dosya biçimleri ve sıkıştırma kodlayıcıları (eski)

*Bu makale aşağıdaki bağlayıcılar için geçerlidir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md), [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md), [Azure Dosya Depolama](connector-azure-file-storage.md), Dosya [Sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Bulut Depolama](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md), ve [SFTP](connector-sftp.md).*

>[!IMPORTANT]
>Veri Fabrikası yeni biçim tabanlı veri seti modeli tanıttı, ayrıntıları ile ilgili biçim makalebakın: <br>- [Avro formatı](format-avro.md)<br>- [İkili biçim](format-binary.md)<br>- [Sınırlı metin biçimi](format-delimited-text.md)<br>- [JSON biçimi](format-json.md)<br>- [ORC biçimi](format-orc.md)<br>- [Parke formatı](format-parquet.md)<br>Bu makalede belirtilen geri kalan yapılandırmalar hala geriye dönük compabitility için olduğu gibi desteklenir. İleriye dönük yeni modeli kullanmanız önerilir. 

## <a name="text-format-legacy"></a><a name="text-format"></a>Metin biçimi (eski)

>[!NOTE]
>[Delimited metin biçimi](format-delimited-text.md) makalesinden yeni modeli öğrenin. Dosya tabanlı veri deposu veri kümesindeki aşağıdaki yapılandırmalar, geriye dönük uyumluluk için olduğu gibi desteklenmeye devam eder. İleriye dönük yeni modeli kullanmanız önerilir.

Bir metin dosyasından okumak veya bir metin dosyasına `type` yazmak istiyorsanız, veri kümesinin bölümündeki `format` özelliği **TextFormat**olarak ayarlayın. İsterseniz `format` bölümünde aşağıdaki **isteğe bağlı** özellikleri de belirtebilirsiniz. Yapılandırma adımları için [TextFormat örneği](#textformat-example) bölümünü inceleyin.

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| columnDelimiter |Bir dosyadaki sütunları ayırmak için kullanılan karakterdir. Verilerinizde bulunmayan nadir bir yazdırılamayan karakter kullanmayı düşünebilirsiniz. Örneğin, Başlığın Başlangıcı 'nı (SOH) temsil eden "\u0001" belirtin. |Yalnızca bir karaktere izin verilir. **Varsayılan** değer **virgül (",")** olarak belirlenmiştir. <br/><br/>Unicode karakterini kullanmak için, karşılık gelen kodu almak için [Unicode Karakterleri'ne](https://en.wikipedia.org/wiki/List_of_Unicode_characters) başvurun. |Hayır |
| rowDelimiter |Bir dosyadaki satırları ayırmak için kullanılan karakterdir. |Yalnızca bir karaktere izin verilir. **Varsayılan** değer, okuma sırasında **["\r\n", "\r", "\n"]** değerlerinden biri, yazma sırasında ise **"\r\n"** olarak belirlenmiştir. |Hayır |
| escapeChar |Giriş dosyasının içeriğindeki bir sütun ayırıcısına kaçış karakteri eklemek için kullanılan özel karakterdir. <br/><br/>Bir tablo için hem escapeChar hem de quoteChar parametrelerini aynı anda belirtemezsiniz. |Yalnızca bir karaktere izin verilir. Varsayılan değer yoktur. <br/><br/>Örnek: Sütun sınırlayıcınız virgül (",") karakteriyse ancak metin içinde virgül karakteri kullanılıyorsa (örneğin: "Merhaba, dünya"), "$" karakterini kaçış karakteri olarak tanımlayabilir ve kaynakta "Merhaba$, dünya" dizesini kullanabilirsiniz. |Hayır |
| quoteChar |Bir dize değerini tırnak içine almak için kullanılan karakterdir. Tırnak işareti içindeki sütun ve satır sınırlayıcıları, dize değerinin bir parçası olarak kabul edilir. Bu özellik hem giriş hem de çıkış veri kümelerine uygulanabilir.<br/><br/>Bir tablo için hem escapeChar hem de quoteChar parametrelerini aynı anda belirtemezsiniz. |Yalnızca bir karaktere izin verilir. Varsayılan değer yoktur. <br/><br/>Örneğin, sütun sınırlayıcınız virgül (",") karakteriyse ancak metin içinde virgül karakteri kullanılıyorsa (örneğin: <Merhaba, dünya>), " (çift tırnak) karakterini tırnak karakteri olarak tanımlayabilir ve kaynakta "Merhaba, dünya" dizesini kullanabilirsiniz. |Hayır |
| nullValue |Bir null değeri temsil etmek için kullanılan bir veya daha fazla karakterdir. |Bir veya daha fazla karakter olabilir. **Varsayılan** değerler okuma sırasında **"\N" ve "NULL"**, yazma sırasında ise **"\N"** olarak belirlenmiştir. |Hayır |
| encodingName |Kodlama adını belirtir. |Geçerli bir kodlama adı. Bkz. [Encoding.EncodingName Özelliği](https://msdn.microsoft.com/library/system.text.encoding.aspx). Örnek: windows-1250 veya shift_jis. **Varsayılan** değer **UTF-8** olarak belirlenmiştir. |Hayır |
| firstRowAsHeader |İlk satırın üst bilgi olarak kabul edilip edilmeyeceğini belirtir. Giriş veri kümesinde Data Factory ilk satırı üst bilgi olarak okur. Çıkış veri kümesinde Data Factory ilk satırı üst bilgi olarak yazar. <br/><br/>Örnek senaryolar için bkz. [`firstRowAsHeader` ve `skipLineCount` kullanım senaryoları](#scenarios-for-using-firstrowasheader-and-skiplinecount). |True<br/><b>False (varsayılan)</b> |Hayır |
| skipLineCount |Giriş dosyalarından verileri okurken atlayacak **boş olmayan** satır sayısını gösterir. Hem skipLineCount hem de firstRowAsHeader parametresi belirtilirse önce satırlar atlanır, ardından giriş dosyasındaki üst bilgi bilgileri okunur. <br/><br/>Örnek senaryolar için bkz. [`firstRowAsHeader` ve `skipLineCount` kullanım senaryoları](#scenarios-for-using-firstrowasheader-and-skiplinecount). |Tamsayı |Hayır |
| treatEmptyAsNull |Bir giriş dosyasından veri okuma sırasında null veya boş dizenin null değer olarak kabul edilip edilmeyeceğini belirtir. |**True (varsayılan)**<br/>False |Hayır |

### <a name="textformat-example"></a>TextFormat örneği

Bir veri kümesi için aşağıdaki JSON tanımında, bazı isteğe bağlı özellikler belirtilir.

```json
"typeProperties":
{
    "folderPath": "mycontainer/myfolder",
    "fileName": "myblobname",
    "format":
    {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";",
        "quoteChar": "\"",
        "NullValue": "NaN",
        "firstRowAsHeader": true,
        "skipLineCount": 0,
        "treatEmptyAsNull": true
    }
},
```

`quoteChar` yerine `escapeChar` kullanmak için `quoteChar` yazan satırı şu escapeChar ile değiştirin:

```json
"escapeChar": "$",
```

### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>firstRowAsHeader ve skipLineCount kullanım senaryoları

* Dosya olmayan bir kaynaktan bir metin dosyasına kopyalama yapıyorsunuz ve şema meta verilerini (örneğin, SQL şeması) içeren bir üst bilgi satırı eklemek istiyorsunuz. Bu senaryo için çıkış veri kümesinde `firstRowAsHeader` parametresini true olarak belirleyin.
* Üst bilgi satırı içeren bir metin dosyasından dosya olmayan bir havuza kopyalama yapıyorsunuz ve üst bilgi satırını almak istemiyorsunuz. Giriş veri kümesinde `firstRowAsHeader` parametresini true olarak belirleyin.
* Bir metin dosyasından kopyalama yapıyorsunuz ve dosyanın başındaki veri içermeyen veya üst bilgi bilgilerini içeren birkaç satırı atlamak istiyorsunuz. Atlanacak satır sayısını belirtmek için `skipLineCount` değerini belirtin. Dosyanın geri kalan kısmında üst bilgi satırı varsa `firstRowAsHeader` değerini de belirtebilirsiniz. Hem `skipLineCount` hem de `firstRowAsHeader` parametresi belirtilirse önce satırlar atlanır, ardından giriş dosyasındaki üst bilgi bilgileri okunur.

## <a name="json-format-legacy"></a><a name="json-format"></a>JSON biçimi (eski)

>[!NOTE]
>[JSON formatındaki](format-json.md) makaleden yeni modeli öğrenin. Dosya tabanlı veri deposu veri kümesindeki aşağıdaki yapılandırmalar, geriye dönük uyumluluk için olduğu gibi desteklenmeye devam eder. İleriye dönük yeni modeli kullanmanız önerilir.

**Azure Cosmos DB'den olduğu gibi bir JSON dosyasını içe/dışa aktarmak** [için, Verileri Azure Cosmos DB makalesine/aktarmaya taşı'ndaki](connector-azure-cosmos-db.md) JSON belgelerini içe aktarma/dışa aktarma bölümüne bakın.

JSON dosyalarını ayrışdırmak veya verileri JSON biçiminde yazmak `type` istiyorsanız, `format` bölümdeki özelliği **JsonFormat**olarak ayarlayın. İsterseniz `format` bölümünde aşağıdaki **isteğe bağlı** özellikleri de belirtebilirsiniz. Yapılandırma adımları için [JsonFormat örneği](#jsonformat-example) bölümünü inceleyin.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| filePattern |Her bir JSON dosyasında depolanan verilerin desenini belirtir. İzin verilen değerler: **setOfObjects** ve **arrayOfObjects**. **Varsayılan** değer **setOfObjects** olarak belirlenmiştir. Bu desenler hakkında ayrıntılı bilgi için bkz. [JSON dosyası desenleri](#json-file-patterns). |Hayır |
| jsonNodeReference | Bir dizi alanındaki aynı desene sahip verileri yinelemek ve ayıklamak istiyorsanız o dizinin JSON yolunu belirtin. Bu özellik yalnızca JSON **dosyalarından** veri kopyalanırken desteklenir. | Hayır |
| jsonPathDefinition | Her sütun için JSON yolu ifadesini belirtin ve özel bir sütun adıyla eşleyin (küçük harfle başlatın). Bu özellik yalnızca JSON **dosyalarından** veri kopyalanırken desteklenir ve nesne veya diziden veri ayıklayabilirsiniz. <br/><br/> Kök nesne altındaki alanlar için root $ ile, `jsonNodeReference` özelliği tarafından seçilen dizinin içindeki alanlar için ise dizi öğesiyle başlayın. Yapılandırma adımları için [JsonFormat örneği](#jsonformat-example) bölümünü inceleyin. | Hayır |
| encodingName |Kodlama adını belirtir. Geçerli kodlama adlarının listesi için bkz. [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) Özelliği. Örneğin: windows-1250 veya shift_jis. **Varsayılan** değer: **UTF-8**. |Hayır |
| nestingSeparator |İç içe geçme düzeylerini ayırmak için kullanılan karakterdir. Varsayılan değer "." (nokta) olarak belirlenmiştir. |Hayır |

>[!NOTE]
>Dizi halinde çapraz uygulama verilerinin birden çok satıra uygulanması durumunda [(jsonFormat örneklerinde](#jsonformat-example)örnek 1 ->), `jsonNodeReference`yalnızca özelliği kullanarak tek bir diziyi genişletmeyi seçebilirsiniz.

### <a name="json-file-patterns"></a>JSON dosyası desenleri

Kopyalama etkinliği JSON dosyalarının aşağıdaki desenlerini ayrıştırabilir:

- **1. Tür: setOfObjects**

    Her dosya tek bir nesne veya satırlara ayrılmış/bitiştirilmiş birden fazla nesne içerir. Bu seçenek bir çıkış veri kümesinde belirlendiğinde, kopyalama etkinliği her satırda bir nesnenin bulunduğu (satırlara ayrılmış) tek bir JSON dosyası üretir.

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

### <a name="jsonformat-example"></a>JsonFormat örneği

**Örnek Durum 1: JSON dosyalarından veri kopyalama**

**Örnek 1: nesne ve diziden veri ayıklama**

Bu örnekte, bir kök JSON nesnesinin tablosal sonuçtaki tek bir kayıtla eşleşmesi beklenir. Aşağıdaki içeriğe sahip bir JSON dosyanız varsa:

```json
{
    "id": "ed0e4960-d9c5-11e6-85dc-d7996816aad3",
    "context": {
        "device": {
            "type": "PC"
        },
        "custom": {
            "dimensions": [
                {
                    "TargetResourceType": "Microsoft.Compute/virtualMachines"
                },
                {
                    "ResourceManagementProcessRunId": "827f8aaa-ab72-437c-ba48-d8917a7336a3"
                },
                {
                    "OccurrenceTime": "1/13/2017 11:24:37 AM"
                }
            ]
        }
    }
}
```

ve hem nesne hem de diziden veri ayıklayarak bir Azure SQL tablosuna aşağıdaki biçimde kopyalamak istersiniz:

| Kimlik | deviceType | targetResourceType | kaynakManagementProcessRunId | occurrenceTime |
| --- | --- | --- | --- | --- |
| ed0e4960-d9c5-11e6-85dc-d7996816aad3 | PC | Microsoft.Compute/virtualMachines | 827f8aaa-ab72-437c-ba48-d8917a7336a3 | 1/13/2017 11:24:37 AM |

**JsonFormat** türüne sahip giriş veri kümesi şu şekilde tanımlanır: (yalnızca ilgili bölümlerin gösterildiği kısmi tanım). Daha ayrıntılı belirtmek gerekirse:

- `structure` bölümü, tablo verilerine dönüştürme sırasında kullanılan özelleştirilmiş sütun adlarını ve karşılık gelen veri türünü tanımlar. Bu bölüm **isteğe bağlıdır** ve yalnızca sütun eşleme için kullanmanız gerekir. Daha fazla bilgi için, [hedef veri kümesi sütunlarına harita kaynağı veri kümesi sütunlarına](copy-activity-schema-and-type-mapping.md)bakın.
- `jsonPathDefinition`, her sütun için verilerin ayıklanacağı JSON yolunu belirtir. Dizideki verileri kopyalamak için, verilen `array[x].property` `xth` özelliğin değerini nesneden ayıklamak `array[*].property` için veya bu özelliği içeren herhangi bir nesneden değeri bulmak için kullanabilirsiniz.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "deviceType",
            "type": "String"
        },
        {
            "name": "targetResourceType",
            "type": "String"
        },
        {
            "name": "resourceManagementProcessRunId",
            "type": "String"
        },
        {
            "name": "occurrenceTime",
            "type": "DateTime"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonPathDefinition": {"id": "$.id", "deviceType": "$.context.device.type", "targetResourceType": "$.context.custom.dimensions[0].TargetResourceType", "resourceManagementProcessRunId": "$.context.custom.dimensions[1].ResourceManagementProcessRunId", "occurrenceTime": " $.context.custom.dimensions[2].OccurrenceTime"}
        }
    }
}
```

**Örnek 2: diziden aynı desene sahip birden fazla nesneyi çapraz uygulama**

Bu örnekte, bir kök JSON nesnesinin tablosal sonuçtaki birden fazla kayda dönüştürülmesi beklenir. Aşağıdaki içeriğe sahip bir JSON dosyanız varsa:

```json
{
    "ordernumber": "01",
    "orderdate": "20170122",
    "orderlines": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "sanmateo": "No 1" } ]
}
```

ve bunu bir Azure SQL tablosuna aşağıdaki biçimde, dizi içindeki verileri düzleştirerek ve ortak kök bilgileriyle çapraz birleşim yaparak kopyalamak istiyorsanız:

| `ordernumber` | `orderdate` | `order_pd` | `order_price` | `city` |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | `[{"sanmateo":"No 1"}]` |
| 01 | 20170122 | P2 | 13 | `[{"sanmateo":"No 1"}]` |
| 01 | 20170122 | P3 | 231 | `[{"sanmateo":"No 1"}]` |


**JsonFormat** türüne sahip giriş veri kümesi şu şekilde tanımlanır: (yalnızca ilgili bölümlerin gösterildiği kısmi tanım). Daha ayrıntılı belirtmek gerekirse:

- `structure` bölümü, tablo verilerine dönüştürme sırasında kullanılan özelleştirilmiş sütun adlarını ve karşılık gelen veri türünü tanımlar. Bu bölüm **isteğe bağlıdır** ve yalnızca sütun eşleme için kullanmanız gerekir. Daha fazla bilgi için, [hedef veri kümesi sütunlarına harita kaynağı veri kümesi sütunlarına](copy-activity-schema-and-type-mapping.md)bakın.
- `jsonNodeReference`**dizi** `orderlines`altında aynı desene sahip nesnelerden veri ayıklamak ve ayıklamak için gösterir.
- `jsonPathDefinition`, her sütun için verilerin ayıklanacağı JSON yolunu belirtir. `ordernumber`Bu örnekte, `orderdate`, `city` , ve JSON yolu `$.`ile `order_pd` başlayan `order_price` kök nesnesi altında iken `$.`, ve olmadan dizi öğesinden türetilen yol ile tanımlanır .

```json
"properties": {
    "structure": [
        {
            "name": "ordernumber",
            "type": "String"
        },
        {
            "name": "orderdate",
            "type": "String"
        },
        {
            "name": "order_pd",
            "type": "String"
        },
        {
            "name": "order_price",
            "type": "Int64"
        },
        {
            "name": "city",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonNodeReference": "$.orderlines",
            "jsonPathDefinition": {"ordernumber": "$.ordernumber", "orderdate": "$.orderdate", "order_pd": "prod", "order_price": "price", "city": " $.city"}
        }
    }
}
```

**Aşağıdaki noktalara dikkat edin:**

* `structure` ve `jsonPathDefinition`, Data Factory veri kümesinde tanımlanmamışsa, Copy Activity şemayı ilk nesneden algılar ve nesnenin tamamını düzleştirir.
* JSON girişi bir diziye sahipse, Copy Activity dizi değerinin tamamını varsayılan olarak bir dizeye dönüştürür. Verileri `jsonNodeReference` ve/veya `jsonPathDefinition` kullanarak ayıklayabilir ya da `jsonPathDefinition` içinde belirtmeden atlayabilirsiniz.
* Aynı düzeyde birden fazla ad varsa Copy Activity sonuncusunu alır.
* Özellik adları büyük/küçük harfe duyarlıdır. Aynı ada ancak farklı büyük/küçük harf düzenine sahip iki özellik, iki ayrı özellik olarak kabul edilir.

**Durum 2: JSON dosyasına veri yazma**

SQL Veritabanı'nda aşağıdaki tablo varsa:

| Kimlik | order_date | order_price | order_by |
| --- | --- | --- | --- |
| 1 | 20170119 | 2000 | David |
| 2 | 20170120 | 3500 | Patrick |
| 3 | 20170121 | 4000 | Jason |

ve her kayıt için, aşağıdaki biçimde bir JSON nesnesine yazmayı bekliyorsunuz:

```json
{
    "id": "1",
    "order": {
        "date": "20170119",
        "price": 2000,
        "customer": "David"
    }
}
```

**JsonFormat** türüne sahip çıkış veri kümesi şu şekilde tanımlanır: (yalnızca ilgili bölümlerin gösterildiği kısmi tanım). Daha ayrıntılı `structure` olarak, bölüm hedef dosyadaki özelleştirilmiş özellik adlarını tanımlar, `nestingSeparator` (varsayılan ".") adından yuva katmanıtanımlamak için kullanılır. Bu bölüm **isteğe bağlıdır** ve kaynak sütunu adıyla karşılaştırarak özellik adını değiştirmek veya özelliklerin bazılarını iç içe yerleştirmek için kullanmanız gerekir.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "order.date",
            "type": "String"
        },
        {
            "name": "order.price",
            "type": "Int64"
        },
        {
            "name": "order.customer",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat"
        }
    }
}
```

## <a name="parquet-format-legacy"></a><a name="parquet-format"></a>Parke formatı (eski)

>[!NOTE]
>[Parke formatında](format-parquet.md) makaleden yeni modeli öğrenin. Dosya tabanlı veri deposu veri kümesindeki aşağıdaki yapılandırmalar, geriye dönük uyumluluk için olduğu gibi desteklenmeye devam eder. İleriye dönük yeni modeli kullanmanız önerilir.

Parquet dosyalarını ayrıştırmak veya verileri Parquet biçiminde yazmak istiyorsanız `format` `type` özelliğini **ParquetFormat** olarak ayarlayın. typeProperties bölümünün içindeki Format bölümünde herhangi bir özellik belirtmenize gerek yoktur. Örnek:

```json
"format":
{
    "type": "ParquetFormat"
}
```

Aşağıdaki noktalara dikkat edin:

* Karmaşık veri türleri desteklenmez (MAP, LIST).
* Sütun adındaki beyaz alan desteklenmez.
* Parquet dosyası sıkıştırmayla ilgili şu seçeneklere sahiptir: NONE, SNAPPY, GZIP ve LZO. Veri Fabrikası, LZO dışındaki bu sıkıştırılmış biçimlerin herhangi birinde Parke dosyasından gelen verileri okumayı destekler - verileri okumak için meta verilerdeki sıkıştırma kodc'sini kullanır. Ancak Data Factory bir Parquet dosyasına yazarken varsayılan Parquet biçimi SNAPPY seçeneğini kullanır. Şu anda bu davranışı geçersiz kılma seçeneği yoktur.

> [!IMPORTANT]
> Kendi kendine barındırılan Tümleştirme Runtime tarafından yetkilendirilmiş kopya için, örneğin şirket içi ve bulut veri depoları arasında, Parke dosyalarını **olduğu gibi**kopyalamazsanız, IR makinenize **64 bit JRE 8 (Java Runtime Ortamı) veya OpenJDK** yüklemeniz gerekir. Daha fazla ayrıntı içeren aşağıdaki paragrafa bakın.

Parke dosyası serileştirme/deserialization ile Self barındırılan IR üzerinde çalışan kopya için, ADF *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* ilk olarak JRE için kayıt defterikontrol *`JAVA_HOME`* ederek Java çalışma süresini bulur, eğer bulunmazsa, ikinci olarak OpenJDK için sistem değişkenini denetler.

- **JRE kullanmak için**: 64-bit IR 64-bit JRE gerektirir. [Buradan](https://go.microsoft.com/fwlink/?LinkId=808605)bulabilirsin.
- **OpenJDK kullanmak için**: IR sürüm 3.13'ten beri desteklenir. OpenJDK'nın diğer tüm gerekli montajlarını kendi kendine barındırılan IR makinesine paketleyip, sistem ortamını buna göre JAVA_HOME ayarlayın.

>[!TIP]
>Kendi kendine barındırılan Tümleştirme Runtime kullanarak Parke formatına/parke formatından verileri kopyalarsanız ve "Java, mesaj: **java.lang.OutOfMemoryError:Java yığın alanı"** çağırıldığında bir hata oluştu" diyerek hatayı vurursanız, JVM'nin bu tür kopyalamayı güçlendirmek için min/max heap boyutunu ayarlamak için kendi kendine barındırılan IR'yi barındıran makineye bir ortam değişkeni `_JAVA_OPTIONS` ekleyebilirsiniz ve ardından ardışık ardışık hattı yeniden çalıştırabilirsiniz.

![Kendi kendine barındırılan IR'de JVM yığın boyutunu ayarlama](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Örnek: değer `_JAVA_OPTIONS` `-Xms256m -Xmx16g`ile değişken ayarlayın. Bayrak, `Xms` java sanal makinesi (JVM) için ilk bellek `Xmx` ayırma havuzunu belirtirken, en yüksek bellek ayırma havuzunu belirtir. Bu, JVM'nin bellek `Xms` miktarıyla başlatılacağı ve maksimum `Xmx` bellek miktarı kullanabileceği anlamına gelir. Varsayılan olarak, ADF min 64MB ve max 1G kullanır.

### <a name="data-type-mapping-for-parquet-files"></a>Parke dosyaları için veri türü eşleme

| Veri fabrikası geçici veri türü | Parke İlkel Tip | Parke Orijinal Tip (Deserialize) | Parke Orijinal Tip (Serialize) |
|:--- |:--- |:--- |:--- |
| Boole | Boole | Yok | Yok |
| SByte | Int32 | Int8 | Int8 |
| Bayt | Int32 | UInt8 | Int16 |
| Int16 | Int32 | Int16 | Int16 |
| UInt16 | Int32 | UInt16 | Int32 |
| Int32 | Int32 | Int32 | Int32 |
| UInt32 | Int64 | UInt32 | Int64 |
| Int64 | Int64 | Int64 | Int64 |
| UInt64 | Int64/İkili | UInt64 | Ondalık |
| Tek | Kayan | Yok | Yok |
| Çift | Çift | Yok | Yok |
| Ondalık | İkili | Ondalık | Ondalık |
| Dize | İkili | Utf8 | Utf8 |
| DateTime | Int96 | Yok | Yok |
| TimeSpan | Int96 | Yok | Yok |
| DateTimeOffset | Int96 | Yok | Yok |
| Bytearray | İkili | Yok | Yok |
| Guid | İkili | Utf8 | Utf8 |
| Char | İkili | Utf8 | Utf8 |
| CharArray | Desteklenmiyor | Yok | Yok |

## <a name="orc-format-legacy"></a><a name="orc-format"></a>ORC biçimi (eski)

>[!NOTE]
>[ORC formatındaki](format-orc.md) makaleden yeni modeli öğrenin. Dosya tabanlı veri deposu veri kümesindeki aşağıdaki yapılandırmalar, geriye dönük uyumluluk için olduğu gibi desteklenmeye devam eder. İleriye dönük yeni modeli kullanmanız önerilir.

ORC dosyalarını ayrıştırmak veya verileri ORC biçiminde yazmak istiyorsanız `format` `type` özelliğini **OrcFormat** olarak ayarlayın. typeProperties bölümünün içindeki Format bölümünde herhangi bir özellik belirtmenize gerek yoktur. Örnek:

```json
"format":
{
    "type": "OrcFormat"
}
```

Aşağıdaki noktalara dikkat edin:

* Karmaşık veri türleri desteklenmez (STRUCT, MAP, LIST, UNION).
* Sütun adındaki beyaz alan desteklenmez.
* ORC dosyası [sıkıştırmayla ilgili üç seçeneğe sahiptir](https://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY. Data Factory, bu sıkıştırma biçimlerinin herhangi birine sahip ORC dosyalarını okuyabilir. Verileri okumak için meta verilerdeki sıkıştırma kodlayıcısı/kod çözücüsünü kullanır. Ancak Data Factory bir ORC dosyasına yazarken varsayılan ORC değeri olan ZLIB seçeneğini kullanır. Şu anda bu davranışı geçersiz kılma seçeneği yoktur.

> [!IMPORTANT]
> Kendi kendine barındırılan Tümleştirme Runtime tarafından yetkilendirilmiş kopya için, örneğin şirket içi ve bulut veri depoları arasında, ORC dosyalarını **olduğu gibi**kopyalamazsanız, IR makinenize **64 bit JRE 8 (Java Runtime Ortamı) veya OpenJDK** yüklemeniz gerekir. Daha fazla ayrıntı içeren aşağıdaki paragrafa bakın.

ORC dosya serileştirme/deserialization ile Self barındırılan IR üzerinde çalışan kopya için, ADF *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* ilk olarak JRE için kayıt defterini *`JAVA_HOME`* kontrol ederek Java çalışma süresini bulur, eğer bulunmazsa, ikinci olarak OpenJDK için sistem değişkenini denetler.

- **JRE kullanmak için**: 64-bit IR 64-bit JRE gerektirir. [Buradan](https://go.microsoft.com/fwlink/?LinkId=808605)bulabilirsin.
- **OpenJDK kullanmak için**: IR sürüm 3.13'ten beri desteklenir. OpenJDK'nın diğer tüm gerekli montajlarını kendi kendine barındırılan IR makinesine paketleyip, sistem ortamını buna göre JAVA_HOME ayarlayın.

### <a name="data-type-mapping-for-orc-files"></a>ORC dosyaları için veri türü eşleme

| Veri fabrikası geçici veri türü | ORC türleri |
|:--- |:--- |
| Boole | Boole |
| SByte | Bayt |
| Bayt | Kısa |
| Int16 | Kısa |
| UInt16 | int |
| Int32 | int |
| UInt32 | Uzun |
| Int64 | Uzun |
| UInt64 | Dize |
| Tek | Kayan |
| Çift | Çift |
| Ondalık | Ondalık |
| Dize | Dize |
| DateTime | Zaman damgası |
| DateTimeOffset | Zaman damgası |
| TimeSpan | Zaman damgası |
| Bytearray | İkili |
| Guid | Dize |
| Char | Char(1) |

## <a name="avro-format-legacy"></a><a name="avro-format"></a>AVRO formatı (eski)

>[!NOTE]
>Yeni modeli [Avro formatındaki](format-avro.md) makaleden öğrenin. Dosya tabanlı veri deposu veri kümesindeki aşağıdaki yapılandırmalar, geriye dönük uyumluluk için olduğu gibi desteklenmeye devam eder. İleriye dönük yeni modeli kullanmanız önerilir.

Avro dosyalarını ayrıştırmak veya verileri Avro biçiminde yazmak istiyorsanız `format` `type` özelliğini **AvroFormat** olarak ayarlayın. typeProperties bölümünün içindeki Format bölümünde herhangi bir özellik belirtmenize gerek yoktur. Örnek:

```json
"format":
{
    "type": "AvroFormat",
}
```

Avro biçimini bir Hive tablosunda kullanmak için [Apache Hive öğreticisini](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe) inceleyebilirsiniz.

Aşağıdaki noktalara dikkat edin:

* [Karmaşık veri türleri](https://avro.apache.org/docs/current/spec.html#schema_complex) desteklenmez (kayıtlar, enumlar, diziler, haritalar, birlikler ve sabit).

## <a name="compression-support-legacy"></a><a name="compression-support"></a>Sıkıştırma desteği (eski)

Azure Veri Fabrikası kopya sırasında verileri sıkıştırma/dekomprese destekler. Bir giriş `compression` veri kümesinde özellik belirttiğiniz zaman, kopyalama etkinliği kaynaktan sıkıştırılmış verileri okur ve sıkıştırmayı dekomprese olur; ve bir çıktı veri kümesinde özelliği belirttiğiniz zaman, kopyalama etkinliği sıkıştırma sonra lavaboya veri yazın. Aşağıda birkaç örnek senaryo verilmiştir:

* GZIP sıkıştırılmış verileri bir Azure blob'undan okuyun, sıkıştırın ve sonuç verilerini bir Azure SQL veritabanına yazın. Özellik ile giriş Azure Blob veri `compression` `type` kümesini GZIP olarak tanımlarsınız.
* Şirket içi Dosya Sistemi'ndeki düz metin dosyasındaki verileri okuyun, GZip biçimini kullanarak sıkıştırın ve sıkıştırılmış verileri Azure blob'una yazın. Özellik GZip olarak bir `compression` çıktı Azure Blob veri kümesi tanımlarsınız. `type`
* FTP sunucusundan .zip dosyasını okuyun, dosyaları içeri almak için sıkıştırın ve bu dosyaları Azure Data Lake Store'a indirin. ZipDeflate olarak `compression` `type` özelliği ile bir giriş FTP veri kümesi tanımlarsınız.
* Bir Azure blob'undan GZIP sıkıştırılmış verileri okuyun, sıkıştırın, BZIP2 kullanarak sıkıştırın ve sonuç verilerini Azure blob'una yazın. GZIP'e ayarlı `compression` `type` Azure Blob veri kümesini ve BZIP2 `compression` `type` olarak ayarlanmış çıktı veri kümesini tanımlarsınız.

Bir veri kümesi için sıkıştırma belirtmek için, aşağıdaki örnekte olduğu gibi json veri kümesinde **sıkıştırma** özelliğini kullanın:

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "fileName": "pagecounts.csv.gz",
            "folderPath": "compression/file/",
            "format": {
                "type": "TextFormat"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

**Sıkıştırma** bölümünün iki özelliği vardır:

* **Türü:** sıkıştırma codec, **GZIP**olabilir , **Deflate**, **BZIP2**, veya **ZipDeflate**. ZipDeflate dosya(lar) decompress ve dosya tabanlı lavabo veri deposuna yazmak için kopyalama etkinliği kullanırken not, dosyalar klasöre ayıklanır: `<path specified in dataset>/<folder named as source zip file>/`.
* **Seviye:** **Optimal** veya **En Hızlı**olabilir sıkıştırma oranı.

  * **En hızlı:** Elde edilen dosya en iyi şekilde sıkıştırılmamış olsa bile sıkıştırma işlemi mümkün olan en kısa sürede tamamlanmalıdır.
  * **Optimal**: Sıkıştırma işlemi, operasyonun tamamlanması daha uzun sürse bile en iyi şekilde sıkıştırılmalıdır.

    Daha fazla bilgi için [Sıkıştırma Düzeyi](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) konusuna bakın.

> [!NOTE]
> Sıkıştırma ayarları **AvroFormat,** **OrcFormat**veya **Parke Format'taki**veriler için desteklenmez. Bu biçimlerde dosyaları okurken, Veri Fabrikası meta verilerdeki sıkıştırma kodc'sini algılar ve kullanır. Bu biçimlerde dosyalara yazarken, Veri Fabrikası bu biçim için varsayılan sıkıştırma kodc'sini seçer. Örneğin, OrcFormat için ZLIB ve ParkquetFormat için SNAPPY.

## <a name="unsupported-file-types-and-compression-formats"></a>Desteklenmeyen dosya türleri ve sıkıştırma biçimleri

Desteklenmeyen dosyaları dönüştürmek için Azure Veri Fabrikası'nın genişletilebilirlik özelliklerini kullanabilirsiniz.
İki seçenek, Azure Toplu İşi'ni kullanarak Azure İşlevleri ve özel görevleri içerir.

[Katran dosyasının içeriğini ayıklamak](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV2/UntarAzureFilesWithAzureFunction)için Azure işlevini kullanan bir örnek görebilirsiniz. Daha fazla bilgi için [Azure İşlevler etkinliğine](https://docs.microsoft.com/azure/data-factory/control-flow-azure-function-activity)bakın.

Bu işlevselliği özel bir dotnet etkinliğini kullanarak da oluşturabilirsiniz. Daha fazla bilgiyi [burada](https://docs.microsoft.com/azure/data-factory/transform-data-using-dotnet-custom-activity) bulabilirsiniz

## <a name="next-steps"></a>Sonraki adımlar

Desteklenen dosya biçimlerinden ve sıkıştırmalardan desteklenen en son [dosya biçimlerini ve sıkıştırmaları](supported-file-formats-and-compression-codecs.md)öğrenin.
