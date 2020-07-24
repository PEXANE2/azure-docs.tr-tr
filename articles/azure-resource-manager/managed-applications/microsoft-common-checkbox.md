---
title: CheckBox Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Common. CheckBox Kullanıcı arabirimi öğesini açıklar. Kullanıcıların bir seçeneği kontrol etmek veya işaretini kaldırmak için seçmesini sağlar.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: tomfitz
ms.openlocfilehash: 9f40f223cca34df58d2af7373d8f60fd7f383162
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100050"
---
# <a name="microsoftcommoncheckbox-ui-element"></a>Microsoft. Common. CheckBox UI öğesi

CheckBox denetimi, kullanıcıların bir seçeneği denetlemesine ya da işaretini kaldırmanıza olanak sağlar. Denetim işaretlendiğinde **true** , denetim işaretli olmadığında **false** değerini döndürür.

## <a name="ui-sample"></a>UI örneği

:::image type="content" source="./media/managed-application-elements/microsoft-common-checkbox.png" alt-text="Microsoft. Common. CheckBox":::

## <a name="schema"></a>Şema

```json
{
    "name": "legalAccept",
    "type": "Microsoft.Common.CheckBox",
    "label": "I agree to the terms and conditions.",
    "constraints": {
        "required": true,
        "validationMessage": "Please acknowledge the legal conditions."
    }
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
true
```

## <a name="remarks"></a>Açıklamalar

**Gerekli** ayarı **true**olarak belirlediğinizde, Kullanıcı onay kutusunu seçmelidir. Kullanıcı onay kutusunu seçmezseniz, doğrulama iletisi görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
