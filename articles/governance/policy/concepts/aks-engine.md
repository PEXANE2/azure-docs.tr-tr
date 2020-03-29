---
title: AKS Engine için Azure İlkesi öğrenin
description: Aks Engine ile kümeleri yönetmek için Azure İlke'nin Gatekeeper v3'ten CustomResourceDefinitions ve Open Policy Agent'ı nasıl kullandığını öğrenin.
ms.date: 11/04/2019
ms.topic: conceptual
ms.openlocfilehash: c41a9d84dfe43e356e9a4a17af523a37209c2933
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75436436"
---
# <a name="understand-azure-policy-for-aks-engine"></a>AKS Engine için Azure İlke'yi Anlayın

Azure İlkesi, Azure'da kendi kendini yöneten Kubernetes kümesini hızla önyükleme yapmak için kullanışlı bir araç lama sağlayan [AKS Engine](https://github.com/Azure/aks-engine/blob/master/docs/README.md)ile tümleşir. Bu tümleştirme, AKS Engine kendi kendini yöneten kümelerinizde merkezi ve tutarlı bir şekilde ölçekte zorlamave koruma sağlar. Azure Politikası, Kubernetes için bir _kabul denetleyicisi olan_ [Open Policy Agent](https://www.openpolicyagent.org/) (OPA) [Gatekeeper](https://github.com/open-policy-agent/gatekeeper) v3 (beta) kullanımını genişleterek, Azure kaynaklarınızın ve AKS Engine kendi kendini yöneten kümelerinizin uyumluluk durumunu tek bir yerden yönetmeyi ve raporlamayı mümkün kılar.

> [!NOTE]
> AKS Engine için Azure İlkesi Genel Önizleme'dedir ve SLA'sı yoktur. Gatekeeper v3 Beta'da dır ve açık kaynak topluluğu tarafından desteklenir. Hizmet yalnızca yerleşik ilke tanımlarını ve Hizmet Sorumlusu ile yapılandırılan her kaynak grubu için tek bir AKS Engine kümesini destekler.

