---
title: 'Uzaktan çalışma: uzaktan çalışma için ağ sanal gereci (NVA) konuları | Azure VPN Gateway'
description: Bu makale, COVıN-19 pandemıc sırasında Azure 'da ağ sanal gereçlerinde (NVA 'lar) çalışmayı dikkate almanız gereken şeyleri anlamanıza yardımcı olur.
services: vpn-gateway
author: scottnap
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/21/2020
ms.author: scottnap
ms.openlocfilehash: 8a22e8f0bde2d13b4055566d96680fadc2db6e11
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80337102"
---
# <a name="working-remotely-network-virtual-appliance-nva-considerations-for-remote-work"></a>Uzaktan çalışma: uzak çalışma için ağ sanal gereci (NVA) konuları

>[!NOTE]
>Bu makalede, ağ sanal gereçlerinin, Azure 'un, Microsoft ağının ve Azure iş ortağı ekosisteminden yararlanarak uzaktan nasıl çalışabileceğinizi ve COVı-19 çapraz olduğu için kullanıma sunulacak ağ sorunlarını azaltabilirsiniz.
>

Bazı Azure müşterileri, COVıD-19 epidemıc sırasında evden çalışan çalışanları için Noktadan siteye VPN gibi kritik hizmetler sağlamak üzere Azure Marketi 'nden üçüncü taraf ağ sanal gereçlerini (NVA 'lar) kullanır. Bu makalede, uzaktan erişim çözümleri sağlamak üzere Azure 'da NVA 'lar 'i kullanırken dikkate alınması gereken bazı üst düzey rehberlik özetlenmektedir.

## <a name="nva-performance-considerations"></a>NVA performansı konuları

Azure Marketi 'ndeki tüm büyük NVA satıcıları, çözümlerini dağıttığınızda kullanılacak VM boyutu ve örnek sayısı üzerinde önerilere sahip olmalıdır.  Neredeyse tüm NVA satıcıları belirli bir bölgede sizin için kullanılabilir olan herhangi bir boyutu seçmenize izin verince, bu öneriler, satıcının Azure 'da performans testini gerçekleştirmesini sağlayan sanal makine boyutları olduğundan, Azure VM örnek boyutları için satıcı önerilerini izlemeniz çok önemlidir.  

### <a name="consider-the-following"></a>Aşağıdakileri göz önünde bulundurun

