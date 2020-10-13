---
title: Azure ön kapı-yönlendirme mimarisi | Microsoft Docs
description: Bu makale, ön kapı mimarisinin genel görünüm oranını anlamanıza yardımcı olur.
services: front-door
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2020
ms.author: duau
ms.openlocfilehash: 948cf3c65dfdc912f2f807dfac34076985f1bc89
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91449199"
---
# <a name="routing-architecture-overview"></a>Yönlendirme mimarisine genel bakış

Azure ön kapısına istemci isteklerinizi aldığında, bu iki şeyin birini yapar. Önbelleğe almayı etkinleştirirseniz veya bunları ters bir ara sunucu olarak uygun uygulama arka ucuna iletirse, bunlara yanıt verir.

## <a name="selecting-the-front-door-environment-for-traffic-routing-anycast"></a><a name = "anycast"></a>Trafik yönlendirme için ön kapı ortamını seçme (her noktaya yayın)

Azure ön kapılarına yönlendirilen trafik, kullanıcı isteklerinin en az ağ atlamalarını en yakın ortama ulaşmasını sağlayan DNS (etki alanı adı sistemi) ve HTTP (Köprü Metni Aktarım Protokolü) trafiği için her iki [noktaya yayın](https://en.wikipedia.org/wiki/Anycast) kullanır. Bu mimari, bölünmüş TCP 'nin avantajlarını en üst düzeye çıkararak son kullanıcılar için daha iyi gidiş dönüş süreleri sunar. Ön kapı, ortamlarını birincil ve geri dönüş "halkaları" olarak düzenler. Dış halkada kullanıcılara daha yakın olan ve daha düşük gecikme süreleri sunan ortamlar vardır.  İç halkada herhangi bir sorun olması durumunda dış halka ortamı için yük devretmeyi işleyebilen ortamlar vardır. Dış halka tüm trafik için tercih edilen hedeftir ve iç halka, dış halkadan gelen trafik taşmasını idare etmek olur. Ön kapıya göre sunulan her bir ön uç ana bilgisayar veya etki alanı, hem iç hem de Dış halkadaki ortamlar tarafından duyurulan bir birincil VIP (sanal Internet Protokolü adresleri) atamış olur. Geri dönüş VIP yalnızca iç halkadaki ortamlar tarafından duyurulur. 

Bu mimari, son kullanıcılarınızın isteklerinin her zaman en yakın ön kapı ortamına ulaşmasını sağlar. Tercih edilen ön kapı ortamı sağlıksız olsa bile tüm trafik otomatik olarak bir sonraki en yakın ortama taşınıyor.

## <a name="connecting-to-front-door-environment-split-tcp"></a><a name = "splittcp"></a>Ön kapı ortamına bağlanma (TCP 'yi Böl)

[Bölünmüş TCP](https://en.wikipedia.org/wiki/Performance-enhancing_proxy) , yüksek gidiş dönüş süresi daha küçük parçalara tabi olacak bir bağlantıyı kopararak gecikme SÜRELERINI ve TCP sorunlarını azaltmaya yönelik bir tekniktir. Ön kapı ortamları son kullanıcılara yaklaşarak, TCP bağlantıları ön kapı ortamının içinde sonlanır. Uygulama arka ucuna büyük gidiş dönüş süresi (RTT) olan bir TCP bağlantısı iki ayrı bağlantıya ayrılır. Son Kullanıcı ile ön kapı ortamı arasındaki "kısa bağlantı" bağlantısı, bağlantının, gecikme süresine neden olan üç uzun gidiş dönüş yerine üç kısa gidiş dönüş üzerinden kurulduğu anlamına gelir. Ön kapı ortamı ile arka uç arasındaki "uzun bağlantı" önceden kurulabilir ve daha sonra diğer son kullanıcılar üzerinde yeniden kullanılabilir ve bağlantı süresini kaydeder. Bir bağlantıyı güvenli hale getirmeye yönelik daha fazla gidiş dönüş olduğundan, bölünmüş TCP 'nin etkisi bir SSL/TLS (Aktarım Katmanı Güvenliği) bağlantısı kurulurken çarpılır.

## <a name="processing-request-to-match-a-routing-rule"></a>Yönlendirme kuralıyla eşleşen istek işleniyor
Bir bağlantı kurulduktan ve bir TLS anlaşmasını tamamladıktan sonra, bir ön kapılı ortamda bir isteğin yer aldığı ilk adım, yönlendirme kuralıyla eşleşmedir. Eşleştirme, ön kapıda yapılan, istekle eşleşen belirli bir yönlendirme kuralına yönelik yapılandırmalara göre belirlenir. Ön kapıda daha fazla bilgi edinmek için [eşleştirmeyi nasıl yönlendiren](front-door-route-matching.md) hakkında bilgi edinin.

## <a name="identifying-available-backends-in-the-backend-pool-for-the-routing-rule"></a>Yönlendirme kuralı için arka uç havuzunda kullanılabilir arka uçları tanımlama
Ön kapıda gelen istek için bir yönlendirme kuralıyla eşleştirildiği sıradaki adım, önbelleğe alma işlemi yoksa, yönlendirme kuralıyla ilişkili arka uç havuzu için sistem durumu araştırma durumu alma durumundadır. Daha fazla bilgi edinmek için [sistem durumu araştırmalarını](front-door-health-probes.md) kullanarak arka uç durumunun nasıl izlediği hakkında bilgi edinin.

## <a name="forwarding-the-request-to-your-application-backend"></a>İsteği uygulamanızın arka ucuna iletme
Son olarak, önbelleğe almanın yapılandırılmadığını varsayarsak, Kullanıcı isteği [yönlendirme yöntemi](front-door-routing-methods.md) yapılandırmanıza göre "en iyi" arka uca iletilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
