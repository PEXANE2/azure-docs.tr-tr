---
title: HPC VM 'lerde ınifınband 'i etkinleştirme-Azure sanal makineleri | Microsoft Docs
description: Azure HPC VM 'lerinde InfiniBand 'i etkinleştirmeyi öğrenin.
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
ms.openlocfilehash: 0cbfed307cea1bd98bf864046a8c08edb849226a
ms.sourcegitcommit: fbb66a827e67440b9d05049decfb434257e56d2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87797994"
---
# <a name="enable-infiniband"></a>InfiniBand’i etkinleştirme

[RDMA özellikli](../../sizes-hpc.md#rdma-capable-instances) [H serisi](../../sizes-hpc.md) ve [N serisi](../../sizes-gpu.md) VM 'ler, düşük gecikme süresi ve yüksek bant genişliğine sahip InfiniBand ağı üzerinden iletişim kurar. Bu tür bir bağlantı üzerinden RDMA özelliği, dağıtılmış düğüm HPC ve AI iş yüklerinin ölçeklenebilirliğini ve performansını artırmak için önemlidir. InfiniBand etkin H serisi ve N serisi VM 'Ler, en iyileştirilmiş ve tutarlı RDMA performansı için düşük çapta bir tasarıma sahip engelleyici olmayan bir FAT ağacına bağlanır.

Yetenekli VM boyutlarında InfiniBand 'yi etkinleştirmenin çeşitli yolları vardır.

## <a name="vm-images-with-infiniband-drivers"></a>InfiniBand sürücülerle VM görüntüleri
[Sanal](configure.md#vm-images) MAKINELERDEKI desteklenen VM görüntülerinin listesi için bkz. InfiniBand sürücülerle (SR-ıOV veya SR-IOV olmayan VM 'ler için) önceden yüklenmiş veya uygun sürücülerle yapılandırılabilir.
SR-ıOV özellikli [RDMA özellikli VM 'ler](../../sizes-hpc.md#rdma-capable-instances)için, [CENTOS-HPC sürüm 7,6 veya Market 'teki sonrakı bir](https://techcommunity.microsoft.com/t5/Azure-Compute/CentOS-HPC-VM-Image-for-SR-IOV-enabled-Azure-HPC-VMs/ba-p/665557) sürüm VM görüntüleri, başlamak için en kolay yoldur.
Ubuntu VM görüntüleri, [Buradaki yönergeler](https://techcommunity.microsoft.com/t5/azure-compute/configuring-infiniband-for-ubuntu-hpc-and-gpu-vms/ba-p/1221351)KULLANıLARAK hem SR-IOV hem de SR-IOV olmayan VM 'ler için doğru sürücülerle yapılandırılabilir.

## <a name="infiniband-driver-vm-extensions"></a>InfiniBand sürücü VM uzantıları
Linux 'ta, [ınfinibanddriverlinux sanal makine uzantısı](../../extensions/hpc-compute-infiniband-linux.md) , The SR-IOV etkin H ve N serisi VM 'lerde

Windows 'da, [ınfinibanddriverwindows VM Uzantısı](../../extensions/hpc-compute-infiniband-windows.md) , RDMA bağlantısı Için Windows ağ doğrudan SÜRÜCÜLERINI (SR-IOV olmayan VM 'lerde) veya (SR-IOV VM 'lerinde) A8 ve A9 örneklerinin bazı dağıtımlarında, HpcVmDrivers uzantısı otomatik olarak eklenir. HpcVmDrivers VM uzantısının kullanım dışı olduğunu unutmayın; güncellenmeyecektir.

VM uzantısını bir VM 'ye eklemek için [Azure PowerShell](/powershell/azure/) cmdlet 'lerini kullanabilirsiniz. Daha fazla bilgi için bkz. [sanal makine uzantıları ve özellikleri](../../extensions/overview.md). [Klasik dağıtım modelinde](/previous-versions/azure/virtual-machines/windows/classic/agents-and-extensions-classic)dağıtılan VM 'ler için uzantılara de çalışabilirsiniz.

## <a name="manual-installation"></a>El ile yükleme
[Mellanox Openyapılar sürücüleri (OFED)](https://www.mellanox.com/products/InfiniBand-VPI-Software) , [SR-IOV etkin](../../sizes-hpc.md#rdma-capable-instances) [H serisi](../../sizes-hpc.md) ve [N serisi](../../sizes-gpu.md) VM 'lere el ile yüklenebilir.

### <a name="linux"></a>Linux
[Linux Için alınan sürücüler](https://www.mellanox.com/products/infiniband-drivers/linux/mlnx_ofed) aşağıdaki örnekle yüklenebilir. Buradaki örnek RHEL/CentOS için de olsa da, adımlar genel ve Ubuntu (16,04, 18,04 19,04, 20,04) ve SLES (12 SP4 ve 15) gibi uyumlu bir Linux işletim sistemi için kullanılabilir. Diğer kullanıcılar için daha fazla örnek [azhpc-Images](https://github.com/Azure/azhpc-images/blob/master/ubuntu/ubuntu-18.x/ubuntu-18.04-hpc/install_mellanoxofed.sh)depolarından daha fazla örnektir. Gelen kutusu sürücüleri de çalışır, ancak Mellanox ALıNAN sürücüler daha fazla özellik sağlar.

```bash
MLNX_OFED_DOWNLOAD_URL=http://content.mellanox.com/ofed/MLNX_OFED-5.0-2.1.8.0/MLNX_OFED_LINUX-5.0-2.1.8.0-rhel7.7-x86_64.tgz
# Optionally verify checksum
wget --retry-connrefused --tries=3 --waitretry=5 $MLNX_OFED_DOWNLOAD_URL
tar zxvf MLNX_OFED_LINUX-5.0-2.1.8.0-rhel7.7-x86_64.tgz

KERNEL=( $(rpm -q kernel | sed 's/kernel\-//g') )
KERNEL=${KERNEL[-1]}
# Uncomment the lines below if you are running this on a VM
#RELEASE=( $(cat /etc/centos-release | awk '{print $4}') )
#yum -y install http://olcentgbl.trafficmanager.net/centos/${RELEASE}/updates/x86_64/kernel-devel-${KERNEL}.rpm
yum install -y kernel-devel-${KERNEL}
./MLNX_OFED_LINUX-5.0-2.1.8.0-rhel7.7-x86_64/mlnxofedinstall --kernel $KERNEL --kernel-sources /usr/src/kernels/${KERNEL} --add-kernel-support --skip-repo
```

### <a name="windows"></a>Windows
Windows için, [Windows sürücüleri Için Mellanox OFED](https://www.mellanox.com/products/adapter-software/ethernet/windows/winof-2)' i indirip yükleyin.

## <a name="enable-ip-over-infiniband-ib"></a>InfiniBand üzerinde IP 'yi etkinleştir (ıB)
MPı işlerini çalıştırmayı planlıyorsanız, genellikle bir ıpoıb gerekmez. MPı kitaplığı, ıB iletişimi için fiiller arabirimini kullanır (MPı kitaplığı 'nın TCP/IP kanalını açıkça kullanmadığınız durumlar dışında). Ancak, iletişim için TCP/IP kullanan bir uygulamanız varsa ve ıB üzerinden çalıştırmak istiyorsanız, ıB arabirimi üzerinden bir ıpoıb kullanabilirsiniz. InfiniBand üzerinde IP 'yi etkinleştirmek için aşağıdaki komutları kullanın (RHEL/CentOS için).

```bash
sudo sed -i -e 's/# OS.EnableRDMA=y/OS.EnableRDMA=y/g' /etc/waagent.conf
sudo systemctl restart waagent
```

## <a name="next-steps"></a>Sonraki adımlar

- [Desteklenen çeşitli MPI kitaplıklarını](setup-mpi.md) ve En Iyi yapılandırmalarını VM 'lere yükleme hakkında daha fazla bilgi edinin.
- Performans ve ölçeklenebilirlik için iş yüklerini en iyi şekilde yapılandırma hakkında bilgi edinmek için [HB Serisi genel bakış](hb-series-overview.md) ve [HC Serisi genel bakışı](hc-series-overview.md) gözden geçirin.
- En son duyurular ve bazı HPC örnekleri hakkında bilgi edinin ve [Azure Işlem teknik topluluk bloglarında](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute)bu sonuçları elde edin.
- Çalıştırılan HPC iş yüklerinin daha yüksek düzey mimari görünümü için bkz. [Azure 'Da yüksek performanslı bilgi işlem (HPC)](/azure/architecture/topics/high-performance-computing/).
