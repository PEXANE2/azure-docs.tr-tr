---
title: Ağ mimarisi v1
description: App Service Environments ağ topolojisi mimari bakış. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: stefsch
ms.assetid: 13d03a37-1fe2-4e3e-9d57-46dfb330ba52
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: b1b866f3be789c59eea38c5c22b5557d557440be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243854"
---
# <a name="network-architecture-overview-of-app-service-environments"></a>Ağ Mimarisi Uygulama Hizmet Ortamları Genel Bakış
Uygulama Hizmet Ortamları her zaman [sanal bir ağın][virtualnetwork] alt ağı içinde oluşturulur - Bir Uygulama Hizmeti Ortamında çalışan uygulamalar aynı sanal ağ topolojisi içinde bulunan özel uç noktalarla iletişim kurabilir.  Müşteriler sanal ağ altyapılarının bazı bölümlerini kilitleyebileceklerinden, Bir Uygulama Hizmeti Ortamı ile oluşan ağ iletişim akışları türlerini anlamak önemlidir.

## <a name="general-network-flow"></a>Genel Ağ Akışı
Bir Uygulama Hizmet Ortamı (ASE) uygulamalar için herkese açık bir sanal IP adresi (VIP) kullandığında, gelen tüm trafik o genel VIP'ye gelir.  Buna uygulamalar için HTTP ve HTTPS trafiğinin yanı sıra FTP için diğer trafik, uzaktan hata ayıklama işlevi ve Azure yönetimi işlemleri dahildir.  Genel VIP'de bulunan belirli bağlantı noktalarının (hem gerekli hem de isteğe bağlı) tam listesi için, Bir Uygulama Hizmet Ortamına [gelen trafiği denetleme][controllinginboundtraffic] yle ilgili makaleye bakın. 

Uygulama Hizmet Ortamları, yalnızca ILB (dahili yük dengeleyici) adresi olarak da adlandırılan sanal ağ dahili adresine bağlı olan uygulamaları çalıştırmayı da destekler.  Bir ILB'de uygulamalar için ASE, HTTP ve HTTPS trafiğinin yanı sıra uzaktan hata ayıklama çağrıları ILB adresine ulaşır.  En yaygın ILB-ASE yapılandırmaları için FTP/FTPS trafiği de ILB adresine gelecektir.  Ancak Azure yönetim işlemleri, ILB özellikli BIR ASE'nin genel VIP'si üzerindeki 454/455 bağlantı noktalarına akmaya devam eder.

Aşağıdaki diyagram, uygulamaların genel sanal IP adresine bağlı olduğu bir Uygulama Hizmeti Ortamı için gelen ve giden çeşitli ağ akışlarına genel bir bakış gösterir:

![Genel Ağ Akışları][GeneralNetworkFlows]

Uygulama Hizmeti Ortamı, çeşitli özel müşteri uç noktalarıyla iletişim kurabilir.  Örneğin, App Service Environment'da çalışan uygulamalar, aynı sanal ağ topolojisinde IaaS sanal makinelerde çalışan veritabanı sunucusuna(lar) bağlanabilir.

> [!IMPORTANT]
> Ağ diyagramına bakıldığında, "Diğer İşlem Kaynakları" Uygulama Hizmet Ortamından farklı bir Alt ağda dağıtılır. ASE ile aynı Alt Ağ'da kaynak dağıtmak ASE'den bu kaynaklara (belirli ASE içi yönlendirme hariç) bağlantı engelolur. Bunun yerine farklı bir Alt net'e dağıtın (aynı VNET'te). Daha sonra Uygulama Hizmet Ortamı bağlanabilecektir. Hiçbir ek yapılandırma gerekmez.
> 
> 

Uygulama Hizmet Ortamları, bir Uygulama Hizmet Ortamının yönetimi ve işletimi için gerekli olan Sql DB ve Azure Depolama kaynaklarıyla da iletişim kurar.  Bir Uygulama Hizmet Ortamının iletişim kurduğu Sql ve Depolama kaynaklarından bazıları App Service Environment ile aynı bölgede, bazıları ise uzak Azure bölgelerinde bulunur.  Sonuç olarak, Bir Uygulama Hizmeti Ortamının düzgün çalışması için her zaman Internet'e giden bağlantı gereklidir. 

Bir Alt ağda Uygulama Hizmeti Ortamı dağıtıldığından, ağ güvenlik grupları alt ağa gelen trafiği denetlemek için kullanılabilir.  Bir Uygulama Hizmet Ortamına gelen trafiği nasıl kontrol edeceğimiz hakkında ayrıntılı bilgi için aşağıdaki [makaleye][controllinginboundtraffic]bakın.

Bir Uygulama Hizmeti Ortamından giden Internet bağlantısına nasıl izin verilebilen ayrıntılar için [Express Route][ExpressRoute]ile çalışma hakkında aşağıdaki makaleye bakın.  Makalede açıklanan aynı yaklaşım, Siteden Siteye bağlantıyla çalışırken ve zorunlu tünel kullanımı nda da geçerlidir.

## <a name="outbound-network-addresses"></a>Giden Ağ Adresleri
Bir Uygulama Hizmet Ortamı giden aramalar yaptığında, IP Adresi her zaman giden aramalarla ilişkilidir.  Kullanılan belirli IP adresi, çağrılan bitiş noktasının sanal ağ topolojisi içinde mi yoksa sanal ağ topolojisi dışında mı bulunduğuna bağlıdır.

