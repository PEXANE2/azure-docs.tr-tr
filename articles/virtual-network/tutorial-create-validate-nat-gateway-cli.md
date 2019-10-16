---
title: 'Öğretici: NAT Gateway oluşturma ve test etme-Azure CLı'
titlesuffix: Azure NAT service
description: Bu öğreticide, Azure CLı kullanarak bir NAT Gateway oluşturma ve NAT hizmetini test etme işlemlerinin nasıl yapılacağı gösterilmektedir
services: nat
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to test a NAT Gateway for outbound connectivity for my virtual network.
ms.custom: seodec18
ms.service: nat
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2019
ms.author: allensu
ms.openlocfilehash: f97284b3c629dbaab1b0cbb54b1f114342278da7
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72376493"
---
# <a name="tutorial-create-a-nat-gateway-using-azure-cli-and-test-the-nat-service"></a>Öğretici: Azure CLı kullanarak bir NAT ağ geçidi oluşturma ve NAT hizmetini test etme

Bu öğreticide, Azure NAT hizmetini kullanarak Azure 'daki sanal makineler için giden bağlantı sağlamak üzere bir NAT ağ geçidi oluşturma işlemlerinin nasıl yapılacağı gösterilir. NAT ağ geçidini test etmek için bir kaynak ve hedef sanal makine dağıtırsınız.  Kaynaktan hedef sanal makineye bir genel IP adresine giden bağlantı yaparak NAT ağ geçidini test edersiniz.  Bu öğretici, kaynak ve hedefi yalnızca kolaylık sağlaması için aynı kaynak grubundaki iki farklı sanal ağda dağıtır.

>[!NOTE] 
>Azure NAT hizmeti şu anda genel önizleme olarak kullanılabilir ve sınırlı sayıda [bölgede](https://azure.microsoft.com/global-infrastructure/regions/)kullanılabilir. Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.comsupport/legal/preview-supplemental-terms).


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu öğreticiyi Azure Cloud Shell 'i kullanarak tamamlayabilirsiniz veya ilgili komutları yerel olarak çalıştırabilirsiniz.  Azure Cloud Shell 'i hiç kullanmadıysanız, ilk kurulum 'u açmak için [Şimdi oturum](https://shell.azure.com) açmanız gerekir.

Bu komutları yerel olarak çalıştırmayı seçerseniz, CLı yüklemeniz gerekir.  Bu öğreticide, Azure CLı sürüm 2.0.71 veya sonraki bir sürümünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

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

Genel Internet 'e erişmek için NAT ağ geçidi için bir veya daha fazla genel IP adresi gerekir. **Myresourcegroupnat**Içinde **Mypublicipsource** adlı bir genel IP adresi kaynağı oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip) komutunu kullanın.

```azurecli-interactive
  az network public-ip create \
  --resource-group myResourceGroupNAT \
  --name myPublicIPsource \
  --sku standard
```

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

Bir veya daha fazla genel IP adresi kaynağı ya da bir veya daha fazla genel IP öneki veya her ikisi de NAT ağ geçidi ile kullanılabilir. Bu senaryoya göstermek için genel bir IP ön eki kaynağı ekleyeceğiz.   **Myresourcegroupnat**içinde **myPublicIPprefixsource** adlı bir genel IP öneki kaynağı oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip-prefix) öğesini kullanın.

```azurecli-interactive
  az network public-ip prefix create \
  --resource-group myResourceGroupNAT \
  --name myPublicIPprefixsource \
  --length 31
```

### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

Bu bölümde, NAT ağ geçidi kaynağını kullanarak NAT hizmetinin aşağıdaki bileşenlerini oluşturma ve yapılandırma işlemlerinin ayrıntıları verilmiştir:
  - NAT ağ geçidi kaynağı tarafından çevrilen giden akışlar için kullanılacak genel IP havuzu ve genel IP öneki.
  - Boşta kalma zaman aşımını varsayılan olarak 4 dakika ile 10 dakika arasında değiştirin.

