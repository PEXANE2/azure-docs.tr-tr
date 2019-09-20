---
title: 'Konumlar ve bağlantı sağlayıcıları: Azure ExpressRoute | Microsoft Docs'
description: Bu makale, sunulan hizmetlerin konumları ve Azure bölgelerine nasıl bağlanılacağı hakkında ayrıntılı bir genel bakış sağlar. Konuma göre sıralanmıştır.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2019
ms.author: cherylmc
ms.openlocfilehash: 3867e4c59f541fb50c536850e89e03d49a831e42
ms.sourcegitcommit: a7a9d7f366adab2cfca13c8d9cbcf5b40d57e63a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71162077"
---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute ortakları ve eşleme konumları

> [!div class="op_single_selector"]
> * [Sağlayıcıya Göre Konumlar](expressroute-locations.md)
> * [Konuma Göre Sağlayıcılar](expressroute-locations-providers.md)


Bu makaledeki tablolarda, ExpressRoute coğrafi kapsamı ve konumları, ExpressRoute bağlantı sağlayıcıları ve ExpressRoute sistem tümleştiricileri (SIS) hakkında bilgi sağlanmaktadır.

> [!Note]
> Azure bölgeleri ve ExpressRoute konumları iki ayrı ve farklı kavramlardır ve bu iki arasındaki farkı anlamak, Azure hibrit ağ bağlantısını keşfetmek için kritik öneme sahiptir. 
>
>

## <a name="azure-regions"></a>Azure bölgeleri
Azure bölgeleri, Azure işlem, ağ ve depolama kaynaklarının bulunduğu küresel veri merkezlerindedir. Bir Azure kaynağı oluştururken müşterinin bir kaynak konumu seçmesini gerekir. Kaynak konumu, kaynağın oluşturulduğu Azure veri merkezini (veya kullanılabilirlik alanını) belirler.

## <a name="expressroute-locations"></a>ExpressRoute konumları
ExpressRoute konumları (bazen eşleme konumları olarak da adlandırılır veya karşılıyoruz), Microsoft Enterprise Edge (MSEE) cihazlarının bulunduğu ortak konum tesislerindedir. ExpressRoute konumları, Microsoft 'un ağı olan giriş noktasıdır ve küresel olarak dağıtılır ve müşterilere dünyanın dört bir yanındaki ağa bağlanma olanağı sağlar. Bu konumlar, ExpressRoute iş ortaklarının ve ExpressRoute Direct müşterilerinin Microsoft 'un ağına çapraz bağlantı vermesine neden olduğu yerdir. Genel olarak, ExpressRoute konumunun Azure bölgesiyle eşleşmesi gerekmez. Örneğin, bir müşteri, *Seattle* eşleme konumunda *Doğu ABD*kaynak konumuyla bir ExpressRoute devresi oluşturabilir.

Coğrafi bölge içindeki en az bir ExpressRoute konumuna bağlanırsanız coğrafi bölge içindeki tüm bölgeler arasında Azure hizmetlerine erişebileceksiniz. 

## <a name="locations"></a>Coğrafi bölge içindeki ExpressRoute konumlarına Azure bölgeleri
Aşağıdaki tablo, coğrafi bölge içindeki Azure bölgeler ile ExpressRoute konumları arasında yapılan eşlemeyi sağlar.

| **Jeopolitik bölge** | **Bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- | --- |
| **Australia Government** | 1\. | Avustralya Orta, Avustralya Orta 2 |Kanberra, Kanberra2 |
| **Avrupa** | 1\. |Fransa Orta, Fransa Güney, Kuzey Avrupa, Batı Avrupa, UK Batı, UK Güney |Amsterdam, Amsterdam2, Copenhagen, Dublin, Frankfurt, Londra, London2, Marseille, Newport (Wales), Paris, Stockholm, Zurich, Münih |
| **Kuzey Amerika** | 1\. |Doğu ABD, Batı ABD, Doğu ABD 2, Batı ABD 2, Orta ABD, Orta Güney ABD, Orta Kuzey ABD, Batı Orta ABD, Orta Kanada, Doğu Kanada |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miamı, New York, San Antonio, Seattle, Silicon Valley, Silicon Valley2, Washington DC, Washington DC2, Montreal, Quebec City, toron |
| **Asya** | 2 |Doğu Asya, Güneydoğu Asya |Hong Kong ÖIB, Kuala Lumpur, Singapur, Singapur2, Taipei |
| **Hindistan** | 2 |Batı Hindistan, Orta Hindistan, Güney Hindistan |Madras, Madras2, Bombay, Bombay2 |
| **Japonya** | 2 |Batı Japonya, Doğu Japonya |Osaka, Tokyo |
| **Okyanusya** | 2 |Güneydoğu Avustralya, Doğu Avustralya |Auckland, Melbourne, Perth, Sidney | 
| **Güney Kore** | 2 |Kore Orta, Kore Güney |Busan, Seul|
| **BAE** | 3 | BAE Orta, BAE Kuzey | Dubai, Dubai2 |
| **Güney Afrika** | 3 |Güney Afrika Batı, Güney Afrika Kuzey |Cape Town, Johannesburg |
| **Güney Amerika** | 3 |Güney Brezilya |Sao Paulo |

