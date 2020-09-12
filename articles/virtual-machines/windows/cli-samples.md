---
title: Azure CLı örnekleri Windows
description: Azure CLı örnekleri Windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 6c4b707a3cebc1bcdad7c9e14a96d82a8dda2371
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89318803"
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Windows sanal makineleri için Azure CLI Örnekleri

Aşağıdaki tabloda, Windows sanal makinelerini dağıtan Azure CLı kullanılarak oluşturulan Bash betiklerinin bağlantıları yer almaktadır.

| Komut Dosyası | Description |
|---|---|
|**Sanal makineler oluşturma**||
| [Sanal makine oluşturma](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | En az yapılandırmaya sahip bir Windows sanal makinesi oluşturur. |
| [Tam olarak yapılandırılmış bir sanal makine oluşturma](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturur.|
| [Yüksek oranda kullanılabilir sanal makine oluşturma](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yüksek oranda kullanılabilir ve yük dengeli bir yapılandırmada birkaç sanal makine oluşturur. |
| [VM oluşturma ve yapılandırma betiği çalıştırma](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure Özel Betik uzantısı 'nı kullanır. |
| [VM oluşturma ve DSC yapılandırması çalıştırma](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure Istenen durum yapılandırması (DSC) uzantısını kullanır. |
|**Depolamayı yönetin**||
| [VHD'den yönetilen disk oluşturma](../scripts/virtual-machines-cli-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Özelleştirilmiş bir VHD 'den işletim sistemi diski olarak ya da veri diski olarak bir veri VHD 'sinden yönetilen bir disk oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir anlık görüntüden yönetilen disk oluşturur. |
| [Yönetilen diski aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yönetilen diski aynı veya farklı aboneliğe, ancak üst yönetilen diskle aynı bölgeye kopyalar. 
| [Anlık görüntüyü depolama hesabına VHD olarak dışarı aktarma](../scripts/virtual-machines-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yönetilen bir anlık görüntüyü, farklı bölgedeki bir depolama hesabına VHD olarak dışa aktarır. |
| [Yönetilen diskin VHD’sini bir depolama hesabına dışarı aktarma](../scripts/virtual-machines-cli-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yönetilen bir diskin temel VHD 'sini farklı bölgedeki bir depolama hesabına dışarı aktarır. |
| [Anlık görüntüyü aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Anlık görüntüyü aynı veya farklı aboneliğe, ancak üst anlık görüntüyle aynı bölgeye kopyalar. |
|**Ağ sanal makineleri**||
| [Sanal makineler arasındaki ağ trafiğinin güvenliğini sağlama](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | İki sanal makine, tüm ilgili kaynakları ve bir iç ve dış ağ güvenlik grupları (NSG) oluşturur. |
|**Sanal makinelerin güvenliğini sağlama**||
| [VM’yi ve veri disklerini şifreleme](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir Azure Key Vault, şifreleme anahtarı ve hizmet sorumlusu oluşturur, ardından bir VM 'yi şifreler. |
|**Sanal makineleri izleme**||
| [Azure Izleyici ile VM izleme](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir sanal makine oluşturur, Log Analytics aracısını yükleyip VM 'yi bir Log Analytics çalışma alanına kaydeder.  |
| | |
