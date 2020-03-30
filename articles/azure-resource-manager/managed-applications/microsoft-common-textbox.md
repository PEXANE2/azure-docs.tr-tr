---
title: TextBox Kullanıcı Birası öğesi
description: Azure portalı için Microsoft.Common.TextBox UI öğesini açıklar. Biçimlendirilmemiş metin eklemek için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: e9f084badda9ea1905e43c6f00b29aaf957a6dbd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652287"
---
# <a name="microsoftcommontextbox-ui-element"></a>Microsoft.Common.TextBox Kullanıcı Bira öğesi

Biçimlendirilmemiş metni denetlemek için kullanılabilecek bir denetim.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Example text box 1",
  "defaultValue": "my text value",
  "toolTip": "Use only allowed characters",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
"my text value"
```

## <a name="remarks"></a>Açıklamalar

- Doğru `constraints.required` olarak **true**ayarlanmışsa, metin kutusunun başarıyla doğrulamak için bir değeri olmalıdır. Varsayılan değer **false** şeklindedir.
- `constraints.regex`JavaScript normal ifade desenidir. Belirtilirse, metin kutusunun değeri başarılı bir şekilde doğrulamak için desen le eşleşmelidir. Varsayılan değer **null'** dur.
- `constraints.validationMessage`metin kutusunun değeri doğrulama başarısız olduğunda görüntülenecek bir dizedir. Belirtilmemişse, metin kutusunun yerleşik doğrulama iletileri kullanılır. Varsayılan değer **null'** dur.
- Yanlış olarak `constraints.regex` ayarlanan bir `constraints.required` değer belirtmek mümkündür. **false** Bu senaryoda, metin kutusunun başarıyla doğrulanması için bir değer gerekmez. Biri belirtilirse, normal ifade deseniyle eşleşmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
