---
title: Private Link - PostgreSQL için Azure Veritabanı - Tek sunucu
description: PostgreSQL - Single sunucusu için Azure Veritabanı için Özel bağlantının nasıl çalıştığını öğrenin.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 4216abdf8cc8aae00e3ba0c57961c4b8b7403672
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371690"
---
# <a name="private-link-for-azure-database-for-postgresql-single-server"></a>PostgreSQL-Single sunucusu için Azure Veritabanı için Özel Bağlantı

Private Link, PostgreSQL - Single sunucusu için Azure Veritabanı için özel uç noktalar oluşturmanıza olanak tanır ve böylece Azure hizmetlerini özel Sanal Ağınıza (VNet) getirir. Özel bitiş noktası, VNet'teki diğer kaynaklar gibi veritabanı sunucunuza bağlanmak için kullanabileceğiniz özel bir IP'yi ortaya çıkarır.

Private Link işlevselliğini destekleyen PaaS hizmetlerine yönelik bir liste için Özel Bağlantı [belgelerini](https://docs.microsoft.com/azure/private-link/index)inceleyin. Özel bitiş noktası, belirli bir [VNet](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) ve Subnet içindeki özel bir IP adresidir.

> [!NOTE]
> Bu özellik, PostgreSQL Tek sunucu için Azure Veritabanı'nın Genel Amaç ve Bellek Optimize edilmiş fiyatlandırma katmanlarını desteklediği tüm Azure bölgelerinde kullanılabilir.

## <a name="data-exfiltration-prevention"></a>Veri sızma önleme

PostgreSQL Tek sunucu için Azure Veritabanı'ndaki veri filtreleme, veritabanı yöneticisi gibi yetkili bir kullanıcının verileri bir sistemden ayıklayıp kuruluş dışındaki başka bir konuma veya sisteme taşıyabilmesidir. Örneğin, kullanıcı verileri üçüncü bir tarafa ait bir depolama hesabına taşır.

Bir kullanıcının, Batı ABD'de sağlanan PostgreSQL Tek sunucu için bir Azure Veritabanına bağlanan bir Azure Sanal Makinesi (VM) içinde PGAdmin çalıştırdığı bir senaryodüşünün. Aşağıdaki örnekte, ağ erişim denetimlerini kullanarak PostgreSQL Single sunucusu için Azure Veritabanı'ndaki ortak uç noktalarla erişimin nasıl sınırlandırılabildiğini gösterilmektedir.

* *Azure Hizmetlerini* KAPAT'a izin vererek, genel bitiş noktası üzerinden PostgreSQL Single sunucusu için Azure Veritabanı'na yönelik tüm Azure hizmet trafiğini devre dışı bırak. [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/postgresql/concepts-firewall-rules) veya sanal ağ hizmeti bitiş [noktaları](https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-vnet)aracılığıyla sunucuya IP adreslerinin veya aralıkların erişmesine izin verilmedi.

* VM'nin Özel IP adresini kullanarak Yalnızca PostgreSQL Tek sunucu için Azure Veritabanı'na trafik izni verin. Daha fazla bilgi için [Service Endpoint](concepts-data-access-and-security-vnet.md) ve [VNet güvenlik duvarı kurallarıyla](howto-manage-vnet-using-portal.md) ilgili makalelere bakın.

* Azure VM'de, Ağ Güvenlik Grupları (NSG'ler) ve Hizmet Etiketleri'ni kullanarak giden bağlantının kapsamını aşağıdaki gibi daraltın

    * Service Tag = SQL trafiğine izin vermek için bir NSG kuralı *belirtin. WestUS* - yalnızca Batı ABD'de PostgreSQL Tek sunucu için Azure Veritabanı'na bağlantı sağlar
    * *Service Tag = SQL* trafiğini reddetmek için bir NSG kuralı (daha yüksek önceliğe sahip) belirtin - tüm bölgelerde PostgreSQL Veritabanına bağlantıları reddetme</br></br>

Bu kurulumun sonunda, Azure VM yalnızca Batı ABD bölgesindeki PostgreSQL Tek sunucu için Azure Veritabanı'na bağlanabilir. Ancak, bağlantı PostgreSQL Tek sunucu için tek bir Azure Veritabanı ile sınırlı değildir. VM, aboneliğin parçası olmayan veritabanları da dahil olmak üzere Batı ABD bölgesindeki PostgreSQL Single sunucusu için herhangi bir Azure Veritabanına bağlanmaya devam edebilir. Yukarıdaki senaryodaki veri sızma kapsamını belirli bir bölgeye indirgemiş olsak da, bunu tamamen ortadan kaldırmadık.</br>

