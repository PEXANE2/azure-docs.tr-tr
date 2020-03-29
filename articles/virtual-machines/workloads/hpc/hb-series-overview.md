---
title: HB serisi VM'ye genel bakış - Azure Sanal Makineler | Microsoft Dokümanlar
description: Azure'da HB serisi VM boyutu için önizleme desteği hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67707768"
---
# <a name="hb-series-virtual-machines-overview"></a>HB serisi sanal makinelere genel bakış

AMD EPYC'de yüksek performanslı bilgi işlem (HPC) uygulama performansını en üst düzeye çıkarmak, düşünceli bir yaklaşım bellek yerelliği ve işlem yerleşimi gerektirir. Aşağıda AMD EPYC mimarisive HPC uygulamaları için Azure'da uygulamamızı ana hatlarını sıralıyoruz. Fiziksel bir NUMA etki alanına başvurmak için "pNUMA" terimini ve sanallaştırılmış numa etki alanına başvurmak için "vNUMA" terimini kullanacağız.

Fiziksel olarak, bir HB serisi 2 * 32 çekirdekli EPYC 7551 CPU toplam 64 fiziksel çekirdek için. Bu 64 çekirdek, her biri dört çekirdek olan ve "CPU Kompleksi" (veya "CCX") olarak bilinen 16 pNUMA etki alanına (soket başına 8) ayrılır. Her CCX'in kendi L3 önbelleği vardır, bu da bir işletim sistemi nin pNUMA/vNUMA sınırını nasıl göreceğidir. Bitişik CCXs bir çift fiziksel DRAM (HB serisi sunucularda DRAM 32 GB) iki kanal erişim paylaşır.

Azure hipervizörünün VM'ye müdahale etmeden çalışmasına yer sağlamak için fiziksel pNUMA etki alanı 0 (ilk CCX) rezerve ediyoruz. Daha sonra VM için pNUMA etki alanları 1-15 (kalan CCX birimleri) atıyoruz. VM göreceksiniz:

`(15 vNUMA domains) * (4 cores/vNUMA) = 60`VM başına çekirdek

VM, kendisi, pNUMA 0 ona verilmediğini bilmiyor. VM, vSocket 0'da 7 vNUMA ve vSocket 1'de 8 vNUMA ile vNUMA 0-14 olarak vNUMA 15'i anlar. Bu asimetrik olsa da, işletim sisteminiz önyükleme yapmalı ve normal şekilde çalışmalıdır. Daha sonra bu kılavuzda, bu asimetrik NUMA düzeninde MPI uygulamalarını en iyi nasıl çalıştırabileceğimizi öğretiyoruz.

Proses sabitleme HB serisi VM'lerde çalışacaktır, çünkü altta yatan silikonu konuk VM'ye maruz bırakırız. Optimum performans ve tutarlılık için proses sabitlemeyi şiddetle tavsiye ediyoruz.

LinkedIn'deki [AMD EPYC mimarisi](https://bit.ly/2Epv3kC) ve [çok yongalı mimariler](https://bit.ly/2GpQIMb) hakkında daha fazla görüş. Daha ayrıntılı bilgi için [AMD EPYC İşlemciler için HPC Tuning Guide'a](https://bit.ly/2T3AWZ9)bakın.

Aşağıdaki diyagram, Azure Hypervisor ve HB serisi VM için ayrılmış çekirdeklerin ayrıştırıcısını gösterir.

![Azure Hypervisor ve HB serisi VM için ayrılmış çekirdeklerin ayrılması](./media/hb-series-overview/segregation-cores.png)

## <a name="hardware-specifications"></a>Donanım belirtimleri

| HW Teknik Özellikleri                | HB serisi VM                     |
|----------------------------------|----------------------------------|
| Çekirdekler                            | 60 (SMT devre dışı)                |
| CPU                              | AMD EPYC 7551*                   |
| Cpu Frekansı (AVX dışı)          | ~2,55 GHz (tek + tüm çekirdekler)   |
| Bellek                           | 4 GB/çekirdek (toplam 240)            |
| Yerel Disk                       | 700 GB NVMe                      |
| ınfiniband                       | 100 Gb EDR Mellanox ConnectX-5** |
| Ağ                          | 50 Gb Ethernet (40 Gb kullanılabilir) Azure ikinci Gen SmartNIC*** |

## <a name="software-specifications"></a>Yazılım özellikleri

| SW Teknik Özellikleri           |HB serisi VM           |
|-----------------------------|-----------------------|
| Max MPI İş Boyutu            | 6000 çekirdek (100 sanal makine ölçek seti) 12000 çekirdek (200 sanal makine ölçek kümesi)  |
| MPI Desteği                 | MVAPICH2, OpenMPI, MPICH, Platform MPI, Intel MPI  |
| Ek Çerçeveler       | Birleşik İletişim X, libfabric, PGAS |
| Azure Depolama Desteği       | Std + Premium (maksimum 4 disk) |
| SRIOV RDMA için İşletim Sistemi Desteği   | CentOS/RHEL 7.6+, SLES 12 SP4+, WinServer 2016+  |
| Azure CycleCloud Desteği    | Evet                         |
| Azure Toplu Destek         | Evet                         |

## <a name="next-steps"></a>Sonraki adımlar

* Azure'da [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc) ve [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc) için HPC VM boyutları hakkında daha fazla bilgi edinin.

* Azure'da [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
