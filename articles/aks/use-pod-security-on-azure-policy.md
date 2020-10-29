---
title: Azure Kubernetes Service (aks) ile Azure ilkesi ile güvenli Pod
description: Azure Kubernetes Service (aks) üzerinde Azure ilkesi ile pod güvenliğini sağlama hakkında bilgi edinin
services: container-service
ms.topic: article
ms.date: 09/22/2020
author: jluk
ms.openlocfilehash: 5178aa30c3bfec014dd10e2c4f3de182aaef7e68
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92900115"
---
# <a name="secure-pods-with-azure-policy"></a>Azure İlkesi ile pod güvenliğini sağlama

AKS kümenizin güvenliğini artırmak için, hangi işlevlerin verildiğini ve şirket ilkesine göre herhangi bir şeyin çalıştığını kontrol edebilirsiniz. Bu erişim, [AKS Için Azure Ilke eklentisi][kubernetes-policy-reference]tarafından belirtilen yerleşik ilkeler aracılığıyla tanımlanmıştır. Pod 'un belirtiminin güvenlik yönleri üzerinde ek denetim sağlayarak, kök ayrıcalıklar gibi, kümenizde dağıtılan özelliklerin daha sıkı güvenlik uygunluğunu ve görünürlüğünü sağlar. Pod, ilkede belirtilen koşulları karşılamıyorsa, Azure Ilkesi Pod 'un bir ihlalin başlamasını veya bayrak olarak işaretini değiştirmesine izin verebilir. Bu makalede, AKS 'deki yığınların dağıtımını sınırlamak için Azure Ilkesi 'nin nasıl kullanılacağı gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, mevcut bir AKS kümeniz olduğunu varsaymaktadır. AKS kümesine ihtiyacınız varsa bkz. [Azure CLI kullanarak][aks-quickstart-cli] aks hızlı başlangıç veya [Azure Portal kullanımı][aks-quickstart-portal].

### <a name="install-the-azure-policy-add-on-for-aks"></a>AKS için Azure Ilke eklentisini yükler

Azure Ilkesi aracılığıyla AKS 'lerin güvenliğini sağlamak için aks kümesinde AKS için Azure Ilke eklentisini yüklemeniz gerekir. [Azure Ilke eklentisini yüklemek için bu adımları](../governance/policy/concepts/policy-for-kubernetes.md#install-azure-policy-add-on-for-aks)izleyin.

Bu belgede, yukarıdaki kılavuzda, yukarıdaki kılavuzda dağıtılan, aşağıdakilere sahip olduğunuz varsayılır.

* `Microsoft.ContainerService` `Microsoft.PolicyInsights` Kullanılarak ve kaynak sağlayıcıları kaydettirildi`az provider register`
* Azure CLı 2,12 veya üzeri
* Azure Ilke eklentisi ile bir 1,15 veya daha yüksek sürümdeki bir AKS kümesi

## <a name="overview-of-securing-pods-with-azure-policy-for-aks"></a>Aks için Azure ilkesi ile pod 'nin güvenliğini sağlamaya genel bakış