> [!IMPORTANT]
> AKS Engine, AKS Engine veya Gatekeeper v3 için Azure İlkesi için destek almak için AKS Engine GitHub deposunda yeni bir [sorun](https://github.com/Azure/aks-engine/issues/new/choose) oluşturun.

## <a name="overview"></a>Genel Bakış

Azure'da kendi kendini yöneten Kubernetes kümenizle AKS Engine için Azure İlkesi'ni etkinleştirmek ve kullanmak için aşağıdaki işlemleri yapmak için aşağıdaki işlemleri

- [Ön koşullar](#prerequisites)
- [Azure İlkesi Eklentisini Yükleme](#installing-the-add-on)
- [AKS Engine için bir ilke tanımı atama](#built-in-policies)
- [Doğrulama için bekleyin](#validation-and-reporting-frequency)

## <a name="prerequisites"></a>Ön koşullar

Azure İlkesi Eklentisini yüklemeden veya hizmet özelliklerinden herhangi birini etkinleştirmeden önce aboneliğinizin **Microsoft.PolicyInsights** kaynak sağlayıcısını etkinleştirmesi ve küme hizmeti ilkesi için bir rol ataması oluşturması gerekir. 

1. Kaynak sağlayıcısını etkinleştirmek için [Kaynak sağlayıcılar ve türleri](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal) ile ilgili adımları izleyin veya Azure CLI veya Azure PowerShell komutunu çalıştırın:

   - Azure CLI

     ```azurecli-interactive
     # Log in first with az login if you're not using Cloud Shell
     
     # Provider register: Register the Azure Policy provider
     az provider register --namespace 'Microsoft.PolicyInsights'
     ```

   - Azure PowerShell
   
     ```azurepowershell-interactive
     # Log in first with Connect-AzAccount if you're not using Cloud Shell
   
     # Provider register: Register the Azure Policy provider
     Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
     ```

1. Küme hizmeti ilkesi için bir rol ataması oluşturma

   - Küme hizmeti ana uygulama kimliğini bilmiyorsanız, aşağıdaki komutla birlikte arayın.

     ```bash
     # Get the kube-apiserver pod name
     kubectl get pods -n kube-system
   
     # Find the aadClientID value
     kubectl exec <kube-apiserver pod name> -n kube-system cat /etc/kubernetes/azure.json
     ```

   - Azure CLI ile küme hizmeti ana uygulama kimliğine (önceki adımdaki _aadClientID_ değeri) 'İlke Öngörüleri Veri Yazarı (Önizleme)' rol ataması atayın. Abonelik `<subscriptionId>` kimliğinizle `<aks engine cluster resource group>` ve AKS Engine kendi kendini yöneten Kubernetes kümesinin içinde olduğu kaynak grubuyla değiştirin.

     ```azurecli-interactive
     az role assignment create --assignee <cluster service principal app ID> --scope "/subscriptions/<subscriptionId>/resourceGroups/<aks engine cluster resource group>" --role "Policy Insights Data Writer (Preview)"
     ```

## <a name="azure-policy-add-on"></a>Azure İlkesi Eklentisi

Kubernetes için _Azure İlkes_ eklentisi, Azure İlkesi hizmetini Gatekeeper kabul denetleyicisine bağlar. _Kube-sistem_ ad alanına yüklenen eklenti aşağıdaki işlevleri yerine getirir:

- AKS Engine kümesine atamalar için Azure İlkesi ile denetimler
- İlke ayrıntılarını, kısıtlama şablonlarını ve kısıtlamaları karşıdan yükler ve yükler
- AKS Engine kümesinde tam tarayın uyumluluk denetimi çalıştırın
- Denetim ve uyumluluk ayrıntılarını Azure İlkesi'ne geri raporlar

### <a name="installing-the-add-on"></a>Eklentinin yüklenmesi

Ön koşullar tamamlandıktan sonra Azure İlkesi Eklentisi yüklenebilir. Yükleme, bir AKS Altyapısının oluşturma veya güncelleştirme döngüsü sırasında veya varolan bir küme üzerinde bağımsız bir eylem olarak olabilir.

- Oluşturma veya güncelleştirme döngüsü sırasında yükleme

  Azure İlkesi Eklentisi'ni kendi kendini yöneten yeni bir kümenin oluşturulması sırasında veya varolan bir kümeye güncelleştirme olarak etkinleştirmek için AKS Engine için **addons** özellik küme tanımını ekleyin.

  ```json
  "addons": [{
      "name": "azure-policy",
      "enabled": true,
      "config": {
          "auditInterval": "30",
          "constraintViolationsLimit": "20"
      }
  }]
  ```

  Hakkında daha fazla bilgi için, dış kılavuz [AKS Engine küme tanımına](https://github.com/Azure/aks-engine/blob/master/docs/topics/clusterdefinitions.md)bakın.

- Miğfer Grafikleri ile varolan kümeye yükleme

  Kümeyi hazırlamak ve eklentiyi yüklemek için aşağıdaki adımları kullanın:

  1. Gatekeeper'ı _gatekeeper sistem_ ad alanına yükleyin.

     ```bash
     kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/master/deploy/gatekeeper.yaml
     ```
  1. _Kube-sisteme_ _kontrol düzlemi_ etiketi ekleyin. Bu etiket, _Kube-sistem_ bölmelerinin ve hizmetlerinin Gatekeeper ve Azure İlkesi Eklentisi tarafından denetlendiğini kapsamaz.

     ```bash
     kubectl label namespaces kube-system control-plane=controller-manager
     ```

  1. Kubernetes verilerini (Namespace, Pod, Ingress, Service) OPA ile senkronize edin.

     ```bash
     kubectl apply -f https://raw.githubusercontent.com/Azure/azure-policy/master/built-in-references/Kubernetes/gatekeeper-opa-sync.yaml
     ```

     Daha fazla bilgi için [OPA - Verileri çoğaltma](https://github.com/open-policy-agent/gatekeeper#replicating-data)ya da veri çoğaltma'ya bakın.

  1. Miğfer'e Azure İlkesi repo'su ekleyin.

     ```bash
     helm repo add azure-policy https://raw.githubusercontent.com/Azure/azure-policy/master/extensions/policy-addon-kubernetes/helm-charts
     ```

     Daha fazla bilgi için [Helm Chart - Quickstart Guide'a](https://helm.sh/docs/using_helm/#quickstart-guide)bakın.

  1. Eklentiyi Bir Miğfer Grafiği ile yükleyin. Abonelik `<subscriptionId>` kimliğinizle `<aks engine cluster resource group>` ve AKS Engine kendi kendini yöneten Kubernetes kümesinin içinde olduğu kaynak grubuyla değiştirin.

     ```bash
     helm install azure-policy/azure-policy-addon-aks-engine --name azure-policy-addon --set azurepolicy.env.resourceid="/subscriptions/<subscriptionId>/resourceGroups/<aks engine cluster resource group>"
     ```

     Eklenti Miğfer Grafiği'nin ne yüklendirişleri hakkında daha fazla bilgi için GitHub'daki [Azure İlkesi Eklenti Miğfer Grafiği tanımına](https://github.com/Azure/azure-policy/tree/master/extensions/policy-addon-kubernetes/helm-charts) bakın.

     > [!NOTE]
     > Azure İlkesi Eklentisi ile kaynak grubu kimliği arasındaki ilişki nedeniyle, Azure İlkesi her kaynak grubu için yalnızca bir AKS Altyapısı kümesini destekler.

Eklenti yüklemesinin başarılı olduğunu ve azure _ilkesi_ bölmesinin çalıştığını doğrulamak için aşağıdaki komutu çalıştırın:

```bash
kubectl get pods -n kube-system
```

### <a name="validation-and-reporting-frequency"></a>Doğrulama ve raporlama sıklığı

Eklenti, ilke atamalarındaki değişiklikler için her 5 dakikada bir Azure İlkesi ile birlikte giriş yaptı. Bu yenileme döngüsü sırasında, eklenti değişiklikleri denetler. Bu değişiklikler, kısıtlama şablonlarını ve kısıtlamaları oluşturur, güncelleştirir veya siler.

> [!NOTE]
> Küme _yöneticisi_ kısıtlama şablonlarında ve kısıtlamalarında değişiklik yapma iznine sahip olsa da, Azure İlkesi tarafından oluşturulan kısıtlama şablonlarında veya kısıtlamalarında değişiklik yapmak önerilmez veya desteklenmez. Yapılan tüm el ile değişiklikler yenileme döngüsü sırasında kaybolur.

Her 5 dakikada bir, eklenti kümenin tam taramayı çağırır. Tam taramanın ayrıntılarını ve Gatekeeper tarafından kümedeki değişiklik girişimlerinin gerçek zamanlı değerlendirmelerini topladıktan sonra, eklenti sonuçları herhangi bir Azure İlkesi ataması gibi [uyumluluk ayrıntılarına](../how-to/get-compliance-data.md) dahil etmek üzere Azure İlkesi'ne geri bildirir. Yalnızca etkin ilke atamalarının sonuçları denetim döngüsü sırasında döndürülür. Denetim sonuçları, başarısız kısıtlamanın durum alanında listelenen ihlaller olarak da görülebilir.

## <a name="policy-language"></a>İlke dili

AKS Engine'i yönetmek için Azure İlkesi dil yapısı, varolan ilkeleri izler. _EnforceOPAConstraint_ etkisi AKS Motor kümelerinizi yönetmek için kullanılır ve [OPA Constraint Framework](https://github.com/open-policy-agent/frameworks/tree/master/constraint) ve Gatekeeper v3 ile çalışmaya özel _ayrıntılar_ özelliklerini alır. Ayrıntılar ve örnekler için [EnforceOPAConstraint efektine](effects.md#enforceopaconstraint) bakın.

_Details.constraintTemplate_ ve _details.constraint_ özelliklerinin bir parçası olarak, Azure İlkesi bu [Özel Kaynak Tanımlarının](https://github.com/open-policy-agent/gatekeeper#constraint-templates) (CRD) UrI'lerini eklentiye geçirir. Rego, OPA ve Gatekeeper'ın Kubernetes kümesine bir isteği doğrulamak için desteklediği dildir. Azure İlkesi, Kubernetes yönetimi için varolan bir standardı destekleyerek, varolan kuralları yeniden kullanmanın ve birleşik bir bulut uyumluluğu raporlama deneyimi için Azure İlkesi ile eşleştirmeyi mümkün kılar. Daha fazla bilgi için [bkz: Rego nedir?](https://www.openpolicyagent.org/docs/how-do-i-write-policies.html#what-is-rego)

## <a name="built-in-policies"></a>Yerleşik ilkeler

Azure portalını kullanarak AKS Engine kümenizi yönetmeye yönelik yerleşik ilkeleri bulmak için aşağıdaki adımları izleyin:

1. Azure Portalı'nda Azure İlkesi hizmetini başlatın. Sol bölmedeki **Tüm hizmetleri** seçin ve ardından **İlke'yi**arayın ve seçin.

1. Azure İlkesi sayfasının sol bölmesinde **Tanımlar'ı**seçin.

1. Kategori açılır liste kutusundan, filtreyi temizlemek için **Tümünü Seç'i** kullanın ve ardından **Kubernetes'i**seçin.

1. İlke tanımını seçin ve ardından **Atla** düğmesini seçin.

> [!NOTE]
> AKS Engine tanımı için Azure İlkesi atarken, **Kapsam** AKS Engine kümesinin kaynak grubu olmalıdır.

Alternatif olarak, bir AKS Engine ilkesi bulmak ve atamak için [bir ilke ata- Portal](../assign-policy-portal.md) quickstart kullanın. Örnek 'denetim vms' yerine bir AKS Altyapısı ilkesi tanımı arayın.

> [!IMPORTANT]
> **Kategori Kubernetes** yerleşik ilkeler sadece AKS Engine ile kullanım içindir.

## <a name="logging"></a>Günlüğe Kaydetme

### <a name="azure-policy-add-on-logs"></a>Azure İlkesi Eklenti günlükleri

Bir Kubernetes denetleyicisi/kapsayıcısı olarak Azure İlkesi Eklentisi aks engine kümesinde günlük tutar.

Azure İlkesi Eklenti günlüklerini görüntülemek `kubectl`için şunları kullanın:

```bash
# Get the Azure Policy Add-on pod name
kubectl -n kube-system get pods -l app=azure-policy --output=name

# Get the logs for the add-on
kubectl logs <Azure Policy Add-on pod name> -n kube-system
```

### <a name="gatekeeper-logs"></a>Gatekeeper günlükleri

Gatekeeper pod, _gatekeeper-controller-manager-0_, genellikle `gatekeeper-system` `kube-system` veya ad alanında, ancak nasıl dağıtıldı bağlı olarak farklı bir ad alanında olabilir.

Gatekeeper günlüklerini görüntülemek için `kubectl`şunları kullanın:

```bash
NAMESPACE=<namespace of gatekeeper>
kubectl logs gatekeeper-controller-manager-0 -n $NAMESPACE
```

Daha fazla bilgi için OPA belgelerinde [Hata Ayıklama Gatekeeper'a](https://github.com/open-policy-agent/gatekeeper#debugging) bakın.

## <a name="remove-the-add-on"></a>Eklentiyi kaldırma

Azure İlkesi Eklentisi ve Gatekeeper'ı AKS Engine kümenizden kaldırmak için, eklentinin nasıl yüklenmiş olduğuyla uyumlu yöntemi kullanın:

- AKS Engine için küme tanımında **addons** özelliği ayarlayarak yüklü ise:

  _Azure ilkesi_ için **addons** özelliğini false olarak değiştirdikten sonra küme tanımını AKS Engine'e yeniden dağıtın:


  ```json
  "addons": [{
      "name": "azure-policy",
      "enabled": false
  }]
  ```

- Miğfer Grafikleri ile yüklüyse:

  1. Eski kısıtlamaları kaldırma

     Şu anda kaldırma mekanizması sadece Gatekeeper sistemi kaldırır, herhangi bir _ConstraintTemplate_kaldırmaz , _Kısıtlama_, veya kullanıcı tarafından oluşturulan _Config_ kaynakları, ne de eşlik eden _CRDs_kaldırmaz .

     Gatekeeper çalışırken, istenmeyen kısıtlamaları şu şekilde kaldırmak mümkündür:

     - Kısıtlama kaynağının tüm örneklerini silme
     - _CRD'yi_ otomatik olarak temizlemesi gereken _ConstraintTemplate_ kaynağını silme
     - _Config_ kaynağının silmesi, senkronize edilmiş kaynaklardaki sonlandırıcıları kaldırır

  1. Azure İlkesi Eklentisi Eklentisi'ni kaldırma
  
     ```bash
     helm del --purge azure-policy-addon
     ```

  1. Gatekeeper'ı kaldır
  
     ```bash
     kubectl delete -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/master/deploy/gatekeeper.yaml
     ```

## <a name="diagnostic-data-collected-by-azure-policy-add-on"></a>Azure İlkesi Eklentisi tarafından toplanan tanılama verileri

Kubernetes için Azure İlke Eklentisi sınırlı küme tanılama verileri toplar. Bu tanısal veriler yazılım ve performansla ilgili hayati önem taşıyan teknik verilerdir. Aşağıdaki şekillerde kullanılır:

- Azure İlkesi Eklentisini güncel tutun
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