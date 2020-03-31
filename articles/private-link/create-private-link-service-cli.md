---
title: Azure CLI'yi kullanarak Azure Özel Bağlantı hizmeti oluşturun
description: Azure CLI'yi kullanarak Azure Özel Bağlantı hizmetini nasıl oluşturabilirsiniz öğrenin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 6e6148d305af26f7933567ae58023d2ba73263eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75350235"
---
# <a name="create-a-private-link-service-using-azure-cli"></a>Azure CLI'yi kullanarak Özel Bağlantı hizmeti oluşturma
Bu makalede, Azure CLI kullanarak Azure'da Özel Bağlantı hizmeti nasıl oluşturulabileceğiniz gösterilmektedir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLI'yi yerel olarak yüklemeye ve kullanmaya karar verirseniz, bu hızlı başlangıç en son Azure CLI sürümünü kullanmanızı gerektirir. Yüklü sürümünüzü bulmak için `az --version`çalıştırın. Bilgileri yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli) bakın.
## <a name="create-a-private-link-service"></a>Özel Bağlantı hizmeti oluşturma
### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Sanal ağ oluşturmadan önce, sanal ağı barındıracak bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Bu örnek, *westcentralus* konumunda *myResourceGroup* adında bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location westcentralus
```
### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
[az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) komutu ile bir sanal ağ oluşturun. Bu örnek, *mySubnet*adlı bir alt ağ ile *myVirtualNetwork* adlı varsayılan bir sanal ağ oluşturur:

```azurecli-interactive
az network vnet create --resource-group myResourceGroup --name myVirtualNetwork --address-prefix 10.0.0.0/16  
```
### <a name="create-a-subnet"></a>Alt ağ oluşturma
[Az ağ vnet subnet oluşturmak](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create)ile sanal ağ için bir alt ağ oluşturun. Bu örnek, *myVirtualNetwork* sanal ağında *mySubnet* adında bir alt ağ oluşturur:

```azurecli-interactive
az network vnet subnet create --resource-group myResourceGroup --vnet-name myVirtualNetwork --name mySubnet --address-prefixes 10.0.0.0/24    
```
### <a name="create-a-internal-load-balancer"></a>Dahili Yük Dengeleyicisi Oluşturma 
Az ağ lb oluşturmak ile bir iç yük dengeleyici [oluşturun.](/cli/azure/network/lb#az-network-lb-create) Bu örnek, *myResourceGroup*adlı kaynak grubunda *myILB* adında bir iç yük dengeleyicisi oluşturur. 

```azurecli-interactive
az network lb create --resource-group myResourceGroup --name myILB --sku standard --vnet-name MyVirtualNetwork --subnet mySubnet --frontend-ip-name myFrontEnd --backend-pool-name myBackEndPool
```

### <a name="create-a-load-balancer-health-probe"></a>Yük dengeleyici durum yoklaması oluşturma

Durum araştırması tüm sanal makine örneklerini denetleyerek ağ trafiği aldıklarından emin olur. Sistem durumu denetimi başarısız olan sanal makine örnekleri tekrar çevrimiçi olana ve sistem durumu denetimi iyi olduğuna karar verene kadar yük dengeleyiciden kaldırılır. Sanal makinelerin durumunu izlemek için [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest) ile bir durum araştırması oluşturun. 

```azurecli-interactive
  az network lb probe create \
    --resource-group myResourceGroup \
    --lb-name myILB \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı, gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırmasını ve trafiği almak için arka uç IP havuzunu tanımlar. *myFrontEnd* ön uç havuzunda 80 numaralı bağlantı noktasını dinlemek ve yine 80 numaralı bağlantı noktasını kullanarak *myBackEndPool* arka uç adres havuzuna yük dengelemesi yapılmış ağ trafiğini göndermek için [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest) ile *myHTTPRule* yük dengeleyici kuralı oluşturun. 

```azurecli-interactive
  az network lb rule create \
    --resource-group myResourceGroup \
    --lb-name myILB \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe  
```
### <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu örnekte, sanal makine oluşturma yı kapsamayız. Yük dengeleyicisi için arka uç sunucusu olarak kullanılmak üzere iki sanal makine oluşturmak için [Azure CLI kullanarak bakiye VM'lerini yüklemek için dahili yük dengeleyicisi oluşturma](../load-balancer/load-balancer-get-started-ilb-arm-cli.md#create-servers-for-the-backend-address-pool) adımlarını izleyebilirsiniz. 


### <a name="disable-private-link-service-network-policies-on-subnet"></a>Alt ağdaki Özel Bağlantı hizmet ağı ilkelerini devre dışı 
Private Link hizmeti, sanal ağ içinde seçtiğiniz herhangi bir alt ağdan bir IP gerektirir. Şu anda, bu IP'lerde Ağ İlkeleri'ni destekliyoruz.  Bu nedenle, alt ağdaki ağ ilkelerini devre dışı bmeliyiz. [Az network vnet subnet güncelleştirmesi](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update)ile Private Link hizmet ağı ilkelerini devre dışı kAkacak alt ağı güncelleştirin.

```azurecli-interactive
az network vnet subnet update --resource-group myResourceGroup --vnet-name myVirtualNetwork --name mySubnet --disable-private-link-service-network-policies true 
```
 
## <a name="create-a-private-link-service"></a>Özel Bağlantı hizmeti oluşturma  
 
