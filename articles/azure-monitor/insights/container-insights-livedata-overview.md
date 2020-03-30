---
title: Kapsayıcılar için Azure Monitörü ile Canlı Verileri (önizleme) görüntüleyin | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar için Azure Monitor'da kubectl kullanmadan Kubernetes günlüklerinin, olayların ve bölme ölçümlerinin gerçek zamanlı görünümü açıklanmaktadır.
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: 9e7c7a7b7bf276b3451cee1d289b8b07ac0f40ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79216561"
---
# <a name="how-to-view-kubernetes-logs-events-and-pod-metrics-in-real-time"></a>Kubernetes günlüklerini, olaylarını ve pod ölçümlerini gerçek zamanlı olarak görüntüleme

Kapsayıcılar için Azure Monitörü, Azure Kubernetes Hizmeti (AKS) kapsayıcı günlüklerinize (stdout/stderror), olaylara ve pod ölçümlerine doğrudan erişmenize olanak tanıyan gelişmiş bir tanılama özelliği olan Canlı Veri (önizleme) özelliğini içerir. Olaylara ve `kubectl logs -c` `kubectl top pods`olaylara `kubectl get` doğrudan erişimi ortaya çıkarır. Konsol bölmesi, sorun giderme sorunlarına gerçek zamanlı olarak yardımcı olmak için kapsayıcı motoru tarafından oluşturulan günlükleri, olayları ve ölçümleri gösterir.

Bu makalede ayrıntılı bir genel bakış sağlar ve bu özelliği nasıl kullanacağınızı anlamanıza yardımcı olur. 

