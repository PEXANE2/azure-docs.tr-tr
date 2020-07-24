---
title: Mevcut Azure Application Gateway mevcut AKS kümesi için giriş denetleyicisi eklentisini etkinleştirin
description: Mevcut AKS kümeniz için mevcut bir Application Gateway giriş denetleyicisi eklentisini etkinleştirmek için bu öğreticiyi kullanın
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 06/10/2020
ms.author: caya
ms.openlocfilehash: 42952e379b9f68008de23ee3b1717280d8dd6cb2
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87088130"
---
# <a name="tutorial-enable-application-gateway-ingress-controller-add-on-for-an-existing-aks-cluster-with-an-existing-application-gateway-through-azure-cli-preview"></a>Öğretici: mevcut bir AKS kümesi için Application Gateway giriş denetleyicisi eklentisini Azure CLı aracılığıyla mevcut bir Application Gateway ile etkinleştirme (Önizleme)

Azure CLı 'yi [Azure Kubernetes Services (AKS)](https://azure.microsoft.com/services/kubernetes-service/) kümeniz için şu anda önizleme aşamasında olan [Application Gateway giriş denetleyicisi (agic)](ingress-controller-overview.md) eklentisini etkinleştirmek için kullanabilirsiniz. Bu öğreticide, ayrı sanal ağlarda dağıtılan mevcut bir Application Gateway ile Kubernetes uygulamanızı mevcut bir AKS kümesinde ortaya çıkarmak için AGIC eklentisini nasıl kullanacağınızı öğreneceksiniz. Mevcut kaynakların benzetimini yapmak için bir sanal ağda AKS kümesi ve ayrı bir sanal ağda Application Gateway oluşturmaya başlayacaksınız. Ardından, AGIC eklentisini etkinleştireceksiniz, iki sanal ağı birlikte eşleyebilir ve AGIC eklentisi kullanılarak Application Gateway üzerinden kullanıma sunulacak bir örnek uygulama dağıtırsınız. Aynı sanal ağdaki mevcut bir Application Gateway ve mevcut AKS kümesi için AGIC eklentisini etkinleştirirseniz, aşağıdaki eşleme adımını atlayabilirsiniz. Eklenti, AKS kümeniz için AGC 'yi [daha önce Held üzerinden](ingress-controller-overview.md#difference-between-helm-deployment-and-aks-add-on) dağıtmanın çok daha hızlı bir yolunu sağlar ve ayrıca tam olarak yönetilen bir deneyim sunar.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kaynak grubu oluşturma 
> * Yeni bir AKS kümesi oluşturma 
> * Yeni bir Application Gateway oluştur 
> * Mevcut AKS kümesindeki AGIC eklentisini mevcut Application Gateway kullanarak etkinleştirin 
> * AKS kümesi sanal ağıyla Application Gateway sanal ağı eşler
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
az feature list -o table --query "[?contains(name, 'microsoft.containerservice/AKS-IngressApplicationGatewayAddon')].{Name:name,State:properties.state}"
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

## <a name="deploy-a-new-aks-cluster"></a>Yeni bir AKS kümesi dağıtma

Şimdi yeni bir AKS kümesi dağıtarak, için AGIC eklentisini etkinleştirmek istediğiniz mevcut bir AKS kümesine sahip olma benzetimi yapabilirsiniz.  

Aşağıdaki örnekte, *Myresourcegroup*adlı kaynak grubundaki [Azure CNI](https://docs.microsoft.com/azure/aks/concepts-network#azure-cni-advanced-networking) ve [yönetilen kimlikleri](https://docs.microsoft.com/azure/aks/use-managed-identity) kullanarak *MyCluster* adlı yeni bir aks kümesi dağıtacaksınız.    

```azurecli-interactive
az aks create -n myCluster -g myResourceGroup --network-plugin azure --enable-managed-identity 
```

Komuta ek parametreler yapılandırmak için `az aks create` başvuruları [buraya](https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create)ziyaret edin. 

## <a name="deploy-a-new-application-gateway"></a>Yeni bir Application Gateway dağıtma 

Artık, AKS kümenize, *MyCluster*içindeki trafiğin yükünü dengelemek için kullanmak istediğiniz mevcut bir Application Gateway sahip olmak için yeni bir Application Gateway dağıtırsınız. Application Gateway adı *myApplicationGateway*olacaktır, ancak önce *Mypublicıp*adlı bir genel IP kaynağı ve *myvnet* adres alanı 11.0.0.0/8 ve adres alanı 11.1.0.0/16 ile *Mysubnet*adlı bir alt ağ ve *mypublicıp*kullanarak *mysubnet* öğesine Application Gateway dağıtmanız gerekir. 

Bir AKS kümesi ve ayrı sanal ağlarda Application Gateway kullanırken, iki sanal ağın adres alanları çakışmamalıdır. AKS kümesinin dağıttığı varsayılan adres alanı 10.0.0.0/8 olduğundan Application Gateway sanal ağ adresi ön ekini 11.0.0.0/8 olarak ayarlayacağız. 

```azurecli-interactive
az network public-ip create -n myPublicIp -g MyResourceGroup --allocation-method Static --sku Standard
az network vnet create -n myVnet -g myResourceGroup --address-prefix 11.0.0.0/8 --subnet-name mySubnet --subnet-prefix 11.1.0.0/16 
az network application-gateway create -n myApplicationGateway -l canadacentral -g myResourceGroup --sku Standard_v2 --public-ip-address myPublicIp --vnet-name myVnet --subnet mySubnet
```

> [!NOTE]
> Application Gateway giriş denetleyicisi (AGIC) eklentisi, Application Gateway v1 SKU 'Larını **değil** , **yalnızca** Application Gateway v2 SKU 'larını (Standart ve WAF) destekler. 

## <a name="enable-the-agic-add-on-in-existing-aks-cluster-with-existing-application-gateway"></a>Mevcut AKS kümesindeki AGIC eklentisini mevcut Application Gateway etkinleştirme 

Şimdi oluşturduğunuz AKS kümesindeki AGIC eklentisini, *MyCluster*' ı etkinleştirip, *myApplicationGateway*oluşturduğunuz mevcut Application Gateway kullanmak için agic eklentisini belirtmeniz gerekir. Bu öğreticinin başlangıcında aks-Preview uzantısını eklediğinizden/güncelleştirdiğinizden emin olun. 

```azurecli-interactive
appgwId=$(az network application-gateway show -n myApplicationGateway -g myResourceGroup -o tsv --query "id") 
az aks enable-addons -n myCluster -g myResourceGroup -a ingress-appgw --appgw-id $appgwId
```

## <a name="peer-the-two-virtual-networks-together"></a>İki sanal ağı birbirine eşler

AKS kümesini kendi sanal ağında ve başka bir sanal ağdaki Application Gateway dağıttığımız için, trafiğin Application Gateway akışın kümedeki yığınlara akmasını sağlamak üzere iki sanal ağı birbirine eşleyebileceksiniz. İki sanal ağ arasındaki eşleme, bağlantının çift yönlü olduğundan emin olmak için Azure CLı komutunu iki ayrı kez çalıştırmayı gerektirir. İlk komut Application Gateway sanal ağından AKS sanal ağına bir eşleme bağlantısı oluşturacaktır; İkinci komut diğer yönde bir eşleme bağlantısı oluşturacaktır. 

```azurecli-interactive
nodeResourceGroup=$(az aks show -n myCluster -g myResourceGroup -o tsv --query "nodeResourceGroup")
aksVnetName=$(az network vnet list -g $nodeResourceGroup -o tsv --query "[0].name")

aksVnetId=$(az network vnet show -n $aksVnetName -g $nodeResourceGroup -o tsv --query "id")
az network vnet peering create -n AppGWtoAKSVnetPeering -g myResourceGroup --vnet-name myVnet --remote-vnet $aksVnetId --allow-vnet-access

appGWVnetId=$(az network vnet show -n myVnet -g myResourceGroup -o tsv --query "id")
az network vnet peering create -n AKStoAppGWVnetPeering -g $nodeResourceGroup --vnet-name $aksVnetName --remote-vnet $appGWVnetId --allow-vnet-access
```
## <a name="deploy-a-sample-application-using-agic"></a>AGIC kullanarak örnek uygulama dağıtma 

Şimdi, giriş için AGIC eklentisini kullanacak ve Application Gateway AKS kümesine bağlayacaktır, oluşturduğunuz AKS kümesine örnek bir uygulama dağıtırsınız. İlk olarak, komutunu çalıştırarak dağıttığınız AKS kümesi için kimlik bilgilerini alacaksınız `az aks get-credentials` . 

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