>[!NOTE]
> Bu belgede, [Önizleme aşamasında Kubernetes Pod güvenlik ilkesi özelliğine](use-pod-security-policies.md)yönelik ardıl olan pods 'yi güvenli hale getirmek Için Azure ilkesi 'nin nasıl kullanılacağı ayrıntılı olarak açıklanır.
> **AKS için hem Pod güvenlik ilkesi (Önizleme) hem de Azure Ilke eklentisi eşzamanlı olarak etkinleştirilemez.**
> 
> Azure Ilke eklentisini Pod güvenlik ilkesi etkinleştirilmiş bir kümeye yüklüyorsanız, [Pod güvenlik ilkesini devre dışı bırakmak için aşağıdaki adımları izleyin](use-pod-security-policies.md#enable-pod-security-policy-on-an-aks-cluster).

Bir AKS kümesinde, bir kaynak oluşturulup güncelleştirilirken API sunucusuna yapılan istekleri ele almak için bir giriş denetleyicisi kullanılır. Giriş denetleyicisi daha sonra, oluşturulması gerekip gerekmediğini, kaynak isteğini bir dizi kurala karşı *doğrulayabilir* .

Daha önce, özellik [Pod güvenlik ilkesi (Önizleme)](use-pod-security-policies.md) , Kubernetes projesi aracılığıyla, hangi yığınların dağıtılabileceklerini sınırlamak için etkinleştirilmiştir.

Bir aks kümesi, Azure ilke eklentisini kullanarak, daha önce pod güvenlik ilkesine benzer Pod ve diğer Kubernetes kaynaklarını güvenli hale getirmek için yerleşik Azure ilkelerini kullanabilir. AKS için Azure Ilke eklentisi, bir giriş denetleyicisi olan bir yönetim [ağ geçidi](https://github.com/open-policy-agent/gatekeeper)denetleyicisinin yönetilen bir örneğini yükler. Kubernetes için Azure Ilkesi, [rego ilke diline](../governance/policy/concepts/policy-for-kubernetes.md#policy-language)dayanan açık kaynaklı açık ilke aracısına kurulmuştur.

Bu belgede, bir aks kümesindeki Pod 'yi güvenli hale getirmek ve pod güvenlik ilkelerinden (Önizleme) nasıl geçiş yapılacağı hakkında bilgi edinmek için Azure ilkesi 'nin nasıl kullanılacağı açıklanır.

## <a name="limitations"></a>Sınırlamalar

Aşağıdaki genel sınırlamalar, Kubernetes kümeleri için Azure Ilke eklentisi için geçerlidir:

- Kubernetes için Azure Ilke eklentisi, Kubernetes sürüm **1,14** veya üzeri sürümlerde desteklenir.
- Kubernetes için Azure Ilke eklentisi yalnızca Linux düğüm havuzlarına dağıtılabilir
- Yalnızca yerleşik ilke tanımları desteklenir
- Küme başına ilke başına en fazla uyumlu olmayan kayıt sayısı: **500**
- Abonelik başına en fazla uyumlu olmayan kayıt sayısı: **1.000.000**
- Azure Ilke eklentisi dışında ağ geçidi denetleyicisi yüklemeleri desteklenmez. Azure Ilke eklentisini etkinleştirmeden önce önceki bir Gatekeeper yüklemesi tarafından yüklenen tüm bileşenleri kaldırın.
- Bu [kaynak sağlayıcısı modu](../governance/policy/concepts/definition-structure.md#resource-provider-modes) Için [uyumsuzluk nedenleri](../governance/policy/how-to/determine-non-compliance.md#compliance-reasons) kullanılamaz

Aşağıdaki sınırlamalar yalnızca AKS için Azure Ilke eklentisi için geçerlidir:

- Aks [Pod güvenlik ilkesi (Önizleme)](use-pod-security-policies.md) ve aks Için Azure ilke eklentisi etkinleştirilebilir. 
- Ad alanları, değerlendirme için Azure Ilke eklentisi tarafından otomatik olarak dışlanır: _KUVE sistem_ , _Gatekeeper-System_ ve _aks-Periscope_ .

### <a name="recommendations"></a>Öneriler

Aşağıda, Azure Ilke eklentisinin kullanılmasına yönelik genel öneriler verilmiştir:

- Azure Ilke eklentisinin çalışması için 3 Gatekeeper bileşeni gerekir: 1 denetim Pod ve 2 Web kancası Pod çoğaltmaları. Bu bileşenler, küme içinde denetim ve zorlama işlemleri gerektiren Kubernetes kaynakları ve ilke atamalarının sayısı arttıkça daha fazla kaynak kullanır.

  - En fazla 20 kısıtlama içeren tek bir kümede 500 ' den az pod için: bileşen başına 2 sanal CPU ve 350 MB bellek.
  - En fazla 40 kısıtlama içeren tek bir kümede 500 ' den fazla dizin için: bileşen başına 3 vCPU ve 600 MB bellek.

Aşağıdaki öneri yalnızca AKS ve Azure Ilkesi eklentisi için geçerlidir:

- `CriticalAddonsOnly`Gatekeeper pods 'yi zamanlamak için Taint ile sistem düğüm havuzunu kullanın. Daha fazla bilgi için bkz. [sistem düğüm havuzlarını kullanma](use-system-pools.md#system-and-user-node-pools).
- AKS kümelerinizdeki giden trafiği güvenli hale getirin. Daha fazla bilgi için bkz. [küme düğümleri Için denetim çıkış trafiği](limit-egress-traffic.md).
- Küme `aad-pod-identity` etkinleştirilmişse, düğüm tarafından yönetilen kimlik (NMI) Pod, Azure örnek meta veri uç noktasına yapılan çağrıları ele almak için düğümlerin Iptables 'larını değiştirir. Bu yapılandırma, Pod kullanılmasa bile meta veri uç noktasına yapılan her türlü isteğin NMI tarafından yakalanmasıdır `aad-pod-identity` . AzurePodIdentityException CRD, `aad-pod-identity` CRD 'de tanımlanan etiketlerle eşleşen bir pod 'dan kaynaklanan meta veri uç noktasına yapılan tüm isteklerin NMI içinde herhangi bir işlem yapılmadan proxy olması gerektiğini bildirmek üzere yapılandırılabilir. `kubernetes.azure.com/managedby: aks` _Kuto-System_ ad alanındaki etiketli sistem KÖKLERI `aad-pod-identity` , AzurePodIdentityException CRD 'yi yapılandırarak içinde dışlanmalıdır. Daha fazla bilgi için bkz. [belirli bir pod veya uygulama için AAD-Pod kimliğini devre dışı bırakma](https://azure.github.io/aad-pod-identity/docs/configure/application_exception).
  Bir özel durum yapılandırmak için, [MIC özel durum YAML](https://github.com/Azure/aad-pod-identity/blob/master/deploy/infra/mic-exception.yaml)'yi yükler.

Azure Ilke eklentisi, CPU ve bellek kaynaklarının çalışmasını gerektirir. Bu gereksinimler bir kümenin boyutu arttıkça artar. Azure Ilke eklentisinin kullanımı için genel yönergeler için bkz. [Azure ilke önerileri][policy-recommendations] .

## <a name="azure-policies-to-secure-kubernetes-pods"></a>Kubernetes Pod güvenliğini sağlamak için Azure ilkeleri

Azure Ilke eklentisi yüklendikten sonra varsayılan olarak hiçbir ilke uygulanmaz.

Tek bir AKS kümesindeki Azure ilkeleri ve özel olarak oluşan iki yerleşik girişim vardır.
Her ilke bir efekt ile özelleştirilebilir. [Aks ilkelerinin tam listesi ve bunların desteklenen etkileri burada listelenmiştir][policy-samples]. [Azure ilke etkileri](../governance/policy/concepts/effects.md)hakkında daha fazla bilgi edinin.

Azure ilkeleri, yönetim grubu, abonelik veya kaynak grubu düzeyinde uygulanabilir. Kaynak grubu düzeyinde bir ilke atarken, hedef AKS kümesinin kaynak grubunun ilke kapsamında seçildiğinden emin olun. Azure Ilke eklentisi yüklü olan atanan kapsamdaki her küme, ilke kapsamındadır.

[Pod güvenlik ilkesi (Önizleme) ](use-pod-security-policies.md)kullanıyorsanız, [Azure ilkesine geçiş yapmayı ve diğer davranış farklılıkları hakkında](#migrate-from-kubernetes-pod-security-policy-to-azure-policy)bilgi edinin.

### <a name="built-in-policy-initiatives"></a>Yerleşik ilke girişimleri

Azure Ilkesindeki bir girişim, tekil bir hedef elde etmek için tasarlanmış ilke tanımlarının koleksiyonudur. Girişimlerin kullanımı, AKS kümelerinde ilkelerin yönetimini ve atanmasını kolaylaştırabilir. Bir girişim tek bir nesne olarak bulunur, [Azure ilke girişimleri](../governance/policy/overview.md#initiative-definition)hakkında daha fazla bilgi edinin.

Kubernetes için Azure Ilkesi, düğüm, [taban çizgisi](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Fa8640138-9b0a-4a28-b8cb-1666c838647d) ve [Kısıtlanmış](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F42b8ef37-b724-4e24-bbc8-7a7708edfe00)güvenli iki yerleşik girişim sunmaktadır.

Hem yerleşik girişimler, [Kubernetes 'in Pod güvenlik ilkesinde](https://github.com/kubernetes/website/blob/master/content/en/examples/policy/baseline-psp.yaml)kullanılan tanımlardan oluşturulur.

|[Pod güvenlik ilkesi denetimi](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#what-is-a-pod-security-policy)| Azure Ilke tanımı bağlantısı| [Temel girişim](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Fa8640138-9b0a-4a28-b8cb-1666c838647d) | [Kısıtlanmış girişim](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F42b8ef37-b724-4e24-bbc8-7a7708edfe00) |
|---|---|---|---|
|Ayrıcalıklı kapsayıcıların çalıştırılmasına izin verme|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F95edb821-ddaf-4404-9732-666045e056b4)| Evet | Evet
|Konak ad alanlarının paylaşılan kullanımına izin verme|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F47a1ee2f-2a2a-4576-bf2a-e0e36709c2b8)| Evet | Evet
|Konak ağının ve bağlantı noktalarının tüm kullanımını kısıtla|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F82985f06-dc18-4a48-bc1c-b9f4f0098cfe)| Evet | Evet
|Konak FileSystem 'ın herhangi bir kullanımını kısıtla|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F098fc59e-46c7-4d99-9b16-64990e543d75)| Evet | Evet
|Linux yeteneklerini [varsayılan küme](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) ile kısıtla|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc26596ff-4d70-4e6a-9a30-c2506bd2f80c) | Evet | Evet
|Tanımlı birim türlerinin kullanımını kısıtla|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F16697877-1118-4fb1-9b65-9898ec2509ec)| - | Evet-izin verilen birim türleri `configMap` , `emptyDir` ,,, `projected` `downwardAPI``persistentVolumeClaim`|
|Köke yönelik ayrıcalık yükseltme|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1c6e92c9-99f0-4e55-9cf2-0c234dc48f99) | - | Evet |
|Kapsayıcının Kullanıcı ve grup kimliklerini kısıtlama|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff06ddb64-5fa3-4b77-b166-acb36f7f6042) | - | Evet|
|Pod birimlerine sahip bir FSGroup ayırmayı kısıtlama|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff06ddb64-5fa3-4b77-b166-acb36f7f6042) | - | Evet-izin verilen kurallar,,, `runAsUser: mustRunAsNonRoot` `supplementalGroup: mustRunAs 1:65536` `fsGroup: mustRunAs 1:65535` `runAsGroup: mustRunAs 1:65535` .  |
|Seccomp profili gerektirir|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F975ce327-682c-4f2e-aa46-b9598289b86c) | - | Evet, allowedProfiles * `docker/default` veya `runtime/default` |

