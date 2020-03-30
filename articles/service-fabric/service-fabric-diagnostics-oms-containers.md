---
title: Azure Monitor günlükleriyle kapsayıcıları izleme
description: Azure Hizmet Kumaşı kümelerinde çalışan kapsayıcıları izlemek için Azure Monitor günlüklerini kullanın.
author: srrengar
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 8d4231de13da3f8b2960bd4852136f803a97a546
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614443"
---
# <a name="monitor-containers-with-azure-monitor-logs"></a>Azure Monitor günlükleriyle kapsayıcıları izleme
 
Bu makalede, kapsayıcı olaylarını görüntülemek için Azure Monitor günlükleri kapsayıcı izleme çözümünü ayarlamak için gereken adımlar ele geçmektedir. Kapsayıcı olayları toplamak için kümenizi ayarlamak için bu [adım adım öğreticiye](service-fabric-tutorial-monitoring-wincontainers.md)bakın. 

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="set-up-the-container-monitoring-solution"></a>Konteyner izleme çözümünü ayarlama

> [!NOTE]
> Kümeniz için Azure Monitor günlüklerinin ayarlanması ve düğümlerinizde Log Analytics aracısının dağıtılması gerekir. Bunu yapmazsanız, Azure Monitor [günlüklerini ayarla](service-fabric-diagnostics-oms-setup.md) ve [Log Analytics aracısını önce bir kümeye ekleyin](service-fabric-diagnostics-oms-agent.md) adımlarını izleyin.

1. Kümeniz Azure Monitor günlükleri ve Log Analytics aracısıyla ayarlandıktan sonra konteynırlarınızı dağıtın. Bir sonraki adıma geçmeden önce konteynerlerinizin dağıtılmasını bekleyin.

2. Azure Marketi'nde, *Kapsayıcı İzleme Çözümü'ni* arayın ve İzleme + Yönetim kategorisi altında görünen **Kapsayıcı İzleme Çözümü** kaynağını tıklayın.

    ![Kapsayıcılar çözümü ekleme](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Küme için zaten oluşturulmuş olan aynı çalışma alanı içinde çözüm oluşturun. Bu değişiklik, aracının kapsayıcılar üzerinde docker verileri toplamaya başlamasını otomatik olarak tetikler. Yaklaşık 15 dakika içinde, aşağıdaki resimde gösterildiği gibi, gelen günlükleri ve istatistikleri ile çözüm ışığı görmelisiniz.

    ![Temel Günlük Analiz Panosu](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Aracı, Azure Monitor günlüklerinde sorgulanabilen veya performans göstergelerini görselleştirmek için kullanılabilen kapsayıcıya özgü birkaç günlük toplamayı sağlar. Toplanan günlük türleri şunlardır:

* ContainerInventory: kapsayıcı konumu, adı ve görüntüleri hakkındaki bilgileri gösterir
* ContainerImageInventory: D'ler veya boyutlar da dahil olmak üzere dağıtılan görüntüler hakkında bilgi
* ContainerLog: belirli hata günlükleri, docker günlükleri (stdout, vb) ve diğer girişler
* ContainerServiceLog: çalıştırılabilen docker daemon komutları
* Perf: kapsayıcı işlemci, bellek, ağ trafiği, disk i/o ve ana bilgisayar makinelerinden özel ölçümler dahil performans sayaçları



## <a name="next-steps"></a>Sonraki adımlar
* [Azure Monitor günlükleri Kapsayıcıçözümü](../azure-monitor/insights/containers.md)hakkında daha fazla bilgi edinin.
* Servis Kumaşı - [Servis Kumaşı ve konteynerler](service-fabric-containers-overview.md) hakkında daha fazla bilgi edinin
* Azure Monitor [günlüklerinin](../log-analytics/log-analytics-log-searches.md) bir parçası olarak sunulan günlük arama ve sorgulama özelliklerine aşina olun
* Algılama ve tanılama da yardımcı olmak için [otomatik uyarı](../log-analytics/log-analytics-alerts.md) kuralları ayarlamak için Azure Monitor günlüklerini yapılandırın