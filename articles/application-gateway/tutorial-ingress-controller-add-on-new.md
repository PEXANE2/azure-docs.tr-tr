---
title: Yeni Azure Application Gateway yeni AKS kümesi için giriş denetleyicisi eklentisini etkinleştirin
description: Yeni AKS kümeniz için yeni bir Application Gateway giriş denetleyicisi eklentisini etkinleştirmek üzere bu öğreticiyi kullanın
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 06/10/2020
ms.author: caya
ms.openlocfilehash: bb7ba071b1c0b49a818857e417ef6bf676a9a0df
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84806478"
---
# <a name="tutorial-enable-application-gateway-ingress-controller-add-on-for-a-new-aks-cluster-with-a-new-application-gateway-through-azure-cli-preview"></a>Öğretici: Azure CLı ile yeni bir Application Gateway ile yeni bir AKS kümesi için Application Gateway giriş denetleyicisi eklentisini etkinleştirme (Önizleme)

Azure CLı 'yi [Azure Kubernetes Services (AKS)](https://azure.microsoft.com/services/kubernetes-service/) kümeniz için şu anda önizleme aşamasında olan [Application Gateway giriş denetleyicisi (agic)](ingress-controller-overview.md) eklentisini etkinleştirmek için kullanabilirsiniz. Bu öğreticide, tarafından otomatik olarak kullanılacak bir Application Gateway oluşturacak olan AGIC eklentisi etkinken bir AKS kümesi oluşturacaksınız. Daha sonra, Application Gateway aracılığıyla uygulamayı açığa çıkarmak için AGIC eklentisini kullanacak örnek bir uygulama dağıtırsınız. Eklenti, AKS kümeniz için AGC 'yi [daha önce Held üzerinden](ingress-controller-overview.md#difference-between-helm-deployment-and-aks-add-on) dağıtmanın çok daha hızlı bir yolunu sağlar ve ayrıca tam olarak yönetilen bir deneyim sunar.    

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Kaynak grubu oluşturma 
> * AGIC eklentisi etkinken yeni bir AKS kümesi oluşturma 
> * AKS kümesindeki Infer için AGIC kullanarak örnek bir uygulama dağıtma
> * Application Gateway aracılığıyla uygulamanın erişilebilir olup olmadığını denetleyin

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

Aşağıdaki örnekte gösterildiği gibi [az Feature Register](https://docs.microsoft.com/cli/azure/feature#az-feature-register) komutunu kullanarak *aks-IngressApplicationGatewayAddon* Özellik bayrağını kaydedin; Bu işlemi abonelik başına yalnızca bir kez yapmanız gerekir, eklenti hala önizlemededir:
```azurecli-interactive
az feature register --name AKS-IngressApplicationGatewayAddon --namespace microsoft.containerservice
```

Durumun kaydı gösterilmesi birkaç dakika sürebilir. [Az Feature List](https://docs.microsoft.com/cli/azure/feature#az-feature-register) komutunu kullanarak kayıt durumunu denetleyebilirsiniz:
```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-IngressApplicationGatewayAddon')].{Name:name,State:properties.state}"
```

Hazırlandığınızda, [az Provider Register](https://docs.microsoft.com/cli/azure/provider#az-provider-register) komutunu kullanarak Microsoft. Containerservice kaynak sağlayıcısı kaydını yenileyin:
```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

Bu öğreticide aks-Preview uzantısını yüklediğinizden/güncelleştirdiğinizden emin olun; aşağıdaki Azure CLı komutlarını kullanın
```azurecli-interactive
az extension add --name aks-preview
az extension list
```
```azurecli-interactive
az extension update --name aks-preview
az extension list
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure 'da, ilgili kaynakları bir kaynak grubuna ayırabilirsiniz. [Az Group Create](/cli/azure/group#az-group-create)kullanarak bir kaynak grubu oluşturun. Aşağıdaki örnek, *canadamerkezi* konumunda (bölge) *myresourcegroup* adlı bir kaynak grubu oluşturur. 

```azurecli-interactive
az group create --name myResourceGroup --location canadacentral
```

## <a name="deploy-a-new-aks-cluster-with-agic-add-on-enabled"></a>AGIC eklentisi etkinken yeni bir AKS kümesi dağıtma

Artık AGIC eklentisi etkinken yeni bir AKS kümesi dağıtacaksınız. AGIC eklentisi etkinken yeni bir AKS kümesi dağıttığınızda ve kullanmak üzere mevcut bir Application Gateway sağlamazsanız, AKS kümesine trafik sunmak için otomatik olarak yeni bir Application Gateway oluşturacak ve ayarlayacağız.  

> [!NOTE]
> Application Gateway giriş denetleyicisi (AGIC) eklentisi, Application Gateway v1 SKU 'Larını **değil** , **yalnızca** Application Gateway v2 SKU 'larını (Standart ve WAF) destekler. AGIC eklentisi aracılığıyla yeni bir Application Gateway dağıtırken, yalnızca bir Application Gateway Standard_v2 SKU 'SU dağıtabilirsiniz. Application Gateway WAF_v2 SKU 'SU için AGIC eklentisini etkinleştirmek istiyorsanız, lütfen Portal üzerinden Application Gateway WAF 'yi etkinleştirin ya da önce WAF_v2 oluşturun ve ardından [mevcut BIR AKS kümesi ve mevcut Application Gateway Ile agic eklentisinin nasıl etkinleştirileceği](tutorial-ingress-controller-add-on-existing.md)hakkındaki yönergeleri izleyin. 

Aşağıdaki örnekte, [Azure CNI](https://docs.microsoft.com/azure/aks/concepts-network#azure-cni-advanced-networking) kullanılarak *MyCluster* adlı yeni bir aks kümesi ve oluşturduğunuz kaynak grubundaki agic eklentisi etkinleştirilmiş olan [yönetilen kimlikleri](https://docs.microsoft.com/azure/aks/use-managed-identity) ve *myresourcegroup*' yi kullanarak dağıtacaksınız. Yeni bir AKS kümesini, mevcut bir Application Gateway belirtmeden etkin bir şekilde dağıtmak, Standard_v2 SKU Application Gateway otomatik olarak oluşturulması anlamına gelir. Ayrıca, Application Gateway adını ve alt ağ adres alanını da belirtirsiniz. Application Gateway adı *myApplicationGateway* olur ve kullandığımız alt ağ adres alanı 10.2.0.0/16 ' dır. Bu öğreticinin başlangıcında aks-Preview uzantısını eklediğinizden/güncelleştirdiğinizden emin olun. 

```azurecli-interactive
az aks create -n myCluster -g myResourceGroup --network-plugin azure --enable-managed-identity -a ingress-appgw --appgw-name myApplicationGateway --appgw-subnet-prefix "10.2.0.0/16" 
```

Komuta ek parametreler yapılandırmak için `az aks create` başvuruları [buraya](https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create)ziyaret edin. 

> [!NOTE]
> Oluşturduğunuz AKS kümesi, oluşturduğunuz kaynak grubunda, *Myresourcegroup*' de görünür. Ancak otomatik olarak oluşturulan Application Gateway, aracı havuzlarının olduğu düğüm kaynak grubunda bulunur. Düğüm kaynak grubu varsayılan olarak *MC_resource-Group-name_cluster-name_location*olarak adlandırılır, ancak değiştirilebilir. 

## <a name="deploy-a-sample-application-using-agic"></a>AGIC kullanarak örnek uygulama dağıtma

Şimdi, oluşturduğunuz AKS kümesine örnek bir uygulama dağıtarak, giriş için AGIC eklentisini kullanacak ve Application Gateway AKS kümesine bağlayacaksınız. İlk olarak, komutunu çalıştırarak dağıttığınız AKS kümesi için kimlik bilgilerini alacaksınız `az aks get-credentials` . 

```azurecli-interactive
az aks get-credentials -n myCluster -g myResourceGroup
```

Oluşturduğunuz kümeye kimlik bilgilerini aldıktan sonra, kümeye giriş için AGIC kullanan bir örnek uygulama ayarlamak için aşağıdaki komutu çalıştırın. AGIC, daha önce, dağıttığınız yeni örnek uygulamaya karşılık gelen yönlendirme kuralları ile ayarladığınız Application Gateway güncellecektir.  

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/docs/examples/aspnetapp.yaml 
```

## <a name="check-that-the-application-is-reachable"></a>Uygulamanın erişilebilir olup olmadığını denetleyin

Application Gateway, AKS kümesine trafik sunacak şekilde ayarlandığına göre, uygulamanızın ulaşılabilir olduğunu doğrulayalım. İlk olarak, giriş adresinin IP adresini alacaksınız. 

```azurecli-interactive
kubectl get ingress
```

Oluşturduğunuz örnek uygulamanın, yukarıdaki komutu çalıştırmadan aldığınız Application Gateway IP adresini ziyaret ederek çalışır durumda olup olmadığını denetleyin veya ile kontrol edin `curl` . Güncelleştirme için bir dakika Application Gateway sürebilir. bu nedenle, Application Gateway portalda hala "güncelleştirme" durumunda olduğundan, IP adresine ulaşmaya çalışmadan önce bu işlemi tamamlayabiliriz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, uygulama ağ geçidini ve tüm ilgili kaynakları silin.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar
* [AGIC eklentisini devre dışı bırakma hakkında daha fazla bilgi edinin](./ingress-controller-disable-addon.md)
* [AGIC ile hangi ek açıklamaların desteklendiği hakkında daha fazla bilgi edinin](./ingress-controller-annotations.md)
* [AGIC sorunlarını giderme](./ingress-controller-troubleshoot.md)

