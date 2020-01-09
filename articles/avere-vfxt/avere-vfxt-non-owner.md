---
title: Avere vFXT sahip olmayan geçici çözüm-Azure
description: Abonelik sahibi olmayan kullanıcılara Azure için avere vFXT dağıtma izni veren geçici çözüm
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: rohogue
ms.openlocfilehash: d50c07d78c15d26a191b982d24da8a4808a31ecd
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75415066"
---
# <a name="authorize-non-owners-to-deploy-avere-vfxt"></a>Sahip olmayanları Avere vFXT dağıtımı yapmak için yetkilendirme

Bu yönergeler, abonelik sahibi ayrıcalıkları olmayan bir kullanıcının Azure sistemi için avere vFXT oluşturmasına izin veren bir geçici çözümdür.

(Avere vFXT sistemini dağıtmanın önerilen yolu, [avere vFXT 'yi oluşturmaya hazırlanma](avere-vfxt-prereqs.md)bölümünde açıklandığı gibi, sahip ayrıcalıklarına sahip bir kullanıcının oluşturma adımlarını oluşturmaktır.)  

Geçici çözüm, kullanıcılarına kümeyi yüklemek için yeterli izinleri veren ek bir erişim rolü oluşturmayı içerir. Rol, bir abonelik sahibi tarafından oluşturulmalıdır ve bir sahibin onu uygun kullanıcılara ataması gerekir.

Abonelik sahibi ayrıca avere vFXT Market görüntüsü için [kullanım koşullarını kabul](avere-vfxt-prereqs.md) etmelidir.

> [!IMPORTANT]
> Bu adımların tümü, küme için kullanılacak abonelikte sahip ayrıcalıklarına sahip bir kullanıcı tarafından alınmalıdır.

1. Bu satırları kopyalayın ve bir dosyaya kaydedin (örneğin, `averecreatecluster.json`). `AssignableScopes` deyimindeki abonelik KIMLIĞINIZI kullanın.

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

1. Rolü oluşturmak için şu komutu çalıştırın:

   `az role definition create --role-definition <PATH_TO_FILE>`

    Örnek:

    ```azurecli
    az role definition create --role-definition ./averecreatecluster.json
    ```

1. Bu rolü, kümeyi oluşturacak olan kullanıcıya atayın:

   `az role assignment create --assignee <USERNAME> --scope /subscriptions/<SUBSCRIPTION_ID> --role 'avere-create-cluster'`

Bu yordamdan sonra, bu role atanan her kullanıcı abonelik için aşağıdaki izinlere sahiptir:

* Ağ altyapısını oluşturma ve yapılandırma
* Küme denetleyicisini oluşturma
* Kümeyi oluşturmak için küme denetleyicisinden küme oluşturma betikleri çalıştırma
