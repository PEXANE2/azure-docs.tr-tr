---
title: Kopyalama etkinliğinde şema eşleme | Microsoft Docs
description: "' Deki kopyalama etkinliğinin Azure Data Factory, verileri kopyalarken kaynak verilerden şemaları ve veri türlerini havuz verilerine nasıl eşlediğini öğrenin."
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: b705123dc6492466c30b3c1ddaf4b330b0d684a1
ms.sourcegitcommit: a6718e2b0251b50f1228b1e13a42bb65e7bf7ee2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71272274"
---
# <a name="schema-mapping-in-copy-activity"></a>Kopyalama etkinliğinde şema eşleme

Bu makalede, veri kopyalama sırasında Azure Data Factory kopyalama etkinliğinin şema eşleme ve kaynak verilerden veri türü eşleme nasıl olduğu açıklanır.

## <a name="schema-mapping"></a>Şema eşleme

Verileri kaynaktan havuza kopyalarken sütun eşleme uygulanır. Varsayılan olarak, etkinlik **eşleme kaynağı verilerini sütun adlarına göre havuza**kopyalayın. İhtiyaya göre sütun eşlemeyi özelleştirmek için [Açık eşleme](#explicit-mapping) belirtebilirsiniz. Daha özel olarak, kopyalama etkinliği:

1. Kaynaktaki verileri okuyun ve kaynak şemayı belirleme
2. Sütunları ada göre eşlemek için varsayılan sütun eşlemesini kullanın veya belirtilmişse açık sütun eşlemesi uygulayın.
3. Verileri havuza yaz

### <a name="explicit-mapping"></a>Açık eşleme

Kopyalama etkinliği-> `translator`  ->  `mappings` özelliğinde eşlenecek sütunları belirtebilirsiniz. Aşağıdaki örnek, sınırlı metinden Azure SQL veritabanı 'na veri kopyalamak için bir işlem hattındaki kopyalama etkinliğini tanımlar.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [{
        "referenceName": "DelimitedTextInput",
        "type": "DatasetReference"
    }],
    "outputs": [{
        "referenceName": "AzureSqlOutput",
        "type": "DatasetReference"
    }],
    "typeProperties": {
        "source": { "type": "DelimitedTextSource" },
        "sink": { "type": "SqlSink" },
        "translator": {
            "type": "TabularTranslator",
            "mappings": [
                {
                    "source": {
                        "name": "UserId",
                        "type": "Guid"
                    },
                    "sink": {
                        "name": "MyUserId"
                    }
                }, 
                {
                    "source": {
                        "name": "Name",
                        "type": "String"
                    },
                    "sink": {
                        "name": "MyName"
                    }
                }, 
                {
                    "source": {
                        "name": "Group",
                        "type": "String"
                    },
                    "sink": {
                        "name": "MyGroup"
                    }
                }
            ]
        }
    }
}
```

Aşağıdaki özellikler, ve `translator` `mappings`  ->  ile>`source` nesne altında desteklenir: `sink`

| Özellik | Açıklama                                                  | Gerekli |
| -------- | ------------------------------------------------------------ | -------- |
| name     | Kaynak veya havuz sütununun adı.                           | Evet      |
| ordinal  | Sütun dizini. 1 ile başlayın. <br>Üst bilgi satırı olmayan sınırlandırılmış metin kullanılırken geçerlidir ve gereklidir. | Hayır       |
| path     | Ayıklanacak veya eşlenecek her alan için JSON yol ifadesi. Hiyerarşik veriler için geçerlidir örn. MongoDB/REST.<br>Kök nesnesi altındaki alanlar için JSON yolu root $; ile başlar özelliği tarafından `collectionReference` seçilen dizi içindeki alanlar için JSON yolu dizi öğesinden başlar. | Hayır       |
| type     | Kaynak veya havuz sütununun geçici veri türü Data Factory. | Hayır       |
| culture  | Kaynak veya havuz sütununun kültürü. <br>Tür `Datetime` veya`Datetimeoffset`olduğunda geçerlidir. Varsayılan, `en-us` değeridir. | Hayır       |
| format   | Tür `Datetime` veya`Datetimeoffset`olduğunda kullanılacak biçim dizesi. Tarih saat biçimini biçimlendirmek için [özel tarih ve saat biçim dizelerine](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) bakın. | Hayır       |

Aşağıdaki özellikler, ve `translator` `mappings`  ->  ile`sink`nesnesine ek olarak altında desteklenir: `source`

| Özellik            | Açıklama                                                  | Gerekli |
| ------------------- | ------------------------------------------------------------ | -------- |
| collectionReference | Yalnızca hiyerarşik veriler (örneğin, MongoDB/REST kaynak olduğunda) desteklenir.<br>Aynı düzene sahip **bir dizi alanı içindeki** nesnelerden verileri yinelemek ve ayıklamak istiyorsanız, her nesne için bu dizinin JSON yolunu belirtin. | Hayır       |

### <a name="alternative-column-mapping"></a>Alternatif sütun eşleme

Tablosal şekilli veriler arasında eşlenecek kopyalama `translator` etkinliği->  ->  `columnMappings` belirtebilirsiniz. Bu durumda, hem giriş hem de çıkış veri kümeleri için "yapı" bölümü gereklidir. Sütun eşleme **, "Structure" kaynak veri kümesindeki sütunların tüm veya alt kümelerinin havuz veri kümesi "Structure" içindeki tüm sütunlara eşlenmesinin**kullanılmasını destekler. Aşağıda bir özel durumla sonuçlanan hata koşulları verilmiştir:

* Kaynak veri deposu sorgu sonucunun, giriş veri kümesi "yapısı" bölümünde belirtilen bir sütun adı yok.
* Havuz veri deposu (önceden tanımlanmış şema varsa), çıkış veri kümesi "yapısı" bölümünde belirtilen bir sütun adına sahip değildir.
* Havuz veri kümesinin "yapısında" eşlemede belirtilenden daha az sütun veya daha fazla sütun.
* Yinelenen eşleme.

Aşağıdaki örnekte, giriş veri kümesi bir yapıya sahiptir ve şirket içi Oracle veritabanındaki bir tabloya işaret eder.

```json
{
    "name": "OracleDataset",
    "properties": {
        "structure":
         [
            { "name": "UserId"},
            { "name": "Name"},
            { "name": "Group"}
         ],
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "OracleLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTable"
        }
    }
}
```

Bu örnekte, çıkış veri kümesi bir yapıya sahiptir ve Salesfoce içindeki bir tabloya işaret eder.

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "structure":
        [
            { "name": "MyUserId"},
            { "name": "MyName" },
            { "name": "MyGroup"}
        ],
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "SalesforceLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SinkTable"
        }
    }
}
```

