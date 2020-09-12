---
title: HB Serisi VM boyutu performansı
description: Azure 'da HB Serisi VM boyutları için performans testi sonuçları hakkında bilgi edinin.
author: vermagit
manager: gwallace
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 09/09/2020
ms.author: amverma
ms.openlocfilehash: 34e9ef3ab46f2ce11500aa87db9676635d3e9b4f
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90016291"
---
# <a name="hb-series-virtual-machine-sizes"></a>HB Serisi sanal makine boyutları

HB Serisi boyutlarda çeşitli performans testleri çalıştırıldı. Bu performans testinin bazı sonuçları aşağıda verilmiştir.

| İş Yükü                                        | HB                    |
|-------------------------------------------------|-----------------------|
| STREAM Triad                                    | 260 GB/sn (CCX başına 32-33 GB/sn)  |
| Yüksek performanslı linpack (HPL)                  | 1.000 GigaFLOPS (Rtepe), 860 GigaFLOPS (Rmax) |
| RDMA gecikme & bant genişliği                        | 1,27 mikrosaniye, 99,1 GB/sn   |
| Yerel NVMe SSD üzerinde FIO                           | 1,7 GB/sn okuma, 1,0 GB/sn yazma      |  
| 4 * Azure Premium SSD (P30 yönetilen diskler, RAID0) * *  | 725 MB/s okuma, 780 MB/yazma   |


## <a name="mpi-latency"></a>MPı gecikmesi

OSU mikro kıyaslama paketinden MPı gecikme süresi testi çalıştırılır. Örnek betikler [GitHub](https://github.com/Azure/azhpc-images/blob/04ddb645314a6b2b02e9edb1ea52f079241f1297/tests/run-tests.sh) 'da

```bash
./bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./osu_latency 
```

:::image type="content" source="./media/latency-hb.png" alt-text="Azure HB üzerinde MPı gecikmesi.":::

## <a name="mpi-bandwidth"></a>MPı bant genişliği

OSU mikro kıyaslama paketinden MPı bant genişliği sınaması çalıştırılır. Örnek betikler [GitHub](https://github.com/Azure/azhpc-images/blob/04ddb645314a6b2b02e9edb1ea52f079241f1297/tests/run-tests.sh) 'da

```bash
./mvapich2-2.3.install/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./mvapich2-2.3/osu_benchmarks/mpi/pt2pt/osu_bw
```

:::image type="content" source="./media/bandwidth-hb.png" alt-text="Azure HB üzerinde MPı bant genişliği.":::


## <a name="mellanox-perftest"></a>Mellanox Perftest

[Mellanox Perftest paketinin](https://community.mellanox.com/s/article/perftest-package) gecikme süresi (ib_send_lat) ve bant genişliği (ib_send_bw) gibi birçok InfiniBand testi vardır. Örnek bir komut aşağıda verilmiştir.

```console
numactl --physcpubind=[INSERT CORE #]  ib_send_lat -a
```

## <a name="next-steps"></a>Sonraki adımlar

- En son duyurular ve bazı yüksek performanslı bilgi Işlem (HPC) örnekleri ve [Azure Işlem teknik topluluğu bloglarında](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute)sonuçları okuyun.
- HPC iş yüklerini çalıştırmanın daha üst düzey mimari görünümü için bkz. [Azure 'Da yüksek performanslı bilgi işlem (HPC)](/azure/architecture/topics/high-performance-computing/).
