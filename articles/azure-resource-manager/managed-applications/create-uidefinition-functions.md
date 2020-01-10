---
title: UI tanımı işlevleri oluşturma
description: Azure yönetilen uygulamalar için Kullanıcı arabirimi tanımları oluştururken kullanılacak işlevleri açıklar
author: tfitzmac
ms.topic: conceptual
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 6e56c5e528a17d42a75da54158f00857a917645c
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75650662"
---
# <a name="createuidefinition-functions"></a>Createuıdefinition işlevleri
Bu bölüm bir Createuıdefinition 'ın desteklenen tüm işlevlerine yönelik imzaları içerir.

Bir işlevi kullanmak için, bildirimi köşeli ayraçla çevreleyin. Örneğin:

```json
"[function()]"
```

Dizeler ve diğer işlevlere bir işlev için parametre olarak başvurulabilir, ancak dizeler tek tırnak işaretleri içine alınmalıdır. Örneğin:

```json
"[fn1(fn2(), 'foobar')]"
```

Uygun olduğunda, nokta işlecini kullanarak bir işlevin çıktısının özelliklerine başvurabilirsiniz. Örneğin:

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a>Başvuru işlevleri
Bu işlevler, bir Createuıdefinition özelliklerinden veya bağlamından çıkışlara başvurmak için kullanılabilir.

### <a name="basics"></a>temeller
Temel bilgiler adımında tanımlanan bir öğenin çıkış değerlerini döndürür.

Aşağıdaki örnek, temel bilgiler adımında `foo` adlı öğenin çıkışını döndürür:

```json
"[basics('foo')]"
```

### <a name="steps"></a>adımlar
Belirtilen adımda tanımlanan bir öğenin çıkış değerlerini döndürür. Temel bilgiler adımında öğelerin çıkış değerlerini almak için, bunun yerine `basics()` kullanın.

Aşağıdaki örnek, `foo`adlı adımda `bar` adlı öğenin çıkışını döndürür:

```json
"[steps('foo').bar]"
```

### <a name="location"></a>location
Temel bilgiler adımında veya geçerli bağlamda seçilen konumu döndürür.

Aşağıdaki örnek `"westus"`döndürebilir:

```json
"[location()]"
```

## <a name="string-functions"></a>Dize işlevleri
Bu işlevler yalnızca JSON dizeleri ile kullanılabilir.

### <a name="concat"></a>concat
Bir veya daha fazla dizeyi birleştirir.

Örneğin, `"bar"``element1` çıkış değeri varsa, bu örnek `"foobar!"`dize döndürür:

```json
"[concat('foo', steps('step1').element1, '!')]"
```

### <a name="substring"></a>dizeden
Belirtilen dizenin alt dizesini döndürür. Alt dize belirtilen dizinde başlar ve belirtilen uzunluğa sahiptir.

Aşağıdaki örnek `"ftw"`döndürür:

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a>replace
Geçerli dizedeki belirtilen dizenin tüm oluşumlarının başka bir dizeyle değiştirildiği bir dizeyi döndürür.

Aşağıdaki örnek `"Everything is awesome!"`döndürür:

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a>guid
Genel olarak benzersiz bir dize (GUID) oluşturur.

Aşağıdaki örnek `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`döndürebilir:

```json
"[guid()]"
```

### <a name="tolower"></a>ToLower
Küçük harfe dönüştürülmüş bir dize döndürür.

Aşağıdaki örnek `"foobar"`döndürür:

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a>ToUpper
Büyük harfe dönüştürülmüş bir dize döndürür.

Aşağıdaki örnek `"FOOBAR"`döndürür:

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a>Koleksiyon işlevleri
Bu işlevler, JSON dizeleri, diziler ve nesneler gibi koleksiyonlarla birlikte kullanılabilir.

