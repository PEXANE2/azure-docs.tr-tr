---
title: Azure Trafik Yöneticisi için performans hususları | Microsoft Dokümanlar
description: Trafik Yöneticisi'ndeki performansı ve Trafik Yöneticisi'ni kullanırken web sitenizin performansını nasıl test edileni anlayın
services: traffic-manager
documentationcenter: ''
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: rohink
ms.openlocfilehash: 84367a00643c48e7fe2fb7f907bab64589193b2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938535"
---
# <a name="performance-considerations-for-traffic-manager"></a>Traffic Manager için performans konuları

Bu sayfa, Trafik Yöneticisi kullanarak performans hususları açıklar. Şu senaryoyu göz önünde bulundurun:

Web sitenizin WestUS ve EastAsia bölgelerinde örnekleri var. Örneklerden biri trafik yöneticisi sondası için sağlık kontrolünde başarısız oluyor. Uygulama trafiği sağlıklı bölgeye yönlendirilir. Bu başarısız lık beklenmektedir, ancak performans, uzak bir bölgeye seyahat eden trafiğin gecikmesine bağlı olarak sorun olabilir.

## <a name="performance-considerations-for-traffic-manager"></a>Traffic Manager için performans konuları

Trafik Yöneticisi'nin web sitenizde sahip olabileceği tek performans etkisi ilk DNS aramasIdır. Trafik Yöneticisi profilinizin adı için bir DNS isteği, trafficmanager.net bölgesini barındıran Microsoft DNS kök sunucusu tarafından işlenir. Trafik Yöneticisi, Trafik Yöneticisi ilkesini ve sonda sonuçlarına göre Microsoft'un DNS kök sunucularını doldurur ve düzenli olarak güncelleştirir. Bu nedenle, ilk DNS araması sırasında bile Trafik Yöneticisi'ne DNS sorgusu gönderilmez.

Trafik Yöneticisi birkaç bileşenden oluşur: DNS ad sunucuları, bir API hizmeti, depolama katmanı ve bir uç nokta izleme hizmeti. Trafik Yöneticisi hizmet bileşeni başarısız olursa, Trafik Yöneticisi profilinizle ilişkili DNS adı üzerinde hiçbir etkisi yoktur. Microsoft DNS sunucularında kayıtlar değişmeden kalır. Ancak, uç nokta izleme ve DNS güncelleştirmesi gerçekleşmez. Bu nedenle, Trafik Yöneticisi birincil siteniz indiğinde başarısız sitenize işaret etmek için DNS'yi güncelleştiremez.

DNS ad çözünürlüğü hızlıdır ve sonuçlar önbelleğe alınır. İlk DNS aramasının hızı, istemcinin ad çözümlemesi için kullandığı DNS sunucularına bağlıdır. Genellikle, bir istemci ~ 50 ms içinde bir DNS arama tamamlayabilirsiniz. Aramanın sonuçları, DNS Time-to-live (TTL) süresi boyunca önbelleğe alınır. Trafik Yöneticisi için varsayılan TTL 300 saniyedir.

Trafik Trafik Yöneticisi üzerinden akmaz. DNS araması tamamlandıktan sonra, istemcinin web sitenizin bir örneği için bir IP adresi vardır. İstemci doğrudan bu adrese bağlanır ve Trafik Yöneticisi'nden geçmez. Seçtiğiniz Trafik Yöneticisi ilkesinin DNS performansı üzerinde hiçbir etkisi yoktur. Ancak, Performans yönlendirme yöntemi uygulama deneyimini olumsuz etkileyebilir. Örneğin, politikanız Kuzey Amerika trafiğini Asya'da barındırılan bir örneğe yönlendirilerse, bu oturumlar için ağ gecikmesi bir performans sorunu olabilir.

## <a name="measuring-traffic-manager-performance"></a>Trafik Yöneticisi Performansının Ölçülmesi

Trafik Yöneticisi profilinin performansını ve davranışını anlamak için kullanabileceğiniz birkaç web sitesi vardır. Bu sitelerin çoğu ücretsizdir, ancak sınırlamaları olabilir. Bazı siteler ücret karşılığında gelişmiş izleme ve raporlama sunar.

Bu sitelerdeki araçlar DNS gecikmelerini ölçer ve dünyanın dört bir yanındaki istemci konumları için çözülmüş IP adreslerini görüntüler. Bu araçların çoğu DNS sonuçlarını önbelleğe vermez. Bu nedenle, araçlar bir test çalıştırılsa tam DNS aramasını gösterir. Kendi istemcinizden test ettiğinizde, TTL süresi boyunca tam DNS arama performansını yalnızca bir kez yaşarsınız.

## <a name="sample-tools-to-measure-dns-performance"></a>DNS performansını ölçmek için örnek araçlar

* [Çözüldü](https://www.solvedns.com/dns-comparison/)

    SolveDNS birçok performans aracı sunar. DNS Karşılaştırma aracı, DNS adınızı çözmenin ne kadar sürdüğünü ve bunun diğer DNS hizmet sağlayıcılarıyla karşılaştırıldığında ne kadar sürdüğünü gösterebilir.

* [Websitepulse](https://www.websitepulse.com/help/tools.php)

    En basit araçlardan biri WebSitePulse olduğunu. DNS çözümleme süresini, First Bayte'i, Son Bayt'ı ve diğer performans istatistiklerini görmek için URL'yi girin. Üç farklı test yeri arasından seçim yapabilirsiniz. Bu örnekte, ilk yürütmenin DNS aramasının 0,204 sn aldığını gösterdiğini görürsünüz.

    ![nabız1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Sonuçlar önbelleğe alındıklarından, aynı Trafik Yöneticisi bitiş noktası DNS araması için ikinci test 0,002 sn alır.

    ![nabız2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

* [CA App Sentetik Monitör](https://asm.ca.com/en/checkit.php)

    Eskiden Watch-mouse Denetim Web Sitesi aracı olarak bilinen bu site, aynı anda birden fazla coğrafi bölgeden DNS çözünürlük süresini gösterir. DNS çözünürlük süresini, bağlantı süresini ve hızı çeşitli coğrafi konumlardan görmek için URL'yi girin. Hangi barındırılan hizmetin dünyanın farklı konumları için döndürülddöndüğünü görmek için bu testi kullanın.

    ![nabız1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

* [Pingdom](https://tools.pingdom.com/)

    Bu araç, bir web sayfasının her öğesi için performans istatistikleri sağlar. Sayfa Çözümlemesi sekmesi, DNS araması için harcanan zaman yüzdesini gösterir.

* [DNS'im nedir?](https://www.whatsmydns.net/)

    Bu site 20 farklı konumdan bir DNS araması yapar ve sonuçları bir harita üzerinde görüntüler.

* [Web Arabirimi Kaz](https://www.digwebinterface.com)

    Bu site, CNAMEs ve A kayıtları da dahil olmak üzere daha ayrıntılı DNS bilgileri gösterir. Seçenekler altında 'Verimi Renklendir' ve 'İstatistikleri' kontrol ettiğinizden ve Nameservers altında 'Tümü'nü seçtiğinizden emin olun.

## <a name="next-steps"></a>Sonraki Adımlar

[Trafik Yöneticisi trafik yönlendirme yöntemleri hakkında](traffic-manager-routing-methods.md)

[Traffic Manager ayarlarınızı test etme](traffic-manager-testing-settings.md)

[Traffic Manager üzerindeki işlemler (REST API Başvurusu)](https://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Trafik Yöneticisi Cmdlets](https://docs.microsoft.com/powershell/module/az.trafficmanager)

