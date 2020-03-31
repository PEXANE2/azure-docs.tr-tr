---
title: Kopyalama etkinliğinde şema eşleme
description: Azure Veri Fabrikası'ndaki kopyalama etkinliğinin, verileri kopyalarken kaynak verilerden verileri batırmaya kadar şemaları ve veri türlerini nasıl eşler olduğunu öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260819"
---
# <a name="schema-mapping-in-copy-activity"></a>Kopyalama etkinliğinde şema eşleme

Bu makalede, Azure Veri Fabrikası kopyalama etkinliğinin, veri kopyasını çalıştırırken kaynak verilerden verileri batırmaya kadar şema eşleme ve veri türü eşlemenasıl yaptığı açıklanmaktadır.

## <a name="schema-mapping"></a>Şema haritalama

Sütun eşleme, verileri kaynaktan batmaya kopyalarken uygulanır. Varsayılan olarak, **sütun adlarına göre batmak için etkinlik eşlemi kaynak verilerini**kopyalayın. Gereksiniminize göre sütun eşlemi özelleştirmek için [açık eşleme](#explicit-mapping) belirtebilirsiniz. Daha ayrıntılı olarak, kopyalama etkinliği:

1. Verileri kaynaktan okuyun ve kaynak şemasını belirleyin
2. Sütunları ada göre eşleştirmek için varsayılan sütun eşlemini kullanın veya belirtilirse açık sütun eşlemi uygulayın.
3. Verileri batmak için yazın

### <a name="explicit-mapping"></a>Açık eşleme

Kopyalama etkinliği -> `translator`  ->  `mappings` özelliğinde eşlenecek sütunları belirtebilirsiniz. Aşağıdaki örnek, sınırlı metinden Azure SQL Veritabanı'na veri kopyalamak için bir ardışık ardışık ardışık etkinlikte bir kopyalama etkinliği tanımlar.

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

Aşağıdaki özellikler -> `translator`  ->  `mappings` nesnesi `source` altında `sink`desteklenir ve:

| Özellik | Açıklama                                                  | Gerekli |
| -------- | ------------------------------------------------------------ | -------- |
| ad     | Kaynak veya lavabo sütununun adı.                           | Evet      |
| Sıralı  | Sütun dizini. 1 ile başla. <br>Üstbilgi satırı olmadan sınırlı metin kullanırken uygulayın ve gerekli. | Hayır       |
| yol     | Ayıklamak veya eşlemek için her alan için JSON yol ifadesi. MongoDB/REST gibi hiyerarşik veriler için başvurun.<br>Kök nesnesi altındaki alanlar için JSON yolu $köküyle başlar; özellik tarafından `collectionReference` seçilen dizi içindeki alanlar için, JSON yolu dizi öğesinden başlar. | Hayır       |
| type     | Kaynak veya lavabo sütununun Veri Fabrikası geçici veri türü. | Hayır       |
| kültür  | Kaynak veya lavabo sütununun kültürü. <br>Türü veya `Datetime` `Datetimeoffset`' olduğunda uygulayın Varsayılan değer: `en-us`. | Hayır       |
| biçim   | Biçim dizesi, tür `Datetime` `Datetimeoffset`veya . olduğunda kullanılacak Datetime'ın nasıl biçimlendirilenözel [Tarih ve Saat Biçim Dizeleri'ne](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) bakın. | Hayır       |

Aşağıdaki özellikler ile nesne `translator`  ->  `mappings` ek olarak `source` desteklenir ve: `sink`

| Özellik            | Açıklama                                                  | Gerekli |
| ------------------- | ------------------------------------------------------------ | -------- |
| koleksiyonReferans | Yalnızca MongoDB/REST gibi hiyerarşik veriler kaynak olduğunda desteklenir.<br>Aynı desenle **bir dizi alanı içindeki** nesnelerden verileri yinelemek ve ayıklamak ve nesne başına satır başına dönüştürmek istiyorsanız, çapraz uygulama yapmak için bu dizinin JSON yolunu belirtin. | Hayır       |

### <a name="alternative-column-mapping"></a>Alternatif sütun eşleme

Tabular şekilli `translator`  ->  `columnMappings` veriler arasında eşlenebilmek için kopyalama etkinliği -> belirtebilirsiniz. Bu durumda, hem giriş hem de çıktı veri kümeleri için "yapı" bölümü gereklidir. Sütun eşleme, **kaynak veri kümesindeki "yapı"daki sütunların tümünün veya alt kümesinin lavabo veri kümesindeki "yapı" tüm sütunlarına eşlemi**sağlar. Bir özel durumla sonuçlanan hata koşulları şunlardır:

* Kaynak veri deposu sorgu sonucu giriş veri kümesi "yapı" bölümünde belirtilen bir sütun adı yok.
* Lavabo veri deposu (önceden tanımlanmış şema ile) çıktı veri kümesi "yapı" bölümünde belirtilen bir sütun adı yoksa.
* Lavabo veri kümesinin "yapısında" eşlemede belirtilenden daha az sütun veya daha fazla sütun.
* Yinelenen eşleme.

