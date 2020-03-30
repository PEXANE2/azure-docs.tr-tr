---
title: Azure CLI Örnekleri Windows
description: Azure CLI Örnekleri Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 8d57220f79f1349937a279e57a17d51a76bcccb3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033636"
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Windows sanal makineleri için Azure CLI Örnekleri

Aşağıdaki tablo, Windows sanal makinelerini dağıtan Azure CLI kullanılarak oluşturulmuş bash komut dosyalarına bağlantılar içerir.

| | |
|---|---|
|**Sanal makineler oluşturun**||
| [Sanal bir makine oluşturma](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | En az yapılandırmaya sahip bir Windows sanal makinesi oluşturur. |
| [Tam olarak yapılandırılmış bir sanal makine oluşturma](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir kaynak grubu, sanal makine ve ilgili tüm kaynaklar oluşturur.|
| [Yüksek kullanılabilir sanal makineler oluşturun](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yüksek kullanılabilir ve yük dengeli yapılandırmabirkaç sanal makineler oluşturur. |
| [VM oluşturma ve yapılandırma komut dosyası çalıştırma](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Sanal bir makine oluşturur ve IIS'yi yüklemek için Azure Özel Komut Dosyası uzantısını kullanır. |
| [VM oluşturun ve DSC yapılandırması çalıştırın](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Sanal bir makine oluşturur ve IIS'yi yüklemek için Azure İstenilen Durum Yapılandırması (DSC) uzantısını kullanır. |
|**Depolamayı yönetin**||
| [VHD'den yönetilen disk oluşturma](../scripts/virtual-machines-windows-cli-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | İşletim sistemi diski olarak özel leştirilmiş bir VHD'den veya veri diski olarak veri VHD'sinden yönetilen bir disk oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-windows-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Anlık görüntüden yönetilen bir disk oluşturur. |
| [Yönetilen diski aynı veya farklı aboneye kopyalama](../scripts/virtual-machines-windows-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Kopyalar, diski aynı veya farklı aboneliği yöneten ancak üst yönetilen diskle aynı bölgede. 
| [Anlık görüntüyü depolama hesabına VHD olarak dışarı aktarma](../scripts/virtual-machines-windows-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yönetilen anlık görüntü'ü VHD olarak farklı bölgedeki bir depolama hesabına dışa aktarım. |
| [Yönetilen diskin VHD’sini bir depolama hesabına dışarı aktarma](../scripts/virtual-machines-windows-cli-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yönetilen bir diskin temel VHD'sini farklı bölgedeki bir depolama hesabına dışa aktarım. |
| [Anlık görüntüyle aynı veya farklı abonelmeye kopyala](../scripts/virtual-machines-windows-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Anlık görüntüleri aynı veya farklı abonelik için kopyalar, ancak üst anlık görüntüyle aynı bölgede. |
|**Ağ sanal makineleri**||
| [Sanal makineler arasındaki ağ trafiğinin güvenliğini sağlama](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | İki sanal makine, tüm ilgili kaynaklar ve bir iç ve dış ağ güvenlik grupları (NSG) oluşturur. |
|**Güvenli sanal makineler**||
| [VM’yi ve veri disklerini şifreleme](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Azure Key Vault, şifreleme anahtarı ve servis sorumlusu oluşturur, ardından vm'yi şifreler. |
|**Sanal makineleri izleme**||
| [Azure Monitör ile Bir VM'yi izleme](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Sanal bir makine oluşturur, Log Analytics aracısını yükler ve VM'yi log analytics çalışma alanına kaydeder.  |
| | |
