---
title: Azure’da Kubernetes öğreticisi - Kümeyi dağıtma
description: Bu Azure Kubernetes Service (AKS) öğreticisinde bir AKS kümesi oluşturacak ve kubectl istemcisini kullanarak Kubernetes ana düğümüne bağlanacaksınız.
services: container-service
ms.topic: tutorial
ms.date: 02/25/2020
ms.custom: mvc
ms.openlocfilehash: 609ac66ca27d5cad7dd2fb295c3a2a721a1cda16
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81392704"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>Hızlı Başlangıç: Azure Kubernetes Hizmeti (AKS) kümesini dağıtma

Kubernetes, kapsayıcılı uygulamalar için dağıtılmış bir platform sunar. AKS ile, hızlı bir üretime hazır Kubernetes küme oluşturabilirsiniz. Yedi parçalık bu öğreticinin üçüncü kısmında, AKS içinde bir Kubernetes kümesi dağıtılır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defterine kimlik doğrulaması yapabilen bir Kubernetes AKS kümesini dağıtma
> * Kubernetes CLI (kubectl) yükleme
> * kubectl istemcisini AKS kümenize bağlanacak şekilde yapılandırma

Ek öğreticilerde, Azure Oy uygulaması kümeye dağıtılır, ölçeklendirilir ve güncelleştirilir.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir kapsayıcı görüntüsü oluşturuldu ve Azure Container Registry örneğine yüklendi. Bu adımları yapmadıysanız ve takip etmek istiyorsanız, Tutorial [1 'den][aks-tutorial-prepare-app]başlayın – Kapsayıcı görüntüleri oluşturun.

Bu öğretici, Azure CLI sürümünü 2.0.53 veya daha sonra çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-a-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

AKS kümeleri Kubernetes rol tabanlı erişim denetimlerini (RBAC) kullanabilir. Bu denetimler, kullanıcılara atanmış olan rollere göre kaynaklara erişim vermenizi sağlayabilir. Bir kullanıcıya birden çok rol atanmışsa izinler birleştirilir ve izinler tek bir ad alanına veya tüm kümeye kapsamlandırılabilir. Bir AKS kümesi oluşturduğunuzda Azure CLI varsayılan ayarlarda RBAC özelliğini otomatik olarak etkinleştirir.

[az aks create][] komutunu kullanarak bir AKS kümesi oluşturun. Aşağıdaki örnek, *myResourceGroup* adlı kaynak grubunda *myAKSCluster* adlı bir küme oluşturur. Bu kaynak grubu [önceki öğreticide][aks-tutorial-prepare-acr]oluşturuldu. Aks kümesinin diğer Azure kaynaklarıyla etkileşim kurabilmesi için, bir Azure Etkin Dizin hizmet ilkesi otomatik olarak oluşturulur, çünkü siz belirtmediğiniz için. Burada, bu hizmet sorumlusuna önceki öğreticide oluşturduğunuz Azure Kapsayıcı Kayıt Defteri (ACR) örneğinden [resim çekme hakkı verilir.][container-registry-integration] Daha kolay yönetim için hizmet sorumlusu yerine yönetilen bir [kimlik](use-managed-identity.md) kullanabileceğinizi unutmayın.

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 2 \
    --generate-ssh-keys \
    --attach-acr <acrName>
```

Ayrıca, ACR'den görüntü çekmek için bir hizmet ilkesini el ile yapılandırabilirsiniz. Daha fazla bilgi için, [hizmet ilkeleriyle ACR kimlik doğrulaması](../container-registry/container-registry-auth-service-principal.md) veya [Kubernetes'ten bir çekme sırrıyla kimlik doğrulaması'na](../container-registry/container-registry-auth-kubernetes.md)bakın.

Birkaç dakika sonra dağıtım tamamlar ve AKS dağıtımı yla ilgili JSON biçimli bilgileri döndürür.

> [!NOTE]
> Kümenizin güvenilir çalışmasını sağlamak için en az 2 (iki) düğüm çalıştırmalısınız.

## <a name="install-the-kubernetes-cli"></a>Kubernetes CLI'yi yükleme

Yerel bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([kubectl][kubectl]) kullanmanız gerekir.

Azure Cloud Shell'i kullanıyorsanız `kubectl` zaten yüklüdür. [az aks install-cli][] komutunu kullanarak da yerel ortama yükleyebilirsiniz:

```azurecli
az aks install-cli
```

## <a name="connect-to-cluster-using-kubectl"></a>kubectl istemcisini kullanarak kümeye bağlanma

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][] komutunu kullanın. Aşağıdaki örnek, *myResourceGroup'ta* *myAKSCluster* adlı AKS kümesi için kimlik bilgilerini alır:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize olan bağlantıyı doğrulamak için küme düğümlerinin listesini döndürmek için [kubectl get düğümleri][kubectl-get] komutunu çalıştırın:

```
$ kubectl get nodes

NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-12345678-0   Ready    agent   32m   v1.14.8
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide AKS'de bir Kubernetes kümesi dağıttınız ve `kubectl` istemcisini ona bağlanacak şekilde yapılandırdınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defterine kimlik doğrulaması yapabilen bir Kubernetes AKS kümesini dağıtma
> * Kubernetes CLI (kubectl) yükleme
> * kubectl istemcisini AKS kümenize bağlanacak şekilde yapılandırma

Kümeye uygulama dağıtmayı öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes'te uygulama dağıtma][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az ad sp create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az acr show]: /cli/azure/acr#az-acr-show
[az role assignment create]: /cli/azure/role/assignment#az-role-assignment-create
[az aks create]: /cli/azure/aks#az-aks-create
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-cli-install]: /cli/azure/install-azure-cli
[container-registry-integration]: ./cluster-container-registry-integration.md
