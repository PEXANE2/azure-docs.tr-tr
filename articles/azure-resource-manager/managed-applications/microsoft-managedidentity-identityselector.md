---
title: IdentitySelector UI öğesi
description: Azure portalı için Microsoft.ManagedIdentity.IdentitySelector UI öğesini açıklar. Yönetilen kimlikleri kaynağa atamak için kullanın.
author: tfitzmac
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: tomfitz
ms.openlocfilehash: cb66a2684e0b83f4f0cc01a07cc724f6beab4d68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77087550"
---
# <a name="microsoftmanagedidentityidentityselector-ui-element"></a>Microsoft.ManagedIdentity.IdentitySelector UI öğesi

Dağıtımdaki bir kaynak için [yönetilen kimlikatama](../../active-directory/managed-identities-azure-resources/overview.md) denetimi.

## <a name="ui-sample"></a>UI örneği

Denetim aşağıdaki öğelerden oluşur:

![Microsoft.ManagedIdentity.IdentitySelector ilk adım](./media/managed-application-elements/microsoft.managedidentity.identityselector1.png)

Kullanıcı **Ekle'yi**seçtiğinde aşağıdaki form açılır. Kullanıcı kaynak için bir veya daha fazla kullanıcı tarafından atanan kimlik seçebilir.

![Microsoft.ManagedIdentity.IdentitySelector ikinci adım](./media/managed-application-elements/microsoft.managedidentity.identityselector2.png)

Seçili kimlikler tabloda görüntülenir. Kullanıcı bu tablodan öğe ekleyebilir veya silebilir.

![Microsoft.ManagedIdentity.IdentitySelector üçüncü adım](./media/managed-application-elements/microsoft.managedidentity.identityselector3.png)

## <a name="schema"></a>Şema

```json
{
  "name": "identity",
  "type": "Microsoft.ManagedIdentity.IdentitySelector",
  "label": "Managed Identity Configuration",
  "toolTip": {
    "systemAssignedIdentity": "Enable system assigned identity to grant the resource access to other existing resources.",
    "userAssignedIdentity": "Add user assigned identities to grant the resource access to other existing resources."
  },
  "defaultValue": {
    "systemAssignedIdentity": "Off"
  },
  "options": {
    "hideSystemAssignedIdentity": false,
    "hideUserAssignedIdentity": false
  },
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
{
  "identity": {
    "value": {
      "type": "UserAssigned",
      "userAssignedIdentities": {
        "/subscriptions/xxxx/resourceGroups/TestResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/TestUserIdentity1": {}
      }
    }
  }
}
```

## <a name="remarks"></a>Açıklamalar

- Atanan sistem için bir başlangıç değeri ayarlamak için **defaultValue.systemAssignedIdentity'i** kullanın. Varsayılan değer **Kapalıdır.** Aşağıdaki değerlere izin verilir:
  - **On** – Kaynağa kimlik atanan bir sistem atanır.
  - **Kapalı** – Kaynağa atanan bir sistem atanmamış.
  - **OnOnly** – Kaynağa kimlik atanan bir sistem atanır. Kullanıcılar dağıtım sırasında bu değeri kaldıramaz.
  - **OffOnly** – Kaynağa kimlik atanan bir sistem atanmadı. Kullanıcılar dağıtım sırasında bu değeri kaldıramaz.

- **options.hideSystemAssignedIdentity** **doğru**ayarlanmışsa, atanan sistemi yapılandırmak için ui kimlik görüntülenmez. Bu seçeneğin varsayılan değeri **yanlıştır.**
- **options.hideUserAssignedIdentity** **doğru**ayarlanmışsa, atanan kullanıcıkimliğini yapılandırmak için kullanıcı ui görüntülenmez. Kaynağa atanmış bir kullanıcı kimliği atanmamış. Bu seçeneğin varsayılan değeri **yanlıştır.**

## <a name="next-steps"></a>Sonraki adımlar

- UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
- UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.