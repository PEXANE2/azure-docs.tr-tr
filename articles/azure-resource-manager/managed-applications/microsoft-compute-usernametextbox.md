---
title: UserNameTextBox Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. COMPUTE. UserNameTextBox Kullanıcı arabirimi öğesini açıklar. Kullanıcıların Windows veya Linux kullanıcı adlarını sağlamasına olanak sağlar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: c7544ae7d872a64547cb6c57ce8af9a09fc6c3d8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75651910"
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Microsoft. COMPUTE. UserNameTextBox UI öğesi

Windows ve Linux Kullanıcı adları için yerleşik doğrulamaya sahip bir metin kutusu denetimi.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
"Example name"
```

## <a name="remarks"></a>Açıklamalar

- `constraints.required` **True**olarak ayarlanırsa, metin kutusunun başarıyla doğrulanacak bir değere sahip olması gerekir. Varsayılan değer **true**'dur.
- `osPlatform`belirtilmelidir ve **Windows** ya da **Linux**olabilir.
- `constraints.regex`, JavaScript normal ifade deseninin. Belirtilmişse, metin kutusunun değeri, başarıyla doğrulanacak şekilde Düzenle eşleşmelidir. Varsayılan değer **null**.
- `constraints.validationMessage`, metin kutusunun değeri tarafından `constraints.regex`belirtilen doğrulama başarısız olduğunda görüntülenecek bir dizedir. Belirtilmezse, metin kutusunun yerleşik doğrulama iletileri kullanılır. Varsayılan değer **null**.
- Bu öğe için `osPlatform`belirtilen değeri temel alan yerleşik doğrulamaya sahiptir. Yerleşik doğrulama, özel bir normal ifadeyle birlikte kullanılabilir. İçin `constraints.regex` bir değer belirtilmişse, hem yerleşik hem de özel doğrulamalar tetiklenir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
