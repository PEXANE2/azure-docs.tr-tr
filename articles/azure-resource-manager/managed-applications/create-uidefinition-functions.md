---
title: UI tanım işlevleri oluşturma
description: Azure Yönetilen Uygulamalar için Kullanıcı Arabirimi tanımları yaparken kullanılacak işlevleri açıklar
author: tfitzmac
ms.topic: conceptual
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 6e56c5e528a17d42a75da54158f00857a917645c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248456"
---
# <a name="createuidefinition-functions"></a>CreateUiDefinition işlevleri
Bu bölüm, createuiDefinition tüm desteklenen işlevleri için imzaiçerir.

Bir işlev kullanmak için, bildirgeyi kare köşeli ayraçlarla çevrele. Örnek:

```json
"[function()]"
```

Dizeleri ve diğer işlevler bir işlev için parametre olarak başvurulabilir, ancak dizeleri tek tırnak içinde çevrilmelidir. Örnek:

```json
"[fn1(fn2(), 'foobar')]"
```

Uygun olduğu durumlarda, nokta işleci kullanarak bir işlevin çıktıözelliklerine başvuruyapabilirsiniz. Örnek:

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a>Başvuru işlevleri
Bu işlevler, CreateUiDefinition özellikleri veya bağlamından çıktıları referans almak için kullanılabilir.

### <a name="basics"></a>Temel
Temeller adımında tanımlanan bir öğenin çıktı değerlerini verir.

Aşağıdaki örnek, Temeller adımında `foo` adlı öğenin çıktısını döndürür:

```json
"[basics('foo')]"
```

### <a name="steps"></a>adımlar
Belirtilen adımda tanımlanan bir öğenin çıkış değerlerini verir. Temel ler adımındaki öğelerin çıktı değerlerini `basics()` almak için bunun yerine kullanın.

Aşağıdaki örnek, adlı `bar` `foo`adımda adlı öğenin çıktısını döndürür:

```json
"[steps('foo').bar]"
```

### <a name="location"></a>location
Temel Bilgiler adımında veya geçerli bağlamda seçilen konumu döndürür.

Aşağıdaki örnek dönebilir: `"westus"`

```json
"[location()]"
```

## <a name="string-functions"></a>Dize işlevleri
Bu işlevler yalnızca JSON dizeleri ile kullanılabilir.

### <a name="concat"></a>concat
Bir veya daha fazla ipi birleştirir.

Örneğin, `element1` if `"bar"`çıktı değeri ise , sonra bu `"foobar!"`örnek dize döndürür:

```json
"[concat('foo', steps('step1').element1, '!')]"
```

### <a name="substring"></a>Dize
Belirtilen dizenin alt dizesini döndürür. Alt dize belirtilen dizinde başlar ve belirtilen uzunluğa sahiptir.

Aşağıdaki örnek `"ftw"`döner:

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a>Değiştirmek
Geçerli dizede belirtilen dize tüm oluşumları başka bir dize ile değiştirilir bir dize döndürür.

Aşağıdaki örnek `"Everything is awesome!"`döner:

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a>Guıd
Genel olarak benzersiz bir dize (GUID) oluşturur.

Aşağıdaki örnek dönebilir: `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`

```json
"[guid()]"
```

### <a name="tolower"></a>Tolower
Küçük harfe dönüştürülmüş bir dize döndürür.

Aşağıdaki örnek `"foobar"`döner:

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a>Toupper
Büyük harfe dönüştürülmüş bir dize döndürür.

Aşağıdaki örnek `"FOOBAR"`döner:

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a>Toplama işlevleri
Bu işlevler JSON dizeleri, diziler ve nesneler gibi koleksiyonlarla kullanılabilir.