Az Network NAT Gateway ile genel bir Azure NAT ağ geçidi oluşturun [az Network NAT Gateway Create](https://docs.microsoft.com/cli/azure/network/nat?view=azure-cli-latest) **Mynatgateway** ' i genel IP adresi **Mypublicipsource** ve genel IP öneki **myPublicIPprefixsource** ve boşta kalma zaman aşımını şu şekilde değiştirir **10 dakika**.

```azurecli-interactive
  az network nat gateway create \
    --resource-group myResourceGroupNAT \
    --name myNATgateway \
    --public-ip-addresses myPublicIPsource \
    --public-ip-prefixes myPublicIPprefixsource \
    --idle-timeout 10       
  ```

Bu noktada, NAT ağ geçidi çalışır ve yok, bir sanal ağın hangi alt ağlarının bunu kullanması gerektiğini yapılandırmaktır.

## <a name="prepare-the-source-for-outbound-traffic"></a>Kaynağı giden trafik için hazırlama

Bir tam test ortamının yapılandırması ve bir sonraki adımlarda testlerin yürütülmesi sırasında size rehberlik edeceğiz. Daha önce oluşturduğumuz NAT ağ geçidi kaynağını kullanacak kaynak ile başlayacağız.

### <a name="configure-virtual-network-for-source"></a>Kaynak için sanal ağı Yapılandır

Bir VM 'yi dağıtmadan ve NAT ağ geçidinizi test etmeden önce sanal ağı oluşturuyoruz.

[Az Network VNET Create](https://docs.microsoft.com/cli/azure/network/vnet)komutunu kullanarak **Myresourcegroupnat** içinde **mysubnetsource** adlı bir alt ağ ile **myvnetsource** adlı bir sanal ağ oluşturun.  Sanal ağın IP adresi alanı **192.168.0.0/16** ve sanal ağ içindeki alt ağ **192.168.0.0/24**' dir.

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupNAT \
    --location eastus2 \
    --name myVnetsource \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetsource \
    --subnet-prefix 192.168.0.0/24
```

### <a name="configure-nat-service-for-source-subnet"></a>Kaynak alt ağ için NAT hizmetini yapılandırma

Zaten NAT ağ geçidini oluşturmuş ve bundan sonra sanal ağ adlı **Mysubnetsource** alt ağını, [az Network VNET alt ağ güncelleştirmesiyle](https://docs.microsoft.com/cli/azure/network/vnet/subnet) **mynatsource** adlı belirli bir NAT ağ geçidi kaynağı **kullanacak şekilde** yapılandırdık.  Bu komut, belirtilen alt ağda NAT hizmetini etkinleştirir.

```azurecli-interactive
    az network vnet subnet update \
    --resource-group myResourceGroupNAT
    --vnet-name myVnetsource \
    --name mySubnetsource \
    --nat-gateway myNATgateway
```

Internet hedeflerine giden tüm trafik artık NAT hizmetini kullanıyor.  UDR 'yi yapılandırmak gerekli değildir.

NAT ağ geçidini test etmeden önce bir kaynak VM 'si oluşturuyoruz.  Bu sanal makineye dışarıdan erişmek için örnek düzeyi genel IP olarak genel bir IP adresi kaynağı atayacağız. Bu adres yalnızca test için erişim için kullanılır.  NAT hizmetinin diğer giden seçeneklere göre nasıl öncelikli olduğunu göstereceğiz.

Ayrıca, bu VM 'yi ortak IP olmadan oluşturabilir ve bir alıştırma olarak genel IP 'si olmadan bir sıçrama kutusu olarak kullanmak için başka bir VM oluşturabilirsiniz.

### <a name="create-public-ip-for-source-vm"></a>Kaynak VM için genel IP oluşturma

Kaynak VM 'ye erişmek için kullanılacak bir genel IP oluşturacağız. **Myresourcegroupnat**Içinde **Mypublicipsourcevm** adlı bir genel IP adresi kaynağı oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip) komutunu kullanın.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupNAT \
    --name myPublicIPsourceVM
    --sku standard
```

### <a name="create-an-nsg-for-source-vm"></a>Kaynak VM için bir NSG oluşturma

Standart genel IP adresleri ' varsayılan olarak güvenli ' olduğundan, SSH erişimi için gelen erişime izin vermek üzere bir NSG oluşturulması gerekir.  NAT hizmeti akış yönü duyarlı olduğu için, aynı alt ağda NAT ağ geçidi yapılandırıldıktan sonra bu NSG giden olarak kullanılmaz. **Myresourcegroupnat**Içinde **Mynsgsource** adlı bir NSG kaynağı oluşturmak için [az Network NSG Create](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create) kullanın.

```azurecli-interactive
  az network nsg create \
    --resource-group myResourceGroupNAT \
    --name myNSGsource 
```

### <a name="expose-ssh-endpoint-on-source-vm"></a>Kaynak VM 'de SSH uç noktasını kullanıma sunma

Kaynak VM 'ye SSH erişimi için NSG 'de bir kural oluşturacağız. **Myresourcegroupnat**Içinde **Mynsgsource** adlı NSG adında **SSH** adlı bir NSG kuralı oluşturmak için [az Network NSG Rule Create](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) kullanın.

```azurecli-interactive
  az network nsg rule create \
    --resource-group myResourceGroupNAT \
    --nsg-name myNSGsource \
    --priority 100 \
    --name ssh \
    --description "SSH access" \
    --access allow 
    --protocol tcp \
    --direction inbound \
    --destination-port-ranges 22
```

### <a name="create-nic-for-source-vm"></a>Kaynak VM için NIC oluşturma

[Az Network Nic Create](/cli/azure/network/nic#az-network-nic-create) komutuyla bir ağ arabirimi oluşturun ve genel IP adresi ve ağ güvenlik grubuyla ilişkilendirin. 

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupNAT \
    --name myNicsource \
    --vnet-name myVnetsource \
    --subnet mySubnetsource \
    --public-ip-address myPublicIPSourceVM
    --network-security-group myNSGsource
```

### <a name="create-a-source-vm"></a>Kaynak VM oluşturma

[Az VM Create](/cli/azure/vm#az-vm-create)ile sanal makineyi oluşturun.  Bu sanal makine için SSH anahtarları oluşturuyoruz ve özel anahtarı daha sonra kullanmak üzere depolarız.

```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupNAT \
    --name myVMsource \
    --nics myNicsource \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --no-wait
```

Komut hemen geri dönebilirken, VM 'nin dağıtılması birkaç dakika sürebilir.

## <a name="prepare-destination-for-outbound-traffic"></a>Giden trafik için hedefi hazırlama

Artık, NAT hizmeti tarafından test yapmanıza olanak tanımak için giden trafik için bir hedef oluşturacağız.

### <a name="configure-virtual-network-for-destination"></a>Sanal ağı hedef için yapılandırma

Bir VM 'yi ve hedef için dağıtmadan önce, hedef sanal makinenin yerleştirilebileceği bir sanal ağ da oluşturmanız gerekir.  Bunlar, hedef uç noktayı açığa çıkarmak için bazı küçük değişikliklerle kaynak VM ile aynı adımlardır.

[Az Network VNET Create](https://docs.microsoft.com/cli/azure/network/vnet)komutunu kullanarak **Myresourcegroupnat** içinde **mysubnetdestination** adlı bir alt ağ ile **myvnetdestination** adlı bir sanal ağ oluşturun.  Sanal ağın IP adresi alanı **192.168.0.0/16** ve sanal ağ içindeki alt ağ **192.168.0.0/24**' dir.

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupNAT \
    --location westus \
    --name myVnetdestination \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetdestination \
    --subnet-prefix 192.168.0.0/24
```

### <a name="create-public-ip-for-destination-vm"></a>Hedef VM için genel IP oluşturma

Kaynak VM 'ye erişmek için kullanılacak bir genel IP oluşturacağız. **Myresourcegroupnat**Içinde **Mypublicipdestinationvm** adlı bir genel IP adresi kaynağı oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip) komutunu kullanın. 

```azurecli-interactive
  az network public-ip create \
  --resource-group myResourceGroupNAT \
  --name myPublicIPdestinationVM
  --sku standard
  --location westus
```

### <a name="create-an-nsg-for-destination-vm"></a>Hedef VM için bir NSG oluşturma

Standart genel IP adresleri ' varsayılan olarak güvenli ' olduğundan, SSH erişimi için gelen erişime izin vermek üzere bir NSG oluşturulması gerekir.  NAT hizmeti akış yönü duyarlı olduğu için, aynı alt ağda NAT ağ geçidi yapılandırıldıktan sonra bu NSG giden olarak kullanılmaz. **Myresourcegroupnat**Içinde **Mynsgdestination** adlı bir NSG kaynağı oluşturmak için [az Network NSG Create](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create) kullanın.

```azurecli-interactive
    az network nsg create \
    --resource-group myResourceGroupNAT \
    --name myNSGdestination
    --location westus
```

### <a name="expose-ssh-endpoint-on-destination-vm"></a>Hedef VM 'de SSH uç noktasını kullanıma sunma

Hedef VM 'ye SSH erişimi için NSG 'de bir kural oluşturacağız. **Myresourcegroupnat**Içinde **Mynsgdestination** adlı NSG adında **SSH** adlı bir NSG kuralı oluşturmak için [az Network NSG Rule Create](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) kullanın.

```azurecli-interactive
    az network nsg rule create \
    --resource-group myResourceGroupNAT \
    --nsg-name myNSGdestination \
    --priority 100 \
    --name ssh \
    --description "SSH access" \
    --access allow 
    --protocol tcp \
    --direction inbound \
    --destination-port-ranges 22
```

### <a name="expose-http-endpoint-on-destination-vm"></a>Hedef VM 'de HTTP uç noktasını kullanıma sunma

Hedef VM 'ye HTTP erişimi için NSG 'de bir kural oluşturacağız. **Myresourcegroupnat**Içinde **Mynsgdestination** adlı NSG adında **http** adlı bir NSG kuralı oluşturmak için [az Network NSG Rule Create](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) kullanın.

```azurecli-interactive
    az network nsg rule create \
    --resource-group myResourceGroupNAT \
    --nsg-name myNSGdestination \
    --priority 101 \
    --name http \
    --description "HTTP access" \
    --access allow 
    --protocol tcp \
    --direction inbound \
    --destination-port-ranges 80
```

### <a name="create-nic-for-destination-vm"></a>Hedef VM için NIC oluşturma

[Az Network Nic Create](/cli/azure/network/nic#az-network-nic-create) komutunu kullanarak bir ağ arabirimi oluşturun ve **Mypublicipdestinationvm** genel IP adresiyle Ilişkilendirin ve **mynsgdestination**ağ güvenlik grubunu kullanın. 

```azurecli-interactive
    az network nic create \
    --resource-group myResourceGroupNAT \
    --name myNicdestination \
    --vnet-name myVnetdestination \
    --subnet mySubnetdestination \
    --public-ip-address myPublicIPdestinationVM
    --network-security-group myNSGdestination
    --location westus
```

### <a name="create-a-destination-vm"></a>Hedef VM oluşturma

[Az VM Create](/cli/azure/vm#az-vm-create)ile sanal makineyi oluşturun.  Bu sanal makine için SSH anahtarları oluşturuyoruz ve özel anahtarı daha sonra kullanmak üzere depolarız.

 ```azurecli-interactive
    az vm create \
    --resource-group myResourceGroupNAT \
    --name myVMdestination \
    --nics myNicdestination \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --no-wait
    --location westus
```
Komut hemen geri dönebilirken, VM 'nin dağıtılması birkaç dakika sürebilir.

## <a name="prepare-a-web-server-and-test-payload-on-destination-vm"></a>Hedef VM 'de bir Web sunucusunu ve test yükünü hazırlama

İlk olarak hedef sanal makinenin IP adresini bulduk.  Hedef VM 'nin genel IP adresini almak için [az Network public-IP Show](/cli/azure/network/public-ip#az-network-public-ip-show)kullanın. 

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupNAT \
    --name myPublicIPdestinationVM \
    --query [ipAddress] \
    --output tsv
``` 

>[!IMPORTANT]
>Genel IP adresini kopyalayın ve sonra sonraki adımlarda kullanabilmeniz için bir not defteri 'ne yapıştırın. Bu, hedef sanal makine olduğunu gösterir.

### <a name="sign-in-to-destination-vm"></a>Hedef VM 'de oturum açma

SSH kimlik bilgileri, önceki işlemden bulut kabuğunuzda depolanmalıdır.  Tarayıcınızda bir [Azure Cloud Shell](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın. 

```bash
ssh <ip-address-destination>
```

Oturum açtıktan sonra aşağıdaki komutları kopyalayıp yapıştırın.  

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get -y install nginx && \
sudo ln -sf /dev/null /var/log/nginx/access.log && \
sudo touch /var/www/html/index.html && \
sudo rm /var/www/html/index.nginx-debian.html && \
sudo dd if=/dev/zero of=/var/www/html/100k bs=1024 count=100
```

Bu komutlar, sanal makinenizi güncelleştirir, NGINX yükler ve NAT hizmetini kullanarak kaynak VM 'den almak için kullanabileceğiniz bir 100 Kbayt dosyası oluşturur.

Hedef VM ile SSH oturumunu kapatın.

## <a name="prepare-test-on-source-vm"></a>Kaynak VM 'de testi hazırlama

İlk olarak, kaynak VM 'nin IP adresini keşfetmemiz gerekir.  Kaynak VM 'nin genel IP adresini almak için [az Network public-IP Show](/cli/azure/network/public-ip#az-network-public-ip-show)kullanın. 

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupNAT \
    --name myPublicIPsourceVM \
    --query [ipAddress] \
    --output tsv
``` 

>[!IMPORTANT]
>Genel IP adresini kopyalayın ve sonra sonraki adımlarda kullanabilmeniz için bir not defteri 'ne yapıştırın. Bunun kaynak sanal makine olduğunu gösterir.

### <a name="log-into-source-vm"></a>Kaynak VM 'de oturum açma

Yine, SSH kimlik bilgileri Cloud Shell 'de depolanır. Tarayıcınızda [Azure Cloud Shell](https://shell.azure.com) için yeni bir sekme açın.  Sanal makineye SSH için önceki adımda alınan IP adresini kullanın. 

```bash
ssh <ip-address-source>
```

NAT hizmetinin test edilmesine hazırlanmak için aşağıdaki komutları kopyalayıp yapıştırın.

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get install -y nload golang && \
echo 'export GOPATH=${HOME}/go' >> .bashrc && \
echo 'export PATH=${PATH}:${GOPATH}/bin' >> .bashrc && \
. ~/.bashrc &&
go get -u github.com/rakyll/hey

```

Bu işlem, sanal makinenizi güncelleştirir, git ' i yükler, GitHub 'dan [Hey](https://github.com/rakyll/hey) yükler ve kabuk ortamınızı güncelleştirir.

Artık NAT hizmetini test etmeye hazırsınız.

## <a name="validate-nat-service"></a>NAT hizmetini doğrula

Kaynak VM 'de oturum açarken, hedef IP adresine istek oluşturmak için **kıvrımlı** ve **Hey** kullanabilirsiniz.

100 Kbayt dosyasını almak için kıvrımlı kullanın.  Aşağıdaki örnekteki **\<IP-adres-hedef >** , daha önce KOPYALADıĞıNıZ hedef IP adresiyle değiştirin.  **--Output** parametresi, alınan dosyanın atılacağını gösterir.

```bash
curl http://<ip-address-destination>/100k --output /dev/null
```

Ayrıca, **Hey**kullanarak bir dizi istek da oluşturabilirsiniz. Yeniden **\<IP-adres-hedef >** , daha önce KOPYALADıĞıNıZ hedef IP adresiyle değiştirin.

```bash
hey -n 100 -c 10 -t 30 --disable-keepalive http://<ip-address-destination>/100k
```

Bu işlem, 30 saniyelik bir zaman aşımı ile ve TCP bağlantısını yeniden kullanmadan 100 istek, 10 eşzamanlı bir işlem oluşturur.  Her istek 100 Kbayt alır.  Çalıştırmanın sonunda,, NAT hizmetinin ne kadar iyi gerçekleştirildiğiyle ilgili bazı istatistikleri **rapor eder.**

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve içinde yer alan tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive 
  az group delete --name myResourceGroupNAT
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir NAT ağ geçidi oluşturdunuz, kaynak ve hedef VM oluşturdunuz ve sonra NAT ağ geçidini test ediyor. Azure NAT hizmeti hakkında daha fazla bilgi edinmek için Azure NAT hizmeti için Diğer öğreticilere geçin.

Ayrıca, Azure Izleyici 'de ölçümleri inceleyerek, NAT hizmeti 'nin çalışma durumunu görebilir ve kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanılayabilirsiniz.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi eklenerek kolayca adreslenir.

> [!div class="nextstepaction"]

