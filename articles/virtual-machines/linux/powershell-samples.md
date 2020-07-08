---
title: Azure sanal makine PowerShell örnekleri
description: Azure sanal makine PowerShell örnekleri
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.openlocfilehash: 8d7db5fe88890b7f807263e50757e637ad808eb1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81759313"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Azure sanal makine PowerShell örnekleri

Aşağıdaki tabloda, Linux sanal makineleri oluşturup yöneten PowerShell betikleri örneklerinin bağlantıları yer almaktadır.

| | |
|---|---|
|**Sanal makineler oluşturma**||
| [Tam olarak yapılandırılmış bir sanal makine oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturur.|
| [Docker etkin olarak bir VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-docker-host.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir sanal makine oluşturur, bu VM 'yi bir Docker Konağı olarak yapılandırır ve NGıNX kapsayıcısını çalıştırır. |
| [VM oluşturma ve yapılandırma betiği çalıştırma](./../scripts/virtual-machines-linux-powershell-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir sanal makine oluşturur ve NGıNX yüklemek için Azure Özel Betik uzantısı 'nı kullanır. |
| [WordPress yüklü bir VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir sanal makine oluşturur ve WordPress 'yi yüklemek için Azure Özel Betik uzantısı 'nı kullanır. |
| [Yönetilen işletim sistemi diskinden VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Mevcut bir yönetilen diski işletim sistemi diski olarak ekleyerek bir sanal makine oluşturur. |
| [Anlık görüntüden VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Önce anlık görüntüden yönetilen bir disk oluşturup sonra yeni yönetilen diski işletim sistemi diski olarak ekleyerek bir anlık görüntüden sanal makine oluşturur. |
|**Depolamayı yönetin**||
| [Aynı veya farklı bir abonelikte bulunan bir VHD'den yönetilen disk oluşturma](../scripts/virtual-machines-linux-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Özelleştirilmiş bir VHD 'den işletim sistemi diski olarak veya bir veri VHD 'sinden, aynı veya farklı bir abonelikte bir veri diski olarak yönetilen bir disk oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-linux-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir anlık görüntüden yönetilen disk oluşturur. |
| [Anlık görüntüyü bir depolama hesabına VHD olarak dışarı aktarma](../scripts/virtual-machines-linux-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen bir anlık görüntüyü, farklı bir bölgedeki depolama hesabına VHD olarak dışa aktarır. |
| [Yönetilen diskin VHD’sini bir depolama hesabına dışarı aktarma](../scripts/virtual-machines-linux-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen bir diskin temel VHD 'sini farklı bir bölgedeki depolama hesabına dışarı aktarır. |
| [VHD’den anlık görüntü oluşturma](../scripts/virtual-machines-linux-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir VHD 'den anlık görüntü oluşturur ve daha sonra bu anlık görüntüyü kullanarak birden fazla özdeş yönetilen disk oluşturur.  |
| [Anlık görüntüyü aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-linux-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Anlık görüntüyü, üst anlık görüntüyle aynı bölgedeki aynı veya farklı bir aboneliğe kopyalar. |
|**Sanal makineleri izleme**||
| [Azure İzleyici günlükleri ile VM izleme](./../scripts/virtual-machines-linux-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir sanal makine oluşturur, Log Analytics aracısını yükleyip VM 'yi bir Log Analytics çalışma alanına kaydeder.  |
| [Yönetilen diski aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-linux-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen bir diski, ana yönetilen diskle aynı bölgedeki aynı veya farklı bir aboneliğe kopyalar.
| [PowerShell ile bir abonelikteki tüm VM’lerle ilgili ayrıntıları toplama](../scripts/virtual-machines-powershell-sample-collect-vm-details.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Belirtilen abonelikte VM adı, kaynak grubu adı, bölge, sanal ağ, alt ağ, özel IP adresi, işletim sistemi türü ve VM 'lerin genel IP adresini içeren bir CSV oluşturur.
| | |
