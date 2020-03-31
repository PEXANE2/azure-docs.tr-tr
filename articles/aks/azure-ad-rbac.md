---
title: Azure Kubernetes Hizmetinde RBAC ve Azure AD ile küme kaynaklarını denetleme
description: Azure Kubernetes Hizmeti'nde (AKS) rol tabanlı erişim denetimi (RBAC) kullanarak küme kaynaklarına erişimi kısıtlamak için Azure Active Directory grup üyeliğini nasıl kullanacağınızı öğrenin
services: container-service
ms.topic: article
ms.date: 04/16/2019
ms.openlocfilehash: 456b6dcdd590b48e06c830db85b726d4bebb69e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596530"
---
# <a name="control-access-to-cluster-resources-using-role-based-access-control-and-azure-active-directory-identities-in-azure-kubernetes-service"></a>Azure Kubernetes Hizmeti'nde rol tabanlı erişim denetimi ve Azure Etkin Dizin kimliklerini kullanarak küme kaynaklarına erişimi denetleme

Azure Kubernetes Hizmeti (AKS), kullanıcı kimlik doğrulaması için Azure Active Directory (AD) kullanacak şekilde yapılandırılabilir. Bu yapılandırmada, Azure AD kimlik doğrulama belirteci kullanarak bir AKS kümesinde oturum açabilirsiniz. Ayrıca, kubernetes rol tabanlı erişim denetimini (RBAC) kullanıcının kimliğini veya grup üyeliğini temel alan küme kaynaklarına erişimi sınırlamak için yapılandırabilirsiniz.

Bu makalede, aks kümesinde Kubernetes RBAC kullanarak ad alanlarına ve küme kaynaklarına erişimi denetlemek için Azure AD grup üyeliğini nasıl kullanacağınızı gösterilmektedir. Örnek gruplar ve kullanıcılar Azure AD'de oluşturulur, ardından aks kümesinde, kaynak oluşturmak ve görüntülemek için uygun izinleri vermek için Roller ve RoleBindings oluşturulur.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure AD tümleştirmesi ile etkinleştirilmiş varolan bir AKS kümeniz olduğunu varsayar. AKS kümesine ihtiyacınız varsa, [Azure Etkin Dizini AKS ile tümleştir'e][azure-ad-aks-cli]bakın.

Azure CLI sürümü 2.0.61 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

## <a name="create-demo-groups-in-azure-ad"></a>Azure AD'de demo grupları oluşturma

Bu makalede, Kubernetes RBAC ve Azure AD'nin küme kaynaklarına erişimi nasıl denetlediğini göstermek için kullanılabilecek iki kullanıcı rolü oluşturalım. Aşağıdaki iki örnek rol kullanılır:

* **Uygulama geliştiricisi**
    * *Appdev* grubunun bir parçası olan *aksdev* adlı bir kullanıcı.
* **Site güvenilirlik mühendisi**
    * *Opssre* grubunun bir parçası olan *akssre* adlı bir kullanıcı.

Üretim ortamlarında, bir Azure AD kiracısında ki varolan kullanıcıları ve grupları kullanabilirsiniz.

İlk olarak, [az aks göster][az-aks-show] komutunu kullanarak AKS kümenizin kaynak kimliğini alın. Ek komutlarda başvurulabilmesi için kaynak kimliğini *AKS_ID* adlı bir değişkene atayın.

```azurecli-interactive
AKS_ID=$(az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --query id -o tsv)
```

Az reklam grubu oluşturma komutunu kullanarak uygulama geliştiricileri için Azure AD'de ilk örnek grubu [oluşturun.][az-ad-group-create] Aşağıdaki örnek *appdev*adlı bir grup oluşturur:

```azurecli-interactive
APPDEV_ID=$(az ad group create --display-name appdev --mail-nickname appdev --query objectId -o tsv)
```

Şimdi, az rol atamaoluşturma komutunu kullanarak *appdev* grubu için bir Azure [rol ataması oluşturun.][az-role-assignment-create] Bu atama, grubun `kubectl` herhangi bir üyesinin bir AKS kümesiyle azure *Kubernetes Hizmet Kümesi Kullanıcı Rolü*vererek etkileşimde kullanılmasını sağlar.

```azurecli-interactive
az role assignment create \
  --assignee $APPDEV_ID \
  --role "Azure Kubernetes Service Cluster User Role" \
  --scope $AKS_ID
```