Aşağıdaki örnekte, giriş veri kümesinin bir yapısı vardır ve şirket içi Oracle veritabanındaki bir tabloyu işaret ediyor.

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

Bu örnekte, çıktı veri kümesi bir yapıya sahiptir ve Salesfoce'daki bir tabloyu işaret ediyor.

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

Aşağıdaki JSON, bir ardışık ardışık ardışık ardışık alanda bir kopyalama etkinliği tanımlar. Kaynaktan sütunlar **çevirmen** -> **sütunMappings** özelliği kullanılarak lavaboda sütunlar eşlenir.

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

Sütun eşlemini belirtmek `"columnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"` için sözdizimini kullanıyorsanız, yine de olduğu gibi desteklenir.

### <a name="alternative-schema-mapping"></a>Alternatif şema haritalama

Hiyerarşik şekilli veriler `translator`  ->  `schemaMapping` ve tabular şekilli veriler arasında eşleştirmek için kopyalama etkinliği -> belirtebilirsiniz(örneğin, MongoDB/REST'ten metin dosyasına kopya ve Oracle'dan Azure Cosmos DB'nin MongoDB için API'sine kopya. Aşağıdaki özellikler kopyalama etkinliği `translator` bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği çevirmeninin tür özelliği şu şekilde ayarlanmalıdır: **TabularTranslator** | Evet |
| şemaHaritalama | **Kaynak taraftan lavabo tarafına**eşleme ilişkisini temsil eden anahtar değer çiftleri topluluğu.<br/>- **Anahtar:** kaynağı temsil eder. **Tabular kaynak**için, veri kümesi yapısında tanımlandığı şekilde sütun adını belirtin; **hiyerarşik kaynak**için, ayıklamak ve eşlemek için her alan için JSON yol ifadesini belirtin.<br>- **Değer:** lavabo temsil eder. **Tabular lavabo**için, veri kümesi yapısında tanımlandığı şekilde sütun adını belirtin; **hiyerarşik lavabo**için, ayıklamak ve eşlemek için her alan için JSON yol ifadesini belirtin. <br>Hiyerarşik veriler söz konusu olduğunda, kök nesnealtındaki alanlar için JSON yolu kök $ile başlar; özellik tarafından `collectionReference` seçilen dizi içindeki alanlar için, JSON yolu dizi öğesinden başlar.  | Evet |
| koleksiyonReferans | Aynı desenle **bir dizi alanı içindeki** nesnelerden verileri yinelemek ve ayıklamak ve nesne başına satır başına dönüştürmek istiyorsanız, çapraz uygulama yapmak için bu dizinin JSON yolunu belirtin. Bu özellik yalnızca hiyerarşik veriler kaynak olduğunda desteklenir. | Hayır |

**Örnek: MongoDB'den Oracle'a kopya:**

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

ve dizi içindeki verileri *düzleştirmek (order_pd ve order_price)* ve ortak kök *bilgileriyle (sayı, tarih ve şehir)* çapraz birleştirme yaparak aşağıdaki biçimde bir Azure SQL tablosuna kopyalamak istiyorsunuz:

| Ordernumber | Sipariştarihi | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | Seattle |
| 01 | 20170122 | P2 | 13 | Seattle |
| 01 | 20170122 | P3 | 231 | Seattle |

Şema eşleme kuralını aşağıdaki kopya etkinliği JSON örneği olarak yapılandırın:

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

## <a name="data-type-mapping"></a>Veri türü eşlemesi

Kopyalama etkinliği, aşağıdaki 2 adımlı yaklaşımla bağme türleri eşlemesine kaynak türleri gerçekleştirir:

1. Yerel kaynak türlerinden Azure Veri Fabrikası geçici veri türlerine dönüştürme
2. Azure Veri Fabrikası geçici veri türlerinden yerel lavabo türüne dönüştürme

Her bağlayıcı konusundaki "Veri türü eşleme" bölümünde yerel türile geçici tür arasındaki eşlemi bulabilirsiniz.

### <a name="supported-data-types"></a>Desteklenen veri türleri

Veri Fabrikası aşağıdaki geçici veri türlerini destekler: Veri kümesi [yapısı](concepts-datasets-linked-services.md#dataset-structure-or-schema) yapılandırmasında tür bilgilerini yapılandırırken aşağıdaki değerleri belirtebilirsiniz:

* Bayt[]
* Boole
* Tarih saat
* Datetimeoffset
* Ondalık
* Çift
* Guid
* Int16
* Int32
* Int64
* Tek
* Dize
* Timespan

## <a name="next-steps"></a>Sonraki adımlar
Diğer Kopyalama Etkinliği makalelerini görün:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
