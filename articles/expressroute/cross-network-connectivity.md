---
title: Azure ağ arası bağlantı
description: Bu sayfada, Azure ağ özelliklerine dayalı çapraz ağ bağlantısı ve çözümü için bir uygulama senaryosu açıklanmaktadır.
services: expressroute
author: rambk
ms.service: expressroute
ms.topic: article
ms.date: 04/03/2019
ms.author: rambala
ms.openlocfilehash: 48ec26cc98310dfeb61aa17018c940b431cfbcee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75644265"
---
# <a name="cross-network-connectivity"></a>Ağlar arası bağlantı

Fabrikam Inc. Doğu ABD'de büyük bir fiziksel varlığa ve Azure dağıtımına sahiptir. Fabrikam, ExpressRoute üzerinden şirket içi ve Azure dağıtımları arasında arka uç bağlantısına sahiptir. Benzer şekilde, Contoso Ltd.'nin Batı ABD'de bir varlığı ve Azure dağıtımı vardır. Contoso, ExpressRoute aracılığıyla şirket içi ve Azure dağıtımları arasında arka uç bağlantısına sahiptir.  

Fabrikam A.Ş. Contoso Ltd.'yi satın aldı. Birleşmenin ardından Fabrikam ağları birbirine bağlamak istiyor. Aşağıdaki şekil senaryoyu göstermektedir:

 [![1.1.2.2.]][1]

