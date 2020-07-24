---
title: Options grubu UI öğesi
description: Azure portal için Microsoft. Common. Options grubu Kullanıcı arabirimi öğesini açıklar. Kullanıcıların yönetilen bir uygulama dağıtımında kullanılabilir seçeneklerden seçim yapmasına olanak sağlar.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: tomfitz
ms.openlocfilehash: aa73b4cbded98291a14792a7151df9fdfb885b53
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87004208"
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Microsoft. Common. Options grubu UI öğesi

Options Grup denetimi, kullanıcıların iki veya daha fazla seçenekten birini seçmesini sağlar. Kullanıcı yalnızca bir seçenek seçebilir.

> [!NOTE]
> Geçmişte, bu denetim seçenekleri yatay olarak işlendi. Şimdi denetim, seçenekleri radyo düğmeleri olarak dikey olarak sunar.

## <a name="ui-sample"></a>UI örneği

:::image type="content" source="./media/managed-application-elements/microsoft-common-optionsgroup-2.png" alt-text="Microsoft.Common.OptionsGroup":::

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
