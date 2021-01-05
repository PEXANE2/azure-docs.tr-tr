---
title: HC Serisi VM 'ye genel bakış-Azure sanal makineleri | Microsoft Docs
description: Azure 'da HC Serisi VM boyutu için Önizleme desteği hakkında bilgi edinin.
author: vermagit
ms.service: virtual-machines
ms.subservice: workloads
ms.topic: article
ms.date: 08/19/2020
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: b9fe978da9accd28ea0e538f458325f10c9c5d8d
ms.sourcegitcommit: beacda0b2b4b3a415b16ac2f58ddfb03dd1a04cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97831899"
---
# <a name="hc-series-virtual-machine-overview"></a>HC Serisi sanal makineye genel bakış

Intel Xeon ölçeklenebilir Işlemcilerde HPC uygulama performansını en üst düzeye çıkarmak, bu yeni mimaride yerleştirmeyi işlemek için düşünceli bir yaklaşım gerektirir. Burada, HPC uygulamaları için Azure HC Serisi VM 'lerde uygulama uygulamamız ana hatlarıyla açıklanmıştır. "PNUMA" terimini bir fiziksel NUMA etki alanına ve "vNUMA" adlı bir sanallaştırılmış NUMA etki alanına başvuracak şekilde kullanacağız. Benzer şekilde, fiziksel CPU çekirdekleri için "pCore" terimini ve sanallaştırılmış CPU çekirdeklerini belirtmek için "vCore" terimini kullanacağız.

Fiziksel olarak, bir [HC Serisi](../../hc-series.md) sunucu 2 * 24 çekirdekli Intel Xeon Platinum 8168 CPU ve toplam 48 fiziksel çekirdek için. Her CPU tek bir pNUMA etki alanıdır ve altı adet DRAM kanalına Birleşik erişim sağlar. Intel Xeon Platinum CPU 'Ları, önceki nesle kıyasla (256 KB/çekirdek-> 1 MB/çekirdek) daha büyük bir L2 önbellek özelliğine sahiptir ve ayrıca, önceki Intel CPU 'Lara kıyasla L3 önbelleğini de azaltır (2,5 MB/Core-> 1,375 MB/çekirdek).

Yukarıdaki topoloji, HC Serisi hiper yönetici yapılandırmasını da taşır. Azure Hiper Yöneticisi 'nin VM ile kesintiye uğramadan çalışması için yer sağlamak üzere Pçekirdeklerini 0-1 ve 24-25 (yani her bir yuvada ilk 2 Pcore) ayırdık. Daha sonra, tüm kalan çekirdekler için sanal makineye pNUMA etki alanları atarsınız. Bu nedenle, VM şunları görür:

`(2 vNUMA domains) * (22 cores/vNUMA) = 44` VM başına çekirdek

VM 'de Pçekirdeklerin 0-1 ve 24-25 'e verilmeyen bir bilgi bulunmamaktadır. Bu nedenle, her vNUMA 'yı yerel olarak 22 çekirdeğe sahip gibi kullanıma sunar.

Intel Xeon Platinum, altın ve gümüş CPU, Ayrıca, CPU yuvası içinde ve dışında iletişim için bir on-zar 2B ağ ağı sunar. En iyi performans ve tutarlılık için işlem sabitlenmesini önemle öneririz. Temeldeki Silicon, Konuk VM 'ye olduğu için, işlem sabitleme, HC Serisi VM 'lerde çalışacaktır.

Aşağıdaki diyagramda, Azure Hiper Yöneticisi ve HC Serisi VM için ayrılan çekirdekler gösterilmektedir.

![Azure Hiper Yöneticisi ve HC Serisi VM için ayrılan çekirdekler](./media/hc-series-overview/segregation-cores.png)

## <a name="hardware-specifications"></a>Donanım belirtimleri

| Donanım belirtimleri          | HC Serisi VM                     |
|----------------------------------|----------------------------------|
| Çekirdekler                            | 44 (HT Disabled)                 |
| CPU                              | Intel Xeon Platinum 8168         |
| CPU sıklığı (AVX olmayan)          | 3,7 GHz (tek çekirdek), 2.7-3.4 GHz (tüm çekirdekler) |
| Bellek                           | 8 GB/çekirdek (352 toplam)            |
| Yerel Disk                       | 700 GB SSD                       |
| InfiniBand                       | 100 GB EDR Mellanox ConnectX-5   |
| Ağ                          | 50 GB Ethernet (40 GB kullanılabilir) Azure ikinci gen Smartnıc    |

## <a name="software-specifications"></a>Yazılım belirtimleri

| Yazılım belirtimleri     |HC Serisi VM           |
|-----------------------------|-----------------------|
| Maksimum MPı Işi boyutu            | 13200 çekirdek (tek bir sanal makine ölçek kümesindeki Tekplacementgroup = true ile, 300 VM)  |
| MPı desteği                 | HPC-X, Intel MPI, OpenMPI, MVAPICH2, MPICH, platform MPı  |
| Ek çerçeveler       | Birleşik Iletişim X, libfabric, PGAS |
| Azure depolama desteği       | Standart ve Premium diskler (en fazla 4 disk) |
| SRLOV RDMA için işletim sistemi desteği   | CentOS/RHEL 7.6 +, SLES 12 SP4 +, WinServer 2016 +  |
| Orchestrator desteği        | CycleCloud, Batch  |

## <a name="next-steps"></a>Sonraki adımlar

- [Intel Xeon SP mimarisi](https://software.intel.com/content/www/us/en/develop/articles/intel-xeon-processor-scalable-family-technical-overview.html)hakkında daha fazla bilgi edinin.
- En son duyurular ve bazı HPC örnekleri hakkında bilgi edinin ve [Azure Işlem teknik topluluk bloglarında](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute)bu sonuçları elde edin.
- Çalıştırılan HPC iş yüklerinin daha yüksek düzey mimari görünümü için bkz. [Azure 'Da yüksek performanslı bilgi işlem (HPC)](/azure/architecture/topics/high-performance-computing/).
