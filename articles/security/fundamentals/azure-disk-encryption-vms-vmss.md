---
title: Sanal makineler ve sanal makine ölçek kümeleri için Azure Disk Şifrelemesi
description: Bu makale, Azure Disk Şifreleme'ye genel bir bakış sağlar
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 10/15/2019
ms.custom: seodec18
ms.openlocfilehash: c881b2b9743766e4d35e6cb05f6f3469803850bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062117"
---
# <a name="azure-disk-encryption-for-virtual-machines-and-virtual-machine-scale-sets"></a>Sanal makineler ve sanal makine ölçek kümeleri için Azure Disk Şifrelemesi

Azure Disk şifrelemesi hem Linux hem de Windows sanal makinelerine ve sanal makine ölçek kümelerine uygulanabilir. 

## <a name="linux-virtual-machines"></a>Linux sanal makineleri

Aşağıdaki makaleler, Linux sanal makinelerinin şifrelemi için kılavuz sağlar.

### <a name="current-version-of-azure-disk-encryption"></a>Azure Disk Şifreleme'nin geçerli sürümü

- [Linux sanal makineleri için Azure Disk Şifrelemesi'ne genel bakış](../../virtual-machines/linux/disk-encryption-overview.md)
- [Linux VM'lerinde Azure Disk Şifrelemesi senaryoları](../../virtual-machines/linux/disk-encryption-linux.md)
- [Azure CLI ile Linux VM oluşturma ve şifreleme](../../virtual-machines/linux/disk-encryption-cli-quickstart.md)
- [Azure PowerShell ile Linux VM oluşturma ve şifreleme](../../virtual-machines/linux/disk-encryption-powershell-quickstart.md)
- [Azure portalı ile Linux VM oluşturma ve şifreleme](../../virtual-machines/linux/disk-encryption-portal-quickstart.md)
- [Linux için Azure Disk Şifreleme Uzantısı Şeması](../../virtual-machines/extensions/azure-disk-enc-linux.md)
- [Azure Disk Şifrelemesi için anahtar kasası oluşturma ve yapılandırma](../../virtual-machines/linux/disk-encryption-key-vault.md)
- [Azure Disk Şifrelemesi örnek betikleri](../../virtual-machines/linux/disk-encryption-sample-scripts.md)
- [Azure Disk Şifrelemesi sorunlarını giderme](../../virtual-machines/linux/disk-encryption-troubleshooting.md)
- [Azure Disk Şifrelemesi hakkında sık sorulan sorular](../../virtual-machines/linux/disk-encryption-faq.md)

### <a name="azure-disk-encryption-with-azure-ad-previous-version"></a>Azure AD ile Azure disk şifrelemesi (önceki sürüm)

- [Linux sanal makineleri için Azure AD ile Azure Disk Şifreleme'ye genel bakış](../../virtual-machines/linux/disk-encryption-overview-aad.md)
- [Linux VM'lerinde Azure AD senaryoları ile Azure Disk Şifreleme](../../virtual-machines/linux/disk-encryption-linux.md)
- [Azure AD ile Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma (önceki sürüm)](../../virtual-machines/linux/disk-encryption-key-vault-aad.md)

## <a name="windows-virtual-machines"></a>Windows sanal makineleri

Aşağıdaki makaleler, Windows sanal makinelerini şifrelemek için kılavuz sağlar.

### <a name="current-version-of-azure-disk-encryption"></a>Azure Disk Şifreleme'nin geçerli sürümü

- [Windows sanal makineleri için Azure Disk Şifrelemesi'ne genel bakış](../../virtual-machines/windows/disk-encryption-overview.md)
- [Windows VM'lerinde Azure Disk Şifrelemesi senaryoları](../../virtual-machines/windows/disk-encryption-windows.md)
- [Azure CLI ile Windows VM oluşturma ve şifreleme](../../virtual-machines/windows/disk-encryption-cli-quickstart.md)
- [Azure PowerShell ile Windows VM oluşturma ve şifreleme](../../virtual-machines/windows/disk-encryption-powershell-quickstart.md)
- [Azure portalı ile Windows VM oluşturma ve şifreleme](../../virtual-machines/windows/disk-encryption-portal-quickstart.md)
- [Windows için Azure Disk Şifreleme Uzantısı Şeması](../../virtual-machines/extensions/azure-disk-enc-windows.md)
- [Azure Disk Şifrelemesi için anahtar kasası oluşturma ve yapılandırma](../../virtual-machines/windows/disk-encryption-key-vault.md)
- [Azure Disk Şifrelemesi örnek betikleri](../../virtual-machines/windows/disk-encryption-sample-scripts.md)
- [Azure Disk Şifrelemesi sorunlarını giderme](../../virtual-machines/windows/disk-encryption-troubleshooting.md)
- [Azure Disk Şifrelemesi hakkında sık sorulan sorular](../../virtual-machines/windows/disk-encryption-faq.md)

### <a name="azure-disk-encryption-with-azure-ad-previous-version"></a>Azure AD ile Azure disk şifrelemesi (önceki sürüm)

- [Windows sanal makineleri için Azure AD ile Azure Disk Şifrelemeye Genel Bakış](../../virtual-machines/windows/disk-encryption-overview-aad.md)
- [Windows VM'lerde Azure REKLAM senaryoları ile Azure Disk Şifreleme](../../virtual-machines/windows/disk-encryption-windows.md)
- [Azure AD ile Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma (önceki sürüm)](../../virtual-machines/windows/disk-encryption-key-vault-aad.md)

## <a name="virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri

Aşağıdaki makaleler, sanal makine ölçek kümelerini şifrelemek için kılavuz sağlar.

- [Sanal makine ölçek kümeleri için Azure Disk Şifreleme'ye genel bakış](../../virtual-machine-scale-sets/disk-encryption-overview.md) 
- [Azure CLI'yi kullanarak sanal makine ölçeği kümelerini şifreleme](../../virtual-machine-scale-sets/disk-encryption-cli.md) 
- [Azure Powershell'i kullanarak sanal makine ölçeği kümelerini şifreleyin.](../../virtual-machine-scale-sets/disk-encryption-powershell.md)
- [Azure Kaynak Yöneticisi'ni kullanarak sanal makine ölçeği kümelerini şifreleme](../../virtual-machine-scale-sets/disk-encryption-azure-resource-manager.md)
- [Azure Disk Şifreleme için önemli bir kasa oluşturma ve yapılandırma](../../virtual-machine-scale-sets/disk-encryption-key-vault.md)
- [Sanal makine ölçeği kümesi uzantısı sıralaması ile Azure Disk Şifreleme'yi kullanma](../../virtual-machine-scale-sets/disk-encryption-extension-sequencing.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure şifrelemeye genel bakış](encryption-overview.md)
- [Bekleme sırasında veri şifrelemesi](encryption-atrest.md)
- [Veri güvenliği ve şifreleme için en iyi uygulamalar](data-encryption-best-practices.md)