Aşağıdaki JSON bir işlem hattındaki kopyalama etkinliğini tanımlar. **Çevirmen** -> **ColumnMappings** özelliği kullanılarak, havuzdaki sütunlara eşlenen kaynak sütunları.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [
        {
            "referenceName": "OracleDataset",
            "type": "DatasetReference"
        }
    ],
    "outputs": [
        {
            "referenceName": "SalesforceDataset",
            "type": "DatasetReference"
        }
    ],
    "typeProperties":    {
        "source": { "type": "OracleSource" },
        "sink": { "type": "SalesforceSink" },
        "translator":
        {
            "type": "TabularTranslator",
            "columnMappings":
            {
                "UserId": "MyUserId",
                "Group": "MyGroup",
                "Name": "MyName"
            }
        }
    }
}
```

Sütun eşlemesini belirtmek için öğesinin `"columnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"` sözdizimini kullanıyorsanız, hala olduğu gibi desteklenir.

### <a name="alternative-schema-mapping"></a>Alternatif şema eşleme

MongoDB/Rest 'den metin `translator` dosyasına kopyalama yapmak ve Oracle 'dan MongoDB için Azure Cosmos DB ' a kopyalamak için kopyalama etkinliği->  ->  `schemaMapping` ' ni belirtebilirsiniz. Kopyalama etkinliği `translator` bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği çeviricisinin Type özelliği şu şekilde ayarlanmalıdır: **TabularTranslator** | Evet |
| schemaMapping | **Kaynak taraftan havuz tarafına**olan eşleme ilişkisini temsil eden anahtar-değer çiftleri koleksiyonu.<br/>- **Key:** kaynak temsil eder. **Tablo kaynağı**için, sütun adını veri kümesi yapısında tanımlanan şekilde belirtin; **hiyerarşik kaynak**için, Ayıklanacak ve eşlenecek her alan için JSON yolu ifadesini belirtin.<br>- **Value:** havuz temsil eder. **Tablo havuzu**için, sütun adını veri kümesi yapısında tanımlanan şekilde belirtin; **hiyerarşik havuz**için, Ayıklanacak ve eşlenecek her alan için JSON yolu ifadesini belirtin. <br>Hiyerarşik veriler söz konusu olduğunda, kök nesne altındaki alanlar için JSON yolu kök $; ile başlar özelliği tarafından `collectionReference` seçilen dizi içindeki alanlar için JSON yolu dizi öğesinden başlar.  | Evet |
| collectionReference | Aynı düzene sahip **bir dizi alanı içindeki** nesnelerden verileri yinelemek ve ayıklamak istiyorsanız, her nesne için bu dizinin JSON yolunu belirtin. Bu özellik yalnızca hiyerarşik veriler kaynak olduğunda desteklenir. | Hayır |

**Örnek: MongoDB 'den Oracle 'a kopyalama:**

Örneğin, aşağıdaki içeriğe sahip MongoDB belgeniz varsa:

```json
{
    "id": {
        "$oid": "592e07800000000000000000"
    },
    "number": "01",
    "date": "20170122",
    "orders": [
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
    "city": [ { "name": "Seattle" } ]
}
```

Ayrıca, dizi içindeki verileri *(order_pd ve order_price)* ve çapraz birleştirmeyi ortak kök bilgiyle *(sayı, tarih ve şehir)* DÜZLEŞTIREREK bir Azure SQL tablosuna aşağıdaki biçimde kopyalamak istersiniz:

| Sipariş numarası | orderDate | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | Seattle |
| 01 | 20170122 | P2 | 13 | Seattle |
| 01 | 20170122 | P3 | 231 | Seattle |

Şema eşleme kuralını aşağıdaki kopyalama etkinliği JSON örneği olarak yapılandırın:

```json
{
    "name": "CopyFromMongoDBToOracle",
    "type": "Copy",
    "typeProperties": {
        "source": {
            "type": "MongoDbV2Source"
        },
        "sink": {
            "type": "OracleSink"
        },
        "translator": {
            "type": "TabularTranslator",
            "schemaMapping": {
                "orderNumber": "$.number",
                "orderDate": "$.date",
                "order_pd": "prod",
                "order_price": "price",
                "city": " $.city[0].name"
            },
            "collectionReference":  "$.orders"
        }
    }
}
```

## <a name="data-type-mapping"></a>Veri türü eşleme

Kopyalama etkinliği, aşağıdaki 2 adımlı yaklaşımla birlikte havuz türleri eşleme için kaynak türleri gerçekleştirir:

1. Yerel kaynak türlerinden Azure Data Factory geçici veri türlerine Dönüştür
2. Azure Data Factory geçici veri türlerinden yerel havuz türüne Dönüştür

Her bir bağlayıcı konusunun "veri türü eşleme" bölümünde yerel tür ile ara tür arasındaki eşlemeyi bulabilirsiniz.

### <a name="supported-data-types"></a>Desteklenen veri türleri

Data Factory aşağıdaki geçici veri türlerini destekler: [Veri kümesi yapısı](concepts-datasets-linked-services.md#dataset-structure-or-schema) yapılandırmasında tür bilgilerini yapılandırırken aşağıdaki değerleri belirtebilirsiniz:

* Byte[]
* Boole değeri
* Datetime
* Datetimeoffset
* Decimal
* Double
* Guid
* Int16
* Int32
* Int64
* Single
* Dize
* Zaman aralığı

## <a name="next-steps"></a>Sonraki adımlar
Bir kopyalama etkinliği makalelere bakın:

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
