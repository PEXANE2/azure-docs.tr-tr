---
title: 'Konumlar ve bağlantı sağlayıcıları: Azure ExpressRoute | Microsoft Docs'
description: Bu makale, sunulan hizmetlerin konumları ve Azure bölgelerine nasıl bağlanılacağı hakkında ayrıntılı bir genel bakış sağlar. Konuma göre sıralanmıştır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: cherylmc
ms.openlocfilehash: 0b8c8a0baba5a80c6a383e6f8f14e29b482dbc82
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878621"
---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute ortakları ve eşleme konumları

> [!div class="op_single_selector"]
> * [Sağlayıcıya Göre Konumlar](expressroute-locations.md)
> * [Konuma Göre Sağlayıcılar](expressroute-locations-providers.md)


Bu makaledeki tablolar, ExpressRoute coğrafi kapsamı ve konumları, ExpressRoute bağlantı sağlayıcıları ve ExpressRoute Sistem Entegratörleri (SI) hakkında bilgi sağlar.

> [!Note]
> Azure bölgeleri ve ExpressRoute konumları iki farklı ve farklı kavramdır, ikisi arasındaki farkı anlamak Azure karma ağ bağlantısını keşfetmek için çok önemlidir. 
>
>

## <a name="azure-regions"></a>Azure bölgeleri
Azure bölgeleri, Azure bilgi işlem, ağ ve depolama kaynaklarının bulunduğu genel veri merkezleridir. Azure kaynağı oluştururken, müşterinin bir kaynak konumu seçmesi gerekir. Kaynak konumu, kaynağın hangi Azure veri merkezinde (veya kullanılabilirlik bölgesinde) oluşturulduğunu belirler.

## <a name="expressroute-locations"></a>ExpressRoute konumları
ExpressRoute konumları (bazen eşleme konumları veya buluşma konumları olarak da adlandırılır), Microsoft Enterprise edge (MSEE) aygıtlarının bulunduğu ortak konum olanaklarıdır. ExpressRoute konumları Microsoft'un ağının giriş noktasıdır ve müşterilere microsoft'un dünya çapındaki ağına bağlanma fırsatı sağlayarak küresel olarak dağıtılır. Bu konumlar, ExpressRoute iş ortaklarının ve ExpressRoute Direct müşterilerinin Microsoft ağına çapraz bağlantı lar sayönettiği konumlardır. Genel olarak, ExpressRoute konumunun Azure bölgesiyle eşleşmesi gerekmez. Örneğin, bir müşteri *Seattle* Peering konumunda, *Doğu ABD,* kaynak konumu ile bir ExpressRoute devre oluşturabilirsiniz.

Coğrafi bölge içindeki en az bir ExpressRoute konumuna bağlanırsanız coğrafi bölge içindeki tüm bölgeler arasında Azure hizmetlerine erişebileceksiniz. 

## <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a><a name="locations"></a>Bir coğrafi bölge içindeki Azure bölgeler ile ExpressRoute konumları arasında eşleme
Aşağıdaki tablo, coğrafi bölge içindeki Azure bölgeler ile ExpressRoute konumları arasında yapılan eşlemeyi sağlar.

