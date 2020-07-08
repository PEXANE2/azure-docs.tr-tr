---
title: UI tanımı işlevleri oluşturma
description: Azure yönetilen uygulamalar için Kullanıcı arabirimi tanımları oluştururken kullanılacak işlevleri açıklar
author: tfitzmac
ms.topic: conceptual
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: a93f4ff2ddc0737692de9e5619cf7a7521936224
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82980822"
---
# <a name="createuidefinition-functions"></a>Createuıdefinition işlevleri
Bu bölüm bir Createuıdefinition 'ın desteklenen tüm işlevlerine yönelik imzaları içerir.

Bir işlevi kullanmak için, çağrıyı köşeli ayraçla çevreleyin. Örneğin:

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

### <a name="basics"></a>temel bilgileri
Temel bilgiler adımında tanımlanan bir öğenin çıkış değerlerini döndürür.

Aşağıdaki örnek, temel bilgiler adımında adlı öğesinin çıkışını döndürür `foo` :

```json
"[basics('foo')]"
```

### <a name="steps"></a>adımlar
Belirtilen adımda tanımlanan bir öğenin çıkış değerlerini döndürür. Temel bilgiler adımında öğelerin çıkış değerlerini almak için `basics()` yerine kullanın.

Aşağıdaki örnek, adlı adımda adlı öğesinin çıkışını döndürür `bar` `foo` :

```json
"[steps('foo').bar]"
```

### <a name="location"></a>location
Temel bilgiler adımında veya geçerli bağlamda seçilen konumu döndürür.

Aşağıdaki örnek şunu döndürebilir `"westus"` :

```json
"[location()]"
```

## <a name="string-functions"></a>Dize işlevleri
Bu işlevler yalnızca JSON dizeleri ile kullanılabilir.

### <a name="concat"></a>concat
Bir veya daha fazla dizeyi birleştirir.

Örneğin, çıkış değeri `element1` Eğer ise `"bar"` , bu örnek dizeyi döndürür `"foobar!"` :

```json
"[concat('foo', steps('step1').element1, '!')]"
```

### <a name="substring"></a>dizeden
Belirtilen dizenin alt dizesini döndürür. Alt dize belirtilen dizinde başlar ve belirtilen uzunluğa sahiptir.

Aşağıdaki örnek şunu döndürür `"ftw"` :

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a>değiştirin
Geçerli dizedeki belirtilen dizenin tüm oluşumlarının başka bir dizeyle değiştirildiği bir dizeyi döndürür.

Aşağıdaki örnek şunu döndürür `"Everything is awesome!"` :

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a>guid
Genel olarak benzersiz bir dize (GUID) oluşturur.

Aşağıdaki örnek şunu döndürebilir `"c7bc8bdc-7252-4a82-ba53-7c468679a511"` :

```json
"[guid()]"
```

### <a name="tolower"></a>toLower
Küçük harfe dönüştürülmüş bir dize döndürür.

Aşağıdaki örnek şunu döndürür `"foobar"` :

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a>toUpper
Büyük harfe dönüştürülmüş bir dize döndürür.

Aşağıdaki örnek şunu döndürür `"FOOBAR"` :

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a>Koleksiyon işlevleri
Bu işlevler, JSON dizeleri, diziler ve nesneler gibi koleksiyonlarla birlikte kullanılabilir.

### <a name="contains"></a>şunu içerir
`true`Bir dize belirtilen alt dizeyi içeriyorsa, bir dizi belirtilen değeri içeriyorsa veya bir nesne belirtilen anahtarı içeriyorsa döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek şunu döndürür `true` :

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `false` :

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Kabul edilecek `element1` değer:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek şunu döndürür `true` :

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a>length
Bir dizedeki karakter sayısını, bir dizideki değerlerin sayısını veya bir nesnedeki anahtar sayısını döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek şunu döndürür `6` :

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `3` :

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Kabul edilecek `element1` değer:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek şunu döndürür `2` :

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a>empty
`true`Dize, dizi veya nesne null ya da boş ise döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek şunu döndürür `true` :

