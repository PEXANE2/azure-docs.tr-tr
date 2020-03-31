---
title: Kapsayıcılar için Azure Monitor ile GPU izlemeyi yapılandırın | Microsoft Dokümanlar
description: Bu makalede, nvidia ve AMD GPU etkin düğümleri ile izleme Kubernetes kümeleri yapılandırmak nasıl yapılandırırsınız kapsayıcılar için Azure Monitor ile açıklar.
ms.topic: conceptual
ms.date: 03/27/2020
ms.openlocfilehash: 958f5ab33edcd280f5673391eba907728f1153c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80373316"
---
# <a name="configure-gpu-monitoring-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitörü ile GPU izlemeyi yapılandırın

Aracı sürümü *ciprod03022019*ile başlayarak, kapsayıcılar için Azure monitörü tümleşik aracı şimdi GPU'ya duyarlı Kubernetes küme düğümlerinde GPU (grafik işleme birimleri) kullanımını izlemeyi destekler ve GPU kaynaklarını isteyen ve kullanan bölmeleri/kapları izler.

## <a name="supported-gpu-vendors"></a>Desteklenen GPU satıcıları

Kapsayıcılar için Azure Monitor, Aşağıdaki GPU satıcılarından GPU kümelerinin izlenmesini destekler:

- [Nvidia](https://developer.nvidia.com/kubernetes-gpu)

- [Amd](https://github.com/RadeonOpenCompute/k8s-device-plugin)

Kapsayıcılar için Azure Monitor düğümlerde GPU kullanımını otomatik olarak izlemeye başlar ve GPU aşağıdaki ölçümleri 60 sn aralıklarla toplayıp **InsightMetrics** tablosunda depolayarak bakla ve iş yüklerini talep eder:

|Ölçüm adı |Metrik boyut (etiketler) |Açıklama |
|------------|------------------------|------------|
|konteynerGpuDutyCycle |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuSatıcı|GPU'nun bir kapsayıcı için meşgul/etkin olarak işlendiği geçmiş örnek dönemdeki (60 saniye) süre yüzdesi. Görev döngüsü 1 ile 100 arasında bir sayıdır. |
|konteynerGpuLimits |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |Her kapsayıcı bir veya daha fazla GPU olarak sınırları belirtebilir. GPU'nun bir kısmını istemek veya sınırlamak mümkün değildir. |
|konteynerGpuRequests |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |Her konteyner bir veya daha fazla GPU talep edebilir. GPU'nun bir kısmını istemek veya sınırlamak mümkün değildir.|
|konteynerGpumemoryTotalBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuSatıcı |Belirli bir kapsayıcı için kullanılabilecek baytlarda GPU Bellek miktarı. |
|konteynerGpumemoryUsedBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuSatıcı |Belirli bir kapsayıcı tarafından kullanılan baytlarda GPU Bellek miktarı. |
|nodeGpuAllocatable |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuSatıcı |Kubernetes tarafından kullanılabilen bir düğümdeki GPU sayısı. |
|nodeGpuKapasite |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuSatıcı |Düğümdeki Toplam GPU sayısı. |

## <a name="gpu-performance-charts"></a>GPU performans grafikleri 

Kapsayıcılar için Azure Monitörü, tabloda daha önce her küme için GPU çalışma kitabı olarak listelenen ölçümler için önceden yapılandırılmış grafikler içerir. GPU çalışma kitabı **Düğümü GPU'yu,** sol bölmeden Çalışma Kitapları'nı ve Insight'taki Çalışma **Kitaplarını** Görünüm Açılır Listesini seçerek doğrudan bir AKS kümesinden bulabilirsiniz. **Workbooks**

## <a name="next-steps"></a>Sonraki adımlar

- GPU özellikli düğümleri içeren bir AKS kümesini nasıl dağıtılanın öğrenebilirsiniz öğrenmek [için Azure Kubernetes Hizmeti'nde](../../aks/gpu-cluster.md) (AKS) bilgi işlem yoğun iş yükleri için GPU'ları kullanın.

- [Microsoft Azure'da GPU Optimize Edilmiş VM SKU'lar](../../virtual-machines/sizes-gpu.md)hakkında daha fazla bilgi edinin.

- Bir [kümedeki](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) bir veya daha fazla düğümde GUBERNETEs'in gpu desteğini gözden geçirin.