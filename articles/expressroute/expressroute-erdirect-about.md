---
title: Azure ExpressRoute Direct hakkında
description: Azure ExpressRoute Direct 'in temel özellikleri ve kullanılabilir SKU 'Lar ve teknik gereksinimler gibi ExpressRoute Direct 'e eklemek için gereken bilgiler hakkında bilgi edinin.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: duau
ms.openlocfilehash: 0cc07f9647c4f7836438adbe2b4cf9fa24a1c890
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89566407"
---
# <a name="about-expressroute-direct"></a>ExpressRoute Direct hakkında

ExpressRoute Direct, doğrudan Microsoft 'un Dünya genelinde dağıtılmış olan eşleme konumlarında doğrudan Microsoft 'un küresel ağına bağlanmasına olanak sağlar. ExpressRoute Direct, ölçek sırasında etkin/etkin bağlantıyı destekleyen çift 100 Gbps veya 10 Gbps bağlantı sağlar.

ExpressRoute Direct 'in sağladığı temel özellikler dahil, ancak bunlarla sınırlı değildir:

* Depolama ve Cosmos DB gibi hizmetler için Büyük Veri Alımı özelliği
* Düzenlenmiş ve şunun gibi adanmış ve yalıtılmış bağlantı gerektiren sektörler için fiziksel yalıtım: bankacılık, kamu ve perakende
* Bağlantı hattı dağıtımının iş birimine dayalı detaylı denetimi

## <a name="onboard-to-expressroute-direct"></a>ExpressRoute Direct 'e ekleme

ExpressRoute Direct kullanmadan önce aboneliğinizi kaydetmeniz gerekir. Kaydolmak için aşağıdaki ayrıntılar dahil olmak üzere abonelik KIMLIĞINIZLE bir e-posta gönderin <ExpressRouteDirect@microsoft.com> :

* **ExpressRoute Direct** ile başarmak için kullandığınız senaryolar
* Konum tercihleri-tüm konumların tam listesi için [Iş ortakları ve eşleme konumları](expressroute-locations-providers.md) bölümüne bakın
* Uygulama için zaman çizelgesi
* Diğer sorular

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>Bir hizmet sağlayıcısı ve ExpressRoute Direct kullanarak ExpressRoute

| **Bir hizmet sağlayıcısı kullanarak ExpressRoute** | **ExpressRoute Direct** | 
| --- | --- |
| Mevcut altyapıya hızlı ekleme ve bağlantı sağlamak için hizmet sağlayıcılarını kullanır | 100 Gbps/10 Gbps altyapısı ve tüm katmanların tam yönetimi gerekir
| Ethernet ve MPLS dahil yüzlerce sağlayıcı ile tümleşir | Düzenlenen sektörler ve çok büyük veri alımı için doğrudan/ayrılmış kapasite |
| 50 Mbps 'den 10 Gbps 'ye devre SKU 'Ları | Müşteri, 100 Gbps ExpressRoute Direct üzerinde aşağıdaki devre SKU 'Larının birleşimini seçebilir: <ul><li>5 Gbps</li><li>10 Gbps</li><li>40 Gbps</li><li>100 Gbps</li></ul> Müşteri, 10 Gbps ExpressRoute Direct üzerinde aşağıdaki devre SKU 'Larının birleşimini seçebilir:<ul><li>1 Gbps</li><li>2 Gbps</li><li>5 Gbps</li><li>10 Gbps</li></ul>
| Tek kiracı için iyileştirildi | Birden çok iş birimi ve birden çok iş ortamı ile tek bir kiracı için iyileştirildi

## <a name="expressroute-direct-circuits"></a>ExpressRoute doğrudan devreleri

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute ile Microsoft Azure ve Microsoft 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.

Her bir eşleme konumu Microsoft 'un küresel ağına erişebilir ve varsayılan olarak, coğrafi olmayan bir bölgedeki herhangi bir bölgeye erişebilir ve Premium bir devrenle tüm genel bölgelere erişebilir.  

Çoğu senaryonun işlevselliği, bir ExpressRoute hizmet sağlayıcısı 'nı çalıştırmak için kullanılan devrelere eşdeğerdir. ExpressRoute Direct kullanılarak sunulan daha fazla ayrıntı düzeyi ve yeni özellikleri desteklemek için, ExpressRoute doğrudan devrelerde mevcut olan bazı önemli yetenekler vardır.

