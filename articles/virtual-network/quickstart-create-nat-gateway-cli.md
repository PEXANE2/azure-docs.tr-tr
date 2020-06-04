---
title: 'Hızlı başlangıç: NAT ağ geçidi oluşturma-Azure CLı'
titlesuffix: Azure Virtual Network NAT
description: Bu hızlı başlangıçta, Azure CLı kullanarak bir NAT Gateway oluşturma gösterilmektedir
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumundD
Customer intent: I want to create a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.subservice: nat
ms.devlang: na
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 02/18/2020
ms.author: allensu
ms.openlocfilehash: dcc3eda4c19e8d15040feb31d2a45922035c6cb0
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84341550"
---
# <a name="quickstart-create-a-nat-gateway-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak NAT ağ geçidi oluşturma

Bu hızlı başlangıçta Azure sanal ağ NAT hizmetini nasıl kullanacağınız gösterilmektedir. Azure 'da bir sanal makine için giden bağlantı sağlamak üzere bir NAT ağ geçidi oluşturacaksınız. 


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu öğreticiyi Azure Cloud Shell kullanarak tamamlayabilirsiniz veya ilgili komutları yerel olarak çalıştırabilirsiniz.  Azure Cloud Shell hiç kullanmadıysanız, İlk kurulumdan geçmek için [Şimdi oturum açın](https://shell.azure.com) .
Bu komutları yerel olarak çalıştırmayı seçerseniz, CLı yüklemeniz gerekir.  Bu öğreticide, Azure CLı sürüm 2.0.71 veya sonraki bir sürümünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek **eastus2** konumunda **Myresourcegroupnat** adlı bir kaynak grubu oluşturur:

```azurecli-interactive
  az group create \
    --name myResourceGroupNAT \
    --location eastus2
```

## <a name="create-the-nat-gateway"></a>NAT ağ geçidini oluşturma

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Genel Internet 'e erişmek için NAT ağ geçidi için bir veya daha fazla genel IP adresi gerekir. **Myresourcegroupnat**Içinde **Mypublicıp** adlı bir genel IP adresi kaynağı oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip) komutunu kullanın.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupNAT \
    --name myPublicIP \
    --sku standard
```

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

NAT ağ geçidiyle bir veya daha fazla genel IP adresi kaynağı, genel IP ön ekleri veya her ikisini birden kullanabilirsiniz. Bu senaryoya göstermek için genel bir IP öneki kaynağı ekleyeceğiz.   **Myresourcegroupnat**Içinde **Mypublicipprefix** adlı bir genel IP ön eki kaynağı oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-create) komutunu kullanın.

```azurecli-interactive
  az network public-ip prefix create \
    --resource-group myResourceGroupNAT \
    --name myPublicIPprefix \
    --length 31
```

### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

Bu bölümde, NAT ağ geçidi kaynağını kullanarak NAT hizmetinin aşağıdaki bileşenlerini oluşturma ve yapılandırma işlemlerinin ayrıntıları verilmiştir:
  - NAT ağ geçidi kaynağı tarafından çevrilen giden akışlar için kullanılacak genel IP havuzu ve genel IP öneki.
  - Boşta kalma zaman aşımını varsayılan olarak 4 dakika ile 10 dakika arasında değiştirin.

[Az Network NAT Gateway](https://docs.microsoft.com/cli/azure/network/nat?view=azure-cli-latest) adlı **mynatgateway**ADLı bir genel Azure NAT ağ geçidi oluşturun. Komut, **Mypublicıp** genel IP adresini ve **Mypublicipprefix**genel IP önekini kullanır. Komut, boşta kalma zaman aşımını **10** dakika olarak değiştirir.

```azurecli-interactive
  az network nat gateway create \
    --resource-group myResourceGroupNAT \
    --name myNATgateway \
    --public-ip-addresses myPublicIP \
    --public-ip-prefixes myPublicIPprefix \
    --idle-timeout 10       
  ```

Bu noktada, NAT ağ geçidi çalışır ve yok, bir sanal ağın hangi alt ağlarının bunu kullanması gerektiğini yapılandırmaktır.

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma

Bir VM 'yi dağıtmadan ve NAT ağ geçidinizi kullanabilmeniz için, sanal ağı oluşturmanız gerekir.

[Az Network VNET Create](https://docs.microsoft.com/cli/azure/network/vnet)kullanılarak **Myresourcegroupnat** içinde **mysubnet** adlı bir alt ağ ile **myvnet** adlı bir sanal ağ oluşturun.  Sanal ağın IP adresi alanı **192.168.0.0/16**' dır. Sanal ağ içindeki alt ağ **192.168.0.0/24**' dir.

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupNAT \
    --location eastus2 \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.0.0/24
```

### <a name="configure-nat-service-for-source-subnet"></a>Kaynak alt ağ için NAT hizmetini yapılandırma

**Myvnet** sanal ağındaki **mysubnet** kaynak alt ağını, [az Network VNET alt ağ güncelleştirmesiyle](https://docs.microsoft.com/cli/azure/network/vnet/subnet) **MYNATGATEWAY** adlı belirli bir NAT Gateway kaynağını kullanacak şekilde yapılandıracağız.  Bu komut, belirtilen alt ağda NAT hizmetini etkinleştirir.

```azurecli-interactive
  az network vnet subnet update \
    --resource-group myResourceGroupNAT \
    --vnet-name myVnet \
    --name mySubnet \
    --nat-gateway myNATgateway
```

Internet hedeflerine giden tüm trafik artık NAT ağ geçidini kullanıyor.  UDR 'yi yapılandırmak gerekli değildir.

## <a name="create-a-vm-to-use-the-nat-service"></a>NAT hizmetini kullanmak için bir VM oluşturma

Artık NAT hizmetini kullanmak için bir VM oluşturacağız.  Bu VM 'nin VM 'ye erişiminizi sağlamak için örnek düzeyi genel IP olarak kullanılacak genel bir IP 'si vardır.  NAT hizmeti akış yönü farkındadır ve alt ağınızdaki varsayılan Internet hedefini değiştirecek. VM 'nin genel IP adresi giden bağlantılar için kullanılmayacak.

### <a name="create-public-ip-for-source-vm"></a>Kaynak VM için genel IP oluşturma

SANAL makineye erişmek için kullanılacak bir genel IP oluşturacağız.  **Myresourcegroupnat**Içinde **Mypublicipvm** adlı bir genel IP adresi kaynağı oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip) komutunu kullanın.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupNAT \
    --name myPublicIPVM \
    --sku standard
```

### <a name="create-an-nsg-for-vm"></a>VM için bir NSG oluşturma

Standart genel IP adresleri ' varsayılan olarak güvenli ' olduğundan, SSH erişimi için gelen erişime izin vermek üzere bir NSG oluşturulması gerekir. **Myresourcegroupnat**Içinde **mynsg** adlı bir NSG kaynağı oluşturmak için [az Network NSG Create](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create) kullanın.

```azurecli-interactive
  az network nsg create \
    --resource-group myResourceGroupNAT \
    --name myNSG 
```

### <a name="expose-ssh-endpoint-on-source-vm"></a>Kaynak VM 'de SSH uç noktasını kullanıma sunma

Kaynak VM 'ye SSH erişimi için NSG 'de bir kural oluşturacağız. **Myresourcegroupnat**Içinde **mynsg** adlı NSG adında **SSH** adlı bir NSG kuralı oluşturmak için [az Network NSG Rule Create](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) kullanın.

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

[Az Network Nic Create](/cli/azure/network/nic#az-network-nic-create) komutuyla bir ağ arabirimi oluşturun ve genel IP adresi ve ağ güvenlik grubuyla ilişkilendirin. 

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

[Az VM Create](/cli/azure/vm#az-vm-create)ile sanal makineyi oluşturun.  Bu sanal makine için SSH anahtarları oluşturuyoruz ve özel anahtarı daha sonra kullanmak üzere depolarız.

 ```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupNAT \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --generate-ssh-keys
```

VM 'nin dağıtılmasını bekleyin, ardından kalan adımlara devam edin.

## <a name="discover-the-ip-address-of-the-vm"></a>VM 'nin IP adresini bulma

İlk olarak, oluşturduğunuz VM 'nin IP adresini bulduk. VM 'nin genel IP adresini almak için [az Network public-IP Show](/cli/azure/network/public-ip#az-network-public-ip-show)kullanın. 

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupNAT \
    --name myPublicIPVM \
    --query [ipAddress] \
    --output tsv
``` 

>[!IMPORTANT]
>Genel IP adresini kopyalayın ve ardından sanal makineye erişmek için kullanabilmek üzere bir not defteri 'ne yapıştırın.

### <a name="sign-in-to-vm"></a>VM 'de oturum açma

SSH kimlik bilgileri, önceki işlemden Cloud Shell depolanmalıdır.  Tarayıcınızda bir [Azure Cloud Shell](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın.

```bash
ssh <ip-address-destination>
```

Artık NAT hizmetini kullanmaya hazırsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve içinde yer alan tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive 
  az group delete \
    --name myResourceGroupNAT
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir NAT ağ geçidi ve onu kullanmak için bir VM oluşturdunuz. 

NAT hizmeti 'nin çalışıyor olduğunu görmek için Azure Izleyici 'de ölçümleri gözden geçirin. Kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanılayın.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi eklenerek adreslenir.


- [Azure sanal ağ NAT](./nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [Azure CLI kullanarak NAT Gateway kaynağını](./quickstart-create-nat-gateway-cli.md)dağıtmaya yönelik hızlı başlangıç.
- [Azure PowerShell kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-powershell.md)dağıtmaya yönelik hızlı başlangıç.
- [Azure Portal kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-portal.md)dağıtmaya yönelik hızlı başlangıç.
> [!div class="nextstepaction"]

