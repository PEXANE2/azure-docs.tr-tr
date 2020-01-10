---
title: Açılan Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Common. DropDown Kullanıcı arabirimi öğesini açıklar. Yönetilen bir uygulamayı dağıttığınızda kullanılabilir seçenekler arasından seçim yapmak için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: a09f9695c18f368a585dbcd0d1e654dee4adfa03
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75652391"
---
# <a name="microsoftcommondropdown-ui-element"></a>Microsoft. Common. DropDown UI öğesi

Açılan liste içeren seçim denetimi.

## <a name="ui-sample"></a>UI örneği

![Microsoft. Common. DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Example drop down",
  "defaultValue": "Value two",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Value one",
        "value": "one"
      },
      {
        "label": "Value two",
        "value": "two"
      }
    ],
    "required": true
  },
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
"two"
```

## <a name="remarks"></a>Açıklamalar

- `constraints.allowedValues` etiketi bir öğe için görüntüleme metintir ve değeri seçildiğinde öğenin çıkış değeridir.
- Belirtilmişse, varsayılan değer `constraints.allowedValues`mevcut bir etiket olmalıdır. Belirtilmemişse, `constraints.allowedValues` ilk öğe seçilir. Varsayılan değer **null**.
- `constraints.allowedValues` en az bir öğe içermelidir.
- Zorunlu olmayan bir değere öykünmek için, `constraints.allowedValues`bir etiketi ve değeri `""` (boş dize) olan bir öğe ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
