---
title: Operatör en iyi uygulamaları - Azure Kubernetes Hizmetlerinde Küme yalıtımı (AKS)
description: Azure Kubernetes Hizmeti'nde (AKS) yalıtım için küme operatörünün en iyi yalıtım uygulamalarını öğrenin
services: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.openlocfilehash: 228b856d5c5ffa2bfac7df12094667e02f797690
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594864"
---
# <a name="best-practices-for-cluster-isolation-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) küme yalıtımı için en iyi uygulamalar

Azure Kubernetes Hizmeti'nde (AKS) kümeleri yönetirken, genellikle ekipleri ve iş yüklerini yalıtmanız gerekir. AKS, çok kiracılı kümeleri çalıştırma ve kaynakları yalıtma konusunda esneklik sağlar. Kubernetes'e olan yatırımınızı en üst düzeye çıkarmak için, bu çoklu kira ve izolasyon özellikleri anlaşılmalıdır ve uygulanmalıdır.

Bu en iyi uygulamalar makalesi küme işleçleri için yalıtım üzerinde duruluyor. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Çok kiracılı kümeler ve kaynakların ayrılması için plan
> * AKS kümelerinizde mantıksal veya fiziksel yalıtım kullanın

## <a name="design-clusters-for-multi-tenancy"></a>Çoklu kira için tasarım kümeleri

Kubernetes, aynı kümede yer alan ekipleri ve iş yüklerini mantıksal olarak yalıtmanızı sağlayan özellikler sunar. Burada amaç, en az sayıda ayrıcalığı vermek ve kapsamı, ekiplerin ihtiyacı olan kaynaklarla sınırlı tutmaktır. Kubernetes'teki Bir [Ad Alanı][k8s-namespaces] mantıksal bir yalıtım sınırı oluşturur. Ek Kubernetes özellikleri ve izolasyon ve çoklu kira için hususlar aşağıdaki alanları içerir:

