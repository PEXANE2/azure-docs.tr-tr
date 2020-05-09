---
title: Azure Event Grid için olay filtreleme
description: Azure Event Grid aboneliği oluştururken olayların nasıl filtreleneceğini açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: spelluru
ms.openlocfilehash: 0f503b21d5a7d0fdfbee79354c198775789c0b91
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82888776"
---
# <a name="understand-event-filtering-for-event-grid-subscriptions"></a>Event Grid abonelikleri için olay filtrelemeyi anlayın

Bu makalede, hangi olayların uç noktanıza gönderileceğini filtrelemeye yönelik farklı yollar açıklanmaktadır. Bir olay aboneliği oluştururken filtreleme için üç seçeneğiniz vardır:

* Olay türleri
* Konu şununla başlıyor veya şununla bitiyor
* Gelişmiş alanlar ve işleçler

## <a name="event-type-filtering"></a>Olay türü filtreleme

Varsayılan olarak, olay kaynağı için tüm [olay türleri](event-schema.md) uç noktaya gönderilir. Uç noktanıza yalnızca belirli olay türlerini gönderilmeye karar verebilirsiniz. Örneğin, kaynaklarınız için güncelleştirmeler hakkında bildirim alabilir, ancak silme işlemleri gibi diğer işlemlere yönelik bilgilendirilirsiniz. Bu durumda, `Microsoft.Resources.ResourceWriteSuccess` olay türüne göre filtreleyin. Olay türlerine sahip bir dizi sağlayın veya olay kaynağı için `All` tüm olay türlerini almak üzere belirtin.

Olay türüne göre filtreleme için JSON sözdizimi şöyledir:

```json
"filter": {
  "includedEventTypes": [
    "Microsoft.Resources.ResourceWriteFailure",
    "Microsoft.Resources.ResourceWriteSuccess"
  ]
}
```

## <a name="subject-filtering"></a>Konu filtreleme

Konuya göre basit filtreleme için, konu için bir başlangıç veya bitiş değeri belirtin. Örneğin, konunun bittiğini yalnızca bir metin dosyasını depolama hesabına `.txt` yüklemeyle ilgili olayları almak üzere belirtebilirsiniz. Ya da, o kapsayıcının tüm olaylarını almak için `/blobServices/default/containers/testcontainer` , depolama hesabındaki diğer kapsayıcıları değil, konunun ile başlayan bir filtre uygulayabilirsiniz.

Olayları özel konulara yayımlarken, etkinliklerinizin etkinlikleri oluşturun ve bu da abonelerin olayla ilgilenip ilgilenmediğini bilmesini kolaylaştırır. Aboneler olayları filtrelemek ve yönlendirmek için Subject özelliğini kullanır. Örneğin, abonelerin bu yolun segmentlerine göre filtreleyebilmesi için olayın gerçekleştiği yolu eklemeyi göz önünde bulundurun. Yol, abonelerin olayları en dar veya genel olarak filtrelemesine olanak sağlar. Konu `/A/B/C` içinde üç segment yolu sağlarsanız, aboneler çok sayıda olayı almak için ilk kesime `/A` göre filtreleyebilirsiniz. Bu aboneler, veya `/A/B/C` `/A/D/E`gibi konularla olayları alır. Diğer aboneler, daha dar `/A/B` bir olay kümesi almak için tarafından filtreleyebilirler.

Konuya göre filtrelemeye yönelik JSON sözdizimi şöyledir:

```json
"filter": {
  "subjectBeginsWith": "/blobServices/default/containers/mycontainer/log",
  "subjectEndsWith": ".jpg"
}

```

## <a name="advanced-filtering"></a>Gelişmiş filtreleme

Veri alanlarındaki değerlere göre filtrelemek ve karşılaştırma işlecini belirtmek için Gelişmiş filtreleme seçeneğini kullanın. Gelişmiş filtreleme ' de şunları belirtirsiniz:

* işleç türü-karşılaştırma türü.
* anahtar-filtreleme için kullanmakta olduğunuz olay verileri alanı. Bir sayı, Boolean veya dize olabilir.
* Values-anahtarla karşılaştırılacak değer veya değerler.

Birden çok değer içeren tek bir filtre belirtirseniz, **ya** da bir işlem gerçekleştirilir, bu nedenle anahtar alanının değeri bu değerlerden biri olmalıdır. Örnek aşağıda verilmiştir:

```json
"advancedFilters": [
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/microsoft.devtestlab/",
            "/providers/Microsoft.Compute/virtualMachines/"
        ]
    }
]
```

Birden çok farklı filtre belirtirseniz, **ve** bir işlem gerçekleştirilir, bu nedenle her filtre koşulu karşılanmalıdır. Örnek aşağıda verilmiştir: 

```json
"advancedFilters": [
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/microsoft.devtestlab/"
        ]
    },
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/Microsoft.Compute/virtualMachines/"
        ]
    }
]
```

### <a name="operators"></a>İşleçler

