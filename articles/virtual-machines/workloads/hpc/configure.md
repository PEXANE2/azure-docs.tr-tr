---
title: Yüksek performanslı bilgi Işlem-Azure sanal makineleri | Microsoft Docs
description: Azure 'da yüksek performanslı bilgi Işlem hakkında bilgi edinin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67707819"
---
# <a name="optimization-for-linux"></a>Linux için iyileştirme

Bu makalede, işletim sistemi görüntünüzü iyileştirmek için bazı önemli teknikler gösterilmektedir. [InfiniBand 'yi etkinleştirme](enable-infiniband.md) ve işletim sistemi görüntülerini iyileştirme hakkında daha fazla bilgi edinin.

## <a name="update-lis"></a>LIS 'yi Güncelleştir

Özel bir görüntü (örneğin, CentOS/RHEL 7,4 veya 7,5 gibi eski bir işletim sistemi) ile dağıtım yapıyorsanız, VM 'de LIS 'yi güncelleştirin.

```bash
wget https://aka.ms/lis
tar xzf lis
pushd LISISO
./upgrade.sh
```

## <a name="reclaim-memory"></a>Belleği geri kazanma

Uzak bellek erişimini önlemek için, geri kazanma otomatik olarak belleği artırabilirsiniz.

```bash
echo 1 >/proc/sys/vm/zone_reclaim_mode
```

VM yeniden başlatıldıktan sonra bunu kalıcı hale getirmek için:

```bash
echo "vm.zone_reclaim_mode = 1" >> /etc/sysctl.conf sysctl -p
```

## <a name="disable-firewall-and-selinux"></a>Güvenlik duvarını ve SELinux 'u devre dışı bırakma

Güvenlik duvarını ve SELinux 'u devre dışı bırakın.

```bash
systemctl stop iptables.service
systemctl disable iptables.service
systemctl mask firewalld
systemctl stop firewalld.service
systemctl disable firewalld.service
iptables -nL
sed -i -e's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## <a name="disable-cpupower"></a>Cpugücünü devre dışı bırak

Cpugücünü devre dışı bırakın.

```bash
service cpupower status
if enabled, disable it:
service cpupower stop
sudo systemctl disable cpupower
```

## <a name="next-steps"></a>Sonraki adımlar

* [InfiniBand 'yi etkinleştirme](enable-infiniband.md) ve işletim sistemi görüntülerini iyileştirme hakkında daha fazla bilgi edinin.

* Azure 'da [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
