---
title: Ağ özellikleri
description: Azure Uygulama Hizmeti'ndeki ağ özellikleri ve güvenlik veya işlevsellik için ağ gereksinimleriniz için ihtiyacınız olan özellikler hakkında bilgi edinin.
author: ccompy
ms.assetid: 5c61eed1-1ad1-4191-9f71-906d610ee5b7
ms.topic: article
ms.date: 03/16/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: bc3d35830d833c0223a400140c53e583d2f6ed37
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475434"
---
# <a name="app-service-networking-features"></a>App Service ağ özellikleri

Azure Uygulama Hizmeti'ndeki uygulamalar birden çok şekilde dağıtılabilir. Varsayılan olarak, App Service barındırılan uygulamalar doğrudan internet erişimine açıktır ve yalnızca internet barındırılan uç noktalara ulaşabilir. Ancak birçok müşteri uygulamasının gelen ve giden ağ trafiğini denetlemesi gerekir. Bu gereksinimleri karşılamak için Uygulama Hizmeti'nde çeşitli özellikler mevcuttur. Sorun, belirli bir sorunu çözmek için hangi özelliğin kullanılması gerektiğini bilmektir. Bu belge, müşterilerin bazı örnek kullanım örneklerine göre hangi özelliğin kullanılması gerektiğini belirlemelerine yardımcı olmak için tasarlanmıştır.

Azure Uygulama Hizmeti için iki birincil dağıtım türü vardır. Ücretsiz, Paylaşılan, Temel, Standart, Premium ve Premiumv2 fiyatlandırma SK'lerinde Uygulama Hizmeti planlarını barındıran çok kiracılı kamu hizmeti vardır. Ardından, Doğrudan Azure Sanal Ağınızda (VNet) İzole SKU Uygulama Hizmeti planlarını barındıran tek kiracı Lı App Service Environment (ASE) bulunur. Kullandığınız özellikler, çok kiracılı hizmette veya BIR ASE'de yseniz, değişiklik gösterir. 

## <a name="multi-tenant-app-service-networking-features"></a>Çok kiracılı Uygulama Hizmeti ağ özellikleri 

Azure Uygulama Hizmeti dağıtılmış bir sistemdir. Gelen HTTP/HTTPS isteklerini işleyen rollere ön uç denir. Müşteri iş yükünü barındıran rollere işçiler denir. Bir Uygulama Hizmeti dağıtımındaki tüm roller çok kiracılı bir ağda bulunur. Aynı Uygulama Hizmeti ölçeği biriminde çok sayıda farklı müşteri olduğundan, App Service ağını doğrudan ağınıza bağlayamazsınız. Ağları bağlamak yerine, uygulama iletişiminin farklı yönlerini ele alacak özelliklere ihtiyacımız vardır. Uygulamanızdan gelen istekleri işleyen özellikler, uygulamanızdan arama yaparken sorunları çözmek için kullanılamaz. Aynı şekilde, uygulamanızdan gelen aramaların sorunlarını çözen özellikler de uygulamanızın sorunlarını çözmek için kullanılamaz.  

| Gelen özellikler | Giden özellikler |
|---------------------|-------------------|
| Uygulama atanan adres | Karma Bağlantılar |
| Erişim Kısıtlamaları | Ağ Geçidi gerekli VNet Entegrasyonu |
| Hizmet Uç Noktaları | Sanal Ağ Tümleştirmesi |

Aksi belirtilmedikçe, tüm özellikler birlikte kullanılabilir. Çeşitli sorunları çözmek için özellikleri karıştırabilirsiniz.

## <a name="use-case-and-features"></a>Büyük/küçük harf ve özellikleri kullanma

Herhangi bir kullanım örneği için, sorunu çözmek için birkaç yol olabilir.  Kullanılacak doğru özellik bazen yalnızca kullanım örneğinin dışındaki nedenlerden kaynaklanmaktadır. Aşağıdaki gelen kullanım durumları, uygulamanıza giden trafiği kontrol etme yle ilgili sorunları çözmek için App Service ağ özelliklerinin nasıl kullanılacağını önerir. 
 
