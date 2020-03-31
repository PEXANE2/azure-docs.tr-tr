---
title: Operatör en iyi uygulamaları - Azure Kubernetes Hizmetleri'nde (AKS) gelişmiş zamanlayıcı özellikleri
description: Azure Kubernetes Hizmetinde (AKS) küme operatörünün bozuk para ve tolere, düğüm seçiciler ve yakınlık veya bölmearası yakınlık ve afinite gibi gelişmiş zamanlayıcı özelliklerini kullanmak için en iyi uygulamaları öğrenin
services: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.openlocfilehash: 546c1d6ae25a33c6df93469ccf8c230b4b1c474b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252902"
---
# <a name="best-practices-for-advanced-scheduler-features-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) gelişmiş zamanlayıcı özellikleri için en iyi uygulamalar

Azure Kubernetes Hizmeti'nde (AKS) kümeleri yönetirken, genellikle ekipleri ve iş yüklerini yalıtmanız gerekir. Kubernetes zamanlayıcısı, belirli düğümlerde hangi bölmelerin zamanlanabileceğini veya çok bölmeli uygulamaların küme ye nasıl uygun şekilde dağıtılabildiğini denetlemenize izin veren gelişmiş özellikler sağlar. 

Bu en iyi uygulamalar makalesi küme operatörleri için gelişmiş Kubernetes zamanlama özelliklerine odaklanır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Bölmelerde zamanlanabilecek bölmeleri sınırlamak için bozuk para ve tolere kullanma
> * Düğüm seçicileri veya düğüm afinitesiyle belirli düğümlerde çalıştırmak için bölmelere tercih verme
> * Bölme veya bölmeleri birbirine kenetüre veya anti-afinite ile gruplandırma

## <a name="provide-dedicated-nodes-using-taints-and-tolerations"></a>Leke ve tolerasyonlar kullanarak özel düğümler sağlayın

**En iyi uygulama kılavuzu** - Giriş denetleyicileri gibi kaynak yoğun uygulamalar için erişimi belirli düğümlerle sınırlandırın. Düğüm kaynaklarını bunları gerektiren iş yükleri için kullanılabilir tutun ve düğümler üzerindeki diğer iş yüklerinin zamanlanmasına izin vermez.

AKS kümenizi oluşturduğunuzda, GPU desteği veya çok sayıda güçlü CPU'su yla düğümleri dağıtabilirsiniz. Bu düğümler genellikle makine öğrenimi (ML) veya yapay zeka (AI) gibi büyük veri işleme iş yükleri için kullanılır. Bu donanım türü genellikle dağıtılacak pahalı bir düğüm kaynağı olduğundan, bu düğümlerde zamanlanabilecek iş yüklerini sınırlayın. Bunun yerine, giriş hizmetlerini çalıştırmak ve diğer iş yüklerini önlemek için kümedeki bazı düğümleri ayırmak isteyebilirsiniz.

Farklı düğümler için bu destek birden çok düğüm havuzları kullanılarak sağlanır. AKS kümesi bir veya daha fazla düğüm havuzu sağlar.

Kubernetes zamanlayıcısı, düğümlerde hangi iş yüklerinin çalıştırılabildiği kısıtlamak için bozuk para ve tolere kullanabilir.

* Yalnızca belirli bölmelerin zamanlanabileceğini belirten bir düğüme **bir leke** uygulanır.
* Bir **tolerans** daha sonra onları bir düğüm leke *tolere* sağlayan bir bölme uygulanır.

Bir AKS kümesine bir bölme dağıttığınızda, Kubernetes yalnızca bir tolerasyonun lekeyle hizalandığı düğümlerde bölmeleri zamanlar. Örnek olarak, AKS kümenizde GPU destekli düğümler için bir düğüm havuzu nuz olduğunu varsayalım. *GPU*gibi adı, ardından zamanlama için bir değer olarak tanımlarsınız. Bu değeri *NoSchedule*olarak ayarlarsanız, bölme uygun tolere tanımlamazsa, Kubernetes zamanlayıcısı düğümde bölmeleri zamanlayamaz.

```console
kubectl taint node aks-nodepool1 sku=gpu:NoSchedule
```

Düğümlere uygulanan bir lekeyle, bölme belirtiminde düğümlerin zamanlamasına izin veren bir tolerasyon tanımlarsınız. Aşağıdaki örnek, önceki `sku: gpu` `effect: NoSchedule` adımda düğüme uygulanan lekeyi tanımlar:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Bu bölme, örneğin kullanma `kubectl apply -f gpu-toleration.yaml`gibi dağıtıldığında, Kubernetes uygulanan leke ile düğümler üzerinde pod başarıyla zamanlayabilirsiniz. Bu mantıksal yalıtım, küme içindeki kaynaklara erişimi denetlemenize olanak tanır.

