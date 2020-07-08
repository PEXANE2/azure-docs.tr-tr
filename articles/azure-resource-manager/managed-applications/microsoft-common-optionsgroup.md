---
title: Options grubu UI öğesi
description: Azure portal için Microsoft. Common. Options grubu Kullanıcı arabirimi öğesini açıklar. Kullanıcıların yönetilen bir uygulama dağıtımında kullanılabilir seçeneklerden seçim yapmasına olanak sağlar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: c1da0b0082bfe046adf9596a10397e9063c888be
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75652352"
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Microsoft. Common. Options grubu UI öğesi

Kullanılabilir seçeneklerin bulunduğu bir seçim denetimi.

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

- Etiketi, `constraints.allowedValues` bir öğe için görüntüleme metintir ve değeri seçildiğinde öğenin çıkış değeridir.
- Belirtilmişse, varsayılan değer içinde bulunan bir etiket olmalıdır `constraints.allowedValues` . Belirtilmemişse, içindeki ilk öğe `constraints.allowedValues` Varsayılan olarak seçilidir. Varsayılan değer **null**.
- `constraints.allowedValues`en az bir öğe olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
