---
title: TextBox Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Common. TextBox Kullanıcı arabirimi öğesini açıklar. Biçimlendirilmemiş metin eklemek için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: c89bc434d9d67144a95b5c2f23e7664078fe7825
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87474317"
---
# <a name="microsoftcommontextbox-ui-element"></a>Microsoft. Common. TextBox Kullanıcı arabirimi öğesi

Biçimlendirilmemiş metni düzenlemek için kullanılabilen bir denetim.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft-common-textbox.png)

## <a name="schema"></a>Şema

```json
{
    "name": "nameInstance",
    "type": "Microsoft.Common.TextBox",
    "label": "Name",
    "defaultValue": "contoso123",
    "toolTip": "Use only allowed characters",
    "placeholder": "",
    "constraints": {
        "required": true,
        "validations": [
            {
                "regex": "^[a-z0-9A-Z]{1,30}$",
                "message": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
            },
            {
                "isValid": "[startsWith(steps('resourceConfig').nameInstance, 'contoso')]",
                "message": "Must start with 'contoso'."
            }
        ]
    },
    "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
"contoso123"
```

## <a name="remarks"></a>Açıklamalar

- `constraints.required` **True**olarak ayarlanırsa, metin kutusunun başarıyla doğrulanacak bir değere sahip olması gerekir. Varsayılan değer **false** şeklindedir.
- `validations`Özelliği, metin kutusunda belirtilen değeri denetlemek için koşullar eklediğiniz bir dizidir.
- `regex`Özelliği bir JavaScript normal ifade deseninin. Belirtilmişse, metin kutusunun değeri, başarıyla doğrulanacak şekilde Düzenle eşleşmelidir. Varsayılan değer **null**.
- `isValid`Özelliği true veya false olarak değerlendirilen bir ifade içeriyor. İfade içinde, metin kutusunun geçerli olup olmadığını belirleyen koşulu tanımlarsınız.
- `message`Özelliği, metin kutusunun değeri doğrulanamazsa görüntülenecek bir dizedir.
- `regex` `required` **False**olarak ayarlandığında için bir değer belirtmek mümkündür. Bu senaryoda, metin kutusunun başarıyla doğrulanması için bir değer gerekli değildir. Belirtilmişse, normal ifade düzeniyle eşleşmesi gerekir.
- `placeholder`Özelliği, Kullanıcı düzenlenmeden çalışmaya başladığında kaybolması gereken yardım metindir. `placeholder`Ve `defaultValue` her ikisi de tanımlıysa, `defaultValue` öncelik alır ve gösterilir.

## <a name="example"></a>Örnek

Aşağıdaki örnek, bir kaynak adının kullanılabilirliğini denetlemek için [Microsoft. Solutions. ArmApiControl](microsoft-solutions-armapicontrol.md) denetimiyle birlikte bir metin kutusu kullanır.

```json
"basics": [
    {
        "name": "nameApi",
        "type": "Microsoft.Solutions.ArmApiControl",
        "request": {
            "method": "POST",
            "path": "[concat(subscription().id, '/providers/Microsoft.Storage/checkNameAvailability?api-version=2019-06-01')]",
            "body": "[parse(concat('{\"name\": \"', basics('txtStorageName'), '\", \"type\": \"Microsoft.Storage/storageAccounts\"}'))]"
        }
    },
    {
        "name": "txtStorageName",
        "type": "Microsoft.Common.TextBox",
        "label": "Storage account name",
        "constraints": {
            "validations": [
                {
                    "isValid": "[not(equals(basics('nameApi').nameAvailable, false))]",
                    "message": "[concat('Name unavailable: ', basics('txtStorageName'))]"
                }
            ]
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
