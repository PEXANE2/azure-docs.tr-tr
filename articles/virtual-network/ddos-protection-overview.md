---
title: Azure DDoS Koruma Standardına Genel Bakış
description: Azure DDoS Koruması hizmeti hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/22/2020
ms.author: kumud
ms.openlocfilehash: f1dd33425a57689974fc98a28724adf7b130ab40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536352"
---
# <a name="azure-ddos-protection-standard-overview"></a>Azure DDoS Koruma Standardına genel bakış

Dağıtılmış hizmet engelleme (DDoS) saldırıları, uygulamalarını buluta taşıyan müşterilerin karşılaştığı en büyük kullanılabilirlik ve güvenlik sorunlarından biridir. Bir DDoS saldırısı, uygulamanın kaynaklarını tüketerek uygulamayı yasal kullanıcılar için kullanılamaz hale getirir. DDoS saldırıları internet üzerinden genel olarak erişilebilen herhangi bir uç noktasını hedefleyebilir.

Azure DDoS koruması, uygulama tasarımının en iyi uygulamalarıyla birleştiğinde, DDoS saldırılarına karşı savunma sağlar. Azure DDoS koruması aşağıdaki hizmet katmanları sağlar:

- **Temel**: Azure platformunun bir parçası olarak otomatik olarak etkinleştirildi. Her zaman açık trafik izleme ve ortak ağ düzeyinde saldırıların gerçek zamanlı azaltma, Microsoft'un çevrimiçi hizmetleri tarafından kullanılan aynı savunma sağlar.Azure'un küresel ağının tüm ölçeği, bölgeler genelinde saldırı trafiğini dağıtmak ve azaltmak için kullanılabilir.IPv4 ve IPv6 Azure [genel IP adresleri](virtual-network-public-ip-address.md)için koruma sağlanır.
- **Standart**: Temel hizmet katmanı üzerinde özellikle Azure Sanal Ağ kaynaklarına ayarlı ek azaltma özellikleri sağlar. DDoS Koruma Standardı'nı etkinleştirmek kolaydır ve uygulama değişikliği gerektirmez. Koruma ilkeleri adanmış trafik izleme ve makine öğrenimi algoritmalarıyla düzenlenir. İlkeler, Azure Yük Bakiyesi, Azure Uygulama Ağıt ağı ve Azure Hizmet Dokusu örnekleri gibi sanal ağlarda dağıtılan kaynaklarla ilişkili genel IP adreslerine uygulanır, ancak bu koruma Uygulama Hizmeti Ortamları için geçerli değildir.Gerçek zamanlı telemetri, bir saldırı sırasında Azure Monitor görünümleri aracılığıyla ve tarih için kullanılabilir. Zengin saldırı azaltma analitiği tanılama ayarları üzerinden kullanılabilir. Uygulama katmanı [koruması, Azure Uygulama Ağ Geçidi Web Uygulaması Güvenlik Duvarı](../application-gateway//application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) aracılığıyla veya Azure Marketi'nden üçüncü taraf güvenlik duvarı yükleyerek eklenebilir. IPv4 ve IPv6 Azure [genel IP adresleri](virtual-network-public-ip-address.md)için koruma sağlanır.

|Özellik                                         |DDoS Koruma Temel                 |DDoS Koruma Standardı                      |
|------------------------------------------------|--------------------------------------|----------------------------------------------|
|Aktif trafik izleme & her zaman algılama |Evet                                   |Evet                                           |
|Otomatik saldırı azaltıcı etkenleri                    |Evet                                   |Evet                                           |
|Kullanılabilirlik garantisi                          |Azure Bölgesi                          |Uygulama                                   |
|Azaltma ilkeleri                             |Azure trafik bölgesi hacmi için ayarlanmış |Uygulama trafiği hacmi için ayarlanmış          |
|Ölçümler & uyarıları                                |Hayır                                    |Azure monitörü üzerinden tanılama günlüklerini & gerçek zamanlı saldırı ölçümleri                                 |
|Azaltma raporları                              |Hayır                                    |Saldırı azaltma raporları sonrası                |
|Azaltma akış günlükleri                            |Hayır                                    |SIEM entegrasyonu için NRT günlük akışı           |
|Azaltma ilkesi özelleştirme                 |Hayır                                    |DDoS Uzmanlarını Devreye Almak                           |
|Destek                                         |En iyi çaba                           |Etkin bir saldırı sırasında DDoS Uzmanlarına erişim|
|SLA                                             |Azure Bölgesi                          |Maliyet koruması & uygulama garantisi       |
|Fiyatlandırma                                         |Ücretsiz                                  |Aylık & kullanıma dayalı                         |

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>DDoS Koruma Standardının azaltan DDoS saldırı türleri

