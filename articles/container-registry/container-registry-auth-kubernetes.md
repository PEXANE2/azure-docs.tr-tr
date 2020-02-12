---
title: Kubernetes kümesinden kimlik doğrulama
description: Hizmet sorumlusu kullanarak bir çekme gizli anahtarı oluşturarak Azure Container kayıt defterinizde görüntülere erişimi olan bir Kubernetes kümesi sağlamayı öğrenin
ms.topic: article
author: karolz-ms
ms.author: karolz
ms.reviewer: danlep
ms.date: 02/10/2020
ms.openlocfilehash: 0608ca0e0e53acf2f19910a7f1107dacf67d4e61
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77154900"
---
# <a name="pull-images-from-an-azure-container-registry-to-a-kubernetes-cluster"></a>Azure Container Registry 'den bir Kubernetes kümesine görüntü çekme

[Minikube](https://minikube.sigs.k8s.io/) ve [Kind](https://kind.sigs.k8s.io/)gibi "yerel" Kubernetes kümeleri de dahil olmak üzere herhangi bir Kubernetes kümesiyle kapsayıcı görüntülerinin kaynağı olarak bir Azure Container Registry kullanabilirsiniz. Bu makalede, bir Azure Active Directory hizmet sorumlusu temelinde bir Kubernetes çekme sırrı oluşturma gösterilmektedir. Ardından, bir Kubernetes dağıtımında bir Azure Container Registry 'den görüntü çekmek için gizli anahtarı kullanın.

> [!TIP]
> Yönetilen [Azure Kubernetes hizmetini](../aks/intro-kubernetes.md)kullanıyorsanız, küme Için hedef Azure Container Registry ile [kümenizi de tümleştirebilirsiniz](../aks/cluster-container-registry-integration.md?toc=/azure/container-registry/toc.json&bc=/azure/container-registry/breadcrumb/toc.json) . 

Bu makalede, zaten özel bir Azure Container Registry oluşturmuş olduğunuz varsayılmaktadır. Ayrıca, çalışan bir Kubernetes kümesine sahip olmanız ve `kubectl` komut satırı aracı aracılığıyla erişilebilir olması gerekir.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

Hizmet sorumlusu parolasını kaydetmezseniz veya hatırlamıyorsanız, [az ad SP kimlik bilgisi sıfırlama][az-ad-sp-credential-reset] komutuyla sıfırlayabilirsiniz:

```azurecli
az ad sp credential reset  --name http://<service-principal-name> --query password --output tsv
```

Bu komut, hizmet sorumlusu için yeni ve geçerli bir parola döndürür.

## <a name="create-an-image-pull-secret"></a>Resim çekme parolası oluşturma

Kubernetes, kayıt defterinizde kimlik doğrulaması yapmak için gereken bilgileri depolamak için bir *resim çekme gizli anahtarı* kullanır. Bir Azure Container Registry için çekme gizli dizisi oluşturmak için hizmet sorumlusu KIMLIĞI, parola ve kayıt defteri URL 'sini sağlarsınız. 

Aşağıdaki `kubectl` komutuyla bir resim çekme gizli dizisi oluşturun:

```console
kubectl create secret docker-registry <secret-name> \
  --namespace <namespace> \
  --docker-server=https://<container-registry-name>.azurecr.io \
  --docker-username=<service-principal-ID> \
  --docker-password=<service-principal-password>
```
burada:

| Değer | Açıklama |
| :--- | :--- |
| `secret-name` | Resim çekme parolasının adı, örneğin *ACR-sır* |
| `namespace` | Gizli anahtarı yerleştirmek için Kubernetes ad alanı <br/> Yalnızca parolayı varsayılan ad alanından başka bir ad alanına yerleştirmek istiyorsanız gereklidir |
| `container-registry-name` | Azure Container Registry 'nizin adı |
| `service-principal-ID` | Kayıt defterinize erişmek için Kubernetes tarafından kullanılacak hizmet sorumlusunun KIMLIĞI |
| `service-principal-password` | Hizmet sorumlusu parolası |

## <a name="use-the-image-pull-secret"></a>Görüntü çekme gizli anahtarını kullanma

Görüntü çekme gizli anahtarını oluşturduktan sonra, Kubernetes Pod ve dağıtımları oluşturmak için bunu kullanabilirsiniz. Dağıtım dosyasında `imagePullSecrets` altında gizli dizi adını sağlayın. Örneğin:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: your-awesome-app-pod
  namespace: awesomeapps
spec:
  containers:
    - name: main-app-container
      image: your-awesome-app:v1
      imagePullPolicy: IfNotPresent
  imagePullSecrets:
    - name: acr-secret
```

Yukarıdaki örnekte `your-awesome-app:v1`, Azure Container Registry 'den çekilecek görüntünün adıdır ve `acr-secret` kayıt defterine erişmek için oluşturduğunuz çekme gizliliğine ait addır. Pod 'u dağıttığınızda, Kubernetes kümede zaten mevcut değilse görüntüyü Kayıt defterinizden otomatik olarak çeker.


## <a name="next-steps"></a>Sonraki adımlar

* Hizmet sorumluları ve Azure Container Registry çalışma hakkında daha fazla bilgi için bkz. [hizmet sorumluları ile Azure Container Registry kimlik doğrulaması](container-registry-auth-service-principal.md)
* [Kubernetes belgelerindeki](https://kubernetes.io/docs/concepts/containers/images/#specifying-imagepullsecrets-on-a-pod) resim çekme gizli dizileri hakkında daha fazla bilgi edinin


<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset