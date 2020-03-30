---
title: Kapsayıcılar için Azure Monitör ile Kubernetes izleme | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar için Azure Monitor ile bir Kubernetes kümesinin performansını nasıl görüntüleyebileceğiniz ve analiz edebileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 03/26/2020
ms.openlocfilehash: 227fe70512536790d179797394b6fba22e7eb50e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298367"
---
# <a name="monitor-your-kubernetes-cluster-performance-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitörü ile Kubernetes küme performansınızı izleme

Kapsayıcılar için Azure Monitor ile, Azure Kubernetes Hizmeti (AKS), Azure Yığını veya diğer ortamda barındırılan Kubernetes kümelerinin iş yükünü iki perspektiften izlemek için performans grafiklerini ve sistem durumu durumunu kullanabilirsiniz. Doğrudan kümeden izleyebilir veya Azure Monitor'dan bir abonelikteki tüm kümeleri görüntüleyebilirsiniz. Azure Kapsayıcı Örneklerini görüntülemek, belirli bir AKS kümesini izlerken de mümkündür.

Bu makale, iki perspektifi ve Azure Monitor'un algılanan sorunları hızla değerlendirmenize, araştırmanıza ve çözmenize nasıl yardımcı olduğunu anlamanıza yardımcı olur.

Kapsayıcılar için Azure Monitörü'ne nasıl etkinleştirilir hakkında bilgi için [kapsayıcılar için Yerleşik Azure Monitör'e](container-insights-onboard.md)bakın.

Azure Monitor, aboneliklerinizdeki kaynak grupları arasında dağıtılan Linux ve Windows Server 2019 çalıştıran tüm izlenen Kubernetes kümelerinin sistem durumunu gösteren bir çoklu küme görünümü sağlar. Çözüm tarafından izlenmeyen tüm ortamlarda keşfedilen kümeleri gösterir. Küme durumunu hemen anlayabilir ve buradan düğüm ve denetleyici performans sayfasına dinlenebilir veya kümenin performans grafiklerini görmek için gezinebilirsiniz. Keşfedilen ve izlenmeyen olarak tanımlanan AKS kümeleri için, bunlar için istediğiniz zaman izleme yi etkinleştirebilirsiniz. 

