---
title: Trafik Yöneticisi Uç Nokta Türleri | Microsoft Dokümanlar
description: Bu makalede, Azure Trafik Yöneticisi ile kullanılabilecek farklı uç nokta türleri açıklanmaktadır
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: rohink
ms.openlocfilehash: 3d8f899a7899243129d31c2620a51dc764a8e917
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250939"
---
# <a name="traffic-manager-endpoints"></a>Traffic Manager uç noktaları

Microsoft Azure Trafik Yöneticisi, ağ trafiğinin farklı veri merkezlerinde çalışan uygulama dağıtımlarına nasıl dağıtıldığını denetlemenize olanak tanır. Her uygulama dağıtımını Traffic Manager'da bir 'uç nokta' olarak yapılandırırsınız. Traffic Manager bir DNS isteği aldığında, DNS yanıtında geri döndürmek için kullanılabilir bir uç nokta seçer. Trafik yöneticisi seçimi geçerli bitiş noktası durumuna ve trafik yönlendirme yöntemine temel adadır. Daha fazla bilgi için [Trafik Yöneticisi Nasıl Çalışır.](traffic-manager-how-it-works.md)

Traffic Manager tarafından desteklenen üç tür uç nokta vardır:

* **Azure uç noktaları**, Azure'da barındırılan hizmetler için kullanılır.
* **Dış uç noktalar**; IPv4/IPv6 adresleri, FQDN’ler ya da Azure dışında barındırılan şirket içindeki veya farklı bir barındırma sağlayıcısındaki hizmetler için kullanılır.
* **İç içe uç noktalar** ise daha büyük ve daha karmaşık dağıtımların ihtiyaçlarını desteklemek üzere daha esnek trafik yönlendirme şemaları oluşturmak amacıyla Traffic Manager profillerini birleştirmek için kullanılır.

Farklı türlerdeki uç noktaların tek bir Traffic Manager profilinde nasıl birleştirileceği konusunda bir kısıtlama yoktur. Her profil, uç nokta türlerinin herhangi bir karışımını içerebilir.

Aşağıdaki bölümlerde her uç nokta türü daha derinlemesine açıklayınız.

## <a name="azure-endpoints"></a>Azure uç noktaları

Azure uç noktaları Trafik Yöneticisi'ndeki Azure tabanlı hizmetler için kullanılır. Aşağıdaki Azure kaynak türleri desteklenir:

* PaaS bulut hizmetleri.
* Web Apps
* Web Uygulama Yuvaları
* PublicIPAddress kaynakları (doğrudan veya Azure Yük Bakiyesi aracılığıyla VM'lere bağlanabilen). PublicIpAddress'in Trafik Yöneticisi profilinde kullanılmak üzere atanmış bir DNS adı olmalıdır.

PublicIPAddress kaynakları Azure Kaynak Yöneticisi kaynaklarıdır. Bunlar klasik dağıtım modelinde yok. Böylece yalnızca Trafik Yöneticisi'nin Azure Kaynak Yöneticisi deneyimlerinde desteklenirler. Diğer uç nokta türleri hem Kaynak Yöneticisi hem de klasik dağıtım modeli aracılığıyla desteklenir.

Azure uç noktalarını kullanırken, Trafik Yöneticisi bir Web Uygulamasının durdurulduğunu ve başlatıldığında algılar. Bu durum bitiş noktası durumuna yansıtılır. Ayrıntılar için [Trafik Yöneticisi uç nokta izleme](traffic-manager-monitoring.md#endpoint-and-profile-status) konusuna bakın. Temel hizmet durdurulduğunda, Trafik Yöneticisi uç nokta sağlık denetimleri gerçekleştirmez veya trafiği bitiş noktasına yönlendirmez. Durdurulan örnek için Trafik Yöneticisi faturalandırma olayları oluşmaz. Hizmet yeniden başlatıldığında, faturalandırma devam eder ve bitiş noktası trafik almaya uygundur. Bu algılama PublicIpAddress uç noktaları için geçerli değildir.

## <a name="external-endpoints"></a>Dış uç noktalar

Dış uç noktalar IPv4/IPv6 adresleri, FQDN'ler veya Azure dışındaki hizmetler için kullanılır. IPv4/IPv6 adres uç noktalarının kullanımı, trafik yöneticisinin son noktaların durumunu onlar için Bir DNS adı gerektirmeden denetlemesine olanak tanır. Sonuç olarak, Trafik Yöneticisi bir yanıtta bu bitiş noktasını döndürerken A/AAAA kayıtları olan sorguları yanıtlayabilir. Azure dışındaki hizmetler, şirket içinde veya farklı bir sağlayıcıyla barındırılan bir hizmeti içerebilir. IPv4 veya IPv6 adresleri olarak belirtilen uç noktalar dışında, yalnızca dış uç noktaları olabilecek uç noktalar dışında, dış uç noktalar tek tek veya aynı Trafik Yöneticisi profilinde Azure Uç Noktaları ile birleştirilebilir. Azure uç noktalarını Dış uç noktalarıyla birleştirmek çeşitli senaryolara olanak sağlar:

* Azure'u kullanarak etkin etkin veya etkin-pasif bir başarısız modelde varolan bir şirket içi uygulama için artırılmış artıklık sağlayın. 
* Trafiği, kendileriyle ilişkili bir DNS adı olmayan uç noktalarına yönlendirin. Buna ek olarak, bir DNS adının IP adresini almak için ikinci bir DNS sorgusu çalıştırma gereksinimini kaldırarak genel DNS arama gecikmesini azaltın.
* Dünyanın dört bir yanındaki kullanıcılar için uygulama gecikmesini azaltın, mevcut bir şirket içi uygulamayı Azure'daki ek coğrafi konumlara genişletin. Daha fazla bilgi için [Trafik Yöneticisi 'Performans' trafik](traffic-manager-routing-methods.md#performance)yönlendirmesi'ne bakın.
* Azure'u kullanarak talepteki ani artışı karşılamak için sürekli olarak veya 'buluta patlama' çözümü olarak varolan bir şirket içi uygulama için ek kapasite sağlayın.

Bazı durumlarda, Azure hizmetlerine başvurmak için Dış uç noktaları kullanmak yararlıdır (örneğin, [SSS'ye](traffic-manager-faqs.md#traffic-manager-endpoints)bakın). Bu durumda, sistem durumu denetimleri Dış uç puan oranıyla değil, Azure uç noktaları oranında faturalandırılır. Ancak, Azure uç noktalarının aksine, temel hizmeti durdurur veya silerseniz, Trafik Yöneticisi'ndeki bitiş noktasını devre dışı bırakana veya silene kadar sistem durumu denetimi faturalandırması devam eder.

## <a name="nested-endpoints"></a>İç içe uç noktalar

İç içe uç noktalar, esnek trafik yönlendirme düzenleri oluşturmak ve daha büyük, karmaşık dağıtımların gereksinimlerini desteklemek için birden çok Trafik Yöneticisi profilini birleştirir. İç içe uç noktaları yla, 'üst' profiline bitiş noktası olarak bir 'alt' profil eklenir. Hem alt hem de üst profiller, iç içe geçen diğer profiller de dahil olmak üzere her türden diğer uç noktaları içerebilir. Daha fazla bilgi için [iç içe olan Trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md)bakın.

## <a name="web-apps-as-endpoints"></a>Uç nokta olarak Web Uygulamaları

Trafik Yöneticisi'nde Web Apps'ı bitiş noktası olarak yapılandırırken bazı ek hususlar geçerlidir:

1. Yalnızca 'Standart' SKU veya üzeri Web Uygulamaları Traffic Manager ile kullanıma uygundur. Daha düşük bir SKU'nun Web Uygulamasını ekleme girişimleri başarısız oldu. Varolan bir Web Uygulamasının SKU'su düşürülerek Trafik Yöneticisi'nin artık o Web Uygulamasına trafik göndermemesi sonuçlanır. Desteklenen planlar hakkında daha fazla bilgi için [Uygulama Hizmet Planları'na](https://azure.microsoft.com/pricing/details/app-service/plans/) bakın
2. Bir bitiş noktası bir HTTP isteği aldığında, hangi Web Uygulamasının isteğe hizmet etmesi gerektiğini belirlemek için istekteki 'ana bilgisayar' üstbilgisini kullanır. Ana bilgisayar üstbilgisi, isteği başlatmak için kullanılan DNS adını (örneğin, 'contosoapp.azurewebsites.net' içerir. Web Uygulamanızla farklı bir DNS adı kullanmak için, DNS adının Uygulama için özel bir etki alanı adı olarak kaydedilmesi gerekir. Azure bitiş noktası olarak bir Web Uygulaması bitiş noktası eklerken, Trafik Yöneticisi profili DNS adı Uygulama için otomatik olarak kaydedilir. Bitiş noktası silindiğinde bu kayıt otomatik olarak kaldırılır.
3. Her Trafik Yöneticisi profilinde her Azure bölgesinden en fazla bir Web Uygulaması bitiş noktası olabilir. Bu kısıtlamayı geçici olarak çözüme getirmek için, Bir Web App'i Harici bitiş noktası olarak yapılandırabilirsiniz. Daha fazla bilgi için [SSS](traffic-manager-faqs.md#traffic-manager-endpoints)bölümüne bakın.

## <a name="enabling-and-disabling-endpoints"></a>Uç noktaları etkinleştirme ve devre dışı bırakma

Trafik Yöneticisi'nde bir bitiş noktasının devre dışı bırakılması, trafiği bakım modunda olan veya yeniden dağıtılan bir uç noktadan geçici olarak kaldırmak için yararlı olabilir. Bitiş noktası yeniden çalıştırDıktan sonra yeniden etkinleştirilebilir.

Uç noktalar Traffic Manager portalı, PowerShell, CLI veya REST API üzerinden etkinleştirilebilir ve devre dışı edilebilir.

> [!NOTE]
> Azure bitiş noktasını devre dışı bırakmanın Azure'daki dağıtım durumuyla hiçbir ilgisi yoktur. Bir Azure hizmeti (VM veya Web Uygulaması gibi) çalışıyor ve Trafik Yöneticisi'nde devre dışı bırakılmış olsa bile trafik almaya devam eder. Trafik, Trafik Yöneticisi profili DNS adı yerine doğrudan servis örneğine yöneltilebilir. Daha fazla bilgi için [Trafik Yöneticisi'nin nasıl çalıştığını](traffic-manager-how-it-works.md)görün.

Her bitiş noktasının trafik almak için geçerli uygunluğu aşağıdaki etkenlere bağlıdır:

* Profil durumu (etkin/devre dışı bırakılmış)
* Bitiş noktası durumu (etkin/devre dışı bırakılmış)
* Bu bitiş noktası için sağlık kontrollerinin sonuçları

Ayrıntılar için [Trafik Yöneticisi uç nokta izleme](traffic-manager-monitoring.md#endpoint-and-profile-status)konusuna bakın.

> [!NOTE]
> Trafik Yöneticisi DNS düzeyinde çalıştığından, varolan bağlantıları herhangi bir uç noktaya etkileyemiyor. Bir bitiş noktası kullanılamıyorsa, Trafik Yöneticisi yeni bağlantıları kullanılabilir başka bir bitiş noktasına yönlendirir. Ancak, devre dışı veya sağlıksız bitiş noktasının arkasındaki ana bilgisayar, bu oturumlar sonlanana kadar varolan bağlantılar üzerinden trafik almaya devam edebilir. Uygulamalar, trafiğin varolan bağlantılardan akmasını sağlamak için oturum süresini sınırlamalıdır.

Profildeki tüm uç noktalar devre dışı bırakılırsa veya profilin kendisi devre dışı bırakılırsa, Trafik Yöneticisi yeni bir DNS sorgusuna 'NXDOMAIN' yanıtı gönderir.

## <a name="faqs"></a>SSS

* [Trafik Yöneticisi'ni birden çok abonelikten son puanla kullanabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions)

* [Trafik Yöneticisi'ni Bulut Hizmeti 'Evreleme' yuvalarıyla kullanabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-traffic-manager-with-cloud-service-staging-slots)

* [Trafik Yöneticisi IPv6 uç noktalarını destekliyor mu?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-traffic-manager-support-ipv6-endpoints)

* [Trafik Yöneticisi'ni aynı bölgede birden fazla Web Uygulamasıyla kullanabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region)

* [Trafik Yöneticisi profilimin Azure uç noktalarını farklı bir kaynak grubuna nasıl taşıyabilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-do-i-move-my-traffic-manager-profiles-azure-endpoints-to-a-different-resource-group-or-subscription)

## <a name="next-steps"></a>Sonraki adımlar

* [Trafik Yöneticisi'nin nasıl çalıştığını](traffic-manager-how-it-works.md)öğrenin.
* Trafik Yöneticisi [uç nokta izleme ve otomatik arıza](traffic-manager-monitoring.md)hakkında bilgi edinin.
* Trafik Yöneticisi [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md)hakkında bilgi edinin.
