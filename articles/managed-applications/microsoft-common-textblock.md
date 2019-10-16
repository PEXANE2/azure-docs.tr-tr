---
title: Azure TextBlock Kullanıcı arabirimi öğesi | Microsoft Docs
description: Azure portal için Microsoft. Common. TextBlock Kullanıcı arabirimi öğesini açıklar. Arabirime metin eklemek için kullanın.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: da2453889f04352dbabe182772312d70deec4464
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72331620"
---
# <a name="microsoftcommontextblock-ui-element"></a>Microsoft. Common. TextBlock Kullanıcı arabirimi öğesi
Portal arabirimine metin eklemek için kullanılabilen bir denetim.

## <a name="ui-sample"></a>UI örneği
![Microsoft. Common. TextBox](./media/managed-application-elements/microsoft.common.textblock.png)

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
