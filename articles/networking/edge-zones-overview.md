---
title: Azure Edge bölgesi önizlemesi hakkında
description: "Microsoft 'tan Edge bilgi işlem teklifleri hakkında bilgi edinin: Azure Edge bölgesi."
services: vnf-manager
author: ganesr
ms.service: vnf-manager
ms.topic: article
ms.date: 04/02/2020
ms.author: ganesr
ms.openlocfilehash: 90e796c244950d6d374a02757b608099c229c1ea
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146941"
---
# <a name="about-azure-edge-zone-preview"></a>Azure Edge bölgesi önizlemesi hakkında

Azure Edge bölgesi, kullanıcıya veri işlemeyi kapatma imkanı sağlayan Microsoft Azure bir teklif ailesidir. Uygulamaların düşük gecikme süresi ve yüksek performans gereksinimlerini karşılamak için VM 'Leri, kapsayıcıları ve diğer seçili Azure hizmetlerini uç bölgelere dağıtabilirsiniz.

Edge bölgeleri için tipik kullanım örneği senaryoları şunları içerir:

- Robotics 'de gerçek zamanlı komut ve denetim.
- Yapay zeka ve makine öğrenimi ile gerçek zamanlı analiz ve ınayri
- Makine vizyonu.
- Karma Gerçeklik ve VDı senaryoları için uzaktan işleme.
- Modern çok oyunculu oyun.
- Medya akışı ve içerik teslimi.
- İzleme ve güvenlik.

Üç tür Azure Edge bölgesi vardır:

- Azure Edge bölgeleri
- Taşıyıcı ile Azure Edge bölgeleri
- Azure özel kenar bölgeleri

## <a name="azure-edge-zones"></a><a name="edge-zones"></a>Azure Edge bölgeleri

![Azure Edge bölgeleri](./media/edge-zones-overview/edge-zones.png "Azure Edge bölgeleri")

Azure Edge bölgeleri, Azure bölgelerindeki Azure bölgelerinden en fazla yer alan popülasyon merkezlerine yerleştirilmiş Azure 'un küçük ve küçük boyut özellikli uzantılarıdır. Azure Edge bölgeleri, gecikme süresi duyarlı ve yoğun işleme kullanan uygulamaları son kullanıcılara yakın bir şekilde çalıştırmanızı sağlayan VM 'Leri, kapsayıcıları ve seçili bir Azure hizmetleri kümesini destekler. Azure Edge bölgeleri, Microsoft Global Network 'ün bir parçasıdır. Bu kişiler, uç bölgede çalışan uygulamalar arasında kullanıcıya yakın güvenli, güvenilir, yüksek bant genişliğine bağlantı sağlar. Azure bölgelerinde çalışan Azure hizmetlerinin tam kümesini sunar. Azure Edge bölgeleri, Microsoft tarafından aittir ve çalıştırılır. Hizmetleri uç bölgelere yönetmek ve dağıtmak için aynı Azure araçları ve aynı portalın kümesini kullanabilirsiniz.

Yaygın kullanım örnekleri şunlardır:

- Oyun ve oyun akışı.
- Medya akışı ve içerik teslimi.
- Yapay zeka ve makine öğrenimi ile gerçek zamanlı analiz ve ınayri
- Karma Gerçeklik için işleme.

Azure Edge bölgeleri aşağıdaki Metro alanlarında kullanılabilir olacaktır:

- New York, NY
- Los Angeles, CA
- Miami, FL

Daha fazla bilgi için [Edge bölgesi ekibine başvurun](https://aka.ms/EdgeZones) .

## <a name="azure-edge-zones-with-carrier"></a><a name="carrier"></a>Taşıyıcı ile Azure Edge bölgeleri

![Taşıyıcı ile kenar bölgeleri](./media/edge-zones-overview/edge-carrier.png "Taşıyıcı ile kenar bölgeleri")

Taşıyıcı ile Azure Edge bölgeleri, Azure 'un, mobil operatörlerin popülasyon merkezlerindeki veri merkezlerine yerleştirilmiş küçük ayak izi uzantılarına sahiptir. Taşıyıcı altyapısına sahip Azure Edge bölgesi, mobil işlecin 5G ağından bir atlama yerleştirmiştir. Bu yerleştirme, mobil cihazlardan uygulamalara 10 milisaniyeden kısa gecikme süresi sağlar.

Taşıyıcının bulunduğu Azure Edge bölgeleri, mobil operatörlerin veri merkezlerinde dağıtılır ve Microsoft Global Network 'e bağlanır. Bunlar, kullanıcıya yakın çalışan uygulamalar arasında güvenli, güvenilir, yüksek bant genişliğine sahip bir bağlantı sağlar. Azure bölgelerinde çalışan Azure hizmetlerinin tam kümesini sunar. Geliştiriciler, hizmet oluşturmak ve uç bölgelere dağıtmak için aynı tanıdık araç kümesini kullanabilir.

Yaygın kullanım örnekleri şunlardır:

- Oyun ve oyun akışı.
- Medya akışı ve içerik teslimi.
- Yapay zeka ve makine öğrenimi ile gerçek zamanlı analiz ve ınayri
- Karma Gerçeklik için işleme.
- Bağlı otomobil 'leri.
- Tele-ilaç.

Edge bölgeleri aşağıdaki işleçlerle iş ortaklığı içinde sunulacaktır:

- &T (Atlanta, Dallas ve Los Angeles)

## <a name="azure-private-edge-zones"></a><a name="private-edge-zones"></a>Azure özel kenar bölgeleri

![Özel kenar bölgeleri](./media/edge-zones-overview/private-edge.png "Özel kenar bölgeleri")

Azure özel sınır bölgeleri, şirket içinde yer alan Azure 'un küçük pencere kaplama uzantılarıdır. Azure özel kenar bölgesi [Azure Stack Edge](https://azure.microsoft.com/products/azure-stack/edge/) platformunu temel alır. Şirket içinde dağıtılan bilgi işlem ve depolama hizmetlerine düşük gecikmeli erişim sağlar. Özel kenar bölgesi ayrıca ISV 'leri ve sanallaştırılmış ağ işlevlerini (VNFs) Şirket içi sanal makineler ve kapsayıcılarla birlikte [Azure yönetilen uygulamalar](https://azure.microsoft.com/services/managed-applications/) olarak dağıtmanızı sağlar. Bu VNFs, mobil paket çekirdekleri, yönlendiriciler, güvenlik duvarları ve SD-WAN gereçlerine sahip olabilir. Azure özel kenar bölgesi, Azure portal VNFs ve uygulamalarının yaşam döngülerini yönetmenize olanak tanıyan, bulut Yerel bir düzenleme çözümüyle birlikte gelir.

Azure özel kenar bölgesi, Azure 'da uygulama derlemek ve dağıtmak için kullandığınız tanıdık araçları kullanarak şirket içinde uygulamalar geliştirmenize ve dağıtmanıza olanak tanır. 

Ayrıca şunları yapmanızı sağlar: 

- Özel mobil ağları çalıştırın (özel LTE, özel 5 g).
- Güvenlik duvarları gibi güvenlik işlevlerini uygulayın.
- Aynı özel kenar bölgesi gereçlerinde SD-WAN gereçlerini kullanarak şirket içi ağlarınızı birden çok Dalla ve Azure arasında genişletin ve bunları Azure 'dan yönetin.

Yaygın kullanım örnekleri şunlardır:

- Robotics 'de gerçek zamanlı komut ve denetim.
- Yapay zeka ve makine öğrenimi ile gerçek zamanlı analiz ve ınayrikeli.
- Makine vizyonu.
- Karma Gerçeklik ve VDı senaryoları için uzaktan işleme.
- İzleme ve güvenlik.

Özel kenar bölgelerini kullanan uçtan uca çözümleri etkinleştirmek için bir VNF satıcıları, ISV 'Ler ve MSP iş ortakları zengin ekosistemimiz vardır. Daha fazla bilgi için [özel Edge bölgesi ekibine başvurun](https://aka.ms/EdgeZonesPartner) .

### <a name="private-edge-zone-partners"></a><a name="private-edge-partners"></a>Özel sınır dilimi iş ortakları

![Özel sınır dilimi iş ortakları](./media/edge-zones-overview/partners.png "Özel sınır bölgeleri iş ortakları")

#### <a name="virtualized-network-functions-vnfs"></a><a name="vnf"></a>Sanallaştırılmış ağ işlevleri (VNFs)

##### <a name="virtualized-evolved-packet-core-vepc-for-mobile-networks"></a><a name="vEPC"></a>Mobil ağlar için sanallaştırılmış paket çekirdeği (vEPC)

- [Afirmış ağlar](https://www.affirmednetworks.com/)
- [Druid yazılımı](https://www.druidsoftware.com/)
- [Expeto](https://www.expeto.io/)
- [Mavenir](https://mavenir.com/)
- [Metaswitch](https://www.metaswitch.com/)
- [Nokia dijital Otomasyonu bulutu](https://www.dac.nokia.com/)

##### <a name="mobile-radio-partners"></a><a name="mobile-radio"></a>Mobil radyo iş ortakları

- [CommScope Ruckus](https://support.ruckuswireless.com/)

##### <a name="sd-wan-vendors"></a><a name="sdwan-vendors"></a>SD-WAN satıcıları

- [Netlen Ry](https://netfoundry.io/)
- [Nokia 'den Nuage ağları](https://www.nuagenetworks.net/)
- [VMware SD-WAN by Velocloud](https://www.velocloud.com/)

##### <a name="router-vendors"></a><a name="router-vendors"></a>Yönlendirici satıcıları

- [Arista](https://www.arista.com/)

##### <a name="firewall-vendors"></a><a name="firewall-vendors"></a>Güvenlik Duvarı satıcıları

- [Palo Alto Networks](https://www.paloaltonetworks.com/)

##### <a name="managed-solutions-providers-mobile-operators-and-global-system-integrators-gsis"></a><a name="msp-mobile"></a>Yönetilen çözüm sağlayıcıları: mobil işleçler ve küresel sistem tümleştiricileri (Gsıs)

| Gsıs ve işleçler | Mobil işleçler |
| --- | --- |
| Amdocs                       | Etisalat             |
| American Tower               | NTT Communications   |
| CenturyLink                  | Proximus             |
| Expeto                       | Rogers               |
| Federasyon kablosuz           | SK telekomünikasyon           |
| Bilgi sys                      | Telefonica           |
| Tech Mahindra                | Telstra              |
|                              | Vodafone             |

Nasıl iş ortağı olacağı hakkında bilgi edinmek için [özel kenar bölgesi ekibine başvurun](https://aka.ms/EdgeZonesPartner) .

### <a name="private-edge-zone-solutions"></a><a name="solutions-private-edge"></a>Özel sınır dilimi çözümleri

#### <a name="private-mobile-network-on-private-edge-zones"></a><a name="private-mobile-private-edge"></a>Özel Kenar bölgelerinde özel mobil ağ

![Özel Kenar bölgelerinde özel mobil ağ](./media/edge-zones-overview/mobile-networks.png "Özel Kenar bölgelerinde özel mobil ağ")

Artık özel Kenar bölgelerinde özel bir mobil ağ dağıtabilirsiniz. Özel mobil ağlar, son derece düşük gecikme süresi, yüksek kapasite ve iş açısından kritik uygulamalar için gereken güvenilir ve güvenli kablosuz ağları etkinleştirir. 

Özel mobil ağlar, şunun gibi senaryolar etkinleştirebilir: 
- Ambarlarda otomatik Kılavuzlu araçlar (AGVs) komutu ve denetimi. 
- Akıllı fabrikalara ait robots ile gerçek zamanlı iletişim.
- Genişletmüş gerçeklik ve sanal gerçeklik Edge uygulamaları.

Sanallaştırılmış bir paket çekirdeği (vEPC) ağ işlevi, özel bir mobil ağın fsınbıdır. Artık özel Kenar bölgelerinde bir vEPC dağıtımı yapabilirsiniz. Özel sınır bölgelerinde bulunan vEPC iş ortaklarının listesi için bkz. [vepc ISV 'ler](#vEPC).

Özel Kenar bölgelerinde özel bir mobil ağ çözümünü dağıtmak, mobil erişim noktaları, SIM kartlar ve diğer VNET gibi diğer sanal NFS gibi diğer bileşenleri gerektirir. Lisanslı veya lisanssız bir spekten erişim, özel bir mobil ağ kurmak için önemlidir. Ve bu, RF planlaması, fiziksel düzen, yükleme ve destek ile ilgili yardıma ihtiyacınız bulunabilir. İş ortaklarının listesi için bkz. [mobil radyo ortakları](#mobile-radio).

Microsoft, bu işlemin tüm yönleriyle yardımcı olabilecek bir iş ortağı ekosistemi sağlar. İş ortakları, ağ planlama, gerekli cihazları satın alma, donanım ayarlama ve yapılandırmayı Azure 'dan yönetme konusunda yardımcı olabilir. Microsoft ile sıkı bir şekilde tümleştirilmiş bir doğrulanan iş ortakları kümesi, çözümünüzün güvenilir ve kolay bir şekilde kullanılmasını sağlar. Geri kalanında yardımcı olması için temel senaryolarınıza odaklanabilir ve Microsoft ve iş ortaklarına güvenebilirsiniz.

#### <a name="sd-wan-on-private-edge-zones"></a><a name="sdwan-private-edge"></a>SD-WAN-özel Kenar bölgelerinde

![SD-WAN-özel Kenar bölgelerinde](./media/edge-zones-overview/sd-wan.png "SD-WAN-özel Kenar bölgelerinde")
 
SD-WAN, şu avantajlara sahip kurumsal düzeyde geniş alan ağları (WAN) oluşturmanızı sağlar:

- Artan bant genişliği
- Buluta yüksek performanslı erişim
- Hizmet ekleme
- Güvenilirlik
- İlke yönetimi
- Kapsamlı ağ görünürlüğü
    
SD-WAN, yedekli yönetim denetleyicilerinden daha düşük düzeyde mülkiyet maliyetinde düzenlenmiş sorunsuz şube ofisi bağlantısı sağlar.
SD-WAN-özel sınır bölgelerinde, bir CAPEX merkezli modelden, BT bütçelerini azaltmak için bir hizmet olarak yazılım (SaaS) modeline geçiş yapmanızı sağlar. Yeni hizmetleri etkinleştirmek ve tüm ağ üzerinden hemen yaymak için, tercih ettiğiniz SD-WAN iş ortakları, Orchestrator veya Controller seçeneğini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki takımlara başvurun:

* [Edge bölgesi ekibi](https://aka.ms/EdgeZones)
* [İş ortağı olmak için özel Edge bölgesi ekibi](https://aka.ms/EdgeZonesPartner)
