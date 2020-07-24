---
title: UI tanımı işlevleri oluşturma
description: Azure yönetilen uygulamalar için Kullanıcı arabirimi tanımları oluştururken kullanılacak işlevleri açıklar
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: e4255f0d42e28a72ad55d9b7f81d0dc49b2950cb
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87040988"
---
# <a name="createuidefinition-functions"></a>Createuıdefinition işlevleri

Bu makalede Createuıdefinition için desteklenen işlevlere genel bir bakış sunulmaktadır.

## <a name="function-syntax"></a>İşlev sözdizimi

Bir işlevi kullanmak için, çağrıyı köşeli ayraçla çevreleyin. Örneğin:

```json
"[function()]"
```

Dizeler ve diğer işlevlere bir işlev için parametre olarak başvurulabilir, ancak dizeler tek tırnak işaretleri içine alınmalıdır. Örneğin:

```json
"[fn1(fn2(), 'demo text')]"
```

Uygun olduğunda, nokta işlecini kullanarak bir işlevin çıktısının özelliklerine başvurabilirsiniz. Örneğin:

```json
"[func().prop1]"
```

## <a name="collection-functions"></a>Koleksiyon işlevleri

Bu işlevler, JSON dizeleri, diziler ve nesneler gibi koleksiyonlarla birlikte kullanılabilir.

* [vardır](create-ui-definition-collection-functions.md#contains)
* [empty](create-ui-definition-collection-functions.md#empty)
* [filtreyle](create-ui-definition-collection-functions.md#filter)
* [adı](create-ui-definition-collection-functions.md#first)
* [soyadına](create-ui-definition-collection-functions.md#last)
* [uzunluklu](create-ui-definition-collection-functions.md#length)
* [harita](create-ui-definition-collection-functions.md#map)
* [Şimdilik](create-ui-definition-collection-functions.md#skip)
* [ayırmayı](create-ui-definition-collection-functions.md#split)
* [take](create-ui-definition-collection-functions.md#take)

## <a name="conversion-functions"></a>Dönüştürme işlevleri

Bu işlevler, JSON veri türleri ve kodlamaları arasındaki değerleri dönüştürmek için kullanılabilir.

* [bool](create-ui-definition-conversion-functions.md#bool)
* [decodeBase64](create-ui-definition-conversion-functions.md#decodebase64)
* [decodeURIComponent](create-ui-definition-conversion-functions.md#decodeuricomponent)
* [encodeBase64](create-ui-definition-conversion-functions.md#encodebase64)
* [encodeURIComponent](create-ui-definition-conversion-functions.md#encodeuricomponent)
* [float](create-ui-definition-conversion-functions.md#float)
* [int](create-ui-definition-conversion-functions.md#int)
* [MAZ](create-ui-definition-conversion-functions.md#parse)
* [dizisinde](create-ui-definition-conversion-functions.md#string)

## <a name="date-functions"></a>Tarih işlevleri

* [addHours](create-ui-definition-date-functions.md#addhours)
* [addMinutes](create-ui-definition-date-functions.md#addminutes)
* [addSeconds](create-ui-definition-date-functions.md#addseconds)
* [utcNow](create-ui-definition-date-functions.md#utcnow)

## <a name="logical-functions"></a>Mantıksal işlevler

Bu işlevler, conditionals içinde kullanılabilir. Bazı işlevler tüm JSON veri türlerini desteklemiyor olabilir.

* [ve](create-ui-definition-logical-functions.md#and)
* [Coalesce](create-ui-definition-logical-functions.md#coalesce)
* [eşittir](create-ui-definition-logical-functions.md#equals)
* [büyüktür](create-ui-definition-logical-functions.md#greater)
* [greaterOrEquals](create-ui-definition-logical-functions.md#greaterorequals)
* [if](create-ui-definition-logical-functions.md#if)
* [büyüktür](create-ui-definition-logical-functions.md#less)
* [lessOrEquals](create-ui-definition-logical-functions.md#lessorequals)
* [not](create-ui-definition-logical-functions.md#not)
* [veya](create-ui-definition-logical-functions.md#or)

## <a name="math-functions"></a>Matematik İşlevleri

* [add](create-ui-definition-math-functions.md#add)
* [Ceil](create-ui-definition-math-functions.md#ceil)
* [div](create-ui-definition-math-functions.md#div)
* [sını](create-ui-definition-math-functions.md#floor)
* [Biçimlendir](create-ui-definition-math-functions.md#max)
* [dk](create-ui-definition-math-functions.md#min)
* [alma](create-ui-definition-math-functions.md#mod)
* [MUL](create-ui-definition-math-functions.md#mul)
* [ran](create-ui-definition-math-functions.md#rand)
* [aralığı](create-ui-definition-math-functions.md#range)
* [alt](create-ui-definition-math-functions.md#sub)

## <a name="referencing-functions"></a>Başvuru işlevleri

* [temel bilgileri](create-ui-definition-referencing-functions.md#basics)
* [konumuna](create-ui-definition-referencing-functions.md#location)
* [resourceGroup](create-ui-definition-referencing-functions.md#resourcegroup)
* [olanları](create-ui-definition-referencing-functions.md#steps)
* [aboneliğiniz](create-ui-definition-referencing-functions.md#subscription)

## <a name="string-functions"></a>Dize işlevleri

* [Concat](create-ui-definition-string-functions.md#concat)
* [endsWith](create-ui-definition-string-functions.md#endswith)
* [guid](create-ui-definition-string-functions.md#guid)
* [IndexOf](create-ui-definition-string-functions.md#indexof)
* [LastIndexOf](create-ui-definition-string-functions.md#lastindexof)
* [değiştirin](create-ui-definition-string-functions.md#replace)
* [startsWith](create-ui-definition-string-functions.md#startswith)
* [dizeden](create-ui-definition-string-functions.md#substring)
* [toLower](create-ui-definition-string-functions.md#tolower)
* [toUpper](create-ui-definition-string-functions.md#toupper)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager giriş için bkz. [Azure Resource Manager genel bakış](../management/overview.md).