```json
"[empty('')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `false` :

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Kabul edilecek `element1` değer:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek şunu döndürür `false` :

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a>Örnek 4: null ve tanımsız
`element1` `null` Ya da tanımsız olduğunu varsayın. Aşağıdaki örnek şunu döndürür `true` :

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a>adı
Belirtilen dizenin ilk karakterini döndürür; belirtilen dizinin ilk değeri; veya belirtilen nesnenin ilk anahtarı ve değeri.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek şunu döndürür `"f"` :

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `1` :

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Kabul edilecek `element1` değer:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Aşağıdaki örnek şunu döndürür `{"key1": "foobar"}` :

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a>Son
Belirtilen dizenin son karakterini, belirtilen dizinin son değerini veya belirtilen nesnenin son anahtarını ve değerini döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek şunu döndürür `"r"` :

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `2` :

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Kabul edilecek `element1` değer:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek şunu döndürür `{"key2": "raboof"}` :

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a>almanız
Dizenin başından itibaren belirtilen sayıda bitişik karakteri, dizinin başından itibaren belirtilen sayıda bitişik değeri ya da nesnenin başından itibaren belirtilen sayıda bitişik anahtar ve değeri döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek şunu döndürür `"foo"` :

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `[1, 2]` :

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Kabul edilecek `element1` değer:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

Aşağıdaki örnek şunu döndürür `{"key1": "foobar"}` :

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a>Atla
Bir koleksiyonda belirtilen sayıda öğeyi atlar ve kalan öğeleri döndürür.

#### <a name="example-1-string"></a>Örnek 1: dize
Aşağıdaki örnek şunu döndürür `"bar"` :

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a>Örnek 2: dizi
`element1`Değer döndürür `[1, 2, 3]` . Aşağıdaki örnek şunu döndürür `[3]` :

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Örnek 3: nesne
Kabul edilecek `element1` değer:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
Aşağıdaki örnek şunu döndürür `{"key2": "raboof"}` :

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a>Mantıksal işlevler
Bu işlevler, conditionals içinde kullanılabilir. Bazı işlevler tüm JSON veri türlerini desteklemiyor olabilir.

### <a name="equals"></a>eşittir
`true`Her iki parametrenin de aynı tür ve değere sahip olup olmadığını döndürür. Bu işlev tüm JSON veri türlerini destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[equals(0, 0)]"
```

Aşağıdaki örnek şunu döndürür `true` :

```json
"[equals('foo', 'foo')]"
```

