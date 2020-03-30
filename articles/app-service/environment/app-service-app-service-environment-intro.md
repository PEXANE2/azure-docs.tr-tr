---
title: ASE v1'e Giriş
description: App Service Environment v1 özellikleri hakkında bilgi edinin. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: stefsch
ms.assetid: 78e6d4f5-da46-4eb5-a632-b5fdc17d2394
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: f655b7793bfbb5bbeddfc2f1f8e7bc973dabeb4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687372"
---
# <a name="introduction-to-app-service-environment-v1"></a>Uygulama Hizmet Ortamına Giriş v1

> [!NOTE]
> Bu makale, App Service Environment v1 hakkındadır.  Uygulama Hizmet Ortamı'nın kullanımı daha kolay olan ve daha güçlü altyapıda çalışan daha yeni bir sürümü vardır. Yeni sürüm hakkında daha fazla bilgi edinmek [için Uygulama Hizmet Ortamına Giriş](intro.md)ile başlayın.

## <a name="overview"></a>Genel Bakış

Uygulama Hizmet Ortamı, Web Uygulamaları, Mobil Uygulamalar ve API Uygulamaları da dahil olmak üzere yüksek ölçekte güvenli bir şekilde çalışan Azure Uygulama Hizmeti uygulamalarını çalıştırmak için tamamen yalıtılmış ve özel bir ortam sağlayan [Azure Uygulama Hizmeti'nin](../overview.md) [Premium][PremiumTier] hizmet planı seçeneğidir.  

Uygulama Hizmet Ortamları, şunları gerektiren uygulama iş yükleri için idealdir:

* Çok yüksek ölçekli
* Yalıtım ve güvenli ağ erişimi

Müşteriler, tek bir Azure bölgesinde ve birden çok Azure bölgesinde birden çok Uygulama Hizmeti Ortamı oluşturabilir.  Bu, Yüksek RPS iş yüklerini desteklemek için Uygulama Hizmet Ortamlarını durum dışı uygulama katmanlarını yatay olarak ölçeklemek için ideal hale getirir.

Uygulama Hizmet Ortamları yalnızca tek bir müşterinin uygulamalarını çalıştırmak için yalıtılır ve her zaman sanal bir ağa dağıtılır.  Müşteriler hem gelen hem de giden uygulama ağı trafiği üzerinde ince gelişmiş denetime sahiptir ve uygulamalar sanal ağlar üzerinden şirket içi kurumsal kaynaklara yüksek hızlı güvenli bağlantılar kurabilir.

Uygulama Hizmeti Ortamlarının yüksek ölçekli ve güvenli ağ erişimini nasıl sağladığına genel bir bakış için, Uygulama Hizmet Ortamlarında [AzureCon Derin Dalış'a][AzureConDeepDive] bakın!

Birden çok Uygulama Hizmeti Ortamı nı kullanarak yatay olarak ölçekleme konusunda derin bir dalış yapmak [için, coğrafi olarak dağıtılan][GeodistributedAppFootprint]bir uygulama ayak izinin nasıl kurulabildiğini anlatan makaleye bakın.

AzureCon Deep Dive'de gösterilen güvenlik mimarisinin nasıl yapılandırıldığını görmek için, Uygulama Hizmeti Ortamları ile katmanlı bir [güvenlik mimarisi](app-service-app-service-environment-layered-security.md) uygulama yla ilgili makaleye bakın.

Uygulama Hizmet Ortamlarında çalışan uygulamaların erişimi, web uygulaması güvenlik duvarları (WAF) gibi yukarı akım aygıtları tarafından geçitlenebilir.  [App Service Environments için bir WAF yapılandırma](app-service-app-service-environment-web-application-firewall.md) ile ilgili makale bu senaryoyu kapsar.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a>Özel İşlem Kaynakları

Bir Uygulama Hizmet Ortamındaki tüm bilgi işlem kaynakları yalnızca tek bir aboneye ayrılmıştır ve Bir Uygulama Hizmet Ortamı, tek bir uygulama tarafından özel kullanım için elli (50) 'ye kadar bilgi işlem kaynağıyla yapılandırılabilir.

Uygulama Hizmet Ortamı, ön uç bilgi işlem kaynak havuzunun yanı sıra bir ila üç alt bilgi işlem kaynak havuzundan oluşur.

Ön uç havuzu, SSL sonlandırmaişleminden sorumlu bilgi işlem kaynaklarının yanı sıra Uygulama Hizmet Ortamındaki uygulama isteklerinin otomatik yük dengelemesini içerir.

Her çalışan havuzu, uygulama hizmet [planlarına][AppServicePlan]ayrılan ve bir veya daha fazla Azure Uygulama Hizmeti uygulaması içeren hesaplama kaynakları içerir.  Bir Uygulama Hizmet Ortamında en fazla üç farklı çalışan havuzu olabileceğinden, her çalışan havuzu için farklı işlem kaynakları seçme esnekliğine sahipsiniz.  

Örneğin, bu, uygulama geliştirme veya test uygulamaları için tasarlanmış Uygulama Hizmeti Planları için daha az güçlü bilgi işlem kaynaklarına sahip bir alt havuz oluşturmanıza olanak tanır.  İkinci (hatta üçüncü) bir alt havuz, üretim uygulamalarını çalıştıran Uygulama Hizmeti Planları için tasarlanmış daha güçlü bilgi işlem kaynaklarını kullanabilir.

Ön uç ve çalışan havuzları için kullanılabilir bilgi işlem kaynaklarının miktarı hakkında daha fazla bilgi için, [Uygulama Hizmet Ortamını Nasıl Yapılandırılatırabilirsiniz'][HowToConfigureanAppServiceEnvironment]a bakın.  

Bir Uygulama Hizmet Ortamında desteklenen kullanılabilir bilgi işlem kaynak boyutları yla ilgili ayrıntılar [için, App Service Fiyatlandırma][AppServicePricing] sayfasına başvurun ve Premium fiyatlandırma katmanındaki Uygulama Hizmeti Ortamları için kullanılabilir seçenekleri inceleyin.

## <a name="virtual-network-support"></a>Sanal Ağ Desteği

Bir Uygulama Hizmet **Ortamı,** Azure Kaynak Yöneticisi sanal ağında **veya** klasik dağıtım modeli sanal ağında[(sanal ağlarda daha fazla bilgi)][MoreInfoOnVirtualNetworks]oluşturulabilir.  Bir Uygulama Hizmet Ortamı her zaman sanal ağda ve daha doğrusu sanal ağın bir alt ağında bulunduğundan, hem gelen hem de giden ağ iletişimlerini denetlemek için sanal ağların güvenlik özelliklerinden yararlanabilirsiniz.  

Uygulama Hizmeti Ortamı, genel bir IP adresiyle Internet'e veya yalnızca bir Azure İç Yük Dengeleyicisi (ILB) adresiyle karşı karşıya olabilir.

Gelen ağ iletişimlerini Uygulama Hizmeti Ortamının bulunduğu alt ağla sınırlamak için [ağ güvenlik gruplarını][NetworkSecurityGroups] kullanabilirsiniz.  Bu, uygulamaları web uygulaması güvenlik duvarları ve ağ SaaS sağlayıcıları gibi yukarı akım aygıtlarının ve hizmetlerinin arkasında çalıştırmanızı sağlar.

Uygulamalar, iç veritabanları ve web hizmetleri gibi şirket kaynaklarına da sıklıkla erişmelidir.  Yaygın bir yaklaşım, bu uç noktaları yalnızca bir Azure sanal ağı içinde akan dahili ağ trafiği için kullanılabilir hale getirmektir.  Bir Uygulama Hizmet Ortamı dahili hizmetlerle aynı sanal ağa katıldıktan sonra, sitede çalışan uygulamalar, [Site'den Siteye][SiteToSite] ve [Azure ExpressRoute][ExpressRoute] bağlantıları üzerinden erişilebilen uç noktalar da dahil olmak üzere bu uygulamalara erişebilir.

Uygulama Hizmet Ortamları'nın sanal ağlar ve şirket içi ağlarla nasıl çalıştığı hakkında daha fazla bilgi için [Ağ Mimarisi,][NetworkArchitectureOverview] [Gelen Trafiği Denetleme][ControllingInboundTraffic]ve Arka Uçlara Güvenli Bir Şekilde Bağlanma ile ilgili aşağıdaki [makalelere][SecurelyConnectingToBackends]başvurun. 

## <a name="getting-started"></a>Başlarken

Uygulama Hizmet Ortamlarına başlamak için Uygulama [Hizmet Ortamı Nasıl Oluşturulur][HowToCreateAnAppServiceEnvironment]

App Service Environment ağ mimarisine genel bir bakış için [Ağ Mimarisine Genel Bakış][NetworkArchitectureOverview] makalesine bakın.

ExpressRoute ile Uygulama Hizmet Ortamı kullanma hakkında daha fazla bilgi için [Ekspres Rota ve Uygulama Hizmet Ortamları][NetworkConfigDetailsForExpressRoute]ile ilgili aşağıdaki makaleye bakın.

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: https://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: ../overview-hosting-plans.md
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[LogicApps]: https://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  app-service-app-service-environment-geo-distributed-scale.md
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  app-service-web-configure-an-app-service-environment.md
[ControllingInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SecurelyConnectingToBackends]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NetworkArchitectureOverview]:  app-service-app-service-environment-network-architecture-overview.md
[NetworkConfigDetailsForExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->