DDoS Koruma Standardı aşağıdaki saldırı türlerini azaltabilir:

- **Hacimsel saldırılar**: Saldırının amacı, ağ katmanını önemli miktarda meşru görünen bir trafikle doldurmaktır. Bu UDP sel, amplifikasyon sel ve diğer sahte paket sel içerir. DDoS Koruma Standardı, Azure'un küresel ağ ölçeğiyle bu olası çok gigabaytlı saldırıları otomatik olarak özümseyerek ve temizleyerek azaltır.
- **Protokol saldırıları**: Bu saldırılar, katman 3 ve katman 4 protokol yığınındaki bir zayıflıktan yararlanarak hedefe erişilemez hale getirir. Syn sel saldırılarını, yansıma saldırılarını ve diğer protokol saldırılarını içerir. DDoS Koruma Standardı, istemciyle etkileşimkurarak ve kötü amaçlı trafiği engelleyerek bu saldırıları azaltır, kötü amaçlı ve yasal trafik arasında ayrım yapar. 
- **Kaynak (uygulama) katmanı saldırıları**: Bu saldırılar, ana bilgisayarlar arasında veri iletimini bozmak için web uygulama paketlerini hedef lekarşılar. Saldırılar, HTTP protokol ihlalleri, SQL enjeksiyonu, site ler arası komut dosyası ekleme ve diğer katman 7 saldırılarını içerir. Bu saldırılara karşı savunma sağlamak için Azure [Application Gateway web uygulaması güvenlik duvarı](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)ve DDoS Koruma Standardı gibi bir Web Uygulaması Güvenlik Duvarı kullanın. [Azure](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall)Marketi'nde üçüncü taraf web uygulaması güvenlik duvarı teklifleri de mevcuttur.

DDoS Koruma Standardı, sanal makinelerle ilişkili genel IP adresleri, yük dengeleyicileri ve uygulama ağ geçitleri de dahil olmak üzere sanal ağdaki kaynakları korur. Uygulama Ağ Geçidi web uygulaması güvenlik duvarı veya ortak bir IP ile sanal ağda dağıtılan bir üçüncü taraf web uygulaması güvenlik duvarı ile birleştiğinde, DDoS Koruma Standardı tam katman 3 katman 7 azaltma yeteneği sağlayabilir.

## <a name="ddos-protection-standard-features"></a>DDoS Koruma Standardı özellikleri

![DDoS işlevselliği](./media/ddos-protection-overview/ddosfeatures.png)

DDoS Koruma Standardı özellikleri şunlardır:

- **Yerel platform entegrasyonu:** Azure'a yerel olarak entegre edilmiştir. Azure portalı üzerinden yapılandırma içerir. DDoS Protection Standard kaynaklarınızı ve kaynak yapılandırmanızı anlar.
- **Anahtar teslim koruması:** Basitleştirilmiş yapılandırma, DDoS Koruma Standardı etkinleştirilir etkinleştirilir etkinleştirmez sanal ağdaki tüm kaynakları hemen korur. Hiçbir müdahale veya kullanıcı tanımı gereklidir. DDoS Koruma Standardı, algılandıktan sonra saldırıyı anında ve otomatik olarak azaltır.
- **Her zaman trafiğe karşı izleme:** Uygulama trafik desenleri, DDoS saldırılarının göstergeleri için haftanın 7 günü, günde 24 saat izlenir. Koruma ilkeleri aşıldığında azaltma gerçekleştirilir.
- **Uyarlanabilir atokalma:** Akıllı trafik profiloluşturma, uygulamanızın trafiğini zaman içinde öğrenir ve hizmetiniz için en uygun profili seçer ve günceller. Trafik zaman içinde değiştikçe profil ayarlanır.
- **Çok Katmanlı koruma:** Bir web uygulaması güvenlik duvarı yla kullanıldığında tam yığın DDoS koruması sağlar.
- **Kapsamlı azaltma ölçeği:** Bilinen en büyük DDoS saldırılarına karşı korumak için küresel kapasiteye sahip 60'tan fazla farklı saldırı türü azaltılabilir.
- **Saldırı analizi:** Bir saldırı sırasında beş dakikalık artışlarla ayrıntılı raporlar ve saldırı sona erdikten sonra tam bir özet alın. Bir saldırı sırasında neredeyse gerçek zamanlı izleme için çevrimdışı güvenlik bilgileri ve olay yönetimi (SIEM) sistemine akış azaltma akışı günlükleri.
- **Saldırı ölçümleri:** Her saldırının özetlenmiş ölçümlerine Azure Monitor üzerinden erişilebilir.
- **Saldırı uyarısı:** Uyarılar, bir saldırının başlangıcında ve sonunda ve saldırının süresi boyunca yerleşik saldırı ölçümleri kullanılarak yapılandırılabilir. Uyarılar, Microsoft Azure Monitor günlükleri, Splunk, Azure Depolama, E-posta ve Azure portalı gibi operasyonel yazılımınıza entegre olur.
- **Maliyet garantisi:** Belgelenmiş DDoS saldırıları için veri aktarımı ve uygulama ölçeklendirme hizmet kredileri.

## <a name="ddos-protection-standard-mitigation"></a>DDoS Koruma Standardı azaltma

DDoS Koruma Standardı gerçek trafik kullanımını izler ve sürekli olarak DDoS İlkesi'nde tanımlanan eşiklerle karşılaştırır. Trafik eşiği aşıldığında, DDoS azaltma işlemi otomatik olarak başlatılır. Trafik eşiğin altında döndüğünde, azaltma kaldırılır.

![Risk azaltma](./media/ddos-protection-overview/mitigation.png)

Azaltma sırasında, korunan kaynağa gönderilen trafik DDoS koruma hizmeti tarafından yeniden yönlendirilir ve aşağıdaki denetimler gibi çeşitli denetimler gerçekleştirilir:

- Paketlerin internet özelliklerine uygun olduğundan ve biçimsiz olmadığından emin olun.
- Trafiğin sahte bir paket olup olmadığını belirlemek için istemciyle etkileşimkurun (örn: SYN Auth veya SYN Cookie veya kaynağın yeniden iletmesi için bir paket bırakarak).
- Başka zorlama yöntemi uygulanamazsa, hız sınırı paketleri.

DDoS koruması saldırı trafiğini engeller ve kalan trafiği amaçlanan hedefe gönderir. Saldırı tespit edildikten itibaren birkaç dakika içinde Azure İzleyici ölçümleriyle bilgilendirme yapılır. DDoS Koruma Standardı telemetri üzerinde günlük yapılandırarak, gelecekteki analiz için kullanılabilir seçenekleriçin günlükleri yazabilirsiniz. DDoS Koruma Standardı için Azure Monitor'daki metrik veriler 30 gün boyunca saklanır.

Microsoft, simülasyonlar için DDoS Koruması etkin leştirilmiş genel IP adreslerine karşı trafik oluşturabileceğiniz bir arayüz oluşturmak için [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) ile ortaklık yaptı. BreakPoint Cloud simülasyonu şunları yapmanızı sağlar:

- Microsoft Azure DDoS Koruma Standardı'nın Azure kaynaklarınızı DDoS saldırılarına karşı nasıl koruduğunun doğrulanması
- DDoS saldırısı altındayken olay yanıt sürecinizi optimize edin
- Belge DDoS uyumluluğu
- Ağ güvenlik ekiplerinizi eğitin

## <a name="next-steps"></a>Sonraki adımlar

- [DDoS Koruma Standardına Yapılandır](manage-ddos-protection.md)