| Gelen kullanım örnekleri | Özellik |
|---------------------|-------------------|
| Uygulamanız için IP tabanlı SSL gereksinimlerini destekleyin | uygulama atanan adres |
| Uygulamanız için paylaşılmayan, özel gelen adres | uygulama atanan adres |
| İyi tanımlanmış bir dizi adresten uygulamanıza erişimi kısıtlama | Erişim Kısıtlamaları |
| Bir VNet'teki kaynaklardan uygulamama erişimi kısıtlama | Hizmet Uç Noktaları </br> ILB ASE </br> Özel bitiş noktası (Önizleme) |
| Uygulamamı VNet'imde özel bir IP'de pozlama | ILB ASE </br> hizmet bitiş noktaları olan bir Uygulama Ağ Geçidi'nde gelen ler için özel IP </br> Hizmet Bitiş Noktası (Önizleme) |
| Uygulamamı bir WAF ile koruyun | Uygulama Ağ Geçidi + ILB ASE </br> Hizmet uç noktalarıyla Application Gateway </br> Erişim Kısıtlamaları olan Azure Ön Kapı |
| Farklı bölgelerdeki uygulamalarıma bakiye trafiği yükleme | Erişim Kısıtlamaları olan Azure Ön Kapı | 
| Aynı bölgedeki yük dengesi trafiği | [Hizmet uç noktalarıyla Application Gateway][appgwserviceendpoints] | 

Aşağıdaki giden kullanım durumları, uygulamanızın giden erişim gereksinimlerini çözmek için App Service ağ özelliklerinin nasıl kullanılacağını önerir. 

| Giden kullanım örnekleri | Özellik |
|---------------------|-------------------|
| Aynı bölgedeki bir Azure Sanal Ağındaki kaynaklara erişin | Sanal Ağ Tümleştirmesi </br> Ase |
| Farklı bir bölgedeki Azure Sanal Ağı'ndaki kaynaklara erişin | Ağ Geçidi gerekli VNet Entegrasyonu </br> ASE ve VNet eşleme |
| Hizmet bitiş noktalarıyla güvence altına alınmıştır kaynaklara erişim | Sanal Ağ Tümleştirmesi </br> Ase |
| Azure'a bağlı olmayan özel bir ağdaki kaynaklara erişin | Karma Bağlantılar |
| ExpressRoute devreleri arasında kaynaklara erişin | Sanal Ağ Tümleştirmesi </br> Ase | 
| Web uygulamanızdan güvenli giden trafik | VNet Tümleştirme ve Ağ Güvenlik Grupları </br> Ase | 
| Web uygulamanızdan giden trafiği yönlendirme | VNet Tümleştirme ve Rota Tabloları </br> Ase | 


### <a name="default-networking-behavior"></a>Varsayılan ağ davranışı

Azure Uygulama Hizmeti ölçek birimleri, her dağıtımda birçok müşteriyi destekler. Ücretsiz ve Paylaşılan SKU, çok kiracılı çalışanlarda müşteri iş yüklerini barındırmayı planlar. Temel ve yukarıdaki planlar, yalnızca bir Uygulama Hizmeti planına (ASP) adanmış müşteri iş yüklerini barındırMaktadır. Standart Uygulama Hizmeti planınız varsa, bu plandaki tüm uygulamalar aynı çalışan üzerinde çalışır. İşçiyi ölçeklendirirseniz, bu ASP'deki tüm uygulamalar ASP'nizdeki her örnek için yeni bir çalışanda çoğaltılır. Premiumv2 için kullanılan işçiler, diğer planlariçin kullanılan işçilerden farklıdır. Her Uygulama Hizmeti dağıtımının, bu Uygulama Hizmeti dağıtımındaki uygulamalara gelen tüm trafik için kullanılan bir IP adresi vardır. Ancak her yerde 4 ila 11 adresleri giden aramalar yapmak için kullanılır. Bu adresler, bu Uygulama Hizmeti dağıtımındaki tüm uygulamalar tarafından paylaşılır. Giden adresler farklı alt türlerine göre farklıdır. Bu, Ücretsiz, Paylaşılan, Temel, Standart ve Premium ADP'ler tarafından kullanılan adreslerin Premiumv2 ASP'lerinden gelen giden aramalarda kullanılan adreslerden farklı olduğu anlamına gelir. Uygulamanızın özelliklerine bakarsanız, uygulamanız tarafından kullanılan gelen ve giden adresleri görebilirsiniz. BIR IP ACL ile bir bağımlılığı kilitlemeniz gerekiyorsa, olası Giden Adresler'i kullanın. 

![Uygulama özellikleri](media/networking-features/app-properties.png)

Uygulama Hizmeti,hizmeti yönetmek için kullanılan bir dizi uç noktaya sahiptir.  Bu adresler ayrı bir belgede yayınlanır ve AppServiceManagement IP servis etiketinde de yer alan adreslerdir. AppServiceManagement etiketi yalnızca bu tür trafiğe izin vermeniz gereken bir Uygulama Hizmet Ortamı (ASE) ile kullanılır. App Service gelen adresleri AppService IP servis etiketinde izlenir. App Service tarafından kullanılan giden adresleri içeren bir IP hizmet etiketi yoktur. 

![Uygulama Hizmeti gelen ve giden diyagram](media/networking-features/default-behavior.png)

### <a name="app-assigned-address"></a>Uygulama atanan adres 

Uygulama atanan adres özelliği IP tabanlı SSL özelliğinin bir dalıdır ve uygulamanızla Birlikte SSL ayarlayarak erişilir. Bu özellik IP tabanlı SSL aramaları için kullanılabilir, ancak uygulamanıza yalnızca sahip olduğu bir adresi vermek için de kullanılabilir. 

![Uygulama atanan adres diyagramı](media/networking-features/app-assigned-address.png)

Atanmış bir adres kullandığınızda, trafiğiniz yine de App Service ölçeği birimine gelen tüm trafiği işleyen aynı ön uç rollerinden geçer. Ancak uygulamanız için atanan adres yalnızca uygulamanız tarafından kullanılır. Bu özelliğin kullanım örnekleri şunlardır:

* Uygulamanız için IP tabanlı SSL gereksinimlerini destekleyin
* Uygulamanız için başka hiçbir şeyle paylaşılmayan özel bir adres ayarlama

[IP tabanlı SSL Yapılandırma][appassignedaddress]öğretici ile uygulamanızda bir adres ayarlamayı öğrenebilirsiniz. 

### <a name="access-restrictions"></a>Erişim Kısıtlamaları 

Erişim Kısıtlamaları **özelliği, gelen** istekleri kaynak IP adresine göre filtrelemenize olanak tanır. Filtreleme eylemi, uygulamalarınızın çalıştığı alt rollerden yukarı doğru olan ön uç rolleri üzerinde gerçekleşir. Ön uç rolleri işçilerden akış yukarı olduğundan, Erişim Kısıtlamaları özelliği uygulamalarınız için ağ düzeyi koruması olarak kabul edilebilir. Bu özellik, öncelik sırasına göre değerlendirilen adres bloklarının listesini oluşturmanıza ve reddetmenize olanak tanır. Azure Ağ'da bulunan Ağ Güvenlik Grubu (NSG) özelliğine benzer.  Bu özelliği bir ASE'de veya çok kiracılı hizmette kullanabilirsiniz. ILB ASE ile kullanıldığında, özel adres bloklarından erişimi kısıtlayabilirsiniz.

![Erişim Kısıtlamaları](media/networking-features/access-restrictions.png)

Erişim Kısıtlamaları özelliği, uygulamanıza ulaşmak için kullanılabilecek IP adreslerini kısıtlamak istediğiniz senaryolarda yardımcı olur. Bu özelliğin kullanım örnekleri arasında:

* İyi tanımlanmış bir dizi adresten uygulamanıza erişimi kısıtlama 
* Azure Ön Kapı gibi bir yük dengeleme hizmeti aracılığıyla erişimi kısıtlama. Gelen trafiğinizi Azure Ön Kapı'ya kilitlemek istiyorsanız, 147.243.0.0/16 ve 2a01:111:2050:/44'ten gelen trafiğe izin verecek kurallar oluşturun. 

![Ön Kapı ile Erişim Kısıtlamaları](media/networking-features/access-restrictions-afd.png)

Uygulamanıza erişimi yalnızca Azure Sanal Ağınızdaki (VNet) kaynaklardan ulaşılabilmesi için kilitlemek istiyorsanız, kaynağınız VNet'inizde ne olursa olsun statik bir genel adrese ihtiyacınız vardır. Kaynakların ortak adresi yoksa, bunun yerine Hizmet Bitiş Noktaları özelliğini kullanmanız gerekir. [Erişim Kısıtlamalarını Yapılandırma][iprestrictions]öğreticisiyle bu özelliği nasıl etkinleştireceklerinizi öğrenin.

### <a name="service-endpoints"></a>Hizmet uç noktaları

Hizmet bitiş noktaları, uygulamanıza **gelen** erişimi kilitlemenize olanak tanır, böylelikle kaynak adres seçtiğiniz bir alt ağ kümesinden gelmelidir. Bu özellik IP Erişim Kısıtlamaları ile birlikte çalışır. Hizmet bitiş noktaları, IP Erişim Kısıtlamaları ile aynı kullanıcı deneyiminde ayarlanır. VNet'lerinizde ortak adreslerin yanı sıra alt ağları da içeren erişim kurallarının izin ver/reddet listesini oluşturabilirsiniz. Bu özellik, şu senaryoları destekler:

![hizmet bitiş noktaları](media/networking-features/service-endpoints.png)

* Uygulamanıza gelen trafiği kilitlemek için uygulamanızla bir Uygulama Ağ Geçidi ayarlama
* Uygulamanıza erişimi VNet'inizdeki kaynaklarla kısıtlamak. Bu, VM'leri, AS'leri ve hatta VNet Tümleştirmesi kullanan diğer uygulamaları içerebilir 

![uygulama ağ geçidi ile hizmet bitiş noktaları](media/networking-features/service-endpoints-appgw.png)

[Hizmet Bitiş Noktası Erişim Kısıtlamalarını Yapılandırma][serviceendpoints] öğreticisinde uygulamanızla hizmet bitiş noktalarını yapılandırma hakkında daha fazla bilgi edinebilirsiniz

### <a name="private-endpoint-preview"></a>Özel Bitiş Noktası (Önizleme)

Private Endpoint, Azure Private Link ile sizi Web Uygulamanıza özel ve güvenli bir şekilde bağlayan bir ağ arabirimidir. Private Endpoint, VNet'inizden gelen özel bir IP adresini kullanır ve Web Uygulamasını VNet'inize etkin bir şekilde getirir. Bu özellik yalnızca Web Uygulamanıza **gelen** akışlar içindir.
[Azure Web Uygulaması için Özel Uç Noktaları Kullanma (Önizleme)][privateendpoints]
 
### <a name="hybrid-connections"></a>Karma Bağlantılar

Uygulama Hizmeti Karma Bağlantıları, uygulamalarınızın belirtilen TCP uç noktalarına **giden** aramalar yapmalarını sağlar. Bitiş noktası şirket içinde, bir VNet'te veya 443 bağlantı noktasından Azure'a giden trafiğin izin verdiği herhangi bir yerde olabilir. Özellik, Bir Windows Server 2012 veya daha yeni ana bilgisayara Karma Bağlantı Yöneticisi (HCM) adlı bir geçiş aracısı yüklemesini gerektirir. HCM'nin 443 portundan Azure Relay'e ulaşabilmesi gerekir. HCM portaldaki App Service Hybrid Connections UI'den indirilebilir. 

![Karma Bağlantılar ağ akışı](media/networking-features/hybrid-connections.png)

App Service Karma Bağlantılar özelliği, Azure Röle Karma Bağlantılar özelliği ne eklenir. Uygulama Hizmeti, yalnızca uygulamanızdan bir TCP ana bilgisayara ve bağlantı noktasına giden aramaları destekleyen özel bir özellik biçimini kullanır. Bu ana bilgisayar ve bağlantı noktasının yalnızca HCM'nin yüklü olduğu ana bilgisayarda çözülmesi gerekir. Uygulama, App Service'de, Karma Bağlantınızda tanımlanan ana bilgisayar ve bağlantı noktası üzerinde bir DNS araması yaptığında, trafik otomatik olarak Hibrit Bağlantı'dan geçmek ve Karma Bağlantı Yöneticisi'nden çıkmak için yönlendirilir. Karma Bağlantılar hakkında daha fazla bilgi edinmek [için, Uygulama Hizmeti Karma Bağlantıları][hybridconn] hakkındaki belgeleri okuyun

Bu özellik genellikle şu şekilde kullanılır:

* Vpn veya ExpressRoute ile Azure'a bağlı olmayan özel ağlardaki kaynaklara erişin
* Destekleyici veritabanlarını taşımaya gerek kalmadan şirket içi uygulamaların kaldırılmasını ve Uygulama Hizmetine kaydırMalarını destekleyin  
* Karma Bağlantı başına tek bir ana bilgisayara ve bağlantı noktasına güvenli bir şekilde erişim sağlayın. Ağ özelliklerinin çoğu ağa açık erişime sahiptir ve Karma Bağlantılar ile yalnızca erişebileceğiniz tek ana bilgisayar ve bağlantı noktasına sahipsiniz.
* Diğer giden bağlantı yöntemleri kapsamında olmayan senaryoları kapsamaktadır
* Uygulamaların şirket içi kaynaklardan kolayca yararlanabileceği Uygulama Hizmeti'nde geliştirme gerçekleştirin 

Özellik, gelen güvenlik duvarı deliği olmadan şirket içi kaynaklara erişim sağladığından, geliştiriciler arasında popülerdir. Diğer giden App Service ağ özellikleri çok Azure Sanal Ağ ile ilgilidir. Karma Bağlantılar vnet üzerinden gidiyor bir bağımlılık yoktur ve ağ ihtiyaçlarını daha geniş bir yelpazede için kullanılabilir. App Service Hibrit Bağlantılar özelliğinin bunun üzerinde ne yaptığınızı umursamadığını veya bilmediğini unutmayın. Diğer bir veritabanı, bir web hizmeti veya bir ana bilgisayar üzerinde rasgele bir TCP soketi erişmek için kullanabilirsiniz demek. Özellik aslında TCP paketlerini tüneller. 

Hibrid Bağlantılar geliştirme için popüler olsa da, aynı zamanda çok sayıda üretim uygulamalarında da kullanılır. Bir web hizmetine veya veritabanına erişmek için harikadır, ancak birçok bağlantı oluşturmayla ilgili durumlar için uygun değildir. 

### <a name="gateway-required-vnet-integration"></a>Ağ Geçidi gerekli VNet Entegrasyonu 

Gereken uygulama hizmeti VNet Tümleştirme özelliği, uygulamanızın bir Azure Sanal Ağına **giden** isteklerde bulunmasını sağlar. Bu özellik, uygulamanızın üzerinde çalıştığı ana bilgisayarı, vnet'inizde bir sanal ağ ağ geçidine, siteye noktalı bir VPN ile bağlayarak çalışır. Özelliği yapılandırdığınızda, uygulamanız her örneğe atanan noktadan siteye adreslerden birini alır. Bu özellik, herhangi bir bölgedeki Klasik veya Kaynak Yöneticisi VNet'teki kaynaklara erişmenizi sağlar. 

![Ağ Geçidi gerekli VNet Entegrasyonu](media/networking-features/gw-vnet-integration.png)

Bu özellik, diğer VNet'lerde kaynaklara erişim sorununu çözer ve hatta vnet üzerinden diğer VNet'lere veya şirket içi kaynaklara bağlanmak için kullanılabilir. ExpressRoute'a bağlı VNets ile çalışmaz, ancak Siteden siteye VPN'e bağlı ağlarla çalışır. ASE zaten VNet'inizde olduğundan, bu özelliği bir Uygulama Hizmet Ortamında (ASE) kullanmak normalde uygun değildir. Bu özelliğin çözdüğünden kullanım örnekleri şunlardır:

* Azure sanal ağlarınızda özel IP'lerin kaynaklarına erişim 
* Siteden siteye VPN varsa, kaynaklara şirket içinde erişim 
* Eşlenen VNet'lerde kaynaklara erişim 

Bu özellik etkinleştirildiğinde, uygulamanız hedef VNet'in ile yapılandırıldığı DNS sunucusunu kullanır. Bu özellik hakkında daha fazla bilgi için [App Service VNet Integration][vnetintegrationp2s]ile ilgili belgelerde yer alabilirsiniz. 

### <a name="vnet-integration"></a>Sanal Ağ Tümleştirmesi

