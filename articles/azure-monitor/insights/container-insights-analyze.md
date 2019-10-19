---
title: Kapsayıcılar için Azure Izleyici ile AKS küme performansını izleme | Microsoft Docs
description: Bu makalede, kapsayıcılar için Azure Izleyici ile performansı ve günlük verilerini nasıl görüntüleyebileceğiniz ve çözümleyebileceğinizi anlatmaktadır.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 09/17/2019
ms.openlocfilehash: a65951a90767acce5570244af8a9250845e12def
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72554235"
---
# <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici ile AKS küme performansını anlayın
Kapsayıcılar için Azure Izleyici ile, iki perspektiften Azure Kubernetes hizmeti (AKS) kümelerinin iş yükünü izlemek için performans grafiklerini ve sistem durumu ' nu kullanabilirsiniz. Doğrudan bir AKS kümesinden izleyebilirsiniz veya Azure Izleyici 'deki bir abonelikteki tüm AKS kümelerini izleyebilirsiniz. Belirli bir AKS kümesini izlerken Azure Container Instances görüntüleme de mümkündür.

Bu makale, iki perspektifi anlamanıza ve Azure Izleyici 'nin algılanan sorunları hızlı bir şekilde değerlendirmenize, araştırmanıza ve çözmenize nasıl yardımcı olduğunu anlamanıza yardımcı olur.

Kapsayıcılar için Azure Izleyicisini etkinleştirme hakkında daha fazla bilgi için bkz. [kapsayıcılar Için Azure izleyicisini](container-insights-onboard.md)ekleme.

Azure Izleyici, tüm izlenen AKS kümelerinin sistem durumunu, aboneliklerinizdeki kaynak grupları arasında dağıtılan Linux ve Windows Server 2019 ' i çalıştıran bir çoklu küme görünümü sağlar. Çözüm tarafından izlenmeyen, bulunan AKS kümelerini gösterir. Küme durumunu hemen anlayabilir ve buradan düğüm ve denetleyici performans sayfasına gidebilir veya kümenin performans grafiklerini görmek için gidebilirsiniz. Algılanan ve izlenmeyen olarak tanımlanan AKS kümelerinde, her zaman için izlemeyi etkinleştirebilirsiniz. 

Bir Windows Server kümesini bir Linux kümesiyle karşılaştırılan kapsayıcılar için Azure Izleyici ile izlemenin başlıca farkları şunlardır:

- Bellek RSS ölçümü Windows düğüm ve kapsayıcıları için kullanılamaz.
- Disk depolama kapasitesi bilgileri Windows düğümleri için kullanılamaz.
- Canlı Günlükler desteği Windows kapsayıcı günlükleri hariç kullanılabilir.
- Docker ortamları değil yalnızca Pod ortamları izlenir.
- Önizleme sürümü ile en fazla 30 Windows Server kapsayıcısı desteklenir. Bu sınırlama, Linux kapsayıcıları için geçerlidir. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com)’ında oturum açın. 

## <a name="multi-cluster-view-from-azure-monitor"></a>Azure Izleyici 'den çok küme görünümü

Dağıtılan tüm AKS kümelerinin sistem durumunu görüntülemek için Azure portal sol bölmeden **izleyici** ' yi seçin. **Öngörüler** bölümünde **kapsayıcılar**' ı seçin. 

![Azure Izleyici çok küme panosu örneği](./media/container-insights-analyze/azmon-containers-multiview.png)

**İzlenen kümeler** sekmesinde şunları öğrenirsiniz:

