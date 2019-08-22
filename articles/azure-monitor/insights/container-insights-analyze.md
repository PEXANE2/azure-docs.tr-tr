---
title: AKS küme performansını izleme Azure İzleyici ile kapsayıcılar için | Microsoft Docs
description: Bu makalede nasıl görüntüleyebilir ve kapsayıcılar için Azure İzleyici ile performans ve günlük verilerini analiz etme açıklanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/22/2019
ms.author: magoedte
ms.openlocfilehash: 154848c33960cb78b10c58e7a39ddec669d4fae0
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69872983"
---
# <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici ile AKS kümesi performansını anlama
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

[Azure Portal](https://portal.azure.com) oturum açın. 

## <a name="multi-cluster-view-from-azure-monitor"></a>Azure İzleyicisi'nden çoklu küme görüntüle

Dağıtılan tüm AKS kümelerinin sistem durumunu görüntülemek için Azure portal sol bölmeden **izleyici** ' yi seçin. Altında **Insights** bölümünden **kapsayıcıları**. 

![Azure İzleyici çok küme Panosu örneği](./media/container-insights-analyze/azmon-containers-multiview.png)

**İzlenen kümeler** sekmesinde şunları öğrenirsiniz:

- Kaç küme kritik veya sağlıksız durumda, ne kadar sağlıklı veya raporlama değil (bilinmeyen bir durum olarak adlandırılır).
- [Azure Kubernetes altyapısının (AKS-Engine)](https://github.com/Azure/aks-engine) dağıtımlarının tümünün sağlıklı olup olmadığı.
- Küme başına kaç düğüm ve Kullanıcı ve sistem düğüm dağıtımı yapılır.
- Kullanılabilir disk alanı miktarı ve bir kapasite sorunu var.

Dahil edilen sistem durumu durumlar şunlardır: 

* **Sağlıklı**: VM için herhangi bir sorun algılanmadı ve gereken şekilde çalışıyor. 
* **Kritik**: Normal işlemsel durumu beklenen şekilde geri yüklemek için giderilmesi gereken bir veya daha fazla kritik sorun algılandı.
* **Uyarı**: Giderilmesi gereken bir veya daha fazla sorun algılandı veya sağlık durumu kritik hale gelebilir.
* **Bilinmiyor**: Hizmet, düğüm veya Pod ile bağlantı yapamadıysa, durum bilinmeyen bir duruma geçer.
* **Bulunamadı**: Bu çözüm için çalışma alanı, kaynak grubu veya çalışma alanını içeren abonelik silindi.
* **Yetkisiz**: Kullanıcı çalışma alanındaki verileri okumak için gerekli izinlere sahip değil.
* **Hata**: Çalışma alanından veri okunmaya çalışılırken bir hata oluştu.
* **Yanlış yapılandırılmış**: Kapsayıcılar için Azure Izleyici, belirtilen çalışma alanında doğru şekilde yapılandırılmadı.
* **Veri yok**: Son 30 dakikalık veriler çalışma alanına bildirilmemiştir.

Sistem durumu, genel küme durumunu üç durumun *en kötü* durumu olarak bir özel durum olarak hesaplar. Üç durumdan biri bilinmiyorsa, genel küme durumu **bilinmiyor**olarak görünür. 

Aşağıdaki tabloda, çok küme görünümünde izlenen bir kümenin sistem durumunu denetleyen hesaplamanın bir dökümü verilmiştir.

| |Durum |Kullanılabilirlik |  
|-------|-------|-----------------|  
|**Kullanıcı Pod**| | |  
| |Sorunsuz |100% |  
| |Uyarı |90 - %99 |  
| |Kritik |< % 90'dır |  
| |Bilinmeyen |Son 30 dakika içerisinde bildirilmedi varsa |  
|**Sistem Pod**| | |  
| |Sorunsuz |100% |
| |Uyarı |Yok |
| |Kritik |< % 100 |
| |Bilinmeyen |Son 30 dakika içerisinde bildirilmedi varsa |
|**Node** | | |
| |Sorunsuz |> %85 |
| |Uyarı |60 - %84 |
| |Kritik |< % 60 |
| |Bilinmeyen |Son 30 dakika içerisinde bildirilmedi varsa |

Küme listesinden, kümenin adını seçerek **küme** sayfasına ayrıntıya gidebilirsiniz. Ardından, söz konusu kümenin **düğümler** sütununda düğümlerin toplamasını seçerek **düğümlerin** performans sayfasına gidin. Ya da, **Kullanıcı Pod** veya **sistem Pod** sütununun toplamasını seçerek **denetleyiciler** performans sayfasına gidebilirsiniz.  

## <a name="view-performance-directly-from-an-aks-cluster"></a>Bir AKS kümesi doğrudan performans görünümü

Kapsayıcılar için Azure Izleyici 'ye erişim, sol bölmedeki **Öngörüler** ' i seçerek bir aks kümesinden doğrudan kullanılabilir. AKS kümeniz hakkında bilgiler dört perspektifle düzenlenmiştir:

- Küme
- Düğümler 
- Denetleyiciler 
- Kapsayıcılar

**Öngörüler** > **kümesi**' ni seçtiğinizde varsayılan sayfa açılır. Dört satırlık performans grafikleri, kümenizin önemli performans ölçümlerini görüntüler. 

![Örnek performans grafiklerini küme sekmesinde](./media/container-insights-analyze/containers-cluster-perfview.png)

Performans grafiklerinde dört performans ölçümü görüntülenir:

- **Düğüm CPU kullanımı&nbsp;%** : Tüm küme için CPU kullanımının toplanmış bir perspektifi. Zaman aralığı sonuçlarını filtrelemek için grafiğin üzerindeki yüzdebirlik değeri seçicideki **AVG**, **Min**, **50. Yüzdeliğini**, **90**, **95 TH**veya **Max** ' ı seçin. Filtreler tek tek veya birleştirilmiş olarak kullanılabilir. 
- **Düğüm bellek kullanımı&nbsp;%** : Tüm küme için bellek kullanımının toplanmış bir perspektifi. Zaman aralığı sonuçlarını filtrelemek için grafiğin üzerindeki yüzdebirlik değeri seçicideki **AVG**, **Min**, **50. Yüzdeliğini**, **90**, **95 TH**veya **Max** ' ı seçin. Filtreler tek tek veya birleştirilmiş olarak kullanılabilir. 
- **Düğüm sayısı**: Kubernetes 'in düğüm sayısı ve durumu. Temsil edilen küme düğümlerinin durumları toplam, Ready ve Ready. Bunlar tek tek filtrelenebilir veya grafiğin üstündeki seçiciyle birleştirilebilir. 
- **Etkin Pod sayısı**: Kubernetes 'in Pod sayısı ve durumu. Temsil edilen durumların durumları toplam, bekleyen, çalışıyor, bilinmiyor, başarılı veya başarısız. Bunlar tek tek filtrelenebilir veya grafiğin üstündeki seçiciyle birleştirilebilir. 

Grafikteki her bir veri noktasında geçiş yapmak için sol ve sağ ok tuşlarını kullanın. Yüzdelik çizgiler arasında geçiş yapmak için yukarı ve aşağı ok tuşlarını kullanın. Seçili grafiği, görüntülediğiniz son Azure panosuna sabitlemek için, grafiklerden birinin sağ üst köşesindeki sabitleme simgesini seçin. Panodan grafiği yeniden boyutlandırabilir ve yeniden konumlandırabilirsiniz. Panodaki grafiğin seçilmesi sizi kapsayıcılar için Azure Izleyicisine yönlendirir ve doğru kapsamı ve görünümü yükler.

Kapsayıcılar için Azure Izleyici Ayrıca, kendi çizim grafiklerinizi oluşturabileceğiniz, eğilimleri araştırabileceğiniz ve araştırıp panolara sabitlerken Azure Izleyici [ölçümleri Gezginini](../platform/metrics-getting-started.md)da destekler. Ölçüm Gezgini ' nden, ölçümlerinizi [ölçüm tabanlı bir uyarı kuralının](../platform/alerts-metric.md)temeli olarak görselleştirmek için ayarladığınız ölçütleri de kullanabilirsiniz. 

## <a name="view-container-metrics-in-metrics-explorer"></a>Ölçüm Gezgininde kapsayıcı ölçümlerini görüntüleme

Ölçüm Gezgini ' nde, kapsayıcılar için Azure Izleyici 'den toplanmış düğüm ve pod kullanım ölçümlerini görüntüleyebilirsiniz. Aşağıdaki tabloda, kapsayıcı ölçümlerini görselleştirmek için ölçüm grafiklerini nasıl kullanacağınızı anlamanıza yardımcı olacak Ayrıntılar özetlenmektedir.

|Ad Alanı | Ölçüm |
|----------|--------|
| Öngörüler. kapsayıcı/düğümler | |
| | Cpuusagemiliçekirdekler |
| | cpuUsagePercentage |
| | memoryRssBytes |
| | memoryRssPercentage |
| | memoryWorkingSetBytes |
| | memoryWorkingSetPercentage |
| | nodesCount |
| Öngörüler. kapsayıcı/pods | |
| | Pod sayısı |

Bir ölçümü, boyuta göre görüntülemek ve farklı segmentlerinin birbirleriyle nasıl karşılaştırılacağını görselleştirmek için [bölebilirsiniz](../platform/metrics-charts.md#apply-splitting-to-a-chart) . Bir düğüm için grafiği *konak* boyutuna göre segmentleyebilirsiniz. Pod 'dan aşağıdaki boyutlara göre segmentleyebilirsiniz:

* Denetleyici
* Kubernetes ad alanı
* Düğüm
* Aşama

## <a name="analyze-nodes-controllers-and-container-health"></a>Düğümleri, denetleyicileri ve kapsayıcı sistem durumunu çözümleme

**Düğümler**, **denetleyiciler**ve **kapsayıcılar** sekmelerine geçtiğinizde, sayfanın sağ tarafında otomatik olarak bir özellik bölmesi görüntülenir. Bu, Kubernetes nesnelerini düzenlemek için tanımladığınız etiketleri içeren seçili öğenin özelliklerini gösterir. Bir Linux düğümü seçildiğinde, **yerel disk kapasitesi** bölümü, düğüm için sunulan her disk için kullanılan disk alanını ve yüzdeyi de gösterir. Bölmeyi görüntülemek veya gizlemek için bölmedeki **bağlantıyıseçin.>>**

![Örnek Kubernetes perspektif özellikleri bölmeleri](./media/container-insights-analyze/perspectives-preview-pane-01.png)

Özellikler bölmesinde güncelleştirmeleri hiyerarşideki nesneler genişlettikçe tabanlı seçili nesne üzerinde. Bölmeden, bölmenin üst kısmındaki **Kubernetes olay günlüklerini görüntüle** bağlantısını seçerek önceden tanımlanmış günlük aramalarıyla Kubernetes olaylarını da görüntüleyebilirsiniz. Kubernetes günlük verilerini görüntüleme hakkında daha fazla bilgi için bkz. [verileri çözümlemek Için arama günlükleri](container-insights-log-search.md). Küme kaynaklarını gözden geçirdikten sonra, kapsayıcı günlüklerini ve olayları gerçek zamanlı olarak görebilirsiniz. Bu özellik ve erişim vermek ve denetlemek için gereken yapılandırma hakkında daha fazla bilgi için bkz. [kapsayıcılar Için Azure izleyici ile günlükleri gerçek zamanlı olarak görüntüleme](container-insights-live-logs.md). 

Görüntüleme sonuçlarını **hizmet**, **düğüm**, **ad alanı**veya **düğüm havuzuna**göre filtrelemek için sayfanın üst kısmındaki **+ Filtre Ekle** seçeneğini kullanın. Filtre kapsamını seçtikten sonra, **Select value (s)** alanında gösterilen değerlerden birini seçin. Filtre yapılandırıldıktan sonra, AKS kümesinin herhangi bir perspektifi görüntülenirken Global olarak uygulanır. Formül eşittir işareti yalnızca destekler. Sonuçlarınızı daraltmak için birinci üzerine ek filtreler ekleyebilirsiniz. Örneğin, **düğümüne**göre bir filtre belirtirseniz, ikinci filtre Için yalnızca **hizmet** veya **ad alanı** seçebilirsiniz.

![Örnek sonuçları daraltmak için filtre kullanma](./media/container-insights-analyze/add-filter-option-01.png)

Bir sekmede filtre belirtilmesi, başka bir sekmede seçim yaptığınızda çalışmaya devam eder. Belirtilen filtrenin yanındaki **x** simgesini seçtikten sonra silinir. 

**Düğümler** sekmesine geçin ve satır hiyerarşisi kümenizdeki bir düğümle başlayan Kubernetes nesne modelini izler. Düğümde çalışan bir veya daha fazla Pod 'yi görüntülemek için düğümü genişletin. Birden fazla kapsayıcı Pod olarak gruplandırılmışsa, hiyerarşide son satır olarak görüntülenir. Konakta işlemci veya bellek baskısı varsa Pod ile ilgili olmayan iş yüklerinin kaç tane üzerinde çalıştığını da görüntüleyebilirsiniz.

![Performans görünümündeki Kubernetes düğüm hiyerarşisinin örneği](./media/container-insights-analyze/containers-nodes-view.png)

Windows Server 2019 işletim sistemini çalıştıran Windows Server kapsayıcıları, listedeki tüm Linux tabanlı düğümlerden sonra gösterilir. Bir Windows Server düğümünü genişlettiğinizde, düğüm üzerinde çalışan bir veya daha fazla Pod ve kapsayıcıyı görüntüleyebilirsiniz. Düğüm seçildikten sonra Özellikler bölmesi sürüm bilgilerini gösterir. Windows Server düğümlerinde bir aracı yüklü olmadığından, aracı bilgileri dışlandı. 

![Windows Server düğümleri listelenen örnek düğüm hiyerarşisi](./media/container-insights-analyze/nodes-view-windows.png) 

Linux işletim sistemini çalıştıran Azure Container Instances sanal düğümler, listedeki son AKS kümesi düğümünden sonra gösterilir. Bir Container Instances sanal düğümünü genişlettiğinizde, düğüm üzerinde çalışan bir veya daha fazla Container Instances Pod ve kapsayıcıyı görüntüleyebilirsiniz. Ölçümler, yalnızca pods için düğümler için toplanmaz ve raporlanır.

![Listelenen Container Instances ile örnek düğüm hiyerarşisi](./media/container-insights-analyze/nodes-view-aci.png)

Genişletilmiş bir düğümden, bu denetleyici için filtrelenmiş performans verilerini görüntülemek için düğüm üzerinde çalışan Pod veya kapsayıcıdan denetleyiciye gidebilirsiniz. Belirli bir düğüm için **Denetleyici** sütununun altındaki değeri seçin.
 
![Performans görünümünde düğümden denetleyiciye kadar örnek detaya gitme](./media/container-insights-analyze/drill-down-node-controller.png)

Bu nesneler için durum ve kaynak kullanımını gözden geçirmek için sayfanın üst kısmındaki denetleyiciler veya kapsayıcılar ' ı seçin. Bellek kullanımını gözden geçirmek için, **ölçüm** açılan LISTESINDE **bellek RSS** veya **bellek çalışma kümesi**' ni seçin. **Bellek RSS** yalnızca Kubernetes sürüm 1.8 ve üzeri desteklenir. Aksi takdirde, değerlerini görüntüleyin **Min&nbsp; %**  olarak *NaN&nbsp;%* , tanımlanmamış bir temsil eden sayısal veri türü değeri olan veya çıkarıldığında değeri.

**Bellek çalışma kümesi** , dahil edilen hem yerleşik belleği hem de sanal belleği (önbellek) gösterir ve uygulamanın kullandığı toplam şeydir. **Bellek RSS** yalnızca yerleşik bellek olan ana belleği gösterir. Bu ölçüm, kullanılabilir belleğin gerçek kapasitesini gösterir.

![Kapsayıcı düğümlerini performans görünümü](./media/container-insights-analyze/containers-node-metric-dropdown.png)

Varsayılan olarak, performans verileri son altı saati temel alır, ancak sol üstteki **Timerange** seçeneğini kullanarak pencereyi değiştirebilirsiniz. Ayrıca, yüzdebirlik seçicideki **Min**, **AVG**, **50. Yüzdeliğini**, **90**, **95 TH**ve **Max** ' ı seçerek zaman aralığındaki sonuçları filtreleyebilirsiniz. 

![Verileri filtreleme için yüzdebirlik seçimi](./media/container-insights-analyze/containers-metric-percentile-filter.png)

**Eğilim** sütununun altındaki çubuk grafiğin üzerine geldiğinizde, her çubuk, hangi ölçümün seçili olduğuna bağlı olarak, 15 dakikalık bir dönem içinde CPU veya bellek kullanımını gösterir. Bir klavye aracılığıyla eğilim grafiğini seçtikten sonra, her bir çubukta tek tek geçiş yapmak için alt + sayfa yukarı tuşu veya alt + sayfa aşağı tuşunu kullanın. Çubuğun üzerine geldiğinizde yaptığınız aynı ayrıntıları alırsınız.

![Eğilim çubuğu grafiği üzerine gelme örneği](./media/container-insights-analyze/containers-metric-trend-bar-01.png) 

Sonraki örnekte, listedeki ilk düğüm için, *aks-nodepool1-* , **kapsayıcılar** değeri 9 ' dır. Bu değer, dağıtılan kapsayıcıların toplam sayısını temsil eden bir pakettir.

![Düğüm başına kapsayıcıların toplu örneği](./media/container-insights-analyze/containers-nodes-containerstotal.png)

Bu bilgiler, kümenizdeki düğümler arasında doğru kapsayıcılara sahip olup olmadığını hızlı bir şekilde belirlemenize yardımcı olabilir. 

**Düğümler** sekmesini görüntülerken sunulan bilgiler aşağıdaki tabloda açıklanmıştır.

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Ana bilgisayar adı. |
| Durum | Düğüm durumu görünümünü Kubernetes. |
| En&nbsp;az%,&nbsp;ortalama%, 50&nbsp;.&nbsp;%, 90&nbsp;TH%,% 95, en fazla&nbsp;%  | Seçili süre ve çözüm oranlarına dayanarak ortalama düğüm yüzdesi. |
| En az, ortalama, 50 TH, 90 TH, en fazla | Ortalama düğümlerin fiili değeri, seçilen süre boyunca yüzdebirlik değerini temel alır. Ortalama değer bir düğüm için ayarlanan CPU/bellek sınırı üzerinden ölçülür. Pod ve kapsayıcılar için, ana bilgisayar tarafından bildirilen ortalama değerdir. |
| Kapsayıcılar | Kapsayıcı sayısı. |
| Çalışma Süresi | Bir düğüm başlatıldığında veya yeniden başlatıldıktan sonra süresini temsil eder. |
| Denetleyici | Yalnızca kapsayıcılar ve pod'ları için. Bu, içinde hangi denetleyiciyi olduğunu gösterir. Tüm pod'ların bazı görüntülenebilir bir denetleyici olduğundan **yok**. | 
| Eğilim en&nbsp;az%,&nbsp;ortalama%, 50&nbsp;.&nbsp;%, 90&nbsp;TH%, en fazla&nbsp;% | Çubuk grafik eğilimini denetleyici ortalama yüzdebirlik ölçüm yüzdesini temsil eder. |

Seçicide seçin **denetleyicileri**.

![Denetleyici görünümünü seç](./media/container-insights-analyze/containers-controllers-tab.png)

Burada, denetleyicilerinizin performans durumunu ve sanal düğüm denetleyicilerini veya bir denetleyiciye bağlı olmayan sanal düğüm ayırlarını Container Instances görüntüleyebilirsiniz.

![\<Ad > denetleyicileri performans görünümü](./media/container-insights-analyze/containers-controllers-view.png)

Satır hiyerarşisi bir denetleyici ile başlar. Bir denetleyiciyi genişlettiğinizde bir veya daha fazla Pod 'yi görüntüleyebilirsiniz. Pod ' ı genişletin ve son satır, Pod 'a gruplanmış kapsayıcıyı görüntüler. Genişletilmiş bir denetleyicide, bu düğüm için filtrelenmiş performans verilerini görüntülemek için üzerinde çalıştığı düğümün detayına gidebilirsiniz. Bir denetleyiciye bağlı olmayan Container Instances Pod listede son olarak listelenmiştir.

![Container Instances listelenen pod'ları ile örnek denetleyicileri hiyerarşisi](./media/container-insights-analyze/controllers-view-aci.png)

Belirli bir denetleyicinin **düğüm** sütununun altındaki değeri seçin.

![Performans görünümünde düğümden denetleyiciye kadar örnek detaya gitme](./media/container-insights-analyze/drill-down-controller-node.png)

Denetleyicileri görüntülerken görüntülenen bilgiler aşağıdaki tabloda açıklanmıştır.

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı.|
| Durum | Çalışma bittikten sonra, *Tamam*, *sonlandırıldı*, *başarısız*, *durduruldu*veya *duraklatıldı*gibi bir durumla çalıştıktan sonra kapsayıcıların toplama durumu. Kapsayıcı çalışıyorsa ancak durum doğru şekilde görüntülenmiyorsa veya aracı tarafından çekilmediyse ve 30 dakikadan uzun süredir yanıt vermediyse durum *bilinmiyor*demektir. Durum simgesinin ek ayrıntıları aşağıdaki tabloda verilmiştir.|
| En&nbsp;az%,&nbsp;ortalama%, 50&nbsp;.&nbsp;%, 90&nbsp;TH%,% 95, en fazla&nbsp;%| Her varlık için yüzdebirlik ve seçili ölçümün ortalama yüzdesi toplama ortalaması. |
| En az, ortalama, 50 TH, 90 TH, en fazla  | Seçilen yüzdelik dilim için kapsayıcı ortalama CPU millicore veya bellek performansını dökümü. Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Kapsayıcılar | Denetleyici veya pod için kapsayıcı toplam sayısı. |
| Yeniden başlatma | Yeniden başlatma sayısı kapsayıcılardan dökümü. |
| Çalışma Süresi | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Node | Yalnızca kapsayıcılar ve pod'ları için. Bu, içinde hangi denetleyiciyi olduğunu gösterir. | 
| Eğilim en&nbsp;az%,&nbsp;ortalama%, 50&nbsp;.&nbsp;%, 90&nbsp;TH%, en fazla&nbsp;% | Çubuk grafik eğilimini denetleyicinin ortalama yüzdebirlik ölçüm temsil eder. |

Durum alanındaki simgeler, kapsayıcıların çevrimiçi durumunu gösterir.
 
| Simge | Durum | 
|--------|-------------|
| ![Hazır çalışan durum simgesi](./media/container-insights-analyze/containers-ready-icon.png) | (Hazır) çalıştıran|
| ![Bekleniyor veya duraklatıldı durum simgesi](./media/container-insights-analyze/containers-waiting-icon.png) | Bekleyen veya duraklatıldı|
| ![Son durum simgesi çalıştıran bildirdi.](./media/container-insights-analyze/containers-grey-icon.png) | Son rapor çalışıyor ancak 30 dakikadan uzun süredir yanıt vermedi|
| ![Başarılı durum simgesi](./media/container-insights-analyze/containers-green-icon.png) | Başarılı bir şekilde durdurulmuş veya yanıt vermemesine başarısız|

Durum simgesi ne pod sağlar göre sayısını görüntüler. İki en kötü durum gösterir ve durum geldiğinizde, kapsayıcıda tüm pod'ların bir toplama durumunu görüntüler. Hazır durumda değilse, durum değeri görüntüler **(0)** .

Seçicide seçin **kapsayıcıları**.

![Kapsayıcılar görünümünü seç](./media/container-insights-analyze/containers-containers-tab.png)

Burada, Azure Kubernetes ve Azure Container Instances, kapsayıcılarınızın performans durumunu görüntüleyebilirsiniz. 

![\<Ad > kapsayıcıları performans görünümü](./media/container-insights-analyze/containers-containers-view.png)

Bir kapsayıcıdan, bir pod veya filtre bu nesne için performans verilerini görüntülemek için düğümü aşağı detaya gidebilirsiniz. Belirli bir kapsayıcı için **Pod** veya **node** sütununun altındaki değeri seçin.

![Performans görünümünde düğümden kapsayıcılara örnek detaya gitme](./media/container-insights-analyze/drill-down-controller-node.png)

Kapsayıcıları görüntülerken görüntülenen bilgiler aşağıdaki tabloda açıklanmıştır.

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı.|
| Durum | Kapsayıcılar, varsa durumu. Durum simgesi, ek ayrıntılar sonraki tabloda verilmiştir.|
| En&nbsp;az%,&nbsp;ortalama%, 50&nbsp;.&nbsp;%, 90&nbsp;TH%,% 95, en fazla&nbsp;% | Her varlık için yüzdebirlik ve seçili ölçümün ortalama yüzdesini dökümü. |
| En az, ortalama, 50 TH, 90 TH, en fazla | Ortalama CPU millicore veya bellek performans seçilen yüzdelik dilim kapsayıcısının dökümü. Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Pod | Pod bulunduğu kapsayıcısı.| 
| Node |  Kapsayıcı bulunduğu düğümü. | 
| Yeniden başlatma | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Çalışma Süresi | Kapsayıcı kullanmaya ya da yeniden beri süresini temsil eder. |
| Eğilim en&nbsp;az%,&nbsp;ortalama%, 50&nbsp;.&nbsp;%, 90&nbsp;TH%, en fazla&nbsp;% | Çubuk grafik eğilimini kapsayıcı ortalama yüzdebirlik ölçüm yüzdesini temsil eder. |

Durum alanındaki simgeler, aşağıdaki tabloda açıklandığı gibi, pods 'nin çevrimiçi durumlarını gösterir.
 
| Simge | Durum |  
|--------|-------------|  
| ![Hazır çalışan durum simgesi](./media/container-insights-analyze/containers-ready-icon.png) | (Hazır) çalıştıran|  
| ![Bekleniyor veya duraklatıldı durum simgesi](./media/container-insights-analyze/containers-waiting-icon.png) | Bekleyen veya duraklatıldı|  
| ![Son durum simgesi çalıştıran bildirdi.](./media/container-insights-analyze/containers-grey-icon.png) | Son çalıştırma bildirdi ancak 30 dakikadan fazla yanıt henüz|  
| ![Sonlandırılan durum simgesi](./media/container-insights-analyze/containers-terminated-icon.png) | Başarılı bir şekilde durdurulmuş veya yanıt vermemesine başarısız|  
| ![Başarısız durum simgesi](./media/container-insights-analyze/containers-failed-icon.png) | Durumu başarısız |  

## <a name="workbooks"></a>Çalışma Kitapları

Çalışma kitapları metin, [günlük sorgularını](../log-query/query-language.md), [ölçümleri](../platform/data-platform-metrics.md)ve parametreleri zengin etkileşimli raporlara birleştirir. Çalışma kitapları aynı Azure kaynaklarına erişimi olan diğer takım üyeleri tarafından düzenlenebilir.

Kapsayıcılar için Azure Izleyici, başlamanıza olanak sağlamak için dört çalışma kitabı içerir:

- **Disk kapasitesi**: Aşağıdaki perspektiflere göre bir kapsayıcı içindeki düğüme sunulan her bir disk için etkileşimli disk kullanımı grafikleri sunar:

    - Tüm diskler için disk yüzdesi kullanımı.
    - Tüm diskler için boş disk alanı.
    - Her bir düğümün diskini, kullanılan alan yüzdesini, kullanılan alan yüzdesi, boş disk alanı (GiB) ve boş disk alanı eğilimi (GiB) gösteren bir kılavuz. Tabloda bir satır seçildiğinde, kullanılan alanın yüzdesi ve boş disk alanı (GiB), satırın altında gösterilir. 

- **DISK GÇ**: Aşağıdaki perspektiflere göre bir kapsayıcı içindeki düğüme sunulan her bir disk için etkileşimli disk kullanımı grafikleri sunar:

    - Disk g/ç, okunan bayt/sn, bayt/sn yazma ve okuma ve yazma bayt/sn eğilimlerini ile tüm diskler arasında özetlenir.
    - Sekiz performans grafiği disk g/ç performans sorunlarını ölçmenize ve belirlemesine yardımcı olacak ana performans göstergelerini gösterir.

- **Kubelet**: Anahtar düğüm işletim istatistiklerini gösteren iki kılavuz içerir:

    - Node Grid 'e göre genel bakış, her düğüm için yüzde ve eğilim bazında toplam işlem, toplam hata ve başarılı işlemleri özetler.
    - İşlem türüne göre genel bakış toplam işlem, toplam hata ve yüzde ve eğilim bazında başarılı işlemler için her bir işlem için özetler.

- **Ağ**: Her düğümün ağ bağdaştırıcısı için etkileşimli ağ kullanım grafikleri sunar ve bir ızgara, ağ bağdaştırıcılarınızın performansını ölçmenize yardımcı olmak için ana performans göstergelerini sunar.

**Çalışma kitaplarını görüntüle** açılır listesinden her birini seçerek bu çalışma kitaplarına erişirsiniz.

![Çalışma kitaplarını görüntüle açılan listesi](./media/container-insights-analyze/view-workbooks-dropdown-list.png)

## <a name="next-steps"></a>Sonraki adımlar

- DevOps veya işletimsel işlemlerinizi ve yordamlarınızı desteklemek üzere yüksek CPU ve bellek kullanımı için uyarı oluşturmayı öğrenmek üzere [kapsayıcılar Için Azure izleyici ile performans uyarıları oluşturma](container-insights-alerts.md) konusunu inceleyin.
- Önceden tanımlanmış sorguları ve kümelerinizi uyarmak, görselleştirmek veya analiz etmek üzere değerlendirilecek veya özelleştirecek örnekleri görmek için [günlük sorgusu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin.
