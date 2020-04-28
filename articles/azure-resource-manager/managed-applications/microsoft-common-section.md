---
title: Bölüm UI öğesi
description: Azure portal için Microsoft. Common. Section Kullanıcı arabirimi öğesini açıklar. Yönetilen uygulamaları dağıtmak için portaldaki öğeleri gruplandırmak için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 999a7386ee7c5b3ef76f87280cc2d0cd45f143cd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75652261"
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft. Common. Section Kullanıcı arabirimi öğesi

Bir veya daha fazla öğeyi bir başlık altında gruplandıran bir denetim.

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

- `elements`en az bir öğe içermeli ve hariç `Microsoft.Common.Section`tüm öğe türlerine sahip olabilir.
- Bu öğe `toolTip` özelliği desteklemiyor.

## <a name="sample-output"></a>Örnek çıktı
İçindeki `elements`öğelerin çıkış değerlerine erişmek için, [temel bilgiler ()](create-uidefinition-functions.md#basics) veya [Steps ()](create-uidefinition-functions.md#steps) işlevlerini ve nokta gösterimini kullanın:

```json
steps('configuration').section1.text1
```

Türündeki `Microsoft.Common.Section` öğeler kendisine hiçbir çıkış değeri içermez.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
