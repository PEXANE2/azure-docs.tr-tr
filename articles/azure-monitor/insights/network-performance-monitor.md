---
title: Azure'da Ağ Performans Monitörü çözümü | Microsoft Dokümanlar
description: Azure'daki Ağ Performans İzleyicisi, ağ performansı sorunlarını algılamak ve bulmak için ağlarınızın performansını neredeyse gerçek zamanlı olarak izlemenize yardımcı olur.
ms.subservice: logs
ms.topic: conceptual
author: vinynigam
ms.author: vinigam
ms.date: 02/20/2018
ms.openlocfilehash: 9660e87f3ee4e1c1c6a270f14928fdd111664e66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480887"
---
# <a name="network-performance-monitor-solution-in-azure"></a>Azure'da Ağ Performans İzleme çözümü

![Ağ Performans Monitörü simgesi](./media/network-performance-monitor/npm-symbol.png)


Ağ Performans İzleyicisi, ağ altyapınızdaki çeşitli noktalar arasındaki ağ performansını izlemenize yardımcı olan bulut tabanlı karma ağ izleme çözümüdür. Ayrıca hizmet ve uygulama uç noktalarına yapılan ağ bağlantısını izlemenize ve Azure ExpressRoute’un performansını izlemenize yardımcı olur. 

Ağ Performans İzleyicisi, trafik karaollama, yönlendirme hataları ve geleneksel ağ izleme yöntemlerinin algılanamayan sorunları gibi ağ sorunlarını algılar. Bir ağ bağlantısı için bir eşik ihlal edildiğinde, çözüm uyarılar oluşturur ve size bildirir. Ayrıca ağ performans sorunlarının zamanında tespit edilmesini sağlar ve sorunun kaynağını belirli bir ağ kesimine veya cihazına yerelleştirir. 

Ağ Performans Monitörü üç geniş özellik sunar: 

* [Performans Monitörü](network-performance-monitor-performance-monitor.md): Bulut dağıtımları ve şirket içi konumlar, birden fazla veri merkezi ve şube ofisleri ve görev açısından kritik çok katmanlı uygulamalar veya mikro hizmetler arasında ağ bağlantısını izleyebilirsiniz. Performance Monitor ile, kullanıcılar şikayet etmeden önce ağ sorunlarını algılayabilirsiniz.

* [Hizmet Bağlantı İzleyicisi](network-performance-monitor-service-connectivity.md): Kullanıcılarınızdan önemsediğiniz hizmetlere olan bağlantılarını izleyebilir, yolda hangi altyapının olduğunu belirleyebilir ve ağ darboğazlarının nerede oluştuğunu belirleyebilirsiniz. Kesintileri kullanıcılarınızdan önce öğrenebilir ve ağ yolunuzdaki sorunların tam konumunu görebilirsiniz. 

    Bu özellik, http, HTTPS, TCP ve ICMP'ye dayalı testler gerçekleştirmenize yardımcı olur ve hizmetinizin kullanılabilirliğini ve yanıt süresini neredeyse gerçek zamanlı olarak veya tarihsel olarak izlemenize yardımcı olur. Ayrıca paket kaybı ve gecikme içinde ağın katkısını izleyebilirsiniz. Ağ topolojisi haritasıyla, ağ yavaşlamalarını yalıtabilirsiniz. Her atlamada gecikme verileriyle birlikte, düğümden hizmete ağ yolu boyunca oluşan sorunlu noktaları tanımlayabilirsiniz. Yerleşik testler ile, herhangi bir ön yapılandırma olmadan Office 365 ve Dynamics CRM'ye ağ bağlantısını izleyebilirsiniz. Bu özellik sayesinde, web siteleri, SaaS uygulamaları, PaaS uygulamaları ve SQL veritabanları gibi TCP özellikli uç noktalarına ağ bağlantısını izleyebilirsiniz.

* [ExpressRoute Monitor](network-performance-monitor-expressroute.md): Azure ExpressRoute üzerinden şubeleriniz ile Azure arasında uçuça bağlantı ve performansı izleyin.  

