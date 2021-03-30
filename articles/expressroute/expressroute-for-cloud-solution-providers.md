---
title: Bulut çözümü sağlayıcıları için ExpressRoute-Azure | Microsoft Docs
description: Bu makalede, Azure hizmetleri ve ExpressRoute 'un teklifleriyle birleştirmek isteyen bulut çözümü sağlayıcıları için bilgiler sağlanmaktadır.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 10/10/2016
ms.author: duau
ms.custom: seodec18
ms.openlocfilehash: 09fee610ccc15874481ecfd4693e4b89379caa7a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92330045"
---
# <a name="expressroute-for-cloud-solution-providers-csp"></a>Bulut Çözüm Sağlayıcıları (CSP) için ExpressRoute
Microsoft, geleneksel satıcılar veya dağıtımcıların (CSP), yeni hizmetler geliştirmeye yatırım yapmaya gerek kalmadan müşterileriniz için hızlı bir şekilde yeni hizmetler ve çözümler sağlayabilmesi amacıyla hiper ölçekli hizmetler sağlar. Bulut Çözüm Sağlayıcısının (CSP) bu hizmetleri doğrudan yönetebilmesini sağlamak için Microsoft, CSP’nin Microsoft Azure kaynaklarını müşterilerinizin adına yönetebilmesine olanak sağlayan programlar ve API’ler sunar. Bu kaynaklardan biri de ExpressRoute’dur. ExpressRoute, CSP’nin var olan Azure hizmetlerine bağlanmasına olanak sağlar. ExpressRoute, Azure 'daki hizmetlere yönelik yüksek hızlı bir özel iletişim bağlantıdır. 

ExpressRoute, tek bir müşterinin aboneliğine bağlı olan ve birden fazla müşteri tarafından paylaşılamayan yüksek kullanılabilirlik için bir dizi devreden oluşur. Yüksek kullanılabilirliği sürdürmek için her bağlantı hattı farklı bir yönlendiricide sonlandırılmalıdır.

> [!NOTE]
> Her ExpressRoute devresi için bant genişliği ve bağlantı sayısı sınırı vardır. Tek bir müşterinin ihtiyaçları bu sınırları aşarsa, karma ağ uygulamaları için birden çok ExpressRoute devresine gerek duyar.
> 
> 

Microsoft Azure tarafından sağlanan ve müşterilerinize sunabileceğiniz hizmetlerin sayısı gün geçtikçe artıyor. ExpressRoute, Microsoft Azure ortamına yüksek hızlı düşük gecikme süresi erişimi sağlayarak size ve müşterilerinizin bu hizmetlerden yararlanmasını sağlar.

## <a name="microsoft-azure-management"></a>Microsoft Azure yönetimi
Microsoft, kendi hizmet yönetimi sistemlerinizle programlı tümleştirme sağlayarak Azure müşteri aboneliklerini yönetmek için CSP 'Leri API 'Ler sağlar. Desteklenen yönetim özelliklerini [burada](/previous-versions/windows/mt844538(v=win.10)) bulabilirsiniz.

## <a name="microsoft-azure-resource-management"></a>Microsoft Azure kaynak yönetimi
Müşterinizle aranızdaki sözleşmeye bağlı olarak aboneliğin nasıl yönetileceği belirlenir. CSP, kaynakların oluşturulmasını ve bakımını doğrudan yönetebilir veya müşteri, Microsoft Azure aboneliğinin kontrolünü sağlayarak Azure kaynaklarını gereksinim duydukları gibi oluşturabilir. Müşteriniz Microsoft Azure aboneliklerinde kaynak oluşturmayı yönetirse şu iki modelden birini kullanır: "doğrudan *bağlantı*" modeli veya "*doğrudan*" modeli. Bu modeller aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.  

### <a name="connect-through-model"></a>Aracılı bağlantı modeli
!["Bağlantı-geçiş" modelini gösteren diyagram.](./media/expressroute-for-cloud-solution-providers/connect-through.png)  

