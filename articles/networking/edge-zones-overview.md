---
title: Azure Kenar Bölgeleri Hakkında - Önizleme
description: Microsoft'un kenar bilgi işlem teklifleri hakkında bilgi edinin.
services: vnf-manager
author: ganesr
ms.service: vnf-manager
ms.topic: article
ms.date: 03/31/2020
ms.author: ganesr
ms.openlocfilehash: 4666022fc62fcb423906b46f0ff2274a2191f341
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437620"
---
# <a name="about-azure-edge-zones---preview"></a>Azure Kenar Bölgeleri Hakkında - Önizleme

Azure Kenar Bölgeleri, Microsoft Azure'dan kullanıcıya yakın veri işleme sağlayan bir teklif ailesidir. Düşük gecikme süresi ve yüksek iş verme gereksinimlerini gidermek için VM'leri, kapsayıcıları ve diğer belirli Azure hizmetlerini Kenar Bölgeleri'ne dağıtabilirsiniz.

Kenar Bölgeleri için tipik kullanım durumu senaryoları şunlardır:

- Robotikte gerçek zamanlı komuta ve kontrol
- Yapay Zeka ve Makine Öğrenimi ile gerçek zamanlı analiz ve inferencing
- Yapay Görme
- Karışık gerçeklik ve VDI senaryoları için uzaktan görüntüleme
- Sürükleyici çok oyunculu oyun
- Medya akışı ve içerik dağıtımı
- Gözetim ve güvenlik

Azure Kenar Bölgeleri üç ayrı teklifte gelir:

- Azure Kenar Bölgeleri
- Operatörlü Azure Kenar Bölgeleri
- Azure Özel Kenar Bölgeleri

## <a name="azure-edge-zones"></a><a name="edge-zones"></a>Azure Kenar Bölgeleri

![Kenar Bölgeleri](./media/edge-zones-overview/edge-zones.png "Kenar bölgeleri")

Azure Kenar Bölgeleri, Azure bölgelerinden çok uzaktaki nüfus merkezlerine yerleştirilen Azure'un küçük ayak izi uzantılarıdır. Azure Kenar Bölgeleri, gecikmeye duyarlı ve son kullanıcılara yakın yoğun uygulamalar çalıştırmanıza izin veren vm'leri, kapsayıcıları ve belirli bir Azure hizmet kümesini destekler. Azure Kenar Bölgeleri Microsoft global ağının bir parçasıdır ve Edge Zone'da kullanıcıya yakın çalışan uygulamalar ile Azure bölgeleri içinde çalışan tüm Azure hizmetleri arasında güvenli, güvenilir ve yüksek bant genişliğine sahip bağlantı sunar. Azure Kenar Bölgeleri Microsoft'a aittir ve Microsoft tarafından işletilmektedir ve hizmetleri yönetmek ve Kenar Bölgeleri'ne dağıtmak için aynı Azure araçları ve portalı kümesini kullanmanıza olanak tanır.

Tipik kullanım örnekleri şunlardır:

- Oyun ve oyun akışı
- Medya akışı ve içerik dağıtımı
- Yapay zeka ve makine öğrenimi kullanarak gerçek zamanlı analitik ve çıkarı
- Karışık gerçeklik için render

Azure Kenar Bölgeleri aşağıdaki metrolarda kullanılabilir:

- New York, NY
- Los Angeles, Kaliforniya
- Miami, FL

