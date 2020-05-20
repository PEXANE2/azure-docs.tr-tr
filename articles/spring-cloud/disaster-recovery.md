---
title: Azure yay bulutu coğrafi-olağanüstü durum kurtarma | Microsoft Docs
description: Spring Cloud uygulamanızı bölgesel kesintilerden nasıl koruyacağınızı öğrenin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: brendm
ms.openlocfilehash: 81ca6b2c365b0dd8a249a337f51d78516cb2cc61
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83657201"
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

Birden çok bölgede Azure Spring Cloud uygulamalarınız varsa, her bölgedeki uygulamalarınıza giden trafik akışını denetlemek için Azure Traffic Manager kullanın.  Hizmet IP 'sini kullanarak her hizmet için bir Azure Traffic Manager uç noktası tanımlayın. Müşteriler Azure Spring Cloud Service ' i işaret eden bir Azure Traffic Manager DNS adına bağlanmalıdır.  Azure Traffic Manager yük, trafiği tanımlanan uç noktalar arasında dengeler.  Bir olağanüstü durum veri merkezini ortaya çıkardığında, Azure Traffic Manager, trafiği bu bölgeden çiftine yönlendirirler ve hizmet devamlılığını sağlar.

## <a name="create-azure-traffic-manager-for-azure-spring-cloud"></a>Azure yay bulutu için Azure Traffic Manager oluşturma

1. Azure yay bulutu 'nı iki farklı bölgede oluşturun.
İki farklı bölgede (Doğu ABD ve Batı Avrupa) dağıtılan Azure Spring Cloud 'ın iki hizmet örneğine ihtiyacınız olacak. İki hizmet örneği oluşturmak için Azure portal kullanarak mevcut bir Azure yay bulutu uygulamasını başlatın. Her biri trafik için birincil ve yük devretme uç noktası olarak görev yapar. 

**İki hizmet örneği bilgisi:**

| Hizmet Adı | Konum | Uygulama |
|--|--|--|
| hizmet-örnek-a | Doğu ABD | Ağ Geçidi/kimlik doğrulama-hizmet/hesap-hizmet |
| hizmet-örnek-b | Batı Avrupa | Ağ Geçidi/kimlik doğrulama-hizmet/hesap-hizmet |

2. Hizmet için özel etki alanı ayarla özel etki alanı [belgeyi](spring-cloud-tutorial-custom-domain.md) izleyerek bu iki mevcut hizmet örneği için özel etki alanı ayarlayın. Başarılı olduktan sonra, her iki hizmet örneği de özel etki alanına bağlanır: bcdr-test.contoso.com

3. Traffic Manager ve iki uç nokta oluşturma: [Azure Portal kullanarak Traffic Manager profili oluşturun](https://docs.microsoft.com/azure/traffic-manager/quickstart-create-traffic-manager-profile).

Traffic Manager profili aşağıda verilmiştir:
* Traffic Manager DNS adı:`http://asc-bcdr.trafficmanager.net`
* Uç nokta profilleri: 

| Profil | Tür | Hedef | Öncelik | Özel üstbilgi ayarları |
|--|--|--|--|--|
| Bir profilin uç noktası | Dış uç nokta | service-sample-a.asc-test.net | 1 | Ana bilgisayar: bcdr-test.contoso.com |
| Uç nokta B profili | Dış uç nokta | service-sample-b.asc-test.net | 2 | Ana bilgisayar: bcdr-test.contoso.com |

4. DNS bölgesinde bir CNAME kaydı oluşturun: bcdr-test.contoso.com CNAME asc-bcdr.trafficmanager.net. 

5. Artık ortam tamamen ayarlanmıştır. Müşteriler uygulamaya şu yolla erişebilmelidir: bcdr-test.contoso.com