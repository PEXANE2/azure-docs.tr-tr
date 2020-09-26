---
title: Azure VM 'lerini yeni aboneliğe veya kaynak grubuna taşıma
description: Sanal makineleri yeni bir kaynak grubuna veya aboneliğe taşımak için Azure Resource Manager kullanın.
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: 219a8b438d2715f6e97085a527b386e51759ec2c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91317115"
---
# <a name="move-guidance-for-virtual-machines"></a>Sanal makineler için taşıma Kılavuzu

Bu makalede, şu anda desteklenmeyen senaryolar ve sanal makineleri yedekleme ile taşıma adımları açıklanır.

## <a name="scenarios-not-supported"></a>Senaryolar desteklenmez

Aşağıdaki senaryolar henüz desteklenmemektedir:

* Standart SKU 'SU Load Balancer veya standart SKU genel IP 'si olan sanal makine ölçek kümeleri taşınamaz.
* Planların eklendiği Market kaynaklarından oluşturulan sanal makineler abonelikler arasında taşınamaz. Sanal makinenin geçerli abonelikte sağlamasını kaldırın ve yeni abonelikte yeniden dağıtın.
* Sanal ağdaki tüm kaynakları taşımadan, var olan bir sanal ağdaki sanal makineler yeni bir aboneliğe taşınamaz.
* Düşük öncelikli sanal makineler ve düşük öncelikli sanal makine ölçek kümeleri kaynak grupları veya abonelikler arasında taşınamaz.
* Bir kullanılabilirlik kümesindeki sanal makineler tek tek taşınamaz.

## <a name="azure-disk-encryption"></a>Azure disk şifrelemesi

[Linux VM 'leri Için Azure disk şifrelemesi](../../../virtual-machines/linux/disk-encryption-overview.md) veya [Windows VM 'Leri Için Azure disk şifrelemesi](../../../virtual-machines/windows/disk-encryption-overview.md)uygulamak üzere bir anahtar kasası ile tümleştirilmiş bir sanal makineyi taşıyamazsınız. VM 'yi taşımak için şifrelemeyi devre dışı bırakmanız gerekir.

```azurecli-interactive
az vm encryption disable --resource-group demoRG --name myVm1
```

```azurepowershell-interactive
Disable-AzVMDiskEncryption -ResourceGroupName demoRG -VMName myVm1
```

## <a name="virtual-machines-with-azure-backup"></a>Azure Backup olan sanal makineler

Azure Backup ile yapılandırılmış sanal makineleri taşımak için, geri yükleme noktalarını kasadan silmeniz gerekir.

Sanal makineniz için [geçici silme](../../../backup/backup-azure-security-feature-cloud.md) etkinse, bu geri yükleme noktaları tutulurken sanal makineyi taşıyamazsınız. Geri yükleme noktalarını sildikten sonra [geçici silmeyi devre dışı bırakın](../../../backup/backup-azure-security-feature-cloud.md#enabling-and-disabling-soft-delete) veya 14 gün bekleyin.

### <a name="portal"></a>Portal

1. Yedeklemeyi geçici olarak durdurun ve yedekleme verilerini koruyun.
2. Azure Backup ile yapılandırılmış sanal makineleri taşımak için aşağıdaki adımları uygulayın:

   1. Sanal makinenizin konumunu bulun.
   2. Şu adlandırma düzenine sahip bir kaynak grubu bulun: `AzureBackupRG_<VM location>_1` . Örneğin, ad *AzureBackupRG_westus2_1*biçimindedir.
   3. Azure portal, **gizli türleri göster**' i işaretleyin.
   4. Adlandırma düzenine sahip **Microsoft. COMPUTE/restorePointCollections** türünde kaynağı bulun `AzureBackup_<VM name>_###########` .
   5. Bu kaynağı silin. Bu işlem, kasadaki yedeklenen verileri değil yalnızca anlık kurtarma noktalarını siler.
   6. Silme işlemi tamamlandıktan sonra, sanal makinenizi taşıyabilirsiniz.

3. VM 'yi hedef kaynak grubuna taşıyın.
4. Yedeklemeyi sürdürür.

### <a name="powershell"></a>PowerShell

1. Sanal makinenizin konumunu bulun.

1. Adlandırma düzenine sahip bir kaynak grubu bulun `AzureBackupRG_<VM location>_1` . Örneğin, ad olabilir `AzureBackupRG_westus2_1` .

1. Yalnızca bir sanal makine taşıyorsanız, bu sanal makine için geri yükleme noktası koleksiyonunu alın.

   ```azurepowershell-interactive
   $restorePointCollection = Get-AzResource -ResourceGroupName AzureBackupRG_<VM location>_1 -name AzureBackup_<VM name>* -ResourceType Microsoft.Compute/restorePointCollections
   ```

   Bu kaynağı silin. Bu işlem, kasadaki yedeklenen verileri değil yalnızca anlık kurtarma noktalarını siler.

   ```azurepowershell-interactive
   Remove-AzResource -ResourceId $restorePointCollection.ResourceId -Force
   ```

1. Bu konumdaki tüm sanal makineleri arka UPS ile taşıyorsanız, bu sanal makineler için geri yükleme noktası koleksiyonlarını alın.

   ```azurepowershell-interactive
   $restorePointCollection = Get-AzResource -ResourceGroupName AzureBackupRG_<VM location>_1 -ResourceType Microsoft.Compute/restorePointCollections
   ```

   Her kaynağı silin. Bu işlem, kasadaki yedeklenen verileri değil yalnızca anlık kurtarma noktalarını siler.

   ```azurepowershell-interactive
   foreach ($restorePoint in $restorePointCollection)
   {
     Remove-AzResource -ResourceId $restorePoint.ResourceId -Force
   }
   ```

### <a name="azure-cli"></a>Azure CLI’si

1. Sanal makinenizin konumunu bulun.

1. Adlandırma düzenine sahip bir kaynak grubu bulun `AzureBackupRG_<VM location>_1` . Örneğin, ad olabilir `AzureBackupRG_westus2_1` .

1. Yalnızca bir sanal makine taşıyorsanız, bu sanal makine için geri yükleme noktası koleksiyonunu alın.

   ```azurecli-interactive
   RESTOREPOINTCOL=$(az resource list -g AzureBackupRG_<VM location>_1 --resource-type Microsoft.Compute/restorePointCollections --query "[?starts_with(name, 'AzureBackup_<VM name>')].id" --output tsv)
   ```

   Bu kaynağı silin. Bu işlem, kasadaki yedeklenen verileri değil yalnızca anlık kurtarma noktalarını siler.

   ```azurecli-interactive
   az resource delete --ids $RESTOREPOINTCOL
   ```

1. Bu konumdaki tüm sanal makineleri arka UPS ile taşıyorsanız, bu sanal makineler için geri yükleme noktası koleksiyonlarını alın.

   ```azurecli-interactive
   RESTOREPOINTCOL=$(az resource list -g AzureBackupRG_<VM location>_1 --resource-type Microsoft.Compute/restorePointCollections)
   ```

   Her kaynağı silin. Bu işlem, kasadaki yedeklenen verileri değil yalnızca anlık kurtarma noktalarını siler.

   ```azurecli-interactive
   az resource delete --ids $RESTOREPOINTCOL
   ```

## <a name="next-steps"></a>Sonraki adımlar

* Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../move-resource-group-and-subscription.md).

* Kurtarma hizmeti kasalarını yedekleme için taşıma hakkında daha fazla bilgi için bkz. [Kurtarma Hizmetleri sınırlamaları](../../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).
