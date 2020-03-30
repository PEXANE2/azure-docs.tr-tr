---
title: Azure Site Kurtarma kasası silme
description: Azure Site Kurtarma için yapılandırılan Kurtarma Hizmetleri kasasını nasıl silebilirsiniz öğrenin
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/05/2019
ms.author: rajanaki
ms.openlocfilehash: 0e409ffdedbac822aedf48833f2dd85f8e04afa2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894972"
---
# <a name="delete-a-site-recovery-services-vault"></a>Site Recovery Hizmetleri kasasını silme

Bu makalede, Site Kurtarma için bir Kurtarma Hizmetleri kasasının nasıl silinir. Azure Yedekleme'de kullanılan bir kasayı silmek için [bkz.](../backup/backup-azure-delete-vault.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


## <a name="before-you-start"></a>Başlamadan önce

Bir tonozu silmeden önce kayıtlı sunucuları ve kasadaki öğeleri kaldırmanız gerekir. Kaldırmanız gerekenler, dağıttığınız çoğaltma senaryolarına bağlıdır. 


## <a name="delete-a-vault-azure-vm-to-azure"></a>Kasa-Azure VM'sini Azure'a silme

1. Tüm korumalı VM'leri silmek için [bu yönergeleri](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-azure-vm-azure-to-azure) izleyin.
2. Sonra, kasayı silin.

## <a name="delete-a-vault-vmware-vm-to-azure"></a>Bir kasa-VMware VM'yi Azure'a silme

1. Tüm korumalı VM'leri silmek için [bu yönergeleri](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) izleyin.
2. Tüm çoğaltma ilkelerini silmek için [aşağıdaki adımları](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) izleyin.
3. Bu adımları kullanarak vCenter [başvurularını](vmware-azure-manage-vcenter.md#delete-a-vcenter-server)silin.
4. Bir yapılandırma sunucusunun devre dışı bırakılması için [bu yönergeleri](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server) izleyin.
5. Sonra, kasayı silin.


## <a name="delete-a-vault-hyper-v-vm-with-vmm-to-azure"></a>Bir kasa-Hyper-V VM'yi (VMM ile) Azure'a silme

1. System Center VMM tarafından yönetilen Hyper-V VM'leri silmek için [aşağıdaki adımları](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario) izleyin.
2. Tüm çoğaltma ilkelerini ilişkilendirin ve silin. **Sistem Merkezi VMM** > **Çoğaltma İlkeleri**için **Site Kurtarma Altyapısı** > > kasanızda bunu yapın.
3. Bağlı bir VMM sunucusunun kaydını çıkarmak için [aşağıdaki adımları](site-recovery-manage-registration-and-protection.md#unregister-a-vmm-server) izleyin.
4. Sonra, kasayı silin.

## <a name="delete-a-vault-hyper-v-vm-to-azure"></a>Azure'a kasa-Hyper-V VM silme

1. Tüm korumalı VM'leri silmek için [aşağıdaki adımları](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure) izleyin.
2. Tüm çoğaltma ilkelerini ilişkilendirin ve silin. **Hiper-V Siteleri** > **Çoğaltma İlkeleri**için **Site Kurtarma Altyapısı** > > kasanızda bunu yapın.
3. Hyper-V ana bilgisayar kaydını çıkarmak için [bu yönergeleri](site-recovery-manage-registration-and-protection.md#unregister-a-hyper-v-host-in-a-hyper-v-site) izleyin.
4. Hyper-V sitesini silin.
5. Sonra, kasayı silin.


## <a name="use-powershell-to-force-delete-the-vault"></a>Kasayı silmeye zorlamak için PowerShell'i kullanın 

> [!Important]
> Ürünü test ediyorsanız ve veri kaybıyla ilgilenmiyorsanız, kasayı ve tüm bağımlılıklarını hızla kaldırmak için kuvvet silme yöntemini kullanın.
> PowerShell komutu kasanın tüm içeriğini siler ve **geri döndürülemez.**

Korumalı öğeler olsa bile Site Kurtarma kasasını silmek için şu komutları kullanın:

    Connect-AzAccount

    Select-AzSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzRecoveryServicesVault -Name "vaultname"

    Remove-AzRecoveryServicesVault -Vault $vault

[Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault)ve [Remove-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/remove-azrecoveryservicesvault)hakkında daha fazla bilgi edinin.
