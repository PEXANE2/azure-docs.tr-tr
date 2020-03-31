---
title: MultiStorageAccountCombo Kullanıcı Yanı elemanı
description: Azure portalı için Microsoft.Storage.MultiStorageAccountCombo UI öğesini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: 06412a1f08f1f242a3f3bd9be17b795ee09fcf9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651884"
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a>Microsoft.Storage.MultiStorageAccountCombo Kullanıcı Yanı öğesi

Ortak bir önek ile başlayan adlarla birkaç depolama hesabı oluşturmak için bir denetim grubu.

## <a name="ui-sample"></a>UI örneği

![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Storage.MultiStorageAccountCombo",
  "label": {
    "prefix": "Storage account prefix",
    "type": "Storage account type"
  },
  "toolTip": {
    "prefix": "",
    "type": ""
  },
  "defaultValue": {
    "prefix": "sa",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "count": 2,
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="remarks"></a>Açıklamalar

- Değer, `defaultValue.prefix` depolama hesabı adlarının sırasını oluşturmak için bir veya daha fazla arayıcı yla birleştirilmiştir. Örneğin, `defaultValue.prefix` **sa** ise `count` ve **2**ise, depolama hesabı adları **sa1** ve **sa2** oluşturulur. Oluşturulan depolama hesabı adları benzersizlik için otomatik olarak doğrulanır.
- Depolama hesabı adları lexicographically dayalı `count`oluşturulur. Örneğin, 10 `count` ise, depolama hesabı adları iki basamaklı tümselerle sona erer (01, 02, 03).
- Varsayılan değer `defaultValue.prefix` **null**ve for `defaultValue.type` **Premium_LRS.**
- Belirtilmeyen `constraints.allowedTypes` herhangi bir tür gizlidir ve `constraints.excludedTypes` belirtilmeyen herhangi bir tür gösterilir. `constraints.allowedTypes`ve `constraints.excludedTypes` her ikisi de isteğe bağlıdır, ancak aynı anda kullanılamaz.
- Depolama hesabı adları oluşturmaya ek `count` olarak, öğe için uygun çarpanı ayarlamak için kullanılır. **2**gibi statik bir değeri veya başka bir öğeden `[steps('step1').storageAccountCount]`dinamik bir değeri destekler. Varsayılan değer **1'dir.**

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