### <a name="contains"></a>içerir
Bir dize belirtilen alt dizeyi içeriyorsa, bir dizi belirtilen değeri içeriyorsa veya bir nesne belirtilen anahtarı içeriyorsa `true` döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `true`döndürür:

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` `[1, 2, 3]`döndürdüğünü varsayın. Aşağıdaki örnek `false`döndürür:

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
`element1` döndürdüğünü varsayın:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek `true`döndürür:

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a>length
Bir dizedeki karakter sayısını, bir dizideki değerlerin sayısını veya bir nesnedeki anahtar sayısını döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `6`döndürür:

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` `[1, 2, 3]`döndürdüğünü varsayın. Aşağıdaki örnek `3`döndürür:

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
`element1` döndürdüğünü varsayın:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek `2`döndürür:

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a>boş
Dize, dizi veya nesne null ya da boş ise `true` döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `true`döndürür:

```json
"[empty('')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` `[1, 2, 3]`döndürdüğünü varsayın. Aşağıdaki örnek `false`döndürür:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
`element1` döndürdüğünü varsayın:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek `false`döndürür:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a>Örnek 4: null ve tanımsız
`element1` `null` veya tanımsız olduğunu varsayın. Aşağıdaki örnek `true`döndürür:

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a>adı
Belirtilen dizenin ilk karakterini döndürür; belirtilen dizinin ilk değeri; veya belirtilen nesnenin ilk anahtarı ve değeri.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `"f"`döndürür:

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` `[1, 2, 3]`döndürdüğünü varsayın. Aşağıdaki örnek `1`döndürür:

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
`element1` döndürdüğünü varsayın:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Aşağıdaki örnek `{"key1": "foobar"}`döndürür:

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a>Son
Belirtilen dizenin son karakterini, belirtilen dizinin son değerini veya belirtilen nesnenin son anahtarını ve değerini döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `"r"`döndürür:

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` `[1, 2, 3]`döndürdüğünü varsayın. Aşağıdaki örnek `2`döndürür:

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
`element1` döndürdüğünü varsayın:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek `{"key2": "raboof"}`döndürür:

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a>take
Dizenin başından itibaren belirtilen sayıda bitişik karakteri, dizinin başından itibaren belirtilen sayıda bitişik değeri ya da nesnenin başından itibaren belirtilen sayıda bitişik anahtar ve değeri döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `"foo"`döndürür:

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` `[1, 2, 3]`döndürdüğünü varsayın. Aşağıdaki örnek `[1, 2]`döndürür:

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
`element1` döndürdüğünü varsayın:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek `{"key1": "foobar"}`döndürür:

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a>Atla
Bir koleksiyonda belirtilen sayıda öğeyi atlar ve kalan öğeleri döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `"bar"`döndürür:

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` `[1, 2, 3]`döndürdüğünü varsayın. Aşağıdaki örnek `[3]`döndürür:

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
`element1` döndürdüğünü varsayın:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Aşağıdaki örnek `{"key2": "raboof"}`döndürür:

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a>Mantıksal işlevler
Bu işlevler, conditionals içinde kullanılabilir. Bazı işlevler tüm JSON veri türlerini desteklemiyor olabilir.

### <a name="equals"></a>eşittir
Her iki parametrenin de aynı tür ve değere sahip olması halinde `true` döndürür. Bu işlev tüm JSON veri türlerini destekler.

Aşağıdaki örnek `true`döndürür:

```json
"[equals(0, 0)]"
```

Aşağıdaki örnek `true`döndürür:

```json
"[equals('foo', 'foo')]"
```

Aşağıdaki örnek `false`döndürür:

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a>less
İlk parametre ikinci parametreden kesinlikle daha küçükse `true` döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek `true`döndürür:

```json
"[less(1, 2)]"
```

Aşağıdaki örnek `false`döndürür:

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a>lessOrEquals
İlk parametre ikinci parametreye eşit veya daha küçükse `true` döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek `true`döndürür:

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a>greater
İlk parametre ikinci parametreden kesinlikle daha büyükse `true` döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek `false`döndürür:

