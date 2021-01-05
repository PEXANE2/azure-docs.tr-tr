---
title: Sık karşılaşılan hataları giderme
description: İlke tanımları, çeşitli SDK ve Kubernetes için eklenti oluşturma sorunlarını giderme hakkında bilgi edinin.
ms.date: 12/01/2020
ms.topic: troubleshooting
ms.openlocfilehash: b88d00575adb571c59b562d25067c4a1716fb50f
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97882985"
---
# <a name="troubleshoot-errors-using-azure-policy"></a>Azure Ilkesini kullanarak hatalarda sorun giderme

İlke tanımları oluştururken, SDK ile çalışırken veya [Kubernetes eklentisi Için Azure ilkesini](../concepts/policy-for-kubernetes.md) ayarlarken hatalarla karşılaşabilirsiniz. Bu makalede, oluşabilecek çeşitli genel hatalar ve bunların nasıl çözümleneceği açıklanır.

## <a name="finding-error-details"></a>Hata ayrıntılarını bulma

Hata ayrıntılarının konumu hataya neden olan eyleme bağlıdır.

- Özel bir ilkeyle çalışırken, şema hakkında geri bildirim almak veya kaynakların nasıl değerlendirildiğini görmek üzere sonuç [uyumluluk verilerini](../how-to/get-compliance-data.md) gözden geçirmek için Azure Portal deneyin.
- SDK, çeşitli SDK ile çalışırken işlevin neden başarısız olduğuna ilişkin ayrıntıları sağlar.
- Kubernetes eklentisi ile çalışırken, kümede [oturum açma](../concepts/policy-for-kubernetes.md#logging) ile başlayın.

## <a name="general-errors"></a>Genel hatalar

### <a name="scenario-alias-not-found"></a>Senaryo: diğer ad bulunamadı

#### <a name="issue"></a>Sorun

Azure Ilkesi Azure Resource Manager özellikleriyle eşlemek için [takma adlar](../concepts/definition-structure.md#aliases) kullanır.

#### <a name="cause"></a>Nedeni

İlke tanımında yanlış veya varolmayan diğer ad kullanılıyor.

#### <a name="resolution"></a>Çözüm

İlk olarak, Kaynak Yöneticisi özelliğinin bir diğer adı olduğunu doğrulayın. Kullanılabilir diğer adları aramak için Visual Studio Code veya SDK [Için Azure ilke uzantısı](../how-to/extension-for-vscode.md) 'nı kullanın. Bir Kaynak Yöneticisi özelliği için diğer ad yoksa, bir destek bileti oluşturun.

### <a name="scenario-evaluation-details-not-up-to-date"></a>Senaryo: değerlendirme ayrıntıları güncel değil

#### <a name="issue"></a>Sorun

Kaynak "başlatılmadı" durumunda veya uyumluluk ayrıntıları güncel değil.

#### <a name="cause"></a>Nedeni

Yeni bir ilke veya girişim atamasının uygulanması yaklaşık 30 dakika sürer. Mevcut bir atamanın kapsamındaki yeni veya güncelleştirilmiş kaynaklar 15 dakika sonra kullanılabilir hale gelir. Her 24 saatte bir standart uyumluluk taraması yapılır. Daha fazla bilgi için bkz. [değerlendirme Tetikleyicileri](../how-to/get-compliance-data.md#evaluation-triggers).

#### <a name="resolution"></a>Çözüm

İlk olarak, bir değerlendirmenin tamamlanmasını ve uyumluluk sonuçlarının Azure portal veya SDK 'da kullanılabilir hale gelmesi için uygun süreyi bekleyin. Azure PowerShell veya REST API ile yeni bir değerlendirme taraması başlatmak için bkz. [isteğe bağlı değerlendirme taraması](../how-to/get-compliance-data.md#on-demand-evaluation-scan).

### <a name="scenario-compliance-not-as-expected"></a>Senaryo: uyumluluk beklendiği gibi değil

#### <a name="issue"></a>Sorun

Kaynak, bu kaynak için beklenen, _uyumlu_ veya _uyumsuz_ olan değerlendirme durumunda değil.

#### <a name="cause"></a>Nedeni

Kaynak, ilke atamasının doğru kapsamında değil veya ilke tanımı istenen şekilde çalışmıyor.

#### <a name="resolution"></a>Çözüm

İlke tanımınızda sorun gidermek için aşağıdaki adımları izleyin:

1. İlk olarak, bir değerlendirmenin tamamlanmasını ve uyumluluk sonuçlarının Azure portal veya SDK 'da kullanılabilir hale gelmesi için uygun süreyi bekleyin. Azure PowerShell veya REST API ile yeni bir değerlendirme taraması başlatmak için bkz. [isteğe bağlı değerlendirme taraması](../how-to/get-compliance-data.md#on-demand-evaluation-scan).
1. Atama parametrelerinin ve atama kapsamının doğru şekilde ayarlandığından emin olun.
1. [İlke tanımı modunu](../concepts/definition-structure.md#mode) denetleyin:
   - Tüm kaynak türleri için ' All ' modu.
   - İlke tanımı etiketleri veya konumu denetlediğinde ' dizinli ' modu.
1. Kaynak kapsamının [dışlandığından](../concepts/assignment-structure.md#excluded-scopes) veya [muaf](../concepts/exemption-structure.md)olmadığından emin olun.
1. Bir ilke atamasının uyumluluğu `0/0` kaynakları gösteriyorsa, atama kapsamında geçerli olacak hiçbir kaynak belirlenmemiştir. Hem ilke tanımını hem de atama kapsamını denetleyin.
1. Uyumlu olması beklenen uyumlu olmayan bir kaynak için, [uyumsuzluk nedenlerini belirlemek](../how-to/determine-non-compliance.md)için denetleyin. Tanımın değerlendirilen Özellik değeri ile karşılaştırılması, kaynağın neden uyumlu olmadığını gösterir.
   - **Hedef değer** yanlış ise, ilke tanımını gözden geçirin.
   - **Geçerli değer** yanlışsa kaynak yükünü ile doğrulayın `resources.azure.com` .
1. Sorun gidermeyi denetle: diğer yaygın sorunlar ve çözümler için [zorlama beklendiği gibi değil](#scenario-enforcement-not-as-expected) .

Yinelenen ve özelleştirilmiş yerleşik ilke tanımı veya özel tanımınızda sorun yaşıyorsanız, sorunu doğru bir şekilde yönlendirmek için **Ilke yazma** altında bir destek bileti oluşturun.

### <a name="scenario-enforcement-not-as-expected"></a>Senaryo: zorlama beklendiği gibi değil

#### <a name="issue"></a>Sorun

Azure Ilkesi tarafından işlem yapılması beklenen bir kaynak değildir ve [Azure etkinlik günlüğünde](../../../azure-monitor/platform/platform-logs-overview.md)giriş yoktur.

#### <a name="cause"></a>Nedeni

İlke ataması, _Disabled_'ın [Enforcementmode](../concepts/assignment-structure.md#enforcement-mode) için yapılandırılmış. Zorlama modu devre dışı olsa da, ilke efekti zorlanmaz ve etkinlik günlüğünde giriş yoktur.

#### <a name="resolution"></a>Çözüm

İlke atamalarınızın zorlanmasıyla ilgili sorunları gidermek için aşağıdaki adımları izleyin:

1. İlk olarak, bir değerlendirmenin tamamlanmasını ve uyumluluk sonuçlarının Azure portal veya SDK 'da kullanılabilir hale gelmesi için uygun süreyi bekleyin. Azure PowerShell veya REST API ile yeni bir değerlendirme taraması başlatmak için bkz. [isteğe bağlı değerlendirme taraması](../how-to/get-compliance-data.md#on-demand-evaluation-scan).
1. Atama parametrelerinin ve atama kapsamının doğru ayarlandığından ve **Enforcementmode** 'un _etkin_ olduğundan emin olun.
1. [İlke tanımı modunu](../concepts/definition-structure.md#mode) denetleyin:
   - Tüm kaynak türleri için ' All ' modu.
   - İlke tanımı etiketleri veya konumu denetlediğinde ' dizinli ' modu.
1. Kaynak kapsamının [dışlandığından](../concepts/assignment-structure.md#excluded-scopes) veya [muaf](../concepts/exemption-structure.md)olmadığından emin olun.
1. Kaynak yükünün ilke mantığıyla eşleştiğini doğrulayın. Bu işlem, [BIR har izlemesi yakalanarak](../../../azure-portal/capture-browser-trace.md) veya ARM şablon özelliklerini inceleyerek yapılabilir.
1. Sorun gidermeyi denetle: diğer yaygın sorunlar ve çözümler için [Uyumluluk beklendiği gibi değil](#scenario-compliance-not-as-expected) .

Yinelenen ve özelleştirilmiş yerleşik ilke tanımı veya özel tanımınızda sorun yaşıyorsanız, sorunu doğru bir şekilde yönlendirmek için **Ilke yazma** altında bir destek bileti oluşturun.

### <a name="scenario-denied-by-azure-policy"></a>Senaryo: Azure Ilkesi tarafından reddedildi

#### <a name="issue"></a>Sorun

Bir kaynağın oluşturulması veya güncelleştirilmesi reddedildi.

#### <a name="cause"></a>Nedeni

Yeni veya güncelleştirilmiş kaynağınızın kapsamına yönelik bir ilke ataması, bir ilke tanımının bir [reddetme](../concepts/effects.md#deny) etkisi ile ölçütlerine sahip olduğunu karşılar. Bu tanımların toplantılarının oluşturulması veya güncelleştirilmesini engellenir.

#### <a name="resolution"></a>Çözüm

Bir reddetme ilke atamasından alınan hata iletisi, ilke tanımı ve ilke atama kimliklerini içerir. İletideki hata bilgileri kaçırıldığında, [etkinlik günlüğünde](../../../azure-monitor/platform/activity-log.md#view-the-activity-log)da kullanılabilir. Bu bilgileri, kaynak kısıtlamalarını anlamak ve izin verilen değerleri eşlemek için istekteki kaynak özelliklerini ayarlamak üzere daha fazla bilgi almak için kullanın.

## <a name="template-errors"></a>Şablon hataları

### <a name="scenario-policy-supported-functions-processed-by-template"></a>Senaryo: şablon tarafından işlenen Ilke tarafından desteklenen işlevler

#### <a name="issue"></a>Sorun

Azure Ilkesi, bir dizi Azure Resource Manager şablonu (ARM şablonu) işlevini ve yalnızca bir ilke tanımında bulunan işlevleri destekler. Kaynak Yöneticisi, bu işlevleri bir ilke tanımının bir parçası yerine bir dağıtımın parçası olarak işler.

#### <a name="cause"></a>Nedeni

Veya gibi desteklenen işlevleri kullanarak `parameter()` `resourceGroup()` , ilke tanımı ve Azure ilke altyapısının işlevini işlemek yerine, dağıtım sırasında işlevin işlenen sonucuna neden olur.

#### <a name="resolution"></a>Çözüm

Bir işlevi bir ilke tanımının parçası olacak şekilde geçirmek için, tüm dizeyi, özelliği gibi görünecek şekilde kaçış `[` `[[resourceGroup().tags.myTag]` . Kaçış karakteri, şablonu işlerken Kaynak Yöneticisi değeri bir dize olarak işlemesine neden olur. Daha sonra Azure Ilkesi, işlevin beklenen şekilde dinamik olmasını sağlayan ilkeyi ilke tanımına koyar. Daha fazla bilgi için bkz. [Azure Resource Manager şablonlarındaki sözdizimi ve ifadeler](../../../azure-resource-manager/templates/template-expressions.md).

## <a name="add-on-for-kubernetes-installation-errors"></a>Kubernetes yükleme hataları için eklenti

### <a name="scenario-install-using-helm-chart-fails-on-password"></a>Senaryo: parola üzerinde Held grafik kullanarak Install başarısız oluyor

#### <a name="issue"></a>Sorun

`helm install azure-policy-addon`Komut aşağıdaki iletilerden biriyle başarısız olur:

- `!: event not found`
- `Error: failed parsing --set data: key "<key>" has no value (cannot end with ,)`

#### <a name="cause"></a>Nedeni

Oluşturulan parola, `,` helb grafiğinin bölünmesi için bir virgül () içerir.

#### <a name="resolution"></a>Çözüm

`,` `helm install azure-policy-addon` Bir ters eğik çizgiyle () çalışırken parola değerindeki virgül () karakterini kaçış `\` .

### <a name="scenario-install-using-helm-chart-fails-as-name-already-exists"></a>Senaryo: ad zaten mevcut olduğu için Held grafiğini kullanarak Install başarısız olur

#### <a name="issue"></a>Sorun

`helm install azure-policy-addon`Komut aşağıdaki iletiyle başarısız olur:

- `Error: cannot re-use a name that is still in use`

#### <a name="cause"></a>Nedeni

Ada sahip Held grafiği `azure-policy-addon` zaten yüklenmiş veya kısmen yüklenmiş.

#### <a name="resolution"></a>Çözüm

[Kubernetes eklentisinin Azure ilkesini kaldırmak](../concepts/policy-for-kubernetes.md#remove-the-add-on)için yönergeleri izleyin, sonra komutu yeniden çalıştırın `helm install azure-policy-addon` .

### <a name="scenario-azure-virtual-machine-user-assigned-identities-are-replaced-by-system-assigned-managed-identities"></a>Senaryo: Azure sanal makinesi Kullanıcı tarafından atanan kimlikler, sistem tarafından atanan Yönetilen kimlikler ile değiştirilmiştir

#### <a name="issue"></a>Sorun

Konuk yapılandırma ilkesi girişimlerini makineler içindeki denetim ayarlarına atadıktan sonra, makineye atanan kullanıcı tarafından atanan Yönetilen kimlikler artık atanmaz. Yalnızca sistem tarafından atanan yönetilen kimlik atanır.

#### <a name="cause"></a>Nedeni

Konuk yapılandırma DeployIfNotExists tanımlarında daha önce kullanılan ilke tanımları, sistem tarafından atanan kimliğin makineye atandığını, ancak kullanıcı tarafından atanan kimlik atamalarını da kaldırmış olduğunu ister.

#### <a name="resolution"></a>Çözüm

Daha önce Bu soruna neden olan tanımlar \[ kullanım dışı olarak görünür \] ve Kullanıcı tarafından atanan yönetilen kimlik kaldırılmadan önkoşulları yöneten ilke tanımlarına göre değiştirilmiştir. El ile bir adım gereklidir. Kullanım dışı olarak işaretlenen tüm mevcut ilke atamalarını \[ silin \] ve bunları orijinalle aynı ada sahip güncelleştirilmiş önkoşul ilkesi girişimi ve ilke tanımlarıyla değiştirin.

Ayrıntılı bir anlatım için aşağıdaki blog gönderisine bakın:

[Konuk yapılandırma denetim ilkeleri için önemli değişiklik yayınlandı](https://techcommunity.microsoft.com/t5/azure-governance-and-management/important-change-released-for-guest-configuration-audit-policies/ba-p/1655316)

## <a name="add-on-for-kubernetes-general-errors"></a>Kubernetes genel hataları için eklenti

### <a name="scenario-add-on-is-unable-to-reach-the-azure-policy-service-endpoint-due-to-egress-restrictions"></a>Senaryo: eklenti, çıkış kısıtlamaları nedeniyle Azure Ilke hizmeti uç noktasına ulaşamıyor

#### <a name="issue"></a>Sorun

Eklenti Azure Ilke hizmeti uç noktasına ulaşamıyor ve aşağıdaki hatalardan birini döndürür:

- `failed to fetch token, service not reachable`
- `Error getting file "Get https://raw.githubusercontent.com/Azure/azure-policy/master/built-in-references/Kubernetes/container-allowed-images/template.yaml: dial tcp 151.101.228.133.443: connect: connection refused`

#### <a name="cause"></a>Nedeni

Bu sorunlar, bir küme çıkışı kilitlendiğinde oluşur.

#### <a name="resolution"></a>Çözüm

Aşağıdaki makalelerdeki etki alanlarının ve bağlantı noktalarının açık olduğundan emin olun:

- [AKS kümeleri için gerekli giden ağ kuralları ve FQDN 'Ler](../../../aks/limit-egress-traffic.md#required-outbound-network-rules-and-fqdns-for-aks-clusters)
- [Azure Arc etkin Kubernetes için Azure Policy eklentisini yükler (Önizleme)](../concepts/policy-for-kubernetes.md#install-azure-policy-add-on-for-azure-arc-enabled-kubernetes)

### <a name="scenario-add-on-is-unable-to-reach-the-azure-policy-service-endpoint-due-to-aad-pod-identity-configuration"></a>Senaryo: eklenti, AAD-Pod kimlik yapılandırması nedeniyle Azure Ilke hizmeti uç noktasına ulaşamıyor

#### <a name="issue"></a>Sorun

Eklenti Azure Ilke hizmeti uç noktasına ulaşamıyor ve aşağıdaki hatalardan birini döndürür:

- `azure.BearerAuthorizer#WithAuthorization: Failed to refresh the Token for request to https://gov-prod-policy-data.trafficmanager.net/checkDataPolicyCompliance?api-version=2019-01-01-preview: StatusCode=404`
- `adal: Refresh request failed. Status Code = '404'. Response body: getting assigned identities for pod kube-system/azure-policy-8c785548f-r882p in CREATED state failed after 16 attempts, retry duration [5]s, error: <nil>`

#### <a name="cause"></a>Nedeni

Bu hata, kümeye _Add-Pod kimliği_ yüklendiğinde ve _Kuto-System_ 'ların _AAD-Pod kimliği_'nde dışlanmadığı durumlarda oluşur.

_AAD-Pod kimliği_ bileşen düğümü yönetilen kimliği (NMI) Pod, Azure örnek meta veri uç noktası çağrılarını ele almak için düğümlerin Iptables 'larını değiştirir. Bu kurulum, Pod 'ın _AAD-Pod kimliği_ kullanmasa bile meta veri uç noktasına yapılan tüm istekler nmi tarafından ele getirilir anlamına gelir.
**AzurePodIdentityException** CRD, _AAD_ 'de tanımlanan etiketlerle eşleşen bir pod 'dan kaynaklanan meta veri uç noktası isteklerinin, NMI içinde herhangi bir işlem yapılmadan proxy olması gerektiğini bildirmek üzere yapılandırılabilir.

#### <a name="resolution"></a>Çözüm

`kubernetes.azure.com/managedby: aks` **AzurePodIdentityException** CRD 'yi yapılandırarak _AAD-Pod-identity_ içindeki _kuin-System_ ad alanındaki etiketli sistem yığınlarını hariç tutun.

Daha fazla bilgi için bkz. [belirli bir pod/uygulama IÇIN AAD Pod kimliğini devre dışı bırakma](https://azure.github.io/aad-pod-identity/docs/configure/application_exception).

Bir özel durum yapılandırmak için aşağıdaki örneğe bakın:

```yaml
apiVersion: "aadpodidentity.k8s.io/v1"
kind: AzurePodIdentityException
metadata:
  name: mic-exception
  namespace: default
spec:
  podLabels:
    app: mic
    component: mic
---
apiVersion: "aadpodidentity.k8s.io/v1"
kind: AzurePodIdentityException
metadata:
  name: aks-addon-exception
  namespace: kube-system
spec:
  podLabels:
    kubernetes.azure.com/managedby: aks
```

### <a name="scenario-the-resource-provider-isnt-registered"></a>Senaryo: kaynak sağlayıcı kayıtlı değil

#### <a name="issue"></a>Sorun

Eklenti Azure Ilke hizmeti uç noktasına ulaşabilir, ancak eklenti günlüklerinde aşağıdaki hatalardan birini görür:

```
The resource provider 'Microsoft.PolicyInsights' is not registered in subscription '{subId}'. See
https://aka.ms/policy-register-subscription for how to register subscriptions.
```

veya

```
policyinsightsdataplane.BaseClient#CheckDataPolicyCompliance: Failure responding to request:
StatusCode=500 -- Original Error: autorest/azure: Service returned an error. Status=500
Code="InternalServerError" Message="Encountered an internal server error."
```

#### <a name="cause"></a>Nedeni

`Microsoft.PolicyInsights`Kaynak sağlayıcısı kayıtlı değil ve ilke tanımlarını almak ve uyumluluk verilerini döndürmek için eklenti için kayıtlı olması gerekir.

#### <a name="resolution"></a>Çözüm

`Microsoft.PolicyInsights`Kaynak sağlayıcısını küme aboneliğine kaydedin. Yönergeler için bkz. [kaynak sağlayıcısını kaydetme](../../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider).

### <a name="scenario-the-subscription-is-disabled"></a>Senaryo: abonelik devre dışı

#### <a name="issue"></a>Sorun

Eklenti Azure Ilke hizmeti uç noktasına ulaşabilir, ancak şu hatayı görür:

```
The subscription '{subId}' has been disabled for azure data-plane policy. Please contact support.
```

#### <a name="cause"></a>Nedeni

Bu hata, aboneliğin sorunlu olduğu belirlenen ve aboneliği engelleyecek Özellik bayrağının eklendiği anlamına gelir `Microsoft.PolicyInsights/DataPlaneBlocked` .

#### <a name="resolution"></a>Çözüm

`azuredg@microsoft.com`Bu sorunu araştırmak ve çözmek için özellik ekibine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- [Microsoft Q&A](/answers/topics/azure-policy.html)aracılığıyla uzmanlardan yanıt alın.
- [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.
- Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
