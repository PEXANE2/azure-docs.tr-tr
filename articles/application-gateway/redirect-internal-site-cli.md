---
title: CLI kullanarak dahili yeniden yönlendirme
titleSuffix: Azure Application Gateway
description: Azure CLI'yi kullanarak dahili web trafiğini uygun havuza yönlendiren bir uygulama ağ geçidioluşturmayı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: victorh
ms.openlocfilehash: 7d37e36a4cdfed462904e2d02871345ad89d7ac9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74074562"
---
# <a name="create-an-application-gateway-with-internal-redirection-using-the-azure-cli"></a>Azure CLI'yi kullanarak dahili yeniden yönlendirme içeren bir uygulama ağ geçidi oluşturma

Bir [uygulama ağ geçidi](overview.md)oluştururken web trafiği yeniden [yönlendirmesini](multiple-site-overview.md) yapılandırmak için Azure CLI'yi kullanabilirsiniz. Bu öğreticide, sanal makine ölçeği kümesini kullanarak bir arka uç havuzu tanımlarsınız. Ardından, web trafiğinin uygun havuza ulaştığından emin olmak için sahip olduğunuz etki alanlarını temel alan olarak dinleyicileri ve kuralları yapılandırırsınız. Bu öğretici, birden çok etki alanına sahip olduğunuzu varsayar ve *www\.contoso.com* ve *www\.contoso.org*örneklerini kullanır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * Uygulama ağ geçidi oluşturma
> * Dinleyici ekleme ve yeniden yönlendirme kuralı
> * Arka uç havuzu yla sanal makine ölçeği kümesi oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun.

Aşağıdaki örnek *eastus* konumunda *myResourceGroupAG* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 

[az network vnet create](/cli/azure/network/vnet) komutunu kullanarak *myVNet* adlı sanal ağı ve *myAGSubnet* adlı alt ağı oluşturun. Daha sonra [az ağ vnet subnet oluşturmak](/cli/azure/network/vnet/subnet)kullanarak sunucuların arka uç havuzu tarafından gerekli *myBackendSubnet* adlı alt ağ ekleyebilirsiniz. [az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create) komutunu kullanarak *myAGPublicIPAddress* adlı genel IP adresini oluşturun.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

*myAppGateway* adlı uygulama ağ geçidini oluşturmak için [az network application-gateway create](/cli/azure/network/application-gateway) komutunu kullanabilirsiniz. Azure CLI kullanarak bir uygulama ağ geçidi oluşturduğunuzda, kapasite, sku ve HTTP ayarları gibi yapılandırma bilgilerini belirtirsiniz. Uygulama ağ geçidi, *myAGSubnet*’e ve daha önce oluşturduğunuz *myAGPublicIPAddress*’e atanır. 

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

Uygulama ağ geçidinin oluşturulması birkaç dakika sürebilir. Uygulama ağ geçidi oluşturulduktan sonra şu yeni özellikleri görürsünüz:

- *appGatewayBackendPool*: Bir uygulama ağ geçidi en az bir arka uç adres havuzuna sahip olmalıdır.
- *appGatewayBackendHttpSettings*: İletişim için 80 numaralı bağlantı noktasının ve HTTP protokolünün kullanıldığını belirtir.
- *appGatewayHttpListener*: *appGatewayBackendPool* ile ilişkili varsayılan dinleyicidir.
- *appGatewayFrontendIP*: *appGatewayHttpListener*’a *myAGPublicIPAddress*’i atar.
- *kural 1*: *appGatewayHttpListener* ile ilişkili varsayılan yönlendirme kuralıdır.


## <a name="add-listeners-and-rules"></a>Dinleyiciler ve kurallar ekleme 

Uygulama ağ geçidinin trafiği arka uç havuzuna uygun şekilde yönlendirmesini sağlamak içn bir dinleyici gereklidir. Bu öğreticide iki etki alanınız için iki dinleyici oluşturacaksınız. Bu örnekte, *dinleyiciler www\.contoso.com* ve www *\.contoso.org*etki alanları için oluşturulur.