```json
"[greater(1, 2)]"
```

Aşağıdaki örnek `true`döndürür:

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a>greaterOrEquals
İlk parametre ikinci parametreye eşit veya daha büyükse `true` döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek `true`döndürür:

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a>'nı ve
Tüm parametrelerin `true`değerlendirmesi durumunda `true` döndürür. Bu işlev, yalnızca Boole türünden iki veya daha fazla parametreyi destekler.

Aşağıdaki örnek `true`döndürür:

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Aşağıdaki örnek `false`döndürür:

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a>veya
Parametrelerden en az biri `true`olarak değerlendirilirse `true` döndürür. Bu işlev, yalnızca Boole türünden iki veya daha fazla parametreyi destekler.

Aşağıdaki örnek `true`döndürür:

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Aşağıdaki örnek `true`döndürür:

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a>değil
Parametre `false`değerlendirilirse `true` döndürür. Bu işlev yalnızca Boole türündeki parametreleri destekler.

Aşağıdaki örnek `true`döndürür:

```json
"[not(false)]"
```

Aşağıdaki örnek `false`döndürür:

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a>coalesce
İlk null olmayan parametrenin değerini döndürür. Bu işlev tüm JSON veri türlerini destekler.

`element1` ve `element2` tanımsız olduğunu varsayın. Aşağıdaki örnek `"foobar"`döndürür:

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a>Dönüştürme işlevleri
Bu işlevler, JSON veri türleri ve kodlamaları arasındaki değerleri dönüştürmek için kullanılabilir.

### <a name="int"></a>int
Parametreyi bir tamsayıya dönüştürür. Bu işlev, Number ve String türündeki parametreleri destekler.

Aşağıdaki örnek `1`döndürür:

```json
"[int('1')]"
```

Aşağıdaki örnek `2`döndürür:

```json
"[int(2.9)]"
```

### <a name="float"></a>float
Parametreyi bir kayan noktaya dönüştürür. Bu işlev, Number ve String türündeki parametreleri destekler.

Aşağıdaki örnek `1.0`döndürür:

```json
"[float('1.0')]"
```

Aşağıdaki örnek `2.9`döndürür:

```json
"[float(2.9)]"
```

### <a name="string"></a>string
Parametreyi bir dizeye dönüştürür. Bu işlev tüm JSON veri türlerinin parametrelerini destekler.

Aşağıdaki örnek `"1"`döndürür:

```json
"[string(1)]"
```

Aşağıdaki örnek `"2.9"`döndürür:

```json
"[string(2.9)]"
```

Aşağıdaki örnek `"[1,2,3]"`döndürür:

```json
"[string([1,2,3])]"
```

Aşağıdaki örnek `"{"foo":"bar"}"`döndürür:

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a>bool
Parametreyi Boole değerine dönüştürür. Bu işlev Number, String ve Boolean türündeki parametreleri destekler. JavaScript 'teki Boole değerleri ile benzer şekilde, `0` veya `'false'` hariç herhangi bir değer `true`döndürür.

Aşağıdaki örnek `true`döndürür:

```json
"[bool(1)]"
```

Aşağıdaki örnek `false`döndürür:

```json
"[bool(0)]"
```

Aşağıdaki örnek `true`döndürür:

```json
"[bool(true)]"
```

Aşağıdaki örnek `true`döndürür:

```json
"[bool('true')]"
```

### <a name="parse"></a>MAZ
Parametreyi yerel bir türe dönüştürür. Diğer bir deyişle, bu işlev `string()`tersidir. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek `1`döndürür:

```json
"[parse('1')]"
```

Aşağıdaki örnek `true`döndürür:

```json
"[parse('true')]"
```

Aşağıdaki örnek `[1,2,3]`döndürür:

```json
"[parse('[1,2,3]')]"
```

Aşağıdaki örnek `{"foo":"bar"}`döndürür:

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a>encodeBase64
Parametreyi bir Base-64 kodlu dize olarak kodlar. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek `"Zm9vYmFy"`döndürür:

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a>decodeBase64
Base-64 kodlu dizeden parametrenin kodunu çözer. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek `"foobar"`döndürür:

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a>encodeURIComponent
Parametreyi bir URL kodlamalı dize olarak kodlar. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek `"https%3A%2F%2Fportal.azure.com%2F"`döndürür:

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a>decodeURIComponent
URL kodlamalı bir dizeden parametrenin kodunu çözer. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek `"https://portal.azure.com/"`döndürür:

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a>Matematik İşlevleri
### <a name="add"></a>add
İki sayı ekler ve sonucu döndürür.

Aşağıdaki örnek `3`döndürür:

```json
"[add(1, 2)]"
```

### <a name="sub"></a>sub
İkinci sayıyı ilk sayıdan çıkartır ve sonucu döndürür.

Aşağıdaki örnek `1`döndürür:

```json
"[sub(3, 2)]"
```

### <a name="mul"></a>MUL
İki sayıyı çarpar ve sonucu döndürür.

Aşağıdaki örnek `6`döndürür:

```json
"[mul(2, 3)]"
```

### <a name="div"></a>div
İlk sayıyı ikinci sayıya böler ve sonucu döndürür. Sonuç her zaman bir tamsayıdır.

Aşağıdaki örnek `2`döndürür:

```json
"[div(6, 3)]"
```

### <a name="mod"></a>alma
İlk sayıyı ikinci sayıya böler ve kalanı döndürür.

Aşağıdaki örnek `0`döndürür:

```json
"[mod(6, 3)]"
```

Aşağıdaki örnek `2`döndürür:

```json
"[mod(6, 4)]"
```

### <a name="min"></a>dk
İki sayının küçük sayısını döndürür.

Aşağıdaki örnek `1`döndürür:

```json
"[min(1, 2)]"
```

### <a name="max"></a>en büyük
İki sayıdan daha büyük bir değer döndürür.

Aşağıdaki örnek `2`döndürür:

```json
"[max(1, 2)]"
```

### <a name="range"></a>aralık
Belirtilen Aralık içinde bir integral sayı dizisi oluşturur.

Aşağıdaki örnek `[1,2,3]`döndürür:

```json
"[range(1, 3)]"
```

### <a name="rand"></a>rand
Belirtilen Aralık içinde rastgele bir integral sayı döndürür. Bu işlev, şifreli olarak güvenli rastgele sayılar oluşturmaz.

Aşağıdaki örnek `42`döndürebilir:

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a>Kat
Belirtilen sayıdan küçük veya eşit en büyük tamsayıyı döndürür.

Aşağıdaki örnek `3`döndürür:

```json
"[floor(3.14)]"
```

### <a name="ceil"></a>Ceil
Belirtilen sayıdan büyük veya eşit en büyük tamsayıyı döndürür.

Aşağıdaki örnek `4`döndürür:

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a>Tarih işlevleri
### <a name="utcnow"></a>UtcNow
Yerel bilgisayardaki geçerli tarih ve saatin ISO 8601 biçiminde bir dize döndürür.

Aşağıdaki örnek `"1990-12-31T23:59:59.000Z"`döndürebilir:

```json
"[utcNow()]"
```

### <a name="addseconds"></a>addSeconds
Belirtilen zaman damgasına tamsayı cinsinden bir sayı ekler.

Aşağıdaki örnek `"1991-01-01T00:00:00.000Z"`döndürür:

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a>addMinutes
Belirtilen zaman damgasına tamsayı cinsinden bir sayı ekler.

Aşağıdaki örnek `"1991-01-01T00:00:59.000Z"`döndürür:

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a>addHours
Belirtilen zaman damgasına tamsayı saat sayısını ekler.

Aşağıdaki örnek `"1991-01-01T00:59:59.000Z"`döndürür:

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Resource Manager giriş için bkz. [Azure Resource Manager genel bakış](../management/overview.md).

