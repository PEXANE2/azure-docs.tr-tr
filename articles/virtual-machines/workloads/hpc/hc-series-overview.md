---
title: HC Serisi VM 'ye genel bakış-Azure sanal makineleri | Microsoft Docs
description: Azure 'da HC Serisi VM boyutu için Önizleme desteği hakkında bilgi edinin.
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
ms.openlocfilehash: a4cd74c9c85ee7413cde9f0fb4cf3ffb54c9b3d0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76906753"
---
# <a name="hc-series-virtual-machine-overview"></a>HC Serisi sanal makineye genel bakış

Intel Xeon ölçeklenebilir Işlemcilerde HPC uygulama performansını en üst düzeye çıkarmak, bu yeni mimaride yerleştirmeyi işlemek için düşünceli bir yaklaşım gerektirir. Burada, HPC uygulamaları için Azure HC Serisi VM 'lerde uygulama uygulamamız ana hatlarıyla açıklanmıştır. "PNUMA" terimini bir fiziksel NUMA etki alanına ve "vNUMA" adlı bir sanallaştırılmış NUMA etki alanına başvuracak şekilde kullanacağız. Benzer şekilde, fiziksel CPU çekirdekleri için "pCore" terimini ve sanallaştırılmış CPU çekirdeklerini belirtmek için "vCore" terimini kullanacağız.

Fiziksel olarak, bir HC sunucusu toplam 48 fiziksel çekirdek için 2 * 24 çekirdekli Intel Xeon Platinum 8168 CPU olur. Her CPU tek bir pNUMA etki alanıdır ve altı adet DRAM kanalına Birleşik erişim sağlar. Intel Xeon Platinum CPU 'Ları, önceki nesle kıyasla (256 KB/çekirdek-> 1 MB/çekirdek) daha büyük bir L2 önbellek özelliğine sahiptir ve ayrıca, önceki Intel CPU 'Lara kıyasla L3 önbelleğini de azaltır (2,5 MB/Core-> 1,375 MB/çekirdek).

Yukarıdaki topoloji, HC Serisi hiper yönetici yapılandırmasını da taşır. Azure Hiper Yöneticisi 'nin VM ile kesintiye uğramadan çalışması için yer sağlamak üzere Pçekirdeklerini 0-1 ve 24-25 (yani her bir yuvada ilk 2 Pcore) ayırdık. Daha sonra, tüm kalan çekirdekler için sanal makineye pNUMA etki alanları atarsınız. Bu nedenle, VM şunları görür:

`(2 vNUMA domains) * (22 cores/vNUMA) = 44`VM başına çekirdek

VM 'de Pçekirdeklerin 0-1 ve 24-25 'e verilmeyen bir bilgi bulunmamaktadır. Bu nedenle, her vNUMA 'yı yerel olarak 22 çekirdeğe sahip gibi kullanıma sunar.

Intel Xeon Platinum, altın ve gümüş CPU, Ayrıca, CPU yuvası içinde ve dışında iletişim için bir on-zar 2B ağ ağı sunar. En iyi performans ve tutarlılık için işlem sabitlenmesini önemle öneririz. Temeldeki Silicon, Konuk VM 'ye olduğu için, işlem sabitleme, HC Serisi VM 'lerde çalışacaktır. Daha fazla bilgi için bkz. [Intel Xeon SP mimarisi](https://bit.ly/2RCYkiE).

Aşağıdaki diyagramda, Azure Hiper Yöneticisi ve HC Serisi VM için ayrılan çekirdekler gösterilmektedir.

![Azure Hiper Yöneticisi ve HC Serisi VM için ayrılan çekirdekler](./media/hc-series-overview/segregation-cores.png)

## <a name="hardware-specifications"></a>Donanım belirtimleri

| Donanım belirtimleri          | HC Serisi VM                     |
|----------------------------------|----------------------------------|
| Çekirdekler                            | 44 (HT Disabled)                 |
| CPU                              | Intel Xeon Platinum 8168 *        |
| CPU sıklığı (AVX olmayan)          | 3,7 GHz (tek çekirdek), 2.7-3.4 GHz (tüm çekirdekler) |
| Bellek                           | 8 GB/çekirdek (352 toplam)            |
| Yerel Disk                       | 700 GB NVMe                      |
| InfiniBand                       | 100 GB EDR Mellanox ConnectX-5 * * |
| Ağ                          | 50 GB Ethernet (40 GB kullanılabilir) Azure ikinci gen Smartnıc * * * |

## <a name="software-specifications"></a>Yazılım belirtimleri

| Yazılım belirtimleri     | HC Serisi VM          |
|-----------------------------|-----------------------|
| Maksimum MPı Işi boyutu            | 13200 çekirdek (tek bir VMSS 'de Tekplacementgroup = true ile 300 VM) |
| MPı desteği                 | MVAPICH2, OpenMPI, MPICH, platform MPı, Intel MPı  |
| Ek çerçeveler       | Birleşik Iletişim X, libfabric, PGAS |
| Azure depolama desteği       | STD + Premium (en fazla 4 disk) |
| SRLOV RDMA için işletim sistemi desteği   | CentOS/RHEL 7.6 +, SLES 12 SP4 +, WinServer 2016 + |
| Azure CycleCloud desteği    | Evet                         |
| Azure Batch desteği         | Evet                         |

## <a name="next-steps"></a>Sonraki adımlar

* Azure 'da [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc) ve [WINDOWS](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc) için HPC VM boyutları hakkında daha fazla bilgi edinin.

* Azure 'da [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
