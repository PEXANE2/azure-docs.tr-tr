---
title: Azure 'un uygulama teslim paketiyle Azure ön kapısının yük dengelemesi | Microsoft Docs
description: Bu makale, Azure 'un uygulama teslim paketiyle yük dengelemeyi nasıl önerdiği hakkında bilgi edinmenize yardımcı olur
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 44af14a01e7b045b7abb6a84db89a67f3dd22445
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80875291"
---
# <a name="load-balancing-with-azures-application-delivery-suite"></a>Azure uygulama teslim paketiyle yük dengeleme

## <a name="introduction"></a>Giriş
Microsoft Azure, ağ trafiğinizin nasıl dağıtılacağını ve yük dengelenmesi yönetmek için birden çok genel ve bölgesel hizmet sağlar: Traffic Manager, ön kapı, Application Gateway ve Load Balancer.  Azure 'un pek çok bölgesi ve zikzak mimarisiyle birlikte bu hizmetlerin birlikte kullanılması, güçlü ve ölçeklenebilir yüksek performanslı uygulamalar oluşturmanıza olanak tanır.

![Uygulama teslim paketi ][1]
 
Bu hizmetler iki kategoride bozulur:
1. Traffic Manager ve ön kapı gibi **küresel Yük Dengeleme Hizmetleri** , son kullanıcılarınızın bulut genelindeki veya hatta karma şirket içi hizmetlerinizin yanı sıra bölgesel arka uçlarınızın genelinde trafiği dağıtır. Küresel Yük Dengeleme, trafiğinizi en yakın hizmet arka ucunuza yönlendirir ve kullanıcılarınız için her zaman açık ve performans performansını sürdürmek üzere hizmet güvenilirliği veya performansındaki değişikliklere tepki verir. 
2. Standart Load Balancer veya Application Gateway gibi **Bölgesel Yük Dengeleme Hizmetleri** , sanal makineler (VM) veya bir bölgedeki bölgesel hizmet uç noktaları arasında sanal ağlar (VNet) içinde trafik dağıtma yeteneği sağlar.

Uygulamanızda küresel ve bölgesel hizmetler birleştirmek, kullanıcılarınızı IaaS, PaaS veya şirket içi hizmetlerinize yönlendirmek için uçtan uca güvenilir, performanslı ve güvenli bir yol sağlar. Sonraki bölümde, bu hizmetlerin her birini açıklıyoruz.

