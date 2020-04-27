---
title: Azure VM 'lerini yeni aboneliğe veya kaynak grubuna taşıma
description: Sanal makineleri yeni bir kaynak grubuna veya aboneliğe taşımak için Azure Resource Manager kullanın.
ms.topic: conceptual
ms.date: 03/31/2020
ms.openlocfilehash: 144888c4a66ef68448ae8bc863f6aef0923dfb69
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "82160128"
---
# <a name="move-guidance-for-virtual-machines"></a>Sanal makineler için taşıma Kılavuzu

Bu makalede, şu anda desteklenmeyen senaryolar ve sanal makineleri yedekleme ile taşıma adımları açıklanır.

## <a name="scenarios-not-supported"></a>Senaryolar desteklenmez

Aşağıdaki senaryolar henüz desteklenmemektedir:

* Kullanılabilirlik Alanları yönetilen diskler farklı bir aboneliğe taşınamaz.
* Standart SKU 'SU Load Balancer veya standart SKU genel IP 'si olan sanal makine ölçek kümeleri taşınamaz.
* Plan ekli olan market kaynaklarından oluşturulan sanal makineler, kaynak grupları veya abonelikler arasında taşınamaz. Sanal makinenin geçerli abonelikte sağlamasını kaldırın ve yeni abonelikte yeniden dağıtın.
* Sanal ağdaki tüm kaynakları taşımadan, var olan bir sanal ağdaki sanal makineler yeni bir aboneliğe taşınamaz.
* Düşük öncelikli sanal makineler ve düşük öncelikli sanal makine ölçek kümeleri kaynak grupları veya abonelikler arasında taşınamaz.
* Bir kullanılabilirlik kümesindeki sanal makineler tek tek taşınamaz.

## <a name="virtual-machines-with-azure-backup"></a>Azure Backup olan sanal makineler

Azure Backup ile yapılandırılmış sanal makineleri taşımak için, geri yükleme noktalarını kasadan silmeniz gerekir.

Sanal makineniz için [geçici silme](../../../backup/backup-azure-security-feature-cloud.md) etkinse, bu geri yükleme noktaları tutulurken sanal makineyi taşıyamazsınız. Geri yükleme noktalarını sildikten sonra [geçici silmeyi devre dışı bırakın](../../../backup/backup-azure-security-feature-cloud.md#disabling-soft-delete) veya 14 gün bekleyin.

### <a name="portal"></a>Portal

1. Yedeklemeyi geçici olarak durdurun ve yedekleme verilerini koruyun.
2. Azure Backup ile yapılandırılmış sanal makineleri taşımak için aşağıdaki adımları uygulayın:

   1. Sanal makinenizin konumunu bulun.
   2. Şu adlandırma düzenine sahip bir kaynak grubu bulun: `AzureBackupRG_<location of your VM>_1`. Örneğin, *AzureBackupRG_westus2_1*
   3. Azure portal, **gizli türleri göster**' i işaretleyin.
   4. Adlandırma düzenine `AzureBackup_<name of your VM that you're trying to move>_###########`sahip **Microsoft. COMPUTE/restorepointcollections** türünde kaynağı bulun.
   5. Bu kaynağı silin. Bu işlem, kasadaki yedeklenen verileri değil yalnızca anlık kurtarma noktalarını siler.
   6. Silme işlemi tamamlandıktan sonra, sanal makinenizi taşıyabilirsiniz.

3. VM 'yi hedef kaynak grubuna taşıyın.
4. Yedeklemeyi sürdürür.

### <a name="powershell"></a>PowerShell

* Sanal makinenizin konumunu bulun.
* Aşağıdaki adlandırma düzenine sahip bir kaynak grubu bulun: `AzureBackupRG_<location of your VM>_1` örneğin, AzureBackupRG_westus2_1
* PowerShell 'de `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1` cmdlet 'ini kullanın
* Adlandırma düzenine sahip olan türde `Microsoft.Compute/restorePointCollections` kaynağı bulma`AzureBackup_<name of your VM that you're trying to move>_###########`
* Bu kaynağı silin. Bu işlem, kasadaki yedeklenen verileri değil yalnızca anlık kurtarma noktalarını siler.

### <a name="azure-cli"></a>Azure CLI

* Sanal makinenizin konumunu bulun.
* Aşağıdaki adlandırma düzenine sahip bir kaynak grubu bulun: `AzureBackupRG_<location of your VM>_1` örneğin, AzureBackupRG_westus2_1
* CLı 'de şunu kullanın`az resource list -g AzureBackupRG_<location of your VM>_1`
* Adlandırma düzenine sahip olan türde `Microsoft.Compute/restorePointCollections` kaynağı bulma`AzureBackup_<name of your VM that you're trying to move>_###########`
* Bu kaynağı silin. Bu işlem, kasadaki yedeklenen verileri değil yalnızca anlık kurtarma noktalarını siler.

## <a name="next-steps"></a>Sonraki adımlar

* Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../move-resource-group-and-subscription.md).

* Kurtarma hizmeti kasalarını yedekleme için taşıma hakkında daha fazla bilgi için bkz. [Kurtarma Hizmetleri sınırlamaları](../../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).