Bozuk para ları uyguladığınızda, uygulama geliştiricilerinizle ve sahipleriyle birlikte çalışarak dağıtımlarında gerekli tolere olan ları tanımlamalarına izin verin.

Taintler ve tolerations hakkında daha fazla bilgi için, [kusurlu ve tolere uygulayarak][k8s-taints-tolerations]bakın.

AKS'de birden çok düğüm havuzunun nasıl kullanılacağı hakkında daha fazla bilgi için, [AKS'deki bir küme için birden çok düğüm havuzu oluştur ve yönet'][use-multiple-node-pools]e bakın.

### <a name="behavior-of-taints-and-tolerations-in-aks"></a>AKS'de leke ve tolere davranış

AKS'de bir düğüm havuzu yükselttidiğinizde, yeni düğümlere uygulandığında bir düğüm deseni ve tolerasyonları bir dizi izleyin:

- **Sanal makine ölçek kümeleri kullanan varsayılan kümeler**
  - İki düğümlü kümeniz olduğunu varsayalım - *düğüm1* ve *düğüm2.* Düğüm havuzunu yükseltin.
  - İki ek düğüm oluşturulur, *düğüm3* ve *düğüm4*, ve taintler sırasıyla geçirilir.
  - Özgün *düğüm1* ve *node2* silinir.

- **Sanal makine ölçeği olmayan kümeler destek kümesi**
  - Yine, iki düğümlü kümeniz olduğunu varsayalım - *düğüm1* ve *düğüm2.* Yükseltme yaptığınızda, ek bir düğüm *(düğüm3)* oluşturulur.
  - *Düğüm1'deki* tabaplar *düğüm3'e*uygulanır, sonra *düğüm1* silinir.
  - Başka bir yeni düğüm oluşturulur *(addüğüm1*, önceki *düğüm1* silindiğinden) ve *düğüm2* taintler yeni *düğüm1*uygulanır. Ardından, *düğüm2* silinir.
  - Özünde *düğüm1* *düğüm 3*olur ve *düğüm2* *düğüm olur1.*

AKS'de bir düğüm havuzuölçeklendirildiğinde, lekeler ve tolerasyonlar tasarım gereği taşımaz.

## <a name="control-pod-scheduling-using-node-selectors-and-affinity"></a>Düğüm seçicileri ve yakınlığı kullanarak bölme planlamasını kontrol etme

**En iyi uygulama kılavuzu** - Düğüm seçicileri, düğüm afinitesini veya bölmeler arası yakınlığı kullanarak düğümler üzerindeki bölmelerin zamanlamasını denetleyin. Bu ayarlar, Kubernetes zamanlayıcısının düğümdeki donanım gibi iş yüklerini mantıksal olarak yalıtmasını sağlar.

Taints ve tolerations mantıksal olarak sert bir kesme ile kaynakları izole etmek için kullanılır - bölme bir düğüm lekesi tolere değilse, düğüm üzerinde zamanlanmaz. Alternatif bir yaklaşım düğüm seçicileri kullanmaktır. Yerel olarak bağlı SSD depolama veya büyük miktarda bellek belirtmek için düğümleri etiketler ve sonra pod belirtimibir düğüm seçici tanımlayın. Kubernetes daha sonra bu bölmeleri eşleşen bir düğüm üzerinde zamanlar. Tolerasyonların aksine, eşleşen bir düğüm seçiciolmayan bölmeler etiketli düğümlerde zamanlanabilir. Bu davranış, düğümler üzerinde kullanılmayan kaynakların tüketilmesine izin verir, ancak eşleşen düğüm seçicitanımlayan bölmelere öncelik verir.

Yüksek miktarda belleğe sahip düğümlere bir örnek bakalım. Bu düğümler, yüksek miktarda bellek isteyen bölmelere tercih verebilir. Kaynakların boşta kalmadığından emin olmak için, diğer bölmelerin çalışmasına da izin verirler.

```console
kubectl label node aks-nodepool1 hardware:highmem
```

Bir bakla belirtimi daha sonra bir düğüm üzerinde etiket kümesi eşleşen bir düğüm seçici tanımlamak için `nodeSelector` özellik ekler:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
    nodeSelector:
      hardware: highmem
