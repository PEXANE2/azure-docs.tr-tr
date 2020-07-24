---
title: Kapsayıcılar için Azure Izleyicisine genel bakış | Microsoft Docs
description: Bu makalede AKS kapsayıcı öngörüleri çözümünü izleyen kapsayıcılar için Azure Izleyici ve AKS kümelerinizin sistem durumunu ve Azure 'daki Container Instances izleyerek sunduğu değer açıklanmaktadır.
ms.topic: conceptual
ms.date: 05/21/2020
ms.openlocfilehash: c5fcdd3bfc42e298791e1ab761fa4db432a93dd7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091173"
---
# <a name="azure-monitor-for-containers-overview"></a>Kapsayıcılar için Azure İzleyici’ye genel bakış

Kapsayıcılar için Azure Izleyici, ' ye dağıtılan kapsayıcı iş yüklerinin performansını izlemek için tasarlanmış bir özelliktir:

- [Azure Kubernetes Service (AKS)](../../aks/intro-kubernetes.md) üzerinde barındırılan yönetilen Kubernetes kümeleri
- [Aks altyapısı](https://github.com/Azure/aks-engine) kullanılarak Azure üzerinde barındırılan, kendi kendine yönetilen Kubernetes kümeleri
- [Azure Container Instances](../../container-instances/container-instances-overview.md)
- [Azure Stack](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1910) veya şirket içinde barındırılan, kendi kendine yönetilen Kubernetes kümeleri
- [Azure Red Hat OpenShift](../../openshift/intro-openshift.md)
- [Azure Arc etkin Kubernetes](../../azure-arc/kubernetes/overview.md) (Önizleme)

Kapsayıcılar için Azure Izleyici, Linux ve Windows Server 2019 işletim sistemi çalıştıran kümeleri destekler.

Kapsayıcılarınızın izlenmesi, özellikle de bir üretim kümesini birden çok uygulamayla birlikte çalışırken kritik öneme sahiptir.

Kapsayıcılar için Azure Izleyici, ölçüm API 'SI aracılığıyla Kubernetes 'te bulunan denetleyicilerden, düğümlerden ve kapsayıcılardan bellek ve işlemci ölçümleri toplayarak performans görünürlüğüne sahip olmanızı sağlar. Kapsayıcı günlükleri de toplanır.  Kubernetes kümelerinden izlemeyi etkinleştirdikten sonra, ölçümler ve Günlükler, Linux için Log Analytics aracısının kapsayıcılı bir sürümü aracılığıyla sizin için otomatik olarak toplanır. Ölçümler ölçüm deposuna yazılır ve günlük verileri [Log Analytics](../log-query/log-query-overview.md) çalışma alanıyla ilişkili Günlükler deposuna yazılır.

![Kapsayıcılar için Azure Izleyici mimarisi](./media/container-insights-overview/azmon-containers-architecture-01.png)

## <a name="what-does-azure-monitor-for-containers-provide"></a>Kapsayıcılar için Azure Izleyici ne sağlar?

Kapsayıcılar için Azure Izleyici, Azure Izleyici 'nin farklı özelliklerini kullanarak kapsamlı bir izleme deneyimi sağlar. Bu özellikler, Linux ve Windows Server 2019 işletim sistemi ve kapsayıcı iş yüklerini çalıştıran Kubernetes kümenizin performansını ve sistem durumunu anlamanıza olanak tanır. Kapsayıcılar için Azure Izleyici ile şunları yapabilirsiniz:

* Düğüm üzerinde çalışan AKS kapsayıcılarını ve bunların ortalama işlemci ve bellek kullanımını belirler. Bu bilgi, kaynak performans sorunlarını belirlemenize yardımcı olabilir.
* Kapsayıcı gruplarının ve Azure Container Instances barındırılan kapsayıcılarının işlemci ve bellek kullanımını belirler.
* Kapsayıcının bir denetleyicide veya Pod içinde nerede olduğunu belirler. Bu bilgi, denetleyicinin veya Pod 'un genel performansını görüntülemenize yardımcı olabilir.
* Pod 'u destekleyen standart süreçlerle ilgisi olmayan konakta çalışan iş yüklerinin kaynak kullanımını gözden geçirin.
* Kümenin davranışını ortalama ve en ağır yüklerle anlayın. Bu bilgi, kapasite gereksinimlerini belirlemenize ve kümenin hangi yük için uygun olduğunu belirlemenize yardımcı olabilir.
* Düğümleri veya kapsayıcılardaki CPU ve bellek kullanımı eşiklerinizi aştığında ya da altyapı ya da düğümler sistem durumu toplamakta olan kümede bir sistem durumu değişikliği gerçekleştiğinde sizi önceden bilgilendirmesi veya kaydetmek için uyarıları yapılandırın.
* Özel uyarılar, panolar oluşturmak ve ayrıntılı analiz gerçekleştirmek için [sorguları](container-insights-log-search.md) kullanarak düğümlerden ve Kubernetes tarafından toplanan uygulama ve iş yükü ölçümlerini görüntülemek Için [Prometheus](https://prometheus.io/docs/introduction/overview/) ile tümleştirin.
* Şirket içi ve [aks altyapısına Azure Stack üzerinde](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908) [dağıtılan](https://github.com/Azure/aks-engine) kapsayıcı iş yüklerini izleyin.
* [Azure Red Hat OpenShift 'e dağıtılan](../../openshift/intro-openshift.md)kapsayıcı iş yüklerini izleyin.

    >[!NOTE]
    >Azure Red Hat OpenShift desteği şu anda genel önizlemede bir özelliktir.
    >

* [Azure Arc etkin Kubernetes (Önizleme) için dağıtılan](../../azure-arc/kubernetes/overview.md)kapsayıcı iş yüklerini izleyin.

Bir Windows Server kümesini bir Linux kümesiyle karşılaştırılan izlemedeki başlıca farklar şunlardır:

- Windows 'un bellek RSS ölçümü yoktur ve bu nedenle Windows düğüm ve kapsayıcıları için kullanılamaz. [Çalışma kümesi](/windows/win32/memory/working-set) ölçümü kullanılabilir.
- Disk depolama kapasitesi bilgileri Windows düğümleri için kullanılamaz.
- Docker ortamları değil yalnızca Pod ortamları izlenir.
- Önizleme sürümü ile en fazla 30 Windows Server kapsayıcısı desteklenir. Bu sınırlama, Linux kapsayıcıları için geçerlidir.

AKS kümenizi kapsayıcılar için Azure Izleyici ile izleme hakkında bilgi edinmenize yardımcı olmak için, aşağıdaki videoya göz atın.

> [!VIDEO https://www.youtube.com/embed/RjsNmapggPU]

## <a name="how-do-i-access-this-feature"></a>Bu özelliğe erişmek Nasıl yaparım??

Azure izleyici 'yi Azure Izleyici 'den veya doğrudan seçilen AKS kümesinden iki yolla kapsayıcılar için Azure Izleyicisine erişebilirsiniz. Azure Izleyici 'den, izlenen ve olmayan tüm kapsayıcılardan oluşan küresel bir perspektife sahipsiniz, bu, abonelikleriniz ve kaynak gruplarınız üzerinde arama ve filtreleme olanağı sağlar ve ardından seçili kapsayıcıdan kapsayıcılar için Azure Izleyici 'ye gidebilir.  Aksi takdirde, özelliğe doğrudan, AKS sayfasından seçilen bir AKS kapsayıcısından erişebilirsiniz.

![Kapsayıcılar için Azure Izleyicisine erişme yöntemlerine genel bakış](./media/container-insights-overview/azmon-containers-experience.png)

Yapılandırma, denetim ve kaynak kullanımını görüntülemek için AKS dışında çalışan Docker ve Windows kapsayıcı konaklarınızı izlemeye ve yönetmeye ilgileniyorsanız, bkz. [kapsayıcı izleme çözümü](../../azure-monitor/insights/containers.md).

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes kümenizi izlemeye başlamak için, izlemeyi etkinleştirmek üzere gereksinimleri ve kullanılabilir yöntemleri anlamak üzere [kapsayıcılar Için Azure Izleyicisini nasıl etkinleştireceğinizi](container-insights-onboard.md) gözden geçirin.
