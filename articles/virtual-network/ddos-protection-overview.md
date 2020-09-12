---
title: Azure DDoS koruması standardına genel bakış
description: Azure DDoS koruma standardı 'nın, uygulama tasarımı en iyi uygulamaları ile birlikte kullanıldığında, DDoS saldırılarına karşı savunma olanağı sağladığını öğrenin.
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
ms.openlocfilehash: 90dbc0b888c6d6eeed97a676024abc9cc0dca6fe
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90017175"
---
# <a name="azure-ddos-protection-standard-overview"></a>Azure DDoS koruması standardına genel bakış

Dağıtılmış hizmet engelleme (DDoS) saldırıları, uygulamalarını buluta taşıyan müşterilerin karşılaştığı en büyük kullanılabilirlik ve güvenlik sorunlarından biridir. DDoS saldırısı, uygulamanın kaynaklarını tüketmeye çalışır ve uygulamayı meşru kullanıcılar için kullanılamaz hale getirir. DDoS saldırıları internet üzerinden genel olarak erişilebilen herhangi bir uç noktasını hedefleyebilir.

Azure DDoS koruması, uygulama tasarımı en iyi uygulamaları ile birlikte, DDoS saldırılarına karşı savunma sağlar. Azure 'daki her özellik Azure 'un altyapı DDoS (temel) koruması tarafından korunur.Azure DDoS koruması standardı, Azure sanal ağ kaynaklarına özel olarak ayarlanan temel hizmet katmanı üzerinden ek risk azaltma özellikleri sağlar. 

DDoS koruma standardının etkinleştirilmesi kolaydır ve uygulama değişikliği gerektirmez. Koruma ilkeleri adanmış trafik izleme ve makine öğrenimi algoritmalarıyla düzenlenir. İlkeler, Azure Load Balancer, Azure Application Gateway ve Azure Service Fabric örnekleri gibi sanal ağlarda dağıtılan kaynaklarla ilişkili genel IP adreslerine uygulanır, ancak bu koruma App Service ortamları için uygulanmaz.Gerçek zamanlı telemetri, bir saldırı sırasında Azure Izleyici görünümleri ve geçmiş için kullanılabilir. Tanılama ayarları aracılığıyla zengin saldırı risk azaltma analizlerine ulaşılabilir. Uygulama katmanı koruması, [azure Application Gateway Web uygulaması güvenlik duvarı](../application-gateway//application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) aracılığıyla veya Azure Marketi 'nden üçüncü taraf güvenlik duvarı yükleyerek eklenebilir. IPv4 ve IPv6 Azure [genel IP adresleri](virtual-network-public-ip-address.md)için koruma sağlanır.

![DDoS koruması temel vs standardı](./media/ddos-protection-overview/ddoscomparison.png)

Azure DDoS koruması, müşteri verilerini depolamaz.

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>DDoS koruması standart azaltmalarına sahip DDoS saldırıları türleri

DDoS koruma standardı aşağıdaki tür saldırıları hafifletmenize yardımcı olabilir:

- **Volumetric saldırıları**: Bu saldırılar, ağ katmanını önemli miktarda meşru trafik ile taşmıştır. Bunlar UDP floods, ampmi, floods ve diğer sahte paket floods içerirler. DDoS koruma standardı, bu potansiyel çok gigabaytlık saldırıları, Azure 'un küresel ağ ölçeklendirilmesi ve bunları otomatik olarak yaparak, bu olası çok kiracılı saldırıları azaltır.
- **Protokol saldırıları**: Bu saldırılar, katman 3 ve katman 4 protokol yığınındaki zayıf bir durumu kötüye vererek bir hedefi işlemez. SYN taşma saldırıları, yansıma saldırıları ve diğer protokol saldırıları içerirler. DDoS koruması standardı, bu saldırıları azaltır, istemciyle etkileşime girerek ve kötü amaçlı trafiği engelleyerek kötü amaçlı ve meşru trafik arasında ayrım gerçekleştirebilir. 
- **Kaynak (uygulama) katmanı saldırıları**: Bu saldırılar, ana bilgisayarlar arasında veri aktarımını kesintiye uğratan Web uygulaması paketlerini hedeflemelidir. HTTP protokol ihlalleri, SQL ekleme, siteler arası komut dosyası ve diğer katman 7 saldırıları içerirler. Azure [Application Gateway Web uygulaması güvenlik duvarı](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)gibi bir Web uygulaması güvenlik duvarı ve bu saldırılara karşı savunma sağlamak Için DDoS koruma standardı kullanın. Ayrıca, [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall)'nde bulunan üçüncü taraf Web uygulaması güvenlik duvarı teklifleri de mevcuttur.

DDoS koruma standardı sanal makineler, yük dengeleyiciler ve uygulama ağ geçitleri ile ilişkili genel IP adresleri dahil bir sanal ağdaki kaynakları korur. Application Gateway Web uygulaması güvenlik duvarı ile veya genel IP 'si olan bir sanal ağda dağıtılan bir üçüncü taraf Web uygulaması güvenlik duvarı ile birlikte kullanıldığında, DDoS koruma standardı, katman 7 hafifletme özelliği için tam katman 3 sağlayabilir.

## <a name="ddos-protection-standard-features"></a>DDoS koruması standart özellikleri

![DDoS işlevselliği](./media/ddos-protection-overview/ddosfeatures.png)

DDoS koruması standart özellikleri şunlardır:

- **Yerel platform tümleştirmesi:** Azure ile yerel olarak tümleşiktir. Azure portal aracılığıyla yapılandırmayı içerir. DDoS koruma standardı, kaynaklarınızı ve kaynak yapılandırmanızı anlamıştır.
- **Anahtar koruma:** Basitleştirilmiş yapılandırma, DDoS koruma standardı etkin olduğu anda bir sanal ağ üzerindeki tüm kaynakları hemen korur. Müdahale veya Kullanıcı tanımı gerekli değildir. DDoS koruma standardı, algılandıktan sonra hızla ve otomatik olarak saldırıyı azaltır.
- **Her zaman açık trafik izleme:** Uygulama trafik desenleriniz günde 24 saat, haftada 7 gün, DDoS saldırıları için göstergeler aranıyor. Koruma ilkeleri aşıldığında risk azaltma gerçekleştirilir.
- **Uyarlamalı ayarlama:** Akıllı trafik profili oluşturma, uygulamanızın zaman içindeki trafiğini öğrenir ve hizmetiniz için en uygun profili seçer ve güncelleştirir. Profil zaman içinde trafik değişikliği olarak ayarlanır.
- **Çok katmanlı koruma:** Bir Web uygulaması güvenlik duvarıyla birlikte kullanıldığında, tam yığın koruması sağlar.
- **Kapsamlı risk azaltma ölçeği:** 60 üzerinde farklı saldırı türleri, en büyük bilinen DDoS saldırılarına karşı koruma sağlamak için küresel kapasiteyle birlikte azaltılabilir.
- **Saldırı Analizi:** Saldırı sırasında beş dakikalık artışlarla ayrıntılı raporlar alın ve saldırı bittikten sonra tam bir Özet yapın. Bir saldırı sırasında neredeyse gerçek zamanlı izleme için [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-ddos-protection) 'e veya çevrimdışı güvenlik bilgilerine ve olay yönetimi (SIEM) sistemine yönelik günlük azaltma akışı.
- **Saldırı ölçümleri:** Her bir saldırıya ait özetlenen ölçümler Azure Izleyici aracılığıyla erişilebilir.
- **Saldırı uyarısı:** Uyarılar, yerleşik saldırı ölçümleri kullanılarak saldırı başlangıcında ve durdurulduğunda ve saldırının süresi boyunca yapılandırılabilir. Uyarılar, Microsoft Azure Izleme günlükleri, splunk, Azure depolama, e-posta ve Azure portal gibi işletimsel yazılımınızla tümleştirilir.
- **Maliyet garantisi:** Veri aktarımı ve belgelenmiş DDoS saldırıları için uygulama genişleme hizmeti kredileri.

## <a name="ddos-protection-standard-mitigation"></a>DDoS koruması standart risk azaltma

DDoS koruması standart gerçek trafik kullanımını izler ve DDoS Ilkesinde tanımlanan eşiklere göre sürekli olarak karşılaştırır. Trafik eşiği aşıldığında, DDoS risk azaltma otomatik olarak başlatılır. Trafik eşiğin altına döndüğünde, risk azaltma kaldırılır.

![Risk azaltma](./media/ddos-protection-overview/mitigation.png)

Risk azaltma sırasında, korunan kaynağa gönderilen trafik DDoS koruma hizmeti tarafından yeniden yönlendirilir ve aşağıdaki denetimler gibi çeşitli denetimler gerçekleştirilir:

- Paketlerin internet belirtimlerine uyduğundan ve hatalı biçimlendirilmiş olmadığından emin olun.
- Trafiğin potansiyel olarak sahte bir paket olup olmadığını (ör: SYN auth veya SYN tanımlama bilgisi) ve yeniden aktarmak için kaynak için bir paket bırakarak istemciyle etkileşime geçin.
- Başka bir zorlama yöntemi gerçekleştirilmeyen hız limiti paketleri.

DDoS koruması saldırı trafiğini engeller ve kalan trafiği amaçlanan hedefe gönderir. Saldırı tespit edildikten itibaren birkaç dakika içinde Azure İzleyici ölçümleriyle bilgilendirme yapılır. DDoS koruması standart telemetrisinde günlüğe kaydetmeyi yapılandırarak, gelecekteki analizler için günlükleri kullanılabilir seçeneklere yazabilirsiniz. DDoS koruma standardı için Azure Izleyici 'de ölçüm verileri 30 gün boyunca tutulur.

Microsoft, simülasyon için DDoS koruması etkinleştirilmiş genel IP adreslerine karşı trafik oluşturabileceğiniz bir arabirim oluşturmak üzere [BreakingPoint bulutu](https://www.ixiacom.com/products/breakingpoint-cloud) ile işbirliği yaptı. Kesme noktası bulutu benzetimi şunları yapmanıza olanak sağlar:

- DDoS koruması standardının Microsoft Azure DDoS saldırılarına karşı Azure kaynaklarınızı nasıl koruduğunu doğrulayın
- DDoS saldırısı kapsamındaki olay yanıt sürecinizi iyileştirin
- DDoS uyumluluğunu belgele
- Ağ Güvenlik ekiplerinizi eğitme

## <a name="next-steps"></a>Sonraki adımlar

- [DDoS koruma standardını yapılandırma](manage-ddos-protection.md)
