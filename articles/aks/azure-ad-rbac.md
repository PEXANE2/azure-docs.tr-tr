---
title: Kümeler için Azure AD ve RBAC kullanma
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Service (AKS) içindeki rol tabanlı erişim denetimi (RBAC) kullanarak küme kaynaklarına erişimi kısıtlamak için Azure Active Directory grubu üyeliğini nasıl kullanacağınızı öğrenin
services: container-service
ms.topic: article
ms.date: 07/21/2020
ms.openlocfilehash: 646b1b5fb5079f0b959aaa2337c1dbab09ff4134
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87057341"
---
# <a name="control-access-to-cluster-resources-using-role-based-access-control-and-azure-active-directory-identities-in-azure-kubernetes-service"></a>Azure Kubernetes hizmetindeki rol tabanlı erişim denetimi ve Azure Active Directory kimliklerini kullanarak küme kaynaklarına erişimi denetleme

Azure Kubernetes hizmeti (AKS), Kullanıcı kimlik doğrulaması için Azure Active Directory (AD) kullanacak şekilde yapılandırılabilir. Bu yapılandırmada, bir Azure AD kimlik doğrulama belirteci kullanarak bir AKS kümesinde oturum açın. Ayrıca, Kubernetes rol tabanlı erişim denetimi 'ni (RBAC), bir kullanıcının kimliğine veya grup üyeliğine dayalı olarak küme kaynaklarına erişimi sınırlandırmak için de yapılandırabilirsiniz.

Bu makalede, bir AKS kümesinde Kubernetes RBAC kullanarak ad alanları ve küme kaynaklarına erişimi denetlemek için Azure AD grup üyeliğini nasıl kullanacağınız gösterilmektedir. Örnek gruplar ve kullanıcılar Azure AD 'de oluşturulur, ardından kaynaklar oluşturmak ve görüntülemek için uygun izinleri vermek üzere AKS kümesinde roller ve RoleBindings oluşturulur.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure AD tümleştirmesi ile mevcut bir AKS kümeniz olduğunu varsaymaktadır. AKS kümesine ihtiyacınız varsa bkz. [Azure ACTIVE DIRECTORY aks Ile tümleştirme][azure-ad-aks-cli].

Azure CLı sürüm 2.0.61 veya sonraki bir sürümün yüklü ve yapılandırılmış olması gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

## <a name="create-demo-groups-in-azure-ad"></a>Azure AD 'de tanıtım grupları oluşturma

Bu makalede, Kubernetes RBAC ve Azure AD denetimi 'nin küme kaynaklarına nasıl erişiminin olduğunu göstermek için kullanılabilecek iki kullanıcı rolü oluşturalım. Aşağıdaki iki örnek rol kullanılır:

* **Uygulama geliştiricisi**
    * *AppDev* grubunun bir parçası olan *aksdev* adlı bir kullanıcı.
* **Site güvenilirliği mühendisi**
    * *Opssre* grubunun bir parçası olan *akssre* adlı bir kullanıcı.

Üretim ortamlarında, bir Azure AD kiracısı içinde mevcut kullanıcıları ve grupları kullanabilirsiniz.

İlk olarak, [az aks Show][az-aks-show] komutunu kullanarak aks KÜMENIZIN kaynak kimliğini alın. Kaynak KIMLIĞINI *AKS_ID* adlı bir değişkene atayın, böylece ek komutlarda başvurulabilirler.

```azurecli-interactive
AKS_ID=$(az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --query id -o tsv)
```

[Az Ad Group Create][az-ad-group-create] komutunu kullanarak uygulama geliştiricileri IÇIN Azure AD 'de ilk örnek grubunu oluşturun. Aşağıdaki örnek *AppDev*adlı bir grup oluşturur:

```azurecli-interactive
APPDEV_ID=$(az ad group create --display-name appdev --mail-nickname appdev --query objectId -o tsv)
```

Şimdi, [az role atama Create][az-role-assignment-create] komutunu kullanarak *AppDev* grubu için bir Azure rol ataması oluşturun. Bu atama, grubun herhangi bir üyesinin `kubectl` *Azure Kubernetes hizmet kümesi Kullanıcı rolü*vererek bir aks kümesiyle etkileşime geçmesini sağlar.

```azurecli-interactive
az role assignment create \
  --assignee $APPDEV_ID \
  --role "Azure Kubernetes Service Cluster User Role" \
  --scope $AKS_ID
```

> [!TIP]
> Gibi bir hata alırsanız `Principal 35bfec9328bd4d8d9b54dea6dac57b82 does not exist in the directory a5443dcd-cd0e-494d-a387-3039b419f0d5.` , Azure AD grubu nesne kimliğinin dizin aracılığıyla yayılması için birkaç saniye bekleyin ve sonra `az role assignment create` komutu tekrar deneyin.

İkinci bir örnek grup oluşturun; Bu bir tane, *opssre*adlı SRES için:

```azurecli-interactive
OPSSRE_ID=$(az ad group create --display-name opssre --mail-nickname opssre --query objectId -o tsv)
```

Daha sonra, *Azure Kubernetes hizmet kümesi Kullanıcı rolünü*grubun üyelerine vermek Için bir Azure rol ataması oluşturun:

```azurecli-interactive
az role assignment create \
  --assignee $OPSSRE_ID \
  --role "Azure Kubernetes Service Cluster User Role" \
  --scope $AKS_ID
```

## <a name="create-demo-users-in-azure-ad"></a>Azure AD 'de tanıtım kullanıcıları oluşturma

Uygulama geliştiricilerimiz ve SREs için Azure AD 'de oluşturulan iki örnek grup ile, artık iki örnek kullanıcı oluşturmaya izin verir. Makalenin sonundaki RBAC tümleştirmesini test etmek için, AKS kümesinde bu hesaplarla oturum açın.

[Az ad User Create][az-ad-user-create] komutunu kullanarak Azure AD 'de ilk kullanıcı hesabını oluşturun.

Aşağıdaki örnek, görünen ad *aks dev* ve Kullanıcı asıl adı (UPN) olan bir kullanıcı oluşturur `aksdev@contoso.com` . UPN 'yi Azure AD kiracınız için doğrulanmış bir etki alanı içerecek şekilde güncelleştirin ( *contoso.com* yerine kendi etki alanınızı değiştirin) ve kendi güvenli kimlik bilgilerinizi sağlayın `--password` :

```azurecli-interactive
AKSDEV_ID=$(az ad user create \
  --display-name "AKS Dev" \
  --user-principal-name aksdev@contoso.com \
  --password P@ssw0rd1 \
  --query objectId -o tsv)
```

Şimdi, [az Ad Group member Add][az-ad-group-member-add] komutunu kullanarak kullanıcıyı önceki bölümde oluşturulan *AppDev* grubuna ekleyin:

```azurecli-interactive
az ad group member add --group appdev --member-id $AKSDEV_ID
```

İkinci bir kullanıcı hesabı oluşturun. Aşağıdaki örnek, görünen adı *aks SRE* ve Kullanıcı asıl adı (UPN) olan bir kullanıcı oluşturur `akssre@contoso.com` . Daha sonra, UPN 'yi Azure AD kiracınız için doğrulanmış bir etki alanı içerecek şekilde güncelleştirin ( *contoso.com* yerine kendi etki alanınızı değiştirin) ve kendi güvenli kimlik bilgilerinizi sağlayın `--password` :

```azurecli-interactive
# Create a user for the SRE role
AKSSRE_ID=$(az ad user create \
  --display-name "AKS SRE" \
  --user-principal-name akssre@contoso.com \
  --password P@ssw0rd1 \
  --query objectId -o tsv)

# Add the user to the opssre Azure AD group
az ad group member add --group opssre --member-id $AKSSRE_ID
```

## <a name="create-the-aks-cluster-resources-for-app-devs"></a>Uygulama geliştiriciler için aks kümesi kaynakları oluşturma

Azure AD grupları ve kullanıcılar artık oluşturulmuştur. Grup üyelerinin, bir AKS kümesine normal kullanıcı olarak bağlanması için Azure rol atamaları oluşturulmuştur. Şimdi bu farklı grupların belirli kaynaklara erişmesine izin vermek için AKS kümesini yapılandıralim.

İlk olarak, [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanarak küme yöneticisi kimlik bilgilerini alın. Aşağıdaki bölümlerden birinde, Azure AD kimlik doğrulama akışını eylemde görmek için normal *Kullanıcı* kümesi kimlik bilgilerini alırsınız.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

[Kubectl Create Namespace][kubectl-create] komutunu kullanarak aks kümesinde bir ad alanı oluşturun. Aşağıdaki örnek, bir ad alanı adı *geliştirme*oluşturur:

```console
kubectl create namespace dev
```

Kubernetes 'te *Roller* verilecek izinleri tanımlar ve *rolebindings* bunları istenen kullanıcılara veya gruplara uygular. Bu atamalar, belirli bir ad alanına veya tüm küme genelinde uygulanabilir. Daha fazla bilgi için bkz. [RBAC yetkilendirmesi kullanma][rbac-authorization].

İlk olarak, *dev* ad alanı Için bir rol oluşturun. Bu rol, ad alanına tam izinler verir. Üretim ortamlarında, farklı kullanıcılar veya gruplar için daha ayrıntılı izinler belirtebilirsiniz.

Adlı bir dosya oluşturun `role-dev-namespace.yaml` ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: dev-user-full-access
  namespace: dev
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: ["batch"]
  resources:
  - jobs
  - cronjobs
  verbs: ["*"]
```

[Kubectl Apply][kubectl-apply] komutunu kullanarak rolü oluşturun ve YAML bildiriminizde dosya adını belirtin:

```console
kubectl apply -f role-dev-namespace.yaml
```

Ardından, [az Ad Group Show][az-ad-group-show] komutunu kullanarak *AppDev* grubunun kaynak kimliğini alın. Bu grup bir sonraki adımda RoleBinding konusunun konusu olarak ayarlanır.

```azurecli-interactive
az ad group show --group appdev --query objectId -o tsv
```

Şimdi, ad alanı erişimi için önceden oluşturulmuş rolü kullanmak üzere *AppDev* grubu Için bir rolebinding oluşturun. Adlı bir dosya oluşturun `rolebinding-dev-namespace.yaml` ve aşağıdaki YAML bildirimini yapıştırın. Son satırda, *Groupobjectıd* ' yi önceki komuttan gelen nesne kimliği çıkışıyla değiştirin:

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: dev-user-access
  namespace: dev
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: dev-user-full-access
subjects:
- kind: Group
  namespace: dev
  name: groupObjectId
```

[Kubectl Apply][kubectl-apply] komutunu kullanarak rolebinding oluşturun ve YAML bildiriminizde dosya adını belirtin:

```console
kubectl apply -f rolebinding-dev-namespace.yaml
```

## <a name="create-the-aks-cluster-resources-for-sres"></a>SREs için AKS kümesi kaynakları oluşturma

Şimdi, SREs için bir ad alanı, rol ve RoleBinding oluşturmak üzere önceki adımları tekrarlayın.

İlk olarak, [kubectl Create Namespace][kubectl-create] komutunu kullanarak *SRE* için bir ad alanı oluşturun:

```console
kubectl create namespace sre
```

Adlı bir dosya oluşturun `role-sre-namespace.yaml` ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: sre-user-full-access
  namespace: sre
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: ["batch"]
  resources:
  - jobs
  - cronjobs
  verbs: ["*"]
```

[Kubectl Apply][kubectl-apply] komutunu kullanarak rolü oluşturun ve YAML bildiriminizde dosya adını belirtin:

```console
kubectl apply -f role-sre-namespace.yaml
```

[Az Ad Group Show][az-ad-group-show] komutunu kullanarak *opssre* grubunun kaynak kimliğini alın:

```azurecli-interactive
az ad group show --group opssre --query objectId -o tsv
```

*Opssre* grubu için, ad alanı erişimi için önceden oluşturulmuş rolü kullanmak üzere bir rolebinding oluşturun. Adlı bir dosya oluşturun `rolebinding-sre-namespace.yaml` ve aşağıdaki YAML bildirimini yapıştırın. Son satırda, *Groupobjectıd* ' yi önceki komuttan gelen nesne kimliği çıkışıyla değiştirin:

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: sre-user-access
  namespace: sre
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: sre-user-full-access
subjects:
- kind: Group
  namespace: sre
  name: groupObjectId
```

[Kubectl Apply][kubectl-apply] komutunu kullanarak rolebinding oluşturun ve YAML bildiriminizde dosya adını belirtin:

```console
kubectl apply -f rolebinding-sre-namespace.yaml
```

## <a name="interact-with-cluster-resources-using-azure-ad-identities"></a>Azure AD kimliklerini kullanarak küme kaynaklarıyla etkileşim kurma

Şimdi, bir AKS kümesinde kaynak oluşturup yönetirken beklenen izinlerin çalışmasını test edelim. Bu örneklerde, Pod 'yi kullanıcının atanan ad alanında zamanlar ve görüntüleyebilirsiniz. Ardından, atanan ad alanının dışında Pod 'yi zamanlamayı ve görüntülemeyi deneyolursunuz.

İlk olarak, [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanarak *kubeconfig* bağlamını sıfırlayın. Önceki bölümde, Küme Yöneticisi kimlik bilgilerini kullanarak bağlamı ayarlarsınız. Yönetici Kullanıcı Azure AD oturum açma istemlerini atlar. Parametresi olmadan `--admin` , tüm Isteklerin Azure AD ile doğrulanmasını gerektiren Kullanıcı bağlamı uygulanır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --overwrite-existing
```

*Geliştirme* ad alanındaki [kubectl Run][kubectl-run] komutunu kullanarak temel NGINX Pod 'u zamanlayın:

```console
kubectl run nginx-dev --image=nginx --namespace dev
```

Oturum açma isteminde, makalenin başlangıcında oluşturulan kendi hesabınızın kimlik bilgilerini girin `appdev@contoso.com` . Başarıyla oturum açtıktan sonra, hesap belirteci gelecekteki komutlar için önbelleğe alınır `kubectl` . Aşağıdaki örnek çıktıda gösterildiği gibi NGıNX başarıyla zamanlanır:

```console
$ kubectl run nginx-dev --image=nginx --namespace dev

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code B24ZD6FP8 to authenticate.

pod/nginx-dev created
```

Şimdi, *dev* ad alanındaki Pod 'yi görüntülemek için [kubectl Get Pod][kubectl-get] komutunu kullanın.

```console
kubectl get pods --namespace dev
```

Aşağıdaki örnek çıktıda gösterildiği gibi, NGıNX Pod, başarıyla *çalışıyor*:

```console
$ kubectl get pods --namespace dev

NAME        READY   STATUS    RESTARTS   AGE
nginx-dev   1/1     Running   0          4m
```

### <a name="create-and-view-cluster-resources-outside-of-the-assigned-namespace"></a>Atanan ad alanının dışında küme kaynakları oluşturma ve görüntüleme

Şimdi de *dev* ad alanı dışında Pod görüntülemeyi deneyin. Şu şekilde görmek için [kubectl Get Pod][kubectl-get] komutunu tekrar kullanın `--all-namespaces` :

```console
kubectl get pods --all-namespaces
```

Kullanıcının grup üyeliğinde, bu eyleme izin veren bir Kubernetes rolü yoktur, çünkü aşağıdaki örnek çıktıda gösterildiği gibi:

```console
$ kubectl get pods --all-namespaces

Error from server (Forbidden): pods is forbidden: User "aksdev@contoso.com" cannot list resource "pods" in API group "" at the cluster scope
```

Aynı şekilde, farklı bir ad alanında (örneğin, *SRE* ad alanı) bir pod zamanlamaya çalışın. Aşağıdaki örnek çıktıda gösterildiği gibi, kullanıcının grup üyeliği bir Kubernetes rolü ve RoleBinding ile hizalanmaz:

```console
$ kubectl run nginx-dev --image=nginx --namespace sre

Error from server (Forbidden): pods is forbidden: User "aksdev@contoso.com" cannot create resource "pods" in API group "" in the namespace "sre"
```

### <a name="test-the-sre-access-to-the-aks-cluster-resources"></a>AKS kümesi kaynaklarına erişimi test etme

Azure AD grup üyeliğimizin ve Kubernetes RBAC 'in farklı kullanıcılar ve gruplar arasında doğru şekilde çalıştığını onaylamak için, *opssre* kullanıcısı olarak oturum açıldığında önceki komutları deneyin.

[Diğer aks Get-Credentials][az-aks-get-credentials] komutunu kullanarak *kubeconfig* bağlamını sıfırlayın ve bu, daha önce önbelleğe alınmış kimlik doğrulama belirtecini *aksdev* kullanıcısına temizler:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --overwrite-existing
```

Atanan *SRE* ad alanında Pod 'yi zamanlamayı ve görüntülemeyi deneyin. İstendiğinde, `opssre@contoso.com` makalenin başlangıcında oluşturduğunuz kendi kimlik bilgilerinizle oturum açın:

```console
kubectl run nginx-sre --image=nginx --namespace sre
kubectl get pods --namespace sre
```

Aşağıdaki örnek çıktıda gösterildiği gibi, pods 'yi başarıyla oluşturabilir ve görüntüleyebilirsiniz:

```console
$ kubectl run nginx-sre --image=nginx --namespace sre

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BM4RHP3FD to authenticate.

pod/nginx-sre created

$ kubectl get pods --namespace sre

NAME        READY   STATUS    RESTARTS   AGE
nginx-sre   1/1     Running   0
```

Şimdi, atanmış SRE ad alanı dışında bir pod görüntülemeyi veya zamanlamayı deneyin:

```console
kubectl get pods --all-namespaces
kubectl run nginx-sre --image=nginx --namespace dev
```

`kubectl`Aşağıdaki örnek çıktıda gösterildiği gibi bu komutlar başarısız olur. Kullanıcının grup üyeliği ve Kubernetes rolü ve RoleBindings, diğer ad alanlarında kaynak oluşturma veya Yönetici kaynakları için izin vermez:

```console
$ kubectl get pods --all-namespaces
Error from server (Forbidden): pods is forbidden: User "akssre@contoso.com" cannot list pods at the cluster scope

$ kubectl run nginx-sre --image=nginx --namespace dev
Error from server (Forbidden): pods is forbidden: User "akssre@contoso.com" cannot create pods in the namespace "dev"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, Azure AD 'de AKS kümesinde ve kullanıcılar ve gruplar 'daki kaynakları oluşturdunuz. Tüm bu kaynakları temizlemek için aşağıdaki komutları çalıştırın:

```azurecli-interactive
# Get the admin kubeconfig context to delete the necessary cluster resources
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin

# Delete the dev and sre namespaces. This also deletes the pods, Roles, and RoleBindings
kubectl delete namespace dev
kubectl delete namespace sre

# Delete the Azure AD user accounts for aksdev and akssre
az ad user delete --upn-or-object-id $AKSDEV_ID
az ad user delete --upn-or-object-id $AKSSRE_ID

# Delete the Azure AD groups for appdev and opssre. This also deletes the Azure role assignments.
az ad group delete --group appdev
az ad group delete --group opssre
```

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes kümelerinin güvenliğini sağlama hakkında daha fazla bilgi için bkz. [AKS Için erişim ve kimlik seçenekleri][rbac-authorization].

Kimlik ve kaynak denetiminde en iyi uygulamalar için bkz. [AKS 'de kimlik doğrulama ve yetkilendirme Için en iyi uygulamalar][operator-best-practices-identity].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-run]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#run

<!-- LINKS - internal -->
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[install-azure-cli]: /cli/azure/install-azure-cli
[azure-ad-aks-cli]: azure-ad-integration-cli.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-ad-group-create]: /cli/azure/ad/group#az-ad-group-create
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-ad-user-create]: /cli/azure/ad/user#az-ad-user-create
[az-ad-group-member-add]: /cli/azure/ad/group/member#az-ad-group-member-add
[az-ad-group-show]: /cli/azure/ad/group#az-ad-group-show
[rbac-authorization]: concepts-identity.md#kubernetes-role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
