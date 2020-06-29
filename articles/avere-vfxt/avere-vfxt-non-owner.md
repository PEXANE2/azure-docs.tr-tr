---
title: Avere vFXT sahip olmayan geçici çözüm-Azure
description: Abonelik sahibi olmayan kullanıcılara Azure için avere vFXT dağıtma izni veren geçici çözüm
author: ekpgh
ms.service: avere-vfxt
ms.topic: how-to
ms.date: 12/19/2019
ms.author: rohogue
ms.openlocfilehash: 0d9b1060ee35af6cbc2e1b95b0f7813072c52d2e
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85505384"
---
# <a name="authorize-non-owners-to-deploy-avere-vfxt"></a>Sahip olmayanları Avere vFXT dağıtımı yapmak için yetkilendirme

Bu yönergeler, abonelik sahibi ayrıcalıkları olmayan bir kullanıcının Azure sistemi için avere vFXT oluşturmasına izin veren bir geçici çözümdür.

(Avere vFXT sistemini dağıtmanın önerilen yolu, [avere vFXT 'yi oluşturmaya hazırlanma](avere-vfxt-prereqs.md)bölümünde açıklandığı gibi, sahip ayrıcalıklarına sahip bir kullanıcının oluşturma adımlarını oluşturmaktır.)  

Geçici çözüm, kullanıcılarına kümeyi yüklemek için yeterli izinleri veren ek bir erişim rolü oluşturmayı içerir. Rol, bir abonelik sahibi tarafından oluşturulmalıdır ve bir sahibin onu uygun kullanıcılara ataması gerekir.

Abonelik sahibi ayrıca avere vFXT Market görüntüsü için [kullanım koşullarını kabul](avere-vfxt-prereqs.md) etmelidir.

> [!IMPORTANT]
> Bu adımların tümü, küme için kullanılacak abonelikte sahip ayrıcalıklarına sahip bir kullanıcı tarafından alınmalıdır.

1. Bu satırları kopyalayın ve bir dosyaya kaydedin (örneğin, `averecreatecluster.json` ). Deyimindeki abonelik KIMLIĞINIZI kullanın `AssignableScopes` .

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

Bu işlemi tamamladıktan sonra, rol abonelik için aşağıdaki izinleri atanmış herhangi bir kullanıcıya izin verir:

* Ağ altyapısını oluşturma ve yapılandırma
* Küme denetleyicisini oluşturma
* Kümeyi oluşturmak için küme denetleyicisinden küme oluşturma betikleri çalıştırma
