---
title: 'Hızlı başlangıç: NAT ağ geçidi oluşturma - Azure CLI'
titlesuffix: Azure Virtual Network NAT
description: Bu hızlı başlangıç, Azure CLI'yi kullanarak bir NAT ağ geçidinin nasıl oluşturulurolduğunu gösterir
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumundD
Customer intent: I want to create a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 02/18/2020
ms.author: allensu
ms.openlocfilehash: 9402960927f56092e226ab81bd3e6ede0cf6a52d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79202207"
---
# <a name="quickstart-create-a-nat-gateway-using-azure-cli"></a>Hızlı başlangıç: Azure CLI'yi kullanarak Bir NAT ağ geçidi oluşturma

Bu hızlı başlangıç, Azure Sanal Ağ NAT hizmetini nasıl kullanacağınızı gösterir. Azure'daki sanal bir makine için giden bağlantı sağlamak için bir NAT ağ geçidi oluşturursunuz. 


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu öğreticiyi Azure Cloud Shell'i kullanarak tamamlayabilir veya ilgili komutları yerel olarak çalıştırabilirsiniz.  Azure Cloud Shell'i hiç kullanmadıysanız, ilk kuruluma geçmek için [şimdi oturum açın.](https://shell.azure.com)
Bu komutları yerel olarak çalıştırmayı seçerseniz, CLI'yi yüklemeniz gerekir.  Bu öğretici, Azure CLI sürümü 2.0.71 veya sonraki sürümlerini çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki **örnekeastus2** konumunda **myResourceGroupNAT** adlı bir kaynak grubu oluşturur:

```azurecli-interactive
  az group create \
    --name myResourceGroupNAT \
    --location eastus2
```

## <a name="create-the-nat-gateway"></a>NAT Ağ Geçidi oluşturma

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Genel Internet'e erişmek için NAT ağ geçidi için bir veya daha fazla genel IP adresine ihtiyacınız vardır. **myResourceGroupNAT'ta** **myPublicIP** adlı genel bir IP adresi kaynağı oluşturmak için [az network public-ip oluşturmayı](https://docs.microsoft.com/cli/azure/network/public-ip) kullanın.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupNAT \
    --name myPublicIP \
    --sku standard
```

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

Bir veya daha fazla genel IP adresi kaynağı, genel IP önekleri veya her ikisini de NAT ağ geçidiyle kullanabilirsiniz. Bu senaryoya göstermek için genel bir IP öneki kaynağı ekleyeceğiz.   **myResourceGroupNAT'ta** **myPublicIPprefix** adlı genel bir IP öneki kaynağı oluşturmak için [az network public-ip öneki oluşturmayı](https://docs.microsoft.com/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-create) kullanın.

```azurecli-interactive
  az network public-ip prefix create \
    --resource-group myResourceGroupNAT \
    --name myPublicIPprefix \
    --length 31
```

### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

Bu bölümde, NAT ağ geçidi kaynağını kullanarak NAT hizmetinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz ayrıntılı olarak açıklanmaktadır:
  - NAT ağ geçidi kaynağı tarafından çevrilen giden akışlar için kullanılacak genel BIR IP havuzu ve genel IP öneki.
  - Boşta kalma süresini varsayılan 4 dakikadan 10 dakikaya değiştirin.

**myNATağ geçidi**adlı [az ağ nat ağ geçidi oluşturarak](https://docs.microsoft.com/cli/azure/network/nat?view=azure-cli-latest) küresel bir Azure NAT ağ geçidi oluşturun. Komut hem genel IP adresi **myPublicIP** ve ortak IP öneki **myPublicIPprefix**kullanır. Komut, boşta kalma süresini **10** dakikaya değiştirir.

```azurecli-interactive
  az network nat gateway create \
    --resource-group myResourceGroupNAT \
    --name myNATgateway \
    --public-ip-addresses myPublicIP \
    --public-ip-prefixes myPublicIPprefix \
    --idle-timeout 10       
  ```

Bu noktada, NAT ağ geçidi işlevseldir ve eksik olan tek şey, sanal bir ağın hangi alt ağlarını kullanması gerektiğini yapılandırmaktır.

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma

Bir VM dağıtmadan ve NAT ağ geçidinizi kullanmadan önce sanal ağı oluşturmamız gerekir.

[az ağ vnet oluşturmak](https://docs.microsoft.com/cli/azure/network/vnet)kullanarak **myResourceGroupNAT** **mySubnet** adlı bir alt ağ ile **myVnet** adlı bir sanal ağ oluşturun.  Sanal ağ için IP adresi alanı **192.168.0.0/16'dır.** Sanal ağ içindeki alt ağ **192.168.0.0/24'dür.**

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupNAT \
    --location eastus2 \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.0.0/24
```

### <a name="configure-nat-service-for-source-subnet"></a>Kaynak alt ağı için NAT hizmetini yapılandırma

Biz [az ağ vnet subnet güncelleştirmesi](https://docs.microsoft.com/cli/azure/network/vnet/subnet)ile belirli bir NAT ağ geçidi kaynak **myNATgateway** kullanmak için sanal ağ **myVnet** kaynak **mySubnet** yapılandıracağız.  Bu komut, belirtilen alt ağdaki NAT hizmetini etkinleştirir.

```azurecli-interactive
  az network vnet subnet update \
    --resource-group myResourceGroupNAT \
    --vnet-name myVnet \
    --name mySubnet \
    --nat-gateway myNATgateway
```

Internet hedeflerine giden tüm giden trafik artık NAT ağ geçidini kullanıyor.  Bir UDR yapılandırmak için gerekli değildir.

## <a name="create-a-vm-to-use-the-nat-service"></a>NAT hizmetini kullanmak için bir VM oluşturma

Şimdi NAT hizmetini kullanmak için bir VM oluşturacağız.  Bu VM, VM'ye erişmenizi sağlamak için örnek düzeyinde public IP olarak kullanabileceği ortak bir IP'ye sahiptir.  NAT hizmeti akış yönü farkındadır ve alt anızdaki varsayılan Internet hedefinin yerini alır. VM'nin genel IP adresi giden bağlantılar için kullanılmaz.

### <a name="create-public-ip-for-source-vm"></a>Kaynak VM için ortak IP oluşturma

VM'ye erişmek için kullanılacak genel bir IP oluştururuz.  **myResourceGroupNAT'ta** **myPublicIPVM** adlı genel bir IP adresi kaynağı oluşturmak için [az ağ public-ip oluşturmayı](https://docs.microsoft.com/cli/azure/network/public-ip) kullanın.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupNAT \
    --name myPublicIPVM \
    --sku standard
```

### <a name="create-an-nsg-for-vm"></a>VM için NSG oluşturma

Standart Genel IP adresleri 'varsayılan olarak güvenli' olduğundan, ssh erişimine izin vermek için bir NSG oluşturmamız gerekir. **MyResourceGroupNAT'ta** **myNSG** adında bir NSG kaynağı oluşturmak için [az ağ nsg'yi kullanın.](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create)

```azurecli-interactive
  az network nsg create \
    --resource-group myResourceGroupNAT \
    --name myNSG 
```

### <a name="expose-ssh-endpoint-on-source-vm"></a>Kaynak VM'de SSH uç noktasını açığa çıkarma

NSG'de SSH'nin kaynak vm'ye erişimi için bir kural oluşturuyoruz. **MyResourceGroupNAT'ta** **myNSG** adlı NSG'de **ssh** adlı bir NSG kuralı oluşturmak için [az network nsg kuralını kullanın.](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create)

```azurecli-interactive
  az network nsg rule create \
    --resource-group myResourceGroupNAT \
    --nsg-name myNSG \
    --priority 100 \
    --name ssh \
    --description "SSH access" \
    --access allow \
    --protocol tcp \
    --direction inbound \
    --destination-port-ranges 22
```

### <a name="create-nic-for-vm"></a>VM için NIC oluşturma

[Az network nic oluşturmak](/cli/azure/network/nic#az-network-nic-create) ve Ortak IP adresi ve ağ güvenlik grubu ile ilişkilendirmek ile bir ağ arabirimi oluşturun. 

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupNAT \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIPVM \
    --network-security-group myNSG
```

### <a name="create-vm"></a>VM oluşturma

[Az vm oluşturmak](/cli/azure/vm#az-vm-create)ile sanal makine oluşturun.  Bu VM için ssh tuşları üretiyoruz ve daha sonra kullanmak üzere özel anahtarı saklıyoruz.

 ```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupNAT \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --generate-ssh-keys
```

VM'nin dağıtılmasını bekleyin ve diğer adımlarla devam edin.

## <a name="discover-the-ip-address-of-the-vm"></a>VM'nin IP adresini keşfedin

Öncelikle oluşturduğunuz VM'nin IP adresini keşfetmemiz gerekir. VM'nin genel IP adresini almak için [az network public-ip show'u](/cli/azure/network/public-ip#az-network-public-ip-show)kullanın. 

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupNAT \
    --name myPublicIPVM \
    --query [ipAddress] \
    --output tsv
``` 

>[!IMPORTANT]
>Ortak IP adresini kopyalayın ve VM'ye erişmek için kullanabilmeniz için not defterine yapıştırın.

### <a name="sign-in-to-vm"></a>VM'de oturum açın

SSH kimlik bilgileri, önceki işlemdeki Bulut Kabuğunuzda depolanmalıdır.  Tarayıcınızda bir [Azure Bulut Kabuğu](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın.

```bash
ssh <ip-address-destination>
```

Artık NAT hizmetini kullanmaya hazırsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az grubu silme](/cli/azure/group#az-group-delete) komutunu kullanabilirsiniz.

```azurecli-interactive 
  az group delete \
    --name myResourceGroupNAT
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kullanmak için bir NAT ağ geçidi ve bir VM oluşturdunuz. 

NAT hizmetinizin çalıştığını görmek için Azure Monitor'da ölçümleri inceleyin. Kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanı.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi de eklenerek giderilir.


- Azure [Sanal Ağ NAT](./nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [Azure CLI'yi kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-cli.md)dağıtmak için hızlı başlatın.
- [Azure PowerShell'i kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-powershell.md)dağıtmak için hızlı başlatın.
- [Azure portalını kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-portal.md)dağıtmak için hızlı başlatın.
> [!div class="nextstepaction"]

