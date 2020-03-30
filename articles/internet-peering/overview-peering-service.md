---
title: Internet peering ve Peering Service
titleSuffix: Azure
description: Internet peering ve Peering Service
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: overview
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 6c1205fcacd3d7228f1aecf1e603b66d9e1fcee5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75775708"
---
# <a name="internet-peering-vs-peering-service"></a>Internet peering ve Peering Service

Internet bakış, Microsoft'un küresel ağı (AS8075) ile Taşıyıcılar veya Hizmet Sağlayıcıları ağı arasındaki tüm ara bağlantıları ifade eder. Bir Hizmet Sağlayıcısı, müşteriden Microsoft ağına en iyi yönlendirmeyle güvenilir ve yüksek performanslı kamu bağlantısı sağlamak için aşağıda açıklanan PeerIng Service ortaklık gereksinimlerini uygulayarak Bir PeerIng Service iş ortağı olabilir.

## <a name="about-peering-service"></a>Eşleme Hizmeti hakkında
Peering Service, kurumsal kullanıcılarına sınıfının en iyisi genel Internet bağlantısı sağlamak için anahtar hizmet sağlayıcılarıyla bir ortaklık programıdır. Programın bir parçası olan iş ortakları, doğrudan, yüksek oranda kullanılabilir, coğrafi olarak yedekli bağlantılara ve Microsoft'a en iyi yönlendirmeye sahip olacaktır. Peering Service, Microsoft bağlantı portföyüne ek olarak eklenir:
*   IaaS veya PaaS kaynaklarına özel bağlantı için ExpressRoute (özel IP alanı desteği)
    *   İş ortağı tabanlı bağlantı
    *   Microsoft'a doğrudan 100G bağlantı
*   Buluta VPN bağlantısı için Internet üzerinden IPSEC
*   Sanal WAN üzerinden Azure'a SD-WAN bağlantısı

Peering Service için hedef segment SaaS bağlantısı, SD-WAN müşterileri şubede internet breakout yapmak isteyen ve herhangi bir müşteri çift strateji MPLS ve kurumsal sınıf Internet olan.

Microsoft Cloud'a bağlanırken birincil amaç, bir kullanıcı sitesinden Microsoft Global Network'e gidiş dönüş süresini (RTT) azaltarak gecikme süresini en aza indirmek olmalıdır, bu da Microsoft'un tüm veri merkezlerini birbirine bağlayan genel ağ omurgasıdır ve birden çok bulut uygulama giriş noktası. Bkz. [Office 365'te en iyi bağlantı ve performansı elde etmek.](https://techcommunity.microsoft.com/t5/Office-365-Blog/Getting-the-best-connectivity-and-performance-in-Office-365/ba-p/124694)

> [!div class="mx-imgBorder"]
> ![Dağıtılmış erişim görüntüsü](./media/distributed-access.png)

Küresel bir işletmenin her şube yukarıdaki şekilde iş ortağının ağı üzerinden mümkün olan en yakın Microsoft kenar konumuna bağlanır.

**Peering Service müşteri avantajları:**
* En iyi performans ve güvenilirlik için Internet üzerinden Microsoft Cloud Services'e en iyi genel yönlendirme.
* Microsoft Cloud'a bağlanmak için tercih edilen SP'yi seçme özelliği.
* Gecikme bildirimi ve önek izleme gibi trafik bilgileri.
* Microsoft Cloud'dan Optimum Ağ Atlamaları (AS Hops).
* Rota analizi ve istatistikleri - Sınır Geçidi Protokolü[(BGP)](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)rota anomalileri (sızıntı/kaçırma algılama) ve suboptimal yönlendirme için olaylar.

## <a name="peering-service-partnership-requirements"></a>Bakan Hizmet ortaklığı gereksinimleri
* Müşteriye en yakın konumda Microsoft Cloud'a bağlantı. İş ortağı Hizmet Sağlayıcısı, kullanıcı trafiğini kullanıcıya en yakın Microsoft kenarına yönlendirir. Benzer şekilde, kullanıcıya yönelik trafikte, Microsoft trafiği (BGP etiketini kullanarak) kullanıcıya en yakın kenar konumuna yönlendirecek ve SP trafiği kullanıcıya teslim edecektir.
* İş ortağı, Microsoft Global Network ile kullanılabilir yüksek, yüksek verim ve coğrafi yedekli bağlantıyı korur.
* İş ortağı, gereksinimi karşılıyorsa, Peering Service'i desteklemek için mevcut akranlarını kullanabilir

## <a name="faq"></a>SSS
Sık sorulan sorular için [Bkz. Akran Hizmeti - SSS.](service-faqs.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Peering Service](https://docs.microsoft.com/azure/peering-service/)ile müşteri avantajları hakkında daha fazla bilgi edinin.
* [Peering Service iş ortağı walkthrough'ta](walkthrough-peering-service-all.md)Doğrudan Bakma Hizmeti ne kadar doğrudan bir bakış etkinleştirme adımları hakkında bilgi edinin.