Az ağ özel bağlantı hizmeti oluşturmak ile Standart Yük Dengeleyici frontend IP yapılandırması kullanarak bir Özel Bağlantı [hizmeti oluşturun.](/cli/azure/network/private-link-service#az-network-private-link-service-create) Bu örnek, *myResourceGroup*adlı kaynak grubunda *myLoadBalancer* adlı Standart Yük Dengeleyici'ni kullanarak *myPLS* adlı bir Özel Bağlantı hizmeti oluşturur. 
 
```azurecli-interactive
az network private-link-service create \
--resource-group myResourceGroup \
--name myPLS \
--vnet-name myVirtualNetwork \
--subnet mySubnet \
--lb-name myILB \
--lb-frontend-ip-configs myFrontEnd \
--location westcentralus 
```
Oluşturulduktan sonra, Özel Bağlantı Hizmeti Kimliği'ne dikkat edin. Bu hizmete bağlantı istemek için daha sonra buna ihtiyacınız olacaktır.  
 
Bu aşamada, Private Link hizmetiniz başarıyla oluşturulur ve trafiği almaya hazırdır. Yukarıdaki örneğin yalnızca Azure CLI kullanarak Özel Bağlantı hizmeti oluşturmayı göstermek için olduğunu unutmayın.  Trafiği dinlemek için yük dengeleyici arka uç havuzlarını veya arka uç havuzlarındaki herhangi bir uygulamayı yapılandırmadık. Uçuça trafik akışlarını görmek istiyorsanız, uygulamanızı Standart Yük Dengeleyicinizin arkasına yapılandırmanız önemle tavsiye edilir.  
 
Daha sonra, Azure CLI'yi kullanarak bu hizmeti farklı sanal ağdaki özel bir bitiş noktasına nasıl eşleyebileceğimizi göstereceğiz. Yine, örnek özel bitiş noktası oluşturma ve Azure CLI kullanarak yukarıda oluşturulan Özel Bağlantı hizmetine bağlanma ile sınırlıdır. Ayrıca, sanal ağda trafiği özel bitiş noktasına göndermek/almak için sanal makineler oluşturabilirsiniz.        
 
## <a name="private-endpoints"></a>Özel uç noktalar

### <a name="create-the-virtual-network"></a>Sanal ağ oluşturma 
 [az ağ vnet oluşturmak](/cli/azure/network/vnet#az-network-vnet-create)ile sanal bir ağ oluşturun. Bu örnek, *myResourcegroup*adlı kaynak grubunda *myPEVNet* adlı bir sanal ağ oluşturur: 
```azurecli-interactive
az network vnet create \
--resource-group myResourceGroup \
--name myPEVnet \
--address-prefix 10.0.0.0/16  
```
### <a name="create-the-subnet"></a>Alt ağı oluşturma 
 [Az ağ vnet subnet oluşturmak](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create)ile sanal ağda bir alt ağ oluşturun. Bu örnek, *myResourcegroup*adlı kaynak grubunda *myPEVnet* adlı sanal ağda *mySubnet* adlı bir alt ağ oluşturur: 

```azurecli-interactive 
az network vnet subnet create \
--resource-group myResourceGroup \
--vnet-name myPEVnet \
--name myPESubnet \
--address-prefixes 10.0.0.0/24 
```   
## <a name="disable-private-endpoint-network-policies-on-subnet"></a>Alt ağdaki özel uç nokta ağ ilkelerini devre dışı 
Özel bitiş noktası, sanal ağ içinde seçtiğiniz herhangi bir alt ağda oluşturulabilir. Şu anda, özel uç noktalarda ağ ilkelerini destekliyoruz.  Bu nedenle, alt ağdaki ağ ilkelerini devre dışı bmeliyiz. [Az network vnet subnet güncelleştirmesi](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update)ile özel uç nokta ağ ilkelerini devre dışı kakmak için alt ağı güncelleştirin. 

```azurecli-interactive
az network vnet subnet update \
--resource-group myResourceGroup \
--vnet-name myPEVnet \
--name myPESubnet \
--disable-private-endpoint-network-policies true 
```
## <a name="create-private-endpoint-and-connect-to-private-link-service"></a>Özel bitiş noktası oluşturun ve özel bağlantı hizmetine bağlanın 
Sanal aağınızda yukarıda oluşturulan Özel Bağlantı hizmetini tüketmek için özel bir bitiş noktası oluşturun:
  
```azurecli-interactive
az network private-endpoint create \
--resource-group myResourceGroup \
--name myPE \
--vnet-name myPEVnet \
--subnet myPESubnet \
--private-connection-resource-id {PLS_resourceURI} \
--connection-name myPEConnectingPLS \
--location westcentralus 
```
Özel *Bağlantı* hizmetinde `az network private-link-service show` özel bağlantı kaynak kimliği alabilirsiniz. Kimlik aşağıdaki gibi görünecektir:   
/subscriptions/subID/resourceGroups/*resourcegroupname*/providers/Microsoft.Network/privateLinkServices/**privatelinkservicename** 
 
## <a name="show-private-link-service-connections"></a>Özel Bağlantı servis bağlantılarını göster 
 
[Az network özel bağlantı hizmeti gösterisini](/cli/azure/network/private-link-service#az-network-private-link-service-show)kullanarak Özel Bağlantı hizmetinizdeki bağlantı isteklerini görün.    
```azurecli-interactive 
az network private-link-service show --resource-group myResourceGroup --name myPLS 
```
## <a name="next-steps"></a>Sonraki adımlar
- [Azure Özel Bağlantı hizmeti](private-link-service-overview.md) hakkında daha fazla bilgi edinin
 
