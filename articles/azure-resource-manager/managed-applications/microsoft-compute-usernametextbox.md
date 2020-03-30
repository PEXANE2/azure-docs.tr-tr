---
title: UserNameTextBox Kullanıcı Arabirimi öğesi
description: Azure portalı için Microsoft.Compute.UserNameTextBox UI öğesini açıklar. Kullanıcıların Windows veya Linux kullanıcı adları sağlamasını sağlar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: c7544ae7d872a64547cb6c57ce8af9a09fc6c3d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651910"
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Microsoft.Compute.UserNameTextBox Kullanıcı Arabirimi öğesi

Windows ve Linux kullanıcı adları için yerleşik doğrulama içeren bir metin kutusu denetimi.

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

- Doğru `constraints.required` olarak **true**ayarlanmışsa, metin kutusunun başarıyla doğrulamak için bir değeri olmalıdır. Varsayılan değer **doğrudur.**
- `osPlatform`belirtilmelidir ve **Windows** veya **Linux**olabilir.
- `constraints.regex`JavaScript normal ifade desenidir. Belirtilirse, metin kutusunun değeri başarılı bir şekilde doğrulamak için desen le eşleşmelidir. Varsayılan değer **null'** dur.
- `constraints.validationMessage`metin kutusunun değeri tarafından `constraints.regex`belirtilen doğrulama başarısız olduğunda görüntülemek için bir dizedir. Belirtilmemişse, metin kutusunun yerleşik doğrulama iletileri kullanılır. Varsayılan değer **null'** dur.
- Bu öğe için `osPlatform`belirtilen değeri temel alan yerleşik doğrulama vardır. Yerleşik doğrulama, özel bir normal ifadeyle birlikte kullanılabilir. Bir `constraints.regex` değer belirtilirse, hem yerleşik hem de özel doğrulamalar tetiklenir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
