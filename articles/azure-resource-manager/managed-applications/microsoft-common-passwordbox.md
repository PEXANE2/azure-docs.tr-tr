---
title: PasswordBox UI öğesi
description: Azure portalı için Microsoft.Common.PasswordBox UI öğesini açıklar. Yönetilen uygulamaları dağıtırken kullanıcıların gizli bir değer sağlamasını sağlar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 5f7d3a47482ccec9778b102d3509b5f8ef343185
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652313"
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Microsoft.Common.PasswordBox UI öğesi

Parola sağlamak ve onaylamak için kullanılabilecek bir denetim.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-zA-Z0-9]{8,}$",
    "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
"p4ssw0rd"
```

## <a name="remarks"></a>Açıklamalar

- Bu öğe `defaultValue` özelliği desteklemez.
- Uygulama ayrıntıları `constraints`için [Microsoft.Common.TextBox](microsoft-common-textbox.md)bölümüne bakın.
- Doğru `options.hideConfirmation` olarak **true**ayarlanmışsa, kullanıcının parolasını onaylamak için ikinci metin kutusu gizlenir. Varsayılan değer **false** şeklindedir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
