---
title: Bölüm UI öğesi
description: Azure portalı için Microsoft.Common.Section UI öğesini açıklar. Yönetilen uygulamaları dağıtmak için portaldaki öğeleri gruplandırmak için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 999a7386ee7c5b3ef76f87280cc2d0cd45f143cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652261"
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft.Common.Section UI öğesi

Bir başlık altında bir veya daha fazla öğeyi gruplayan bir denetim.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Şema

```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Example section",
  "elements": [
    {
      "name": "text1",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 1"
    },
    {
      "name": "text2",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar

- `elements`en az bir öğeye sahip olmalıdır ve `Microsoft.Common.Section`hariç tüm öğe türleri olabilir.
- Bu öğe `toolTip` özelliği desteklemez.

## <a name="sample-output"></a>Örnek çıktı
Öğelerin `elements`çıktı değerlerine erişmek için [temel() veya adım()](create-uidefinition-functions.md#basics) işlevlerini ve nokta gösterimini kullanın: [steps()](create-uidefinition-functions.md#steps)

```json
steps('configuration').section1.text1
```

Tür `Microsoft.Common.Section` öğelerinin çıkış değerleri yoktur.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
