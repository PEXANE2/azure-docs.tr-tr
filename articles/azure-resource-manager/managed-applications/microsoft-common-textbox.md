---
title: TextBox Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Common. TextBox Kullanıcı arabirimi öğesini açıklar. Biçimlendirilmemiş metin eklemek için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 03/03/2021
ms.author: tomfitz
ms.openlocfilehash: 8e4cfcc7fe46c19bf1e58ee5eadf1e0bb8c7bd8e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102124338"
---
# <a name="microsoftcommontextbox-ui-element"></a>Microsoft. Common. TextBox Kullanıcı arabirimi öğesi

Biçimlendirilmemiş metin eklemek için kullanılabilen bir kullanıcı arabirimi (UI) öğesi. `Microsoft.Common.TextBox`Öğesi tek satırlık bir giriş alanıdır, ancak özelliği ile çok satırlı girişi destekler `multiLine` .

## <a name="ui-sample"></a>UI örneği

`TextBox`Öğesi, tek satırlı veya çok satırlı bir metin kutusu kullanır.

:::image type="content" source="media/managed-application-elements/microsoft-common-textbox.png" alt-text="Microsoft. Common. TextBox öğe tek satırlık metin kutusu.":::

:::image type="content" source="media/managed-application-elements/microsoft-common-textbox-multi-line.png" alt-text="Microsoft. Common. TextBox öğesi çok satırlı metin kutusu.":::

## <a name="schema"></a>Şema

```json
{
  "name": "nameInstance",
  "type": "Microsoft.Common.TextBox",
  "label": "Name",
  "defaultValue": "contoso123",
  "toolTip": "Use only allowed characters",
  "placeholder": "",
  "multiLine": false,
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

- `toolTip`Fare imleci bilgi sembolünün üzerine geldiğinde öğesi hakkında metni göstermek için özelliğini kullanın.
- `placeholder`Özelliği, Kullanıcı düzenlenmeden çalışmaya başladığında kaybolması gereken yardım metindir. `placeholder`Ve `defaultValue` her ikisi de tanımlıysa, `defaultValue` öncelik alır ve gösterilir.
- `multiLine`Özelliği Boolean `true` veya olur `false` . Çok satırlı bir metin kutusu kullanmak için özelliğini olarak ayarlayın `true` . Çok satırlı bir metin kutusu gerekmiyorsa, özelliği özelliği olarak ayarlayın `false` veya hariç tutun. Yeni satırlarda, `\n` satır beslemenin JSON çıktısı gösterilir. Çok satırlı metin kutusu bir satır başı `\r` (CR) ve `\n` bir satır besleme (LF) için kabul eder. Örneğin, varsayılan değer `\r\n` CRLF belirtmek için eklenebilir.
- `constraints.required`Olarak ayarlanırsa `true` , metin kutusu başarıyla doğrulanacak bir değere sahip olmalıdır. `false` varsayılan değerdir.
- `validations`Özelliği, metin kutusunda belirtilen değeri denetlemek için koşullar eklediğiniz bir dizidir.
- `regex`Özelliği bir JavaScript normal ifade deseninin. Belirtilmişse, metin kutusunun değeri, başarıyla doğrulanacak şekilde Düzenle eşleşmelidir. `null` varsayılan değerdir. Regex sözdizimi hakkında daha fazla bilgi için bkz. [normal ifade hızlı başvurusu](/dotnet/standard/base-types/regular-expression-language-quick-reference).
- `isValid`Özelliği veya olarak değerlendirilen bir ifade içerir `true` `false` . İfade içinde, metin kutusunun geçerli olup olmadığını belirleyen koşulu tanımlarsınız.
- `message`Özelliği, metin kutusunun değeri doğrulanamazsa görüntülenecek bir dizedir.
- `regex`Ne zaman olarak ayarlandığı için bir değer belirtmek mümkündür `required` `false` . Bu senaryoda, metin kutusunun başarıyla doğrulanması için bir değer gerekli değildir. Belirtilmişse, normal ifade düzeniyle eşleşmesi gerekir.

## <a name="examples"></a>Örnekler

Örneklerde, tek satırlık metin kutusu ve çok satırlı bir metin kutusunun nasıl kullanılacağı gösterilmektedir.

### <a name="single-line"></a>Tek satırlı

Aşağıdaki örnek, bir kaynak adının kullanılabilirliğini denetlemek için [Microsoft. Solutions. ArmApiControl](microsoft-solutions-armapicontrol.md) denetimiyle birlikte bir metin kutusu kullanır.

```json
"basics": [
  {
    "name": "nameApi",
    "type": "Microsoft.Solutions.ArmApiControl",
    "request": {
      "method": "POST",
      "path": "[concat(subscription().id, '/providers/Microsoft.Storage/checkNameAvailability?api-version=2019-06-01')]",
      "body": "[parse(concat('{\"name\": \"', basics('txtStorageName'), '\", \"type\": \"Microsoft.Storage/storageAccounts\"}'))]"
    }
  },
  {
    "name": "txtStorageName",
    "type": "Microsoft.Common.TextBox",
    "label": "Storage account name",
    "constraints": {
      "validations": [
        {
          "isValid": "[not(equals(basics('nameApi').nameAvailable, false))]",
          "message": "[concat('Name unavailable: ', basics('txtStorageName'))]"
        }
      ]
    }
  }
]
```

### <a name="multi-line"></a>Çok satırlı

Bu örnek, `multiLine` yer tutucu metinle çok satırlı bir metin kutusu oluşturmak için özelliğini kullanır.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {
        "name": "demoTextBox",
        "type": "Microsoft.Common.TextBox",
        "label": "Multi-line text box",
        "defaultValue": "",
        "toolTip": "Use 1-60 alphanumeric characters, hyphens, spaces, periods, and colons.",
        "placeholder": "This is a multi-line text box:\nLine 1.\nLine 2.\nLine 3.",
        "multiLine": true,
        "constraints": {
          "required": true,
          "validations": [
            {
              "regex": "^[a-z0-9A-Z -.:\n]{1,60}$",
              "message": "Only 1-60 alphanumeric characters, hyphens, spaces, periods, and colons are allowed."
            }
          ]
        },
        "visible": true
      }
    ],
    "steps": [],
    "outputs": {
      "textBox": "[basics('demoTextBox')]"
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanıcı arabirimi tanımları oluşturmaya giriş için bkz. [ Azure yönetilen uygulamanın oluşturma deneyimi içinCreateUiDefinition.js](create-uidefinition-overview.md).
- UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