- **Eş zamanlı kullanıcıların kapasitesi ve sayısı** -bu sayı özellikle Noktadan siteye VPN kullanıcıları için, her bağlantılı Kullanıcı bir şifrelenmiş (ıPSEC veya SSL VPN) tüneli oluşturacak şekilde önemlidir.  
- **Toplam aktarım hızı** -uzaktan erişim sağlamanız gereken kullanıcı sayısını karşılamak için gereken toplam bant genişliğidir.
- **İhtiyaç duyduğunuz VM boyutu** -NVA satıcısı tarafından önerilen VM boyutlarını her zaman kullanmanız gerekir.  Noktadan siteye VPN için, çok sayıda eşzamanlı kullanıcı bağlantınız varsa, [dv2 ve DSv2 serisi](https://docs.microsoft.com/azure/virtual-machines/dv2-dsv2-series "Dv2 ve Dsv2 serisi") VM 'ler gibi daha büyük VM boyutlarını kullanmanız gerekir. Bu VM 'Ler daha fazla vCPU 'ya sahip olmaya eğilimlidir ve daha fazla eşzamanlı VPN oturumu işleyebilir.  Daha fazla sanal çekirdeğe sahip olmanın yanı sıra, Azure 'da daha büyük VM boyutları daha küçük VM boyutundan daha fazla toplam bant genişliği kapasitesine sahiptir.
    > **Önemli:** Her satıcı, kaynakları farklı kullanır.  Tahmin edilen Kullanıcı yüküne uyum sağlamak için kullanmanız gereken örnek boyutları net değilse, yazılım satıcısıyla doğrudan iletişim kurmanız ve önerisine danışmanız gerekir.
- **Örnek sayısı** -çok sayıda kullanıcı ve bağlantınız olması beklendiğinde, NVA örnek boyutlarınızın ne kadar ölçeklendirdiğini elde etmeniz için sınırlamalar vardır.  Birden çok VM örneği dağıtmak isteyebilirsiniz.
- **IPSEC VPN vs SSL VPN** -genel IPSec VPN UYGULAMALARıNDA, SSL VPN uygulamalarından daha iyi bir uygulama gerçekleştirir.  
- **Lisanslama** -NVA çözümü için satın aldığınız yazılım lisanslarının covıd-19 epidemıc sırasında karşılaşabileceğiniz ani büyümeyi kapsadığına emin olun.  Birçok NVA lisanslama programı, çözümün sahip olduğu bağlantı sayısını veya bant genişliğini sınırlar.
- **Hızlandırılmış ağ** -hızlandırılmış ağ desteği olan bir NVA çözümü düşünün.  Hızlandırılmış ağ, bir VM 'ye tek köklü g/ç Sanallaştırması (SR-ıOV) sağlar ve ağ performansını büyük ölçüde geliştirir. Bu yüksek performanslı yol, desteklenen VM türlerindeki en zorlu ağ iş yükleri ile kullanım için gecikme süresi, değişim ve CPU kullanımını azaltan veri yolundan Konağı atlar. Hızlandırılmış ağ, iki veya daha fazla vCPU ile en genel amaçlı ve işlem için iyileştirilmiş örnek boyutlarında desteklenir.

## <a name="monitoring-resources"></a>İzleme kaynakları

Her NVA çözümünün, NVA 'nın performansını izlemeye yönelik kendi araçları ve kaynakları vardır.  Performans sınırlamalarını anladığınızdan ve NVA 'nın kapasiteye yaklaşmakta veya bu kapasiteyi öğrendiğinizden emin olmak için satıcı belgelerinize başvurun.  Buna ek olarak, Azure Izleyici ağ öngörülerine bakabilir ve aşağıdaki gibi ağ sanal Geremleriniz hakkında temel performans bilgilerini görebilirsiniz:

- CPU Kullanımı
- Ağ Girişi
- Ağ Çıkışı
- Gelen Akışlar
- Giden akışlar

## <a name="next-steps"></a>Sonraki Adımlar

Çoğu büyük NVA iş ortakları, COVıD-19 sırasında ani, beklenmeyen büyüme için ölçeklendirmeyle ilgili rehberlik ister. İş ortağı kaynaklarına yönelik birkaç faydalı bağlantı aşağıda verilmiştir.

[Barbcuda, COVıD sırasında verilerinizi güvenli hale getirirken evden çalışmayı etkinleştir-19](https://www.barracuda.com/covid-19/work-from-home "COVıD sırasında verilerinizi güvenli hale getirirken evden çalışmayı etkinleştirme-19")

[COVıD-19 hazırlığı için Cisco AnyConnect uygulama ve performans/ölçeklendirme başvurusu](https://www.cisco.com/c/en/us/support/docs/security/anyconnect-secure-mobility-client/215331-anyconnect-implementation-and-performanc.html "COVıD-19 hazırlığı için Cisco AnyConnect uygulama ve performans/ölçeklendirme başvurusu")

[Citrix COVıD-19 yanıt Destek Merkezi](https://www.citrix.com/support/covid-19-coronavirus.html "Citrix COVıD-19 yanıt Destek Merkezi")

[Uzak çalışanlarında çarpıcı artışı karşılamak için F5 Kılavuzu](https://www.f5.com/business-continuity "Uzak çalışanlarında çarpıcı artışı karşılamak için F5 Kılavuzu")

[Fortinet COVıD-müşteriler ve Iş ortakları için 19 güncelleştirme](https://www.fortinet.com/covid-19.html "COVıD-müşteriler ve Iş ortakları için 19 güncelleştirme")

[Palo Alto Networks COVıD-19 yanıt Merkezi](https://live.paloaltonetworks.com/t5/COVID-19-Response-Center/ct-p/COVID-19_Response_Center "Palo Alto Networks COVıD-19 yanıt Merkezi")
