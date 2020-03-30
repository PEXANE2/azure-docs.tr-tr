---
title: Azure Sanal Makine PowerShell Örnekleri
description: Azure Sanal Makine PowerShell Örnekleri
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.openlocfilehash: 903860dfd1e30f7941770efd759227a1349d41ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74035243"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Azure Virtual Machine PowerShell örnekleri

Aşağıdaki tablo, Linux sanal makineleri oluşturan ve yöneten PowerShell komut dosyası örneklerine bağlantılar içerir.

| | |
|---|---|
|**Sanal makineler oluşturun**||
| [Tam olarak yapılandırılmış bir sanal makine oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir kaynak grubu, sanal makine ve ilgili tüm kaynaklar oluşturur.|
| [Docker etkinken bir VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-docker-host.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Sanal bir makine oluşturur, bu VM'yi Docker ana bilgisayar olarak yapılandırır ve bir NGINX kapsayıcısı çalıştırır. |
| [VM oluşturma ve yapılandırma komut dosyası çalıştırma](./../scripts/virtual-machines-linux-powershell-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Sanal bir makine oluşturur ve NGINX'i yüklemek için Azure Özel Komut Dosyası uzantısını kullanır. |
| [WordPress yüklü bir VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Sanal bir makine oluşturur ve WordPress'i yüklemek için Azure Özel Komut Dosyası uzantısını kullanır. |
| [Yönetilen bir işletim sistemi diskinden VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Varolan yönetilen bir diski işletim sistemi diski olarak ekleyerek sanal bir makine oluşturur. |
| [Anlık görüntüden VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Anlık görüntüden önce anlık görüntüden yönetilen bir disk oluşturup ardından yeni yönetilen diski işletim sistemi diski olarak ekleyerek sanal bir makine oluşturur. |
|**Depolamayı yönetin**||
| [Aynı veya farklı bir abonelikte bir VHD'den yönetilen bir disk oluşturma](../scripts/virtual-machines-linux-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | İşletim sistemi diski olarak özel leştirilmiş bir VHD'den veya veri diski olarak veri VHD'sinden aynı veya farklı bir abonelikte yönetilen bir disk oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-linux-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Anlık görüntüden yönetilen bir disk oluşturur. |
| [Anlık görüntüyü bir depolama hesabına VHD olarak dışarı aktarma](../scripts/virtual-machines-linux-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen anlık görüntü'ü VHD olarak farklı bir bölgedeki depolama hesabına dışa aktarım. |
| [Yönetilen diskin VHD’sini bir depolama hesabına dışarı aktarma](../scripts/virtual-machines-linux-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen bir diskin temel VHD'sini farklı bir bölgedeki depolama hesabına dışa aktarım. |
| [VHD’den anlık görüntü oluşturma](../scripts/virtual-machines-linux-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir VHD'den anlık görüntü oluşturur ve ardından bu anlık görüntüden birden çok özdeş yönetilen diski hızla oluşturmak için kullanır.  |
| [Anlık görüntünün aynı veya farklı bir aboneye kopyalanması](../scripts/virtual-machines-linux-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Anlık görüntü, üst anlık görüntüyle aynı bölgede bulunan aynı veya farklı bir aboneliği kopyalar. |
|**Sanal makineleri izleme**||
| [Azure Monitör günlükleriyle bir VM izleme](./../scripts/virtual-machines-linux-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Sanal bir makine oluşturur, Log Analytics aracısını yükler ve VM'yi log analytics çalışma alanına kaydeder.  |
| [Yönetilen bir diski aynı veya farklı bir aboneye kopyalama](../scripts/virtual-machines-linux-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen bir diski, üst yönetilen diskle aynı bölgede bulunan aynı veya farklı bir abonelik için kopyalar.
| [PowerShell ile abonelikteki tüm VM'ler hakkında bilgi toplama](../scripts/virtual-machines-powershell-sample-collect-vm-details.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Sağlanan abonelikte VM Adı, Kaynak Grubu Adı, Bölge, Sanal Ağ, AltAğ, Özel IP Adresi, İşletim Sistemi Türü ve Genel IP Adresi içeren bir csv oluşturur.
| | |
