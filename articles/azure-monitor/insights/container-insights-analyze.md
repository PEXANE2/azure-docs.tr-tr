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
ms.openlocfilehash: bbfc8cc61571de8b76ef1f7f0216501ef6d2cdee
ms.sourcegitcommit: b49431b29a53efaa5b82f9be0f8a714f668c38ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68377479"
---
# <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici ile AKS kümesi performansını anlama 
Kapsayıcılar için Azure İzleyici ile sistem durumu ve performans grafiklerini doğrudan bir AKS kümesi veya Azure aboneliğindeki tüm AKS kümeleri, iki perspektiften, Azure Kubernetes Service (AKS) kümesinin ve iş yükünü izlemek için kullanabilirsiniz İzleyici. Belirli bir AKS kümesi izlerken Azure Container Instances'a (ACI) görüntüleme olanağı da sağlar.

Bu makalede, iki perspektiften ve hızlı bir şekilde değerlendirmenize, araştırmanıza ve algılanan sorunları gidermek nasıl yardımcı olduğunu arasında deneyim anlamanıza yardımcı olur.

Kapsayıcılar için Azure İzleyici'ı etkinleştirme hakkında daha fazla bilgi için bkz: [kapsayıcılar için yerleşik Azure İzleyici](container-insights-onboard.md).

Azure Izleyici, tüm izlenen AKS kümelerinin sistem durumunu, aboneliklerinizde kaynak grupları arasında dağıtılan Linux ve Windows Server 2019 ' i çalıştıran bir çoklu küme görünümü sağlar.  Bu çözüm tarafından izlenmeyen AKS kümeleri bulunan gösterir. Küme durumu hemen anlayabilir ve buradan, düğüm ve denetleyici performans page DOWN ayrıntıya veya küme için performans grafikleri görmek için gidin.  Bulunan ve izlenmeyen olarak tanımlanan AKS kümeler için herhangi bir zamanda bu küme için izlemeyi etkinleştirebilirsiniz.  

Bir Linux kümesiyle karşılaştırılan kapsayıcılar için Azure Izleyici ile bir Windows Server kümesini izleyen başlıca farklar şunlardır:

- Bellek RSS ölçümü Windows düğüm ve kapsayıcıları için kullanılamaz.
- Disk depolama kapasitesi bilgileri Windows düğümlerinde kullanılamaz.
- Canlı Günlükler desteği Windows kapsayıcı günlükleri hariç kullanılabilir.
- Docker ortamları değil yalnızca Pod ortamları izlenir.
- Önizleme sürümü ile en fazla 30 Windows Server kapsayıcısı desteklenir. Bu sınırlama, Linux kapsayıcıları için geçerlidir.  

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com) oturum açın. 

## <a name="multi-cluster-view-from-azure-monitor"></a>Azure İzleyicisi'nden çoklu küme görüntüle

Dağıtılan tüm AKS küme sistem durumunu görüntülemek için seçin **İzleyici** Azure portalının sol bölmesinden.  Altında **Insights** bölümünden **kapsayıcıları**.  

![Azure İzleyici çok küme Panosu örneği](./media/container-insights-analyze/azmon-containers-multiview.png)

Üzerinde **izlenen kümeleri** sekmesinde tarafından aşağıdaki öğrenin:

