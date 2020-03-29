---
title: DropDown UI öğesi
description: Azure portalı için Microsoft.Common.DropDown UI öğesini açıklar. Yönetilen bir uygulamayı dağıtırken kullanılabilir seçeneklerden seçim yapmak için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: a09f9695c18f368a585dbcd0d1e654dee4adfa03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652391"
---
# <a name="microsoftcommondropdown-ui-element"></a>Microsoft.Common.DropDown UI öğesi

Açılır listeli bir seçim denetimi.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

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

- Etiket, `constraints.allowedValues` bir öğenin görüntü metnidir ve değeri seçildiğinde öğenin çıktı değeridir.
- Belirtilirse, varsayılan değer `constraints.allowedValues`. Belirtilmemişse, ilk `constraints.allowedValues` öğe seçilir. Varsayılan değer **null'** dur.
- `constraints.allowedValues`en az bir öğeye sahip olmalıdır.
- Gerekli olmayan bir değeri taklit etmek için, etiketve `""` değeri (boş dize) olan bir öğe ekleyin. `constraints.allowedValues`

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
