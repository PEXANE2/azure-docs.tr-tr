---
title: Azure Trafik Yöneticisi ayarlarını doğrula
description: Bu makalede, Trafik Yöneticisi ayarlarınızı nasıl doğrulayacağınızı ve trafik yönlendirme yöntemini nasıl test edebilirsiniz.
services: traffic-manager
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: rohink
ms.openlocfilehash: 94ab5e550f0053fa19b9b93f1d67690211543325
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938393"
---
# <a name="verify-traffic-manager-settings"></a>Traffic Manager ayarlarını doğrulama

Trafik Yöneticisi ayarlarınızı test etmek için, testlerinizi çalıştırabileceğiniz çeşitli konumlarda birden çok istemciye sahip olmanız gerekir. Ardından, Trafik Yöneticisi profilinizdeki uç noktaları teker teker aşağı getirin.

* DNS TTL değerini düşük olarak ayarlayın, böylece değişiklikler hızla yayılır (örneğin, 30 saniye).
* Test ettiğiniz profildeki Azure bulut hizmetlerinizin ve web sitelerinin IP adreslerini bilin.
* Bir Ip adresine DNS adını çözmenize ve bu adresi görüntülemenize izin veren araçlar kullanın.

DNS adlarının profilinizdeki uç noktaların IP adreslerine çözülüp çözülmediğini kontrol esiniz. Adlar, Trafik Yöneticisi profilinde tanımlanan trafik yönlendirme yöntemiyle tutarlı bir şekilde çözülmelidir. DNS adlarını çözmek için **nslookup** veya **kazma** gibi araçları kullanabilirsiniz.

Aşağıdaki örnekler Trafik Yöneticisi profilinizi test yardımcı olur.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Windows'da nslookup ve ipconfig kullanarak Trafik Yöneticisi profilini kontrol edin

1. Yönetici olarak bir komut veya Windows PowerShell istemini açın.
2. DNS çözümleyici önbelleğini temizlemek için yazın. `ipconfig /flushdns`
3. `nslookup <your Traffic Manager domain name>` yazın. Örneğin, aşağıdaki komut etki alanı adını *myapp.contoso* önekiyle denetler

        nslookup myapp.contoso.trafficmanager.net

    Tipik bir sonuç aşağıdaki bilgileri gösterir:

    + Bu Trafik Yöneticisi etki alanı adını çözmek için DNS sunucusunun DNS adı ve IP adresine erişiliyor.
    + "nslookup" sonra komut satırına yazdığınız Trafik Yöneticisi etki alanı adı ve Trafik Yöneticisi etki alanı nın çözümlediği IP adresi. İkinci IP adresi kontrol etmek için önemli biridir. Test ettiğiniz Trafik Yöneticisi profilindeki bulut hizmetlerinden veya web sitelerinden biri için herkese açık sanal IP (VIP) adresiyle eşleşmelidir.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Trafik yönlendirme yöntemi nin başarısız olması nasıl test edilebilir?

1. Tüm uç noktaları yukarı bırakın.
2. Tek bir istemci kullanarak, nslookup veya benzer bir yardımcı program kullanarak şirketinizin etki alanı adı için DNS çözümü isteyin.
3. Çözülmüş IP adresinin birincil bitiş noktasıyla eşleştiğinden emin olun.
4. Trafik Yöneticisi'nin uygulamanın çöktüğını düşünmesi için birincil bitiş noktanızı kaldırın veya izleme dosyasını kaldırın.
5. Trafik Yöneticisi profilinin DNS Time-to-Live (TTL) ve iki dakika daha bekleyin. Örneğin, DNS TTL'niz 300 saniye (5 dakika) ise, yedi dakika beklemeniz gerekir.
6. DNS istemci önbelleğinizi temizle ve nslookup kullanarak DNS çözünürlüğü isteyin. Windows'da, DNS önbelleğinizi ipconfig /flushdns komutuyla temizleyebilirsiniz.
7. Çözülen IP adresinin ikincil bitiş noktanızla eşleştiğinden emin olun.
8. Sırayla her bitiş noktasını aşağı getirerek işlemi tekrarlayın. DNS'nin listedeki bir sonraki bitiş noktasının IP adresini döndürür dedoğrulayın. Tüm uç noktalar çöktüğinde, birincil bitiş noktasının IP adresini yeniden almanız gerekir.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Ağırlıklı trafik yönlendirme yöntemi nasıl test edilir?

1. Tüm uç noktaları yukarı bırakın.
2. Tek bir istemci kullanarak, nslookup veya benzer bir yardımcı program kullanarak şirketinizin etki alanı adı için DNS çözümü isteyin.
3. Çözülen IP adresinin uç noktalarınızdan biriyle eşleştiğinden emin olun.
4. DNS istemci önbelleğinizi temizleyin ve her bitiş noktası için 2 ve 3 adımlarını yineleyin. Uç noktalarınızın her biri için döndürülen farklı IP adresleri görmeniz gerekir.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Performans trafiği yönlendirme yöntemi nasıl test edile

Performans trafiği yönlendirme yöntemini etkili bir şekilde test etmek için, dünyanın farklı yerlerinde müşteriler eve sahip olmalısınız. Hizmetlerinizi sınamak için kullanılabilecek farklı Azure bölgelerinde istemciler oluşturabilirsiniz. Küresel bir ağınız varsa, dünyanın diğer bölgelerindeki istemcilerde uzaktan oturum açabilir ve testlerinizi buradan çalıştırabilirsiniz.

Alternatif olarak, ücretsiz web tabanlı DNS arama ve kazma hizmetleri mevcuttur. Bu araçlardan bazıları, dünyanın çeşitli yerlerinden DNS ad çözümlemesi kontrol etme olanağı sağlar. Örnekler için "DNS araması" üzerinde arama yapın. Gomez veya Keynote gibi üçüncü taraf hizmetleri, profillerinizin trafiği beklendiği gibi dağıttığını doğrulamak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Trafik Yöneticisi trafik yönlendirme yöntemleri hakkında](traffic-manager-routing-methods.md)
* [Traffic Manager için performans konuları](traffic-manager-performance-considerations.md)
* [Traffic Manager düşürülmüş durumu için sorun giderme](traffic-manager-troubleshooting-degraded.md)
