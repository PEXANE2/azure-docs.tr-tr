---
title: Operatör en iyi uygulamaları-Azure Kubernetes hizmetlerindeki temel Zamanlayıcı özellikleri (AKS)
description: Azure Kubernetes Service (AKS) içindeki kaynak kotaları ve pod kesinti bütçeleri gibi temel Zamanlayıcı özelliklerini kullanmaya yönelik küme işletmeni en iyi yöntemlerini öğrenin
services: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.openlocfilehash: cccc476a944b28d24c53a947e434d465c94f94ee
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79126588"
---
# <a name="best-practices-for-basic-scheduler-features-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içindeki temel Zamanlayıcı özellikleri için en iyi yöntemler

Azure Kubernetes Service (AKS) içindeki kümeleri yönetirken, genellikle takımları ve iş yüklerini yalıtmanız gerekir. Kubernetes Zamanlayıcı, işlem kaynaklarının dağıtımını denetlemenize veya bakım olaylarının etkisini sınırlamanıza olanak sağlayan özellikler sağlar.

Bu en iyi yöntemler makalesi, küme işleçleri için temel Kubernetes zamanlama özelliklerine odaklanır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Takımlara veya iş yüklerine sabit miktarda kaynak sağlamak için kaynak kotalarını kullanın
> * Pod kesinti bütçelerini kullanarak zamanlanan bakımın etkisini sınırlayın
> * `kube-advisor` Aracı kullanarak eksik Pod kaynak isteklerini ve sınırlarını denetleyin

## <a name="enforce-resource-quotas"></a>Kaynak kotalarını zorla

**En iyi Yöntem Kılavuzu** -ad alanı düzeyinde kaynak kotalarını planlayın ve uygulayın. Pod kaynak isteklerini ve sınırlarını tanımlamaz, dağıtımı reddedin. Kaynak kullanımını izleyin ve kotaları gerektiği şekilde ayarlayın.

Kaynak istekleri ve limitleri Pod belirtimine yerleştirilir. Bu sınırlar, küme içinde kullanılabilir bir düğüm bulmak için dağıtım zamanında Kubernetes Zamanlayıcı tarafından kullanılır. Bu sınırlar ve istekler bireysel Pod düzeyinde çalışır. Bu değerlerin nasıl tanımlanacağı hakkında daha fazla bilgi için bkz. [Pod kaynak isteklerini ve sınırlarını tanımlama][resource-limits]

Bir geliştirme takımı veya projesindeki kaynakları ayırabilmeniz ve sınırlandırmaya yönelik bir yol sağlamak için *kaynak kotalarını*kullanmanız gerekir. Bu kotalar bir ad alanında tanımlanmıştır ve kotaları aşağıdaki şekilde ayarlamak için kullanılabilir:

* CPU ve bellek veya GPU 'Lar gibi **işlem kaynakları**.
* **Depolama kaynakları**, belirli bir depolama sınıfı için toplam birim sayısını veya disk alanı miktarını içerir.
* En fazla sayıda gizli dizi, hizmet veya iş için **nesne sayısı**.

Kubernetes kaynakları fazla yürütmez. Kaynak isteklerinin veya limitlerin birikimli toplamı atanan kotayı geçtiğinde, başka dağıtımlar başarılı olmaz.

Kaynak kotalarını tanımlarken, ad alanında oluşturulan tüm yığınların Pod belirtimlerinde sınır veya istek sağlaması gerekir. Bu değerleri sağlamıyorsa dağıtımı reddedebilirsiniz. Bunun yerine, [bir ad alanı için varsayılan istekleri ve sınırları yapılandırabilirsiniz][configure-default-quotas].

*Dev-App-Team-Quotas. YAML* adlı aşağıdaki örnek YAML bildirimi toplam *10* CPU, *20gi* bellek ve *10* kat için sabit bir sınır belirler:

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

Bu kaynak kotası, *geliştirme uygulamaları*gibi ad alanı belirtilerek uygulanabilir:

```console
kubectl apply -f dev-app-team-quotas.yaml --namespace dev-apps
```

Uygulama geliştiricileriniz ve sahipleriyle çalışarak, ihtiyaçlarını anlayın ve uygun kaynak kotalarını uygular.

