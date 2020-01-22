---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 3c1e34bb418f9be2e26afc117343f1fa50bd8566
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76309013"
---
Bu SSS, klasik dağıtım modelini kullanan P2S bağlantıları için geçerlidir.

### <a name="what-client-operating-systems-can-i-use-with-point-to-site"></a>Noktadan Siteye ile hangi istemci işletim sistemlerini kullanabilirim?

Aşağıdaki istemci işletim sistemleri desteklenmektedir:

* Windows 7 (32 bit ve 64 bit)
* Windows Server 2008 R2 (yalnızca 64 bit)
* Windows 8 (32 bit ve 64 bit)
* Windows 8.1 (32 bit ve 64 bit)
* Windows Server 2012 (yalnızca 64 bit)
* Windows Server 2012 R2 (yalnızca 64 bit)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-that-supports-sstp-for-point-to-site"></a>Noktadan siteye yönelik SSTP 'yi destekleyen herhangi bir yazılım VPN istemcisi kullanabilir miyim?

Hayır. Destek yalnızca listelenen Windows işletim sistemi sürümleriyle sınırlıdır.

### <a name="how-many-vpn-client-endpoints-can-exist-in-my-point-to-site-configuration"></a>Noktadan siteye yapılandırmanızda kaç VPN istemci uç noktası bulunabilir?

VPN istemci uç noktalarının miktarı, ağ geçidinizin SKU ve protokolüne bağlıdır.
[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Noktadan Siteye bağlanabilirlik için kendi iç PKI kök CA’mı kullanabilir miyim?

Evet. Önceden, yalnızca otomatik olarak imzalanan kök sertifikalar kullanılabiliyordu. En fazla 20 kök sertifika yüklemeye devam edebilirsiniz.

### <a name="can-i-traverse-proxies-and-firewalls-by-using-point-to-site"></a>Noktadan siteye kullanarak proxy ve güvenlik duvarları arasında geçiş yapabilir miyim?

Evet. Güvenli Yuva Tünel Protokolü (SSTP) kullanarak güvenlik duvarları aracılığıyla tünel oluşturma. Bu tünel bir HTTPS bağlantısı olarak görünür.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Noktadan Siteye için yapılandırılmış istemci bilgisayarını yeniden başlatırsam VPN de otomatik olarak yeniden bağlanacak mı?

Varsayılan olarak, istemci bilgisayar VPN bağlantısını otomatik olarak yeniden kurmayacaktır.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Noktadan siteye destek otomatik yeniden bağlanmayı ve VPN istemcilerinde DDNS 'i destekliyor mu?

Hayır. Otomatik yeniden bağlanma ve DDNS Şu anda Noktadan siteye VPN 'lerde desteklenmiyor.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-for-the-same-virtual-network"></a>Aynı sanal ağ için siteden siteye ve Noktadan siteye yapılandırmalara sahip olabilir miyim?

Evet. Ağ geçidiniz için bir RouteBased VPN türü varsa her iki çözüm de çalışacaktır. Klasik dağıtım modeli için dinamik bir ağ geçidiniz olması gerekir. Statik yönlendirme VPN ağ geçitleri veya **-vpntype PolicyBased** cmdlet kullanan ağ geçitleri Için Noktadan siteye hizmeti desteklemiyoruz.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Aynı anda birden çok sanal ağa bağlanmak için Noktadan Siteye istemcisi yapılandırabilir miyim?

Evet. Ancak, sanal ağlar çakışan IP öneklerine sahip olamaz ve Noktadan siteye adres alanları sanal ağlar arasında çakışmamalıdır.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Siteden Siteye ve Noktadan Siteye bağlantılardan ne kadar verimlilik bekleyebilirim?

VPN tünellerinin tam verimini elde etmek zordur. IPsec ve SSTP şifrelemesi ağır VPN protokolleridir. Verimlilik, şirket içi ve internet arasındaki gecikme ve bant genişliğine göre de sınırlandırılır.