## <a name="azure-regions-and-geopolitical-boundaries-for-national-clouds"></a>Ulusal bulutlar için Azure bölgeleri ve geopolitik sınırlar
Aşağıdaki tablo ulusal bulutlar için bölgeler ve coğrafi sınırlar hakkında bilgi sağlar.

| **Jeopolitik bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- |
| **US Government bulutu** |US Gov Arizona, US Gov Iowa, US Gov Teksas, US Gov Virginia, US DoD Orta, US DoD Doğu  |Chicago, Dallas, New York, Phoenix, San Antonio, Seattle, Silikon Vadisi, Washington DC |
| **Çin Doğu** |Çin Doğu, Çin Doğu2 |Shanghai, Shanghai2 |
| **Çin Kuzey** |Çin Kuzey, Çin Kuzey2 |Pekin, Beijing2 |
| **Almanya** |Orta Almanya, Doğu Almanya |Berlin, Frankfurt |

Coğrafi bölgeler arasındaki bağlantı standart ExpressRoute SKU’da desteklenmiyor. Genel bağlantıyı desteklemek için ExpressRoute premium eklentisini etkinleştirmeniz gerekir. Ulusal bulut ortamlarına bağlantı desteklenmiyor. Bu tür bir ihtiyaç ortaya çıkarsa bağlantı sağlayıcınız ile çalışabilirsiniz.

## <a name="partners"></a>ExpressRoute bağlantı sağlayıcıları

Aşağıdaki tabloda bağlantı konumları ve her konum için hizmet sağlayıcıları gösterilmektedir. Hizmet sağlayıcılarını ve hizmet sunabildikleri konumları görüntülemek istiyorsanız bkz. [Hizmet sağlayıcısına göre konumlar](expressroute-locations.md). 

**Yerel Azure bölgeleri** , her bir eşleme konumundaki [ExpressRoute yerellerinin](expressroute-faqs.md) erişebileceği alanlardır. **yok** , ExpressRoute yerel 'in o eşleme konumunda kullanılabilir olmadığını gösterir.


### <a name="production-azure"></a>Üretim Azure
| **Location** | **Sahip ve adres** | **Yerel Azure bölgeleri** | **Hizmet sağlayıcıları** |
| --- | --- | --- | --- |
| **Amsterdam** | [Equinix AM5](https://www.equinix.com/locations/europe-colocation/netherlands-colocation/amsterdam-data-centers/am5/) | Batı Avrupa | Aryaka Networks, AT & T Netbonı, Ingiliz telekomünikasyon, Colt, Equinx, Eunetçalışmalar, GÉANT, Intercloud, ınterxiyon, KPN, x REACH, Level 3 Communications, Megaport, NTT Communications, turuncu, Tata Communications, Telefonica, Telenor, Telia taşıyıcısı, Verizon, Zayo |
| **Amsterdam2** | [Interxiyon AMS8](https://www.interxion.com/Locations/amsterdam/schiphol/) | Batı Avrupa | CenturyLink Cloud Connect, DE-CIX, Colt, ınterxiyon, Vodavfone |
| **Atlanta** | [Equinix AT2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/atlanta-data-centers/at2/) | yok | Equinix, Megaport |
| **Auckland** | [Vocus grubu NZ Albümno](https://www.vocus.co.nz/business/cloud-data-centres) | yok | Devoli, Kordia, Megaport, Spark NZ, Vocus Group NZ |
| **Busan** | [LG CNS](https://datacenter.lgcns.com/Contents/En/Menu_1/Locations_1.aspx) | Kore Güney | LG CNS |
| **Kanberra** | [CDC](https://cdcdatacentres.com.au/content/about-cdc) | Avustralya Orta | CDC |
| **Kanberra2** | [CDC](https://cdcdatacentres.com.au/content/about-cdc) | Avustralya Orta 2| CDC |
| **Cape Town** | [Teraco CT1](https://www.teraco.co.za/data-centre-locations/cape-town/) | Güney Afrika Batı | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Chennai** | Tata Communications | Güney Hindistan | Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chennai2** | Airtel | Güney Hindistan | Airtel |
| **Chicago** | [Equinix CH1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/chicago-data-centers/ch1/) | Orta Kuzey ABD | Aryaka Networks, & T Netbono, CenturyLink Cloud Connect, Geregix, Comcast, Coresıte, Equinix, Intercloud, Internet2, Level 3 Communications, Megaport, PacketFabric, PCFA Global Limited, Sprint, Telia taşıyıcısı, Verizon, Zayo |
| **Kopenhag** | [Interxiyon CPH1](https://www.interxion.com/Locations/copenhagen/) | yok | Interxion |
| **Dallas** | [Equinix DA3](https://www.equinix.com/locations/americas-colocation/united-states-colocation/dallas-data-centers/da3/) | yok | Aryaka Networks, & T Netbonıngıgix, Equinix, Internet2, düzey 3 Communications, Megaport, nörona Networks, Telmex Unınet, Telia taşıyıcısı, Transtelco, Verizon, Zayo|
| **Denver** | [CoreSite DE1](https://www.coresite.com/data-centers/locations/denver/de1) | Batı Orta ABD | CoreSite, Megaport, Zayo |
| **Dubai** | [PCCS](https://www.pacificcontrols.net/cloudservices/index.html) | BAE Kuzey | Etisalat UAE |
| **Dubai2** | [du datamena](http://datamena.com/solutions/data-centre) | BAE Kuzey | du datamena, Cumxcom |
| **Dublin** | [Equinix DB3](https://www.equinix.com/locations/europe-colocation/ireland-colocation/dublin-data-centers/db3/) | Kuzey Avrupa | Colt, eir, Equinix, Interxion, Megaport |
| **Frankfurt** | [Interxiyon FRA11](https://www.interxion.com/Locations/frankfurt/) | yok | DE-CIX, ınterxiyon, turuncu |
| **Hong Kong ÖIB** | [Equinix HK1](https://www.equinix.com/locations/asia-colocation/hong-kong-colocation/hong-kong-data-center/hk1/) | Doğu Asya | Aryaka Networks, Ingiliz Teletası, CenturyLink Cloud Connect, baş Telem, Çin telekomünikasyon küresel, Equinx, Megaport, NTT Communications, turuncu, PCCW küresel Limited, Tata Iletişimleri, Telia taşıyıcısı, Verizon |
| **Johannesburg** | [Teraco JB1](https://www.teraco.co.za/data-centre-locations/johannesburg/#jb1) | Güney Afrika Kuzey | İngiliz Telekoı, Internet çözümleri-bulut bağlantısı, sıvı telekomünikasyon, turuncu, Teraco |
| **Kuala Lumpur** | [ZAMAN dotCom Menara AIMS](https://www.aims.com.my/co-location/points-of-presence.html) | yok | TIME dotCom |
| **Las Vegas** | [Anahtar LV](https://www.switch.com/las-vegas) | yok | CenturyLink Cloud Connect, Megaport |
| **Londra** | [Equinix LD5](https://www.equinix.com/locations/europe-colocation/united-kingdom-colocation/london-data-centers/ld5/) | Birleşik Krallık Güney | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telehouse - KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **London2** | [Telehouse Kuzey Iki](https://www.telehouse.com/global-data-centers/emea/london-data-centers/telehouse-north-two/) | Birleşik Krallık Güney | X ile ulaşma, Equinix |
| **Los Angeles** | [CoreSite LA1](https://www.coresite.com/data-centers/locations/los-angeles/one-wilshire) | yok | CoreSite, Equinx, Megaport, Neutrona Networks, NTT, Zayo |
| **Marsilya** |[Interxiyon MRS1](https://www.interxion.com/Locations/marseille/) | Fransa Güney | DE-CIX, ınterxiyon, Jaguar Network |
| **Melbourne** | [NextDC M1](https://www.nextdc.com/data-centres/m1-melbourne-data-centre) | Avustralya Güneydoğu | AARNet, Devoli, Equinix, Megaport, NEXTDC, Ops, Telstra Corporation, TPG telekomünikasyon |
| **Miami** | [Equinix MI1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/miami-data-centers/mi1/) | yok | C3ntro+, Equinix, Megaport, Neutrona Networks |
| **Montreal** | [Birlikte bulundurgix MTL3](https://www.cologix.com/data-centers/montreal/mtl3/) | yok | Bell Kanada, Kolombiya Gix, Megaport, Telus, Zayo |
| **Mumbai** | Tata Communications | Batı Hindistan | Küresel CloudXchange (GCX), Reliance jıo, Sıfy, Tata Iletişimleri, Verizon |
| **Mumbai2** | Airtel | Batı Hindistan | Airtel, Sify, Vodafone Idea |
| **New York** | [Equinix NY9](https://www.equinix.com/locations/americas-colocation/united-states-colocation/new-york-data-centers/ny9/) | yok | CenturyLink Cloud Connect, Coresite, Equinx, Intercloud, Megaport, paket, Zayo |
| **Newport(Galler)** | [Yeni Nesil Veriler](https://www.nextgenerationdata.co.uk) | Birleşik Krallık Batı | İngiliz telekomünikasyon, Colt, düzey 3 Iletişimleri, yeni nesil veriler |
| **Osaka** | [Equinix OS1](https://www.equinix.com/locations/asia-colocation/japan-colocation/osaka-data-centers/os1/) | Japonya Batı | Colt, Equinx, Internet girişimi Japonya Inc.-IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **Paris** | [Interxiyon PAR5](https://www.interxion.com/Locations/paris/) | Fransa Orta | CenturyLink Cloud Connect, Colt, Equinix, Intercloud, ınterxiyon, turuncu, Telia taşıyıcısı, Zayo |
| **Perth** | [NextDC P1](https://www.nextdc.com/data-centres/p1-perth-data-centre) | yok | Megaport, NextDC |
| **Quebec City** | [Görüş](https://vantage-dc.com/data_centers/quebec-city-data-center-campus/) | Doğu Kanada | Bell Canada, Megaport |
| **San Antonio** | [CyrusOne SA1](https://cyrusone.com/locations/texas/san-antonio-texas/) | Orta Güney ABD | CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** | [Equinix SP2](https://www.equinix.com/locations/americas-colocation/brazil-colocation/sao-paulo-data-centers/sp2/) | Güney Brezilya | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica, UOLDIVEO |
| **Seattle** | [Equinix SE2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/seattle-data-centers/se2/) | Batı ABD 2 | Aryaka Networks, Equinx, düzey 3 Communications, Megaport, Telus, Zayo |
| **Seul** | [KINX Gasan IDC](https://www.kinx.net/support/location/?lang=en) | Kore Orta | KINX, LG CNS, Sejong Telecom |
| **Silikon Vadisi** | [Equinix SV1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/silicon-valley-data-centers/sv1/) | Batı ABD | Aryaka Networks, AT & T Netbonu, Ingiliz Teletası, CenturyLink Cloud Connect, Comcast, Coresite, Equınx, Intercloud, Internet2, x REACH, paket, PacketFabric, Level 3 Communications, Megaport, turuncu, Sprint, Tata Iletişimleri, Telia taşıyıcısı, Verizon, Zayo |
| **Silicon Valley2** | [Coresite SV7](https://www.coresite.com/data-centers/locations/silicon-valley/sv7) | Batı ABD | Coresite | 
| **Singapur** | [Equinix SG1](https://www.equinix.com/locations/asia-colocation/singapore-colocation/singapore-data-center/sg1/) | Güneydoğu Asya | Aryaka Networks, AT&T NetBond, British Telecom, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon, Vodafone |
| **Singapur2** | [Genel anahtar Day Seng](https://www.globalswitch.com/locations/singapore-data-centres/) | Güneydoğu Asya | Colt, Epsilon Global Communications, Megaport, SingTel |
| **Stockholm** | [Equinix SK1](https://www.equinix.com/locations/europe-colocation/sweden-colocation/stockholm-data-centers/sk1/) | yok | Telia taşıyıcısı |
| **Sidney** | [Equinix SY2](https://www.equinix.com/locations/asia-colocation/australia-colocation/sydney-data-centers/sy2/) | Avustralya Doğu | AARNet, AT & T Netbonı, Ingiliz telekomünikasyon, Devoli, Equinx, Kordia, Megaport, NEXTDC, NTT Communications, OPTI, turuncu, Spark NZ, Telstra Corporation, TPG Teley, Verizon, Vocus Group NZ |
| **Taipei** | Baş Telem | yok | Baş Telem, FarEasTone |
| **Tokyo** | [Equinix TY4](https://www.equinix.com/locations/asia-colocation/japan-colocation/tokyo-data-centers/ty4/) | Japonya Doğu | Aryaka Networks, AT & T Netbonu, Ingiliz telekomünikasyon, CenturyLink Cloud Connect, Colt, Equinix, Internet Initiative Japonya Inc.-IIJ, NTT Communications, NTT Doğu, turuncu, Softbank, Verizon |
| **Toronto** | [Birlikte bulundurgix TOR1](https://www.cologix.com/data-centers/toronto/tor1/) | Orta Kanada | & T Netbono, Bell Kanada, CenturyLink Cloud Connect, birlikte bulundurma, Equinx, x Reach Megaport, Telus, Verizon, Zayo |
| **Washington DC** | [Equinix DC2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/washington-dc-data-centers/dc2/) | Doğu ABD, Doğu ABD 2 | Aryaka Networks, AT & T Netbonu, Ingiliz Teletası, CenturyLink Cloud Connect, Geregix, Comcast, Coresite, Equınix, Internet2, Intercloud, Level 3 Communications, Megaport, Neutrona Networks,, NTT Communications, turuncu, PacketFabric, SES, Sprint, Tata İletişimler, Telia taşıyıcısı, Verizon, Zayo |
| **Washington DC2** | [Coresite Reston](https://www.coresite.com/data-centers/locations/northern-virginia-washington-dc/reston-campus) | Doğu ABD, Doğu ABD 2 |Coresite, Viasat, Zayo | 
| **Zürih** | [Interxiyon ZUR2](https://www.interxion.com/Locations/zurich/) | yok | Intercloud, ınterxiyon |

 **+** çok yakında anlamına geliyor

### <a name="national-cloud-environments"></a>Ulusal bulut ortamları

### <a name="us-government-cloud"></a>US Government bulutu
| **Location** | **Hizmet sağlayıcıları** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **New York** |Equinix, CenturyLink Cloud Connect, Verizon |
| **Phoenix** | & T Netbono, CenturyLink Cloud Connect, Megaport |
| **San Antonio** | CenturyLink Cloud Connect, Megaport |
| **Silikon Vadisi** | Equinix, Level 3 Communications, Verizon |
| **Seattle** | Equinix, Megaport |
| **Washington DC** |& T Netbono, CenturyLink Cloud Connect, Equinx, düzey 3 Communications, Megaport, Verizon |

### <a name="china"></a>Çin
| **Location** | **Hizmet sağlayıcıları** |
| --- | --- |
| **Pekin** |China Telecom |
| **Beijing2** | Çin Telem, GDS |
| **Şangay** |China Telecom |
| **Shanghai2** | Çin Telem, GDS |

Daha fazla bilgi için bkz. [Çin’de ExpressRoute](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Almanya
| **Location** | **Hizmet sağlayıcıları** |
| --- | --- |
| **Berlin** |e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="c1partners"></a>Exchange sağlayıcıları üzerinden bağlantı
Bağlantı sağlayıcınız önceki bölümlerde listelenmemişse hala bağlantı oluşturabilirsiniz.

* Yukarıdaki tabloda yer alan değişimlerin herhangi birine bağlı olup olmadığını görmek için bağlantı sağlayıcınıza başvurun. Değişim sağlayıcıları tarafından sunulan hizmetler hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kontrol edebilirsiniz. Birkaç bağlantı sağlayıcı Ethernet değişimlerine zaten bağlı.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [Equinix Cloud Exchange](https://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](https://www.interxion.com/)
  * [NextDC](https://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)
  
* Bağlantı sağlayıcınızı, ağınızı seçtiğiniz eşleme konumuna genişletmesini sağlayın.
  * Bağlantı sağlayıcınızın bağlantınızı yüksek oranda kullanılabilir şekilde genişlettiğinden emin olun, böylece hiç tek nokta arızası olmaz.
* Microsoft’a bağlanmak için bağlantı sağlayınız olarak değişime sahip bir ExpressRoute bağlantı hattı sipariş edin.
  * Bağlantı kurmak için [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)’daki adımları izleyin.

## <a name="connectivity-through-satellite-operators"></a>Uydu işleçleri üzerinden bağlantı
Uzaktan çalışıyorsanız ve fiber bağlantınız yoksa veya diğer bağlantı seçeneklerini araştırmak istiyorsanız aşağıdaki uydu işleçlerini kontrol edebilirsiniz. 

* Intelsat
* [IR](https://www.ses.com/networks/signature-solutions/signature-cloud/ses-and-azure-expressroute)
* [Viasat](http://www.directcloud.viasatbusiness.com/)

## <a name="c1partners"></a>Ek hizmet sağlayıcıları üzerinden bağlantı
| **Location** | **Exchange** | **Bağlantı sağlayıcıları** |
| --- | --- | --- |
| **Amsterdam** | Equinx, ınterxiyon, düzey 3 Iletişimleri | BICS, CloudXpress, Eurofiber, Fastweb S. p. A, Gulf Köprüsü International, Kalaam Telebel B. S. C, MainOne, Nianet, POST Telepı, Proximus, TDC Erhverv, Telekom |
| **Atlanta** | Equinix| Crown kalele
| **Cape Town** | Teraco | MTN |
| **Chicago** | Equinix| Crown kalele, SPI Enterprise, WINI akışı |
| **Dallas** | Equinix, Megaport | AXTEL, C3ntro Teley, Cox Iş, Crown, veri bulunan |
| **Frankfurt** | Interxion | BICS, Cinia, Nianet, QSC AG, Telekod Deutschland GmbH |
| **Hamburg** | Equinix | Cinia |
| **Hong Kong ÖIB** | Equinix | Başkan, makro görünümü telekomünikasyon |
| **Johannesburg** | Teraco | MTN |
| **Londra** | Bics, equinx, eunetçalışma| Bezeq International Ltd., CoreAzure, Epsilon telekomünikasyon Limited, üstel E, HSO, NexGen Networks, Proximus, Tamares Teley, Zain |
| **Los Angeles** | Equinix |Crown kalele, SPI kurumsal, Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies, Inc. Aptum teknolojileri, Roger, ZiRrO |
| **New York** |Equinix, Megaport | ALCE Iş, Crown role, SPI Enterprise, Webair |
| **Paris** | Equinix | Proximus |
| **Quebec City** | Megaport | Fibrenoire |
| **Sao Paula** | Equinix | Venha PRA Nua |
| **Seattle** |Equinix | Alaska Communications |
| **Silikon Vadisi** |Coresite, Equinx | Cox Business, SPIN Enterprise, WINI Stream, X2nsat Inc. |
| **Singapur** |Equinix |1CLOUDSTAR, BICS, CMC telekomünikasyon, Epsilon telekomünikasyon Limited, LGA telekomünikasyon, Birleşik bilgi Highway (UıH) |
| **Slough** | Equinix | HSO|
| **Sidney** | Megaport | Macquarie Telecom Group|
| **Tokyo** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix, Megaport | Airgate Technologies Inc., Beanfield Metroconnect, Aptum Technologies, IVedha Inc, Roger, Thinktel, ZiRrO|
| **Washington DC** |Equinix | Allet Iş, BICS, Cox Iş, Crown, GTT Communications Inc, Epsilon telekomünikasyon Limited, Masergy, Wınsırla |

## <a name="expressroute-system-integrators"></a>ExpressRoute sistem tümleştiricileri
İhtiyaçlarınıza uyan özel bağlantıyı etkinleştirme ağınızın ölçeğine bağlı olarak zorlu olabilir. ExpressRoute’a yönelik ekleme işleminde size yardımcı olmak üzere aşağıdaki tabloda listelenen herhangi bir sistem tümleştirici ile çalışabilirsiniz.

| **Continent** | **Sistem tümleştiricileri** |
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
