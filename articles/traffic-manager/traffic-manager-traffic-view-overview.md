---
title: Azure Trafik Yöneticisi'nde Trafik Görünümü
description: Bu girişte, Trafik yöneticisi Trafik görünümünün nasıl çalıştığını öğrenin.
services: traffic-manager
documentationcenter: traffic-manager
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: rohink
ms.custom: ''
ms.openlocfilehash: d5a03fde564b14baee97f50fa63fd58bf83694b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938350"
---
# <a name="traffic-manager-traffic-view"></a>Trafik Yöneticisi Trafik Görünümü

Trafik Yöneticisi, profili oluşturduğunuzda belirtilen yönlendirme yöntemine göre son kullanıcılarınızın sağlıklı uç noktalara yönlendirilmesi için DNS düzeyi yönlendirmesağlar. Trafik Görünümü, Trafik Yöneticisi'ne kullanıcı üslerinizin (DNS çözümleyici parçalı) ve bunların trafik deseni görünümünü sağlar. Trafik Görünümü'ni etkinleştirdiğinizde, bu bilgiler size işlem uygulanabilir öngörüler sağlamak için işlenir. 

Trafik Görünümü'ni kullanarak şunları yapabilirsiniz:
- kullanıcı üslerinizin nerede bulunduğunu (yerel Bir DNS çözümleyici düzeyine kadar parçalı) anlayın.
- bu bölgelerden kaynaklanan trafik hacmini (Azure Trafik Yöneticisi tarafından işlenen DNS sorguları olarak gözlenen) görüntüleyin.
- bu kullanıcıların yaşadığı temsili gecikme nin ne olduğuna dair bilgiler edinin.
- bu kullanıcı tabanlarının her birinden bitiş noktalarının olduğu Azure bölgelerine kadar belirli trafik desenlerine derin denin. 

Örneğin, hangi bölgelerin çok sayıda trafiğe sahip olduğunu ancak daha yüksek gecikme sürelerinden muzdarip olduğunu anlamak için Trafik Görünümü'ni kullanabilirsiniz. Ardından, bu bilgileri, bu kullanıcıların daha düşük bir gecikme sonu deneyimi yaşamaları için yeni Azure bölgelerine ayak izi genişletmenizi planlamak için kullanabilirsiniz.

## <a name="how-traffic-view-works"></a>Trafik Görünümü nasıl çalışır?

Trafik Görünümü, Trafik Yöneticisi'nin bu özelliği etkin olan bir profile karşı son yedi gün içinde alınan gelen sorguları görüntülemesini sağlayarak çalışır. Gelen sorgu bilgilerinden Traffic View, kullanıcıların konumunun gösterimi olarak kullanılan DNS çözümleyicisinin kaynak IP'sini ayıklar. Bunlar daha sonra Trafik Yöneticisi tarafından tutulan IP adreslerinin coğrafi bilgilerini kullanarak kullanıcı tabanı bölgeleri oluşturmak için DNS çözümleyici düzeyinde bir dizi nde gruplandırılır. Trafik Yöneticisi daha sonra sorgunun yönlendirildiği Azure bölgelerine bakar ve bu bölgelerdeki kullanıcılar için bir trafik akışı eşlemi oluşturur.  
Bir sonraki adımda, Trafik Yöneticisi, kullanıcı tabanı bölgesini Azure bölgesi eşlemesiyle, bu bölgelerdeki kullanıcıların yaşadığı ortalama gecikme süresini anlamak için farklı son kullanıcı ağları için koruduğu ağ zekası gecikme tablolarıyla ilişkilendirir Azure bölgelerine bağlanma. Tüm bu hesaplamalar, size sunulmadan önce yerel DNS çözümleyicisi IP düzeyinde birleştirilir. Bilgileri çeşitli şekillerde tüketebilirsiniz.

Trafik görünümü veri güncelleştirmesinin sıklığı birden çok dahili hizmet değişkenine bağlıdır. Ancak, veriler genellikle her 24 saatte bir güncelleştirilir.

>[!NOTE]
>Traffic View'da açıklanan gecikme sonu, son kullanıcı ile bağlı oldukları Azure bölgeleri arasında temsili bir gecikme dir ve DNS arama gecikmesi değildir. Trafik Görünümü, yerel DNS çözümleyicisi ile sorgunun yönlendirilen Azure bölgesi arasındaki gecikme gecikmesi hakkında en iyi çabayı gösterir, kullanılabilir veri yoksa döndürülen gecikme null olacaktır. 

## <a name="visual-overview"></a>Görsel bakış

Trafik Yöneticisi sayfanızdaki **Trafik Görünümü** bölümüne gidince, Trafik Görünümü istatistiklerinin bir bindirmesini içeren bir coğrafi harita sunulur. Harita, Trafik Yöneticisi profiliniz için kullanıcı tabanı ve uç noktaları hakkında bilgi sağlar.

