---
title: PowerShell kullanarak sanal makine ölçek kümelerinde yönetilen kimlikleri Yapılandırma-Azure AD
description: PowerShell kullanarak bir sanal makine ölçek kümesi üzerinde sistem ve Kullanıcı tarafından atanan Yönetilen kimlikler yapılandırmaya yönelik adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2019
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 23d549d3b59eabbeab6b8a892cb6800f0088ece2
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85609084"
---
# <a name="configure-managed-identities-for-azure-resources-on-virtual-machine-scale-sets-using-powershell"></a>PowerShell kullanarak sanal makine ölçek kümelerinde Azure kaynakları için Yönetilen kimlikler yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory ' de otomatik olarak yönetilen kimlik ile Azure hizmetleri sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, PowerShell kullanarak, sanal makine ölçek kümesinde Azure kaynakları işlemleri için yönetilen kimliklerin nasıl gerçekleştirileceğini öğreneceksiniz:
- Bir sanal makine ölçek kümesi üzerinde sistem tarafından atanan yönetilen kimliği etkinleştirin ve devre dışı bırakın
- Bir sanal makine ölçek kümesi üzerinde kullanıcı tarafından atanan yönetilen kimlik ekleme ve kaldırma

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. ** [Sistem tarafından atanan ve Kullanıcı tarafından yönetilen atanan kimlik arasındaki farkı](overview.md#managed-identity-types)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makaledeki yönetim işlemlerini gerçekleştirmek için, hesabınız aşağıdaki Azure rol tabanlı erişim denetimi atamalarına ihtiyaç duyuyor:

    > [!NOTE]
    > Ek Azure AD dizin rolü ataması gerekli değildir.

    - Sanal makine [katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) bir sanal makine ölçek kümesi oluşturmak ve bir sanal makine ölçek kümesinden sistem tarafından atanan yönetilen ve/veya Kullanıcı tarafından atanan yönetilen kimliği etkinleştirmek ve kaldırmak için.
    - Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü.
    - Kullanıcı tarafından atanan yönetilen kimliği ve sanal makine ölçek kümesine atamak ve kaldırmak için [yönetilen kimlik operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü.
- Henüz yapmadıysanız [Azure PowerShell en son sürümünü](/powershell/azure/install-az-ps) yükleyebilirsiniz. 

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, Azure PowerShell kullanarak sistem tarafından atanan yönetilen kimliği etkinleştirmeyi ve kaldırmayı öğreneceksiniz.

### <a name="enable-system-assigned-managed-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Azure sanal makine ölçek kümesi oluşturma sırasında sistem tarafından atanan yönetilen kimliği etkinleştirin

Sistem tarafından atanan yönetilen kimlik etkin bir sanal makine ölçek kümesi oluşturmak için:

1. Sistem tarafından atanan yönetilen kimliğe sahip bir sanal makine ölçek kümesi oluşturmak için [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) cmdlet başvurusu makalesindeki *Örnek 1* ' e bakın.  `-IdentityType SystemAssigned` `New-AzVmssConfig` Cmdlet 'ine parametresini ekleyin:

    ```powershell
    $VMSS = New-AzVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```



## <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Mevcut bir Azure sanal makine ölçek kümesi üzerinde sistem tarafından atanan yönetilen kimliği etkinleştir

Var olan bir Azure sanal makine ölçek kümesinde sistem tarafından atanan bir yönetilen kimliği etkinleştirmeniz gerekiyorsa:

1. Kullanarak Azure 'da oturum açın `Connect-AzAccount` . Sanal makine ölçek kümesini içeren Azure aboneliğiyle ilişkili bir hesap kullanın. Ayrıca, hesabınızın "sanal makine katılımcısı" gibi, sanal makine ölçek kümesinde yazma izinleri veren bir role ait olduğundan emin olun:

   ```powershell
   Connect-AzAccount
   ```

2. İlk olarak cmdlet 'ini kullanarak sanal makine ölçek kümesi özelliklerini alın [`Get-AzVmss`](/powershell/module/az.compute/get-azvmss) . Ardından, sistem tarafından atanan bir yönetilen kimliği etkinleştirmek için `-IdentityType` [Update-AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet 'inde anahtarı kullanın:

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```



### <a name="disable-the-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Sistem tarafından atanan yönetilen kimliği bir Azure sanal makine ölçek kümesinden devre dışı bırakma

Artık sistem tarafından atanan yönetilen kimliğe ihtiyacı olmayan ancak hala Kullanıcı tarafından atanan yönetilen kimliklere ihtiyacı olan bir sanal makine ölçek kümesine sahipseniz, aşağıdaki cmdlet 'i kullanın:

1. Kullanarak Azure 'da oturum açın `Connect-AzAccount` . VM 'yi içeren Azure aboneliğiyle ilişkili bir hesap kullanın. Ayrıca, hesabınızın "sanal makine katılımcısı" gibi, sanal makine ölçek kümesinde yazma izinleri veren bir role ait olduğundan emin olun:

2. Aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Artık sistem tarafından atanan yönetilen kimliğe ihtiyacı olmayan ve Kullanıcı tarafından atanan yönetilen kimliklerine sahip olmayan bir sanal makine ölçek kümesine sahipseniz, aşağıdaki komutları kullanın:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure PowerShell kullanarak bir sanal makine ölçek kümesinden Kullanıcı tarafından atanan yönetilen kimlik ekleme ve kaldırma hakkında bilgi edineceksiniz.

### <a name="assign-a-user-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Azure sanal makine ölçek kümesi oluşturma sırasında Kullanıcı tarafından atanan yönetilen kimlik atama

Kullanıcı tarafından atanan yönetilen kimlik ile yeni bir sanal makine ölçek kümesi oluşturmak, şu anda PowerShell aracılığıyla desteklenmemektedir. Mevcut bir sanal makine ölçek kümesine Kullanıcı tarafından atanan yönetilen kimlik ekleme hakkında sonraki bölüme bakın. Güncelleştirmeler için sonra yeniden denetleyin.

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Mevcut bir Azure sanal makine ölçek kümesine Kullanıcı tarafından atanan bir yönetilen kimlik atama

Var olan bir Azure sanal makine ölçek kümesine Kullanıcı tarafından atanan bir yönetilen kimlik atamak için:

1. Kullanarak Azure 'da oturum açın `Connect-AzAccount` . Sanal makine ölçek kümesini içeren Azure aboneliğiyle ilişkili bir hesap kullanın. Ayrıca, hesabınızın "sanal makine katılımcısı" gibi, sanal makine ölçek kümesinde yazma izinleri veren bir role ait olduğundan emin olun:

   ```powershell
   Connect-AzAccount
   ```

2. İlk olarak cmdlet 'ini kullanarak sanal makine ölçek kümesi özelliklerini alın `Get-AzVM` . Sonra, sanal makine ölçek kümesine Kullanıcı tarafından atanan bir yönetilen kimlik atamak için `-IdentityType` `-IdentityID` [Update-azvmss](/powershell/module/az.compute/update-azvmss) cmdlet 'inde ve anahtarını kullanın. ,,,,, `<VM NAME>` `<SUBSCRIPTION ID>` `<RESROURCE GROUP>` `<USER ASSIGNED ID1>` `USER ASSIGNED ID2` Değerlerini kendi değerlerinizle değiştirin.

   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen kimliği bir Azure sanal makine ölçek kümesinden kaldırma

Sanal makine ölçek kümesinde birden çok kullanıcı tarafından atanan yönetilen kimlik varsa, aşağıdaki komutları kullanarak en son biri hariç tümünü kaldırabilirsiniz. `<RESOURCE GROUP>` ve `<VIRTUAL MACHINE SCALE SET NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. , `<USER ASSIGNED IDENTITY NAME>` Kullanıcı tarafından atanan yönetilen kimliğin Name özelliği olduğundan sanal makine ölçek kümesinde kalması gerekir. Bu bilgiler, şu kullanılarak sanal makine ölçek kümesinin kimlik bölümünde bulunabilir `az vmss show` :

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED IDENTITY NAME>"
```
Sanal makine ölçek kümesinde sistem tarafından atanan yönetilen bir kimlik yoksa ve Kullanıcı tarafından atanan tüm yönetilen kimlikleri bundan kaldırmak istiyorsanız aşağıdaki komutu kullanın:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Sanal makine ölçek kümesinde hem sistem tarafından atanan hem de Kullanıcı tarafından atanan Yönetilen kimlikler varsa, yalnızca sistem tarafından atanan yönetilen kimliği kullan ' a geçerek Kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırabilirsiniz.

```powershell 
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için Yönetilen kimlikler](overview.md)
- Azure VM oluşturma hızlı başlangıçlarını tam olarak görmek için bkz.:
  
  - [PowerShell ile Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 

















