---
title: UI tanımı matematik işlevleri oluştur
description: Matematik işlemleri gerçekleştirirken kullanılacak işlevleri açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: 022e59d847a4d89b4243223637fc6fd1e73255a9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099917"
---
# <a name="createuidefinition-math-functions"></a>Createuıdefinition matematik işlevleri

İşlevleri matematik işlemleri gerçekleştirmenize olanak tanır.

## <a name="add"></a>add

İki sayı ekler ve sonucu döndürür.

Aşağıdaki örnek şunu döndürür `3` :

```json
"[add(1, 2)]"
```

## <a name="ceil"></a>Ceil

Belirtilen sayıdan büyük veya eşit en büyük tamsayıyı döndürür.

Aşağıdaki örnek şunu döndürür `4` :

```json
"[ceil(3.14)]"
```

## <a name="div"></a>div

İlk sayıyı ikinci sayıya böler ve sonucu döndürür. Sonuç her zaman bir tamsayıdır.

Aşağıdaki örnek şunu döndürür `2` :

```json
"[div(6, 3)]"
```

## <a name="floor"></a>sını

Belirtilen sayıdan küçük veya eşit en büyük tamsayıyı döndürür.

Aşağıdaki örnek şunu döndürür `3` :

```json
"[floor(3.14)]"
```

## <a name="max"></a>max

İki sayıdan daha büyük bir değer döndürür.

Aşağıdaki örnek şunu döndürür `2` :

```json
"[max(1, 2)]"
```

## <a name="min"></a>dk

İki sayıdan daha küçük bir değer döndürür.

Aşağıdaki örnek şunu döndürür `1` :

```json
"[min(1, 2)]"
```

## <a name="mod"></a>alma

İlk sayıyı ikinci sayıya böler ve kalanı döndürür.

Aşağıdaki örnek şunu döndürür `0` :

```json
"[mod(6, 3)]"
```

Aşağıdaki örnek şunu döndürür `2` :

```json
"[mod(6, 4)]"
```

## <a name="mul"></a>MUL

İki sayıyı çarpar ve sonucu döndürür.

Aşağıdaki örnek şunu döndürür `6` :

```json
"[mul(2, 3)]"
```

## <a name="rand"></a>rand

Belirtilen Aralık içinde rastgele bir integral sayı döndürür. Bu işlev, şifreli olarak güvenli rastgele sayılar oluşturmaz.

Aşağıdaki örnek şunu döndürebilir `42` :

```json
"[rand(-100, 100)]"
```

## <a name="range"></a>aralık

Belirtilen Aralık içinde bir integral sayı dizisi oluşturur.

Aşağıdaki örnek şunu döndürür `[1,2,3]` :

```json
"[range(1, 3)]"
```

## <a name="sub"></a>alt

İkinci sayıyı ilk sayıdan çıkartır ve sonucu döndürür.

Aşağıdaki örnek şunu döndürür `1` :

```json
"[sub(3, 2)]"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager giriş için bkz. [Azure Resource Manager genel bakış](../management/overview.md).
