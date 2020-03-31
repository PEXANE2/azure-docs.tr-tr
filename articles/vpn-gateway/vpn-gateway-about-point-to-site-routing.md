---
title: 'Azure VPN Ağ Geçidi: P2S yönlendirme hakkında'
description: Bu makale, Site'ye Nokta VPN yönlendirmenin nasıl işlediğini anlamanıza yardımcı olur.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 03/24/2020
ms.author: anzaman
ms.openlocfilehash: 4821f2eb694a36cf0570008b3e62ce39999c58d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239761"
---
# <a name="about-point-to-site-vpn-routing"></a>Noktadan Siteye VPN yönlendirme hakkında

Bu makale, Azure Noktası-To-Site VPN yönlendirmenin nasıl işlediğini anlamanıza yardımcı olur. P2S VPN yönlendirme davranışı istemci işletim sistemi bağlıdır, VPN bağlantısı için kullanılan protokol, ve nasıl sanal ağlar (VNets) birbirine bağlı.

Azure şu anda uzaktan erişim için iki protokolü, IKEv2 ve SSTP'yi destekler. IKEv2, Windows, Linux, MacOS, Android ve iOS gibi birçok istemci işletim sisteminde desteklenir. SSTP yalnızca Windows'da desteklenir. Ağınızın topolojisinde bir değişiklik yapar sanız ve Windows VPN istemcileriniz varsa, değişikliklerin istemciye uygulanabilmesi için Windows istemcileri için VPN istemci paketinin indirilmesi ve yeniden yüklenmesi gerekir.

> [!NOTE]
> Bu makale sadece IKEv2 için geçerlidir.
>

## <a name="about-the-diagrams"></a><a name="diagrams"></a>Diyagramlar hakkında

Bu makalede farklı diyagramlar bir dizi vardır. Her bölüm farklı bir topoloji veya yapılandırma gösterir. Bu makalenin amaçları doğrultusunda, Siteden Siteye (S2S) ve VNet-to-VNet bağlantıları, her ikisi de IPsec tünelleri olduğu gibi aynı şekilde çalışır. Bu makaledeki tüm VPN ağ geçitleri rota tabanlıdır.

## <a name="one-isolated-vnet"></a><a name="isolatedvnet"></a>Bir izole VNet

Bu örnekteki Noktadan Siteye VPN ağ geçidi bağlantısı, başka bir sanal ağa (VNet1) bağlı olmayan veya bakan bir VNet içindir. Bu örnekte, istemciler VNet1'e erişebilir.

![yalıtılmış VNet yönlendirme](./media/vpn-gateway-about-point-to-site-routing/1.jpg "yalıtılmış VNet yönlendirme")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

### <a name="routes-added"></a>Eklenen rotalar

* Windows istemcilerine eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows olmayan istemcilere eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

### <a name="access"></a>Erişim

* Windows istemcileri VNet1'e erişebilir

* Windows olmayan istemciler VNet1'e erişebilir

## <a name="multiple-peered-vnets"></a><a name="multipeered"></a>Birden fazla eşlenen VNets

Bu örnekte, Noktaya Sayfa VPN ağ geçidi bağlantısı VNet1 içindir. VNet1 VNet2 ile bakılır. VNet 2 VNet3 ile bakılır. VNet1 VNet4 ile bakılır. VNet1 ve VNet3 arasında doğrudan bir bakış yoktur. VNet1'de "Ağ geçidi geçişine izin ver" ve VNet2 ve VNet4 "Uzak ağ geçitlerini kullan" özelliğine sahiptir.

Windows kullanan istemciler doğrudan bakan VNet'lere erişebilir, ancak VNet'te veya ağ topolojisinde herhangi bir değişiklik yapılırsa VPN istemcisinin yeniden indirilmesi gerekir. Windows olmayan istemciler doğrudan bakan VNet'lere erişebilir. Erişim geçişli değildir ve yalnızca doğrudan bakan VNet'lerle sınırlıdır.

![birden fazla eşlenmiş VNets](./media/vpn-gateway-about-point-to-site-routing/2.jpg "birden fazla eşlenmiş VNets")

### <a name="address-space"></a>Adres alanı:

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* VNet4: 10.4.0.0/16

### <a name="routes-added"></a>Eklenen rotalar

* Windows istemcilerine eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

* Windows dışı istemcilere eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

### <a name="access"></a>Erişim

* Windows istemcileri VNet1, VNet2 ve VNet4'e erişebilir, ancak topoloji değişikliklerinin etkili olabilmesi için VPN istemcisinin yeniden indirilmesi gerekir.

* Windows olmayan istemciler VNet1, VNet2 ve VNet4'e erişebilir

## <a name="multiple-vnets-connected-using-an-s2s-vpn"></a><a name="multis2s"></a>S2S VPN kullanılarak bağlanan birden fazla VNet

Bu örnekte, Noktaya Sayfa VPN ağ geçidi bağlantısı VNet1 içindir. VNet1, Siteden Siteye VPN bağlantısı kullanılarak VNet2'ye bağlanır. VNet2, Siteden Siteye VPN bağlantısı kullanılarak VNet3'e bağlanır. VNet1 ve VNet3 arasında doğrudan bir e-bakma veya Site'den Siteye VPN bağlantısı yoktur. Tüm Site-To-Site bağlantıları yönlendirme için BGP çalışmıyor.

Windows kullanan istemciler veya desteklenen başka bir işletim sistemi yalnızca VNet1'e erişebilir. Ek VNet'lere erişmek için BGP kullanılmalıdır.

![birden fazla VNets ve S2S](./media/vpn-gateway-about-point-to-site-routing/3.jpg "birden fazla VNets ve S2S")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Eklenen rotalar

* Windows istemcilerine eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows dışı istemcilere eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 192.168.0.0/24

### <a name="access"></a>Erişim

* Windows istemcileri yalnızca VNet1'e erişebilir

* Windows olmayan istemciler yalnızca VNet1'e erişebilir

## <a name="multiple-vnets-connected-using-an-s2s-vpn-bgp"></a><a name="multis2sbgp"></a>S2S VPN (BGP) kullanılarak bağlanan birden fazla VNet

Bu örnekte, Noktaya Sayfa VPN ağ geçidi bağlantısı VNet1 içindir. VNet1, Siteden Siteye VPN bağlantısı kullanılarak VNet2'ye bağlanır. VNet2, Siteden Siteye VPN bağlantısı kullanılarak VNet3'e bağlanır. VNet1 ve VNet3 arasında doğrudan bir e-bakma veya Site'den Siteye VPN bağlantısı yoktur. Tüm Site-To-Site bağlantıları yönlendirme için BGP çalıştırıyor.

Windows kullanan istemciler veya başka bir desteklenen işletim sistemi, Siteden Siteye VPN bağlantısı kullanarak bağlanan tüm VNet'lere erişebilir, ancak bağlı VNet'lere giden yolların Windows istemcilerine el ile eklenmesi gerekir.

![birden fazla VNets ve S2S (BGP)](./media/vpn-gateway-about-point-to-site-routing/4.jpg "birden fazla VNets ve S2S BGP")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Eklenen rotalar

* Windows istemcilerine eklenen yollar: 10.1.0.0/16

* Windows dışı istemcilere eklenen rotalar: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 192.168.0.0/24

### <a name="access"></a>Erişim

* Windows istemcileri VNet1, VNet2 ve VNet3'e erişebilir, ancak VNet2 ve VNet3'e giden yolların el ile eklenmesi gerekir.

* Windows olmayan istemciler VNet1, VNet2 ve VNet3'e erişebilir

## <a name="one-vnet-and-a-branch-office"></a><a name="vnetbranch"></a>Bir VNet ve bir şube

Bu örnekte, Noktaya Sayfa VPN ağ geçidi bağlantısı VNet1 içindir. VNet1 başka bir sanal ağa bağlı değildir/ bakılır, ancak BGP çalışmayan Site-To-Site VPN bağlantısı aracılığıyla şirket içi bir siteye bağlanır.

Windows ve Windows olmayan istemciler yalnızca VNet1'e erişebilir.

![bir VNet ve bir şube ile yönlendirme](./media/vpn-gateway-about-point-to-site-routing/5.jpg "bir VNet ve bir şube ile yönlendirme")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen rotalar

* Windows istemcilerine eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows dışı istemcilere eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

### <a name="access"></a>Erişim

* Windows istemcileri yalnızca VNet1'e erişebilir

* Windows olmayan istemciler yalnızca VNet1'e erişebilir

## <a name="one-vnet-and-a-branch-office-bgp"></a><a name="vnetbranchbgp"></a>Bir VNet ve bir şube (BGP)

Bu örnekte, Noktaya Sayfa VPN ağ geçidi bağlantısı VNet1 içindir. VNet1 başka bir sanal ağa bağlı değildir veya bakmıyor, ancak BGP çalıştıran Siteden Siteye VPN bağlantısı aracılığıyla şirket içi bir siteye (Site1) bağlı.

Windows istemcileri VNet'e ve şubeye (Site1) erişebilir, ancak Site1'e giden yolların istemciye el ile eklenmesi gerekir. Windows'suz istemciler VNet'e ve şirket içi şubeye erişebilir.

![bir VNet ve bir şube (BGP)](./media/vpn-gateway-about-point-to-site-routing/6.jpg "bir VNet ve bir şube")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen rotalar

* Windows istemcilerine eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows dışı istemcilere eklenen yollar: 10.1.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Erişim

* Windows istemcileri VNet1 ve Site1'e erişebilir, ancak Site1'e giden yolların el ile eklenmesi gerekir.

* Windows olmayan istemciler VNet1 ve Site1'e erişebilir.


## <a name="multiple-vnets-connected-using-s2s-and-a-branch-office"></a><a name="multivnets2sbranch"></a>S2S ve şube kullanılarak bağlanan birden fazla VNet

Bu örnekte, Noktaya Sayfa VPN ağ geçidi bağlantısı VNet1 içindir. VNet1, Siteden Siteye VPN bağlantısı kullanılarak VNet2'ye bağlanır. VNet2, Siteden Siteye VPN bağlantısı kullanılarak VNet3'e bağlanır. VNet1 ve VNet3 ağları arasında doğrudan bir izleme veya Siteden Siteye VPN tüneli yoktur. VNet3, Siteden Siteye VPN bağlantısı kullanarak bir şubeye (Site1) bağlanır. Tüm VPN bağlantıları BGP çalışmıyor.

Tüm istemciler yalnızca VNet1'e erişebilir.

![multi-VNet S2S ve şube](./media/vpn-gateway-about-point-to-site-routing/7.jpg "multi-VNet S2S ve şube")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen rotalar

* Windows istemcilerine eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows dışı istemcilere eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Erişim

* Windows istemcileri yalnızca VNet1'e erişebilir

* Windows olmayan istemciler yalnızca VNet1'e erişebilir

## <a name="multiple-vnets-connected-using-s2s-and-a-branch-office-bgp"></a><a name="multivnets2sbranchbgp"></a>S2S ve şube (BGP) kullanılarak bağlanan birden fazla VNet

Bu örnekte, Noktaya Sayfa VPN ağ geçidi bağlantısı VNet1 içindir. VNet1, Siteden Siteye VPN bağlantısı kullanılarak VNet2'ye bağlanır. VNet2, Siteden Siteye VPN bağlantısı kullanılarak VNet3'e bağlanır. VNet1 ve VNet3 ağları arasında doğrudan bir izleme veya Siteden Siteye VPN tüneli yoktur. VNet3, Siteden Siteye VPN bağlantısı kullanarak bir şubeye (Site1) bağlanır. Tüm VPN bağlantıları BGP çalışıyor.

Windows kullanan istemciler, Siteden Siteye VPN bağlantısı kullanarak VNet'lere ve bağlı sitelere erişebilir, ancak VNet2, VNet3 ve Site1'e giden yolların istemciye el ile eklenmesi gerekir. Windows'suz istemciler, herhangi bir el ile müdahale olmaksızın Siteden Siteye VPN bağlantısı kullanarak VNet'lere ve bağlı sitelere erişebilir. Erişim geçişli dir ve istemciler bağlı tüm VNet'lerde ve sitelerdeki (şirket içi) kaynaklara erişebilir.

![multi-VNet S2S ve şube](./media/vpn-gateway-about-point-to-site-routing/8.jpg "multi-VNet S2S ve şube")

### <a name="address-space"></a>Adres alanı

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Eklenen rotalar

* Windows istemcilerine eklenen yollar: 10.1.0.0/16, 192.168.0.0/24

* Windows dışı istemcilere eklenen yollar: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Erişim

* Windows istemcileri VNet1, VNet2, VNet3 ve Site1'e erişebilir, ancak VNet2, VNet3 ve Site1'e giden yolların istemciye el ile eklenmesi gerekir.

* Windows olmayan istemciler VNet1, Vnet2, VNet3 ve Site1'e erişebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. P2S VPN'inizi oluşturmaya başlamak için [Azure portalını kullanarak bir P2S VPN oluşturun.](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
