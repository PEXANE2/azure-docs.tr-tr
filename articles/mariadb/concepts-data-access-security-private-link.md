---
title: MariaDB için Azure veritabanı için özel bağlantı (Önizleme)
description: Özel bağlantının, MariaDB için Azure veritabanı için nasıl çalıştığını öğrenin.
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 92d7522c8382ded182c5f482df3f3d917b4b3a14
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75982376"
---
# <a name="private-link-for-azure-database-for-mariadb-preview"></a>MariaDB için Azure veritabanı için özel bağlantı (Önizleme)

Özel bağlantı, Azure 'daki çeşitli PaaS hizmetlerine özel bir uç nokta aracılığıyla bağlanmanızı sağlar. Azure özel bağlantısı temel olarak Azure hizmetlerini özel sanal ağınız (VNet) içinde sunar. PaaS kaynaklarına, sanal ağ üzerindeki diğer kaynaklar gibi özel IP adresi kullanılarak erişilebilir.

Özel bağlantı işlevselliğini destekleyen PaaS hizmetlerinin listesi için özel bağlantı [belgelerini](https://docs.microsoft.com/azure/private-link/index)gözden geçirin. Özel uç nokta, belirli bir [sanal](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) ağ ve alt ağ içindeki özel bir IP adresidir.

> [!NOTE]
> Bu özellik, MariaDB için Azure veritabanı 'nın Genel Amaçlı ve bellek için Iyileştirilmiş fiyatlandırma katmanlarını desteklediği tüm Azure bölgelerinde kullanılabilir.

## <a name="data-exfiltration-prevention"></a>Veri kaybı önleme

MariaDB için Azure veritabanı 'nda veri değişim-bir veritabanı yöneticisi gibi yetkili bir Kullanıcı, bir sistemden veri ayıklayıp onu kuruluşun dışında başka bir konuma veya sisteme taşıyabildik. Örneğin, Kullanıcı, verileri üçüncü tarafa ait bir depolama hesabına taşıtır.

MariaDB için Azure veritabanı örneğine bağlanan bir Azure VM içinde MariaDB çalışma ekranı çalıştıran bir senaryoya göz önünde bulundurun. Bu MariaDB örneği Batı ABD veri merkezinde bulunur. Aşağıdaki örnekte, ağ erişim denetimleri kullanılarak MariaDB için Azure veritabanı 'nda genel uç noktalarla erişimin nasıl sınırlandırılcağı gösterilmektedir.

* Azure hizmetlerinin kapalı çalışmasına Izin ver ' i ayarlayarak, genel uç nokta aracılığıyla Azure veritabanı için Azure veritabanı trafiğini devre dışı bırakın. Bir IP adresinin veya aralıklarının sunucuya [güvenlik duvarı kuralları](https://docs.microsoft.com/azure/mariadb/concepts-firewall-rules) veya [sanal ağ hizmeti uç noktaları](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-vnet)aracılığıyla erişmesine izin verilmediğinden emin olun.

* Yalnızca, VM 'nin özel IP adresini kullanarak MariaDB için Azure veritabanı 'na giden trafiğe izin verin. Daha fazla bilgi için [hizmet uç noktası](concepts-data-access-security-vnet.md) ve [VNET güvenlik duvarı kuralları](howto-manage-vnet-portal.md)makalesine bakın.

* Azure VM 'de, ağ güvenlik grupları (NSG 'ler) ve hizmet etiketleri kullanarak giden bağlantı kapsamını aşağıdaki şekilde daraltın:

    * Hizmet etiketi = SQL için trafiğe izin veren bir NSG kuralı belirtin. WestUs-Batı ABD yalnızca MariaDB için Azure veritabanı 'na bağlantıya izin veriliyor
    * Hizmet etiketi = SQL-tüm bölgelerde MariaDB veritabanına bağlantıları reddetmek için bir NSG kuralı (daha yüksek önceliğe sahip) belirtin</br></br>

