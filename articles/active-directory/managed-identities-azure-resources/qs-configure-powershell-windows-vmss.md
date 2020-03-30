---
title: Yönetilen kimlikleri PowerShell - Azure AD'yi kullanarak sanal makine ölçeği kümelerinde yapılandırma
description: PowerShell'i kullanarak sanal makine ölçeğinde sistem ve kullanıcı tarafından atanan yönetilen kimlikleri yapılandırmak için adım adım yönergeler.
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
ms.openlocfilehash: 755aee312fd0492fd57a82cb7a437b04ebf72987
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74547260"
---
# <a name="configure-managed-identities-for-azure-resources-on-virtual-machine-scale-sets-using-powershell"></a>PowerShell'i kullanarak sanal makine ölçeği kümelerinde Azure kaynaklarıiçin yönetilen kimlikleri yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, PowerShell'i kullanarak, Azure kaynakları işlemleri için yönetilen kimlikleri sanal makine ölçeği kümesinde nasıl gerçekleştireceklerini öğrenirsiniz:
- Sanal makine ölçeği kümesinde sistem tarafından atanan yönetilen kimliği etkinleştirme ve devre dışı
- Sanal makine ölçeği kümesinde kullanıcı tarafından atanan yönetilen kimlik ekleme ve kaldırma

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından yönetilen atanmış kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için, hesabınızda aşağıdaki Azure rol tabanlı erişim denetimi atamaları gerekir:

    > [!NOTE]
    > Ek Azure AD dizini rol atamaları gerekmez.

    - [Sanal makine](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ölçeği kümesi oluşturmak ve sanal makine ölçeği kümesinden sistem tarafından atanan yönetilen ve/veya kullanıcı tarafından atanan yönetilen kimliği etkinleştirmek ve kaldırmak için Sanal Makine Katılımcısı.
    - Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak için [Yönetilen Kimlik Oluşturici](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü.
    - Kullanıcı tarafından atanan yönetilen bir kimliği sanal makine ölçeği kümesinden ve sanal makine ölçeğikümesine atamak ve kaldırmak için [Yönetilen Kimlik İşlemi](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü.
- Azure [PowerShell'in en son sürümünü](/powershell/azure/install-az-ps) yükleyin. 

## <a name="system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik

Bu bölümde, Azure PowerShell kullanarak sistem tarafından atanmış yönetilen bir kimliği etkinleştirmeyi ve kaldırmayı öğrenirsiniz.

### <a name="enable-system-assigned-managed-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Azure sanal makine ölçeği kümesi nin oluşturulması sırasında sistem tarafından atanmış yönetilen kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimlik etkinleştirilmiş sanal bir makine ölçeği kümesi oluşturmak için:

1. Sistem tarafından atanmış yönetilen bir kimliğe sahip sanal bir makine ölçeği kümesi oluşturmak için [Yeni-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) cmdlet referans makalesinde *Örnek 1'e* bakın.  Cmdlet'e `-IdentityType SystemAssigned` `New-AzVmssConfig` parametre ekleyin:

    ```powershell
    $VMSS = New-AzVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```



## <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Varolan bir Azure sanal makine ölçeğinde sistem le atanmış yönetilen kimliği etkinleştirme

Varolan bir Azure sanal makine ölçeğinde sistem tarafından atanmış yönetilen bir kimliği etkinleştirmeniz gerekiyorsa:

1. Azure'da oturum `Connect-AzAccount`açarak oturum açın. Sanal makine ölçeği kümesini içeren Azure aboneliğiyle ilişkili bir hesap kullanın. Ayrıca, hesabınızın sanal makine ölçeği kümesinde "Sanal Makine Katılımcısı" gibi izinler yazmanızı sağlayan bir role ait olduğundan emin olun:

   ```powershell
   Connect-AzAccount
   ```

2. Önce [`Get-AzVmss`](/powershell/module/az.compute/get-azvmss) cmdlet kullanarak sanal makine ölçeği kümesi özelliklerini alın. Daha sonra sistem tarafından atanmış yönetilen `-IdentityType` bir kimliği etkinleştirmek için [Update-AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet üzerindeki anahtarı kullanın:

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```



### <a name="disable-the-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Sistem tarafından atanan yönetilen kimliği Azure sanal makine ölçeği kümesinden devre dışı bırak

Artık sistem tarafından atanan yönetilen kimliğe ihtiyaç duymadığı, ancak yine de kullanıcı tarafından atanan yönetilen kimliklere ihtiyaç dolan sanal bir makine ölçeği kümeniz varsa, aşağıdaki cmdlet'i kullanın:

1. Azure'da oturum `Connect-AzAccount`açarak oturum açın. VM içeren Azure aboneliğiyle ilişkili bir hesap kullanın. Ayrıca, hesabınızın sanal makine ölçeği kümesinde "Sanal Makine Katılımcısı" gibi izinler yazmanızı sağlayan bir role ait olduğundan emin olun:

2. Aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Artık sistem tarafından atanan yönetilen kimliğe ihtiyaç duymadığı sanal bir makine ölçeği kümeniz varsa ve kullanıcı tarafından atanan yönetilen kimlikleri yoksa, aşağıdaki komutları kullanın:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure PowerShell'i kullanarak kullanıcı tarafından atanan yönetilen bir kimliği sanal makine ölçeğinden nasıl ekleyeceğinizi ve kaldırabileceğinizi öğrenirsiniz.

### <a name="assign-a-user-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Azure sanal makine ölçeği kümesi nin oluşturulması sırasında kullanıcı tarafından atanan yönetilen bir kimlik atama

Kullanıcı tarafından atanan yönetilen bir kimliğe sahip yeni bir sanal makine ölçeği kümesi oluşturmak şu anda PowerShell üzerinden desteklenmez. Varolan sanal makine ölçeği kümesine kullanıcı tarafından atanan yönetilen kimliğin nasıl eklenişmeye ilişkin sonraki bölümüne bakın. Güncelleştirmeler için sonra yeniden denetleyin.

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen bir kimliği varolan bir Azure sanal makine ölçeği kümesine atama

Kullanıcı tarafından atanan yönetilen bir kimliği varolan bir Azure sanal makine ölçeği kümesine atamak için:

1. Azure'da oturum `Connect-AzAccount`açarak oturum açın. Sanal makine ölçeği kümesini içeren Azure aboneliğiyle ilişkili bir hesap kullanın. Ayrıca, hesabınızın sanal makine ölçeği kümesinde "Sanal Makine Katılımcısı" gibi izinler yazmanızı sağlayan bir role ait olduğundan emin olun:

   ```powershell
   Connect-AzAccount
   ```

2. Önce `Get-AzVM` cmdlet kullanarak sanal makine ölçeği kümesi özelliklerini alın. Daha sonra sanal makine ölçeği kümesine kullanıcı tarafından atanan `-IdentityType` yönetilen `-IdentityID` bir kimlik atamak için [Update-AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet'i kullanın ve açın. `<VM NAME>`Değiştirin `<SUBSCRIPTION ID>` `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>` `USER ASSIGNED ID2` , , , kendi değerlerinizle.

   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen kimliği Azure sanal makine ölçeği kümesinden kaldırma

Sanal makine ölçek kümenizde birden çok kullanıcı tarafından atanan yönetilen kimlik varsa, aşağıdaki komutları kullanarak sonuncusu hariç tüm kimlikleri kaldırabilirsiniz. `<RESOURCE GROUP>` ve `<VIRTUAL MACHINE SCALE SET NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Kullanıcı `<USER ASSIGNED IDENTITY NAME>` tarafından atanan yönetilen kimliğin, sanal makine ölçeği kümesinde kalması gereken ad özelliğidir. Bu bilgiler kullanarak sanal makine ölçeği kümesinin kimlik `az vmss show`bölümünde bulunabilir:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED IDENTITY NAME>"
```
Sanal makine ölçek kümenizde sistem tarafından atanmış yönetilen bir kimlik yoksa ve kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırmak istiyorsanız, aşağıdaki komutu kullanın:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Sanal makine ölçek kümenizde hem sistem tarafından atanmış hem de kullanıcı tarafından atanan yönetilen kimlikler varsa, yalnızca sistem tarafından atanan yönetilen kimliği kullanmaya geçerek kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırabilirsiniz.

```powershell 
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlikler](overview.md)
- Tam Azure VM oluşturma Quickstarts için bkz:
  
  - [PowerShell ile Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 

















