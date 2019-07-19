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
ms.date: 07/17/2019
ms.author: cherylmc
ms.openlocfilehash: 331a554332db8c05d4ece1a1df5d6557d6e86906
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310355"
---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute ortakları ve eşleme konumları

> [!div class="op_single_selector"]
> * [Sağlayıcıya Göre Konumlar](expressroute-locations.md)
> * [Konuma Göre Sağlayıcılar](expressroute-locations-providers.md)


Bu makaledeki tablolar ExpressRoute bağlantı sağlayıcıları, ExpressRoute coğrafi kapsamı, ExpressRoute üzerinden desteklenen Microsoft bulut hizmetleri ve ExpressRoute Sistem Tümleştiricileri (SIs) hakkında bilgi sağlar.

## <a name="partners"></a>ExpressRoute bağlantı sağlayıcıları
ExpressRoute tüm Azure bölgeleri ve konumları arasında desteklenir. Aşağıdaki harita Azure bölgeleri ve ExpressRoute konumlarının listesini sağlar. ExpressRoute konumları birkaç hizmet sağlayıcının sahip olduğu Microsoft eşlerine başvurur.

![Konum eşleme][0]

Coğrafi bölge içindeki en az bir ExpressRoute konumuna bağlanırsanız coğrafi bölge içindeki tüm bölgeler arasında Azure hizmetlerine erişebileceksiniz. 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Bir coğrafi bölge içindeki Azure bölgeler ile ExpressRoute konumları arasında eşleme
Aşağıdaki tablo, coğrafi bölge içindeki Azure bölgeler ile ExpressRoute konumları arasında yapılan eşlemeyi sağlar.

| **Jeopolitik bölge** | **Bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- | --- |
| **Australia Government** | 1\. | Avustralya Orta, Avustralya Orta 2 |Kanberra, Kanberra2 |
| **Avrupa** | 1\. |Fransa Orta, Fransa Güney, Kuzey Avrupa, Batı Avrupa, UK Batı, UK Güney |Amsterdam, Amsterdam2, Dublin, Frankfurt, Londra, London2, Marseille, Newport (Wales), Paris, Zurich |
| **Kuzey Amerika** | 1\. |Doğu ABD, Batı ABD, Doğu ABD 2, Batı ABD 2, Orta ABD, Orta Güney ABD, Orta Kuzey ABD, Batı Orta ABD, Orta Kanada, Doğu Kanada |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miamı, New York, San Antonio, Seattle, Silicon Valley, Silicon Valley2, Washington DC, Washington DC2, Montreal, Quebec City, toron |
| **Asya** | 2 |Doğu Asya, Güneydoğu Asya |Hong Kong ÖIB, Kuala Lumpur, Singapur, Singapur2, Taipei |
| **Hindistan** | 2 |Batı Hindistan, Orta Hindistan, Güney Hindistan |Madras, Madras2, Bombay, Bombay2 |
| **Japonya** | 2 |Batı Japonya, Doğu Japonya |Osaka, Tokyo |
| **Okyanusya** | 2 |Güneydoğu Avustralya, Doğu Avustralya |Auckland, Melbourne, Perth, Sidney | 
| **Güney Kore** | 2 |Kore Orta, Kore Güney |Busan, Seul|
| **BAE** | 3 | BAE Orta, BAE Kuzey | Dubai, Dubai2 |
| **Güney Afrika** | 3 |Güney Afrika Batı, Güney Afrika Kuzey |Cape Town, Johannesburg |
| **Güney Amerika** | 3 |Güney Brezilya |Sao Paulo |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Ulusal bulutlar için bölgeler ve coğrafi sınırlar
Aşağıdaki tablo ulusal bulutlar için bölgeler ve coğrafi sınırlar hakkında bilgi sağlar.

| **Jeopolitik bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- |
| **US Government bulutu** |US Gov Arizona, US Gov Iowa, US Gov Teksas, US Gov Virginia, US DoD Orta, US DoD Doğu  |Chicago, Dallas, New York, Phoenix, San Antonio, Seattle, Silikon Vadisi, Washington DC |
| **Çin Doğu** |Çin Doğu, Çin Doğu2 |Shanghai, Shanghai2 |
| **Çin Kuzey** |Çin Kuzey, Çin Kuzey2 |Pekin, Beijing2 |
| **Almanya** |Orta Almanya, Doğu Almanya |Berlin, Frankfurt |