Bu kurulumun sonunda, Azure VM yalnızca Batı ABD bölgesindeki MariaDB için Azure veritabanı 'na bağlanabilir. Ancak, bağlantı, MariaDB için tek bir Azure veritabanı ile sınırlı değildir. VM, aboneliğin parçası olmayan veritabanları da dahil olmak üzere Batı ABD bölgesindeki MariaDB için Azure veritabanı 'na yine de bağlanabilir. Yukarıdaki senaryodaki veri taşalım kapsamını belirli bir bölgeye azalttık, ancak bunu tamamen ortadan kaldırdık.</br>

Özel bağlantı ile artık özel uç noktaya erişimi kısıtlamak için NSG 'ler gibi ağ erişim denetimlerini ayarlayabilirsiniz. Tek tek Azure PaaS kaynakları, belirli özel uç noktalara eşlenir. Kötü amaçlı bir Insider yalnızca eşlenmiş PaaS kaynağına erişebilir (örneğin, MariaDB için Azure veritabanı) ve başka bir kaynak olamaz.

## <a name="on-premises-connectivity-over-private-peering"></a>Özel eşleme üzerinden şirket içi bağlantı

Şirket içi makinelerden ortak uç noktaya bağlandığınızda, IP adresinizin sunucu düzeyinde bir güvenlik duvarı kuralı kullanılarak IP tabanlı güvenlik duvarına eklenmesi gerekir. Bu model geliştirme veya test iş yükleri için bireysel makinelere erişim sağlamak için iyi bir şekilde çalıştığından, bir üretim ortamında yönetilmesi zordur.

