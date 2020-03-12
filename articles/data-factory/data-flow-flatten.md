---
title: Eşleme veri akışında dönüştürmeyi düzleştirme
description: Düzleştirme dönüşümünü kullanarak hiyerarşik verileri yeniden Renklendir
author: kromerm
ms.author: makromer
ms.review: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: 74f6df1fbc749a5ec015afb954ca6b12cbe0f18f
ms.sourcegitcommit: 20429bc76342f9d365b1ad9fb8acc390a671d61e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79086950"
---
# <a name="flatten-transformation-in-mapping-data-flow"></a>Eşleme veri akışında dönüştürmeyi düzleştirme

JSON gibi hiyerarşik yapıların içinde dizi değerleri almak için Düzleştir dönüşümünü kullanın ve bunları tek tek satırlara geri alın. Bu işlem, Normalleştirilmemiş olarak bilinir.

## <a name="configuration"></a>Yapılandırma

Düzleştirme dönüştürmesi aşağıdaki yapılandırma ayarlarını içerir

![Ayarları Düzleştir](media/data-flow/flatten1.png "Ayarları Düzleştir")

### <a name="unroll-by"></a>Alma ölçütü

Geri almak için bir dizi seçin. Çıktı verilerinde her dizide öğe başına bir satır olacaktır. Giriş satırındaki diziyi al değeri null veya boş ise, null olarak untoplaedilmemiş değerler içeren bir çıkış satırı olacaktır.

### <a name="unroll-root"></a>Kökü al

Varsayılan olarak, düzet dönüştürmesi, içinde bulunan hiyerarşinin en üstüne bir diziyi kaydeder. İsteğe bağlı olarak, geri alma kökiniz olarak bir dizi seçebilirsiniz. Döngüleri kökü, dizi olmayan ya da içeren bir karmaşık nesneler dizisi olmalıdır. Bir döngüleri kökü seçilirse, çıkış verileri, döngüleri kökündeki her öğe için en az bir satır içerir. Giriş satırında hiç bir öğe yoksa, çıkış verilerinden bırakılır. Bir döngüleri kökünü seçme, her zaman varsayılan davranıştan daha az veya eşit sayıda satır çıktısı alacak.

### <a name="flatten-mapping"></a>Eşlemeyi Düzleştir

Seçim dönüşümüne benzer şekilde, gelen alanlardan yeni yapının projeksiyonunu ve diğer dizi dizisini seçin. Bir Normalleştirilmemiş dizi eşlenmişse, çıkış sütunu dizi ile aynı veri türü olacaktır. Dizi tarafından geri alma, alt diziler içeren karmaşık nesnelerden oluşan bir diziyse, bu alt sınır için bir öğe eşleme bir diziyi çıktı olarak alır.

Eşleme çıktlarınızın doğrulanması için İnceleme sekmesine ve veri önizlemesine bakın.

## <a name="examples"></a>Örnekler

Düzleştirme dönüşümünün aşağıdaki örnekleri için aşağıdaki JSON nesnesine başvurun

``` json
[{
  "name":"MSFT","location":"Redmond", "satellites": ["Bay Area", "Shanghai"],
  "goods": {
    "trade":true, "customers":["government", "distributer", "retail"],
    "orders":[
        {"orderId":1,"orderTotal":123.34,"shipped":{"orderItems":[{"itemName":"Laptop","itemQty":20},{"itemName":"Charger","itemQty":2}]}},
        {"orderId":2,"orderTotal":323.34,"shipped":{"orderItems":[{"itemName":"Mice","itemQty":2},{"itemName":"Keyboard","itemQty":1}]}}
    ]}},
{"name":"Company1","location":"Seattle", "satellites": ["New York"],
  "goods":{"trade":false, "customers":["store1", "store2"],
  "orders":[
      {"orderId":4,"orderTotal":123.34,"shipped":{"orderItems":[{"itemName":"Laptop","itemQty":20},{"itemName":"Charger","itemQty":3}]}},
      {"orderId":5,"orderTotal":343.24,"shipped":{"orderItems":[{"itemName":"Chair","itemQty":4},{"itemName":"Lamp","itemQty":2}]}}
    ]}},
{"name": "Company2", "location": "Bellevue",
  "goods": {"trade": true, "customers":["Bank"], "orders": [{"orderId": 4, "orderTotal": 123.34}]}},
{"name": "Company3", "location": "Kirkland"}]
```

