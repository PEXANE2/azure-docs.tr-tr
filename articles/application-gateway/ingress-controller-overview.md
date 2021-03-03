---
title: Azure Application Gateway giriş denetleyicisi nedir?
description: Bu makalede Application Gateway giriş denetleyicisinin ne olduğuna ilişkin bir giriş sunulmaktadır.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 03/02/2021
ms.author: caya
ms.openlocfilehash: 2564fd38056241fd48f58f5f6039bf64f92b6741
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101714417"
---
# <a name="what-is-application-gateway-ingress-controller"></a>Application Gateway giriş denetleyicisi nedir?
Application Gateway giriş denetleyicisi (AGIC), [Azure Kubernetes hizmeti (AKS](https://azure.microsoft.com/services/kubernetes-service/) ) müşterilerinin, bulut yazılımlarını Internet 'e sunmak için Azure 'un yerel [Application Gateway](https://azure.microsoft.com/services/application-gateway/) L7 yük dengeleyicisinden yararlanmasını sağlayan bir Kubernetes uygulamasıdır. AGIC, üzerinde barındırdığı Kubernetes kümesini izler ve bir Application Gateway sürekli olarak güncelleştirir, böylece seçili hizmetler Internet 'e sunulur.

Giriş denetleyicisi, müşterinin AKS 'i üzerinde kendi Pod 'iyle çalışır. AGIC, değişiklikler için Kubernetes kaynaklarının bir alt kümesini izler. AKS kümesinin durumu belirli bir yapılandırmaya Application Gateway çevrilir ve [Azure Resource Manager (ARM)](../azure-resource-manager/management/overview.md)uygulanır.

## <a name="benefits-of-application-gateway-ingress-controller"></a>Application Gateway giriş denetleyicisinin avantajları
AGIC, AKS kümesinin önünde başka bir yük dengeleyici/genel IP 'si olması gereğini ortadan kaldırır ve isteklerin AKS kümesine ulaşması için veri yolunuzda birden çok sıçramayı önler. Application Gateway, özel IP 'leri doğrudan kullanarak ve NodePort ya da KubeProxy Hizmetleri gerektirmeyen bir düğüm ile iletişim yapmaz. Bu, dağıtımlarınız için daha iyi performans da getirir.

Giriş denetleyicisi yalnızca Standard_v2 ve WAF_v2 SKU 'Ları tarafından desteklenir ve bu da otomatik ölçeklendirme avantajları sağlar. Application Gateway, AKS kümenizdeki kaynakları tüketmeksizin trafik yüküyle ilgili bir artış veya azalmaya yanıt verebilir.

AGIC 'e ek olarak Application Gateway kullanmak, TLS ilkesi ve Web uygulaması güvenlik duvarı (WAF) işlevselliği sağlayarak AKS kümenizi korumanıza de yardımcı olur.

![Azure Application Gateway + AKS](./media/application-gateway-ingress-controller-overview/architecture.png)

AGIC, hizmet ve dağıtımlar/pods ile birlikte Kubernetes giriş [kaynağı](https://kubernetes.io/docs/user-guide/ingress/)aracılığıyla yapılandırılır. Azure 'un yerel Application Gateway L7 yük dengeleyicisinden yararlanarak birçok özellik sağlar. Birkaç ad vermek için:
  - URL yönlendirme
  - Tanımlama bilgisi tabanlı benzeşim
  - TLS sonlandırma
  - Uçtan uca TLS
  - Ortak, özel ve karma Web siteleri için destek
  - Tümleşik Web uygulaması güvenlik duvarı

## <a name="difference-between-helm-deployment-and-aks-add-on"></a>Helk dağıtım ve AKS Add-On arasındaki fark
AKS kümeniz için AGIC dağıtmanın iki yolu vardır. İlk yöntem Held ile aynıdır; İkincisi, bir eklenti olarak AKS aracılığıyla yapılır. AGIC 'in bir AKS eklentisi olarak dağıtılmasının birincil avantajı, Held aracılığıyla dağıtmanın çok daha kolay olması. Yeni bir kurulum için, Azure CLı 'de tek bir satırda eklenti olarak etkinleştirilen yeni bir Application Gateway ve yeni bir AKS kümesi dağıtabilirsiniz. Eklenti Ayrıca, otomatik güncelleştirmeler ve artırılmış destek gibi ek avantajlar sağlayan, tam olarak yönetilen bir hizmettir. AGIC (Helm ve AKS eklentisi) dağıtmanın her iki yolu da Microsoft tarafından tam olarak desteklenmektedir. Ek olarak, eklenti, ilk sınıf eklentisi olarak AKS ile daha iyi tümleştirme sağlar.

AGIC eklentisi, müşterinin AKS kümesinde bir pod olarak dağıtılır, ancak helk dağıtım sürümü ve AGIC 'nin eklenti sürümü arasında birkaç farklılık vardır. İki sürüm arasındaki farklılıkların bir listesi aşağıda verilmiştir: 
  - Helk dağıtım değerleri AKS eklentisi üzerinde değiştirilemez:
    - `verbosityLevel` Varsayılan olarak 5 olarak ayarlanacak
    - `usePrivateIp`Varsayılan olarak false olarak ayarlanacak; Bu, [kullanım-özel-IP ek açıklaması](ingress-controller-annotations.md#use-private-ip) ile üzerine yazılabilir
    - `shared` eklenti üzerinde desteklenmez 
    - `reconcilePeriodSeconds` eklenti üzerinde desteklenmez
    - `armAuth.type` eklenti üzerinde desteklenmez
  - Helk aracılığıyla dağıtılan AGIC, ProhibitedTargets 'i destekler, bu da AGIC, diğer mevcut arka uçları etkilemeden AKS kümelerinde özel olarak Application Gateway yapılandırabileceği anlamına gelir. AGIC eklentisi şu anda bunu desteklemiyor. 
  - AGIC eklentisi yönetilen bir hizmet olduğundan, müşteriler agic eklentisinin en son sürümüne otomatik olarak güncelleştirilecek, bu da müşteri 'nin AGC 'yi el ile güncelleştirmesi gereken helk 'ın aksine. 

> [!NOTE]
> Müşteriler, AKS kümesi başına yalnızca bir adet AGIC eklentisi dağıtabilir ve her bir AGIC eklentisi şu anda yalnızca bir Application Gateway hedefleyebilir. Küme başına birden fazla AGIC veya bir Application Gateway hedefleyen birden çok AGI gerektiren dağıtımlar için lütfen Helm aracılığıyla dağıtılan AGIC kullanmaya devam edin. 

## <a name="next-steps"></a>Sonraki adımlar
- [**Aks Add-On doğa alanı dağıtımı**](tutorial-ingress-controller-add-on-new.md): boş tablet ALTYAPıSıNDAN agic eklenti, aks ve Application Gateway yükleme yönergeleri.
- [**Aks Add-On brownfield dağıtımı**](tutorial-ingress-controller-add-on-existing.md): BIR aks kümesine mevcut bir Application Gateway sahip olan agic eklentisini ekleyin.
- [**Helk**](ingress-controller-install-new.md), yenı aks kümesi ve Blank-kurşun altyapısı üzerinde yeni Application Gateway.
- [**Helk brownfield dağıtımı**](ingress-controller-install-existing.md): mevcut bir aks kümesinde Held aracılığıyla AGC 'yi dağıtın ve Application Gateway.