Bir Linux kümesiyle karşılaştırıldığında kapsayıcılar için Azure Monitor ile bir Windows Server kümesini izlemedeki temel farklar [burada](container-insights-overview.md#what-does-azure-monitor-for-containers-provide) genel bakış makalesinde açıklanmıştır.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com)oturum açın. 

## <a name="multi-cluster-view-from-azure-monitor"></a>Azure Monitör'den çok kümeli görünüm

Dağıtılan tüm Kubernetes kümelerinin sistem durumu durumunu görüntülemek için Azure portalındaki sol bölmeden **Monitör'ü** seçin. İstatistikler bölümünde **Kapsayıcılar'ı** seçin. **Containers** 

![Azure Monitörü çok kümeli pano örneği](./media/container-insights-analyze/azmon-containers-multiview.png)

Aşağıdaki kümeleri göstermek için ızgarada sunulan sonuçları kapsam dışı edebilirsiniz:

* **Azure** - AKS ve AKS-Engine kümeleri Azure Kubernetes Hizmetinde barındırılan
* **Azure Yığını (Önizleme)** - Azure Yığını'nda barındırılan AKS-Motor kümeleri
* **Azure'a Ait Olmayan (Önizleme)** - Şirket içinde barındırılan Kubernetes kümeleri
* **Tümü** - Azure, Azure Yığını'nda ve kapsayıcılar için Azure Monitörü'nde bulunan şirket içi ortamlarda barındırılan tüm Kubernetes kümelerini görüntüleyin

Belirli bir ortamdan kümeleri görüntülemek için, sayfanın sol üst köşesindeki **Ortamlar** hapından seçin.

![Çevre hapı seçici örneği](./media/container-insights-analyze/clusters-multiview-environment-pill.png)

**İzlenen kümeler** sekmesinde aşağıdakileri öğrenirsiniz:

- Kaç küme kritik veya sağlıksız durumda, ancak kaç tanesi sağlıklı veya raporlama değil (Bilinmeyen durum olarak adlandırılır).
- [Azure Kubernetes Engine (AKS motoru)](https://github.com/Azure/aks-engine) dağıtımlarının tümünün sağlıklı olup olmadığı.
- Küme başına kaç düğüm ve kullanıcı ve sistem bölmesi dağıtılır.
- Ne kadar disk alanı kullanılabilir ve kapasite sorunu varsa.

Dahil edilen sağlık durumları şunlardır: 

* **Sağlıklı**: VM için hiçbir sorun algılanmadı ve gerektiği gibi çalışıyor. 
* **Kritik**: Beklendiği gibi normal çalışma durumunu geri yüklemek için ele alınması gereken bir veya daha fazla kritik sorun algılanır.
* **Uyarı**: Ele alınması gereken veya sağlık durumunun kritik hale gelebileceği bir veya daha fazla sorun algılanır.
* **Bilinmiyor**: Hizmet düğüm veya bölmeyle bağlantı kuramadıysa, durum Bilinmeyen bir duruma dönüşür.
* **Bulunamadı**: Bu çözümün çalışma alanını içeren çalışma alanı, kaynak grubu veya abonelik silindi.
* **Yetkisiz**: Kullanıcının çalışma alanındaki verileri okumak için gerekli izinleri yoktur.
* **Hata**: Çalışma alanından verileri okumaya çalışırken bir hata oluştu.
* **Yanlış yapılandırıldı**: Kapsayıcılar için Azure Monitor, belirtilen çalışma alanında doğru şekilde yapılandırılmadı.
* **Veri yok**: Veriler son 30 dakikadır çalışma alanına bildirilmemiş.

Sistem durumu, bir istisna dışında, genel küme durumunu üç durumun *en kötüsü* olarak hesaplar. Üç durumdan herhangi biri Bilinmiyorsa, genel küme durumu **Bilinmiyor'u**gösterir. 

Aşağıdaki tablo, çok küme görünümünde izlenen bir küme için sistem durumu durumlarını kontrol eden hesaplamanın bir dökümünü sağlar.

| |Durum |Kullanılabilirlik |  
|-------|-------|-----------------|  
|**Kullanıcı bölmesi**| | |  
| |Sağlam |%100 |  
| |Uyarı |90 - 99% |  
| |Kritik |<%90 |  
| |Bilinmiyor |Son 30 dakika içinde rapor edilmezse |  
|**Sistem bölmesi**| | |  
| |Sağlam |%100 |
| |Uyarı |Yok |
| |Kritik |<%100 |
| |Bilinmiyor |Son 30 dakika içinde rapor edilmezse |
|**Node** | | |
| |Sağlam |>%85 |
| |Uyarı |60 - 84% |
| |Kritik |<%60 |
| |Bilinmiyor |Son 30 dakika içinde rapor edilmezse |

Kümeler listesinden, kümenin adını seçerek **Küme** sayfasına kadar ayrıntılı olarak inceleyebilirsiniz. Ardından, belirli bir küme için **Düğümler** sütunundaki düğümlerin topünü seçerek **Düğümler** performans sayfasına gidin. Veya, **Kullanıcı bölmeleri** veya Sistem **bölmeleri** sütununun yuvarlanmasını seçerek **Denetleyiciler** performans sayfasına kadar ayrıntıya inebilirsiniz.

## <a name="view-performance-directly-from-a-cluster"></a>Performansı doğrudan bir kümeden görüntüleme

Kapsayıcılar için Azure Monitor'a erişim, sol bölmeden **Öngörüler** > **Kümesi'ni** seçerek veya çoklu küme görünümünden bir küme seçtiğinizde doğrudan bir AKS kümesinden kullanılabilir. Kümeniz hakkındaki bilgiler dört perspektif halinde düzenlenmiştir:

- Küme
- Düğümler 
- Denetleyiciler 
- Kapsayıcılar

>[!NOTE]
>Bu makalenin geri kalanında açıklanan deneyim, Azure Yığını'nda veya çoklu küme görünümünden seçildiğinde barındırılan Kubernetes kümelerinizin performansını ve sistem durumu durumunu görüntülemek için de geçerlidir. 

Varsayılan sayfa açılır ve kümenizin temel performans ölçümlerini gösteren dört satır performans grafiği görüntüler. 

![Küme sekmesinde örnek performans grafikleri](./media/container-insights-analyze/containers-cluster-perfview.png)

Performans grafikleri dört performans ölçümü görüntüler:

- **Düğüm CPU kullanımı&nbsp;**: Tüm küme için CPU kullanımının toplu bir perspektifidir. Zaman aralığının sonuçlarını filtrelemek için, grafiğin üzerindeki yüzdelik seçicide **Avg**, **Min**, **50th**, **90th**, **95th**veya **Max'i** seçin. Filtreler tek tek veya birleştirilmiş olarak kullanılabilir. 
- **Düğüm bellek kullanımı&nbsp;**: Tüm küme için bellek kullanımının toplu bir perspektifidir. Zaman aralığının sonuçlarını filtrelemek için, grafiğin üzerindeki yüzdelik seçicide **Avg**, **Min**, **50th**, **90th**, **95th**veya **Max'i** seçin. Filtreler tek tek veya birleştirilmiş olarak kullanılabilir. 
- **Düğüm sayısı**: Kubernetes'ten düğüm sayısı ve durum. Temsil edilen küme düğümlerinin durumları Toplam, Hazır ve Hazır Değildir. Bunlar tek tek filtrelenebilir veya grafiğin üstündeki seçicide birleştirilebilir. 
- **Etkin pod sayısı**: Kubernetes'ten bir pod sayısı ve durum. Temsil edilen bölmelerin durumları Toplam, Bekleyen, Çalışan, Bilinmeyen, Başarılı veya Başarısız dır. Bunlar tek tek filtrelenebilir veya grafiğin üstündeki seçicide birleştirilebilir. 

Grafikteki her veri noktasında gezinmek için Sol ve Sağ ok tuşlarını kullanın. Yüzdelik çizgiler arasında geçiş yapmak için Yukarı ve Aşağı ok tuşlarını kullanın. Seçili grafiği görüntülediğiniz son Azure panosuna sabitlemek için grafiklerden herhangi birinin sağ üst köşesindeki pin simgesini seçin. Panodan grafiği yeniden boyutlandırAbilir ve yeniden konumlandırabilirsiniz. Panodan grafiği seçmek, kapsayıcılar için sizi Azure Monitörü'ne yönlendirir ve doğru kapsam ve görünümü yükler.

Kapsayıcılar için Azure Monitor, kendi çizim grafiklerinizi oluşturabileceğiniz, eğilimleri ilişkilendirebileceğiniz ve araştırabileceğiniz ve panolara sabitleme yapabileceğiniz Azure Monitor [ölçümleri gezginini](../platform/metrics-getting-started.md)de destekler. Metrikler gezgininden, [metrik tabanlı bir uyarı kuralının](../platform/alerts-metric.md)temeli olarak ölçümlerinizi görselleştirmek için belirlediğiniz ölçütleri de kullanabilirsiniz. 

## <a name="view-container-metrics-in-metrics-explorer"></a>Ölçüler gezgininde kapsayıcı ölçümlerini görüntüleme

Ölçüler gezgininde, kapsayıcılar için Azure Monitor'dan toplanan düğüm ve bölme kullanım ölçümlerini görüntüleyebilirsiniz. Aşağıdaki tablo, kapsayıcı ölçümlerini görselleştirmek için metrik grafiklerin nasıl kullanılacağını anlamanıza yardımcı olacak ayrıntıları özetley.

|Ad Alanı | Ölçüm | Açıklama | 
|----------|--------|-------------|
| insights.container/nodes | |
| | cpuUsageMillicores | Küme genelinde CPU kullanımının toplu ölçümü. 1000 birime (milli = 1000) bölünmüş bir CPU çekirdeğidir. Birçok uygulamanın tek çekirdek kullandığı bir kaptaki çekirdek kullanımını belirlemek için kullanılır.| 
| | cpuKullanımYüzdesi | Küme genelinde yüzde olarak ölçülen toplam ortalama CPU kullanımı.|
| | memoryRssBytes | Konteyner RSS bellek bayt kullanılır.| 
| | memoryRssPercentage | Konteyner RSS bellek yüzde olarak kullanılır.|
| | memoryWorkingSetBytes | Konteyner çalışma seti bellek kullanılır.| 
| | memoryWorkingSetPercentage | Kapsayıcı çalışma kümesi bellek yüzde olarak kullanılır. | 
| | düğümsayısı | Kubernetes'ten bir düğüm sayısı.|
| insights.container/pods | |
| | PodSayısı | Kubernetes'ten bir bakla sayısı.|

Bir ölçütü boyuta göre görüntülemek için [bölebilir](../platform/metrics-charts.md#apply-splitting-to-a-chart) ve farklı segmentlerinin birbiriyle karşılaştırıldığında nasıl görüntüleyebilirsiniz. Bir düğüm için grafiği *ana bilgisayar* boyutuna göre bölümlere ayırabilirsiniz. Bir bölmeden, aşağıdaki boyutlara göre bölümlere ayırabilirsiniz:

* Denetleyicisi
* Kubernetes isim alanı
* Node
* Aşama

## <a name="analyze-nodes-controllers-and-container-health"></a>Düğümleri, denetleyicileri ve konteyner durumunu analiz edin

**Düğümler,** **Denetleyiciler**ve **Kapsayıcılar** sekmelerine geçtiğiniz zaman, özellik bölmesi sayfanın sağ tarafında otomatik olarak görüntülenir. Kubernetes nesnelerini düzenlemek için tanımladığınız etiketleri içeren seçili öğenin özelliklerini gösterir. Bir Linux düğümü seçildiğinde, **Yerel Disk Kapasitesi** bölümü de kullanılabilir disk alanını ve düğüme sunulan her disk için kullanılan yüzdeyi gösterir. Bölmeyi **>>** görüntülemek veya gizlemek için bölmedeki bağlantıyı seçin.

Hiyerarşideki nesneleri genişletirken, özellikler bölmesi seçili nesneyi temel alınsın. Bölmeden, bölmenin üst kısmındaki **Canlı Verileri Görüntüle (önizleme)** bağlantısını seçerek Kubernetes kapsayıcı günlüklerini (stdout/stderror), olayları ve pod ölçümlerini de görüntüleyebilirsiniz. Bu verileri görüntülemek için erişim vermek ve denetlemek için gereken yapılandırma hakkında daha fazla bilgi için [Canlı Verileri Kurulum (önizleme)](container-insights-livedata-setup.md)bilgisine bakın. Küme kaynaklarını gözden geçirerken, bu verileri kapsayıcıdan gerçek zamanlı olarak görebilirsiniz. Bu özellik hakkında daha fazla bilgi için [Kubernetes günlüklerini, olayları ve pod ölçümlerini gerçek zamanlı olarak nasıl görüntüleyin.](container-insights-livedata-overview.md) Önceden tanımlanmış günlük aramalarına dayalı olarak çalışma alanınızda depolanan Kubernetes günlük verilerini görüntülemek **için, analitik** açılır listesinde görünüm **kapsayıcı günlüklerini** görüntüleyin'i seçin. Bu konu hakkında daha fazla bilgi için, [verileri çözümlemek için Arama günlüklerine](container-insights-log-search.md#search-logs-to-analyze-data)bakın.

**Hizmet,** **Düğüm,** **Ad Alanı**veya **Düğüm Havuzu**tarafından görünümün sonuçlarını filtrelemek için sayfanın üst kısmındaki + **Filtre Ekle** seçeneğini kullanın. Filtre kapsamını seçtikten **sonra, Değer Seç alanında** gösterilen değerlerden birini seçin. Filtre yapılandırıldıktan sonra, AKS kümesinin herhangi bir perspektifini görüntülerken genel olarak uygulanır. Formül yalnızca eşit işareti destekler. Sonuçlarınızı daha da daraltmak için ilkinin üstüne ek filtreler ekleyebilirsiniz. Örneğin, **Düğüme**göre bir filtre belirtirseniz, yalnızca ikinci filtre için **Hizmet** veya **Ad Alanı'nı** seçebilirsiniz.

Bir sekmede filtre belirtme, başka bir sekme seçtiğinizde uygulanmaya devam ediyor. Belirtilen filtrenin yanındaki **x** simgesini seçtikten sonra silinir. 

**Düğümler** sekmesine geçin ve satır hiyerarşisi kümenizdeki bir düğümle başlayan Kubernetes nesne modelini izler. Düğümüzerinde çalışan bir veya daha fazla bölmeyi görüntülemek için düğümü genişletin. Bir bölmeye birden fazla kapsayıcı gruplandırılırsa, bunlar hiyerarşideki son satır olarak görüntülenir. Ana bilgisayarda işlemci veya bellek basıncı varsa, ana bilgisayarda pod ile ilgili olmayan kaç iş yükü çalıştığını da görüntüleyebilirsiniz.

![Performans görünümünde Kubernetes Düğüm hiyerarşisi örneği](./media/container-insights-analyze/containers-nodes-view.png)

Windows Server 2019 işletim sistemi çalıştıran Windows Server kapsayıcıları, listedeki tüm Linux tabanlı düğümlerden sonra gösterilir. Bir Windows Server düğümadını genişletdiğinizde, düğümüzerinde çalışan bir veya daha fazla bölme ve kapsayıcıyı görüntüleyebilirsiniz. Bir düğüm seçildikten sonra, özellikler bölmesi sürüm bilgilerini gösterir. Windows Server düğümlerinde bir aracı yüklü olmadığından aracı bilgileri dışlanır. 

![Windows Server düğümleri listelenen örnek Düğüm hiyerarşisi](./media/container-insights-analyze/nodes-view-windows.png) 

Linux IŞLETIM Sistemi'ni çalıştıran Azure Kapsayıcı Örnekleri, listedeki son AKS küme düğümünden sonra gösterilir. Kapsayıcı Örnekleri sanal düğüm genişletdiğinizde, düğüm üzerinde çalışan bir veya daha fazla Kapsayıcı Örnekleri bölmeleri ve kapsayıcılar görüntüleyebilirsiniz. Ölçümler düğümler için toplanmaz ve raporlanır, yalnızca bölmeler için.

![Listelenen Kapsayıcı Örnekleri ile Örnek Düğüm hiyerarşisi](./media/container-insights-analyze/nodes-view-aci.png)

Genişletilmiş bir düğümden, bu denetleyici için filtre uygulanmış performans verilerini görüntülemek için düğüm üzerinde çalışan bölme veya kapsayıcıdan denetleyiciye kadar ayrıntıya inebilirsiniz. Belirli düğüm için **Denetleyici** sütunun altındaki değeri seçin.
 
![Performans görünümünde düğümden denetleyiciye örnek detaylandırma](./media/container-insights-analyze/drill-down-node-controller.png)

Bu nesnelerin durumunu ve kaynak kullanımını gözden geçirmek için sayfanın üst kısmındadenetleyicileri veya kapsayıcıları seçin. Bellek kullanımını gözden geçirmek için, **Metrik** açılır listesinde **Bellek RSS** veya **Bellek çalışma kümesini**seçin. **Memory RSS** yalnızca Kubernetes sürüm 1.8 ve sonrası için desteklenir. Aksi takdirde, **Min&nbsp; ** değerlerini, tanımlanmamış veya temsil edilemeyen bir değeri temsil eden sayısal bir veri türü değeri olan *NaN&nbsp;* olarak gösterirsiniz.

![Kapsayıcı düğümleri performans görünümü](./media/container-insights-analyze/containers-node-metric-dropdown.png)

**Bellek çalışma kümesi,** hem yerleşik belleği hem de sanal belleği (önbellek) gösterir ve uygulamanın ne kullandığının toplamıdır. **Bellek RSS** sadece ana bellek gösterir (başka bir deyişle yerleşik bellek başka bir şey değildir). Bu metrik, kullanılabilir belleğin gerçek kapasitesini gösterir. Yerleşik bellek ve sanal bellek arasındaki fark nedir?

- Yerleşik bellek veya ana bellek, kümenin düğümleri için kullanılabilir makine belleği gerçek miktarıdır.

- Sanal bellek, bellek baskısı altında yken bellekten diske veri değişimi için işletim sistemi tarafından kullanılan sabit disk alanı (önbellek) ayrılmıştır ve gerektiğinde belleğe geri getirir.

Varsayılan olarak, performans verileri son altı saate dayanır, ancak sol üstteki **TimeRange** seçeneğini kullanarak pencereyi değiştirebilirsiniz. Ayrıca, yüzdelik seçicide **Min**, **Avg**, **50th**, **90th**, **95th**, ve **Max** seçerek zaman aralığında sonuçları filtreleyebilirsiniz. 

![Veri filtreleme için yüzdelik seçimi](./media/container-insights-analyze/containers-metric-percentile-filter.png)

**Trend** sütununun altındaki çubuk grafiğin üzerine baktığınızda, her çubuk, 15 dakikalık bir örnek süre içinde hangi ölçümün seçildiğine bağlı olarak CPU veya bellek kullanımını gösterir. Klavyeden trend grafiğini seçtikten sonra, her çubuküzerinden tek tek geçiş yapmak için Alt+Page yukarı tuşunu veya Alt+Sayfa aşağı tuşunu kullanın. Barın üzerinde gezinirsen elde edeceğin detayları alırsın.

![Eğilim çubuğu grafiği gezinme örneği](./media/container-insights-analyze/containers-metric-trend-bar-01.png) 

Sonraki örnekte, listedeki ilk düğüm için *aks-nodepool1-*, **Kapsayıcılar** için değeri 9'dur. Bu değer, dağıtılan toplam kapsayıcı sayısının bir toplamıdır.

![Her düğüm başına kapsayıcıların yuvarlantı](./media/container-insights-analyze/containers-nodes-containerstotal.png)

Bu bilgiler, kümenizdeki düğümler arasında kapsayıcılar arasında uygun bir bakiyeye sahip olup olmadığınızı hızlı bir şekilde belirlemenize yardımcı olabilir. 

**Düğümler** sekmesini görüntülediğinizde sunulan bilgiler aşağıdaki tabloda açıklanmıştır.

| Sütun | Açıklama | 
|--------|-------------|
| Adı | Ana bilgisayarın adı. |
| Durum | Düğüm durumunuk kubernetes görünümü. |
| Min&nbsp;%,&nbsp;Avg %,&nbsp;50.%, 90.%&nbsp;%, 95.%,&nbsp;Max&nbsp;%  | Seçili süre boyunca yüzdelik dilime dayalı ortalama düğüm yüzdesi. |
| Min, Avg, 50, 90, 95th, Max | Ortalama düğümlerin seçilen süre boyunca yüzdelik dilimini temel alan gerçek değeri. Ortalama değer, düğüm için ayarlanan CPU/Bellek sınırından ölçülür. Bakla lar ve kapsayıcılar için, ana bilgisayar tarafından bildirilen ortalama değerdir. |
| Kapsayıcılar | Konteyner sayısı. |
| Uptime | Düğümün başladığı veya yeniden başlatıldırılan zamanı temsil eder. |
| Denetleyicisi | Sadece konteynerler ve baklalar için. Hangi denetleyicide bulunduğunu gösterir. Tüm bölmeler denetleyicide değildir, bu nedenle bazıları **N/A**görüntüleyebilir. | 
| Trend&nbsp;Min %,&nbsp;Avg %, 50%,&nbsp;%90,&nbsp;95.%,&nbsp;Max&nbsp;% | Çubuk grafik eğilimi denetleyicinin ortalama yüzdelik metrik yüzdesini temsil eder. |

**Diğer işlem**adlı bir düğümü genişlettikten sonra iş yükü fark edebilirsiniz. Düğümünüzde çalışan kapsayıcı olmayan işlemleri temsil eder ve şunları içerir:

* Konteynersiz kubernetes kendi kendini yöneten veya yönetilen Kubernetes

* Kapsayıcı çalışma zamanı süreçleri  

* Kubelet  

* Düğümünüzde çalışan sistem süreçleri

* Düğüm donanımı veya VM üzerinde çalışan diğer Kubernetes dışı iş yükleri

Şöyle hesaplanır: *Konteyner işleminden CAdvisor* - *Kullanımı'ndan toplam*kullanım.  

Seçicide **Denetleyicileri**seçin.

![Denetleyiciler görünümünü seçin](./media/container-insights-analyze/containers-controllers-tab.png)

Burada denetleyicilerinizin ve Konteyner Örnekleri sanal düğüm denetleyicilerinizin veya denetleyiciye bağlı olmayan sanal düğüm bölmelerinin performans durumunu görüntüleyebilirsiniz.

![\<Ad> denetleyicileri performans görünümü](./media/container-insights-analyze/containers-controllers-view.png)

Satır hiyerarşisi bir denetleyici ile başlar. Bir denetleyiciyi genişletdiğinizde, bir veya daha fazla bölmeyi görüntüleyebilirsiniz. Bir bölmeyi genişletin ve son satır da kapsüle gruplanmış kapsayıcıyı görüntüler. Genişletilmiş bir denetleyiciden, bu düğüm için filtre uygulanmış performans verilerini görüntülemek için üzerinde çalıştığı düğüme kadar ayrıntıya inebilirsiniz. Denetleyiciye bağlı olmayan Kapsayıcı Örnekleri bölmeleri listede en son listelenir.

![Listelenen Kapsayıcı Örnekleri bölmeleri ile Örnek Denetleyiciler hiyerarşisi](./media/container-insights-analyze/controllers-view-aci.png)

Belirli denetleyici için **Düğüm** sütunun altındaki değeri seçin.

![Performans görünümünde düğümden denetleyiciye örnek detaylandırma](./media/container-insights-analyze/drill-down-controller-node.png)

Denetleyicileri görüntülediğinizde görüntülenen bilgiler aşağıdaki tabloda açıklanmıştır.

| Sütun | Açıklama | 
|--------|-------------|
| Adı | Denetleyicinin adı.|
| Durum | *Tamam*, *Sonlandırılan*, *Durduruldu*, *Durduruldu*veya *Duraklatılmış*gibi durumla çalışan tamamlandıktan sonra kapsayıcıların toplama durumu . Kapsayıcı çalışıyorsa ancak durum düzgün görüntülenmiyorsa veya aracı tarafından alınmadıysa ve 30 dakikadan uzun süredir yanıt vermediyse, durum *bilinmiyor.* Durum simgesinin ek ayrıntıları aşağıdaki tabloda verilmiştir.|
| Min&nbsp;%,&nbsp;Avg %,&nbsp;50.%, 90.%&nbsp;%, 95.%,&nbsp;Max&nbsp;%| Seçili metrik ve yüzdelik dilim için her varlığın ortalama yüzdesinin toplama ortalaması. |
| Min, Avg, 50, 90, 95th, Max  | Seçilen yüzdelik alan için kapsayıcının ortalama CPU milicore veya bellek performansının birikme. Ortalama değer, bir bölme için ayarlanan CPU/Bellek sınırından ölçülür. |
| Kapsayıcılar | Denetleyici veya pod için toplam kapsayıcı sayısı. |
| Yeni -den başlatma | Kapsayıcılardan yeniden başlatma sayısının toplaması. |
| Uptime | Bir kapsayıcının başlamasından bu yana geçen zamanı temsil eder. |
| Node | Sadece konteynerler ve baklalar için. Hangi denetleyicide bulunduğunu gösterir. | 
| Trend&nbsp;Min %,&nbsp;Avg %, 50%,&nbsp;%90,&nbsp;95.%,&nbsp;Max&nbsp;% | Çubuk grafik eğilimi denetleyicinin ortalama yüzdelik ölçümünü temsil eder. |

Durum alanındaki simgeler kapsayıcıların çevrimiçi durumunu gösterir.
 
| Simge | Durum | 
|--------|-------------|
| ![Çalışma durumu simgesini hazırlayın](./media/container-insights-analyze/containers-ready-icon.png) | Çalışan (Hazır)|
| ![Bekleme veya Duraklatma durum simgesi](./media/container-insights-analyze/containers-waiting-icon.png) | Bekleme veya Duraklatma|
| ![Bildirilen son çalışan durum simgesi](./media/container-insights-analyze/containers-grey-icon.png) | En son çalışan rapor edildi ancak 30 dakikadan uzun süredir yanıt vermedi|
| ![Başarılı durum simgesi](./media/container-insights-analyze/containers-green-icon.png) | Başarıyla durduruldu veya durdurulamadı|

Durum simgesi, bölmenin sağladığına göre bir sayım görüntüler. En kötü iki durumu gösterir ve durumun üzerinde gezinirken, kapsayıcıdaki tüm bölmelerden bir toplama durumu görüntüler. Hazır bir durum yoksa durum değeri **(0)** görüntüler.

Seçicide **Kapsayıcılar'ı**seçin.

![Kapsayıcılar görünümünü seçin](./media/container-insights-analyze/containers-containers-tab.png)

Burada Azure Kubernetes ve Azure Kapsayıcı Örnekleri kapsayıcılarınızın performans durumunu görüntüleyebilirsiniz. 

![\<Ad> kapsayıcılar performans görünümü](./media/container-insights-analyze/containers-containers-view.png)

Bir kapsayıcıdan, bu nesne için filtre uygulanmış performans verilerini görüntülemek için bir bölme veya düğüme kadar delinebilirsiniz. Belirli bir kapsayıcı için **Pod** veya **Düğüm** sütunun altındaki değeri seçin.

![Performans görünümünde düğümden kapsayıcılara örnek detaylandırma](./media/container-insights-analyze/drill-down-controller-node.png)

Kapsayıcıları görüntülediğinizde görüntülenen bilgiler aşağıdaki tabloda açıklanmıştır.

| Sütun | Açıklama | 
|--------|-------------|
| Adı | Denetleyicinin adı.|
| Durum | Varsa konteynerlerin durumu. Durum simgesinin ek ayrıntıları sonraki tabloda sağlanır.|
| Min&nbsp;%,&nbsp;Avg %,&nbsp;50.%, 90.%&nbsp;%, 95.%,&nbsp;Max&nbsp;% | Seçili metrik ve yüzdelik dilim için her varlığın ortalama yüzdesinin toplaması. |
| Min, Avg, 50, 90, 95th, Max | Seçilen yüzdelik alan için kapsayıcının ortalama CPU milicore veya bellek performansının toplaması. Ortalama değer, bir bölme için ayarlanan CPU/Bellek sınırından ölçülür. |
| Pod | Kapsülün bulunduğu konteyner.| 
| Node |  Konteynırın bulunduğu düğüm. | 
| Yeni -den başlatma | Bir kapsayıcının başlamasından bu yana geçen zamanı temsil eder. |
| Uptime | Bir kapsayıcının başlatılmasından veya yeniden başlatılmasından bu yana geçen zamanı temsil eder. |
| Trend&nbsp;Min %,&nbsp;Avg %, 50%,&nbsp;%90,&nbsp;95.%,&nbsp;Max&nbsp;% | Çubuk grafik eğilimi, kapsayıcının ortalama yüzdelik metrik yüzdesini temsil eder. |

Durum alanındaki simgeler, aşağıdaki tabloda açıklandığı gibi bölmelerin çevrimiçi durumlarını gösterir.
 
| Simge | Durum |  
|--------|-------------|  
| ![Çalışma durumu simgesini hazırlayın](./media/container-insights-analyze/containers-ready-icon.png) | Çalışan (Hazır)|  
| ![Bekleme veya Duraklatma durum simgesi](./media/container-insights-analyze/containers-waiting-icon.png) | Bekleme veya Duraklatma|  
| ![Bildirilen son çalışan durum simgesi](./media/container-insights-analyze/containers-grey-icon.png) | En son çalışan rapor edildi ama 30 dakikadan fazla dır yanıt vermedi|  
| ![Sonlandırılmış durum simgesi](./media/container-insights-analyze/containers-terminated-icon.png) | Başarıyla durduruldu veya durdurulamadı|  
| ![Başarısız durum simgesi](./media/container-insights-analyze/containers-failed-icon.png) | Başarısız durum |  

## <a name="workbooks"></a>Çalışma Kitapları

Çalışma kitapları, metin, [günlük sorguları,](../log-query/query-language.md) [ölçümler](../platform/data-platform-metrics.md)ve parametreleri zengin etkileşimli raporlarda birleştirir. Çalışma kitapları, aynı Azure kaynaklarına erişimi olan diğer ekip üyeleri tarafından değiştirilebilir.

Kapsayıcılar için Azure Monitörü, başlamanızı sağlamak için dört çalışma kitabı içerir:

- **Disk kapasitesi**: Bir kap içinde düğüme sunulan her disk için aşağıdaki perspektifler tarafından etkileşimli disk kullanım grafikleri sunar:

    - Tüm diskler için disk yüzdekullanımı.
    - Tüm diskler için boş disk alanı.
    - Her düğümün diskini, kullanılan alanın yüzdesini, kullanılan alanın yüzdesini, boş disk alanı (GiB) eğilimini ve boş disk alanı eğilimini (GiB) gösteren bir ızgara. Tabloda bir satır seçildiğinde, satırın altında kullanılan alan ve boş disk alanı (GiB) yüzdesi gösterilir. 

- **Disk IO**: Bir kap içinde düğüme sunulan her disk için aşağıdaki perspektifler tarafından etkileşimli disk kullanım grafikleri sunar:

    - Disk I/O tüm disklerde bayt/sn okuyarak özetlenebilir, bayt/sn yazar ve bayt/sn eğilimlerini okuyup yazar.
    - Sekiz performans grafiği, disk G/Ç darboğazlarını ölçmeye ve tanımlamaya yardımcı olmak için önemli performans göstergeleri gösterir.

- **Kubelet**: Anahtar düğüm çalışma istatistiklerini gösteren iki ızgara içerir:

    - Düğüm ızgarası ile genel bakış, her düğüm için toplam işlemi, toplam hataları ve başarılı işlemleri yüzde ve eğilimle özetler.
    - Operasyon türüne göre genel bakış, her işlem için toplam işlemi, toplam hataları ve başarılı işlemleri yüzde ve eğilime göre özetler.

- **Ağ**: Her düğümün ağ bağdaştırıcısı için etkileşimli ağ kullanım grafikleri sunar ve bir ızgara ağ bağdaştırıcılarınızın performansını ölçmeye yardımcı olmak için temel performans göstergelerini sunar.

Bu çalışma kitaplarına, **Çalışma Kitaplarını Görüntüle** açılır listesinden her birini seçerek erişebilirsiniz.

![Çalışma Kitapları açılır listesini görüntüle](./media/container-insights-analyze/view-workbooks-dropdown-list.png)

## <a name="next-steps"></a>Sonraki adımlar

- DevOps veya operasyonel süreç ve yordamlarınızı desteklemek için yüksek CPU ve bellek kullanımı için uyarılar oluşturmayı öğrenmek [için kapsayıcılar için Azure Monitor ile performans uyarıları oluşturun'ı](container-insights-alerts.md) gözden geçirin.

- Kümelerinizi uyarmak, görselleştirmek veya analiz etmek için önceden tanımlanmış sorguları ve örnekleri görmek veya değerlendirmek için [günlük sorgu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin.

- Kubernetes kümenizin sistem durumu durumunu görüntüleme hakkında bilgi edinmek için [monitör küme durumunu](container-insights-health.md) görüntüleyin.