Coğrafi bölgeler arasındaki bağlantı standart ExpressRoute SKU’da desteklenmiyor. Genel bağlantıyı desteklemek için ExpressRoute premium eklentisini etkinleştirmeniz gerekir. Ulusal bulut ortamlarına bağlantı desteklenmiyor. Bu tür bir ihtiyaç ortaya çıkarsa bağlantı sağlayıcınız ile çalışabilirsiniz.

## <a name="locations"></a>Bağlantı sağlayıcı konumları

Aşağıdaki tabloda bağlantı konumları ve her konum için hizmet sağlayıcıları gösterilmektedir. Hizmet sağlayıcılarını ve hizmet sunabildikleri konumları görüntülemek istiyorsanız bkz. [Hizmet sağlayıcısına göre konumlar](expressroute-locations.md#locations). 

**Yerel Azure bölgeleri** , her bir eşleme konumundaki [ExpressRoute yerellerinin](expressroute-faqs.md) erişebileceği alanlardır. **yok** , ExpressRoute yerel 'in o eşleme konumunda kullanılabilir olmadığını gösterir.


### <a name="production-azure"></a>Üretim Azure
| **Location** | **Eşdüzey Hizmet Sağlama Konumu Sahibi** | **Yerel Azure bölgeleri** | **Hizmet Sağlayıcılar** |
| --- | --- | --- | --- |
| **Amsterdam** | Equinix | Batı Avrupa | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Interxion, KPN, IX Reach, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Telia Carrier, Verizon, Zayo |
| **Amsterdam2** | Interxion | Batı Avrupa | CenturyLink Cloud Connect, DE-CIX, ınterxiyon, Vodavfone |
| **Atlanta** | Equinix | yok | Equinix, Megaport |
| **Auckland** | Vocus grubu NZ | yok | Devoli, Kordia, Megaport, Vocus grubu NZ |
| **Busan** |LG CNS | Kore Güney | LG CNS |
| **Kanberra** | CDC | Avustralya Orta | CDC |
| **Kanberra2** | CDC | Avustralya Orta 2| CDC |
| **Cape Town** | Teraco | Güney Afrika Batı | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Chennai** | Tata Communications | Güney Hindistan | Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chennai2** | Airtel | Güney Hindistan | Airtel |
| **Chicago** | Equinix | Orta Kuzey ABD | Aryaka Networks, & T Netbono, CenturyLink Cloud Connect, Geregix, Comcast, Coresıte, Equinix, Intercloud, Internet2, Level 3 Communications, Megaport, PacketFabric, PCFA Global Limited, Sprint, Telia taşıyıcısı, Verizon, Zayo |
| **Dallas** | Equinix | yok | Aryaka Networks, & T Netbonıngıgix, Equinix, Internet2, düzey 3 Communications, Megaport, nörona Networks, Telmex Unınet, Telia taşıyıcısı, Transtelco, Verizon, Zayo|
| **Denver** | CoreSite | Batı Orta ABD | CoreSite, Megaport, Zayo |
| **Dubai** | Etisalat UAE | BAE Kuzey | Etisalat UAE |
| **Dubai2** | du datamena | BAE Kuzey | du datamena, Cumxcom |
| **Dublin** | Equinix | Kuzey Avrupa | Colt, eir, Equinix, Interxion, Megaport |
| **Frankfurt** | Interxion | yok | DE-CIX, ınterxiyon |
| **Hong Kong ÖIB** | Equinix | Doğu Asya | Aryaka Networks, Ingiliz Teletası, CenturyLink Cloud Connect, baş Telem, Çin telekomünikasyon küresel, Equinx, Megaport, NTT Communications, turuncu, PCCW küresel Limited, Tata Iletişimleri, Telia taşıyıcısı, Verizon |
| **Johannesburg** | Teraco | Güney Afrika Kuzey | İngiliz Telem, Internet çözümleri-bulut bağlantısı, sıvı telekomünikasyon, Teraco |
| **Kuala Lumpur** | TIME dotCom | yok | TIME dotCom |
| **Las Vegas** | Anahtar | yok | CenturyLink Cloud Connect, Megaport |
| **Londra** | Equinix | Birleşik Krallık Güney | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telehouse - KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **London2** | Teleevi | Birleşik Krallık Güney | X ile ulaşma, Equinix |
| **Los Angeles** | CoreSite | yok | CoreSite, Equinx, Megaport, Neutrona Networks, NTT, Zayo |
| **Marsilya** |Interxion | Fransa Güney | DE-CIX, ınterxiyon, Jaguar Network |
| **Melbourne** | NextDC | Avustralya Güneydoğu | AARNet, Devoli, Equinix, Megaport, NEXTDC, Ops, Telstra Corporation, TPG telekomünikasyon |
| **Miami** | Equinix | yok | C3ntro+, Equinix, Megaport, Neutrona Networks |
| **Montreal** | Cologix | yok | Bell Canada, Cologix, Telus, Zayo |
| **Mumbai** | Tata Communications | Batı Hindistan | Küresel CloudXchange (GCX), Reliance jıo, Sıfy, Tata Iletişimleri, Verizon |
| **Mumbai2** | Airtel | Batı Hindistan | Airtel, Sify, Vodafone Idea |
| **New York** | Equinix | yok | CenturyLink Cloud Connect, Coresite, Equinx, Intercloud, Megaport, paket, Zayo |
| **Newport(Galler)** | Next Generation Data | Birleşik Krallık Batı | İngiliz telekomünikasyon, Colt, düzey 3 Iletişimleri, yeni nesil veriler |
| **Osaka** | Equinix | Japonya Batı | Colt, Equinx, Internet girişimi Japonya Inc.-IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **Paris** | Interxion | Fransa Orta | CenturyLink Cloud Connect, Colt, Equinix, Intercloud, ınterxiyon, turuncu, Telia taşıyıcısı, Zayo |
| **Perth** | NextDC | yok | Megaport, NextDC |
| **Quebec City** | 4Degrees | Doğu Kanada | Bell Canada, Megaport |
| **San Antonio** | CyrusOne | Orta Güney ABD | CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** | Equinix | Güney Brezilya | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica, UOLDIVEO |
| **Seattle** | Equinix | Batı ABD 2 | Aryaka Networks, Equinx, düzey 3 Communications, Megaport, Telus, Zayo |
| **Seul** | KINX | Kore Orta | KINX, LG CNS, Sejong Telecom |
| **Silikon Vadisi** | Equinix | Batı ABD | Aryaka Networks, AT & T Netbonu, Ingiliz Teletası, CenturyLink Cloud Connect, Comcast, Coresite, Equınx, Intercloud, x REACH, paket, PacketFabric, düzey 3 Communications, Megaport, turuncu, Sprint, Tata Communications, Verizon, Zayo |
| **Silicon Valley2** | Coresite | Batı ABD | Coresite | 
| **Singapur** | Equinix | Güneydoğu Asya | Aryaka Networks, AT&T NetBond, British Telecom, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon, Vodafone |
| **Singapur2** | Global Switch | Güneydoğu Asya | Colt, Epsilon Global Communications, Megaport, SingTel |
| **Sidney** | Equinix | Avustralya Doğu | AARNet, AT & T Netbonı, Ingiliz telekomünikasyon, Devoli, Equinx, Kordia, Megaport, NEXTDC, NTT Communications, OPTI, turuncu, Spark NZ, Telstra Corporation, TPG Teley, Verizon, Vocus Group NZ |
| **Taipei** | Baş Telem | yok | Baş Telem, FarEasTone |
| **Tokyo** | Equinix | Japonya Doğu | Aryaka Networks, AT & T Netbonu, Ingiliz telekomünikasyon, CenturyLink Cloud Connect, Colt, Equinix, Internet Initiative Japonya Inc.-IIJ, NTT Communications, NTT Doğu, turuncu, Softbank, Verizon |
| **Toronto** | Cologix | Orta Kanada | & T Netbono, Bell Kanada, CenturyLink Cloud Connect, birlikte bulundurma, Equinx, x Reach Megaport, Telus, Verizon, Zayo |
| **Washington DC** | Equinix | Doğu ABD, Doğu ABD 2 | Aryaka Networks, AT & T Netbonu, Ingiliz telekomünikasyon, CenturyLink Cloud Connect, Geregix, Comcast, Coresite, Equınix, Internet2, Intercloud, düzey 3 Communications, Megaport, Neutrona Networks, NTT Communications, turuncu, PacketFabric, Sprint, Tata İletişimler, Telia taşıyıcısı, Verizon, Zayo |
| **Washington DC2** | Coresite | Doğu ABD, Doğu ABD 2 |Coresite | 
| **Zürih** | Interxion | yok | Intercloud, ınterxiyon |

 **+** çok yakında anlamına geliyor

### <a name="national-cloud-environments"></a>Ulusal bulut ortamları

### <a name="us-government-cloud"></a>US Government bulutu
| **Location** | **Hizmet Sağlayıcılar** |
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
| **Location** | **Hizmet Sağlayıcılar** |
| --- | --- |
| **Pekin** |China Telecom |
| **Beijing2** | Çin Telem, GDS |
| **Şangay** |China Telecom |
| **Shanghai2** | Çin Telem, GDS |

Daha fazla bilgi için bkz. [Çin’de ExpressRoute](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Almanya
| **Konum** | **Hizmet Sağlayıcılar** |
| --- | --- |
| **Berlin** |e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="c1partners"></a>Exchange Sağlayıcıları Üzerinden Bağlantı
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

## <a name="c1partners"></a>Diğer Hizmet Sağlayıcılar Üzerinden Bağlantı
| **Location** | **Exchange** | **Bağlantı Sağlayıcılar** |
| --- | --- | --- |
| **Amsterdam** | Equinx, ınterxiyon, düzey 3 Iletişimleri, Telecity | BICS, CloudXpress, Eurofiber, Fastweb S. p. A, Gulf Köprüsü International, Kalaam Telebel B. S. C, MainOne, Nianet, Post, Proximus, TDC Erhverv, telekomünikasyon Italia parlak Le, Telekom Deutschland GmbH, Telia |
| **Atlanta** | Equinix| Crown kalele
| **Cape Town** | Teraco | MTN |
| **Chicago** | Equinix| Crown kalele, SPI Enterprise, WINI akışı |
| **Dallas** | Equinix, Megaport | AXTEL, C3ntro Teley, Cox Iş, Crown, veri bulunan |
| **Frankfurt** | Interxiyon, Telecity | BICS, Cinia, Nianet, QSC AG, Telekod Deutschland GmbH |
| **Hamburg** | Equinix | Cinia |
| **Hong Kong ÖIB** | Equinix | Başkan, makro görünümü telekomünikasyon |
| **Johannesburg** | Teraco | MTN |
| **Londra** | BICS, Equinx, Eunetçalışma, Telecity | Bezeq International Ltd., CoreAzure, Epsilon telekomünikasyon Limited, üstel E, HSO, NexGen Networks, Proximus, Tamares Teley, Zain |
| **Los Angeles** | Equinix |Crown kalele, SPI kurumsal, Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies, Inc. Cogeco eşi 1, Roçüler, ZiRrO |
| **New York** |Equinix, Megaport | ALCE Iş, Crown role, SPI Enterprise, Webair |
| **Paris** | Equinix | Proximus |
| **Quebec City** | Megaport | Fibrenoire |
| **Sao Paula** | Equinix | Venha PRA Nua |
| **Seattle** |Equinix | Alaska Communications |
| **Silikon Vadisi** |Coresite, Equinx | Cox Business, SPIN Enterprise, WINI Stream, X2nsat Inc. |
| **Singapur** |Equinix |1CLOUDSTAR, BICS, Epsilon Telecommunications Limited, LGA Telecom, United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sidney** | Megaport | Macquarie Telecom Group|
| **Tokyo** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix, Megaport | Airgate Technologies Inc., Beanfield Metroconnect, Cogeco peer 1, IVedha Inc, Roger, Thinktel, ZiRrO|
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
