---
title: InfiniBand etkin H serisi ve N serisi Azure sanal makinelerinin yapılandırması ve Iyileştirmesi
description: HPC için InfiniBand etkin H serisi ve N serisi VM 'Ler yapılandırmak ve iyileştirmek hakkında bilgi edinin.
author: vermagit
ms.service: virtual-machines
ms.subservice: workloads
ms.topic: article
ms.date: 10/23/2020
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: b1686b08608e4f1c49cd918e86e8149f0fe2a21c
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94963377"
---
# <a name="configure-and-optimize-vms"></a>VM’leri yapılandırma ve iyileştirme

Bu makalede, HPC için InfiniBand özellikli [H serisi](../../sizes-hpc.md) ve [N serisi](../../sizes-gpu.md) VM 'leri yapılandırmak ve iyileştirmek üzere bilinen teknikler paylaşır.

## <a name="vm-images"></a>VM görüntüleri
InfiniBand etkinleştirilmiş VM 'lerde, RDMA 'yı etkinleştirmek için uygun sürücüler gereklidir. Linux 'ta, marketteki CentOS-HPC sanal makine görüntüleri, uygun sürücülerle önceden yapılandırılmış olarak gelir. Ubuntu VM görüntüleri, [Buradaki yönergeler](https://techcommunity.microsoft.com/t5/azure-compute/configuring-infiniband-for-ubuntu-hpc-and-gpu-vms/ba-p/1221351)kullanılarak doğru sürücülerle yapılandırılabilir. Ayrıca, uygun sürücü ve yapılandırmayla [özel VM görüntüleri](../../linux/tutorial-custom-images.md) oluşturmanız ve bu yinelenen öğeyi yeniden kullanmanız önerilir.

> [!NOTE]
> GPU etkin [N serisi](../../sizes-gpu.md) VM 'lerde, uygun GPU sürücüleri [VM uzantıları](../../extensions/hpccompute-gpu-linux.md) veya [el ile](../../linux/n-series-driver-setup.md)eklenebilecek şekilde de gereklidir. Market 'teki bazı VM görüntüleri de NVIDIA GPU sürücüleriyle önceden yüklenmiş olarak gelir.

### <a name="centos-hpc-vm-images"></a>CentOS-HPC VM görüntüleri

#### <a name="non-sr-iov-enabled-vms"></a>SR-ıOV etkin olmayan VM 'Ler
SR-ıOV olmayan [RDMA özellikli VM 'ler](../../sizes-hpc.md#rdma-capable-instances)için, CENTOS-HPC sürüm 6,5 veya sonraki bir sürümü, market 'te 7,5 ' e kadar uygundur. Örneğin, [H16 serisi VM 'ler](../../h-series.md)için 7,1 sürümleri 7,5 ' ye önerilir. Bu VM görüntüleri, RDMA ve Intel MPı sürüm 5,1 için ağ doğrudan sürücüleriyle önceden yüklenmiş olarak gelir.

> [!NOTE]
> SR-ıOV etkin olmayan VM 'Ler için bu CentOS tabanlı HPC görüntülerinde, **VUM** yapılandırma dosyasında çekirdek güncelleştirmeleri devre dışı bırakılır. Bunun nedeni, NetworkDirect Linux RDMA sürücülerinin bir RPM paketi olarak dağıtılmaktadır ve çekirdek güncelleştirilirse sürücü güncelleştirmeleri çalışmayabilir.

#### <a name="sr-iov-enabled-vms"></a>SR-ıOV özellikli VM 'Ler
  SR-ıOV özellikli [RDMA özellikli VM 'ler](../../sizes-hpc.md#rdma-capable-instances)için, [CENTOS-HPC sürüm 7,6 veya Market 'teki sonrakı bir](https://techcommunity.microsoft.com/t5/Azure-Compute/CentOS-HPC-VM-Image-for-SR-IOV-enabled-Azure-HPC-VMs/ba-p/665557) sürüm VM görüntüleri uygundur. Bu VM görüntüleri, RDMA ve çeşitli yaygın olarak kullanılan MPı kitaplıkları ve bilimsel bilgi işlem paketleri ve başlamak için en kolay yol ile en iyi duruma getirilmiş ve önceden yüklenmiş olarak gelir.

  Bir taban CentOS Market görüntüsünden CentOS-HPC sürüm 7,6 ve üzeri VM görüntülerinin oluşturulmasında kullanılan betiklerin örneği [azhpc-Images](https://github.com/Azure/azhpc-images/tree/master/centos)depolarından oluşur.
  
  > [!NOTE] 
  > En son Azure HPC Market görüntülerinin, ConnectX3-Pro InfiniBand kartlarını desteklemeyen, Mellanox OFED 5,1 ve üzeri bir sürüm vardır. SR-ıOV etkinleştirilmiş N serisi VM boyutları, FDR InfiniBand (ör. NCv3) ile aşağıdaki CentOS-HPC VM görüntü sürümlerini veya daha eski bir sürümü kullanabilir:
  >- OpenLogic: CentOS-HPC: 7.6:7.6.2020062900
  >- OpenLogic: CentOS-HPC: 7_6gen2:7.6.2020062901
  >- OpenLogic: CentOS-HPC: 7.7:7.7.2020062600
  >- OpenLogic: CentOS-HPC: 7_7-Gen2:7.7.2020062601
  >- OpenLogic: CentOS-HPC: 8_1:8.1.2020062400
  >- OpenLogic: CentOS-HPC: 8_1-Gen2:8.1.2020062401


### <a name="rhelcentos-vm-images"></a>RHEL/CentOS VM görüntüleri
Market 'teki RHEL veya CentOS tabanlı, HPC olmayan VM görüntüleri, SR-ıOV özellikli [RDMA özellikli VM](../../sizes-hpc.md#rdma-capable-instances)'lerde kullanılmak üzere yapılandırılabilir. [InfiniBand 'yi etkinleştirme](enable-infiniband.md) ve VM 'lerde [MPI ayarlama](setup-mpi.md) hakkında daha fazla bilgi edinin.

  Bir taban CentOS Market görüntüsünden CentOS-HPC sürüm 7,6 ve üzeri VM görüntülerinin oluşturulmasında kullanılan betiklerin örneği [azhpc-Images](https://github.com/Azure/azhpc-images/tree/master/centos)depolarından oluşur.
  
  > [!NOTE]
  > Mellanox OFED 5,1 ve üzeri, FDR InfiniBand (ör. NCv3) ile SR-ıOV etkinleştirilmiş N serisi VM boyutlarında ConnectX3-Pro InfiniBand kartlarını desteklemez. Lütfen ConnectX3-Pro kartlarla birlikte N serisi VM 'de LTS Mellanox OFED Version 4.9-0.1.7.0 veya daha eski bir sürümü kullanın. Lütfen daha [fazla ayrıntı görüntüleyin](https://www.mellanox.com/products/infiniband-drivers/linux/mlnx_ofed).

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
