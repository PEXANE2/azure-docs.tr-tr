---
title: Kubernetes kümesinden kimlik doğrulaması
description: Bir hizmet ilkesini kullanarak bir çekme sırrı oluşturarak Azure konteyner kayıt defterinizdeki resimlere erişebilen bir Kubernetes kümesini nasıl sağlayacağınızı öğrenin
ms.topic: article
author: karolz-ms
ms.author: karolz
ms.reviewer: danlep
ms.date: 02/10/2020
ms.openlocfilehash: 0608ca0e0e53acf2f19910a7f1107dacf67d4e61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77154900"
---
# <a name="pull-images-from-an-azure-container-registry-to-a-kubernetes-cluster"></a>Azure kapsayıcı kayıt defterinden Kubernetes kümesine görüntüleri çekme

Bir Azure kapsayıcı kayıt defterini, [minikube](https://minikube.sigs.k8s.io/) ve [tür](https://kind.sigs.k8s.io/)gibi "yerel" Kubernetes kümeleri de dahil olmak üzere herhangi bir Kubernetes kümesiyle kapsayıcı görüntülerinin kaynağı olarak kullanabilirsiniz. Bu makalede, Bir Azure Etkin Dizin hizmet ilkesine dayalı bir Kubernetes pull secret nasıl oluşturulacak gösterilmektedir. Ardından, Kubernetes dağıtımında azure kapsayıcı kayıt defterinden görüntüleri çekmek için sırrı kullanın.

> [!TIP]
> Yönetilen [Azure Kubernetes Hizmetini](../aks/intro-kubernetes.md)kullanıyorsanız, [kümenizi](../aks/cluster-container-registry-integration.md?toc=/azure/container-registry/toc.json&bc=/azure/container-registry/breadcrumb/toc.json) görüntü çekmeler için hedef bir Azure konteyner kayıt defteriyle de entegre edebilirsiniz. 

Bu makalede, zaten özel bir Azure kapsayıcı kayıt defteri oluşturduğunuz varsayar. Ayrıca, komut satırı aracı yla çalışan ve erişilebilir `kubectl` bir Kubernetes kümesine sahip olmanız gerekir.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

Hizmet temel parolasını kaydetmez veya hatırlamazsanız, [az reklam sp kimlik bilgisi][az-ad-sp-credential-reset] sıfırlama komutuyla sıfırlayabilirsiniz:

```azurecli
az ad sp credential reset  --name http://<service-principal-name> --query password --output tsv
```

Bu komut, hizmet müdürünüz için yeni ve geçerli bir parola döndürür.

## <a name="create-an-image-pull-secret"></a>Görüntü çekme sırrı oluşturma

Kubernetes, kayıt defterinize kimlik doğrulamak için gereken bilgileri depolamak için bir *görüntü çekme sırrı* kullanır. Azure kapsayıcı kayıt defteri için çekme sırrını oluşturmak için hizmet ana kimliğini, parolayı ve kayıt defteri URL'sini sağlarsınız. 

Aşağıdaki `kubectl` komutla bir görüntü çekme sırrı oluşturun:

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
| `secret-name` | Görüntünün adı gizli çekin, örneğin, *kısaltma-gizli* |
| `namespace` | Kubernetes namespace içine gizli koymak için <br/> Yalnızca sırrı varsayılan ad alanı dışında bir ad alanına yerleştirmek istiyorsanız gereklidir |
| `container-registry-name` | Azure kapsayıcı kayıt defterinizin adı |
| `service-principal-ID` | Kayıt defterinize erişmek için Kubernetes tarafından kullanılacak hizmet sorumlusunun kimliği |
| `service-principal-password` | Hizmet temel şifresi |

## <a name="use-the-image-pull-secret"></a>Görüntü çekme sırrını kullanın

Görüntüyü oluşturduktan sonra gizli çekme, Kubernetes bölmeleri ve dağıtımları oluşturmak için kullanabilirsiniz. Dağıtım dosyasında altındaki `imagePullSecrets` sırrın adını sağlayın. Örnek:

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

Önceki örnekte, `your-awesome-app:v1` Azure kapsayıcı kayıt defterinden çekilecek görüntünün adı `acr-secret` ve kayıt defterine erişmek için oluşturduğunuz çekme sırrının adıdır. Bölmeyi dağıttığınızda, Kubernetes kümede zaten yoksa resmi kayıt defterinizden otomatik olarak çeker.


## <a name="next-steps"></a>Sonraki adımlar

* Hizmet ilkeleri ve Azure Kapsayıcı Kayıt Defteri ile çalışma hakkında daha fazla şey [için, hizmet ilkeleriyle birlikte Azure Kapsayıcı Kayıt Defteri kimlik doğrulaması](container-registry-auth-service-principal.md)
* [Kubernetes belgelerinde](https://kubernetes.io/docs/concepts/containers/images/#specifying-imagepullsecrets-on-a-pod) görüntü çekme sırları hakkında daha fazla bilgi edinin


<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset