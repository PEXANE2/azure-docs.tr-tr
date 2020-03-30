---
title: Ağ la ilgili hususlar - Azure Özel HSM | Microsoft Dokümanlar
description: Azure Özel HSM dağıtımları için geçerli ağ dikkatlerine genel bakış
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
ms.openlocfilehash: 044930c9df7b54515b9b66426a6b05aa9517a3a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70881276"
---
# <a name="azure-dedicated-hsm-networking"></a>Azure Özel HSM ağ

Azure Özel HSM, son derece güvenli bir ağ ortamı gerektirir. Bu, Azure bulutundan müşterinin BT ortamına (şirket içi), dağıtılmış uygulamaları kullanarak veya yüksek kullanılabilirlik senaryoları için geçerlidir. Azure Ağı bunu sağlar ve ele alınması gereken dört ayrı alan vardır.

- Azure'da Sanal Ağınızda (VNet) HSM aygıtları oluşturma
- HSM aygıtlarının yapılandırması ve yönetimi için şirket içi bulut tabanlı kaynaklara bağlanma
- Uygulamalar ve HSM aygıtları arasında bağlantı kurmak için sanal ağlar oluşturma ve bağlama
- İletişim kurmak ve aynı zamanda yüksek kullanılabilirlik senaryolarını etkinleştirmek için bölgeler arasında sanal ağları bağlama

## <a name="virtual-network-for-your-dedicated-hsms"></a>Özel HSM'leriniz için sanal ağ

Özel HSM'ler sanal ağa entegre edilir ve azure'da müşterilerin kendi özel ağına yerleştirilir. Bu, sanal ağdaki sanal makinelerden veya bilgi işlem kaynaklarından aygıtlara erişim sağlar.  
Azure hizmetlerini sanal ağa ve sağladığı yeteneklere entegre etme hakkında daha fazla bilgi için Azure hizmetleri için [Sanal ağa](../virtual-network/virtual-network-for-azure-services.md) bakın.

### <a name="virtual-networks"></a>Sanal ağlar

Özel bir HSM aygıtı sağlamadan önce, müşterilerin önce Azure'da bir Sanal Ağ oluşturmaları veya müşteri aboneliğinde zaten var olan bir ağı kullanmaları gerekir. Sanal ağ, Özel HSM aygıtının güvenlik çevresini tanımlar. Sanal ağlar oluşturma hakkında daha fazla bilgi için [sanal ağ belgelerine](../virtual-network/virtual-networks-overview.md)bakın.

### <a name="subnets"></a>Alt ağlar

Alt ağlar, sanal ağı, içine yerleştirdiğiniz Azure kaynakları tarafından kullanılabilir ayrı adres alanlarına böler. Özel HSM'ler sanal ağdaki bir alt ağa dağıtılır. Müşterinin alt ağına dağıtılan her özel HSM aygıtı, bu alt ağdan özel bir IP adresi alır. HSM aygıtının dağıtıldığı alt ağ, hizmete açıkça devredilmesi gerekir: Microsoft.HardwareSecurityModules/dedicatedHSMs. Bu, alt ağa dağıtım için HSM hizmetine belirli izinler verir. Özel HSM'ler delegasyonu, alt ağa belirli ilke kısıtlamaları uygular. Ağ Güvenlik Grupları (NSG'ler) ve Kullanıcı Tanımlı Yollar (ÜDS) şu anda devredilen alt ağlarda desteklenmemektedir. Sonuç olarak, bir alt ağ özel HSM'lere devredildikten sonra, yalnızca HSM kaynaklarını dağıtmak için kullanılabilir. Diğer müşteri kaynaklarının alt ağa gönderilmesi başarısız olur.


### <a name="expressroute-gateway"></a>ExpressRoute ağ geçidi

Geçerli mimarinin gereksinimi, HSM aygıtının Azure'a entegrasyonunu etkinleştirmek için bir HSM aygıtının yerleştirilmesi gereken müşteri alt ağına bir ER ağ geçidiyapılandırmasıdır. Bu ER ağ geçidi, şirket içi konumları Azure'daki müşterilere hsm aygıtlarına bağlamak için kullanılamaz.

## <a name="connecting-your-on-premises-it-to-azure"></a>Şirket içi BT'nizi Azure'a bağlama

Bulut tabanlı kaynaklar oluştururken, şirket içi BT kaynaklarına özel bir bağlantı için tipik bir gereksinimdir. Özel HSM durumunda, bu ağırlıklı olarak HSM istemci yazılımı için HSM aygıtları yapılandırmak için ve aynı zamanda yedekleme ve analiz için HSM'lerden günlükleri çekme gibi etkinlikler için olacaktır. Burada önemli bir karar noktası seçenekler olduğu gibi bağlantının doğasıdır.  Azure bulutundaki kaynaklarla (HSM'ler dahil) güvenli iletişim gerektiren birden fazla şirket içi kaynak olacağından, en esnek seçenek Site'den Siteye VPN'dir. Bu, bir müşteri kuruluşunun bağlantıyı kolaylaştırmak için bir VPN aygıtına sahip olmasını gerektirir. Tek bir yönetim iş istasyonu gibi şirket içinde yalnızca tek bir uç nokta varsa, Site'ye noktadan sayfavpn bağlantısı kullanılabilir.
Bağlantı seçenekleri hakkında daha fazla bilgi için [VPN Ağ Geçidi planlama seçeneklerine](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#planningtable)bakın.

> [!NOTE]
> Şu anda, ExpressRoute şirket içi kaynaklara bağlantı için bir seçenek değildir. Ayrıca, yukarıda açıklandığı gibi kullanılan ExpressRoute Ağ Geçidi'nin şirket içi altyapıya bağlantılar için olmadığı da unutulmamalıdır.

### <a name="point-to-site-vpn"></a>Noktadan Siteye VPN

Noktadan siteye Sanal Özel Ağ, şirket içinde tek bir uç noktaya güvenli bağlantının en basit biçimidir. Azure tabanlı özel HSM'ler için yalnızca tek bir yönetim iş istasyonuna sahip olmak istiyorsanız, bu durum alakalı olabilir.

### <a name="site-to-site-vpn"></a>Siteler arası VPN

Siteden siteye Sanal Özel Ağ, Azure tabanlı Özel HSM'ler ile şirket içi BT'niz arasında güvenli iletişim sağlar. Bunu yapmak için bir neden HSM's şirket içinde bir yedekleme tesisi olan ve yedekleme çalıştırmak için ikisi arasında bir bağlantı gerekir.

## <a name="connecting-virtual-networks"></a>Sanal ağları bağlama

Özel HSM için tipik bir dağıtım mimarisi, HSM aygıtlarının oluşturulduğu ve sağlandığı tek bir sanal ağ ve ilgili alt ağla başlar. Aynı bölge içinde, Özel HSM'den yararlanacak uygulama bileşenleri için ek sanal ağlar ve alt ağlar olabilir. Bu ağlar arasında iletişimi etkinleştirmek için Sanal Ağ Eşlemesini kullanırız.

### <a name="virtual-network-peering"></a>Sanal ağ eşleme

Bir bölgede bir birinin kaynaklarına erişilmesi gereken birden çok sanal ağ varsa, Sanal Ağ Eşleme aralarında güvenli iletişim kanalları oluşturmak için kullanılabilir.  Sanal ağ eşleme, yalnızca güvenli iletişim sağlamakla kalmıyor, aynı zamanda Azure'daki kaynaklar arasında düşük gecikme li ve yüksek bant genişliğine bağlı bağlantılar da sağlar.

![ağ eşleme](media/networking/peering.png)

## <a name="connecting-across-azure-regions"></a>Azure Bölgeleri arasında bağlanma

HSM aygıtları, yazılım kitaplıkları aracılığıyla trafiği alternatif bir HSM'ye yönlendirme yeteneğine sahiptir. Aygıtlar arızalanırsa veya aygıta erişim kaybolursa trafik yeniden yönlendirmesi yararlıdır. Bölgesel düzey hata senaryoları, diğer bölgelerde HSM'ler dağıtılarak ve bölgeler arasında sanal ağlar arasında iletişimi etkinleştirerek azaltılabilir.

### <a name="cross-region-ha-using-vpn-gateway"></a>VPN ağ geçidini kullanarak çapraz bölge HA

Genel olarak dağıtılan uygulamalar veya yüksek kullanılabilirlik bölgesel başarısız senaryoları için, bölgeler arasında sanal ağları bağlamak gerekir. Azure Özel HSM ile, iki sanal ağ arasında güvenli bir tünel sağlayan bir VPN Ağ Geçidi kullanılarak yüksek kullanılabilirlik elde edilebilir. VPN Ağ Geçidi'ni kullanarak Vnet-to-Vnet bağlantıları hakkında daha fazla bilgi için VPN Ağ Geçidi nedir başlıklı makaleye [bakın?](../vpn-gateway/vpn-gateway-about-vpngateways.md#V2V)

> [!NOTE]
> Global Vnet eşleme şu anda Özel HSM'ler ile bölgeler arası bağlantı senaryolarında kullanılamaz ve bunun yerine VPN ağ geçidi kullanılmalıdır. 

![global-vnet](media/networking/global-vnet.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Sık sorulan sorular](faq.md)
- [Desteklenebilirlik](supportability.md)
- [Yüksek kullanılabilirlik](high-availability.md)
- [Fiziksel Güvenlik](physical-security.md)
- [İzleme](monitoring.md)
- [Dağıtım mimarisi](deployment-architecture.md)