Çağrılan bitiş noktası sanal ağ topolojisinin **dışındaysa,** kullanılan giden adres (diğer adıyla giden NAT adresi) Uygulama Hizmet Ortamının genel VIP'sidir.  Bu adres, Özellikler'deki Uygulama Hizmet Ortamı'nın portal kullanıcı arabiriminde bulunabilir.

![Giden IP Adresi][OutboundIPAddress]

Bu adres, Yalnızca Uygulama Hizmet Ortamında bir uygulama oluşturarak ve uygulamanın adresine *nslookup* gerçekleştirerek yalnızca genel VIP'si olan ASE'ler için de belirlenebilir. Ortaya çıkan IP adresi hem genel VIP hem de App Service Environment'ın giden NAT adresidir.

Çağrılan uç nokta sanal ağ topolojisinin **içindeyse,** arama uygulamasının giden adresi uygulamayı çalıştıran tek tek bilgi işlem kaynağının iç IP adresi olacaktır.  Ancak uygulamalar için sanal ağ dahili IP adresleri kalıcı bir haritalama yoktur.  Uygulamalar farklı işlem kaynakları arasında hareket edebilir ve bir Uygulama Hizmeti Ortamındaki kullanılabilir işlem kaynakları havuzu ölçekleme işlemleri nedeniyle değişebilir.

Ancak, Bir Uygulama Hizmet Ortamı her zaman bir alt ağ içinde bulunduğundan, bir uygulamayı çalıştıran bir bilgi işlem kaynağının iç IP adresinin her zaman alt ağın CIDR aralığında yer alacağı garanti edilir.  Sonuç olarak, sanal ağdaki diğer uç noktalara erişimi sağlamak için ince taneli ALA'lar veya ağ güvenlik grupları kullanıldığında, App Service Environment'ı içeren alt ağ aralığına erişim izni verilmesi gerekir.

Aşağıdaki diyagram bu kavramları daha ayrıntılı olarak gösterir:

![Giden Ağ Adresleri][OutboundNetworkAddresses]

Yukarıdaki diyagramda:

* Uygulama Hizmet Ortamının genel VIP'si 192.23.1.2 olduğundan, bu "Internet" uç noktalarına arama yaparken kullanılan giden IP adresidir.
* App Service Environment için içeren alt netin CIDR aralığı 10.0.1.0/26'dır.  Aynı sanal ağ altyapısındaki diğer uç noktalar, uygulamalardan gelen çağrıları bu adres aralığındaki bir yerden gelen çağrıları görür.

## <a name="calls-between-app-service-environments"></a>Uygulama Servis Ortamları Arasındaki Aramalar
Aynı sanal ağda birden çok Uygulama Hizmeti Ortamı dağıtTığınızda ve bir Uygulama Hizmet Ortamından başka bir Uygulama Hizmet Ortamına giden çağrılar yaparsanız daha karmaşık bir senaryo oluşabilir.  Bu tür çapraz Uygulama Hizmeti Ortamı çağrıları da "Internet" çağrıları olarak kabul edilir.

Aşağıdaki diyagram, tek bir Uygulama Hizmet Ortamında uygulamaları içeren katmanlı bir mimari örneği (örn. "Ön kapı" web uygulamaları) ikinci bir Uygulama Hizmet Ortamındaki uygulamaları (örn. Internet'ten erişilemeyecek dahili arka uç API uygulamaları) arama yapar. 

![Uygulama Servis Ortamları Arasındaki Aramalar][CallsBetweenAppServiceEnvironments] 

Yukarıdaki örnekte "ASE One"ın giden IP adresi 192.23.1.2'dir.  Bu Uygulama Hizmet Ortamında çalışan bir uygulama, aynı sanal ağda bulunan ikinci bir Uygulama Hizmet Ortamı ("ASE Two") üzerinde çalışan bir uygulamaya giden arama yaparsa, giden arama "Internet" çağrısı olarak kabul edilir.  Sonuç olarak, ikinci Uygulama Hizmet Ortamı'na gelen ağ trafiği 192.23.1.2'den (yani ilk Uygulama Hizmeti Ortamının alt net adres aralığından değil) kaynaklanan olarak gösterecektir.

Farklı Uygulama Hizmeti Ortamları arasındaki çağrılar "Internet" çağrıları olarak kabul edilse de, her iki Uygulama Hizmeti Ortamı da aynı Azure bölgesinde bulunduğunda ağ trafiği bölgesel Azure ağında kalır ve fiziksel olarak genel Internet.  Sonuç olarak, ikinci App Service Environment'ın alt ağındaki ağ güvenlik grubunu yalnızca ilk App Service Environment'dan gelen aramalara izin vermek için kullanabilirsiniz (giden IP adresi 192.23.1.2' dir), böylece Uygulama arasında güvenli iletişim Servis Ortamları.

## <a name="additional-links-and-information"></a>Ek Linkler ve Bilgiler
App Service Environments tarafından kullanılan ve gelen trafiği kontrol etmek için ağ güvenlik gruplarını kullanan gelen bağlantı noktalarıyla ilgili [ayrıntılara buradan][controllinginboundtraffic]ulaşabilirsiniz.

Uygulama Hizmeti Ortamlarına giden Internet erişimi vermek için kullanıcı tanımlı yolların kullanımına ilişkin ayrıntılar bu [makalede][ExpressRoute]mevcuttur. 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[ExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

