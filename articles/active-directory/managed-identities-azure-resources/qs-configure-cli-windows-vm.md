---
title: Azure CLI kullanarak Azure VM'de yönetilen kimlikleri yapılandırma - Azure AD
description: Azure CLI kullanarak bir Azure VM'de sistem ve kullanıcı tarafından atanan yönetilen kimlikleri yapılandırmak için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
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
ms.openlocfilehash: 2f2efaceefc53b3c0b5dfd899baf9fd30fdf9a76
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244153"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-azure-cli"></a>Azure CLI'yi kullanarak Azure VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure CLI'yi kullanarak, Azure kaynakları işlemleri için aşağıdaki yönetilen kimlikleri bir Azure VM'de nasıl gerçekleştireceklerini öğrenirsiniz:

- Azure VM'de sistem tarafından atanan yönetilen kimliği etkinleştirme ve devre dışı
- Azure VM'de kullanıcı tarafından atanan yönetilen bir kimlik ekleme ve kaldırma

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- CLI komut dosyası örneklerini çalıştırmak için üç seçeneğiniz var:
    - Azure portalından [Azure Bulut Shell'i](../../cloud-shell/overview.md) kullanın (sonraki bölüme bakın).
    - Her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesi aracılığıyla gömülü Azure Bulut Kabuğu'nu kullanın.
    - Yerel bir CLI konsolu kullanmayı tercih ediyorsanız [Azure CLI'nin en son sürümünü yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli) 
      
      > [!NOTE]
      > Komutlar Azure [CLI'nin](https://docs.microsoft.com/cli/azure/install-azure-cli)en son sürümüyansıtacak şekilde güncelleştirildi.     

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik

Bu bölümde, Azure CLI'yi kullanarak bir Azure VM'de sistem tarafından atanan yönetilen kimliği etkinleştirmeyi ve devre dışı kalmayı öğrenirsiniz.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Azure VM oluşturma sırasında sistem tarafından atanmış yönetilen kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimliği etkinleştirilmiş bir Azure VM oluşturmak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. VM'yi dağıtırken kullanmak istediğiniz Azure aboneliğiyle ilişkilendirilmiş bir hesap kullanın:

   ```azurecli-interactive
   az login
   ```

2. VM'nizin ve onunla ilgili kaynakların kapsaması ve dağıtımı için, [az group create](/cli/azure/group/#az-group-create) komutunu kullanarak bir [kaynak grubu](../../azure-resource-manager/management/overview.md#terminology) oluşturun. Bunun yerine kullanmak istediğiniz bir kaynak grubunuz varsa, bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. [az vm create](/cli/azure/vm/#az-vm-create) kullanarak VM oluşturun. Aşağıdaki örnek, `--assign-identity` parametre tarafından istendiği gibi sistem tarafından atanmış yönetilen bir kimliğe sahip *myVM* adlı bir VM oluşturur. `--admin-username` ve `--admin-password` parametreleri, sanal makinede oturum açmak için yönetici hesabının kullanıcı adı ve parolasını belirtir. Bu değerleri ortamınıza uyacak şekilde güncelleştirin: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Varolan bir Azure VM'de sistem le atanmış yönetilen kimliği etkinleştirme

VM'de sistem tarafından atanan yönetilen kimliği etkinleştirmek için hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. VM içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

   ```azurecli-interactive
   az login
   ```

2. Az [vm kimlik atama](/cli/azure/vm/identity/) `identity assign` komutu ile sistem tarafından atanan kimliği varolan bir VM'ye etkinleştirin:

   ```azurecli-interactive
   az vm identity assign -g myResourceGroup -n myVm
   ```

### <a name="disable-system-assigned-identity-from-an-azure-vm"></a>Azure VM'den sistem le atanmış kimliği devre dışı

VM'de sistem tarafından atanan yönetilen kimliği devre dışı katmak için hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

Sistem tarafından atanan kimliğe artık ihtiyaç duymadığı, ancak yine de kullanıcı tarafından atanan kimliklere ihtiyaç duymadığı bir Sanal Makineniz varsa, aşağıdaki komutu kullanın:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Artık sistem tarafından atanmış kimliğe ihtiyaç duymadığı bir sanal makineniz varsa ve kullanıcı tarafından atanmış kimlikleri yoksa aşağıdaki komutu kullanın:

> [!NOTE]
> Değer `none` büyük/küçük harf duyarlıdır. Küçük olmalı. 

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type="none"
```


## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure CLI'yi kullanarak kullanıcı tarafından atanan yönetilen bir kimliği azure VM'den nasıl ekleyeceğinizi ve kaldıracağınızı öğreneceksiniz.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-an-azure-vm"></a>Azure VM oluşturma sırasında kullanıcı tarafından atanmış yönetilen bir kimlik atama

Oluşturma sırasında bir VM'ye kullanıcı tarafından atanan bir kimlik atamak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [Yönetilen Kimlik Operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamalarına ihtiyacı vardır. Ek Azure AD dizin idemi rol atamaları gerekmez.

1. Kullanmak istediğiniz bir kaynak grubunuz varsa bu adımı atlayabilirsiniz. Az [grubu](~/articles/azure-resource-manager/management/overview.md#terminology) oluşturmayı kullanarak, kullanıcı tarafından atanan yönetilen kimliğinizin oluşturulması ve dağıtılması için bir kaynak [grubu oluşturun.](/cli/azure/group/#az-group-create) `<RESOURCE GROUP>` ve `<LOCATION>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Kullanıcı tarafından atanan yönetilen kimliği oluşturmak için [az identity create](/cli/azure/identity#az-identity-create) kullanın.  `-g` parametresi kullanıcı tarafından atanan yönetilen kimliğin oluşturulduğu kaynak grubunu belirtirken, `-n` parametresi de bunun adını belirtir.    
    
   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g myResourceGroup -n myUserAssignedIdentity
   ```
   Yanıt, aşağıdakine benzer şekilde oluşturulan kullanıcı tarafından atanan yönetilen kimliğin ayrıntılarını içerir. Kullanıcı tarafından atanan yönetilen kimliğe atanan kaynak kimliği değeri aşağıdaki adımda kullanılır.

   ```json
   {
       "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
       "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<myUserAssignedIdentity>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
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

3. [az vm create](/cli/azure/vm/#az-vm-create) kullanarak VM oluşturun. Aşağıdaki örnek, `--assign-identity` parametretarafından belirtildiği gibi, yeni kullanıcı tarafından atanan kimlikle ilişkili bir VM oluşturur. `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. 

   ```azurecli-interactive 
   az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY NAME>
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Kullanıcı tarafından atanan yönetilen bir kimliği varolan bir Azure VM'sine atama

Bir VM'ye kullanıcı tarafından atanan bir kimlik atamak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [Yönetilen Kimlik Operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamalarına ihtiyacı vardır. Ek Azure AD dizin idemi rol atamaları gerekmez.

1. Kullanıcı tarafından atanan kimliği oluşturmak için [az identity create](/cli/azure/identity#az-identity-create) kullanın.  Parametre, `-g` kullanıcı tarafından atanan kimliğin oluşturulduğu kaynak grubunu `-n` ve parametrenin adını belirtir. `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

    > [!IMPORTANT]
    > Addaki özel karakterlerle (yani alt alt) kullanıcı tarafından atanan yönetilen kimlikler oluşturmak şu anda desteklenmez. Lütfen alfasayısal karakterler kullanın. Güncelleştirmeler için sonra yeniden denetleyin.  Daha fazla bilgi için [Bkz. SSS ve bilinen sorunlar](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
   Yanıt, aşağıdakine benzer şekilde oluşturulan kullanıcı tarafından atanan yönetilen kimliğin ayrıntılarını içerir. 

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

2. Az vm kimliğini atayarak kullanıcı tarafından atanan kimliği VM'nize [atayın.](/cli/azure/vm) `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Kullanıcı `<USER ASSIGNED IDENTITY NAME>` tarafından atanan yönetilen kimliğin `name` kaynak özelliği, önceki adımda oluşturulduğu gibi:

    ```azurecli-interactive
    az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities <USER ASSIGNED IDENTITY>
    ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Kullanıcı tarafından atanan yönetilen kimliği Azure VM'den kaldırma

Bir VM'ye kullanıcı tarafından atanan bir kimliği kaldırmak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır. 

Bu, sanal makineye atanan tek kullanıcı atanmış `UserAssigned` yönetilen kimlikse, kimlik türü değerinden kaldırılır.  `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Kullanıcı `<USER ASSIGNED IDENTITY>` tarafından atanan kimliğin `name` özelliği, sanal makinenin kimlik bölümünde bulunabilir: `az vm identity show`

```azurecli-interactive
az vm identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <USER ASSIGNED IDENTITY>
```

VM'nizde sistem leatanmış yönetilen bir kimlik yoksa ve kullanıcı tarafından atanan tüm kimlikleri bu kimlikten kaldırmak istiyorsanız, aşağıdaki komutu kullanın:

> [!NOTE]
> Değer `none` büyük/küçük harf duyarlıdır. Küçük olmalı.

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type="none" identity.userAssignedIdentities=null
```

VM'nizde hem sistem le hem de kullanıcı tarafından atanmış kimlikler varsa, yalnızca sistem tarafından atanan kimlikleri kullanmak için geçiş yaparak kullanıcı tarafından atanan tüm kimlikleri kaldırabilirsiniz. Aşağıdaki komutu kullanın:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.userAssignedIdentities=null 
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure kaynaklarına genel bakış için yönetilen kimlikler](overview.md)
- Tam Azure VM oluşturma Quickstarts için bkz: 
  - [CLI ile Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-cli.md)  
  - [CLI ile bir Linux sanal makine oluşturun](../../virtual-machines/linux/quick-create-cli.md) 

















