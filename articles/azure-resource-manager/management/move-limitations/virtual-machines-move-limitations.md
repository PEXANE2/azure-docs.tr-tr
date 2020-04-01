---
title: Azure VM'leri yeni abonelik veya kaynak grubuna taşıma
description: Sanal makineleri yeni bir kaynak grubuna veya aboneye taşımak için Azure Kaynak Yöneticisi'ni kullanın.
ms.topic: conceptual
ms.date: 03/31/2020
ms.openlocfilehash: df34268b7741f76621c290e9979cf24d828ddc09
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478658"
---
# <a name="move-guidance-for-virtual-machines"></a>Sanal makineler için kılavuzu taşıma

Bu makalede, şu anda desteklenmeyen senaryolar ve yedekleme ile sanal makineleri taşımak için adımlar açıklanır.

## <a name="scenarios-not-supported"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar henüz desteklenmez:

* Kullanılabilirlik Bölgelerindeyönetilen Diskler farklı bir aboneye taşınamaz.
* Standart SKU Yük Dengeleyiciveya Standart SKU Public IP'li Sanal Makine Ölçek Setleri hareket ettirilemez.
* Planların eklenmiş olduğu Marketplace kaynaklarından oluşturulan sanal makineler kaynak grupları veya abonelikler arasında taşıılamaz. Geçerli abonelikteki sanal makineyi sağlamadan ayırın ve yeni abonelikte yeniden dağıtın.
* Varolan bir sanal ağdaki sanal makineler, sanal ağdaki tüm kaynakları taşımadığınızda yeni bir aboneye taşınamamaktadır.
* Düşük öncelikli sanal makineler ve düşük öncelikli sanal makine ölçek kümeleri kaynak grupları veya abonelikler arasında taşıedilemez.
* Kullanılabilirlik kümesindeki sanal makineler tek tek hareket ettirilemiyor.

## <a name="virtual-machines-with-azure-backup"></a>Azure Yedekleme ile sanal makineler

Azure Yedekleme ile yapılandırılan sanal makineleri taşımak için geri yükleme noktalarını kasadan silmeniz gerekir.

Sanal makineniz için [yumuşak silme](../../../backup/backup-azure-security-feature-cloud.md) etkinleştirilirse, bu geri yükleme noktaları tutulurken sanal makineyi taşıyamazsınız. [Yumuşak silmeyi devre dışı bırakın](../../../backup/backup-azure-security-feature-cloud.md#disabling-soft-delete) veya geri yükleme noktalarını sildikten sonra 14 gün bekleyin.

### <a name="portal"></a>Portal

1. Yedekleme için yapılandırılan sanal makineyi seçin.

1. Sol bölmede **Yedek'i**seçin.

1. **Yedeklemeyi Durdur'u**seçin.

1. **Verileri sil'i**seçin.

1. Silme tamamlandıktan sonra kasa ve sanal makineyi hedef aboneye taşıyabilirsiniz. Taşımadan sonra yedeklemelere devam edebilirsiniz.

### <a name="powershell"></a>PowerShell

* Sanal Makinenizin konumunu bulun.
* Aşağıdaki adlandırma deseni olan bir `AzureBackupRG_<location of your VM>_1` kaynak grubu bulun: örneğin, AzureBackupRG_westus2_1
* PowerShell'de ise `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1` cmdlet'i kullanın
* Adlandırma deseni `Microsoft.Compute/restorePointCollections` olan türü olan kaynağı bulma`AzureBackup_<name of your VM that you're trying to move>_###########`
* Bu kaynağı silin. Bu işlem kasadaki yedeklenmiş verileri değil, yalnızca anlık kurtarma noktalarını siler.

### <a name="azure-cli"></a>Azure CLI

* Sanal Makinenizin konumunu bulun.
* Aşağıdaki adlandırma deseni olan bir `AzureBackupRG_<location of your VM>_1` kaynak grubu bulun: örneğin, AzureBackupRG_westus2_1
* CLI'de ise,`az resource list -g AzureBackupRG_<location of your VM>_1`
* Adlandırma deseni `Microsoft.Compute/restorePointCollections` olan türü olan kaynağı bulma`AzureBackup_<name of your VM that you're trying to move>_###########`
* Bu kaynağı silin. Bu işlem kasadaki yedeklenmiş verileri değil, yalnızca anlık kurtarma noktalarını siler.

## <a name="next-steps"></a>Sonraki adımlar

* Komutların kaynakları taşıması [için](../move-resource-group-and-subscription.md)bkz.

* Yedekleme için Kurtarma Hizmeti kasalarını taşıma hakkında bilgi için, [Kurtarma Hizmetleri sınırlamalarına](../../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)bakın.
