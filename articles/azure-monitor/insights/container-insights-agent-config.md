---
title: Kapsayıcılar için Azure Izleyici aracı veri toplamayı yapılandırma | Microsoft Docs
description: Bu makalede stdout/stderr ve ortam değişkenleri günlük toplamayı denetlemek için kapsayıcılar aracısının Azure Izleyicisini nasıl yapılandırabileceğiniz açıklanmaktadır.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 10/08/2019
ms.openlocfilehash: 2b72252c5c85679c1c65fa2dcf9c5acc6c54003c
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72554210"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici için aracı veri toplamayı yapılandırma

Kapsayıcılar için Azure Izleyici, Kapsayıcılı aracıdan Azure Kubernetes Service (AKS) üzerinde barındırılan yönetilen Kubernetes kümelerine dağıtılan kapsayıcı iş yüklerinden stdout, stderr ve ortam değişkenlerini toplar. Bu aracı, bir Prometheus sunucusunu ve veritabanını ayarlayıp yönetmeye gerek kalmadan, kapsayıcı aracı kullanılarak Prometheus 'tan de zaman serisi verilerini (ölçümler olarak da bilinir) toplayabilir. Bu deneyimi denetlemek için özel bir Kubernetes ConfigMaps oluşturarak, aracı veri toplama ayarlarını yapılandırabilirsiniz. 

Bu makalede, gereksinimlerinize göre ConfigMap oluşturmayı ve veri toplamayı yapılandırmayı gösterir.

>[!NOTE]
>Prometheus desteği şu anda genel önizlemede bir özelliktir.
>

## <a name="configmap-file-settings-overview"></a>ConfigMap dosya ayarlarına genel bakış

Bunu sıfırdan oluşturmak zorunda kalmadan özelleştirmelerinizle kolayca düzenlemenizi sağlayan bir şablon ConfigMap dosyası sağlanır. Başlamadan önce, [configmaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) hakkında Kubernetes belgelerini gözden geçirmeniz ve configmaps oluşturma, yapılandırma ve dağıtma konusunda bilgi edinin. Bu, ad alanı başına veya tüm küme genelinde stderr ve STDOUT filtrelemenize ve kümedeki tüm düğüm/düğümlerde çalışan herhangi bir kapsayıcı için ortam değişkenlerine izin verir.

>[!IMPORTANT]
>Kapsayıcı iş yüklerinden stdout, stderr ve ortam değişkenlerini toplamak için desteklenen en düşük aracı sürümü ciprod06142019 veya üzeri. Scraping Prometheus ölçümleri için desteklenen en düşük aracı sürümü ciprod07092019 veya daha yeni bir sürüm. Aracı sürümünüzü doğrulamak için **düğüm** sekmesinde bir düğüm seçin ve Özellikler bölmesinde **Aracı görüntüsü etiketi** özelliğinin değeri.  

### <a name="data-collection-settings"></a>Veri toplama ayarları

Aşağıda, veri toplamayı denetlemek için yapılandırılabilecek ayarlar verilmiştir.

|Anahtar |Veri türü |Değer |Açıklama |
|----|----------|------|------------|
|`schema-version` |Dize (büyük/küçük harfe duyarlı) |v1 |Bu, bu ConfigMap ayrıştırılırken aracı tarafından kullanılan şema sürümüdür. Şu anda desteklenen şema sürümü v1. Bu değerin değiştirilmesi desteklenmez ve ConfigMap değerlendirildiğinde reddedilir.|
|`config-version` |Dize | | , Kaynak denetimi sisteminizde/deponuzda bu yapılandırma dosyasının sürümünün izini sürme yeteneğini destekler. İzin verilen en fazla karakter sayısı 10 ' dur ve diğer tüm karakterler kesilir. |
|`[log_collection_settings.stdout] enabled =` |Boole | true veya false | Bu, stdout kapsayıcı günlüğü koleksiyonunun etkinleştirilip etkinleştirilmediğini denetler. @No__t_0 olarak ayarlandığında ve STDOUT günlük toplama için hiçbir ad alanı dışlanmazsa (aşağıdaki `log_collection_settings.stdout.exclude_namespaces` ayarı), stdout günlükleri kümedeki tüm düğüm/düğümler genelinde tüm kapsayıcılardan toplanacaktır. ConfigMaps içinde belirtilmemişse, varsayılan değer `enabled = true` ' dır. |
|`[log_collection_settings.stdout] exclude_namespaces =`|Dize | Virgülle ayrılmış dizi |Stdout günlüklerinin toplanmayacak Kubernetes ad alanları dizisi. Bu ayar yalnızca `log_collection_settings.stdout.enabled` `true` olarak ayarlandıysa geçerlidir. ConfigMap içinde belirtilmemişse, varsayılan değer `exclude_namespaces = ["kube-system"]` olur.|
|`[log_collection_settings.stderr] enabled =` |Boole | true veya false |Bu, stderr kapsayıcı günlüğü koleksiyonunun etkinleştirilip etkinleştirilmediğini denetler. @No__t_0 olarak ayarlandığında ve STDOUT günlük toplama (`log_collection_settings.stderr.exclude_namespaces` ayarı) için hiçbir ad alanı dışlanmadığı zaman, stderr günlükleri kümedeki tüm düğüm/düğümler genelinde tüm kapsayıcılardan toplanacaktır. ConfigMaps içinde belirtilmemişse, varsayılan değer `enabled = true` ' dır. |
|`[log_collection_settings.stderr] exclude_namespaces =` |Dize |Virgülle ayrılmış dizi |Stderr günlüklerinin toplanmayacak Kubernetes ad alanları dizisi. Bu ayar yalnızca `log_collection_settings.stdout.enabled` `true` olarak ayarlandıysa geçerlidir. ConfigMap içinde belirtilmemişse, varsayılan değer `exclude_namespaces = ["kube-system"]` olur. |
| `[log_collection_settings.env_var] enabled =` |Boole | true veya false | Bu ayar, kümedeki tüm pods 'ler/düğümler genelinde ortam değişkeni toplamayı denetler ve ConfigMaps içinde belirtilmediğinde varsayılan olarak `enabled = true` olarak ayarlar. Ortam değişkenlerinin toplanması genel olarak etkinleştirilmişse, ortam değişkenini `AZMON_COLLECT_ENV` bir Dockerfile ayarı veya altındaki [](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/) **pod için yapılandırma dosyasında yanlış olarak ayarlayarak belirli bir kapsayıcı için devre dışı bırakabilirsiniz. env:** bölümü. Ortam değişkenlerinin toplanması genel olarak devre dışıysa, belirli bir kapsayıcı için koleksiyonu etkinleştiremezsiniz (yani, kapsayıcı düzeyinde uygulanabilecek tek geçersiz kılma, genel olarak etkinleştirildiğinde koleksiyonu devre dışı bırakmalıdır.). |

### <a name="prometheus-scraping-settings"></a>Prometheus scraping ayarları

![Prometheus için kapsayıcı izleme mimarisi](./media/container-insights-agent-config/monitoring-kubernetes-architecture.png)

Kapsayıcılar için Azure Izleyici, aşağıdaki tabloda gösterildiği gibi, aşağıdaki mekanizmalarla birden çok scraping ile Prometheus ölçümlerinin toplanmasını sağlamak için sorunsuz bir deneyim sunar. Ölçümler, kapsayıcı iş yüklerinden stdout, stderr ve ortam değişkenlerinin toplanmasını yapılandırmak için kullanılan dosya olan tek bir ConfigMap dosyasında belirtilen bir dizi ayar aracılığıyla toplanır. 

Prometheus ölçülerinin etkin bir şekilde kullanılması, iki perspektiften birinden gerçekleştirilir:

* Küme genelinde HTTP URL 'SI ve bir hizmetin listelenen uç noktalarından hedefleri, kuas-DNS ve kuin-eyalet-durum ölçümleri gibi k8s Hizmetleri ve bir uygulamaya özgü Pod ek açıklamalarını bulur. Bu bağlamda toplanan ölçümler ConfigMap bölümünde *[Prometheus data_collection_settings. Cluster]* tanımlanacaktır.
* Düğüm genelinde HTTP URL 'SI ve bir hizmetin listelenen bitiş noktalarından hedefleri bulur. Bu bağlamda toplanan ölçümler ConfigMap bölümünde *[Prometheus_data_collection_settings. Node]* tanımlanacaktır.

| Uç nokta | Kapsam | Örnek |
|----------|-------|---------|
| Pod ek açıklaması | Küme genelinde | açıklamaları <br>`prometheus.io/scrape: "true"` <br>`prometheus.io/path: "/mymetrics"` <br>`prometheus.io/port: "8000"` <br>`prometheus.io/scheme: "http"` |
| Kubernetes hizmeti | Küme genelinde | `http://my-service-dns.my-namespace:9100/metrics` <br>`https://metrics-server.kube-system.svc.cluster.local/metrics` |
| URL/uç nokta | Düğüm başına ve/veya küme genelinde | `http://myurl:9101/metrics` |

