---
title: HB Serisi VM 'ye genel bakış-Azure sanal makineleri | Microsoft Docs
description: Azure 'da HB Serisi VM boyutu için Önizleme desteği hakkında bilgi edinin.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/16/2019
ms.author: amverma
ms.openlocfilehash: 62e4d3dbd7357f8c98df3307c1c8fe52cbed1c5e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67707768"
---
# <a name="hb-series-virtual-machines-overview"></a>HB Serisi sanal makinelere genel bakış

Yüksek performanslı bilgi işlem (HPC) uygulama performansının AMD EPıC üzerinde en üst düzeye çıkarmak, düşünceli bir yaklaşım bellek konumu ve işlem yerleşimi gerektirir. Aşağıda, AMD EPıC mimarisini ve bu uygulamayı HPC uygulamaları için Azure 'da uygulamamız ana hatlarıyla planlıyoruz. "PNUMA" terimini bir fiziksel NUMA etki alanına ve "vNUMA" adlı bir sanallaştırılmış NUMA etki alanına başvuracak şekilde kullanacağız.

Fiziksel olarak, bir HB Serisi 64 toplam 32 fiziksel çekirdek için 2 *-çekirdekli EPYıC 7551 CPU olur. Bu 64 çekirdekler, her biri dört çekirdekli ve "CPU karmaşık" (veya "CCX") olarak bilinen 16 pNUMA etki alanlarına (yuva başına 8) bölünmüştür. Her CCX 'in kendi L3 önbelleği vardır. Bu, bir işletim sisteminin bir pNUMA/vNUMA sınırını nasıl görebileceği anlamına gelir. Bitişik CCXs çifti, iki fiziksel DRAM kanalına erişimi paylaşır (32 GB/s-serisi sunucu).

Azure Hiper Yöneticisi 'nin VM ile kesintiye uğramadan çalışması için oda sağlamak üzere fiziksel pNUMA etki alanı 0 ' ı (ilk CCX) ayırdık. Daha sonra VM için pNUMA etki alanları 1-15 (kalan CCX birimleri) atarsınız. VM şu şekilde görüntülenir:

`(15 vNUMA domains) * (4 cores/vNUMA) = 60`VM başına çekirdek

Sanal makine, pNUMA 0 ' ın kendisine verilmediğini bilmez. VM, vNUMA 0-14 olarak pNUMA 1-15 'yi, vSocket 0 ve vSocket 1 üzerinde 8 vNUMA üzerinde 7 vNUMA ile anlamıştır. Bu, asimetrik olsa da, işletim sistemi normal şekilde önyükleme ve çalışır olmalıdır. Bu kılavuzda daha sonra, bu asimetrik NUMA düzeninde MPı uygulamalarının ne kadar iyi çalıştırılacağını öğreneceksiniz.

İşlem sabitleme, HB Serisi VM 'lerde çalışarak temel bir Silicon as 'yi konuk VM 'de kullanıma sunduk. En iyi performans ve tutarlılık için işlem sabitlenmesini önemle öneririz.

Daha fazla bilgi için bkz. LinkedIn 'de daha fazla [AMD epi mimarisi](https://bit.ly/2Epv3kC) ve [Çoklu yonga mimarileri](https://bit.ly/2GpQIMb) . Daha ayrıntılı bilgi için bkz. [AMD EPYıC işlemcileri Için HPC ayarlama Kılavuzu](https://bit.ly/2T3AWZ9).

Aşağıdaki diyagramda, Azure Hiper Yöneticisi ve HB Serisi VM için ayrılan çekirdekler gösterilmektedir.

![Azure Hiper Yöneticisi ve HB Serisi VM için ayrılan çekirdekler](./media/hb-series-overview/segregation-cores.png)

## <a name="hardware-specifications"></a>Donanım belirtimleri

| HW belirtimleri                | HB Serisi VM                     |
|----------------------------------|----------------------------------|
| Çekirdekler                            | 60 (SMT devre dışı)                |
| CPU                              | AMD EPYıC 7551 *                   |
| CPU sıklığı (AVX olmayan)          | ~ 2,55 GHz (tek + tüm çekirdekler)   |
| Bellek                           | 4 GB/çekirdek (240 toplam)            |
| Yerel Disk                       | 700 GB NVMe                      |
| InfiniBand                       | 100 GB EDR Mellanox ConnectX-5 * * |
| Ağ                          | 50 GB Ethernet (40 GB kullanılabilir) Azure ikinci gen Smartnıc * * * |

## <a name="software-specifications"></a>Yazılım belirtimleri

| Yazılım belirtimleri           |HB Serisi VM           |
|-----------------------------|-----------------------|
| Maksimum MPı Işi boyutu            | 6000 çekirdek (100 sanal makine ölçek kümeleri) 12000 çekirdek (200 Sanal Makine Ölçek Kümeleri)  |
| MPı desteği                 | MVAPICH2, OpenMPI, MPICH, platform MPı, Intel MPı  |
| Ek çerçeveler       | Birleşik Iletişim X, libfabric, PGAS |
| Azure depolama desteği       | STD + Premium (en fazla 4 disk) |
| SRLOV RDMA için işletim sistemi desteği   | CentOS/RHEL 7.6 +, SLES 12 SP4 +, WinServer 2016 +  |
| Azure CycleCloud desteği    | Yes                         |
| Azure Batch desteği         | Evet                         |

## <a name="next-steps"></a>Sonraki adımlar

* Azure 'da [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc) ve [WINDOWS](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc) için HPC VM boyutları hakkında daha fazla bilgi edinin.

* Azure 'da [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
