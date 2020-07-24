---
title: UI tanımı toplama işlevleri oluştur
description: Diziler ve nesneler gibi koleksiyonlarla çalışırken kullanılacak işlevleri açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: 2a075c5c99f457681cd49e75014487bf9cca263c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099929"
---
# <a name="createuidefinition-collection-functions"></a>Createuıdefinition koleksiyon işlevleri

Bu işlevler, JSON dizeleri, diziler ve nesneler gibi koleksiyonlarla birlikte kullanılabilir.

## <a name="contains"></a>contains

`true`Bir dize belirtilen alt dizeyi içeriyorsa, bir dizi belirtilen değeri içeriyorsa veya bir nesne belirtilen anahtarı içeriyorsa döndürür.

### <a name="example-string-contains"></a>Örnek: dize Contains

Aşağıdaki örnek şunu döndürür `true` :

```json
"[contains('webapp', 'web')]"
```

### <a name="example-array-contains"></a>Örnek: dizi içerir

`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `false` :

```json
"[contains(steps('demoStep').element1, 4)]"
```

### <a name="example-object-contains"></a>Örnek: nesne şunu içerir

Kabul edilecek `element1` değer:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Aşağıdaki örnek şunu döndürür `true` :

```json
"[contains(steps('demoStep').element1, 'key1')]"
```

## <a name="empty"></a>empty

`true`Dize, dizi veya nesne null ya da boş ise döndürür.

### <a name="example-string-empty"></a>Örnek: dize boş

Aşağıdaki örnek şunu döndürür `true` :

```json
"[empty('')]"
```

### <a name="example-array-empty"></a>Örnek: dizi boş

`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `false` :

```json
"[empty(steps('demoStep').element1)]"
```

### <a name="example-object-empty"></a>Örnek: nesne boş

Kabul edilecek `element1` değer:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Aşağıdaki örnek şunu döndürür `false` :

```json
"[empty(steps('demoStep').element1)]"
```

### <a name="example-null-and-undefined"></a>Örnek: null ve tanımsız

`element1` `null` Ya da tanımsız olduğunu varsayın. Aşağıdaki örnek şunu döndürür `true` :

```json
"[empty(steps('demoStep').element1)]"
```

## <a name="filter"></a>filtre

Lambda işlevi olarak belirtilen filtreleme mantığını uyguladıktan sonra yeni bir dizi döndürür. İlk parametre, filtreleme için kullanılacak dizidir. İkinci parametre, filtreleme mantığını belirten lambda işlevidir.

Aşağıdaki örnek, dizisini döndürür `[ { "name": "abc" } ]` .

```json
"[filter(parse('[{\"name\":\"abc\"},{\"name\":\"xyz\"}]'), (item) => contains(item.name, 'abc'))]"
```

## <a name="first"></a>adı

Belirtilen dizenin ilk karakterini döndürür; belirtilen dizinin ilk değeri; veya belirtilen nesnenin ilk anahtarı ve değeri.

### <a name="example-string-first"></a>Örnek: önce dize

Aşağıdaki örnek şunu döndürür `"c"` :

```json
"[first('contoso')]"
```

### <a name="example-array-first"></a>Örnek: önce dizi

`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `1` :

```json
"[first(steps('demoStep').element1)]"
```

#### <a name="example-object-first"></a>Örnek: önce nesne

Kabul edilecek `element1` değer:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Aşağıdaki örnek şunu döndürür `{"key1": "Linux"}` :

```json
"[first(steps('demoStep').element1)]"
```

## <a name="last"></a>Son

Belirtilen dizenin son karakterini, belirtilen dizinin son değerini veya belirtilen nesnenin son anahtarını ve değerini döndürür.

### <a name="example-string-last"></a>Örnek: dize son

Aşağıdaki örnek şunu döndürür `"o"` :

```json
"[last('contoso')]"
```

### <a name="example-array-last"></a>Örnek: dizi son

`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `3` :

```json
"[last(steps('demoStep').element1)]"
```

### <a name="example-object-last"></a>Örnek: nesne son

Kabul edilecek `element1` değer:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Aşağıdaki örnek şunu döndürür `{"key2": "Windows"}` :

```json
"[last(steps('demoStep').element1)]"
```

## <a name="length"></a>length

Bir dizedeki karakter sayısını, bir dizideki değerlerin sayısını veya bir nesnedeki anahtar sayısını döndürür.

### <a name="example-string-length"></a>Örnek: dize uzunluğu

Aşağıdaki örnek şunu döndürür `7` :

```json
"[length('Contoso')]"
```

### <a name="example-array-length"></a>Örnek: dizi uzunluğu

`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `3` :

```json
"[length(steps('demoStep').element1)]"
```

### <a name="example-object-length"></a>Örnek: nesne uzunluğu

Kabul edilecek `element1` değer:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Aşağıdaki örnek şunu döndürür `2` :

```json
"[length(steps('demoStep').element1)]"
```

## <a name="map"></a>map

Belirtilen dizide Lambda işlevini çağırdıktan sonra yeni bir dizi döndürür. İlk parametre Lambda işlevi için kullanılacak dizidir. İkinci parametre lambda işlevidir.

Aşağıdaki örnek her iki katına de yeni bir dizi döndürür. Sonuç olarak `[2, 4, 6]` .

```json
"[map(parse('[1, 2, 3]'), (item) => mul(2, item))]"
```

Aşağıdaki örnek yeni bir dizi döndürür `["abc", "xyz"]` .

```json
"[map(parse('[{\"name\":\"abc\"},{\"name\":\"xyz\"}]'), (item) => item.name)]"
```

## <a name="skip"></a>Atla

Bir koleksiyonda belirtilen sayıda öğeyi atlar ve kalan öğeleri döndürür.

### <a name="example-string-skip"></a>Örnek: String Skip

Aşağıdaki örnek şunu döndürür `"app"` :

```json
"[skip('webapp', 3)]"
```

### <a name="example-array-skip"></a>Örnek: Array Skip

`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `[3]` :

```json
"[skip(steps('demoStep').element1, 2)]"
```

### <a name="example-object-skip"></a>Örnek: nesne atla

Kabul edilecek `element1` değer:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```
Aşağıdaki örnek şunu döndürür `{"key2": "Windows"}` :

```json
"[skip(steps('demoStep').element1, 1)]"
```

## <a name="split"></a>split

Ayırıcı tarafından ayrılan girdinin alt dizelerini içeren dizelerin dizisini döndürür.

Aşağıdaki örnek, dizisini döndürür `[ "555", "867", "5309" ]` .

```json
"[split('555-867-5309', '-')]"
```

## <a name="take"></a>take

Dizenin başından itibaren belirtilen sayıda bitişik karakteri, dizinin başından itibaren belirtilen sayıda bitişik değeri ya da nesnenin başından itibaren belirtilen sayıda bitişik anahtar ve değeri döndürür.

### <a name="example-string-take"></a>Örnek: dize al

Aşağıdaki örnek şunu döndürür `"web"` :

```json
"[take('webapp', 3)]"
```

### <a name="example-array-take"></a>Örnek: dizi alma

`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `[1, 2]` :

```json
"[take(steps('demoStep').element1, 2)]"
```

### <a name="example-object-take"></a>Örnek: nesne al

Kabul edilecek `element1` değer:

```json
{
  "key1": "Linux",
  "key2": "Windows"
}
```

Aşağıdaki örnek şunu döndürür `{"key1": "Linux"}` :

```json
"[take(steps('demoStep').element1, 1)]"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager giriş için bkz. [Azure Resource Manager genel bakış](../management/overview.md).