| **Jeopolitik bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- |
| **Australia Government** | Orta Avustralya, Orta Avustralya 2 |Kanberra, Kanberra2 |
| **Avrupa** | Fransa Merkez, Fransa Güney, Almanya Kuzey, Almanya Batı Orta, Kuzey Avrupa, Norveç Doğu, Norveç Batı, İsviçre Kuzey, İsviçre Batı, İngiltere Batı, İngiltere Güney, Batı Avrupa |Amsterdam, Amsterdam2, Kopenhag, Dublin, Frankfurt, Cenevre, Londra, Londra2, Marsilya, Milano, Münih, Newport(Galler), Oslo, Paris, Stavanger, Stockholm, Zürih, Münih |
| **Kuzey Amerika** | Doğu ABD, Batı ABD, Doğu ABD 2, Batı ABD 2, Orta ABD, Orta Güney ABD, Orta Kuzey ABD, Orta Batı ABD, Orta Kanada, Doğu Kanada |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silikon Vadisi, Silikon Vadisi2, Washington DC, Washington DC2, Montreal, Quebec City, Toronto, Vancouver |
| **Asya** | Doğu Asya, Güneydoğu Asya | Bangkok, Hong Kong, Hong Kong2, Jakarta, Kuala Lumpur, Singapur, Singapur2, Taipei |
| **Hindistan** | Hindistan Batı, Hindistan Orta, Hindistan Güney |Madras, Madras2, Bombay, Bombay2 |
| **Japonya** | Batı Japonya, Doğu Japonya |Osaka, Tokyo, Tokyo2 |
| **Okyanusya** | Güneydoğu Avustralya, Doğu Avustralya |Auckland, Melbourne, Perth, Sidney, Sidney2 | 
| **Güney Kore** | Güney Kore - Orta, Güney Kore - Güney |Busan, Seul|
| **BAE** | BAE Merkez, BAE Kuzey | Dubai, Dubai2 |
| **Güney Afrika** | Güney Afrika Batı, Güney Afrika Kuzey |Cape Town, Johannesburg |
| **Güney Amerika** | Güney Brezilya |Sao Paulo |

## <a name="azure-regions-and-geopolitical-boundaries-for-national-clouds"></a>Ulusal bulutlar için azure bölgeler ve jeopolitik sınırlar
Aşağıdaki tablo ulusal bulutlar için bölgeler ve coğrafi sınırlar hakkında bilgi sağlar.

| **Jeopolitik bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- |
| **ABD bulutu** |US Gov Arizona, US Gov Iowa, US Gov Teksas, US Gov Virginia, Orta US DoD, Doğu US DoD  |Atlanta, Chicago, Dallas, New York, Phoenix, San Antonio, Seattle, Silikon Vadisi, Washington DC |
| **Doğu Çin** |Doğu Çin, Doğu Çin2 |Şangay, Şangay2 |
| **Kuzey Çin** |Kuzey Çin, Kuzey Çin2 |Pekin, Pekin2 |
| **Almanya** |Orta Almanya, Almanya Doğu |Berlin, Frankfurt |

Coğrafi bölgeler arasındaki bağlantı standart ExpressRoute SKU’da desteklenmiyor. Genel bağlantıyı desteklemek için ExpressRoute premium eklentisini etkinleştirmeniz gerekir. Ulusal bulut ortamlarına bağlantı desteklenmiyor. Bu tür bir ihtiyaç ortaya çıkarsa bağlantı sağlayıcınız ile çalışabilirsiniz.

## <a name="expressroute-connectivity-providers"></a><a name="partners"></a>ExpressRoute bağlantı sağlayıcıları

Aşağıdaki tabloda bağlantı konumları ve her konum için hizmet sağlayıcıları gösterilmektedir. Hizmet sağlayıcılarını ve hizmet sunabildikleri konumları görüntülemek istiyorsanız bkz. [Hizmet sağlayıcısına göre konumlar](expressroute-locations.md).