Bir URL belirtildiğinde, kapsayıcılar için Azure Izleyici yalnızca uç noktayı Scrapes. Kubernetes hizmeti belirtildiğinde, IP adresini almak için hizmet adı küme DNS sunucusu ile çözümlenir ve sonra çözümlenen hizmet, daha sonra çözülür.

|Kapsam | Anahtar | Veri türü | Değer | Açıklama |
|------|-----|-----------|-------|-------------|
| Küme genelinde | | | | Ölçümler için atık uç noktalarına aşağıdaki üç yöntemden birini belirtin. |
| | `urls` | Dize | Virgülle ayrılmış dizi | HTTP uç noktası (IP adresi veya geçerli URL yolu belirtildi). Örneğin: `urls=[$NODE_IP/metrics]`. ($NODE _ıP, kapsayıcılar için belirli bir Azure Izleyici parametresi ve düğüm IP adresi yerine kullanılabilir. Tümü büyük harf olmalıdır.) |
| | `kubernetes_services` | Dize | Virgülle ayrılmış dizi | Kuin-State-ölçümlerini kullanarak bir Kubernetes hizmeti dizisi. Örneğin, `kubernetes_services = ["https://metrics-server.kube-system.svc.cluster.local/metrics", http://my-service-dns.my-namespace:9100/metrics]`.|
| | `monitor_kubernetes_pods` | Boole | true veya false | Küme genelinde ayarları `true` olarak ayarlandığında, kapsayıcılar için Azure Izleyici Aracı, aşağıdaki Prometheus ek açıklamaları için tüm küme genelinde düğüm genellerini hurdaya çıkarabilirsiniz:<br> `prometheus.io/scrape:`<br> `prometheus.io/scheme:`<br> `prometheus.io/path:`<br> `prometheus.io/port:` |
| | `prometheus.io/scrape` | Boole | true veya false | Pod 'un scraping öğesini sunar. `monitor_kubernetes_pods` `true` olarak ayarlanmalıdır. |
| | `prometheus.io/scheme` | Dize | http veya https | Varsayılan olarak HTTP üzerinden atık yapılır. Gerekirse, `https` olarak ayarlayın. | 
| | `prometheus.io/path` | Dize | Virgülle ayrılmış dizi | Ölçümlerinin alınacağı HTTP kaynak yolu. Ölçüm yolu `/metrics` değilse, bu ek açıklama ile tanımlayın. |
| | `prometheus.io/port` | Dize | 9102 | Iskartaya çıkış için bir bağlantı noktası belirtin. Bağlantı noktası ayarlanmamışsa, varsayılan olarak 9102 olur. |
| Düğüm genelinde | `urls` | Dize | Virgülle ayrılmış dizi | HTTP uç noktası (IP adresi veya geçerli URL yolu belirtildi). Örneğin: `urls=[$NODE_IP/metrics]`. ($NODE _ıP, kapsayıcılar için belirli bir Azure Izleyici parametresi ve düğüm IP adresi yerine kullanılabilir. Tümü büyük harf olmalıdır.) |
| Düğüm genelinde veya küme genelinde | `interval` | Dize | 60s | Koleksiyon aralığı varsayılan değeri bir dakikadır (60 saniye). Koleksiyonu *[prometheus_data_collection_settings. Node]* ve/veya *[prometheus_data_collection_settings. Cluster]* için s, m, h gibi zaman birimlerine göre değiştirebilirsiniz. |
| Düğüm genelinde veya küme genelinde | `fieldpass`<br> `fielddrop`| Dize | Virgülle ayrılmış dizi | İzin ver (`fieldpass`) ve izin verme (`fielddrop`) listesini ayarlayarak uç noktada toplanacak veya değil, belirli ölçümleri belirtebilirsiniz. Önce izin verilenler listesini ayarlamanız gerekir. |

ConfigMaps genel bir liste ve aracıya yalnızca bir ConfigMap uygulanmış olabilir. Koleksiyonlar üzerine başka bir ConfigMaps olamaz.

## <a name="configure-and-deploy-configmaps"></a>ConfigMaps yapılandırma ve dağıtma

ConfigMap yapılandırma dosyanızı yapılandırmak ve kümenize dağıtmak için aşağıdaki adımları gerçekleştirin.

1. ConfigMap YAML dosyasını şablon olarak [indirin](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) ve kapsayıcı-AZM-MS-agentconfig. YAML olarak kaydedin.  

