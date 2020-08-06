---
title: InfiniBand etkin H serisi ve N serisi Azure sanal makinelerinin yapılandırması ve Iyileştirmesi
description: HPC için InfiniBand etkin H serisi ve N serisi VM 'Ler yapılandırmak ve iyileştirmek hakkında bilgi edinin.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 08/01/2020
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: dfa1c790dc0f2e229b3bfa19616e5760c3d3d02e
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87825149"
---
# <a name="configure-and-optimize-vms"></a>VM’leri yapılandırma ve iyileştirme

Bu makalede, HPC için InfiniBand özellikli [H serisi](../../sizes-hpc.md) ve [N serisi](../../sizes-gpu.md) VM 'leri yapılandırmak ve iyileştirmek üzere bilinen teknikler paylaşır.

## <a name="vm-images"></a>VM görüntüleri
InfiniBand etkinleştirilmiş VM 'lerde, RDMA 'yı etkinleştirmek için uygun sürücüler gereklidir. Linux 'ta, marketteki CentOS-HPC sanal makine görüntüleri, uygun sürücülerle önceden yapılandırılmış olarak gelir. Ubuntu VM görüntüleri, [Buradaki yönergeler](https://techcommunity.microsoft.com/t5/azure-compute/configuring-infiniband-for-ubuntu-hpc-and-gpu-vms/ba-p/1221351)kullanılarak doğru sürücülerle yapılandırılabilir. Ayrıca, uygun sürücü ve yapılandırmayla [özel VM görüntüleri](../../linux/tutorial-custom-images.md) oluşturmanız ve bu yinelenen öğeyi yeniden kullanmanız önerilir.