### <a name="contains"></a>içerir
Bir `true` dize belirtilen alt dizeyi içeriyorsa, bir dizi belirtilen değeri içeriyorsa veya bir nesne belirtilen anahtarı içeriyorsa döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `true`döner:

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` İadeleri `[1, 2, 3]`varsayalım. Aşağıdaki örnek `false`döner:

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
İadeleri varsayalım: `element1`

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek `true`döner:

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a>length
Bir dizedeki karakter sayısını, dizideki değer sayısını veya bir nesnedeki anahtar sayısını döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `6`döner:

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` İadeleri `[1, 2, 3]`varsayalım. Aşağıdaki örnek `3`döner:

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
İadeleri varsayalım: `element1`

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek `2`döner:

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a>empty
Dize, dizi veya nesne boşsa döndürür. `true`

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `true`döner:

```json
"[empty('')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` İadeleri `[1, 2, 3]`varsayalım. Aşağıdaki örnek `false`döner:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
İadeleri varsayalım: `element1`

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek `false`döner:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a>Örnek 4: null ve tanımsız
`element1` Farz `null` et ya da tanımlanmamış. Aşağıdaki örnek `true`döner:

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a>Ilk
Belirtilen dizenin ilk karakterini döndürür; belirtilen dizinin ilk değeri; veya belirtilen nesnenin ilk anahtarı ve değeri.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `"f"`döner:

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` İadeleri `[1, 2, 3]`varsayalım. Aşağıdaki örnek `1`döner:

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
İadeleri varsayalım: `element1`

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Aşağıdaki örnek `{"key1": "foobar"}`döner:

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a>Son
Belirtilen dizenin son karakterini, belirtilen dizinin son değerini veya belirtilen nesnenin son anahtarını ve değerini döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `"r"`döner:

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` İadeleri `[1, 2, 3]`varsayalım. Aşağıdaki örnek `2`döner:

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
İadeleri varsayalım: `element1`

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek `{"key2": "raboof"}`döner:

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a>almak
Dize başından itibaren belirli sayıda bitişik karakter, dizinin başlangıcından itibaren belirli sayıda bitişik değer veya nesnenin başlangıcından itibaren belirli sayıda bitişik anahtar ve değer verir.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `"foo"`döner:

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` İadeleri `[1, 2, 3]`varsayalım. Aşağıdaki örnek `[1, 2]`döner:

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
İadeleri varsayalım: `element1`

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek `{"key1": "foobar"}`döner:

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a>Atla
Koleksiyondaki belirli sayıda öğeyi atlar ve kalan öğeleri döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek `"bar"`döner:

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1` İadeleri `[1, 2, 3]`varsayalım. Aşağıdaki örnek `[3]`döner:

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
İadeleri varsayalım: `element1`

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Aşağıdaki örnek `{"key2": "raboof"}`döner:

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a>Mantıksal işlevler
Bu fonksiyonlar koşullu olarak kullanılabilir. Bazı işlevler tüm JSON veri türlerini desteklemeyebilir.

### <a name="equals"></a>equals
Her `true` iki parametre de aynı tür ve değere sahipse döndürür. Bu işlev tüm JSON veri türlerini destekler.

Aşağıdaki örnek `true`döner:

```json
"[equals(0, 0)]"
```

Aşağıdaki örnek `true`döner:

```json
"[equals('foo', 'foo')]"
```

Aşağıdaki örnek `false`döner:

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a>daha az
İlk `true` parametre ikinci parametreden kesinlikle daha azsa döndürür. Bu işlev yalnızca tür sayısı ve dize parametrelerini destekler.

Aşağıdaki örnek `true`döner:

```json
"[less(1, 2)]"
```

Aşağıdaki örnek `false`döner:

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a>lessOrEquals
İlk `true` parametre ikinci parametreden küçük veya eşitse döndürür. Bu işlev yalnızca tür sayısı ve dize parametrelerini destekler.

Aşağıdaki örnek `true`döner:

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a>büyüktür
İlk `true` parametre ikinci parametreden kesinlikle büyükse döndürür. Bu işlev yalnızca tür sayısı ve dize parametrelerini destekler.

Aşağıdaki örnek `false`döner:

```json
"[greater(1, 2)]"
```

