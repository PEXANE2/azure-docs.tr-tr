---
title: Azure yay bulutu coğrafi-olağanüstü durum kurtarma | Microsoft Docs
description: Spring Cloud uygulamanızı bölgesel kesintilerden nasıl koruyacağınızı öğrenin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: brendm
ms.openlocfilehash: 4961e5a63e5bc1933cf19b1f291b521d89cbda0e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76279146"
---
# <a name="azure-spring-cloud-disaster-recovery"></a>Azure yay bulut olağanüstü durum kurtarma

Bu makalede, Azure Spring Cloud uygulamalarınızı kapalı kalma süresi yaşamadan korumak için kullanabileceğiniz bazı stratejiler açıklanmaktadır.  Bölgesel felaketlerden kaynaklanan hiçbir bölge veya veri merkezi kesinti yaşar, ancak dikkatli bir planlama, müşterilerinizin etkilerini hafifletmeye başlayabilir.

## <a name="plan-your-application-deployment"></a>Uygulama dağıtımınızı planlayın

Azure yay bulut uygulamaları belirli bir bölgede çalışır.  Azure, dünyanın dört bir yanındaki birden fazla coğrafyada çalışmaktadır. Azure coğrafya, en az bir Azure bölgesi içeren, dünyanın tanımlı bir alanıdır. Bir Azure bölgesi, bir veya daha fazla veri merkezi içeren coğrafya içindeki bir alandır.  Her Azure bölgesi aynı coğrafya içindeki başka bir bölge ile eşleştirilir ve bir bölgesel çift oluşturur. Azure, her bir çiftin tek seferde yalnızca bir bölgenin güncelleştirildiğinden emin olmak için, bölgesel çiftler genelinde platform güncelleştirmelerini (planlı bakım) seri hale getirir. Birden çok bölgeyi etkileyen kesinti durumunda, her bir çiftin en az bir bölgesine kurtarma için öncelik alınacaktır.

Olağanüstü durumlarda yüksek kullanılabilirlik ve koruma sağlamak için, Spring Cloud uygulamalarınızı birden çok bölgeye dağıtmanız gerekir.  Azure, Spring Cloud dağıtımlarını bölgesel çiftlerine planlayabilmeniz için [eşleştirilmiş bölgelerin](../best-practices-availability-paired-regions.md) bir listesini sağlar.  Mikro hizmet mimarinizi tasarlarken üç temel faktörleri göz önünde bulundurmanız önerilir: bölge kullanılabilirliği, Azure eşleştirilmiş bölgeler ve hizmet kullanılabilirliği.

*  Bölge kullanılabilirliği: ağ gecikmesini ve iletim süresini en aza indirmek için kullanıcılarınıza yakın bir coğrafi alan seçin.
*  Azure eşlenmiş bölgeler: ihtiyacınız olursa, koordine edilen platform güncelleştirmelerini ve önceliklendirilmiş kurtarma çalışmalarını sağlamak için seçtiğiniz coğrafi alan dahilinde eşleştirilmiş bölgeleri seçin.
*  Hizmet kullanılabilirliği: eşleştirilmiş bölgelerin Hot/Hot, Hot/sıcak veya sıcak/soğuk çalışmasına karar verin.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Trafiği yönlendirmek için Azure Traffic Manager kullanma

[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) , DNS tabanlı trafik yük dengelemesi sağlar ve ağ trafiğini birden çok bölgeye dağıtabilir.  Müşterileri, en yakın Azure Spring Cloud Service örneğine yönlendirmek için Azure Traffic Manager kullanın.  En iyi performans ve artıklık için, Azure Spring Cloud Service 'e göndermeden önce tüm uygulama trafiğini Azure Traffic Manager aracılığıyla doğrudan yönlendirin.

Birden çok bölgede Azure Spring Cloud uygulamalarınız varsa, her bölgede uygulamalarınıza giden trafik akışını denetlemek için Azure Traffic Manager kullanın.  Hizmet IP 'sini kullanarak her hizmet için bir Azure Traffic Manager uç noktası tanımlayın. Müşteriler Azure Spring Cloud Service ' i işaret eden bir Azure Traffic Manager DNS adına bağlanmalıdır.  Azure Traffic Manager yük, trafiği tanımlanan uç noktalar arasında dengeler.  Bir olağanüstü durum veri merkezini ortaya çıkardığında, Azure Traffic Manager, trafiği bu bölgeden çiftine yönlendirirler ve hizmet devamlılığını sağlar.