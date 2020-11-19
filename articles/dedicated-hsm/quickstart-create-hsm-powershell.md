---
title: 'Hızlı başlangıç: Azure PowerShell Azure ayrılmış HSM oluşturma'
description: Azure PowerShell ile Azure ayrılmış HSM oluşturma
services: dedicated-hsm
author: msmbaldwin
ms.author: mbaldwin
ms.topic: quickstart
ms.service: key-vault
ms.devlang: azurepowershell
ms.date: 11/13/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: eebfb257d0324cf2771bd3af979ddbebb8429fb7
ms.sourcegitcommit: 230d5656b525a2c6a6717525b68a10135c568d67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94905633"
---
# <a name="quickstart-create-an-azure-dedicated-hsm-with-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell Azure ayrılmış HSM oluşturma

Bu makalede, [az. ayrılmış Atedhsm](/powershell/module/az.dedicatedhsm) PowerShell modülünü kullanarak Azure ADANMıŞ bir HSM 'nin nasıl oluşturabileceğiniz açıklanmaktadır.

## <a name="requirements"></a>Gereksinimler

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [azure-powershell-requirements-no-header.md](../../includes/azure-powershell-requirements-no-header.md)]

  > [!IMPORTANT]
  > **Az. ayrılmış Atedhsm** PowerShell modülü önizlemedeyken, cmdlet 'ini kullanarak ayrı olarak yüklemelisiniz `Install-Module` . Bu PowerShell modülü genel kullanıma sunulduğunda, gelecekteki az PowerShell modülü sürümlerinin bir parçası olur ve Azure Cloud Shell içinden varsayılan olarak kullanılabilir.

  ```azurepowershell-interactive
  Install-Module -Name Az.DedicatedHsm
  ```

* Birden çok Azure aboneliğiniz varsa, kaynakların faturalandırılması gereken uygun aboneliği seçin. [Set-AzContext](/powershell/module/az.accounts/set-azcontext) cmdlet 'ini kullanarak belirli bir abonelik seçin.

  ```azurepowershell-interactive
  Set-AzContext -SubscriptionId 00000000-0000-0000-0000-000000000000
  ```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) cmdlet 'ini kullanarak bir [Azure Kaynak grubu](../azure-resource-manager/management/overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek, belirtilen ada ve belirtilen konuma sahip bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzResourceGroup -Name myRG -Location westus
```

## <a name="create-a-dedicated-hsm"></a>Adanmış bir HSM oluşturma

Adanmış bir HSM oluşturmak için [New-az, Atedhsm](/powershell/module/az.dedicatedhsm/new-azdedicatedhsm) cmdlet 'ini kullanırsınız. Aşağıdaki örnek, belirtilen abonelikte ayrılmış bir HSM oluşturur.

```azurepowershell-interactive
$Params = @{
  Name  = 'MyHSM'
  ResourceGroupName = 'myRG'
  Location = 'westus'
  Sku = 'SafeNet Luna Network HSM A790'
  StampId = 'stamp1'
  SubnetId = '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myhsm-vnet/subnets/hsmsubnet'
  NetworkInterface = @{PrivateIPAddress = '10.2.1.120'}
}
New-AzDedicatedHsm @Params
```

```Output
Name       Provisioning State SKU                           Location
----       ------------------ ---                           --------
myhsm      Succeeded          SafeNet Luna Network HSM A790 westus
```

## <a name="get-a-dedicated-hsm"></a>Adanmış bir HSM al

Mevcut bir ayrılmış HSM hakkında bilgi almak için [Get-Azrootatedhsm](/powershell/module/az.dedicatedhsm/get-azdedicatedhsm) cmdlet 'ini kullanırsınız. Aşağıdaki örnek, belirtilen ayrılmış HSM 'yi alır.

```azurepowershell-interactive
Get-AzDedicatedHsm -Name MyHSM -ResourceGroupName myRG
```

```Output
Name       Provisioning State SKU                           Location
----       ------------------ ---                           --------
myhsm      Succeeded          SafeNet Luna Network HSM A790 westus
```

## <a name="update-a-dedicated-hsm"></a>Adanmış bir HSM 'yi güncelleştirme

Ayrılmış bir HSM 'yi güncelleştirmek için [Update-az, Atedhsm](/powershell/module/az.dedicatedhsm/update-azdedicatedhsm) cmdlet 'ini kullanırsınız. Aşağıdaki örnek, belirtilen abonelikte ayrılmış bir HSM 'yi güncelleştirir.

```azurepowershell-interactive
Update-AzDedicatedHsm -Name MyHSM -ResourceGroupName myRG -Tag @{'key1' = '1'; 'key2' = 2; 'key3' = 3}
```

```Output
PS C:\>Update-AzDedicatedHsm -Name  hsm-n7wfxi -ResourceGroupName dedicatedhsm-rg-n359cz -Tag @{'key1' = '1';
'key2' = 2; 'key3' = 3}

Name       Provisioning State SKU                           Location
----       ------------------ ---                           --------
myhsm      Succeeded          SafeNet Luna Network HSM A790 westus
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynaklar gerekmiyorsa, aşağıdaki örnekleri çalıştırarak bunları silebilirsiniz.

### <a name="remove-a-dedicated-hsm"></a>Adanmış bir HSM 'yi kaldırma

Adanmış bir HSM 'yi kaldırmak için [Remove-az, Atedhsm](/powershell/module/az.dedicatedhsm/remove-azdedicatedhsm) cmdlet 'ini kullanırsınız. Aşağıdaki örnek, belirtilen ayrılmış HSM 'yi siler.

```azurepowershell-interactive
Remove-AzDedicatedHsm -Name hsm-7t2xaf -ResourceGroupName lucas-manual-test
```

### <a name="delete-the-resource-group"></a>Kaynak grubunu silme

> [!CAUTION]
> Aşağıdaki örnek, belirtilen kaynak grubunu ve içinde yer alan tüm kaynakları siler.
> Bu makalenin kapsamı dışındaki kaynaklar belirtilen kaynak grubunda mevcutsa, bunlar da silinir.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myRG
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure ayrılmış HSM](overview.md)hakkında daha fazla bilgi edinin.
