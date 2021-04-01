---
title: Bölüm UI öğesi
description: Azure portal için Microsoft. Common. Section Kullanıcı arabirimi öğesini açıklar. Yönetilen uygulamaları dağıtmak için portaldaki öğeleri gruplandırmak için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 924aff8f2ba3d796b65f52494845f3b10018065c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "87063972"
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft. Common. Section Kullanıcı arabirimi öğesi

Bir veya daha fazla öğeyi bir başlık altında gruplandıran bir denetim.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.Section](./media/managed-application-elements/microsoft-common-section.png)

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

- `elements` en az bir öğe içermeli ve hariç tüm öğe türlerine sahip olabilir `Microsoft.Common.Section` .
- Bu öğe `toolTip` özelliği desteklemiyor.

## <a name="sample-output"></a>Örnek çıktı
İçindeki öğelerin çıkış değerlerine erişmek için `elements` , [temel bilgiler ()](create-ui-definition-referencing-functions.md#basics) veya [Steps ()](create-ui-definition-referencing-functions.md#steps) işlevlerini ve nokta gösterimini kullanın:

```json
steps('configuration').section1.text1
```

Türündeki öğeler `Microsoft.Common.Section` kendisine hiçbir çıkış değeri içermez.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