[Network Performance Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview) tarafından desteklenen çeşitli yetenekler hakkında daha fazla bilgi çevrimiçi olarak mevcuttur.
 
## <a name="supported-regions"></a>Desteklenen Bölgeler
NPM, aşağıdaki bölgelerden birinde barındırılan bir çalışma alanından, dünyanın herhangi bir yerindeki ağlar ve uygulamalar arasındaki bağlantıyı izleyebilir:
* Kuzey Avrupa
* Batı Avrupa
* Orta Fransa
* Orta Kanada
* Batı ABD
* Orta Batı ABD
* Orta Kuzey ABD
* Orta Güney ABD
* Orta ABD
* Doğu ABD
* Doğu ABD 2
* Batı ABD 2
* Doğu Japonya
* Güneydoğu Asya
* Güney Doğu Avustralya
* Orta Avustralya
* Doğu Avustralya
* Güney İngiltere
* Doğu Asya
* Güney Kore - Orta
* Orta Hindistan
* ABD Hükümeti Virginia
* Çin Doğu 2


ExpressRoute Monitor için desteklenen bölgelerin listesi [belgelerde](https://docs.microsoft.com/azure/expressroute/how-to-npm?utm_swu=8117)mevcuttur.


## <a name="set-up-and-configure"></a>Ayarlama ve yapılandırma

### <a name="install-and-configure-agents"></a>Aracıları yükleme ve yapılandırma 

Windows bilgisayarlarına bağlantı verme aracılarını [Azure Monitor'a](../platform/agent-windows.md) yüklemek ve [İşlem Yöneticisi'ni Azure Monitor'a bağlamak için](../platform/om-agents.md)temel işlemleri kullanın.

### <a name="where-to-install-the-agents"></a>Aracılar nerede yüklenir 

* **Performans İzleyicisi**: Diğer alt ağlara ağ bağlantısını izlemek istediğiniz her alt ağa bağlı en az bir düğüme Log Analytics aracılarını yükleyin.

    Bir ağ bağlantısını izlemek için, bu bağlantının her iki uç noktaya aracıyükleyin. Ağınızın topolojisi hakkında emin değilseniz, ağ performansını izlemek istediğiniz kritik iş yüklerine sahip aracıları sunuculara yükleyin. Örneğin, bir web sunucusu ile SQL çalıştıran bir sunucu arasındaki ağ bağlantısını izlemek istiyorsanız, her iki sunucuya da bir aracı yükleyin. Aracılar, ana bilgisayarların kendileri değil, ana bilgisayarlar arasındaki ağ bağlantısını (bağlantıları) izler. 

* **Service Connectivity Monitor**: Ağ bağlantısını hizmet bitiş noktasına kadar izlemek istediğiniz her düğüme bir Log Analytics aracısı yükleyin. O1, O2 ve O3 etiketli ofis sitelerinizden Office 365'e ağ bağlantısını izlemek istiyorsanız buna bir örnektir. Log Analytics aracısını O1, O2 ve O3'te en az bir düğüme yükleyin. 

* **ExpressRoute Monitor**: Azure sanal ağınıza en az bir Log Analytics aracısı yükleyin. Ayrıca, ExpressRoute özel bakışları aracılığıyla bağlı olan şirket içi alt ağınıza en az bir aracı yükleyin.  

### <a name="configure-log-analytics-agents-for-monitoring"></a>İzleme için Log Analytics aracılarını yapılandırın 

Ağ Performans İzleyicisi, kaynak ve hedef aracılar arasındaki ağ performansını izlemek için sentetik işlemler kullanır. Performans İzleyicisi ve Hizmet Bağlantısı İzleme yeteneklerinde izleme protokolü olarak TCP ve ICMP arasında seçim yapabilirsiniz. ExpressRoute Monitor için izleme protokolü olarak yalnızca TCP kullanılabilir. Güvenlik duvarının seçtiğiniz protokolü izlemek için kullanılan Log Analytics aracıları arasında iletişime izin verdiğinden emin olun. 

* **TCP protokolü**: İzleme protokolü olarak TCP'yi seçerseniz, aracıların birbirine bağlanabilmesini sağlamak için Ağ Performans İzleyicisi ve ExpressRoute Monitor için kullanılan aracıların güvenlik duvarı bağlantı noktasını açın. Bağlantı noktasını açmak için, yönetim ayrıcalıklarına sahip bir PowerShell penceresinde herhangi bir parametre olmadan [EnableRules.ps1](https://aka.ms/npmpowershellscript) PowerShell komut dosyasını çalıştırın.

    Komut dosyası, çözümün gerektirdiği kayıt defteri anahtarlarını oluşturur. Ayrıca, aracıların birbirleriyle TCP bağlantıları oluşturmasına izin vermek için Windows Güvenlik Duvarı kuralları oluşturur. Komut dosyası tarafından oluşturulan kayıt defteri anahtarları, hata ayıklama günlüklerini ve günlükler dosyasının yolunu günlüğe kaydedip günlüğe kaydetmemeyi belirtir. Komut dosyası, iletişim için kullanılan TCP bağlantı noktasını da tanımlar. Bu anahtarların değerleri komut dosyası tarafından otomatik olarak ayarlanır. Bu anahtarları el ile değiştirmeyin. Varsayılan olarak açılan bağlantı noktası 8084'dür. Komut dosyasına parametre portUNumarası sağlayarak özel bir bağlantı noktası kullanabilirsiniz. Komut dosyasının çalıştırıldığı tüm bilgisayarlarda aynı bağlantı noktasını kullanın. 

    >[!NOTE]
    > Komut dosyası yalnızca Windows Güvenlik Duvarı'nı yerel olarak yapılandırır. Bir ağ güvenlik duvarınız varsa, Ağ Performans İzleyicisi tarafından kullanılan TCP bağlantı noktası için trafiğe izin verdiğinden emin olun.

    >[!NOTE]
    > Service Connectivity Monitor için [EnableRules.ps1](https://aka.ms/npmpowershellscript ) PowerShell komut dosyasını çalıştırmanız gerekmez.

    

* **ICMP protokolü**: İzleme protokolü olarak ICMP'yi seçerseniz, ICMP'yi güvenilir bir şekilde kullanmak için aşağıdaki güvenlik duvarı kurallarını etkinleştirin:
    
   ```
   netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
   ```
 

### <a name="configure-the-solution"></a>Çözümü yapılandırma 

1. [Azure pazar](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview)yerinden çalışma alanınıza Ağ Performans İzleyicisi çözümünü ekleyin. [Ayrıca, Çözüm Galerisi'nden Azure Monitörü Ekle çözümlerinde](../../azure-monitor/insights/solutions.md)açıklanan işlemi de kullanabilirsiniz. 
2. Günlük Analizi çalışma alanınızı açın ve **Genel Bakış** döşemesini seçin. 
3. İleti Çözümü ile **Ağ Performans Monitörü** döşemesini seçin *ek yapılandırma gerektirir.*

   ![Ağ Performans Monitörü döşemesi](media/network-performance-monitor/npm-config.png)

4. **Kurulum** sayfasında, Log Analytics aracılarını yükleme ve **ortak ayarlar** görünümünde izleme için aracıları yapılandırma seçeneğini görürsünüz. Daha önce açıklandığı gibi, Log Analytics aracılarını yüklediyseniz ve yapılandırdıysanız, kullanmak istediğiniz özelliği yapılandırmak için **Kurulum** görünümünü seçin. 

   **Performans İzleyicisi**: **Varsayılan** Performans İzleyicisi kuralında sentetik işlemler için kullanılacak protokolü seçin ve **Devam & kaydet'i**seçin. Bu protokol seçimi yalnızca sistem tarafından oluşturulan varsayılan kural için tutar. Bir Performans İzleyicisi kuralını her oluşturduğunuzda protokolü seçmeniz gerekir. **Performans İzleyicisi** sekmesindeki **Varsayılan** kural ayarlarına her zaman geçebilir (gün-0 yapılandırmanızı tamamladıktan sonra görünür) ve protokolü daha sonra değiştirebilirsiniz. Performans İzleyicisi özelliğini istemiyorsanız, **Performans İzleyicisi** sekmesindeki **Varsayılan** kural ayarlarından varsayılan kuralı devre dışı kullanabilirsiniz.

   ![Performans Monitörü görünümü](media/network-performance-monitor/npm-synthetic-transactions.png)
    
   **Hizmet Bağlantı İzni**: Bu özellik, aracılarınızdan Office 365 ve Dynamics 365'e ağ bağlantısını izlemek için önceden yapılandırılmış testler sağlar. Yanlarındaki onay kutularını seçerek izlemek istediğiniz Office 365 ve Dynamics 365 hizmetlerini seçin. İzlemek istediğiniz aracıları seçmek için **Aracı ekle'yi**seçin. Bu özelliği kullanmak istemiyorsanız veya daha sonra ayarlamak istemiyorsanız, hiçbir şey seçmeyin ve **Devam & kaydet'i**seçin.

   ![Hizmet Bağlantı Monitörü görünümü](media/network-performance-monitor/npm-service-endpoint-monitor.png)

   **ExpressRoute Monitor**: Bu Log Analytics çalışma alanına bağlı Azure aboneliğindeki sanal ağlara bağlı tüm ExpressRoute özel eşlemelerini keşfetmek için **Şimdi Keşfet'i** seçin. 

   ![ExpressRoute Monitör görünümü](media/network-performance-monitor/npm-express-route.png)

   Keşif tamamlandıktan sonra, keşfedilen devreler ve eşlemeler bir tabloda listelenir. 

   ![Ağ Performans Monitörü Yapılandırma sayfası](media/network-performance-monitor/npm-private-peerings.png)
    
Bu devreler ve eşlemeler için izleme başlangıçta devre dışı bir durumdadır. İzlemek istediğiniz her kaynağı seçin ve sağdaki ayrıntılar görünümünden onlar için izlemeyi yapılandırın. Yapılandırmayı kaydetmek için **Kaydet'i** seçin. Daha fazla bilgi için "ExpressRoute izlemeyi yapılandır" makalesine bakın. 

Kurulum tamamlandıktan sonra, verilerin doldurulması 30 dakika ile bir saat arasında sürer. Çözüm ağınızdaki verileri toplarken, ileti Çözümü'nün Ağ Performans Monitörü **Genel Bakış** döşemesi üzerinde *ek yapılandırma gerektirdiğini* görürsünüz. Veriler toplandıktan ve dizine alındıktan sonra, **Genel Bakış** döşemesi değişir ve bir özet halinde ağ durumunuz hakkında sizi bilgilendirir. Daha sonra, Log Analytics aracılarının yüklü olduğu düğümlerin ve ortamınızdan keşfedilen alt ağların izlenmesini de yapabilirsiniz.

#### <a name="edit-monitoring-settings-for-subnets-and-nodes"></a>Alt ağlar ve düğümler için izleme ayarlarını değiştirme 

En az bir aracı yüklü olan tüm alt ağlar yapılandırma sayfasındaki **Alt Ağlar** sekmesinde listelenir. 


Belirli alt ağların izlenmesini etkinleştirmek veya devre dışı kılabilir:

1. **Alt ağ kimliğinin**yanındaki onay kutusunu seçin veya temizleyin. Ardından, **İzleme için Kullanım'ın** uygun şekilde seçildiğinden veya temizlendiğinden emin olun. Birden çok alt ağ seçebilir veya temizleyebilirsiniz. Devre dışı bırakıldığında, alt ağlar izlenmez ve aracılar diğer aracılara ping atmayı durdurmak için güncelleştirilir. 
2. Belirli bir alt ağda izlemek istediğiniz düğümleri seçin. Listeden alt ağı seçin ve gerekli düğümleri izlenmeyen ve izlenen düğümleri içeren listeler arasında taşıyın. Alt ağa özel bir açıklama ekleyebilirsiniz.
3. Yapılandırmayı kaydetmek için **Kaydet'i** seçin. 

#### <a name="choose-nodes-to-monitor"></a>İzlemek için düğümleri seçin

Üzerlerinde aracı yüklü olan tüm **düğümler Düğümler** sekmesinde listelenir. 

1. İzlemek veya izlemeyi durdurmak istediğiniz düğümleri seçin veya temizleyin. 
2. **İzleme için Kullan'ı**seçin veya uygun şekilde temizleyin. 
3. **Kaydet'i**seçin. 


İstediğinizin özelliklerini yapılandırın:

- [Performans İzleyicisi](network-performance-monitor-performance-monitor.md#configuration)
- [Hizmet Bağlantısı İzleyicisi](network-performance-monitor-performance-monitor.md#configuration)
- [ExpressRoute Monitör](network-performance-monitor-expressroute.md#configuration)

 

## <a name="data-collection-details"></a>Veri toplama ayrıntıları
Ağ Performans İzleyicisi, kayıp ve gecikme bilgilerini toplamak için protokol olarak TCP'yi seçtiğinizde TCP SYN-SYNACK-ACK el sıkışma paketlerini kullanır. Ağ Performans İzleyicisi, protokol olarak ICMP'yi seçtiğinizde ICMP ICMP ECHO REPLY kullanır. İzleme yolu topoloji bilgilerini almak için de kullanılır.

Aşağıdaki tabloda veri toplama yöntemleri ve Ağ Performans İzleyicisi için verilerin nasıl toplandığıyla ilgili diğer ayrıntılar gösterilmektedir.

| Platform | Doğrudan aracı | Sistem Merkezi Operasyon Yöneticisi aracısı | Azure Storage | Operasyon Yöneticisi gerekli mi? | Yönetim grubu aracılığıyla gönderilen Operasyon Yöneticisi aracı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP el sıkışma /ICMP ECHO mesajları her 5 saniyede, veri her 3 dakikada bir gönderilen |
 

 
Çözüm, ağın durumunu değerlendirmek için sentetik işlemler kullanır. Ağ değişimi TCP paketlerindeki veya ICMP Echo'daki çeşitli noktalara yüklenen Log Analytics aracıları birbiriyle. Aracıların TCP paketleri mi yoksa ICMP Echo mu kullandığını izlemek için seçtiğiniz protokole bağlıdır. Bu süreçte, aracılar gidiş-dönüş süresini ve varsa paket kaybını öğrenirler. Düzenli olarak, her aracı, test edilmesi gereken ağdaki tüm çeşitli yolları bulmak için diğer aracılara bir izleme rotası da gerçekleştirir. Bu verileri kullanarak, aracılar ağ gecikmesi ve paket kaybı rakamlarını anlayabilir. Testler her beş saniyede bir tekrarlanır. Veriler, Azure Monitor'daki Log Analytics çalışma alanına yüklenmeden önce aracılar tarafından yaklaşık üç dakika boyunca toplanır.



>[!NOTE]
> Aracılar birbirleriyle sık sık iletişim kursa da, testleri yürütürken önemli ağ trafiği oluşturmaz. Aracılar, kaybı ve gecikmeyi belirlemek için yalnızca TCP SYN-SYNACK-ACK el sıkışma paketlerine güvenir. Veri paketleri değiştirin. Bu işlem sırasında, aracılar yalnızca gerektiğinde birbirleriyle iletişim kurarlar. Aracı iletişim topolojisi ağ trafiğini azaltmak için optimize ediktedir.

## <a name="use-the-solution"></a>Çözümü kullanın 

### <a name="network-performance-monitor-overview-tile"></a>Ağ Performans Monitörü Genel Bakış döşemesi 

Ağ Performans İzleme çözümünü etkinleştirdikten sonra, **Genel Bakış** sayfasındaki çözüm döşemesi ağ durumu hakkında hızlı bir genel bakış sağlar. 

 ![Ağ Performans Monitörü Genel Bakış döşemesi](media/network-performance-monitor/npm-overview-tile.png)

### <a name="network-performance-monitor-dashboard"></a>Ağ Performans Monitörü panosu 

* **En İyi Ağ Sağlık Olayları**: Bu sayfa, sistemdeki en son sağlık olaylarının ve uyarıların ve olayların etkin olduğu zamandan bu yana geçen sürenin bir listesini sağlar. İzleme kuralı için seçilen ölçümün (kayıp, gecikme süresi, yanıt süresi veya bant genişliği kullanımı) değeri eşiği aştığında bir sistem durumu olayı veya uyarı oluşturulur. 

* **ExpressRoute Monitor**: Bu sayfa, çözüm monitörleri ile çeşitli ExpressRoute bakan bağlantıları için sistem durumu özetleri sağlar. **Topoloji** döşemesi, ağınızda izlenen ExpressRoute devreleri aracılığıyla ağ yollarının sayısını gösterir. **Topoloji** görünümüne gitmek için bu döşemeyi seçin.

* **Hizmet Bağlantı İzni**: Bu sayfa, oluşturduğunuz farklı testler için sistem durumu özetleri sağlar. **Topoloji** döşemesi izlenen uç nokta sayısını gösterir. **Topoloji** görünümüne gitmek için bu döşemeyi seçin.

* **Performans İzleyicisi**: Bu sayfa, çözümün izlediği **Ağ** bağlantıları ve **Alt ağ** bağlantıları için sistem durumu özetleri sağlar. **Topoloji** döşemesi, ağınızda izlenen ağ yollarının sayısını gösterir. **Topoloji** görünümüne gitmek için bu döşemeyi seçin. 

* **Ortak Sorgular**: Bu sayfa, ham ağ izleme verilerini doğrudan getiren bir arama sorgusu kümesi içerir. Özelleştirilmiş raporlama için kendi sorgularınızı oluşturmak için bu sorguları başlangıç noktası olarak kullanabilirsiniz. 

   ![Ağ Performans Monitörü panosu](media/network-performance-monitor/npm-dashboard.png)

 

### <a name="drill-down-for-depth"></a>Derinlik için detaya inin 

İlginizi çeken herhangi bir alana daha derininde inmek için çözüm panosundaki çeşitli bağlantıları seçebilirsiniz. Örneğin, panoda bir uyarı veya sağlıksız bir ağ bağlantısı gördüğünüzde, daha fazla araştırmak için bu uyarıyı seçin. Bir sayfa, belirli ağ bağlantısının tüm alt ağ bağlantılarını listeler. Her alt ağ bağlantısının kaybını, gecikmesini ve sistem durumu durumunu görebilirsiniz. Hangi alt ağ bağlantısının sorunlara neden olduğunu hızlı bir şekilde öğrenebilirsiniz. Sağlıksız subnet bağlantısının tüm düğüm bağlantılarını görmek için **düğüm bağlantılarını** görüntüle'yi seçin. Ardından, tek tek düğümden düğüme bağlantıları görebilir ve sağlıksız düğüm bağlantılarını bulabilirsiniz. 

Kaynak ve hedef düğümleri arasındaki yolların hop-by-hop topolojisini görüntülemek için **topolojiyi** görüntüleyin'i seçin. Sağlıksız rotalar kırmızı görünür. Sorunu ağın belirli bir bölümüne hızla tanımlayabilmeniz için her atlamanın katkıda olduğu gecikme süresini görüntüleyebilirsiniz.

 

### <a name="network-state-recorder-control"></a>Ağ Durumu Kaydedici denetimi

Her görünüm, belirli bir zamanda ağ sağlığınızın anlık görüntüsünü görüntüler. Varsayılan olarak, en son durum gösterilir. Sayfanın üst kısmındaki çubuk, durum durumunun görüntülendiği zamanı gösterir. Ağ sağınızın anlık görüntüsünü önceki bir zamanda görüntülemek için **Eylemler'i**seçin. Ayrıca, en son durumu görüntülediğiniz sırada herhangi bir sayfa için otomatik yenilemeyi etkinleştirebilir veya devre dışı kullanabilirsiniz. 

 ![Ağ Durumu Kaydedici](media/network-performance-monitor/network-state-recorder.png)

 

### <a name="trend-charts"></a>Eğilim grafikleri 

Ayrıntıya indiğiniz her düzeyde, geçerli ölçümün eğilimini görebilirsiniz. Bu kayıp, gecikme, yanıt süresi veya bant genişliği kullanımı olabilir. Eğilimin zaman aralığını değiştirmek için grafiğin en üstündeki zaman denetimini kullanın. 

Eğilim grafikleri, performans ölçümünün performansının tarihsel bir perspektifini gösterir. Bazı ağ sorunları doğada geçicidir ve yalnızca ağın geçerli durumuna bakarak yakalamak zordur. Sorunlar hızlı bir şekilde yüzey ve kimse fark etmeden önce kaybolur, sadece zaman daha sonraki bir noktada yeniden ortaya çıkmak için. Bu tür geçici sorunlar uygulama yöneticileri için de zor olabilir. Tüm uygulama bileşenleri düzgün çalışır gibi görünse bile, sorunlar genellikle uygulama yanıt süresinde açıklanamayan artışlar olarak görünür. 

Bir eğilim grafiğine bakarak bu tür sorunları kolayca algılayabilirsiniz. Sorun, ağ gecikmesi veya paket kaybında ani bir artış olarak görünür. Sorunu araştırmak için, sorunun oluştuğu zaman için ağ anlık görüntüsünü ve topolojisini görüntülemek için Ağ Durumu Kaydedici denetimini kullanın.

 
![Eğilim grafikleri](media/network-performance-monitor/trend-charts.png)
 

### <a name="topology-map"></a>Topoloji haritası 

Ağ Performans İzleyicisi, etkileşimli bir topoloji haritasında kaynak ve hedef bitiş noktası arasındaki yolların hop-by-hop topolojisini gösterir. Topoloji haritasını görüntülemek için çözüm panosundaki **Topoloji** döşemesini seçin. Ayrıca, detaya inen sayfalardaki **Topolojiyi Görüntüle** bağlantısını da seçebilirsiniz. 

Topoloji haritası, kaynak ve hedef arasında kaç yol olduğunu ve veri paketlerinin hangi yolları izlediğini görüntüler. Her ağ atlama tarafından katkıda gecikme de görülebilir. Toplam yol gecikmesinin eşiğin üzerinde olduğu tüm yollar (ilgili izleme kuralında ayarlanır) kırmızı renkle gösterilir. 

Topoloji haritasında bir düğüm seçtiğinizde veya üzerinde gezindiğinizde, FQDN ve IP adresi gibi düğüm özelliklerini görürsünüz. IP adresini görmek için bir atlama seçin. Katkıda bulunan gecikme yi fark ederek zahmetli ağ atlamasını tanımlayabilirsiniz. Belirli yolları filtrelemek için, daraltılabilir eylem bölmesinde filtreleri kullanın. Ağ topolojilerini basitleştirmek için, eylem bölmesinde kaydırıcıyı kullanarak ara atlamaları gizleyin. Fare tekerleğinizi kullanarak topoloji haritasını yakınlaştırabilir veya uzaklaştırabilirsiniz. 

Haritada gösterilen topoloji katman 3 topolojisidir ve katman 2 aygıtları ve bağlantıları içermez. 

 
![Topoloji haritası](media/network-performance-monitor/topology-map.png)
 

## <a name="log-queries-in-azure-monitor"></a>Azure Monitör'de sorguları günlüğe kaydetme

Ağ Performans Monitörü panosu ve ayrıntılı sayfalar aracılığıyla grafik olarak açıkta kalan tüm [veriler, günlük sorgularında](../log-query/log-query-overview.md)yerel olarak da kullanılabilir. Depodaki verilerin etkileşimli çözümlemesi yapabilir ve farklı kaynaklardan gelen verileri ilişkilendirebilirsiniz. Ayrıca özel uyarılar ve görünümler oluşturabilir ve verileri Excel, Power BI veya paylaşılabilir bir bağlantıya aktarabilirsiniz. Panodaki **Ortak Sorgular** alanında, kendi sorgularınızı ve raporlarınızı oluşturmak için başlangıç noktası olarak kullanabileceğiniz bazı yararlı sorgular vardır. 

## <a name="alerts"></a>Uyarılar

Ağ Performans İzleyicisi, [Azure Monitor'un](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)uyarı özelliklerini kullanır.

Bu, tüm bildirimlerin [eylem grupları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups)kullanılarak yönetildiği anlamına gelir.  

Log Analytics aracılığıyla uyarı oluşturan bir NPM kullanıcısıysanız: 
1. Sizi Azure portalına yönlendirecek bir bağlantı görürsünüz. Portala erişmek için tıklayın.
2. Ağ Performans Monitörü çözüm döşemesini tıklatın. 
3. Yapılandırmaya gidin.  
4. Bir uyarı oluşturmak istediğiniz testi seçin ve aşağıdaki adımları izleyin.

Azure portalı üzerinden uyarı oluşturan bir NPM kullanıcısıysanız:  
1. E-postanızı doğrudan girmeyi seçebilir veya eylem grupları aracılığıyla uyarı oluşturmayı seçebilirsiniz.
2. E-postanızı doğrudan girmeyi seçerseniz, **NPM E-posta Eylem Grubu** adında bir eylem grubu oluşturulur ve bu eylem grubuna e-posta kimliği eklenir.
3. Eylem gruplarını kullanmayı seçerseniz, önceden oluşturulmuş bir eylem grubu seçmeniz gerekir. Burada bir eylem grubu oluşturmayı [öğrenebilirsiniz.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups#create-an-action-group-by-using-the-azure-portal) 
4. Uyarı başarıyla oluşturulduktan sonra, uyarılarınızı yönetmek için Uyarıları Yönet bağlantısını kullanabilirsiniz. 

Her uyarı oluşturduğunuzda, NPM Azure Monitor'da sorgu tabanlı günlük uyarı kuralı oluşturur. Bu sorgu varsayılan olarak her 5 dakikada bir tetiklenir. Azure monitörü oluşturulan ilk 250 günlük uyarı kuralları için ücret almaz ve 250 günlük uyarı kuralları sınırının üzerindeki tüm uyarı kuralları [Azure Monitor fiyatlandırma sayfasındaki Uyarılar fiyatlandırması uyarınca](https://azure.microsoft.com/pricing/details/monitor/)faturalandırılır.
Bildirimler, [Azure Monitor fiyatlandırma sayfasındaki Bildirimler fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/)uyarınca ayrı olarak ücretlendirilir.


## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma hakkında bilgi [online](network-performance-monitor-pricing-faq.md)olarak mevcuttur.

## <a name="provide-feedback"></a>Geri bildirimde bulunma 

* **Kullanıcı Sesi:** Üzerinde çalışmamızı istediğiniz Ağ Performans Monitörü özellikleri için fikirlerinizi gönderebilirsiniz. [UserVoice sayfasını](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring)ziyaret edin. 

* **Kohortumuza katılın:** Her zaman yeni müşterilerin kohortumuza katılmasını istiyoruz. Bunun bir parçası olarak, yeni özelliklere erken erişim ve Ağ Performans Monitörü'nün geliştirilmesine yardımcı olacak bir fırsat elde elabilirsiniz. Katılmak istiyorsanız, bu [hızlı anketi](https://aka.ms/npmcohort)doldurun. 

## <a name="next-steps"></a>Sonraki adımlar 
[Performans Monitörü,](network-performance-monitor-performance-monitor.md) [Servis Bağlantı Monitörü](network-performance-monitor-performance-monitor.md)ve [ExpressRoute Monitor](network-performance-monitor-expressroute.md)hakkında daha fazla bilgi edinin. 