Gerekli ağ geçidi VNet Tümleştirme özelliği çok yararlıdır, ancak expressroute genelinde kaynaklara erişimi çözmez. ExpressRoute bağlantılarına ulaşma gereksiniminin yanı üstüne, uygulamaların hizmet bitiş noktası güvenli hizmetlere çağrı yapabilmesi için ihtiyaç vardır. Bu ek gereksinimlerin her ikisini de çözmek için başka bir VNet Tümleştirme özelliği eklendi. Yeni VNet Tümleştirme özelliği, uygulamanızın arka ucunu aynı bölgedeki bir Kaynak Yöneticisi VNet'teki bir alt ağa yerleştirmenizi sağlar. Bu özellik, zaten bir VNet'te bulunan bir Uygulama Hizmet Ortamından kullanılamaz. Bu özellik şunları sağlar:

* Aynı bölgedeki Kaynak Yöneticisi VNet'lerde kaynaklara erişim
* Hizmet bitiş noktalarıyla güvence altına alan kaynaklara erişim 
* ExpressRoute veya VPN bağlantıları üzerinden erişilebilen kaynaklara erişim
* Tüm giden trafiğin güvenliğini sağlama 
* Tüm giden trafiği tünele sokmaya zorluyor. 

![Sanal Ağ Tümleştirmesi](media/networking-features/vnet-integration.png)

Bu özellik hakkında daha fazla bilgi edinmek için [App Service VNet Entegrasyonu][vnetintegration]ile ilgili dokümanları okuyun.

## <a name="app-service-environment"></a>App Service Ortamı 

Uygulama Hizmet Ortamı (ASE), Azure Uygulama Hizmeti'nin VNet'inizde çalışan tek bir kiracı dağıtımıdır. ASE aşağıdaki gibi kullanım örnekleri sağlar:

* VNet'inizdeki kaynaklara erişin
* ExpressRoute'daki kaynaklara erişin
* Uygulamalarınızı VNet'inizde özel bir adresle ortaya çıkarma 
* Hizmet bitiş noktaları arasında kaynaklara erişin 

ASE ile VNet Tümleştirmesi veya hizmet bitiş noktaları gibi özellikleri kullanmanız gerekmez, çünkü ASE zaten VNet'inizdedir. Hizmet uç noktaları üzerinden SQL veya Storage gibi kaynaklara erişmek istiyorsanız, ASE alt netindeki hizmet uç noktalarını etkinleştirin. VNet'teki kaynaklara erişmek istiyorsanız, ek yapılandırma gerekmez.  ExpressRoute'daki kaynaklara erişmek istiyorsanız, zaten VNet'tesiniz ve ASE veya içindeki uygulamalarda herhangi bir şeyi yapılandırmanız gerekmez. 

ILB ASE'deki uygulamalar özel bir IP adresinde açıklanabildiği için, waf cihazlarını kolayca ekleyerek yalnızca istediğiniz uygulamaları internete maruz bırakabilir ve geri kalanını güvende tutabilirsiniz. Çok katmanlı uygulamaların kolay geliştirilmesine katkıda dır. 

Bir ASE'den gelen çok kiracılı hizmetten henüz mümkün olmayan bazı şeyler vardır. Bunlar arasında şunlar yer alıyor:

* Uygulamalarınızı özel bir IP adresinde ortaya çıkarma
* Uygulamanızın bir parçası olmayan ağ denetimleriyle tüm giden trafiği güvenli hale 
* Uygulamalarınızı tek bir kiracı hizmetinde barındırın 
* Çok kiracılı hizmette mümkün olandan çok daha fazla örneğe kadar ölçeklendirin 
* Uygulamalarınız tarafından kullanılmak üzere özel CA istemci sertifikalarını özel CA güvenli uç noktalarıyla yükleyin 
* TLS 1.1'i, sistemde barındırılan tüm uygulamalarda uygulama düzeyinde devre dışı bırakma olanağı olmadan zorlayın 
* ASE'nizdeki tüm uygulamalar için müşterilerle paylaşılmayan özel bir giden adres sağlayın 