![Trafik Yöneticisi Trafik Görünümü Coğrafi görünüm][1]

### <a name="user-base-information"></a>Kullanıcı temel bilgileri

Konum bilgilerinin bulunduğu yerel DNS çözümleyicileri için haritada gösterilirler. DNS çözümleyicisinin rengi, Trafik Yöneticisi sorguları için bu DNS çözümleyicisini kullanan son kullanıcıların yaşadığı ortalama gecikmeyi gösterir.

Haritada bir DNS çözümleyici konumunun üzerinde gezinirseniz, şunları gösterir:
- DNS çözümleyicisinin IP adresi
- Trafik Müdürü tarafından görülen DNS sorgu trafiğinin hacmi
- bitiş noktası ile DNS çözümleyicisi arasındaki bir çizgi olarak DNS çözümleyicisinden gelen trafiğin yönlendirildiği uç noktalar 
- onları bağlayan hattın rengi olarak temsil edilen, o konumdan bitiş noktasına kadar olan ortalama gecikme

### <a name="endpoint-information"></a>Bitiş noktası bilgileri

Uç noktaların bulunduğu Azure bölgeleri haritada mavi noktalar olarak gösterilir. Bitiş noktanız dış noktaysa ve üzerinde eşlenmiş bir Azure bölgesi yoksa, haritanın üst kısmında gösterilir. Trafiğin o bitiş noktasına yönlendirildiği farklı konumları (kullanılan DNS çözümleyicisine göre) görmek için herhangi bir bitiş noktasına tıklayın. Bağlantılar bitiş noktası ile DNS çözümleyici konumu arasında bir çizgi olarak gösterilir ve bu çift arasındaki temsili gecikme ye göre renklenir. Ayrıca, bitiş noktasının adını, çalıştığı Azure bölgesini ve bu Trafik Yöneticisi profili tarafından ona yönlendirilen isteklerin toplam hacmini görebilirsiniz.


## <a name="tabular-listing-and-raw-data-download"></a>Tabular liste ve ham veri indir

Trafik Görünümü verilerini Azure portalında bir tabular biçiminde görüntüleyebilirsiniz. Her DNS çözümleyicisi IP / uç nokta çifti için, DNS çözümleyicisinin IP adresini, bitiş noktasının bulunduğu Azure bölgesinin adını ve coğrafi konumunu (varsa), bu DNS çözümleyicisiyle ilişkili isteklerin hacmini ve bu DNS'yi kullanan son kullanıcılarla ilişkili temsilci gecikmesini (varsa) gösteren bir giriş vardır. Trafik Görünümü verilerini, seçtiğiniz bir analitik iş akışının bir parçası olarak kullanılabilecek bir CSV dosyası olarak da indirebilirsiniz.

## <a name="billing"></a>Faturalandırma

Trafik Görünümü'ni kullandığınızda, sunulan öngörüleri oluşturmak için kullanılan veri noktalarının sayısına göre faturalandırılırsınız. Şu anda, kullanılan tek veri noktası türü Trafik Yöneticisi profilinize karşı alınan sorgulardır. Fiyatlandırma hakkında daha fazla bilgi için [Trafik Yöneticisi fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/traffic-manager/)ziyaret edin.

## <a name="faqs"></a>SSS

* [Trafik Görünümü ne işe yarar?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-does-traffic-view-do)

* [Trafik Görünümü'nden nasıl yararlanabilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-benefit-from-using-traffic-view)

* [Trafik Görünümü, Azure monitörü aracılığıyla kullanılabilen Trafik Yöneticisi ölçümlerinden nasıl farklı?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-is-traffic-view-different-from-the-traffic-manager-metrics-available-through-azure-monitor)

* [Trafik Görünümü EDNS İstemci Alt net bilgilerini kullanıyor mu?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-traffic-view-use-edns-client-subnet-information)

* [Trafik Görünümü kaç günlük veri kullanır?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-many-days-of-data-does-traffic-view-use)

* [Trafik Görünümü dış uç noktaları nasıl işler?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-does-traffic-view-handle-external-endpoints)

* [Aboneliğimdeki her profil için Trafik Görünümü'ni etkinleştirmem gerekiyor mu?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#do-i-need-to-enable-traffic-view-for-each-profile-in-my-subscription)

* [Trafik Görünümü'ni nasıl kapatabilirim?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-turn-off-traffic-view)

* [Trafik Görünümü faturalandırma nasıl çalışır?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-does-traffic-view-billing-work)

## <a name="next-steps"></a>Sonraki adımlar

- [Trafik Yöneticisi'nin nasıl çalıştığını](traffic-manager-overview.md) öğrenin
- Trafik Yöneticisi tarafından desteklenen [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) hakkında daha fazla bilgi edinin
- Trafik Yöneticisi profilini nasıl [oluşturabilirsiniz](traffic-manager-create-profile.md) öğrenin

<!--Image references-->
[1]: ./media/traffic-manager-traffic-view-overview/trafficview.png