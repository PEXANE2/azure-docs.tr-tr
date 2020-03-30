---
title: Operatör en iyi uygulamaları - Azure Kubernetes Hizmetleri'ndeki (AKS) temel zamanlayıcı özellikleri
description: Azure Kubernetes Hizmeti'nde (AKS) kaynak kotaları ve bölme kesintisi bütçeleri gibi temel zamanlayıcı özelliklerini kullanmak için küme operatörünün en iyi uygulamalarını öğrenin
services: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.openlocfilehash: cccc476a944b28d24c53a947e434d465c94f94ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79126588"
---
# <a name="best-practices-for-basic-scheduler-features-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) temel zamanlayıcı özellikleri için en iyi uygulamalar

Azure Kubernetes Hizmeti'nde (AKS) kümeleri yönetirken, genellikle ekipleri ve iş yüklerini yalıtmanız gerekir. Kubernetes zamanlayıcısı, bilgi işlem kaynaklarının dağıtımını denetlemenize veya bakım olaylarının etkisini sınırlamanıza izin veren özellikler sağlar.

Bu en iyi uygulamalar makalesi küme operatörleri için temel Kubernetes zamanlama özelliklerine odaklanır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Takımlara veya iş yüklerine sabit miktarda kaynak sağlamak için kaynak kotalarını kullanma
> * Bölme kesintisi bütçelerini kullanarak zamanlanmış bakımın etkisini sınırlama
> * Aracı kullanarak eksik bölme kaynak `kube-advisor` isteklerini ve sınırlarını denetleme

## <a name="enforce-resource-quotas"></a>Kaynak kotalarını zorlama

**En iyi uygulama kılavuzu** - Ad alanı düzeyinde kaynak kotaları planlayın ve uygulayın. Bölmeler kaynak isteklerini ve sınırlarını tanımlayamazsa, dağıtımı reddedin. Kaynak kullanımını izleyin ve kotaları gerektiği gibi ayarlayın.

Kaynak istekleri ve limitleri pod belirtimine yerleştirilir. Bu sınırlar, kümede kullanılabilir bir düğüm bulmak için dağıtım zamanında Kubernetes zamanlayıcısı tarafından kullanılır. Bu sınırlar ve istekler tek tek pod düzeyinde çalışır. Bu değerlerin nasıl tanımlanacak hakkında daha fazla bilgi için [bkz.][resource-limits]

Bir geliştirme ekibi veya proje genelinde kaynakları ayırmanın ve sınırlamanın bir yolunu sağlamak için *kaynak kotalarını*kullanmanız gerekir. Bu kotalar bir ad alanında tanımlanır ve kotaları aşağıdaki temellere göre ayarlamak için kullanılabilir:

* CPU ve bellek veya GPU gibi **hesaplama kaynakları.**
* **Depolama kaynakları,** belirli bir depolama sınıfı için toplam birim sayısını veya disk alanı miktarını içerir.
* **Nesne sayısı**, en fazla sayıda sırlar, hizmetler veya iş sayısı gibi oluşturulabilir.

Kubernetes kaynakları fazla işlemez. Kaynak isteklerinin veya sınırların toplam toplamı atanan kotayı geçtikten sonra, başka dağıtım başarılı olmaz.

Kaynak kotaları tanımladığınızda, ad alanında oluşturulan tüm bölmeler, bölme belirtimlerinde sınırlar veya istekler sağlamalıdır. Bu değerleri sağlamazlarsa, dağıtımı reddedebilirsiniz. Bunun yerine, [bir ad alanı için varsayılan istekleri ve sınırları yapılandırabilirsiniz.][configure-default-quotas]

Aşağıdaki örnek YAML *manifestosunda dev-app-team-quotas.yaml* adlı *toplam 10* CPU, *20Gi* bellek ve *10* bölme sabit bir sınır belirler:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-app-team
spec:
  hard:
    cpu: "10"
    memory: 20Gi
    pods: "10"
```

Bu kaynak *kotası, dev uygulamalar*gibi ad alanı belirtilerek uygulanabilir:

```console
kubectl apply -f dev-app-team-quotas.yaml --namespace dev-apps
```

İhtiyaçlarını anlamak ve uygun kaynak kotalarını uygulamak için uygulama geliştiricilerinizle ve sahipleriyle birlikte çalışın.

Kullanılabilir kaynak nesneleri, kapsamlar ve öncelikler hakkında daha fazla bilgi için [Kubernetes'teki Kaynak kotalarına][k8s-resource-quotas]bakın.

## <a name="plan-for-availability-using-pod-disruption-budgets"></a>Bölme kesintisi bütçelerini kullanarak kullanılabilirlik planı

**En iyi uygulama kılavuzu** - Uygulamaların kullanılabilirliğini korumak için, kümede en az sayıda bakla bulunduğundan emin olmak için Pod Kesinti Bütçeleri (PDBs) tanımlayın.

Bölmelerin kaldırılmasına neden olan iki yıkıcı olay vardır:

* *İstemsiz kesintiler* küme işlecinin veya uygulama sahibinin tipik denetiminin ötesindeki olaylardır.
  * Bu istemsiz kesintiler fiziksel makinede bir donanım hatası, çekirdek paniği veya bir düğüm VM'nin silinmesini içerir
* *Gönüllü kesintiler* küme işleci veya uygulama sahibi tarafından istenen olaylardır.
  * Bu gönüllü kesintiler küme yükseltmeleri, güncelleştirilmiş dağıtım şablonu veya yanlışlıkla bir bölme silme içerir.

İstemeden oluşan aksaklıklar, dağıtımda bölmelerinizin birden çok kopyasını kullanarak azaltılabilir. AKS kümesinde birden çok düğüm çalıştırmak da bu istemsiz kesintiler ile yardımcı olur. Gönüllü kesintiler için Kubernetes, küme işlecinin kullanılabilir en az veya en fazla kullanılamayan kaynak sayımı tanımlamasına izin veren *bölme kesintisi bütçeleri* sağlar. Bu bölme kesintisi bütçeleri, gönüllü bir kesinti olayı meydana geldiğinde dağıtımların veya yineleme kümelerinin nasıl yanıt veriş olduğunu planlamanıza sağlar.

Bir küme yükseltilecekse veya dağıtım şablonu güncellenecekse, Kubernetes zamanlayıcısı, gönüllü kesinti olayları devam etmeden önce ek bölmelerin diğer düğümlerde zamanlanmasından emin olur. Zamanlayıcı, kümedeki diğer düğümlerde tanımlı bölme sayısı başarıyla zamanlanana kadar bir düğüm yeniden başlatılmadan önce bekler.

NGINX çalıştıran beş podlu bir çoğaltma kümesi örneğine bakalım. Yineleme kümesindeki bölmelere etiket `app: nginx-frontend`atanır. Küme yükseltmesi gibi isteğe bağlı bir kesinti olayı sırasında, en az üç bölmenin çalışmaya devam etmesini istersiniz. *PodDisruptionBudget* nesnesi için aşağıdaki YAML bildirimi şu gereksinimleri tanımlar:

```yaml
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
   name: nginx-pdb
