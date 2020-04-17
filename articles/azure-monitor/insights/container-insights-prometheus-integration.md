---
title: Prometheus Tümleştirme kapları için Azure Monitörünü yapılandırın | Microsoft Dokümanlar
description: Bu makalede, Kubernetes kümenizle Prometheus'un ölçümlerini kazımak için kapsayıcılar aracısı için Azure Monitörünü nasıl yapılandırabileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 04/16/2020
ms.openlocfilehash: 7fcf52cceb69834f68f8e4ce7a2674972a6430fd
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537381"
---
# <a name="configure-scraping-of-prometheus-metrics-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitörü ile Prometheus ölçümlerinin kazınma işlemlerini yapılandırın

[Prometheus](https://prometheus.io/) popüler bir açık kaynak metrik izleme çözümüdür ve [Cloud Native Compute Foundation'ın](https://www.cncf.io/)bir parçasıdır. Kapsayıcılar için Azure Monitör, Prometheus ölçümlerini toplamak için sorunsuz bir biniş deneyimi sağlar. Genellikle, Prometheus kullanmak için, bir mağaza ile bir Prometheus sunucu kurmak ve yönetmek gerekir. Azure Monitor ile tümleştirerek bir Prometheus sunucusu gerekmez. Prometheus ölçümlerinin bitiş noktasını ihracatçılarınız veya bölmeleriniz (uygulama) aracılığıyla ortaya çıkarmanız gerekir ve kapsayıcılar için Azure Monitor için konteyneraracı sıyrıklarını sizin için ölçümleri kazıyabilir. 

![Prometheus için konteyner izleme mimarisi](./media/container-insights-prometheus-integration/monitoring-kubernetes-architecture.png)

>[!NOTE]
>Prometheus ölçümlerini kazımak için desteklenen minimum aracı sürümü ciprod07092019 veya sonraki sürümdür ve `KubeMonAgentEvents` tablodaki yapılandırma ve aracı hataları yazmak için desteklenen aracı sürümü ciprod10112019'dur. Aracı sürümleri ve her sürümde nelerin yer aldığı hakkında daha fazla bilgi için [aracı sürüm notlarına](https://github.com/microsoft/Docker-Provider/tree/ci_feature_prod)bakın. Aracı lı sürümünüzü doğrulamak için, **Düğüm** sekmesinden bir düğüm seçin ve **Aracı Görüntü Etiketi** özelliğinin özellik bölmesinde not değeri belirleyin.

Prometheus ölçümlerinin kazıntımı, barındırılan Kubernetes kümeleri ile desteklenir:

- Azure Kubernetes Hizmeti (AKS)
- Azure Yığını veya şirket içi
- Azure Red Hat OpenShift

>[!NOTE]
>Azure Red Hat OpenShift için *openshift-azure oturum açma* alanında bir şablon ConfigMap dosyası oluşturulur. Aracıdan ölçümleri veya veri toplamayı etkin olarak kazımak için yapılandırılmamıştır.
>

## <a name="azure-red-hat-openshift-prerequisites"></a>Azure Red Hat OpenShift Ön Koşulları

Başlamadan önce, kapsayıcı aracıyı ve Prometheus kazıma ayarlarını yapılandırmak için Azure Red Hat OpenShift kümenizin Müşteri Küme Yöneticisi rolünün bir üyesi olduğunuzu onaylayın. *OSA-müşteri yöneticileri* grubunun bir üyesi olduğunuzu doğrulamak için aşağıdaki komutu çalıştırın:

``` bash
  oc get groups
```

Çıktı aşağıdakilere benzeyecektir:

``` bash
NAME                  USERS
osa-customer-admins   <your-user-account>@<your-tenant-name>.onmicrosoft.com
```

*OSA-müşteri-yöneticiler* grubunun üyesiyseniz, `container-azm-ms-agentconfig` Aşağıdaki komutu kullanarak ConfigMap'i listeleyebilmelisiniz:

``` bash
oc get configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

Çıktı aşağıdakilere benzeyecektir:

``` bash
NAME                           DATA      AGE
container-azm-ms-agentconfig   4         56m
```

### <a name="prometheus-scraping-settings"></a>Prometheus kazıma ayarları

Prometheus'tan ölçümlerin etkin kazıntımı iki perspektiften birinden gerçekleştirilir:

* Küme çapında - HTTP URL ve bir hizmetin listelenen uç noktalarından hedefleri keşfedin. Örneğin, kube-dns ve kube-state-ölçümleri gibi k8s hizmetleri ve bir uygulamaya özgü bölme ek açıklamaları. Bu bağlamda toplanan ölçümler ConfigMap bölümünde tanımlanır *[Prometheus data_collection_settings.cluster]*.
* Düğüm çapında - HTTP URL ve bir hizmetin listelenen uç noktalarından hedefleri keşfedin. Bu bağlamda toplanan ölçümler ConfigMap bölümünde *[Prometheus_data_collection_settings.düğüm]* tanımlanır.

| Uç Nokta | Kapsam | Örnek |
|----------|-------|---------|
| Pod ek açıklama | Küme genişliğinde | Ek açıklama -ları: <br>`prometheus.io/scrape: "true"` <br>`prometheus.io/path: "/mymetrics"` <br>`prometheus.io/port: "8000"` <br>`prometheus.io/scheme: "http"` |
| Kubernetes servisi | Küme genişliğinde | `http://my-service-dns.my-namespace:9100/metrics` <br>`https://metrics-server.kube-system.svc.cluster.local/metrics` |
| url/bitiş noktası | Düğüm başına ve/veya küme genelinde | `http://myurl:9101/metrics` |

Bir URL belirtildiğinde, kapsayıcılar için Azure Monitörü yalnızca bitiş noktasını sıyırır. Kubernetes hizmeti belirtildiğinde, ip adresini almak için küme DNS sunucusu ile hizmet adı çözülür ve ardından çözülen hizmet kazınır.

|Kapsam | Anahtar | Veri türü | Değer | Açıklama |
|------|-----|-----------|-------|-------------|
| Küme genişliğinde | | | | Ölçümler için uç noktaları kazımak için aşağıdaki üç yöntemden birini belirtin. |
| | `urls` | Dize | Virgülle ayrılmış dizi | HTTP bitiş noktası (IP adresi veya geçerli URL yolu belirtilir). Örneğin: `urls=[$NODE_IP/metrics]`. ($NODE_IP, kapsayıcılar parametresi için belirli bir Azure Monitörüdür ve düğüm IP adresi yerine kullanılabilir. Tüm büyük harf olmalıdır.) |
| | `kubernetes_services` | Dize | Virgülle ayrılmış dizi | Kube-state-metrics ölçümleri kazımak için Kubernetes hizmetleri bir dizi. Örneğin,`kubernetes_services = ["https://metrics-server.kube-system.svc.cluster.local/metrics",http://my-service-dns.my-namespace:9100/metrics]`.|
| | `monitor_kubernetes_pods` | Boole | true veya false | Küme genelindeki `true` ayarlarda ayarlandığında, kapsayıcılar için Azure Monitor aracısı aşağıdaki Prometheus ek açıklamaları için tüm kümeye Kubernetes bölmelerini kazır:<br> `prometheus.io/scrape:`<br> `prometheus.io/scheme:`<br> `prometheus.io/path:`<br> `prometheus.io/port:` |
| | `prometheus.io/scrape` | Boole | true veya false | Bölmenin kazınmasını sağlar. `monitor_kubernetes_pods``true`olarak ayarlanmalıdır. |
| | `prometheus.io/scheme` | Dize | http veya https | Varsayılan olarak HTTP üzerinden hurdaya çıkmak için. Gerekirse, `https`ayarlayın. | 
| | `prometheus.io/path` | Dize | Virgülle ayrılmış dizi | Ölçümleri almak için http kaynak yolu. Metrikler yolu değilse, `/metrics`bu ek açıklama ile tanımlayın. |
| | `prometheus.io/port` | Dize | 9102 | Kazınacak bir bağlantı noktası belirtin. Bağlantı noktası ayarlı değilse, varsayılan olarak 9102'ye göre olur. |
| | `monitor_kubernetes_pods_namespaces` | Dize | Virgülle ayrılmış dizi | Kubernetes bölmelerinden ölçümleri kazımak için ad alanlarının listesine izin verir.<br> Örneğin, `monitor_kubernetes_pods_namespaces = ["default1", "default2", "default3"]` |
| Düğüm genişliğinde | `urls` | Dize | Virgülle ayrılmış dizi | HTTP bitiş noktası (IP adresi veya geçerli URL yolu belirtilir). Örneğin: `urls=[$NODE_IP/metrics]`. ($NODE_IP, kapsayıcılar parametresi için belirli bir Azure Monitörüdür ve düğüm IP adresi yerine kullanılabilir. Tüm büyük harf olmalıdır.) |
| Düğüm çapında veya Küme genelinde | `interval` | Dize | 60s | Toplama aralığı varsayılan bir dakika (60 saniye). Koleksiyonu *[prometheus_data_collection_settings.düğüm]* ve/veya *[prometheus_data_collection_settings.cluster]* için s, m, h gibi zaman birimleriyle değiştirebilirsiniz. |
| Düğüm çapında veya Küme genelinde | `fieldpass`<br> `fielddrop`| Dize | Virgülle ayrılmış dizi | İzin ver (`fieldpass`) ve izin verme (`fielddrop`) girişini ayarlayarak, son noktadan toplanacak veya toplanmayacak belirli ölçümler belirtebilirsiniz. İzin listesini önce ayarlamanız gerekir. |

ConfigMaps küresel bir listedir ve aracıya yalnızca bir ConfigMap uygulanabilir. Koleksiyonları geçersiz kakan başka bir ConfigMaps olamaz.

## <a name="configure-and-deploy-configmaps"></a>ConfigMaps'i yapılandırma ve dağıtma

Kubernetes kümeleri için ConfigMap yapılandırma dosyanızı yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. Şablon ConfigMap yaml dosyasını [indirin](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) ve konteyner-azm-ms-agentconfig.yaml olarak kaydedin.

   >[!NOTE]
   >ConfigMap şablonu kümede zaten mevcut olduğundan, Azure Red Hat OpenShift ile çalışırken bu adım gerekli değildir.

2. Prometheus ölçümlerini kazımak için özelleştirmelerinizle ConfigMap yaml dosyasını edin. Azure Red Hat OpenShift için ConfigMap yaml dosyasını düzenliyorsanız, önce dosyayı metin düzenleyicisinde açmak için komutu `oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging` çalıştırın.

    >[!NOTE]
    >Mutabakatı önlemek `openshift.io/reconcile-protect: "true"` için *konteyner-azm-ms-agentconfig* ConfigMap meta verilerinin altına aşağıdaki ek açıklama eklenmelidir. 
    >```
    >metadata:
    >   annotations:
    >       openshift.io/reconcile-protect: "true"
    >```

    - Kubernetes hizmetlerini küme genelinde toplamak için aşağıdaki örneği kullanarak ConfigMap dosyasını yapılandırın.

        ```
        prometheus-data-collection-settings: |- 
        # Custom Prometheus metrics data collection settings
        [prometheus_data_collection_settings.cluster] 
        interval = "1m"  ## Valid time units are s, m, h.
        fieldpass = ["metric_to_pass1", "metric_to_pass12"] ## specify metrics to pass through 
        fielddrop = ["metric_to_drop"] ## specify metrics to drop from collecting
        kubernetes_services = ["http://my-service-dns.my-namespace:9102/metrics"]
        ```

    - Prometheus ölçümlerinin küme deki belirli bir URL'den kazınmasını yapılandırmak için aşağıdaki örneği kullanarak ConfigMap dosyasını yapılandırın.

        ```
        prometheus-data-collection-settings: |- 
        # Custom Prometheus metrics data collection settings
        [prometheus_data_collection_settings.cluster] 
        interval = "1m"  ## Valid time units are s, m, h.
        fieldpass = ["metric_to_pass1", "metric_to_pass12"] ## specify metrics to pass through 
        fielddrop = ["metric_to_drop"] ## specify metrics to drop from collecting
        urls = ["http://myurl:9101/metrics"] ## An array of urls to scrape metrics from
        ```

    - Kümedeki her bir düğüm için bir aracının DaemonSet'inden Prometheus ölçümlerinin kazınmasını yapılandırmak için ConfigMap'te aşağıdakileri yapılandırın:
    
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
        >$NODE_IP, kapsayıcılar parametresi için belirli bir Azure Monitörüdür ve düğüm IP adresi yerine kullanılabilir. Hepsi büyük harfle olmalı. 

    - Prometheus ölçümlerinin kazınmasını bölme ek açıklamasını belirterek yapılandırmak için aşağıdaki adımları gerçekleştirin:

       1. ConfigMap'te aşağıdakileri belirtin:

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
    
          Ek açıklamaları olan bölmeler için izlemeyi belirli ad alanlarıyla sınırlamak istiyorsanız, örneğin yalnızca üretim iş yüklerine adanmış bölmeleri içerir, ConfigMap'te ayarlayın `monitor_kubernetes_pod` `true` ve kazınacak ad alanlarını belirten ad alanı filtresi `monitor_kubernetes_pods_namespaces` ekleyin. Örneğin, `monitor_kubernetes_pods_namespaces = ["default1", "default2", "default3"]`

3. Azure Red Hat OpenShift dışındaki kümeler için aşağıdaki kubectl komutunu çalıştırın: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Örnek: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 

    Azure Red Hat OpenShift için değişikliklerinizi editöre kaydedin.

Yapılandırma değişikliğinin etkinleşmeden çağrısının bitişi birkaç dakika sürebilir ve kümedeki bütün omsagent podları yeniden başlatırılabilir. Yeniden başlatma, tüm omsagent bölmeleri için bir yuvarlanma yeniden başlatma, aynı anda tüm yeniden başlatma. Yeniden başlatmalar tamamlandığında, aşağıdakilere benzer ve sonucu içeren bir ileti `configmap "container-azm-ms-agentconfig" created`görüntülenir: .

Azure Red Hat OpenShift için güncelleştirilmiş ConfigMap komutunu çalıştırarak `oc describe configmaps container-azm-ms-agentconfig -n openshift-azure-logging`görüntüleyebilirsiniz. 

## <a name="applying-updated-configmap"></a>Güncelleştirilmiş ConfigMap uygulama

Kümenize zaten bir ConfigMap dağıttıysanız ve bunu daha yeni bir yapılandırmayla güncelleştirmek istiyorsanız, daha önce kullandığınız ConfigMap dosyasını güncelleyebilir ve daha önce olduğu gibi aynı komutları kullanarak uygulayabilirsiniz.

Azure Red Hat OpenShift dışındaki Kubernetes kümeleri `kubectl apply -f <configmap_yaml_file.yaml`için komutu çalıştırın. 

Azure Red Hat OpenShift kümesi için, dosyayı değiştirmek `oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging` ve sonra kaydetmek için varsayılan düzenleyicinizde açmak için komutu çalıştırın.

Yapılandırma değişikliğinin etkinleşmeden çağrısının bitişi birkaç dakika sürebilir ve kümedeki bütün omsagent podları yeniden başlatırılabilir. Yeniden başlatma, tüm omsagent bölmeleri için bir yuvarlanma yeniden başlatma, aynı anda tüm yeniden başlatma. Yeniden başlatmalar tamamlandığında, aşağıdakilere benzer ve sonucu içeren bir ileti `configmap "container-azm-ms-agentconfig" updated`görüntülenir: .

## <a name="verify-configuration"></a>Yapılandırmayı doğrulama

Yapılandırmanın bir kümeye başarıyla uygulandığını doğrulamak için, aracı bölmesinden günlükleri `kubectl logs omsagent-fdf58 -n=kube-system`gözden geçirmek için aşağıdaki komutu kullanın: . 

>[!NOTE]
>Bu komut Azure Red Hat OpenShift kümesi için geçerli değildir.
> 

Omsagent bölmelerinden yapılandırma hataları varsa, çıktı aşağıdakilere benzer hatalar gösterir:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Yapılandırma değişikliklerinin uygulanmasıyla ilgili hatalar da gözden geçirilebilir. Yapılandırma değişikliklerinin ek sorun giderme ve Prometheus ölçümlerinin kazınması için aşağıdaki seçenekler kullanılabilir:

- Aynı `kubectl logs` komutu kullanarak bir aracı pod günlükleri gönderen 
    >[!NOTE]
    >Bu komut Azure Red Hat OpenShift kümesi için geçerli değildir.
    > 

- Canlı Verilerden (önizleme). Canlı Veri (önizleme) günlükleri aşağıdakilere benzer hataları gösterir:

    ```
    2019-07-08T18:55:00Z E! [inputs.prometheus]: Error in plugin: error making HTTP request to http://invalidurl:1010/metrics: Get http://invalidurl:1010/metrics: dial tcp: lookup invalidurl on 10.0.0.10:53: no such host
    ```

- Log Analytics çalışma alanınızdaki **KubeMonAgentEvents** tablosundan. Veriler, kazıma hataları için *Uyarı* önem derecesi ve yapılandırma hataları için *Hata* önem derecesi ile her saat gönderilir. Hata yoksa, tablodaki *giriş,* hiçbir hata bildiren önem Bilgisi içeren verilere sahip olacaktır. **Etiketler** özelliği, hatanın oluştuğu bölme ve kapsayıcı kimliği ve ayrıca son saat içinde ilk oluşumu, son oluşumu ve sayısı hakkında daha fazla bilgi içerir.

- Azure Red Hat OpenShift için, openshift-azure günlük günlüğü koleksiyonunun etkin olup olmadığını doğrulamak için **ContainerLog** tablosunda arama yaparak omsagent günlüklerini kontrol edin.

Hatalar omsagent'un dosyayı ayrıştmasını önleyerek yeniden başlatılmasına ve varsayılan yapılandırmayı kullanmasına neden olur. Azure Red Hat OpenShift dışındaki kümelerde ConfigMap'teki hatayı düzelttikten sonra, yaml dosyasını kaydedin ve `kubectl apply -f <configmap_yaml_file.yaml`aşağıdaki komutu çalıştırarak güncelleştirilmiş ConfigMaps'i uygulayın: . 

Azure Red Hat OpenShift için, aşağıdaki komutu çalıştırarak güncelleştirilmiş ConfigMaps'i düzenleme ve kaydedin: `oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging`.

## <a name="query-prometheus-metrics-data"></a>Prometheus ölçümlerini sorgula veri

Azure Monitor tarafından kazınmış prometheus ölçümlerini ve aracı tarafından bildirilen yapılandırma/kazıma hatalarını görüntülemek için [Sorgu Prometheus ölçümleri verilerini](container-insights-log-search.md#query-prometheus-metrics-data) ve [Sorgu config veya kazıma hatalarını](container-insights-log-search.md#query-config-or-scraping-errors)gözden geçirin.

## <a name="view-prometheus-metrics-in-grafana"></a>Grafana'daki Prometheus ölçümlerini görüntüleyin

Kapsayıcılar için Azure Monitor, Grafana panolarındaki Günlük Analizi çalışma alanınızda depolanan görüntüleme ölçümlerini destekler. Özel Grafana panolarında görselleştirmek için izlenen kümelerinizden ek verileri sorgulamayı öğrenmenize yardımcı olmak için Grafana'nın [pano deposundan](https://grafana.com/grafana/dashboards?dataSource=grafana-azure-monitor-datasource&category=docker) indirebileceğiniz bir şablon ve referans sağladık. 

## <a name="review-prometheus-data-usage"></a>Prometheus veri kullanımını gözden geçirin

Yüksek olup olmadığını anlamak için her ölçüm boyutunun günlük GB'deki yutma hacmini belirlemek için aşağıdaki sorgu sağlanır.

```
InsightsMetrics 
| where Namespace == "prometheus"
| where TimeGenerated > ago(24h)
| summarize VolumeInGB = (sum(_BilledSize) / (1024 * 1024 * 1024)) by Name
| order by VolumeInGB desc
| render barchart
```
Çıktı aşağıdakilere benzer sonuçlar gösterecektir:

![Veri alma biriminin günlük sorgu sonuçları](./media/container-insights-prometheus-integration/log-query-example-usage-03.png)

Çalışma alanında alınan veri hacminin yüksek olup olmadığını anlamak için GB'deki her ölçüm boyutunun bir ay süreyle ne olduğunu tahmin etmek için aşağıdaki sorgu sağlanır.

```
InsightsMetrics 
| where Namespace contains "prometheus"
| where TimeGenerated > ago(24h)
| summarize EstimatedGBPer30dayMonth = (sum(_BilledSize) / (1024 * 1024 * 1024)) * 30 by Name
| order by EstimatedGBPer30dayMonth desc
| render barchart
```

Çıktı aşağıdakilere benzer sonuçlar gösterecektir:

![Veri alma biriminin günlük sorgu sonuçları](./media/container-insights-prometheus-integration/log-query-example-usage-02.png)

Azure Monitör Günlükleri ile veri kullanımını izleme ve maliyeti analiz etme hakkında daha fazla bilgi Kullanımı [ve maliyetleri yönet'te](../platform/manage-cost-storage.md)mevcuttur.

## <a name="next-steps"></a>Sonraki adımlar

[Burada](container-insights-agent-config.md)konteyner iş yüklerinden stdout, stderr ve çevresel değişkenler için aracı toplama ayarlarını yapılandırma hakkında daha fazla bilgi edinin. 