* **Zamanlama,** kaynak kotaları ve bölme kesintisi bütçeleri gibi temel özelliklerin kullanımını içerir. Bu özellikler hakkında daha fazla bilgi [için, AKS'deki temel zamanlayıcı özellikleri için en iyi uygulamalara][aks-best-practices-scheduler]bakın.
  * Daha gelişmiş zamanlayıcı özellikleri, leke ve tolerasyonlar, düğüm seçicileri ve düğüm ve pod afinite veya anti-afiity içerir. Bu özellikler hakkında daha fazla bilgi [için, AKS'deki gelişmiş zamanlayıcı özellikleri için en iyi uygulamalara][aks-best-practices-advanced-scheduler]bakın.
* **Ağ,** bölmelere gelen ve çıkan trafik akışını denetlemek için ağ ilkelerinin kullanımını içerir.
* **Kimlik doğrulama ve yetkilendirme,** Azure Anahtar Kasası'nda rol tabanlı erişim denetimi (RBAC) ve Azure Active Directory (AD) tümleştirmesi, bölme kimlikleri ve sırları içerir. Bu özellikler hakkında daha fazla bilgi için [AKS'de kimlik doğrulama ve yetkilendirme için en iyi uygulamalara][aks-best-practices-identity]bakın.
* **Kapsayıcılar** arasında bakla güvenlik ilkeleri, pod güvenlik bağlamları, tarama görüntüleri ve güvenlik açıkları için çalışma süreleri bulunur. Ayrıca, kapsayıcı erişimini temel düğüme kısıtlamak için App Armor veya Seccomp (Secure Computing) kullanmayı da içerir.

## <a name="logically-isolate-clusters"></a>Kümeleri mantıksal olarak yalıtma

**En iyi uygulama kılavuzu** - Takımları ve projeleri ayırmak için mantıksal yalıtım kullanın. Ekipleri veya uygulamaları yalıtmak için dağıttığınız fiziksel AKS kümelerinin sayısını en aza indirmeye çalışın.

Mantıksal yalıtım ile, tek bir AKS kümesi birden çok iş yükü, takım veya ortam için kullanılabilir. Kubernetes [Ad alanları][k8s-namespaces] iş yükleri ve kaynaklar için mantıksal yalıtım sınırını oluşturur.

![AKS'de bir Kubernetes kümesinin mantıksal yalıtımı](media/operator-best-practices-cluster-isolation/logical-isolation.png)

Kümelerin mantıksal olarak ayrılması genellikle fiziksel olarak yalıtılmış kümelerden daha yüksek bir bakla yoğunluğu sağlar. Kümede boşta kalan daha az fazla işlem kapasitesi vardır. Kubernetes küme otomatik ölçekleyicisi ile birleştirildiğinde, talepleri karşılamak için düğüm sayısını yukarı veya aşağı ölçeklendirebilirsiniz. Otomatik ölçekleme için bu en iyi uygulama yaklaşımı, yalnızca gerekli düğüm sayısını çalıştırmanızı sağlar ve maliyetleri en aza indirir.

Kubernetes ortamları, AKS veya başka bir yerde, düşmanca çok kiracı kullanımı için tamamen güvenli değildir. Çok kiracılı bir ortamda, birden çok kiracı ortak, paylaşılan bir altyapı üzerinde çalışmaktadır. Sonuç olarak, tüm kiracılara güvenilemiyorsa, bir kiracının diğerinin güvenliğini ve hizmetini etkilemesini önlemek için ek planlama yapmanız gerekir. *Pod Güvenlik İlkesi* ve düğümler için daha ince taneli rol tabanlı erişim denetimleri (RBAC) gibi ek güvenlik özellikleri, yararlanmaları daha da zorlaştırır. Ancak, düşmanca çok kiracılı iş yüklerini çalıştırırken gerçek güvenlik için, bir hipervizör güvenmeniz gereken tek güvenlik düzeyidir. Kubernetes için güvenlik etki alanı tek bir düğüm değil, tüm küme olur. Bu tür çok kiracılı çok kiracılı iş yükleri için fiziksel olarak yalıtılmış kümeler kullanmalısınız.

## <a name="physically-isolate-clusters"></a>Fiziksel olarak yalıtma kümeleri

**En iyi uygulama kılavuzu** - Her ayrı takım veya uygulama dağıtımı için fiziksel yalıtım kullanımını en aza indirin. Bunun yerine, önceki bölümde belirtildiği gibi *mantıksal* yalıtım kullanın.

Küme yalıtımı için ortak bir yaklaşım fiziksel olarak ayrı AKS kümeleri kullanmaktır. Bu yalıtım modelinde, takımlar veya iş yükleri kendi AKS kümelerine atanır. Bu yaklaşım genellikle iş yüklerini veya ekipleri yalıtmanın en kolay yolu gibi görünür, ancak ek yönetim ve mali ek yük ekler. Şimdi bu birden çok kümeyi korumanız ve tek tek erişim sağlamanız ve izinleri atamanız gerekir. Ayrıca tüm tek tek düğümler için faturalandırılırsınız.

![AKS'de tek tek Kubernetes kümelerinin fiziksel yalıtımı](media/operator-best-practices-cluster-isolation/physical-isolation.png)

Fiziksel olarak ayrı kümeler genellikle düşük pod yoğunluğuna sahiptir. Her takım veya iş yükü kendi AKS kümesine sahip olduğundan, küme genellikle hesaplama kaynaklarıyla aşırı şekilde karşılanır. Genellikle, bu düğümlerde az sayıda bölme zamanlanır. Düğümlerde kullanılmayan kapasite, diğer takımlar tarafından geliştirilmekte olan uygulamalar veya hizmetler için kullanılamaz. Bu fazla kaynaklar, fiziksel olarak ayrı kümelerde ek maliyetlere katkıda bulunur.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede küme yalıtımı üzerinde duruldu. AKS'deki küme işlemleri hakkında daha fazla bilgi için aşağıdaki en iyi uygulamalara bakın:

* [Temel Kubernetes zamanlayıcı özellikleri][aks-best-practices-scheduler]
* [Gelişmiş Kubernetes zamanlayıcı özellikleri][aks-best-practices-advanced-scheduler]
* [Kimlik doğrulama ve yetkilendirme][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->

<!-- INTERNAL LINKS -->
[k8s-namespaces]: concepts-clusters-workloads.md#namespaces
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-identity]: operator-best-practices-identity.md
