---
title: Kapsayıcılar için Azure Izleyici aracı veri toplamayı yapılandırma | Microsoft Docs
description: Bu makalede stdout/stderr ve ortam değişkenleri günlük toplamayı denetlemek için kapsayıcılar aracısının Azure Izleyicisini nasıl yapılandırabileceğiniz açıklanmaktadır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2019
ms.author: magoedte
ms.openlocfilehash: 12010aaa7bc90bd200264549ad3efb79f46576c6
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67867684"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici için aracı veri toplamayı yapılandırma

Kapsayıcılar için Azure Izleyici, Kapsayıcılı aracıdan Azure Kubernetes Service (AKS) üzerinde barındırılan yönetilen Kubernetes kümelerine dağıtılan kapsayıcı iş yüklerinden stdout, stderr ve ortam değişkenlerini toplar. Bu aracı, bir Prometheus sunucusunu ve veritabanını kurmak ve yönetmek zorunda kalmadan, kapsayıcı aracı kullanılarak Prometheus 'tan de zaman serisi verilerini (ölçümler olarak da bilinir) toplayabilir. Bu deneyimi denetlemek için özel bir Kubernetes ConfigMaps oluşturarak, aracı veri toplama ayarlarını yapılandırabilirsiniz. 

Bu makalede, gereksinimlerinize göre ConfigMap oluşturmayı ve veri toplamayı yapılandırmayı gösterir.

>[!NOTE]
>Prometheus desteği şu anda genel önizlemede bir özelliktir.
>

## <a name="configure-your-cluster-with-custom-data-collection-settings"></a>Kümenizi özel veri toplama ayarlarıyla yapılandırma

Bunu sıfırdan oluşturmak zorunda kalmadan özelleştirmelerinizle kolayca düzenlemenizi sağlayan bir şablon ConfigMap dosyası sağlanır. Başlamadan önce, Configmaps hakkında Kubernetes belgelerini gözden geçirmeniz [](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) ve configmaps oluşturma, yapılandırma ve dağıtma konusunda bilgi edinin. Bu, ad alanı başına veya tüm küme genelinde stderr ve STDOUT filtrelemenize ve kümedeki tüm düğüm/düğümlerde çalışan herhangi bir kapsayıcı için ortam değişkenlerine izin verir.

>[!IMPORTANT]
>Kapsayıcı iş yüklerinden stdout, stderr ve ortam değişkenlerini toplamak için desteklenen en düşük aracı sürümü ciprod06142019 veya üzeri. Scraping Prometheus ölçümleri için desteklenen en düşük aracı sürümü ciprod07092019 veya daha yeni bir sürüm. Aracı sürümünüzü doğrulamak için **düğüm** sekmesinde bir düğüm seçin ve Özellikler bölmesinde **Aracı görüntüsü etiketi** özelliğinin değeri.  

### <a name="overview-of-configurable-data-collection-settings"></a>Yapılandırılabilir veri toplama ayarlarına genel bakış

Aşağıda, veri toplamayı denetlemek için yapılandırılabilecek ayarlar verilmiştir.