### <a name="no-unroll-root-with-string-array"></a>Dize dizisi ile hiçbir toplama kökü yok

| Alma ölçütü | Kökü al | Yansıtma |
| --------- | ----------- | ---------- |
| mallar. müşteriler | Yok | ad <br> Müşteri = mallar. müşteri |

#### <a name="output"></a>Çıktı

```
{ 'MSFT', 'government'},
{ 'MSFT', 'distributer'},
{ 'MSFT', 'retail'},
{ 'Company1', 'store'},
{ 'Company1', 'store2'},
{ 'Company2', 'Bank'},
{ 'Company3', null}
```

### <a name="no-unroll-root-with-complex-array"></a>Karmaşık dizi ile hiçbir toplama kökü yok

| Alma ölçütü | Kökü al | Yansıtma |
| --------- | ----------- | ---------- |
| mallar. Orders. sevkedildi. OrderItems | Yok | ad <br> OrderID = mal. Orders. OrderID <br> ItemName = mal. Orders. sevkedildi. OrderItems. ItemName <br> ımqty = mallar. Orders. sevkedildi. OrderItems. ıtemqty <br> Konum = Konum |

#### <a name="output"></a>Çıktı

```
{ 'MSFT', 1, 'Laptop', 20, 'Redmond'},
{ 'MSFT', 1, 'Charger', 2, 'Redmond'},
{ 'MSFT', 2, 'Mice', 2, 'Redmond'},
{ 'MSFT', 2, 'Keyboard', 1, 'Redmond'},
{ 'Company1', 4, 'Laptop', 20, 'Seattle'},
{ 'Company1', 4, 'Charger', 3, 'Seattle'},
{ 'Company1', 5, 'Chair', 4, 'Seattle'},
{ 'Company1', 5, 'Lamp', 2, 'Seattle'},
{ 'Company2', 4, null, null, 'Bellevue'},
{ 'Company3', null, null, null, 'Kirkland'}
```

### <a name="same-root-as-unroll-array"></a>Geri alma dizisi olarak aynı kök

| Alma ölçütü | Kökü al | Yansıtma |
| --------- | ----------- | ---------- |
| mallar. siparişler | mallar. siparişler | ad <br> mallar. Orders. sevkedildi. OrderItems. ItemName <br> mallar. müşteriler <br> location |

#### <a name="output"></a>Çıktı

```
{ 'MSFT', ['Laptop','Charger'], ['government','distributer','retail'], 'Redmond'},
{ 'MSFT', ['Mice', 'Keyboard'], ['government','distributer','retail'], 'Redmond'},
{ 'Company1', ['Laptop','Charger'], ['store', 'store2'], 'Seattle'},
{ 'Company1', ['Chair', 'Lamp'], ['store', 'store2'], 'Seattle'},
{ 'Company2', null, ['Bank'], 'Bellevue'}
```

### <a name="unroll-root-with-complex-array"></a>Karmaşık dizi ile kök al

| Alma ölçütü | Kökü al | Yansıtma |
| --------- | ----------- | ---------- |
| mallar. Orders. sevkedildi. OrderItem | mallar. siparişler |ad <br> OrderID = mal. Orders. OrderID <br> ItemName = mal. Orders. sevkedildi. OrderItems. ItemName <br> ımqty = mallar. Orders. sevkedildi. OrderItems. ıtemqty <br> Konum = Konum |

#### <a name="output"></a>Çıktı

```
{ 'MSFT', 1, 'Laptop', 20, 'Redmond'},
{ 'MSFT', 1, 'Charger', 2, 'Redmond'},
{ 'MSFT', 2, 'Mice', 2, 'Redmond'},
{ 'MSFT', 2, 'Keyboard', 1, 'Redmond'},
{ 'Company1', 4, 'Laptop', 20, 'Seattle'},
{ 'Company1', 4, 'Charger', 3, 'Seattle'},
{ 'Company1', 5, 'Chair', 4, 'Seattle'},
{ 'Company1', 5, 'Lamp', 2, 'Seattle'},
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

* Satırları sütunlara eklemek için [Pivot dönüşümünü](data-flow-pivot.md) kullanın.
* Sütunları satırlara pivot olarak eklemek için [UNPIVOT dönüşümünü](data-flow-unpivot.md) kullanın.