\* Docker/default, v 1.11 beri Kubernetes 'te kullanım dışıdır

### <a name="additional-optional-policies"></a>Ek isteğe bağlı ilkeler

Bir girişim uygulama dışında listedir uygulanabilecek ek Azure ilkeleri vardır. Gereksinimleriniz yerleşik girişimlerle karşılanmazsa, girişimlere ek olarak bu ilkeleri eklemeyi göz önünde bulundurun.

|[Pod güvenlik ilkesi denetimi](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#what-is-a-pod-security-policy)| Azure Ilke tanımı bağlantısı| Temel girişim 'ya ek olarak Uygula | Kısıtlanmış girişim 'ya ek olarak Uygula |
|---|---|---|---|
|Kapsayıcılar tarafından kullanılan AppArmor profilini tanımlama|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F511f5417-5d12-434d-ab2e-816901e72a5e) | İsteğe bağlı | İsteğe bağlı |
|Salt okunmayan takmaları izin ver|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fdf49d893-a74c-421d-bc95-c663042e5b80) | İsteğe bağlı | İsteğe bağlı |
|Belirli FlexVolume sürücüleriyle kısıtla|[Genel bulut](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff4a8fce0-2dd5-4c21-9a36-8f0ec809d663) | İsteğe bağlı-yalnızca FlexVolume sürücülerini kısıtlamak istiyorsanız kullanın, ancak diğerlerini "tanımlanan birim türlerinin kullanımını kısıtla" olarak ayarlayın. | Uygulanamaz-kısıtlanmış girişim, tüm FlexVolume sürücülerine izin vermeyen "tanımlanmış birim türlerinin kullanımını kısıtla" yı içerir |

