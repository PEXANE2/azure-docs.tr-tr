---
title: Azure Ön Kapı - Uygulama katmanı güvenliği | Microsoft Dokümanlar
description: Bu makale, Azure Ön Kapı'nın uygulama arka uçlarınızı nasıl koruduğuve güvenli hale getirebildiğini anlamanıza yardımcı olur
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: e458926930c1b95d48886559551878fc6c9d0673
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471804"
---
# <a name="application-layer-security-with-front-door"></a>Ön Kapılı uygulama katmanı güvenliği
Azure Ön Kapı, web uygulamalarınızı ağ saldırılarına ve SQL Injection veya Cross Site Scripting (XSS) gibi yaygın web güvenlik açıklarından korumak için web uygulama koruma özelliği sağlar. Ön uçlar için etkinleştirilen Front Door'un uygulama katmanı güvenliği, azure'un ağ kenarında, arka uçlarınızdan uzakta, kötü amaçlı saldırıları durdurarak genel olarak dağıtılır ve her zaman açıktır. Daha fazla güvenlik ve performans optimizasyonu ile Ön Kapı, son kullanıcılarınıza hızlı ve güvenli web deneyimleri sunar.

## <a name="application-protection"></a>Uygulama koruması
Front Door'un uygulama koruması, dünyanın her bir kenar ortamında, uygulamalara uygun olarak yapılandırılır ve otomatik olarak web uygulamalarınıza ulaşmasını engelleyen http(ler) olmayan trafiği engeller. Çok kiracılı dağıtılmış mimarimiz, performanstan ödün vermeden ölçekte küresel koruma sağlar. Http(ler) iş yükleri için, Front Door'un web uygulama koruma hizmeti özel kurallar için zengin bir kural altyapısı, yaygın saldırılara karşı önceden yapılandırılmış kural kümesi ve bir kuralla eşleşen tüm istekler için ayrıntılı günlük sağlar. Yalnızca izin verme, engelleme veya günlük gibi esnek eylemler desteklenir.

## <a name="custom-access-control-rules"></a>Özel erişim denetim kuralları
- **IP izin listesi ve blok listesi:** İstemci IP adresleri listesine göre web uygulamalarınıza erişimi denetlemek için özel kurallar yapılandırabilirsiniz. Hem IP v4 hem de IP v6 desteklenir
- **Coğrafi tabanlı erişim kontrolü:** Web uygulamalarınız için erişimi bir istemci IP'nin geldiği ülke koduna göre denetlemek için özel kurallar yapılandırabilirsiniz
- **HTTP parametreleri filtreleme:** Üstbilgi, URL ve sorgu dizeleri de dahil olmak üzere http(ler) istek parametrelerini eşleştirerek özel erişim kurallarını yapılandırabilirsiniz

## <a name="azure-managed-rules"></a>Azure tarafından yönetilen kurallar
- Genel olarak üst Düzey OWASP güvenlik açıklarına karşı önceden yapılandırılmış bir kural kümesi varsayılan olarak etkinleştirilir. Önizlemede, kurallar kümesi sqli ve xss isteklerini denetlemeyi içerir. Ek kurallar eklenecektir. Önceden yapılandırılmış kuralların uygulamalarınız için beklendiği gibi çalıştığını doğrulamak için yalnızca günlük eylemiyle başlamayı seçebilirsiniz 

## <a name="rate-limiting"></a>Hız sınırlaması
- Bir oran denetimi kuralı herhangi bir istemci IP anormal yüksek trafik sınırlamaktır.  Bir dakikalık süre boyunca istemci IP tarafından izin verilen web istekleri sayısına bir eşik belirleyebilirsiniz.

## <a name="centralized-protection-policy"></a>Merkezi koruma politikası
- Birkaç koruma kuralı tanımlayabilir ve bunları öncelik sırasına göre bir İlke'ye ekleyebilirsiniz. Özel kurallar, özel durumlara izin vermek için yönetilen kural kümesinden daha yüksek önceliğe sahiptir. Tek bir ilke web uygulamanızla ilişkilidir.  Aynı web uygulaması koruma ilkesi tüm konumlardaki tüm kenar sunucularına çoğaltılır, tüm bölgelerde tutarlı güvenlik ilkesi sağlar

## <a name="configuration"></a>Yapılandırma
- Önizleme sırasında, Front Door'un uygulama koruma kurallarını ve ilkelerini oluşturmak ve dağıtmak için REST API'lerini, PowerShell'i veya CLI'yi kullanabilirsiniz. Hizmet genel olarak kullanılabilir olmadan önce portal erişimi desteklenir. 


## <a name="monitoring"></a>İzleme
Front Door, uyarıları izlemek ve eğilimleri kolayca izlemek için Azure Monitor ile entegre edilmiş gerçek zamanlı ölçümler kullanarak saldırılara karşı web uygulamalarını izleme olanağı sağlar.

## <a name="pricing"></a>Fiyatlandırma
Ön Kapı'nın uygulama katmanı güvenliği önizleme sırasında ücretsizdir.


## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
