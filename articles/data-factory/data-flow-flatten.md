---
title: Veri akışını haritalamada düzdönüşüm
description: Düzleştirme dönüşümlerini kullanarak hiyerarşik verileri normalden arındır
author: kromerm
ms.author: makromer
ms.review: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: b19aae8ab6730936a826f5bb069bfdb7d696cdfa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246645"
---
# <a name="flatten-transformation-in-mapping-data-flow"></a>Veri akışını haritalamada düzdönüşüm

JSON gibi hiyerarşik yapıların içindeki dizi değerlerini almak ve bunları tek tek satırlara dönüştürmek için düzleştir dönüşümlerini kullanın. Bu işlem denormalization olarak bilinir.

## <a name="configuration"></a>Yapılandırma

Düzleştirmek dönüşümü aşağıdaki yapılandırma ayarlarını içerir

![Ayarları düzleştirmek](media/data-flow/flatten1.png "Ayarları düzleştirmek")

### <a name="unroll-by"></a>Unroll tarafından

Unroll için bir dizi seçin. Çıktı verilerinin her dizideki öğe başına bir satırı olur. Giriş satırındaki dizi tarafından unroll null veya boş ise, null olarak unrolled değerleri ile bir çıkış satırı olacaktır.

### <a name="unroll-root"></a>Kökünü aç

Varsayılan olarak, düzleme dönüştürme, bir diziyi içinde bulunduğu hiyerarşinin en üstüne alır. İsteğe bağlı olarak unroll kök olarak bir dizi seçebilirsiniz. Unroll kökü, dizitarafından unroll'u içeren veya karmaşık nesnelerden oluşan bir dizi olmalıdır. Unroll kökü seçilirse, çıktı verileri unroll kökündeki öğeler başına en az bir satır içerir. Giriş satırıun unroll kökünde herhangi bir öğe yoksa, çıktı verilerinden bırakılır. Unroll kök seçimi her zaman varsayılan davranış daha az veya eşit sayıda satır çıktı olacaktır.

### <a name="flatten-mapping"></a>Düzleme haritalama

Seçili dönüşüme benzer şekilde, gelen alanlardan yeni yapının projeksiyonunu ve normalden arındırılmış diziyi seçin. Normalden arındırılmış bir dizi eşlenirse, çıktı sütunu diziyle aynı veri türünde olur. Dizi tarafından unroll alt dizileri içeren karmaşık nesnelerin bir dizi ise, bu subarry bir öğe eşleme bir dizi çıktı olacaktır.

Eşleme çıktınızı doğrulamak için denetim sekmesine ve veri önizlemesine bakın.

## <a name="examples"></a>Örnekler

Düzleştirmek dönüşümüne aşağıdaki örnekler için aşağıdaki JSON nesnesine bakın

``` json
{
  "name":"MSFT","location":"Redmond", "satellites": ["Bay Area", "Shanghai"],
  "goods": {
    "trade":true, "customers":["government", "distributer", "retail"],
    "orders":[
        {"orderId":1,"orderTotal":123.34,"shipped":{"orderItems":[{"itemName":"Laptop","itemQty":20},{"itemName":"Charger","itemQty":2}]}},
        {"orderId":2,"orderTotal":323.34,"shipped":{"orderItems":[{"itemName":"Mice","itemQty":2},{"itemName":"Keyboard","itemQty":1}]}}
    ]}}
{"name":"Company1","location":"Seattle", "satellites": ["New York"],
  "goods":{"trade":false, "customers":["store1", "store2"],
  "orders":[
      {"orderId":4,"orderTotal":123.34,"shipped":{"orderItems":[{"itemName":"Laptop","itemQty":20},{"itemName":"Charger","itemQty":3}]}},
      {"orderId":5,"orderTotal":343.24,"shipped":{"orderItems":[{"itemName":"Chair","itemQty":4},{"itemName":"Lamp","itemQty":2}]}}
    ]}}
{"name": "Company2", "location": "Bellevue",
  "goods": {"trade": true, "customers":["Bank"], "orders": [{"orderId": 4, "orderTotal": 123.34}]}}
{"name": "Company3", "location": "Kirkland"}
```

### <a name="no-unroll-root-with-string-array"></a>Dize dizisi ile unroll kökü yok

| Unroll tarafından | Kökünü aç | Yansıtma |
| --------- | ----------- | ---------- |
| goods.customers | None | ad <br> customer = goods.customer |

#### <a name="output"></a>Çıktı

```
{ 'MSFT', 'government'}
{ 'MSFT', 'distributer'}
{ 'MSFT', 'retail'}
{ 'Company1', 'store'}
{ 'Company1', 'store2'}
{ 'Company2', 'Bank'}
{ 'Company3', null}
```

### <a name="no-unroll-root-with-complex-array"></a>Karmaşık dizili unroll kökü yok

| Unroll tarafından | Kökünü aç | Yansıtma |
| --------- | ----------- | ---------- |
| goods.orders.shipped.orderItems | None | ad <br> orderId = goods.orders.orderId <br> itemName = goods.orders.shipped.orderItems.itemName <br> itemQty = goods.orders.shipped.orderItems.itemQty <br> yer = konum |

#### <a name="output"></a>Çıktı

```
{ 'MSFT', 1, 'Laptop', 20, 'Redmond'}
{ 'MSFT', 1, 'Charger', 2, 'Redmond'}
{ 'MSFT', 2, 'Mice', 2, 'Redmond'}
{ 'MSFT', 2, 'Keyboard', 1, 'Redmond'}
{ 'Company1', 4, 'Laptop', 20, 'Seattle'}
{ 'Company1', 4, 'Charger', 3, 'Seattle'}
{ 'Company1', 5, 'Chair', 4, 'Seattle'}
{ 'Company1', 5, 'Lamp', 2, 'Seattle'}
{ 'Company2', 4, null, null, 'Bellevue'}
{ 'Company3', null, null, null, 'Kirkland'}
```

### <a name="same-root-as-unroll-array"></a>Unroll dizisiyle aynı kök

| Unroll tarafından | Kökünü aç | Yansıtma |
| --------- | ----------- | ---------- |
| goods.orders | goods.orders | ad <br> goods.orders.shipped.orderItems.itemName <br> goods.customers <br> location |

#### <a name="output"></a>Çıktı

```
{ 'MSFT', ['Laptop','Charger'], ['government','distributer','retail'], 'Redmond'}
{ 'MSFT', ['Mice', 'Keyboard'], ['government','distributer','retail'], 'Redmond'}
{ 'Company1', ['Laptop','Charger'], ['store', 'store2'], 'Seattle'}
{ 'Company1', ['Chair', 'Lamp'], ['store', 'store2'], 'Seattle'}
{ 'Company2', null, ['Bank'], 'Bellevue'}
```

### <a name="unroll-root-with-complex-array"></a>Karmaşık diziile kökü aç

| Unroll tarafından | Kökünü aç | Yansıtma |
| --------- | ----------- | ---------- |
| goods.orders.shipped.orderItem | goods.orders |ad <br> orderId = goods.orders.orderId <br> itemName = goods.orders.shipped.orderItems.itemName <br> itemQty = goods.orders.shipped.orderItems.itemQty <br> yer = konum |

#### <a name="output"></a>Çıktı

```
{ 'MSFT', 1, 'Laptop', 20, 'Redmond'}
{ 'MSFT', 1, 'Charger', 2, 'Redmond'}
{ 'MSFT', 2, 'Mice', 2, 'Redmond'}
{ 'MSFT', 2, 'Keyboard', 1, 'Redmond'}
{ 'Company1', 4, 'Laptop', 20, 'Seattle'}
{ 'Company1', 4, 'Charger', 3, 'Seattle'}
{ 'Company1', 5, 'Chair', 4, 'Seattle'}
{ 'Company1', 5, 'Lamp', 2, 'Seattle'}
{ 'Company2', 4, null, null, 'Bellevue'}
```

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<incomingStream>
foldDown(unroll(<unroll cols>),
    mapColumn(
        name,
        each(<array>(type == '<arrayDataType>')),
        each(<array>, match(true())),
        location
    )) ~> <transformationName>
```

### <a name="example"></a>Örnek

```
source foldDown(unroll(goods.orders.shipped.orderItems, goods.orders),
    mapColumn(
        name,
        orderId = goods.orders.orderId,
        itemName = goods.orders.shipped.orderItems.itemName,
        itemQty = goods.orders.shipped.orderItems.itemQty,
        location = location
    ),
    skipDuplicateMapInputs: false,
    skipDuplicateMapOutputs: false) 
```    

## <a name="next-steps"></a>Sonraki adımlar

* Satırları sütunlara döndürmek için [Özet dönüşüm'üne](data-flow-pivot.md) kullanın.
* Sütunları satırlara döndürmek için [Unpivot dönüşümlerini](data-flow-unpivot.md) kullanın.
