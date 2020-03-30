---
title: PowerShell 'i kullanarak azure vm'de yönetilen kimlikleri yapılandırma - Azure AD
description: PowerShell'i kullanarak Azure kaynakları için yönetilen kimlikleri Azure vm'de yapılandırmak için adım adım yönergeler.
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
ms.openlocfilehash: f24c89477d71df3f497590b49841403576343bd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74547225"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-powershell"></a>PowerShell'i kullanarak Azure kaynaklarıiçin yönetilen kimlikleri Azure VM'de yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, PowerShell'i kullanarak, Azure kaynakları işlemleri için aşağıdaki yönetilen kimlikleri bir Azure VM'de nasıl gerçekleştireceklerini öğrenirsiniz.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Azure [PowerShell'in en son sürümünü](/powershell/azure/install-az-ps) yükleyin.

## <a name="system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik

Bu bölümde, Azure PowerShell kullanarak sisteme atanan yönetilen kimliği nasıl etkinleştireceğinizi ve devre dışı kaldığınız öğrenilir.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Azure VM oluşturma sırasında sistem tarafından atanmış yönetilen kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimliği etkinleştirilmiş bir Azure VM oluşturmak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

1. Yalnızca gerekli bölümleri tamamlayan aşağıdaki Azure VM Quickstarts'dan birine bakın ("Azure'da oturum açın", "Kaynak grubu oluştur", "Ağ grubu oluştur", "VM oluştur").
    
    "VM oluştur" bölümüne ulaştığınızda, [Yeni-AzVMConfig](/powershell/module/az.compute/new-azvm) cmdlet sözdiziminde küçük bir değişiklik yapın. Örneğin, vm'yi sistem tarafından atanmış kimlik etkinleştirilmiş olarak sağlamak için bir `-AssignIdentity:$SystemAssigned` parametre eklediğinizden emin olun:
      
    ```powershell
    $vmConfig = New-AzVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [PowerShell kullanarak Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md)
   - [PowerShell'i kullanarak bir Linux sanal makinesi oluşturun](../../virtual-machines/linux/quick-create-powershell.md)



### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Varolan bir Azure VM'de sistem le atanmış yönetilen kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimliği, başlangıçta onsuz sağlanan bir VM'de etkinleştirmek için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

1. Azure'da oturum `Connect-AzAccount`açarak oturum açın. VM içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

   ```powershell
   Connect-AzAccount
   ```

2. Önce `Get-AzVM` cmdlet kullanarak VM özelliklerini alın. Daha sonra sistem tarafından atanmış yönetilen `-AssignIdentity` bir kimliği etkinleştirmek için [Update-AzVM](/powershell/module/az.compute/update-azvm) cmdlet üzerindeki anahtarı kullanın:

   ```powershell
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```



### <a name="add-vm-system-assigned-identity-to-a-group"></a>Bir gruba atanan VM sistemi ekleme

VM'de sistem atanan kimliği etkinleştirdikten sonra, bir gruba ekleyebilirsiniz.  Aşağıdaki yordam, bir gruba kimlik atanan bir VM sistemi ekler.

1. Azure'da oturum `Connect-AzAccount`açarak oturum açın. VM içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

   ```powershell
   Connect-AzAccount
   ```

2. VM'nin `ObjectID` hizmet sorumlusunun `Id` (döndürülen değerler alanında belirtildiği gibi) alın ve not alın:

   ```powerhshell
   Get-AzADServicePrincipal -displayname "myVM"
   ```

3. Grubun `ObjectID` (döndürülen değerler `Id` alanında belirtildiği gibi) alın ve not alın:

   ```powershell
   Get-AzADGroup -searchstring "myGroup"
   ```

4. VM'nin hizmet ilkesini gruba ekleyin:

   ```powershell
   Add-AzureADGroupMember -ObjectId "<objectID of group>" -RefObjectId "<object id of VM service principal>"
   ```

## <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Azure VM'den sistem tarafından atanan yönetilen kimliği devre dışı

VM'de sistem tarafından atanan yönetilen kimliği devre dışı katmak için hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

Artık sistem tarafından atanan yönetilen kimliğe ihtiyaç duymadığı ancak yine de kullanıcı tarafından atanan yönetilen kimliklere ihtiyaç dolan bir Sanal Makineniz varsa, aşağıdaki cmdlet'i kullanın:

1. Azure'da oturum `Connect-AzAccount`açarak oturum açın. VM içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

   ```powershell
   Connect-AzAccount
   ```

2. `Get-AzVM` Cmdlet kullanarak VM özelliklerini alın `-IdentityType` ve parametreyi aşağıdakilere `UserAssigned`ayarlayın:

   ```powershell   
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 
   Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

Artık sistem tarafından atanan yönetilen kimliğe ihtiyaç duymadığı bir sanal makineniz varsa ve kullanıcı tarafından atanmış yönetilen kimlikleri yoksa, aşağıdaki komutları kullanın:

```powershell
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```



## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure PowerShell kullanarak kullanıcı tarafından atanan yönetilen bir kimliği niçin bir VM'den ekleyeceğinizi ve kaldırabileceğinizi öğrenirsiniz.

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>Oluşturma sırasında kullanıcı tarafından atanan yönetilen bir kimliği VM'ye atama

Bir VM'ye kullanıcı tarafından atanan bir kimlik atamak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [Yönetilen Kimlik Operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamalarına ihtiyacı vardır. Ek Azure AD dizin idemi rol atamaları gerekmez.

1. Yalnızca gerekli bölümleri tamamlayan aşağıdaki Azure VM Quickstarts'dan birine bakın ("Azure'da oturum açın", "Kaynak grubu oluştur", "Ağ grubu oluştur", "VM oluştur"). 
  
    "VM oluştur" bölümüne ulaştığınızda cmdlet sözdiziminde [`New-AzVMConfig`](/powershell/module/az.compute/new-azvm) hafif bir değişiklik yapın. VM'yi `-IdentityType UserAssigned` kullanıcı tarafından atanmış bir kimlikle sağlamak için parametreleri ve parametreleri `-IdentityID` ekleyin.  `<VM NAME>`Değiştirin`<SUBSCRIPTION ID>` `<RESROURCE GROUP>`, `<USER ASSIGNED IDENTITY NAME>` , ve kendi değerleri ile.  Örnek:
    
    ```powershell 
    $vmConfig = New-AzVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [PowerShell kullanarak Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md)
    - [PowerShell'i kullanarak bir Linux sanal makinesi oluşturun](../../virtual-machines/linux/quick-create-powershell.md)



### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Kullanıcı tarafından atanan yönetilen bir kimliği varolan bir Azure VM'sine atama

Bir VM'ye kullanıcı tarafından atanan bir kimlik atamak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [Yönetilen Kimlik Operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamalarına ihtiyacı vardır. Ek Azure AD dizin idemi rol atamaları gerekmez.

1. Azure'da oturum `Connect-AzAccount`açarak oturum açın. VM içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

   ```powershell
   Connect-AzAccount
   ```

2. [New-AzUserAssignedIdentity](/powershell/module/az.managedserviceidentity/new-azuserassignedidentity) cmdlet kullanarak kullanıcı tarafından atanmış bir yönetilen kimlik oluşturun.  `Id` Bir sonraki adımda buna ihtiyacınız olacağından çıktıya dikkat edin.

   > [!IMPORTANT]
   > Kullanıcı tarafından atanan yönetilen kimlikler oluşturmak yalnızca alfasayısal, alt çizgi ve tire (0-9 \_ veya a-z veya A-Z veya -) karakterlerini destekler. Ayrıca, VM/VMSS'nin düzgün çalışması için ad 3 ile 128 karakter uzunluğunda sınırlandırılmalıdır. Daha fazla bilgi için [Bkz. SSS ve bilinen sorunlar](known-issues.md)

   ```powershell
   New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. `Get-AzVM` Cmdlet kullanarak VM özelliklerini alın. Ardından, kullanıcı tarafından atanan yönetilen bir kimliği Azure VM'ye atamak için `-IdentityType` [Update-AzVM](/powershell/module/az.compute/update-azvm) cmdlet'ini kullanın ve `-IdentityID` açın.  Parametrenin`-IdentityId` değeri, önceki `Id` adımda belirttiğiniz değerdir.  `<VM NAME>`Değiştirin `<SUBSCRIPTION ID>` `<RESROURCE GROUP>`, `<USER ASSIGNED IDENTITY NAME>` , ve kendi değerleri ile.

   > [!WARNING]
   > VM'ye atanan daha önce kullanıcı tarafından atanmış yönetilen `Identity` kimlikleri korumak için VM nesnesinin özelliğini sorgula (örneğin, `$vm.Identity`).  Yönetilen kimlikler atanan herhangi bir kullanıcı döndürülürse, bunları VM'ye atamak istediğiniz yeni atanan yönetilen kimlikle birlikte aşağıdaki komuta ekleyin.

   ```powershell
   $vm = Get-AzVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```



### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Kullanıcı tarafından atanan yönetilen kimliği Azure VM'den kaldırma

Bir VM'ye kullanıcı tarafından atanan bir kimliği kaldırmak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.

VM'nizde birden çok kullanıcı tarafından atanmış yönetilen kimlik varsa, aşağıdaki komutları kullanarak sonuncusu hariç tüm kimlikleri kaldırabilirsiniz. `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Kullanıcı `<USER ASSIGNED IDENTITY NAME>` tarafından atanan yönetilen kimliğin VM'de kalması gereken ad özelliğidir. Bu bilgiler VM nesnesinin `Identity` özelliği sorgulayarak bulunabilir.  Örneğin, `$vm.Identity`:

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID <USER ASSIGNED IDENTITY NAME>
```
VM'nizde sistem leatanmış yönetilen bir kimlik yoksa ve kullanıcı tarafından atanan tüm yönetilen kimlikleri bu kimlikten kaldırmak istiyorsanız, aşağıdaki komutu kullanın:

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
VM'nizde hem sistem le atanmış hem de kullanıcı tarafından atanan yönetilen kimlikler varsa, yalnızca sistem tarafından atanan yönetilen kimlikleri kullanmak için geçiş yaparak tüm kullanıcı tarafından atanan yönetilen kimlikleri kaldırabilirsiniz.

```powershell 
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlikler](overview.md)
- Tam Azure VM oluşturma Quickstarts için bkz:
  
  - [PowerShell ile Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 
