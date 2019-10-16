---
title: Azure Options grubu Kullanıcı arabirimi öğesi | Microsoft Docs
description: Azure portal için Microsoft. Common. Options grubu Kullanıcı arabirimi öğesini açıklar. Kullanıcıların yönetilen bir uygulama dağıtımında kullanılabilir seçeneklerden seçim yapmasına olanak sağlar.
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
ms.openlocfilehash: 7bec506575f1c526e59487edb67349cdf862f1a7
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72331665"
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Microsoft. Common. Options grubu UI öğesi
Kullanılabilir seçeneklerin bulunduğu bir seçim denetimi.

## <a name="ui-sample"></a>UI örneği
![Microsoft. Common. Options grubu](./media/managed-application-elements/microsoft.common.optionsgroup.png)

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

## <a name="remarks"></a>Açıklamalar
- @No__t-0 etiketi, bir öğe için görüntüleme metintir ve değeri seçildiğinde öğenin çıkış değeridir.
- Belirtilmişse, varsayılan değer `constraints.allowedValues` ' da mevcut bir etiket olmalıdır. Belirtilmezse, `constraints.allowedValues` ' daki ilk öğe varsayılan olarak seçilidir. Varsayılan değer **null**.
- `constraints.allowedValues` ' ın en az bir öğesi olmalıdır.

## <a name="sample-output"></a>Örnek çıktı
```json
"two"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
