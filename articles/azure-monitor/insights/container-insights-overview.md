---
title: Kapsayıcılar için Azure İzleyicisi'nin genel bakış | Microsoft Docs
description: Bu makalede, AKS kapsayıcı öngörüleri çözümüdür ve değer AKS kümeleri ve Azure Container Instances'da durumunu izleyerek sunar izleyen kapsayıcılar için Azure İzleyici açıklanır.
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: 3ff2c35ae9f5838447ce90e2a020649427920a43
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198046"
---
# <a name="azure-monitor-for-containers-overview"></a>Kapsayıcılara genel bakış için Azure İzleyici

Kapsayıcılar için Azure Izleyici, ' ye dağıtılan kapsayıcı iş yüklerinin performansını izlemek için tasarlanmış bir özelliktir:

- [Azure Kubernetes Service (AKS)](../../aks/intro-kubernetes.md) üzerinde barındırılan yönetilen Kubernetes kümeleri
- [Aks altyapısı](https://github.com/Azure/aks-engine) kullanılarak Azure üzerinde barındırılan, kendi kendine yönetilen Kubernetes kümeleri
- [Azure Container Instances](../../container-instances/container-instances-overview.md)
- [Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1910) veya şirket içinde barındırılan, kendi kendine yönetilen Kubernetes kümeleri
- [Azure Red Hat OpenShift](../../openshift/intro-openshift.md)

Kapsayıcılar için Azure Izleyici, Linux ve Windows Server 2019 işletim sistemi çalıştıran kümeleri destekler. 

Özellikle birden çok uygulama ile ölçekli olarak bir üretim kümesi çalıştırırken, kapsayıcılarınızın izlenmesi kritik önem taşır.

Kapsayıcılar için Azure İzleyici denetleyicileri, düğümleri ve Kubernetes ölçümler API aracılığıyla kullanılabilir olan kapsayıcıları bellekten toplanması ve işlemci ölçümleri performans görünürlük sağlar. Kapsayıcı günlükleri de toplanır.  Kubernetes kümelerinden izlemeyi etkinleştirdikten sonra, ölçümler ve Günlükler, Linux için Log Analytics aracısının kapsayıcılı bir sürümü aracılığıyla sizin için otomatik olarak toplanır. Ölçümler ölçüm deposuna yazılır ve günlük verileri [Log Analytics](../log-query/log-query-overview.md) çalışma alanıyla ilişkili Günlükler deposuna yazılır. 

![Kapsayıcılar için Azure Izleyici mimarisi](./media/container-insights-overview/azmon-containers-architecture-01.png)
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>Kapsayıcıları sağlamak için Azure İzleyici yapar?

Kapsayıcılar için Azure Izleyici, Azure Izleyici 'nin farklı özelliklerini kullanarak kapsamlı bir izleme deneyimi sağlar. Bu özellikler, Linux ve Windows Server 2019 işletim sistemi ve kapsayıcı iş yüklerini çalıştıran Kubernetes kümenizin performansını ve sistem durumunu anlamanıza olanak tanır. Kapsayıcılar için Azure Izleyici ile şunları yapabilirsiniz:

* Düğüm ve bunların ortalama işlemci ve bellek kullanımı çalışan AKS kapsayıcı belirleyin. Bu bilgi, kaynak darboğazları belirlemenize yardımcı olabilir.
* Kapsayıcı grupları ve Azure Container Instances'da barındırılan kapsayıcılarına işlemci ve bellek kullanımı belirleyin.  
* Kapsayıcının bir denetleyicide veya Pod içinde nerede olduğunu belirler. Bu bilgi, denetleyicinin ya da pod'ın genel performansını görüntülemenize yardımcı olabilir. 
* Pod destekleyen standart işlemlere ilgisiz, ana bilgisayarda çalışan iş yüklerini kaynak kullanımını gözden geçirin.
* Ortalama ve en yoğun iş yükü altında kümeye davranışını anlayın. Bu bilgi, kapasite gereksinimlerini tanımlama ve kümenin karşılayabileceği en fazla yükü belirlemek yardımcı olabilir. 
* Düğümleri veya kapsayıcılardaki CPU ve bellek kullanımı eşiklerinizi aştığında ya da altyapı ya da düğümler sistem durumu toplamakta olan kümede bir sistem durumu değişikliği gerçekleştiğinde sizi önceden bilgilendirmesi veya kaydetmek için uyarıları yapılandırın.
* Özel uyarılar, panolar oluşturmak ve ayrıntılı analiz gerçekleştirmek için [sorguları](container-insights-log-search.md) kullanarak düğümlerden ve Kubernetes tarafından toplanan uygulama ve iş yükü ölçümlerini görüntülemek Için [Prometheus](https://prometheus.io/docs/introduction/overview/) ile tümleştirin.
* Şirket içi ve [aks altyapısına Azure Stack üzerinde](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908) [dağıtılan](https://github.com/Azure/aks-engine) kapsayıcı iş yüklerini izleyin.
* [Azure Red Hat OpenShift 'e dağıtılan](../../openshift/intro-openshift.md)kapsayıcı iş yüklerini izleyin.

    >[!NOTE]
    >Azure Red Hat OpenShift desteği şu anda genel önizlemede bir özelliktir.
    >

Bir Windows Server kümesini bir Linux kümesiyle karşılaştırılan izlemedeki başlıca farklar şunlardır:

- Bellek RSS ölçümü Windows düğüm ve kapsayıcıları için kullanılamaz.
- Disk depolama kapasitesi bilgileri Windows düğümleri için kullanılamaz.
- Kapsayıcı günlükleri Windows düğümlerinde çalışan kapsayıcılar için kullanılamaz.
- Canlı veriler (Önizleme) özelliği desteği, Windows kapsayıcı günlükleri dışında bulunur.
- Docker ortamları değil yalnızca Pod ortamları izlenir.
- Önizleme sürümü ile en fazla 30 Windows Server kapsayıcısı desteklenir. Bu sınırlama, Linux kapsayıcıları için geçerlidir. 

AKS kümenizi kapsayıcılar için Azure Izleyici ile izleme hakkında bilgi edinmenize yardımcı olmak için, aşağıdaki videoya göz atın.

> [!VIDEO https://www.youtube.com/embed/RjsNmapggPU]

## <a name="how-do-i-access-this-feature"></a>Bu özelliği nasıl erişim sağlanır?

İki yolu, seçilen AKS kümesini Azure İzleyici ya da doğrudan kapsayıcılar için Azure İzleyici erişebilirsiniz. Azure Izleyici 'den, izlenen ve olmayan tüm kapsayıcılardan oluşan küresel bir perspektife sahipsiniz, bu, abonelikleriniz ve kaynak gruplarınız üzerinde arama ve filtreleme olanağı sağlar ve ardından Azure Izleyici 'de kapsayıcılar için Seçili kapsayıcı.  Aksi takdirde, özelliğe doğrudan, AKS sayfasından seçilen bir AKS kapsayıcısından erişebilirsiniz.  

![Kapsayıcılar için Azure İzleyici erişim yöntemlerine genel bakış](./media/container-insights-overview/azmon-containers-experience.png)

Yapılandırma, denetim ve kaynak kullanımını görüntülemek için AKS dışında çalışan Docker ve Windows kapsayıcı konaklarınızı izlemeye ve yönetmeye ilgileniyorsanız, bkz. [kapsayıcı izleme çözümü](../../azure-monitor/insights/containers.md).

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes kümenizi izlemeye başlamak için, izlemeyi etkinleştirmek üzere gereksinimleri ve kullanılabilir yöntemleri anlamak üzere [kapsayıcılar Için Azure Izleyicisini nasıl etkinleştireceğinizi](container-insights-onboard.md) gözden geçirin. 
