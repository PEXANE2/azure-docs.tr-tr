---
title: MultiStorageAccountCombo UI öğesi
description: Azure portal için Microsoft. Storage. MultiStorageAccountCombo UI öğesini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: 06412a1f08f1f242a3f3bd9be17b795ee09fcf9d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75651884"
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a>Microsoft. Storage. MultiStorageAccountCombo UI öğesi

Ortak bir ön ekiyle başlayan adlara sahip birkaç depolama hesabı oluşturmaya yönelik bir denetim grubu.

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

- İçin değeri, `defaultValue.prefix` depolama hesabı adları dizisini oluşturmak için bir veya daha fazla tamsayı ile birleştirilir. Örneğin, `defaultValue.prefix` **sa** ve `count` **2**ise, **SA1** ve **SA2** depolama hesabı adları oluşturulur. Oluşturulan depolama hesabı adları otomatik olarak benzersizlik için onaylanır.
- Depolama hesabı adları, ile sözcüıgrafik tabanlı olarak oluşturulur `count` . Örneğin, 10 ise `count` , depolama hesabı adları iki basamaklı tamsayılarla biter (01, 02, 03).
- İçin varsayılan değer `defaultValue.prefix` **null**ve için `defaultValue.type` **Premium_LRS**.
- İçinde belirtilmeyen herhangi bir tür `constraints.allowedTypes` gizlidir ve içinde belirtilmeyen herhangi bir tür `constraints.excludedTypes` gösterilir. `constraints.allowedTypes`ve `constraints.excludedTypes` her ikisi de isteğe bağlıdır, ancak aynı anda kullanılamaz.
- Depolama hesabı adları oluşturmanın yanı sıra, `count` öğesi için uygun çarpanı ayarlamak için kullanılır. **2**gibi bir statik değeri ya da gibi başka bir öğeden dinamik bir değeri destekler `[steps('step1').storageAccountCount]` . Varsayılan değer **1**' dir.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
