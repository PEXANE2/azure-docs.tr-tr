---
title: Şablon sözdizimi ve ifadeler
description: Azure Kaynak Yöneticisi şablonları için bildirimsel JSON sözdizimini açıklar.
ms.topic: conceptual
ms.date: 03/17/2020
ms.openlocfilehash: 172838fa24709eb60fbcb6a68277f44bbd42f01e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460118"
---
# <a name="syntax-and-expressions-in-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarında sözdizimi ve ifadeler

Şablonun temel sözdizimi JSON'dur. Ancak, şablon içinde kullanılabilir JSON değerlerini genişletmek için ifadeleri kullanabilirsiniz.  İfadeler köşeli ayraçla başlayıp biter: `[` ve `]`. İfadenin değeri, şablon dağıtıldığında değerlendirilir. İfade dize, tamsayı, boole, dizi veya nesne döndürebilir.

Şablon ifadesi 24.576 karakteri geçemez.

## <a name="use-functions"></a>İşlev kullanma

Azure Kaynak Yöneticisi, şablonda kullanabileceğiniz [işlevler](template-functions.md) sağlar. Aşağıdaki örnekte, bir parametrenin varsayılan değerinde bir işlev kullanan bir ifade gösterilmektedir:

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
```

İfade içinde sözdizimi, `resourceGroup()` Kaynak Yöneticisi'nin şablon içinde kullanılmasını sağladığı işlevlerden birini çağırır. Bu durumda, [kaynak Grubu](template-functions-resource.md#resourcegroup) işlevidir. JavaScript'te olduğu gibi işlev çağrıları `functionName(arg1,arg2,arg3)`da . Sözdizimi, `.location` bu işlev tarafından döndürülen nesneden bir özellik alır.

Şablon işlevleri ve parametreleri büyük/küçük harf duyarsızdır. Örneğin, Kaynak Yöneticisi **değişkenleri ('var1')** ve **DEĞIŞKENLER('VAR1')** olarak aynı şekilde çözer. Değerlendirilmesi, işlev açıkça durumda (toUpper veya toLower gibi) değiştirir sürece, işlev durumda korur. Belirli kaynak türlerinin, işlevlerin değerlendirilme sinden ayrı büyük/küçük harf gereksinimleri olabilir.

Bir işleviçin parametre olarak bir dize değeri geçmek için tek tırnak kullanın.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]"
```

Çoğu işlev, kaynak grubuna, aboneye, yönetim grubuna veya kiracıya dağıtılmış olsun aynı şekilde çalışır. Aşağıdaki işlevlerin kapsamına dayalı kısıtlamaları vardır:

* [resourceGroup](template-functions-resource.md#resourcegroup) - yalnızca bir kaynak grubuna dağıtımlarda kullanılabilir.
* [resourceId](template-functions-resource.md#resourceid) - herhangi bir kapsamda kullanılabilir, ancak geçerli parametreler ilerki kapsamına bağlı olarak değişir.
* [abonelik](template-functions-resource.md#subscription) - yalnızca bir kaynak grubuna veya aboneye dağıtımlarda kullanılabilir.

## <a name="escape-characters"></a>Kaçış karakterleri

Bir sol parantez `[` ile başlayıp sağ bir parantez `]`ile bitirmek için, ancak bir ifade olarak yorumlanmış değil, `[[`ile dize başlatmak için ekstra bir parantez ekleyin . Örneğin, değişken:

```json
"demoVar1": "[[test value]"
```

`[test value]`Giderir.

Ancak, edebi dize bir parantez ile bitmiyorsa, ilk parantez kaçmayın. Örneğin, değişken:

```json
"demoVar2": "[test] value"
```

`[test] value`Giderir.

Şablona JSON nesnesi eklemek gibi bir ifadedeki çift tırnak tan kaçmak için ters eğik çizgiyi kullanın.

```json
"tags": {
    "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
},
```

Parametre değerlerini geçerken, kaçış karakterlerinin kullanımı parametre değerinin belirtildiği yere bağlıdır. Şablonda varsayılan bir değer ayarlarsanız, fazladan sol ayraç gerekir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "demoParam1":{
            "type": "string",
            "defaultValue": "[[test value]"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput": {
            "type": "string",
            "value": "[parameters('demoParam1')]"
        }
    }
}
```

Varsayılan değeri kullanırsanız, şablon `[test value]`döndürür.

Ancak, komut satırı üzerinden bir parametre değeri geçerseniz, karakterler tam anlamıyla yorumlanır. Önceki şablonu şu şekilde dağıtma:

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName demoGroup -TemplateFile azuredeploy.json -demoParam1 "[[test value]"
```

`[[test value]` döndürür. Bunun yerine, kullanın:

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName demoGroup -TemplateFile azuredeploy.json -demoParam1 "[test value]"
```

Parametre dosyasındaki değerleri aktarırken aynı biçimlendirme geçerlidir. Karakterler kelimenin tam anlamıyla yorumlanır. Önceki şablonla kullanıldığında, aşağıdaki parametre dosyası `[test value]`döndürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "demoParam1": {
            "value": "[test value]"
        }
   }
}
```

## <a name="null-values"></a>Null değerleri

Bir özelliği null'a ayarlamak için **null** veya **[json('null')]** kullanabilirsiniz. [Parametre](template-functions-array.md#json) olarak verdiğinizde `null` json işlevi boş bir nesne döndürür. Her iki durumda da, Kaynak Yöneticisi şablonları özellik yokmuş gibi davranır.

```json
"stringValue": null,
"objectValue": "[json('null')]"
```

## <a name="next-steps"></a>Sonraki adımlar

* Şablon işlevlerinin tam listesi için [Azure Kaynak Yöneticisi şablon işlevlerine](template-functions.md)bakın.
* Şablon dosyaları hakkında daha fazla bilgi için [bkz.](template-syntax.md)