Yukarıdaki şeklin ortasındaki kesik olar istenilen ağ ara bağlantılarını gösterir. Özellikle, istenen üç tür çapraz bağlantı vardır: 1) Fabrikam ve Contoso VNets çapraz bağlantı, 2) Çapraz bölgesel şirket içi ve VNets çapraz bağlantılar (diğer bir şekilde, Fabrikam şirket içi ağı Contoso VNet'e bağlamak ve Contoso'yu bağlamak Fabrikam VNet'e şirket içi ağ) ve 3) Fabrikam ve Contoso şirket içi ağ çapraz bağlanmak. 

Aşağıdaki tablo, birleşmeden önce Contoso Ltd.'nin ExpressRoute'unun özel bakışlarının rota tablosunu gösterir.

[![2 2 2.000]][2]

Aşağıdaki tablo, birleşmeden önce Contoso aboneliğinde bir VM'nin etkili rotalarını gösterir. Tablo başına, VNet'teki VM, varsayılan ağ dışında VNet adres alanı nın ve Contoso şirket içi ağının farkındadır. 

[![4 4]][4]

Aşağıdaki tablo, birleşmeden önce Fabrikam A.Ş.'nin ExpressRoute'unun özel bakışlarının rota tablosunu gösterir.

[![3 3]][3]

Aşağıdaki tablo, birleşmeden önce Fabrikam aboneliğinde bir VM'nin etkili rotalarını gösterir. Tablo başına, VNet'teki VM, varsayılan ağ dışında VNet adres alanı nın ve Fabrikam şirket içi ağının farkındadır.

[![5 5,5]][5]

Bu makalede, adım adım gidelim ve aşağıdaki Azure ağ özelliklerini kullanarak istenilen çapraz bağlantıları nasıl elde ediletilelim tartışalım:

* [Sanal ağ eşleme][Virtual network peering] 
* [Sanal ağ ExpressRoute bağlantısı][connection]
* [Küresel Erişim][Global Reach] 

## <a name="cross-connecting-vnets"></a>Çapraz bağlantı VNets

Sanal ağ eşleme (VNet eşleme) iki sanal ağı bağlarken en iyi ve en iyi ağ performansını sağlar. VNet eşleme, hem aynı Azure bölgesinde (genellikle VNet eşleme olarak adlandırılır) hem de iki farklı Azure bölgesinde (genellikle Global VNet eşleme olarak adlandırılır) iki VNet'i eşleneme yi destekler. 

Global VNet'i Contoso'daki VNet'ler ve Fabrikam Azure abonelikleri arasında yapılandıralım. İki sanal ağ arasında sanal ağ eşleme oluşturma hakkında [bkz.][Configure VNet peering]

Aşağıdaki resim, Global VNet eşlemesini yapılandırdıktan sonra ağ mimarisini gösterir.

[![6]][6]

Aşağıdaki tablo, Contoso abonelik VM bilinen yolları gösterir. Tablonun son girişine dikkat edin. Bu giriş, sanal ağları çapraz bağlamanın bir sonucudur.

[![7]][7]

Aşağıdaki tablo, Fabrikam aboneliği VM tarafından bilinen yolları gösterir. Tablonun son girişine dikkat edin. Bu giriş, sanal ağları çapraz bağlamanın bir sonucudur.

[![8]][8]

VNet peering doğrudan iki sanal ağları bağlar (yukarıdaki iki tabloda *VNetGlobalPeering* giriş için bir sonraki atlama yok bakın)

## <a name="cross-connecting-vnets-to-the-on-premises-networks"></a>VNet'leri şirket içi ağlara çapraz bağlama

Bir ExpressRoute devresini birden fazla sanal ağa bağlayabiliriz. ExpressRoute devresine bağlanabilen maksimum sanal ağ sayısı için [Abonelik ve hizmet sınırlarına][Subscription limits] bakın. 

Sanal ağlar ve şirket içi ağlar arasında çapraz bağlantı sağlamak için Fabrikam ExpressRoute devresini Contoso abonelik VNet'e ve benzer şekilde Contoso ExpressRoute devresini Fabrikam abonelik VNet'e bağlayalım. Sanal bir ağı farklı bir abonelikteki ExpressRoute devresine bağlamak için bir yetkilendirme oluşturmamız ve kullanmamız gerekir.  Makaleye bakın: [Bir ExpressRoute devresine sanal ağ bağlayın.][Connect-ER-VNet]

Aşağıdaki resim, ExpressRoute çapraz bağlantısını sanal ağlara yapılandırdıktan sonra ağ mimarisini gösterir.

[![9]][9]

Aşağıdaki tablo, expressroute üzerinden şirket içi ağlara sanal ağları çapraz bağlandıktan sonra Contoso Ltd.'nin ExpressRoute'unun özel bakışlarının rota tablosunu gösterir. Rota tablosunun her iki sanal ağa da ait rotaları olduğunu görün.

[![10]][10]

Aşağıdaki tablo, Sanal ağları ExpressRoute üzerinden şirket içi ağlara bağladıktan sonra Fabrikam Inc.'in ExpressRoute'unun özel bakışlarının rota tablosunu gösterir. Rota tablosunun her iki sanal ağa da ait rotaları olduğunu görün.

[![11]][11]

Aşağıdaki tablo, Contoso abonelik VM bilinen yolları gösterir. Tablonun *Sanal ağ ağ geçidi* girişlerine dikkat edin. VM, her iki şirket içi ağ için de rotalar görür.

[![12]][12]

Aşağıdaki tablo, Fabrikam aboneliği VM tarafından bilinen yolları gösterir. Tablonun *Sanal ağ ağ geçidi* girişlerine dikkat edin. VM, her iki şirket içi ağ için de rotalar görür.

[![13]][13]

>[!NOTE]
>Fabrikam ve/veya Contoso aboneliklerinde vnet'leri ilgili hub VNet'e de konuşmuş olabilirsiniz (hub ve kollu tasarım bu makaledeki mimari diyagramlarda gösterilemez). ExpressRoute hub VNet ağ geçitleri arasındaki çapraz bağlantılar da Doğu ve Batı hub ve kollu arasında iletişim sağlayacaktır.
>

## <a name="cross-connecting-on-premises-networks"></a>Şirket içi ağları çapraz bağlama

ExpressRoute Global Reach, farklı ExpressRoute devrelerine bağlı şirket içi ağlar arasında bağlantı sağlar. Global Reach'i Contoso ve Fabrikam ExpressRoute devreleri arasında yapılandıralım. ExpressRoute devreleri farklı aboneliklerde olduğundan, bir yetkilendirme oluşturmamız ve kullanmamız gerekir. Adım adım kılavuzluk için [ExpressRoute Global Reach makalelerini yapılandırın][Configure Global Reach] makalesine bakın.

Aşağıdaki resim, Global Reach'i yapılandırdıktan sonra ağ mimarisini gösterir.

[![14]][14]

Aşağıdaki tablo, Global Reach'i yapılandırdıktan sonra Contoso Ltd.'nin ExpressRoute'unun özel bakışlarının rota tablosunu gösterir. Rota tablosunun her iki şirket içi ağlara ait rotaları olduğunu görün. 

[![15]][15]

Aşağıdaki tablo, Global Reach'i yapılandırdıktan sonra Fabrikam Inc.'in ExpressRoute'unun özel bakışlarının rota tablosunu gösterir. Rota tablosunun her iki şirket içi ağlara ait rotaları olduğunu görün.

[![16]][16]

## <a name="next-steps"></a>Sonraki adımlar

VNet ve VNet-peering ile ilgili diğer sorularınız için [sanal ağ SSS'ye][VNet-FAQ]bakın. ExpressRoute ve sanal ağ bağlantısı yla ilgili diğer sorularınız için [ExpressRoute SSS][ER-FAQ] bölümüne bakın.

Global Reach, ülke/bölge bazında kullanıma sunuldu. Global Erişimin istediğiniz ülkelerde/bölgelerde kullanılabilir olup olmadığını görmek için [ExpressRoute Global Reach'e][Global Reach]bakın.

<!--Image References-->
[1]: ./media/cross-network-connectivity/premergerscenario.png "Uygulama senaryosu"
[Birleşmeden]: ./media/cross-network-connectivity/contosoexr-rt-premerger.pngönce 2 "Contoso ExpressRoute rota tablosu"
[Birleşmeden]: ./media/cross-network-connectivity/fabrikamexr-rt-premerger.pngönce 3 "Fabrikam ExpressRoute rota tablosu"
[Birleşmeden]: ./media/cross-network-connectivity/contosovm-routes-premerger.pngönce 4 "Contoso VM rotası"
[Birleşmeden]: ./media/cross-network-connectivity/fabrikamvm-routes-premerger.pngönce 5 "Fabrikam VM rotası"
[6]: ./media/cross-network-connectivity/vnet-peering.png "VNet-peering sonrası Mimari"
[7]: ./media/cross-network-connectivity/contosovm-routes-peering.png "VNet'ten sonra 7 Contoso VM rotası"
[8]: ./media/cross-network-connectivity/fabrikamvm-routes-peering.png "VNet'ten sonra 8 Fabrikam VM rotası"
[9]: ./media/cross-network-connectivity/exr-x-connect.png "ExpressRoutes çapraz bağlantı dan sonra Mimari"
[ExR]: ./media/cross-network-connectivity/contosoexr-rt-xconnect.png "ve VNets'i çapraz bağladıktan sonra 10 Contoso ExpressRoute rota tablosu"
 "ExR ve VNets'i çapraz bağladıktan sonra" [11]: ./media/cross-network-connectivity/fabrikamexr-rt-xconnect.pngFabrikam ExpressRoute rota tablosu
[ExR]: ./media/cross-network-connectivity/contosovm-routes-xconnect.png "ve VNets'i çapraz bağladıktan sonra 12 Contoso VM rotası"
 "ExR ve VNets'i çapraz bağladıktan sonra" [13]: ./media/cross-network-connectivity/fabrikamvm-routes-xconnect.pngFabrikam VM rotası
[14]: ./media/cross-network-connectivity/globalreach.png "Global Reach'i yapılandırdıktan sonra mimari"
Global Reach'ten sonra [15]: ./media/cross-network-connectivity/contosoexr-rt-gr.png "Contoso ExpressRoute rota tablosu"
Global Reach sonrası [16]: ./media/cross-network-connectivity/fabrikamexr-rt-gr.png "Fabrikam ExpressRoute rota tablosu"

<!--Link References-->
[Virtual network peering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview
[connection]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager
[Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-global-reach
[Configure VNet peering]: https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach
[Subscription limits]: https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits
[Connect-ER-VNet]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager
[ER-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[VNet-FAQ]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq