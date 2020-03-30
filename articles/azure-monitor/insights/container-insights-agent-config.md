---
title: Kapsayıcılar aracısı veri toplama için Azure Monitörünü yapılandırın | Microsoft Dokümanlar
description: Bu makalede, stdout/stderr ve ortam değişkenleri günlük koleksiyonunu denetlemek için kapsayıcılar aracısı için Azure Monitörünü nasıl yapılandırabileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 28b93190298ae61732ff7d2e297899af4ba0e5f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75933017"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitor için aracı veri toplamayı yapılandırma

Kapsayıcılar için Azure Monitor, konteynerize aracıdan yönetilen Kubernetes kümelerine dağıtılan kapsayıcı iş yüklerinden stdout, stderr ve çevresel değişkenler toplar. Bu deneyimi denetlemek için özel bir Kubernetes ConfigMaps oluşturarak aracı veri toplama ayarlarını yapılandırabilirsiniz. 

Bu makalede, ConfigMap oluşturmak ve gereksinimlerinize göre veri toplama yapılandırmanasıl gösterin.

>[!NOTE]
>Azure Red Hat OpenShift için *openshift-azure oturum açma* alanında bir şablon ConfigMap dosyası oluşturulur. 
>

## <a name="configmap-file-settings-overview"></a>ConfigMap dosya ayarlarına genel bakış

Bir şablon ConfigMap dosyası kolayca sıfırdan oluşturmak zorunda kalmadan özelleştirmeler ile bunları yeniden dediğiniz için izin verilir. Başlamadan önce, ConfigMaps hakkındaki Kubernetes belgelerini gözden geçirmeli ve [ConfigMaps'i](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) nasıl oluşturabileceğiniz, yapılandırabileceğiniz ve dağıtabileceğiniz hakkında bilgi edinmelisiniz. Bu, kümedeki tüm bölmelerde/düğümlerde çalışan herhangi bir kapsayıcı için ad alanı başına veya tüm küme boyunca stderr ve stdout filtrelemenize olanak sağlar.

>[!IMPORTANT]
>Konteyner iş yüklerinden stdout, stderr ve çevresel değişkenleri toplamak için desteklenen minimum aracı sürümü ciprod06142019 veya daha sonraki dir. Aracı lı sürümünüzü doğrulamak için, **Düğüm** sekmesinden bir düğüm seçin ve **Aracı Görüntü Etiketi** özelliğinin özellik bölmesinde not değeri belirleyin. Aracı sürümleri ve her sürümde nelerin yer aldığı hakkında daha fazla bilgi için [aracı sürüm notlarına](https://github.com/microsoft/Docker-Provider/tree/ci_feature_prod)bakın.

### <a name="data-collection-settings"></a>Veri toplama ayarları

Aşağıda veri toplama yı denetlemek için yapılandırılabilen ayarlar verilmiştir.

|Anahtar |Veri türü |Değer |Açıklama |
|----|----------|------|------------|
|`schema-version` |String (büyük/küçük harf duyarlı) |v1 |Bu, bu ConfigMap ayrıştırMa aracısı tarafından kullanılan şema sürümüdür. Şu anda desteklenen şema-sürümü v1 olduğunu. Bu değerin değiştirilmesi desteklenmez ve ConfigMap değerlendirildiğinde reddedilir.|
|`config-version` |Dize | | Kaynak denetim sisteminizde/deposunda bu config dosyasının sürümünü izleme yeteneğini destekler. İzin verilen en büyük karakterler 10 karakterdir ve diğer tüm karakterler kesilir. |
|`[log_collection_settings.stdout] enabled =` |Boole | true veya false | Stdout kapsayıcı günlük koleksiyonu etkinse bu denetler. Stdout `true` log collection için ayarlandığında ve hiçbir`log_collection_settings.stdout.exclude_namespaces` ad alanı hariç tutulunca (aşağıdaki ayar) stdout günlükleri kümedeki tüm bölmeler/düğümler deki tüm kapsayıcılardan toplanır. ConfigMaps'te belirtilmemişse, `enabled = true`varsayılan değer . |
|`[log_collection_settings.stdout] exclude_namespaces =`|Dize | Virgülle ayrılmış dizi |Stdout günlüklerinin toplanmadığı Kubernetes ad alanları dizisi. Bu ayar yalnızca `log_collection_settings.stdout.enabled` ' olarak `true`ayarlanırsa etkilidir. ConfigMap'te belirtilmemişse, `exclude_namespaces = ["kube-system"]`varsayılan değer .|
|`[log_collection_settings.stderr] enabled =` |Boole | true veya false |Stderr kapsayıcı günlük koleksiyonu etkinse bu denetler. Stdout `true` log collection (ayar)`log_collection_settings.stderr.exclude_namespaces` için ayarlandığında ve hiçbir ad alanı hariç tutulunca, kümedeki tüm bölmeler/düğümler deki tüm kapsayıcılardan stderr günlükleri toplanır. ConfigMaps'te belirtilmemişse, `enabled = true`varsayılan değer . |
|`[log_collection_settings.stderr] exclude_namespaces =` |Dize |Virgülle ayrılmış dizi |Stderr günlüklerinin toplanmadığı Kubernetes ad alanları dizisi. Bu ayar yalnızca `log_collection_settings.stdout.enabled` ' olarak `true`ayarlanırsa etkilidir. ConfigMap'te belirtilmemişse, `exclude_namespaces = ["kube-system"]`varsayılan değer . |
| `[log_collection_settings.env_var] enabled =` |Boole | true veya false | Bu ayar, kümedeki tüm bölmeler/düğümler arasında ortam değişkeni koleksiyonunu denetler ve ConfigMaps'te `enabled = true` belirtilmediğinde varsayılan olarak. Ortam değişkenlerinin toplanması genel olarak etkinleştirilmişse, `AZMON_COLLECT_ENV` ortam değişkenini Dockerfile ayarı ile veya **env:** bölümü altındaki [Pod'un yapılandırma dosyasında](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/) **False** olarak ayarlayarak belirli bir kapsayıcı için devre dışı bırakabilirsiniz. Ortam değişkenlerinin toplanması genel olarak devre dışı bırakılırsa, belirli bir kapsayıcı için koleksiyonu etkinleştiremezsiniz (diğer bir şekilde, kapsayıcı düzeyinde uygulanabilecek tek geçersiz kılma, genel olarak zaten etkinleştirildiğinde koleksiyonu devre dışı etmektir.) |
| `[log_collection_settings.enrich_container_logs] enabled =` |Boole | true veya false | Bu ayar, kümedeki tüm kapsayıcı günlükleri için ContainerLog tablosuna yazılan her günlük kaydıiçin Ad ve Görüntü özellik değerlerini doldurmak için kapsayıcı günlük zenginleştirmesini denetler. ConfigMap'te `enabled = false` belirtilmediğinde varsayılan olarak bu değere iner. |

ConfigMaps küresel bir listedir ve aracıya yalnızca bir ConfigMap uygulanabilir. Koleksiyonları geçersiz kakan başka bir ConfigMaps olamaz.

## <a name="configure-and-deploy-configmaps"></a>ConfigMaps'i yapılandırma ve dağıtma

ConfigMap yapılandırma dosyanızı yapılandırmak ve kümenize dağıtmak için aşağıdaki adımları gerçekleştirin.

1. Şablon ConfigMap yaml dosyasını [indirin](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) ve konteyner-azm-ms-agentconfig.yaml olarak kaydedin. 

   >[!NOTE]
   >ConfigMap şablonu kümede zaten mevcut olduğundan, Azure Red Hat OpenShift ile çalışırken bu adım gerekli değildir.

2. Stdout, stderr ve/veya çevresel değişkenleri toplamak için özelleştirmelerinizle ConfigMap yaml dosyasını değiştirin. Azure Red Hat OpenShift için ConfigMap yaml dosyasını düzenliyorsanız, önce dosyayı metin düzenleyicisinde açmak için komutu `oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging` çalıştırın.

    - Stdout günlük koleksiyonu için belirli ad alanlarını hariç tutmak için, aşağıdaki `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`örneği kullanarak anahtarı/değeri yapılandırAbilirsiniz: .
    
    - Belirli bir kapsayıcı için ortam değişkeni koleksiyonunu devre `[log_collection_settings.env_var] enabled = true` dışı düşürmek için, değişken toplamayı genel olarak etkinleştirmek için anahtarı/değeri ayarlayın ve ardından belirli kapsayıcıiçin yapılandırmayı tamamlamak için [buradaki](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) adımları izleyin.
    
    - Stderr log collection küme genelinde devre dışı kalmak için, aşağıdaki örneği `[log_collection_settings.stderr] enabled = false`kullanarak anahtarı/değeri yapılandırAbilirsiniz: .

3. Azure Red Hat OpenShift dışındaki kümeler için aşağıdaki kubectl komutunu `kubectl apply -f <configmap_yaml_file.yaml>` çalıştırarak ConfigMap'i oluşturun: Azure Red Hat OpenShift dışındaki kümelerde. 
    
    Örnek: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 

    Azure Red Hat OpenShift için değişikliklerinizi editöre kaydedin.

Yapılandırma değişikliğinin etkinleşmeden çağrısının bitişi birkaç dakika sürebilir ve kümedeki bütün omsagent podları yeniden başlatırılabilir. Yeniden başlatma, tüm omsagent bölmeleri için bir yuvarlanma yeniden başlatma, aynı anda tüm yeniden başlatma. Yeniden başlatmalar tamamlandığında, aşağıdakilere benzer ve sonucu içeren bir ileti `configmap "container-azm-ms-agentconfig" created`görüntülenir: .

## <a name="verify-configuration"></a>Yapılandırmayı doğrulama

Yapılandırmanın Azure Red Hat OpenShift dışındaki bir kümeye başarıyla uygulandığını doğrulamak için, aracı bölmesinden günlükleri gözden geçirmek için aşağıdaki komutu kullanın: `kubectl logs omsagent-fdf58 -n=kube-system`. Omsagent bölmelerinden yapılandırma hataları varsa, çıktı aşağıdakilere benzer hatalar gösterir:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Yapılandırma değişikliklerinin uygulanmasıyla ilgili hatalar da gözden geçirilebilir. Yapılandırma değişikliklerinin ek sorun giderme sini gerçekleştirmek için aşağıdaki seçenekler kullanılabilir:

- Bir aracı pod aynı `kubectl logs` komutu kullanarak günlükleri gönderen. 

    >[!NOTE]
    >Bu komut Azure Red Hat OpenShift kümesi için geçerli değildir.
    > 

- Canlı günlüklerden. Canlı günlükler aşağıdakilere benzer hataları gösterir:

    ```
    config::error::Exception while parsing config map for log collection/env variable settings: \nparse error on value \"$\" ($end), using defaults, please check config map for errors
    ```

- Log Analytics çalışma alanınızdaki **KubeMonAgentEvents** tablosundan. Veriler yapılandırma hataları için *Hata* şiddeti ile her saat gönderilir. Hata yoksa, tablodaki *giriş,* hiçbir hata bildiren önem Bilgisi içeren verilere sahip olacaktır. **Etiketler** özelliği, hatanın oluştuğu bölme ve kapsayıcı kimliği ve ayrıca son bir saat içinde ilk oluşumu, son oluşumu ve sayısı hakkında daha fazla bilgi içerir.

- Azure Red Hat OpenShift ile, openshift-azure-log günlük koleksiyonunun etkin olup olmadığını doğrulamak için **ContainerLog** tablosunda arama yaparak omsagent günlüklerini kontrol edin.

Azure Red Hat OpenShift dışındaki kümelerde ConfigMap'teki hatayı düzelttikten sonra, yaml dosyasını kaydedin ve `kubectl apply -f <configmap_yaml_file.yaml`aşağıdaki komutu çalıştırarak güncelleştirilmiş ConfigMaps'i uygulayın: . Azure Red Hat OpenShift için, aşağıdaki komutu çalıştırarak güncelleştirilmiş ConfigMaps'i düzenleme ve kaydedin:

``` bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

## <a name="applying-updated-configmap"></a>Güncelleştirilmiş ConfigMap uygulama

Azure Red Hat OpenShift dışındaki kümelerde zaten bir ConfigMap dağıttıysanız ve daha yeni bir yapılandırmayla güncelleştirmek istiyorsanız, daha önce kullandığınız ConfigMap dosyasını güncelleyebilir `kubectl apply -f <configmap_yaml_file.yaml`ve daha önce olduğu gibi aynı komutu kullanarak uygulayabilirsiniz. Azure Red Hat OpenShift için, aşağıdaki komutu çalıştırarak güncelleştirilmiş ConfigMaps'i düzenleme ve kaydedin:

``` bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

Yapılandırma değişikliğinin etkinleşmeden çağrısının bitişi birkaç dakika sürebilir ve kümedeki bütün omsagent podları yeniden başlatırılabilir. Yeniden başlatma, tüm omsagent bölmeleri için bir yuvarlanma yeniden başlatma, aynı anda tüm yeniden başlatma. Yeniden başlatmalar tamamlandığında, aşağıdakilere benzer ve sonucu içeren bir ileti `configmap "container-azm-ms-agentconfig" updated`görüntülenir: .

## <a name="verifying-schema-version"></a>Şema sürümünü doğrulama

Desteklenen config şema sürümleri omsagent pod pod ek açıklama (şema-sürümleri) olarak kullanılabilir. Aşağıdaki kubectl komutu ile onları görebilirsiniz:`kubectl describe pod omsagent-fdf58 -n=kube-system`

Çıktı açıklama şema sürümleri ile aşağıdaki benzer gösterecektir:

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

## <a name="next-steps"></a>Sonraki adımlar

- Kapsayıcılar için Azure Monitörü önceden tanımlanmış bir uyarı kümesi içermez. DevOps veya operasyonel süreç ve yordamlarınızı desteklemek için yüksek CPU ve bellek kullanımı için önerilen uyarıları nasıl oluşturup nasıl oluşturup nasıl oluşturup oluşturabilirsiniz öğrenmek [için kapsayıcılar için Azure Monitor ile performans uyarıları oluştur'u](container-insights-alerts.md) gözden geçirin.

- AKS veya karma kümenizin sistem durumu ve kaynak kullanımını ve üzerlerinde çalışan iş yüklerini toplamak için etkinleştirilen izleme yle, kapsayıcılar için Azure [Monitörü'nü nasıl kullanacağınızı](container-insights-analyze.md) öğrenin.

- Kümelerinizi uyarmak, görselleştirmek veya çözümlemek için değerlendirmek veya özelleştirmek için önceden tanımlanmış sorguları ve örnekleri görmek için [günlük sorgu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin.
