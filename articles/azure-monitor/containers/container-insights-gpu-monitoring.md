---
title: Kapsayıcı öngörüleri ile GPU izlemeyi yapılandırma | Microsoft Docs
description: Bu makalede, kapsayıcı öngörüleri ile NVıDıA ve AMD GPU etkin düğümleri ile Kubernetes kümelerini izlemeyi nasıl yapılandırabileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 03/27/2020
ms.openlocfilehash: 2958b000ac0dabcd7fddf75a58f553b705a95e9a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101731876"
---
# <a name="configure-gpu-monitoring-with-container-insights"></a>Kapsayıcı öngörüleri ile GPU izlemeyi yapılandırma

*Ciprod03022019* aracı sürümü ile başlayarak, kapsayıcı öngörüleri tümleşik Aracısı artık GPU 'Yu algılayan Kubernetes küme düğümlerinde GPU (grafik işleme birimleri) kullanımını ızlemeyı ve GPU kaynaklarını isteyen ve bu kapsayıcıları/kapsayıcıları izlemenizi sağlar.

## <a name="supported-gpu-vendors"></a>Desteklenen GPU satıcıları

Kapsayıcı öngörüleri, GPU kümelerinin aşağıdaki GPU satıcılarından izlenmesini destekler:

- [NVIDIA](https://developer.nvidia.com/kubernetes-gpu)

- [AMD](https://github.com/RadeonOpenCompute/k8s-device-plugin)

Kapsayıcı öngörüleri, düğüm üzerindeki GPU kullanımını otomatik olarak izlemeye başlar ve aşağıdaki ölçümleri 60sec aralıklarında toplayarak ve bunları **ınsiurs ölçümleri** tablosunda depolayarak Pod ve iş yükleri isteyen GPU 'ları otomatik olarak başlatır.

>[!NOTE]
>Küme, GPU düğümleri ile sağlandıktan sonra GPU iş yüklerini çalıştırmak için AKS 'in gerektirdiği şekilde [GPU sürücüsünün](../../aks/gpu-cluster.md) yüklü olduğundan emin olun. Kapsayıcı öngörüleri, düğüm içinde çalışan GPU sürücü Pod aracılığıyla GPU ölçümleri toplar. 

|Ölçüm adı |Ölçüm boyutu (Etiketler) |Description |
|------------|------------------------|------------|
|Containergpudutyıcycle |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor|GPU 'nun bir kapsayıcı için meşgul/etkin olarak işlenmesi sırasında geçen örnek dönem (60 saniye) üzerinden geçen sürenin yüzdesi. Vergi çevrimi 1 ile 100 arasında bir sayıdır. |
|Containergpulimit |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |Her kapsayıcı, bir veya daha fazla GPU olarak sınırlar belirtebilir. GPU 'nun bir kesiri istemek veya sınırlamak mümkün değildir. |
|containerGpuRequests |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |Her kapsayıcı bir veya daha fazla GPU talep edebilir. GPU 'nun bir kesiri istemek veya sınırlamak mümkün değildir.|
|containerGpumemoryTotalBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Belirli bir kapsayıcıda kullanılmak üzere kullanılabilecek, bayt cinsinden GPU belleği miktarı. |
|Containergpuımemoryusedbytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Belirli bir kapsayıcı tarafından kullanılan bayt cinsinden GPU belleği miktarı. |
|nodeGpuAllocatable |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Kubernetes tarafından kullanılabilecek bir düğümdeki GPU sayısı. |
|nodeGpuCapacity |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Bir düğümdeki toplam GPU sayısı. |

## <a name="gpu-performance-charts"></a>GPU performans grafikleri 

Kapsayıcı öngörüleri, tabloda daha önce listelenen ölçümler için, her küme için bir GPU çalışma kitabı olarak önceden yapılandırılmış grafikler içerir. Kapsayıcı öngörüleri için kullanılabilen çalışma kitaplarının bir açıklaması için bkz. kapsayıcı içgörüleri [Içindeki çalışma kitapları](../insights/container-insights-reports.md) .

## <a name="next-steps"></a>Sonraki adımlar

- GPU etkin düğümleri içeren bir AKS kümesini dağıtmayı öğrenmek için bkz. [Azure Kubernetes Service (aks) üzerinde yoğun işlem yoğunluklu iş yükleri Için GPU 'Ları kullanma](../../aks/gpu-cluster.md) .

- [Microsoft Azure 'de GPU IYILEŞTIRILMIŞ VM SKU 'ları](../../virtual-machines/sizes-gpu.md)hakkında daha fazla bilgi edinin.

- [Kubernetes 'Te GPU desteğini](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) inceleyerek, bir kümedeki bir veya daha fazla düğümde GPU 'ları yönetmek Için Kubernetes deneysel desteği hakkında daha fazla bilgi edinin.
