---
title: Azure CLI Örnekleri
description: Azure sanal makineleri için CLı örnekleri
author: cynthn
ms.service: virtual-machines-linux
ms.topic: how-to
ms.date: 03/01/2019
ms.author: cynthn
ms.custom: devx-track-azurecli
ms.openlocfilehash: 63c420a7ae082cbf38b529e71d8324550a84b7bd
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89319177"
---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>Linux sanal makineleri için Azure CLI Örnekleri

Aşağıdaki tablo, Azure CLI’si kullanılarak oluşturulan bash komut dosyalarına yönelik bağlantılar içerir.

| Komut Dosyası | Description |
|---|---|
|**Sanal makineler oluşturma**||
| [Sanal makine oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | En az yapılandırmaya sahip bir Linux sanal makinesi oluşturur. |
| [Tam olarak yapılandırılmış bir sanal makine oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturur.|
| [Yüksek oranda kullanılabilir sanal makine oluşturma](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yüksek oranda kullanılabilir ve yük dengeli bir yapılandırmada birkaç sanal makine oluşturur. |
| [VM oluşturma ve yapılandırma betiği çalıştırma](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir sanal makine oluşturur ve NGıNX yüklemek için Azure Özel Betik uzantısı 'nı kullanır. |
| [WordPress yüklü bir VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir sanal makine oluşturur ve WordPress 'yi yüklemek için Azure Özel Betik uzantısı 'nı kullanır. |
| [Yönetilen işletim sistemi diskinden VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Mevcut bir yönetilen diski işletim sistemi diski olarak ekleyerek bir sanal makine oluşturur. |
| [Anlık görüntüden VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Önce anlık görüntüden yönetilen bir disk oluşturup sonra yeni yönetilen diski işletim sistemi diski olarak ekleyerek bir anlık görüntüden sanal makine oluşturur. |
|**Depolamayı yönetin**||
| [VHD'den yönetilen disk oluşturma](../scripts/virtual-machines-cli-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Özelleştirilmiş bir VHD 'den işletim sistemi diski olarak ya da veri diski olarak bir veri VHD 'sinden yönetilen bir disk oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir anlık görüntüden yönetilen disk oluşturur. |
| [Yönetilen diski aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen diski aynı veya farklı aboneliğe, ancak üst yönetilen diskle aynı bölgeye kopyalar. 
| [Anlık görüntüyü depolama hesabına VHD olarak dışarı aktarma](../scripts/virtual-machines-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen bir anlık görüntüyü, farklı bölgedeki bir depolama hesabına VHD olarak dışa aktarır. |
| [Yönetilen diskin VHD’sini bir depolama hesabına dışarı aktarma](../scripts/virtual-machines-cli-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen bir diskin temel VHD 'sini farklı bölgedeki bir depolama hesabına dışarı aktarır. |
| [Anlık görüntüyü aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Anlık görüntüyü aynı veya farklı aboneliğe, ancak üst anlık görüntüyle aynı bölgeye kopyalar. |
|**Ağ sanal makineleri**||
| [Sanal makineler arasındaki ağ trafiğinin güvenliğini sağlama](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | İki sanal makine, tüm ilgili kaynakları ve bir iç ve dış ağ güvenlik grupları (NSG) oluşturur. |
|**Sanal makinelerin güvenliğini sağlama**||
| [VM’yi ve veri disklerini şifreleme](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir Azure Key Vault, şifreleme anahtarı ve hizmet sorumlusu oluşturur, ardından bir VM 'yi şifreler. |
|**Sanal makineleri izleme**||
| [Azure İzleyici günlükleri ile VM izleme](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir sanal makine oluşturur, Log Analytics aracısını yükleyip VM 'yi bir Log Analytics çalışma alanına kaydeder.  |
|**Sanal makinelerde sorun giderme**||
| [VM işletim sistemi diski ile ilgili sorun giderme](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | İkinci bir VM 'de bir veri diski olarak bir VM 'den işletim sistemi diskini takar. |
| | |
