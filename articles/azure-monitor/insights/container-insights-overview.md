---
title: Kapsayıcılar için Azure İzleyicisi'nin genel bakış | Microsoft Docs
description: Bu makalede, AKS kapsayıcı öngörüleri çözümüdür ve değer AKS kümeleri ve Azure Container Instances'da durumunu izleyerek sunar izleyen kapsayıcılar için Azure İzleyici açıklanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2019
ms.author: magoedte
ms.openlocfilehash: e1fca1532352c4955f478df70f818ee3151053c1
ms.sourcegitcommit: d060947aae93728169b035fd54beef044dbe9480
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68742199"
---
# <a name="azure-monitor-for-containers-overview"></a>Kapsayıcılara genel bakış için Azure İzleyici

Kapsayıcılar için Azure İzleyici, Azure Container Instances'ı veya Azure Kubernetes Service (AKS) barındırılan yönetilen Kubernetes kümeleri için Dağıtılmış bir kapsayıcı iş yüklerinin performansını izlemek için tasarlanmış bir özelliktir. Özellikle birden çok uygulama ile ölçekli olarak bir üretim kümesi çalıştırırken, kapsayıcılarınızın izlenmesi kritik önem taşır.

Kapsayıcılar için Azure İzleyici denetleyicileri, düğümleri ve Kubernetes ölçümler API aracılığıyla kullanılabilir olan kapsayıcıları bellekten toplanması ve işlemci ölçümleri performans görünürlük sağlar. Kapsayıcı günlükleri de toplanır.  Kubernetes kümelerinden izlemeyi etkinleştirdikten sonra, ölçümler ve Günlükler, Linux için Log Analytics aracısının kapsayıcılı bir sürümü aracılığıyla sizin için otomatik olarak toplanır. Ölçümler ölçüm deposuna yazılır ve günlük verileri [Log Analytics](../log-query/log-query-overview.md) çalışma alanıyla ilişkili Günlükler deposuna yazılır. 

![Kapsayıcılar için Azure Izleyici mimarisi](./media/container-insights-overview/azmon-containers-architecture-01.png)
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>Kapsayıcıları sağlamak için Azure İzleyici yapar?

Kapsayıcılar için Azure Izleyici, Azure Izleyici 'nin farklı özelliklerini kullanarak kapsamlı bir izleme deneyimi sunarak, Kubernetes kümenizin ve kapsayıcı iş yüklerinizin performansını ve durumunu anlamanıza olanak sağlar. Kapsayıcılar için Azure Izleyici ile şunları yapabilirsiniz:

* Düğüm ve bunların ortalama işlemci ve bellek kullanımı çalışan AKS kapsayıcı belirleyin. Bu bilgi, kaynak darboğazları belirlemenize yardımcı olabilir.
* Kapsayıcı grupları ve Azure Container Instances'da barındırılan kapsayıcılarına işlemci ve bellek kullanımı belirleyin.  
* Kapsayıcının bir denetleyicide veya Pod içinde nerede olduğunu belirler. Bu bilgi, denetleyicinin ya da pod'ın genel performansını görüntülemenize yardımcı olabilir.
* Pod destekleyen standart işlemlere ilgisiz, ana bilgisayarda çalışan iş yüklerini kaynak kullanımını gözden geçirin.
* Ortalama ve en yoğun iş yükü altında kümeye davranışını anlayın. Bu bilgi, kapasite gereksinimlerini tanımlama ve kümenin karşılayabileceği en fazla yükü belirlemek yardımcı olabilir. 
* Düğümleri veya kapsayıcılardaki CPU ve bellek kullanımı eşiklerinizi aştığında, sizi önceden bilgilendirmesi veya kayıt yapmanız için uyarıları yapılandırın.
* Özel uyarılar, panolar oluşturmak ve ayrıntılı analiz gerçekleştirmek için [sorguları](container-insights-log-search.md) kullanarak düğümlerden ve Kubernetes tarafından toplanan uygulama ve iş yükü ölçümlerini görüntülemek Için [Prometheus](https://prometheus.io/docs/introduction/overview/) ile tümleştirin.

    >[!NOTE]
    >Prometheus desteği şu anda genel önizlemede bir özelliktir.
    >

* [AKS-Engine 'e dağıtılan](https://github.com/microsoft/OMS-docker/tree/aks-engine)kapsayıcı iş yüklerini izleyin.

AKS kümenizi kapsayıcılar için Azure Izleyici ile izleme hakkında bilgi edinmenize yardımcı olmak için, aşağıdaki videoya göz atın.

> [!VIDEO https://www.youtube.com/embed/RjsNmapggPU]

## <a name="how-do-i-access-this-feature"></a>Bu özelliği nasıl erişim sağlanır?

İki yolu, seçilen AKS kümesini Azure İzleyici ya da doğrudan kapsayıcılar için Azure İzleyici erişebilirsiniz. Azure Izleyici 'den, izlenen ve olmayan tüm kapsayıcılardan oluşan küresel bir perspektife sahipsiniz, bu, abonelikleriniz ve kaynak gruplarınız üzerinde arama ve filtreleme olanağı sağlar ve ardından Azure Izleyici 'de kapsayıcılar için Seçili kapsayıcı.  Aksi takdirde, özelliğe doğrudan, AKS sayfasından seçilen bir AKS kapsayıcısından erişebilirsiniz.  

![Kapsayıcılar için Azure İzleyici erişim yöntemlerine genel bakış](./media/container-insights-overview/azmon-containers-experience.png)

Yapılandırma, denetim ve kaynak kullanımını görüntülemek için AKS dışında çalışan Docker ve Windows kapsayıcı konaklarınızı izlemeye ve yönetmeye ilgileniyorsanız, bkz. [kapsayıcı izleme çözümü](../../azure-monitor/insights/containers.md).

## <a name="next-steps"></a>Sonraki adımlar

AKS kümenizi izlemeye başlamak için, izlemeyi etkinleştirmek üzere gereksinimleri ve kullanılabilir yöntemleri anlamak üzere [kapsayıcılar Için Azure Izleyicisini nasıl etkinleştireceğinizi](container-insights-onboard.md) gözden geçirin.  