> [!TIP]
> `Principal 35bfec9328bd4d8d9b54dea6dac57b82 does not exist in the directory a5443dcd-cd0e-494d-a387-3039b419f0d5.`Azure AD grup nesnesi kimliğinin dizin boyunca yayılmasını sağlamak için birkaç saniye bekleyin `az role assignment create` ve ardından komutu yeniden deneyin gibi bir hata alırsanız.

Ikinci bir örnek grup oluşturun, *opssre*adlı Kobİ'ler için bu:

```azurecli-interactive
OPSSRE_ID=$(az ad group create --display-name opssre --mail-nickname opssre --query objectId -o tsv)
```

Yine, grubun üyelerine *Azure Kubernetes Hizmet Kümesi Kullanıcı Rolü*vermek için bir Azure rol ataması oluşturun:

```azurecli-interactive
az role assignment create \
  --assignee $OPSSRE_ID \
  --role "Azure Kubernetes Service Cluster User Role" \
  --scope $AKS_ID
```

## <a name="create-demo-users-in-azure-ad"></a>Azure AD'de demo kullanıcıları oluşturma

Uygulama geliştiricilerimiz ve SR'lerimiz için Azure AD'de oluşturulan iki örnek grupla artık iki örnek kullanıcı oluşturmanıza olanak sağlıyor. Makalenin sonundaki RBAC tümleştirmesini sınamak için, bu hesaplarla AKS kümesinde oturum açın.

Az reklam kullanıcı oluşturma komutunu kullanarak Azure AD'deki ilk kullanıcı hesabını [oluşturun.][az-ad-user-create]

Aşağıdaki örnek, *AKS Dev* görüntü adı ve kullanıcı ana adı (UPN) ile bir kullanıcı `aksdev@contoso.com`oluşturur. UPN'yi Azure AD kiracınız için doğrulanmış bir etki alanı içerecek şekilde güncelleştirin *(contoso.com* kendi etki alanınızla değiştirin) ve kendi güvenli `--password` kimlik bilgilerinizi sağlayın:

```azurecli-interactive
AKSDEV_ID=$(az ad user create \
  --display-name "AKS Dev" \
  --user-principal-name aksdev@contoso.com \
  --password P@ssw0rd1 \
  --query objectId -o tsv)
```

Şimdi [kullanıcıyı az reklam grubu üyesi ekleme][az-ad-group-member-add] komutunu kullanarak önceki bölümde oluşturulan *appdev* grubuna ekleyin:

```azurecli-interactive
az ad group member add --group appdev --member-id $AKSDEV_ID
```

İkinci bir kullanıcı hesabı oluşturun. Aşağıdaki örnek, *AKS SRE* görüntü adı ve kullanıcı ana adı (UPN) ile bir kullanıcı `akssre@contoso.com`oluşturur. Yine, UPN'yi Azure AD kiracınız için doğrulanmış bir etki alanı içerecek şekilde `--password` güncelleştirin *(contoso.com* kendi etki alanınızla değiştirin) ve kendi güvenli kimlik bilgilerinizi sağlayın:

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

## <a name="create-the-aks-cluster-resources-for-app-devs"></a>Uygulama devs için AKS küme kaynaklarını oluşturma

Azure REKLAM grupları ve kullanıcılar artık oluşturulur. Grup üyelerinin normal bir kullanıcı olarak aks kümesine bağlanması için azure rol atamaları oluşturuldu. Şimdi, AKS kümesini bu farklı grupların belirli kaynaklara erişmesine izin verecek şekilde yapılandıralım.

İlk olarak, [az aks get-credentials][az-aks-get-credentials] komutunu kullanarak küme yöneticisi kimlik bilgilerini alın. Aşağıdaki bölümlerden birinde, Azure AD kimlik doğrulama akışını iş başında görmek için normal *kullanıcı* kümesi kimlik bilgilerini alırsınız.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

[Kubectl create namespace][kubectl-create] komutunu kullanarak AKS kümesinde bir ad alanı oluşturun. Aşağıdaki örnek bir ad alanı adı *dev*oluşturur:

```console
kubectl create namespace dev
```

Kubernetes'te *Roller,* verilecek izinleri tanımlar ve *RoleBindings* bunları istenen kullanıcılara veya gruplara uygular. Bu atamalar belirli bir ad alanına veya tüm kümeye uygulanabilir. Daha fazla bilgi için [RBAC yetkilendirmesi kullanma'ya][rbac-authorization]bakın.

İlk olarak, *dev* ad alanı için bir Rol oluşturun. Bu rol, ad alanına tam izin verir. Üretim ortamlarında, farklı kullanıcılar veya gruplar için daha ayrıntılı izinler belirtebilirsiniz.

Adlandırılmış `role-dev-namespace.yaml` bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
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

