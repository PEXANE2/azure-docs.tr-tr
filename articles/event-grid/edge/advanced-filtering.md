---
title: Gelişmiş filtreleme - Azure Olay Izgara IoT Edge | Microsoft Dokümanlar
description: IoT Edge'deki Event Grid'de gelişmiş filtreleme.
author: HiteshMadan
manager: rajarv
ms.author: himad
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: d7fdc5074f3c92eea4f236a9b1f7c823b930f391
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72992567"
---
# <a name="advanced-filtering"></a>Gelişmiş filtreleme
Olay Grid json yükü herhangi bir özellik üzerinde filtreler belirtme sağlar. Bu filtreler, her dış `AND` koşulun isteğe bağlı iç `OR` koşullara sahip olduğu koşullar kümesi olarak modellenir. Her `AND` koşul için aşağıdaki değerleri belirtirsiniz:

* `OperatorType`- Karşılaştırma türü.
* `Key`- Filtreyi uygulayacağınız özelliğe json yolu.
* `Value`- Filtrenin çalıştırıldığı başvuru değeri (veya) `Values` - Filtrenin çalıştırıldığı referans değerleri kümesi.

## <a name="json-syntax"></a>JSON sözdizimi

Gelişmiş bir filtre için JSON sözdizimi aşağıdaki gibidir:

```json
{
    "filter": {
        "advancedFilters": [{
                "operatorType": "NumberGreaterThanOrEquals",
                "key": "Data.Key1",
                "value": 5
            }, {
                "operatorType": "StringContains",
                "key": "Subject",
                "values": ["container1", "container2"]
            }
        ]
    }
}
```

## <a name="filtering-on-array-values"></a>Dizi değerlerine filtreleme

Olay Grid bugün bir dizi değerüzerinde filtreleme desteklemez. Gelen bir olay gelişmiş filtre anahtarı için bir dizi değeri varsa, eşleşen işlem başarısız olur. Gelen olay, olay aboneliğiyle eşleşmez.

## <a name="and-or-not-semantics"></a>VE-OR-DEĞİL semantik

Daha önce verilen json örneğinde bir `AdvancedFilters` dizi olduğuna dikkat edin. Her `AdvancedFilter` dizi öğesini `AND` bir koşul olarak düşünün.

Birden çok değeri (, , `NumberIn` `NumberNotIn`, `StringIn`vb.) destekleyen işleçler `OR` için, her değer bir koşul olarak kabul edilir. Yani, `StringBeginsWith("a", "b", "c")` bir ya `a` da ya `b` `c`da ile başlayan herhangi bir dize değeri eşleşecektir.

> [!CAUTION]
> NOT operatörleri `NumberNotIn` - `StringNotIn` ve `Values` sahada verilen her değer üzerinde AND koşulları olarak şekilde.
>
> Bunu yapmamak filtreyi Tümlerini Kabul Et filtresi yapar ve filtreleme amacını yenecektir.

## <a name="floating-point-rounding-behavior"></a>Kayan nokta yuvarlama davranışı

Olay Grid `decimal` tüm sayısal değerleri işlemek için .NET türünü kullanır. JSON aboneliğinde belirtilen sayı değerleri kayan nokta yuvarlama davranışına tabi değildir.

## <a name="case-sensitivity-of-string-filters"></a>Dize filtrelerinin büyük/küçük harf duyarlılığı

Tüm dize karşılaştırmaları büyük/küçük harf duyarsızdır. Bugün bu davranışı değiştirmenin bir yolu yok.

## <a name="allowed-advanced-filter-keys"></a>İzin verilen gelişmiş filtre tuşları

Özellik `Key` ya iyi bilinen bir üst düzey özellik olabilir veya her nokta iç içe json nesnesine adım anlamına gelen birden çok nokta ile bir json yolu olabilir.

Olay Izgara'nın JSONPath `$` belirtiminin aksine, Anahtar'daki karakter için özel bir anlamı yoktur.

### <a name="event-grid-schema"></a>Olay ızgara şeması

Olay Izgara şemasındaki olaylar için:

* Kimlik
* Konu başlığı
* Özne
* Olay türü
* DataVersion
* Data.Prop1
* Data.Prop*Prop2.Prop3.Prop4.Prop5

### <a name="custom-event-schema"></a>Özel olay şeması

Olay Izgara yük `Key` üzerinde herhangi bir zarf şema zorlamaz beri özel olay şema üzerinde herhangi bir kısıtlama yoktur.

## <a name="numeric-single-value-filter-examples"></a>Sayısal tek değerli filtre örnekleri

* NumberGreaterthan
* NumberGreaterThanOrEquals
* Sayısız
* NumberLessThanOrEquals

```json
{
    "filter": {
        "advancedFilters": [
            {
                "operatorType": "NumberGreaterThan",
                "key": "Data.Key1",
                "value": 5
            },
            {
                "operatorType": "NumberGreaterThanOrEquals",
                "key": "Data.Key2",
                "value": *456
            },
            {
                "operatorType": "NumberLessThan",
                "key": "Data.P*P2.P3",
                "value": 1000
            },
            {
                "operatorType": "NumberLessThanOrEquals",
                "key": "Data.P*P2",
                "value": 999
            }
        ]
    }
}
```

## <a name="numeric-range-value-filter-examples"></a>Sayısal aralık değeri filtresi örnekleri

* NumberIn
* NumberNotin

```json
{
    "filter": {
        "advancedFilters": [
            {
                "operatorType": "NumberIn",
                "key": "Data.Key1",
                "values": [1, 10, 100]
            },
            {
                "operatorType": "NumberNotIn",
                "key": "Data.Key2",
                "values": [2, 3, 4.56]
            }
        ]
    }
}
```

## <a name="string-range-value-filter-examples"></a>String aralığı değeri filtresi örnekleri

* StringContains
* StringBeginsWith
* StringEndsWith
* Stringin
* StringNotin

```json
{
    "filter": {
        "advancedFilters": [
            {
                "operatorType": "StringContains",
                "key": "Data.Key1",
                "values": ["microsoft", "azure"]
            },
            {
                "operatorType": "StringBeginsWith",
                "key": "Data.Key2",
                "values": ["event", "grid"]
            },
            {
                "operatorType": "StringEndsWith",
                "key": "Data.P3.P4",
                "values": ["jpg", "jpeg", "png"]
            },
            {
                "operatorType": "StringIn",
                "key": "RootKey",
                "values": ["exact", "string", "matches"]
            },
            {
                "operatorType": "StringNotIn",
                "key": "RootKey",
                "values": ["aws", "bridge"]
            }
        ]
    }
}
```

## <a name="boolean-single-value-filter-examples"></a>Boolean tek değerli filtre örnekleri

* BoolEquals

```json
{
    "filter": {
        "advancedFilters": [
            {
                "operatorType": "BoolEquals",
                "key": "BoolKey1",
                "value": true
            },
            {
                "operatorType": "BoolEquals",
                "key": "BoolKey2",
                "value": false
            }
        ]
    }
}
```