**Rakamlar** için kullanılabilir işleçler şunlardır:

* NumberGreaterThan
* NumberGreaterThanOrEquals
* NumberLessThan
* Numberlessals
* Numberın
* Numbernotın

**Boole değerleri** için kullanılabilir işleç: 
- BoolEquals

**Dizeler** için kullanılabilir işleçler şunlardır:

* StringContains
* StringBeginsWith
* StringEndsWith
* Stringın
* Stringnotın

Tüm dize karşılaştırmaları büyük/küçük harfe duyarlı **değildir** .

### <a name="key"></a>Anahtar

Event Grid şemasındaki olaylar için, anahtar için aşağıdaki değerleri kullanın:

* Kimlik
* Konu başlığı
* Özne
* Olay türü
* Veri sürümü
* Olay verileri (Data. KEY1 gibi)

Bulut olayları şemasındaki olaylar için, anahtar için aşağıdaki değerleri kullanın:

* Even
* Kaynak
* Olay türü
* EventTypeVersion
* Olay verileri (Data. KEY1 gibi)

Özel giriş şeması için, olay verileri alanlarını (Data. KEY1 gibi) kullanın.

### <a name="values"></a>Değerler

Değerler şu şekilde olabilir:

* number
* string
* boole
* array

### <a name="limitations"></a>Sınırlamalar

Gelişmiş filtreleme aşağıdaki sınırlamalara sahiptir:

* Olay Kılavuzu aboneliği başına beş gelişmiş filtre
* dize değeri başına 512 karakter
* **İçindeki** ve **Not** işleçleri için beş değer
* (Nokta) karakterleriyle anahtarlar. ** `.` ** Örneğin: `http://schemas.microsoft.com/claims/authnclassreference` veya `john.doe@contoso.com`. Şu anda Anahtarlar içinde kaçış karakterleri için destek yoktur. 

Aynı anahtar birden fazla filtrede kullanılabilir.

### <a name="examples"></a>Örnekler

### <a name="stringcontains"></a>StringContains

```json
"advancedFilters": [{
    "operatorType": "StringContains",
    "key": "data.key1",
    "values": [
        "microsoft", 
        "azure"
    ]
}]
```

### <a name="stringbeginswith"></a>StringBeginsWith

```json
"advancedFilters": [{
    "operatorType": "StringBeginsWith",
    "key": "data.key1",
    "values": [
        "event", 
        "grid"
    ]
}]
```

### <a name="stringendswith"></a>StringEndsWith

```json
"advancedFilters": [{
    "operatorType": "StringEndsWith",
    "key": "data.key1",
    "values": [
        "jpg", 
        "jpeg", 
        "png"
    ]
}]
```

### <a name="stringin"></a>Stringın

```json
"advancedFilters": [{
    "operatorType": "StringIn",
    "key": "data.key1",
    "values": [
        "exact", 
        "string", 
        "matches"
    ]
}]
```

### <a name="stringnotin"></a>Stringnotın

```json
"advancedFilters": [{
    "operatorType": "StringNotIn",
    "key": "data.key1",
    "values": [
        "aws", 
        "bridge"
    ]
}]
```

### <a name="numberin"></a>Numberın

```json

"advancedFilters": [{
    "operatorType": "NumberIn",
    "key": "data.counter",
    "values": [
        5,
        1
    ]
}]

```

### <a name="numbernotin"></a>Numbernotın

```json
"advancedFilters": [{
    "operatorType": "NumberNotIn",
    "key": "data.counter",
    "values": [
        41,
        0,
        0
    ]
}]
```

### <a name="numberlessthan"></a>NumberLessThan

```json
"advancedFilters": [{
    "operatorType": "NumberLessThan",
    "key": "data.counter",
    "value": 100
}]
```

### <a name="numbergreaterthan"></a>NumberGreaterThan

```json
"advancedFilters": [{
    "operatorType": "NumberGreaterThan",
    "key": "data.counter",
    "value": 20
}]
```

### <a name="numberlessthanorequals"></a>Numberlessals

```json
"advancedFilters": [{
    "operatorType": "NumberLessThanOrEquals",
    "key": "data.counter",
    "value": 100
}]
```

### <a name="numbergreaterthanorequals"></a>NumberGreaterThanOrEquals

```json
"advancedFilters": [{
    "operatorType": "NumberGreaterThanOrEquals",
    "key": "data.counter",
    "value": 30
}]
```

### <a name="boolequals"></a>BoolEquals

```json
"advancedFilters": [{
    "operatorType": "BoolEquals",
    "key": "data.isEnabled",
    "value": true
}]
```


## <a name="next-steps"></a>Sonraki adımlar

* PowerShell ve Azure CLı ile olayları filtreleme hakkında bilgi edinmek için bkz. [Event Grid olayları filtreleme](how-to-filter-events.md).
* Event Grid kullanmaya hızlıca başlamak için bkz. [özel olayları oluşturma ve Azure Event Grid ile yönlendirme](custom-event-quickstart.md).
