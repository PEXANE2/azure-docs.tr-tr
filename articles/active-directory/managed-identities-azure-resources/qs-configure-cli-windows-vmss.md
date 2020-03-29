---
title: Yönetilen kimlikleri sanal makine ölçeği kümesinde yapılandırma - Azure CLI - Azure AD
description: Azure CLI'yi kullanarak sistemi ve kullanıcı tarafından atanan yönetilen kimlikleri Azure sanal makine ölçeğinde yapılandırmak için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: MarkusVi
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2019
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2832a8c584c0fbe707f22501809d772c6ffb970b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75430085"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-virtual-machine-scale-set-using-azure-cli"></a>Azure CLI'yi kullanarak sanal makine ölçeğinde Azure kaynakları için yönetilen kimlikleri yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure CLI'yi kullanarak Azure kaynakları işlemleri için aşağıdaki yönetilen kimlikleri bir Azure sanal makine ölçeğinde nasıl gerçekleştirdiğinizi öğreneceksiniz:
- Azure sanal makine ölçeğinde sistem tarafından atanan yönetilen kimliği etkinleştirme ve devre dışı bırak
- Azure sanal makine ölçeğinde kullanıcı tarafından atanan yönetilen bir kimlik ekleme ve kaldırma


## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için, hesabınızda aşağıdaki Azure rol tabanlı erişim denetimi atamaları gerekir:

    > [!NOTE]
    > Ek Azure AD dizini rol atamaları gerekmez.

    - [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) sanal makine ölçeği kümesi oluşturmak ve sistemi ve/veya kullanıcı tarafından atanan yönetilen kimliği sanal makine ölçeği kümesinden etkinleştirmek ve kaldırmak için.
    - Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak için [Yönetilen Kimlik Oluşturici](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü.
    - Kullanıcı tarafından atanan yönetilen bir kimliği sanal makine ölçeği kümesinden ve sanal makine ölçeğikümesine atamak ve kaldırmak için [Yönetilen Kimlik İşlemi](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü.
- CLI komut dosyası örneklerini çalıştırmak için üç seçeneğiniz var:
    - Azure portalından [Azure Bulut Shell'i](../../cloud-shell/overview.md) kullanın (sonraki bölüme bakın).
    - Her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesi aracılığıyla gömülü Azure Bulut Kabuğu'nu kullanın.
    - Yerel bir CLI konsolu kullanmak isterseniz [Azure CLI'nin](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya sonraki) en son sürümünü yükleyin. 
      
      > [!NOTE]
      > Komutlar Azure [CLI'nin](https://docs.microsoft.com/cli/azure/install-azure-cli)en son sürümüyansıtacak şekilde güncelleştirildi.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik

Bu bölümde, Azure CLI kullanarak bir Azure sanal makine ölçeği kümesi için sistem tarafından atanan yönetilen kimliği nasıl etkinleştirdiğinizi ve devre dışı bırakabileceğinizi öğrenirsiniz.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Azure sanal makine ölçeği kümesi nin oluşturulması sırasında sistem tarafından atanmış yönetilen kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimlik etkinleştirilmiş sanal bir makine ölçeği kümesi oluşturmak için:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. Sanal makine ölçeği kümesini dağıtmak istediğiniz Azure aboneliğiyle ilişkili bir hesap kullanın:

   ```azurecli-interactive
   az login
   ```

2. Az [grup](../../azure-resource-manager/management/overview.md#terminology) oluşturma yı kullanarak sanal makine ölçek kümenizin ve ilgili kaynaklarının oluşturulması ve dağıtılması için bir kaynak [grubu oluşturun.](/cli/azure/group/#az-group-create) Kullanmak istediğiniz bir kaynak grubunuz varsa bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. [Az vmss oluşturarak](/cli/azure/vmss/#az-vmss-create) sanal bir makine ölçeği kümesi oluşturun. Aşağıdaki örnek, `--assign-identity` parametre tarafından istendiği gibi sistem tarafından atanmış yönetilen bir kimliğe sahip *myVMSS* adlı sanal bir makine ölçeği kümesi oluşturur. `--admin-username` ve `--admin-password` parametreleri, sanal makinede oturum açmak için yönetici hesabının kullanıcı adı ve parolasını belirtir. Bu değerleri ortamınıza uyacak şekilde güncelleştirin: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Varolan bir Azure sanal makine ölçeğinde sistem le atanmış yönetilen kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimliği varolan bir Azure sanal makine ölçeğinde etkinleştirmeniz gerekiyorsa:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. Sanal makine ölçeği kümesini içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

   ```azurecli-interactive
   az login
   ```

2. Varolan bir VM'ye sistem tarafından atanan yönetilen bir kimliği etkinleştirmek için [az vmss kimlik atama](/cli/azure/vmss/identity/#az-vmss-identity-assign) komutunu kullanın:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Azure sanal makine ölçeği kümesinden sistem tarafından atanan yönetilen kimliği devre dışı bırak

Sistem tarafından atanan yönetilen kimliğe artık ihtiyaç duymadığı, ancak yine de kullanıcı tarafından atanan yönetilen kimliklere ihtiyaç duymadığı sanal bir makine ölçeği kümeniz varsa, aşağıdaki komutu kullanın:

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Artık sistem tarafından atanan yönetilen kimliğe ihtiyaç duymadığı bir sanal makineniz varsa ve kullanıcı tarafından atanmış yönetilen kimlikleri yoksa, aşağıdaki komutu kullanın:

> [!NOTE]
> Değer `none` büyük/küçük harf duyarlıdır. Küçük olmalı. 

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type="none"
```



## <a name="user-assigned-managed-identity"></a>kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure CLI kullanarak kullanıcı tarafından atanan yönetilen bir kimliği nasıl etkinleştirdiğinizi ve kaldırabileceğinizi öğrenirsiniz.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesi nin oluşturulması sırasında kullanıcı tarafından atanan yönetilen bir kimlik atama

Bu bölüm, sanal makine ölçeği kümesi nin oluşturulması nda ve kullanıcı tarafından atanan yönetilen bir kimliğin sanal makine ölçeği kümesine atanmasına yol sunar. Kullanmak istediğiniz sanal makine ölçeği kümesi zaten varsa, bu bölümü atlayın ve bir sonrakine ilerleyin.

1. Kullanmak istediğiniz bir kaynak grubunuz varsa bu adımı atlayabilirsiniz. Az [grubu](~/articles/azure-resource-manager/management/overview.md#terminology) oluşturmayı kullanarak, kullanıcı tarafından atanan yönetilen kimliğinizin oluşturulması ve dağıtılması için bir kaynak [grubu oluşturun.](/cli/azure/group/#az-group-create) `<RESOURCE GROUP>` ve `<LOCATION>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Kullanıcı tarafından atanan yönetilen kimliği oluşturmak için [az identity create](/cli/azure/identity#az-identity-create) kullanın.  `-g` parametresi kullanıcı tarafından atanan yönetilen kimliğin oluşturulduğu kaynak grubunu belirtirken, `-n` parametresi de bunun adını belirtir. `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
   ```
   Yanıt, aşağıdakine benzer şekilde oluşturulan kullanıcı tarafından atanan yönetilen kimliğin ayrıntılarını içerir. Kullanıcı `id` tarafından atanan yönetilen kimliğe atanan kaynak değeri aşağıdaki adımda kullanılır.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. [Az vmss oluşturarak](/cli/azure/vmss/#az-vmss-create)sanal bir makine ölçeği kümesi oluşturun. Aşağıdaki örnek, `--assign-identity` parametretarafından belirtildiği gibi, yeni kullanıcı tarafından atanan yönetilen kimlikle ilişkili sanal bir makine ölçeği kümesi oluşturur. `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>` ve `<USER ASSIGNED IDENTITY>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY>
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen bir kimliği varolan sanal makine ölçeği kümesine atama

1. Kullanıcı tarafından atanan yönetilen kimliği oluşturmak için [az identity create](/cli/azure/identity#az-identity-create) kullanın.  `-g` parametresi kullanıcı tarafından atanan yönetilen kimliğin oluşturulduğu kaynak grubunu belirtirken, `-n` parametresi de bunun adını belirtir. `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
   Yanıt, aşağıdakine benzer şekilde oluşturulan kullanıcı tarafından atanan yönetilen kimliğin ayrıntılarını içerir.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY >/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. [Az vmss kimlik atamasını](/cli/azure/vmss/identity)kullanarak kullanıcı tarafından atanan yönetilen kimliği sanal makine ölçeğine atayın. `<RESOURCE GROUP>` ve `<VIRTUAL MACHINE SCALE SET NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Kullanıcı `<USER ASSIGNED IDENTITY>` tarafından atanan kimliğin kaynak `name` özelliği, önceki adımda oluşturulduğu gibi:

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VIRTUAL MACHINE SCALE SET NAME> --identities <USER ASSIGNED IDENTITY>
    ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen kimliği Azure sanal makine ölçeği kümesinden kaldırma

Sanal makine ölçeğinden kullanıcı tarafından atanan yönetilen bir kimliği kaldırmak için [az vmss kimlik kaldır](/cli/azure/vmss/identity#az-vmss-identity-remove)kullanın. Bu, sanal makine ölçeği kümesine atanan tek kullanıcı `UserAssigned` atanmış yönetilen kimlikse, kimlik türü değerinden kaldırılır.  `<RESOURCE GROUP>` ve `<VIRTUAL MACHINE SCALE SET NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Kullanıcı `<USER ASSIGNED IDENTITY>` tarafından atanan yönetilen kimliğin `name` özelliği, sanal `az vmss identity show`makine ölçeği kullanılarak ayarlanan kimlik bölümünde bulunabilir:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VIRTUAL MACHINE SCALE SET NAME> --identities <USER ASSIGNED IDENTITY>
```

Sanal makine ölçek kümenizde sistem tarafından atanmış yönetilen bir kimlik yoksa ve kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırmak istiyorsanız, aşağıdaki komutu kullanın:

> [!NOTE]
> Değer `none` büyük/küçük harf duyarlıdır. Küçük olmalı.

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type="none" identity.userAssignedIdentities=null
```

Sanal makine ölçek kümenizde hem sistem tarafından atanmış hem de kullanıcı tarafından atanan yönetilen kimlikler varsa, yalnızca sistem tarafından atanan yönetilen kimliği kullanmak için geçiş yaparak kullanıcı tarafından atanan tüm kimlikleri kaldırabilirsiniz. Aşağıdaki komutu kullanın:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='SystemAssigned' identity.userAssignedIdentities=null 
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlikler](overview.md)
- Tam Azure sanal makine ölçeği kümesi Quickstart için bkz: 

  - [CLI ile Sanal Makine Ölçeği Seti Oluşturma](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