2. Stdout, stderr ve/veya ortam değişkenlerini toplamak için, özelleştirmelerinizle ConfigMap YAML dosyasını düzenleyin.

    - Stdout günlük toplama için belirli ad alanlarını dışlamak üzere anahtarı/değeri aşağıdaki örneği kullanarak yapılandırırsınız: `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`.
    
    - Belirli bir kapsayıcı için ortam değişkeni toplamayı devre dışı bırakmak için, anahtar/değer `[log_collection_settings.env_var] enabled = true` değişken toplamayı küresel olarak etkinleştirecek şekilde ayarlayın ve ardından belirli bir kapsayıcının yapılandırmasını gerçekleştirmek için [buradaki](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) adımları izleyin.
    
    - Stderr günlük toplama kümesi genelinde devre dışı bırakmak için anahtar/değer aşağıdaki örneği kullanarak yapılandırılır: `[log_collection_settings.stderr] enabled = false`.
    
3. Kubernetes Services kümesi genelinde toplamayı yapılandırmak için aşağıdaki örneği kullanarak ConfigMap dosyasını yapılandırın.

    ```
    prometheus-data-collection-settings: |- 
    # Custom Prometheus metrics data collection settings
    [prometheus_data_collection_settings.cluster] 
    interval = "1m"  ## Valid time units are s, m, h.
    fieldpass = ["metric_to_pass1", "metric_to_pass12"] ## specify metrics to pass through 
    fielddrop = ["metric_to_drop"] ## specify metrics to drop from collecting
    kubernetes_services = ["http://my-service-dns.my-namespace:9102/metrics"]
    ```

4. Küme genelinde belirli bir URL 'den Prometheus ölçümlerinin bir listesini yapılandırmak için, aşağıdaki örneği kullanarak ConfigMap dosyasını yapılandırın.

    ```
    prometheus-data-collection-settings: |- 
    # Custom Prometheus metrics data collection settings
    [prometheus_data_collection_settings.cluster] 
    interval = "1m"  ## Valid time units are s, m, h.
    fieldpass = ["metric_to_pass1", "metric_to_pass12"] ## specify metrics to pass through 
    fielddrop = ["metric_to_drop"] ## specify metrics to drop from collecting
    urls = ["http://myurl:9101/metrics"] ## An array of urls to scrape metrics from
    ```

5. Kümedeki her bir düğüm için bir aracının DaemonSet Prometheus ölçümlerinin scraping 'ini yapılandırmak için ConfigMap içinde aşağıdakileri yapılandırın:
    
    ```
    prometheus-data-collection-settings: |- 
    # Custom Prometheus metrics data collection settings 
    [prometheus_data_collection_settings.node] 
    interval = "1m"  ## Valid time units are s, m, h. 
    urls = ["http://$NODE_IP:9103/metrics"] 
    fieldpass = ["metric_to_pass1", "metric_to_pass2"] 
    fielddrop = ["metric_to_drop"] 
    ```

    >[!NOTE]
    >$NODE _ıP, kapsayıcılar için belirli bir Azure Izleyici parametresi ve düğüm IP adresi yerine kullanılabilir. Tümü büyük harf olmalıdır. 

6. Bir pod ek açıklaması belirterek Prometheus ölçümlerinin korumasını yapılandırmak için aşağıdaki adımları uygulayın:

    1. ConfigMap ' te şunları belirtin:

        ```
         prometheus-data-collection-settings: |- 
         # Custom Prometheus metrics data collection settings
         [prometheus_data_collection_settings.cluster] 
         interval = "1m"  ## Valid time units are s, m, h
         monitor_kubernetes_pods = true 
        ```

    2. Pod ek açıklamaları için aşağıdaki yapılandırmayı belirtin:

        ```
         - prometheus.io/scrape:"true" #Enable scraping for this pod 
         - prometheus.io/scheme:"http:" #If the metrics endpoint is secured then you will need to set this to `https`, if not default ‘http’
         - prometheus.io/path:"/mymetrics" #If the metrics path is not /metrics, define it with this annotation. 
         - prometheus.io/port:"8000" #If port is not 9102 use this annotation
        ```

7. Aşağıdaki kubectl komutunu çalıştırarak ConfigMap oluşturun: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Örnek: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    Yapılandırma değişikliğinin, yürürlüğe girmeden önce tamamlanması birkaç dakika sürebilir ve kümedeki tüm omsagent 'lar yeniden başlatılır. Yeniden başlatma, tüm omsagent pods için aynı anda yeniden başlatma işlemi için bir yeniden başlatma işlemi yapılır. Yeniden başlatmalar tamamlandığında aşağıdakine benzer bir ileti görüntülenir ve sonuç: `configmap "container-azm-ms-agentconfig" created` olur.

