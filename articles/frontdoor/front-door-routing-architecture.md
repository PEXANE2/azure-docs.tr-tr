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
ms.date: 09/10/2018
ms.author: duau
ms.openlocfilehash: b36852e27f6aa3a909dd645c19a12c55e082b761
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89399336"
---
# <a name="routing-architecture-overview"></a>Yönlendirme mimarisine genel bakış

İstemci isteklerinizi aldığında Azure ön kapısı, bunlara yanıt verir (önbelleğe alma etkinse) ya da bunları uygun uygulama arka ucuna (ters proxy olarak) iletir.

</br>Azure ön kapısına yönlendirirken ve arka uçlara yönlendirirken trafiği iyileştirmek için fırsatlar vardır.

## <a name="selecting-the-front-door-environment-for-traffic-routing-anycast"></a><a name = "anycast"></a>Trafik yönlendirme için ön kapı ortamını seçme (her noktaya yayın)

Azure ön kapılarına yönlendirme hem DNS (etki alanı adı sistemi) hem de HTTP (Köprü Metni Aktarım Protokolü) trafiği için her iki [noktaya](https://en.wikipedia.org/wiki/Anycast) de yararlanır. bu nedenle, Kullanıcı trafiği ağ topolojisi (en az atlama) açısından en yakın ortama gider. Bu mimari genellikle son kullanıcılar için daha iyi gidiş dönüş süreleri sunar (bölünmüş TCP 'nin avantajlarını en üst düzeye çıkarır). Ön kapı, ortamlarını birincil ve geri dönüş "halkaları" olarak düzenler.  Dış halkada kullanıcılara daha yakın olan ve daha düşük gecikme süreleri sunan ortamlar vardır.  İç halkada bir sorun olması durumunda dış halka ortamı için yük devretmeyi işleyebilen ortamlar vardır. Dış halka tüm trafik için tercih edilen hedeftir, ancak dış halkadan gelen trafik taşmasını işlemek için iç halka gereklidir. VIP 'ler (sanal Internet Protokolü adresleri) açısından, her ön uç ana bilgisayar veya ön kapıya ait etki alanı, hem iç hem de Dış halkadaki ortamlar tarafından duyurulacak birincil VIP ve yalnızca iç halkadaki ortamlar tarafından duyurulan bir geri dönüş VIP 'si atanır. 

</br>Bu genel strateji, son kullanıcılarınızın her zaman en yakın ön kapı ortamına ulaşmasını sağlar ve tercih edilen ön kapı ortamı sağlıksız olsa bile trafik otomatik olarak bir sonraki en yakın ortama gider.

## <a name="connecting-to-front-door-environment-split-tcp"></a><a name = "splittcp"></a>Ön kapı ortamına bağlanma (TCP 'yi Böl)

[Bölünmüş TCP](https://en.wikipedia.org/wiki/Performance-enhancing_proxy) , yüksek gidiş dönüş süresi daha küçük parçalara tabi olacak bir bağlantıyı kopararak gecikme SÜRELERINI ve TCP sorunlarını azaltmaya yönelik bir tekniktir.  Ön kapı ortamlarını son kullanıcılara yaklaştırarak ve TCP bağlantılarını ön kapı ortamında sonlandırarak, büyük gidiş dönüş süresi (RTT) ile uygulama arka ucuna kadar bir TCP bağlantısı iki TCP bağlantısına bölünür. Son Kullanıcı ile ön kapı ortamı arasındaki kısa bağlantı, bağlantının üç uzun gidiş dönüş yerine üç kısa gidiş dönüşlere göre kurulması, gecikme süresinin kaydedilmesi anlamına gelir.  Ön kapı ortamı ile arka uç arasındaki uzun bağlantı önceden kurulabilir ve birden çok Son Kullanıcı çağrısı boyunca yeniden kullanılabilir ve TCP bağlantı süresini kaydedebilir.  Bağlantıyı güvenli hale getirmeye yönelik daha fazla gidiş dönüş olduğu için, bir SSL/TLS (Aktarım Katmanı Güvenliği) bağlantısı kurulurken etki çarpılır.

## <a name="processing-request-to-match-a-routing-rule"></a>Yönlendirme kuralıyla eşleşen istek işleniyor
Bir bağlantı kurulduktan ve bir TLS anlaşması gerçekleştirdikten sonra, bir istek ön kapılı ortama eklendiğinde bir yönlendirme kuralıyla eşleşen ilk adımdır. Bu eşleştirme temelde, ön kapıdaki tüm yapılandırmalardan, istekle eşleşecek belirli yönlendirme kuralına göre belirlenir. Ön kapıda daha fazla bilgi edinmek için [eşleştirmeyi nasıl yönlendiren](front-door-route-matching.md) hakkında bilgi edinin.

## <a name="identifying-available-backends-in-the-backend-pool-for-the-routing-rule"></a>Yönlendirme kuralı için arka uç havuzunda kullanılabilir arka uçları tanımlama
Ön kapıda, gelen isteğe bağlı olarak bir yönlendirme kuralı eşleşmesi varsa ve önbelleğe alma yoksa, sonraki adım eşleşen rotayla ilişkili arka uç havuzu için sistem durumu araştırma durumunu çekmek olacaktır. Daha fazla bilgi edinmek için [sistem durumu araştırmalarını](front-door-health-probes.md) kullanarak arka uç durumunun nasıl izlediği hakkında bilgi edinin.

## <a name="forwarding-the-request-to-your-application-backend"></a>İsteği uygulamanızın arka ucuna iletme
Son olarak, yapılandırılmış önbelleğe alma olmadığı varsayılarak, Kullanıcı isteği [ön kapı yönlendirme yöntemi](front-door-routing-methods.md) yapılandırmanıza göre "en iyi" arka uca iletilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
