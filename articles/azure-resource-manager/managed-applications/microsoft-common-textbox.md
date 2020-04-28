---
title: TextBox Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Common. TextBox Kullanıcı arabirimi öğesini açıklar. Biçimlendirilmemiş metin eklemek için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: e9f084badda9ea1905e43c6f00b29aaf957a6dbd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75652287"
---
# <a name="microsoftcommontextbox-ui-element"></a>Microsoft. Common. TextBox Kullanıcı arabirimi öğesi

Biçimlendirilmemiş metni düzenlemek için kullanılabilen bir denetim.

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

- `constraints.required` **True**olarak ayarlanırsa, metin kutusunun başarıyla doğrulanacak bir değere sahip olması gerekir. Varsayılan değer **false** şeklindedir.
- `constraints.regex`, JavaScript normal ifade deseninin. Belirtilmişse, metin kutusunun değeri, başarıyla doğrulanacak şekilde Düzenle eşleşmelidir. Varsayılan değer **null**.
- `constraints.validationMessage`metin kutusunun değeri doğrulanamazsa görüntülenecek bir dizedir. Belirtilmezse, metin kutusunun yerleşik doğrulama iletileri kullanılır. Varsayılan değer **null**.
- `constraints.regex` **False**olarak ayarlandığında için `constraints.required` bir değer belirtmek mümkündür. Bu senaryoda, metin kutusunun başarıyla doğrulanması için bir değer gerekli değildir. Belirtilmişse, normal ifade düzeniyle eşleşmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