[Kubectl uygula][kubectl-apply] komutunu kullanarak Rolü oluşturun ve YAML manifestonuzun dosya adını belirtin:

```console
kubectl apply -f role-dev-namespace.yaml
```

Ardından, [az reklam grubu göster][az-ad-group-show] komutunu kullanarak *appdev* grubunun kaynak kimliğini alın. Bu grup, bir sonraki adımda rolebinding konusu olarak ayarlanır.

```azurecli-interactive
az ad group show --group appdev --query objectId -o tsv
```

Şimdi, ad alanı erişimi için daha önce oluşturulan Rolü kullanmak için *appdev* grubu için bir RoleBinding oluşturun. Adlandırılmış `rolebinding-dev-namespace.yaml` bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın. Son satırda, *groupObjectId'i* önceki komuttaki grup nesnekimliği çıkışıyla değiştirin:

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
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

[Kubectl uygula][kubectl-apply] komutunu kullanarak RoleBinding'i oluşturun ve YAML manifestonuzun dosya adını belirtin:

```console
kubectl apply -f rolebinding-dev-namespace.yaml
```

## <a name="create-the-aks-cluster-resources-for-sres"></a>Kobİ'ler için AKS küme kaynaklarını oluşturma

Şimdi, SREs için bir ad alanı, rol ve rolebinding oluşturmak için önceki adımları yineleyin.

İlk olarak, [kubectl oluşturma namespace][kubectl-create] komutunu kullanarak *sre* için bir ad alanı oluşturun:

```console
kubectl create namespace sre
```

Adlandırılmış `role-sre-namespace.yaml` bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
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

[Kubectl uygula][kubectl-apply] komutunu kullanarak Rolü oluşturun ve YAML manifestonuzun dosya adını belirtin:

```console
kubectl apply -f role-sre-namespace.yaml
```

[az reklam grubu göster][az-ad-group-show] komutunu kullanarak *opssre* grubunun kaynak kimliğini alın:

```azurecli-interactive
az ad group show --group opssre --query objectId -o tsv
```

Ad alanı erişimi için daha önce oluşturulan Rolü kullanmak için *opssre* grubu için bir RoleBinding oluşturun. Adlandırılmış `rolebinding-sre-namespace.yaml` bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın. Son satırda, *groupObjectId'i* önceki komuttaki grup nesnekimliği çıkışıyla değiştirin:

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
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

[Kubectl uygula][kubectl-apply] komutunu kullanarak RoleBinding'i oluşturun ve YAML manifestonuzun dosya adını belirtin:

```console
kubectl apply -f rolebinding-sre-namespace.yaml
```

## <a name="interact-with-cluster-resources-using-azure-ad-identities"></a>Azure AD kimliklerini kullanarak küme kaynaklarıyla etkileşimde bulunun

Şimdi, aks kümesinde kaynak oluşturup yönettiğiniz zaman beklenen izinlerin çalıştığını test edelim. Bu örneklerde, kullanıcının atanan ad alanında bölmeleri zamanlar ve görüntüleyebilirsiniz. Ardından, atanan ad alanının dışında bölmeleri zamanlamaya ve görüntülemeye çalışırsınız.

İlk olarak, [az aks get-credentials][az-aks-get-credentials] komutunu kullanarak *kubeconfig* bağlamını sıfırla. Önceki bölümde, bağlamı küme yöneticisi kimlik bilgilerini kullanarak ayarlarsınız. Yönetici kullanıcı, istemlerde Azure AD oturum açma komutlarını atlar. `--admin` Parametresi olmadan, tüm isteklerin Azure AD kullanılarak kimlik doğrulaması yapılmasını gerektiren kullanıcı bağlamı uygulanır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --overwrite-existing
```

*Dev* ad alanında [kubectl çalıştır][kubectl-run] komutunu kullanarak temel bir NGINX bölmesini zamanlayın:

```console
kubectl run --generator=run-pod/v1 nginx-dev --image=nginx --namespace dev
```

Oturum açma istemi olarak, makalenin `appdev@contoso.com` başında oluşturulan kendi hesabınızın kimlik bilgilerini girin. Başarılı bir şekilde oturum açtığınızda, hesap belirteci `kubectl` gelecekteki komutlar için önbelleğe alınır. NGINX, aşağıdaki örnek çıktıda gösterildiği gibi başarılı bir zamanlamadır:

```console
$ kubectl run --generator=run-pod/v1 nginx-dev --image=nginx --namespace dev

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code B24ZD6FP8 to authenticate.

