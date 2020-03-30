---
title: Azure ExpressRoute Direct Hakkında
description: Bu sayfa ExpressRoute Direct'e genel bir bakış sağlar
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: jaredro
ms.openlocfilehash: f26fdd75e0a6c6228d329a8b9be4743072ae54c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74083598"
---
# <a name="about-expressroute-direct"></a>ExpressRoute Direct hakkında

ExpressRoute Direct, dünya çapında stratejik olarak dağıtılan konumlarda Microsoft'un küresel ağına doğrudan bağlanma olanağı sağlar. ExpressRoute Direct, ölçekte Active/Active bağlantısını destekleyen çift 100 Gbps veya 10 Gbps bağlantı sağlar.

ExpressRoute Direct'in sağladığı temel özellikler şunlardır, ancak bunlarla sınırlı değildir:

* Depolama ve Cosmos DB gibi hizmetler için Büyük Veri Alımı özelliği
* Düzenlenmiş ve özel ve izole bağlantı gerektiren endüstriler için fiziksel izolasyon: Bankacılık, Devlet ve Perakende
* Bağlantı hattı dağıtımının iş birimine dayalı detaylı denetimi

## <a name="onboard-to-expressroute-direct"></a>ExpressRoute Direct için Onboard

ExpressRoute Direct'i kullanmadan önce aboneliğinizi kaydedebilirsiniz. Kaydolmak için, <ExpressRouteDirect@microsoft.com> aşağıdaki ayrıntılar da dahil olmak üzere abonelik kimliğinizle birlikte bir E-posta gönderin:

* **ExpressRoute Direct** ile gerçekleştirmek istediğiniz senaryolar
* Konum tercihleri - Tüm konumların tam listesi için [Ortaklar ve bakan konumlarına](expressroute-locations-providers.md) bakın
* Uygulama için zaman çizelgesi
* Diğer sorularınız

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>Bir servis sağlayıcısı ve ExpressRoute Direct kullanarak ExpressRoute

| **Servis sağlayıcı kullanarak ExpressRoute** | **ExpressRoute Direct** | 
| --- | --- |
| Mevcut altyapıya hızlı biniş ve bağlantı sağlamak için hizmet sağlayıcılardan yararlanıyor | 100 Gbps/10 Gbps altyapı ve tüm katmanların tam yönetimi gerektirir
| Ethernet ve MPLS dahil olmak üzere yüzlerce sağlayıcıyla tümleşir | Düzenlenmiş endüstriler ve büyük veri alımı için doğrudan/Özel kapasite |
| 50 Mbps'den 10 Gbps'ye Kadar Devresi SUs'ları | Müşteri 100 Gbps ExpressRoute Direct'te aşağıdaki devre SUS'larının bir kombinasyonunu seçebilir: <ul><li>5 Gbps</li><li>10 Gbps</li><li>40 Gbps</li><li>100 Gb/sn</li></ul> Müşteri 10 Gbps ExpressRoute Direct'te aşağıdaki devre SUS'larının bir kombinasyonunu seçebilir:<ul><li>1 Gbps</li><li>2 Gbps</li><li>5 Gbps</li><li>10 Gbps</li></ul>
| Tek kiracı için optimize edilsin | Birden fazla iş birimi ve birden fazla çalışma ortamı na sahip tek kiracı için optimize edilmiştir

## <a name="expressroute-direct-circuits"></a>ExpressRoute Doğrudan devreleri

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute ile Microsoft Azure ve Office 365 gibi Microsoft bulut hizmetlerine bağlantılar kurabilirsiniz.

Her bir bakan konumu Microsoft'un küresel ağına erişebilir ve varsayılan olarak jeopolitik bölgedeki herhangi bir bölgeye erişebilir ve premium devreyle tüm küresel bölgelere erişebilir.  

Çoğu senaryodaki işlevsellik, çalışması için expressroute servis sağlayıcısı kullanan devrelere eşdeğerdir. ExpressRoute Direct kullanılarak sunulan daha ayrıntılı lığı ve yeni yetenekleri desteklemek için, ExpressRoute Direct Circuits'de belirli temel özellikler vardır.

## <a name="circuit-skus"></a>Devre SKUs

ExpressRoute Direct, Azure depolama ve diğer büyük veri hizmetlerine yönelik büyük veri alma senaryolarını destekler. 100 Gbps ExpressRoute Direct'teki ExpressRoute devreleri artık **40 Gbps** ve **100 Gbps** devre SUs'u da destekliyor. Fiziksel bağlantı noktası çiftleri yalnızca **100 veya 10 Gbps'dir** ve birden çok sanal devreye sahip olabilir. Devre boyutları:

| **100 Gbps ExpressRoute Doğrudan** | **10 Gbps ExpressRoute Doğrudan** | 
| --- | --- |
| **Abone Bant Genişliği**: 200 Gbps | **Abone Bant Genişliği**: 20 Gbps |
| <ul><li>5 Gbps</li><li>10 Gbps</li><li>40 Gbps</li><li>100 Gb/sn</li></ul> | <ul><li>1 Gbps</li><li>2 Gbps</li><li>5 Gbps</li><li>10 Gbps</li></ul>

## <a name="technical-requirements"></a>Teknik Gereksinimler

* Microsoft Enterprise Edge Router (MSEE) Arabirimleri:
    * Çift 10 veya 100 Gigabit Ethernet bağlantı noktası yalnızca yönlendirici çifti arasında
    * Tek ModlR Fiber bağlantı
    * IPv4 ve IPv6
    * IP MTU 1500 bayt

* Anahtar/Yönlendirici Katmanı 2/Katman 3 Bağlantı:
    * 1 802.1Q (Dot1Q) etiketini veya iki Tag 802.1Q (QinQ) etiket kapsüllemasını desteklemelidir
    * Etertype = 0x8100
    * Microsoft tarafından belirtilen VLAN KIMLIĞIne dayalı dış VLAN etiketini (STAG) eklemeniz gerekir - *yalnızca QinQ'da geçerlidir*
    * Bağlantı noktası ve cihaz başına birden fazla BGP oturumlarını (VLAN) desteklemeli
    * IPv4 ve IPv6 bağlantısı. *IPv6 için ek alt arabirim oluşturulmaz. IPv6 adresi varolan alt arabirime eklenecektir.* 
    * İsteğe bağlı: ExpressRoute devrelerinde varsayılan olarak yapılandırılan [Çift Yönlü Yönlendirme Algılama (BFD)](https://docs.microsoft.com/azure/expressroute/expressroute-bfd) desteği

## <a name="vlan-tagging"></a>VLAN Etiketleme

ExpressRoute Direct hem QinQ hem de Dot1Q VLAN etiketlemesini destekler.

* **QinQ VLAN Etiketleme,** ExpressRoute devresi bazında yalıtılmış yönlendirme etki alanlarını sağlar. Azure devre oluşturmada dinamik olarak bir S-Tag ayırır ve değiştirilemez. Devre (Özel ve Microsoft) üzerinde her bakan VLAN olarak benzersiz bir C-Tag kullanır. C-Tag ExpressRoute Direct bağlantı noktalarındadevreler arasında benzersiz olması gerekmez.

* **Dot1Q VLAN Etiketleme,** ExpressRoute Direct bağlantı noktası çifti bazında tek bir etiketli VLAN'a izin verir. Bir eşlemede kullanılan C-Tag, ExpressRoute Direct bağlantı noktası çiftindeki tüm devreler ve görünümler arasında benzersiz olmalıdır.

## <a name="workflow"></a>İş akışı

[![iş akışı](./media/expressroute-erdirect-about/workflow1.png)](./media/expressroute-erdirect-about/workflow1.png#lightbox)

## <a name="sla"></a>SLA

ExpressRoute Direct, Microsoft Global Network'e Etkin/Etkin yedek bağlantıları yla aynı kurumsal sınıf SLA'yı sağlar. ExpressRoute altyapısı gereksizdir ve Microsoft Global Network'e bağlantı gereksizdir ve çeşitlidir ve müşteri gereksinimlerine göre ölçeklenir. 

## <a name="next-steps"></a>Sonraki adımlar

[ExpressRoute Direct'i yapılandırma](expressroute-howto-erdirect.md)
