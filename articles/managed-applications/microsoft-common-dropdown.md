---
title: Azure açılır Kullanıcı arabirimi öğesi | Microsoft Docs
description: Azure portal için Microsoft. Common. DropDown Kullanıcı arabirimi öğesini açıklar. Yönetilen bir uygulamayı dağıttığınızda kullanılabilir seçenekler arasından seçim yapmak için kullanın.
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
ms.openlocfilehash: 5784b5df9b522f0489ca1f6087627b45cec5349d
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72331734"
---
# <a name="microsoftcommondropdown-ui-element"></a>Microsoft. Common. DropDown UI öğesi
Açılan liste içeren seçim denetimi.

## <a name="ui-sample"></a>UI örneği
![Microsoft. Common. DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Example drop down",
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
- Belirtilmişse, varsayılan değer `constraints.allowedValues` ' da mevcut bir etiket olmalıdır. Belirtilmemişse, `constraints.allowedValues` ' daki ilk öğe seçilir. Varsayılan değer **null**.
- `constraints.allowedValues` ' ın en az bir öğesi olmalıdır.
- Zorunlu olmayan bir değere öykünmek için, `""` (boş dize) etiketi ve değeri olan bir öğeyi `constraints.allowedValues` ' e ekleyin.

## <a name="sample-output"></a>Örnek çıktı
```json
"two"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
