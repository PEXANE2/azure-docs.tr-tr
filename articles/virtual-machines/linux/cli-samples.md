---
title: Azure CLI Örnekleri
description: Azure CLI Örnekleri
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 03/01/2019
ms.author: cynthn
ms.openlocfilehash: 76803f8c3a703071eb733c5cfde65482ffd07f40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970089"
---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>Linux sanal makineleri için Azure CLI Örnekleri

Aşağıdaki tablo, Azure CLI’si kullanılarak oluşturulan bash komut dosyalarına yönelik bağlantılar içerir.

| | |
|---|---|
|**Sanal makineler oluşturun**||
| [Sanal bir makine oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | En az yapılandırmaya sahip bir Linux sanal makinesi oluşturur. |
| [Tam olarak yapılandırılmış bir sanal makine oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir kaynak grubu, sanal makine ve ilgili tüm kaynaklar oluşturur.|
| [Yüksek kullanılabilir sanal makineler oluşturun](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yüksek kullanılabilir ve yük dengeli yapılandırmabirkaç sanal makineler oluşturur. |
| [VM oluşturma ve yapılandırma komut dosyası çalıştırma](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Sanal bir makine oluşturur ve NGINX'i yüklemek için Azure Özel Komut Dosyası uzantısını kullanır. |
| [WordPress yüklü bir VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Sanal bir makine oluşturur ve WordPress'i yüklemek için Azure Özel Komut Dosyası uzantısını kullanır. |
| [Yönetilen bir işletim sistemi diskinden VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Varolan yönetilen bir diski işletim sistemi diski olarak ekleyerek sanal bir makine oluşturur. |
| [Anlık görüntüden VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Anlık görüntüden sanal bir makine oluşturur, önce anlık görüntüden yönetilen bir disk oluşturur ve ardından yeni yönetilen diski işletim sistemi diski olarak bağlar. |
|**Depolamayı yönetin**||
| [VHD'den yönetilen disk oluşturma](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | İşletim sistemi diski olarak özel leştirilmiş bir VHD'den veya veri diski olarak veri VHD'sinden yönetilen bir disk oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Anlık görüntüden yönetilen bir disk oluşturur. |
| [Yönetilen diski aynı veya farklı aboneye kopyalama](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Kopyalar, diski aynı veya farklı aboneliği yöneten ancak üst yönetilen diskle aynı bölgede. 
| [Anlık görüntüyü depolama hesabına VHD olarak dışarı aktarma](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen anlık görüntü'ü VHD olarak farklı bölgedeki bir depolama hesabına dışa aktarım. |
| [Yönetilen diskin VHD’sini bir depolama hesabına dışarı aktarma](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen bir diskin temel VHD'sini farklı bölgedeki bir depolama hesabına dışa aktarım. |
| [Anlık görüntüyle aynı veya farklı abonelmeye kopyala](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Anlık görüntüleri aynı veya farklı abonelik için kopyalar, ancak üst anlık görüntüyle aynı bölgede. |
|**Ağ sanal makineleri**||
| [Sanal makineler arasındaki ağ trafiğinin güvenliğini sağlama](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | İki sanal makine, tüm ilgili kaynaklar ve bir iç ve dış ağ güvenlik grupları (NSG) oluşturur. |
|**Güvenli sanal makineler**||
| [VM’yi ve veri disklerini şifreleme](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Azure Key Vault, şifreleme anahtarı ve servis sorumlusu oluşturur, ardından vm'yi şifreler. |
|**Sanal makineleri izleme**||
| [Azure Monitör günlükleriyle bir VM izleme](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Sanal bir makine oluşturur, Log Analytics aracısını yükler ve VM'yi bir Log Analytics çalışma alanına kaydeder.  |
|**Sorun giderme sanal makineleri**||
| [VM işletim sistemi diski ile ilgili sorun giderme](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | İşletim sistemi diskini bir VM'den ikinci bir VM'ye veri diski olarak bağlar. |
| | |