[az network application-gateway http-listener create](/cli/azure/network/application-gateway/http-listener#az-network-application-gateway-http-listener-create) kullanarak gereken arka uç dinleyicilerini ekleyin.

```azurecli-interactive
az network application-gateway http-listener create \
  --name contosoComListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.contoso.com
az network application-gateway http-listener create \
  --name contosoOrgListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.contoso.org   
  ```

### <a name="add-the-redirection-configuration"></a>Yeniden yönlendirme yapılandırmasını ekleme

[Az network application-gateway yönlendirme-config oluşturma](/cli/azure/network/application-gateway/redirect-config#az-network-application-gateway-redirect-config-create)kullanarak uygulama ağ geçidiwww contoso.com için *dinleyiciye\.* www *\.consoto.org* trafik gönderen yeniden yönlendirme yapılandırması ekleyin.

```azurecli-interactive
az network application-gateway redirect-config create \
  --name orgToCom \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Permanent \
  --target-listener contosoListener \
  --include-path true \
  --include-query-string true
```

### <a name="add-routing-rules"></a>Yönlendirme kuralları ekleme

Kurallar oluşturuldukları sırayla işlenir ve trafik, uygulama ağ geçidine gönderilen URL ile eşleşen ilk kural kullanılarak yönlendirilir. Örneğin, aynı bağlantı noktasında temel bir dinleyici kullanan bir kuralınız ve çok siteli dinleyici kullanan bir kuralınız varsa çok siteli kuralın beklendiği gibi çalışması için çok siteli dinleyicinin kuralı temel dinleyici kuralından önce listelenmelidir. 

Bu örnekte, iki yeni kural oluşturur ve oluşturulan varsayılan kuralı silersiniz.  Kuralı [az network application-gateway rule create](/cli/azure/network/application-gateway/rule#az-network-application-gateway-rule-create) komutunu kullanarak ekleyebilirsiniz.

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name contosoComRule \
  --resource-group myResourceGroupAG \
  --http-listener contosoComListener \
  --rule-type Basic \
  --address-pool appGatewayBackendPool
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name contosoOrgRule \
  --resource-group myResourceGroupAG \
  --http-listener contosoOrgListener \
  --rule-type Basic \
  --redirect-config orgToCom
az network application-gateway rule delete \
  --gateway-name myAppGateway \
  --name rule1 \
  --resource-group myResourceGroupAG
```

## <a name="create-virtual-machine-scale-sets"></a>Sanal makine ölçek kümesi oluşturma

Bu örnekte, oluşturduğunuz arka uç havuzunu destekleyen sanal bir makine ölçeği kümesi oluşturursunuz. Oluşturduğunuz ölçek kümesi *myvmss* olarak adlandırılır ve NGINX yüklediğiniz iki sanal makine örneği içerir.

```azurecli-interactive
az vmss create \
  --name myvmss \
  --resource-group myResourceGroupAG \
  --image UbuntuLTS \
  --admin-username azureuser \
  --admin-password Azure123456! \
  --instance-count 2 \
  --vnet-name myVNet \
  --subnet myBackendSubnet \
  --vm-sku Standard_DS2 \
  --upgrade-policy-mode Automatic \
  --app-gateway myAppGateway \
  --backend-pool-name appGatewayBackendPool
```

### <a name="install-nginx"></a>NGINX yükleme

Bu komutu kabuk penceresinde çalıştırın:

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh" }'
```

## <a name="create-cname-record-in-your-domain"></a>Etki alanınızda CNAME kaydı oluşturma

Uygulama ağ geçidi genel IP adresiyle oluşturulduktan sonra DNS adresini alabilir ve etki alanınızda bir CNAME kaydı oluşturmak için kullanabilirsiniz. Uygulama ağ geçidinin DNS adresini almak için [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show) komutunu kullanabilirsiniz. DNSSetting için *fqdn* değerini kopyalayın ve bu değeri oluşturduğunuz CNAME kaydının değeri olarak kullanın. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebileceğinden A kayıtlarının kullanımı önerilmez.

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [dnsSettings.fqdn] \
  --output tsv
```

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Tarayıcınızın adres çubuğuna, etki alanı adınızı girin. Mesela, http:\//www.contoso.com.

![Uygulama ağ geçidinde contoso test etme](./media/redirect-internal-site-cli/application-gateway-nginxtest.png)

Adresi diğer etki alanınızla değiştirin,\/örneğin http: /www.contoso.org ve trafiğin www\.contoso.com için dinleyiciye yönlendirildiğini görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> * Ağı ayarlama
> * Uygulama ağ geçidi oluşturma
> * Dinleyici ekleme ve yeniden yönlendirme kuralı
> * Arka uç havuzu yla sanal makine ölçeği kümesi oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturma
