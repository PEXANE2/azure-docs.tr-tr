---
title: Ağ değerlendirmeleri-Azure ayrılmış HSM | Microsoft Docs
description: Azure ayrılmış HSM dağıtımları için geçerli olan ağ konularına genel bakış
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: mbaldwin
ms.openlocfilehash: 06cd02177d7d5c478f3378eb05517f1a37297e92
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84300741"
---
# <a name="azure-dedicated-hsm-networking"></a>Azure ayrılmış HSM ağı

Azure ayrılmış HSM, yüksek güvenlikli bir ağ ortamı gerektirir. Bu, Azure bulutunun şirket BT ortamına (Şirket içi), dağıtılmış uygulamalar kullanılarak veya yüksek kullanılabilirlik senaryolarında olup olmadığı doğru bir şekilde yapılır. Azure ağ bunu sağlar ve değinilmesi gereken dört farklı alan vardır.

- Azure 'da sanal ağınız (VNet) içinde HSM cihazları oluşturma
- HSM cihazlarının yapılandırması ve yönetimi için şirket içinde bulut tabanlı kaynaklara bağlanma
- Bağlama ve bağlantı temelli uygulama kaynakları ve HSM cihazları için sanal ağlar oluşturma ve bağlama
- Ağlarda iletişim kurmak için sanal ağları bölgeler arasında bağlama ve ayrıca yüksek kullanılabilirlik senaryolarını etkinleştirme

## <a name="virtual-network-for-your-dedicated-hsms"></a>Adanmış HSM 'niz için sanal ağ

Adanmış HSM 'ler bir sanal ağla tümleşiktir ve müşterilere Azure 'da özel ağa yerleştirilir. Bu, sanal makinelerden cihazlara veya sanal ağdaki işlem kaynaklarına erişim sağlar.  
Azure hizmetlerini sanal ağla ve sağladığı yeteneklere tümleştirme hakkında daha fazla bilgi için bkz. [Azure hizmetleri Için sanal ağ](../virtual-network/virtual-network-for-azure-services.md) belgeleri.

### <a name="virtual-networks"></a>Sanal ağlar

Özel bir HSM cihazını sağlamadan önce, müşterilerin ilk olarak bir Azure Ağa gelen oluşturması veya müşteriler aboneliğinde zaten mevcut olan bir tane kullanması gerekir. Sanal ağ, adanmış HSM cihazı için güvenlik çevre birimi tanımlar. Sanal ağlar oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ belgeleri](../virtual-network/virtual-networks-overview.md).

### <a name="subnets"></a>Alt ağlar

