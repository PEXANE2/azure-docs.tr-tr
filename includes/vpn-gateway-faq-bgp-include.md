---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/17/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 32e4658af48a0ae3bde08de18cf1d8204878d671
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "91025045"
---
### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>BGP tüm Azure VPN Gateway SKU'larında destekleniyor mu?
BGP, temel SKU hariç tüm Azure VPN Gateway SKU 'Larında desteklenir.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Azure İlke Temelli VPN ağ geçitleriyle BGP kullanabilir miyim?
Hayır, BGP yalnızca Rota Temelli VPN Gateway’lerinde desteklenir.

### <a name="what-asn-autonomous-system-numbers-can-i-use"></a>Hangi ASN (özerk sistem numaraları) kullanabilirim?
Azure veya ıANA tarafından ayrılan aralıklar **hariç** , hem şirket içi ağlarınız hem de Azure sanal ağlarınız için kendi genel ağlarınızı veya özel ans 'leri kullanabilirsiniz:

> [!IMPORTANT]
>
> Aşağıdaki ASNs 'ler Azure veya ıANA tarafından ayrılmıştır:
> * Azure tarafından ayrılan ASNs:
>    * Ortak ASN’ler: 8074, 8075, 12076
>    * Özel ASN’ler: 65515, 65517, 65518, 65519, 65520
> * [IANA tarafından ayrılmış](http://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml) ASNs
>    * 23456, 64496-64511, 65535-65551 and 429496729
>
> Azure VPN ağ geçitlerine bağlanırken şirket içi VPN cihazlarınız için bu ASNs belirtemezsiniz.
>

### <a name="can-i-use-32-bit-4-byte-asns-autonomous-system-numbers"></a>32 bit (4 baytlık) ASNs (otonom sistem numarası) kullanabilir miyim?
Evet, Azure VPN ağ geçitleri artık 32 bit (4 baytlık) ASNs 'yi destekliyor. ASN 'yi ondalık biçimde kullanarak yapılandırmak için PowerShell/CLı/SDK kullanın.

### <a name="what-private-asns-can-i-use"></a>Hangi özel ASNs 'yi kullanabilirim?
Kullanılabilecek kullanılabilir özel ASNs aralığı şunlardır:

* 64512-65514, 65521-65534

Bu ASNs 'ler ıANA veya Azure tarafından kullanılmak üzere ayrılmamıştır ve bu nedenle Azure VPN Gateway atamak için kullanılabilir.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Azure VPN Gateway BGP Eşdeğer IP’si için hangi adresi kullanıyor?

* Varsayılan olarak, Azure VPN ağ geçidi, etkin-etkin VPN ağ geçitleri için GatewaySubnet aralığından tek bir IP adresi ayırır ya da etkin-etkin VPN ağ geçitleri için iki IP adresi ayırır. VPN ağ geçidini oluşturduğunuzda bu adresler otomatik olarak ayrılır. PowerShell (Get-AzVirtualNetworkGateway) kullanarak ayrılan gerçek BGP IP adreslerini alabilir, "bgpPeeringAddress" özelliğini arayabilir) veya Azure portal (ağ geçidi yapılandırma sayfasında "BGP ASN 'yi Yapılandır" özelliğinin altında).

* Şirket içi VPN yönlendiricileriniz, BGP IP adresleri olarak **APIPA** IP adreslerini (169.254. x. x) kullanıyorsa, Azure VPN ağ geçidiniz üzerinde ek BIR **Azure APIPA BGP IP adresi** belirtmeniz gerekir. Azure VPN ağ geçidi, yerel ağ geçidinde belirtilen şirket içi APIPA BGP eşi veya APIPA içi olmayan BGP eşi için özel IP adresi ile kullanılacak APIPA adresini seçer. Daha fazla bilgi için bkz. [BGP 'Yi yapılandırma](../articles/vpn-gateway/bgp-howto.md).

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>VPN cihazımdaki BGP Eşdeğer IP adreslerinin gereksinimleri nelerdir?
Şirket içi BGP eş adresiniz, VPN cihazınızın genel IP adresiyle veya VPN Gateway sanal ağ adres alanından **aynı olmamalıdır.** BGP Eşdeğer IP’si için VPN cihazında farklı bir IP adresi kullanın. Bu, cihazdaki geri döngü arabirimine, düzenli bir IP adresine veya bir APIPA adresine atanmış bir adres olabilir. Cihazınız BGP için APIPA adresini kullanıyorsa [BGP 'Yi yapılandırma](../articles/vpn-gateway/bgp-howto.md)bölümünde açıklandığı gıbı Azure VPN Ağ geçidinizde BIR APIPA BGP IP adresi belirtmeniz gerekir. Bu adresi, konumu temsil eden ilgili Yerel Ağ Geçidi’nde belirtin.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>BGP kullandığımda Yerel Ağ Geçidi için adres önekim olarak ne belirtmeliyim?

> [!IMPORTANT]
>
>Bu, daha önce belgelenen gereksinimin bir değişikidir. Bir bağlantı için BGP kullanıyorsanız, karşılık gelen yerel ağ geçidi kaynağı için **Adres alanı** alanını ***boş*** bırakmanız gerekir. Azure VPN Gateway, IPSec tüneli üzerinden şirket içi BGP eşi IP 'ye dahili olarak bir ana bilgisayar yolu ekler. Adres alanı alanına/32 **yolunu eklemeyin.** Bu, gereksizdir ve şirket içi VPN cihaz BGP IP 'si olarak bir APIPA adresi kullanırsanız, bu alana eklenemez. Adres alanı alanına başka ön ekler eklerseniz, BGP aracılığıyla öğrenilen yollara ek olarak Azure VPN Gateway 'e **statik yollar** olarak eklenir.
>

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Aynı ASN’yi hem şirket içi VPN ağlarında, hem de Azure VNet'lerde kullanabilir miyim?
Hayır, BGP’yle bunlara birlikte bağlanıyorsanız, şirket içi ağlar ve Azure VNet'ler arasında farklı ASN’ler atamanız gerekir. Şirket içi ve dışı karışık bağlantınız için BGP'nin etkin olup olmamasından bağımsız olarak Azure VPN Ağ Geçitlerine varsayılan 65515 ASN'si atanmıştır. VPN Gateway oluştururken farklı ASN atayarak bu varsayılan değeri geçersiz kılabilir veya ağ geçidini oluşturduktan sonra ASN’yi değiştirebilirsiniz. Şirket içi ASN’lerinizi ilgili Azure Yerel Ağ Geçitlerine atamanız gerekir.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Azure VPN Gateway’ler bana hangi adres öneklerini tanıtacak?
Azure VPN Gateway şirket içi BGP cihazlarınıza şu rotaları tanıtacaktır:

* VNet adres önekleriniz
* Azure VPN Gateway’e bağlı her Yerel Ağ Geçidi için adres önekleri
* Azure VPN Gateway’e bağlı diğer BGP eşdeğer oturumlarından öğrenilen rotalar; **varsayılan rota ve herhangi bir VNet önekiyle çakışan rotalar dışında**.

### <a name="how-many-prefixes-can-i-advertise-to-azure-vpn-gateway"></a>Azure VPN Gateway 'e kaç önek tanıtırım?
En fazla 4000 ön eki destekliyoruz. Ön ek sayısı bu sınırı aşarsa BGP oturumu düşürülür.

### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Varsayılan yolu (0.0.0.0/0) Azure VPN ağ geçitlerine tanıtabilir miyim?
Evet.

Bu, tüm VNet çıkış trafiğini şirket içi sitenize doğru zorlayacaktır ve sanal ağ VM 'lerinin internet 'ten VM 'lere olan RDP veya SSH gibi doğrudan Internet 'ten gelen genel iletişimi kabul etmesini engeller.

### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Tam ön eklerimi Sanal Ağ ön eklerim olarak tanıtabilir miyim?

Hayır, aynı ön eklerin Sanal Ağ adresi ön eklerinden biri olarak tanıtılması Azure platformu tarafından engellenir veya filtrelenir. Ancak, Sanal Ağınızda bulunanların üst kümesi olan bir ön ek tanıtabilirsiniz. 

Örneğin, sanal ağınızda 10.0.0.0/16 adres alanı kullanılıyorsa 10.0.0.0/8 olarak tanıtabilirsiniz. Ancak 10.0.0.0/16 veya 10.0.0.0/24 tanıtamazsınız.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>VNet - VNet bağlantılarımla BGP kullanabilir miyim?
Evet, BGP’yi hem şirket içi bağlantılar için, hem de VNet - VNet bağlantıları için kullanabilirsiniz.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Azure VPN Gateway’lerim için BGP’yi BGP olmayan bağlantılarla karıştırabilir miyim?
Evet, aynı Azure VPN Gateway için BGP ve BGP olmayan bağlantıları karıştırabilirsiniz.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Azure VPN Gateway BGP transit rotasını destekliyor mu?
Evet, BGP transit rotası desteklense de, özel durum olarak Azure VPN Gateway’lerinin varsayılan rotaları diğer BGP eşdeğerlerine **TANITMAMALARI** geçerlidir. Transit rotayı birden fazla Azure VPN Gateway’de etkinleştirmek için tüm ara VNet - VNet bağlantılarında BGP’yi etkinleştirmelisiniz. Daha fazla bilgi için bkz. [BGP hakkında](../articles/vpn-gateway/vpn-gateway-bgp-overview.md).

### <a name="can-i-have-more-than-one-tunnel-between-azure-vpn-gateway-and-my-on-premises-network"></a>Azure VPN Gateway ve şirket içi ağım arasında birden fazla tünelim olabilir mi?
Evet, bir Azure VPN Gateway ve şirket içi ağınız arasında birden fazla S2S tüneli kurabilirsiniz. Bu tünellerin tümü, Azure VPN ağ geçitleriniz için Toplam Tünel sayısına karşılık olarak sayılacağını ve her iki tünelde BGP 'yi etkinleştirmeniz gerektiğini unutmayın.

Örneğin, Azure VPN ağ geçidiniz ile şirket içi ağlarınızdan biri arasında iki yedekli Tüneliniz varsa, Azure VPN ağ geçidinizin toplam kotasının 2 tünellerini tüketirler.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>BGP bulunan iki Azure VNet arasında birden çok tünelim olabilir mi?
Evet, ancak sanal ağ geçitlerinden en az birinin etkin-etkin yapılandırmada olması gerekir.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-coexistence-configuration"></a>Bir ExpressRoute/S2S VPN birlikte bulunma yapılandırmasında S2S VPN için BGP kullanabilir miyim?
Evet. 

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>BGP eşdeğer oturumu için şirket içi VPN cihazıma ne eklemeliyim?
VPN cihazınızdaki Azure BGP Eşdeğer IP adresinin, IPsec S2S VPN tüneline yönlenmiş konak rotasını eklemelisiniz. Örneğin, Azure VPN Eşdeğer IP’si "10.12.255.30" olursa, "10.12.255.30" için VPN cihazınızdaki eşleşen IPsec tüneli arabiriminin sonraki durak arabirimine sahip bir konak rotası eklemelisiniz.
