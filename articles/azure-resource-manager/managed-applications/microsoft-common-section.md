---
title: Bölüm UI öğesi
description: Azure portal için Microsoft. Common. Section Kullanıcı arabirimi öğesini açıklar. Yönetilen uygulamaları dağıtmak için portaldaki öğeleri gruplandırmak için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 999a7386ee7c5b3ef76f87280cc2d0cd45f143cd
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75652261"
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft. Common. Section Kullanıcı arabirimi öğesi

Bir veya daha fazla öğeyi bir başlık altında gruplandıran bir denetim.

## <a name="ui-sample"></a>UI örneği

![Microsoft. Common. Section](./media/managed-application-elements/microsoft.common.section.png)

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

- `elements` en az bir öğe içermeli ve `Microsoft.Common.Section`hariç tüm öğe türlerine sahip olabilir.
- Bu öğe `toolTip` özelliğini desteklemiyor.

## <a name="sample-output"></a>Örnek çıktı
`elements`öğelerin çıkış değerlerine erişmek için, [temel bilgiler ()](create-uidefinition-functions.md#basics) veya [Steps ()](create-uidefinition-functions.md#steps) işlevlerini ve nokta gösterimini kullanın:

```json
steps('configuration').section1.text1
```

`Microsoft.Common.Section` türündeki öğeler kendisine ait hiçbir çıkış değeri içermez.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
