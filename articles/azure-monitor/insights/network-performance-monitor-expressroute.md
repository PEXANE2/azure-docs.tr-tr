---
title: Azure Günlük Analizi'nde Ağ Performans Monitörü çözümü | Microsoft Dokümanlar
description: Azure ExpressRoute üzerinden şube ofisleriniz ile Azure arasında uça,5 bağlantı ve performansı izlemek için Ağ Performans Monitörü'ndeki ExpressRoute Monitor özelliğini kullanın.
ms.subservice: logs
ms.topic: conceptual
author: abshamsft
ms.author: absha
ms.date: 11/27/2018
ms.openlocfilehash: 6ac610d7dcf9849b6b439741957684867b9d01aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77660759"
---
# <a name="expressroute-monitor"></a>ExpressRoute Monitör

Ağ Performans İzleyicisi'ndeki Azure ExpressRoute [Monitörü](network-performance-monitor.md) özelliğini kullanarak, şubeleriniz ile Azure Arasında Azure ExpressRoute üzerinden uça bağlantı ve performansı izleyebilirsiniz. Önemli avantajları şunlardır: 

- Aboneliğinizle ilişkili ExpressRoute devrelerinin otomatik algılaması.
- ExpressRoute için devrede bant genişliği kullanımı, kaybı ve gecikme sinin, bakmanın ve Azure Sanal Ağ düzeyinin izlenmesi.
- ExpressRoute devrelerinizin ağ topolojisinin keşfi.

![ExpressRoute Monitör](media/network-performance-monitor-expressroute/expressroute-intro.png)

## <a name="configuration"></a>Yapılandırma 
Ağ Performans Monitörü için yapılandırmayı açmak [için, Ağ Performans Monitörü çözümünü](network-performance-monitor.md) açın ve **Yapılandır'ı**seçin.

### <a name="configure-network-security-group-rules"></a>Ağ güvenlik grubu kurallarını yapılandırma 
Azure'da Ağ Performans İzleyicisi üzerinden izleme için kullanılan sunucular için, ağ güvenlik grubu (NSG) kurallarını, Ağ Performans Monitörü tarafından sentetik işlemler için kullanılan bağlantı noktasında TCP trafiğine izin verecek şekilde yapılandırın. Varsayılan bağlantı noktası 8084'dür. Bu yapılandırma, Azure VM'lerine yüklenen Log Analytics aracısının şirket içi bir izleme aracısıyla iletişim kurmasını sağlar. 

NSG'ler hakkında daha fazla bilgi için [Ağ güvenlik gruplarına](../../virtual-network/manage-network-security-group.md)bakın. 

>[!NOTE]
> Bu adıma devam etmeden önce şirket içi sunucu aracısını ve Azure sunucu aracısını yükleyin ve EnableRules.ps1 PowerShell komut dosyasını çalıştırın. 

 
### <a name="discover-expressroute-peering-connections"></a>ExpressRoute bakan bağlantılarını keşfedin 
 
1. **ExpressRoute Peerings** görünümünü seçin.
2. Azure aboneliğindeki sanal ağlara bağlı tüm ExpressRoute özel eşlemelerini bu Azure Log Analytics çalışma alanına bağlı olarak keşfetmek için **Şimdi Keşfet'i** seçin.

    >[!NOTE]
    > Çözüm şu anda yalnızca ExpressRoute özel bakışlarını keşfediyor. 

    >[!NOTE]
    > Yalnızca bu Log Analytics çalışma alanıyla bağlantılı abonelikle ilişkili sanal ağlara bağlı özel eşlemeler keşfedilir. ExpressRoute bu çalışma alanına bağlı abonelik dışındaki sanal ağlara bağlıysa, bu aboneliklerde bir Log Analytics çalışma alanı oluşturun. Ardından, bu bakışları izlemek için Ağ Performans Monitörü'ni kullanın. 

    ![ExpressRoute Monitor yapılandırması](media/network-performance-monitor-expressroute/expressroute-configure.png)
 
   Keşif tamamlandıktan sonra, keşfedilen özel bakan bağlantıları bir tabloda listelenir. Bu eşlemeler için izleme başlangıçta devre dışı bırakılmış bir durumdadır. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>ExpressRoute eşleme bağlantılarının izlenmesini etkinleştirin 

