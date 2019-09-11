---
title: Azure Resource Manager şablonu sözdizimi ve ifadeleri
description: Azure Resource Manager şablonları için bildirim temelli JSON sözdizimini açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: tomfitz
ms.openlocfilehash: 1e7288da19e2e81d609b952e03d5143b03a65c63
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70259489"
---
# <a name="syntax-and-expressions-in-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarındaki sözdizimi ve ifadeler

Şablonun temel sözdizimi JSON ' dır. Ancak, şablon içinde kullanılabilir olan JSON değerlerini genişletmek için ifadeleri kullanabilirsiniz.  İfadeler sırasıyla Köşeli parantezlerle `[` `]`başlar ve biter. İfade değeri, şablon dağıtıldığında değerlendirilir. Bir ifade dize, tamsayı, Boolean, dizi veya nesne döndürebilir.

Şablon ifadesi 24.576 karakterden uzun olamaz.

## <a name="use-functions"></a>İşlevleri kullanma

Aşağıdaki örnek, bir parametresinin varsayılan değerindeki bir ifadeyi gösterir:

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
```

İfade içinde sözdizimi `resourceGroup()` , Kaynak Yöneticisi bir şablon içinde kullanmak için sağladığı işlevlerden birini çağırır. Bu durumda, [resourceGroup](resource-group-template-functions-resource.md#resourcegroup) işlevi. JavaScript içinde olduğu gibi, işlev çağrıları olarak `functionName(arg1,arg2,arg3)`biçimlendirilir. Söz dizimi `.location` , bu işlev tarafından döndürülen nesneden bir özelliği alır.

Şablon işlevleri ve parametreleri büyük/küçük harfe duyarlıdır. Örneğin, Kaynak Yöneticisi **değişkenleri (' var1 ')** ve **DEĞIŞKENLERI (' var1 ')** aynı şekilde çözümler. Değerlendirildiğinde, işlev açıkça büyük/küçük harf (toUpper veya toLower gibi) değiştirmediği sürece işlev, büyük/küçük harf durumunu korur. Belirli kaynak türlerinde, işlevlerin nasıl değerlendirildiğinden ayrı olarak durum gereksinimleri olabilir.

Bir parametreye parametre olarak bir dize değeri geçirmek için tek tırnak kullanın.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]"
```

## <a name="escape-characters"></a>Kaçış karakterleri

Değişmez bir dizenin bir sol köşeli ayraç `[` ile başlaması ve sağ parantez `]`ile bitmesi, ancak bir ifade olarak yorumlanmaması için, dizeyi ile `[[`başlatmak için fazladan bir köşeli ayraç ekleyin. Örneğin, değişkeni:

```json
"demoVar1": "[[test value]"
```

Olarak `[test value]`çözümlenir.

Ancak, değişmez dize bir köşeli ayraç ile bitmezse ilk köşeli ayracı atmayın. Örneğin, değişkeni:

```json
"demoVar2": "[test] value"
```

Olarak `[test] value`çözümlenir.

Şablonda JSON nesnesi ekleme gibi bir ifadede çift tırnak işaretleri için ters eğik çizgi kullanın.

```json
"tags": {
    "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
},
```

## <a name="next-steps"></a>Sonraki adımlar

* Şablon işlevlerinin tam listesi için bkz. [Azure Resource Manager şablon işlevleri](resource-group-template-functions.md).
* Şablon dosyaları hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](resource-group-authoring-templates.md).
