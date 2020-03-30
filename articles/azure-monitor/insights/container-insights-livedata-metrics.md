---
title: Kapsayıcılar için Azure Monitor ile ölçümleri gerçek zamanlı olarak görüntüleyin | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar için Azure Monitor ile kubectl kullanmadan ölçümlerin gerçek zamanlı görünümü açıklanmaktadır.
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: 4604635c985057ec0b7f49a0d1cca7111dfc8eec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79216569"
---
# <a name="how-to-view-metrics-in-real-time"></a>Ölçümleri gerçek zamanlı görüntüleme

Kapsayıcılar için Azure Monitor Canlı Veri (önizleme) özelliği, kümedeki düğüm ve bölme durumu yla ilgili ölçümleri gerçek zamanlı olarak görselleştirmenize olanak tanır. Bu Insight ile birlikte yer `kubectl top nodes` `kubectl get pods –all-namespaces`alan `kubectl get nodes` performans grafiklerinde verileri aramak, ayrışdırmak ve görselleştirmek için ,, ve komutları doğrudan erişim öyküner. 

Bu makalede ayrıntılı bir genel bakış sağlar ve bu özelliği nasıl kullanacağınızı anlamanıza yardımcı olur.  

>[!NOTE]
>[Özel kümeler](https://azure.microsoft.com/updates/aks-private-cluster/) olarak etkinleştirilen AKS kümeleri bu özellik ile desteklenmez. Bu özellik, tarayıcınızdan bir proxy sunucusu aracılığıyla Kubernetes API'ye doğrudan erişmenize dayanır. Bu proxy Kubernetes API engellemek için ağ güvenliği etkinleştirmek bu trafiği engeller. 

>[!NOTE]
>Bu özellik, Azure Çin'i de dahil olmak üzere tüm Azure bölgelerinde kullanılabilir. Şu anda Azure ABD Hükümeti'nde kullanılamıyor.

Canlı Veri (önizleme) özelliğini ayarlama veya sorun giderme ile ilgili yardım için [kurulum kılavuzumuzu](container-insights-livedata-setup.md)inceleyin.

## <a name="how-it-works"></a>Nasıl çalışır? 

Canlı Veri (önizleme) özelliği doğrudan Kubernetes API'ye erişebilir ve kimlik doğrulama modeli hakkında ek bilgilere [buradan](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)ulaşabilirsiniz. 

Bu özellik, varsayılan olarak her beş saniyede bir `/api/v1/nodes` `/apis/metrics.k8s.io/v1beta1/nodes`olan `/api/v1/pods`ölçümler uç noktalarına (, ve ) karşı bir yoklama işlemi gerçekleştirir. Bu veriler tarayıcınızda önbelleğe alınmış ve **Canlı Ol (önizleme)** seçeneğini seçerek **Cluster** sekmesindeki kapsayıcılar için Azure Monitor'da bulunan dört performans grafiğinde listelenir. Sonraki her anket, beş dakikalık bir görselleştirme penceresine grafiklenir. 

![Küme görünümünde Canlı'ya Git seçeneği](./media/container-insights-livedata-metrics/cluster-view-go-live-example-01.png)

Yoklama **aralığı, her** 1, 5, 15 ve 30 saniyede bir yeni veriler için yoklama ayarlamanızı sağlayan Set aralığı açılır tarihinden yapılandırılır. 

![Go Live açılır yoklama aralığı](./media/container-insights-livedata-metrics/cluster-view-polling-interval-dropdown.png)

>[!IMPORTANT]
>Bir sorunu kısa bir süre için sorun giderirken yoklama aralığını bir saniyeye ayarlamanızı öneririz. Bu istekler, Kubernetes API'sinin kümenizdeki kullanılabilirliğini ve daraltma durumunu etkileyebilir. Daha sonra, daha uzun bir yoklama aralığına yeniden yapılandırır. 

>[!IMPORTANT]
>Bu özelliğin çalışması sırasında hiçbir veri kalıcı olarak depolanır. Bu oturum sırasında yakalanan tüm bilgiler, tarayıcınızı kapattığınızda veya özellikten uzaklaştığınızda hemen silinir. Veriler yalnızca beş dakikalık pencere içinde görselleştirme için mevcut kalır; beş dakikadan eski tüm ölçümler de kalıcı olarak silinir.

Bu grafikler, canlı modda görüntülediğiniz son Azure panosuna sabitlenemez.

## <a name="metrics-captured"></a>Yakalanan ölçümler

### <a name="node-cpu-utilization---node-memory-utilization-"></a>Düğüm CPU kullanımı % / Düğüm Bellek kullanımı % 

Bu iki performans grafiği, `kubectl top nodes` **cpu%ve MEMORY%** sütunlarının sonuçlarını ilgili **MEMORY%** grafiğe çağırmak ve yakalamak eşdeğeri ile eşleştirilir. 

![Kubectl üst düğümleri örnek sonuçlar](./media/container-insights-livedata-metrics/kubectl-top-nodes-example.png)

![Düğümler CPU kullanım yüzdesi grafiği](./media/container-insights-livedata-metrics/cluster-view-node-cpu-util.png)

![Düğüm Bellek kullanım yüzdesi grafiği](./media/container-insights-livedata-metrics/cluster-view-node-memory-util.png)

Yüzdelik hesaplamalar, kümenizdeki aykırı düğümleri belirlemeye yardımcı olmak için daha büyük kümelerde çalışır. Örneğin, düğümlerin küçültme amacıyla yeterince kullanılmadığını anlamak için. **Min** toplama kullanarak kümede hangi düğümlerin düşük kullanıma sahip olduğunu görebilirsiniz. Daha fazla araştırmak için **Düğümler** sekmesini seçin ve ızgarayı CPU veya bellek kullanımına göre sıralayın.

Bu, hangi düğümlerin sınırlarına itildiğini ve ölçeklendirme gerekip gerekip gerekip gerekmeytolmadığını anlamanıza da yardımcı olur. **Hem Max** hem de **P95** toplamalarını kullanmak, kümede yüksek kaynak kullanımı olan düğümler olup olmadığını görmenize yardımcı olabilir. Daha fazla araştırma için, yine Düğümler sekmesine **geçersiniz.**

### <a name="node-count"></a>Düğüm sayısı

Bu performans grafiği, DURUM sütununa göre `kubectl get nodes` **durum** türlerine göre gruplanmış bir grafiğe gerek yok etme ve eşleme eşdeğeri eşlenir.

![Kubectl düğümleri örnek sonuçlar olsun](./media/container-insights-livedata-metrics/kubectl-get-nodes-example.png)

![Düğümsayısı grafiği](./media/container-insights-livedata-metrics/cluster-view-node-count-01.png)

Düğümler **Hazır** veya **Hazır Değil** durumunda bildirilir. Bunlar sayılır (ve toplam sayı oluşturulur) ve bu iki toplamanın sonuçları listelenir.
Örneğin, düğümlerinizin başarısız durumlara düşüp düşmediğini anlamak için. **Hazır Olmayan** toplamayı kullanarak kümenizdeki düğüm sayısını şu anda **Hazır Değil** durumunda hızla görebilirsiniz.

### <a name="active-pod-count"></a>Etkin pod sayısı

Bu performans grafiği, durum türlerine `kubectl get pods –all-namespaces` göre gruplanan **DURUM** sütunu çağıran eşdeğeri ile eşlenir.

![Kubectl bakla örnek sonuçları olsun](./media/container-insights-livedata-metrics/kubectl-get-pods-example.png)

![Düğümler bölme sayım grafiği](./media/container-insights-livedata-metrics/cluster-view-node-pod-count.png)

>[!NOTE]
>Tarafından yorumlanan durum `kubectl` adları grafikte tam olarak eşleşmeyebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Uyarılar, görselleştirmeler oluşturmak veya kümelerinizin daha fazla analizini gerçekleştirmek için önceden tanımlanmış sorguları ve örnekleri görmek için [günlük sorgu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin.
