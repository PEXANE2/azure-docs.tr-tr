---
title: Azure VM'leri yeni abonelik veya kaynak grubuna taşıma
description: Sanal makineleri yeni bir kaynak grubuna veya aboneye taşımak için Azure Kaynak Yöneticisi'ni kullanın.
ms.topic: conceptual
ms.date: 10/10/2019
ms.openlocfilehash: 97c49f90dab2aafd89de322e57ad44ff1fc9d367
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75479767"
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

Azure Yedekleme ile yapılandırılan sanal makineleri taşımak için aşağıdaki geçici çözümden yararlanın:

* Sanal Makinenizin konumunu bulun.
* Aşağıdaki adlandırma deseni olan bir `AzureBackupRG_<location of your VM>_1` kaynak grubu bulun: örneğin, AzureBackupRG_westus2_1
* Azure portalında ise ,"Gizli türleri göster" işaretleyin
* PowerShell'de ise `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1` cmdlet'i kullanın
* CLI'de ise,`az resource list -g AzureBackupRG_<location of your VM>_1`
* Adlandırma deseni `Microsoft.Compute/restorePointCollections` olan türü olan kaynağı bulma`AzureBackup_<name of your VM that you're trying to move>_###########`
* Bu kaynağı silin. Bu işlem kasadaki yedeklenmiş verileri değil, yalnızca anlık kurtarma noktalarını siler.
* Silme tamamlandıktan sonra kasa ve sanal makineyi hedef aboneye taşıyabilirsiniz. Taşımadan sonra, veri kaybı olmadan yedeklemelere devam edebilirsiniz.
* Yedekleme için Kurtarma Hizmeti kasalarını taşıma hakkında bilgi için, [Kurtarma Hizmetleri sınırlamalarına](../../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Komutların kaynakları taşıması [için](../move-resource-group-and-subscription.md)bkz.