>[!NOTE]
>[Özel kümeler](https://azure.microsoft.com/updates/aks-private-cluster/) olarak etkinleştirilen AKS kümeleri bu özellik ile desteklenmez. Bu özellik, tarayıcınızdan bir proxy sunucusu aracılığıyla Kubernetes API'ye doğrudan erişmenize dayanır. Bu proxy Kubernetes API engellemek için ağ güvenliği etkinleştirmek bu trafiği engeller. 

>[!NOTE]
>Bu özellik, Azure Çin'i de dahil olmak üzere tüm Azure bölgelerinde kullanılabilir. Şu anda Azure ABD Hükümeti'nde kullanılamıyor.

Canlı Veri (önizleme) özelliğini ayarlama veya sorun giderme yardımı için [kurulum kılavuzumuzu](container-insights-livedata-setup.md)inceleyin. Bu özellik doğrudan Kubernetes API erişebilirsiniz ve kimlik doğrulama modeli hakkında ek bilgiler [burada](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)bulabilirsiniz. 

## <a name="live-data-preview-functionality-overview"></a>Canlı Veri (önizleme) işlevselliği genel bakış

### <a name="search"></a>Search

![Canlı Veri konsolu bölme filtresi örneği](./media/container-insights-livedata-overview/livedata-pane-filter-example.png)

Canlı Veri (önizleme) özelliği arama işlevselliğini içerir. **Arama** alanında, bir anahtar sözcük veya terim yazarak sonuçları filtreleyebilirsiniz ve hızlı incelemeye izin vermek için eşleşen sonuçlar vurgulanır. Olayları görüntülerken, arama çubuğunun sağında bulunan **Filtre** hapını kullanarak sonuçları ayrıca sınırlayabilirsiniz. Seçtiğiniz kaynağa bağlı olarak, hap, seçilecek bir Pod, Ad Alanı veya küme listeler.  

### <a name="scroll-lock-and-pause"></a>Kaydırma Kilidi ve Duraklatma 

Otomatik kaydırmayı askıya almak ve bölmenin davranışını denetlemek için, okunan yeni veriler arasında el ile gezinmenize olanak sağlamak için **Kaydırma** seçeneğini kullanabilirsiniz. Otomatik kaydırmayı yeniden etkinleştirmek için **Kaydırma** seçeneğini yeniden seçmeniz yeterlidir. Ayrıca **Duraklatma** seçeneğini seçerek günlük veya olay verilerinin alınmasını duraklatabilirsiniz ve devam etmeye hazır olduğunuzda **Oynat'ı**seçmeniz yeterlidir.  

![Canlı Veri konsolu bölmesi duraklatma canlı görünüm](./media/container-insights-livedata-overview/livedata-pane-scroll-pause-example.png)

>[!IMPORTANT]
>Bir sorunu giderirken otomatik kaydırmayı yalnızca kısa bir süre için askıya almanızı veya duraklatmanızı öneririz. Bu istekler, Kubernetes API'sinin kümenizdeki kullanılabilirliğini ve daraltma durumunu etkileyebilir. 

>[!IMPORTANT]
>Bu özelliğin çalışması sırasında hiçbir veri kalıcı olarak depolanır. Oturum sırasında yakalanan tüm bilgiler tarayıcınızı kapattığınızda veya tarayıcıdan uzaklaştığınızda silinir. Veriler yalnızca ölçümler özelliğinin beş dakikalık penceresi içinde görselleştirme için mevcut kalır; beş dakikadan eski tüm ölçümler de silinir. Makul bellek kullanım sınırları içinde Canlı Veri (önizleme) arabellek sorguları (burada daha spesifik olması gerekir, makul nedir?). 

## <a name="view-logs"></a>Günlükleri görüntüleme

**Düğümler, Denetleyiciler**ve **Kapsayıcılar**görünümünden konteyner motoru tarafından oluşturulan gerçek zamanlı **Containers** günlük verilerini görüntüleyebilirsiniz. Günlük verilerini görüntülemek için aşağıdaki adımları gerçekleştirin.

1. Azure portalında AKS küme kaynak grubuna göz atın ve AKS kaynağınızı seçin.

2. AKS küme panosunda, sol **taraftaKi İzleme** **altında, Insights'ı**seçin. 

3. **Düğümler, Denetleyiciler**veya **Kapsayıcılar**sekmesini seçin. **Containers**

4. Performans ızgarasından bir nesne seçin ve sağ tarafta bulunan özellikler bölmesinde **canlı verileri görüntüle (önizleme)** seçeneğini seçin. AKS kümesi Azure AD kullanılarak tek oturumla yapılandırılırsa, bu tarayıcı oturumu sırasında ilk kullanımda kimlik doğrulamanız istenir. Azure ile hesabınızı seçin ve kimlik doğrulamasını tamamlayın.  

    >[!NOTE]
    >Özellikler bölmesinden analitik olarak **Görünüm** seçeneğini seçerek Log Analytics çalışma alanınızdaki verileri görüntülerken, günlük arama sonuçları **nodları,** **Daemon Kümelerini,** Çoğaltma **Kümelerini,** **İş İli,** **Cron İşleri,** **Podları**ve artık var olmayan **Kapsayıcıları** potansiyel olarak gösterir. Kullanılamayan bir kapsayıcı için günlükleri aramaya `kubectl` çalışmak da burada başarısız olur. Geçmiş günlükleri, olayları ve ölçümleri görüntüleme hakkında daha fazla bilgi edinmek için [analitik te Görünüm](container-insights-log-search.md#search-logs-to-analyze-data) özelliğini gözden geçirin.  

Başarılı bir şekilde kimlik doğrulaması yaptıktan sonra, Canlı Veri (önizleme) konsol bölmesi, günlük verilerini sürekli bir akışta görüntülediğiniz performans veri ızgarasının altında görünür. Alma durumu göstergesi bölmenin en sağında bulunan yeşil bir onay işareti gösteriyorsa, bu verilerin alınabileceği ve konsolunuza akmaya başladığı anlamına gelir.  

![Düğüm özellikleri bölmesi görünüm veri seçeneği](./media/container-insights-livedata-overview/node-properties-pane.png)  

Bölme başlığı, kapsayıcının gruplandırılan bölmenin adını gösterir.

## <a name="view-events"></a>Etkinlikleri görüntüleme

Bir kapsayıcı, bölme, düğüm, ReplicaSet, DaemonSet, iş, CronJob veya Dağıtım seçildiğinde **Düğümler,** **Denetleyiciler,** **Kapsayıcılar**ve **Dağıtımlar (önizleme)** görünümünden konteyner motoru tarafından oluşturulan gerçek zamanlı olay verilerini görüntüleyebilirsiniz. Olayları görüntülemek için aşağıdaki adımları gerçekleştirin.

1. Azure portalında AKS küme kaynak grubuna göz atın ve AKS kaynağınızı seçin.

2. AKS küme panosunda, sol **taraftaKi İzleme** **altında, Insights'ı**seçin. 

3. **Düğümler, Denetleyiciler,** **Kapsayıcılar**veya **Dağıtımlar (önizleme)** sekmesini seçin. **Containers**

4. Performans ızgarasından bir nesne seçin ve sağ tarafta bulunan özellikler bölmesinde **canlı verileri görüntüle (önizleme)** seçeneğini seçin. AKS kümesi Azure AD kullanılarak tek oturumla yapılandırılırsa, bu tarayıcı oturumu sırasında ilk kullanımda kimlik doğrulamanız istenir. Azure ile hesabınızı seçin ve kimlik doğrulamasını tamamlayın.  

    >[!NOTE]
    >Özellikler bölmesinden analitik olarak **Görünüm** seçeneğini seçerek Log Analytics çalışma alanınızdaki verileri görüntülerken, günlük arama sonuçları **nodları,** **Daemon Kümelerini,** Çoğaltma **Kümelerini,** **İş İli,** **Cron İşleri,** **Podları**ve artık var olmayan **Kapsayıcıları** potansiyel olarak gösterir. Kullanılamayan bir kapsayıcı için günlükleri aramaya `kubectl` çalışmak da burada başarısız olur. Geçmiş günlükleri, olayları ve ölçümleri görüntüleme hakkında daha fazla bilgi edinmek için [analitik te Görünüm](container-insights-log-search.md#search-logs-to-analyze-data) özelliğini gözden geçirin.  

Başarıyla kimlik doğrulaması yaptıktan sonra, Performans veri ızgarasının altında Canlı Veri (önizleme) konsol bölmesi görünür. Alma durumu göstergesi bölmenin en sağında bulunan yeşil bir onay işareti gösteriyorsa, bu verilerin alınabileceği ve konsolunuza akmaya başladığı anlamına gelir. 
    
Seçtiğiniz nesne bir kapsayıcıysa, bölmedeki **Olaylar** seçeneğini seçin. Bir Düğüm, Pod veya denetleyici seçtiyseniz, olayları görüntüleme otomatik olarak seçilir. 

![Denetleyici özellikleri bölme görünümü olayları](./media/container-insights-livedata-overview/controller-properties-live-event.png)  

Bölme başlığı, kapsayıcının gruplandırılan Pod'un adını gösterir.

### <a name="filter-events"></a>Olayları filtreleme 

Olayları görüntülerken, arama çubuğunun sağında bulunan **Filtre** hapını kullanarak sonuçları ayrıca sınırlayabilirsiniz. Seçtiğiniz kaynağa bağlı olarak, hap, seçilecek bir Pod, Ad Alanı veya küme listeler.  

## <a name="view-metrics"></a>Ölçümleri görüntüle 

Yalnızca Pod seçildiğinde **Düğümler** veya **Denetleyiciler** görünümünden kapsayıcı motoru tarafından oluşturulan gerçek **Pod** zamanlı metrik verileri görüntüleyebilirsiniz. Ölçümleri görüntülemek için aşağıdaki adımları gerçekleştirin.

1. Azure portalında AKS küme kaynak grubuna göz atın ve AKS kaynağınızı seçin.

2. AKS küme panosunda, sol **taraftaKi İzleme** **altında, Insights'ı**seçin. 

3. **Düğümler** veya **Denetleyiciler** sekmesini seçin.

4. Performans ızgarasından bir **Pod** nesnesi seçin ve sağ tarafta bulunan özellikler bölmesinde **canlı verileri görüntüle (önizleme)** seçeneğini seçin. AKS kümesi Azure AD kullanılarak tek oturumla yapılandırılırsa, bu tarayıcı oturumu sırasında ilk kullanımda kimlik doğrulamanız istenir. Azure ile hesabınızı seçin ve kimlik doğrulamasını tamamlayın.  

    >[!NOTE]
    >Özellikler bölmesinden analitik olarak **Görünüm** seçeneğini seçerek Log Analytics çalışma alanınızdaki verileri görüntülerken, günlük arama sonuçları **nodları,** **Daemon Kümelerini,** Çoğaltma **Kümelerini,** **İş İli,** **Cron İşleri,** **Podları**ve artık var olmayan **Kapsayıcıları** potansiyel olarak gösterir. Kullanılamayan bir kapsayıcı için günlükleri aramaya `kubectl` çalışmak da burada başarısız olur. Geçmiş günlükleri, olayları ve ölçümleri görüntüleme hakkında daha fazla bilgi edinmek için [analitik te Görünüm](container-insights-log-search.md#search-logs-to-analyze-data) özelliğini gözden geçirin.  

Başarıyla kimlik doğrulaması yaptıktan sonra, Performans veri ızgarasının altında Canlı Veri (önizleme) konsol bölmesi görünür. Metrik veriler alınır ve iki grafikte sunulmak üzere konsolunuza akış başlar. Bölme başlığı, kapsayıcının gruplandırılan bölmenin adını gösterir.

![Pod ölçümlerini görüntüle örneği](./media/container-insights-livedata-overview/pod-properties-live-metrics.png)  

## <a name="next-steps"></a>Sonraki adımlar

- Azure Monitörünü kullanmayı ve AKS kümenizin diğer yönlerini izlemeyi öğrenmeye devam etmek için Bkz. [Azure Kubernetes Hizmet sağlığını görüntüleyin.](container-insights-analyze.md)

- Uyarılar, görselleştirmeler oluşturmak veya kümelerinizin daha fazla analizini gerçekleştirmek için önceden tanımlanmış sorguları ve örnekleri görmek için [günlük sorgu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin.
