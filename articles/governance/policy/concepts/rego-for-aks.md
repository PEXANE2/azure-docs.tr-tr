---
title: Azure Kubernetes Hizmeti için Azure İlkesi öğrenin
description: Azure Kubernetes Hizmeti'ndeki kümeleri yönetmek için Azure İlke'nin Rego ve Open İlkes Aracısını nasıl kullandığını öğrenin.
ms.date: 03/27/2020
ms.topic: conceptual
ms.openlocfilehash: d77c5cf94a8239f4617e563961cbe1cc40e48fe0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80372654"
---
# <a name="understand-azure-policy-for-azure-kubernetes-service"></a>Azure Kubernetes Service için Azure İlkesini anlama

Azure İlkesi, kümelerinizde merkezi ve tutarlı bir şekilde ölçekte zorlama lar ve korumalar uygulamak için [Azure Kubernetes Hizmeti](../../../aks/intro-kubernetes.md) (AKS) ile tümleşir.
Azure İlkesi, [Açık İlke Aracısı](https://www.openpolicyagent.org/) (OPA) için bir _kabul denetleyiciwebhook'u_ olan [Gatekeeper](https://github.com/open-policy-agent/gatekeeper) v3'ün kullanımını genişleterek, Azure kaynaklarınızın ve AKS kümelerinizin uyumluluk durumunu tek bir yerden yönetmeyi ve raporlamayı mümkün kılar.

> [!IMPORTANT]
> AKS için Azure İlkesi Önizleme'dedir ve yalnızca yerleşik ilke tanımlarını destekler. Yerleşik ilkeler **Kubernetes** kategorisindedir. **EnforceRegoPolicy** etkisi ve ilgili **Kubernetes Hizmet** kategori ilkeleri _amortismana sunuluyor._ Bunun yerine, güncelleştirilmiş [EnforceOPAConstraint efektini](./effects.md#enforceopaconstraint) kullanın.

> [!WARNING]
> Bu özellik henüz tüm bölgelerde kullanılamıyor. Kullanıma ilişkin bir durum [için, AKS Sorunları - İlke Eklentisi için Değişiklik Kesme'ye](https://github.com/Azure/AKS/issues/1529)bakın.

## <a name="overview"></a>Genel Bakış

AKS kümenizle AKS için Azure İlkesi'ni etkinleştirmek ve kullanmak için aşağıdaki işlemleri yapmak için aşağıdaki işlemleri

- [Önizleme özelliklerini kullanma](#opt-in-for-preview)
- [Azure İlkesi Eklentisini Yükleme](#installation-steps)
- [AKS için bir ilke tanımı atama](#built-in-policies)
- [Doğrulama için bekleyin](#validation-and-reporting-frequency)

## <a name="opt-in-for-preview"></a>Önizleme için kabul edin

Azure İlkesi Eklentisi'ni yüklemeden veya hizmet özelliklerinden herhangi birini etkinleştirmeden önce **aboneliğinizmicrosoft.ContainerService** kaynak sağlayıcısını ve **Microsoft.PolicyInsights** kaynak sağlayıcısını etkinleştirmeli ve ardından önizlemeye katılmak üzere onaylanmalı. Önizlemeye katılmak için Azure portalında veya Azure CLI ile aşağıdaki adımları izleyin:

- Azure portalı:

  1. **Microsoft.ContainerService** ve **Microsoft.PolicyInsights** kaynak sağlayıcılarını kaydedin. Adımlar için [Kaynak sağlayıcıları ve türleri](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal)bkz.

  1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

     ![Tüm Hizmetlerde Politika Ara](../media/rego-for-aks/search-policy.png)

  1. Azure **İlkesi** sayfasının sol tarafında Önizlemeye Katıl'ı seçin.

     ![AKS önizleme seçimi için Politikaya katıl](../media/rego-for-aks/join-aks-preview.png)

  1. Önizlemeye eklenmesini istediğiniz abonelik satırını seçin.

  1. Abonelik listesinin en üstündeki **Kabul düğmesini** seçin.

- Azure CLI:

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  # Provider register: Register the Azure Kubernetes Service provider
  az provider register --namespace Microsoft.ContainerService

  # Provider register: Register the Azure Policy provider
  az provider register --namespace Microsoft.PolicyInsights

  # Feature register: enables installing the add-on
  az feature register --namespace Microsoft.ContainerService --name AKS-AzurePolicyAutoApprove
  
  # Use the following to confirm the feature has registered
  az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-AzurePolicyAutoApprove')].{Name:name,State:properties.state}"
  
  # Once the above shows 'Registered' run the following to propagate the update
  az provider register -n Microsoft.ContainerService
  
  ```

## <a name="azure-policy-add-on"></a>Azure İlkesi Eklentisi

Kubernetes için _Azure İlkes_ eklentisi, Azure İlkesi hizmetini Gatekeeper kabul denetleyicisine bağlar. _Kube-sistem_ ad alanına yüklenen eklenti aşağıdaki işlevleri yerine getirir:

- Kümedeki atamalar için Azure İlkesi hizmetini denetler.
- Kümedeki ilkeleri [kısıtlama şablonu](https://github.com/open-policy-agent/gatekeeper#constraint-templates) ve [kısıtlama](https://github.com/open-policy-agent/gatekeeper#constraints) özel kaynakları olarak dağır.
- Denetim ve uyumluluk ayrıntılarını Azure İlkesi hizmetine geri raporlar.

### <a name="installing-the-add-on"></a>Eklentinin yüklenmesi

#### <a name="prerequisites"></a>Ön koşullar

Eklentiyi AKS kümenize yüklemeden önce önizleme uzantısı yüklü olmalıdır. Bu adım Azure CLI ile yapılır:

1. Gatekeeper v2 ilkeleri yüklendiyse, **İlkeler (önizleme)** sayfasının altındaki AKS kümenizdeki **Devre ve Düzenle** düğmesini kaldırın.

1. Azure CLI sürümü 2.0.62 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

1. AKS kümesi sürüm _1.14_ veya daha yüksek olmalıdır. AKS küme sürümünüzü doğrulamak için aşağıdaki komut dosyasını kullanın:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Look for the value in kubernetesVersion
   az aks list
   ```

1. AKS için Azure CLI önizleme uzantısının _0.4.0_ sürümünü yükleyin: `aks-preview`

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Install/update the preview extension
   az extension add --name aks-preview

   # Validate the version of the preview extension
   az extension show --name aks-preview --query [version]
   ```

   > [!NOTE]
   > Daha önce aks önizleme uzantısını yüklediyseniz, tüm _güncelleştirmeleri_ komutu kullanarak yükleyin. `az extension update --name aks-preview`

#### <a name="installation-steps"></a>Yükleme adımları

Ön koşullar tamamlandıktan sonra, yönetmek istediğiniz AKS kümesine Azure İlkesi Eklentisi'ni yükleyin.

- Azure portalında

  1. **Tüm hizmetleri**tıklatarak, ardından **Kubernetes hizmetlerini**arayarak ve seçerek AKS hizmetini Azure portalında başlatın.

  1. AKS kümelerinizden birini seçin.

  1. Kubernetes hizmet sayfasının sol tarafında **İlkeler (önizleme)** seçeneğini belirleyin.

     ![AKS kümesinden ilkeler](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. Ana sayfada **Eklentiyi Etkinleştir** düğmesini seçin.

     ![AKS eklentisi için Azure İlkesi'ni etkinleştirme](../media/rego-for-aks/enable-policy-add-on.png)

     > [!NOTE]
     > **Eklentiyi Etkinleştir** düğmesi gri renkteyse, abonelik henüz önizlemeye eklenmedi. Gerekli adımlar [için önizleme için Opt-in'e](#opt-in-for-preview) bakın. Devre **Dışı Atdüğmesine** uygunsa, Gatekeeper v2 hala yüklenir ve kaldırılması gerekir.

- Azure CLI

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks enable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

### <a name="validation-and-reporting-frequency"></a>Doğrulama ve raporlama sıklığı

Eklenti, ilke atamalarındaki değişiklikler için her 15 dakikada bir Azure İlkesi hizmetini denetler.
Bu yenileme döngüsü sırasında, eklenti değişiklikleri denetler. Bu değişiklikler, kısıtlama şablonlarını ve kısıtlamaları oluşturur, güncelleştirir veya siler.

> [!NOTE]
> Küme yöneticisi, kısıtlama şablonları ve kısıtlama kaynakları oluşturma ve güncelleştirme iznine sahip olsa da, el ile güncelleştirmeler üzerine yazıldığı için bunlar desteklenmez.

Her 15 dakikada bir, eklenti kümenin tam taramayı çağırır. Tam taramanın ayrıntılarını ve Gatekeeper tarafından kümedeki değişiklik denemelerinin gerçek zamanlı değerlendirmelerini topladıktan sonra, eklenti sonuçları herhangi bir Azure İlkesi ataması gibi [uyumluluk ayrıntılarına](../how-to/get-compliance-data.md#portal) dahil etmek üzere Azure İlkesi hizmetine geri bildirir. Yalnızca etkin ilke atamalarının sonuçları denetim döngüsü sırasında döndürülür. Denetim sonuçları, başarısız kısıtlamanın durum alanında listelenen [ihlaller](https://github.com/open-policy-agent/gatekeeper#audit) olarak da görülebilir.

## <a name="policy-language"></a>İlke dili

Kubernetes'i yönetmek için Azure İlkesi dil yapısı, varolan ilkelere uyar. _EnforceOPAConstraint_ etkisi Kubernetes kümelerinizi yönetmek için kullanılır ve [OPA Constraint Framework](https://github.com/open-policy-agent/frameworks/tree/master/constraint) ve Gatekeeper v3 ile çalışmaya özel ayrıntı özelliklerini alır. Ayrıntılar ve örnekler için [EnforceOPAConstraint efektine](./effects.md#enforceopaconstraint) bakın.
  
_Details.constraintTemplate_ ve _details.constraint_ özelliklerinin bir parçası olarak, Azure İlkesi bu [Özel Kaynak Tanımlarının](https://github.com/open-policy-agent/gatekeeper#constraint-templates) (CRD) UrI'lerini eklentiye geçirir. Rego, OPA ve Gatekeeper'ın Kubernetes kümesine bir isteği doğrulamak için desteklediği dildir. Azure İlkesi, Kubernetes yönetimi için varolan bir standardı destekleyerek, varolan kuralları yeniden kullanmanın ve birleşik bir bulut uyumluluğu raporlama deneyimi için Azure İlkesi ile eşleştirmeyi mümkün kılar. Daha fazla bilgi için [bkz: Rego nedir?](https://www.openpolicyagent.org/docs/latest/policy-language/#what-is-rego)

## <a name="built-in-policies"></a>Yerleşik ilkeler

Azure portalını kullanarak kümenizi yönetmeye yönelik yerleşik ilkeleri bulmak için aşağıdaki adımları izleyin:

1. Azure Portalı'nda Azure İlkesi hizmetini başlatın. Sol bölmedeki Tüm hizmetleri seçin ve ardından **İlke'yi**arayın ve seçin.

1. Azure İlkesi sayfasının sol bölmesinde **Tanımlar'ı**seçin.

1. Kategori açılır liste kutusundan, filtreyi temizlemek için Tümünü Seç'i kullanın ve ardından **Kubernetes'i**seçin.

1. İlke tanımını seçin ve ardından **Atla** düğmesini seçin.

1. **Kapsamı,** ilke atamasının uygulanacağı Kubernetes kümesinin yönetim grubu, aboneliği veya kaynak grubuna ayarlayın.

   > [!NOTE]
   > AKS tanımı için Azure İlkesi atarken, **Kapsam** AKS küme kaynağını içermelidir.

1. İlke atamasına kolayca tanımlamak için kullanabileceğiniz bir **Ad** ve **Açıklama** verin.

1. [İlke zorlamasını](./assignment-structure.md#enforcement-mode) aşağıdaki değerlerden birine ayarlayın.

   - **Etkin** - Kümedeki ilkeyi uygulayın. Kubernetes'in ihlallerle giriş talepleri reddedildi.
   
   - **Devre Dışı -** Kümedeki ilkeyi uygulamayın. Kubernetes'in ihlallerle giriş talepleri reddedilmez. Uyumluluk değerlendirme sonuçları hala mevcuttur. Kümeleri çalıştırmak için yeni ilkeler sunarken, devre _dışı_ bırakma seçeneği, ihlalleri olan kabul istekleri reddedilmedikçe ilkeleri sınamak için yararlıdır.

1. **Sonraki'ni**seçin.

1. **Parametre değerlerini** ayarlama
   
   - Kubernetes ad alanlarını ilke değerlendirmesinden hariç tutmak için, ad **alanı dışlayıcılıklarında**ad alanları listesini belirtin. Bu dışlamak için tavsiye edilir: _kube-sistemi_

1. **İncele ve oluştur**’u seçin.

Alternatif olarak, bir AKS ilkesi bulmak ve atamak için [bir ilke ata- Portal](../assign-policy-portal.md) hızlı başlat. Örnek 'denetim vms' yerine Bir Kubernetes ilke tanımı arayın.

> [!IMPORTANT]
> **Kategori Kubernetes** yerleşik ilkeleri sadece AKS ile kullanım içindir. Yerleşik ilkelerin listesi için [Kubernetes örneklerine](../samples/built-in-policies.md#kubernetes)bakın.

## <a name="logging"></a>Günlüğe Kaydetme

### <a name="azure-policy-add-on-logs"></a>Azure İlkesi Eklenti günlükleri

Bir Kubernetes denetleyicisi/kapsayıcısı olarak, hem Azure İlkes Eklentisi hem de Gatekeeper günlüklerini AKS kümesinde tutar. Günlükler AKS kümesinin **Öngörüler** sayfasında açıklanır. Daha fazla bilgi için, [kapsayıcılar için Azure Monitor ile AKS küme performansını anlayın' a](../../../azure-monitor/insights/container-insights-analyze.md)bakın.

## <a name="remove-the-add-on"></a>Eklentiyi kaldırma

AKS kümenizden Azure İlke Eklentisi'ni kaldırmak için Azure portalını veya Azure CLI'yi kullanın:

- Azure portalında

  1. **Tüm hizmetleri**tıklatarak, ardından **Kubernetes hizmetlerini**arayarak ve seçerek AKS hizmetini Azure portalında başlatın.

  1. Azure İlkesi Eklentisini devre dışı katmak istediğiniz AKS kümenizi seçin.

  1. Kubernetes hizmet sayfasının sol tarafında **İlkeler (önizleme)** seçeneğini belirleyin.

     ![AKS kümesinden ilkeler](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. Ana sayfada eklentiyi **devre dışı** bırakma düğmesini seçin.

     ![AKS eklentisi için Azure İlkesini devre dışı](../media/rego-for-aks/disable-policy-add-on.png)

- Azure CLI

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks disable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

## <a name="diagnostic-data-collected-by-azure-policy-add-on"></a>Azure İlkesi Eklentisi tarafından toplanan tanılama verileri

Kubernetes için Azure İlke Eklentisi sınırlı küme tanılama verileri toplar. Bu tanısal veriler yazılım ve performansla ilgili hayati önem taşıyan teknik verilerdir. Aşağıdaki şekillerde kullanılır:

- Azure İlkesi Eklentisini Güncel Tutun
- Azure İlkesi Eklentisi Eklentisi'ni güvenli, güvenilir ve performanslı tutun
- Eklentinin kullanımının toplu analizi yoluyla Azure İlkesi Eklentisini geliştirin

Eklenti tarafından toplanan bilgiler kişisel veri değildir. Aşağıdaki ayrıntılar şu anda toplanır:

- Azure İlkesi Eklenti aracısı sürümü
- Küme türü
- Küme bölgesi
- Küme kaynak grubu
- Kaynak kimliğini kümele
- Küme abonelik kimliği
- Küme İşletim Sistemi (Örnek: Linux)
- Küme şehir (Örnek: Seattle)
- Küme durumu veya eyalet (Örnek: Washington)
- Küme ülke veya bölge (Örnek: Amerika Birleşik Devletleri)
- İlke değerlendirmesinde aracı yüklemesi sırasında Azure İlkesi Eklentisi'nin karşılaştığı özel durumlar/hatalar
- Azure İlkesi Eklentisi tarafından yüklenmemiş Gatekeeper ilkeleri sayısı

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [İlke tanım yapısını](definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](effects.md) konusunu gözden geçirin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](../how-to/programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları](../how-to/remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.
