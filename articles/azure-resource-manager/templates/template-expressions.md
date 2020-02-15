---
title: Şablon sözdizimi ve ifadeleri
description: Azure Resource Manager şablonları için bildirim temelli JSON sözdizimini açıklar.
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 7bca3125f80225d2180734f483194a63e39d9cf5
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77207409"
---
# <a name="syntax-and-expressions-in-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarındaki sözdizimi ve ifadeler

Şablonun temel sözdizimi JSON ' dır. Ancak, şablon içinde kullanılabilir olan JSON değerlerini genişletmek için ifadeleri kullanabilirsiniz.  İfadeler, sırasıyla `[` ve `]`parantez ile başlayıp biter. İfade değeri, şablon dağıtıldığında değerlendirilir. Bir ifade dize, tamsayı, Boolean, dizi veya nesne döndürebilir.

Şablon ifadesi 24.576 karakterden uzun olamaz.

## <a name="use-functions"></a>İşlev kullanma

Azure Resource Manager, bir şablonda kullanabileceğiniz [işlevleri](template-functions.md) sağlar. Aşağıdaki örnek, bir parametresinin varsayılan değerinde bir işlevi kullanan bir ifade gösterir:

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
```

İfade içinde `resourceGroup()` sözdizimi, Kaynak Yöneticisi bir şablon içinde kullanım için sağladığı işlevlerden birini çağırır. Bu durumda, [resourceGroup](template-functions-resource.md#resourcegroup) işlevi. JavaScript içinde olduğu gibi, işlev çağrıları `functionName(arg1,arg2,arg3)`olarak biçimlendirilir. Söz dizimi `.location`, bu işlev tarafından döndürülen nesneden bir özelliği alır.

Şablon işlevleri ve parametreleri büyük/küçük harfe duyarlıdır. Örneğin, Kaynak Yöneticisi **değişkenleri (' var1 ')** ve **DEĞIŞKENLERI (' var1 ')** aynı şekilde çözümler. Değerlendirildiğinde, işlev açıkça büyük/küçük harf (toUpper veya toLower gibi) değiştirmediği sürece işlev, büyük/küçük harf durumunu korur. Belirli kaynak türlerinde, işlevlerin nasıl değerlendirildiğinden ayrı olarak durum gereksinimleri olabilir.

Bir parametreye parametre olarak bir dize değeri geçirmek için tek tırnak kullanın.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]"
```

Çoğu işlev aynı şekilde bir kaynak grubuna, aboneliğe, yönetim grubuna veya kiracıya dağıtılıp aynı şekilde çalışır. Aşağıdaki işlevlerde kapsama dayalı kısıtlamalar vardır:

* [resourceGroup](template-functions-resource.md#resourcegroup) -yalnızca bir kaynak grubuna yapılan dağıtımlarda kullanılabilir.
* [RESOURCEID](template-functions-resource.md#resourceid) -herhangi bir kapsamda kullanılabilir, ancak geçerli parametreler kapsama göre değişir.
* [abonelik](template-functions-resource.md#subscription) -yalnızca bir kaynak grubuna veya aboneliğe yapılan dağıtımlarda kullanılabilir.

## <a name="escape-characters"></a>Kaçış karakterleri

Değişmez bir dizenin bir sol köşeli `[` ayraç ile başlaması ve sağ köşeli ayraç `]`ile bitmesi, ancak bir ifade olarak yorumlanmaması için, dizeyi `[[`ile başlatmak için fazladan bir köşeli ayraç ekleyin. Örneğin, değişkeni:

```json
"demoVar1": "[[test value]"
```

`[test value]`çözümleniyor.

Ancak, değişmez dize bir köşeli ayraç ile bitmezse ilk köşeli ayracı atmayın. Örneğin, değişkeni:

```json
"demoVar2": "[test] value"
```

`[test] value`çözümleniyor.

Şablonda JSON nesnesi ekleme gibi bir ifadede çift tırnak işaretleri için ters eğik çizgi kullanın.

```json
"tags": {
    "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
},
```

## <a name="null-values"></a>Null değerler

Bir özelliği null olarak ayarlamak için **null** veya **[JSON (' null ')]** kullanabilirsiniz. [JSON işlevi](template-functions-array.md#json) , parametre olarak `null` sağladığınızda boş bir nesne döndürür. Her iki durumda da Kaynak Yöneticisi şablonlar, özelliği mevcut olmadığı gibi kabul eder.

```json
"stringValue": null,
"objectValue": "[json('null')]"
```

## <a name="next-steps"></a>Sonraki adımlar

* Şablon işlevlerinin tam listesi için bkz. [Azure Resource Manager şablon işlevleri](template-functions.md).
* Şablon dosyaları hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
