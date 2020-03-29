---
title: OptionsGroup UI öğesi
description: Azure portalı için Microsoft.Common.OptionsGroup UI öğesini açıklar. Yönetilen bir uygulamayı dağıtırken kullanıcıların kullanılabilir seçeneklerden seçim yapmalarını sağlar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: c1da0b0082bfe046adf9596a10397e9063c888be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652352"
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Microsoft.Common.OptionsGroup UI öğesi

Kullanılabilir seçenekler satırı içeren bir seçim denetimi.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
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
- Belirtilirse, varsayılan değer `constraints.allowedValues`. Belirtilmemişse, `constraints.allowedValues` ilk öğe varsayılan olarak seçilir. Varsayılan değer **null'** dur.
- `constraints.allowedValues`en az bir öğeye sahip olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
