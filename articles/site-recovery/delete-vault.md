---
title: Azure Site Recovery hizmeti için yapılandırılmış bir kurtarma hizmetleri kasasını silme
description: Azure Site Recovery için yapılandırılmış bir kurtarma hizmetleri kasasının nasıl silineceğini öğrenin
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: a13dee2010688b02fd86fb05900826470a7d7a08
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67876033"
---
# <a name="delete-a-site-recovery-services-vault"></a>Site Recovery Services kasasını silme

Bağımlılıklar, Azure Site Recovery kasasını silmenizi engelleyebilir. Gerçekleştirmeniz gereken eylemler Site Recovery senaryoya göre farklılık gösterir. Azure Backup kullanılan bir kasayı silmek için bkz. [Azure 'da bir yedekleme kasasını silme](../backup/backup-azure-delete-vault.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="delete-a-site-recovery-vault"></a>Site Recovery kasasını silme 
Kasayı silmek için senaryonuz için önerilen adımları izleyin.
### <a name="azure-vms-to-azure"></a>Azure VM'lerini Azure'a

1. [Bir VMware için korumayı devre dışı bırakma](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-azure-vm-azure-to-azure)' daki adımları izleyerek korunan tüm VM 'leri silin.
2. Kasayı silin.

### <a name="vmware-vms-to-azure"></a>VMware VM'lerini Azure'a

1. [Bir VMware için korumayı devre dışı bırakma](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure)' daki adımları izleyerek korunan tüm VM 'leri silin.

2. [Çoğaltma Ilkesini silme](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy)bölümündeki adımları izleyerek tüm çoğaltma ilkelerini silin.

3. [VCenter Server silme](vmware-azure-manage-vcenter.md#delete-a-vcenter-server)bölümündeki adımları izleyerek vCenter 'a başvuruları silin.

4. Yapılandırma sunucusu [yetkisini](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server)alma bölümündeki adımları izleyerek yapılandırma sunucusunu silin.

5. Kasayı silin.


### <a name="hyper-v-vms-with-vmm-to-azure"></a>Hyper-V VM’lerini (VMM ile) Azure’a
1. [Hyper-V VM (VMM ile) için korumayı devre dışı bırakma](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario)bölümündeki adımları izleyerek korunan tüm VM 'leri silin.

2. **System Center VMM** -> **çoğaltma ilkeleri** için kasanıza > **Site Recovery altyapı** -> & tüm çoğaltma ilkelerini kaldırın ve silin

3.  [Bağlı BIR VMM sunucusunun kaydını](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server)silme bölümündeki ADıMLARı izleyerek VMM sunucularının başvurularını silin.

4.  Kasayı silin.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Azure 'da Hyper-V VM 'Leri (Virtual Machine Manager olmayan)
1. [Hyper-v sanal makinesi için korumayı devre dışı bırakma](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure)' daki adımları izleyerek korunan tüm VM 'leri silin (Hyper-v ' y i Azure 'a).

2. **Hyper-V siteleri** -> **çoğaltma ilkeleri** için kasanıza > Site Recovery > **altyapısına** göz atarak tüm çoğaltma ilkelerinin ilişkisini kaldırın &

3. Hyper- [v konağının kaydını](site-recovery-manage-registration-and-protection.md#unregister-a-hyper-v-host-in-a-hyper-v-site)silme bölümündeki adımları izleyerek Hyper-v sunucularına başvuruları silin.

4. Hyper-V sitesini silin.

5. Kasayı silin.


## <a name="use-powershell-to-force-delete-the-vault"></a>Kasayı silmeyi zorlamak için PowerShell 'i kullanma 

> [!Important]
> Ürünü sınıyorsanız ve veri kaybı hakkında endişe ediyorsanız, kasayı ve tüm bağımlılıklarını hızlı bir şekilde kaldırmak için silmeyi zorla silme yöntemini kullanın.
> PowerShell komutu kasanın tüm içeriğini siler ve geri **alınamaz**.

Korunan öğeler olsa bile Site Recovery kasasını silmek için şu komutları kullanın:

    Connect-AzAccount

    Select-AzSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzRecoveryServicesVault -Name "vaultname"

    Remove-AzRecoveryServicesVault -Vault $vault

[Get-Azrecoveryserviceskasasının](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault)yanı sıra [-Azrecoveryserviceskasasını kaldırma](https://docs.microsoft.com/powershell/module/az.recoveryservices/remove-azrecoveryservicesvault)hakkında daha fazla bilgi edinin.