|Anahtar |Veri türü |Değer |Açıklama |
|----|----------|------|------------|
|`schema-version` |Dize (büyük/küçük harfe duyarlı) |v1 |Bu, bu ConfigMap ayrıştırılırken aracı tarafından kullanılan şema sürümüdür. Şu anda desteklenen şema sürümü v1. Bu değerin değiştirilmesi desteklenmez ve ConfigMap değerlendirildiğinde reddedilir.|
|`config-version` |Dize | | , Kaynak denetimi sisteminizde/deponuzda bu yapılandırma dosyasının sürümünün izini sürme yeteneğini destekler. İzin verilen en fazla karakter sayısı 10 ' dur ve diğer tüm karakterler kesilir. |
|`[log_collection_settings.stdout] enabled =` |Boole değeri | true veya false | Bu, stdout kapsayıcı günlüğü koleksiyonunun etkinleştirilip etkinleştirilmediğini denetler. Olarak `true` ayarlandığında ve STDOUT günlük toplama için hiçbir ad alanı dışlandığında (`log_collection_settings.stdout.exclude_namespaces` aşağıdaki ayar), stdout günlükleri kümedeki tüm düğüm/düğümler genelinde tüm kapsayıcılardan toplanacaktır. ConfigMaps içinde belirtilmemişse, varsayılan değer `enabled = true`. |
|`[log_collection_settings.stdout] exclude_namespaces =`|Dize | Virgülle ayrılmış dizi |Stdout günlüklerinin toplanmayacak Kubernetes ad alanları dizisi. Bu ayar yalnızca `log_collection_settings.stdout.enabled` , olarak `true`ayarlandıysa geçerlidir. ConfigMap içinde belirtilmemişse, varsayılan değer `exclude_namespaces = ["kube-system"]`.|
|`[log_collection_settings.stderr] enabled =` |Boole değeri | true veya false |Bu, stderr kapsayıcı günlüğü koleksiyonunun etkinleştirilip etkinleştirilmediğini denetler. Olarak `true` ayarlandığında ve STDOUT günlük toplama (`log_collection_settings.stderr.exclude_namespaces` ayarı) için hiçbir ad alanı dışlandığında, stderr günlükleri kümedeki tüm düğüm/düğümler genelinde tüm kapsayıcılardan toplanacaktır. ConfigMaps içinde belirtilmemişse, varsayılan değer `enabled = true`. |
|`[log_collection_settings.stderr] exclude_namespaces =` |Dize |Virgülle ayrılmış dizi |Stderr günlüklerinin toplanmayacak Kubernetes ad alanları dizisi. Bu ayar yalnızca `log_collection_settings.stdout.enabled` , olarak `true`ayarlandıysa geçerlidir. ConfigMap içinde belirtilmemişse, varsayılan değer `exclude_namespaces = ["kube-system"]`. |
| `[log_collection_settings.env_var] enabled =` |Boole değeri | true veya false | Bu, ortam değişkeni koleksiyonunun etkin olup olmadığını denetler. Olarak `false`ayarlandığında, kümedeki tüm pods/düğümlerde çalışan herhangi bir kapsayıcı için hiçbir ortam değişkeni toplanmaz. ConfigMap içinde belirtilmemişse, varsayılan değer `enabled = true`. |

## <a name="overview-of-configurable-prometheus-scraping-settings"></a>Yapılandırılabilir Prometheus scraping ayarlarına genel bakış

Prometheus ölçülerinin etkin bir şekilde kullanılması iki perspektiften birinden yapılır:

* Küme genelinde HTTP URL 'SI ve bir hizmetin listelenen uç noktalarından hedefleri, kuas-DNS ve kuin-eyalet-durum ölçümleri gibi k8s Hizmetleri ve bir uygulamaya özgü Pod ek açıklamalarını bulur. Bu bağlamda toplanan ölçümler ConfigMap bölümünde *[Prometheus data_collection_settings. Cluster]* tanımlanacaktır.
* Düğüm genelinde HTTP URL 'SI ve bir hizmetin listelenen bitiş noktalarından hedefleri bulur. Bu bağlamda toplanan ölçümler ConfigMap bölümünde *[Prometheus_data_collection_settings. Node]* tanımlanacaktır.

|`Scope` | Anahtar | Veri türü | Value | Açıklama |
|------|-----|-----------|-------|-------------|
| Küme genelinde | | | | Ölçümler için atık uç noktalarına aşağıdaki üç yöntemden birini belirtin. |
| | `urls` | Dize | Virgülle ayrılmış dizi | HTTP uç noktası (IP adresi veya geçerli URL yolu belirtildi). Örneğin: `urls=[$NODE_IP/metrics]`. ($NODE _ıP, kapsayıcılar için belirli bir Azure Izleyici parametresi ve düğüm IP adresi yerine kullanılabilir. Tümü büyük harf olmalıdır.) |
| | `kubernetes_services` | Dize | Virgülle ayrılmış dizi | Kuin-State-ölçümlerini kullanarak bir Kubernetes hizmeti dizisi. Örneğin,`kubernetes_services = ["https://metrics-server.kube-system.svc.cluster.local/metrics", http://my-service-dns.my-namespace:9100/metrics]`.|
| | `monitor_kubernetes_pods` | Boole değeri | true veya false | , Küme genelinde `true` ayarlar halinde ayarlandığında, kapsayıcılar için Azure izleyici Aracı, aşağıdaki Prometheus ek açıklamaları için tüm küme genelinde atık olarak çalışır:<br> `prometheus.io/scrape:`<br> `prometheus.io/scheme:`<br> `prometheus.io/path:`<br> `prometheus.io/port:` |
| | `prometheus.io/scrape` | Boole değeri | true veya false | Pod 'un scraping öğesini sunar. |
| | `prometheus.io/scheme` | Dize | http veya https | Varsayılan olarak HTTP üzerinden atık yapılır. Gerekirse, olarak `https`ayarlayın. | 
| | `prometheus.io/path` | Dize | Virgülle ayrılmış dizi | Ölçümlerinin alınacağı HTTP kaynak yolu. Ölçüm yolu `/metrics`yoksa, bu ek açıklama ile tanımlayın. |
| | `prometheus.io/port` | Dize | 9102 | Dinlemek için bir bağlantı noktası belirtin. Bağlantı noktası ayarlanmamışsa, varsayılan olarak 9102 olur. |
| Düğüm genelinde | `urls` | Dize | Virgülle ayrılmış dizi | HTTP uç noktası (IP adresi veya geçerli URL yolu belirtildi). Örneğin: `urls=[$NODE_IP/metrics]`. ($NODE _ıP, kapsayıcılar için belirli bir Azure Izleyici parametresi ve düğüm IP adresi yerine kullanılabilir. Tümü büyük harf olmalıdır.) |
| Düğüm genelinde veya küme genelinde | `interval` | Dize | 60s | Koleksiyon aralığı varsayılan değeri bir dakikadır (60 saniye). *[Prometheus_data_collection_settings. Node]* ve/veya *[prometheus_data_collection_settings. Cluster]* koleksiyonunu NS, US (veya Âμs), MS, s, m, h gibi zaman birimlerine göre değiştirebilirsiniz. |
| Düğüm genelinde veya küme genelinde | `fieldpass`<br> `fielddrop`| Dize | Virgülle ayrılmış dizi | İzin ver (`fieldpass`) ve Allow (`fielddrop`) listesini ayarlayarak uç noktada toplanacak veya değil, belirli ölçümleri belirtebilirsiniz. Önce izin verilenler listesini ayarlamanız gerekir. |

ConfigMap genel bir liste ve aracıya uygulanan yalnızca bir ConfigMap olabilir. Koleksiyonlar üzerine başka bir ConfigMap olamaz.

### <a name="configure-and-deploy-configmaps"></a>ConfigMaps yapılandırma ve dağıtma

ConfigMap yapılandırma dosyanızı yapılandırmak ve kümenize dağıtmak için aşağıdaki adımları gerçekleştirin.

1. ConfigMap YAML dosyasını şablon olarak [indirin](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) ve kapsayıcı-AZM-MS-agentconfig. YAML olarak kaydedin.  
1. ConfigMap YAML dosyasını özelleştirmelerinizle düzenleyin.

    - Stdout günlük toplama için belirli ad alanlarını dışlamak için anahtar/değer aşağıdaki örneği kullanarak yapılandırılır: `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`.
    - Belirli bir kapsayıcı için ortam değişkeni toplamayı devre dışı bırakmak için, değişken toplamayı küresel `[log_collection_settings.env_var] enabled = true` olarak etkinleştirmek üzere anahtar/değer ayarlayın ve ardından belirli bir kapsayıcının yapılandırmasını gerçekleştirmek için [buradaki](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) adımları izleyin.
    - Stderr günlük toplama kümesi genelinde devre dışı bırakmak için anahtar/değer aşağıdaki örneği kullanarak yapılandırılır: `[log_collection_settings.stderr] enabled = false`.

1. Aşağıdaki kubectl komutunu çalıştırarak ConfigMap oluşturun: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Örnek: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    Yapılandırma değişikliğinin, yürürlüğe girmeden önce tamamlanması birkaç dakika sürebilir ve kümedeki tüm omsagent 'lar yeniden başlatılır. Yeniden başlatma, tüm omsagent pods için aynı anda yeniden başlatma işlemi için bir yeniden başlatma işlemi yapılır. Yeniden başlatmalar tamamlandığında, aşağıdakine benzer bir ileti görüntülenir ve sonucu içerir: `configmap "container-azm-ms-agentconfig" created`.

Yapılandırmanın başarıyla uygulandığını doğrulamak için, bir aracı Pod 'dan günlükleri gözden geçirmek üzere aşağıdaki komutu kullanın: `kubectl logs omsagent-fdf58 -n=kube-system`. Omsagent pods 'den yapılandırma hataları varsa, çıktıda aşağıdakine benzer hatalar gösterilir:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Prometheus için yapılandırma değişikliklerini uygulamayla ilgili hatalar İnceleme için de kullanılabilir.  Bir aracı Pod 'dan, aynı `kubectl logs` komutu veya canlı günlükleri kullanarak günlüklerden. Canlı günlüklerde aşağıdakine benzer hatalar gösterilmektedir:

```
2019-07-08T18:55:00Z E! [inputs.prometheus]: Error in plugin: error making HTTP request to http://invalidurl:1010/metrics: Get http://invalidurl:1010/metrics: dial tcp: lookup invalidurl on 10.0.0.10:53: no such host
```

