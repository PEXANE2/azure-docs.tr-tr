---
title: Yay etkin Kubernetes kümesinde Gilar kullanarak yapılandırma dağıtma (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Azure Arc etkin küme yapılandırması (Önizleme) için Gilar 'ı kullanma
keywords: Giüstler, Kubernetes, K8s, Azure, Arc, Azure Kubernetes hizmeti, kapsayıcılar
ms.openlocfilehash: e25fdf3a51b3e9264c85707df31d3a4d107b25ea
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87049964"
---
# <a name="deploy-configurations-using-gitops-on-arc-enabled-kubernetes-cluster-preview"></a>Yay etkin Kubernetes kümesinde Gilar kullanarak yapılandırma dağıtma (Önizleme)

Gila, bir git deposunda istenen Kubernetes yapılandırmasının (dağıtımlar, ad alanları vb.) ve ardından bu yapılandırmaların bir operatör kullanılarak kümeye bir yoklama ve çekme temelli dağıtımı ile ilgili olduğunu bildiren bir uygulamadır. Bu belge, Azure Arc etkin Kubernetes kümelerinde bu tür iş akışlarının kurulumunu içerir.

Kümeniz ve bir veya daha fazla git deposu arasındaki bağlantı, Azure Resource Manager ' de `sourceControlConfiguration` uzantı kaynağı olarak izlenir. `sourceControlConfiguration`Kaynak özellikleri, Kubernetes kaynaklarının git 'ten kümenize nasıl akmasını gerektiğini temsil eder. Veriler, `sourceControlConfiguration` verilerin gizliliğini sağlamak için bir Azure Cosmos DB veritabanında şifreli olarak depolanır.

`config-agent`Kümenizde çalışan, `sourceControlConfiguration` Azure Arc etkin Kubernetes kaynağında yeni veya güncelleştirilmiş uzantı kaynaklarını Izlerken, git deposunu izlemek için Flox operatörü dağıtmaktan ve üzerinde yapılan tüm güncelleştirmeleri yaymaktan sorumludur `sourceControlConfiguration` . `sourceControlConfiguration` `namespace` Çok kiracılı hale ulaşmak Için aynı Azure Arc etkinleştirilmiş Kubernetes kümesi kapsamında birden fazla kaynak oluşturmak da mümkündür. Böyle bir durumda, her operatör yalnızca ilgili ad alanına yapılandırma dağıtabilir.

Git deposu, ad alanları, ConfigMaps, dağıtımlar, DaemonSets vb. dahil olmak üzere geçerli bir Kubernetes kaynağı içerebilir.  Ayrıca, uygulamaları dağıtmak için Held grafikleri de içerebilir. Yaygın bir senaryo kümesi, kuruluşunuz için ortak RBAC rolleri ve bağlamaları, izleme veya günlüğe kaydetme aracılarını veya küme genelinde Hizmetleri içerebilen temel bir yapılandırma tanımlamayı içerir.

Aynı model, heterojen ortamlar arasında dağıtılabilen daha büyük bir küme koleksiyonunu yönetmek için kullanılabilir. Örneğin, kuruluşunuz için taban çizgisi yapılandırmasını tanımlayan ve aynı anda onlarca Kubernetes kümesine uygulayabilen bir havuzunuz olabilir. [Azure ilkesi](use-azure-policy.md) , bir `sourceControlConfiguration` kapsam (abonelik veya kaynak grubu) altındaki tüm Azure Arc etkin Kubernetes kaynaklarında belirli bir parametre kümesiyle bir oluşturma işlemini otomatikleştirebilir.

Bu başlangıç kılavuzu, Küme Yöneticisi kapsamına sahip bir yapılandırma kümesi uygularken size kılavuzluk eder.

## <a name="create-a-configuration"></a>Yapılandırma oluşturma

- Örnek depo:<https://github.com/Azure/arc-k8s-demo>

Örnek depo, birkaç ad alanı sağlamak, ortak bir iş yükü dağıtmak ve takıma özgü bazı yapılandırmalar sağlamak isteyen bir küme operatörü etrafında yapılandırılır. Bu depoyu kullanmak, kümenizde aşağıdaki kaynakları oluşturur:

**Ad alanları:** `cluster-config` , `team-a` , `team-b` 
 **Dağıtım:** `cluster-config/azure-vote` 
 **configmap:**`team-a/endpoints`

Yeni `config-agent` `sourceControlConfiguration` `config-agent` veya güncelleştirilmiş bir yapılandırma almak için en fazla geçen süre olan 30 saniyede bir Azure 'u yeni veya güncelleştirilmiş olarak yoklar.
Özel bir depoyu ile ilişkilendirirken `sourceControlConfiguration` , [özel bir git deposundan yapılandırma uygulama](#apply-configuration-from-a-private-git-repository)adımlarını da tamamlatığınızdan emin olun.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

İçin Azure CLı uzantısını kullanarak `k8sconfiguration` , bağlantılı kümemizi [örnek bir git deposuna](https://github.com/Azure/arc-k8s-demo)bağlayalim. Bu yapılandırmaya bir ad vereceğiz `cluster-config` , aracıyı `cluster-config` ad alanına dağıtmasını ve operatör izinleri vermesini sağlıyoruz `cluster-admin` .

```console
az k8sconfiguration create \
    --name cluster-config \
    --cluster-name AzureArcTest1 --resource-group AzureArcTest \
    --operator-instance-name cluster-config --operator-namespace cluster-config \
    --repository-url https://github.com/Azure/arc-k8s-demo \
    --scope cluster --cluster-type connectedClusters
```

**Çıktıların**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "ComplianceStatus": 1,
    "clientAppliedTime": null,
    "level": 3,
    "message": "{\"OperatorMessage\":null,\"ClusterState\":null}"
  },
  "configKind": "Git",
  "configName": "cluster-config",
  "configOperator": {
    "operatorInstanceName": "cluster-config",
    "operatorNamespace": "cluster-config",
    "operatorParams": "--git-readonly",
    "operatorScope": "cluster",
    "operatorType": "flux"
  },
  "configType": "",
  "id": null,
  "name": null,
  "operatorInstanceName": null,
  "operatorNamespace": null,
  "operatorParams": null,
  "operatorScope": null,
  "operatorType": null,
  "providerName": "ConnectedClusters",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": null,
  "repositoryUrl": null,
  "sourceControlConfiguration": {
    "repositoryPublicKey": "",
    "repositoryUrl": "git://github.com/Azure/arc-k8s-demo.git"
  },
  "type": null
}
```

#### <a name="repository-url-parameter"></a>Depo-URL parametresi

--Repository-URL parametresi değeri için desteklenen senaryolar aşağıda verilmiştir.

| Senaryo | Biçimlendir | Açıklama |
| ------------- | ------------- | ------------- |
| Özel GitHub deposu-SSH | git@github.com:username/repo | Flox tarafından oluşturulan SSH anahtar çifti.  Kullanıcının GitHub hesabına ortak anahtarı dağıt anahtarı olarak eklemesi gerekir. |
| Genel GitHub deposu | `http://github.com/username/repo`or git://github.com/username/repo   | Genel git deposu  |

Bu senaryolar, akıcı x tarafından desteklenir, ancak sourceControlConfiguration tarafından desteklenmez. 

| Senaryo | Biçimlendir | Açıklama |
| ------------- | ------------- | ------------- |
| Özel GitHub deposu-HTTPS | `https://github.com/username/repo` | Flox SSH anahtar çifti oluşturmaz.  [Yönergeler](https://docs.fluxcd.io/en/1.17.0/guides/use-git-https.html) |
| Özel Git Konağı | user@githost:path/to/repo | [Yönergeler](https://docs.fluxcd.io/en/1.18.0/guides/use-private-git-host.html) |
| Özel GitHub deposu-SSH (kendi anahtarını getir) | git@github.com:username/repo | [Kendi SSH KeyPair kullanın](https://docs.fluxcd.io/en/1.17.0/guides/provide-own-ssh-key.html) |


#### <a name="additional-parameters"></a>Ek parametreler

Yapılandırma oluşturmayı özelleştirmek için birkaç ek parametre aşağıda verilmiştir:

`--enable-helm-operator`: Hele grafik dağıtımları desteğini etkinleştirmek için *Isteğe bağlı* anahtar.

`--helm-operator-chart-values`: Hele işleci için *Isteğe bağlı* grafik değerleri (etkinse).  Örneğin, '--Held. Versions = v3 ' olarak ayarlayın.

`--helm-operator-chart-version`: Held işleci için *Isteğe bağlı* grafik sürümü (etkinse). Varsayılan: ' 0.6.0 '.

`--operator-namespace`: İşleç ad alanı için *Isteğe bağlı* ad. Varsayılan: ' varsayılan '

`--operator-params`: İşleç için *Isteğe bağlı* parametreler. Tek tırnak içinde verilmelidir. Örneğin, ```--operator-params='--git-readonly --git-path=releases' ```

--İşleci-params içinde desteklenen seçenekler

| Seçenek | Açıklama |
| ------------- | ------------- |
| --Git-dal  | Kubernetes bildirimleri için kullanılacak git deposunun dalı. Varsayılan değer ' Master '. |
| --Git-yol  | Kubernetes bildirimlerini bulmak için Flox için Git deposu içindeki göreli yol. |
| --Git-ReadOnly | Git deposu salt okunurdur, Flox buna yazmaya çalışmaz. |
| --bildirim oluşturma  | Etkinleştirilirse, Flox. Flox. YAML 'yi arayacaktır ve Kustomize veya diğer bildirim oluşturucularını çalıştırır. |
| --Git-yoklama-Interval  | Yeni işlemeler için git deposunun yoklamak için gereken süre. Varsayılan değer ' 5dk ' (5 dakikadır). |
| --Sync-çöp toplama  | Etkinleştirilirse, Flox oluşturduğu kaynakları silecek, ancak artık git içinde mevcut değil. |
| --Git-etiket  | Git dalını etiketlemek için kullanılan eşitleme ilerlemesini izlemenin etiketi.  Varsayılan değer ' Flox-Sync '. |
| --Git-Kullanıcı  | Git yürütmesi için Kullanıcı adı. |
| --Git-e-posta  | Git yürütmesi için kullanılacak e-posta. |

* '--Git-User ' veya '--git-email ' ayarlanmamışsa (Bu, akıcı x 'in depoya yazmasını istemediğiniz anlamına gelir), sonra--git-ReadOnly otomatik olarak ayarlanır (henüz ayarlanmamışsa).

* EnableHelmOperator değeri true ise, operatorInstanceName + operatorNamespace dizeleri 47 karakteri aşamaz.  Bu sınıra uymayacak, şu hatayı alırsınız:

   ```console
   {"OperatorMessage":"Error: {failed to install chart from path [helm-operator] for release [<operatorInstanceName>-helm-<operatorNamespace>]: err [release name \"<operatorInstanceName>-helm-<operatorNamespace>\" exceeds max length of 53]} occurred while doing the operation : {Installing the operator} on the config","ClusterState":"Installing the operator"}
   ```

Daha fazla bilgi için bkz. [Flox belgeleri](https://aka.ms/FluxcdReadme).

> [!TIP]
> Azure portal, Azure Arc etkinleştirilmiş Kubernetes kaynak dikey penceresinin **yapılandırmalar** sekmesinde bir sourceControlConfiguration oluşturmak mümkündür.

## <a name="validate-the-sourcecontrolconfiguration"></a>SourceControlConfiguration 'ı doğrulama

Azure CLı 'yı kullanarak `sourceControlConfiguration` başarıyla oluşturulduğunu doğrulayın.

```console
az k8sconfiguration show --resource-group AzureArcTest --name cluster-config --cluster-name AzureArcTest1 --cluster-type connectedClusters
```

`sourceControlConfiguration`Kaynağın uyumluluk durumu, iletiler ve hata ayıklama bilgileriyle güncelleştirildiğini unutmayın.

**Çıktıların**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Installed",
    "lastConfigApplied": "2019-12-05T05:34:41.481000",
    "message": "...",
    "messageLevel": "3"
  },
  "id": "/subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "cluster-config",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "cluster-config",
  "operatorParams": "--git-readonly",
  "operatorScope": "cluster",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "...",
  "repositoryUrl": "git://github.com/Azure/arc-k8s-demo.git",
  "resourceGroup": "AzureArcTest",
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

Oluşturulduğunda, `sourceControlConfiguration` çok sayıda şey meydana gelir:

1. Azure Arc, `config-agent` Yeni veya güncelleştirilmiş yapılandırmalara yönelik Azure Resource Manager izler ( `Microsoft.KubernetesConfiguration/sourceControlConfiguration` )
1. `config-agent`Yeni yapılandırmayı bildirimler `Pending`
1. `config-agent`yapılandırma özelliklerini okur ve yönetilen bir örneğini dağıtmaya hazırlar`flux`
    * `config-agent`hedef ad alanını oluşturur
    * `config-agent`bir Kubernetes hizmet hesabını uygun izinle ( `cluster` veya `namespace` kapsamla) hazırlar
    * `config-agent`bir örneğini dağıtır`flux`
    * `flux`bir SSH anahtarı oluşturur ve ortak anahtarı günlüğe kaydeder
1. `config-agent`durumu geri bildirir`sourceControlConfiguration`

Sağlama işlemi gerçekleşirken, `sourceControlConfiguration` birkaç durum değişikliği arasında hareket eder. Yukarıdaki komutla ilerlemeyi izleyin `az k8sconfiguration show ...` :

1. `complianceStatus` -> `Pending`: ilk ve devam eden durumları temsil eder
1. `complianceStatus` -> `Installed`: `config-agent` kümeyi başarıyla yapılandırıp dağıtım hatasız olarak dağıtabilecek `flux`
1. `complianceStatus` -> `Failed`: `config-agent` bir hata dağıtımıyla karşılaşıldı `flux` , Ayrıntılar `complianceStatus.message` yanıt gövdesinde kullanılabilir olmalıdır

## <a name="apply-configuration-from-a-private-git-repository"></a>Özel bir git deposundan yapılandırma Uygula

Özel bir git deposu kullanıyorsanız, döngüyü kapatmak için bir görev daha gerçekleştirmeniz gerekir: tarafından oluşturulan ortak anahtarı `flux` depoya bir **dağıtım anahtarı** olarak ekleyin.

**Azure CLı kullanarak ortak anahtarı alın**

```console
$ az k8sconfiguration show --resource-group <resource group name> --cluster-name <connected cluster name> --name <configuration name> --query 'repositoryPublicKey'
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAREDACTED"
```

**Azure portal ortak anahtarı al**

1. Azure portal bağlı küme kaynağına gidin.
2. Kaynak sayfasında, "konfigürasyonlar" ı seçin ve bu küme için yapılandırmaların listesini görüntüleyin.
3. Özel Git deposunu kullanan yapılandırmayı seçin.
4. Açılan bağlam penceresinde, pencerenin alt kısmında **Depo ortak anahtarını**kopyalayın.

**Ortak anahtarı git deposuna bir dağıtım anahtarı olarak ekleyin**

1. GitHub ' ı açın, Çatalınıza gidin, **Ayarlar**' a gidin ve **anahtarları dağıtın**
2. **Dağıtım anahtarı Ekle** 'ye tıklayın
3. Bir başlık sağlayın
4. **Yazma erişimine Izin ver** 'i denetle
5. Ortak anahtarı (herhangi bir çevreleyen tırnak işareti) Yapıştır
6. **Anahtar Ekle** 'ye tıklayın

Bu anahtarların nasıl yönetileceği hakkında daha fazla bilgi için GitHub belgelerine bakın.

**Bir Azure DevOps deposu kullanıyorsanız, anahtarı SSH Anahtarlarınıza ekleyin**

1. Sağ üst taraftaki (profil görüntüsünün yanında) **Kullanıcı ayarları** altında **SSH ortak anahtarlar** ' a tıklayın.
1. **+ Yeni anahtar** seçin
1. Ad girin
1. Ortak anahtarı çevreleyen tırnak olmadan Yapıştır
1. **Ekle** 'ye tıklayın

## <a name="validate-the-kubernetes-configuration"></a>Kubernetes yapılandırmasını doğrulama

Örneği yükledikten sonra `config-agent` `flux` , git deposunda tutulan Kaynaklar kümeye akışa başlamalıdır. Ad alanlarının, dağıtımların ve kaynakların oluşturulduğunu görmek için işaretleyin:

```console
kubectl get ns --show-labels
```

**Çıktıların**

```console
NAME              STATUS   AGE    LABELS
azure-arc         Active   24h    <none>
cluster-config    Active   177m   <none>
default           Active   29h    <none>
itops             Active   177m   fluxcd.io/sync-gc-mark=sha256.9oYk8yEsRwWkR09n8eJCRNafckASgghAsUWgXWEQ9es,name=itops
kube-node-lease   Active   29h    <none>
kube-public       Active   29h    <none>
kube-system       Active   29h    <none>
team-a            Active   177m   fluxcd.io/sync-gc-mark=sha256.CS5boSi8kg_vyxfAeu7Das5harSy1i0gc2fodD7YDqA,name=team-a
team-b            Active   177m   fluxcd.io/sync-gc-mark=sha256.vF36thDIFnDDI2VEttBp5jgdxvEuaLmm7yT_cuA2UEw,name=team-b
```

,, `team-a` `team-b` `itops` Ve `cluster-config` ad alanlarının oluşturulduğunu görebiliriz.

`flux`İşleci `cluster-config` , şu deneyimle yönlendirerek ad alanına dağıtıldı `sourceControlConfig` :

```console
kubectl -n cluster-config get deploy  -o wide
```

**Çıktıların**

```console
NAME             READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES                         SELECTOR
cluster-config   1/1     1            1           3h    flux         docker.io/fluxcd/flux:1.16.0   instanceName=cluster-config,name=flux
memcached        1/1     1            1           3h    memcached    memcached:1.5.15               name=memcached
```

## <a name="further-exploration"></a>Daha fazla araştırma

Yapılandırma deposunun bir parçası olarak dağıtılan diğer kaynakları keşfedebilirsiniz:

```console
kubectl -n team-a get cm -o yaml
kubectl -n itops get all
```

## <a name="delete-a-configuration"></a>Yapılandırma silme

`sourceControlConfiguration`Azure CLI veya Azure Portal kullanarak bir silme.  Sil komutunu başlattıktan sonra, `sourceControlConfiguration` kaynak Azure 'da hemen silinir, ancak ilişkili nesnelerin kümeden tam olarak silinmesi 1 saate kadar sürebilir (Bu zaman gecikmesini azaltmak için bir biriktirme listesi öğesidir).

> [!NOTE]
> Ad alanı kapsamına sahip bir sourceControlConfiguration oluşturulduktan sonra, ad alanında rol bağlama olan kullanıcıların `edit` Bu ad alanına iş yüklerini dağıtması mümkündür. `sourceControlConfiguration`Ad alanı kapsamı silindiğinde, ad alanı değişmeden kalır ve bu diğer iş yüklerinin kesilmesini önlemek için silinmez.
> Değişiklik yapıldığında, izlenen git deposundan dağıtımların sonucu olan kümedeki değişiklikler silinmez `sourceControlConfiguration` .

```console
az k8sconfiguration delete --name '<config name>' -g '<resource group name>' --cluster-name '<cluster name>' --cluster-type connectedClusters
```

**Çıktıların**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
```

## <a name="next-steps"></a>Sonraki adımlar

- [Kaynak denetimi yapılandırması ile Held kullanma](./use-gitops-with-helm.md)
- [Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)
