---
title: Açılan Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Common. DropDown Kullanıcı arabirimi öğesini açıklar. Yönetilen bir uygulamayı dağıttığınızda kullanılabilir seçenekler arasından seçim yapmak için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: tomfitz
ms.openlocfilehash: cc50e49d4bc59235a147d114d86ecdff95dca797
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87474351"
---
# <a name="microsoftcommondropdown-ui-element"></a>Microsoft. Common. DropDown UI öğesi

Açılan liste içeren seçim denetimi. Yalnızca tek bir öğe veya birden çok öğe seçimine izin verebilirsiniz. Ayrıca, isteğe bağlı olarak öğelerle birlikte bir açıklama ekleyebilirsiniz.

## <a name="ui-sample"></a>UI örneği

DropDown öğesi, portalda görünüşünü belirleyen farklı seçeneklere sahiptir.

Seçim için yalnızca tek bir öğeye izin verildiğinde, denetim şöyle görünür:

:::image type="content" source="./media/managed-application-elements/microsoft-common-dropdown-1.png" alt-text="Microsoft. Common. DropDown tek seçim":::

Açıklamalar dahil edildiğinde, denetim şöyle görünür:

:::image type="content" source="./media/managed-application-elements/microsoft-common-dropdown-2.png" alt-text="Açıklamalarla birlikte Microsoft. Common. DropDown tek seçim":::

Çoklu seçim etkinleştirildiğinde denetim, birden fazla öğe seçmek için bir **Tümünü Seç** seçeneği ve onay kutuları ekler:

:::image type="content" source="./media/managed-application-elements/microsoft-common-dropdown-3.png" alt-text="Microsoft. Common. DropDown çoklu seçim":::

Açıklamalar çoklu seçim etkinleştirilmiş olarak dahil edilebilir.

:::image type="content" source="./media/managed-application-elements/microsoft-common-dropdown-4.png" alt-text="Microsoft. Common. DropDown çoklu seçme açıklamalarını içeren":::

Filtreleme etkinken denetim, filtreleme değerini eklemek için bir metin kutusu içerir.

:::image type="content" source="./media/managed-application-elements/microsoft-common-dropdown-5.png" alt-text="Microsoft. Common. DropDown çoklu seçme açıklamalarını içeren":::

## <a name="schema"></a>Şema

```json
{
    "name": "element1",
    "type": "Microsoft.Common.DropDown",
    "label": "Example drop down",
    "placeholder": "",
    "defaultValue": "Value two",
    "toolTip": "",
    "multiselect": true,  
    "selectAll": true,  
    "filter": true,  
    "filterPlaceholder": "Filter items ...",  
    "multiLine": true,  
    "defaultDescription": "A value for selection",  
    "constraints": {
        "allowedValues": [
            {
                "label": "Value one",
                "description": "The value to select for option 1.",
                "value": "one"
            },
            {
                "label": "Value two",
                "description": "The value to select for option 2.",
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

- `multiselect`Kullanıcıların birden fazla öğe seçip seçmeyeceğini belirtmek için kullanın.
- , Varsayılan olarak `selectAll` `true` Çoklu seçim etkindir.
- `filter`Özelliği, kullanıcıların uzun bir seçenek listesi içinde arama yapmasına olanak sağlar.
- Etiketi, `constraints.allowedValues` bir öğe için görüntüleme metintir ve değeri seçildiğinde öğenin çıkış değeridir.
- Belirtilmişse, varsayılan değer içinde bulunan bir etiket olmalıdır `constraints.allowedValues` . Belirtilmemişse, içindeki ilk öğe `constraints.allowedValues` seçilir. Varsayılan değer **null**.
- `constraints.allowedValues`en az bir öğe olmalıdır.
- Zorunlu olmayan bir değere öykünmek için, etiketi ve değeri `""` (boş dize) olan bir öğe ekleyin `constraints.allowedValues` .
- `defaultDescription`Özelliği, açıklaması olmayan öğeler için kullanılır.
- `placeholder`Özelliği, Kullanıcı düzenlenmeden çalışmaya başladığında kaybolması gereken yardım metindir. `placeholder`Ve `defaultValue` her ikisi de tanımlıysa, `defaultValue` öncelik alır ve gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
