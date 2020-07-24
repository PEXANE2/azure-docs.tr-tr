---
title: UI tanımı dönüştürme işlevleri oluştur
description: Veri türleri ve kodlamalar arasında değerleri dönüştürürken kullanılacak işlevleri açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: b69cd35b27b343da08727b4c4ee9b4fd025e1df7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099926"
---
# <a name="createuidefinition-conversion-functions"></a>Createuıdefinition dönüştürme işlevleri

Bu işlevler, JSON veri türleri ve kodlamaları arasındaki değerleri dönüştürmek için kullanılabilir.

## <a name="bool"></a>bool

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

## <a name="decodebase64"></a>decodeBase64

Base-64 kodlu dizeden parametrenin kodunu çözer. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `"Contoso"` :

```json
"[decodeBase64('Q29udG9zbw==')]"
```

## <a name="decodeuricomponent"></a>decodeURIComponent

URL kodlamalı bir dizeden parametrenin kodunu çözer. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `"https://portal.azure.com/"` :

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="encodebase64"></a>encodeBase64

Parametreyi bir Base-64 kodlu dize olarak kodlar. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `"Q29udG9zbw=="` :

```json
"[encodeBase64('Contoso')]"
```

## <a name="encodeuricomponent"></a>encodeURIComponent

Parametreyi bir URL kodlamalı dize olarak kodlar. Bu işlev yalnızca dize türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `"https%3A%2F%2Fportal.azure.com%2F"` :

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

## <a name="float"></a>float

Parametreyi bir kayan noktaya dönüştürür. Bu işlev, Number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `1.0` :

```json
"[float('1.0')]"
```

Aşağıdaki örnek şunu döndürür `2.9` :

```json
"[float(2.9)]"
```

## <a name="int"></a>int

Parametreyi bir tamsayıya dönüştürür. Bu işlev, Number ve String türündeki parametreleri destekler.

Aşağıdaki örnek şunu döndürür `1` :

```json
"[int('1')]"
```

Aşağıdaki örnek şunu döndürür `2` :

```json
"[int(2.9)]"
```

## <a name="parse"></a>parse

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

Aşağıdaki örnek şunu döndürür `{"type":"webapp"}` :

```json
"[parse('{\"type\":\"webapp\"}')]"
```

## <a name="string"></a>string

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

Aşağıdaki örnek şunu döndürür `"{"type":"webapp"}"` :

```json
"[string({\"type\":\"webapp\"})]"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager giriş için bkz. [Azure Resource Manager genel bakış](../management/overview.md).
