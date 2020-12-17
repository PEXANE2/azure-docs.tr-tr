---
title: Yay etkin Kubernetes kümesinde Gilar kullanarak yapılandırma dağıtma (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Azure Arc etkin bir Kubernetes kümesi (Önizleme) yapılandırmak için Gilar 'ı kullanma
keywords: Gilar, Kubernetes, K8s, Azure, Arc, Azure Kubernetes hizmeti, AKS, kapsayıcılar
ms.openlocfilehash: 85771824a6cecd10346937220e400028a4570377
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97653461"
---
# <a name="deploy-configurations-using-gitops-on-arc-enabled-kubernetes-cluster-preview"></a>Yay etkin Kubernetes kümesinde Gilar kullanarak yapılandırma dağıtma (Önizleme)

Gilar, Kubernetes ile ilişkili olduğu gibi, bir git deposunda Kubernetes yapılandırmasının (dağıtımlar, ad alanları vb.) istenen durumunu bildirme ve ardından bu yapılandırmaların bir işleç kullanılarak kümeye bir yoklama ve çekme tabanlı dağıtımı ile ilgili bir uygulamadır. Bu belge, Azure Arc etkin Kubernetes kümelerinde bu tür iş akışlarının kurulumunu içerir.

Kümeniz ve git deposu arasındaki bağlantı, Azure Resource Manager `Microsoft.KubernetesConfiguration/sourceControlConfigurations` uzantı kaynağı olarak oluşturulur. `sourceControlConfiguration`Kaynak özellikleri, Kubernetes kaynaklarının git 'ten kümenize nasıl akmasını gerektiğini temsil eder. Veriler, `sourceControlConfiguration` verilerin gizliliğini sağlamak için bir Azure Cosmos DB veritabanında şifreli olarak depolanır.

`config-agent`Kümenizde çalışan, `sourceControlConfiguration` Azure Arc etkin Kubernetes kaynağında yeni veya güncelleştirilmiş uzantı kaynaklarını izlerken, her biri için Git deposunu izlemek üzere bir Flox operatörü dağıtmak `sourceControlConfiguration` ve herhangi bir güncelleştirmenin herhangi bir güncelleştirme uygulanması konusunda sorumludur `sourceControlConfiguration` . Çok `sourceControlConfiguration` kiracılı hale ulaşmak için aynı Azure Arc etkin Kubernetes kümesinde birden çok kaynak oluşturmak mümkündür. Bunları `sourceControlConfiguration` `namespace` ilgili ad alanları içinde ile sınırlamak için her bir farklı kapsama sahip oluşturabilirsiniz.

Git deposu, ad alanları, ConfigMaps, dağıtımlar, DaemonSets vb. gibi geçerli bir Kubernetes kaynağını tanımlayan YAML biçimli bildirimler içerebilir.  Ayrıca, uygulamaları dağıtmak için Held grafikleri de içerebilir. Yaygın bir senaryo kümesi, kuruluşunuz için genel Azure rolleri ve bağlamaları, izleme veya günlüğe kaydetme aracılarını veya küme genelinde Hizmetleri içerebilen temel bir yapılandırma tanımlamayı içerir.

Aynı model, heterojen ortamlar arasında dağıtılabilen daha büyük bir küme koleksiyonunu yönetmek için kullanılabilir. Örneğin, kuruluşunuz için taban çizgisi yapılandırmasını tanımlayan ve aynı anda onlarca Kubernetes kümesine uygulayabilen bir havuzunuz olabilir. [Azure ilkesi](use-azure-policy.md) , bir `sourceControlConfiguration` kapsam (abonelik veya kaynak grubu) altındaki tüm Azure Arc etkin Kubernetes kaynaklarında belirli bir parametre kümesiyle bir oluşturma işlemini otomatikleştirebilir.

Bu başlangıç kılavuzu, Küme Yöneticisi kapsamına sahip bir yapılandırma kümesi uygularken size kılavuzluk eder.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir Azure Arc etkin Kubernetes bağlı kümesine sahip olduğunuzu varsaymaktadır. Bağlı bir kümeye ihtiyacınız varsa, [küme bağlama hızlı başlangıç](./connect-cluster.md)bölümüne bakın.

## <a name="create-a-configuration"></a>Yapılandırma oluşturma

Bu belgede kullanılan [örnek depo](https://github.com/Azure/arc-k8s-demo) , birkaç ad alanı sağlamak, ortak bir iş yükü dağıtmak ve takıma özgü bazı yapılandırmalar sağlamak isteyen bir küme operatörü etrafında yapılandırılmıştır. Bu depoyu kullanmak, kümenizde aşağıdaki kaynakları oluşturur:

**Ad alanları:** `cluster-config` , `team-a` , `team-b` 
 **Dağıtım:** `cluster-config/azure-vote` 
 **configmap:**`team-a/endpoints`

Yeni `config-agent` `sourceControlConfiguration` `config-agent` veya güncelleştirilmiş bir yapılandırma almak için en fazla geçen süre olan 30 saniyede bir Azure 'u yeni veya güncelleştirilmiş olarak yoklar.
Özel bir depoyu ile ilişkilendirirken `sourceControlConfiguration` , [özel bir git deposundan yapılandırma uygulama](#apply-configuration-from-a-private-git-repository)adımlarını da tamamlatığınızdan emin olun.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

`k8sconfiguration`Bağlı bir kümeyi [örnek git deposuna](https://github.com/Azure/arc-k8s-demo)bağlamak için Azure CLI uzantısını kullanın. Bu yapılandırmaya bir ad vereceğiz `cluster-config` , aracıyı `cluster-config` ad alanına dağıtmasını ve operatör izinleri vermesini sağlıyoruz `cluster-admin` .

```console
az k8sconfiguration create --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --operator-instance-name cluster-config --operator-namespace cluster-config --repository-url https://github.com/Azure/arc-k8s-demo --scope cluster --cluster-type connectedClusters
```

**Çıktıların**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Pending",
    "lastConfigApplied": "0001-01-01T00:00:00",
    "message": "{\"OperatorMessage\":null,\"ClusterState\":null}",
    "messageLevel": "3"
  },
  "configurationProtectedSettings": {},
  "enableHelmOperator": false,
  "helmOperatorProperties": null,
  "id": "/subscriptions/<sub id>/resourceGroups/<group name>/providers/Microsoft.Kubernetes/connectedClusters/<cluster name>/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "cluster-config",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "cluster-config",
  "operatorParams": "--git-readonly",
  "operatorScope": "cluster",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "",
  "repositoryUrl": "https://github.com/Azure/arc-k8s-demo",
  "resourceGroup": "MyRG",
  "sshKnownHostsContents": "",
  "systemData": {
    "createdAt": "2020-11-24T21:22:01.542801+00:00",
    "createdBy": null,
    "createdByType": null,
    "lastModifiedAt": "2020-11-24T21:22:01.542801+00:00",
    "lastModifiedBy": null,
    "lastModifiedByType": null
  },
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
  ```

#### <a name="use-a-public-git-repo"></a>Ortak bir git deposu kullanma

| Parametre | Biçimlendir |
| ------------- | ------------- |
| --Depo-URL | http [s]:/sunucu/repo [. git] veya git:/sunucu/repo [. git]

#### <a name="use-a-private-git-repo-with-ssh-and-flux-created-keys"></a>SSH ve Flox tarafından oluşturulan anahtarlarla özel bir git deposu kullanma

| Parametre | Biçimlendir | Notlar
| ------------- | ------------- | ------------- |
| --Depo-URL | ssh://user@server/repo[. git] veya user@server:repo [. git] | `git@` yerine kullanılabilir `user@`

> [!NOTE]
> Flox tarafından üretilen ortak anahtar, git hizmeti sağlayıcınızdaki Kullanıcı hesabına eklenmelidir. Anahtar, Kullanıcı hesabı > yerine depoya eklenirse, `git@` `user@` URL 'de yerine kullanın. [Daha fazla ayrıntı görüntüleyin](#apply-configuration-from-a-private-git-repository)

#### <a name="use-a-private-git-repo-with-ssh-and-user-provided-keys"></a>SSH ve Kullanıcı tarafından sağlanmış anahtarlarla özel bir git deposu kullanma

| Parametre | Biçimlendir | Notlar |
| ------------- | ------------- | ------------- |
| --Depo-URL  | ssh://user@server/repo[. git] veya user@server:repo [. git] | `git@` yerine kullanılabilir `user@` |
| --SSH-private-anahtar | [pek biçiminde](https://aka.ms/PEMformat) Base64 kodlamalı anahtar | Anahtarı doğrudan sağlama |
| --SSH-private-anahtar-dosya | yerel dosyanın tam yolu | Yerel dosyanın pek biçimli anahtarını içeren tam yolunu sağlayın

> [!NOTE]
> Kendi özel anahtarınızı doğrudan veya bir dosyaya sağlayın. Anahtar [pek biçiminde](https://aka.ms/PEMformat) olmalı ve yeni satır (\n) ile bitmelidir.  İlişkili ortak anahtar, git hizmeti sağlayıcınızdaki Kullanıcı hesabına eklenmelidir. Anahtar, Kullanıcı hesabı yerine depoya eklenirse, yerine kullanın `git@` `user@` . [Daha fazla ayrıntı görüntüleyin](#apply-configuration-from-a-private-git-repository)

#### <a name="use-a-private-git-host-with-ssh-and-user-provided-known-hosts"></a>SSH ve Kullanıcı tarafından sağlanmış bilinen konaklarla özel bir git Konağı kullanma

| Parametre | Biçimlendir | Notlar |
| ------------- | ------------- | ------------- |
| --Depo-URL  | ssh://user@server/repo[. git] veya user@server:repo [. git] | `git@` yerine kullanılabilir `user@` |
| --SSH-bilinen-konaklar | base64 kodlu | doğrudan sağlanmış bilinen ana bilgisayarlar içeriği |
| --SSH-bilinen-hosts-dosya | yerel dosyanın tam yolu | Yerel bir dosyada belirtilen bilinen ana bilgisayar içeriği

> [!NOTE]
> Flox işleci, SSH bağlantısını kurmadan önce git deposunun kimliğini doğrulamak için bilinen ana bilgisayar dosyasındaki ortak git konaklarının bir listesini tutar. Yaygın olmayan bir git deposu veya kendi git ana bilgisayarınızı kullanıyorsanız, akıcı x 'in deponuzu tanımlayabilmesi için ana bilgisayar anahtarını sağlamanız gerekebilir. Bilinen ana bilgisayar içeriğinizi doğrudan veya bir dosyasına sağlayabilirsiniz. [Bilinen ana bilgisayarlar içerik biçimi belirtimini görüntüleyin](https://aka.ms/KnownHostsFormat).
> Bunu, yukarıda açıklanan SSH anahtar senaryolarından biriyle birlikte kullanabilirsiniz.

#### <a name="use-a-private-git-repo-with-https"></a>HTTPS ile özel bir git deposu kullanma

| Parametre | Biçimlendir | Notlar |
| ------------- | ------------- | ------------- |
| --Depo-URL | https://server/repo[. git] | Temel kimlik doğrulaması ile HTTPS |
| --https-Kullanıcı | ham veya base64 kodlu | HTTPS Kullanıcı adı |
| --https-anahtar | ham veya base64 kodlu | HTTPS kişisel erişim belirteci veya parolası

> [!NOTE]
> HTTPS helmrelease özel kimlik doğrulaması yalnızca Held operatörü grafik sürümü >= 1.2.0 ile desteklenir.  Sürüm 1.2.0 varsayılan olarak kullanılır.
> Şu anda Azure Kubernetes Hizmetleri tarafından yönetilen kümeler için HTTPS helk sürümü özel kimlik doğrulaması desteklenmiyor.
> Proxy 'niz aracılığıyla Git deposuna erişmek için Flox 'e ihtiyacınız varsa, Azure Arc aracılarını ara sunucu ayarlarıyla güncelleştirmeniz gerekecektir. [Daha fazla bilgi](https://docs.microsoft.com/azure/azure-arc/kubernetes/connect-cluster#connect-using-an-outbound-proxy-server)

#### <a name="additional-parameters"></a>Ek parametreler

Yapılandırmayı özelleştirmek için kullanabileceğiniz daha fazla parametre bulabilirsiniz:

`--enable-helm-operator` : Hele grafik dağıtımları desteğini etkinleştirmek için *Isteğe bağlı* anahtar.

`--helm-operator-params` : Hele işleci için *Isteğe bağlı* grafik değerleri (etkinse).  Örneğin, '--Held. Versions = v3 ' olarak ayarlayın.

`--helm-operator-chart-version` : Held işleci için *Isteğe bağlı* grafik sürümü (etkinse). Varsayılan: ' 1.2.0 '.

`--operator-namespace` : İşleç ad alanı için *Isteğe bağlı* ad. Varsayılan: ' varsayılan '

`--operator-params` : İşleç için *Isteğe bağlı* parametreler. Tek tırnak içinde verilmelidir. Örneğin, ```--operator-params='--git-readonly --git-path=releases --sync-garbage-collection' ```

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
> Azure Arc etkin Kubernetes kaynağının **Gima** sekmesindeki Azure Portal sourceControlConfiguration oluşturmak mümkündür.

## <a name="validate-the-sourcecontrolconfiguration"></a>SourceControlConfiguration 'ı doğrulama

Azure CLı 'yı kullanarak `sourceControlConfiguration` başarıyla oluşturulduğunu doğrulayın.

```console
az k8sconfiguration show --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

`sourceControlConfiguration`Kaynağın uyumluluk durumu, iletiler ve hata ayıklama bilgileriyle güncelleştirildiğini unutmayın.

**Çıktıların**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Installed",
    "lastConfigApplied": "2020-12-10T18:26:52.801000+00:00",
    "message": "...",
    "messageLevel": "Information"
  },
  "configurationProtectedSettings": {},
  "enableHelmOperator": false,
  "helmOperatorProperties": {
    "chartValues": "",
    "chartVersion": ""
  },
  "id": "/subscriptions/<sub id>/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
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
  "sshKnownHostsContents": null,
  "systemData": {
    "createdAt": "2020-12-01T03:58:56.175674+00:00",
    "createdBy": null,
    "createdByType": null,
    "lastModifiedAt": "2020-12-10T18:30:56.881219+00:00",
    "lastModifiedBy": null,
    "lastModifiedByType": null
},
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

Oluşturulduğunda, `sourceControlConfiguration` çok sayıda şey meydana gelir:

1. Azure Arc, `config-agent` Yeni veya güncelleştirilmiş yapılandırmalara yönelik Azure Resource Manager izler ( `Microsoft.KubernetesConfiguration/sourceControlConfigurations` )
1. `config-agent`Yeni yapılandırmayı bildirimler `Pending`
1. `config-agent` yapılandırma özelliklerini okur ve yönetilen bir örneğini dağıtmaya hazırlar `flux`
    * `config-agent` hedef ad alanını oluşturur
    * `config-agent` bir Kubernetes hizmet hesabını uygun izinle ( `cluster` veya `namespace` kapsamla) hazırlar
    * `config-agent` bir örneğini dağıtır `flux`
    * `flux` bir SSH anahtarı oluşturur ve ortak anahtarı günlüğe kaydeder (SSH seçeneği akışkan x tarafından oluşturulan anahtarlarla kullanılıyorsa)
1. `config-agent``sourceControlConfiguration`Azure 'da durumu kaynağa geri bildirir

Sağlama işlemi gerçekleşirken, `sourceControlConfiguration` birkaç durum değişikliği arasında hareket eder. Yukarıdaki komutla ilerlemeyi izleyin `az k8sconfiguration show ...` :

1. `complianceStatus` -> `Pending`: ilk ve devam eden durumları temsil eder
1. `complianceStatus` -> `Installed`: `config-agent` kümeyi başarıyla yapılandırıp dağıtım hatasız olarak dağıtabilecek `flux`
1. `complianceStatus` -> `Failed`: `config-agent` bir hata dağıtımıyla karşılaşıldı `flux` , Ayrıntılar `complianceStatus.message` yanıt gövdesinde kullanılabilir olmalıdır

## <a name="apply-configuration-from-a-private-git-repository"></a>Özel bir git deposundan yapılandırma Uygula

Özel bir git deposu kullanıyorsanız, deponuzda SSH ortak anahtarını yapılandırmanız gerekir. Ortak anahtarı git deposunda veya depoya erişimi olan git kullanıcısına yapılandırabilirsiniz. SSH ortak anahtarı sizin sağladığınız ya da akıcı x 'in oluşturduğu bir tane olacaktır.

**Kendi ortak anahtarınızı alın**

Kendi SSH anahtarlarınızı oluşturduysanız, özel ve ortak anahtarlarınız zaten vardır.

**Azure CLı kullanarak ortak anahtarı alın (Flox anahtarları oluşturuyorsa yararlıdır)**

```console
$ az k8sconfiguration show --resource-group <resource group name> --cluster-name <connected cluster name> --name <configuration name> --query 'repositoryPublicKey'
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAREDACTED"
```

**Azure portal ortak anahtarı al (Flox anahtarları oluşturuyorsa yararlı olur)**

1. Azure portal bağlı küme kaynağına gidin.
2. Kaynak sayfasında, "Gilar" ı seçin ve bu küme için yapılandırmaların listesini görüntüleyin.
3. Özel Git deposunu kullanan yapılandırmayı seçin.
4. Açılan bağlam penceresinde, pencerenin alt kısmında **Depo ortak anahtarını** kopyalayın.

GitHub kullanıyorsanız, aşağıdaki 2 seçenekten birini kullanın:

**Seçenek 1: Kullanıcı hesabınıza ortak anahtar ekleme (hesabınızdaki tüm depolar için geçerlidir)**

1. GitHub ' ı açın, sayfanın sağ üst köşesindeki profil simgesine tıklayın.
2. **Ayarlar** ' a tıklayın
3. **SSH ve GPG anahtarlarına** tıklayın
4. **Yenı SSH anahtarına** tıklayın
5. Bir başlık sağlayın
6. Ortak anahtarı (herhangi bir çevreleyen tırnak işareti) Yapıştır
7. **SSH anahtarı Ekle** 'ye tıklayın

**Seçenek 2: ortak anahtarı git deposuna bir dağıtım anahtarı olarak ekleyin (yalnızca bu depo için geçerlidir)**

1. GitHub ' ı açın, depoya, **Ayarlar**' a gidin ve **anahtarları dağıtın**
2. **Dağıtım anahtarı Ekle** 'ye tıklayın
3. Bir başlık sağlayın
4. **Yazma erişimine Izin ver** 'i denetle
5. Ortak anahtarı (herhangi bir çevreleyen tırnak işareti) Yapıştır
6. **Anahtar Ekle** 'ye tıklayın

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

## <a name="further-exploration"></a>Keşfetmeye devam edin

Yapılandırma deposunun bir parçası olarak dağıtılan diğer kaynakları keşfedebilirsiniz:

```console
kubectl -n team-a get cm -o yaml
kubectl -n itops get all
```

## <a name="delete-a-configuration"></a>Yapılandırma silme

`sourceControlConfiguration`Azure CLI veya Azure Portal kullanarak bir silme.  Sil komutunu başlattıktan sonra, `sourceControlConfiguration` kaynak Azure 'da hemen silinir ve ilgili nesnelerin kümeden tam olarak silinmesi 10 dakika içinde gerçekleşmelidir.  `sourceControlConfiguration`Silindiği zaman hatalı durumdaysa, ilişkili nesnelerin tam silinmesi bir saate kadar sürebilir.

> [!NOTE]
> Ad alanı kapsamına sahip bir sourceControlConfiguration oluşturulduktan sonra, ad alanında rol bağlama olan kullanıcıların `edit` Bu ad alanında iş yüklerini dağıtması mümkündür. `sourceControlConfiguration`Ad alanı kapsamı silindiğinde, ad alanı değişmeden kalır ve bu diğer iş yüklerinin kesilmesini önlemek için silinmez.  Gerekirse, bu ad alanını kubectl ile el ile silebilirsiniz.
> Değişiklik yapıldığında, izlenen git deposundan dağıtımların sonucu olan kümedeki değişiklikler silinmez `sourceControlConfiguration` .

```console
az k8sconfiguration delete --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

**Çıktıların**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
```

## <a name="next-steps"></a>Sonraki adımlar

- [Kaynak denetimi yapılandırması ile Held kullanma](./use-gitops-with-helm.md)
- [Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)
