---
title: EtiketlerByResource UI öğesi
description: Azure portalı için Microsoft.Common.TagsByResource UI öğesini açıklar. Dağıtım sırasında bir kaynağa etiketleri uygulamak için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: tomfitz
ms.openlocfilehash: 23a7c54a84ec083b8fa470f26582913fcc3d2ee6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652209"
---
# <a name="microsoftcommontagsbyresource-ui-element"></a>Microsoft.Common.TagsByResource UI öğesi

[Etiketleri](../management/tag-resources.md) dağıtımdaki kaynaklarla ilişkilendirme denetimi.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.tagsbyresource.png)

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

- `resources` Dizideki en az bir öğe belirtilmelidir.
- Her `resources` öğe tam nitelikli kaynak türü olmalıdır. Bu öğeler **Kaynak** açılır açılır durumda görünür ve kullanıcı tarafından etiketlenebilir.
- Denetimin çıktısı, bir Azure Kaynak Yöneticisi şablonundaki etiket değerlerinin kolay atanması için biçimlendirilir. Denetimin çıktısını şablonda almak için şablonunuzda aşağıdaki örnekte gösterildiği gibi bir parametre ekleyin:

  ```json
  "parameters": {
    "tagsByResource": { "type": "object", "defaultValue": {} }
  }
  ```

  Etiketlenebilen her kaynak için, etiket özelliğini bu kaynak türü için parametre değerine atayın:

  ```json
  {
    "name": "saName1",
    "type": "Microsoft.Storage/storageAccounts",
    "tags": "[ if(contains(parameters('tagsByResource'), 'Microsoft.Storage/storageAccounts'), parameters('tagsByResource')['Microsoft.Storage/storageAccounts'], json('{}')) ]",
    ...
  ```

- tagsByResource parametresi'ne erişirken [if](../templates/template-functions-logical.md#if) işlevini kullanın. Verilen kaynak türüne etiket atanmadığınızda boş bir nesne atamanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
- UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