Özel bağlantı ile, [Express Route](https://azure.microsoft.com/services/expressroute/) (er), özel eşleme veya [VPN tüneli](https://docs.microsoft.com/azure/vpn-gateway/)kullanarak özel uç noktaya şirket içi erişimi etkinleştirebilirsiniz. Daha sonra, genel uç nokta aracılığıyla tüm erişimi devre dışı bırakabilir ve IP tabanlı güvenlik duvarını kullanmaz.

## <a name="configure-private-link-for-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı için özel bağlantı yapılandırma

### <a name="creation-process"></a>Oluşturma Işlemi

Özel bağlantıları etkinleştirmek için özel uç noktalar gereklidir. Bu işlem, aşağıdaki nasıl yapılır kılavuzlarından yararlanarak yapılabilir.

* [Azure Portal](https://docs.microsoft.com/azure/mariadb/howto-configure-privatelink-portal)
* [CLI](https://docs.microsoft.com/azure/mariadb/howto-configure-privatelink-cli)

### <a name="approval-process"></a>Onay Işlemi

Ağ Yöneticisi özel uç nokta (PE) oluşturduğunda, yönetici özel uç nokta bağlantısını (PEC) MariaDB için Azure veritabanı 'na yönetebilir.

> [!NOTE]
> Şu anda, MariaDB için Azure veritabanı yalnızca özel uç nokta için otomatik onayı destekler.

* Azure portal, MariaDB sunucu kaynağı için Azure veritabanı 'na gidin. 
    * Sol bölmedeki özel uç nokta bağlantılarını seçin
    * Tüm özel uç nokta bağlantılarının (lar) bir listesini gösterir
    * Karşılık gelen özel uç nokta (PE) oluşturuldu

![Özel uç nokta portalını seçin](media/concepts-data-access-and-security-private-link/select-private-link-portal.png)

* Listeden tek bir PEC seçin.

![onay bekleyen özel uç noktayı seçin](media/concepts-data-access-and-security-private-link/select-private-link.png)

* MariaDB Sunucu Yöneticisi bir PEC 'i onaylamayı veya reddetmeyi seçebilir ve isteğe bağlı olarak kısa bir metin yanıtı ekleyebilir.

![Özel uç nokta iletisini seçin](media/concepts-data-access-and-security-private-link/select-private-link-message.png)

* Onay veya reddetme sonrasında, liste, yanıt metniyle birlikte uygun durumu yansıtır

![Özel uç nokta son durumunu seçin](media/concepts-data-access-and-security-private-link/show-private-link-approved-connection.png)

## <a name="use-cases-of-private-link-for-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı için özel bağlantı durumları kullanın

İstemciler aynı VNet 'ten özel uç noktaya, aynı bölgedeki eşlenmiş VNet 'e veya bölgeler arasında VNet-VNet bağlantısı aracılığıyla bağlanabilir. Ayrıca, istemciler ExpressRoute, özel eşleme veya VPN tüneli kullanarak şirket içinden bağlanabilir. Aşağıda, yaygın kullanım durumlarını gösteren basitleştirilmiş bir diyagram bulunur.

![Özel uç noktaya genel bakış ' ı seçin](media/concepts-data-access-and-security-private-link/show-private-link-overview.png)

### <a name="connecting-from-an-azure-vm-in-peered-virtual-network-vnet"></a>Eşlenen sanal ağdaki (VNet) bir Azure VM 'den bağlanma
Eşlenmiş VNet 'teki bir Azure VM 'den MariaDB için Azure veritabanı 'na bağlantı kurmak üzere [VNET eşlemesini](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-powershell) yapılandırın.

### <a name="connecting-from-an-azure-vm-in-vnet-to-vnet-environment"></a>VNet-VNet ortamında bir Azure VM 'sinden bağlantı kurma
Farklı bir bölgede veya abonelikte bulunan bir Azure VM 'den MariaDB için Azure veritabanı bağlantısı kurmak üzere [VNET-VNET VPN Gateway bağlantısını](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal) yapılandırın.

### <a name="connecting-from-an-on-premises-environment-over-vpn"></a>VPN üzerinden şirket içi bir ortamdan bağlanma
Şirket içi bir ortamdan MariaDB için Azure veritabanı arasında bağlantı kurmak için seçeneklerden birini seçin ve uygulayın:

* [Noktadan siteye bağlantı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)
* [Siteden siteye VPN bağlantısı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell)
* [ExpressRoute devresi](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager)

## <a name="private-link-combined-with-firewall-rules"></a>Özel bağlantı, güvenlik duvarı kurallarıyla birleştirildi

Özel bağlantıyı güvenlik duvarı kurallarıyla birlikte kullandığınızda aşağıdaki durumlar ve sonuçlar mümkündür:

* Herhangi bir güvenlik duvarı kuralı yapılandırmazsanız, varsayılan olarak, MariaDB için Azure veritabanı 'na hiçbir trafik erişemeyecektir.

* Ortak trafiği veya bir hizmet uç noktasını yapılandırırsanız ve özel uç noktalar oluşturursanız, ilgili güvenlik duvarı kuralı tarafından farklı gelen trafik türlerine izin verilir.

* Herhangi bir ortak trafik veya hizmet uç noktası yapılandırmazsanız ve özel uç noktalar oluşturursanız, MariaDB için Azure veritabanı 'nda yalnızca özel uç noktalar aracılığıyla erişilebilir. Genel trafiği veya bir hizmet uç noktası yapılandırmazsanız, tüm onaylanan özel uç noktalar reddedildikten veya silindikten sonra, hiçbir trafik, MariaDB için Azure veritabanı 'na erişemeyecektir.

## <a name="next-steps"></a>Sonraki adımlar

MariaDB güvenlik özellikleri için Azure veritabanı hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* MariaDB için Azure veritabanı için bir güvenlik duvarı yapılandırmak üzere bkz. [güvenlik duvarı desteği](https://docs.microsoft.com/azure/mariadb/concepts-firewall-rules).

* MariaDB için Azure veritabanınız için bir sanal ağ hizmeti uç noktası yapılandırma hakkında bilgi edinmek için bkz. [sanal ağlardan erişimi yapılandırma](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-vnet).

* MariaDB bağlantısı için Azure veritabanı 'na genel bakış için bkz. [MariaDB Için Azure veritabanı bağlantı mimarisi](https://docs.microsoft.com/azure/MariaDB/concepts-connectivity-architecture)
