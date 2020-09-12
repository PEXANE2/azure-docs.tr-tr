---
title: Kapsayıcılar için Azure Izleyici ile GPU izlemeyi yapılandırma | Microsoft Docs
description: Bu makalede, kapsayıcılar için Azure Izleyici ile NVıDıA ve AMD GPU etkin düğümleri ile Kubernetes kümelerini izlemeyi nasıl yapılandırabileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 03/27/2020
ms.openlocfilehash: 4c6044d407dc4abd0e69bac0190cc19c901022c3
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89569705"
---
# <a name="configure-gpu-monitoring-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici ile GPU izlemeyi yapılandırma

Aracı sürümü *ciprod03022019*ile başlayarak, kapsayıcılar için Azure izleyici tümleşik aracı artık GPU 'Yu algılayan Kubernetes küme düğümlerinde GPU (grafik işleme birimleri) kullanımını ızlemeyı ve GPU kaynaklarını isteyen ve bunları kullanan kapsayıcıları/kapsayıcıları izlemeyi destekliyor.

## <a name="supported-gpu-vendors"></a>Desteklenen GPU satıcıları

Kapsayıcılar için Azure Izleyici, GPU kümelerinin aşağıdaki GPU satıcılarından izlenmesini destekler:

- [NVIDIA](https://developer.nvidia.com/kubernetes-gpu)

- [AMD](https://github.com/RadeonOpenCompute/k8s-device-plugin)

Kapsayıcılar için Azure izleyici, düğüm üzerindeki GPU kullanımının izlenmesini otomatik olarak başlatır ve aşağıdaki ölçümleri 60sec aralıklarında toplayarak ve bunları **ınsiurm ölçümleri** tablosunda depolayarak, Pod ve iş yükleri isteyen GPU 'ları

>[!NOTE]
>Küme, GPU düğümleri ile sağlandıktan sonra GPU iş yüklerini çalıştırmak için AKS 'in gerektirdiği şekilde [GPU sürücüsünün](../../aks/gpu-cluster.md) yüklü olduğundan emin olun. Kapsayıcılar için Azure izleyici, düğüm içinde çalışan GPU sürücü Pod aracılığıyla GPU ölçümleri toplar. 

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

Kapsayıcılar için Azure Izleyici, tabloda daha önce listelenen ölçümler için, her küme için bir GPU çalışma kitabı olarak önceden yapılandırılmış grafikler içerir. Sol bölmedeki **çalışma kitaplarını** seçerek ve Öngörüler Içindeki **çalışma kitaplarını görüntüle** açılır listesinden GPU çalışma kitabı **düğümü GPU** 'su doğrudan bir aks kümesinden bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

- GPU etkin düğümleri içeren bir AKS kümesini dağıtmayı öğrenmek için bkz. [Azure Kubernetes Service (aks) üzerinde yoğun işlem yoğunluklu iş yükleri Için GPU 'Ları kullanma](../../aks/gpu-cluster.md) .

- [Microsoft Azure 'de GPU IYILEŞTIRILMIŞ VM SKU 'ları](../../virtual-machines/sizes-gpu.md)hakkında daha fazla bilgi edinin.

- [Kubernetes 'Te GPU desteğini](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) inceleyerek, bir kümedeki bir veya daha fazla düğümde GPU 'ları yönetmek Için Kubernetes deneysel desteği hakkında daha fazla bilgi edinin.