1. İzlemek istediğiniz özel eşleme bağlantısını seçin.
2. Sağdaki bölmede, **bu Eşleme** onay kutusunu İzle'yi seçin. 
3. Bu bağlantı için sistem durumu olayları oluşturmak istiyorsanız, **bu eşleme için Sistem Durumu İzlemesini Etkinleştir'i**seçin. 
4. İzleme koşullarını seçin. Eşik değerlerini girerek sistem durumu etkinliği oluşturma için özel eşikler ayarlayabilirsiniz. Koşulun değeri, eşleme bağlantısı için seçili eşiğinin üzerine çıktığında, bir sistem durumu olayı oluşturulur. 
5. Bu karşılayaçıkan bağlantıyı izlemek için kullanmak istediğiniz izleme aracılarını seçmek için **Aracı ekle'yi** seçin. Bağlantının her iki ucuna da aracı lar eklediğinizden emin olun. Bu eşleme bağlı sanal ağda en az bir aracı gerekir. Ayrıca bu bakışla bağlantılı en az bir şirket içi temsilciye de ihtiyacınız vardır. 
6. Yapılandırmayı kaydetmek için **Kaydet'i** seçin. 

   ![ExpressRoute izleme yapılandırması](media/network-performance-monitor-expressroute/expressroute-configure-discovery.png)


Kuralları etkinleştirdikten ve değerleri ve aracıları seçtikten sonra, değerlerin doldurulması ve **ExpressRoute İzleme** kutucukların görünmesi için 30 ila 60 dakika bekleyin. İzleme kutularını gördüğünüzde, ExpressRoute devreleriniz ve bağlantı kaynaklarınız artık Ağ Performans Monitörü tarafından izlenir. 

>[!NOTE]
> Bu özellik, yeni sorgu diline yükseltilmiş çalışma alanlarında güvenilir bir şekilde çalışır.

## <a name="walkthrough"></a>Kılavuz 

Ağ Performans İzleyicisi panosu, ExpressRoute devrelerinin ve bakan bağlantılarının genel görünümünü gösterir. 

![Ağ Performans Monitörü panosu](media/network-performance-monitor-expressroute/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Devreler listesi 

İzlenen tüm ExpressRoute devrelerinin listesini görmek için ExpressRoute devreleri döşemesini seçin. Bir devre seçebilir ve sistem durumu durumunu, paket kaybı, bant genişliği kullanımı ve gecikme sonu için eğilim grafiklerini görüntüleyebilirsiniz. Grafikler etkileşimli. Grafikleri çizmek için özel bir zaman penceresi seçebilirsiniz. Fareyi, yakınlaştırmak ve ince taneli veri noktalarını görmek için grafikteki bir alanın üzerine sürükleyin. 

![ExpressRoute devreleri listesi](media/network-performance-monitor-expressroute/expressroute-circuits.png) 

### <a name="trends-of-loss-latency-and-throughput"></a>Kayıp eğilimleri, gecikme ve iş artışı 

Bant genişliği kullanımı, gecikme sonu ve kayıp grafikleri etkileşimlidir. Fare denetimlerini kullanarak bu grafiklerin herhangi bir bölümüne yakınlaştırabilirsiniz. Ayrıca, diğer aralıklar için bant genişliği, gecikme ve kayıp verilerini de görebilirsiniz. **Eylemler** düğmesinin altındaki sol üstte **Tarih/Saat'i**seçin. 

![ExpressRoute gecikmesi](media/network-performance-monitor-expressroute/expressroute-latency.png) 

### <a name="peerings-list"></a>Eşlemeler listesi 

Özel bakış üzerinden sanal ağlara yapılan tüm bağlantıların listesini getirmek için panodaki **Özel Eşler** döşemesini seçin. Burada, sanal ağ bağlantısı seçebilir ve sistem durumu durumunu, paket kaybı, bant genişliği kullanımı ve gecikme sonu için eğilim grafiklerini görüntüleyebilirsiniz. 

![ExpressRoute eşlemeleri](media/network-performance-monitor-expressroute/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Devre topolojisi 

Devre topolojisini görüntülemek için **Topoloji** karo'yu seçin. Bu eylem seçili devre veya bakan topoloji görünümüne götürür. Topoloji diyagramı ağdaki her kesim için gecikme yitirme sağlar ve her katman 3 atlama diyagramın bir düğüm ile temsil edilir. Atlama seçimi, atlama hakkında daha fazla ayrıntı ortaya çıkarır. Şirket içi atlamaları içerecek şekilde görünürlük düzeyini artırmak için kaydırıcı çubuğunu **FILTERS'in**altına taşıyın. Kaydırıcı çubuğunu sola veya sağa taşımak, topoloji grafiğindeki atlama sayısını artırır veya azaltır. Her segmentteki gecikme, ağınızdaki yüksek gecikmeli kesimlerin daha hızlı yalıtılmış olarak yalıtMasına olanak tanır.

![ExpressRoute topolojisi](media/network-performance-monitor-expressroute/expressroute-topology.png)

### <a name="detailed-topology-view-of-a-circuit"></a>Bir devrenin ayrıntılı topoloji görünümü 

Bu görünüm, sanal ağ bağlantılarını gösterir. 

![ExpressRoute sanal ağ bağlantıları](media/network-performance-monitor-expressroute/expressroute-vnet.png)
 
## <a name="diagnostics"></a>Tanılama 

Ağ Performans İzleyicisi, birkaç devre bağlantısı sorunlarını tanılamanıza yardımcı olur. Görebileceğiniz sorunlardan bazıları aşağıda listelenmiştir.

**LogAnalytics**aracılığıyla bildirim kodlarını görebilir ve uyarıları üzerlerinde ayarlayabilirsiniz. **NPM Diagnostics** sayfasında tetiklenen her tanılama iletisinin açıklamalarını görebilirsiniz.

| Bildirim Kodu (Günlükler) | Açıklama |
| --- | --- |
| 5501 | ExpressRoute devresinin ikincil bağlantısı üzerinden geçileme |
| 5502 | ExpressRoute devresinin birincil bağlantısı üzerinden geçiş yapamamak |
| 5503 | Çalışma alanına bağlı abonelik için devre bulunamadı | 
| 5508 | Yol için herhangi bir devreden trafiğin geçip geçmediğini belirleyememek |
| 5510 | Trafik istenilen devreden geçmiyor | 
| 5511 | Trafik amaçlanan sanal ağdan geçmiyor | 

**Devre devre düştü.** Ağ Performans İzleyicisi, şirket içi kaynaklarınız ve Azure sanal ağlarınız arasındaki bağlantı kaybolur kaybolmaz sizi size haber vetir. Bu bildirim, kullanıcı yükseltmeleri almadan ve kapalı kalma süresini azaltmadan önce proaktif eylemde bulunmanıza yardımcı olur.

![ExpressRoute devresi devre soldu](media/network-performance-monitor-expressroute/expressroute-circuit-down.png)
 

**Trafik amaçlanan devreden akmıyor.** Ağ Performans İzleyicisi, trafik amaçlanan ExpressRoute devresi üzerinden akmadığı nda sizi haber leştirir. Devre devre kapanıyorsa ve trafik yedek rotadan akıyorsa bu sorun olabilir. Yönlendirme sorunu varsa da olabilir. Bu bilgiler, yönlendirme ilkelerinizdeki yapılandırma sorunlarını proaktif olarak yönetmenize ve en iyi ve güvenli rotanın kullanıldığından emin olsanız yardımcı olur. 

 

**Trafik birincil devreden akmıyor.** Ağ Performans İzleyicisi, ikincil ExpressRoute devresi üzerinden trafik aktığında sizi haberleştirir. Bu durumda herhangi bir bağlantı sorunu yaşamasanız da, birincil devreyle ilgili sorunları proaktif olarak gidermek sizi daha iyi hazır hale getirir. 

 
![ExpressRoute trafik akışı](media/network-performance-monitor-expressroute/expressroute-traffic-flow.png)


**Pik kullanım nedeniyle bozulma.** Azure iş yükü bozulmasının bant genişliği kullanımındaki en yüksek düşüşten kaynaklanıp kaynaklanmadığını belirlemek için bant genişliği kullanım eğilimini gecikme eğilimiyle ilişkilendirebilirsiniz. O zaman buna göre harekete geçebilirsiniz.

![ExpressRoute bant genişliği kullanımı](media/network-performance-monitor-expressroute/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılı ağ performansı veri kayıtlarını görüntülemek için [günlükleri arayın.](../../azure-monitor/log-query/log-query-overview.md)
