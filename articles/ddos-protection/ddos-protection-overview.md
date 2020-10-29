---
title: Azure DDoS koruması standardına genel bakış
description: Azure DDoS koruma standardı 'nın, uygulama tasarımı en iyi uygulamaları ile birlikte kullanıldığında, DDoS saldırılarına karşı savunma olanağı sağladığını öğrenin.
services: virtual-network
documentationcenter: na
author: yitoh
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/9/2020
ms.author: yitoh
ms.openlocfilehash: a8d8a41b5d527833ef071275d90cf9102cec47f7
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92905649"
---
# <a name="azure-ddos-protection-standard-overview"></a>Azure DDoS Koruması Standardı’na genel bakış

Dağıtılmış hizmet engelleme (DDoS) saldırıları, uygulamalarını buluta taşıyan müşterilerin karşılaştığı en büyük kullanılabilirlik ve güvenlik sorunlarından biridir. DDoS saldırısı, uygulamanın kaynaklarını tüketmeye çalışır ve uygulamayı meşru kullanıcılar için kullanılamaz hale getirir. DDoS saldırıları internet üzerinden genel olarak erişilebilen herhangi bir uç noktasını hedefleyebilir.

Azure 'daki her özellik hiçbir ek ücret ödemeden Azure 'un altyapı DDoS (temel) koruması tarafından korunur. Küresel olarak dağıtılan Azure ağının ölçeği ve kapasitesi, her zaman açık trafik izleme ve gerçek zamanlı risk azaltma aracılığıyla ortak ağ katmanı saldırılarına karşı savunma sağlar. DDoS koruması temel, Kullanıcı Yapılandırması veya uygulama değişikliği gerektirmez. DDoS koruması temel, Azure DNS gibi PaaS hizmetleri de dahil olmak üzere tüm Azure hizmetlerini korumanıza yardımcı olur.

Azure DDoS koruma standardı, uygulama tasarımı en iyi uygulamaları ile birlikte, DDoS saldırılarına karşı savunmak için gelişmiş DDoS azaltma özellikleri sağlar. Bir sanal ağdaki belirli Azure kaynaklarınızı korumaya yardımcı olmak üzere otomatik olarak ayarlanır. Korumanın, yeni veya mevcut bir sanal ağda etkinleştirilmesi basittir ve uygulama veya kaynak değişikliği gerektirmez. Günlük, uyarı ve telemetri dahil olmak üzere temel hizmet üzerinde çeşitli avantajları vardır. 

![Azure DDoS Koruması hizmeti karşılaştırması](./media/ddos-protection-overview/ddos-comparison.png)

Azure DDoS koruması, müşteri verilerini depolamaz.

## <a name="features"></a>Özellikler

- **Yerel platform tümleştirmesi:** Azure ile yerel olarak tümleşiktir. Azure portal aracılığıyla yapılandırmayı içerir. DDoS koruma standardı, kaynaklarınızı ve kaynak yapılandırmanızı anlamıştır.
- **Anahtar koruma:** Basitleştirilmiş yapılandırma, DDoS koruma standardı etkin olduğu anda bir sanal ağ üzerindeki tüm kaynakları hemen korur. Müdahale veya Kullanıcı tanımı gerekli değildir. DDoS koruma standardı, algılandıktan sonra hızla ve otomatik olarak saldırıyı azaltır.
- **Her zaman açık trafik izleme:** Uygulama trafik desenleriniz günde 24 saat, haftada 7 gün, DDoS saldırıları için göstergeler aranıyor. Koruma ilkeleri aşıldığında risk azaltma gerçekleştirilir.
- **Uyarlamalı ayarlama:** Akıllı trafik profili oluşturma, uygulamanızın zaman içindeki trafiğini öğrenir ve hizmetiniz için en uygun profili seçer ve güncelleştirir. Profil zaman içinde trafik değişikliği olarak ayarlanır.
- **Çok katmanlı koruma:** Bir Web uygulaması güvenlik duvarıyla birlikte kullanıldığında, tam yığın koruması sağlar.
- **Kapsamlı risk azaltma ölçeği:** 60 üzerinde farklı saldırı türleri, en büyük bilinen DDoS saldırılarına karşı koruma sağlamak için küresel kapasiteyle birlikte azaltılabilir.
- **Saldırı Analizi:** Saldırı sırasında beş dakikalık artışlarla ayrıntılı raporlar alın ve saldırı bittikten sonra tam bir Özet yapın. Bir saldırı sırasında neredeyse gerçek zamanlı izleme için [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-ddos-protection) 'e veya çevrimdışı güvenlik bilgilerine ve olay yönetimi (SIEM) sistemine yönelik günlük azaltma akışı.
- **Saldırı ölçümleri:** Her bir saldırıya ait özetlenen ölçümler Azure Izleyici aracılığıyla erişilebilir.
- **Saldırı uyarısı:** Uyarılar, yerleşik saldırı ölçümleri kullanılarak saldırı başlangıcında ve durdurulduğunda ve saldırının süresi boyunca yapılandırılabilir. Uyarılar, Microsoft Azure Izleme günlükleri, splunk, Azure depolama, e-posta ve Azure portal gibi işletimsel yazılımınızla tümleştirilir.
- **DDoS hızlı yanıt** : saldırı araştırması ve analizine yönelik yardım Için DDoS koruması hızlı yanıt (drr) ekibine katılın. Daha fazla bilgi için bkz. [DDoS hızlı yanıt](ddos-rapid-response.md).
- **Maliyet garantisi:** Veri aktarımı ve belgelenmiş DDoS saldırıları için uygulama genişleme hizmeti kredileri.

## <a name="pricing"></a>Fiyatlandırma

Azure DDoS koruması standart fiyatlandırması hakkında bilgi edinmek için bkz. [Azure DDoS koruması standart fiyatlandırması](https://azure.microsoft.com/pricing/details/ddos-protection/).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [DDoS koruma planı oluşturma](manage-ddos-protection.md)