Daha fazla bilgi [için Edge Zones ekibine ulaşın.](https://aka.ms/EdgeZones)

## <a name="azure-edge-zones-with-carrier"></a><a name="carrier"></a>Operatörlü Azure Kenar Bölgeleri

![Taşıyıcılı Kenar Bölgeleri](./media/edge-zones-overview/edge-carrier.png "Taşıyıcılı Kenar Bölgeleri")

Operatörlü Azure Kenar Bölgeleri, azure'un nüfus merkezlerindeki cep telefonu operatörlerinin veri merkezlerine yerleştirilen küçük ayak izi uzantılarıdır. Taşıyıcı altyapısına sahip Azure Kenar Bölgeleri, mobil cihazlardan gelen uygulamalara 10 milisaniyenin altında gecikme süresi sunan mobil operatörün 5G ağından bir adım uzakta yerleştirilir. Operatörlü Azure Kenar Bölgeleri, cep telefonu operatörlerinin veri merkezlerinde dağıtılır ve Microsoft küresel ağına bağlanır. Kullanıcıya yakın çalışan uygulamalar la Azure bölgelerinde çalışan tüm Azure hizmetleri arasında güvenli, güvenilir ve yüksek bant genişliği bağlantısı sunar. Geliştiriciler, kenar bölgelerine hizmet oluşturmak ve dağıtmak için aynı tanıdık araçlar kümesini kullanabilir.

Tipik kullanım örnekleri şunlardır:

- Oyun ve oyun akışı
- Medya akışı ve içerik dağıtımı
- Yapay zeka ve makine öğrenimi kullanarak gerçek zamanlı analitik ve çıkarı
- Karışık gerçeklik için render
- Bağlı otomobiller
- Teletıp

Kenar Bölgeleri aşağıdaki operatörlerle işbirliği içinde sunulacaktır:

- AT&T (Atlanta, Dallas ve Los Angeles)
- Etisalat
- Rogers
- Vodafone

## <a name="azure-private-edge-zones"></a><a name="private-edge-zones"></a>Azure Özel Kenar Bölgeleri

![Özel Kenar Bölgeleri](./media/edge-zones-overview/private-edge.png "Özel Kenar Bölgeleri")

Azure Özel Kenar Bölgeleri, Azure'un şirket içi yerleştirilen küçük ayak izi uzantılarıdır. Azure Özel Kenar Bölgeleri, [Azure Yığını Kenarı](https://azure.microsoft.com/products/azure-stack/edge/) platformuna dayanır ve şirket içinde dağıtılan bilgi işlem ve depolama hizmetlerine düşük gecikme gecikmesi erişimi sağlar. Özel Kenar Bölgeleri ayrıca mobil paket çekirdekleri, yönlendiriciler, güvenlik duvarları ve SD-WAN cihazları gibi sanallaştırılmış ağ işlevlerini (VNFs) ve ISVS'den gelen uygulamaları Azure uygulamaları sanal makineler ve kapsayıcılarla birlikte yan yana [yöneterken](https://azure.microsoft.com/services/managed-applications/) dağıtmanıza da olanak tanır. Azure Özel Kenar Bölgeleri, Sanallaştırılmış Ağ Fonksiyonlarının (VNF) yaşam döngüsünü ve Azure portalındaki uygulamaları yönetmenize olanak tanıyan bulut ayarı kullanılabilirliği yle birlikte gelir.

Azure Özel Kenar Bölgeleri, Azure'da uygulamaları oluşturmak ve dağıtmak için kullanılan aynı tanıdık araçları kullanarak uygulamaları şirket içinde geliştirmenize ve dağıtmanıza olanak tanır. Ayrıca özel mobil ağları (özel LTE, özel 5G), güvenlik duvarları gibi güvenlik işlevleri ni çalıştırabilir ve aynı Özel Kenar Bölgesi cihazlarındaki SD-WAN cihazlarını kullanarak şirket içi ağlarınızı birden çok şubeye ve Azure'a genişletebilir ve Azure'dan yönetebilirsiniz.

Tipik kullanım örnekleri şunlardır:

- Robotikte gerçek zamanlı komuta ve kontrol
- Yapay Zeka ve Makine Öğrenimi ile gerçek zamanlı analiz ve inferencing
- Yapay Görme
- Karışık gerçeklik ve VDI senaryoları için uzaktan görüntüleme
- Gözetim ve güvenlik

Özel Kenar Bölgeleri'ni kullanarak uçtan uca çözümler etkinleştirmek için VNF satıcıları, ISV'ler ve MSP iş ortaklarından oluşan zengin bir ekosistemimiz vardır. Daha fazla bilgi [için Özel Kenar Bölgeleri ekibine ulaşın.](https://aka.ms/EdgeZonesPartner)

## <a name="private-edge-zones---partners"></a><a name="private-edge-partners"></a>Özel Kenar Bölgeleri - Ortaklar

![Özel Kenar Bölgesi Ortakları](./media/edge-zones-overview/partners.png "Özel Kenar Bölgesi Ortakları")

### <a name="virtualized-network-functions-vnfs"></a><a name="vnf"></a>Sanallaştırılmış Ağ Fonksiyonları (VNFs)

#### <a name="virtualized-evolved-packet-core-vepc-for-mobile-networks"></a><a name="vEPC"></a>Mobil Ağlar için Sanallaştırılmış Gelişmiş Paket Çekirdek (vEPC)

- [Onaylanan Ağlar](https://www.affirmednetworks.com/)
- [Druid Yazılım](https://www.druidsoftware.com/)
- [Expeto](https://www.expeto.io/)
- [Mavenir](https://mavenir.com/)
- [Metaswitch](https://www.metaswitch.com/)
- [Nokia Dijital Otomasyon Bulutu](https://www.dac.nokia.com/)

#### <a name="mobile-radio-partners"></a><a name="mobile-radio"></a>Mobil Radyo Ortakları

- [Commscope Ruckus](https://support.ruckuswireless.com/)

#### <a name="sd-wan-vendors"></a><a name="sdwan-vendors"></a>SD-WAN Satıcıları

- [NetFoundry](https://netfoundry.io/)
- [Nokia'dan NuageNetworks](https://www.nuagenetworks.net/)
- [VMware SD-WAN tarafından Velocloud](https://www.velocloud.com/)

#### <a name="router-vendors"></a><a name="router-vendors"></a>Yönlendirici Satıcıları

- [Öz](https://www.arista.com/)

Ortak olma hakkında daha fazla bilgi için [Özel Kenar Bölgeleri ekibine ulaşın.](https://aka.ms/EdgeZonesPartner)

#### <a name="firewall-vendors"></a><a name="firewall-vendors"></a>Güvenlik Duvarı Satıcıları

- Palo Alto Networks

### <a name="managed-solutions-providers---mobile-operators-and-global-system-integrators"></a><a name="msp-mobile"></a>Yönetilen Çözüm Sağlayıcıları - Mobil Operatörler ve Küresel Sistem Entegratörleri

| Global SIs ve Operatörler | Mobil Operatörler |
| --- | --- |
| Amdocs                       | Etisalat             |
| Amerikan Kulesi               | Rogers               |
| Expeto                       | Singtel              |
| Federe Kablosuz           | Vodafone             |
| ınfosys                      |      *                |
| Teknik Mahindra                |      *                |

Ortak olma hakkında daha fazla bilgi için [Özel Kenar Bölgeleri ekibine ulaşın.](https://aka.ms/EdgeZonesPartner)

## <a name="private-edge-zones---solutions"></a><a name="solutions-private-edge"></a>Özel Kenar Bölgeleri - Çözümler

### <a name="private-mobile-network-on-private-edge-zones"></a><a name="private-mobile-private-edge"></a>Özel Kenar Bölgelerinde Özel Mobil Ağ

![Özel Kenar Bölgelerinde Özel Mobil Ağ](./media/edge-zones-overview/mobile-networks.png "Özel Kenar Bölgelerinde Özel Mobil Ağ")

Artık özel kenar bölgelerinde özel bir mobil ağ dağıtabilirsiniz. Özel mobil ağlar, iş açısından kritik uygulamalar için gerekli olan ultra düşük gecikme süresi, yüksek kapasite ve güvenilir ve güvenli bir kablosuz ağ sağlar. Özel mobil ağlar, bir depoda otomatik kılavuzlu araçların (AGV) komuta sı ve kontrolü, akıllı bir fabrikadaki robotlar ve artırılmış gerçeklik uygulamaları ve sanal gerçeklik kenarı uygulamaları gibi senaryolara olanak sağlar.

Sanallaştırılmış gelişmiş paket çekirdek (vEPC) ağ fonksiyonu özel bir mobil ağın beynini oluşturur. Artık Özel Kenar Bölgelerinde bir vEPC dağıtabilirsiniz. Özel kenar bölgelerinde kullanılabilen vEPC iş ortaklarının listesi için [vEPC ISV'lere](#vEPC)bakın.

Özel Kenar Bölgeleri'nde özel bir mobil ağ çözümü dağıtmak, mobil erişim noktaları, SIM kartlar ve yönlendiriciler gibi diğer VNF'ler gibi diğer bileşenleri gerektirir. Lisanslı veya lisanssız spektruma erişim, özel bir mobil ağ kurmak için çok önemlidir. Ayrıca, RF planlama, fiziksel düzen, yükleme ve destek konusunda yardıma ihtiyacınız olabilir. İş ortaklarının listesi için [Mobil radyo ortakları](#mobile-radio)bölümüne bakın.

Microsoft, ağı planlamaktan, gerekli aygıtları satın almaktan donanım ı kurmaktan yapılandırmayı Azure'dan yönetmeye kadar bu sürecin tüm yönleriyle yardımcı olabilecek bir iş ortağı ekosistemi sağlar. Microsoft ile sıkı bir şekilde entegre edilmiş bir dizi doğrulanmış iş ortağıyla, çözümün güvenilir ve kullanımı kolay olduğundan emin olabilirsiniz. Geri kalanı yla ilgili yardımcı olması için Microsoft'a ve iş ortaklarına güvenirken temel senaryolarınıza odaklanabilirsiniz.

### <a name="sd-wan-on-private-edge-zones"></a><a name="sdwan-private-edge"></a>Özel Kenar Bölgelerinde SD-WAN

![Özel Kenar Bölgelerinde SD-WAN](./media/edge-zones-overview/sd-wan.png "Özel Kenar Bölgelerinde SD-WAN")
 
SD-WAN bir teknoloji olarak, artan bant genişliği, buluta yüksek performanslı erişim, hizmet ekleme, güvenilirlik, ilke yönetimi ve kapsamlı ağ görünürlüğü ile kurumsal sınıf Geniş Alan Ağları (WAN) oluşturmanıza olanak tanır. SD-WAN, daha düşük sahip olma maliyetiyle gereksiz merkezi denetleyicilerden yönetilen kesintisiz şube bağlantısı sağlar.
Özel Kenar Bölgelerindeki SD-WAN, BT bütçelerini azaltmak için capex merkezli modelden hizmet olarak yazılım (SaaS) modeline geçmenizi sağlar. Yeni hizmetleri etkinleştirmek ve bunları hemen tüm ağa yaymak için SD-WAN iş ortakları orkestratör veya denetleyici seçtiğiniz kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki takımlara ulaşın:

* [Kenar Bölgeleri ekibi](https://aka.ms/EdgeZones)
* [Özel Kenar Bölgeleri ekibi - Ortak olmak için](https://aka.ms/EdgeZonesPartner)