Private Link ile artık özel bitiş noktasına erişimi kısıtlamak için NSG'ler gibi ağ erişim denetimleri ayarlayabilirsiniz. Tek tek Azure PaaS kaynakları daha sonra belirli özel uç noktalara eşlenir. Kötü niyetli bir kişi yalnızca eşlenen PaaS kaynağına (örneğin PostgreSQL Tek sunucu için Azure Veritabanı) erişebilir ve başka bir kaynağa erişemez.

## <a name="on-premises-connectivity-over-private-peering"></a>Özel bakış üzerinde şirket içi bağlantı

Şirket içi makinelerden ortak bitiş noktasına bağlandığınızda, IP adresinizin Sunucu düzeyinde güvenlik duvarı kuralı kullanılarak IP tabanlı güvenlik duvarına eklenmesi gerekir. Bu model, dev veya test iş yükleri için tek tek makinelere erişim sağlamak için iyi çalışıyor olsa da, bir üretim ortamında yönetmek zordur.

Private Link [ile, Express Route](https://azure.microsoft.com/services/expressroute/) (ER), özel bakanlık veya [VPN tünelini](https://docs.microsoft.com/azure/vpn-gateway/)kullanarak özel bitiş noktasına binalar arası erişimi etkinleştirebilirsiniz. Daha sonra tüm erişimi ortak bitiş noktası üzerinden devre dışı bırakıp IP tabanlı güvenlik duvarını kullanamazlar.

## <a name="configure-private-link-for-azure-database-for-postgresql-single-server"></a>PostgreSQL Tek sunucu için Azure Veritabanı için Özel Bağlantıyı Yapılandırma

### <a name="creation-process"></a>Oluşturma Süreci

Özel Bağlantı'yı etkinleştirmek için özel uç noktalar gereklidir. Bu, aşağıdaki nasıl yapılır kılavuzları kullanılarak yapılabilir.

* [Azure portalında](https://docs.microsoft.com/azure/postgresql/howto-configure-privatelink-portal)
* [CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-privatelink-cli)

### <a name="approval-process"></a>Onay Süreci
Ağ yöneticisi özel bitiş noktası (PE) oluşturduktan sonra, PostgreSQL yöneticisi, PostgreSQL için Azure Veritabanı'na özel uç nokta bağlantısını (PEC) yönetebilir. Ağ yöneticisi ile DBA arasındaki bu görev ayrımı, PostgreSQL bağlantısı için Azure Veritabanı'nın yönetimi için yararlıdır. 

* Azure portalındaki PostgreSQL sunucu kaynağı için Azure Veritabanı'na gidin. 
    * Sol bölmedeki özel bitiş noktası bağlantılarını seçin
    * Tüm özel uç nokta bağlantılarının (PECs) listesini gösterir
    * İlgili özel bitiş noktası (PE) oluşturuldu

![özel bitiş noktası portalını seçin](media/concepts-data-access-and-security-private-link/select-private-link-portal.png)

* Listeden tek bir PEC'i seçerek seçin.

![onay bekleyen özel bitiş noktasını seçin](media/concepts-data-access-and-security-private-link/select-private-link.png)

* PostgreSQL sunucu yöneticisi bir PEC'i onaylamayı veya reddetmeyi seçebilir ve isteğe bağlı olarak kısa bir metin yanıtı ekleyebilir.

![özel bitiş noktası iletisini seçin](media/concepts-data-access-and-security-private-link/select-private-link-message.png)

* Onay veya ret ten sonra, liste yanıt metni ile birlikte uygun durumu yansıtacak

![özel bitiş noktası son durumunu seçin](media/concepts-data-access-and-security-private-link/show-private-link-approved-connection.png)

## <a name="use-cases-of-private-link-for-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı için Özel Bağlantı servis taleplerini kullanma

İstemciler özel bitiş noktasına aynı VNet'ten, aynı bölgede bakan VNet'ten veya bölgeler arasında VNet-vNet bağlantısı üzerinden bağlanabilir. Ayrıca, istemciler ExpressRoute, özel bakış veya VPN tünelleme kullanarak şirket içinde bağlanabilir. Aşağıda, ortak kullanım durumlarını gösteren basitleştirilmiş bir diyagram verilmiştir.

![özel bitiş noktası genel görünümünü seçin](media/concepts-data-access-and-security-private-link/show-private-link-overview.png)

### <a name="connecting-from-an-azure-vm-in-peered-virtual-network-vnet"></a>Eşli Sanal Ağda (VNet) Azure VM'den bağlanma
GDeZİ Veritabanı'na bağlantı kurmak için [VNet eşlemesini](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-powershell) yapılandırın - Bir Azure VM'den tek sunucu, bir eşlenen VNet'te.

### <a name="connecting-from-an-azure-vm-in-vnet-to-vnet-environment"></a>VNet-to-VNet ortamında bir Azure VM'den bağlanma
PostgreSQL için bir Azure Veritabanına bağlantı kurmak için [VNet-to-VNet VPN ağ geçidi bağlantısını](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal) yapılandırın - Farklı bir bölgedeki veya abonelikteki bir Azure VM'den tek sunucu.

### <a name="connecting-from-an-on-premises-environment-over-vpn"></a>VPN üzerinden şirket içi ortamdan bağlanma
Şirket içi ortamdan PostgreSQL için Azure Veritabanı'na bağlantı kurmak için - Tek sunucu, seçeneklerden birini seçin ve uygulayın:

* [Noktadan Siteye bağlantı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)
* [Konumlar arası VPN bağlantısı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell)
* [ExpressRoute devresi](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager)

## <a name="private-link-combined-with-firewall-rules"></a>Özel Bağlantı, güvenlik duvarı kurallarıyla birleştirildi

Güvenlik duvarı kurallarıyla birlikte Private Link'i kullandığınızda aşağıdaki durumlar ve sonuçlar mümkündür:

* Herhangi bir güvenlik duvarı kuralı yapılandırmazsanız, varsayılan olarak, Hiçbir trafik PostgreSQL Tek sunucu için Azure Veritabanı'na erişemez.

* Genel trafiği veya hizmet bitiş noktasını yapılandırırsanız ve özel bitiş noktaları oluşturursanız, farklı gelen trafik türleri ilgili güvenlik duvarı kuralı türütarafından yetkilendirilir.

* Herhangi bir genel trafik veya hizmet bitiş noktasını yapılandırmaz ve özel uç noktalar oluşturursanız, PostgreSQL Tek sunucu için Azure Veritabanı'na yalnızca özel uç noktalardan erişilebilir. Genel trafiği veya hizmet bitiş noktasını yapılandırmazsanız, onaylanan tüm özel uç noktalar reddedildikten veya silindikten sonra, PostgreSQL Tek sunucu için Azure Veritabanı'na hiçbir trafik erişemez.

## <a name="deny-public-access-for-azure-database-for-postgresql-single-server"></a>PostgreSQL Tek sunucu için Azure Veritabanı için genel erişimi reddetme

PostgreSQL Tek sunucu için Azure Veritabanına erişmek için yalnızca özel uç noktalara güvenmek istiyorsanız, Veritabanı sunucusunda **Genel Ağ Erişimini Reddet** yapılandırmasını ayarlayarak tüm ortak uç noktaları[(güvenlik duvarı kuralları](concepts-firewall-rules.md) ve [VNet hizmet bitiş noktaları)](concepts-data-access-and-security-vnet.md)ayarlayabilirsiniz. 

Bu ayar *EVET* olarak ayarlandığında, yalnızca özel uç noktalar üzerinden bağlanan bağlantılarA PostgreSQL için Azure Veritabanınıza izin verilir. Bu ayar *NO* olarak ayarlandığında istemciler, güvenlik duvarınıza veya VNet hizmet bitiş noktası ayarLarınıza bağlı olarak PostgreSQL için Azure Veritabanınıza bağlanabilir. Ayrıca, Özel ağ erişiminin değeri müşterilere ayarlandıktan sonra varolan 'Güvenlik Duvarı kuralları' ve 'VNet hizmet bitiş noktası kuralı' eklenemez ve/veya güncelleyemez

> [!Note]
> Bu özellik, PostgreSQL için Azure Veritabanı - Tek sunucunun Genel Amaç ve Bellek Optimize edilmiş fiyatlandırma katmanlarını desteklediği tüm Azure bölgelerinde kullanılabilir.
>
> Bu ayarın, PostgreSQL Tek sunucu için Azure Veritabanınız için SSL ve TLS yapılandırmaları üzerinde herhangi bir etkisi yoktur.

Azure portalından PostgreSQL Tek sunucu için Azure Veritabanınız için **Genel Ağ Erişimini Reddet'i** nasıl ayarlayabilirsiniz öğrenmek için, Genel Ağ Erişimini [Reddet'i nasıl yapılandırılatırabilirsiniz.](howto-deny-public-network-access.md)

## <a name="next-steps"></a>Sonraki adımlar

PostgreSQL Tek sunucu güvenlik özellikleri için Azure Veritabanı hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* PostgreSQL Tek sunucu için Azure Veritabanı için bir güvenlik duvarı yapılandırmak için [Güvenlik Duvarı desteğine](https://docs.microsoft.com/azure/postgresql/concepts-firewall-rules)bakın.

* PostgreSQL Tek sunucu için Azure Veritabanınız için sanal ağ hizmeti bitiş noktasını nasıl yapılandırılaceksiniz öğrenmek için [bkz.](https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-vnet)

* PostgreSQL Tek sunucu bağlantısı için Azure Veritabanı'na genel bakış için [PostgreSQL Bağlantı Mimarisi için Azure Veritabanı'na](https://docs.microsoft.com/azure/postgresql/concepts-connectivity-architecture) bakın