### <a name="centos-hpc-vm-images"></a>CentOS-HPC VM görüntüleri
SR-ıOV olmayan [RDMA özellikli VM 'ler](../../sizes-hpc.md#rdma-capable-instances)için, CENTOS-HPC sürüm 6,5 veya sonraki bir sürümü, market 'te 7,5 ' e kadar uygundur. Örneğin, [H16 serisi VM 'ler](../../h-series.md)için 7,1 sürümleri 7,5 ' ye önerilir. Bu VM görüntüleri, RDMA ve Intel MPı sürüm 5,1 için ağ doğrudan sürücüleriyle önceden yüklenmiş olarak gelir.

  SR-ıOV özellikli [RDMA özellikli VM 'ler](../../sizes-hpc.md#rdma-capable-instances)için, [CENTOS-HPC sürüm 7,6 veya Market 'teki sonrakı bir](https://techcommunity.microsoft.com/t5/Azure-Compute/CentOS-HPC-VM-Image-for-SR-IOV-enabled-Azure-HPC-VMs/ba-p/665557) sürüm VM görüntüleri uygundur. Bu VM görüntüleri, RDMA ve çeşitli yaygın olarak kullanılan MPı kitaplıkları ve bilimsel bilgi işlem paketleri ve başlamak için en kolay yol ile en iyi duruma getirilmiş ve önceden yüklenmiş olarak gelir.

  Bir taban CentOS Market görüntüsünden CentOS-HPC sürüm 7,6 ve üzeri VM görüntülerinin oluşturulmasında kullanılan betiklerin örneği [azhpc-Images](https://github.com/Azure/azhpc-images/tree/master/centos)depolarından oluşur.

### <a name="rhelcentos-vm-images"></a>RHEL/CentOS VM görüntüleri
Market 'teki RHEL veya CentOS tabanlı, HPC olmayan VM görüntüleri, SR-ıOV özellikli [RDMA özellikli VM](../../sizes-hpc.md#rdma-capable-instances)'lerde kullanılmak üzere yapılandırılabilir. [InfiniBand 'yi etkinleştirme](enable-infiniband.md) ve VM 'lerde [MPI ayarlama](setup-mpi.md) hakkında daha fazla bilgi edinin.

  Bir taban CentOS Market görüntüsünden CentOS-HPC sürüm 7,6 ve üzeri VM görüntülerinin oluşturulmasında kullanılan betiklerin örneği [azhpc-Images](https://github.com/Azure/azhpc-images/tree/master/centos)depolarından oluşur.

### <a name="ubuntu-vm-images"></a>Ubuntu VM görüntüleri
Market 'teki Ubuntu Server 16,04 LTS, 18,04 LTS ve 20,04 LTS VM görüntüleri hem SR-ıOV hem de SR-ıOV olmayan [RDMA özellikli VM 'ler](../../sizes-hpc.md#rdma-capable-instances)için desteklenir. [InfiniBand 'yi etkinleştirme](enable-infiniband.md) ve VM 'lerde [MPI ayarlama](setup-mpi.md) hakkında daha fazla bilgi edinin.

  Ubuntu 18,04 LTS tabanlı HPC VM görüntülerinin oluşturulmasında kullanılabilen betikler örneği [azhpc-Images](https://github.com/Azure/azhpc-images/tree/master/ubuntu/ubuntu-18.x/ubuntu-18.04-hpc)depodayılır.

### <a name="suse-linux-enterprise-server-vm-images"></a>VM görüntülerini SUSE Linux Enterprise Server
HPC için SLES 12 SP3, HPC için SLES 12 SP3 (Premium), HPC için SLES 12 SP1, HPC için SLES 12 SP1 (Premium), marketteki SLES 12 SP4 ve SLES 15 VM görüntüleri desteklenir. Bu VM görüntüleri, RDMA ve Intel MPı sürüm 5,1 için ağ doğrudan sürücüleriyle önceden yüklenmiş olarak gelir. VM 'lerde [MPI ayarlama](setup-mpi.md) hakkında daha fazla bilgi edinin.

## <a name="optimize-vms"></a>VM 'Leri iyileştirme

Aşağıda, sanal makinede iyileştirilmiş performans için bazı isteğe bağlı iyileştirme ayarları verilmiştir.

### <a name="update-lis"></a>LIS 'yi Güncelleştir

İşlevsellik veya performans için gerekliyse, [Linux Integration Services (LIS) sürücüleri](../../linux/endorsed-distros.md) desteklenen işletim sistemi distroları üzerinde yüklenebilir veya güncelleştirilemeyebilir, özellikle de özel bir görüntü veya CentOS/RHEL 6. x veya daha önceki 7. x sürümü gibi eski bir işletim sistemi sürümü kullanılarak dağıtımı yapılabilir.

```bash
wget https://aka.ms/lis
tar xzf lis
pushd LISISO
./upgrade.sh
```

### <a name="reclaim-memory"></a>Belleği geri kazanma

Uzak bellek erişimini önlemek için otomatik olarak geri kazanma belleği aracılığıyla performansı artırabilirsiniz.

```bash
echo 1 >/proc/sys/vm/zone_reclaim_mode
```

VM yeniden başlatıldıktan sonra bunu kalıcı hale getirmek için:

```bash
echo "vm.zone_reclaim_mode = 1" >> /etc/sysctl.conf sysctl -p
```

### <a name="disable-firewall-and-selinux"></a>Güvenlik duvarını ve SELinux 'u devre dışı bırakma

```bash
systemctl stop iptables.service
systemctl disable iptables.service
systemctl mask firewalld
systemctl stop firewalld.service
systemctl disable firewalld.service
iptables -nL
sed -i -e's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

### <a name="disable-cpupower"></a>Cpugücünü devre dışı bırak

```bash
service cpupower status
if enabled, disable it:
service cpupower stop
sudo systemctl disable cpupower
```

### <a name="configure-walinuxagent"></a>Walınuxagent 'ı yapılandırma

```bash
sed -i -e 's/# OS.EnableRDMA=y/OS.EnableRDMA=y/g' /etc/waagent.conf
```
İsteğe bağlı olarak, Walınuxagent, HPC iş yüküne maksimum VM kaynak kullanılabilirliği için iş öncesi adım ve iş sonrası için devre dışı bırakılmış olabilir.


## <a name="next-steps"></a>Sonraki adımlar

- InfiniBand özellikli [H serisi](../../sizes-hpc.md) ve [N serisi](../../sizes-gpu.md) VM 'lerde [InfiniBand 'yi etkinleştirme](enable-infiniband.md) hakkında daha fazla bilgi edinin.
- [Desteklenen çeşitli MPI kitaplıklarını](setup-mpi.md) ve En Iyi yapılandırmalarını VM 'lere yükleme hakkında daha fazla bilgi edinin.
- Performans ve ölçeklenebilirlik için iş yüklerini en iyi şekilde yapılandırma hakkında bilgi edinmek için [HB Serisi genel bakış](hb-series-overview.md) ve [HC Serisi genel bakışı](hc-series-overview.md) gözden geçirin.
- En son duyurular ve bazı HPC örnekleri hakkında bilgi edinin ve [Azure Işlem teknik topluluk bloglarında](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute)bu sonuçları elde edin.
- Çalıştırılan HPC iş yüklerinin daha yüksek düzey mimari görünümü için bkz. [Azure 'Da yüksek performanslı bilgi işlem (HPC)](/azure/architecture/topics/high-performance-computing/).