---
title: Azure Trafik Yöneticisi nasıl çalışır | Microsoft Dokümanlar
description: Bu makale, Trafik Yöneticisi'nin yüksek performans ve web uygulamalarınızın kullanılabilirliği için trafiği nasıl güzergahlar olduğunu anlamanıza yardımcı olacaktır
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/05/2019
ms.author: rohink
ms.openlocfilehash: 4863ffd383cfcd46bad462156e26293d145fd418
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294862"
---
# <a name="how-traffic-manager-works"></a>Trafik Yöneticisi Nasıl Çalışır?

Azure Trafik Yöneticisi, uygulama bitiş noktalarınız arasında trafik dağılımını denetlemenize olanak tanır. Uç nokta, Azure içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir.

Trafik Yöneticisi iki önemli avantaj sağlar:

- Trafiğin çeşitli trafik yönlendirme [yöntemlerinden](traffic-manager-routing-methods.md) birine göre dağılımı
- [Uç nokta sonu durumunun sürekli izlenmesi](traffic-manager-monitoring.md) ve bitiş noktaları başarısız olduğunda otomatik arıza

İstemcinin bir hizmete bağlanabilmesi için önce o hizmetin DNS adını bir IP adresine çözümlemesi gerekir. İstemci bu işlemin ardından ilgili IP adresine bağlanarak hizmete erişebilir.

**Anlaşılması gereken en önemli nokta Trafik Yöneticisi'nin DNS düzeyinde çalıştığıdır.**  Trafik Yöneticisi, trafiği yönlendirme yönteminin kurallarına göre istemcileri belirli hizmet bitiş noktalarına yönlendirmek için DNS kullanır. İstemciler seçilen bitiş noktasına **doğrudan**bağlanır. Trafik Yöneticisi bir proxy veya ağ geçidi değildir. Trafik Yöneticisi, istemci ile hizmet arasında geçen trafiği görmez.

## <a name="traffic-manager-example"></a>Trafik Yöneticisi örneği

Contoso Corp yeni bir ortak portalı geliştirdi. Bu portalın URL'si `https://partners.contoso.com/login.aspx`. Uygulama Azure'un üç bölgesinde barındırılır. Kullanılabilirliği artırmak ve genel performansı en üst düzeye çıkarmak için, istemci trafiğini kullanılabilir en yakın bitiş noktasına dağıtmak için Trafik Yöneticisi'ni kullanırlar.

Bu yapılandırmayı gerçekleştirmek için aşağıdaki adımları tamamlarlar:

1. Hizmetlerinin üç örneğini dağıtın. Bu dağıtımların DNS adları 'contoso-us.cloudapp.net', 'contoso-eu.cloudapp.net' ve 'contoso-asia.cloudapp.net' dir.
1. 'contoso.trafficmanager.net' adlı bir Trafik Yöneticisi profili oluşturun ve üç uç nokta boyunca 'Performans' trafik yönlendirme yöntemini kullanacak şekilde yapılandırın.
1. DNS CNAME kaydı nı kullanarak, 'contoso.trafficmanager.net' işaret etmek için kibir etki alanı adlarını 'partners.contoso.com' olarak yapılandırın.

![Trafik Yöneticisi DNS yapılandırması][1]