* **Yerel Azure Bölgeleri,** her bir bakan konumda [ExpressRoute Local'ın](expressroute-faqs.md) erişebileceği bölgelerdir. **n/a,** ExpressRoute Local'ın o bakan konumda kullanılmadığını gösterir.

* **Bölge** [fiyatlandırma](https://azure.microsoft.com/pricing/details/expressroute/)anlamına gelir.


### <a name="global-commercial-azure"></a>Küresel ticari Azure
| **Konum** | **Adres** | **Bölge** | **Yerel Azure bölgeleri** | **ER Doğrudan** | **Servis sağlayıcılar** |
| --- | --- | --- | --- | --- | --- |
| **Amsterdam** | [Equinix AM5](https://www.equinix.com/locations/europe-colocation/netherlands-colocation/amsterdam-data-centers/am5/) | 1 | Batı Avrupa | 10G, 100g | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Interxion, KPN, IX Reach, Seviye 3 İletişim, Megaport, NTT İletişim, Orange, Tata Communications, Telefonica, Telenor, Telia Carrier, Verizon, Zayo |
| **Amsterdam2** | [Interxion AMS8](https://www.interxion.com/Locations/amsterdam/schiphol/) | 1 | Batı Avrupa | 10G, 100g | CenturyLink Cloud Connect, Colt, DE-CIX, euNetworks, Interxion, Orange, Vodafone |
| **Atlanta** | [Equinix AT2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/atlanta-data-centers/at2/) | 1 | yok | yok | Equinix, Megaport |
| **Auckland** | [Vocus Grup NZ Albany](https://www.vocus.co.nz/business/cloud-data-centres) | 2 | yok | 10G | Devoli, Kordia, Megaport, Kıvılcım NZ, Vocus Grup NZ |
| **Bangkok** | [Aıs](https://business.ais.co.th/solution/en/azure-expressroute.html) | 2 | yok | 10G | Aıs |
| **Busan** | [LG CNS](https://www.lgcns.com/datacenter) | 2 | Güney Kore - Güney | yok | LG CNS |
| **Kanberra** | [CDC](https://cdcdatacentres.com.au/content/about-cdc) | 1 | Orta Avustralya | 10G, 100g | CDC |
| **Kanberra2** | [CDC](https://cdcdatacentres.com.au/content/about-cdc) | 1 | Orta Avustralya 2| 10G, 100g | CDC |
| **Cape Town** | [Teraco CT1](https://www.teraco.co.za/data-centre-locations/cape-town/) | 3 | Güney Afrika Batı | 10G | BCX, İnternet Çözümleri - Cloud Connect, Liquid Telecom, Teraco |
| **Chennai** | Tata Communications | 2 | Güney Hindistan | 10G | Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chennai2** | Airtel | 2 | Güney Hindistan | yok | Airtel |
| **Chicago** | [Equinix CH1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/chicago-data-centers/ch1/) | 1 | Orta Kuzey ABD | 10G, 100g | Aryaka Networks, AT&T NetBond, CenturyLink Cloud Connect, Cologix, Colt, Comcast, Coresite, Equinix, InterCloud, Internet2, Seviye 3 İletişim, Megaport, PacketFabric, PCCW Global Limited, Sprint, Telia Carrier, Verizon, Zayo |
| **Kopenhag** | [Interxion CPH1](https://www.interxion.com/Locations/copenhagen/) | 1 | yok | 10G | Interxion |
| **Dallas** | [Equinix DA3](https://www.equinix.com/locations/americas-colocation/united-states-colocation/dallas-data-centers/da3/) | 1 | yok | 10G, 100g | Aryaka Ağları, AT&T NetBond, Cologix, Equinix, Internet2, Seviye 3 İletişim, Megaport, NötronA Ağları, Telmex Uninet, Telia Taşıyıcı, Transtelco, Verizon, Zayo|
| **Denver** | [CoreSite DE1](https://www.coresite.com/data-centers/locations/denver/de1) | 1 | Orta Batı ABD | yok | CoreSite, Megaport, Zayo |
| **Dubai** | [PCCS](https://www.pacificcontrols.net/cloudservices/index.html) | 3 | BAE Kuzey | yok | Etisalat BAE |
| **Dubai2** | [du datamena](http://datamena.com/solutions/data-centre) | 3 | BAE Kuzey | yok | du datamena, Megaport, Turuncu, Orixcom |
| **Dublin** | [Equinix DB3](https://www.equinix.com/locations/europe-colocation/ireland-colocation/dublin-data-centers/db3/) | 1 | Kuzey Avrupa | 10G, 100g | Colt, eir, Equinix, euNetworks, Interxion, Megaport |
| **Frankfurt** | [Interxion FRA11](https://www.interxion.com/Locations/frankfurt/) | 1 | Almanya Batı Orta | 10G, 100g | CenturyLink Cloud Connect, Colt, DE-CIX, Equinix, GEANT, Interxion, Megaport, Orange, Telia Carrier |
| **Cenevre** | [Equinix GV2](https://www.equinix.com/locations/europe-colocation/switzerland-colocation/geneva-data-centers/gv2/) | 1 | İsviçre Batı | 10G, 100g | Equinix, Megaport |
| **Hong Kong** | [Equinix HK1](https://www.equinix.com/locations/asia-colocation/hong-kong-colocation/hong-kong-data-center/hk1/) | 2 | Doğu Asya | yok | Aryaka Networks, British Telecom, CenturyLink Cloud Connect, Chief Telecom, China Telecom Global, Equinix, InterCloud, Megaport, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telia Carrier, Verizon |
| **Hong Kong2** | [MEGA-i](https://www.iadvantage.net/index.php/locations/mega-i) | 2 | yok | 10G | |
| **Jakarta** | Telkom Endonezya | 4 | yok | 10G | |
| **Johannesburg** | [Teraco JB1](https://www.teraco.co.za/data-centre-locations/johannesburg/#jb1) | 3 | Güney Afrika Kuzey | 10G | British Telecom, Internet Solutions - Cloud Connect, Liquid Telecom, Orange, Teraco |
| **Kuala Lumpur** | [TIME dotCom Menara AMBE](https://www.aims.com.my/co-location/points-of-presence.html) | 2 | yok | yok | TIME dotCom |
| **Las Vegas** | [LV'yi değiştir](https://www.switch.com/las-vegas) | 1 | yok | yok | CenturyLink Cloud Connect, Megaport |
| **Londra** | [Equinix LD5](https://www.equinix.com/locations/europe-colocation/united-kingdom-colocation/london-data-centers/ld5/) | 1 | Güney Birleşik Krallık | 10G, 100g | AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telehouse - KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayoa |
| **Londra2** | [Telehouse Kuzey İki](https://www.telehouse.net/data-centres/emea/uk-data-centres/london-data-centres/north-two) | 1 | Güney Birleşik Krallık | 10G, 100g | CenturyLink Cloud Connect, Colt, IX Reach, Equinix, Megaport, Telehouse - KDDI |
| **Los Angeles** | [CoreSite LA1](https://www.coresite.com/data-centers/locations/los-angeles/one-wilshire) | 1 | yok | yok | CoreSite, Equinix, Megaport, Nötron Ağları, NTT, Transtelco, Zayo |
| **Marsilya** |[Interxion MRS1](https://www.interxion.com/Locations/marseille/) | 1 | Güney Fransa | yok | DE-CIX, GEANT, Interxion, Jaguar Network, Ooredoo Bulut Bağlantı |
| **Melbourne** | [NextDC M1](https://www.nextdc.com/data-centres/m1-melbourne-data-centre) | 2 | Güneydoğu Avustralya | 10G, 100g | AARNet, Devoli, Equinix, Megaport, NEXTDC, Optus, Telstra Corporation, TPG Telekom |
| **Miami** | [Equinix MI1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/miami-data-centers/mi1/) | 1 | yok | 10G | C3ntro, Equinix, Megaport, Nötron Ağları |
| **Milan** | [IRIDEOS](https://irideos.it/en/data-centers/) | 1 | yok | 10G | |
| **Montreal** | [Cologix MTL3](https://www.cologix.com/data-centers/montreal/mtl3/) | 1 | yok | 10G, 100g | Çan Kanada, Cologix, Megaport, Telus, Zayo |
| **Mumbai** | Tata Communications | 2 | Batı Hindistan | yok | Global CloudXchange (GCX), Reliance Jio, Sify, Tata İletişim, Verizon |
| **Bombay2** | Airtel | 2 | Batı Hindistan | yok | Airtel, Sify, Vodafone Idea |
| **Münih** | [KenarConneX](https://www.edgeconnex.com/locations/europe/) | 1 | yok | 10G, 100g | |
| **New York** | [Equinix NY9](https://www.equinix.com/locations/americas-colocation/united-states-colocation/new-york-data-centers/ny9/) | 1 | yok | yok | CenturyLink Cloud Connect, Colt, Coresite, Equinix, InterCloud, Megaport, Paket, Zayo |
| **Newport(Galler)** | [Next Generation Data](https://www.nextgenerationdata.co.uk) | 1 | Batı Birleşik Krallık | yok | British Telecom, Colt, Seviye 3 İletişim, Yeni Nesil Veriler |
| **Osaka** | [Equinix OS1](https://www.equinix.com/locations/asia-colocation/japan-colocation/osaka-data-centers/os1/) | 2 | Batı Japonya | 10G, 100g | Colt, Equinix, Internet Initiative Japan Inc. - IIJ, Megaport, NTT İletişim, NTT SmartConnect, Softbank |
| **Oslo** | [DigiPlex Ulven](https://www.digiplex.com/locations/oslo-datacentre) | 1 | Norveç Doğu | 10G, 100g | Megaport, Telenor, Telia Taşıyıcı |
| **Paris** | [Interxion PAR5](https://www.interxion.com/Locations/paris/) | 1 | Orta Fransa | 10G, 100g | CenturyLink Cloud Connect, Colt, Equinix, Intercloud, Interxion, Orange, Telia Carrier, Zayo |
| **Perth** | [NextDC P1](https://www.nextdc.com/data-centres/p1-perth-data-centre) | 2 | yok | 10G | Megaport, NextDC |
| **Quebec City** | [Vantage](https://vantage-dc.com/data_centers/quebec-city-data-center-campus/) | 1 | Doğu Kanada | yok | Bell Canada, Megaport |
| **San Antonio** | [CyrusOne SA1](https://cyrusone.com/locations/texas/san-antonio-texas/) | 1 | Orta Güney ABD | 10G, 100g | CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** | [Equinix SP2](https://www.equinix.com/locations/americas-colocation/brazil-colocation/sao-paulo-data-centers/sp2/) | 3 | Güney Brezilya | yok | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica, UOLDIVEO |
| **Seattle** | [Equinix SE2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/seattle-data-centers/se2/) | 1 | Batı ABD 2 | 10G, 100g | Aryaka Ağları, Equinix, Seviye 3 İletişim, Megaport, Telus, Zayo |
| **Seul** | [KINX Gasan IDC](https://www.kinx.net/support/location/?lang=en) | 2 | Güney Kore - Orta | 10G, 100g | KINX, KT, LG CNS, Sejong Telekom |
| **Silikon Vadisi** | [Equinix SV1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/silicon-valley-data-centers/sv1/) | 1 | Batı ABD | 10G, 100g | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, Comcast, Coresite, Equinix, InterCloud, Internet2, IX Reach, Packet, PacketFabric, Level 3 İletişim, Megaport, Orange, Sprint, Tata İletişim, Telia Carrier, Verizon, Zayo |
| **Silikon Vadisi2** | [Coresite SV7](https://www.coresite.com/data-centers/locations/silicon-valley/sv7) | 1 | Batı ABD | 10G, 100g | Colt, Coresite | 
| **Singapur** | [Equinix SG1](https://www.equinix.com/locations/asia-colocation/singapore-colocation/singapore-data-center/sg1/) | 2 | Güneydoğu Asya | 10G, 100g | Aryaka Networks, AT&T NetBond, British Telecom, China Mobile International, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon, Vodafone |
| **Singapur2** | [Global Switch Tai Seng](https://www.globalswitch.com/locations/singapore-data-centres/) | 2 | Güneydoğu Asya | 10G, 100g | Çin Unicom Global, Colt, Epsilon Global İletişim, Megaport, SingTel |
| **Stavanger** | [Yeşil Dağ DC1](https://greenmountain.no/dc1-stavanger/) | 1 | Norveç Batı | 10G, 100g | |
| **Stockholm** | [Equinix SK1](https://www.equinix.com/locations/europe-colocation/sweden-colocation/stockholm-data-centers/sk1/) | 1 | yok | 10G | Equinix, Telia Taşıyıcı |
| **Sidney** | [Equinix SY2](https://www.equinix.com/locations/asia-colocation/australia-colocation/sydney-data-centers/sy2/) | 2 | Doğu Avustralya | 10G, 100g | AARNet, AT&T NetBond, British Telecom, Devoli, Equinix, Kordia, Megaport, NEXTDC, NTT İletişim, Optus, Orange, Spark NZ, Telstra Corporation, TPG Telekom, Verizon, Vocus Group NZ |
| **Sidney2** | [NextDC S1](https://www.nextdc.com/data-centres/s1-sydney-data-centre) | 2 | Doğu Avustralya | 10G, 100g | Megaport, NextDC |
| **Taipei** | Baş Telekom | 2 | yok | 10G | Şef Telekom, FarEasTone |
| **Tokyo** | [Equinix TY4](https://www.equinix.com/locations/asia-colocation/japan-colocation/tokyo-data-centers/ty4/) | 2 | Doğu Japonya | 10G, 100g | Aryaka Networks, AT&T NetBond, BBIX, British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, Megaport, NTT Communications, NTT EAST, Orange, Softbank, Verizon |
| **Tokyo2** | [TOKYO'DA](https://www.attokyo.com/) | 2 | Doğu Japonya | 10G, 100g | TOKYO'DA |
| **Toronto** | [Cologix TOR1](https://www.cologix.com/data-centers/toronto/tor1/) | 1 | Orta Kanada | 10G, 100g | AT&T NetBond, Bell Kanada, CenturyLink Cloud Connect, Cologix, Equinix, IX Reach Megaport, Telus, Verizon, Zayo |
| **Vancouver** | [Cologix VAN1](https://www.cologix.com/data-centers/vancouver/van1/) | 1 | yok | 10G, 100g | |
| **Washington DC** | [Equinix DC2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/washington-dc-data-centers/dc2/) | 1 | Doğu ABD, Doğu ABD 2 | 10G, 100g | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Cologix, Colt, Comcast, Coresite, Equinix, Internet2, InterCloud, IX Reach, Level 3 Communications, Megaport, Neutrona Networks, NTT Communications, Orange, PacketFabric, SES, Sprint, Tata Communications, Telia Carrier, Verizon, Zayo |
| **Washington DC2** | [Coresite Reston](https://www.coresite.com/data-centers/locations/northern-virginia-washington-dc/reston-campus) | 1 | Doğu ABD, Doğu ABD 2 | 10G, 100g | CenturyLink Bulut Bağlantısı, Coresite, Intelsat, Viasat, Zayo | 
| **Zürih** | [Interxion ZUR2](https://www.interxion.com/Locations/zurich/) | 1 | İsviçre Kuzey | 10G, 100g | Intercloud, Interxion, Megaport, Swisscom |

 **+** yakında gösterir

### <a name="national-cloud-environments"></a>Ulusal bulut ortamları

Azure ulusal bulutları birbirinden ve genel ticari Azure'dan izole edilmiştir. Bir Azure bulutu için ExpressRoute diğerlerinde Azure bölgelerine bağlanabilir.

### <a name="us-government-cloud"></a>ABD bulutu
| **Konum** | **Adres** | **Yerel Azure bölgeleri**| **ER Doğrudan** | **Servis sağlayıcılar** |
| --- | --- | --- | --- | --- |
| **Atlanta** | [Equinix AT1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/atlanta-data-centers/at1/) | yok | 10G, 100g | Equinix |
| **Chicago** | [Equinix CH1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/chicago-data-centers/ch1/) | yok | 10G, 100g | AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** | [Equinix DA3](https://www.equinix.com/locations/americas-colocation/united-states-colocation/dallas-data-centers/da3/) | yok | 10G, 100g | Equinix, Megaport, Verizon |
| **New York** | [Equinix NY5](https://www.equinix.com/locations/americas-colocation/united-states-colocation/new-york-data-centers/ny5/) | yok | 10G, 100g | Equinix, CenturyLink Cloud Connect, Verizon |
| **Phoenix** | [CyrusOne Chandler](https://cyrusone.com/locations/arizona/phoenix-arizona-chandler/) | US Gov Arizona | yok | AT&T NetBond, CenturyLink Bulut Bağlantı, Megaport |
| **San Antonio** | [CyrusOne SA2](https://cyrusone.com/locations/texas/san-antonio-texas-ii/) | US Gov Texas | yok | CenturyLink Cloud Connect, Megaport |
| **Silikon Vadisi** | [Equinix SV4](https://www.equinix.com/locations/americas-colocation/united-states-colocation/silicon-valley-data-centers/sv4/) | yok | 10G, 100g | AT&T, Equinix, Seviye 3 İletişim, Verizon |
| **Seattle** | [Equinix SE2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/seattle-data-centers/se2/) | yok | yok | Equinix, Megaport |
| **Washington DC** | [Equinix DC2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/washington-dc-data-centers/dc2/) | ABD DoD Doğu, ABD Gov Virginia | 10G, 100g | AT&T NetBond, CenturyLink Cloud Connect, Equinix, Seviye 3 İletişim, Megaport, Verizon |

### <a name="china"></a>Çin
| **Konum** | **Servis sağlayıcılar** |
| --- | --- |
| **Pekin** |China Telecom |
| **Pekin2** | Çin Telekom, Çin Unicom, GDS |
| **Shanghai** |China Telecom |
| **Şangay2** | Çin Telekom, GDS |

Daha fazla bilgi için [Çin'deki ExpressRoute'a](http://www.windowsazure.cn/home/features/expressroute/) bakın

### <a name="germany"></a>Almanya
| **Konum** | **Servis sağlayıcılar** |
| --- | --- |
| **Berlin** |e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="connectivity-through-exchange-providers"></a><a name="c1partners"></a>Exchange sağlayıcıları aracılığıyla bağlantı
Bağlantı sağlayıcınız önceki bölümlerde listelenmemişse hala bağlantı oluşturabilirsiniz.

* Yukarıdaki tabloda yer alan değişimlerin herhangi birine bağlı olup olmadığını görmek için bağlantı sağlayıcınıza başvurun. Değişim sağlayıcıları tarafından sunulan hizmetler hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kontrol edebilirsiniz. Birkaç bağlantı sağlayıcı Ethernet değişimlerine zaten bağlı.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [Equinix Cloud Exchange](https://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](https://www.interxion.com/)
  * [NextDC](https://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [PacketFabric](https://www.packetfabric.com/cloud-connectivity/microsoft-azure)
  * [Teraco](https://www.teraco.co.za/platform-teraco/africa-cloud-exchange/)
  
* Bağlantı sağlayıcınızı, ağınızı seçtiğiniz eşleme konumuna genişletmesini sağlayın.
  * Bağlantı sağlayıcınızın bağlantınızı yüksek oranda kullanılabilir şekilde genişlettiğinden emin olun, böylece hiç tek nokta arızası olmaz.
* Microsoft’a bağlanmak için bağlantı sağlayınız olarak değişime sahip bir ExpressRoute bağlantı hattı sipariş edin.
  * Bağlantı kurmak için [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)’daki adımları izleyin.

## <a name="connectivity-through-satellite-operators"></a>Uydu operatörleri aracılığıyla bağlantı
Uzaktan uzaktaysanız ve fiber bağlantınız yoksa veya diğer bağlantı seçeneklerini keşfetmek istiyorsanız aşağıdaki uydu operatörlerini kontrol edebilirsiniz. 

* ıntelsat
* [SES](https://www.ses.com/networks/signature-solutions/signature-cloud/ses-and-azure-expressroute)
* [Viasat](http://www.directcloud.viasatbusiness.com/)

## <a name="connectivity-through-additional-service-providers"></a><a name="c1partners"></a>Ek hizmet sağlayıcılar aracılığıyla bağlantı
| **Konum** | **cihazlar** | **Bağlantı sağlayıcıları** |
| --- | --- | --- |
| **Amsterdam** | Equinix, Interxion, Seviye 3 İletişim | BICS, CloudXpress, Eurofiber, Fastweb S.p.A, Gulf Bridge International, Kalaam Telecom Bahreyn B.S.C, MainOne, Nianet, POST Telecom Luxembourg, Proximus, RETN, TDC Erhverv, Telecom Italia Sparkle, Telekom Deutschland GmbH, Telia |
| **Atlanta** | Equinix| Taç Kalesi
| **Cape Town** | Teraco | MTN |
| **Chicago** | Equinix| Taç Kalesi, Spectrum Enterprise, Windstream |
| **Dallas** | Equinix, Megaport | Axtel, C3ntro Telecom, Cox Business, Crown Castle, Data Foundry, Spectrum Enterprise, Transtelco |
| **Frankfurt** | Interxion | BICS, Cinia, Equinix, Nianet, QSC AG, Telekom Deutschland GmbH |
| **Hamburg** | Equinix | Cinia |
| **Hong Kong ÖİB** | Equinix | Şef, Macroview Telekom |
| **Johannesburg** | Teraco | MTN |
| **Londra** | BICS, Equinix, euNetworks| Bezeq International Ltd., CoreAzure, Epsilon Telekomünikasyon Limited, Üstel E, HSO, NexGen Networks, Proximus, Tamares Telecom, Zain |
| **Los Angeles** | Equinix |Taç Kalesi, Spectrum Enterprise, Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix| Airgate Technologies, Inc. Aptum Technologies, Rogers, Zirro |
| **New York** |Equinix, Megaport | Altice İş, Crown Castle, Spectrum Enterprise, Webair |
| **Paris** | Equinix | Proximus |
| **Quebec City** | Megaport | Fibrenoire |
| **Sao Paula** | Equinix | Venha Pra Nuvem |
| **Seattle** |Equinix | Alaska Communications |
| **Silikon Vadisi** |Coresite, Equinix | Cox İş, Spectrum Enterprise, Windstream, X2nsat Inc |
| **Singapur** |Equinix |1CLOUDSTAR, BICS, CMC Telecom, Epsilon Telekomünikasyon Limited, LGA Telekom, Birleşik Bilgi Otoyolu (UIH) |
| **Slough** | Equinix | HSO|
| **Sidney** | Megaport | Macquarie Telecom Group|
| **Tokyo** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix, Megaport | Airgate Technologies Inc., Beanfield Metroconnect, Aptum Technologies, IVedha Inc, Rogers, Thinktel, Zirro|
| **Washington DC** |Equinix | Altice İş, BICS, Cox Business, Crown Castle, Gtt İletişim A.Ş., Epsilon Telekomünikasyon Limited, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>ExpressRoute sistem tümleştiricileri
İhtiyaçlarınıza uyan özel bağlantıyı etkinleştirme ağınızın ölçeğine bağlı olarak zorlu olabilir. ExpressRoute’a yönelik ekleme işleminde size yardımcı olmak üzere aşağıdaki tabloda listelenen herhangi bir sistem tümleştirici ile çalışabilirsiniz.

| **Kıta** | **Sistem tümleştiriciler** |
| --- | --- |
| **Asya** |Avanade Inc., OneAs1a |
| **Avustralya** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Avrupa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **Kuzey Amerika** |Avanade Inc., Equinix Professional Services, FlexManage, Lightstream, Perficient, Presidio |
| **Güney Amerika** |Avanade Inc., Venha Pra Nuvem |
## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
* Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Konum eşleme"
