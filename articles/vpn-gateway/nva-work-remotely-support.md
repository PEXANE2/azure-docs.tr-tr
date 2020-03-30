---
title: 'Uzaktan çalışma: Ağ Sanal Cihaz (NVA) uzaktan çalışma için hususlar | Azure VPN Ağ Geçidi'
description: Bu makale, COVID-19 salgını sırasında Azure'daki Ağ Sanal Cihazları (NVAs) ile çalışırken göz önünde bulundurmanız gereken şeyleri anlamanıza yardımcı olur.
services: vpn-gateway
author: scottnap
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/21/2020
ms.author: scottnap
ms.openlocfilehash: 8a22e8f0bde2d13b4055566d96680fadc2db6e11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337102"
---
# <a name="working-remotely-network-virtual-appliance-nva-considerations-for-remote-work"></a>Uzaktan çalışma: Ağ Sanal Cihaz (NVA) uzak çalışma için hususlar

>[!NOTE]
>Bu makalede, COVID-19 krizi nedeniyle karşılaştığınız ağ sorunlarını uzaktan çalışmak ve ağ sorunlarını azaltmak için Ağ Sanal Cihazları, Azure, Microsoft ağı ve Azure iş ortağı ekosisteminden nasıl yararlanabileceğiniz açıklanmaktadır.
>

