---
title: TextBlock Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Common. TextBlock Kullanıcı arabirimi öğesini açıklar. Arabirime metin eklemek için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 418056cb149f4441bac49db839a0dba40c2bb42d
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75652222"
---
# <a name="microsoftcommontextblock-ui-element"></a>Microsoft. Common. TextBlock Kullanıcı arabirimi öğesi

Portal arabirimine metin eklemek için kullanılabilen bir denetim.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textblock.png)

## <a name="schema"></a>Şema

```json
{
  "name": "text1",
  "type": "Microsoft.Common.TextBlock",
  "visible": true,
  "options": {
    "text": "Please provide the configuration values for your application.",
    "link": {
      "label": "Learn more",
      "uri": "https://www.microsoft.com"
    }
  }
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
"Please provide the configuration values for your application. Learn more"
```

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
