---
title: Azure sanal makinelerini yeni aboneliğe veya kaynak grubuna taşıyın | Microsoft Docs
description: Sanal makineleri yeni bir kaynak grubuna veya aboneliğe taşımak için Azure Resource Manager kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: tomfitz
ms.openlocfilehash: 443d6f2bcbb61d9106b079a4e63c48bb433d19c6
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286725"
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

Azure Backup ile yapılandırılmış sanal makineleri taşımak için aşağıdaki geçici çözümü kullanın:

* Sanal makinenizin konumunu bulun.
* Şu adlandırma düzenine sahip bir kaynak grubu bulun: `AzureBackupRG_<location of your VM>_1` Örneğin, AzureBackupRG_westus2_1
* Azure portal ' de, "gizli türleri göster" seçeneğini işaretleyin.
* PowerShell 'de `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1` cmdlet 'ini kullanın
* CLı 'de `az resource list -g AzureBackupRG_<location of your VM>_1` ' ı kullanın.
* @No__t-0 türünde `AzureBackup_<name of your VM that you're trying to move>_###########` adlandırma düzenine sahip kaynağı bulun
* Bu kaynağı silin. Bu işlem, kasadaki yedeklenen verileri değil yalnızca anlık kurtarma noktalarını siler.
* Silme işlemi tamamlandıktan sonra, kasayı ve sanal makineyi hedef aboneliğe taşıyabilirsiniz. Taşıma işleminden sonra verilerde kayıp olmadan yedeklemeye devam edebilirsiniz.
* Kurtarma hizmeti kasalarını yedekleme için taşıma hakkında daha fazla bilgi için bkz. [Kurtarma Hizmetleri sınırlamaları](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="next-steps"></a>Sonraki adımlar

Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../resource-group-move-resources.md).
