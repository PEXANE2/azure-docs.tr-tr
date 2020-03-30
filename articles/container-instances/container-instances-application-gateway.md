---
title: Konteyner grubu için statik IP adresi
description: Sanal ağda bir kapsayıcı grubu oluşturun ve statik bir ön uç IP adresini kapsayıcı web uygulamasına maruz bırakmak için bir Azure uygulama ağ geçidi kullanın
ms.topic: article
ms.date: 03/16/2020
ms.openlocfilehash: 5c3a14f93af3ecc614dc296f0a4d2815d7a64a66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481798"
---
# <a name="expose-a-static-ip-address-for-a-container-group"></a>Kapsayıcı grubu için statik bir IP adresi ortaya çıkarma

Bu makalede, bir Azure [uygulama ağ geçidi](../application-gateway/overview.md)kullanarak bir kapsayıcı [grubu](container-instances-container-groups.md) için statik, ortak IP adresi ortaya çıkarmak için bir yol gösterilmektedir. Azure Kapsayıcı Örnekleri'nde çalışan dışa dönük kapsayıcılı bir uygulama için statik bir giriş noktasına ihtiyaç duyduğunuzda aşağıdaki adımları izleyin. 

Bu makalede, bu senaryo için kaynakları oluşturmak için Azure CLI'yi kullanıyorsunuz:

* Azure sanal ağı
* Küçük bir web uygulaması barındıran [sanal ağda (önizleme)](container-instances-vnet.md) dağıtılan bir kapsayıcı grubu
* Ortak ön uç IP adresi, ağ geçidinde bir web sitesini barındıracak bir dinleyici ve arka uç kapsayıcı grubuna giden bir rota içeren bir uygulama ağ geçidi

Uygulama ağ geçidi çalıştığı ve kapsayıcı grubunun ağın devredilen alt ağında kararlı bir özel IP adresi ortaya çıkardığı sürece, kapsayıcı grubuna bu genel IP adresinden erişilebilir.