Aşağıdaki örnek şunu döndürür `false` :

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a>daha az
`true`İlk parametrenin ikinci parametreden kesinlikle daha az olup olmadığını döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[less(1, 2)]"
```

Aşağıdaki örnek şunu döndürür `false` :

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a>lessOrEquals
`true`İlk parametrenin ikinci parametreye eşit veya ondan küçük olup olmadığını döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a>büyüktür
`true`İlk parametrenin ikinci parametreden kesinlikle büyük olup olmadığını döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `false` :

```json
"[greater(1, 2)]"
```

Aşağıdaki örnek şunu döndürür `true` :

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a>greaterOrEquals
`true`İlk parametrenin ikinci parametreye eşit veya ondan büyük olup olmadığını döndürür. Bu işlev yalnızca number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a>ve
`true`Tüm parametrelerin değerlendirmesi halinde döndürür `true` . Bu işlev, yalnızca Boole türünden iki veya daha fazla parametreyi destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Aşağıdaki örnek şunu döndürür `false` :

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a>veya
`true`Parametrelerden en az biri olarak değerlendiriliyorsa, döndürür `true` . Bu işlev, yalnızca Boole türünden iki veya daha fazla parametreyi destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

Aşağıdaki örnek şunu döndürür `true` :

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a>not
`true`Parametresi olarak değerlendirilirse döndürür `false` . Bu işlev yalnızca Boole türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `true` :

```json
"[not(false)]"
```

Aşağıdaki örnek şunu döndürür `false` :

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a>Coalesce
İlk null olmayan parametrenin değerini döndürür. Bu işlev tüm JSON veri türlerini destekler.

`element1`Ve tanımsız olduğunu varsayın `element2` . Aşağıdaki örnek şunu döndürür `"foobar"` :

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

Bu işlev özellikle, sayfa yüklendikten sonra Kullanıcı eylemi nedeniyle gerçekleşen isteğe bağlı çağrının bağlamında faydalıdır. Bir örnek, Kullanıcı arabirimindeki bir alana yerleştirilmiş olan kısıtlamaların, **Başlangıçta görünür olmayan** başka bir alanın şu anda seçili olan değerine bağlıdır. Bu durumda, `coalesce()` Kullanıcı alanla etkileşime geçtiğinde istenen etkiye sahip olsa da, işlevin sayfa yükleme sırasında sözdizimsel olarak geçerli olmasına izin vermek için kullanılabilir.

`DropDown`Kullanıcının birkaç farklı veritabanı türünden seçmesine izin veren bunu göz önünde bulundurun:

```
{
    "name": "databaseType",
    "type": "Microsoft.Common.DropDown",
    "label": "Choose database type",
    "toolTip": "Choose database type",
    "defaultValue": "Oracle Database",
    "visible": "[bool(steps('section_database').connectToDatabase)]"
    "constraints": {
        "allowedValues": [
            {
                "label": "Azure Database for PostgreSQL",
                "value": "postgresql"
            },
            {
                "label": "Oracle Database",
                "value": "oracle"
            },
            {
                "label": "Azure SQL",
                "value": "sqlserver"
            }
        ],
        "required": true
    },
```

Bu alanın geçerli seçilen değerindeki başka bir alanın eylemini yapmak için, `coalesce()` burada gösterildiği gibi kullanın:

```
"regex": "[concat('^jdbc:', coalesce(steps('section_database').databaseConnectionInfo.databaseType, ''), '.*$')]",
```

Bu, `databaseType` Başlangıçta görünür olmadığından ve bu nedenle bir değere sahip olmadığından gereklidir. Bu, tüm ifadenin doğru değerlendirilmesine neden olur.

## <a name="conversion-functions"></a>Dönüştürme işlevleri
Bu işlevler, JSON veri türleri ve kodlamaları arasındaki değerleri dönüştürmek için kullanılabilir.

### <a name="int"></a>int
Parametreyi bir tamsayıya dönüştürür. Bu işlev, Number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `1` :

```json
"[int('1')]"
```

Aşağıdaki örnek şunu döndürür `2` :

```json
"[int(2.9)]"
```

### <a name="float"></a>float
Parametreyi bir kayan noktaya dönüştürür. Bu işlev, Number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `1.0` :

```json
"[float('1.0')]"
```

Aşağıdaki örnek şunu döndürür `2.9` :

```json
"[float(2.9)]"
```

### <a name="string"></a>string
Parametreyi bir dizeye dönüştürür. Bu işlev tüm JSON veri türlerinin parametrelerini destekler.

Aşağıdaki örnek şunu döndürür `"1"` :

```json
"[string(1)]"
```

Aşağıdaki örnek şunu döndürür `"2.9"` :

```json
"[string(2.9)]"
```

Aşağıdaki örnek şunu döndürür `"[1,2,3]"` :

```json
"[string([1,2,3])]"
```

Aşağıdaki örnek şunu döndürür `"{"foo":"bar"}"` :

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a>bool
Parametreyi Boole değerine dönüştürür. Bu işlev Number, String ve Boolean türündeki parametreleri destekler. JavaScript 'teki Boole değerleri, veya döndüren herhangi bir `0` değer `'false'` `true` .

Aşağıdaki örnek şunu döndürür `true` :

```json
"[bool(1)]"
```

Aşağıdaki örnek şunu döndürür `false` :

```json
"[bool(0)]"
```

Aşağıdaki örnek şunu döndürür `true` :

```json
"[bool(true)]"
```

Aşağıdaki örnek şunu döndürür `true` :

```json
"[bool('true')]"
```

### <a name="parse"></a>parse
Parametreyi yerel bir türe dönüştürür. Diğer bir deyişle, bu işlev tersidir `string()` . Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `1` :

```json
"[parse('1')]"
```

Aşağıdaki örnek şunu döndürür `true` :

```json
"[parse('true')]"
```

Aşağıdaki örnek şunu döndürür `[1,2,3]` :

```json
"[parse('[1,2,3]')]"
```

Aşağıdaki örnek şunu döndürür `{"foo":"bar"}` :

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a>encodeBase64
Parametreyi bir Base-64 kodlu dize olarak kodlar. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `"Zm9vYmFy"` :

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a>decodeBase64
Base-64 kodlu dizeden parametrenin kodunu çözer. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `"foobar"` :

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a>encodeURIComponent
Parametreyi bir URL kodlamalı dize olarak kodlar. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `"https%3A%2F%2Fportal.azure.com%2F"` :

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a>decodeURIComponent
URL kodlamalı bir dizeden parametrenin kodunu çözer. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `"https://portal.azure.com/"` :

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a>Matematik İşlevleri
### <a name="add"></a>add
İki sayı ekler ve sonucu döndürür.

Aşağıdaki örnek şunu döndürür `3` :

```json
"[add(1, 2)]"
```

### <a name="sub"></a>alt
İkinci sayıyı ilk sayıdan çıkartır ve sonucu döndürür.

Aşağıdaki örnek şunu döndürür `1` :

```json
"[sub(3, 2)]"
```

### <a name="mul"></a>MUL
İki sayıyı çarpar ve sonucu döndürür.

Aşağıdaki örnek şunu döndürür `6` :

```json
"[mul(2, 3)]"
```

### <a name="div"></a>div
İlk sayıyı ikinci sayıya böler ve sonucu döndürür. Sonuç her zaman bir tamsayıdır.

Aşağıdaki örnek şunu döndürür `2` :

```json
"[div(6, 3)]"
```

### <a name="mod"></a>alma
İlk sayıyı ikinci sayıya böler ve kalanı döndürür.

Aşağıdaki örnek şunu döndürür `0` :

```json
"[mod(6, 3)]"
```

Aşağıdaki örnek şunu döndürür `2` :

```json
"[mod(6, 4)]"
```

### <a name="min"></a>dk
İki sayının küçük sayısını döndürür.

Aşağıdaki örnek şunu döndürür `1` :

```json
"[min(1, 2)]"
```

### <a name="max"></a>max
İki sayıdan daha büyük bir değer döndürür.

Aşağıdaki örnek şunu döndürür `2` :

```json
"[max(1, 2)]"
```

### <a name="range"></a>aralık
Belirtilen Aralık içinde bir integral sayı dizisi oluşturur.

Aşağıdaki örnek şunu döndürür `[1,2,3]` :

```json
"[range(1, 3)]"
```

### <a name="rand"></a>rand
Belirtilen Aralık içinde rastgele bir integral sayı döndürür. Bu işlev, şifreli olarak güvenli rastgele sayılar oluşturmaz.

Aşağıdaki örnek şunu döndürebilir `42` :

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a>sını
Belirtilen sayıdan küçük veya eşit en büyük tamsayıyı döndürür.

Aşağıdaki örnek şunu döndürür `3` :

```json
"[floor(3.14)]"
```

### <a name="ceil"></a>Ceil
Belirtilen sayıdan büyük veya eşit en büyük tamsayıyı döndürür.

Aşağıdaki örnek şunu döndürür `4` :

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a>Tarih işlevleri
### <a name="utcnow"></a>utcNow
Yerel bilgisayardaki geçerli tarih ve saatin ISO 8601 biçiminde bir dize döndürür.

Aşağıdaki örnek şunu döndürebilir `"1990-12-31T23:59:59.000Z"` :

```json
"[utcNow()]"
```

### <a name="addseconds"></a>addSeconds
Belirtilen zaman damgasına tamsayı cinsinden bir sayı ekler.

Aşağıdaki örnek şunu döndürür `"1991-01-01T00:00:00.000Z"` :

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a>addMinutes
Belirtilen zaman damgasına tamsayı cinsinden bir sayı ekler.

Aşağıdaki örnek şunu döndürür `"1991-01-01T00:00:59.000Z"` :

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a>addHours
Belirtilen zaman damgasına tamsayı saat sayısını ekler.

Aşağıdaki örnek şunu döndürür `"1991-01-01T00:59:59.000Z"` :

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Resource Manager giriş için bkz. [Azure Resource Manager genel bakış](../management/overview.md).

