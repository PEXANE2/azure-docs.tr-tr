---
title: Azure Ön Kapı - yönlendirme mimarisi | Microsoft Dokümanlar
description: Bu makale, Front Door mimarisinin küresel görünüm yönünü anlamanıza yardımcı olur.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: fd1f06bcb92ea97e0e9e9a6eefeac957031575a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471566"
---
# <a name="routing-architecture-overview"></a>Yönlendirme mimarisine genel bakış

Azure Ön Kapı, istemci isteklerinizi aldığında bunlara yanıt verir (önbelleğe alma etkinse) veya uygun uygulama arka ucuna (ters proxy olarak) iletir.

</br>Azure Ön Kapı'ya yönlendirmenin yanı sıra arka uçlara yönlendirme yaparken trafiği en iyi duruma getirmek için fırsatlar vardır.

## <a name="selecting-the-front-door-environment-for-traffic-routing-anycast"></a><a name = "anycast"></a>Trafik yönlendirmesi için Ön Kapı ortamının seçilmesi (Anycast)

Azure Ön Kapı ortamlarına yönlendirme, [Anycast'ı](https://en.wikipedia.org/wiki/Anycast) hem DNS (Etki Alanı Adı Sistemi) hem de HTTP (Hypertext Transfer Protocol) trafiği için kullandığından, kullanıcı trafiği ağ topolojisi (en az atlama) açısından en yakın ortama gider. Bu mimari genellikle son kullanıcılar için daha iyi gidiş-dönüş süreleri sunar (Split TCP'nin avantajlarını en üst düzeye çıkarır). Ön Kapı birincil ve geri dönüş "halkalar" içine ortamlarını düzenler.  Dış halka, kullanıcılara daha yakın ortamlara sahiptir ve daha düşük gecikmeler sunar.  İç halka, bir sorun olması durumunda dış halka ortamının arızasını kaldırabilecek ortamlara sahiptir. Dış halka tüm trafik için tercih edilen hedeftir, ancak iç halka dış halkadan trafik taşma işlemek için gereklidir. VIP'ler (Sanal İnternet Protokolü adresleri) açısından, frontend ana bilgisayar veya Front Door tarafından sunulan etki alanı, hem iç hem de dış halkadaki ortamlar tarafından duyurulan birincil VIP'nin yanı sıra yalnızca ortamlar tarafından duyurulan bir geri dönüş VIP'si olarak atanır. iç halka. 

</br>Bu genel strateji, son kullanıcılarınızdan gelen isteklerin her zaman en yakın Ön Kapı ortamına ulaşmasını ve tercih edilen Ön Kapı ortamı sağlıksız olsa bile trafiğin otomatik olarak bir sonraki en yakın ortama taşınmasını sağlar.

## <a name="connecting-to-front-door-environment-split-tcp"></a><a name = "splittcp"></a>Ön Kapı ortamına bağlanma (Split TCP)

[Split TCP,](https://en.wikipedia.org/wiki/Performance-enhancing_proxy) yüksek gidiş-dönüş süresine daha küçük parçalara ayrılacak bir bağlantıyı kırarak gecikme sürelerini ve TCP sorunlarını azaltan bir tekniktir.  Ön Kapı ortamlarını son kullanıcılara yaklaştırarak ve TCP bağlantılarını Ön Kapı ortamı içinde sonlandırmak, uygulama arka ucuna büyük bir gidiş-dönüş süresine (RTT) sahip bir TCP bağlantısı iki TCP bağlantısına bölünür. Son kullanıcı ile Ön Kapı ortamı arasındaki kısa bağlantı, bağlantının üç uzun gidiş-dönüş yerine üç kısa gidiş dönüş üzerinden kurulduğu ve gecikmeden tasarruf ettiği anlamına gelir.  Ön Kapı ortamı ile arka uç arasındaki uzun bağlantı önceden kurulabilir ve birden çok son kullanıcı araması arasında yeniden kullanılabilir ve yine TCP bağlantı süresini kurtarır.  Bağlantıyı güvenli hale getirmek için daha fazla gidiş-dönüş yolculuk olduğu için, bir SSL/TLS (Transport Layer Security) bağlantısı kurarken etki çarpılır.

## <a name="processing-request-to-match-a-routing-rule"></a>Yönlendirme kuralını eşleştirmek için işleme isteği
Bir bağlantı kurduktan ve SSL el sıkışması yaptıktan sonra, bir istek Ön Kapı ortamına düştüğünde, yönlendirme kuralını eşleştirmek ilk adımdır. Bu maç temelde Ön Kapı, hangi özel yönlendirme kuralı isteği maç için tüm yapılandırmaları belirlemedir. Front Door'un daha fazla bilgi edinmek için [rota eşleştirmesini](front-door-route-matching.md) nasıl yaptığı hakkında bilgi edinin.

## <a name="identifying-available-backends-in-the-backend-pool-for-the-routing-rule"></a>Yönlendirme kuralı için arka uç havuzunda kullanılabilir arka uçları tanımlama
Ön Kapı gelen isteği temel alan bir yönlendirme kuralı için bir eşleşme olduğunda ve önbelleğe alma yoksa, bir sonraki adım eşleşen rota ile ilişkili arka uç havuzu için sağlık sondası durumunu çekmektir. Daha fazla bilgi edinmek için [Sağlık Sondaları](front-door-health-probes.md) kullanarak Front Door'un arka uç durumunu nasıl izlediğini okuyun.

## <a name="forwarding-the-request-to-your-application-backend"></a>İsteğini uygulama arka uçunuza iletme
Son olarak, yapılandırılmış önbelleğe alma olmadığını varsayarsak, kullanıcı isteği Ön [Kapı yönlendirme yöntemi](front-door-routing-methods.md) yapılandırmanıza göre "en iyi" arka uca iletilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
