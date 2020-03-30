---
title: Azure Kubernetes Hizmeti (AKS) kümesinin kimlik bilgilerini sıfırlama
description: Azure Kubernetes Hizmeti (AKS) kümesi için hizmet sorumlusu veya AAD Uygulama kimlik bilgilerini nasıl güncelleştirip sıfırladığını öğrenin
services: container-service
ms.topic: article
ms.date: 03/11/2019
ms.openlocfilehash: b7d652be3733cb130a3973909de59489047efe0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475553"
---
# <a name="update-or-rotate-the-credentials-for-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) kimlik bilgilerini güncelleştirme veya döndürme

Varsayılan olarak, AKS kümeleri bir yıllık son kullanma süresi olan bir hizmet ilkesiyle oluşturulur. Son kullanma tarihine yaklaştıkça, hizmet ilkesini ek bir süre uzatmak için kimlik bilgilerini sıfırlayabilirsiniz. Ayrıca, kimlik bilgilerini tanımlı bir güvenlik ilkesinin parçası olarak güncelleştirmek veya döndürmek isteyebilirsiniz. Bu makalede, bir AKS kümesi için bu kimlik bilgileri nasıl güncelleştirileştirilir.

[AKS kümenizi Azure Etkin Dizini ile tümleştirmiş][aad-integration]ve kümeniz için kimlik doğrulama sağlayıcısı olarak kullanabilirsiniz. Bu durumda kümeniz, AAD Server Uygulaması ve AAD İstemci Uygulaması için oluşturulan 2 kimlik daha olacak sayılsa, bu kimlik bilgilerini de sıfırlayabilirsiniz. 

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI sürümü 2.0.65 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="update-or-create-a-new-service-principal-for-your-aks-cluster"></a>AKS kümeniz için yeni bir Hizmet Sorumlusu güncelleştirme veya oluşturma

Bir AKS kümesinin kimlik bilgilerini güncelleştirmek istediğinizde şunları seçebilirsiniz:

* küme tarafından kullanılan varolan hizmet sorumlusunun kimlik bilgilerini güncelleştirmek veya
* bir hizmet sorumlusu oluşturun ve bu yeni kimlik bilgilerini kullanmak için kümeyi güncelleştirin.

### <a name="reset-existing-service-principal-credential"></a>Varolan Hizmet Temel Kimlik Belgesini Sıfırlama

Varolan hizmet sorumlusunun kimlik bilgilerini güncelleştirmek için [az aks göster][az-aks-show] komutunu kullanarak kümenizin hizmet ana kimliğini alın. Aşağıdaki örnek, *myResourceGroup* kaynak grubunda *myAKSCluster* adlı kümenin kimliğini alır. Hizmet temel kimliği, ek komutta kullanılmak üzere *SP_ID* adlı bir değişken olarak ayarlanır.

```azurecli-interactive
SP_ID=$(az aks show --resource-group myResourceGroup --name myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)
```

Hizmet ana kimliği içeren değişken kümesi yle, artık [az reklam sp kimlik bilgilerini sıfırla][az-ad-sp-credential-reset]'yı kullanarak kimlik bilgilerini sıfırla. Aşağıdaki örnek, Azure platformunun hizmet sorumlusu için yeni bir güvenli sır oluşturmasına olanak tanır. Bu yeni güvenli sır da bir değişken olarak saklanır.

```azurecli-interactive
SP_SECRET=$(az ad sp credential reset --name $SP_ID --query password -o tsv)
```

Şimdi yeni [hizmet temel kimlik bilgileri ile AKS küme güncellemeye devam edin.](#update-aks-cluster-with-new-service-principal-credentials) Bu adım, Hizmet Sorumlusu değişikliklerinin AKS kümesine yansıması için gereklidir.

### <a name="create-a-new-service-principal"></a>Yeni Hizmet Müdürü Oluşturma

Önceki bölümde varolan hizmet temel kimlik bilgilerini güncelleştirmeyi seçtiyseniz, bu adımı atlayın. [AKS kümesini yeni hizmet temel kimlik bilgileriyle güncelleştirmeye](#update-aks-cluster-with-new-service-principal-credentials)devam edin.

Bir hizmet sorumlusu oluşturmak ve sonra aks kümesini bu yeni kimlik bilgilerini kullanmak üzere güncelleştirmek [için az reklam sp create-for-rbac][az-ad-sp-create] komutunu kullanın. Aşağıdaki örnekte, `--skip-assignment` parametresi, ek varsayılan atamaların atanmasını engellemektedir:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Çıktı aşağıdaki örneğe benzerdir. Kendi `appId` ve `password`’lerinizi not edin. Bu değerler bir sonraki adımda kullanılır.

```json
{
  "appId": "7d837646-b1f3-443d-874c-fd83c7c739c5",
  "name": "7d837646-b1f3-443d-874c-fd83c7c739c",
  "password": "a5ce83c9-9186-426d-9183-614597c7f2f7",
  "tenant": "a4342dc8-cd0e-4742-a467-3129c469d0e5"
}
```

Şimdi, aşağıdaki örnekte gösterildiği gibi, kendi [az reklam sp create-for-rbac][az-ad-sp-create] komutunuzdan çıktıkullanarak hizmet ana kimliği ve istemci sırrı için değişkenler tanımlayın. *SP_ID* *appId*ve *SP_SECRET* *şifreniz:*

```console
SP_ID=7d837646-b1f3-443d-874c-fd83c7c739c5
SP_SECRET=a5ce83c9-9186-426d-9183-614597c7f2f7
```

Şimdi yeni [hizmet temel kimlik bilgileri ile AKS küme güncellemeye devam edin.](#update-aks-cluster-with-new-service-principal-credentials) Bu adım, Hizmet Sorumlusu değişikliklerinin AKS kümesine yansıması için gereklidir.

## <a name="update-aks-cluster-with-new-service-principal-credentials"></a>AKS kümesini yeni Hizmet Sorumlusu kimlik bilgileriyle güncelleştirin

Varolan hizmet sorumlusunun kimlik bilgilerini güncelleştirmeyi veya bir hizmet sorumlusu oluşturmayı seçmiş olsanız da, [artık AZ aks update-credentials][az-aks-update-credentials] komutunu kullanarak AKS kümesini yeni kimlik bilgilerinizle güncelleştirmiş siniz. *--hizmet sorumlusu* ve *--istemci-gizli* değişkenleri kullanılır:

```azurecli-interactive
az aks update-credentials \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --reset-service-principal \
    --service-principal $SP_ID \
    --client-secret $SP_SECRET
```

Hizmet temel kimlik bilgilerinin AKS'de güncellenmesi birkaç dakika sürer.

## <a name="update-aks-cluster-with-new-aad-application-credentials"></a>AKS Cluster'ı yeni AAD Uygulama kimlik bilgileriyle güncelleştirin

[AAD tümleştirme adımlarını][create-aad-app]izleyerek yeni AAD Server ve İstemci uygulamaları oluşturabilirsiniz. Veya hizmet [temel sıfırlama ile aynı yöntemi](#reset-existing-service-principal-credential)izleyerek mevcut AAD Uygulamaları nızı sıfırla. Bundan sonra sadece aynı [az aks update-credentials][az-aks-update-credentials] komutu kullanarak küme AAD Uygulama kimlik bilgilerini güncelleştirmeniz gerekir ama *--sıfır-aad* değişkenleri kullanarak.

```azurecli-interactive
az aks update-credentials \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --reset-aad \
    --aad-server-app-id <SERVER APPLICATION ID> \
    --aad-server-app-secret <SERVER APPLICATION SECRET> \
    --aad-client-app-id <CLIENT APPLICATION ID>
```


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS kümesinin kendisi ve AAD Tümleştirme Uygulamaları için hizmet ilkesi güncelleştirildi. Kümedeki iş yükleri için kimliğin nasıl yönetilene ilgili daha fazla bilgi için [AKS'de kimlik doğrulama ve yetkilendirme için en iyi uygulamalara][best-practices-identity]bakın.

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-update-credentials]: /cli/azure/aks#az-aks-update-credentials
[best-practices-identity]: operator-best-practices-identity.md
[aad-integration]: azure-ad-integration.md
[create-aad-app]: azure-ad-integration.md#create-the-server-application
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset
