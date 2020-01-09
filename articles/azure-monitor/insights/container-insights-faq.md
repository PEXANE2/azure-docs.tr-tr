---
title: Kapsayıcılar için Azure Izleyici sık sorulan sorular | Microsoft Docs
description: Kapsayıcılar için Azure Izleyici, AKS kümelerinizin ve Azure 'daki Container Instances sistem durumunu izleyen bir çözümdür. Bu makalede yaygın soruların yanıtları vardır.
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: 0984de51221c506bb1824e4dcfd93eef56453a4d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75405076"
---
# <a name="azure-monitor-for-containers-frequently-asked-questions"></a>Kapsayıcılar için Azure Izleyici sık sorulan sorular

Bu Microsoft SSS, kapsayıcılar için Azure Izleyici hakkında sık sorulan sorulardan oluşan bir listesidir. Çözümle ilgili başka sorularınız varsa, [tartışma forumuna](https://feedback.azure.com/forums/34192--general-feedback) gidin ve sorularınızı gönderin. Sık sorulan bir soru, böylece hızla ve kolayca bulunabilir, bu makaleye ekleriz.

## <a name="i-dont-see-image-and-name-property-values-populated-when-i-query-the-containerlog-table"></a>ContainerLog tablosunu sorgulıyorum, resim ve ad özelliği değerlerini doldurdum.

Aracı sürümü ciprod12042019 ve üzeri için, varsayılan olarak bu iki özellik her günlük satırı için doldurulmaz ve toplanan günlük verilerinde maliyeti en aza indirir. Bu özellikleri içeren tabloyu, değerlerini sorgulamak için iki seçenek vardır:

### <a name="option-1"></a>Seçenek 1 

Sonuçlara bu özellik değerlerini dahil etmek için diğer tabloları birleştirin.

Containerıd özelliğine katılarak ```ContainerInventory``` tablosundan Image ve ImageTag özelliklerini içerecek şekilde sorgularınızı değiştirin. Name özelliğini (daha önce ```ContainerLog``` tablosunda göründüğü gibi) KubepodInventory tablosunun ContaineName alanındaki Containerıd özelliğine katılarak dahil edebilirsiniz. Önerilen seçenek budur.

Aşağıdaki örnek, bu alan değerlerinin birleşimlerle nasıl alınacağını anlatan örnek bir ayrıntılı sorgudur.

```
//lets say we are querying an hour worth of logs
let startTime = ago(1h);
let endTime = now();
//below gets the latest Image & ImageTag for every containerID, during the time window
let ContainerInv = ContainerInventory | where TimeGenerated >= startTime and TimeGenerated < endTime | summarize arg_max(TimeGenerated, *)  by ContainerID, Image, ImageTag | project-away TimeGenerated | project ContainerID1=ContainerID, Image1=Image ,ImageTag1=ImageTag;
//below gets the latest Name for every containerID, during the time window
let KubePodInv  = KubePodInventory | where ContainerID != "" | where TimeGenerated >= startTime | where TimeGenerated < endTime | summarize arg_max(TimeGenerated, *)  by ContainerID2 = ContainerID, Name1=ContainerName | project ContainerID2 , Name1;
//now join the above 2 to get a 'jointed table' that has name, image & imagetag. Outer left is safer in-case there are no kubepod records are if they are latent
let ContainerData = ContainerInv | join kind=leftouter (KubePodInv) on $left.ContainerID1 == $right.ContainerID2;
//now join ContainerLog table with the 'jointed table' above and project-away redundant fields/columns and rename columns that were re-written
//Outer left is safer so you dont lose logs even if we cannot find container metadata for loglines (due to latency, time skew between data types etc...)
ContainerLog
| where TimeGenerated >= startTime and TimeGenerated < endTime 
| join kind= leftouter (
   ContainerData
) on $left.ContainerID == $right.ContainerID2 | project-away ContainerID1, ContainerID2, Name, Image, ImageTag | project-rename Name = Name1, Image=Image1, ImageTag=ImageTag1 

```

### <a name="option-2"></a>Seçenek 2

Her kapsayıcı günlük satırı için bu özellikler için koleksiyonu yeniden etkinleştirin.

İlk seçenek, söz konusu sorgu değişiklikleri nedeniyle uygun değilse, [veri toplama yapılandırma ayarlarında](./container-insights-agent-config.md)açıklandığı gibi aracı yapılandırma eşlemesindeki ```log_collection_settings.enrich_container_logs``` ayarını etkinleştirerek bu alanları toplamayı yeniden etkinleştirebilirsiniz.

> [!NOTE]
> İkinci seçenek, bu zenginleştirme işlemini gerçekleştirmek için kümedeki > Her düğümden API sunucusu çağrıları oluştururken 50 ' den fazla düğümü olan büyük kümeler için önerilmez. Bu seçenek ayrıca, toplanan her günlük satırı için veri boyutunu artırır.

## <a name="can-i-view-metrics-collected-in-grafana"></a>Grafana ' de toplanan ölçümleri görüntüleyebilir miyim?

Kapsayıcılar için Azure Izleyici, Grafana panolar içinde Log Analytics çalışma alanınızda depolanan ölçümlerin görüntülenmesini destekler. Grafana 'in [Pano deposundan](https://grafana.com/grafana/dashboards?dataSource=grafana-azure-monitor-datasource&category=docker) indirebileceğiniz bir şablon sağladık ve özel Grafana panolarında görselleştirmek üzere izlenen kümelerinizdeki ek verileri sorgulama hakkında bilgi edinmenize yardımcı olur. 

## <a name="can-i-monitor-my-aks-engine-cluster-with-azure-monitor-for-containers"></a>AKS-Engine kümenizi kapsayıcılar için Azure Izleyici ile izleyebilir miyim?

Kapsayıcılar için Azure Izleyici, Azure 'da barındırılan AKS-Engine (eski adıyla ACS-Engine) kümeleri için dağıtılan kapsayıcı iş yüklerini izlemeyi destekler. Bu senaryoya yönelik izlemeyi etkinleştirmek için gereken adımlara ilişkin daha fazla bilgi ve bir genel bakış için bkz. [AKS-Engine için kapsayıcılar Için Azure Izleyicisini kullanma](https://github.com/microsoft/OMS-docker/tree/aks-engine).

## <a name="why-dont-i-see-data-in-my-log-analytics-workspace"></a>Neden Log Analytics çalışma alanım 'da veri görmüyorum?

Log Analytics çalışma alanında belirli bir zamanda herhangi bir veri göremiyorsanız, günlük toplanacak veri miktarını denetlemek için varsayılan 500 MB sınırına veya belirtilen günlük ucuna ulaşmış olabilirsiniz. Gün için sınır karşılandığında, veri toplama işlemi yalnızca bir sonraki günde duraklar ve sürdürülür. Veri kullanımınızı gözden geçirmek ve tahmin edilen kullanım desenlerinize göre farklı bir fiyatlandırma katmanına güncelleştirmek için bkz. [günlük verisi kullanımı ve maliyeti](../platform/manage-cost-storage.md). 

## <a name="what-are-the-container-states-specified-in-the-containerinventory-table"></a>Containerınventory tablosunda belirtilen kapsayıcı durumları nelerdir?

Containerınventory tablosu, hem durdurulan hem de çalışan kapsayıcılar hakkında bilgiler içerir. Tablo, tüm kapsayıcılar için Docker 'ı sorgulayan (çalışıyor ve durdurulmuş) ve bu verileri Log Analytics çalışma alanına ileten aracının içindeki bir iş akışı tarafından doldurulur.
 
## <a name="how-do-i-resolve-missing-subscription-registration-error"></a>Nasıl yaparım? *eksik abonelik kayıt* hatası.

**Microsoft. operationsmanagement Için eksik abonelik kaydı**hatası alırsanız, çalışma alanının tanımlandığı abonelikte **Microsoft. operationsmanagement** kaynak sağlayıcısını kaydederek bu sorunu çözebilirsiniz. Bunun nasıl yapılacağını gösteren belgeler [burada](../../azure-resource-manager/resource-manager-register-provider-errors.md)bulunabilir.

## <a name="is-there-support-for-rbac-enabled-aks-clusters"></a>RBAC etkin AKS kümeleri için destek var mı?

Kapsayıcı Izleme çözümü RBAC 'yi desteklemez, ancak kapsayıcılar için Azure Izleyici ile desteklenir. Çözüm ayrıntıları sayfası, bu kümelerin verilerini gösteren dikey pencerelerde doğru bilgileri göstermez.

## <a name="how-do-i-enable-log-collection-for-containers-in-the-kube-system-namespace-through-helm"></a>Nasıl yaparım?, KUIN-System ad alanındaki kapsayıcılar için günlük toplamayı Held aracılığıyla etkinleştirmek istiyor musunuz?

Kuto-System ad alanındaki kapsayıcılardan günlük koleksiyonu varsayılan olarak devre dışıdır. Omsagent üzerinde bir ortam değişkeni ayarlanarak günlük toplama etkinleştirilebilir. Daha fazla bilgi için bkz. [kapsayıcılar Için Azure izleyici](https://github.com/helm/charts/tree/master/incubator/azuremonitor-containers) GitHub sayfası. 

## <a name="how-do-i-update-the-omsagent-to-the-latest-released-version"></a>Omsagent 'ı yayınlanan en son sürüme güncelleştirmek Nasıl yaparım? mı?

Aracıyı yükseltmeyi öğrenmek için bkz. [Aracı yönetimi](container-insights-manage-agent.md).

## <a name="how-do-i-enable-multi-line-logging"></a>Çok satırlı günlüğe kaydetme etkinleştirilsin mi Nasıl yaparım??

Şu anda kapsayıcılar için Azure Izleyici çok satırlı günlüğe kaydetmeyi desteklemez, ancak kullanılabilir geçici çözümler vardır. Tüm hizmetleri JSON biçiminde yazacak şekilde yapılandırabilir ve ardından Docker/Moby, bunları tek bir satır olarak yazar.

Örneğin, bir örnek Node. js uygulaması için aşağıdaki örnekte gösterildiği gibi, günlüğünüz bir JSON nesnesi olarak sardırabilirsiniz:

```
console.log(json.stringify({ 
      "Hello": "This example has multiple lines:",
      "Docker/Moby": "will not break this into multiple lines",
      "and you will receive":"all of them in log analytics",
      "as one": "log entry"
      }));
```

Bu veriler, sorgulama yaptığınızda Günlükler için Azure Izleyici 'de aşağıdaki örnekteki gibi görünür:

```
LogEntry : ({“Hello": "This example has multiple lines:","Docker/Moby": "will not break this into multiple lines", "and you will receive":"all of them in log analytics", "as one": "log entry"}

```

Soruna ilişkin ayrıntılı bir bakış için aşağıdaki [GitHub bağlantısını](https://github.com/moby/moby/issues/22920)inceleyin.

## <a name="how-do-i-resolve-azure-ad-errors-when-i-enable-live-logs"></a>Canlı günlükleri etkinleştirdiğimde Azure AD hatalarını Nasıl yaparım? çözün misiniz? 

Şu hatayı görebilirsiniz: **istekte belirtilen yanıt URL 'si, uygulama için yapılandırılan yanıt URL 'leriyle eşleşmiyor: ' < Uygulama kimliği\>'** . Çözümü çözecek çözüm, [kapsayıcılar Için Azure izleyici ile kapsayıcı verilerinin gerçek zamanlı olarak nasıl görüntüleneceği](container-insights-livedata-setup.md#configure-ad-integrated-authentication)makalesinde bulunabilir. 

## <a name="why-cant-i-upgrade-cluster-after-onboarding"></a>Ekleme işleminden sonra kümeyi neden yükseltemiyorum?

Bir AKS kümesi için Azure Izleyicisini etkinleştirdikten sonra, kümenin verilerini gönderdiği Log Analytics çalışma alanını sildiğinizde, kümeyi yükseltmeye çalışırken başarısız olur. Bu sorunu geçici olarak çözmek için, izlemeyi devre dışı bırakmanız ve ardından aboneliğinizdeki farklı bir geçerli çalışma alanına başvuruda bulunan yeniden etkinleştirmeniz gerekir. Küme yükseltmesini yeniden gerçekleştirmeye çalıştığınızda, başarıyla işlemeli ve tamamlanmalıdır.  

## <a name="which-ports-and-domains-do-i-need-to-openwhitelist-for-the-agent"></a>Aracı için hangi bağlantı noktalarına ve etki alanlarına açık/beyaz listeye ihtiyacım var?

Azure, Azure ABD kamu ve Azure Çin bulutları ile Kapsayıcılı aracı için gereken ara sunucu ve güvenlik duvarı yapılandırma bilgileri için [ağ güvenlik duvarı gereksinimlerine](container-insights-onboard.md#network-firewall-requirements) bakın.

## <a name="next-steps"></a>Sonraki adımlar

AKS kümenizi izlemeye başlamak için gözden [nasıl eklenirim Azure izlemek için kapsayıcılar](container-insights-onboard.md) izlemeyi etkinleştirmek için gereksinimleri ve kullanılabilir yöntemlerin anlaşılması. 