```

Bu zamanlayıcı seçeneklerini kullandığınızda, pod belirtimlerini doğru bir şekilde tanımlamalarına izin vermek için uygulama geliştiricilerinizle ve sahipleriyle birlikte çalışın.

Düğüm seçicileri kullanma hakkında daha fazla bilgi için [bkz.][k8s-node-selector]

### <a name="node-affinity"></a>Düğüm yakınlığı

Düğüm seçici, bölmeleri belirli bir düğüme atamanın temel yoludur. *Düğüm afiyeti*kullanılarak daha fazla esneklik mevcuttur. Düğüm yakınlığı yla, bölme bir düğümle eşleşemezse ne olacağını tanımlarsınız. Kubernetes zamanlayıcısının bir bölmeyle etiketli bir ana bilgisayarla eşleşmesini *gerektirebilirsiniz.* Veya, bir eşleşme *tercih* edebilirsiniz, ancak eşleşmiyorsa bölmenin farklı bir ana bilgisayarda zamanlanmasına izin verebilirsiniz.

Aşağıdaki örnek, düğüm afiyetini *gerekli olanDuringSchedulingIgnoredDuringExecution'a*ayarlar. Bu yakınlık, Kubernetes zamanlamasının eşleşen bir etikete sahip bir düğüm kullanmasını gerektirir. Düğüm yoksa, bölmezamanlamanın devam etmesini beklemek zorundadır. Bölmenin farklı bir düğümde zamanlanmasına izin vermek için, bunun yerine *tercih edilenDuringScheduledIgnoreDuringExecution*değerini ayarlayabilirsiniz:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: hardware
            operator: In
            values: highmem
```

*Ayarı'nın Göz ardı Edilen Yürütme* bölümü, düğüm etiketleri değişirse bölmenin düğümden çıkmaması gerektiğini gösterir. Kubernetes zamanlayıcısı, düğümlerde zaten zamanlanmış bölmeler değil, yalnızca zamanlanan yeni bölmeler için güncelleştirilmiş düğüm etiketlerini kullanır.

Daha fazla bilgi için [Bkz. Affinity ve anti-yakınlık.][k8s-affinity]

### <a name="inter-pod-affinity-and-anti-affinity"></a>Kapsüller arası yakınlık ve anti-afinite

Kubernetes zamanlayıcısının iş yüklerini mantıksal olarak yalıtmak için son bir yaklaşımı, bölmearası yakınlık veya anti-yakınlık kullanmaktır. Ayarlar, bölmelerin varolan eşleşen bir bölmeye sahip bir düğümde *zamanlanmaması* gerektiğini veya zamanlanması *gerektiğini* tanımlar. Varsayılan olarak, Kubernetes zamanlayıcısı düğümler arasında bir yineleme kümesinde birden çok bölme zamanlamak için çalışır. Bu davranış etrafında daha özel kurallar tanımlayabilirsiniz.

Buna iyi bir örnek, Redis için azure önbelleği de kullanan bir web uygulamasıdır. Kubernetes zamanlayıcısının yinelemeleri düğümler arasında dağıtmasını istemek için bölme afinite karşıtı kuralları kullanabilirsiniz. Daha sonra, her web uygulaması bileşeninin ilgili önbellekle aynı ana bilgisayarda zamanlandığından emin olmak için yakınlık kurallarını kullanabilirsiniz. Baklaların düğümler arasında dağılımı aşağıdaki örnek gibi görünür:

| **Düğüm 1** | **Düğüm 2** | **Düğüm 3** |
|------------|------------|------------|
| webapp-1   | webapp-2   | webapp-3   |
| önbellek-1    | önbellek-2    | önbellek-3    |

Bu örnek, düğüm seçicileri veya düğüm afinitekullanımıdaha karmaşık bir dağıtımdır. Dağıtım, Kubernetes'in düğümlerde bölmeleri nasıl zamanlattığı ve kaynakları mantıksal olarak nasıl yalıtabileceği üzerinde denetim sağlar. Redis örneğinde Azure Önbellek'li bu web uygulamasının tam bir örneği için, [aynı düğümdeki Ortak bul bölmelerine][k8s-pod-affinity]bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, gelişmiş Kubernetes zamanlayıcı özellikleri üzerinde duruldu. AKS'deki küme işlemleri hakkında daha fazla bilgi için aşağıdaki en iyi uygulamalara bakın:

* [Çok kiracılılık ve küme yalıtımı][aks-best-practices-scheduler]
* [Temel Kubernetes zamanlayıcı özellikleri][aks-best-practices-scheduler]
* [Kimlik doğrulama ve yetkilendirme][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->
[k8s-taints-tolerations]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[k8s-node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[k8s-affinity]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity
[k8s-pod-affinity]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#always-co-located-in-the-same-node

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
[aks-best-practices-identity]: operator-best-practices-identity.md
[use-multiple-node-pools]: use-multiple-node-pools.md
