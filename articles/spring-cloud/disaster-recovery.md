---
title: Azure Bahar Bulutu coğrafi felaket kurtarma | Microsoft Dokümanlar
description: Bahar Bulutu uygulamanızı bölgesel kesintilerden nasıl koruyacağınızı öğrenin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: brendm
ms.openlocfilehash: 4961e5a63e5bc1933cf19b1f291b521d89cbda0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76279146"
---
# <a name="azure-spring-cloud-disaster-recovery"></a>Azure Bahar Bulutu olağanüstü kurtarma

Bu makalede, Azure Bahar Bulutu uygulamalarınızın kapalı kalma sürelerini yaşamaktan korumak için kullanabileceğiniz bazı stratejiler açıklanmaktadır.  Herhangi bir bölge veya veri merkezi bölgesel felaketlerden kaynaklanan kapalı kalma sürelerine neden olabilir, ancak dikkatli planlama müşterileriniz üzerindeki etkiyi azaltabilir.

## <a name="plan-your-application-deployment"></a>Uygulama dağıtımınızı planlayın

Azure İlkbahar Bulutu uygulamaları belirli bir bölgede çalışır.  Azure, dünyanın dört bir yanındaki birden fazla coğrafyada çalışmaktadır. Azure coğrafyası, dünyanın en az bir Azure Bölgesi içeren tanımlı bir alanıdır. Azure bölgesi, bir veya daha fazla veri merkezi içeren bir coğrafya içindeki bir alandır.  Her Azure bölgesi, aynı coğrafyadaki başka bir bölgeyle eşleştirilir ve bölgesel bir çift haline getirir. Azure, platform güncelleştirmelerini (planlı bakım) bölgesel çiftler arasında seri hale sağlayarak her çiftte aynı anda yalnızca bir bölgenin güncelleştirilmesini sağlar. Birden çok bölgeyi etkileyen bir kesinti durumunda, kurtarma için her çiftten en az bir bölgeye öncelik verilecektir.

Yüksek kullanılabilirlik ve felaketlere karşı koruma sağlamak, Bahar Bulutu uygulamalarınızı birden çok bölgeye dağıtmanızı gerektirir.  Azure, Bahar Bulutu dağıtımlarınızı bölgesel çiftlere göre planlayabilmeniz için [eşleştirilmiş bölgelerin](../best-practices-availability-paired-regions.md) bir listesini sağlar.  Mikro hizmet mimarinizi tasarlarken üç önemli faktörü göz önünde bulundurmanızı öneririz: bölge kullanılabilirliği, Azure eşleştirilmiş bölgeler ve hizmet kullanılabilirliği.

*  Bölge kullanılabilirliği: Ağ gecikmesini ve iletim süresini en aza indirmek için kullanıcılarınıza yakın bir coğrafi bölge seçin.
*  Azure eşleştirilmiş bölgeler: Eşgüdümlü platform güncelleştirmeleri ve gerekirse öncelikli kurtarma çabaları sağlamak için seçtiğiniz coğrafi bölge içinde eşleştirilmiş bölgeleri seçin.
*  Servis durumu: Eşleştirilmiş bölgelerinizin sıcak/sıcak mı, sıcak/sıcak mı yoksa sıcak/soğuk mu olması gerektiğine karar verin.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Trafiği yönlendirmek için Azure Trafik Yöneticisi'ni kullanın

[Azure Trafik Yöneticisi,](../traffic-manager/traffic-manager-overview.md) DNS tabanlı trafik yükü dengelemesağlar ve ağ trafiğini birden çok bölgeye dağıtabilir.  Müşterileri kendilerine en yakın Azure İlkbahar Bulutu hizmeti örneğine yönlendirmek için Azure Trafik Yöneticisi'ni kullanın.  En iyi performans ve artıklık için, Azure İlkbahar Bulut hizmetinize göndermeden önce tüm uygulama trafiğini Azure Trafik Yöneticisi'ne yönlendirin.

Birden çok bölgede Azure İlkbahar Bulut uygulamalarınız varsa, her bölgedeki uygulamalarınız için trafik akışını denetlemek için Azure Trafik Yöneticisi'ni kullanın.  Hizmet IP'sini kullanarak her hizmet için bir Azure Trafik Yöneticisi bitiş noktası tanımlayın. Müşteriler, Azure İlkbahar Bulutu hizmetini işaret eden bir Azure Trafik Yöneticisi DNS adı bağlanması gerekir.  Azure Trafik Yöneticisi yük, trafiği tanımlanan uç noktalar arasında dengeler.  Bir veri merkezine bir felaket çarpatırsa, Azure Trafik Yöneticisi o bölgeden gelen trafiği çiftine yönlendirerek hizmet sürekliliğini sağlar.