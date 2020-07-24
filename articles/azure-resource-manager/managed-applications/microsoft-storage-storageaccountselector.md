---
title: StorageAccountSelector Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Storage. StorageAccountSelector Kullanıcı arabirimi öğesini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: fe70c42387e9dedea8943b5dcce04a1f9ded5190
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87033316"
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Microsoft. Storage. StorageAccountSelector Kullanıcı arabirimi öğesi

Yeni veya mevcut bir depolama hesabı seçmek için bir denetim.

## <a name="ui-sample"></a>UI örneği

Denetim varsayılan değeri gösterir.

![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft-storage-storageaccountselector.png)

Denetim, kullanıcının yeni bir depolama hesabı oluşturmasını veya var olan bir depolama hesabını seçmesini sağlar.

![Microsoft. Storage. StorageAccountSelector yeni](./media/managed-application-elements/microsoft-storage-storageaccountselector-new.png)

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

- Belirtilmişse `defaultValue.name` benzersizlik için otomatik olarak onaylanır. Depolama hesabı adı benzersiz değilse, kullanıcının farklı bir ad belirtmesi veya var olan bir depolama hesabını seçmesi gerekir.
- İçin varsayılan değer `defaultValue.type` **Premium_LRS**.
- İçinde belirtilmeyen herhangi bir tür `constraints.allowedTypes` gizlidir ve içinde belirtilmeyen herhangi bir tür `constraints.excludedTypes` gösterilir. `constraints.allowedTypes`ve `constraints.excludedTypes` her ikisi de isteğe bağlıdır, ancak aynı anda kullanılamaz.
- `options.hideExisting` **Doğru**ise, Kullanıcı var olan bir depolama hesabını seçemezsiniz. Varsayılan değer **false** şeklindedir.

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
