---
title: Kapsayıcılar için Azure Izleyici izleme maliyeti | Microsoft Docs
description: Bu makalede, müşterilerin kullanımını ve ilişkili maliyetlerini yönetmesine yardımcı olmak üzere kapsayıcılar için Azure Izleyici tarafından toplanan ölçüm verileri & ölçümleri için izleme maliyeti açıklanmaktadır.
ms.topic: conceptual
ms.date: 05/27/2020
ms.openlocfilehash: 44802f611919fbf88894576eecf581a668e05b5b
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84122468"
---
# <a name="understand-monitoring-costs-for-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici 'nin izleme maliyetlerini anlama

Bu makale, aşağıdakileri anlamanıza yardımcı olmak üzere kapsayıcılar için Azure Izleyici için fiyatlandırma kılavuzu sağlar:

* Bu öngörüyü etkinleştirmeden önce maliyetleri önceden tahmin etme

* Kapsayıcılar için Azure Izleyici bir veya daha fazla kapsayıcı için etkinleştirildikten sonra maliyetleri ölçme

* Verilerin toplanmasını denetleme ve maliyet indirimleri yapma

Azure Izleyici günlükleri, Kubernetes kümeniz tarafından oluşturulan verileri toplar, dizinler ve depolar. 

Azure Izleyici fiyatlandırma modeli, öncelikle Log Analytics çalışma alanınıza günde gigabayt cinsinden alınan veri miktarına bağlıdır. Bir Log Analytics çalışma alanının maliyeti yalnızca toplanan veri hacminin temeli değildir, seçilen plana de bağlıdır ve kümelerinizde oluşturulan verileri depolamayı ne kadar tercih edersiniz.

>[!NOTE]
>Tüm boyutlar ve fiyatlandırma yalnızca örnek tahmine yöneliktir. Azure Izleyici Log Analytics fiyatlandırma modelinizi ve Azure bölgenizi temel alan en son fiyatlandırma için lütfen Azure Izleyici [fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/) sayfasına bakın.

Aşağıda, bir Kubernetes kümesinden, maliyeti etkileyen kapsayıcılar için Azure Izleyici ve kullanımınıza göre özelleştirilebilecek veri türlerinin bir özeti verilmiştir:

- Bu, kümedeki her Kubernetes ad alanındaki izlenen her kapsayıcıdan stdout, stderr kapsayıcısı kaydeder

- Kümedeki tüm izlenen kapsayıcılardan kapsayıcı ortam değişkenleri

- Kümede izleme gerektirmeyen Kubernetes işleri/pods tamamlandı

- Prometheus ölçümlerinin etkin koruması

- AKS kümenizdeki Kubernetes ana düğüm günlüklerinin, *kuas-apiserver* ve *kuin-Controller-Manager*gibi ana bileşenler tarafından oluşturulan günlük verilerini analiz etmek için [tanılama günlüğü](../../aks/view-master-logs.md) .

## <a name="what-is-collected-from-kubernetes-clusters"></a>Kubernetes kümelerinden toplanan özellikler

Kapsayıcılar için Azure Izleyici, Log Analytics çalışma alanınızda günlük verileri olarak yazılmış, önceden tanımlanmış bir ölçüm kümesi ve toplanan envanter öğeleri içerir. Aşağıda listelenen tüm ölçümler, her dakikada varsayılan olarak toplanır.

### <a name="node-metrics-collected"></a>Toplanan düğüm ölçümleri

Aşağıdaki liste, toplanan düğüm başına 24 ölçüdür:

- Cpuusme Anoçekirdekleri
- cpuCapacityNanoCores
- Cpuallocatablenanoçekirdekler
- memoryRssBytes
- memoryWorkingSetBytes
- Memorycapacitybayt sayısı
- memoryAllocatableBytes
- restartTimeEpoch
- kullanılan (disk)
- ücretsiz (disk)
- used_percent (disk)
- io_time (diskio)
- yazma işlemleri (diskıo)
- okuma (diskio)
- write_bytes (diskio)
- write_time (diskio)
- iops_in_progress (diskio)
- read_bytes (diskio)
- read_time (diskio)
- err_in (net)
- err_out (net)
- bytes_recv (net)
- bytes_sent (net)
- Kubelet_docker_operations (kubelet)

### <a name="container-metrics"></a>Kapsayıcı ölçümleri

Aşağıdaki liste, her kapsayıcı için sekiz ölçütedir:

- Cpuusme Anoçekirdekleri
- cpuRequestNanoCores
- cpuLimitNanoCores
- memoryRssBytes
- memoryWorkingSetBytes
- memoryRequestBytes
- memoryLimitBytes
- restartTimeEpoch

### <a name="cluster-inventory"></a>Küme envanteri

Aşağıdaki liste, varsayılan olarak toplanan küme envanteri verileri:

- KubePodInventory – kapsayıcı başına dakika başına 1
- Kubenodeınventory – dakika başına düğüm başına 1
- KubeServices – dakikada her hizmet için 1
- Containerınventory – dakikada kapsayıcı başına 1

## <a name="estimating-costs-to-monitor-your-aks-cluster"></a>AKS kümenizi izlemeye yönelik maliyetleri tahmin etme

Aşağıdaki tahmin, bir Azure Kubernetes hizmeti (AKS) kümesini aşağıdaki boyutlandırma örneği ile temel alır. Ayrıca, tahmin yalnızca ölçümler ve toplanan envanter verileri için geçerlidir. Kapsayıcı günlükleri (stdout, stderr ve çevresel değişkenler) için, iş yükü tarafından oluşturulan günlük boyutlarına göre farklılık gösterir ve tahminimizin dışında tutulur.

Aşağıdaki şekilde yapılandırılmış bir AKS kümesinin izlenmesini etkinleştirdiyseniz,

- Üç düğüm
- Düğüm başına iki disk
- Düğüm başına bir ağ arabirimi
- 20 Pod (her Pod 'da bir kapsayıcı = toplam olarak 20 kapsayıcı)
- İki Kubernetes ad alanı
- Beş Kubernetes hizmeti (Kube-sistem pods, hizmet ve ad alanı içerir)
- Toplama sıklığı = 60 saniye (varsayılan)

Atanan Log Analytics çalışma alanında saat başına oluşturulan veri tablolarını ve hacmini görebilirsiniz. Bu tabloların her biri hakkında daha fazla bilgi için bkz. [kapsayıcı kayıtları](container-insights-log-search.md#container-records).

|Tablo | Tahmin boyutu (MB/saat) |
|------|---------------|
|Perf | 12,9 |
|Insightsölçümlerini | 11,3 |
|KubePodInventory | 1,5 |
|Kubenodeınventory | 0,75 |
|KubeServices | 0.13 |
|Containerınventory | 3,6 |
|KubeHealth | 0.1 |
|KubeMonAgentEvents |0,005 |

Toplam = 31 MB/saat = 23,1 GB/ay (bir ay = 31 gün)

Kullandıkça Öde modeli olan Log Analytics için varsayılan [fiyatlandırmayı](https://azure.microsoft.com/pricing/details/monitor/) kullanarak aylık Azure izleyici maliyetini tahmin edebilirsiniz. Kapasite rezervasyonu dahil ettikten sonra, seçilen ayırmaya bağlı olarak aylık fiyat daha yüksek olacaktır.

## <a name="controlling-ingestion-to-reduce-cost"></a>Maliyeti azaltmak için alımı denetleme

Kuruluşunuzun farklı iş biriminin Kubernetes altyapısını ve bir Log Analytics çalışma alanını paylaştığı bir senaryo düşünün. Her bir iş birimiyle bir Kubernetes ad alanıyla ayrılır. Son yayınlanan çalışma kitabını kullanarak her çalışma alanında ne kadar veri alınacağını görselleştirebilirsiniz. [Çalışma kitapları galerisinde](../platform/workbooks-overview.md#getting-started)bulunan **kapsayıcı öngörüleri kullanım** çalışma kitabı, belgelerimizde paylaşdığımız bir sorgu kitaplığını oluşturmaya gerek kalmadan verilerinizin kaynağını görselleştirmenize yardımcı olur. Bu çalışma kitabında, bu tür perspektiflerden faturalanabilir verileri şu şekilde görüntüleyebilmeniz gereken grafikler vardır:

- Çözüme göre GB cinsinden alınan toplam faturalanabilir veri

- Kapsayıcı günlükleri tarafından alınan faturalanabilir veriler (uygulama günlükleri)

- Faturalanabilir kapsayıcı verileri Kubernetes ad alanı başına alınır

- Faturalanabilir kapsayıcı, küme adına göre ayrılmış verileri günlüğe kaydeder

- Günlük kaynak girişi tarafından alınan faturalanabilir kapsayıcı günlük verileri

- Tanılama ana düğüm günlükleri tarafından alınan faturalandırılabilir Tanılama verileri

Analizinizi tamamladıktan sonra, hangi kaynak veya kaynakların en çok veri oluşturduğunu veya gereksinimlerinizi aşan verileri oluşturduğunu belirleme hakkında veri toplamayı yeniden yapılandırabilirsiniz. Stdout, stderr ve ortam değişkenlerinin toplanmasını yapılandırma hakkında ayrıntılar, [aracı veri koleksiyonu ayarlarını yapılandırma](container-insights-agent-config.md) makalesinde açıklanmıştır.

Aşağıda, denetim maliyetine yardımcı olmak üzere ConfigMap dosyasını değiştirerek kümenize uygulayabileceğiniz değişikliklere örnek verilmiştir.

1. ConfigMap dosyasında aşağıdakileri değiştirerek, kümedeki tüm ad alanlarında stdout günlüklerini devre dışı bırakın:

    ```
    [log_collection_settings]       
       [log_collection_settings.stdout]          
          enabled = false
    ```

2. Dosya geliştirme ad boşlukınızdan (örneğin, **dev-test**) stderr günlüklerini toplamayı devre dışı bırakın ve configmap dosyasında aşağıdaki gibi değiştirerek diğer ad alanlarından (örneğin, **Üretim** ve **varsayılan**) stderr günlüklerini toplamaya devam edin:

    >[!NOTE]
    >Kuto-System günlük koleksiyonu varsayılan olarak devre dışıdır. Varsayılan ayar tutulur, hariç tutma ad alanları listesine **geliştirme ve test** ad alanı eklemek stderr günlük koleksiyonuna uygulanır.

    ```
    [log_collection_settings.stderr]          
       enabled = true          
          exclude_namespaces = ["kube-system", "dev-test"]
    ```

3. ConfigMap dosyasında aşağıdakini değiştirerek ortam değişkeni koleksiyonunu küme genelinde devre dışı bırakın. Bu, her Kubernetes ad alanındaki tüm kapsayıcılar için geçerlidir. 

    ```
    [log_collection_settings.env_var]
        enabled = false
    ```

4. Tamamlanmış işleri temizlemek için ConfigMap dosyasında aşağıdakileri değiştirerek iş tanımında temizleme ilkesini belirtin:

    ```
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: pi-with-ttl
    spec:
      ttlSecondsAfterFinished: 100
    ```

Bu değişikliklerden bir veya daha fazlasını ConfigMaps 'larınıza uyguladıktan sonra, bu değişiklikleri kümenize uygulamak için [güncelleştirilmiş ConfigMap 'ı uygulama](container-insights-prometheus-integration.md#applying-updated-configmap) bölümüne bakın.

### <a name="prometheus-metrics-scraping"></a>Prometheus ölçümleri scraping

[Prometheus ölçüm koruması](container-insights-prometheus-integration.md)'nı kullanıyorsanız, kümeinizden topladığınız ölçüm sayısını sınırlandırmak için aşağıdakileri göz önünde bulundurun:

- Scraping sıklığının en iyi şekilde ayarlandığından emin olun (varsayılan değer 60 saniyedir). Sıklığı 15 saniyeye artırabilmenize karşın, korbir ölçüdeki ölçümlerin bu sıklıkta yayınlanarak emin olmanız gerekir. Aksi halde, veri alımı ve bekletme maliyetlerine ekleme ve daha az değere sahip olan aralıklarda Log Analytics çalışma alanınıza gönderilen çok sayıda yinelenen ölçüm olur. 

- Kapsayıcılar için Azure Izleyici, ölçü adına göre dışlama & ekleme listelerini destekler. Örneğin, kümenizde **kubedns** ölçümleri varsa, varsayılan olarak gönderideki 'i alan yüzlerce, ancak büyük olasılıkla yalnızca bir alt küme ile ilgileniyor olabilirsiniz. Verilerin ıskartaya çıkarılması için bir ölçüm listesi belirtdiğini veya veri alma hacminde tasarruf etmeniz dışında diğerlerini dışlamamayı onaylayın. Bu ölçümlerin çoğunu yalnızca Log Analytics faturanızda ek ücretler ekleyecek şekilde etkinleştirmek ve bu ölçümleri kullanmak kolaydır.

- Pod ek açıklamalarını kullanarak korlama yaparken, kullanmazsınız (örneğin, **dev-test** ad alanı), Pod ölçümlerinin srapı ölçümlerini dışarıda bırakmanız için ad alanına göre filtrelemenizi sağlayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Izleyici kapsayıcılar için toplanan verilerden en son kullanım desenlerine bağlı olarak maliyetlerin nasıl anlaşıldığını anlama hakkında daha fazla bilgi için bkz. [kullanımı yönetme ve maliyetleri tahmin](../platform/manage-cost-storage.md)etme.