### <a name="unsupported-built-in-policies-for-managed-aks-clusters"></a>Yönetilen AKS kümeleri için desteklenmeyen yerleşik ilkeler

> [!NOTE]
> Yönetilen bir hizmet olarak AKS tarafından yönetilen ve korunan yönleri özelleştirme nedeniyle, aşağıdaki 3 ilke **aks 'te desteklenmez** . Bu ilkeler özellikle, yönetilmeyen denetim düzlemleri olan Azure Arc bağlantılı kümeler için oluşturulmuştur.

|[Pod güvenlik ilkesi denetimi](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#what-is-a-pod-security-policy)|
|---|
|Bir kapsayıcının özel SELinux bağlamını tanımlama|
|Kapsayıcılar tarafından kullanılan sysctl profilini kısıtlama|
|Varsayılan proc takma türleri saldırı yüzeyini azaltmak için tanımlanır|

<!---
# Removing until custom initiatives are supported the week after preview

#### Custom initiative

If the built-in initiatives to address pod security do not match your requirements, you can choose your own policies to exist in a custom initiative. Read more about [building custom initatives in Azure Policy](../governance/policy/tutorials/create-and-manage#create-and-assign-an-initiative-definition).

--->

### <a name="namespace-exclusion"></a>Ad alanı dışlaması

> [!WARNING]
> Bir kümenin sağlıklı kalması için, kuas-System gibi yönetici ad alanlarında bulunan pods 'lerin, gerekli bir sistem Pod 'ı nedeniyle ilke ihlallerini tetikleyebilmesi için, varsayılan dışlanan ad alanları listesinden gerekli bir ad alanının kaldırılması gerekir.

AKS 'ler, DNS çözümlemesi gibi kritik hizmetleri sağlamak için sistem kimliklerinin bir kümede çalıştırılmasını gerektirir. Pod işlevselliğini sınırlayan ilkeler, sistem Pod kararlılığı özelliğini etkileyebilir. Sonuç olarak, aşağıdaki ad alanları, **oluşturma, güncelleştirme ve ilke denetimi sırasında giriş istekleri sırasında ilke değerlendirmesinden çıkarılır** . Bu, bu ad alanlarına yeni dağıtımları Azure ilkelerinden hariç tutulacak şekilde zorlar.

1. kuin-sistem
1. Gatekeeper-sistem
1. Azure-yay
1. aks-Periscope

Oluşturma, güncelleştirme ve Denetim sırasında diğer özel ad alanları değerlendirmeden dışlanamaz. Bu Dışlamalar, tasdikli bir ad alanında çalışan ve denetim ihlallerinin tetiklenmesinden kaçınmak istediğiniz özel bir pod varsa kullanılmalıdır.

## <a name="apply-the-baseline-initiative"></a>Temel girişim 'yi uygulama

> [!TIP]
> Tüm ilkeler varsayılan olarak bir denetim efektidir. Etkileri, Azure Ilkesi aracılığıyla dilediğiniz zaman reddedecek şekilde güncelleştirilemeyebilir.

Temel girişim 'yi uygulamak için Azure portal aracılığıyla atayabiliriz.
<!--
1. Navigate to the Policy service in Azure portal
1. In the left pane of the Azure Policy page, select **Definitions**
1. Search for "Baseline Profile" on the search pane to the right of the page
1. Select `Kubernetes Pod Security Standards Baseline Profile for Linux-based workloads` from the `Kubernetes` category
-->
1. Pod güvenlik temel aldığı girişimi gözden geçirmek için [Azure Portal bu bağlantıyı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Fa8640138-9b0a-4a28-b8cb-1666c838647d) izleyin
1. **Kapsamı** abonelik düzeyine veya yalnızca Azure ilke eklentisi etkin olan aks kümelerini tutan kaynak grubuna ayarlayın
1. **Parametreler** sayfasını seçin ve **Effect** `audit` `deny` temel girişimi ihlal eden yeni dağıtımları engellemek Için ' den ' a kadar etkisini güncelleştirin
1. Oluşturma, güncelleştirme ve Denetim sırasında değerlendirmeden dışlamak üzere ek ad alanları ekleyin, [bazı ad alanları ilke değerlendirmesinden zorla çıkarılır.](#namespace-exclusion) 
 ![ Efekt Güncelleştir](media/use-pod-security-on-azure-policy/update-effect.png)
1. İlkeleri göndermek için **gözden geçir + oluştur** ' u seçin

İlkeleri çalıştırarak, kümenize ilke uygulandığını doğrulayın `kubectl get constrainttemplates` .

> [!NOTE]
> İlkelerin her kümeyle [eşitlenmesi 20 dakikaya kadar](../governance/policy/concepts/policy-for-kubernetes.md#assign-a-built-in-policy-definition) sürebilir.

Çıktının şuna benzer olması gerekir:

```console
$ kubectl get constrainttemplate
NAME                                     AGE
k8sazureallowedcapabilities              30m
k8sazureblockhostnamespace               30m
k8sazurecontainernoprivilege             30m
k8sazurehostfilesystem                   30m
k8sazurehostnetworkingports              30m
```

## <a name="validate-rejection-of-a-privileged-pod"></a>Ayrıcalıklı Pod 'ın ret olduğunu doğrulama

' In güvenlik bağlamı ile pod zamanladığınızda, ilk test edelim `privileged: true` . Bu güvenlik bağlamı Pod 'un ayrıcalıklarını ilerletir. Taban çizgisi girişimi ayrıcalıklı yığınlara izin vermez, bu nedenle istek reddedilir ve dağıtım reddedilmekte olur.

Adlı bir dosya oluşturun `nginx-privileged.yaml` ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged
spec:
  containers:
    - name: nginx-privileged
      image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
      securityContext:
        privileged: true
```

[Kubectl Apply][kubectl-apply] komutuyla Pod oluşturun ve YAML bildiriminizde adı belirtin:

```console
kubectl apply -f nginx-privileged.yaml
```

Beklenen şekilde, aşağıdaki örnek çıktıda gösterildiği gibi Pod 'un zamanlanması başarısız olur:

```console
$ kubectl apply -f privileged.yaml

Error from server ([denied by azurepolicy-container-no-privilege-00edd87bf80f443fa51d10910255adbc4013d590bec3d290b4f48725d4dfbdf9] Privileged container is not allowed: nginx-privileged, securityContext: {"privileged": true}): error when creating "privileged.yaml": admission webhook "validation.gatekeeper.sh" denied the request: [denied by azurepolicy-container-no-privilege-00edd87bf80f443fa51d10910255adbc4013d590bec3d290b4f48725d4dfbdf9] Privileged container is not allowed: nginx-privileged, securityContext: {"privileged": true}
```

Pod, zamanlama aşamasına ulaşmıyor, bu nedenle, üzerinde geçiş yapmadan önce silinecek kaynak yok.

## <a name="test-creation-of-an-unprivileged-pod"></a>Ayrıcalıksız Pod 'ın test oluşturması

Önceki örnekte kapsayıcı görüntüsü, NGıNX 'i 80 numaralı bağlantı noktasına bağlamak için otomatik olarak kök kullanmaya çalıştı. Bu istek temel ilke girişimi tarafından reddedildi, bu yüzden Pod başlatılamıyor. Artık, ayrıcalıklı erişim olmadan aynı NGıNX Pod 'u çalıştırmayı deneyelim.

Adlı bir dosya oluşturun `nginx-unprivileged.yaml` ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged
spec:
  containers:
    - name: nginx-unprivileged
      image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
```

[Kubectl Apply][kubectl-apply] komutunu kullanarak Pod 'ı oluşturun ve YAML bildiriminizde adı belirtin:

```console
kubectl apply -f nginx-unprivileged.yaml
```

Pod başarıyla zamanlandı. [Kubectl Get Pod][kubectl-get] komutunu kullanarak Pod 'un durumunu denetlediğinizde, Pod *çalışır* :

```console
$ kubectl get pods

NAME                 READY   STATUS    RESTARTS   AGE
nginx-unprivileged   1/1     Running   0          18s
```

Bu örnekte, yalnızca koleksiyondaki ilkeleri ihlal eden dağıtımları etkileyen temel girişim gösterilmektedir. İzin verilen dağıtımlar çalışmaya devam eder.

[Kubectl Delete][kubectl-delete] komutunu kullanarak NGINX unayrıcalıklı Pod öğesini SILIN ve YAML bildiriminizde adı belirtin:

```console
kubectl delete -f nginx-unprivileged.yaml
```

## <a name="disable-policies-and-the-azure-policy-add-on"></a>İlkeleri ve Azure Ilkesi eklentisini devre dışı bırakma

Temel girişim 'yi kaldırmak için:

1. Azure portal Ilke bölmesine gidin
1. Sol bölmedeki **atamaları** seçin
1. "..." Düğmesine tıklayın. Ana hat profilinin yanındaki düğme
1. "Atamayı Sil" i seçin

![Atamayı Sil](media/use-pod-security-on-azure-policy/delete-assignment.png)

Azure Ilkesi eklentisini devre dışı bırakmak için [az aks Disable-addons][az-aks-disable-addons] komutunu kullanın.

```azurecli
az aks disable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
```

[Azure Portal Azure Ilke eklentisini](../governance/policy/concepts/policy-for-kubernetes.md#remove-the-add-on-from-aks)kaldırma hakkında bilgi edinin.

## <a name="migrate-from-kubernetes-pod-security-policy-to-azure-policy"></a>Kubernetes Pod güvenlik ilkesinden Azure Ilkesine geçiş yapma

Pod güvenlik ilkesinden geçiş yapmak için, bir kümede aşağıdaki işlemleri gerçekleştirmeniz gerekir.

1. Kümede [Pod güvenlik Ilkesini devre dışı bırak](use-pod-security-policies.md#clean-up-resources)
1. [Azure Ilke eklentisini][kubernetes-policy-reference] etkinleştirin
1. İstenen Azure ilkelerini [kullanılabilir yerleşik ilkelerden][policy-samples] etkinleştirin

Pod güvenlik ilkesi ve Azure Ilkesi arasındaki davranış değişikliklerinin özeti aşağıda verilmiştir.

|Senaryo| Pod güvenlik ilkesi | Azure İlkesi |
|---|---|---|
|Yükleme|Pod güvenlik ilkesi özelliğini etkinleştir |Azure Ilke eklentisini etkinleştir
|İlkeleri dağıtma| Pod güvenlik ilkesi kaynağı dağıtma| Abonelik veya kaynak grubu kapsamına Azure ilkeleri atayın. Kubernetes kaynak uygulamaları için Azure Ilke eklentisi gereklidir.
| Varsayılan ilkeler | AKS 'de Pod güvenlik ilkesi etkinleştirildiğinde, varsayılan ayrıcalıklı ve Kısıtlanmamış ilkeler uygulanır. | Azure Ilke eklentisi etkinleştirilerek hiçbir varsayılan ilke uygulanmaz. Azure Ilkesinde ilkeleri açıkça etkinleştirmeniz gerekir.
| Kimin ilke oluşturup atayabileceği | Küme Yöneticisi bir pod güvenlik ilkesi kaynağı oluşturur | Kullanıcıların AKS kümesi kaynak grubunda en az ' Owner ' veya ' kaynak Ilkesi katılımcısı ' izinlerine sahip olması gerekir. --API, kullanıcılar AKS kümesi kaynak kapsamına ilke atayabilir. Kullanıcının AKS küme kaynağı üzerinde en az ' Owner ' veya ' kaynak Ilkesi katılımcısı ' izinlerine sahip olması gerekir. -Azure portal, ilkeler yönetim grubu/abonelik/kaynak grubu düzeyinde atanabilir.
| İlkeleri yetkilendirme| Kullanıcılar ve hizmet hesapları, Pod güvenlik ilkelerini kullanmak için açık izinler gerektirir. | İlkeleri yetkilendirmek için ek atama gerekmez. Azure 'da ilkeler atandıktan sonra, tüm küme kullanıcıları bu ilkeleri kullanabilir.
| İlke uygulanabilirliği | Yönetici Kullanıcı, Pod güvenlik ilkelerinin zorlanmasını atlar. | Tüm kullanıcılar (yönetici & yönetici olmayan) aynı ilkeleri görür. Kullanıcılara dayalı özel bir büyük harf yoktur. İlke uygulaması ad alanı düzeyinde dışlenebilir.
| İlke kapsamı | Pod güvenlik ilkeleri gösterilemez değildir | Azure Ilkesi tarafından kullanılan kısıtlama şablonları gösterilemez değildir.
| Reddetme/denetim/mutasyon eylemi | Pod güvenlik ilkeleri yalnızca reddetme eylemlerini destekler. Mutasyon, oluşturma isteklerinde varsayılan değerlerle yapılabilir. Doğrulama, güncelleştirme istekleri sırasında yapılabilir.| Azure Ilkesi, hem denetim & reddetme eylemlerini destekler. Mutasyon henüz desteklenmiyor, ancak planlandı.
| Pod güvenlik ilkesi uyumluluğu | Pod güvenlik ilkesi etkinleştirilmeden önce var olan yığınların uyumluluğuna ilişkin bir görünürlük yoktur. Pod güvenlik ilkeleri etkinleştirildikten sonra oluşturulan uyumlu olmayan Pod 'ler reddedilir. | Azure ilkelerini uygulamadan önce var olan uyumlu olmayan bir pod, ilke ihlallerine göre görünür. İlkeler reddetme efektiyle ayarlandıysa, Azure ilkeleri etkinleştirildikten sonra oluşturulan uyumlu olmayan Pod 'ler reddedilir.
| Kümedeki ilkeleri görüntüleme | `kubectl get psp` | `kubectl get constrainttemplate` -Tüm ilkeler döndürülür.
| Pod güvenlik ilkesi standart-ayrıcalıklı | Özellik etkinleştirilirken ayrıcalıklı bir pod güvenlik ilkesi kaynağı varsayılan olarak oluşturulur. | Ayrıcalıklı mod hiçbir kısıtlama gerektirmez, sonuç olarak herhangi bir Azure Ilke atamasına sahip değildir.
| [Pod güvenlik ilkesi standart-taban çizgisi/varsayılan](https://kubernetes.io/docs/concepts/security/pod-security-standards/#baseline-default) | Kullanıcı bir pod güvenlik ilkesi temel kaynağı yüklüyor. | Azure Ilkesi, taban çizgisi Pod güvenlik ilkesiyle eşleşen [yerleşik bir temel girişim](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Fa8640138-9b0a-4a28-b8cb-1666c838647d) sağlar.
| [Pod güvenlik ilkesi standart-kısıtlı](https://kubernetes.io/docs/concepts/security/pod-security-standards/#restricted) | Kullanıcı, bir pod güvenlik ilkesi kısıtlanmış kaynağı yüklüyor. | Azure Ilkesi, kısıtlı Pod güvenlik ilkesiyle eşleşen [yerleşik bir kısıtlanmış girişim](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F42b8ef37-b724-4e24-bbc8-7a7708edfe00) sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ayrıcalıklı erişimin kullanımını engellemek için ayrıcalıklı yığınların dağıtılmasını kısıtlayan bir Azure ilkesinin nasıl uygulanacağı açıklanır. Birimlerin kullanımını kısıtlayan ilkeler gibi uygulanabilen birçok ilke vardır. Kullanılabilir seçenekler hakkında daha fazla bilgi için bkz. [Kubernetes Için Azure ilkesi belgeleri][kubernetes-policy-reference].

Pod ağ trafiğini sınırlama hakkında daha fazla bilgi için bkz. [aks 'deki ağ ilkelerini kullanarak Pod arasındaki trafiği güvenli hale getirme][network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[aad-pod-identity]: https://github.com/Azure/aad-pod-identity
[aad-pod-identity-exception]: https://azure.github.io/aad-pod-identity/docs/configure/application_exception

<!-- LINKS - internal -->
[policy-recommendations]: ../governance/policy/concepts/policy-for-kubernetes.md
[policy-limitations]: ../governance/policy/concepts/policy-for-kubernetes.md?#limitations
[kubernetes-policy-reference]: ../governance/policy/concepts/policy-for-kubernetes.md
[policy-samples]: ./policy-reference.md#microsoftcontainerservice
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policies]: use-network-policies.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-aks-disable-addons]: /cli/azure/aks#az-aks-disable-addons
[az-policy-assignment-delete]: /cli/azure/policy/assignment#az-policy-assignment-delete