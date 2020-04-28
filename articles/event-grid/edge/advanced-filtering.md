---
title: Gelişmiş filtreleme-Azure Event Grid IoT Edge | Microsoft Docs
description: IoT Edge Event Grid Gelişmiş filtreleme.
author: HiteshMadan
manager: rajarv
ms.author: himad
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: d7fdc5074f3c92eea4f236a9b1f7c823b930f391
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "72992567"
---
# <a name="advanced-filtering"></a>Gelişmiş filtreleme
Event Grid, JSON yükünde herhangi bir özellikte filtre belirtilmesine izin verir. Bu filtreler, her bir dış koşulun `AND` isteğe bağlı iç `OR` koşullara sahip olduğu koşullar kümesi olarak modellenir. Her `AND` koşul için aşağıdaki değerleri belirtirsiniz:

* `OperatorType`-Karşılaştırma türü.
* `Key`-Filtrenin uygulanacağı özelliğin JSON yolu.
* `Value`-Filtrenin çalıştırıldığı başvuru değeri (veya) `Values` -filtrenin çalıştırıldığı başvuru değerleri kümesi.

## <a name="json-syntax"></a>JSON sözdizimi

Gelişmiş bir filtrenin JSON sözdizimi aşağıdaki gibidir:

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

## <a name="filtering-on-array-values"></a>Dizi değerlerinde filtreleme

Event Grid, bugün bir değer dizisinde filtrelemeyi desteklemez. Gelen bir olayda gelişmiş filtre anahtarı için bir dizi değeri varsa, eşleşen işlem başarısız olur. Gelen olay, olay aboneliğiyle eşleşmeyen bir şekilde sona erer.

## <a name="and-or-not-semantics"></a>AND-OR-semantiğini

Daha önce verilen JSON örneğinde bir dizi `AdvancedFilters` olduğunu unutmayın. Her `AdvancedFilter` dizi öğesini bir `AND` koşul olarak düşünün.

Birden çok değeri destekleyen `NumberIn`işleçler (örneğin `NumberNotIn` `StringIn`,, vb.) için, her bir değer bir `OR` koşul olarak değerlendirilir. Bu nedenle, `StringBeginsWith("a", "b", "c")` ya da `a` `b` veya `c`ile başlayan herhangi bir dize değeriyle eşleşir.

> [!CAUTION]
> NOT işleci değildir `NumberNotIn` ve `StringNotIn` `Values` alanında verilen her bir değer için ve koşulları gibi davranır.
>
> Bunu yapmamaları, filtrenin bir kabul etme filtresi olmasını ve filtreleme amacını ertelemesini ister.

## <a name="floating-point-rounding-behavior"></a>Kayan nokta yuvarlama davranışı

Event Grid, `decimal` tüm sayısal değerleri işlemek için .NET türünü kullanır. Olay aboneliği JSON öğesinde belirtilen sayı değerleri kayan nokta yuvarlama davranışına tabi değil.

## <a name="case-sensitivity-of-string-filters"></a>Dize filtrelerinin büyük/küçük harf duyarlılığı

Tüm dize karşılaştırmaları büyük/küçük harfe duyarlıdır. Bu davranışı bugün değiştiremenin bir yolu yoktur.

## <a name="allowed-advanced-filter-keys"></a>İzin verilen gelişmiş filtre anahtarları

`Key` Özelliği iyi bilinen bir en üst düzey özellik olabilir veya birden çok noktalı bir JSON yolu olabilir, burada her nokta iç içe geçmiş bir JSON nesnesine adımlamayı belirtir.

Event Grid, JSONPath belirtiminin aksine, anahtardaki `$` karakter için özel anlamı yoktur.

### <a name="event-grid-schema"></a>Olay Kılavuzu şeması

Event Grid şemasındaki olaylar için:

* Kimlik
* Konu başlığı
* Özne
* Olay türü
* Veri sürümü
* Data. Prop1
* Data. prop * Prop2. Prop3. Prop4. Prop5

### <a name="custom-event-schema"></a>Özel olay şeması

Event Grid, `Key` yük üzerinde herhangi bir zarf şemasını zorlayamasından dolayı özel olay şemasında kısıtlama yoktur.

## <a name="numeric-single-value-filter-examples"></a>Sayısal tek değerli filtre örnekleri

* NumberGreaterThan
* NumberGreaterThanOrEquals
* NumberLessThan
* Numberlessals

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

## <a name="numeric-range-value-filter-examples"></a>Sayısal Aralık-değer Filtre örnekleri

* Numberın
* Numbernotın

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

## <a name="string-range-value-filter-examples"></a>Dize aralığı-değer Filtre örnekleri

* StringContains
* StringBeginsWith
* StringEndsWith
* Stringın
* Stringnotın

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
