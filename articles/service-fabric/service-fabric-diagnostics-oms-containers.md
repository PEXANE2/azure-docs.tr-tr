---
title: Azure Izleyici günlükleri ile kapsayıcıları izleme
description: Azure Service Fabric kümelerinde çalıştırılan kapsayıcıları izlemek için Azure Izleyici günlüklerini kullanın.
author: srrengar
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 8d4231de13da3f8b2960bd4852136f803a97a546
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614443"
---
# <a name="monitor-containers-with-azure-monitor-logs"></a>Azure Izleyici günlükleri ile kapsayıcıları izleme
 
Bu makalede, kapsayıcı olaylarını görüntülemek için Azure Izleyici günlükleri kapsayıcı izleme çözümünü ayarlamak için gereken adımlar ele alınmaktadır. Kümenizi kapsayıcı olayları toplayacak şekilde ayarlamak için bu [adım adım öğreticiye](service-fabric-tutorial-monitoring-wincontainers.md)bakın. 

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="set-up-the-container-monitoring-solution"></a>Kapsayıcı izleme çözümünü ayarlama

> [!NOTE]
> Kümeniz için Azure Izleyici günlüklerini ayarlamış olmanız ve Log Analytics aracısının düğümlere dağıtılmasını sağlayabilirsiniz. Aksi takdirde, [Azure izleyici günlüklerini ayarlama](service-fabric-diagnostics-oms-setup.md) ve [Log Analytics aracısını önce kümeye ekleme](service-fabric-diagnostics-oms-agent.md) bölümündeki adımları uygulayın.

1. Kümeniz Azure Izleyici günlükleri ve Log Analytics aracısında kurulduktan sonra Kapsayıcılarınızı dağıtın. Bir sonraki adıma geçmeden önce kapsayıcılarınızın dağıtılmasını bekleyin.

2. Azure Marketi 'nde *kapsayıcı Izleme çözümünü* arayın ve izleme ve yönetim kategorisinin altında görüntülenen **kapsayıcı izleme çözümü** kaynağına tıklayın.

    ![Kapsayıcılar çözümü ekleme](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Çözümü, küme için önceden oluşturulmuş aynı çalışma alanı içinde oluşturun. Bu değişiklik, kapsayıcılarda Docker verilerinin toplamaya başlamak için aracıyı otomatik olarak tetikler. Yaklaşık 15 dakika içinde, aşağıdaki görüntüde gösterildiği gibi çözüm ışığını gelen Günlükler ve istatistikler ile birlikte görmeniz gerekir.

    ![Temel Log Analytics panosu](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Aracı, Azure Izleyici günlüklerinde sorgulanabilen birkaç kapsayıcıya özgü günlük koleksiyonunun toplanmasını sağlar veya performans göstergelerini görselleştirmek için kullanılabilir. Toplanan günlük türleri şunlardır:

* Containerınventory: kapsayıcı konumu, adı ve görüntüleri hakkında bilgi gösterir
* Containerımageınventory: kimlikler veya boyutlar dahil dağıtılan görüntülerle ilgili bilgiler
* ContainerLog: belirli hata günlükleri, Docker günlükleri (stdout, vb.) ve diğer girdiler
* ContainerServiceLog: çalışan Docker Daemon komutları
* Performans: kapsayıcı CPU, bellek, ağ trafiği, disk g/ç ve konak makinelerinden özel ölçümler dahil performans sayaçları



## <a name="next-steps"></a>Sonraki adımlar
* [Azure izleyici günlük kapsayıcıları çözümü](../azure-monitor/insights/containers.md)hakkında daha fazla bilgi edinin.
* Service Fabric [Service Fabric ve kapsayıcılarda](service-fabric-containers-overview.md) kapsayıcı düzenlemesi hakkında daha fazla bilgi edinin
* Azure Izleyici günlüklerinin bir parçası olarak sunulan [günlük araması ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleriyle familiarized alın
* Algılama ve tanılama konusunda yardımcı olmak üzere [otomatik uyarı](../log-analytics/log-analytics-alerts.md) kuralları ayarlamak Için Azure izleyici günlüklerini yapılandırma