Kullanılabilir kaynak nesneleri, kapsamlar ve öncelikler hakkında daha fazla bilgi için bkz. [Kubernetes 'Te kaynak kotaları][k8s-resource-quotas].

## <a name="plan-for-availability-using-pod-disruption-budgets"></a>Pod kesintisi bütçeleri kullanarak kullanılabilirlik planlaması

**En iyi Yöntem Kılavuzu** -uygulama kullanılabilirliğini sürdürmek için, küme içinde en az sayıda parça kullanılabilir olduğundan emin olmak için pod kesinti bütçeleri (pdb 'leri) tanımlayın.

Pod 'nin kaldırılmasına neden olan iki kesintiye neden olan olay vardır:

* *Involuntary kesintiler* , küme işlecinin veya uygulama sahibinin tipik denetiminden daha fazla olaylardır.
  * Bu involuntary kesintiler, fiziksel makinede bir donanım hatası, bir çekirdek Panic veya bir düğüm VM 'si silme işlemini içerir
* *Gönüllü kesintiler* , küme operatörü veya uygulama sahibi tarafından istenen olaylardır.
  * Bu gönüllü kesintiler, küme yükseltmeleri, güncelleştirilmiş bir dağıtım şablonu veya yanlışlıkla Pod silme işlemlerini içerir.

İnvoluntary kesintiler, bir dağıtımda birden fazla çoğaltma kullanılarak azaltılan bir şekilde azaltılabilir. AKS kümesinde birden çok düğüm çalıştırmak bu involuntary kesintiler ile de yardımcı olur. İsteyerek kesintiler için Kubernetes, küme işlecinin kullanılabilir en düşük veya en fazla kullanılamayan kaynak sayısını tanımlamasına izin veren *Pod kesinti bütçeleri* sağlar. Bu Pod kesinti bütçeleri, gönüllü bir kesinti olayı gerçekleştiğinde dağıtımların veya çoğaltma kümelerinin nasıl yanıt vereceğini planlamanız için plan yapmanızı sağlar.

Bir küme yükseltilmek veya bir dağıtım şablonu güncelleştiriliyorsa, Kubernetes Zamanlayıcı, isteğe bağlı kesinti olaylarının devam edebilmesi için diğer düğümlerde ek yığınların zamanlanmasını sağlar. Zamanlayıcı, tanımlanan düğüm sayısı kümedeki diğer düğümlerde başarıyla zamanlanana kadar bir düğüm yeniden başlatılmadan önce bekler.

NGıNX çalıştıran beş Pod içeren bir çoğaltma kümesi örneğine göz atalım. Çoğaltma kümesindeki Dizin, etikete `app: nginx-frontend`atanır. Küme yükseltmesi gibi gönüllü bir kesinti olayı sırasında, en az üç Pod çalışmaya devam etmesini sağlamak istersiniz. *Pod kesintiler* için aşağıdaki YAML bildirimi, bu gereksinimleri tanımlar:

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

Ayrıca, küme sayısının ölçeğini ölçeklendirerek çoğaltma kümesini otomatik olarak telafi etmenizi sağlayan *%60*gibi bir yüzde de tanımlayabilirsiniz.

Bir çoğaltma kümesinde kullanılamayan en fazla örnek sayısını tanımlayabilirsiniz. Ayrıca, kullanılamayan en fazla sayıda pod için bir yüzde de tanımlanabilir. Aşağıdaki Pod kesintisi bütçesi YAML bildirimi, çoğaltma kümesinde ikiden fazla sayıda Pod olmadığını tanımlar:

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

Pod kesinti bütçeniz tanımlandıktan sonra, AKS kümenizde diğer Kubernetes nesnesiyle birlikte oluşturursunuz:

```console
kubectl apply -f nginx-pdb.yaml
```

Uygulama geliştiricileriniz ve sahipleriyle birlikte çalışarak ihtiyaçlarını anlayın ve uygun Pod kesintisi bütçelerini uygulayın.

Pod kesinti bütçelerini kullanma hakkında daha fazla bilgi için bkz. [uygulamanız için bir kesinti bütçesi belirtme][k8s-pdbs].

## <a name="regularly-check-for-cluster-issues-with-kube-advisor"></a>Kuin-Advisor ile küme sorunlarını düzenli olarak denetleme

**En iyi Yöntem Kılavuzu** -kümenizdeki sorunları algılamak için `kube-advisor` açık kaynak aracının en son sürümünü düzenli olarak çalıştırın. Mevcut bir aks kümesinde kaynak kotaları uygularsanız, kaynak istekleri ve sınırları `kube-advisor` tanımlı olmayan bir pod bulmak için ilk olarak ' i çalıştırın.

[Kuin-Advisor][kube-advisor] Aracı, bir Kubernetes kümesini tarayan ve bulduğu sorunlar hakkında rapor veren ilişkili bir aks açık kaynak projesidir. Tek bir faydalı denetim, kaynak istekleri ve sınırları olmayan Pod 'yi belirlemektir.

Kumak-Advisor Aracı, Windows Uygulamaları ve Linux uygulamaları için pod özelliklerinin yanı sıra kaynak isteği ve limitleri rapor edebilir, ancak Kuto-Advisor aracının kendisi bir Linux pod üzerinde zamanlanmalıdır. Pod 'un yapılandırmasındaki [düğüm seçicisini][k8s-node-selector] kullanarak belirli bir işletim sistemine sahip bir düğüm havuzunda çalışacak bir pod zamanlayabilirsiniz.

Birden çok geliştirme ekiplerini ve uygulamayı barındıran bir aks kümesinde, bu kaynak istekleri ve limitler kümesi olmadan Pod 'yi izlemek zor olabilir. En iyi uygulama olarak, özellikle de `kube-advisor` ad alanlarına kaynak kotaları atamadıysanız, aks kümelerinizde düzenli olarak çalışır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede temel Kubernetes Zamanlayıcı özelliklerine odaklanılmıştır. AKS 'deki küme işlemleri hakkında daha fazla bilgi için aşağıdaki en iyi yöntemlere bakın:

* [Çok kiracılılık ve küme yalıtımı][aks-best-practices-cluster-isolation]
* [Gelişmiş Kubernetes Zamanlayıcı özellikleri][aks-best-practices-advanced-scheduler]
* [Kimlik doğrulaması ve yetkilendirme][aks-best-practices-identity]

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