> [!NOTE]
> Azure Trafik Yöneticisi ile bir makyaj etki alanı kullanırken, makyaj etki alanı adınızı Trafik Yöneticisi etki alanı adınıza çekmek için bir CNAME kullanmanız gerekir. DNS standartları, bir etki alanının 'tepe noktasında' (veya kökünde) bir CNAME oluşturmanıza izin vermez. Bu nedenle 'contoso.com' (bazen 'çıplak' etki alanı olarak adlandırılır) için bir CNAME oluşturamazsınız. Bir etki alanı için yalnızca 'contoso.com' altında bir CNAME oluşturabilirsiniz, örneğin 'www.contoso.com.' Bu sınırlamayı aşmak için, DNS etki alanınızı [Azure DNS'de](../dns/dns-overview.md) barındırmanızı ve trafik yöneticisi profilinizi belirtmek için [Alias kayıtlarını](../dns/tutorial-alias-tm.md) kullanmanızı öneririz. Alternatif olarak, 'contoso.com' isteklerini 'www.contoso.com' gibi alternatif bir ada yönlendirmek için basit bir HTTP yönlendirmesi kullanabilirsiniz.

### <a name="how-clients-connect-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanarak istemciler nasıl bağlanır?

Önceki örnekten devam eden, bir istemci `https://partners.contoso.com/login.aspx`sayfayı istediğinde, istemci DNS adını çözmek ve bir bağlantı kurmak için aşağıdaki adımları gerçekleştirir:

![Trafik Yöneticisi'ni kullanarak bağlantı kurma][2]

1. İstemci, 'partners.contoso.com' adını çözmek için yapılandırılan özyinelemeli DNS hizmetine bir DNS sorgusu gönderir. Bazen 'yerel DNS' hizmeti olarak da adlandırılan özyinelemeli Bir DNS hizmeti, Doğrudan DNS etki alanlarını barındırmaz. Bunun yerine, istemci, Bir DNS adını çözmek için gereken Çeşitli Yetkili DNS hizmetleriyle Internet üzerinden iletişim kurma işini boşaltır.
2. DNS adını gidermek için, özyinelemeli DNS hizmeti 'contoso.com' etki alanının ad sunucularını bulur. Daha sonra 'partners.contoso.com' DNS kaydını istemek için bu ad sunucularıyla bağlantı kurun. contoso.com DNS sunucuları, contoso.trafficmanager.net işaret eden CNAME kaydını döndürür.
3. Daha sonra, özyinelemeli DNS hizmeti, Azure Trafik Yöneticisi hizmeti tarafından sağlanan 'trafficmanager.net' etki alanının ad sunucularını bulur. Daha sonra bu DNS sunucularına 'contoso.trafficmanager.net' DNS kaydı için bir istek gönderir.
4. Trafik Yöneticisi ad sunucuları isteği alır. Bir bitiş noktası seçerler:

    - Her bitiş noktasının yapılandırılan durumu (devreden uç noktaları döndürülmez)
    - Trafik Yöneticisi sağlık kontrolleri tarafından belirlenen her bitiş noktasının geçerli durumu. Daha fazla bilgi için [Trafik Yöneticisi Uç Nokta İzleme'ye](traffic-manager-monitoring.md)bakın.
    - Seçilen trafik yönlendirme yöntemi. Daha fazla bilgi için [Trafik Yöneticisi Yönlendirme Yöntemleri'ne](traffic-manager-routing-methods.md)bakın.

5. Seçilen bitiş noktası başka bir DNS CNAME kaydı olarak döndürülür. Bu durumda, contoso-us.cloudapp.net döndürüldişe izin verin.
6. Ardından, özyinelemeli DNS hizmeti 'cloudapp.net' etki alanının ad sunucularını bulur. 'contoso-us.cloudapp.net' DNS kaydını istemek için bu ad sunucularına datemas eder. ABD merkezli hizmet bitiş noktasının IP adresini içeren bir DNS 'A' kaydı döndürülür.
7. Özyinelemeli DNS hizmeti sonuçları birleştirir ve istemciye tek bir DNS yanıtı döndürür.
8. İstemci DNS sonuçlarını alır ve verilen IP adresine bağlanır. İstemci, Trafik Yöneticisi aracılığıyla değil, doğrudan uygulama hizmeti bitiş noktasına bağlanır. Https bitiş noktası olduğundan, istemci gerekli SSL/TLS el sıkışmasını gerçekleştirir ve ardından '/login.aspx' sayfası için HTTP GET isteğinde bulundu.

Özyinelemeli DNS hizmeti, aldığı DNS yanıtlarını önbelleğe alır. İstemci aygıtındaki DNS çözümleyicisi de sonucu önbelleğe atanır. Önbelleğe alma, sonraki DNS sorgularının diğer ad sunucularını sorgulamak yerine önbellekteki verileri kullanarak daha hızlı yanıtlanmasını sağlar. Önbelleğin süresi, her DNS kaydının 'canlı' (TTL) özelliğitarafından belirlenir. Daha kısa değerler, önbelleğe daha hızlı önbellek sona erme yle ve böylece Trafik Yöneticisi ad sunucularına daha fazla gidiş-dönüş le sonuçlanır. Daha uzun değerler, trafiği başarısız bir bitiş noktasından uzağa yönlendirmenin daha uzun sürebileceği anlamına gelir. Trafik Yöneticisi, Traffic Manager DNS yanıtlarında kullanılan TTL'yi 0 saniyeye kadar ve 2.147.483.647 saniye [(RFC-1035](https://www.ietf.org/rfc/rfc1035.txt)ile uyumlu maksimum aralık) kadar yüksek olacak şekilde yapılandırmanızı sağlar ve uygulamanızın gereksinimlerini en iyi dengeleyen değeri seçmenize olanak tanır.

## <a name="faqs"></a>SSS

* [Trafik Yöneticisi hangi IP adresini kullanır?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-ip-address-does-traffic-manager-use)

* [Trafik Yöneticisi kullanılarak ne tür trafik yönlendirilebilir?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-types-of-traffic-can-be-routed-using-traffic-manager)

* [Trafik Yöneticisi "yapışkan" oturumları destekliyor mu?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-traffic-manager-support-sticky-sessions)

* [Trafik Yöneticisi'ni kullanırken neden bir HTTP hatası görüyorum?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#why-am-i-seeing-an-http-error-when-using-traffic-manager)

* [Trafik Yöneticisi'ni kullanmanın performans etkisi nedir?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-is-the-performance-impact-of-using-traffic-manager)

* [Trafik Yöneticisi ile hangi uygulama protokollerini kullanabilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-application-protocols-can-i-use-with-traffic-manager)

* [Trafik Yöneticisi'ni "çıplak" alan adı ile kullanabilir miyim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-traffic-manager-with-a-naked-domain-name)

* [Trafik Yöneticisi, DNS sorgularını işlerken istemci alt net adresini dikkate alır mı?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries)

* [DNS TTL nedir ve kullanıcılarımı nasıl etkiler?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-is-dns-ttl-and-how-does-it-impact-my-users)

* [Trafik Yöneticisi yanıtları için TTL'yi ne kadar yüksek veya düşük ayarlayabilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses)

* [Profilime gelen sorguların hacmini nasıl anlayabilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-understand-the-volume-of-queries-coming-to-my-profile)

## <a name="next-steps"></a>Sonraki adımlar

Trafik Yöneticisi [uç nokta izleme ve otomatik arıza](traffic-manager-monitoring.md)hakkında daha fazla bilgi edinin.

Trafik Yöneticisi [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md)hakkında daha fazla bilgi edinin.

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