Aracılı bağlantı modelinde CSP, veri merkeziniz ile müşterinizin Azure aboneliği arasında doğrudan bir bağlantı oluşturur. Bu doğrudan bağlantı, ExpressRoute kullanılarak oluşturulur ve ağınızı Azure’a bağlar. Ardından, müşteriniz ağınıza bağlanır. Bu senaryo, müşterinin Azure hizmetlerine ulaşmak için CSP ağından geçmesini gerektirir. 

Müşterinizin sizin tarafınızdan yönetilmeyen başka Azure abonelikleri varsa, CSP dışı abonelik kapsamında sağlanan hizmetlere bağlanmak için ortak Internet veya kendi özel bağlantılarını kullanırlar. 

CSP, Azure hizmetlerini yönetirken, CSP 'nin önceden oluşturulmuş bir müşteri kimliği deposuna sahip olduğu varsayılır ve bu durumda, CSP aboneliklerinin adına (AODE) göre yönetim için Azure Active Directory çoğaltılır. Bu senaryonun temel sürücüleri, belirli bir iş ortağının veya hizmet sağlayıcının müşteriyle ilgili bir ilişkiye sahip olduğunu, müşterinin Şu anda sağlayıcı hizmetleri 'ni tükettiğini ya da ortağın, tek başına CSP tarafından karşılanamayacak esneklik ve adres sağlamak için, sağlayıcı tarafından barındırılan ve Azure 'da barındırılan çözümlerin bir birleşimini sağlaması ister. Bu model aşağıdaki **şekilde** gösterilmiştir.

!["Bağlantı-geçiş" modeli için ayrıntılı bir senaryo gösteren diyagram.](./media/expressroute-for-cloud-solution-providers/connect-through-model.png)

### <a name="connect-to-model"></a>Doğrudan bağlantı modeli
!["Connect-to" modelini gösteren diyagram.](./media/expressroute-for-cloud-solution-providers/connect-to.png)

Doğrudan bağlantı modelinde hizmet sağlayıcısı, müşterinin veri merkezi ile müşterinin (müşteri) ağı üzerinden ExpressRoute kullanılarak CSP tarafından sağlanan Azure abonelikleri arasında doğrudan bir bağlantı oluşturur.

> [!NOTE]
> ExpressRoute için müşterinin ExpressRoute bağlantı hattını oluşturması ve yönetmesi gerekir.  
> 
> 

Bu bağlantı senaryosu, müşterinin doğrudan veya müşteri tarafından kısmen oluşturulan, sahip olduğu ve yönetilen bir doğrudan ağ bağlantısı kullanarak, CSP tarafından yönetilen Azure aboneliğine erişmek için müşterinin doğrudan bir müşteri ağından bağlanmasını gerektirir. Bu müşteriler için, sağlayıcının Şu anda bir müşteri kimliği deposunun kurulu olmadığı varsayılır ve sağlayıcı, bir müşterinin geçerli tanımlama mağazalarını, aboneliklerinde kendi abonelik yönetimi için Azure Active Directory olarak çoğaltmasına yardımcı olur. Bu senaryo için temel etkenler; belirtilen iş ortağı ya da hizmet sağlayıcının müşteriyle iletişimi olduğu, müşterinin şu anda hizmet sağlayıcının hizmetlerini kullandığı ya da iş ortağının mevcut bir sağlayıcı veri merkezi ya da altyapısına ihtiyaç duymadan sadece Azure’de bulunan çözümleri sunma isteği duyduğu durumları içerir.

!["Connect-to" modeli için ayrıntılı bir senaryo gösteren diyagram.](./media/expressroute-for-cloud-solution-providers/connect-to-model.png)

Bu iki seçenek arasındaki seçimler, müşterinizin ihtiyaçlarına ve Azure hizmetleri sağlamak için geçerli gereksinimlerinize göre yapılır. Bu modellere ve ilişkili rol tabanlı erişim denetimine, ağlara ve kimlik tasarımı desenlerine ilişkin ayrıntılar aşağıdaki linklerde ele alınmaktadır:

* **Azure rol tabanlı erişim denetimi (Azure RBAC)** – azure RBAC Azure Active Directory tabanlıdır.  Azure RBAC hakkında daha fazla bilgi için [buraya](../role-based-access-control/role-assignments-portal.md)bakın.
* **Ağlar** – Microsoft Azure’da ağlarla ilgili çeşitli konuları kapsar.
* **Azure Active Directory (Azure AD)** – Azure ad, Microsoft Azure ve üçüncü taraf SaaS uygulamaları için kimlik yönetimini sağlar. Azure AD hakkında daha fazla bilgi için [buraya](https://azure.microsoft.com/documentation/services/active-directory/)bakın.  

## <a name="network-speeds"></a>Ağ hızları
ExpressRoute, 50 MB/sn 'den 10 GB/sn 'ye kadar olan ağ hızlarını destekler. Bu, müşterilerin benzersiz ortamları için ihtiyaç duydukları miktarda ağ bant genişliğini satın almalarına olanak sağlar.

> [!NOTE]
> Ağ bant genişliği, gerektiğinde iletişimleri bozmadan artırılabilir ancak ağ hızını düşürmek için bağlantı hattının kaldırılması ve daha düşük ağ hızında yeniden oluşturulması gerekir.  
> 
> 

ExpressRoute, yüksek hızlı bağlantıların daha iyi kullanımı için birden fazla sanal ağın tek bir ExpressRoute bağlantı hattına bağlanmasını destekler. Tek bir ExpressRoute bağlantı hattı, aynı müşteriye ait birden fazla Azure aboneliği arasında paylaşılabilir.

## <a name="configuring-expressroute"></a>ExpressRoute’u yapılandırma
ExpressRoute, tek bir ExpressRoute bağlantı hattı üzerinden üç tür trafiği ([yönlendirme etki alanları](#expressroute-routing-domains)) destekleyecek şekilde yapılandırılabilir. Bu trafik, özel eşleme, Microsoft eşleme ve genel eşleme (kullanım dışı) olarak bölünmüştür. Tek bir ExpressRoute bağlantı hattı üzerinden gönderilmek üzere trafik türlerinden birini veya hepsini seçebilirsiniz veya ExpressRoute bağlantı hattının boyutuna ve müşteriniz tarafından gerekli görülen yalıtıma bağlı olarak birden çok ExpressRoute bağlantı hattı kullanabilirsiniz. Müşterinizin güvenlik yaklaşımı, genel ve özel trafiğin aynı bağlantı hattı üzerinden çapraz geçiş yapmalarına izin vermeyebilir.

### <a name="connect-through-model"></a>Aracılı bağlantı modeli
Bir bağlantı temelli yapılandırmada, tüm ağ underpinnings, müşterinizin veri merkezi kaynaklarını Azure 'da barındırılan aboneliklere bağlamak için de sorumlu olursunuz. Azure özelliklerini kullanmak isteyen müşterilerinizin her birine kendi ExpressRoute bağlantısı gerekir ve bu, sizin tarafınızdan yönetilecektir. Müşterinin ExpressRoute devresini temin etmek için kullanacağı yöntemlerin aynısını kullanacaksınız. Bağlantı hattı sağlama ve devre durumları için [ExpressRoute iş akışları](expressroute-workflows.md) makalesinde özetlenen adımların aynısını takip edersiniz. Daha sonra, şirket içi ağ ile Azure vNet arasındaki trafiği denetlemek için Sınır Ağ Geçidi Protokolü (BGP) yollarını yapılandıracaksınız.

### <a name="connect-to-model"></a>Doğrudan bağlantı modeli
Bir bağlantı yapılandırmasında, müşterinizin zaten bir Azure bağlantısı var veya veri merkeziniz yerine kendi veri merkezlerinden ExpressRoute 'u doğrudan Azure 'a bağlayan Internet servis sağlayıcısına bir bağlantı başlatacak. Hazırlama sürecine başlamak için müşteriniz, yukarıda, Aracılı Bağlantı modeli bölümünde belirtilen adımları takip edecektir.  Devre kurulduktan sonra, müşterinizin şirket içi yönlendiricileri hem ağınıza hem de Azure sanal ağlarına erişebilecek şekilde yapılandırmanız gerekecektir.

Yolların kendi veri merkez(ler)inizdeki kaynakların, kendi veri merkezinizdeki müşteri kaynaklarıyla ya da Azure’de barındırılan kaynaklarla iletişim kurabilmesi için bağlantıyı kurma ve yolları yapılandırma sürecinde yardımcı olabilirsiniz. 

## <a name="expressroute-routing-domains"></a>ExpressRoute yönlendirme etki alanları
ExpressRoute yeni devreler için iki yönlendirme etki alanı sunar: özel eşleme ve Microsoft eşleme. Yönlendirme etki alanlarının her biri, yüksek kullanılabilirlik için etkin-etkin yapılandırmada aynı yönlendiricilerle yapılandırılır. ExpressRoute yönlendirme etki alanları hakkında daha fazla ayrıntı için [buraya](expressroute-circuit-peerings.md) göz atın.

Sadece istediğiniz veya ihtiyaç duyduğunuz yol(lar)a izin verecek özel yol filtreleri tanımlayabilirsiniz. Bu değişiklikleri nasıl uygulayacağınızı görmek, yönlendirme filtreleri hakkında daha fazla ayrıntı ve daha fazla bilgi için şu makaleye göz atın:[PowerShell kullanarak bir ExpressRoute yönlendirmesi oluşturma ve değiştirme](expressroute-howto-routing-classic.md)

> [!NOTE]
> Microsoft eşlemesi için bağlantının, müşterinin veya CSP 'nin sahip olduğu bir genel IP adresi olması ve tüm tanımlı kurallara uyması gerekir. Daha fazla bilgi için bkz. [ExpressRoute ](expressroute-prerequisites.md).  
> 
> 

## <a name="routing"></a>Yönlendirme
ExpressRoute, Azure ağlarına Azure Virtual Network Gateway üzerinden bağlanır. Ağ geçitleri, Azure sanal ağlarına yönlendirme sağlar.

Azure Sanal Ağları’nı oluşturma, ayrıca Sanal Ağdan doğrudan trafiğe çift yönlü sanal ağ alt ağı için varsayılan bir yönlendirme tablosu oluşturur. Varsayılan yol tablosu çözüm için yetersizse, giden trafiği özel gereçlere yönlendirmek veya belirli alt ağlara veya dış ağlara yolları engellemek için özel yollar oluşturulabilir.

### <a name="default-routing"></a>Varsayılan yönlendirme
Varsayılan rota tablosu aşağıdaki rotaları içerir:

* Bir alt ağ içinde yönlendirme
* Sanal ağ içinde alt ağdan alt ağa yönlendirme
* İnternet’e yönlendirme
* VPN ağ geçidi kullanarak sanal ağdan sanal ağa yönlendirme
* Bir VPN ya da ExpressRoute ağ geçidi kullanarak sanal ağdan şirket içi ağına yönlendirme

![Varsayılan yönlendirme seçeneklerini gösteren diyagram.](./media/expressroute-for-cloud-solution-providers/default-routing.png)  

### <a name="user-defined-routing-udr"></a>Kullanıcı tanımlı yönlendirme (UDR)
Kullanıcı tanımlı yollar, sanal ağda veya diğer önceden tanımlanmış ağ geçitlerinde (ExpressRoute; İnternet veya VPN) tanımlanmış alt ağdan diğer alt ağlara giden trafik akışının kontrolüne olanak tanır.  Varsayılan sistem yönlendirme tablosu, varsayılan sistem yönlendirme tablosunu özel yollarla değiştiren kullanıcı tanımlı yönlendirme tablosu ile değiştirilebilir. Kullanıcı tanımlı yönlendirme ile müşteriler güvenlik duvarları gibi uygulamalara veya izinsiz giriş algılama gereçlerine belirli yollar oluşturabilir veya belirli alt ağlara kullanıcı tanımlı yolu barındıran alt ağdan erişimi engeller. User-Defined yollara genel bir bakış için [buraya](../virtual-network/virtual-networks-udr-overview.md)bakın. 

## <a name="security"></a>Güvenlik
Doğrudan veya Aracılı bağlantı modellerinden hangisinin kullanıldığına bağlı olarak, müşteriniz kendi sanal ağında güvenlik ilkeleri tanımlar ya da CSP’ye kendi sanal ağlarına tanımlaması için güvenlik ilkesi gereksinimleri sağlar. Aşağıdaki güvenlik ölçütleri tanımlanabilir:

1. **Müşteri Yalıtımı** — Azure platformu, Müşteri Kimliği ve sanal ağ bilgisini her müşterinin trafik bilgisini bir GRE tünelinde yalıtan güvenli bir veritabanında saklayarak müşteri yalıtımı sağlar.
2. **Ağ Güvenlik Grubu (NSG)** kuralları, izin verilen trafiğin Azure içindeki alt ağlara çift yönlü olarak tanımlanmasında kullanılan kurallardır.  Varsayılan olarak NSG, Internet 'ten vNet 'e gelen trafiği engellemek ve bir sanal ağ içindeki trafik için kurallara Izin vermek üzere blok kuralları içerir. Ağ güvenlik grupları hakkında daha fazla bilgi için [buraya](https://azure.microsoft.com/blog/network-security-groups/)bakın.
3. **Zorlamalı tünel** — Bu seçenek, Azure’den kaynaklanan internete bağlı trafiği, şirket içi veri merkezine ExpressRoute bağlantısı üzerinden yeniden yönlendirir. Zorlamalı tünel görünümü hakkında daha fazla bilgi için[buraya](expressroute-routing.md#advertising-default-routes) göz atın.  
4. **Şifreleme** — ExpressRoute bağlantı hatları belirli bir müşteriye ayrılmış olsa da, ağ sağlayıcısının ihlal edilme olasılığı bulunmaktadır ve bu, izinsiz giriş yapanların paket trafiğini incelemesi riskini doğurur.  Bu riske yönelik olarak, bir müşteri ya da CSP, şirket içi kaynaklar ve Azure kaynakları arasında akan tüm trafiği IPSec tünel modu ilkelerine tanımlayarak trafiği bağlantı üzerinden şifreleyebilir (yukarıda Şekil 5’te Müşteri 1 için isteğe bağlı Tünel modu IPSec: ExpressRoute Güvenliği konusunu inceleyin). İkinci seçenek ise ExpressRoute bağlantı hattının her ucunda bir güvenlik duvarı uygulaması kullanmak olabilir.  Bu, ExpressRoute bağlantı hattı üzerinden trafiği şifrelemek için her iki uçta ek üçüncü taraf güvenlik duvarı VM 'Leri/gereçlerinin yüklü olmasını gerektirir.

![alternatif metin](./media/expressroute-for-cloud-solution-providers/expressroute-security.png)  

## <a name="next-steps"></a>Sonraki adımlar
Bulut Çözümü Sağlayıcısı hizmeti, sizlere pahalı altyapı ve özelliklere ihtiyaç duymadan müşterilerinizin gözünde değerinizi artıracak bir yol sunarken, ana dış kaynak sağlayıcısı olarak konumunuzu korur. Microsoft Azure ile sorunsuz tümleştirme, Microsoft Azure yönetimini mevcut yönetim altyapınızın sınırları içinde tümleştirecek CSP API’si aracılığıyla gerçekleştirilebilir.   

Aşağıdaki bağlantılarda ek bilgiler bulunabilir:

[Azure Bulut Çözümü Sağlayıcısı programı](/azure/cloud-solution-provider).  
[Bir Bulut Çözümü Sağlayıcısı olmaya hazırlanma](https://partner.microsoft.com/solutions/cloud-reseller-pre-launch).  
[Microsoft Bulut Çözümü Sağlayıcısı kaynakları](https://partner.microsoft.com/solutions/cloud-reseller-resources).