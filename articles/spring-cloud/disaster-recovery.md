---
title: Azure yay bulutu coğrafi-olağanüstü durum kurtarma | Microsoft Docs
description: Spring Cloud uygulamanızı bölgesel kesintilerden nasıl koruyacağınızı öğrenin
services: spring-cloud
author: jpconnock
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: jeconnoc
ms.openlocfilehash: 4752cba6081d376bd006406a154b9b182c6a3a72
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164971"
---
# <a name="azure-spring-cloud-disaster-recovery"></a>Azure yay bulut olağanüstü durum kurtarma

Bu makalede, Azure Spring Cloud uygulamalarınızı kapalı kalma süresi yaşamadan korumak için kullanabileceğiniz bazı stratejiler açıklanmaktadır.  Bölgesel felaketlerden kaynaklanan hiçbir bölge veya veri merkezi kesinti yaşar, ancak dikkatli bir planlama, müşterilerinizin etkilerini hafifletmeye başlayabilir.

## <a name="plan-your-application-deployment"></a>Uygulama dağıtımınızı planlayın

Azure yay bulut uygulamaları belirli bir bölgede çalışır.  Azure dünyanın dört bir yanındaki birden fazla coğrafi ortamda çalışır. Azure coğrafya, en az bir Azure bölgesi içeren, dünyanın tanımlı bir alanıdır. Bir Azure bölgesi, bir veya daha fazla veri merkezi içeren coğrafya içindeki bir alandır.  Her Azure bölgesi aynı coğrafya içindeki başka bir bölge ile eşleştirilir ve bir bölgesel çift oluşturur. Azure, her bir çiftin tek seferde yalnızca bir bölgenin güncelleştirildiğinden emin olmak için, bölgesel çiftler genelinde platform güncelleştirmelerini (planlı bakım) seri hale getirir. Birden çok bölgeyi etkileyen kesinti durumunda, her bir çiftin en az bir bölgesine kurtarma için öncelik alınacaktır.

Olağanüstü durumlarda yüksek kullanılabilirlik ve koruma sağlamak için, Spring Cloud uygulamalarınızı birden çok bölgeye dağıtmanız gerekir.  Azure, Spring Cloud dağıtımlarını bölgesel çiftlerine planlayabilmeniz için [eşleştirilmiş bölgelerin](../best-practices-availability-paired-regions.md) bir listesini sağlar.  Mikro hizmet mimarinizi tasarlarken üç temel faktörleri göz önünde bulundurmanız önerilir: bölge kullanılabilirliği, Azure eşleştirilmiş bölgeler ve hizmet kullanılabilirliği.

*  Bölge kullanılabilirliği: ağ gecikmesini ve iletim süresini en aza indirmek için kullanıcılarınıza yakın bir coğrafi alan seçin.
*  Azure eşlenmiş bölgeler: ihtiyacınız olursa, koordine edilen platform güncelleştirmelerini ve önceliklendirilmiş kurtarma çalışmalarını sağlamak için seçtiğiniz coğrafi alan dahilinde eşleştirilmiş bölgeleri seçin.
*  Hizmet kullanılabilirliği: eşleştirilmiş bölgelerin Hot/Hot, Hot/sıcak veya sıcak/soğuk çalışmasına karar verin.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Trafiği yönlendirmek için Azure Traffic Manager kullanma

[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) , DNS tabanlı trafik yük dengelemesi sağlar ve ağ trafiğini birden çok bölgeye dağıtabilir.  Müşterileri, en yakın Azure Spring Cloud Service örneğine yönlendirmek için Azure Traffic Manager kullanın.  En iyi performans ve artıklık için, Azure Spring Cloud Service 'e göndermeden önce tüm uygulama trafiğini Azure Traffic Manager aracılığıyla doğrudan yönlendirin.

Birden çok bölgede Azure Spring Cloud uygulamalarınız varsa, her bölgede uygulamalarınıza giden trafik akışını denetlemek için Azure Traffic Manager kullanın.  Hizmet IP 'sini kullanarak her hizmet için bir Azure Traffic Manager uç noktası tanımlayın. Müşteriler Azure Spring Cloud Service ' i işaret eden bir Azure Traffic Manager DNS adına bağlanmalıdır.  Azure Traffic Manager yük, trafiği tanımlanan uç noktalar arasında dengeler.  Bir olağanüstü durum veri merkezini ortaya çıkardığında, Azure Traffic Manager, trafiği bu bölgeden çiftine yönlendirirler ve hizmet devamlılığını sağlar.