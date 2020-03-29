---
title: Yüksek Performanslı Bilgi İşlem - Azure Sanal Makineler | Microsoft Dokümanlar
description: Azure'da Yüksek Performanslı Bilgi İşlem hakkında bilgi edinin.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: 10549abfbdacf1fc1ae6b99f4cab20a290c32a2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67707819"
---
# <a name="optimization-for-linux"></a>Linux için iyileştirme

Bu makalede, işletim sistemi görüntü optimize etmek için birkaç temel teknikler gösterir. [InfiniBand'ı etkinleştirme](enable-infiniband.md) ve işletim sistemi görüntülerini optimize etme hakkında daha fazla bilgi edinin.

## <a name="update-lis"></a>LIS'i güncelleştir

Özel bir görüntü kullanarak dağıtıyorsanız (örneğin, CentOS/RHEL 7.4 veya 7.5 gibi eski bir işletim sistemi), VM'deki LIS'i güncelleyin.

```bash
wget https://aka.ms/lis
tar xzf lis
pushd LISISO
./upgrade.sh
```

## <a name="reclaim-memory"></a>Belleği geri alma

Uzaktan bellek erişimini önlemek için belleği otomatik olarak geri alarak verimliliği artırın.

```bash
echo 1 >/proc/sys/vm/zone_reclaim_mode
```

VM yeniden başlatmadan sonra bu devam etmek için:

```bash
echo "vm.zone_reclaim_mode = 1" >> /etc/sysctl.conf sysctl -p
```

## <a name="disable-firewall-and-selinux"></a>Güvenlik duvarını ve SELinux' u devre düşün

Güvenlik duvarLarını ve SELinux'u devre dışı kalın.

```bash
systemctl stop iptables.service
systemctl disable iptables.service
systemctl mask firewalld
systemctl stop firewalld.service
systemctl disable firewalld.service
iptables -nL
sed -i -e's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## <a name="disable-cpupower"></a>Cpupower'ı devre dışı

Cpupower devre dışı bırakır.

```bash
service cpupower status
if enabled, disable it:
service cpupower stop
sudo systemctl disable cpupower
```

## <a name="next-steps"></a>Sonraki adımlar

* [InfiniBand'ı etkinleştirme](enable-infiniband.md) ve işletim sistemi görüntülerini optimize etme hakkında daha fazla bilgi edinin.

* Azure'da [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
