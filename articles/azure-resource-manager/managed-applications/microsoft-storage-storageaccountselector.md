---
title: StorageAccountSelector UI öğesi
description: Azure portalı için Microsoft.Storage.Storage.StorageAccountSelector UI öğesini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: 4fcaf0e5842ce8a65175d2fc1dfa2483a1203b2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651897"
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Microsoft.Storage.Storage.StorageAccountSelector UI öğesi

Yeni veya varolan bir depolama hesabı seçmek için bir denetim.

## <a name="ui-sample"></a>UI örneği

Denetim varsayılan değeri gösterir.

![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

Denetim, kullanıcının yeni bir depolama hesabı oluşturmasına veya varolan bir depolama hesabı seçmesine olanak tanır.

![Microsoft.Storage.StorageAccountSelector yeni](./media/managed-application-elements/microsoft.storage.storageaccountselector-new.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="remarks"></a>Açıklamalar

- Belirtilirse, `defaultValue.name` benzersizlik için otomatik olarak doğrulanır. Depolama hesabı adı benzersiz değilse, kullanıcıfarklı bir ad belirtmeli veya varolan bir depolama hesabı seçmelidir.
- Varsayılan değer `defaultValue.type` **Premium_LRS.**
- Belirtilmeyen `constraints.allowedTypes` herhangi bir tür gizlidir ve `constraints.excludedTypes` belirtilmeyen herhangi bir tür gösterilir. `constraints.allowedTypes`ve `constraints.excludedTypes` her ikisi de isteğe bağlıdır, ancak aynı anda kullanılamaz.
- `options.hideExisting` **Doğruysa,** kullanıcı varolan bir depolama hesabı seçemez. Varsayılan değer **false** şeklindedir.

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
