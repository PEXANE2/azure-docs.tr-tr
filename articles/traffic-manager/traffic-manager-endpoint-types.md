---
title: Traffic Manager uç noktası türleri | Microsoft Docs
description: Bu makalede, Azure Traffic Manager ile kullanılabilecek farklı uç nokta türleri açıklanmaktadır
services: traffic-manager
documentationcenter: ''
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/21/2021
ms.author: duau
ms.openlocfilehash: 7686f2f97da0113704216dcab741c063a80d3136
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99051236"
---
# <a name="traffic-manager-endpoints"></a>Traffic Manager uç noktaları

Microsoft Azure Traffic Manager, ağ trafiğinin farklı veri merkezlerinde çalışan uygulama dağıtımlarına nasıl dağıtıldığını denetlemenize olanak tanır. Her uygulama dağıtımını Traffic Manager'da bir 'uç nokta' olarak yapılandırırsınız. Traffic Manager bir DNS isteği aldığında, DNS yanıtında geri döndürmek için kullanılabilir bir uç nokta seçer. Traffic Manager, geçerli uç nokta durumu ve trafik yönlendirme yöntemi üzerinde seçimi temel alır. Daha fazla bilgi için bkz. [Traffic Manager nasıl kullanılır](traffic-manager-how-it-works.md).

Traffic Manager tarafından desteklenen üç tür uç nokta vardır:

* **Azure uç noktaları**, Azure'da barındırılan hizmetler için kullanılır.
* **Dış uç noktalar** IPv4/IPv6 adresleri, FQDN 'Ler veya Azure dışında barındırılan hizmetler için kullanılır. Bu hizmetler şirket içinde ya da farklı bir barındırma sağlayıcısı ile olabilir.
* **İç içe uç noktalar** ise daha büyük ve daha karmaşık dağıtımların ihtiyaçlarını desteklemek üzere daha esnek trafik yönlendirme şemaları oluşturmak amacıyla Traffic Manager profillerini birleştirmek için kullanılır.

Farklı türlerin uç noktalarının tek bir Traffic Manager profilinde nasıl birleştirildiğine yönelik bir kısıtlama yoktur. Her profil, uç nokta türlerinin herhangi bir karışımını içerebilir.

Aşağıdaki bölümlerde her bir uç nokta türü daha ayrıntılı olarak açıklanır.

## <a name="azure-endpoints"></a>Azure uç noktaları

Azure uç noktaları, Traffic Manager Azure tabanlı hizmetler için kullanılır. Aşağıdaki Azure Kaynak türleri desteklenir:

* PaaS bulut hizmetleri.
* Web Apps
* Web uygulaması Yuvaları
* Publicıpaddress kaynakları (doğrudan ya da bir Azure Load Balancer aracılığıyla VM 'lere bağlanabilir). Publicıpaddress, bir Traffic Manager profilinde kullanılmak üzere atanmış bir DNS adına sahip olmalıdır.

Publicıpaddress kaynakları Azure Resource Manager kaynaklardır. Klasik dağıtım modelinde mevcut değildir ve yalnızca Traffic Manager Azure Resource Manager deneyimlerinde desteklenir. Diğer uç nokta türleri hem Kaynak Yöneticisi hem de klasik dağıtım modeli aracılığıyla desteklenir.

Azure uç noktaları kullanılırken, Traffic Manager bir Web uygulamasının durdurulup başlatıldığını algılar. Bu durum uç nokta durumuna yansıtılır. Ayrıntılar için [Traffic Manager uç nokta izleme](traffic-manager-monitoring.md#endpoint-and-profile-status) bölümüne bakın. Temel alınan hizmet durdurulduğunda Traffic Manager uç nokta sistem durumu denetimleri yapmaz veya uç noktaya doğrudan trafik vermez. Durdurulan örnek için Traffic Manager faturalandırma olayı gerçekleşmez. Hizmet yeniden başlatıldığında faturalandırma devam eder ve uç nokta trafik almaya uygun olur. Bu algılama, Publicıpaddress uç noktaları için uygulanmaz.

## <a name="external-endpoints"></a>Dış uç noktalar

Dış uç noktalar IPv4/IPv6 adresleri, FQDN 'Ler veya Azure dışındaki hizmetler için kullanılır. IPv4/IPv6 adres uç noktalarının kullanımı, Traffic Manager 'ın bir DNS adı olması gerekmeden uç noktaların sistem durumunu denetlemesini sağlar. Sonuç olarak, bu uç noktayı bir yanıtta döndürürken Traffic Manager bir/AAAA kaydı olan sorgulara yanıt verebilir. Azure dışındaki hizmetler, şirket içinde veya farklı bir sağlayıcı ile barındırılan bir hizmet içerebilir. Dış uç noktalar tek tek veya aynı Traffic Manager profilinde Azure Uç Noktaları ile birlikte kullanılabilir. Özel durum IPv4 veya IPv6 adresleri olarak belirtilen uç noktalar için yalnızca dış uç noktalar olabilir. Azure uç noktalarını dış uç noktalarla birleştirmek çeşitli senaryolara izin vermez:

* Azure kullanarak etkin-etkin veya etkin-Pasif yük devretme modelinde mevcut bir şirket içi uygulama için daha fazla artıklık sağlayın. 
* Trafiği kendileriyle ilişkili bir DNS adı olmayan uç noktalara yönlendirin. Ayrıca, döndürülen bir DNS adının IP adresini almak için ikinci bir DNS sorgusu çalıştırma gereksinimini ortadan kaldırarak genel DNS arama gecikmesini azaltır.
* Dünyanın dört bir yanındaki kullanıcılar için uygulama gecikmesini azaltın, mevcut şirket içi bir uygulamayı Azure 'daki diğer coğrafi konumlara genişletin. Daha fazla bilgi için bkz. [Traffic Manager ' performans ' trafiği yönlendirme](traffic-manager-routing-methods.md#performance).
* Azure kullanarak isteğe bağlı bir ani artış karşılamak için, mevcut bir şirket içi uygulama için sürekli olarak veya ' patlama-buluta ' çözümü olarak daha fazla kapasite sağlayın.

Bazı durumlarda, Azure hizmetlerine başvurmak için dış uç noktaları kullanmak faydalıdır. Örnekler için [SSS](traffic-manager-faqs.md#traffic-manager-endpoints) bölümüne bakın. Durum denetimleri, dış bitiş noktaları üzerinden değil, Azure uç noktaları ücretine göre faturalandırılır. Azure uç noktalarından farklı olarak, temel alınan hizmeti durdurup silerseniz, sistem durumu denetimi faturalandırmaya devam eder. Traffic Manager, uç noktayı devre dışı bıraktıktan veya sildikten sonra faturalandırma durdurulur.

## <a name="nested-endpoints"></a>İç içe geçmiş uç noktalar

İç içe geçmiş uç noktalar, daha büyük ve karmaşık dağıtımların ihtiyaçlarını desteklemek üzere esnek trafik yönlendirme şemaları oluşturmak için birden çok Traffic Manager profilini birleştirir. Iç Içe geçmiş uç noktalar ile ' alt ' profili bir uç nokta olarak ' Parent ' profiline eklenir. Alt ve üst profiller, diğer iç içe profiller de dahil olmak üzere herhangi bir türde diğer uç noktaları içerebilir. 

Daha fazla bilgi için bkz. [iç içe Traffic Manager profilleri](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Uç noktalar olarak Web Apps

Traffic Manager Web Apps bitiş noktaları olarak yapılandırılırken bazı daha fazla dikkat edilecek noktalar geçerlidir:

1. Yalnızca ' standart ' SKU 'sunda veya yukarıdaki Web Apps Traffic Manager kullanım için uygundur. Daha düşük bir SKU 'nun Web uygulamasını ekleme girişimleri başarısız olur. Mevcut bir Web uygulamasının SKU 'SU eski sürüme düşürüyor Traffic Manager, bu Web uygulamasına artık trafik göndermemek için oluşur. Desteklenen planlar hakkında daha fazla bilgi için [App Service planlarına](https://azure.microsoft.com/pricing/details/app-service/plans/) bakın
2. Bir uç nokta bir HTTP isteği aldığında, isteği hangi Web uygulamasının hizmet etmesi gerektiğini belirleyen istekteki ' host ' üst bilgisini kullanır. Ana bilgisayar üst bilgisi, isteği başlatmak için kullanılan DNS adını içerir, örneğin ' contosoapp.azurewebsites.net '. Web uygulamanızla farklı bir DNS adı kullanmak için, DNS adının uygulama için özel bir etki alanı adı olarak kaydedilmesi gerekir. Bir Web uygulaması uç noktasını Azure uç noktası olarak eklerken, uygulama için Traffic Manager profili DNS adı otomatik olarak kaydedilir. Uç nokta silindiğinde bu kayıt otomatik olarak kaldırılır.
3. Her bir Traffic Manager profili, her bir Azure bölgesinden en fazla bir Web uygulaması uç noktasına sahip olabilir. Bu kısıtlamayı geçici olarak çözmek için bir Web uygulamasını dış uç nokta olarak yapılandırabilirsiniz. Daha fazla bilgi için bkz. [SSS](traffic-manager-faqs.md#traffic-manager-endpoints).

## <a name="enabling-and-disabling-endpoints"></a>Uç noktaları etkinleştirme ve devre dışı bırakma

Traffic Manager bir uç noktanın devre dışı bırakılması, trafiği bakım modunda veya yeniden dağıtılmakta olan bir uç noktadan geçici olarak kaldırmak için yararlı olabilir. Uç nokta yeniden çalışmaya başladıktan sonra yeniden etkinleştirilebilir.

Uç noktaları Traffic Manager Portal, PowerShell, CLı veya REST API aracılığıyla etkinleştirebilir veya devre dışı bırakabilirsiniz.

> [!NOTE]
> Azure uç noktasının devre dışı bırakılması, Azure 'daki dağıtım durumuyla ilgili hiçbir şey yapmaz. Bir Azure hizmeti (örneğin, bir VM veya Web uygulaması gibi) çalışır durumda kalır ve Traffic Manager devre dışı bırakıldığında bile trafiği alabilecektir. Trafik, Traffic Manager profili DNS adı üzerinden değil, doğrudan hizmet örneğine bağlanabilir. Daha fazla bilgi için bkz. [Traffic Manager nasıl kullanılır](traffic-manager-how-it-works.md).

Trafiği almak için her uç noktanın geçerli uygunluğu aşağıdaki etkenlere bağlıdır:

* Profil durumu (etkin/devre dışı)
* Uç nokta durumu (etkin/devre dışı)
* Bu uç nokta için durum denetimlerinin sonuçları

Ayrıntılar için bkz. [Traffic Manager uç nokta izleme](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> Traffic Manager DNS düzeyinde çalıştığından, herhangi bir uç noktaya ait varolan bağlantıları etkilemez. Bir uç nokta kullanılamadığında Traffic Manager yeni bağlantıları kullanılabilir başka bir uç noktaya yönlendirir. Ancak, devre dışı veya sağlıksız uç noktanın arkasındaki konak, Bu oturumlar sonlandırılana kadar mevcut bağlantılar aracılığıyla trafik almaya devam edebilir. Uygulamalar, trafiğin mevcut bağlantılardan boşalmaya izin vermek için oturum süresini sınırlımalıdır.

Bir profildeki tüm uç noktalar devre dışıysa veya profilin kendisi devre dışıysa, Traffic Manager yeni bir DNS sorgusuna ' NXDOMAIN ' yanıtı gönderir.

## <a name="faqs"></a>SSS

* [Birden çok abonelikteki uç noktalarla Traffic Manager kullanabilir miyim?](./traffic-manager-faqs.md#can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions)

* [Bulut hizmeti ' hazırlama ' yuvaları ile Traffic Manager kullanabilir miyim?](./traffic-manager-faqs.md#can-i-use-traffic-manager-with-cloud-service-staging-slots)

* [Traffic Manager IPv6 uç noktalarını destekliyor mu?](./traffic-manager-faqs.md#does-traffic-manager-support-ipv6-endpoints)

* [Aynı bölgede birden fazla Web uygulamasıyla Traffic Manager kullanabilir miyim?](./traffic-manager-faqs.md#can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region)

* [Traffic Manager profillerinin Azure uç noktalarını farklı bir kaynak grubuna taşımak Nasıl yaparım??](./traffic-manager-faqs.md#how-do-i-move-my-traffic-manager-profiles-azure-endpoints-to-a-different-resource-group-or-subscription)

## <a name="next-steps"></a>Sonraki adımlar

* [Traffic Manager nasıl çalıştığını](traffic-manager-how-it-works.md)öğrenin.
* Traffic Manager [uç nokta izleme ve otomatik yük devretme](traffic-manager-monitoring.md)hakkında bilgi edinin.
* Traffic Manager [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md)hakkında bilgi edinin.