> [!NOTE]
> Azure, ağ geçidinin sağlandığı ve kullanılabilir olduğu süreye ve işlediği veri miktarına bağlı olarak bir uygulama ağ geçidi için ücret lendirilir. [Fiyatlandırmaya](https://azure.microsoft.com/pricing/details/application-gateway/)bakın.

## <a name="create-virtual-network"></a>Sanal ağ oluşturma

Tipik bir durumda, zaten bir Azure sanal ağınız olabilir. Yoksa, aşağıdaki örnek komutlarla gösterildiği gibi bir tane oluşturun. Sanal ağ, uygulama ağ geçidi ve kapsayıcı grubu için ayrı alt ağlara ihtiyaç duyar.

İhtiyacınız varsa, bir Azure kaynak grubu oluşturun. Örnek:

```azureci
az group create --name myResourceGroup --location eastus
```

[az ağ vnet komutu oluşturmak][az-network-vnet-create] ile sanal bir ağ oluşturun. Bu komut ağdaki *myAGSubnet* alt ağını oluşturur.

```azurecli
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroup \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
```

Arka uç kapsayıcı grubu için bir alt ağ oluşturmak için [az ağ vnet subnet oluşturma][az-network-vnet-subnet-create] komutunu kullanın. Burada *myACISubnet*adlı.

```azurecli
az network vnet subnet create \
  --name myACISubnet \
  --resource-group myResourceGroup \
  --vnet-name myVNet   \
  --address-prefix 10.0.2.0/24
```

Statik bir genel IP kaynağı oluşturmak için [az ağ public-ip oluşturma][az-network-public-ip-create] komutunu kullanın. Daha sonraki bir adımda, bu adres uygulama ağ geçidinin ön ucu olarak yapılandırılır.

```azurecli
az network public-ip create \
  --resource-group myResourceGroup \
  --name myAGPublicIPAddress \
  --allocation-method Static \
  --sku Standard
```

## <a name="create-container-group"></a>Kapsayıcı grubu oluştur

Önceki adımda yapılandırdığınız sanal ağda bir kapsayıcı grubu oluşturmak için aşağıdaki [az kapsayıcı oluşturmayı][az-container-create] çalıştırın. 

Grup *myACISubnet* alt ağına dağıtılır ve `aci-helloworld` görüntüyü çeken *ek kapsayıcı* adlı tek bir örnek içerir. Belgelerdeki diğer makalelerde gösterildiği gibi, bu resim, statik bir HTML sayfasına hizmet veren Node.js ile yazılmış küçük bir web uygulamasını paketler. 

```azurecli
az container create \
  --name appcontainer \
  --resource-group myResourceGroup \
  --image mcr.microsoft.com/azuredocs/aci-helloworld \
  --vnet myVNet \
  --subnet myACISubnet
```

Başarıyla dağıtıldığında, kapsayıcı grubuna sanal ağda özel bir IP adresi atanır. Örneğin, grubun IP adresini almak için aşağıdaki [az kapsayıcı göster][az-container-show] komutunu çalıştırın:

```azurecli
az container show \
  --name appcontainer --resource-group myResourceGroup \
  --query ipAddress.ip --output tsv
```

Çıktı şuna benzer olacaktır: `10.0.2.4`.

Daha sonraki bir adımda kullanmak için IP adresini bir ortam değişkenine kaydedin:

```azurecli
ACI_IP=$(az container show \
  --name appcontainer \
  --resource-group myResourceGroup \
  --query ipAddress.ip --output tsv)
```

## <a name="create-application-gateway"></a>Uygulama ağ geçidi oluşturma

[Uygulama ağ geçidindeki](../application-gateway/quick-create-cli.md)adımları izleyerek sanal ağda bir uygulama ağ geçidi oluşturun. Aşağıdaki [az ağ uygulama ağ geçidi oluşturma][az-network-application-gateway-create] komutu, ortak ön uç IP adresi ve arka uç kapsayıcı grubuna giden bir rota içeren bir ağ geçidi oluşturur. Ağ geçidi ayarları yla ilgili ayrıntılar için [Uygulama Ağ Geçidi belgelerine](/azure/application-gateway/) bakın.

```azurecli
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroup \
  --capacity 2 \
  --sku Standard_v2 \
  --http-settings-protocol http \
  --public-ip-address myAGPublicIPAddress \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --servers "$ACI_IP" 
```


Azure'un uygulama ağ geçidini oluşturması 15 dakika kadar sürebilir. 

## <a name="test-public-ip-address"></a>Ortak IP adresini test edin
  
Artık uygulama ağ geçidinin arkasındaki kapsayıcı grubunda çalışan web uygulamasına erişimi test edebilirsiniz.

Ağ geçidinin ön uç ortak IP adresini almak için [az ağı public-ip show][az-network-public-ip-show] komutunu çalıştırın:

```azurecli
az network public-ip show \
--resource-group myresourcegroup \
--name myAGPublicIPAddress \
--query [ipAddress] \
--output tsv
```

Çıktı, genel bir IP adresidir, benzer: `52.142.18.133`.

Başarılı bir şekilde yapılandırıldığında çalışan web uygulamasını görüntülemek için tarayıcınızdaki ağ geçidinin genel IP adresine gidin. Başarılı erişim şuna benzer:

![Bir Azure kapsayıcı örneğinde çalışan uygulamayı gösteren tarayıcı ekran görüntüsü](./media/container-instances-application-gateway/aci-app-app-gateway.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bir uygulama ağ geçidinin arkasındaki arka uç sunucusu olarak WordPress kapsayıcı örneğini içeren bir kapsayıcı grubu oluşturmak için [hızlı başlangıç şablonuna](https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress-vnet) bakın.
* Ayrıca, SSL sonlandırma sertifikası içeren bir uygulama ağ geçidini de yapılandırabilirsiniz. Genel [bakışve](../application-gateway/ssl-overview.md) [öğreticiye](../application-gateway/create-ssl-portal.md)bakın.
* Senaryonuza bağlı olarak, Azure Kapsayıcı Örnekleri ile diğer Azure yük dengeleme çözümlerini kullanmayı düşünün. Örneğin, trafiği birden çok kapsayıcı örneğine ve birden çok bölgeye dağıtmak için [Azure Trafik Yöneticisi'ni](../traffic-manager/traffic-manager-overview.md) kullanın. Bu [blog yazısı](https://aaronmsft.com/posts/azure-container-instances/)bakın.

[az-network-vnet-create]:  /cli/azure/network/vnet#az-network-vnet-create
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-show]: /cli/azure/network/public-ip#az-network-public-ip-show
[az-network-application-gateway-create]: /cli/azure/network/application-gateway#az-network-application-gateway-create
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show