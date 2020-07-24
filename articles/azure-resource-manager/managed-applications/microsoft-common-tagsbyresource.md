---
title: TagsByResource Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Common. TagsByResource Kullanıcı arabirimi öğesini açıklar. Dağıtım sırasında bir kaynağa etiket uygulamak için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: tomfitz
ms.openlocfilehash: e730201812005a9b469a964e4acd081ebe86b100
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87063941"
---
# <a name="microsoftcommontagsbyresource-ui-element"></a>Microsoft. Common. TagsByResource Kullanıcı arabirimi öğesi

Bir dağıtımdaki kaynaklarla [etiketlerin](../management/tag-resources.md) ilişkilendirilmesi için bir denetim.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft-common-tagsbyresource.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TagsByResource",
  "resources": [
    "Microsoft.Storage/storageAccounts",
    "Microsoft.Compute/virtualMachines"
  ]
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
{
  "Microsoft.Storage/storageAccounts": {
    "Dept": "Finance",
    "Environment": "Production"
  },
  "Microsoft.Compute/virtualMachines": {
    "Dept": "Finance"
  }
}
```

## <a name="remarks"></a>Açıklamalar

- Dizide en az bir öğe `resources` belirtilmelidir.
- İçindeki her öğe `resources` tam nitelikli kaynak türü olmalıdır. Bu öğeler **kaynak** açılan menüsünde görünür ve Kullanıcı tarafından taggable.
- Denetimin çıktısı, bir Azure Resource Manager şablonundaki etiket değerlerinin kolay atanması için biçimlendirilir. Bir şablonda denetimin çıkışını almak için aşağıdaki örnekte gösterildiği gibi şablonunuzda bir parametre ekleyin:

  ```json
  "parameters": {
    "tagsByResource": { "type": "object", "defaultValue": {} }
  }
  ```

  Etiketlenebilir her kaynak için, Etiketler özelliğini bu kaynak türü için parametre değerine atayın:

  ```json
  {
    "name": "saName1",
    "type": "Microsoft.Storage/storageAccounts",
    "tags": "[ if(contains(parameters('tagsByResource'), 'Microsoft.Storage/storageAccounts'), parameters('tagsByResource')['Microsoft.Storage/storageAccounts'], json('{}')) ]",
    ...
  ```

- TagsByResource parametresine erişirken [IF](../templates/template-functions-logical.md#if) işlevini kullanın. Verilen kaynak türüne hiçbir etiket atanmamışsa boş bir nesne atamanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
- UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
