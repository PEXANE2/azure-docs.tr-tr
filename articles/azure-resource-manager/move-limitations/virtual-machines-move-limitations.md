---
title: Azure sanal makinelerini yeni aboneliğe veya kaynak grubuna taşıyın | Microsoft Docs
description: Sanal makineleri yeni bir kaynak grubuna veya aboneliğe taşımak için Azure Resource Manager kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 7b9cce7ac367f42329e3198c75a7640a205d01fe
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70035534"
---
# <a name="move-guidance-for-virtual-machines"></a>Sanal makineler için taşıma Kılavuzu

Bu makalede, şu anda desteklenmeyen senaryolar ve sanal makineleri yedekleme ile taşıma adımları açıklanır.

## <a name="scenarios-not-supported"></a>Senaryolar desteklenmez

Aşağıdaki senaryolar henüz desteklenmemektedir:

* Kullanılabilirlik Alanları yönetilen diskler farklı bir aboneliğe taşınamaz.
* Standart SKU 'SU Load Balancer veya standart SKU genel IP 'si olan sanal makine ölçek kümeleri taşınamaz.
* Market kaynaklardan bağlı planlar ile oluşturulan sanal makineler, kaynak grubu veya abonelik arasında taşınamaz. Sanal makinenin geçerli abonelikte sağlamasını kaldırın ve yeni abonelikte yeniden dağıtın.
* Var olan bir sanal ağdaki sanal makineler, ancak sanal ağdaki tüm kaynakları taşımıyor.
* Düşük öncelikli sanal makineler ve düşük öncelikli sanal makine ölçek kümeleri kaynak grupları veya abonelikler arasında taşınamaz.

## <a name="virtual-machines-with-azure-backup"></a>Azure Backup olan sanal makineler

Azure Backup ile yapılandırılmış sanal makineleri taşımak için aşağıdaki geçici çözümü kullanın:

* Sanal makinenizin konumunu bulun.
* Aşağıdaki adlandırma düzenine sahip bir kaynak grubu bulun: `AzureBackupRG_<location of your VM>_1` Örneğin, AzureBackupRG_westus2_1
* Azure portalında, ardından onay "gizli türleri Göster ise"
* PowerShell'de varsa, kullanmak `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1` cmdlet'i
* CLI, kullanın `az resource list -g AzureBackupRG_<location of your VM>_1`
* Adlandırma düzenine sahip olan türde `Microsoft.Compute/restorePointCollections` kaynağı bulma`AzureBackup_<name of your VM that you're trying to move>_###########`
* Bu kaynağı silin. Bu işlem, kasadaki yedeklenen verileri değil yalnızca anlık kurtarma noktalarını siler.
* Silme işlemi tamamlandıktan sonra, kasayı ve sanal makineyi hedef aboneliğe taşıyabilirsiniz. Taşıma işleminden sonra verilerde kayıp olmadan yedeklemeye devam edebilirsiniz.
* Kurtarma hizmeti kasalarını yedekleme için taşıma hakkında daha fazla bilgi için bkz. [Kurtarma Hizmetleri sınırlamaları](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="next-steps"></a>Sonraki adımlar

Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../resource-group-move-resources.md).
