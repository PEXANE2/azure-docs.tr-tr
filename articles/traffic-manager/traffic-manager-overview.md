---
title: Azure Traffic Manager | Microsoft Docs
description: Bu makalede Azure Traffic Manager'a genel bir bakış sağlanmıştır. Uygulamanızın kullanıcı trafiğine yük dengeleme uygulamak için doğru seçenek olup olmadığına karar verin.
services: traffic-manager
author: rohinkoul
manager: twooley
ms.service: traffic-manager
customer intent: As an IT admin, I want to learn about Traffic Manager and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/23/2019
ms.author: rohink
ms.openlocfilehash: 6eb1ce18f3bc6674efd5343306fff617bc9cd1f7
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80060794"
---
# <a name="what-is-traffic-manager"></a>Traffic Manager nedir?
Azure Traffic Manager, trafiği farklı Azure bölgelerindeki hizmetlere en uygun şekilde dağıtırken yüksek kullanılabilirlik ve yanıtlama hızı sağlayan DNS tabanlı bir trafik yük dengeleyicidir.

Traffic Manager, trafik yönlendirme yöntemine ve uç noktaların sistem durumuna bağlı olarak istemci isteklerini en uygun hizmet uç noktasına yönlendirmek için DNS hizmetini kullanır. Uç nokta, Azure içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir. Traffic Manager, farklı uygulama ihtiyaçlarına ve otomatik yük devretme modellerine uyan farklı [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) ve [uç nokta izleme seçenekleri](traffic-manager-monitoring.md) sunar. Traffic Manager, bir Azure bölgesinin tamamının devre dışı kalması dahil olmak üzere hatalara dayanıklıdır.

>[!NOTE]
> Azure, senaryolarınız için tam olarak yönetilen yük dengeleme çözümleri sunar. Aktarım Katmanı Güvenliği (TLS) protokolü sonlandırma ("SSL yük boşaltma") veya HTTP/HTTPS isteği başına uygulama katmanı işleme özellikleri istiyorsanız [Application Gateway](../application-gateway/application-gateway-introduction.md)'i inceleyin. Bölgesel yük dengeleme arıyorsanız, Yük [Dengeleyici'ni](../load-balancer/load-balancer-overview.md)gözden geçirin. Uçtan uca senaryolarınızda bu çözümleri bir arada da kullanabilirsiniz.
>
> Azure yük dengeleme seçenekleri karşılaştırması için [Azure'da yük dengeleme seçeneklerine genel bakış'a](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview)bakın.

Trafik Yöneticisi aşağıdaki özellikleri sunar:

## <a name="increase-application-availability"></a>Uygulama kullanılabilirliğini artırma

Traffic Manager, uç noktalarınızı izleyerek ve uç nokta kullanım dışı kaldığında otomatik yük devretme sağlayarak kritik uygulamalarını için yüksek kullanılabilirlik sağlar.
    
## <a name="improve-application-performance"></a>Uygulama performansını geliştirme

Azure, dünyanın dört bir yanında bulunan veri merkezlerinde bulut hizmeti veya web sitesi çalıştırmanıza olanak tanır. Traffic Manager, trafiği istemci için en düşük ağ gecikme süresine sahip trafiğe yönlendirerek uygulama yanıt süresini kısaltır.

## <a name="perform-service-maintenance-without-downtime"></a>Kesinti süresi olmadan hizmet bakımı gerçekleştirme

Kesinti süresi yaşamadan uygulamalarınızda planlı bakım çalışmaları yürütebilirsiniz. Trafik Yöneticisi, bakım devam ederken trafiği alternatif uç noktalara yönlendirebilir.

## <a name="combine-hybrid-applications"></a>Hibrit uygulamaları birleştirme

Traffic Manager harici, Azure dışı uç noktalar için sunduğu destek sayesinde "[buluta veri bloğu aktarma](https://azure.microsoft.com/overview/what-is-cloud-bursting/)," "buluta geçiş" ve "buluta devretme" senaryoları gibi hibrit bulut ve şirket içi dağıtımlarda kullanılabilir.

## <a name="distribute-traffic-for-complex-deployments"></a>Karmaşık dağıtımlar için trafiği dağıtma

[İç içe trafik yöneticisi profilleri](traffic-manager-nested-profiles.md)kullanılarak, daha büyük, daha karmaşık dağıtımların gereksinimlerine ölçeklendirmek için karmaşık ve esnek kurallar oluşturmak için birden çok trafik yönlendirme yöntemi birleştirilebilir.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri için bkz. [Traffic Manager Fiyatlandırması](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Sonraki adımlar

- [Traffic Manager profili oluşturmayı](traffic-manager-create-profile.md) öğrenin.
- [Traffic Manager'ın nasıl çalıştığını](traffic-manager-how-it-works.md) öğrenin.
- Traffic Manager hakkında [sık sorulan soruları](traffic-manager-FAQs.md) görüntüleyin.




