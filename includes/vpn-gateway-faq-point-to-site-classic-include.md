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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
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

### <a name="can-i-use-any-software-vpn-client-that-supports-sstp-for-point-to-site"></a>Siteye Puan için SSTP'yi destekleyen herhangi bir yazılım VPN istemcisi kullanabilir miyim?

Hayır. Destek yalnızca listelenen Windows işletim sistemi sürümleriyle sınırlıdır.

### <a name="how-many-vpn-client-endpoints-can-exist-in-my-point-to-site-configuration"></a>Noktaya Sayfa yapılandırmamda kaç VPN istemci uç noktası olabilir?

VPN istemci uç noktalarının miktarı ağ geçidi sku ve protokolüne bağlıdır.
[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Noktadan Siteye bağlanabilirlik için kendi iç PKI kök CA’mı kullanabilir miyim?

Evet. Önceden, yalnızca otomatik olarak imzalanan kök sertifikalar kullanılabiliyordu. Yine de en fazla 20 kök sertifika yükleyebilirsiniz.

### <a name="can-i-traverse-proxies-and-firewalls-by-using-point-to-site"></a>Siteden Siteye'i kullanarak yakınlık ve güvenlik duvarlarını geçiş yapabilir miyim?

Evet. Güvenlik duvarlarından tünel kazmak için Güvenli Soket Tünel Protokolü 'nü (SSTP) kullanıyoruz. Bu tünel bir HTTPS bağlantısı olarak görünür.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Noktadan Siteye için yapılandırılmış istemci bilgisayarını yeniden başlatırsam VPN de otomatik olarak yeniden bağlanacak mı?

Varsayılan olarak, istemci bilgisayar VPN bağlantısını otomatik olarak yeniden kurmaz.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>VPN istemcilerinde Otomatik yeniden bağlanmayı ve DDNS'yi destekliyor mu?

Hayır. Otomatik yeniden bağlanma ve DDNS şu anda Yer Noktadan Siteye VPN'lerde desteklenmez.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-for-the-same-virtual-network"></a>Aynı sanal ağ için Siteden Siteye ve Site'den Siteye yapılandırmaları alabilir miyim?

Evet. Ağ geçidiniz için RouteBased VPN türü varsa, her iki çözüm de çalışır. Klasik dağıtım modeli için dinamik bir ağ geçidiniz olması gerekir. **-VpnType PolicyBased** cmdlet kullanan statik yönlendirme VPN ağ geçitleri veya ağ geçitleri için Site'ye Noktadan Sayfa'yı desteklemeyiz.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Aynı anda birden çok sanal ağa bağlanmak için Noktadan Siteye istemcisi yapılandırabilir miyim?

Evet. Ancak, sanal ağlar çakışan IP önekleri olamaz ve Site'ye Nokta lı adres boşlukları sanal ağlar arasında örtüşmemelidir.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Siteden Siteye ve Noktadan Siteye bağlantılardan ne kadar verimlilik bekleyebilirim?

VPN tünellerinin tam verimini elde etmek zordur. IPsec ve SSTP şifrelemesi ağır VPN protokolleridir. İş işleri, binanız ve ınız arasındaki gecikme ve bant width ile de sınırlılır.
