---
title: Azure ön kapısı-DDoS koruması
description: Bu sayfa, Azure ön kapısının DDoS saldırılarına karşı korumaya nasıl yardımcı olduğu hakkında bilgi sağlar
services: frontdoor
documentationcenter: ''
author: johndowns
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/28/2020
ms.author: jodowns
ms.openlocfilehash: 24a8a559d72eae4dab0b6e740b34890f1d7ff114
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96350792"
---
# <a name="ddos-protection-on-front-door"></a>Ön kapıda DDoS koruması

Azure ön kapısında dağıtılmış hizmet reddi (DDoS) saldırılarını önlemeye yardımcı olabilecek çeşitli özellikler ve özellikler vardır. Bu özellikler, saldırganların uygulamanıza ulaşmasını ve uygulamanızın kullanılabilirliğini ve performansını etkilemesini engelleyebilir.

## <a name="integration-with-azure-ddos-protection-basic"></a>Azure DDoS Protection temel ile tümleştirme

Ön kapı, Azure DDoS Protection temel tarafından korunmaktadır. Ön kapı platformuna varsayılan olarak ve ek bir ücret ödemeden tümleştirilir. Ön kapı küresel olarak dağıtılan ağın tam ölçeği ve kapasitesi, her zaman açık trafik izleme ve gerçek zamanlı risk azaltma aracılığıyla ortak ağ katmanı saldırılarına karşı savunma sağlar. Temel DDoS koruması Ayrıca, genel uç noktaları hedefleyen en yaygın, sık görülen katman 7 DNS sorgu (sel ve katman 3 ve 4 Volumetric saldırılarına karşı savunma sergiler. Bu hizmette ayrıca Microsoft 'un kurumsal ve tüketici hizmetlerini büyük ölçekli saldırılara karşı koruyan kanıtlanmış bir izleme kaydı vardır. Daha fazla bilgi için bkz. [Azure DDoS koruması](../security/fundamentals/ddos-best-practices.md).

## <a name="protocol-blocking"></a>Protokol engelleme

Ön kapı yalnızca HTTP ve HTTPS protokollerinin trafiği kabul eder ve yalnızca bilinen bir üst bilgiyle geçerli istekleri işler `Host` . Bu davranış, bir dizi protokol ve bağlantı noktası, DNS yükseltme saldırısı ve TCP kirletme saldırılarına yayılan Volumetric saldırıları dahil bazı yaygın DDoS saldırı türlerini azaltmaya yardımcı olur.

## <a name="capacity-absorption"></a>Kapasite açıklaması

Ön kapı, genel olarak dağıtılmış, genel olarak dağıtılan bir hizmettir. Her saniyede yüzlerce binlerce istek alan Microsoft 'un büyük ölçekli bulut ürünleri dahil olmak üzere birçok müşteriniz vardır. Ön kapı, Azure 'un ağı, ansorbing ve coğrafi olarak büyük hacimli saldırıları birbirinden ayırarak bulunur. Bu, kötü amaçlı trafiğin Azure ağının kenarından daha fazla ilerlemesine engel olabilir.

## <a name="caching"></a>Önbelleğe Alma

[Ön kapıların önbelleğe alma özellikleri](./front-door-caching.md) , bir saldırı tarafından oluşturulan büyük trafik birimlerinden arka uçları korumak için kullanılabilir. Önbelleğe alınan kaynaklar, arka ucunuza iletilmemesi için ön kapı kenar düğümlerinden döndürülür. Dinamik yanıtlara yönelik kısa önbellek süre sonu süreleri (saniye veya dakika), arka uç hizmetlerindeki yükü büyük ölçüde azaltabilir. Kavramları ve desenleri önbelleğe alma hakkında daha fazla bilgi için bkz. [önbelleğe alma konuları](/azure/architecture/best-practices/caching) ve [önbelleğe alma düzeni](/azure/architecture/patterns/cache-aside).

## <a name="web-application-firewall-waf"></a>Web Uygulaması Güvenlik Duvarı (WAF)

[Ön kapı Web uygulaması güvenlik duvarı (WAF)](../web-application-firewall/afds/afds-overview.md) , farklı saldırı türlerinin sayısını azaltmak için kullanılabilir:

* Yönetilen kural kümesini kullanmak, bir dizi yaygın saldırılara karşı koruma sağlar.
* Tanımlı bir coğrafi bölge veya tanımlı bölge içindeki trafik, Engellenen veya statik bir Web sayfasına yeniden yönlendirilebilir. Daha fazla bilgi için bkz. [coğrafi filtreleme](../web-application-firewall/afds/waf-front-door-geo-filtering.md).
* Kötü amaçlı olarak tanımlayabilen IP adresleri ve aralıkları engellenebilir.
* IP adreslerinin hizmetinizi çok sık aramasını engellemek için hız sınırlaması uygulanabilir.
* Bilinen imzaları olan HTTP veya HTTPS saldırılarını otomatik olarak engellemek ve derecelendirmek için [özel WAF kuralları](../web-application-firewall/afds/waf-front-door-custom-rules.md) oluşturabilirsiniz.

## <a name="for-further-protection"></a>Daha fazla koruma için

Daha fazla korumaya ihtiyacınız varsa arka uçlarınızın dağıtıldığı VNet üzerinde [Azure DDoS koruma standardı](../security/fundamentals/ddos-best-practices.md#ddos-protection-standard) 'nı etkinleştirebilirsiniz. DDoS koruması standart müşterileri, bir saldırı sırasında anında yardım almak için maliyet koruması, SLA garantisi ve DDoS hızlı yanıt ekibinden uzmanlara erişim gibi ek avantajlar sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Bir [WAF profilini ön kapıda](front-door-waf.md)nasıl yapılandıracağınızı öğrenin. 
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.