Alt ağlar sanal ağı, yerleştirdiğiniz Azure kaynakları tarafından kullanılabilen ayrı adres alanlarına bölüler. Adanmış HSM 'ler, sanal ağdaki bir alt ağa dağıtılır. Müşterinin alt ağına dağıtılan her bir adanmış HSM aygıtı, bu alt ağdan özel bir IP adresi alır. HSM cihazının dağıtıldığı alt ağın hizmete açıkça atanmış olması gerekir: Microsoft. HardwareSecurityModules/ayrılmış Atedhsms. Bu, alt ağa dağıtım için HSM hizmetine bazı izinler verir. Adanmış HSM 'lerin temsilciliğini, alt ağda belirli ilke kısıtlamalarını uygular. Ağ güvenlik grupları (NSG 'ler) ve Kullanıcı tanımlı yollar (UDRs) Şu anda temsilci alt ağlarda desteklenmez. Sonuç olarak, bir alt ağ adanmış HSM 'ler için temsilci seçildikten sonra yalnızca HSM kaynaklarını dağıtmak için kullanılabilir. Diğer müşteri kaynaklarının alt ağa dağıtılması başarısız olur.


### <a name="expressroute-gateway"></a>ExpressRoute ağ geçidi

Geçerli mimarinin gereksinimi, HSM cihazının Azure ile tümleştirilmesini sağlamak için bir HSM cihazının yerleştirilmesi gereken müşteriler alt ağındaki bir ER ağ geçidinin yapılandırılmasına neden olur. Bu ER Gateway, şirket içi konumları Azure 'daki müşterilerin HSM cihazlarına bağlamak için kullanılamaz.

## <a name="connecting-your-on-premises-it-to-azure"></a>Şirket içi BT hesabınızı Azure 'a bağlama

Bulut tabanlı kaynaklar oluştururken, şirket içi BT kaynaklarına geri bir özel bağlantı için tipik bir gereksinimdir. Bu özel HSM söz konusu olduğunda, HSM istemci yazılımının HSM cihazlarını yapılandırması ve ayrıca yedekleme ve günlükleri gibi etkinlikler ve analiz için HSMs 'tan günlük çekme işlemleri için ağırlıklı olacaktır. Burada önemli bir karar noktası, seçenekler olduğu gibi bağlantının doğasına alınır.  Azure bulutundaki kaynaklarla (HSM 'ler dahil) güvenli iletişim gerektiren birden çok şirket içi kaynak olacağı için, en esnek seçenek siteden siteye VPN 'dir. Bu, bir müşteri kuruluşunun bağlantıyı kolaylaştırmak için bir VPN cihazına sahip olmasını gerektirir. Bir noktadan siteye VPN bağlantısı, tek bir yönetim iş istasyonu gibi yalnızca şirket içi tek bir uç nokta varsa kullanılabilir.
Bağlantı seçenekleri hakkında daha fazla bilgi için bkz. [VPN Gateway planlama seçenekleri](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#planningtable).

> [!NOTE]
> Şu anda ExpressRoute, şirket içi kaynaklarla bağlantı için bir seçenek değildir. Ayrıca, yukarıda açıklanan ExpressRoute ağ geçidinin şirket içi altyapıyla bağlantı için değil, aynı zamanda kullanılması da not edilmelidir.

### <a name="point-to-site-vpn"></a>Noktadan Siteye VPN

Noktadan siteye sanal özel ağ, şirket içi tek bir uç noktaya yönelik güvenli bağlantının en basit biçimidir. Bu, yalnızca Azure tabanlı adanmış HSM 'ler için tek bir yönetim iş istasyonuna sahip olmak istiyorsanız ilgili olabilir.

### <a name="site-to-site-vpn"></a>Siteler arası VPN

Siteden siteye sanal özel ağ, Azure tabanlı adanmış HSM 'ler ve şirket içi BT 'niz arasında güvenli iletişim sağlar. Bunu yapmak için bir neden, HSM 'nin şirket içi olarak bir yedekleme tesisi olmasını ve yedeklemeyi çalıştırmak için iki ile arasında bağlantı olmasını gerektirir.

## <a name="connecting-virtual-networks"></a>Sanal ağları bağlama

Adanmış HSM için tipik bir dağıtım mimarisi, tek bir sanal ağ ve HSM cihazlarının oluşturulduğu ve sağlandığı ilgili alt ağ ile başlar. Aynı bölge içinde, özel HSM 'yi kullanacak uygulama bileşenleri için ek sanal ağlar ve alt ağlar da olabilir. Bu ağlar genelinde iletişimi etkinleştirmek için sanal ağ eşlemesi kullanırız.

### <a name="virtual-network-peering"></a>Sanal ağ eşleme

Her birinin kaynağına erişmesi gereken bir bölgede birden çok sanal ağ olduğunda, aralarında güvenli iletişim kanalları oluşturmak için sanal ağ eşlemesi kullanılabilir.  Sanal ağ eşlemesi yalnızca güvenli iletişim sağlamaz, ancak Azure 'daki kaynaklar arasında düşük gecikmeli ve yüksek bant genişliğine sahip bağlantılar sağlar.

![ağ eşlemesi](media/networking/peering.png)

## <a name="connecting-across-azure-regions"></a>Azure bölgeleri arasında bağlantı kurma

HSM cihazlarının, trafiği alternatif bir HSM 'ye yeniden yönlendirmek için yazılım kitaplıkları aracılığıyla yeteneği vardır. Cihazların başarısız olması veya bir cihaza erişimin kaybolması durumunda trafik yeniden yönlendirme yararlı olur. Bölgesel düzey hata senaryoları, diğer bölgelerde HSM 'ler dağıtarak ve bölgeler arasında sanal ağlar arasındaki iletişimi etkinleştirerek azaltılabilir.

### <a name="cross-region-ha-using-vpn-gateway"></a>Çapraz bölge HA VPN Gateway kullanma

Küresel olarak dağıtılan uygulamalarda veya yüksek kullanılabilirlik bölgesel yük devretme senaryolarında, sanal ağların bölgeler arasında bağlanması gerekir. Azure ayrılmış HSM ile, yüksek kullanılabilirlik, iki sanal ağ arasında güvenli bir tünel sağlayan bir VPN Gateway kullanılarak elde edilebilir. VPN Gateway kullanarak VNET-VNET bağlantıları hakkında daha fazla bilgi için [VPN Gateway nedir?](../vpn-gateway/design.md#V2V) başlıklı makaleye bakın.

> [!NOTE]
> Genel VNET eşlemesi Şu anda adanmış HSM 'ler ile bölgeler arası bağlantı senaryolarında kullanılamaz ve bunun yerine VPN Gateway kullanılmalıdır. 

![Küresel VNET](media/networking/global-vnet.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Sık sorulan sorular](faq.md)
- [Desteklenebilirlik](supportability.md)
- [Yüksek kullanılabilirlik](high-availability.md)
- [Fiziksel güvenlik](physical-security.md)
- [İzleme](monitoring.md)
- [Dağıtım mimarisi](deployment-architecture.md)