spec:
   minAvailable: 3
   selector:
   matchLabels:
      app: nginx-frontend
```

Ayrıca, bölme sayısını ölçeklendirerek çoğaltma kümesini otomatik olarak telafi etmenizi sağlayan *%60*gibi bir yüzde tanımlayabilirsiniz.

Yineleme kümesinde en fazla kullanılabilir olmayan örnek sayısını tanımlayabilirsiniz. Yine, en fazla kullanılamayan bölmeler için bir yüzde de tanımlanabilir. Aşağıdaki bölme kesintisi bütçesi YAML bildirimi, yineleme kümesindeki en fazla iki bölmenin kullanılamayan olduğunu tanımlar:

```yaml
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
   name: nginx-pdb
spec:
   maxUnavailable: 2
   selector:
   matchLabels:
      app: nginx-frontend
```

Pod kesintisi bütçeniz tanımlandıktan sonra, diğer Kubernetes nesnesinde olduğu gibi AKS kümenizde de oluşturursunuz:

```console
kubectl apply -f nginx-pdb.yaml
```

İhtiyaçlarını anlamak ve uygun bölme kesintisi bütçelerini uygulamak için uygulama geliştiricilerinizle ve sahipleriyle birlikte çalışın.

Bölme kesintisi bütçelerini kullanma hakkında daha fazla bilgi için [bkz.][k8s-pdbs]

## <a name="regularly-check-for-cluster-issues-with-kube-advisor"></a>Kube-advisor ile küme sorunlarını düzenli olarak denetleyin

**En iyi uygulama kılavuzu** - Kümenizdeki sorunları algılamak için açık kaynak aracının `kube-advisor` en son sürümünü düzenli olarak çalıştırın. Varolan bir AKS kümesine kaynak kotaları uygularsanız, kaynak istekleri ve sınırları tanımlı olmayan bölmeleri bulmak için önce çalıştırın. `kube-advisor`

[Kube-danışman][kube-advisor] aracı, bir Kubernetes kümesini tarayan ve bulduğu sorunları raporlayan ilişkili bir AKS açık kaynak projesidir. Yararlı denetimlerden biri, kaynak istekleri ve sınırları olmayan bölmeleri tanımlamaktır.

Kube-advisor aracı kaynak isteği ve Windows uygulamaları için PodSpecs yanı sıra Linux uygulamaları eksik sınırları rapor edebilirsiniz, ancak kube-danışman aracı kendisi bir Linux pod üzerinde zamanlanmış olmalıdır. Pod yapılandırmasında bir [düğüm seçici][k8s-node-selector] kullanarak belirli bir işletim sistemi ile bir düğüm havuzunda çalışacak bir bölme zamanlayabilirsiniz.

Birden çok geliştirme ekibi ve uygulaması barındıran bir AKS kümesinde, bu kaynak istekleri ve sınırları ayarlı olmadan bölmeleri izlemek zor olabilir. En iyi yöntem olarak, `kube-advisor` özellikle ad alanlarına kaynak kotaları atamıyorsanız, AKS kümelerinizde düzenli olarak çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede temel Kubernetes zamanlayıcı özellikleri üzerinde duruldu. AKS'deki küme işlemleri hakkında daha fazla bilgi için aşağıdaki en iyi uygulamalara bakın:

* [Çok kiracılılık ve küme yalıtımı][aks-best-practices-cluster-isolation]
* [Gelişmiş Kubernetes zamanlayıcı özellikleri][aks-best-practices-advanced-scheduler]
* [Kimlik doğrulama ve yetkilendirme][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->
[k8s-resource-quotas]: https://kubernetes.io/docs/concepts/policy/resource-quotas/
[configure-default-quotas]: https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/
[kube-advisor]: https://github.com/Azure/kube-advisor
[k8s-pdbs]: https://kubernetes.io/docs/tasks/run-application/configure-pdb/

<!-- INTERNAL LINKS -->
[resource-limits]: developer-best-practices-resource-management.md#define-pod-resource-requests-and-limits
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-identity]: operator-best-practices-identity.md
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors
