---
title: HC serisi VM'ye genel bakış - Azure Sanal Makineler| Microsoft Dokümanlar
description: Azure'da HC serisi VM boyutu için önizleme desteği hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906753"
---
# <a name="hc-series-virtual-machine-overview"></a>HC serisi sanal makineye genel bakış

Intel Xeon Ölçeklenebilir İşlemciler'de HPC uygulama performansını en üst düzeye çıkarmak, bu yeni mimariye yerleştirme yi işlemek için düşünceli bir yaklaşım gerektirir. Burada, HPC uygulamaları için Azure HC serisi VM'lerde uygulamamızı ana hatlarımızı sarıyoruz. Fiziksel bir NUMA etki alanına başvurmak için "pNUMA" terimini ve sanallaştırılmış numa etki alanına başvurmak için "vNUMA" terimini kullanacağız. Benzer şekilde, fiziksel CPU çekirdeklerine başvurmak için "pCore" terimini ve sanallaştırılmış CPU çekirdeklerine başvurmak için "vCore" terimini kullanacağız.

Fiziksel olarak, bir HC sunucusu 2 * 24 çekirdekli Intel Xeon Platinum 8168 CPU toplam 48 fiziksel çekirdek için. Her CPU tek bir pNUMA etki alanıdır ve altı DRAM kanalına birleşik erişime sahiptir. Intel Xeon Platinum CPU'lar önceki nesillere göre 4 kat daha büyük bir L2 önbelleğine sahiptir (256 KB/core -> 1 MB/core), aynı zamanda önceki Intel CPU'larına göre L3 önbelleğini azaltırken (2,5 MB/çekirdek -> 1,375 MB/çekirdek).

Yukarıdaki topoloji HC-serisi hipervizör konfigürasyonuna da taşınır. Azure hipervizörünün VM'ye müdahale etmeden çalışmasına yer sağlamak için, pCores 0-1 ve 24-25 (diğer bir şekilde her soketteki ilk 2 pCores) rezerve ediyoruz. Daha sonra kalan tüm çekirdekleri VM'ye pNUMA etki alanları atıyoruz. Böylece, VM göreceksiniz:

`(2 vNUMA domains) * (22 cores/vNUMA) = 44`VM başına çekirdek

VM pCores 0-1 ve 24-25 ona verilmedi hiçbir bilgiye sahiptir. Böylece, her vNUMA'yı yerli olarak 22 çekirdeği varmış gibi ortaya çıkarır.

Intel Xeon Platinum, Gold ve Silver CPU'lar da CPU soketi içinde ve dışında iletişim için bir on-die 2D örgü ağı tanıtmak. Optimum performans ve tutarlılık için proses sabitlemeyi şiddetle tavsiye ediyoruz. Proses sabitleme HC serisi VM'lerde çalışacaktır, çünkü altta yatan silikon konuk VM'ye olduğu gibi maruz kalır. Daha fazla bilgi için [Intel Xeon SP mimarisine](https://bit.ly/2RCYkiE)bakın.

Aşağıdaki diyagram, Azure Hypervisor ve HC serisi VM için ayrılmış çekirdeklerin ayrıştırıcısını gösterir.

![Azure Hypervisor ve HC serisi VM için ayrılmış çekirdeklerin ayrılması](./media/hc-series-overview/segregation-cores.png)

## <a name="hardware-specifications"></a>Donanım belirtimleri

| Donanım Özellikleri          | HC serisi VM                     |
|----------------------------------|----------------------------------|
| Çekirdekler                            | 44 (HT devre dışı)                 |
| CPU                              | Intel Xeon Platin 8168*        |
| Cpu Frekansı (AVX dışı)          | 3.7 GHz (tek çekirdek), 2.7-3.4 GHz (tüm çekirdekler) |
| Bellek                           | 8 GB/çekirdek (toplam 352)            |
| Yerel Disk                       | 700 GB NVMe                      |
| ınfiniband                       | 100 Gb EDR Mellanox ConnectX-5** |
| Ağ                          | 50 Gb Ethernet (40 Gb kullanılabilir) Azure ikinci Gen SmartNIC*** |

## <a name="software-specifications"></a>Yazılım özellikleri

| Yazılım Özellikleri     | HC serisi VM          |
|-----------------------------|-----------------------|
| Max MPI İş Boyutu            | 13200 çekirdek (tek li PlacementGroup=true ile tek bir VMSS'de 300 VM) |
| MPI Desteği                 | MVAPICH2, OpenMPI, MPICH, Platform MPI, Intel MPI  |
| Ek Çerçeveler       | Birleşik İletişim X, libfabric, PGAS |
| Azure Depolama Desteği       | Std + Premium (maksimum 4 disk) |
| SRIOV RDMA için İşletim Sistemi Desteği   | CentOS/RHEL 7.6+, SLES 12 SP4+, WinServer 2016+ |
| Azure CycleCloud Desteği    | Evet                         |
| Azure Toplu Destek         | Evet                         |

## <a name="next-steps"></a>Sonraki adımlar

* Azure'da [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc) ve [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc) için HPC VM boyutları hakkında daha fazla bilgi edinin.

* Azure'da [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
