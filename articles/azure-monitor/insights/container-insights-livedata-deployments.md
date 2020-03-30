---
title: Kapsayıcılar Dağıtımları için Azure Monitörünü Görüntüleyin (önizleme) | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar için Azure Monitor'da kubectl kullanmadan Kubernetes Deployments'ın gerçek zamanlı görünümü açıklanmaktadır.
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: 7d0344851e1db8c014a1bb16b228a0c2f76444d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75404768"
---
# <a name="how-to-view-deployments-preview-in-real-time"></a>Dağıtımları (önizleme) gerçek zamanlı olarak görüntüleme

Kapsayıcılar için Azure Monitor ile görünüm Dağıtımları (önizleme) özelliği, kubernetes Dağıtım nesnelerine doğrudan `kubeclt get deployments` erişimi, komutları ve `kubectl describe deployment {your deployment}` komutları ortaya çıkararak gerçek zamanlı olarak taklit eder. 

>[!NOTE]
>[Özel kümeler](https://azure.microsoft.com/updates/aks-private-cluster/) olarak etkinleştirilen AKS kümeleri bu özellik ile desteklenmez. Bu özellik, tarayıcınızdan bir proxy sunucusu aracılığıyla Kubernetes API'ye doğrudan erişmenize dayanır. Bu proxy Kubernetes API engellemek için ağ güvenliği etkinleştirmek bu trafiği engeller. 

>[!NOTE]
>Bu özellik, Azure Çin'i de dahil olmak üzere tüm Azure bölgelerinde kullanılabilir. Şu anda Azure ABD Hükümeti'nde kullanılamıyor.

Daha fazla bilgi edinmek [için, Dağıtımlar](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)hakkındaki Kubernetes belgelerini inceleyin. 

## <a name="how-it-works"></a>Nasıl çalışır?

Canlı Veri (önizleme) özelliği doğrudan Kubernetes API'ye erişebilir ve kimlik doğrulama modeli hakkında ek bilgilere [buradan](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)ulaşabilirsiniz. 

Dağıtımlar (önizleme) özelliği, dağıtımların bitiş noktasına `/apis/apps/v1/deployments`karşı tek seferlik (yenilenebilir) bir yük gerçekleştirir. Belirli bir dağıtım seçmenize ve dağıtım bitiş noktasına `/apis/apps/v1/namespaces/${nameSpace}/deployments/${deploymentName}`karşı belirli bir dağıtımiçin açıklama ayrıntılarını yüklemenize olanak tanır. 

Sayfanın sol üst kısmında **Yenile'yi** seçmek dağıtım listesini yeniler. Bu, komutu yeniden `kubectl` çalıştırmayı simüle eder. 

>[!IMPORTANT]
>Bu özelliğin çalışması sırasında hiçbir veri kalıcı olarak depolanır. Oturum sırasında yakalanan tüm bilgiler tarayıcınızı kapattığınızda veya tarayıcıdan uzaklaştığınızda silinir.  

>[!NOTE]
>Konsoldan Azure panosuna Canlı Veri (Önizleme) verilerini sabitleyemezsiniz.

## <a name="deployments-describe"></a>Dağıtımlar açıklar

Bir dağıtımiçin ayrıntıları açıkla'yı görüntülemek `kubectl describe deployment`için, bu, aşağıdaki adımları gerçekleştirin.

1. Azure portalında AKS küme kaynak grubuna göz atın ve AKS kaynağınızı seçin.

2. AKS küme panosunda, sol **taraftaKi İzleme** **altında, Insights'ı**seçin. 

3. **Dağıtımlar (önizleme)** sekmesini seçin.

    ![Azure portalında dağıtımlar görünümü](./media/container-insights-livedata-deployments/deployment-view.png)

Görünüm, ad alanı ve diğer ayrıntılı bilgilerle birlikte çalışan tüm dağıtımların bir listesini `kubectl get deployments –all-namespaces`gösterince, komutun yürütülmesini taklit eder. Sütunlardan herhangi birini seçerek sonuçları sıralayabilirsiniz. 

![Dağıtımözellikleri bölme ayrıntıları](./media/container-insights-livedata-deployments/deployment-properties-pane-details.png)

Listeden bir dağıtım seçtiğinizde, bir özellik bölmesi sayfanın sağ tarafında otomatik olarak görüntülenir. Bu komutu `kubectl describe deployment {deploymentName}`çalıştırırsanız görüntülenebilen seçili dağıtım ile ilgili bilgileri gösterir. Açıklanan bilgilerin bazı ayrıntıların eksik olduğunu fark etmiş olabilirsiniz. En önemlisi **Şablon** eksik. **Ham** sekmesini seçmek, ayrışmamış Açıkla ayrıntılarına gidinize olanak tanır.  

![Dağıtımözellikleri bölme ham ayrıntılar](./media/container-insights-livedata-deployments/deployment-properties-pane-raw.png)

Dağıtım ayrıntılarını gözden geçirerken, kapsayıcı günlüklerini ve olayları gerçek zamanlı olarak görebilirsiniz. Canlı **konsolu Göster'i** seçin ve Canlı Veri (önizleme) konsol bölmesi, sürekli akışta canlı günlük verilerini görüntüleyebileceğiniz dağıtımlar veri ızgarasının altında görünür. Alma durumu göstergesi bölmenin en sağında bulunan yeşil bir onay işareti gösteriyorsa, bu verilerin alınabileceği ve konsolunuza akmaya başladığı anlamına gelir.

Ad alanına veya küme düzeyi olaylarına göre de filtre uygulayabilirsiniz. Konsolda gerçek zamanlı görüntüleme verileri hakkında daha fazla bilgi edinmek [için kapsayıcılar için Azure Monitor ile Canlı Verileri Görüntüle (önizleme) bilgisine](container-insights-livedata-overview.md)bakın. 

![Dağıtımlar konsoldaki canlı verileri görüntüle](./media/container-insights-livedata-deployments/deployments-console-view-events.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Monitörünü kullanmayı ve AKS kümenizin diğer yönlerini izlemeyi öğrenmeye devam etmek için Bkz. [Azure Kubernetes Hizmet sağlığını görüntüleyin.](container-insights-analyze.md)

- Uyarılar, görselleştirmeler oluşturmak veya kümelerinizin daha fazla analizini gerçekleştirmek için önceden tanımlanmış sorguları ve örnekleri görmek için [günlük sorgu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin.