Hatalar omsagent 'ın dosyayı ayrıştırmasını önler, yeniden başlatılmasına ve varsayılan yapılandırmayı kullanmasına neden olur. ConfigMap 'teki hataları düzelttikten sonra, YAML dosyasını kaydedin ve şu komutu çalıştırarak güncelleştirilmiş ConfigMaps 'leri uygulayın: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="applying-updated-configmap"></a>Güncelleştirilmiş ConfigMap uygulanıyor

Kümenize zaten bir ConfigMap dağıttıysanız ve daha yeni bir yapılandırmayla güncelleştirmek istiyorsanız, daha önce kullandığınız ConfigMap dosyasını düzenlemeniz ve ardından ile aynı komutu `kubectl apply -f <configmap_yaml_file.yaml`kullanarak uygulamanız yeterlidir.

Yapılandırma değişikliğinin, yürürlüğe girmeden önce tamamlanması birkaç dakika sürebilir ve kümedeki tüm omsagent 'lar yeniden başlatılır. Yeniden başlatma, tüm omsagent pods için aynı anda yeniden başlatma işlemi için bir yeniden başlatma işlemi yapılır. Yeniden başlatmalar tamamlandığında, aşağıdakine benzer bir ileti görüntülenir ve sonucu içerir: `configmap "container-azm-ms-agentconfig" updated`.

## <a name="verifying-schema-version"></a>Şema sürümü doğrulanıyor

Desteklenen yapılandırma şeması sürümleri, omsagent pod üzerinde pod ek açıklaması (şema-sürümler) olarak kullanılabilir. Aşağıdaki kubectl komutuyla bunları görebilirsiniz:`kubectl describe pod omsagent-fdf58 -n=kube-system`

Bu çıktı, ek açıklama şeması sürümleriyle aşağıdakine benzer şekilde görünür:

```
    Name:           omsagent-fdf58
    Namespace:      kube-system
    Node:           aks-agentpool-95673144-0/10.240.0.4
    Start Time:     Mon, 10 Jun 2019 15:01:03 -0700
    Labels:         controller-revision-hash=589cc7785d
                    dsName=omsagent-ds
                    pod-template-generation=1
    Annotations:    agentVersion=1.10.0.1
                  dockerProviderVersion=5.0.0-0
                    schema-versions=v1 
```

## <a name="review-prometheus-data-usage"></a>Prometheus veri kullanımını gözden geçirin

Her ölçüm boyutunun, yüksek olup olmadığını anlamak için günde GB cinsinden giriş hacmini belirlemek için aşağıdaki sorgu sağlanır.

```
InsightsMetrics 
| where Namespace contains "prometheus"
| where TimeGenerated > ago(24h)
| summarize VolumeInGB = (sum(_BilledSize) / (1024 * 1024 * 1024)) by Name
| order by VolumeInGB desc
| render barchart
```
Çıktı aşağıdakine benzer sonuçları gösterir:

![Veri alma biriminin günlük sorgu sonuçları](./media/container-insights-agent-config/log-query-example-usage-03.png)

Her ölçüm boyutunun GB cinsinden ne kadar olduğunu tahmin etmek için, çalışma alanında alınan veri hacminin yüksek olup olmadığını anlamak için aşağıdaki sorgu sağlanır.

```
InsightsMetrics 
| where Namespace contains "prometheus"
| where TimeGenerated > ago(24h)
| summarize EstimatedGBPer30dayMonth = (sum(_BilledSize) / (1024 * 1024 * 1024)) * 30 by Name
| order by EstimatedGBPer30dayMonth desc
| render barchart
```

Çıktı aşağıdakine benzer sonuçları gösterir:

![Veri alma biriminin günlük sorgu sonuçları](./media/container-insights-agent-config/log-query-example-usage-02.png)

Veri kullanımını izleme ve maliyeti çözümleme hakkında daha fazla bilgi, [Azure Izleyici günlükleriyle kullanımı ve maliyetleri yönetme](../platform/manage-cost-storage.md)bölümünde bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcılar için Azure Izleyici, önceden tanımlanmış bir uyarı kümesi içermez. DevOps veya işletimsel işlemlerinizi ve yordamlarınızı desteklemek üzere yüksek CPU ve bellek kullanımı için önerilen uyarılar oluşturmayı öğrenmek üzere [kapsayıcılar Için Azure izleyici ile performans uyarılarını oluşturma](container-insights-alerts.md) ' yı gözden geçirin.

- Azure İzleyici ve diğer yönleri AKS kümenizi izlemek öğrenme devam etmek için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](container-insights-analyze.md).

- Daha önceden tanımlanmış sorguları ve Uyarıları izlemek için değerlendirmek veya özelleştirmek üzere ön tanımlı sorguları ve örnekleri görmek için [günlük sorgusu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin, kümelerinizi görselleştirmeyi veya çözümlemeyi inceleyin.