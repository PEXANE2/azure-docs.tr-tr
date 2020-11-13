---
title: Azure DDoS koruması standart sık sorulan sorular
description: Azure DDoS koruma standardı hakkında sık sorulan sorular ve bu, DDoS saldırılarına karşı savunma sağlamaya yardımcı olur.
services: virtual-network
documentationcenter: na
author: yitoh
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/28/2020
ms.author: yitoh
ms.openlocfilehash: ed524642dc53835e686f52b53cfce0c16fb56377
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94579105"
---
# <a name="azure-ddos-protection-standard-frequent-asked-questions"></a>Azure DDoS koruması standart sık sorulan sorular

Bu makalede, Azure DDoS koruma standardı hakkında sık sorulan sorular yanıtlanmaktadır. 

## <a name="what-is-a-distributed-denial-of-service-ddos-attack"></a>Dağıtılmış hizmet reddi (DDoS) saldırısı nedir?
Dağıtılmış hizmet reddi veya DDoS, bir saldırganın uygulamanın işleme yeteneğine göre bir uygulamaya daha fazla istek gönderdiği bir saldırı türüdür. Sonuçta elde edilen etki, uygulamanın kullanılabilirliğini ve müşterilerine hizmet verme yeteneğini etkileyen kaynakların boşalmakta olmasıyla sonuçlanır. Son birkaç yılda, sektörde daha karmaşık ve daha büyük bir şekilde saldırması sayesinde sektör bir keskin artış gördük. DDoS saldırıları, Internet üzerinden genel olarak erişilebilen herhangi bir uç noktada hedeflenebilir.

## <a name="what-is-azure-ddos-protection-standard-service"></a>Azure DDoS koruması standart hizmeti nedir?
Azure DDoS koruma standardı, uygulama tasarımı en iyi uygulamaları ile birlikte, DDoS saldırılarına karşı savunmak için gelişmiş DDoS azaltma özellikleri sağlar. Bir sanal ağdaki belirli Azure kaynaklarınızı korumaya yardımcı olmak üzere otomatik olarak ayarlanır. Korumanın, yeni veya mevcut bir sanal ağda etkinleştirilmesi basittir ve uygulama veya kaynak değişikliği gerektirmez. Günlük, uyarı ve telemetri dahil olmak üzere temel hizmet üzerinde çeşitli avantajları vardır. Bkz. [Azure DDoS koruması standardına genel bakış](ddos-protection-overview.md). 

## <a name="what-about-protection-at-the-service-layer-layer-7"></a>Hizmet katmanında (katman 7) koruma hakkında ne olacak?
Müşteriler, hem Ağ katmanında (katman 3 ve 4, Azure DDoS koruma hizmeti tarafından sunulan) hem de uygulama katmanında (katman 7 Application Gateway WAF SKU 'SU tarafından sunulan) koruma için [Application Gateway WAF SKU 'su](https://docs.microsoft.com/azure/web-application-firewall/ag/ag-overview) Ile birlikte Azure DDoS koruma hizmeti 'ni kullanabilir.

## <a name="are-services-unsafe-in-azure-without-the-service"></a>Hizmetler Azure 'da hizmet olmadan güvenli değil mi?
Azure üzerinde çalışan hizmetler, Azure 'un altyapısını korumak için kullanılan Azure DDoS koruma temel alınarak kendiliğinden korunur. Ancak, altyapıyı koruyan korumanın birçok uygulamanın işleme kapasitesi vardır ve telemetri ya da uyarı sağlamasını sağlar; bu nedenle bir trafik birimi platform tarafından zararsız olarak algılanır ve bu sayede, uygulamayı alan uygulama mal olabilir. 

Uygulama, Azure DDoS koruma standart hizmetine ekleme yaparak saldırıları ve uygulamaya özel eşikleri tespit etmek için adanmış izleme alır. Bir hizmet, DDoS saldırılarına karşı daha sıkı bir savunma sağlayan, beklenen trafik hacmine ayarlanmış bir profille korunacaktır.

## <a name="what-are-the-supported-protected-resource-types"></a>Desteklenen korumalı kaynak türleri nelerdir?
ARM tabanlı VNET 'Lerdeki genel IP 'Ler Şu anda korunan kaynak türüdür. PaaS hizmetleri (Multitenant) sunulduğundan desteklenmez. Bkz. [Azure DDoS koruması standart başvuru mimarileri](ddos-protection-reference-architectures.md).

## <a name="are-classicrdfe-protected-resources-supported"></a>Klasik/RDFE korumalı Kaynaklar destekleniyor mu?
Önizleme sürümünde yalnızca ARM tabanlı korumalı Kaynaklar desteklenir. Klasik/RDFE dağıtımlarında VM 'Ler desteklenmez. Klasik/RDFE kaynakları için destek şu anda planlanmamaktadır. Bkz. [Azure DDoS koruması standart başvuru mimarileri](ddos-protection-reference-architectures.md).

## <a name="can-i-protect-my-on-premise-resources-using-ddos-protection"></a>DDoS korumasını kullanarak şirket içi kaynaklarımı koruyabilir miyim?
DDoS koruması için etkinleştirmek üzere, Azure 'daki bir VNet ile ilişkili hizmetinizin genel uç noktalarına sahip olmanız gerekir. Örnek tasarımlar şunları içerir:
- Azure 'da Web siteleri (IaaS) ve şirket içi veri merkezindeki arka uç veritabanları. 
- Azure 'daki Application Gateway (App Gateway/WAF üzerinde etkinleştirilmiş DDoS koruması) ve şirket içi veri merkezlerinde Web siteleri.

Bkz. [Azure DDoS koruması standart başvuru mimarileri](ddos-protection-reference-architectures.md).

## <a name="can-i-register-a-domain-outside-of-azure-and-associate-that-to-a-protected-resource-like-vm-or-elb"></a>Bir etki alanını Azure dışında kaydedebilir ve bunu VM veya ELB gibi korunan bir kaynakla ilişkilendirebilirsiniz miyim?
Genel IP senaryolarında, DDoS Koruması hizmeti ilişkili etki alanının kaydedildiği veya ilişkili genel IP 'nin Azure 'da barındırıldığı sürece barındırıldığından bağımsız olarak tüm uygulamaları destekleyecektir. 

## <a name="can-i-manually-configure-the-ddos-policy-applied-to-the-vnetspublic-ips"></a>Sanal ağlar/genel IP 'lere uygulanan DDoS ilkesini el ile yapılandırabilir miyim?
Hayır, ilke özelleştirmesi Şu anda kullanılamıyor.

## <a name="can-i-allowlistbloclist-specific-ip-addresses"></a>Belirli IP adreslerini listeleme/engelleme yapabilir miyim?
Hayır, el ile yapılandırma Şu anda kullanılamıyor.

## <a name="how-can-i-test-ddos-protection"></a>DDoS korumasını nasıl test edebilirim?
Bkz. [Benzetim aracılığıyla test etme](test-through-simulations.md).

## <a name="how-long-does-it-take-for-the-metrics-to-load-on-portal"></a>Ölçümlerin portala yüklenmesi ne kadar sürer?
Ölçümler, 5 dakika içinde portalda görünür olmalıdır. Kaynağınız saldırı altındaysa, diğer ölçümler Portal üzerinde 5-7 dakika içinde gösterilmeyecektir. 
    



