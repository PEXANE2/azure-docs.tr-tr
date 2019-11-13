---
title: Azure PasswordBox Kullanıcı arabirimi öğesi | Microsoft Docs
description: Azure portal için Microsoft. Common. PasswordBox Kullanıcı arabirimi öğesini açıklar. Kullanıcıların yönetilen uygulamaları dağıttığı zaman gizli bir değer sağlamasına olanak sağlar.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 083b0be20d4e9ad7769a9c673139a361fb7b70d4
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74009181"
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Microsoft. Common. PasswordBox Kullanıcı arabirimi öğesi
Bir parolayı sağlamak ve onaylamak için kullanılabilen bir denetim.

## <a name="ui-sample"></a>UI örneği
![Microsoft. Common. PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

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

## <a name="remarks"></a>Açıklamalar
- Bu öğe `defaultValue` özelliğini desteklemiyor.
- `constraints`uygulama ayrıntıları için bkz. [Microsoft. Common. TextBox](microsoft-common-textbox.md).
- `options.hideConfirmation` **true**olarak ayarlanırsa, kullanıcının parolasını onaylamak için ikinci metin kutusu gizlenir. Varsayılan değer **false**.

## <a name="sample-output"></a>Örnek çıktı
```json
"p4ssw0rd"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