## <a name="circuit-skus"></a>Devre SKU 'Ları

ExpressRoute Direct, Azure depolama ve diğer büyük veri Hizmetleri için geniş kapsamlı veri alma senaryolarını destekler. 100 Gbps ExpressRoute doğrudan 'daki ExpressRoute devreleri, **40 Gbps** ve **100 Gbps** devre SKU 'larını da desteklemektedir. Fiziksel bağlantı noktası çiftleri yalnızca **100 veya 10 GB/sn** olabilir ve birden çok sanal Devreme sahip olabilir. Devre boyutları:

| **100 Gbps ExpressRoute Direct** | **10 Gbps ExpressRoute Direct** | 
| --- | --- |
| **Abone olunan bant genişliği**: 200 Gbps | **Abone olunan bant genişliği**: 20 Gbps |
| <ul><li>5 Gbps</li><li>10 Gbps</li><li>40 Gbps</li><li>100 Gbps</li></ul> | <ul><li>1 Gbps</li><li>2 Gbps</li><li>5 Gbps</li><li>10 Gbps</li></ul>

## <a name="technical-requirements"></a>Teknik gereksinimler

* Microsoft Kurumsal kenar yönlendirici (MSEE) arabirimleri:
    * Yalnızca yönlendirici çiftinde çift 10 veya 100 Gigabit Ethernet bağlantı noktası
    * Tek modlu LR fiber bağlantı
    * IPv4 ve IPv6
    * IP MTU 1500 bayt

* Anahtar/yönlendirici katman 2/katman 3 bağlantısı:
    * 1 802.1 Q (Dot1Q) etiketi veya iki etiket 802.1 Q (QinQ) etiketi kapsüllemeyi desteklemelidir
    * EtherType = 0x8100
    * Microsoft tarafından belirtilen VLAN KIMLIğINE bağlı olarak dış VLAN etiketi (STAG) eklenmelidir ve *yalnızca QinQ üzerinde geçerlidir*
    * Bağlantı noktası ve cihaz başına birden çok BGP oturumu (VLAN) desteklemelidir
    * IPv4 ve IPv6 bağlantısı. *IPv6 için ek bir alt arabirim oluşturulmaz. IPv6 adresi var olan alt arabirime eklenecektir*. 
    * İsteğe bağlı: ExpressRoute devrelerinde tüm özel eşlemelerde varsayılan olarak yapılandırılan [çift yönlü Iletme algılaması (BFD)](https://docs.microsoft.com/azure/expressroute/expressroute-bfd) desteği

## <a name="vlan-tagging"></a>VLAN etiketleme

ExpressRoute Direct, Qınq ve Dot1Q VLAN etiketlemesini destekler.

* **Qınq VLAN etiketleme** , ExpressRoute bağlantı hattı başına yalıtılmış yönlendirme etki alanlarına izin verir. Azure, devre oluşturma sırasında dinamik olarak S-Tag ayırır ve değiştirilemez. Devre üzerindeki her eşleme (özel ve Microsoft) VLAN olarak benzersiz bir C-Tag kullanır. C etiketinin, ExpressRoute doğrudan bağlantı noktalarında devreler arasında benzersiz olması gerekmez.

* **DOT1Q VLAN etiketleme** , ExpressRoute doğrudan bağlantı noktası çiftliğine göre tek etiketli VLAN için izin verir. Eşleme üzerinde kullanılan bir C etiketi, ExpressRoute doğrudan bağlantı noktası çiftinin tüm devrelerde ve eşleştirmelerinde benzersiz olmalıdır.

## <a name="workflow"></a>İş akışı

[![akışıyla](./media/expressroute-erdirect-about/workflow1.png)](./media/expressroute-erdirect-about/workflow1.png#lightbox)

## <a name="sla"></a>SLA

ExpressRoute Direct, Microsoft Global Network 'e etkin/etkin yedekli bağlantılarla aynı kurumsal düzeyde SLA sağlar. ExpressRoute altyapısı gereksizdir ve Microsoft küresel ağı ile bağlantı, müşteri gereksinimleriyle ilgili olarak yedekli ve farklı bir şekilde ölçeklendirilir. 

## <a name="next-steps"></a>Sonraki adımlar

[ExpressRoute Direct'i yapılandırma](expressroute-howto-erdirect.md)
