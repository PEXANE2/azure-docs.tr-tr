---
title: Şablonlardaki veri türleri
description: Azure Resource Manager şablonlarda kullanılabilir olan veri türlerini açıklar.
ms.topic: conceptual
ms.author: tomfitz
author: tfitzmac
ms.date: 03/04/2021
ms.openlocfilehash: 7d3f15c8852e6e25c621baad9bc6f20c303ffdb9
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102125164"
---
# <a name="data-types-in-arm-templates"></a>ARM şablonlarındaki veri türleri

Bu makalede Azure Resource Manager şablonlarda (ARM şablonları) desteklenen veri türleri açıklanmaktadır. Hem JSON hem de Bıcep veri türlerini içerir.

## <a name="supported-types"></a>Desteklenen türler

ARM şablonu içinde şu veri türlerini kullanabilirsiniz:

* array
* bool
* int
* object
* secureObject-Bıcep 'de değiştiriciye göre belirtildi
* secureString-Bıcep 'de değiştiriciye göre belirtildi
* string

## <a name="arrays"></a>Diziler

Diziler sol köşeli ayraç () ile başlar `[` ve sağ köşeli ayraç () ile biter `]` .

JSON 'da, bir dizi tek bir satırda veya birden çok satırda bildirilebilecek. Her öğe virgülle ayrılır.

Bıcep 'de, bir dizi birden çok satırda bildirilmelidir. Değerler arasında virgül kullanmayın.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "exampleArray": {
    "type": "array",
    "defaultValue": [
      1,
      2,
      3
    ]
  }
},
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param exampleArray array = [
  1
  2
  3
]
```

---

Bir dizinin öğeleri aynı türde veya farklı türlerde olabilir.

# <a name="json"></a>[JSON](#tab/json)

```json
"variables": {
  "mixedArray": [
    "[resourceGroup().name]",
    1,
    true,
    "example string"
  ]
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
var mixedArray = [
  resourceGroup().name
  1
  true
  'example string'
]
```

---

## <a name="booleans"></a>Boolean değerleri

Boole değerleri belirtirken, veya kullanın `true` `false` . Değeri tırnak işaretleriyle çevreleyin.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "exampleBool": {
    "type": "bool",
    "defaultValue": true
  }
},
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param exampleBool bool = true
```

---

## <a name="integers"></a>Tam Sayılar

Tamsayı değerlerini belirtirken, tırnak işaretleri kullanmayın.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "exampleInt": {
    "type": "int",
    "defaultValue": 1
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param exampleInt int = 1
```

---

Satır içi parametre olarak geçirilen tamsayılar için, değer aralığı, dağıtım için kullandığınız SDK veya komut satırı aracıyla sınırlı olabilir. Örneğin, bir şablonu dağıtmak için PowerShell kullanılırken, tamsayı türleri-2147483648 ile 2147483647 arasında değişebilir. Bu sınırlamayı önlemek için bir [parametre dosyasında](parameter-files.md)büyük tamsayı değerlerini belirtin. Kaynak türleri, tamsayı özellikleri için kendi sınırlarını uygular.

## <a name="objects"></a>Nesneler

Nesneler sol küme ayracı () ile başlar `{` ve sağ küme ayracı () ile biter `}` . Bir nesnedeki her özellik anahtar ve değerden oluşur. Anahtar ve değer iki nokta () ile ayrılır `:` .

JSON 'da, anahtar çift tırnak içine alınır. Her özellik virgülle ayrılır.

Bıcep 'de, anahtar tırnak içine alınmaz. Özellikler arasında virgül kullanmayın.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "exampleObject": {
    "type": "object",
    "defaultValue": {
      "name": "test name",
      "id": "123-abc",
      "isCurrent": true,
      "tier": 1
    }
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param exampleObject object = {
  name: 'test name'
  id: '123-abc'
  isCurrent: true
  tier: 1
}
```

---

## <a name="strings"></a>Dizeler

JSON 'da, dizeler çift tırnaklarla işaretlenir. Bıcep 'de dizeler, birlikte tırnak işaretleriyle işaretlenir.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "exampleString": {
    "type": "string",
    "defaultValue": "test value"
  }
},
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param exampleString string = 'test value'
```
---

## <a name="secure-strings-and-objects"></a>Güvenli dizeler ve nesneler

Güvenli dize, dize ile aynı biçimi kullanır ve güvenli nesne nesneyle aynı biçimi kullanır. Bir parametreyi güvenli bir dizeye veya güvenli bir nesneye ayarladığınızda, parametrenin değeri dağıtım geçmişine kaydedilmez ve günlüğe kaydedilmez. Ancak, bu güvenli değeri güvenli bir değer beklemeyen bir özellik olarak ayarlarsanız değer korunmaz. Örneğin, bir etikete güvenli bir dize ayarlarsanız, bu değer düz metin olarak depolanır. Parolalar ve gizlilikler için güvenli dizeler kullanın.

Bıcep ile, `@secure()` değiştiricisini bir dizeye veya nesnesine eklersiniz.

Aşağıdaki örnek iki güvenli parametreyi göstermektedir:

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "password": {
    "type": "secureString"
  },
  "configValues": {
    "type": "secureObject"
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@secure()
param password string

@secure()
param configValues object
```

---

## <a name="next-steps"></a>Sonraki adımlar

Şablon söz dizimi hakkında bilgi edinmek için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
