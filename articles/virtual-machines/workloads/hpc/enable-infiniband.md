---
title: SR-IOV ile InifinBand'i etkinleştirin - Azure Sanal Makineler | Microsoft Dokümanlar
description: SR-IOV ile InfiniBand'ı nasıl etkinleştireceğimiz öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74196775"
---
# <a name="enable-infiniband-with-sr-iov"></a>SR-IOV ile InfiniBand'ı etkinleştirin

Azure NC, ND ve H serisi VM'lerin tümü özel bir InfiniBand ağı tarafından desteklenen bir ağdır. RDMA özellikli tüm boyutlar, Intel MPI kullanarak bu ağdan yararlanabiliyor. Bazı VM serileri, SR-IOV aracılığıyla tüm MPI uygulamaları ve RDMA fiilleri için desteği genişletti. RDMA özellikli VM'ler Arasında [GPU optimize edilmiş](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu) ve [Yüksek performanslı bilgisayar (HPC)](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc) VM'ler yer almaktadır.

## <a name="choose-your-installation-path"></a>Yükleme yolunuzu seçin

Başlamak için en basit seçenek, mevcut olduğunda InfiniBand için önceden yapılandırılmış bir platform görüntüsü kullanmaktır:

- **HPC IaaS VM'ler** – HPC için IaaS VM'ler ile başlamak için en basit çözüm, zaten InfiniBand ile yapılandırılan [CentOS-HPC 7.6 VM OS görüntüsünü](https://techcommunity.microsoft.com/t5/Azure-Compute/CentOS-HPC-VM-Image-for-SR-IOV-enabled-Azure-HPC-VMs/ba-p/665557)kullanmaktır. Bu görüntü zaten InfiniBand ile yapılandırıldığından, el ile yapılandırmanız gerekmez. Uyumlu Windows sürümleri için [Windows RDMA özellikli örneklere](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances)bakın.

- **GPU IaaS VM'ler** – [CentOS-HPC 7.6 VM OS görüntüsü](https://techcommunity.microsoft.com/t5/Azure-Compute/CentOS-HPC-VM-Image-for-SR-IOV-enabled-Azure-HPC-VMs/ba-p/665557)dışında, şu anda GPU optimize edilmiş VM'ler için önceden yapılandırılmış platform görüntüleri bulunmamaktadır. InfiniBand ile özel bir görüntü yapılandırmak için [mellanox OFED'i Manuel olarak yükleyin.](#manually-install-mellanox-ofed)

Özel bir VM görüntüsü veya [GPU optimize edilmiş](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu) bir VM kullanıyorsanız, dağıtımınıza InfiniBandDriverLinux veya InfiniBandDriverWindows VM uzantısı ekleyerek infiniBandIle ile yapılandırmanız gerekir. [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc#rdma-capable-instances) ve [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances)ile bu VM uzantılarını nasıl kullanacağınızı öğrenin.

## <a name="manually-install-mellanox-ofed"></a>Mellanox OFED'i el ile kurun

InfiniBand'i SR-IOV ile el ile yapılandırmak için aşağıdaki adımları kullanın. Bu adımlardaki örnek, RHEL/CentOS için sözdizimini gösterir, ancak adımlar geneldir ve Ubuntu (16.04, 18.04 19.04) ve SLES (12 SP4 ve 15) gibi uyumlu işletim sistemleri için kullanılabilir. Gelen kutusu sürücüleri de çalışır, ancak Mellanox OpenFabrics sürücüleri daha fazla özellik sağlar.

Mellanox sürücüsü için desteklenen dağıtımlar hakkında daha fazla bilgi için, en son [Mellanox OpenFabrics sürücüleri](https://www.mellanox.com/page/products_dyn?product_family=26)bakın. Mellanox OpenFabrics sürücüsü hakkında daha fazla bilgi için [Mellanox kullanım kılavuzuna](https://docs.mellanox.com/category/mlnxofedib)bakın.

Linux'ta InfiniBand'ı nasıl yapılandırılabilenler için aşağıdaki örneğe bakın:

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

Windows için, Windows [sürücüleri için Mellanox OFED'i](https://www.mellanox.com/page/products_dyn?product_family=32&menu_section=34)indirin ve yükleyin.

## <a name="enable-ip-over-infiniband"></a>InfiniBand üzerinden IP'yi etkinleştirin

InfiniBand üzerinden IP'yi etkinleştirmek için aşağıdaki komutları kullanın.

```bash
sudo sed -i -e 's/# OS.EnableRDMA=y/OS.EnableRDMA=y/g' /etc/waagent.conf
sudo systemctl restart waagent
```

## <a name="next-steps"></a>Sonraki adımlar

Azure'da [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