Bazı Azure müşterileri, COVID-19 salgını sırasında evden çalışan çalışanları için siteden noktaya VPN gibi kritik hizmetler sağlamak için Azure Marketi'nden üçüncü taraf Ağ Sanal Cihazları 'nı (NV'ler) kullanır. Bu makalede, uzaktan erişim çözümleri sağlamak için Azure'da NV'ler kullanılırken dikkate alınması gereken bazı üst düzey kılavuzlar sıralanmaktadır.

## <a name="nva-performance-considerations"></a>NVA performans hususları

Azure Marketi'ndeki tüm büyük NVA satıcılarının VM Boyutu ve çözümlerini dağıtırken kullanılacak örnek sayısı hakkında önerileri olmalıdır.  Neredeyse tüm NVA satıcıları belirli bir Bölgede kullanabileceğiniz herhangi bir boyutu seçmenize izin verirken, bu öneriler satıcının performans yaptığı VM boyutları olduğundan, Azure VM örnek boyutları için satıcı önerilerini izlemeniz çok önemlidir Azure ile test edin.  

### <a name="consider-the-following"></a>Aşağıdakileri göz önünde bulundurun

- **Kapasite ve eşzamanlı kullanıcı sayısı** - Her bağlı kullanıcı bir şifreli (IPSec veya SSL VPN) tünel oluşturacak gibi bu sayı özellikle Point-to-Site VPN kullanıcıları için önemlidir.  
- **Toplam iş ortası** - Uzaktan erişim sağlamanız gereken kullanıcı sayısını karşılamak için ihtiyacınız olan toplam bant genişliği nedir.
- **İhtiyacınız olacak VM boyutu** - Her zaman NVA satıcı tarafından önerilen VM boyutları kullanmalısınız.  Noktadan siteye VPN için, çok eşzamanlı kullanıcı bağlantıları olacaksa, [Dv2 ve DSv2 serisi](https://docs.microsoft.com/azure/virtual-machines/dv2-dsv2-series "Dv2 ve Dsv2 Serisi") VM'ler gibi daha büyük VM boyutlarını kullanıyor olmalısınız. Bu VMs daha fazla vCPUs sahip olma eğilimindedir ve daha eşzamanlı VPN oturumları işleyebilir.  Azure'daki daha büyük VM boyutları, daha fazla sanal çekiye sahip olmanın yanı sıra, daha küçük VM boyutlarına göre daha fazla toplam bant genişliği kapasitesine sahiptir.
    > **Önemli:** Her satıcı kaynakları farklı kullanır.  Tahmini kullanıcı yükünüzü karşılamak için hangi örnek boyutlarını kullanmanız gerektiği açık değilse, doğrudan yazılım satıcısına başvurmalı ve onlardan bir öneri istemeniz gerekir.
- **Örnek sayısı** - Çok sayıda kullanıcı ve bağlantı olmasını bekliyorsanız, NVA örnek boyutlarınızın ölçekletilmesinin neler başarabileceğinin sınırları vardır.  Birden çok VM örneğini dağıtmayı düşünün.
- **IPSec VPN vs SSL VPN** - Genel olarak IPSec VPN uygulamaları SSL VPN uygulamalarından daha iyi performans gösterir.  
- **Lisanslama** - NVA çözümü için satın aldığınız yazılım lisanslarının COVID-19 salgını sırasında karşılaşabileceğiniz ani büyümeyi kapsadığından emin olun.  Birçok NVA lisans programı, çözümün yapabildiği bağlantı veya bant genişliği sayısını sınırlar.
- **Hızlandırılmış Ağ** - Hızlandırılmış Ağ için desteği olan bir NVA çözümdüşünün.  Hızlandırılmış ağ, tek kök G/Ç sanallaştırmasını (SR-IOV) VM'ye sağlayarak ağ performansını büyük ölçüde artırır. Bu yüksek performanslı yol, desteklenen VM türlerinde en zorlu ağ iş yükleriyle kullanım için gecikme, gerginlik ve CPU kullanımını azaltarak ana bilgisayarı veri yolundan atlar. Hızlandırılmış ağ, en genel amaç ve bilgi işlem için optimize edilmiş örnek boyutlarında iki veya daha fazla vCPU ile desteklenir.

## <a name="monitoring-resources"></a>Kaynakların izlenmesi

Her NVA çözümünün NVA performansını izlemek için kendi araçları ve kaynakları vardır.  Performans sınırlamalarını anladığınızdan ve NVA'nızın kapasiteye yaklaştığını veya kapasiteye ulaştığını tespit edebileceğinizden emin olmak için satıcılarınızın belgelerine başvurun.  Buna ek olarak Azure Monitor Ağ İstatistikleri'ne bakabilir ve Ağ Sanal Cihazlarınız hakkındaki temel performans bilgilerini görebilirsiniz:

- CPU Kullanımı
- Ağ Girişi
- Ağ Çıkışı
- Gelen Akışlar
- Giden Akışlar

## <a name="next-steps"></a>Sonraki Adımlar

En büyük NVA ortakları COVID-19 sırasında ani, beklenmedik büyüme için ölçekleme etrafında rehberlik gönderdik. İş ortağı kaynaklarına birkaç yararlı bağlantı aşağıda verebistir.

[Barracuda COVID-19 sırasında verilerinizi güvence altına alırken evden çalışmayı etkinleştirin](https://www.barracuda.com/covid-19/work-from-home "COVID-19 sırasında verilerinizi güvence altına alırken Evden İş'i etkinleştirin")

[COVID-19 Hazırlığı için Cisco AnyConnect Uygulama ve Performans/Ölçekleme Referansı](https://www.cisco.com/c/en/us/support/docs/security/anyconnect-secure-mobility-client/215331-anyconnect-implementation-and-performanc.html "COVID-19 Hazırlığı için Cisco AnyConnect Uygulama ve Performans/Ölçekleme Referansı")

[Citrix COVID-19 Mukabele Destek Merkezi](https://www.citrix.com/support/covid-19-coronavirus.html "Citrix COVID-19 Mukabele Destek Merkezi")

[F5 Rehberlik Uzak Çalışanlarda Dramatik Artış Adrese](https://www.f5.com/business-continuity "F5 Rehberlik Uzak Çalışanlarda Dramatik Artış Adrese")

[Fortinet COVID-19 Müşteriler ve İş Ortakları için Güncellemeler](https://www.fortinet.com/covid-19.html "Müşteriler ve İş Ortakları için COVID-19 Güncellemeleri")

[Palo Alto Networks COVID-19 Yanıt Merkezi](https://live.paloaltonetworks.com/t5/COVID-19-Response-Center/ct-p/COVID-19_Response_Center "Palo Alto Networks COVID-19 Yanıt Merkezi")
