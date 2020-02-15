---
title: Şablon işlevleri
description: Değerleri almak, dizeler ve sayı ile çalışmak ve dağıtım bilgilerini almak için Azure Resource Manager şablonda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: a9d10ad4899f35acd45069cb3d351a60632fed3a
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77207051"
---
# <a name="azure-resource-manager-template-functions"></a>Azure Resource Manager şablon işlevleri

Bu makalede, bir Azure Resource Manager şablonunda kullanabileceğiniz tüm işlevler açıklanmaktadır. Şablonunuzda işlevleri kullanma hakkında daha fazla bilgi için bkz. [şablon sözdizimi](template-expressions.md).

Kendi işlevlerinizi oluşturmak için, bkz. [Kullanıcı tanımlı işlevler](template-syntax.md#functions).

Çoğu işlev bir kaynak grubuna, aboneliğe, yönetim grubuna veya kiracıya dağıtıldığında aynı şekilde çalışır. Birkaç işlev tüm kapsamlarında kullanılamaz. Bunlar aşağıdaki listelerde belirtilmiştir.

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

* [dizide](template-functions-array.md#array)
* [Coalesce](template-functions-array.md#coalesce)
* [Concat](template-functions-array.md#concat)
* [vardır](template-functions-array.md#contains)
* [createArray](template-functions-array.md#createarray)
* [olmamalıdır](template-functions-array.md#empty)
* [adı](template-functions-array.md#first)
* [imin](template-functions-array.md#intersection)
* [nesnesinde](template-functions-array.md#json)
* [soyadına](template-functions-array.md#last)
* [uzunluklu](template-functions-array.md#length)
* [Min](template-functions-array.md#min)
* [Biçimlendir](template-functions-array.md#max)
* [aralığı](template-functions-array.md#range)
* [Şimdilik](template-functions-array.md#skip)
* [almanız](template-functions-array.md#take)
* [birleşim](template-functions-array.md#union)

<a id="equals" aria-hidden="true" />
<a id="less" aria-hidden="true" />
<a id="lessorequals" aria-hidden="true" />
<a id="greater" aria-hidden="true" />
<a id="greaterorequals" aria-hidden="true" />

## <a name="comparison-functions"></a>Karşılaştırma işlevleri

Kaynak Yöneticisi, şablonlarınıza karşılaştırmalar yapmak için çeşitli işlevler sağlar.

* [eşittir](template-functions-comparison.md#equals)
* [büyüktür](template-functions-comparison.md#less)
* [Lessotalals](template-functions-comparison.md#lessorequals)
* [ilerisi](template-functions-comparison.md#greater)
* [greaterOrEquals](template-functions-comparison.md#greaterorequals)

<a id="deployment" aria-hidden="true" />
<a id="parameters" aria-hidden="true" />
<a id="variables" aria-hidden="true" />

## <a name="deployment-value-functions"></a>Dağıtım değeri işlevleri

Kaynak Yöneticisi, şablon bölümlerinden ve dağıtımla ilgili değerlerin değerlerini almak için aşağıdaki işlevleri sağlar:

* [dağıtmak](template-functions-deployment.md#deployment)
* [ortamınızın](template-functions-deployment.md#environment)
* [parametreler](template-functions-deployment.md#parameters)
* [değişkenlerinin](template-functions-deployment.md#variables)

<a id="and" aria-hidden="true" />
<a id="bool" aria-hidden="true" />
<a id="if" aria-hidden="true" />
<a id="not" aria-hidden="true" />
<a id="or" aria-hidden="true" />

## <a name="logical-functions"></a>Mantıksal işlevler

Kaynak Yöneticisi mantıksal koşullarla çalışmak için aşağıdaki işlevleri sağlar:

* ['](template-functions-logical.md#and)
* [bool](template-functions-logical.md#bool)
* [kullandıysanız](template-functions-logical.md#if)
* [başlatılmadı](template-functions-logical.md#not)
* [veya](template-functions-logical.md#or)

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

Kaynak Yöneticisi, tamsayılarla çalışmak için aşağıdaki işlevleri sağlar:

* [ekleyemiyorum](template-functions-numeric.md#add)
* [Copyındex](template-functions-numeric.md#copyindex)
* [DIV](template-functions-numeric.md#div)
* [float](template-functions-numeric.md#float)
* ['tir](template-functions-numeric.md#int)
* [Min](template-functions-numeric.md#min)
* [Biçimlendir](template-functions-numeric.md#max)
* [alma](template-functions-numeric.md#mod)
* [MUL](template-functions-numeric.md#mul)
* [alt](template-functions-numeric.md#sub)

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

Resource Manager kaynak değerlerini almak için aşağıdaki işlevleri sunar:

* [Extensionresourceıd](template-functions-resource.md#extensionresourceid)
* [listAccountSas](template-functions-resource.md#list)
* [ListKeys 'i al](template-functions-resource.md#listkeys)
* [Listgizlilikler](template-functions-resource.md#list)
* [Listele](template-functions-resource.md#list)
* [sağlayıcılarla](template-functions-resource.md#providers)
* [başvurunun](template-functions-resource.md#reference)
* [resourceGroup](template-functions-resource.md#resourcegroup) -yalnızca bir kaynak grubuna yapılan dağıtımlarda kullanılabilir.
* [RESOURCEID](template-functions-resource.md#resourceid) -herhangi bir kapsamda kullanılabilir, ancak geçerli parametreler kapsama göre değişir.
* [abonelik](template-functions-resource.md#subscription) -yalnızca bir kaynak grubuna veya aboneliğe yapılan dağıtımlarda kullanılabilir.
* [Subscriptionresourceıd](template-functions-resource.md#subscriptionresourceid)
* [Tenantresourceıd](template-functions-resource.md#tenantresourceid)

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

Kaynak Yöneticisi dizelerle çalışmak için aşağıdaki işlevleri sağlar:

* [biçiminde](template-functions-string.md#base64)
* [base64ToJson](template-functions-string.md#base64tojson)
* [base64ToString](template-functions-string.md#base64tostring)
* [Concat](template-functions-string.md#concat)
* [vardır](template-functions-string.md#contains)
* [dataUri](template-functions-string.md#datauri)
* [dataUriToString](template-functions-string.md#datauritostring)
* [olmamalıdır](template-functions-string.md#empty)
* [endsWith](template-functions-string.md#endswith)
* [adı](template-functions-string.md#first)
* [formatını](template-functions-string.md#format)
* ['ini](template-functions-string.md#guid)
* [IndexOf](template-functions-string.md#indexof)
* [soyadına](template-functions-string.md#last)
* [LastIndexOf](template-functions-string.md#lastindexof)
* [uzunluklu](template-functions-string.md#length)
* [newGuid](template-functions-string.md#newguid)
* [Asma sol](template-functions-string.md#padleft)
* [değiştirin](template-functions-string.md#replace)
* [Şimdilik](template-functions-string.md#skip)
* [split](template-functions-string.md#split)
* [startsWith](template-functions-string.md#startswith)
* [dizisinde](template-functions-string.md#string)
* [dizeden](template-functions-string.md#substring)
* [almanız](template-functions-string.md#take)
* [toLower](template-functions-string.md#tolower)
* [toUpper](template-functions-string.md#toupper)
* [kırpma](template-functions-string.md#trim)
* [uniqueString](template-functions-string.md#uniquestring)
* [kullanılmamışsa](template-functions-string.md#uri)
* [URIComponent](template-functions-string.md#uricomponent)
* [Urıonenttostring](template-functions-string.md#uricomponenttostring)
* [utcNow](template-functions-string.md#utcnow)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager şablonundaki bölümlerin açıklaması için bkz. [yazma Azure Resource Manager şablonları](template-syntax.md)
* Birden çok şablonu birleştirmek için bkz. [Azure Resource Manager ile bağlantılı şablonları kullanma](linked-templates.md)
* Kaynak türünü oluştururken belirtilen sayıda yinelemek için, bkz. [Azure Resource Manager birden fazla kaynak örneği oluşturma](copy-resources.md).
* Oluşturduğunuz şablonun nasıl dağıtılacağını görmek için bkz. [Azure Resource Manager şablonuyla uygulama dağıtma](deploy-powershell.md)