Aşağıdaki örnek `true`döner:

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a>greaterOrEquals
İlk `true` parametre ikinci parametreden büyük veya eşitse döndürür. Bu işlev yalnızca tür sayısı ve dize parametrelerini destekler.

Aşağıdaki örnek `true`döner:

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a>ve
Tüm `true` parametreler i' `true`ye göre değerlendirilirse döndürür. Bu işlev, yalnızca Boolean türünden iki veya daha fazla parametreyi destekler.

Aşağıdaki örnek `true`döner:

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Aşağıdaki örnek `false`döner:

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a>or
Parametrelerden en az biri değerlendirilmesi `true`durumunda döndürür. `true` Bu işlev, yalnızca Boolean türünden iki veya daha fazla parametreyi destekler.

Aşağıdaki örnek `true`döner:

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Aşağıdaki örnek `true`döner:

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a>not
Parametre ' nin `true` `false`değerlendirilmesi durumunda döndürür. Bu işlev yalnızca Boolean türü parametrelerini destekler.

Aşağıdaki örnek `true`döner:

```json
"[not(false)]"
```

Aşağıdaki örnek `false`döner:

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a>Coalesce
İlk null olmayan parametrenin değerini verir. Bu işlev tüm JSON veri türlerini destekler.

`element1` Varsayalım `element2` ve tanımsız. Aşağıdaki örnek `"foobar"`döner:

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a>Dönüştürme işlevleri
Bu işlevler, JSON veri türleri ve kodlamalar arasındaki değerleri dönüştürmek için kullanılabilir.

### <a name="int"></a>int
Parametreyi bir sayacına dönüştürür. Bu işlev, tür numarası ve dize parametrelerini destekler.

Aşağıdaki örnek `1`döner:

```json
"[int('1')]"
```

Aşağıdaki örnek `2`döner:

```json
"[int(2.9)]"
```

### <a name="float"></a>float
Parametreyi kayan noktaya dönüştürür. Bu işlev, tür numarası ve dize parametrelerini destekler.

Aşağıdaki örnek `1.0`döner:

```json
"[float('1.0')]"
```

Aşağıdaki örnek `2.9`döner:

```json
"[float(2.9)]"
```

### <a name="string"></a>string
Parametreyi bir dize dönüştürür. Bu işlev, tüm JSON veri türlerinin parametrelerini destekler.

Aşağıdaki örnek `"1"`döner:

```json
"[string(1)]"
```

Aşağıdaki örnek `"2.9"`döner:

```json
"[string(2.9)]"
```

Aşağıdaki örnek `"[1,2,3]"`döner:

```json
"[string([1,2,3])]"
```

Aşağıdaki örnek `"{"foo":"bar"}"`döner:

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a>bool
Parametreyi Boolean'a dönüştürür. Bu işlev, tür numarası, dize ve Boolean parametrelerini destekler. JavaScript Booleans benzer, herhangi `0` bir `'false'` `true`değer dışında veya döner .

Aşağıdaki örnek `true`döner:

```json
"[bool(1)]"
```

Aşağıdaki örnek `false`döner:

```json
"[bool(0)]"
```

Aşağıdaki örnek `true`döner:

```json
"[bool(true)]"
```

Aşağıdaki örnek `true`döner:

```json
"[bool('true')]"
```

### <a name="parse"></a>parse
Parametreyi yerel bir türe dönüştürür. Başka bir deyişle, bu işlev `string()`ters. Bu işlev yalnızca tür dize parametrelerini destekler.

Aşağıdaki örnek `1`döner:

```json
"[parse('1')]"
```

Aşağıdaki örnek `true`döner:

```json
"[parse('true')]"
```

Aşağıdaki örnek `[1,2,3]`döner:

```json
"[parse('[1,2,3]')]"
```

Aşağıdaki örnek `{"foo":"bar"}`döner:

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a>encodeBase64
Parametreyi bir base-64 kodlanmış dizeyle kodlar. Bu işlev yalnızca tür dize parametrelerini destekler.

Aşağıdaki örnek `"Zm9vYmFy"`döner:

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a>decodeBase64
Parametreyi baz-64 kodlanmış bir dizeden çözer. Bu işlev yalnızca tür dize parametrelerini destekler.

