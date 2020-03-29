---
title: Azure Kubernetes Hizmetinde (AKS) bölme güvenlik ilkelerini kullanma
description: Azure Kubernetes Hizmetinde (AKS) PodSecurityPolicy'yi kullanarak pod kabullerini nasıl denetleriz öğrenin
services: container-service
ms.topic: article
ms.date: 04/17/2019
ms.openlocfilehash: 74177136a7a61186ab1d273b57dbfce550a18ecf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77914543"
---
# <a name="preview---secure-your-cluster-using-pod-security-policies-in-azure-kubernetes-service-aks"></a>Önizleme - Azure Kubernetes Hizmeti'nde (AKS) bölme güvenlik ilkelerini kullanarak kümenizi güvenli hale

AKS kümenizin güvenliğini artırmak için, hangi bölmelerin zamanlanabileceğini sınırlayabilirsiniz. İzin vermediğiniz kaynakları isteyen bölmeler AKS kümesinde çalıştırılamıyor. Bu erişimi pod güvenlik ilkelerini kullanarak tanımlarsınız. Bu makalede, AKS'de bakla dağıtımını sınırlamak için pod güvenlik ilkelerini nasıl kullanacağınızı gösterir.

> [!IMPORTANT]
> AKS önizleme özellikleri self servis tercihidir. Önizlemeler "olduğu gibi" ve "mevcut olduğu gibi" sağlanır ve hizmet düzeyi anlaşmaları ve sınırlı garanti dışında dır. AKS Önizlemeler kısmen en iyi çaba temelinde müşteri desteği tarafından karşılanır. Bu nedenle, bu özellikler üretim kullanımı için değildir. Daha fazla bilgi için lütfen aşağıdaki destek makalelerini görün:
>
> * [AKS Destek Politikaları][aks-support-policies]
> * [Azure Destek SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Azure CLI sürümü 2.0.61 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

### <a name="install-aks-preview-cli-extension"></a>Aks-preview CLI uzantısını yükleyin

Pod güvenlik ilkelerini kullanmak için *aks önizleme* CLI uzantısı sürümü 0.4.1 veya daha yüksek olması gerekir. [az uzantı ekle][az-extension-add] komutunu kullanarak *aks önizleme* Azure CLI uzantısını yükleyin ve az [uzantı güncelleştirme][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-pod-security-policy-feature-provider"></a>Bölme güvenlik ilkesi özelliği sağlayıcısını kaydedin

Pod güvenlik ilkelerini kullanmak için bir AKS kümesi oluşturmak veya güncelleştirmek için, önce aboneliğinizde bir özellik bayrağı etkinleştirin. *PodSecurityPolicyPreview* özellik bayrağını kaydetmek için aşağıdaki örnekte gösterildiği gibi [az özellik kayıt][az-feature-register] komutunu kullanın:

> [!CAUTION]
> Bir özelliği bir abonelikte kaydettirdiğinizde, bu özelliği şu anda geri alamazsınız. Bazı önizleme özelliklerini etkinleştirdikten sonra, varsayılanlar abonelikte oluşturulan tüm AKS kümeleri için kullanılabilir. Üretim aboneliklerinde önizleme özelliklerini etkinleştirme. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name PodSecurityPolicyPreview --namespace Microsoft.ContainerService
```

Durumun *Kayıtlı'yı*göstermesi birkaç dakika sürer. [Az özellik listesi][az-feature-list] komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/PodSecurityPolicyPreview')].{Name:name,State:properties.state}"
```

Hazır olduğunuzda, [az sağlayıcı kayıt][az-provider-register] komutunu kullanarak *Microsoft.ContainerService* kaynak sağlayıcısının kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="overview-of-pod-security-policies"></a>Bakla güvenlik ilkelerine genel bakış

Bir Kubernetes kümesinde, bir kaynak oluşturulacakolduğunda API sunucusundaki istekleri engellemek için bir kabul denetleyicisi kullanılır. Kabul denetleyicisi daha sonra kaynak isteğini bir dizi kurala göre *doğrulayabilir* veya dağıtım parametrelerini değiştirmek için kaynağı *mutasyona* uğrayabilir.

*PodSecurityPolicy,* bir bölme belirtiminin tanımlı gereksinimlerinizi karşıladığını doğrulayan bir kabul denetleyicisidir. Bu gereksinimler ayrıcalıklı kapsayıcıların kullanımını, belirli depolama türlerine erişimi veya kapsayıcının çalıştırabileceği kullanıcı veya grubu sınırlayabilir. Pod belirtimleri pod güvenlik ilkesinde belirtilen gereksinimleri karşılamayan bir kaynak dağıtmaya çalıştığınızda, istek reddedilir. AKS kümesinde hangi bölmelerin zamanlanabileceğini denetleme yeteneği, bazı olası güvenlik açıklarını veya ayrıcalık artışlarını önler.

Bir AKS kümesinde pod güvenlik ilkesini etkinleştirdiğinizde, bazı varsayılan ilkeler uygulanır. Bu varsayılan ilkeler, hangi bölmelerin zamanlanabileceğini tanımlamak için kutunun dışında bir deneyim sağlar. Ancak, küme kullanıcıları, siz kendi ilkelerinizi tanımlayana kadar bölme dağıtma sorunlarıyla karşılaşabilir. Önerilen yaklaşım:

* AKS kümesi oluşturma
* Kendi bölme güvenlik ilkelerinizi tanımlayın
* Pod güvenlik ilkesi özelliğini etkinleştirme

Varsayılan ilkelerin pod dağıtımlarını nasıl sınırlandırdığını göstermek için, bu makalede önce pod güvenlik ilkeleri özelliğini etkinleştiriz, sonra özel bir ilke oluştururuz.

## <a name="enable-pod-security-policy-on-an-aks-cluster"></a>AKS kümesinde bölme güvenlik ilkesini etkinleştirme

[Az aks güncelleştirme][az-aks-update] komutunu kullanarak pod güvenlik ilkesini etkinleştirebilir veya devre dışı kullanabilirsiniz. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myAKSCluster* küme adı üzerinde pod güvenlik ilkesi sağlar.

> [!NOTE]
> Gerçek kullanım için, kendi özel ilkelerinizi tanımlayana kadar bakla güvenlik ilkesini etkinleştirme. Bu makalede, varsayılan ilkelerin bölme dağıtımlarını nasıl sınırladığını görmek için ilk adım olarak bölme güvenlik ilkesini etkinleştirin.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-pod-security-policy
```

## <a name="default-aks-policies"></a>Varsayılan AKS ilkeleri

Pod güvenlik ilkesini etkinleştirdiğinizde, AKS *ayrıcalıklı*olarak adlandırılan bir varsayılan ilke oluşturur. Varsayılan ilkeyi atmayın veya kaldırmayın. Bunun yerine, denetlemek istediğiniz ayarları tanımlayan kendi ilkelerinizi oluşturun. Öncelikle bu varsayılan ilkelerin pod dağıtımlarını nasıl etkilediğine bakalım.

Kullanılabilir ilkeleri görüntülemek için, aşağıdaki örnekte gösterildiği gibi [kubectl get psp][kubectl-get] komutunu kullanın

```console
$ kubectl get psp

NAME         PRIV    CAPS   SELINUX    RUNASUSER          FSGROUP     SUPGROUP    READONLYROOTFS   VOLUMES
privileged   true    *      RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *     configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
```

*Ayrıcalıklı* bölme güvenliği ilkesi, AKS kümesindeki herhangi bir kimlik doğrulaması kullanıcıya uygulanır. Bu atama ClusterRoles ve ClusterRoleBindings tarafından denetlenir. [Kubectl get clusterrolebindings][kubectl-get] komutunu kullanın ve *varsayılan:ayrıcalıklı:* bağlama:

```console
kubectl get clusterrolebindings default:privileged -o yaml
```

Aşağıdaki yoğunlaştırılmış çıktıda gösterildiği gibi, *psp:sınırlı* ClusterRole herhangi bir *sisteme atanır: kimlik doğrulaması* kullanıcılar. Bu yetenek, kendi ilkeleriniz tanımlanmadan temel düzeyde kısıtlama sağlar.

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  [...]
  name: default:privileged
  [...]
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: psp:privileged
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:authenticated
```

Kendi bakla güvenlik ilkelerinizi oluşturmaya başlamadan önce bu varsayılan ilkelerin bölmeleri zamanlamak için kullanıcı istekleriyle nasıl etkileşimde bulundurunu anlamak önemlidir. Sonraki birkaç bölümde, bu varsayılan ilkeleri iş başında görmek için bazı bölmeler zamanlayalım.

## <a name="create-a-test-user-in-an-aks-cluster"></a>AKS kümesinde test kullanıcısı oluşturma

Varsayılan olarak, az [aks get-credentials][az-aks-get-credentials] komutunu *admin* kullandığınızda, AKS kümesi için `kubectl` yönetici kimlik bilgileri config eklenir. Yönetici kullanıcı, pod güvenlik ilkelerinin uygulanmasını atlar. AKS kümeleriniz için Azure Active Directory tümleştirmesini kullanıyorsanız, ilkelerin uygulanmasını iş başında görmek için yönetici olmayan bir kullanıcının kimlik bilgileriyle oturum açabilirsiniz. Bu makalede, AKS kümesinde kullanabileceğiniz bir test kullanıcı hesabı oluşturalım.

[Kubectl create name][kubectl-create] command'ı kullanarak test kaynakları için *psp-aks* adlı örnek bir ad alanı oluşturun. Ardından, [kubectl create serviceaccount][kubectl-create] komutunu kullanarak *yönetici olmayan kullanıcı* adlı bir hizmet hesabı oluşturun:

```console
kubectl create namespace psp-aks
kubectl create serviceaccount --namespace psp-aks nonadmin-user
```

Ardından, [kubectl rolebinding][kubectl-create] komutunu kullanarak ad alanında temel eylemleri gerçekleştirmesi için *yönetici olmayan kullanıcı* için bir RoleBinding oluşturun:

```console
kubectl create rolebinding \
    --namespace psp-aks \
    psp-aks-editor \
    --clusterrole=edit \
    --serviceaccount=psp-aks:nonadmin-user
```

### <a name="create-alias-commands-for-admin-and-non-admin-user"></a>Yönetici ve yönetici olmayan kullanıcı için takma ad komutları oluşturma

Kullanırken `kubectl` normal yönetici kullanıcısı ile önceki adımlarda oluşturulan yönetici olmayan kullanıcı arasındaki farkı vurgulamak için iki komut satırı diğer ad oluşturun:

* **Kubectl-admin** takma adı normal yönetici kullanıcı içindir ve *psp-aks* ad alanına yöneliktir.
* **Kubectl-nonadminuser** diğer adı, önceki adımda oluşturulan *nonadmin kullanıcısı* içindir ve *psp-aks* ad alanına yöneliktir.

Aşağıdaki komutlarda gösterildiği gibi bu iki diğer ad oluşturun:

```console
alias kubectl-admin='kubectl --namespace psp-aks'
alias kubectl-nonadminuser='kubectl --as=system:serviceaccount:psp-aks:nonadmin-user --namespace psp-aks'
```

## <a name="test-the-creation-of-a-privileged-pod"></a>Ayrıcalıklı bir bölmeoluşturmayı test edin

Önce güvenlik bağlamına sahip bir bölme zamanladiğinizde `privileged: true`ne olduğunu test edelim. Bu güvenlik bağlamı bölmenin ayrıcalıklarını arttırır. Varsayılan AKS bölme güvenlik ilkelerini gösteren önceki bölümde, *kısıtlı* ilke bu isteği reddetmelidir.

Adlandırılmış `nginx-privileged.yaml` bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged
spec:
  containers:
    - name: nginx-privileged
      image: nginx:1.14.2
      securityContext:
        privileged: true
```

[Kubectl uygula][kubectl-apply] komutunu kullanarak bölmeyi oluşturun ve YAML manifestonuzun adını belirtin:

```console
kubectl-nonadminuser apply -f nginx-privileged.yaml
```

Aşağıdaki örnek çıktıda gösterildiği gibi bölme zamanlanamıyor:

```console
$ kubectl-nonadminuser apply -f nginx-privileged.yaml

Error from server (Forbidden): error when creating "nginx-privileged.yaml": pods "nginx-privileged" is forbidden: unable to validate against any pod security policy: [spec.containers[0].securityContext.privileged: Invalid value: true: Privileged containers are not allowed]
```

Bölme zamanlama aşamasına ulaşmadığından, devam etmeden önce silmek için kaynak yoktur.

## <a name="test-creation-of-an-unprivileged-pod"></a>Ayrıcalıksız bir bölmenin test oluşturma

Önceki örnekte, pod belirtimi ayrıcalıklı yükseltme istedi. Bu istek varsayılan *sınırlı* pod güvenlik ilkesi tarafından reddedilir, bu nedenle pod zamanlanamaz. Şimdi ayrıcalık yükseltme isteği olmadan aynı NGINX pod çalışan deneyelim.

Adlandırılmış `nginx-unprivileged.yaml` bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx:1.14.2
```

[Kubectl uygula][kubectl-apply] komutunu kullanarak bölmeyi oluşturun ve YAML manifestonuzun adını belirtin:

```console
kubectl-nonadminuser apply -f nginx-unprivileged.yaml
```

Kubernetes zamanlayıcısı pod isteğini kabul eder. Ancak, bölmenin durumuna kullanarak `kubectl get pods`bakarsanız, bir hata var:

```console
$ kubectl-nonadminuser get pods

NAME                 READY   STATUS                       RESTARTS   AGE
nginx-unprivileged   0/1     CreateContainerConfigError   0          26s
```

Pod için olaylara bakmak için [kubectl describe pod][kubectl-describe] komutunu kullanın. Aşağıdaki yoğunlaştırılmış örnek, biz istemesek bile kapsayıcı ve görüntü kök izinleri gerektirir gösterir:

```console
$ kubectl-nonadminuser describe pod nginx-unprivileged

Name:               nginx-unprivileged
Namespace:          psp-aks
Priority:           0
PriorityClassName:  <none>
Node:               aks-agentpool-34777077-0/10.240.0.4
Start Time:         Thu, 28 Mar 2019 22:05:04 +0000
[...]
Events:
  Type     Reason     Age                     From                               Message
  ----     ------     ----                    ----                               -------
  Normal   Scheduled  7m14s                   default-scheduler                  Successfully assigned psp-aks/nginx-unprivileged to aks-agentpool-34777077-0
  Warning  Failed     5m2s (x12 over 7m13s)   kubelet, aks-agentpool-34777077-0  Error: container has runAsNonRoot and image will run as root
  Normal   Pulled     2m10s (x25 over 7m13s)  kubelet, aks-agentpool-34777077-0  Container image "nginx:1.14.2" already present on machine
```

Herhangi bir ayrıcalıklı erişim talep etmesek de, NGINX için konteyner görüntüsünün *80*bağlantı noktası için bir bağlama oluşturması gerekir. *1024* ve altındaki bağlantı noktalarını bağlamak için *kök* kullanıcı gereklidir. Bölme başlatmaya çalıştığında, *kısıtlı* pod güvenlik ilkesi bu isteği reddeder.

Bu örnek, AKS tarafından oluşturulan varsayılan bölme güvenlik ilkelerinin etkin olduğunu ve kullanıcının gerçekleştirebileceği eylemleri kısıtladığını gösterir. Temel bir NGINX bölmesinin reddedilmesini beklemeyin, bu varsayılan politikaların davranışını anlamak önemlidir.

Bir sonraki adıma geçmeden önce [kubectl delete pod][kubectl-delete] komutunu kullanarak bu test bölmesini silin:

```console
kubectl-nonadminuser delete -f nginx-unprivileged.yaml
```

## <a name="test-creation-of-a-pod-with-a-specific-user-context"></a>Belirli bir kullanıcı bağlamı ile bir bölmenin test oluşturma

Önceki örnekte, kapsayıcı görüntüsü nginx'i 80 bağlantı noktasına bağlamak için otomatik olarak root kullanmaya çalışmıştır. Bu istek varsayılan *sınırlı* pod güvenlik ilkesi tarafından reddedildi, bu nedenle pod başlatılamadı. Şimdi aynı NGINX bölmeyi belirli bir kullanıcı bağlamı `runAsUser: 2000`yla çalıştırmayı deneyelim.

Adlandırılmış `nginx-unprivileged-nonroot.yaml` bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged-nonroot
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx:1.14.2
      securityContext:
        runAsUser: 2000
```

[Kubectl uygula][kubectl-apply] komutunu kullanarak bölmeyi oluşturun ve YAML manifestonuzun adını belirtin:

```console
kubectl-nonadminuser apply -f nginx-unprivileged-nonroot.yaml
```

Kubernetes zamanlayıcısı pod isteğini kabul eder. Ancak, pod'un durumuna bir `kubectl get pods`önceki örnekten farklı bir hata kullanırsanız:

```console
$ kubectl-nonadminuser get pods

NAME                         READY   STATUS              RESTARTS   AGE
nginx-unprivileged-nonroot   0/1     CrashLoopBackOff    1          3s
```

Pod için olaylara bakmak için [kubectl describe pod][kubectl-describe] komutunu kullanın. Aşağıdaki yoğunlaştırılmış örnek, pod olaylarını gösterir:

```console
$ kubectl-nonadminuser describe pods nginx-unprivileged

Name:               nginx-unprivileged
Namespace:          psp-aks
Priority:           0
PriorityClassName:  <none>
Node:               aks-agentpool-34777077-0/10.240.0.4
Start Time:         Thu, 28 Mar 2019 22:05:04 +0000
[...]
Events:
  Type     Reason     Age                   From                               Message
  ----     ------     ----                  ----                               -------
  Normal   Scheduled  2m14s                 default-scheduler                  Successfully assigned psp-aks/nginx-unprivileged-nonroot to aks-agentpool-34777077-0
  Normal   Pulled     118s (x3 over 2m13s)  kubelet, aks-agentpool-34777077-0  Container image "nginx:1.14.2" already present on machine
  Normal   Created    118s (x3 over 2m13s)  kubelet, aks-agentpool-34777077-0  Created container
  Normal   Started    118s (x3 over 2m12s)  kubelet, aks-agentpool-34777077-0  Started container
  Warning  BackOff    105s (x5 over 2m11s)  kubelet, aks-agentpool-34777077-0  Back-off restarting failed container
```

Olaylar, kapsayıcının oluşturulduğunu ve başlatıldığını gösterir. Kapsülün neden başarısız bir durumda olduğuna dair hemen bir şey yok. [Kubectl günlükleri][kubectl-logs] komutunu kullanarak pod günlüklerine bakalım:

```console
kubectl-nonadminuser logs nginx-unprivileged-nonroot --previous
```

Aşağıdaki örnek günlük çıkışı, NGINX yapılandırmasının kendisi nde, hizmet başlatmaya çalıştığında bir izin hatası olduğunun bir göstergesidir. Bu hata yine bağlantı noktası 80'e bağlanmagereksiniminden kaynaklanır. Pod belirtimi normal bir kullanıcı hesabı tanımlasa da, bu kullanıcı hesabı NGINX hizmetinin başlatılması ve kısıtlı bağlantı noktasına bağlanması için OS düzeyinde yeterli değildir.

```console
$ kubectl-nonadminuser logs nginx-unprivileged-nonroot --previous

2019/03/28 22:38:29 [warn] 1#1: the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
nginx: [warn] the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
2019/03/28 22:38:29 [emerg] 1#1: mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
nginx: [emerg] mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
```

Yine, varsayılan bölme güvenlik ilkelerinin davranışını anlamak önemlidir. Bu hatayı izlemek biraz daha zordu ve yine temel bir NGINX bölmenin reddedilmesini beklemeyebilirsiniz.

Bir sonraki adıma geçmeden önce [kubectl delete pod][kubectl-delete] komutunu kullanarak bu test bölmesini silin:

```console
kubectl-nonadminuser delete -f nginx-unprivileged-nonroot.yaml
```

## <a name="create-a-custom-pod-security-policy"></a>Özel bir bölme güvenlik ilkesi oluşturma

Artık varsayılan bölme güvenlik ilkelerinin davranışını gördüğünüze *göre, yönetici olmayan kullanıcının* bölmeleri başarıyla zamanlamasının bir yolunu bulalım.

Ayrıcalıklı erişim isteyen bölmeleri reddetmek için bir ilke oluşturalım. *runAsUser* veya izin verilen *birimler*gibi diğer seçenekler açıkça kısıtlanmaz. Bu tür bir ilke ayrıcalıklı erişim isteğini reddeder, ancak aksi takdirde kümenin istenen bölmeleri çalıştırmasına izin verir.

Adlandırılmış `psp-deny-privileged.yaml` bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: psp-deny-privileged
spec:
  privileged: false
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  runAsUser:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
  volumes:
  - '*'
```

[Kubectl uygula][kubectl-apply] komutunu kullanarak ilkeyi oluşturun ve YAML manifestonuzun adını belirtin:

```console
kubectl apply -f psp-deny-privileged.yaml
```

Kullanılabilir ilkeleri görüntülemek için, aşağıdaki örnekte gösterildiği gibi [kubectl get psp][kubectl-get] komutunu kullanın. Bir bölmesi oluşturmak için önceki örneklerde uygulanan varsayılan *sınırlı* ilke ile *psp-reddet ayrıcalıklı* ilke karşılaştırın. Yalnızca *PRIV* yükseltme kullanımı politikanız tarafından reddedilir. *psp-deny-ayrıcalıklı* ilke için kullanıcı veya grup üzerinde herhangi bir kısıtlama yoktur.

```console
$ kubectl get psp

NAME                  PRIV    CAPS   SELINUX    RUNASUSER          FSGROUP     SUPGROUP    READONLYROOTFS   VOLUMES
privileged            true    *      RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *
psp-deny-privileged   false          RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *          configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
```

## <a name="allow-user-account-to-use-the-custom-pod-security-policy"></a>Kullanıcı hesabının özel bölme güvenlik ilkesini kullanmasına izin ver

Önceki adımda, ayrıcalıklı erişim isteyen bölmeleri reddetmek için bir bölme güvenlik ilkesi oluşturdunuz. İlkenin kullanılmasına izin vermek için bir *Rol* veya *ClusterRole*oluşturursunuz. Daha sonra, bu rollerden birini *RoleBinding* veya *ClusterRoleBinding*kullanarak ilişkilendirin.

Bu örnekte, önceki adımda oluşturulan *psp-reddet ayrıcalıklı* ilkeyi *kullanmanıza* olanak tanıyan bir ClusterRole oluşturun. Adlandırılmış `psp-deny-privileged-clusterrole.yaml` bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: psp-deny-privileged-clusterrole
rules:
- apiGroups:
  - extensions
  resources:
  - podsecuritypolicies
  resourceNames:
  - psp-deny-privileged
  verbs:
  - use
```

[Kubectl uygula][kubectl-apply] komutunu kullanarak ClusterRole'ı oluşturun ve YAML manifestonuzun adını belirtin:

```console
kubectl apply -f psp-deny-privileged-clusterrole.yaml
```

Şimdi, önceki adımda oluşturulan ClusterRole'yi kullanmak için bir ClusterRoleBinding oluşturun. Adlandırılmış `psp-deny-privileged-clusterrolebinding.yaml` bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: psp-deny-privileged-clusterrolebinding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: psp-deny-privileged-clusterrole
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:serviceaccounts
```

[Kubectl uygula][kubectl-apply] komutunu kullanarak bir ClusterRoleBinding oluşturun ve YAML manifestonuzun adını belirtin:

```console
kubectl apply -f psp-deny-privileged-clusterrolebinding.yaml
```

> [!NOTE]
> Bu makalenin ilk adımında, AKS kümesinde pod güvenlik ilkesi özelliği etkinleştirildi. Önerilen uygulama, yalnızca kendi ilkelerinizi tanımladıktan sonra bakla güvenlik ilkesi özelliğini etkinleştirmekti. Bu, bölme güvenlik ilkesi özelliğini etkinleştireceğiniz aşamadır. Bir veya daha fazla özel ilke tanımlanmış ve kullanıcı hesapları bu ilkelerle ilişkilendirilmiştir. Artık pod güvenlik ilkesi özelliğini güvenli bir şekilde etkinleştirebilir ve varsayılan ilkelerin neden olduğu sorunları en aza indirebilirsiniz.

## <a name="test-the-creation-of-an-unprivileged-pod-again"></a>Ayrıcalıksız bir bölmenin oluşturulmasını yeniden test edin

Özel bölme güvenlik ilkeniz uygulandığı nda ve kullanıcı hesabının ilkeyi kullanması için bağlayıcı lıkla, yeniden ayrıcalıksız bir bölme oluşturmaya çalışalım. `nginx-privileged.yaml` [Kubectl uygula][kubectl-apply] komutunu kullanarak pod oluşturmak için aynı bildirimi kullanın:

```console
kubectl-nonadminuser apply -f nginx-unprivileged.yaml
```

Bölme başarıyla zamanlandı. [Kubectl get pods][kubectl-get] komutunu kullanarak bölmenin durumunu kontrol ettiğinizde, bölme *çalışıyor:*

```
$ kubectl-nonadminuser get pods

NAME                 READY   STATUS    RESTARTS   AGE
nginx-unprivileged   1/1     Running   0          7m14s
```

Bu örnek, farklı kullanıcılar veya gruplar için AKS kümesine erişimi tanımlamak için özel bölme güvenlik ilkeleri nasıl oluşturabileceğinizi gösterir. Varsayılan AKS ilkeleri, bölmelerin neleri çalıştırabileceği üzerinde sıkı denetimler sağlar, bu nedenle ihtiyacınız olan kısıtlamaları doğru bir şekilde tanımlamak için kendi özel ilkelerinizi oluşturun.

[Kubectl delete][kubectl-delete] komutunu kullanarak NGINX ayrıcalıksız bölmesini silin ve YAML manifestonuzun adını belirtin:

```console
kubectl-nonadminuser delete -f nginx-unprivileged.yaml
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Pod güvenlik ilkesini devre dışı kullanabilirsiniz, [az aks güncelleştirme][az-aks-update] komutunu yeniden kullanın. Aşağıdaki örnek *myResourceGroup*adlı kaynak grubunda *myAKSCluster* küme adı pod güvenlik ilkesi devre dışı kalmaktadır:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --disable-pod-security-policy
```

Ardından ClusterRole ve ClusterRoleBinding'i silin:

```console
kubectl delete -f psp-deny-privileged-clusterrolebinding.yaml
kubectl delete -f psp-deny-privileged-clusterrole.yaml
```

[Kubectl delete][kubectl-delete] komutunu kullanarak güvenlik ilkesini silin ve YAML manifestonuzun adını belirtin:

```console
kubectl delete -f psp-deny-privileged.yaml
```

Son olarak, *psp-aks* ad alanını silin:

```console
kubectl delete namespace psp-aks
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ayrıcalıklı erişim kullanımını önlemek için bir bölme güvenlik ilkesi oluşturmak için nasıl gösterin. Bir ilkenin uygulayabileceği birim türü veya RunAs kullanıcısı gibi birçok özellik vardır. Kullanılabilir seçenekler hakkında daha fazla bilgi için [Kubernetes pod güvenlik ilkesi başvuru dokümanlarına][kubernetes-policy-reference]bakın.

Pod ağ trafiğini sınırlama hakkında daha fazla bilgi için [AKS'deki ağ ilkelerini kullanarak bölmeler arasındaki güvenli trafiği][network-policies]görün.

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[kubernetes-policy-reference]: https://kubernetes.io/docs/concepts/policy/pod-security-policy/#policy-reference

<!-- LINKS - internal -->
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