![VNet'te ASE](media/networking-features/app-service-environment.png)

ASE izole ve özel uygulama barındırma etrafında en iyi hikaye sağlar ama bazı yönetim zorlukları ile birlikte geliyor. Operasyonel bir ASE kullanmadan önce göz önünde bulundurulması gereken bazı şeyler şunlardır:
 
 * Bir ASE VNet içinde çalışır, ancak VNet dışında bağımlılıkları vardır. Bu bağımlılıklara izin verilmeli. [Uygulama Hizmeti Ortamı için Ağ][networkinfo] la ilgili daha fazla bilgi edinin
 * AsE, çok kiracılı hizmet gibi hemen ölçeklendirmez. Reaktif ölçekleme yerine ölçekleme gereksinimlerini tahmin etmeniz gerekir. 
 * Bir ASE ile ilişkili daha yüksek bir ön maliyet var. ASE'nizden en iyi şekilde yararlanmak için, küçük çabalar için kullanılmak yerine birçok iş yükünü tek bir ASE'ye koymayı planlamanız gerekir.
 * ASE'deki uygulamalar, diğerlerinde değil, ASE'deki bazı uygulamalara erişimi kısıtlayamaz.
 * ASE bir alt ağ da bulunur ve herhangi bir ağ kuralları bu ASE'ye gelen ve bu ASE'den gelen tüm trafik için geçerlidir. Gelen trafik kurallarını tek bir uygulama için atamak istiyorsanız, Erişim Kısıtlamaları'nı kullanın. 

## <a name="combining-features"></a>Özellikleri birleştirme 

Çok kiracılı hizmet için belirtilen özellikler, daha ayrıntılı kullanım durumlarını çözmek için birlikte kullanılabilir. İki daha yaygın kullanım durumlarda burada açıklanmıştır ama bunlar sadece örnektir. Çeşitli özelliklerin ne yaptığını anlayarak, sistem mimarisi nin neredeyse tüm ihtiyaçlarını çözebilirsiniz.

### <a name="inject-app-into-a-vnet"></a>Uygulamayı VNet'e enjekte edin

Yaygın bir istek, uygulamanızın vnet'e nasıl yerleştirilen le ilgili olmasıdır. Uygulamanızı bir VNet'e yerleştirmek, bir uygulamanın gelen ve giden uç noktalarının bir VNet içinde olduğu anlamına gelir. ASE bu sorunu çözmek için en iyi çözümü sağlar, ancak özellikleri birleştirerek çok kiracı hizmetinde gerekenlerin çoğunu alabilirsiniz. Örneğin, intranet yalnızca özel gelen ve giden adresleri olan uygulamaları aşağıdakilere göre barındırabilirsiniz:

* Özel gelen ve giden adresle Uygulama Ağ Geçidi Oluşturma
* Servis bitiş noktalarıyla uygulamanız için gelen trafiği güvence altına alma 
* Uygulamanızın arka ucu VNet'inizde olacak şekilde yeni VNet Tümleştirmesini kullanın 

Bu dağıtım stili, internete giden trafik için size özel bir adres vermez veya uygulamanızdan gelen tüm giden trafiği kilitleme olanağı vermez.  Bu dağıtım stili, yalnızca aksi takdirde bir ASE ile elde edeceğiniçok bir şey verecektir. 

### <a name="create-multi-tier-applications"></a>Çok katmanlı uygulamalar oluşturun

Çok katmanlı bir uygulama, API arka uç uygulamalarına yalnızca ön uç katmanından erişilebilen bir uygulamadır. Çok katmanlı bir uygulama oluşturmak için şunları yapabilirsiniz:

* Ön uç web uygulamanızın arka ucunu Bir VNet'teki bir alt ağa bağlamak için VNet Tümleştirmesini kullanın
* API uygulamanıza gelen trafiği yalnızca ön uç web uygulamanız tarafından kullanılan alt ağdan gelmek için güvenli hale getirmek için hizmet uç noktalarını kullanın

![çok katmanlı uygulama](media/networking-features/multi-tier-app.png)

Birden çok ön uç uygulamasının, api uygulamasındaki diğer ön uç uygulamalarından vnet tümleştirmesini ve alt ağlarıyla API uygulamasındaki hizmet bitiş noktalarını kullanarak aynı API uygulamasını kullanmasını sağlayabilirsiniz.  

<!--Links-->
[appassignedaddress]: https://docs.microsoft.com/azure/app-service/configure-ssl-certificate
[iprestrictions]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[serviceendpoints]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[hybridconn]: https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections
[vnetintegrationp2s]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[vnetintegration]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[networkinfo]: https://docs.microsoft.com/azure/app-service/environment/network-info
[appgwserviceendpoints]: https://docs.microsoft.com/azure/app-service/networking/app-gateway-with-service-endpoints
[privateendpoints]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint
