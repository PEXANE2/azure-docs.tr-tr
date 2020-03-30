---
title: Kapsayıcılar için Azure Monitör'e Genel Bakış | Microsoft Dokümanlar
description: Bu makalede, AKS Kapsayıcı Öngörüleri çözümünü izleyen kapsayıcılar için Azure Monitor ve Azure'daki AKS kümelerinizin ve Kapsayıcı Örneklerinizin durumunu izleyerek sağladığı değeri açıklar.
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: 3ff2c35ae9f5838447ce90e2a020649427920a43
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275236"
---
# <a name="azure-monitor-for-containers-overview"></a>Kapsayıcılar için Azure İzleyici'ye genel bakış

Kapsayıcılar için Azure Monitör, aşağıdakiler için dağıtılan kapsayıcı iş yüklerinin performansını izlemek üzere tasarlanmış bir özelliktir:

- Azure [Kubernetes Hizmetinde (AKS)](../../aks/intro-kubernetes.md) barındırılan Yönetilen Kubernetes kümeleri
- [AKS Engine](https://github.com/Azure/aks-engine) kullanarak Azure'da barındırılan kendi kendini yöneten Kubernetes kümeleri
- [Azure Container Instances](../../container-instances/container-instances-overview.md)
- [Azure Yığını'nda](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1910) veya şirket içinde barındırılan kendi kendini yöneten Kubernetes kümeleri
- [Azure Red Hat OpenShift](../../openshift/intro-openshift.md)

Kapsayıcılar için Azure Monitor, Linux ve Windows Server 2019 işletim sistemini çalıştıran kümeleri destekler. 

Kapsayıcılarınızı izlemek, özellikle birden çok uygulamayla ölçekte bir üretim kümesi çalıştırıyorsanız çok önemlidir.

Kapsayıcılar için Azure Monitör, Ölçümler API'si aracılığıyla Kubernetes'te bulunan denetleyicilerden, düğümlerden ve kapsayıcılardan bellek ve işlemci ölçümleri toplayarak performans görünürlüğü sağlar. Kapsayıcı günlükleri de toplanır.  Kubernetes kümelerinden izlemeyi etkinleştirdikten sonra, ölçümler ve günlükler, Linux için Log Analytics aracısının konteynerleştirilmiş bir sürümü aracılığıyla sizin için otomatik olarak toplanır. Ölçümler metrik deposuna yazılır ve günlük verileri [Log Analytics](../log-query/log-query-overview.md) çalışma alanınızla ilişkili günlük deposuna yazılır. 

![Kapsayıcı mimarisi için Azure Monitör](./media/container-insights-overview/azmon-containers-architecture-01.png)
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>Kapsayıcılar için Azure Monitor ne sağlar?

Kapsayıcılar için Azure Monitör, Azure Monitor'un farklı özelliklerini kullanarak kapsamlı bir izleme deneyimi sunar. Bu özellikler, Linux ve Windows Server 2019 işletim sistemini çalıştıran Kubernetes kümenizin performansını ve sistem durumunu ve kapsayıcı iş yüklerini anlamanızı sağlar. Kapsayıcılar için Azure Monitor ile şunları yapabilirsiniz:

* Düğüm de çalışan AKS kapsayıcıları ve bunların ortalama işlemci ve bellek kullanımını tanımlayın. Bu bilgi, kaynak darboğazlarını belirlemenize yardımcı olabilir.
* Azure Kapsayıcı Örnekleri'nde barındırılan kapsayıcı gruplarının ve kapsayıcılarının işlemci ve bellek kullanımını tanımlayın.  
* Kapsayıcının denetleyicide veya bölmede nerede bulunduğunu belirleyin. Bu bilgi, denetleyicinin veya bölmenin genel performansını görüntülemenize yardımcı olabilir. 
* Bölmeyi destekleyen standart işlemlerle ilgisi olmayan ana bilgisayarda çalışan iş yüklerinin kaynak kullanımını gözden geçirin.
* Kümenin ortalama ve en ağır yüklerin altında davranışını anlayın. Bu bilgi, kapasite gereksinimlerini belirlemenize ve kümenin sürdürebileceği maksimum yükü belirlemenize yardımcı olabilir. 
* Düğümveya kapsayıcılarda CPU ve bellek kullanımı eşiklerinizi aştığında veya altyapı veya düğüm ler sistem durumu toplama kümesinde bir sistem durumu değişikliği gerçekleştiğinde uyarıları proaktif olarak bildirmek veya kaydetmek için uyarıları yapılandırın.
* Özel uyarılar, panolar ve ayrıntılı analizler gerçekleştirmek için [sorguları](container-insights-log-search.md) kullanarak düğümlerden ve Kubernetes'lerden topladığı uygulama ve iş yükü ölçümlerini görüntülemek için [Prometheus](https://prometheus.io/docs/introduction/overview/) ile tümleştirin.
* Şirket içinde [AKS Engine'e](https://github.com/Azure/aks-engine) ve [Azure Yığını'nda AKS Engine'e](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908)dağıtılan konteyner iş yüklerini izleyin.
* Azure Red [Hat OpenShift'e dağıtılan](../../openshift/intro-openshift.md)kapsayıcı iş yüklerini izleyin.

    >[!NOTE]
    >Azure Red Hat OpenShift desteği, şu anda genel önizlemede bir özelliktir.
    >

Bir Windows Server kümesini bir Linux kümesiyle karşılaştırıldığında izlemedeki temel farklar şunlardır:

- Bellek RSS ölçümü Windows düğümü ve kapsayıcılar için kullanılamaz.
- Disk depolama kapasitesi bilgileri Windows düğümleri için kullanılamaz.
- Kapsayıcı günlükleri Windows düğümlerinde çalışan kapsayıcılar için kullanılamaz.
- Canlı Veri (önizleme) özellik desteği, Windows kapsayıcı günlükleri dışında kullanılabilir.
- Docker ortamları değil, yalnızca pod ortamları izlenir.
- Önizleme sürümüyle en fazla 30 Windows Server kapsayıcısı desteklenir. Bu sınırlama Linux kapsayıcıları için geçerli değildir. 

Kapsayıcılar için Azure Monitor ile AKS kümenizi izleme hakkında bilgi edinmenize yardımcı olmak için ara düzey derin dalış sağlayan aşağıdaki videoya göz atın.

> [!VIDEO https://www.youtube.com/embed/RjsNmapggPU]

## <a name="how-do-i-access-this-feature"></a>Bu özelliğe nasıl erişebilirim?

Kapsayıcılar için Azure Monitor'a Azure Monitor'dan veya doğrudan seçili AKS kümesinden iki şekilde erişebilirsiniz. Azure Monitor'dan, dağıtılan ve izlenen ve olmayan tüm kapsayıcılar hakkında küresel bir bakış açısına sahip siniz ve abonelikleriniz ve kaynak gruplarınızda arama ve filtre leme olanağı sağlar ve ardından seçili kapsayıcı.  Aksi takdirde, özelliğe doğrudan seçili bir AKS kapsayıcısından AKS sayfasından erişebilirsiniz.  

![Kapsayıcılar için Azure Monitör'e erişim yöntemlerine genel bakış](./media/container-insights-overview/azmon-containers-experience.png)

Yapılandırmayı, denetimi ve kaynak kullanımını görüntülemek için AKS dışında çalışan Docker ve Windows kapsayıcı ana bilgisayarlarınızı izlemek ve yönetmekle ilgileniyorsanız, [Kapsayıcı İzleme çözümüne](../../azure-monitor/insights/containers.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes kümenizi izlemeye başlamak için, denetimi etkinleştirmek için gereksinimleri ve kullanılabilir yöntemleri anlamak [için kapsayıcılar için Azure Monitörünü nasıl etkinleştirin](container-insights-onboard.md) irdeleyin. 