## <a name="global-load-balancing"></a>Küresel Yük Dengeleme
**Traffic Manager** genel DNS yük dengelemesi sağlar. Müşterinin seçtiği yönlendirme ilkesine uygun olarak, gelen DNS isteklerine bakar ve sağlıklı bir arka uca yanıt verir. Yönlendirme yöntemleri seçenekleri şunlardır:
- İstek sahibinin gecikme süresi bakımından en yakın arka uca gönderilmesi için performans yönlendirme.
- Diğer arka uçlarla yedekleme olarak tüm trafiği arka uca yönlendirmek için öncelik yönlendirme.
- Her bir arka uca atanan ağırlığa göre trafiği dağıtan ağırlıklı hepsini bir kez deneme yönlendirmesi.
- Coğrafi yönlendirme belirli coğrafi bölgelerde bulunan istek sahipleri bu bölgelere eşlenen arka uçlara yönlendirildiğinden emin olmak için (örneğin, Ispanya 'dan gelen tüm isteklerin Azure bölgesine Fransa Orta yönlendirilmelidir)
- Bundan sonra gelen isteklerin belirtilen arka uca gönderilmesini sağlamak için IP adresi aralıklarını arka uçlara eşlemenizi sağlayan alt ağ yönlendirme (örneğin, kurumsal HQ 'ın IP adresi aralığından bağlanan tüm kullanıcıların, genel kullanıcılardan farklı web içerikleri alması gerekir)

İstemci doğrudan bu arka uca bağlanır. Azure Traffic Manager, bir arka ucun sağlıksız olduğunu algılar ve sonra istemcileri başka bir sağlıklı örneğe yönlendirir. Hizmet hakkında daha fazla bilgi edinmek için [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) belgelerine bakın.

**Azure ön kapısı** , genel HTTP yük dengelemesi dahil olmak üzere dinamik Web sitesi HıZLANDıRMA (DSA) sağlar.  Gelen HTTP isteklerine, belirtilen ana bilgisayar adı, URL yolu ve yapılandırılmış kuralların en yakın hizmet arka ucuna/bölgesine bakar.  
Ön kapı, Microsoft 'un ağındaki HTTP isteklerini sonlandırır ve uygulama ya da altyapı durumunu veya gecikme süresini algılamak üzere etkin bir şekilde araştırmalar.  Ön kapı daha sonra her zaman trafiği en hızlı ve kullanılabilir (sağlıklı) arka uca yönlendirir. Hizmet hakkında daha fazla bilgi edinmek için ön kapıların [yönlendirme mimarisi](front-door-routing-architecture.md) ayrıntılarına ve [trafik yönlendirme yöntemlerine](front-door-routing-methods.md) bakın.

## <a name="regional-load-balancing"></a>Bölgesel yük dengeleme
Application Gateway, uygulamanız için çeşitli katman 7 yük dengeleme özellikleri sunan bir hizmet olarak uygulama teslim denetleyicisi (ADC) sağlar. Müşterilerin, CPU yoğun TLS sonlandırmasını uygulama ağ geçidine devrederek, Web grubu üretkenliğini iyileştirmelerine olanak tanır. Diğer katman 7 yönlendirme özellikleri, gelen trafik, tanımlama bilgisi tabanlı oturum benzeşimi, URL yolu tabanlı Yönlendirme ve tek bir uygulama ağ geçidinin arkasında birden fazla Web sitesini barındırma olanağı içerir. Application Gateway, Internet 'e yönelik ağ geçidi, yalnızca dahili ağ geçidi veya her ikisinin bir birleşimi olarak yapılandırılabilir. Application Gateway tamamen Azure tarafından yönetilen, ölçeklenebilir ve yüksek oranda kullanılabilir. Daha iyi yönetilebilirlik için zengin tanılama ve günlüğe kaydetme özellikleri sağlar.
Load Balancer, tüm UDP ve TCP protokolleri için yüksek performanslı ve düşük gecikmeli katman 4 Yük Dengeleme hizmetleri sunan Azure SDN yığınının ayrılmaz bir parçasıdır. Gelen ve giden bağlantıları yönetir. Yük dengeleme özelliğine sahip genel ve şirket içi yük uç noktaları yapılandırıp TCP ve HTTP hizmet durumu yoklama seçeneklerini kullanarak gelen bağlantıları arka uç havuz hedefleriyle eşleyebilir ve hizmet kullanılabilirliği sağlayabilirsiniz.


## <a name="choosing-a-global-load-balancer"></a>Küresel yük dengeleyici seçme
Küresel yönlendirme için Traffic Manager ile Azure ön kapısı arasında küresel bir yük dengeleyici seçerken, nelerin benzer olduğunu ve iki hizmet hakkında ne olduğunu göz önünde bulundurmanız gerekir.   Her iki hizmet de sağlar
- **Çoklu coğrafi yedeklilik:** Bir bölge kapalıysa trafik, uygulama sahibinden herhangi bir müdahale olmadan en yakın bölgeye sorunsuz bir şekilde yol açar.
- **En yakın bölge yönlendirme:** Trafik, en yakın bölgeye otomatik olarak yönlendirilir

</br>Aşağıdaki tabloda Traffic Manager ve Azure ön kapısı arasındaki farklar açıklanmaktadır:</br>

| Traffic Manager | Azure Front Door |
| --------------- | ------------------------ |
|**Herhangi bir protokol:** Traffic Manager DNS katmanında çalıştığından, herhangi bir ağ trafiği türünü yönlendirebilirsiniz; HTTP, TCP, UDP vb. | **Http hızlandırma:** Ön kapı trafiği, Microsoft 'un ağının kenarına göre belirlenir.  Bu nedenle, HTTP (S) istekleri, TLS anlaşması gecikmesini azaltan gecikme süresi ve aktarım hızı iyileştirmeleri ve AFD 'den uygulamanıza sık erişimli bağlantılar sağlar.|
|**Şirket içi yönlendirme:** Bir DNS katmanında yönlendirme sayesinde trafik her zaman noktadan noktaya gider.  Şube ofisinizden şirket içi veri merkezinize yönlendirme, doğrudan bir yol alabilir; Traffic Manager kullanarak kendi ağınızda bile. | **Bağımsız ölçeklenebilirlik:** Ön kapı HTTP isteğiyle çalıştığından, farklı URL yollarına yönelik istekler kurallara ve her bir uygulama mikro hizmetinin sistem durumuna göre farklı arka uç/bölgesel hizmet havuzlarına (mikro hizmetlere) yönlendirilebilir.|
|**Faturalama biçimi:** Kullanıcılarınız ve daha fazla kullanıcı içeren hizmetler için DNS tabanlı faturalandırma, daha yüksek kullanımlarda maliyeti azaltır. |**Satır içi güvenlik:** Ön kapı, trafiğin uygulamanıza ulaşmadan önce arka uçlarınızı korumanızı sağlamak için hız sınırlaması ve IP ACL 'leri gibi kurallara olanak sağlar. 

</br>Ön kapılı HTTP iş yükleri için performans, çalışma ve güvenlik avantajları nedeniyle, müşterilerin HTTP iş yükleri için ön kapı kullanmasını öneririz.    Traffic Manager ve ön kapı, uygulamanızın tüm trafiğini karşılamak için paralel olarak kullanılabilir. 

## <a name="building-with-azures-application-delivery-suite"></a>Azure 'un uygulama teslim paketiyle oluşturma 
Tüm Web sitelerinin, API 'Lerin, hizmetlerin coğrafi olarak yedekli olmasını ve kullanıcılara en yakın (en düşük gecikme süresi) konumundan mümkün olduğunda trafik sunmasını öneririz.  Traffic Manager, ön kapı, Application Gateway ve Load Balancer Hizmetleri birleştirmek, coğrafi olarak ve coğrafi olarak yedekli bir şekilde, güvenilirliği, ölçeği ve performansı en üst düzeye çıkarmak için oluşturmanıza olanak sağlar.

Aşağıdaki diyagramda, genel bir Web hizmeti oluşturmak için bu hizmetlerin bir birleşimini kullanan örnek bir hizmet açıklanır.   Bu durumda, mimari, dosya ve nesne teslimi için genel arka uçlara yönlendirmek üzere ön kapı kullanırken,/Store/* düzeniyle eşleşen URL yollarını, diğer tüm istekleri bölgesel uygulama ağ geçitlerine yönlendirirken App Service geçirdikleri hizmete yönlendirirken, mimariyle ilgili genel arka uçları kullanmak için Traffic Manager kullanmıştır.

Bölgesinde, IaaS hizmeti için, uygulama geliştiricisi/images/* düzeniyle eşleşen tüm URL 'Lerin, Web grubunun geri kalanından farklı olan özel bir VM havuzundan sunulduğunu kararmıştır.

Ayrıca, dinamik içeriğe hizmet veren varsayılan VM havuzunun, yüksek kullanılabilirlik kümesinde barındırılan bir arka uç veritabanıyla iletişim sağlaması gerekir. Tüm dağıtım Azure Resource Manager ile ayarlanır.

Aşağıdaki diyagramda bu senaryonun mimarisi gösterilmektedir:

![Uygulama teslim paketi ayrıntılı mimarisi][2] 

> [!NOTE]
> Bu örnek, Azure tarafından sunulan Yük Dengeleme hizmetlerinin olası birçok yapılandırmasından yalnızca biridir. Traffic Manager, ön kapı, Application Gateway ve Load Balancer, Yük Dengeleme gereksinimlerinize en iyi şekilde karışabilir ve eşleştirilebilir. Örneğin, TLS/SSL yük boşaltma veya katman 7 işleme gerekli değilse, Load Balancer Application Gateway yerine kullanılabilir.


## <a name="next-steps"></a>Sonraki Adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.

<!--Image references-->
[1]: ./media/front-door-lb-with-azure-app-delivery-suite/application-delivery-figure1.png
[2]: ./media/front-door-lb-with-azure-app-delivery-suite/application-delivery-figure2.png