pod/nginx-dev created
```

Şimdi *dev* ad alanında bölmeleri görüntülemek için [kubectl get pods][kubectl-get] komutunu kullanın.

```console
kubectl get pods --namespace dev
```

Aşağıdaki örnek çıktıda gösterildiği gibi, NGINX pod başarıyla *Çalışıyor:*

```console
$ kubectl get pods --namespace dev

NAME        READY   STATUS    RESTARTS   AGE
nginx-dev   1/1     Running   0          4m
```

### <a name="create-and-view-cluster-resources-outside-of-the-assigned-namespace"></a>Küme kaynaklarını atanan ad alanının dışında oluşturma ve görüntüleme

Şimdi *dev* ad alanının dışındaki bölmeleri görüntülemeye çalışın. [Kubectl get pods][kubectl-get] komutunu tekrar kullanın, bu kez aşağıdaki gibi görmek `--all-namespaces` için:

```console
kubectl get pods --all-namespaces
```

Kullanıcının grup üyeliğinde, aşağıdaki örnek çıktıda gösterildiği gibi bu eyleme izin veren bir Kubernetes Rolü yoktur:

```console
$ kubectl get pods --all-namespaces

Error from server (Forbidden): pods is forbidden: User "aksdev@contoso.com" cannot list resource "pods" in API group "" at the cluster scope
```

Aynı şekilde, *sre* ad alanı gibi farklı ad alanında bir bölme zamanlamaya çalışın. Kullanıcının grup üyeliği, aşağıdaki örnek çıktıda gösterildiği gibi, bu izinleri vermek için bir Kubernetes Rolü ve RoleBinding ile aynı hizada değildir:

```console
$ kubectl run --generator=run-pod/v1 nginx-dev --image=nginx --namespace sre

Error from server (Forbidden): pods is forbidden: User "aksdev@contoso.com" cannot create resource "pods" in API group "" in the namespace "sre"
```

### <a name="test-the-sre-access-to-the-aks-cluster-resources"></a>AKS küme kaynaklarına SRE erişimini test edin

Azure AD grup üyeliğimizin ve Kubernetes RBAC'ın farklı kullanıcılar ve gruplar arasında doğru çalıştığını doğrulamak *için, opssre* kullanıcısı olarak oturum açtığınızda önceki komutları deneyin.

*Aksdev* kullanıcısı için daha önce önbelleğe alınmış kimlik doğrulama belirteci temizler [az aks get-credentials][az-aks-get-credentials] komutunu kullanarak *kubeconfig* bağlamını sıfırla:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --overwrite-existing
```

Atanan *sre* ad alanında bölmeleri zamanlamayı ve görüntülemeyi deneyin. İstendiğinde, makalenin başında `opssre@contoso.com` oluşturulan kendi kimlik bilgilerinizle oturum açın:

```console
kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace sre
kubectl get pods --namespace sre
```

Aşağıdaki örnek çıktıda gösterildiği gibi, bölmeleri başarıyla oluşturabilir ve görüntüleyebilirsiniz:

```console
$ kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace sre

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BM4RHP3FD to authenticate.

pod/nginx-sre created

$ kubectl get pods --namespace sre

NAME        READY   STATUS    RESTARTS   AGE
nginx-sre   1/1     Running   0
```

Şimdi, atanan SRE ad alanının dışındaki bölmeleri görüntülemeyi veya zamanlamayı deneyin:

```console
kubectl get pods --all-namespaces
kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace dev
```

Bu `kubectl` komutlar, aşağıdaki örnek çıktıda gösterildiği gibi başarısız oldu. Kullanıcının grup üyeliği ve Kubernetes Role and RoleBindings, diğer ad alanlarında kaynak oluşturma veya yöneticilik izni vermez:

```console
$ kubectl get pods --all-namespaces
Error from server (Forbidden): pods is forbidden: User "akssre@contoso.com" cannot list pods at the cluster scope

$ kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace dev
Error from server (Forbidden): pods is forbidden: User "akssre@contoso.com" cannot create pods in the namespace "dev"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, AKS kümesinde kaynaklar ve Azure AD'deki kullanıcılar ve gruplar oluşturdunuz. Tüm bu kaynakları temizlemek için aşağıdaki komutları çalıştırın:

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

Kubernetes kümelerinin nasıl güvenli hale sağaltılması hakkında daha fazla bilgi için [AKS için Erişim ve kimlik seçeneklerine bakın.][rbac-authorization]

Kimlik ve kaynak denetimi ile ilgili en iyi uygulamalar [için AKS'de kimlik doğrulama ve yetkilendirme için en iyi uygulamalara][operator-best-practices-identity]bakın.

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
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
