---
title: Azure Kubernetes Hizmetinde (AKS) kubeconfig'e erişimi sınırlandırın
description: Küme yöneticileri ve küme kullanıcıları için Kubernetes yapılandırma dosyasına (kubeconfig) erişimi nasıl kontrol edeceğinizi öğrenin
services: container-service
ms.topic: article
ms.date: 01/28/2020
ms.openlocfilehash: 25c710cce2855d6af985d3f46082f47573bbc101
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259558"
---
# <a name="use-azure-role-based-access-controls-to-define-access-to-the-kubernetes-configuration-file-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti'ndeki (AKS) Kubernetes yapılandırma dosyasına erişimi tanımlamak için Azure rol tabanlı erişim denetimlerini kullanın

`kubectl` Aracı kullanarak Kubernetes kümeleriyle etkileşimkurabilirsiniz. Azure CLI, AKS kümelerinize bağlanmak için erişim kimlik bilgilerini ve `kubectl`yapılandırma bilgilerini elde etmenin kolay bir yolunu sağlar. Kubernetes yapılandırma *(kubeconfig)* bilgilerini kimlerin alabileceklerini sınırlamak ve daha sonra sahip oldukları izinleri sınırlamak için Azure rol tabanlı erişim denetimlerini (RBAC) kullanabilirsiniz.

Bu makalede, bir AKS kümesi için yapılandırma bilgilerini kimlerin alabileceğinizi sınırlayan RBAC rolleri nasıl atayabileceğinizi gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Bu makalede, Azure CLI sürümünü 2.0.65 veya daha sonra çalıştırdığınızda da vardır. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="available-cluster-roles-permissions"></a>Kullanılabilir küme rolleri izinleri

Aracı kullanarak `kubectl` bir AKS kümesiyle etkileşimkurduğunuzda, küme bağlantısı bilgilerini tanımlayan bir yapılandırma dosyası kullanılır. Bu yapılandırma dosyası genellikle *~/.kube/config'de*depolanır. Bu *kubeconfig* dosyasında birden çok küme tanımlanabilir. [Kubectl config use-context][kubectl-config-use-context] komutunu kullanarak kümeler arasında geçiş yapabilirsiniz.

[Az aks get-credentials][az-aks-get-credentials] komutu bir AKS kümesi için erişim kimlik bilgilerini almanızı sağlar ve *bunları kubeconfig* dosyasında birleştirir. Bu kimlik bilgilerine erişimi denetlemek için Azure rol tabanlı erişim denetimlerini (RBAC) kullanabilirsiniz. Bu Azure RBAC rolleri, *kubeconfig* dosyasını kimlerin alabileceklerini ve küme içinde hangi izinlere sahip olduklarını tanımlamanıza izin verir.

İki yerleşik rol şunlardır:

* **Azure Kubernetes Hizmet Kümesi Yönetici Rolü**  
  * *Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action* API çağrısına erişim sağlar. Bu API [aramaküme yöneticisi kimlik bilgilerini listeler.][api-cluster-admin]
  * *ClusterAdmin* rolü için *kubeconfig* indirin.
* **Azure Kubernetes Hizmet Kümesi Kullanıcı Rolü**
  * *Microsoft.ContainerService/managedClusters/listClusterUserCredential/action* API çağrısına erişim sağlar. Bu API [araması küme kullanıcı kimlik bilgilerini listeler.][api-cluster-user]
  * *ClusterUser* rolü için *kubeconfig* indirin.

Bu RBAC rolleri bir Azure Etkin Dizin (AD) kullanıcısına veya grubuna uygulanabilir.

> ! [NOT] Azure AD kullanan kümelerde, *clusterUser* rolüne sahip kullanıcılar, oturum açmayı gerektiren boş bir *kubeconfig* dosyasına sahiptir. Oturum açtıktan sonra, kullanıcılar Azure AD kullanıcı veya grup ayarlarına göre erişebilirler. *clusterAdmin* rolüne sahip kullanıcılar yönetici erişimine sahiptir.
>
> Azure AD kullanmayan kümeler yalnızca *clusterAdmin* rolünü kullanır.

## <a name="assign-role-permissions-to-a-user-or-group"></a>Bir kullanıcıya veya gruba rol izinleri atama

Kullanılabilir rollerden birini atamak için AKS kümesinin kaynak kimliğini ve Azure AD kullanıcı hesabının veya grubunun kimliğini almanız gerekir. Aşağıdaki örnek komutlar:

* *myResourceGroup* kaynak grubunda *myAKSCluster* adlı kümenin [az aks göster][az-aks-show] komutunu kullanarak küme kaynak kimliğini alın. Gerektiğinde kendi küme ve kaynak grup adınızı sağlayın.
* Kullanıcı kimliğinizi almak için [az hesap gösterisini][az-account-show] ve [az reklam kullanıcı göster][az-ad-user-show] komutlarını kullanın.
* Son olarak, [az rol atama oluşturma][az-role-assignment-create] komutunu kullanarak bir rol atama.

Aşağıdaki örnek, *Azure Kubernetes Hizmet Kümesi Yöneticisi Rolünü* tek bir kullanıcı hesabına atar:

```azurecli-interactive
# Get the resource ID of your AKS cluster
AKS_CLUSTER=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query id -o tsv)

# Get the account credentials for the logged in user
ACCOUNT_UPN=$(az account show --query user.name -o tsv)
ACCOUNT_ID=$(az ad user show --id $ACCOUNT_UPN --query objectId -o tsv)

# Assign the 'Cluster Admin' role to the user
az role assignment create \
    --assignee $ACCOUNT_ID \
    --scope $AKS_CLUSTER \
    --role "Azure Kubernetes Service Cluster Admin Role"
```

> [!TIP]
> Bir Azure REKLAM grubuna izin atamak istiyorsanız, `--assignee` önceki örnekte gösterilen parametreyi *kullanıcı*yerine *grup* için nesne kimliğiyle güncelleştirin. Bir grup için nesne kimliğini elde etmek için [az reklam grubu göster][az-ad-group-show] komutunu kullanın. Aşağıdaki örnek, *appdev*adlı Azure REKLAM grubunun nesne kimliğini alır:`az ad group show --group appdev --query objectId -o tsv`

Önceki atamayı gerektiğinde *Küme Kullanıcı Rolü* olarak değiştirebilirsiniz.

Aşağıdaki örnek çıktı, rol atamasının başarıyla oluşturulduğunu gösterir:

```
{
  "canDelegate": null,
  "id": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster/providers/Microsoft.Authorization/roleAssignments/b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "name": "b2712174-5a41-4ecb-82c5-12b8ad43d4fb",
  "principalId": "946016dd-9362-4183-b17d-4c416d1f8f61",
  "resourceGroup": "myResourceGroup",
  "roleDefinitionId": "/subscriptions/<guid>/providers/Microsoft.Authorization/roleDefinitions/0ab01a8-8aac-4efd-b8c2-3ee1fb270be8",
  "scope": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-and-verify-the-configuration-information"></a>Yapılandırma bilgilerini alma ve doğrulama

RBAC rolleri atanmış, AKS kümeniz için *kubeconfig* tanımını almak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Aşağıdaki örnek, kullanıcıya *Cluster Admin Rolü*verilmişse doğru çalışan *--yönetici* kimlik bilgilerini alır:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Daha sonra kümenin *bağlamında* yönetici yapılandırma bilgilerinin uygulandığını gösterdiğini doğrulamak için [kubectl config view][kubectl-config-view] komutunu kullanabilirsiniz:

```
$ kubectl config view

apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://myaksclust-myresourcegroup-19da35-4839be06.hcp.eastus.azmk8s.io:443
  name: myAKSCluster
contexts:
- context:
    cluster: myAKSCluster
    user: clusterAdmin_myResourceGroup_myAKSCluster
  name: myAKSCluster-admin
current-context: myAKSCluster-admin
kind: Config
preferences: {}
users:
- name: clusterAdmin_myResourceGroup_myAKSCluster
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
    token: e9f2f819a4496538b02cefff94e61d35
```

## <a name="remove-role-permissions"></a>Rol izinlerini kaldırma

Rol atamalarıkaldırmak için [az rol atama silme][az-role-assignment-delete] komutunu kullanın. Önceki komutlarda elde edilen hesap kimliği ve küme kaynak kimliğini belirtin. Rolü kullanıcı yerine bir gruba atadıysanız, parametre için hesap nesnekimliği yerine `--assignee` uygun grup nesne kimliği belirtin:

```azurecli-interactive
az role assignment delete --assignee $ACCOUNT_ID --scope $AKS_CLUSTER
```

## <a name="next-steps"></a>Sonraki adımlar

AKS kümelerine erişimde daha yüksek güvenlik için [Azure Active Directory kimlik doğrulamasını tümleştirin.][aad-integration]

<!-- LINKS - external -->
[kubectl-config-use-context]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config
[kubectl-config-view]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[api-cluster-admin]: /rest/api/aks/managedclusters/listclusteradmincredentials
[api-cluster-user]: /rest/api/aks/managedclusters/listclusterusercredentials
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-account-show]: /cli/azure/account#az-account-show
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-role-assignment-delete]: /cli/azure/role/assignment#az-role-assignment-delete
[aad-integration]: azure-ad-integration.md
[az-ad-group-show]: /cli/azure/ad/group#az-ad-group-show