Aşağıdaki örnek `"foobar"`döner:

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a>kodlamaUriComponent
Parametreyi URL kodlanmış bir dizeyle kodlar. Bu işlev yalnızca tür dize parametrelerini destekler.

Aşağıdaki örnek `"https%3A%2F%2Fportal.azure.com%2F"`döner:

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a>decodeUriComponent
Parametreyi URL kodlanmış bir dizeden çözer. Bu işlev yalnızca tür dize parametrelerini destekler.

Aşağıdaki örnek `"https://portal.azure.com/"`döner:

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a>Matematik İşlevleri
### <a name="add"></a>add
İki sayı ekler ve sonucu döndürür.

Aşağıdaki örnek `3`döner:

```json
"[add(1, 2)]"
```

### <a name="sub"></a>Alt
İkinci sayıyı ilk sayıdan çıkarır ve sonucu döndürür.

Aşağıdaki örnek `1`döner:

```json
"[sub(3, 2)]"
```

### <a name="mul"></a>Mul
İki sayıyı çarpar ve sonucu döndürür.

Aşağıdaki örnek `6`döner:

```json
"[mul(2, 3)]"
```

### <a name="div"></a>div
İlk sayıyı ikinci sayıya böler ve sonucu döndürür. Sonuç her zaman bir sondadır.

Aşağıdaki örnek `2`döner:

```json
"[div(6, 3)]"
```

### <a name="mod"></a>mod
İlk sayıyı ikinci sayıya böler ve geri kalanını döndürür.

Aşağıdaki örnek `0`döner:

```json
"[mod(6, 3)]"
```

Aşağıdaki örnek `2`döner:

```json
"[mod(6, 4)]"
```

### <a name="min"></a>min
İki sayının küçük olanını verir.

Aşağıdaki örnek `1`döner:

```json
"[min(1, 2)]"
```

### <a name="max"></a>max
İki sayının büyüklerini döndürür.

Aşağıdaki örnek `2`döner:

```json
"[max(1, 2)]"
```

### <a name="range"></a>aralık
Belirtilen aralıktaki integral sayıların bir dizisini oluşturur.

Aşağıdaki örnek `[1,2,3]`döner:

```json
"[range(1, 3)]"
```

### <a name="rand"></a>rand
Belirtilen aralıkiçinde rasgele bir integral numarası döndürür. Bu işlev, şifreleme yle güvenli rastgele sayılar oluşturmaz.

Aşağıdaki örnek dönebilir: `42`

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a>Kat
Belirtilen sayıdan daha az veya eşit olan en büyük tümseci döndürür.

Aşağıdaki örnek `3`döner:

```json
"[floor(3.14)]"
```

### <a name="ceil"></a>Ceil
Belirtilen sayıdan daha büyük veya eşit olan en büyük tümseci döndürür.

Aşağıdaki örnek `4`döner:

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a>Tarih işlevleri
### <a name="utcnow"></a>utcŞimdi
Yerel bilgisayardaki geçerli tarih ve saatin ISO 8601 biçiminde bir dize döndürür.

Aşağıdaki örnek dönebilir: `"1990-12-31T23:59:59.000Z"`

```json
"[utcNow()]"
```

### <a name="addseconds"></a>Addseconds
Belirtilen zaman damgasına ayrılmaz bir saniye sayısı ekler.

Aşağıdaki örnek `"1991-01-01T00:00:00.000Z"`döner:

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a>Addminutes
Belirtilen zaman damgasına ayrılmaz bir dakika sayısı ekler.

Aşağıdaki örnek `"1991-01-01T00:00:59.000Z"`döner:

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a>Addhours
Belirtilen zaman damgasına ayrılmaz bir saat sayısı ekler.

Aşağıdaki örnek `"1991-01-01T00:59:59.000Z"`döner:

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Kaynak Yöneticisi'ne giriş için Azure [Kaynak Yöneticisi'ne genel bakış](../management/overview.md)'a bakın.

