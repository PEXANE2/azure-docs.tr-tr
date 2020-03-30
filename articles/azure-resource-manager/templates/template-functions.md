---
title: Şablon işlevleri
description: Değerleri almak, dizeleri ve sayısal larla çalışmak ve dağıtım bilgilerini almak için Azure Kaynak Yöneticisi şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 1d2789e59c091b4e6c39be48b83fe610a592abe1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156268"
---
# <a name="arm-template-functions"></a>ARM şablon fonksiyonları

Bu makalede, bir Azure Kaynak Yöneticisi (ARM) şablonunda kullanabileceğiniz tüm işlevler açıklanmaktadır. Şablonunuzdaki işlevleri kullanma hakkında bilgi için [şablon sözdizimine](template-expressions.md)bakın.

Kendi işlevlerinizi oluşturmak için [Kullanıcı tanımlı işlevlere](template-syntax.md#functions)bakın.

Çoğu işlev, bir kaynak grubuna, abonelikte, yönetim grubuna veya kiracıya dağıtıldığında aynı şekilde çalışır. Birkaç işlev tüm kapsamlarda kullanılamaz. Aşağıdaki listelerde belirtilmiştir.

<a id="array" aria-hidden="true" />
<a id="coalesce" aria-hidden="true" />
<a id="concatarray" aria-hidden="true" />
<a id="contains" aria-hidden="true" />
<a id="createarray" aria-hidden="true" />
<a id="empty" aria-hidden="true" />
<a id="first" aria-hidden="true" />
<a id="intersection" aria-hidden="true" />
<a id="json" aria-hidden="true" />
<a id="last" aria-hidden="true" />
<a id="length" aria-hidden="true" />
<a id="min" aria-hidden="true" />
<a id="max" aria-hidden="true" />
<a id="range" aria-hidden="true" />
<a id="skip" aria-hidden="true" />
<a id="take" aria-hidden="true" />
<a id="union" aria-hidden="true" />

## <a name="array-and-object-functions"></a>Dizi ve nesne işlevleri

Kaynak Yöneticisi diziler ve nesnelerle çalışmak için çeşitli işlevler sağlar.

* [Dizi](template-functions-array.md#array)
* [Coalesce](template-functions-array.md#coalesce)
* [Concat](template-functions-array.md#concat)
* [Içerir](template-functions-array.md#contains)
* [createArray](template-functions-array.md#createarray)
* [empty](template-functions-array.md#empty)
* [Ilk](template-functions-array.md#first)
* [Kesişim](template-functions-array.md#intersection)
* [Json](template-functions-array.md#json)
* [Son](template-functions-array.md#last)
* [Uzun -luğu](template-functions-array.md#length)
* [Dk](template-functions-array.md#min)
* [Max](template-functions-array.md#max)
* [Aralığı](template-functions-array.md#range)
* [Atlamak](template-functions-array.md#skip)
* [almak](template-functions-array.md#take)
* [Birliği](template-functions-array.md#union)

<a id="equals" aria-hidden="true" />
<a id="less" aria-hidden="true" />
<a id="lessorequals" aria-hidden="true" />
<a id="greater" aria-hidden="true" />
<a id="greaterorequals" aria-hidden="true" />

## <a name="comparison-functions"></a>Karşılaştırma işlevleri

Kaynak Yöneticisi şablonlarınızda karşılaştırma yapmak için çeşitli işlevler sağlar.

* [equals](template-functions-comparison.md#equals)
* [daha az](template-functions-comparison.md#less)
* [lessOrEquals](template-functions-comparison.md#lessorequals)
* [büyüktür](template-functions-comparison.md#greater)
* [greaterOrEquals](template-functions-comparison.md#greaterorequals)

<a id="deployment" aria-hidden="true" />
<a id="parameters" aria-hidden="true" />
<a id="variables" aria-hidden="true" />

## <a name="deployment-value-functions"></a>Dağıtım değeri fonksiyonları

Kaynak Yöneticisi, şablonun bölümlerinden ve dağıtımla ilgili değerlerden değerler almak için aşağıdaki işlevleri sağlar:

* [Dağıtım](template-functions-deployment.md#deployment)
* [Ortam](template-functions-deployment.md#environment)
* [Parametre](template-functions-deployment.md#parameters)
* [Değişken](template-functions-deployment.md#variables)

<a id="and" aria-hidden="true" />
<a id="bool" aria-hidden="true" />
<a id="if" aria-hidden="true" />
<a id="not" aria-hidden="true" />
<a id="or" aria-hidden="true" />

## <a name="logical-functions"></a>Mantıksal işlevler

Kaynak Yöneticisi mantıksal koşullarla çalışmak için aşağıdaki işlevleri sağlar:

* [Ve](template-functions-logical.md#and)
* [bool](template-functions-logical.md#bool)
* [if](template-functions-logical.md#if)
* [Değil](template-functions-logical.md#not)
* [Veya](template-functions-logical.md#or)

<a id="add" aria-hidden="true" />
<a id="copyindex" aria-hidden="true" />
<a id="div" aria-hidden="true" />
<a id="float" aria-hidden="true" />
<a id="int" aria-hidden="true" />
<a id="minint" aria-hidden="true" />
<a id="maxint" aria-hidden="true" />
<a id="mod" aria-hidden="true" />
<a id="mul" aria-hidden="true" />
<a id="sub" aria-hidden="true" />

## <a name="numeric-functions"></a>Sayısal işlevler

Kaynak Yöneticisi, tümselerle çalışmak için aşağıdaki işlevleri sağlar:

* [add](template-functions-numeric.md#add)
* [copyIndex](template-functions-numeric.md#copyindex)
* [div](template-functions-numeric.md#div)
* [float](template-functions-numeric.md#float)
* [int](template-functions-numeric.md#int)
* [Dk](template-functions-numeric.md#min)
* [Max](template-functions-numeric.md#max)
* [mod](template-functions-numeric.md#mod)
* [Mul](template-functions-numeric.md#mul)
* [Alt](template-functions-numeric.md#sub)

<a id="extensionResourceId" aria-hidden="true" />
<a id="listkeys" aria-hidden="true" />
<a id="list" aria-hidden="true" />
<a id="providers" aria-hidden="true" />
<a id="reference" aria-hidden="true" />
<a id="resourcegroup" aria-hidden="true" />
<a id="resourceid" aria-hidden="true" />
<a id="subscription" aria-hidden="true" />
<a id="subscriptionResourceId" aria-hidden="true" />
<a id="tenantResourceId" aria-hidden="true" />

## <a name="resource-functions"></a>Kaynak işlevleri

Kaynak Yöneticisi kaynak değerleri almak için aşağıdaki işlevleri sağlar:

* [uzantısıResourceId](template-functions-resource.md#extensionresourceid)
* [listeAccountSas](template-functions-resource.md#list)
* [listeTuşları](template-functions-resource.md#listkeys)
* [listSecrets](template-functions-resource.md#list)
* [liste*](template-functions-resource.md#list)
* [Sağlayıcı](template-functions-resource.md#providers)
* [Başvuru](template-functions-resource.md#reference)
* [resourceGroup](template-functions-resource.md#resourcegroup) - yalnızca bir kaynak grubuna dağıtımlarda kullanılabilir.
* [resourceId](template-functions-resource.md#resourceid) - herhangi bir kapsamda kullanılabilir, ancak geçerli parametreler ilerki kapsamına bağlı olarak değişir.
* [abonelik](template-functions-resource.md#subscription) - yalnızca bir kaynak grubuna veya aboneye dağıtımlarda kullanılabilir.
* [abonelikResourceId](template-functions-resource.md#subscriptionresourceid)
* [tenantResourceId](template-functions-resource.md#tenantresourceid)

<a id="base64" aria-hidden="true" />
<a id="base64tojson" aria-hidden="true" />
<a id="base64tostring" aria-hidden="true" />
<a id="concat" aria-hidden="true" />
<a id="containsstring" aria-hidden="true" />
<a id="datauri" aria-hidden="true" />
<a id="datauritostring" aria-hidden="true" />
<a id="emptystring" aria-hidden="true" />
<a id="endswith" aria-hidden="true" />
<a id="firststring" aria-hidden="true" />
<a id="guid" aria-hidden="true" />
<a id="indexof" aria-hidden="true" />
<a id="laststring" aria-hidden="true" />
<a id="lastindexof" aria-hidden="true" />
<a id="lengthstring" aria-hidden="true" />
<a id="padleft" aria-hidden="true" />
<a id="replace" aria-hidden="true" />
<a id="skipstring" aria-hidden="true" />
<a id="split" aria-hidden="true" />
<a id="startswith" aria-hidden="true" />
<a id="string" aria-hidden="true" />
<a id="substring" aria-hidden="true" />
<a id="takestring" aria-hidden="true" />
<a id="tolower" aria-hidden="true" />
<a id="toupper" aria-hidden="true" />
<a id="trim" aria-hidden="true" />
<a id="uniquestring" aria-hidden="true" />
<a id="uri" aria-hidden="true" />
<a id="uricomponent" aria-hidden="true" />
<a id="uricomponenttostring" aria-hidden="true" />

## <a name="string-functions"></a>Dize işlevleri

Kaynak Yöneticisi dizeleri ile çalışmak için aşağıdaki işlevleri sağlar:

* [base64](template-functions-string.md#base64)
* [base64ToJson](template-functions-string.md#base64tojson)
* [base64ToString](template-functions-string.md#base64tostring)
* [Concat](template-functions-string.md#concat)
* [Içerir](template-functions-string.md#contains)
* [dataUri](template-functions-string.md#datauri)
* [dataUritostring](template-functions-string.md#datauritostring)
* [empty](template-functions-string.md#empty)
* [Endswith](template-functions-string.md#endswith)
* [Ilk](template-functions-string.md#first)
* [Biçim](template-functions-string.md#format)
* [Guıd](template-functions-string.md#guid)
* [ındexof](template-functions-string.md#indexof)
* [Son](template-functions-string.md#last)
* [Lastındexof](template-functions-string.md#lastindexof)
* [Uzun -luğu](template-functions-string.md#length)
* [newGuid](template-functions-string.md#newguid)
* [Padleft](template-functions-string.md#padleft)
* [Değiştirmek](template-functions-string.md#replace)
* [Atlamak](template-functions-string.md#skip)
* [Split](template-functions-string.md#split)
* [Startswith](template-functions-string.md#startswith)
* [Dize](template-functions-string.md#string)
* [Dize](template-functions-string.md#substring)
* [almak](template-functions-string.md#take)
* [Tolower](template-functions-string.md#tolower)
* [Toupper](template-functions-string.md#toupper)
* [Döşeme](template-functions-string.md#trim)
* [uniqueString](template-functions-string.md#uniquestring)
* [Urı](template-functions-string.md#uri)
* [uriComponent](template-functions-string.md#uricomponent)
* [uriComponentToString](template-functions-string.md#uricomponenttostring)
* [utcŞimdi](template-functions-string.md#utcnow)

## <a name="next-steps"></a>Sonraki adımlar

* ARM şablonundaki bölümlerin açıklaması için [bkz.](template-syntax.md)
* Birden çok şablonu birleştirmek için bkz: [Azure Kaynak Yöneticisi ile bağlantılı şablonları kullanma](linked-templates.md)
* Bir kaynak türü oluştururken belirli sayıda kez yeniden sıralamak için azure [kaynak yöneticisinde birden çok kaynak örneği oluşturma](copy-resources.md)bölümüne bakın.
* Oluşturduğunuz şablonu nasıl dağıtabileceğinizi görmek için [bkz.](deploy-powershell.md)