1. Kaç (Bilinmeyen durumu adlandırılır) sağlam veya Raporlama olmayan karşı kritik veya sağlıksız durumda kaç kümeleri misiniz?
2. Tümü my [Azure Kubernetes altyapısı (altyapısı AKS)](https://github.com/Azure/aks-engine) dağıtımları Sağlıklı?
3. Küme başına kaç düğüm, Kullanıcı ve sistem Pod dağıtıldı?
4. Kullanılabilir disk alanı miktarı ve bir kapasite sorunu var mı?

Dahil edilen sistem durumu durumlar şunlardır: 

* **Sağlıklı** – sanal makine için herhangi bir sorun algılandı ve gerektiği gibi çalıştığını. 
* **Kritik** – bir veya daha fazla kritik sorunlar algılandığında, beklendiği gibi normal işletimsel durumunu geri yüklemek için ele alınması gerekiyor.
* **Uyarı** -bir veya daha fazla sorun algılandığında, ilgilenilmesi gereken ya da sağlık durumu kritik duruma gelebilir.
* **Bilinmeyen** – hizmet düğümü veya pod, durum değişikliklerini bilinmeyen bir duruma sahip bir bağlantı yapmak mümkün değildi.
* **Nebyl nalezen** - ya da çalışma alanı, kaynak grubuna ya da silinmiş olan bu çözüm için çalışma alanı içeren bir abonelik.
* **Yetkisiz** -kullanıcı, çalışma alanındaki verileri okumak için gerekli izinlere sahip değil.
* **Hata** -çalışma alanından verileri okuma çalışılırken hata oluştu.
* **Yanlış yapılandırılmış** -belirtilen çalışma alanında kapsayıcılar Için Azure izleyici doğru şekilde yapılandırılmadı.
* **Hiçbir veri** -veri değil olarak son 30 dakika içerisinde çalışma alanına bildirdi.

Sistem durumu, genel küme durumunu tek bir özel durum ile üç durumun en *kötü* durumu olarak hesaplar; üç durum *bilinmiyorsa*, genel küme durumu **bilinmiyor**olarak görünür.  

Aşağıdaki tabloda, izlenen bir küme çoklu küme görünüm için sağlık durumlarını denetleme hesaplama dökümünü sağlar.

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

Aşağı inebilir kümeleri listesinden **küme** için küme adına tıklayarak sayfayı **düğümleri** düğümlerin toplamını tıklayarak performans sayfası **düğümleri** sütun söz konusu belirli küme veya detaya gitmesine **denetleyicileri** toplamını tıklayarak performans sayfası **kullanıcı pod'ların** veya **sistem pod'ların**sütun.   

## <a name="view-performance-directly-from-an-aks-cluster"></a>Bir AKS kümesi doğrudan performans görünümü

Kapsayıcılar için Azure İzleyici erişim, doğrudan bir AKS kümesi ' kullanılabilir seçerek **Insights** sol bölmesinden. AKS kümenizi hakkında bilgi görüntüleme, dört Perspektifler düzenlenmiştir:

- Küme
- Düğümler 
- Denetleyiciler  
- Kapsayıcılar

Varsayılan sayfayı tıkladığınızda açılan **Insights** olduğu **küme**, kümenizin temel performans ölçümlerini görüntüleme dört satırı performans grafiklerini içerir. 

![Örnek performans grafiklerini küme sekmesinde](./media/container-insights-analyze/containers-cluster-perfview.png)

Performans grafiği dört performans ölçümlerini görüntüler:

- **Düğüm CPU kullanımı&nbsp;%** : Tüm küme için CPU kullanımının toplanmış bir perspektifi. Zaman aralığı için sonuç seçerek filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirliklerini Seçici grafiğin içinde ya da tek tek veya birlikte. 
- **Düğüm bellek kullanımı&nbsp;%** : Tüm küme için bellek kullanımının toplanmış bir perspektifi. Zaman aralığı için sonuç seçerek filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirliklerini Seçici grafiğin içinde ya da tek tek veya birlikte. 
- **Düğüm sayısı**: Kubernetes 'in düğüm sayısı ve durumu. Durumlar temsil küme düğümlerinin *tüm*, *hazır*, ve *hazır değil* ve ayrı olarak filtrelenen veya grafiğin üstünde Seçici içinde birleştirilmiş. 
- **Etkinlik Pod sayısı**: Kubernetes 'in Pod sayısı ve durumu. Temsil edilen pod'ları, durumlar *tüm*, *bekleyen*, *çalıştıran*, ve *bilinmeyen* ve ayrı olarak filtrelenen veya içinde birleşik grafiğin üstünde Seçici. 

Sağ/sağ ok tuşlarını, grafikteki her bir veri noktasında geçiş yapmak için ve yukarı/aşağı ok tuşlarını kullanarak yüzdelik çizgiler arasında geçiş yapabilirsiniz. Grafiklerin sağ üst köşesindeki sabitleme simgesine tıkladığınızda seçili grafik, son görüntülediğiniz son Azure panosuna sabitedilir. Panodan grafiği yeniden boyutlandırabilir ve yeniden konumlandırabilirsiniz. Panodaki grafiğin seçilmesi sizi kapsayıcılar için Azure Izleyicisine yönlendirir ve doğru kapsamı ve görünümü yükler.

Kapsayıcılar için Azure Izleyici Ayrıca, kendi çizim grafiklerinizi oluşturabileceğiniz, eğilimleri araştırabileceğiniz ve araştırıp panolara sabitlerken Azure Izleyici [ölçümleri Gezginini](../platform/metrics-getting-started.md)da destekler. Ölçüm Gezgini ' nden, [ölçüm tabanlı bir uyarı kuralının](../platform/alerts-metric.md)temeli olarak ölçümlerinizi görselleştirmek için ayarladığınız ölçütleri de kullanabilirsiniz.  

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

Ölçüm [bölmesini](../platform/metrics-charts.md#apply-splitting-to-a-chart) , boyuta göre görüntülemek ve farklı segmentlerinin birbirleriyle nasıl karşılaştırılacağını görselleştirmek için uygulayabilirsiniz. Bir düğüm için grafiği *konak* boyutuna göre segmentleyebilir ve pod 'dan aşağıdaki boyutlara göre segmentleyebilirsiniz:

* Denetleyici
* Kubernetes ad alanı
* Düğüm
* Aşama

## <a name="analyze-nodes-controllers-and-container-health"></a>Düğümleri, denetleyicileri ve kapsayıcı sistem durumunu çözümleme

İçin değiştirdiğinizde **düğümleri**, **denetleyicileri**, ve **kapsayıcıları** sekmesinde, otomatik olarak sağ tarafında sayfasının görüntülenmesini olan özellik bölmesi. Kubernetes nesnelerini düzenlemek için tanımladığınız Etiketler de dahil olmak üzere, öğenin özelliklerini gösterir. Bir Linux düğümü seçildiğinde, bu, düğüm için **yerel disk kapasitesi** kullanılabilir disk alanının ve düğüm tarafından sunulan her disk için kullanılan yüzde bölümünde de gösterilir. Tıklayarak **>>** bölmesinde bölmesinde view\hide için bağlantı. 

![Örnek Kubernetes Perspektifler Özellikler bölmesi](./media/container-insights-analyze/perspectives-preview-pane-01.png)

Özellikler bölmesinde güncelleştirmeleri hiyerarşideki nesneler genişlettikçe tabanlı seçili nesne üzerinde. Bölmesinden, ayrıca önceden tanımlanmış günlük aramalarının Kubernetes olaylarıyla tıklayarak görüntüleyebilirsiniz **görünümü Kubernetes olay günlüklerini** bölmenin üstündeki bağlantısı. Kubernetes günlük verilerini görüntüleme hakkında daha fazla bilgi için bkz. [arama verileri çözümlemek için günlükleri](container-insights-log-search.md). Küme kaynaklarını gözden geçirirken, kapsayıcı günlüklerini ve olayları gerçek zamanlı olarak görebilirsiniz. Bu özellik ve erişim vermek ve denetlemek için gereken yapılandırma hakkında daha fazla bilgi için bkz. [kapsayıcılar Için Azure izleyici ile günlükleri gerçek zamanlı görüntüleme](container-insights-live-logs.md). 

Görüntüleme sonuçlarını **hizmet**, **düğüm**, **ad alanı**veya **düğüm havuzuna** göre filtrelemek için, sayfanın üst kısmından **+ Filtre Ekle** seçeneğini kullanın ve filtre kapsamını seçtikten sonra, burada gösterilen değerlerden birini seçersiniz. **Değer (ler) i seçin** .  Filtre yapılandırdıktan sonra AKS kümesinin herhangi bir perspektif görüntülerken genel olarak uygulanır.  Formül eşittir işareti yalnızca destekler.  Sonuçlarınızı daraltmak için birinci üzerine ek filtreler ekleyebilirsiniz.  Örneğin, bir filtre tarafından belirtilen **düğüm**, ikinci filtreniz yalnızca seçmenizi izin **hizmet** veya **Namespace**.  

![Örnek sonuçları daraltmak için filtre kullanma](./media/container-insights-analyze/add-filter-option-01.png)

Bir sekmede filtre belirtme devam başka birini seçin ve tıkladığınızda silindiğinde uygulanacak **x** yanında belirtilen filtre simgesi.   

Geçiş **düğümleri** sekmesi ve satır hiyerarşi izler Kubernetes nesne modeli, kümenizdeki bir düğümü başlatma. Düğümünü genişletin ve düğüm üzerinde çalışan bir veya daha fazla pod'ları görüntüleyebilirsiniz. Birden fazla kapsayıcı için bir pod gruplandırılmışsa, hiyerarşideki son satırı olarak görüntülenir. Ayrıca, ana bilgisayar işlemci veya bellek baskısı varsa kaç pod harici ilgili iş konakta çalışan görüntüleyebilirsiniz.

![Performans Görünümü'nde örnek Kubernetes düğüm hiyerarşisi](./media/container-insights-analyze/containers-nodes-view.png)

Windows Server 2019 işletim sistemini çalıştıran Windows Server kapsayıcıları, listedeki tüm Linux tabanlı düğümlerden sonra gösterilir. Bir Windows Server düğümünü genişlettiğinizde, düğümde çalışan bir veya daha fazla düğüm ve kapsayıcıyı görüntüleyebilirsiniz. Bir düğüm seçildiğinde, Özellikler bölmesi, Windows Server düğümlerinde bir aracı yüklü olmadığından, aracı bilgileri hariç olmak üzere sürüm bilgilerini gösterir.  

![Windows Server düğümleri listelenen örnek düğüm hiyerarşisi](./media/container-insights-analyze/nodes-view-windows.png) 

Azure kapsayıcı örnekleri sanal Linux işletim sistemini çalıştıran düğümleri sonra son AKS küme düğümü listesinde gösterilir.  ACI sanal düğümünü genişlettiğinizde, bir veya daha fazla ACI pod'ların ve düğüm üzerinde çalışan kapsayıcılar görüntüleyebilirsiniz.  Ölçümleri değil toplanır ve düğümler için yalnızca pod'ların bildirdi.

![Listelenen Container Instances ile örnek düğüm hiyerarşisi](./media/container-insights-analyze/nodes-view-aci.png)

Genişletilmiş bir düğümden pod veya filtre söz konusu denetleyici için performans verilerini görüntülemek için denetleyiciye düğümde çalışan kapsayıcı detaya gidebilirsiniz. Değerin altında tıklayın **denetleyicisi** sütun için belirli bir düğümün.   
![Örneği detaya gitme düğümünden performans görünümü denetleyicisi](./media/container-insights-analyze/drill-down-node-controller.png)

Denetleyicileri veya sayfanın üstündeki kapsayıcılar'ı seçin ve bu nesneler için durumu ve kaynak kullanımını gözden geçirin.  Bunun yerine, bellek kullanımı, gözden geçirmek istiyorsanız **ölçüm** aşağı açılan listesinden **bellek RSS** veya **bellek çalışma kümesi**. **Bellek RSS** yalnızca Kubernetes sürüm 1.8 ve üzeri desteklenir. Aksi takdirde, değerlerini görüntüleyin **Min&nbsp; %**  olarak *NaN&nbsp;%* , tanımlanmamış bir temsil eden sayısal veri türü değeri olan veya çıkarıldığında değeri.

Bellek çalışma kümesi, dahil edilen hem yerleşik belleği hem de sanal belleği (önbellek) gösterir ve uygulamanın kullandığı toplam şeydir. Bellek RSS yalnızca yerleşik bellek olan ana belleği gösterir. Bu ölçüm, kullanılabilir belleğin gerçek kapasitesini gösterir.

![Kapsayıcı düğümlerini performans görünümü](./media/container-insights-analyze/containers-node-metric-dropdown.png)

Varsayılan olarak, son altı saat üzerinde performans verilerini bağlıdır, ancak penceresini kullanarak değiştirebileceğiniz **TimeRange** sol üst seçeneği. Seçerek zaman aralığı içinde sonuçlarını filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirlik Seçici içinde. 

![Verileri filtreleme için yüzdebirlik seçimi](./media/container-insights-analyze/containers-metric-percentile-filter.png)

Çubuk grafiğin üzerine fare zaman **eğilim** sütun, her bir çubuğun gösterir, bağlı olarak ölçüm seçili 15 dakika içinde bir örnek süresi, CPU veya bellek kullanımı. Bir klavye aracılığıyla eğilim grafiğini seçtikten sonra, alt + PageUp veya alt + PageDown tuşlarını kullanarak her bir çubukta tek tek geçiş yapabilir ve mouseover üzerinde yaptığınız gibi aynı ayrıntıları alabilirsiniz.

![Eğilim Çubuğu grafik örneği üzerine gelme](./media/container-insights-analyze/containers-metric-trend-bar-01.png)    

Sonraki örnekte, - listedeki ilk Not düğüm *aks-nodepool1 -* , değeri **kapsayıcıları** toplu dağıtılan kapsayıcıların toplam sayısı: % 9, olmasıdır.

![Kapsayıcı düğümü örnek başına dökümü](./media/container-insights-analyze/containers-nodes-containerstotal.png)

Kapsayıcı düğümleri arasında uygun bir denge olup olmadığını hızlıca belirlemenize yardımcı olabilir. 

Düğümleri görüntülediğinizde sunulan bilgiler aşağıdaki tabloda açıklanmıştır:

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Ana bilgisayar adı. |
| Durum | Düğüm durumu görünümünü Kubernetes. |
| Ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Seçili süre ve çözüm oranlarına dayanarak ortalama düğüm yüzdesi. |
| Ortalama, Min, maks., 50, 90 | Seçili, saati süresi sırasında ve çözüm oranlarına dayanarak ortalama düğümleri gerçek değeri. Ortalama değer, bir düğüm için CPU/bellek sınırını zamandan ölçülür; pod'ların ve kapsayıcılar için ana bilgisayar tarafından bildirilen ortalama değeri var. |
| Kapsayıcılar | Kapsayıcı sayısı. |
| Çalışma Süresi | Bir düğüm başlatıldığında veya yeniden başlatıldıktan sonra süresini temsil eder. |
| Denetleyiciler | Yalnızca kapsayıcılar ve pod'ları için. Bu, içinde bulunan hangi denetleyicisinin gösterir. Tüm pod'ların bazı görüntülenebilir bir denetleyici olduğundan **yok**. | 
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Çubuk grafik eğilimini denetleyici ortalama yüzdebirlik ölçüm yüzdesini temsil eder. |

Seçicide seçin **denetleyicileri**.

![Select denetleyicileri görüntüle](./media/container-insights-analyze/containers-controllers-tab.png)

Burada denetleyicileri ve ACI sanal düğümü denetleyicileri veya bir denetleyiciye bağlı olmayan sanal düğümü pod'ları performans durumunu görüntüleyebilirsiniz.

![\<Ad > denetleyicileri performans görünümü](./media/container-insights-analyze/containers-controllers-view.png)

Satır hiyerarşi denetleyicisi ile başlar ve bir denetleyici genişlettiğinizde, pod'ların bir veya daha fazla görüntüleyin.  Pod'ı genişletin ve son satırını pod'u gruplandırılmış kapsayıcı görüntüler. Genişletilmiş bir denetleyiciden bu düğüm için filtrelenen performans verilerini görüntülemek için çalıştığı düğüm kadar detaya gidebilirsiniz. ACI pod'ların bir denetleyiciye bağlı olmayan son listede listelenir.

![Container Instances listelenen pod'ları ile örnek denetleyicileri hiyerarşisi](./media/container-insights-analyze/controllers-view-aci.png)

Değerin altında tıklayın **düğüm** belirli denetleyicisi için sütun.   

![Örnek detaya gitme; düğümünden denetleyicisine performans görünümü](./media/container-insights-analyze/drill-down-controller-node.png)

Denetleyicileri görüntülediğinizde görüntülenen bilgileri aşağıdaki tabloda açıklanmıştır:

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı.|
| Durum | Durumu ile çalıştırılması tamamlandığında kapsayıcıları toplama durumunu *Tamam*, *kesildi*, *başarısız* *durduruldu*, veya *Duraklatıldı*. Kapsayıcıyı çalıştıran, ancak durumu ya da düzgün şekilde görüntülenmesini veya aracı tarafından toplanmış değil ve 30 dakikadan fazla yanıt vermedi: durumudur *bilinmeyen*. Ek ayrıntılar durumu simgesinin aşağıdaki tabloda verilmiştir.|
| Ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Seçili ölçüm ve yüzdebirlik için her bir varlığın ortalama yüzdesinin ortalamasını topla. |
| Ortalama, Min, maks., 50, 90  | Seçilen yüzdelik için kapsayıcının ortalama CPU frelicore veya bellek performansını toplama. Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Kapsayıcılar | Denetleyici veya pod için kapsayıcı toplam sayısı. |
| Yeniden başlatma | Kapsayıcılardan yeniden başlatma sayısını topla. |
| Çalışma Süresi | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Node | Yalnızca kapsayıcılar ve pod'ları için. Bu, bulunan hangi denetleyicisinin gösterir. | 
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;%| Çubuk grafik eğilimini denetleyicinin ortalama yüzdebirlik ölçüm temsil eder. |

Durum alanı simgeleri kapsayıcıları çevrimiçi durumunu gösterir:
 
| Simge | Durum | 
|--------|-------------|
| ![Hazır çalışan durum simgesi](./media/container-insights-analyze/containers-ready-icon.png) | (Hazır) çalıştıran|
| ![Bekleyen veya duraklatıldı durum simgesi](./media/container-insights-analyze/containers-waiting-icon.png) | Bekleyen veya duraklatıldı|
| ![Son durum simgesi çalıştıran bildirdi.](./media/container-insights-analyze/containers-grey-icon.png) | Son çalıştırma bildirdi ancak 30 dakikadan fazla yanıt henüz|
| ![Başarılı durum simgesi](./media/container-insights-analyze/containers-green-icon.png) | Başarılı bir şekilde durdurulmuş veya yanıt vermemesine başarısız|

Durum simgesi ne pod sağlar göre sayısını görüntüler. İki en kötü durum gösterir ve durum geldiğinizde, kapsayıcıda tüm pod'ların bir toplama durumunu görüntüler. Hazır durumda değilse, durum değeri görüntüler **(0)** . 

Seçicide seçin **kapsayıcıları**.

![Select kapsayıcıları görüntüleyin](./media/container-insights-analyze/containers-containers-tab.png)

Burada, Azure Kubernetes ve Azure Container Instances, kapsayıcılarınızın performans durumunu görüntüleyebilirsiniz.  

![\<Ad > denetleyicileri performans görünümü](./media/container-insights-analyze/containers-containers-view.png)

Bir kapsayıcıdan, bir pod veya filtre bu nesne için performans verilerini görüntülemek için düğümü aşağı detaya gidebilirsiniz. Değerin altında tıklayın **Pod** veya **düğüm** sütun için belirli bir kapsayıcı.   

![Örnek detaya gitme; düğümünden denetleyicisine performans görünümü](./media/container-insights-analyze/drill-down-controller-node.png)

Kapsayıcıları görüntülediğinizde görüntülenen bilgileri aşağıdaki tabloda açıklanmıştır:

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı.|
| Durum | Kapsayıcılar, varsa durumu. Durum simgesi, ek ayrıntılar sonraki tabloda verilmiştir.|
| Ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Her varlık için yüzdebirlik ve seçili ölçümün ortalama yüzdesini dökümü. |
| Ortalama, Min, maks., 50, 90  | Ortalama CPU millicore veya bellek performans seçilen yüzdelik dilim kapsayıcısının dökümü. Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Pod | Pod bulunduğu kapsayıcısı.| 
| Node |  Kapsayıcı bulunduğu düğümü. | 
| Yeniden başlatma | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Çalışma Süresi | Kapsayıcı kullanmaya ya da yeniden beri süresini temsil eder. |
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Çubuk grafik eğilimini kapsayıcı ortalama yüzdebirlik ölçüm yüzdesini temsil eder. |

Durum alanı simgeleri, aşağıdaki tabloda açıklandığı gibi çevrimiçi, pod'ların durumlarını gösterir:
 
| Simge | Durum |  
|--------|-------------|  
| ![Hazır çalışan durum simgesi](./media/container-insights-analyze/containers-ready-icon.png) | (Hazır) çalıştıran|  
| ![Bekleyen veya duraklatıldı durum simgesi](./media/container-insights-analyze/containers-waiting-icon.png) | Bekleyen veya duraklatıldı|  
| ![Son durum simgesi çalıştıran bildirdi.](./media/container-insights-analyze/containers-grey-icon.png) | Son çalıştırma bildirdi ancak 30 dakikadan fazla yanıt henüz|  
| ![Sonlandırılan durum simgesi](./media/container-insights-analyze/containers-terminated-icon.png) | Başarılı bir şekilde durdurulmuş veya yanıt vermemesine başarısız|  
| ![Başarısız durum simgesi](./media/container-insights-analyze/containers-failed-icon.png) | Durumu başarısız |  

## <a name="workbooks"></a>Çalışma Kitapları

Çalışma kitapları metin, [günlük sorgularını](../log-query/query-language.md), [ölçümleri](../platform/data-platform-metrics.md)ve parametreleri zengin etkileşimli raporlara birleştirir. Çalışma kitapları aynı Azure kaynaklarına erişimi olan diğer takım üyeleri tarafından düzenlenebilir.

Kapsayıcılar için Azure Izleyici, başlamanıza olanak sağlamak için dört çalışma kitabı içerir:

- **Disk kapasitesi**: Aşağıdaki perspektiflere göre bir kapsayıcı içindeki düğüme sunulan her bir disk için etkileşimli disk kullanımı grafikleri sunar:

    - Tüm diskler için disk kullanımı yüzdesi
    - Tüm diskler için boş disk alanı
    - Her düğüm diski,% kullanılan alanı,% kullanılan alan eğilimi, boş disk alanı (GiB) ve boş disk alanı eğilimi (GiB) için bir kılavuz. Tabloda bir satır seçildiğinde, aşağıda kullanılan alan ve boş disk alanı (GiB) aşağıda gösterilmektedir 

- **DISK GÇ**: Aşağıdaki perspektiflere göre bir kapsayıcı içindeki düğüme sunulan her bir disk için etkileşimli disk kullanımı grafikleri sunar:

    - Disk g/ç okuma bayt/sn, bayt/sn yazma ve okuma ve yazma bayt/sn eğilimlerini 
    - Disk g/ç performans sorunlarını ölçmenize ve belirlemesine yardımcı olacak ana performans göstergelerini gösteren sekiz performans grafiği.

- **Kubelet**: Anahtar düğüm işletim istatistiklerini gösteren iki kılavuz içerir:

    - Node Grid 'e göre genel bakış, her düğüm için yüzde ve eğilim bazında toplam işlem, toplam hata ve başarılı işlemleri özetler.
    - İşlem türüne göre genel bakış toplam işlem, toplam hata ve yüzde ve eğilim bazında başarılı işlemler için her bir işlem için özetler.

- **Ağ**: Her düğüm ağ bağdaştırıcısı için etkileşimli ağ kullanım grafikleri ve ağ bağdaştırıcılarınızın performansını ölçmenize yardımcı olmak için ana performans göstergelerini sunan bir kılavuz sunar.  

**Çalışma kitaplarını görüntüle** açılır listesinden her birini seçerek bu çalışma kitaplarına erişirsiniz.  

![Çalışma kitaplarını görüntüle açılan listesi](./media/container-insights-analyze/view-workbooks-dropdown-list.png)

## <a name="next-steps"></a>Sonraki adımlar

- DevOps veya işletimsel işlemlerinizi ve yordamlarınızı desteklemek üzere yüksek CPU ve bellek kullanımı için uyarı oluşturmayı öğrenmek üzere [kapsayıcılar Için Azure izleyici ile performans uyarılarını oluşturma](container-insights-alerts.md) ' yı gözden geçirin. 

- Daha önceden tanımlanmış sorguları ve Uyarıları izlemek için değerlendirmek veya özelleştirmek üzere ön tanımlı sorguları ve örnekleri görmek için [günlük sorgusu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin, kümelerinizi görselleştirmeyi veya çözümlemeyi inceleyin.
