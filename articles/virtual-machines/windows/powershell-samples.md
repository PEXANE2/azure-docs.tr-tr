---
title: Azure Sanal Makine PowerShell Örnekleri
description: Azure Sanal Makine PowerShell Örnekleri
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
ms.openlocfilehash: f068b79f1b1eaa9a11df70052619c8e3993101cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033007"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Azure Virtual Machine PowerShell örnekleri

Aşağıdaki tablo, Windows sanal makineleri (VM) oluşturan ve yöneten PowerShell komut dosyası örneklerine bağlantılar sağlar.

| | |
|---|---|
|**Sanal makineler oluşturun**||
| [Hızlı bir şekilde sanal bir makine oluşturun](./../scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | En az istemli bir kaynak grubu, sanal makine ve ilgili tüm kaynaklar oluşturur.|
| [Tam olarak yapılandırılmış bir sanal makine oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir kaynak grubu, sanal makine ve ilgili tüm kaynaklar oluşturur.|
| [Yüksek kullanılabilir sanal makineler oluşturun](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yüksek kullanılabilir ve yük dengeli yapılandırmada birkaç sanal makine oluşturur.|
| [VM oluşturma ve yapılandırma komut dosyası çalıştırma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Sanal bir makine oluşturur ve IIS'yi yüklemek için Azure Özel Komut Dosyası uzantısını kullanır. |
| [VM oluşturma ve DSC yapılandırması çalıştırma](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Sanal bir makine oluşturur ve IIS'yi yüklemek için Azure İstenilen Durum Yapılandırması (DSC) uzantısını kullanır. |
| [VHD yükleyin ve VM'ler oluşturun](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Azure'a yerel bir VHD dosyası yükler, VHD'den bir görüntü oluşturur ve bu resimden bir VM oluşturur. |
| [Yönetilen bir işletim sistemi diskinden VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Varolan yönetilen bir diski işletim sistemi diski olarak ekleyerek sanal bir makine oluşturur. |
| [Anlık görüntüden VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Anlık görüntüden önce anlık görüntüden yönetilen bir disk oluşturup ardından yeni yönetilen diski işletim sistemi diski olarak ekleyerek sanal bir makine oluşturur. |
|**Depolamayı yönetin**||
| [Aynı veya farklı bir abonelikte bir VHD'den yönetilen bir disk oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | İşletim sistemi diski olarak özel leştirilmiş bir VHD'den veya veri diski olarak veri VHD'sinden aynı veya farklı bir abonelikte yönetilen bir disk oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Anlık görüntüden yönetilen bir disk oluşturur. |
| [Yönetilen bir diski aynı veya farklı bir aboneye kopyalama](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Yönetilen bir diski, üst yönetilen diskle aynı bölgede bulunan aynı veya farklı bir abonelik için kopyalar.
| [Anlık görüntüyü bir depolama hesabına VHD olarak dışarı aktarma](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yönetilen anlık görüntü'ü VHD olarak farklı bir bölgedeki depolama hesabına dışa aktarım. |
| [Yönetilen diskin VHD’sini bir depolama hesabına dışarı aktarma](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yönetilen bir diskin temel VHD'sini farklı bir bölgedeki depolama hesabına dışa aktarım. |
| [VHD’den anlık görüntü oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir VHD'den anlık görüntü oluşturur ve ardından bu anlık görüntüden birden çok özdeş yönetilen diski hızla oluşturmak için kullanır.  |
| [Anlık görüntünün aynı veya farklı bir aboneye kopyalanması](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Anlık görüntü, üst anlık görüntüyle aynı bölgede bulunan aynı veya farklı bir aboneliği kopyalar. |
|**Güvenli sanal makineler**||
| [VM’yi ve veri disklerini şifreleme](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Azure anahtar kasası, şifreleme anahtarı ve hizmet sorumlusu oluşturur ve ardından bir VM'yi şifreler. |
|**Sanal makineleri izleme**||
| [Azure Monitör ile Bir VM'yi izleme](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Sanal bir makine oluşturur, Azure Log Analytics aracısını yükler ve VM'yi bir Log Analytics çalışma alanına kaydeder.  |
| [PowerShell ile abonelikteki tüm VM'ler hakkında bilgi toplama](../scripts/virtual-machines-powershell-sample-collect-vm-details.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Sağlanan abonelikte VM Adı, Kaynak Grubu Adı, Bölge, Sanal Ağ, AltAğ, Özel IP Adresi, İşletim Sistemi Türü ve Genel IP Adresi içeren bir csv oluşturur.
| | |
