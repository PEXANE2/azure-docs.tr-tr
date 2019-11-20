---
title: SR-ıOV-Azure sanal makineleri ile ınifınband 'i etkinleştirme | Microsoft Docs
description: SR-ıOV ile InfiniBand 'i etkinleştirmeyi öğrenin.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 10/17/2019
ms.author: amverma
ms.openlocfilehash: 7f7907482da886d9da17ef1e7844b205f3e4b906
ms.sourcegitcommit: 8e31a82c6da2ee8dafa58ea58ca4a7dd3ceb6132
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74196775"
---
# <a name="enable-infiniband-with-sr-iov"></a>SR-ıOV ile InfiniBand 'yi etkinleştirme

Azure NC, ND ve H serisi VM 'lerin tümü adanmış bir InfiniBand ağı tarafından desteklenir. RDMA etkin olan tüm boyutlar, bu ağı Intel MPı kullanarak kullanabiliyor. Bazı VM dizileri, SR-ıOV aracılığıyla tüm MPı uygulamaları ve RDMA fiilleri desteğini genişletmiştir. RDMA özellikli VM 'Ler, [GPU ile iyileştirilmiş](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu) ve [yüksek performanslı BILGI işlem (HPC)](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc) VM 'lerini içerir.

## <a name="choose-your-installation-path"></a>Yükleme yolunuzu seçin

Başlamak için en basit seçenek, kullanılabilir olduğunda, InfiniBand için önceden yapılandırılmış bir platform görüntüsü kullanmaktır:

- **HPC IaaS VM 'leri** – HPC Için IaaS VM 'leri kullanmaya başlamak için en basit çözüm, zaten InfiniBand ile yapılandırılmış olan [CENTOS-HPC 7,6 VM OS görüntüsünü](https://techcommunity.microsoft.com/t5/Azure-Compute/CentOS-HPC-VM-Image-for-SR-IOV-enabled-Azure-HPC-VMs/ba-p/665557)kullanmaktır. Bu görüntü zaten InfiniBand ile yapılandırılmış olduğundan, el ile yapılandırmanız gerekmez. Uyumlu Windows sürümleri için bkz. [WINDOWS RDMA özellikli örnekler](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances).

- **GPU IaaS VM 'leri** : bir platform görüntüsü, [CENTOS-HPC 7,6 VM OS görüntüsü](https://techcommunity.microsoft.com/t5/Azure-Compute/CentOS-HPC-VM-Image-for-SR-IOV-enabled-Azure-HPC-VMs/ba-p/665557)dışında, GPU için iyileştirilmiş VM 'ler için önceden yapılandırılmış durumda değildir. InfiniBand ile özel bir görüntü yapılandırmak için bkz. [Mellanox OFED 'ı el ile yüklemek](#manually-install-mellanox-ofed).

Özel bir VM görüntüsü veya [GPU ile iyileştirilmiş](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu) bir VM kullanıyorsanız, dağıtımınıza ıbanddriverlinux veya ınfinibanddriverwindows VM uzantısını ekleyerek bunu InfiniBand ile yapılandırmanız gerekir. Bu VM uzantılarını [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc#rdma-capable-instances) ve [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances)ile nasıl kullanacağınızı öğrenin.

## <a name="manually-install-mellanox-ofed"></a>Mellanox 'nin el ile yüklenmesi

InfiniBand 'yi SR-ıOV ile el ile yapılandırmak için aşağıdaki adımları kullanın. Bu adımlarda örnek, RHEL/CentOS için sözdizimi gösterir, ancak adımlar genel olur ve Ubuntu (16,04, 18,04 19,04) ve SLES (12 SP4 ve 15) gibi uyumlu bir işletim sistemi için kullanılabilir. Gelen kutusu sürücüleri de çalışır, ancak Mellanox Opendokuların sürücüleri daha fazla özellik sağlar.

Mellanox sürücüsü için desteklenen dağıtımlar hakkında daha fazla bilgi için bkz. en son [Mellanox Openyapılar sürücüleri](https://www.mellanox.com/page/products_dyn?product_family=26). Mellanox Openyapılar sürücüsü hakkında daha fazla bilgi için bkz. [Mellanox Kullanıcı Kılavuzu](https://docs.mellanox.com/category/mlnxofedib).

Linux üzerinde InfiniBand yapılandırma için aşağıdaki örneğe bakın:

```bash
# Modify the variable to desired Mellanox OFED version
MOFED_VERSION=#4.7-1.0.0.1
# Modify the variable to desired OS
MOFED_OS=#rhel7.6
pushd /tmp
curl -fSsL https://www.mellanox.com/downloads/ofed/MLNX_OFED-${MOFED_VERSION}/MLNX_OFED_LINUX-${MOFED_VERSION}-${MOFED_OS}-x86_64.tgz | tar -zxpf -
cd MLNX_OFED_LINUX-*
sudo ./mlnxofedinstall
popd
```

Windows için, [Windows sürücüleri Için Mellanox OFED](https://www.mellanox.com/page/products_dyn?product_family=32&menu_section=34)' i indirip yükleyin.

## <a name="enable-ip-over-infiniband"></a>InfiniBand üzerinde IP 'yi etkinleştir

InfiniBand üzerinde IP 'yi etkinleştirmek için aşağıdaki komutları kullanın.

```bash
sudo sed -i -e 's/# OS.EnableRDMA=y/OS.EnableRDMA=y/g' /etc/waagent.conf
sudo systemctl restart waagent
```

## <a name="next-steps"></a>Sonraki adımlar

Azure 'da [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
