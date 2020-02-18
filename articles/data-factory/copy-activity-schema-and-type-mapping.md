---
title: Kopyalama etkinliğinde şema eşleme
description: "' Deki kopyalama etkinliğinin Azure Data Factory, verileri kopyalarken kaynak verilerden şemaları ve veri türlerini havuz verilerine nasıl eşlediğini öğrenin."
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: jingwang
ms.openlocfilehash: 9ae07e2a471cc417b467092a2616a5a0cdafb1fe
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77423648"
---
# <a name="schema-mapping-in-copy-activity"></a>Kopyalama etkinliğinde şema eşleme

Bu makalede, veri kopyalama sırasında Azure Data Factory kopyalama etkinliğinin şema eşleme ve kaynak verilerden veri türü eşleme nasıl olduğu açıklanır.

## <a name="schema-mapping"></a>Şema eşleme

Verileri kaynaktan havuza kopyalarken sütun eşleme uygulanır. Varsayılan olarak, etkinlik **eşleme kaynağı verilerini sütun adlarına göre havuza**kopyalayın. İhtiyaya göre sütun eşlemeyi özelleştirmek için [Açık eşleme](#explicit-mapping) belirtebilirsiniz. Daha özel olarak, kopyalama etkinliği:

1. Kaynaktaki verileri okuyun ve kaynak şemayı belirleme
2. Sütunları ada göre eşlemek için varsayılan sütun eşlemesini kullanın veya belirtilmişse açık sütun eşlemesi uygulayın.
3. Verileri havuza yaz

### <a name="explicit-mapping"></a>Açık eşleme

Kopyalama etkinliği ' nde eşlenecek sütunları belirtebilirsiniz-> `translator` -> `mappings` özelliği. Aşağıdaki örnek, sınırlı metinden Azure SQL veritabanı 'na veri kopyalamak için bir işlem hattındaki kopyalama etkinliğini tanımlar.

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

Aşağıdaki özellikler, `source` ve `sink`> nesne `mappings` `translator` -> altında desteklenir:

| Özellik | Açıklama                                                  | Gerekli |
| -------- | ------------------------------------------------------------ | -------- |
| ad     | Kaynak veya havuz sütununun adı.                           | Yes      |
| ordinal  | Sütun dizini. 1 ile başlayın. <br>Üst bilgi satırı olmayan sınırlandırılmış metin kullanılırken geçerlidir ve gereklidir. | Hayır       |
| yol     | Ayıklanacak veya eşlenecek her alan için JSON yol ifadesi. Hiyerarşik veriler için geçerlidir örn. MongoDB/REST.<br>Kök nesnesi altındaki alanlar için JSON yolu root $; ile başlar `collectionReference` özelliği tarafından seçilen dizi içindeki alanlar için JSON yolu dizi öğesinden başlar. | Hayır       |
| type     | Kaynak veya havuz sütununun geçici veri türü Data Factory. | Hayır       |
| culture  | Kaynak veya havuz sütununun kültürü. <br>Tür `Datetime` veya `Datetimeoffset`olduğunda geçerlidir. Varsayılan değer: `en-us`. | Hayır       |
| format   | Tür `Datetime` veya `Datetimeoffset`olduğunda kullanılacak biçim dizesi. Tarih saat biçimini biçimlendirmek için [özel tarih ve saat biçim dizelerine](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) bakın. | Hayır       |

Aşağıdaki özellikler, `source` ve `sink`nesnesine ek olarak `translator` -> `mappings` altında desteklenir:

| Özellik            | Açıklama                                                  | Gerekli |
| ------------------- | ------------------------------------------------------------ | -------- |
| collectionReference | Yalnızca hiyerarşik veriler (örneğin, MongoDB/REST kaynak olduğunda) desteklenir.<br>Aynı düzene sahip **bir dizi alanı içindeki** nesnelerden verileri yinelemek ve ayıklamak istiyorsanız, her nesne için bu dizinin JSON yolunu belirtin. | Hayır       |

### <a name="alternative-column-mapping"></a>Alternatif sütun eşleme

Tablolu şekillendirilmiş veriler arasında eşleme yapmak için > `translator` -> `columnMappings` kopyalama etkinliğini belirtebilirsiniz. Bu durumda, hem giriş hem de çıkış veri kümeleri için "yapı" bölümü gereklidir. Sütun eşleme **, "Structure" kaynak veri kümesindeki sütunların tüm veya alt kümelerinin havuz veri kümesi "Structure" içindeki tüm sütunlara eşlenmesinin**kullanılmasını destekler. Aşağıda bir özel durumla sonuçlanan hata koşulları verilmiştir:

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

Aşağıdaki JSON bir işlem hattındaki kopyalama etkinliğini tanımlar. **Translator** -> **ColumnMappings** özelliği kullanılarak, havuzdaki sütunlara eşlenen kaynak sütunları.

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

Sütun eşlemesini belirtmek için `"columnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"` söz dizimini kullanıyorsanız, hala olduğu gibi desteklenir.

### <a name="alternative-schema-mapping"></a>Alternatif şema eşleme

MongoDB/REST 'den metin dosyasına kopyalama ve Azure Cosmos DB Oracle 'dan MongoDB için ' a kopyalamak üzere kopyalama etkinliği-> `translator` -> `schemaMapping`. Kopyalama etkinliği `translator` bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği çeviricisinin Type özelliği: **Tabulartranslator** olarak ayarlanmalıdır | Yes |
| schemaMapping | **Kaynak taraftan havuz tarafına**olan eşleme ilişkisini temsil eden anahtar-değer çiftleri koleksiyonu.<br/>- **anahtar:** kaynağı temsil eder. **Tablo kaynağı**için, sütun adını veri kümesi yapısında tanımlanan şekilde belirtin; **hiyerarşik kaynak**için, Ayıklanacak ve eşlenecek her alan için JSON yolu ifadesini belirtin.<br>- **değeri:** havuzu temsil eder. **Tablo havuzu**için, sütun adını veri kümesi yapısında tanımlanan şekilde belirtin; **hiyerarşik havuz**için, Ayıklanacak ve eşlenecek her alan için JSON yolu ifadesini belirtin. <br>Hiyerarşik veriler söz konusu olduğunda, kök nesne altındaki alanlar için JSON yolu kök $; ile başlar `collectionReference` özelliği tarafından seçilen dizi içindeki alanlar için JSON yolu dizi öğesinden başlar.  | Yes |
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

Ayrıca, dizi içindeki verileri *(order_pd ve order_price)* ve ortak kök bilgiyle *(numara, tarih ve şehir)* çapraz birleştirmeyi düzleştirerek, bir Azure SQL tablosuna aşağıdaki biçimde kopyalamak istersiniz:

| Sipariş numarası | OrderDate | order_pd | order_price | city |
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
                "$.number": "orderNumber",
                "$.date": "orderDate",
                "prod": "order_pd",
                "price": "order_price",
                "$.city[0].name": "city"
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

Data Factory aşağıdaki geçici veri türlerini destekler: [veri kümesi yapısı](concepts-datasets-linked-services.md#dataset-structure-or-schema) yapılandırmasında tür bilgilerini yapılandırırken aşağıdaki değerleri belirtebilirsiniz:

* Byte[]
* Boole
* Tarih saat
* Datetimeoffset
* Ondalık
* çift
* Guid
* Int16
* Int32
* Int64
* Tek
* Dize
* Timespan

## <a name="next-steps"></a>Sonraki adımlar
Bir kopyalama etkinliği makalelere bakın:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
