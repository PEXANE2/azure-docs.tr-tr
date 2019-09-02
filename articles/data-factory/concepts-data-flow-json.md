---
title: Azure Data Factory veri akışı JSON kavramlarını eşleme
description: Data Factory eşleme veri akışı, Hiyerarşilerle JSON belgelerini işlemeye yönelik yerleşik yeteneklere sahiptir
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: cde42dda47d54c03c50895bc625f99c9350b53e3
ms.sourcegitcommit: d470d4e295bf29a4acf7836ece2f10dabe8e6db2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2019
ms.locfileid: "70210695"
---
# <a name="mapping-data-flow-json-handling"></a>Veri akışı JSON işlemesini eşleme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="creating-json-structures-in-expression-editor"></a>İfade düzenleyicisinde JSON yapıları oluşturma
### <a name="derived-column-transformation"></a>Türetilmiş sütun dönüşümü
Veri akışınıza karmaşık bir sütun eklemek, türetilmiş sütun ifade Düzenleyicisi aracılığıyla daha kolay olur. Yeni bir sütun ekleyip düzenleyiciyi açtıktan sonra iki seçenek vardır: JSON yapısını el ile girin veya etkileşimli olarak alt sütunlar eklemek için Kullanıcı arabirimini kullanın.

#### <a name="interactive-ui-json-design"></a>Etkileşimli UI JSON tasarımı
Çıktı şeması yan bölmesinden, `+` menü kullanılarak yeni alt sütunlar eklenebilir: ![Alt sütun Ekle](media/data-flow/addsubcolumn.png "Alt sütun Ekle")

Buradan, yeni sütunlar ve alt sütunlar aynı şekilde eklenebilir. Karmaşık olmayan her bir alan için, ifade düzenleyicisine sağa doğru bir ifade eklenebilir.

![Karmaşık sütun](media/data-flow/complexcolumn.png "Karmaşık sütun")

#### <a name="manual-json-design"></a>El ile JSON tasarımı
JSON yapısını el ile eklemek için yeni bir sütun ekleyin ve düzenleyicide ifadeyi girin. İfade aşağıdaki genel biçimi izler:
```
@(
    field1=0,
    field2=@(
        field1=0
    )
)
```
Bu ifade "complexColumn" adlı bir sütun için girilmişse, havuza aşağıdaki JSON olarak yazılır:
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

#### <a name="sample-manual-dsl"></a>Örnek el ile DSL
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

## <a name="source-format-options"></a>Kaynak Biçim seçenekleri
### <a name="default"></a>Varsayılan
```
{ "json": "record 1" }
{ "json": "record 2" }
{ "json": "record 3" }
```

### <a name="single-document"></a>Tek belge
* Seçenek bir
```
[
    {
        "json": "record 1"
    },
    {
        "json": "record 2"
    },
    {
        "json": "record 3"
    }
]
```

* İki seçenek
```
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

### <a name="unquoted-column-names"></a>Tırnak işaretleri olmayan sütun adları
```
{ json: "record 1" }
{ json: "record 2" }
{ json: "record 3" }
```

### <a name="has-comments"></a>Açıklamalar içeriyor
```
{ "json": /** comment **/ "record 1" }
{ "json": "record 2" }
{ /** comment **/ "json": "record 3" }
```

### <a name="single-quoted"></a>Tek tırnaklı
```
{ 'json': 'record 1' }
{ 'json': 'record 2' }
{ 'json': 'record 3' }
```

### <a name="backslash-escaped"></a>Ters eğik çizgi kaçışı
```
{ "json": "record 1" }
{ "json": "\} \" \' \\ \n \\n record 2" }
{ "json": "record 3" }
```

# <a name="higher-order-functions"></a>Daha yüksek sıra işlevleri
## <a name="filter"></a>filter
Öğeleri, belirtilen koşulu karşılamayan dizinin dışına filtreler. Filtre, koşul işlevindeki bir öğeye #item olarak bir başvuru bekliyor.

### <a name="examples"></a>Örnekler
```
filter([1, 2, 3, 4], #item > 2) => [3, 4]
filter(['a', 'b', 'c', 'd'], #item == 'a' || #item == 'b') => ['a', 'b']
```

## <a name="map"></a>map
Dizideki her öğeyi, belirtilen ifadeyi kullanarak yeni bir öğeye eşler. Eşleme, ifade işlevindeki bir öğeye #item olarak bir başvuru bekliyor.

### <a name="examples"></a>Örnekler
```
map([1, 2, 3, 4], #item + 2) => [3, 4, 5, 6]
map(['a', 'b', 'c', 'd'], #item + '_processed') => ['a_processed', 'b_processed', 'c_processed', 'd_processed']
```

## <a name="reduce"></a>azal
Öğeleri bir dizide biriktirir. Azaltma, #acc ve #item olarak ilk ifade işlevindeki bir Biriktiricinin ve bir öğenin bir başvurusunu bekler ve ikinci ifade işlevinde kullanılacak #result olarak elde edilen değeri bekler.

### <a name="examples"></a>Örnekler
```
reduce([1, 2, 3, 4], 0, #acc + #item, #result) => 10
reduce(['1', '2', '3', '4'], '0', #acc + #item, #result) => '01234'
reduce([1, 2, 3, 4], 0, #acc + #item, #result + 15) => 25
```

## <a name="sort"></a>düzenine
Belirtilen koşul işlevini kullanarak diziyi sıralar. Sıralama, #item1 ve #item2 olarak ifade işlevindeki birbirini izleyen iki öğeye bir başvuru bekliyor.

### <a name="examples"></a>Örnekler
```
sort([4, 8, 2, 3], compare(#item1, #item2)) => [2, 3, 4, 8]
sort(['a3', 'b2', 'c1'],
        iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) => ['c1', 'b2', 'a3']
sort(['a3', 'b2', 'c1'],
        iif(#item1 >= #item2, 1, -1)) => ['a3', 'b2', 'c1']
```

## <a name="contains"></a>içerir
Belirtilen dizideki herhangi bir öğe, belirtilen koşulda doğru olarak değerlendiriliyorsa true değerini döndürür. Contains, koşul işlevindeki bir öğeye #item olarak bir başvuru bekliyor.

### <a name="examples"></a>Örnekler
```
contains([1, 2, 3, 4], #item == 3) => true
contains([1, 2, 3, 4], #item > 5) => false
```

## <a name="next-steps"></a>Sonraki adımlar

* [Hiyerarşik yapılarınızı derlemek için türetilmiş sütun dönüşümünü kullanın](data-flow-derived-column.md)
