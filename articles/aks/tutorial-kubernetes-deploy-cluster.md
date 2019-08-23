---
title: Azure’da Kubernetes öğreticisi - Kümeyi dağıtma
description: Bu Azure Kubernetes Service (AKS) öğreticisinde bir AKS kümesi oluşturacak ve kubectl istemcisini kullanarak Kubernetes ana düğümüne bağlanacaksınız.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: e0dea5fcc7860e8653d542653e279010f8ede200
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69898845"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>Öğretici: Azure Kubernetes hizmeti (AKS) kümesi dağıtma

Kubernetes, kapsayıcılı uygulamalar için dağıtılmış bir platform sunar. AKS ile hızlı bir şekilde üretim için hazır bir Kubernetes kümesi oluşturabilirsiniz. Yedi parçalık bu öğreticinin üçüncü kısmında, AKS içinde bir Kubernetes kümesi dağıtılır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Kaynak etkileşimleri için hizmet sorumlusu oluşturma
> * Kubernetes AKS kümesini dağıtma
> * Kubernetes CLI (kubectl) yükleme
> * kubectl istemcisini AKS kümenize bağlanacak şekilde yapılandırma

Ek öğreticilerde, Azure oy uygulaması kümeye dağıtılır, ölçeklendirilir ve güncelleştirilir.

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir kapsayıcı görüntüsü oluşturuldu ve Azure Container Registry örneğine yüklendi. Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [öğretici 1 – kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app]' dan başlayın.

Bu öğreticide, Azure CLı sürüm 2.0.53 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bir AKS kümesinin diğer Azure kaynaklarıyla etkileşime geçmesini sağlamak için bir Azure Active Directory hizmet sorumlusu kullanılır. Bu hizmet sorumlusu Azure CLI veya portal ile otomatik olarak oluşturulabilir veya kendiniz önceden bir tane oluşturup ek izinler atayabilirsiniz. Bu öğreticide bir hizmet sorumlusu oluşturacak, önceki öğreticide oluşturulan Azure Container Registry (ACR) örneğine erişim verecek ve ardından bir AKS kümesi oluşturacaksınız.

[az ad sp create-for-rbac][] komutunu kullanarak bir hizmet sorumlusu oluşturun. `--skip-assignment` parametresi, ek izinlerin atanmasını engeller. Varsayılan olarak, bu hizmet sorumlusu bir yıl için geçerlidir.

```azurecli
az ad sp create-for-rbac --skip-assignment
```

Çıktı aşağıdaki örneğe benzer:

```
{
  "appId": "e7596ae3-6864-4cb8-94fc-20164b1588a9",
  "displayName": "azure-cli-2018-06-29-19-14-37",
  "name": "http://azure-cli-2018-06-29-19-14-37",
  "password": "52c95f25-bd1e-4314-bd31-d8112b293521",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

*appId* ve *password* değerlerini not edin. Bu değerler aşağıdaki adımlarda kullanılacaktır.

## <a name="configure-acr-authentication"></a>ACR kimlik doğrulamasını yapılandırma

ACR'de depolanan görüntülere erişmek için AKS hizmet sorumlusuna ACR'den görüntüleri almak için doğru hakları vermeniz gerekir.

İlk olarak [az acr show][] komutunu kullanarak ACR kaynağı kimliğini alın. `<acrName>` kayıt defteri adını ACR örneğinin adıyla ve ACR örneğinizin bulunduğu kaynak grubuyla güncelleştirin.

```azurecli
az acr show --resource-group myResourceGroup --name <acrName> --query "id" --output tsv
```

ACR 'de depolanan görüntüleri çekmek üzere aks kümesi için doğru erişimi vermek üzere [az role atama Create][] komutunu `AcrPull` kullanarak rolü atayın. `<appId`> ve `<acrId>` yerine önceki iki adımda topladığınız değerleri yazın.

```azurecli
az role assignment create --assignee <appId> --scope <acrId> --role acrpull
```

## <a name="create-a-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

AKS kümeleri Kubernetes rol tabanlı erişim denetimlerini (RBAC) kullanabilir. Bu denetimler, kullanıcılara atanmış olan rollere göre kaynaklara erişim vermenizi sağlayabilir. Bir kullanıcıya birden çok rol atanırsa izinler birleştirilir ve izinler tek bir ad alanı veya tüm küme genelinde kapsam olabilir. Bir AKS kümesi oluşturduğunuzda Azure CLI varsayılan ayarlarda RBAC özelliğini otomatik olarak etkinleştirir.

[az aks create][] komutunu kullanarak bir AKS kümesi oluşturun. Aşağıdaki örnek, *myResourceGroup* adlı kaynak grubunda *myAKSCluster* adlı bir küme oluşturur. Bu kaynak grubu, [önceki öğreticide][aks-tutorial-prepare-acr]oluşturulmuştur. `<appId>` ve `<password>` yerine hizmet sorumlusunun oluşturulduğu bir önceki adımdan aldığınız değerlerinizi girin.

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --service-principal <appId> \
    --client-secret <password> \
    --generate-ssh-keys
```

Birkaç dakika sonra dağıtım tamamlanır ve AKS dağıtımı hakkında JSON biçimli bilgileri döndürür.

## <a name="install-the-kubernetes-cli"></a>Kubernetes CLI'yi yükleme

Yerel bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes komut satırı istemcisi olan [kubectl][kubectl]'yi kullanırsınız.

Azure Cloud Shell'i kullanıyorsanız `kubectl` zaten yüklüdür. [az aks install-cli][] komutunu kullanarak da yerel ortama yükleyebilirsiniz:

```azurecli
az aks install-cli
```

## <a name="connect-to-cluster-using-kubectl"></a>kubectl istemcisini kullanarak kümeye bağlanma

Kubernetes kümenize bağlanacak şekilde yapılandırmak `kubectl` için [az aks Get-Credentials][] komutunu kullanın. Aşağıdaki örnek, *Myresourcegroup*Içinde *Myakscluster* adlı aks kümesinin kimlik bilgilerini alır:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenizin bağlantısını doğrulamak için [kubectl Get Nodes][kubectl-get] komutunu çalıştırın:

```
$ kubectl get nodes

NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-12345678-0   Ready    agent   32m   v1.13.10
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide AKS'de bir Kubernetes kümesi dağıttınız ve `kubectl` istemcisini ona bağlanacak şekilde yapılandırdınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Kaynak etkileşimleri için hizmet sorumlusu oluşturma
> * Kubernetes AKS kümesini dağıtma
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
[az role atama Create]: /cli/azure/role/assignment#az-role-assignment-create
[az aks create]: /cli/azure/aks#az-aks-create
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-cli-install]: /cli/azure/install-azure-cli
