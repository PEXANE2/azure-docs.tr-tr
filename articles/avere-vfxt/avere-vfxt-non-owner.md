---
title: Avere vFXT sahipsiz geçici çözüm - Azure
description: Abonelik sahibi izni olmayan kullanıcıların Azure için Avere vFXT dağıtmasına izin vermek için geçici çözüm
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: rohogue
ms.openlocfilehash: 1b411fe465a67f8ea5421ac0dc93348b4e92e8ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76153284"
---
# <a name="authorize-non-owners-to-deploy-avere-vfxt"></a>Sahip olmayanları Avere vFXT dağıtımı yapmak için yetkilendirme

Bu yönergeler, abonelik sahibi ayrıcalıkları olmayan bir kullanıcının Azure sistemi için bir Avere vFXT oluşturmasına olanak tanıyan bir geçici çözüm.

(Avere vFXT sistemini dağıtmanın önerilen yolu, [Avere vFXT'yi oluşturmaya hazırla'da](avere-vfxt-prereqs.md)açıklandığı gibi, oluşturma adımlarını sahip ayrıcalıklarına sahip bir kullanıcının yapmasıdır.)  

Geçici çözüm, kullanıcılarına kümeyi yüklemek için yeterli izin veren ek bir erişim rolü oluşturmayı içerir. Rol bir abonelik sahibi tarafından oluşturulmalıdır ve sahibi nin bu rolü uygun kullanıcılara ataması gerekir.

Bir abonelik sahibi de Avere vFXT pazar görüntüsü için [kullanım koşullarını kabul](avere-vfxt-prereqs.md) etmelidir.

> [!IMPORTANT]
> Bu adımların tümü, küme için kullanılacak abonelikte sahip ayrıcalıklarına sahip bir kullanıcı tarafından yapılmalıdır.

1. Bu satırları kopyalayın ve bir dosyaya `averecreatecluster.json`kaydedin (örneğin, ). Ekstrede `AssignableScopes` abonelik kimliğinizi kullanın.

   ```json
   {
       "AssignableScopes": ["/subscriptions/<SUBSCRIPTION_ID>"],
       "Name": "avere-create-cluster",
       "IsCustom": "true"
       "Description": "Can create Avere vFXT clusters",
       "NotActions": [],
       "Actions": [
           "Microsoft.Authorization/*/read",
           "Microsoft.Authorization/roleAssignments/*",
           "Microsoft.Authorization/roleDefinitions/*",
           "Microsoft.Compute/*/read",
           "Microsoft.Compute/availabilitySets/*",
           "Microsoft.Compute/virtualMachines/*",
           "Microsoft.Network/*/read",
           "Microsoft.Network/networkInterfaces/*",
           "Microsoft.Network/routeTables/write",
           "Microsoft.Network/routeTables/delete",
           "Microsoft.Network/routeTables/routes/delete",
           "Microsoft.Network/virtualNetworks/subnets/join/action",
           "Microsoft.Network/virtualNetworks/subnets/read",

           "Microsoft.Resources/subscriptions/resourceGroups/read",
           "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
           "Microsoft.Storage/*/read",
           "Microsoft.Storage/storageAccounts/listKeys/action"
       ],
   }
   ```

1. Rolü oluşturmak için bu komutu çalıştırın:

   `az role definition create --role-definition <PATH_TO_FILE>`

    Örnek:

    ```azurecli
    az role definition create --role-definition ./averecreatecluster.json
    ```

1. Bu rolü kümeoluşturacak kullanıcıya atama:

   `az role assignment create --assignee <USERNAME> --scope /subscriptions/<SUBSCRIPTION_ID> --role 'avere-create-cluster'`

Bu işlemi tamamladıktan sonra, rol kendisine atanan herhangi bir kullanıcıya abonelik için aşağıdaki izinleri verir:

* Ağ altyapısını oluşturma ve yapılandırma
* Küme denetleyicisini oluşturma
* Küme oluşturmak için küme denetleyicisinden küme oluşturma komut dosyalarını çalıştırma
