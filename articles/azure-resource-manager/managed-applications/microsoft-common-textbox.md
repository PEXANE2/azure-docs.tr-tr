---
title: TextBox Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Common. TextBox Kullanıcı arabirimi öğesini açıklar. Biçimlendirilmemiş metin eklemek için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 547b3ed84c8e4406b65ee8cf51c0db10b6878793
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87063821"
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
