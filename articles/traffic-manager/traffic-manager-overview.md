---
title: Azure Traffic Manager | Microsoft Docs
description: Bu makalede Azure Traffic Manager'a genel bir bakış sağlanmıştır. Uygulamanız için Yük Dengeleme Kullanıcı trafiği için doğru seçenek olup olmadığını öğrenin.
services: traffic-manager
author: duongau
ms.service: traffic-manager
customer intent: As an IT admin, I want to learn about Traffic Manager and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/19/2021
ms.author: duau
ms.openlocfilehash: 8c8897218b153c8584c89abab98934268ccd555d
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102182171"
---
# <a name="what-is-traffic-manager"></a>Traffic Manager nedir?
Azure Traffic Manager, DNS tabanlı bir trafik yük dengeleyicidir. Bu hizmet, genel Azure bölgelerinde genel kullanıma açık uygulamalarınıza giden trafiği dağıtmanıza olanak tanır. Traffic Manager ayrıca, yüksek kullanılabilirlik ve hızlı yanıt verme ile genel uç noktalarınız sağlar.

Traffic Manager, istemci isteklerini bir trafik yönlendirme yöntemine göre uygun hizmet uç noktasına yönlendirmek için DNS kullanır. Traffic Manager ayrıca her uç nokta için sistem durumu izleme sağlar. Uç nokta, Azure 'un içinde veya dışında barındırılan Internet 'e yönelik herhangi bir hizmet olabilir. Traffic Manager, farklı uygulama ihtiyaçlarına ve otomatik yük devretme modellerine uyan farklı [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) ve [uç nokta izleme seçenekleri](traffic-manager-monitoring.md) sunar. Traffic Manager, bir Azure bölgesinin tamamının devre dışı kalması dahil olmak üzere hatalara dayanıklıdır.

>[!NOTE]
> Azure, senaryolarınız için tam olarak yönetilen yük dengeleme çözümleri sunar. 
> * Uygulama katmanındaki bir bölgedeki sunucularınız arasında yük dengelemesi yapmak istiyorsanız [Application Gateway](../application-gateway/overview.md)gözden geçirin.
> * Web trafiğinizi küresel yönlendirmeyi iyileştirmeniz ve hızlı genel yük devretme ile en üst katman Son Kullanıcı performansını ve güvenilirliğini iyileştirmek için bkz. [ön kapı](../frontdoor/front-door-overview.md).
> * Ağ katmanı yük dengelemesi yapmak için [Load Balancer](../load-balancer/load-balancer-overview.md)gözden geçirin. 
> 
> Uçtan uca senaryolarınız, bu çözümlerin gerektiğinde birleştirilmesinin avantajlarından yararlanabilir.
> Azure yük dengeleme seçenekleri karşılaştırması için bkz. [Azure 'da Yük Dengeleme seçeneklerine genel bakış](/azure/architecture/guide/technology-choices/load-balancing-overview).

Traffic Manager aşağıdaki özellikleri sunar:

## <a name="increase-application-availability"></a>Uygulama kullanılabilirliğini artırma

Traffic Manager, uç noktalarınızı izleyerek ve uç nokta kullanım dışı kaldığında otomatik yük devretme sağlayarak kritik uygulamalarını için yüksek kullanılabilirlik sağlar.
    
## <a name="improve-application-performance"></a>Uygulama performansını geliştirme

Azure, dünyanın dört bir yanındaki veri merkezlerinde bulut hizmetleri ve Web siteleri çalıştırmanızı sağlar. Traffic Manager, en düşük gecikme süresine sahip trafiği uç noktaya yönlendirerek Web sitenizin yanıt hızını iyileştirebilir.

## <a name="service-maintenance-without-downtime"></a>Kesinti olmadan hizmet Bakımı

Kapalı kalma süresi olmadan uygulamalarınız üzerinde planlı bakım yapmış olabilirsiniz. Traffic Manager, bakım devam ederken trafiği alternatif uç noktalara yönlendirebilir.

## <a name="combine-hybrid-applications"></a>Hibrit uygulamaları birleştirme

Traffic Manager harici, Azure dışı uç noktalar için sunduğu destek sayesinde "[buluta veri bloğu aktarma](https://azure.microsoft.com/overview/what-is-cloud-bursting/)," "buluta geçiş" ve "buluta devretme" senaryoları gibi hibrit bulut ve şirket içi dağıtımlarda kullanılabilir.

## <a name="distribute-traffic-for-complex-deployments"></a>Karmaşık dağıtımlar için trafiği dağıtma

[İç içe Traffic Manager profillerini](traffic-manager-nested-profiles.md)kullanarak birden çok trafik yönlendirme yöntemi, daha büyük ve daha karmaşık dağıtımlar ihtiyaçlarına göre ölçeklendirilebilecek gelişmiş ve esnek kurallar oluşturmak için birleştirilebilir.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri için bkz. [Traffic Manager Fiyatlandırması](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Sonraki adımlar

- [Traffic Manager profili oluşturmayı](./quickstart-create-traffic-manager-profile.md) öğrenin.
- [Traffic Manager'ın nasıl çalıştığını](traffic-manager-how-it-works.md) öğrenin.
- Traffic Manager hakkında [sık sorulan soruları](traffic-manager-FAQs.md) görüntüleyin.