Yapılandırmanın başarıyla uygulandığını doğrulamak için şu komutu kullanarak bir aracı Pod öğesinden günlükleri gözden geçirin: `kubectl logs omsagent-fdf58 -n=kube-system`. Omsagent pods 'den yapılandırma hataları varsa, çıktıda aşağıdakine benzer hatalar gösterilir:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Prometheus için yapılandırma değişikliklerini uygulamayla ilgili hatalar İnceleme için de kullanılabilir.  Bir aracı Pod öğesinden, aynı `kubectl logs` komutunu veya canlı günlükleri kullanarak günlüklerden. Canlı günlüklerde aşağıdakine benzer hatalar gösterilir:

```
2019-07-08T18:55:00Z E! [inputs.prometheus]: Error in plugin: error making HTTP request to http://invalidurl:1010/metrics: Get http://invalidurl:1010/metrics: dial tcp: lookup invalidurl on 10.0.0.10:53: no such host
```

Hatalar omsagent 'ın dosyayı ayrıştırmasını önler, yeniden başlatılmasına ve varsayılan yapılandırmayı kullanmasına neden olur. ConfigMap 'teki hataları düzelttikten sonra, YAML dosyasını kaydedin ve şu komutu çalıştırarak güncelleştirilmiş ConfigMaps 'leri uygulayın: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="applying-updated-configmap"></a>Güncelleştirilmiş ConfigMap uygulanıyor

Kümenize zaten bir ConfigMap dağıttıysanız ve daha yeni bir yapılandırmayla güncelleştirmek istiyorsanız, daha önce kullandığınız ConfigMap dosyasını düzenleyebilir ve sonra, `kubectl apply -f <configmap_yaml_file.yaml` aynı komutu kullanarak uygulayabilirsiniz.

Yapılandırma değişikliğinin, yürürlüğe girmeden önce tamamlanması birkaç dakika sürebilir ve kümedeki tüm omsagent 'lar yeniden başlatılır. Yeniden başlatma, tüm omsagent pods için aynı anda yeniden başlatma işlemi için bir yeniden başlatma işlemi yapılır. Yeniden başlatmalar tamamlandığında aşağıdakine benzer bir ileti görüntülenir ve sonuç: `configmap "container-azm-ms-agentconfig" updated` olur.

## <a name="verifying-schema-version"></a>Şema sürümü doğrulanıyor

Desteklenen yapılandırma şeması sürümleri, omsagent pod üzerinde pod ek açıklaması (şema-sürümler) olarak kullanılabilir. Aşağıdaki kubectl komutuyla bunları görebilirsiniz: `kubectl describe pod omsagent-fdf58 -n=kube-system`

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

## <a name="query-prometheus-metrics-data"></a>Sorgu Prometheus ölçüm verileri

Azure Izleyici tarafından oluşturulan Prometheus ölçümlerini görüntülemek için ad alanı olarak "Prometheus" belirtin. İşte `default` Kubernetes ad alanından Prometheus ölçümlerini görüntülemek için örnek bir sorgu.

```
InsightsMetrics 
| where Namespace == "prometheus"
| extend tags=parse_json(Tags)
| summarize count() by Name
```

Prometheus verileri, ad ile doğrudan sorgulanabilir.

```
InsightsMetrics 
| where Namespace == "prometheus"
| where Name contains "some_prometheus_metric"
```

## <a name="review-prometheus-data-usage"></a>Prometheus veri kullanımını gözden geçirin

Her ölçüm boyutunun, yüksek olup olmadığını anlamak için günde GB cinsinden giriş hacmini belirlemek için aşağıdaki sorgu sağlanır.

```
InsightsMetrics 
| where Namespace == "prometheus"
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

- Azure Izleyici 'yi kullanmayı ve AKS kümenizin diğer yönlerini izlemeyi öğrenmeye devam etmek için bkz. [Azure Kubernetes hizmet durumunu görüntüleme](container-insights-analyze.md).

- Daha önceden tanımlanmış sorguları ve Uyarıları izlemek için değerlendirmek veya özelleştirmek üzere ön tanımlı sorguları ve örnekleri görmek için [günlük sorgusu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin, kümelerinizi görselleştirmeyi veya çözümlemeyi inceleyin.