- Kaç küme kritik veya sağlıksız durumda, ne kadar sağlıklı veya raporlama değil (bilinmeyen bir durum olarak adlandırılır).
- [Azure Kubernetes altyapısının (AKS-Engine)](https://github.com/Azure/aks-engine) dağıtımlarının tümünün sağlıklı olup olmadığı.
- Küme başına kaç düğüm ve Kullanıcı ve sistem düğüm dağıtımı yapılır.
- Kullanılabilir disk alanı miktarı ve bir kapasite sorunu var.

Dahil edilen sistem durumu durumları şunlardır: 

* **Sağlıklı**: sanal makine için herhangi bir sorun algılanmadı ve gereken şekilde çalışıyor. 
* **Kritik**: bir veya daha fazla kritik sorun, normal işlemsel durumu beklendiği gibi geri yüklemek için ele alınmalıdır.
* **Uyarı**: değinilmesi gereken bir veya daha fazla sorun algılandı veya sistem durumu koşulu kritik hale gelebilir.
* **Bilinmiyor**: hizmet, düğüm veya Pod ile bağlantı yapamadıysa, durum bilinmeyen bir duruma geçer.
* **Bulunamadı**: Bu çözüm için çalışma alanı, kaynak grubu ya da çalışma alanını içeren abonelik silindi.
* **Yetkisiz**: Kullanıcı çalışma alanındaki verileri okumak için gerekli izinlere sahip değil.
* **Hata**: çalışma alanından veri okunmaya çalışılırken bir hata oluştu.
* **Yanlış yapılandırılmış**: kapsayıcı Için Azure izleyici, belirtilen çalışma alanında doğru şekilde yapılandırılmadı.
* **Veri yok**: son 30 dakika boyunca veriler çalışma alanına bildirilmemiştir.

Sistem durumu, genel küme durumunu üç durumun *en kötü* durumu olarak bir özel durum olarak hesaplar. Üç durumdan biri bilinmiyorsa, genel küme durumu **bilinmiyor**olarak görünür. 

Aşağıdaki tabloda, çok küme görünümünde izlenen bir kümenin sistem durumunu denetleyen hesaplamanın bir dökümü verilmiştir.

| |Durum |Erişilebilirlik |  
|-------|-------|-----------------|  
|**Kullanıcı Pod**| | |  
| |Sorunsuz |%100 |  
| |Uyarı |90-99% |  
| |Kritik |%90 < |  
| |Bilinmiyor |Son 30 dakika içinde bildirilmezse |  
|**Sistem Pod**| | |  
| |Sorunsuz |%100 |
| |Uyarı |Yok |
| |Kritik |< %100 |
| |Bilinmiyor |Son 30 dakika içinde bildirilmezse |
|**Node** | | |
| |Sorunsuz |> 85% |
| |Uyarı |60-84% |
| |Kritik |%60 < |
| |Bilinmiyor |Son 30 dakika içinde bildirilmezse |

Küme listesinden, kümenin adını seçerek **küme** sayfasına ayrıntıya gidebilirsiniz. Ardından, söz konusu kümenin **düğümler** sütununda düğümlerin toplamasını seçerek **düğümlerin** performans sayfasına gidin. Ya da, **Kullanıcı Pod** veya **sistem Pod** sütununun toplamasını seçerek **denetleyiciler** performans sayfasına gidebilirsiniz.  

## <a name="view-performance-directly-from-an-aks-cluster"></a>Performansı bir AKS kümesinden doğrudan görüntüleme

Kapsayıcılar için Azure Izleyici 'ye erişim, sol bölmedeki **Öngörüler** ' i seçerek bir aks kümesinden doğrudan kullanılabilir. AKS kümeniz hakkında bilgiler dört perspektifle düzenlenmiştir:

- Küme
- Düğümler 
- Örleri 
- Kapsayıcılar

**Öngörüler**  > **kümesi**' ni seçtiğinizde varsayılan sayfa açılır. Dört satırlık performans grafikleri, kümenizin önemli performans ölçümlerini görüntüler. 

![Küme sekmesindeki örnek performans grafikleri](./media/container-insights-analyze/containers-cluster-perfview.png)

Performans grafiklerinde dört performans ölçümü görüntülenir:

- **Düğüm CPU kullanımı &nbsp; %** : tüm küme için CPU kullanımının toplanmış bir perspektifi. Zaman aralığı sonuçlarını filtrelemek için grafiğin üzerindeki yüzdebirlik değeri seçicideki **AVG**, **Min**, **50. Yüzdeliğini**, **90**, **95 TH**veya **Max** ' ı seçin. Filtreler tek tek veya birleştirilmiş olarak kullanılabilir. 
- **Düğüm bellek kullanımı &nbsp; %** : tüm küme için bellek kullanımının toplanmış bir perspektifi. Zaman aralığı sonuçlarını filtrelemek için grafiğin üzerindeki yüzdebirlik değeri seçicideki **AVG**, **Min**, **50. Yüzdeliğini**, **90**, **95 TH**veya **Max** ' ı seçin. Filtreler tek tek veya birleştirilmiş olarak kullanılabilir. 
- **Düğüm sayısı**: Kubernetes 'in düğüm sayısı ve durumu. Temsil edilen küme düğümlerinin durumları toplam, Ready ve Ready. Bunlar tek tek filtrelenebilir veya grafiğin üstündeki seçiciyle birleştirilebilir. 
- **Etkin Pod sayısı**: Kubernetes 'in Pod sayısı ve durumu. Temsil edilen durumların durumları toplam, bekleyen, çalışıyor, bilinmiyor, başarılı veya başarısız. Bunlar tek tek filtrelenebilir veya grafiğin üstündeki seçiciyle birleştirilebilir. 

Grafikteki her bir veri noktasında geçiş yapmak için sol ve sağ ok tuşlarını kullanın. Yüzdelik çizgiler arasında geçiş yapmak için yukarı ve aşağı ok tuşlarını kullanın. Seçili grafiği, görüntülediğiniz son Azure panosuna sabitlemek için, grafiklerden birinin sağ üst köşesindeki sabitleme simgesini seçin. Panodan grafiği yeniden boyutlandırabilir ve yeniden konumlandırabilirsiniz. Panodaki grafiğin seçilmesi sizi kapsayıcılar için Azure Izleyicisine yönlendirir ve doğru kapsamı ve görünümü yükler.

Kapsayıcılar için Azure Izleyici Ayrıca, kendi çizim grafiklerinizi oluşturabileceğiniz, eğilimleri araştırabileceğiniz ve araştırıp panolara sabitlerken Azure Izleyici [ölçümleri Gezginini](../platform/metrics-getting-started.md)da destekler. Ölçüm Gezgini ' nden, ölçümlerinizi [ölçüm tabanlı bir uyarı kuralının](../platform/alerts-metric.md)temeli olarak görselleştirmek için ayarladığınız ölçütleri de kullanabilirsiniz. 

## <a name="view-container-metrics-in-metrics-explorer"></a>Ölçüm Gezgininde kapsayıcı ölçümlerini görüntüleme

Ölçüm Gezgini ' nde, kapsayıcılar için Azure Izleyici 'den toplanmış düğüm ve pod kullanım ölçümlerini görüntüleyebilirsiniz. Aşağıdaki tabloda, kapsayıcı ölçümlerini görselleştirmek için ölçüm grafiklerini nasıl kullanacağınızı anlamanıza yardımcı olacak Ayrıntılar özetlenmektedir.

|uzayına | Ölçüm | Açıklama | 
|----------|--------|-------------|
| Öngörüler. kapsayıcı/düğümler | |
| | Cpuusagemiliçekirdekler | Küme genelinde CPU kullanımının toplu ölçümü. Bu, 1000 birimlerine bölünen bir CPU çekirdeğleridir (Milli = 1000). Birçok uygulamanın bir çekirdek kullandığı bir kapsayıcıda çekirdekler kullanımını belirlemede kullanılır.| 
| | cpuUsagePercentage | Küme genelinde yüzde cinsinden ölçülen ortalama CPU kullanımı.|
| | memoryRssBytes | Bayt cinsinden kullanılan kapsayıcı RSS belleği.| 
| | memoryRssPercentage | Kapsayıcıda kullanılan kapsayıcı RSS belleği yüzdesi.|
| | memoryWorkingSetBytes | Kapsayıcı çalışma kümesi belleği kullanıldı.| 
| | memoryWorkingSetPercentage | Kapsayıcı çalışma kümesi belleği yüzde olarak kullanıldı. | 
| | nodesCount | Kubernetes 'ten düğüm sayısı.|
| Öngörüler. kapsayıcı/pods | |
| | Pod sayısı | Kubernetes 'den Pod sayısı.|

Bir ölçümü, boyuta göre görüntülemek ve farklı segmentlerinin birbirleriyle nasıl karşılaştırılacağını görselleştirmek için [bölebilirsiniz](../platform/metrics-charts.md#apply-splitting-to-a-chart) . Bir düğüm için grafiği *konak* boyutuna göre segmentleyebilirsiniz. Pod 'dan aşağıdaki boyutlara göre segmentleyebilirsiniz:

* Kumandasını
* Kubernetes ad alanı
* Düğüm
* Aşama

## <a name="analyze-nodes-controllers-and-container-health"></a>Düğümleri, denetleyicileri ve kapsayıcı sistem durumunu çözümleme

**Düğümler**, **denetleyiciler**ve **kapsayıcılar** sekmelerine geçtiğinizde, sayfanın sağ tarafında otomatik olarak bir özellik bölmesi görüntülenir. Bu, Kubernetes nesnelerini düzenlemek için tanımladığınız etiketleri içeren seçili öğenin özelliklerini gösterir. Bir Linux düğümü seçildiğinde, **yerel disk kapasitesi** bölümü, düğüm için sunulan her disk için kullanılan disk alanını ve yüzdeyi de gösterir. Bölmeyi görüntülemek veya gizlemek için bölmedeki **>>** bağlantısını seçin.

![Örnek Kubernetes perspektif özellikleri bölmeleri](./media/container-insights-analyze/perspectives-preview-pane-01.png)

Hiyerarşideki nesneleri genişlettikten sonra Özellikler bölmesi, seçilen nesneye göre güncelleştirilir. Bölmeden, bölmenin üst kısmındaki **Kubernetes olay günlüklerini görüntüle** bağlantısını seçerek önceden tanımlanmış günlük aramalarıyla Kubernetes olaylarını da görüntüleyebilirsiniz. Kubernetes günlük verilerini görüntüleme hakkında daha fazla bilgi için bkz. [verileri çözümlemek Için arama günlükleri](container-insights-log-search.md). Küme kaynaklarını gözden geçirdikten sonra, kapsayıcı günlüklerini ve olayları gerçek zamanlı olarak görebilirsiniz. Bu özellik ve erişim vermek ve denetlemek için gereken yapılandırma hakkında daha fazla bilgi için bkz. [kapsayıcılar Için Azure izleyici ile günlükleri gerçek zamanlı olarak görüntüleme](container-insights-live-logs.md). 

Görüntüleme sonuçlarını **hizmet**, **düğüm**, **ad alanı**veya **düğüm havuzuna**göre filtrelemek için sayfanın üst kısmındaki **+ Filtre Ekle** seçeneğini kullanın. Filtre kapsamını seçtikten sonra, **Select value (s)** alanında gösterilen değerlerden birini seçin. Filtre yapılandırıldıktan sonra, AKS kümesinin herhangi bir perspektifi görüntülenirken Global olarak uygulanır. Formül yalnızca eşittir işaretini destekler. Sonuçlarınızı daha da daraltmak için birinciden daha fazla filtre ekleyebilirsiniz. Örneğin, **düğümüne**göre bir filtre belirtirseniz, ikinci filtre Için yalnızca **hizmet** veya **ad alanı** seçebilirsiniz.

![Sonuçları daraltmak için filtreyi kullanan örnek](./media/container-insights-analyze/add-filter-option-01.png)

Bir sekmede filtre belirtilmesi, başka bir sekmede seçim yaptığınızda çalışmaya devam eder. Belirtilen filtrenin yanındaki **x** simgesini seçtikten sonra silinir. 

**Düğümler** sekmesine geçin ve satır hiyerarşisi kümenizdeki bir düğümle başlayan Kubernetes nesne modelini izler. Düğümde çalışan bir veya daha fazla Pod 'yi görüntülemek için düğümü genişletin. Birden fazla kapsayıcı Pod olarak gruplandırılmışsa, hiyerarşide son satır olarak görüntülenir. Konakta işlemci veya bellek baskısı varsa Pod ile ilgili olmayan iş yüklerinin kaç tane üzerinde çalıştığını da görüntüleyebilirsiniz.

![Performans görünümündeki Kubernetes düğüm hiyerarşisinin örneği](./media/container-insights-analyze/containers-nodes-view.png)

Windows Server 2019 işletim sistemini çalıştıran Windows Server kapsayıcıları, listedeki tüm Linux tabanlı düğümlerden sonra gösterilir. Bir Windows Server düğümünü genişlettiğinizde, düğüm üzerinde çalışan bir veya daha fazla Pod ve kapsayıcıyı görüntüleyebilirsiniz. Düğüm seçildikten sonra Özellikler bölmesi sürüm bilgilerini gösterir. Windows Server düğümlerinde bir aracı yüklü olmadığından, aracı bilgileri dışlandı. 

![Windows Server düğümleri listelenen örnek düğüm hiyerarşisi](./media/container-insights-analyze/nodes-view-windows.png) 

Linux işletim sistemini çalıştıran Azure Container Instances sanal düğümler, listedeki son AKS kümesi düğümünden sonra gösterilir. Bir Container Instances sanal düğümünü genişlettiğinizde, düğüm üzerinde çalışan bir veya daha fazla Container Instances Pod ve kapsayıcıyı görüntüleyebilirsiniz. Ölçümler, yalnızca pods için düğümler için toplanmaz ve raporlanır.

![Container Instances listelenen örnek düğüm hiyerarşisi](./media/container-insights-analyze/nodes-view-aci.png)

Genişletilmiş bir düğümden, bu denetleyici için filtrelenmiş performans verilerini görüntülemek için düğüm üzerinde çalışan Pod veya kapsayıcıdan denetleyiciye gidebilirsiniz. Belirli bir düğüm için **Denetleyici** sütununun altındaki değeri seçin.
 
![Performans görünümünde düğümden denetleyiciye kadar örnek detaya gitme](./media/container-insights-analyze/drill-down-node-controller.png)

Bu nesneler için durum ve kaynak kullanımını gözden geçirmek için sayfanın üst kısmındaki denetleyiciler veya kapsayıcılar ' ı seçin. Bellek kullanımını gözden geçirmek için, **ölçüm** açılan LISTESINDE **bellek RSS** veya **bellek çalışma kümesi**' ni seçin. **Bellek RSS** yalnızca Kubernetes sürüm 1,8 ve üzeri için desteklenir. Aksi takdirde, bir tanımsız veya gösterilemeyen değeri temsil eden sayısal bir veri türü değeri olan, **en az &nbsp; %** *Nan &nbsp; %* olarak değerleri görüntülenir.

![Kapsayıcı düğümleri performans görünümü](./media/container-insights-analyze/containers-node-metric-dropdown.png)

**Bellek çalışma kümesi** , dahil edilen hem yerleşik belleği hem de sanal belleği (önbellek) gösterir ve uygulamanın kullandığı toplam şeydir. **Bellek RSS** yalnızca ana belleği gösterir (başka bir deyişle, diğer bir deyişle, yerleşik bellek). Bu ölçüm, kullanılabilir belleğin gerçek kapasitesini gösterir. Yerleşik bellek ve sanal bellek arasındaki fark nedir?

- Yerleşik bellek veya ana bellek, küme düğümlerinin kullanabileceği gerçek makine belleği miktarıdır.

- Sanal bellek, bellek baskısı altında verileri bellekten diske takas etmek için işletim sistemi tarafından kullanılan ayrılmış sabit disk alanı (önbellek) ve gerektiğinde belleğe geri getirir.

Varsayılan olarak, performans verileri son altı saati temel alır, ancak sol üstteki **Timerange** seçeneğini kullanarak pencereyi değiştirebilirsiniz. Ayrıca, yüzdebirlik seçicideki **Min**, **AVG**, **50. Yüzdeliğini**, **90**, **95 TH**ve **Max** ' ı seçerek zaman aralığındaki sonuçları filtreleyebilirsiniz. 

![Veri filtreleme için yüzdebirlik seçimi](./media/container-insights-analyze/containers-metric-percentile-filter.png)

**Eğilim** sütununun altındaki çubuk grafiğin üzerine geldiğinizde, her çubuk, hangi ölçümün seçili olduğuna bağlı olarak, 15 dakikalık bir dönem içinde CPU veya bellek kullanımını gösterir. Bir klavye aracılığıyla eğilim grafiğini seçtikten sonra, her bir çubukta tek tek geçiş yapmak için alt + sayfa yukarı tuşu veya alt + sayfa aşağı tuşunu kullanın. Çubuğun üzerine geldiğinizde yaptığınız aynı ayrıntıları alırsınız.

![Eğilim çubuğu grafiği üzerine gelme örneği](./media/container-insights-analyze/containers-metric-trend-bar-01.png) 

Sonraki örnekte, listedeki ilk düğüm için, *aks-nodepool1-* , **kapsayıcılar** değeri 9 ' dır. Bu değer, dağıtılan kapsayıcıların toplam sayısını temsil eden bir pakettir.

![Düğüm başına kapsayıcıların toplu örneği](./media/container-insights-analyze/containers-nodes-containerstotal.png)

Bu bilgiler, kümenizdeki düğümler arasında doğru kapsayıcılara sahip olup olmadığını hızlı bir şekilde belirlemenize yardımcı olabilir. 

**Düğümler** sekmesini görüntülerken sunulan bilgiler aşağıdaki tabloda açıklanmıştır.

| Sütun | Açıklama | 
|--------|-------------|
| Adı | Konağın adı. |
| Durum | Düğüm durumunun Kubernetes görünümü. |
| Min &nbsp;%, Ort &nbsp;%, 50. Yüzdeliğini &nbsp;%, &nbsp; 90 .%, 95. &nbsp;%, Max &nbsp; %  | Seçilen süre boyunca yüzdebirlik değerini temel alan ortalama düğüm yüzdesi. |
| En az, ortalama, 50 TH, 90 TH, en fazla | Ortalama düğümlerin fiili değeri, seçilen süre boyunca yüzdebirlik değerini temel alır. Ortalama değer bir düğüm için ayarlanan CPU/bellek sınırı üzerinden ölçülür. Pod ve kapsayıcılar için, ana bilgisayar tarafından bildirilen ortalama değerdir. |
| Kapsayıcılar | Kapsayıcı sayısı. |
| Hizmet | Bir düğümün başlatıldığı veya yeniden başlatıldığı zamandan itibaren geçen süreyi temsil eder. |
| Kumandasını | Yalnızca kapsayıcılar ve pods için. Bu, içinde hangi denetleyiciyi olduğunu gösterir. Tüm Pod 'ler denetleyicide olmadığından **bazıları yok olarak görüntülenebilir.** | 
| Eğilim min &nbsp;%, ortalama &nbsp;%, 50. &nbsp;%, &nbsp; 90 .%, 95. &nbsp;%, Max &nbsp; % | Çubuk grafik eğilimi, denetleyicinin ortalama yüzdebirlik ölçüm yüzdesini temsil eder. |

Seçicide **denetleyiciler**' i seçin.

![Denetleyici görünümünü seç](./media/container-insights-analyze/containers-controllers-tab.png)

Burada, denetleyicilerinizin performans durumunu ve sanal düğüm denetleyicilerini veya bir denetleyiciye bağlı olmayan sanal düğüm ayırlarını Container Instances görüntüleyebilirsiniz.

![\<Name > denetleyicileri performans görünümü](./media/container-insights-analyze/containers-controllers-view.png)

Satır hiyerarşisi bir denetleyici ile başlar. Bir denetleyiciyi genişlettiğinizde bir veya daha fazla Pod 'yi görüntüleyebilirsiniz. Pod ' ı genişletin ve son satır, Pod 'a gruplanmış kapsayıcıyı görüntüler. Genişletilmiş bir denetleyicide, bu düğüm için filtrelenmiş performans verilerini görüntülemek için üzerinde çalıştığı düğümün detayına gidebilirsiniz. Bir denetleyiciye bağlı olmayan Container Instances Pod listede son olarak listelenmiştir.

![Container Instances Pod listelenen örnek denetleyiciler hiyerarşisi](./media/container-insights-analyze/controllers-view-aci.png)

Belirli bir denetleyicinin **düğüm** sütununun altındaki değeri seçin.

![Performans görünümünde düğümden denetleyiciye kadar örnek detaya gitme](./media/container-insights-analyze/drill-down-controller-node.png)

Denetleyicileri görüntülerken görüntülenen bilgiler aşağıdaki tabloda açıklanmıştır.

| Sütun | Açıklama | 
|--------|-------------|
| Adı | Denetleyicinin adı.|
| Durum | Çalışma bittikten sonra, *Tamam*, *sonlandırıldı*, *başarısız*, *durduruldu*veya *duraklatıldı*gibi bir durumla çalıştıktan sonra kapsayıcıların toplama durumu. Kapsayıcı çalışıyorsa ancak durum doğru şekilde görüntülenmiyorsa veya aracı tarafından çekilmediyse ve 30 dakikadan uzun süredir yanıt vermediyse durum *bilinmiyor*demektir. Durum simgesinin ek ayrıntıları aşağıdaki tabloda verilmiştir.|
| Min &nbsp;%, Ort &nbsp;%, 50. Yüzdeliğini &nbsp;%, &nbsp; 90 .%, 95. &nbsp;%, Max &nbsp; %| Seçili ölçüm ve yüzdebirlik için her bir varlığın ortalama yüzdesinin toplam ortalaması. |
| En az, ortalama, 50 TH, 90 TH, en fazla  | Seçilen yüzdelik için kapsayıcının ortalama CPU frelicore veya bellek performansı toplamı. Ortalama değer bir pod için CPU/bellek sınırı kümesinden ölçülür. |
| Kapsayıcılar | Denetleyici veya pod için toplam kapsayıcı sayısı. |
| 'U | Kapsayıcılardan yeniden başlatma sayısı toplaması. |
| Hizmet | Kapsayıcının başlatılmasından bu yana geçen süreyi temsil eder. |
| Düğüm | Yalnızca kapsayıcılar ve pods için. Bu, içinde hangi denetleyiciyi olduğunu gösterir. | 
| Eğilim min &nbsp;%, ortalama &nbsp;%, 50. &nbsp;%, &nbsp; 90 .%, 95. &nbsp;%, Max &nbsp; % | Çubuk grafik eğilimi denetleyicinin ortalama yüzdebirlik ölçüsünü temsil eder. |

Durum alanındaki simgeler, kapsayıcıların çevrimiçi durumunu gösterir.
 
| Simg | Durum | 
|--------|-------------|
| ![Çalışma durumu simgesi hazırlanıyor](./media/container-insights-analyze/containers-ready-icon.png) | Çalışıyor (hazırlanıyor)|
| ![Bekleniyor veya duraklatıldı durum simgesi](./media/container-insights-analyze/containers-waiting-icon.png) | Bekliyor veya duraklatıldı|
| ![Son bildirilen çalışan durum simgesi](./media/container-insights-analyze/containers-grey-icon.png) | Son rapor çalışıyor ancak 30 dakikadan uzun süredir yanıt vermedi|
| ![Başarılı durum simgesi](./media/container-insights-analyze/containers-green-icon.png) | Başarıyla durduruldu veya durdurulamadı|

Durum simgesi, Pod 'un sağladığı şeyi temel alan bir sayı görüntüler. En kötü iki durumu gösterir ve durumun üzerine geldiğinizde, kapsayıcıdaki tüm yığınlardan bir toplama durumu görüntüler. Hazırlık durumu yoksa, durum değeri **(0)** değerini gösterir.

Seçicide **kapsayıcılar**' ı seçin.

![Kapsayıcılar görünümünü seç](./media/container-insights-analyze/containers-containers-tab.png)

Burada Azure Kubernetes ve Azure Container Instances kapsayıcılarınızın performans durumunu görüntüleyebilirsiniz. 

![\<Name > kapsayıcıları performans görünümü](./media/container-insights-analyze/containers-containers-view.png)

Bir kapsayıcıdan, bu nesne için filtrelenmiş performans verilerini görüntülemek için bir pod veya düğüm detayına gidebilirsiniz. Belirli bir kapsayıcı için **Pod** veya **node** sütununun altındaki değeri seçin.

![Performans görünümünde düğümden kapsayıcılara örnek detaya gitme](./media/container-insights-analyze/drill-down-controller-node.png)

Kapsayıcıları görüntülerken görüntülenen bilgiler aşağıdaki tabloda açıklanmıştır.

| Sütun | Açıklama | 
|--------|-------------|
| Adı | Denetleyicinin adı.|
| Durum | Kapsayıcıların durumu (varsa). Durum simgesinin ek ayrıntıları sonraki tabloda verilmiştir.|
| Min &nbsp;%, Ort &nbsp;%, 50. Yüzdeliğini &nbsp;%, &nbsp; 90 .%, 95. &nbsp;%, Max &nbsp; % | Seçili ölçüm ve yüzdebirlik için her bir varlığın ortalama yüzdesinin toplamı. |
| En az, ortalama, 50 TH, 90 TH, en fazla | Seçilen yüzdelik için kapsayıcının ortalama CPU milde veya bellek performansının toplamı. Ortalama değer bir pod için CPU/bellek sınırı kümesinden ölçülür. |
| Ayak | Pod 'un bulunduğu kapsayıcı.| 
| Düğüm |  Kapsayıcının bulunduğu düğüm. | 
| 'U | Kapsayıcının başlatılmasından bu yana geçen süreyi temsil eder. |
| Hizmet | Kapsayıcının başlatıldığı veya yeniden başlatıldığı zamandan itibaren geçen süreyi temsil eder. |
| Eğilim min &nbsp;%, ortalama &nbsp;%, 50. &nbsp;%, &nbsp; 90 .%, 95. &nbsp;%, Max &nbsp; % | Çubuk grafik eğilimi kapsayıcının ortalama yüzdebirlik ölçüm yüzdesini temsil eder. |

Durum alanındaki simgeler, aşağıdaki tabloda açıklandığı gibi, pods 'nin çevrimiçi durumlarını gösterir.
 
| Simg | Durum |  
|--------|-------------|  
| ![Çalışma durumu simgesi hazırlanıyor](./media/container-insights-analyze/containers-ready-icon.png) | Çalışıyor (hazırlanıyor)|  
| ![Bekleniyor veya duraklatıldı durum simgesi](./media/container-insights-analyze/containers-waiting-icon.png) | Bekliyor veya duraklatıldı|  
| ![Son bildirilen çalışan durum simgesi](./media/container-insights-analyze/containers-grey-icon.png) | Son bildirilen, ancak 30 dakikadan uzun bir süredir yanıt vermemiş|  
| ![Sonlandırılan durum simgesi](./media/container-insights-analyze/containers-terminated-icon.png) | Başarıyla durduruldu veya durdurulamadı|  
| ![Başarısız durum simgesi](./media/container-insights-analyze/containers-failed-icon.png) | Başarısız durum |  

## <a name="workbooks"></a>Çalışma Kitapları

Çalışma kitapları metin, [günlük sorgularını](../log-query/query-language.md), [ölçümleri](../platform/data-platform-metrics.md)ve parametreleri zengin etkileşimli raporlara birleştirir. Çalışma kitapları aynı Azure kaynaklarına erişimi olan diğer takım üyeleri tarafından düzenlenebilir.

Kapsayıcılar için Azure Izleyici, başlamanıza olanak sağlamak için dört çalışma kitabı içerir:

- **Disk kapasitesi**: aşağıdaki perspektiflere göre bir kapsayıcı içindeki düğüme sunulan her bir disk için etkileşimli disk kullanımı grafikleri sunar:

    - Tüm diskler için disk yüzdesi kullanımı.
    - Tüm diskler için boş disk alanı.
    - Her bir düğümün diskini, kullanılan alan yüzdesini, kullanılan alan yüzdesi, boş disk alanı (GiB) ve boş disk alanı eğilimi (GiB) gösteren bir kılavuz. Tabloda bir satır seçildiğinde, kullanılan alanın yüzdesi ve boş disk alanı (GiB), satırın altında gösterilir. 

- **DISK GÇ**: aşağıdaki perspektiflere göre bir kapsayıcı içindeki düğüme sunulan her bir disk için etkileşimli disk kullanımı grafikleri sunar:

    - Disk g/ç, okunan bayt/sn, bayt/sn yazma ve okuma ve yazma bayt/sn eğilimlerini ile tüm diskler arasında özetlenir.
    - Sekiz performans grafiği disk g/ç performans sorunlarını ölçmenize ve belirlemesine yardımcı olacak ana performans göstergelerini gösterir.

- **Kubelet**: anahtar düğüm işletim istatistiklerini gösteren iki kılavuz içerir:

    - Node Grid 'e göre genel bakış, her düğüm için yüzde ve eğilim bazında toplam işlem, toplam hata ve başarılı işlemleri özetler.
    - İşlem türüne göre genel bakış toplam işlem, toplam hata ve yüzde ve eğilim bazında başarılı işlemler için her bir işlem için özetler.

- **Ağ**: her bir düğümün ağ bağdaştırıcısı için etkileşimli ağ kullanım grafikleri sunar ve bir ızgara, ağ bağdaştırıcılarınızın performansını ölçmenize yardımcı olmak için ana performans göstergelerini sunar.

**Çalışma kitaplarını görüntüle** açılır listesinden her birini seçerek bu çalışma kitaplarına erişirsiniz.

![Çalışma kitaplarını görüntüle açılan listesi](./media/container-insights-analyze/view-workbooks-dropdown-list.png)

## <a name="next-steps"></a>Sonraki adımlar

- DevOps veya işletimsel işlemlerinizi ve yordamlarınızı desteklemek üzere yüksek CPU ve bellek kullanımı için uyarı oluşturmayı öğrenmek üzere [kapsayıcılar Için Azure izleyici ile performans uyarıları oluşturma](container-insights-alerts.md) konusunu inceleyin.
- Önceden tanımlanmış sorguları ve kümelerinizi uyarmak, görselleştirmek veya analiz etmek üzere değerlendirilecek veya özelleştirecek örnekleri görmek için [günlük sorgusu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin.
