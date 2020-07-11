---
title: Azure portal-SSE ile yönetilen disklerle müşteri tarafından yönetilen anahtarları etkinleştirin
description: Yönetilen disklerinizde Azure portal aracılığıyla müşteri tarafından yönetilen anahtarları etkinleştirin.
author: roygara
ms.date: 07/10/2020
ms.topic: how-to
ms.author: rogarana
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 0128d2b88d6eb2464ef95824330cfab07ee3703d
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86236148"
---
# <a name="enable-customer-managed-keys-with-server-side-encryption---managed-disks---portal"></a>Sunucu tarafı şifreleme ile yönetilen diskler ile müşteri tarafından yönetilen anahtarları etkinleştirme-Portal

Azure Disk Depolama, ' yi seçerseniz yönetilen diskler için sunucu tarafı şifreleme (SSE) kullanırken kendi anahtarlarınızı yönetmenizi sağlar. Müşteri tarafından yönetilen anahtarlar ve diğer yönetilen disk şifreleme türleri ile SSE hakkında kavramsal bilgiler için, disk şifreleme makalemize ait [müşteri tarafından yönetilen anahtarlar](disk-encryption.md#customer-managed-keys) bölümüne bakın.

## <a name="restrictions"></a>Kısıtlamalar

Şimdilik, müşteri tarafından yönetilen anahtarlar aşağıdaki kısıtlamalara sahiptir:

- Diskiniz için bu özellik etkinleştirilirse, devre dışı bırakılamaz.
    Bu sorunu geçici olarak çözmek için, [tüm verileri](disks-upload-vhd-to-managed-disk-cli.md#copy-a-managed-disk) , müşteri tarafından yönetilen anahtarları kullanmayan tamamen farklı bir yönetilen diske kopyalamanız gerekir.
[!INCLUDE [virtual-machines-managed-disks-customer-managed-keys-restrictions](../../../includes/virtual-machines-managed-disks-customer-managed-keys-restrictions.md)]

[!INCLUDE [virtual-machines-disks-encryption-portal](../../../includes/virtual-machines-disks-encryption-portal.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, Azure Active Directory (Azure AD) bir özelliği olan Azure kaynakları için yönetilen kimliklere bağımlıdır. Müşteri tarafından yönetilen anahtarları yapılandırırken, bir yönetilen kimlik, kapsamakta olan kaynaklara otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya yönetilen diski bir Azure AD dizininden diğerine taşırsanız, yönetilen disklerle ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, bu nedenle müşterinin yönettiği anahtarlar artık çalışmayabilir. Daha fazla bilgi için bkz. [Azure AD dizinleri arasında abonelik aktarma](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).

## <a name="next-steps"></a>Sonraki adımlar

- [Müşteri tarafından yönetilen anahtarlarla şifrelenmiş diskler oluşturmak için Azure Resource Manager şablonlarını keşfet](https://github.com/ramankumarlive/manageddiskscmkpreview))
- [Makineleri, müşteri tarafından yönetilen anahtarlar etkinleştirilmiş disklerle Çoğalt](../../site-recovery/azure-to-azure-how-to-enable-replication-cmk-disks.md)
- [PowerShell ile Azure 'da VMware VM 'lerinin olağanüstü durum kurtarması ayarlama](../../site-recovery/vmware-azure-disaster-recovery-powershell.md#replicate-vmware-vms)
- [PowerShell ve Azure Resource Manager kullanarak Hyper-V VM 'Leri için Azure 'da olağanüstü durum kurtarmayı ayarlama](../../site-recovery/hyper-v-azure-powershell-resource-manager.md#step-7-enable-vm-protection)
