---
title: 'Öğretici: Bir NAT ağ geçidi oluşturma ve test edin - Azure CLI'
titlesuffix: Azure Virtual Network NAT
description: Bu öğretici, Azure CLI'yi kullanarak bir NAT ağ geçidioluşturmayı ve NAT hizmetini nasıl test edilebilmektedir
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to test a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 02/18/2020
ms.author: allensu
ms.openlocfilehash: b3e10b3abbe5c9815e51ce67786882dbd294df3f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79202253"
---
# <a name="tutorial-create-a-nat-gateway-using-azure-cli-and-test-the-nat-service"></a>Öğretici: Azure CLI'yi kullanarak bir NAT ağ geçidi oluşturun ve NAT hizmetini test edin

Bu eğitimde, Azure'daki sanal makineler için giden bağlantı sağlamak için bir NAT ağ geçidi oluşturursunuz. NAT ağ geçidini sınamak için bir kaynak ve hedef sanal makine dağıtabilirsiniz. Genel bir IP adresine giden bağlantılar oluşturarak NAT ağ geçidini sınayacaksınız. Bu bağlantılar kaynaktan hedef sanal makineye gelecektir. Bu öğretici, yalnızca basitlik için aynı kaynak grubunda ki iki farklı sanal ağda kaynak ve hedef dağıtır.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu öğreticiyi Azure Cloud Shell'i kullanarak tamamlayabilir veya ilgili komutları yerel olarak çalıştırabilirsiniz.  Azure Bulut Su Şurası'nı kullanmadıysanız, [şimdi oturum açmanız](https://shell.azure.com)gerekir.

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

Genel Internet'e erişmek için NAT ağ geçidi için bir veya daha fazla genel IP adresine ihtiyacınız vardır. **myResourceGroupNAT'ta** **myPublicIPsource** adlı genel bir IP adresi kaynağı oluşturmak için [az network public-ip oluşturmayı](https://docs.microsoft.com/cli/azure/network/public-ip) kullanın.

```azurecli-interactive
  az network public-ip create \
  --resource-group myResourceGroupNAT \
  --name myPublicIPsource \
  --sku standard
```

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

Bir veya daha fazla genel IP adresi kaynağı, genel IP önekleri veya her ikisini de NAT ağ geçidiile kullanabilirsiniz. Bu senaryoya göstermek için genel bir IP öneki kaynağı ekleriz.   **myResourceGroupNAT'ta** **myPublicIPprefixsource** adlı genel bir IP öneki kaynağı oluşturmak için [az network public-ip öneki oluşturmayı](/cli/azure/network/public-ip/prefix?view=azure-cli-latest#az-network-public-ip-prefix-create) kullanın.

```azurecli-interactive
  az network public-ip prefix create \
  --resource-group myResourceGroupNAT \
  --name myPublicIPprefixsource \
  --length 31
```

### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

Bu bölümde, NAT ağ geçidi kaynağını kullanarak NAT hizmetinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz ayrıntılı olarak açıklanmaktadır:
  - NAT ağ geçidi kaynağı tarafından çevrilen giden akışlar için kullanılacak genel BIR IP havuzu ve genel IP öneki.
  - Boşta kalma süresini varsayılan 4 dakikadan 10 dakikaya değiştirin.

**myNATağ geçidi**adlı [az ağ nat ağ geçidi oluşturarak](https://docs.microsoft.com/cli/azure/network/nat?view=azure-cli-latest) küresel bir Azure NAT ağ geçidi oluşturun. Komut hem genel IP adresi **myPublicIP** ve ortak IP öneki **myPublicIPprefix**kullanır. Komut ayrıca boşta kalma süresini 10 dakikaya değiştirir.

```azurecli-interactive
  az network nat gateway create \
    --resource-group myResourceGroupNAT \
    --name myNATgateway \
    --public-ip-addresses myPublicIPsource \
    --public-ip-prefixes myPublicIPprefixsource \
    --idle-timeout 10       
  ```

Bu noktada, NAT ağ geçidi işlevseldir ve eksik olan tek şey, sanal bir ağın hangi alt ağlarını kullanması gerektiğini yapılandırmaktır.

## <a name="prepare-the-source-for-outbound-traffic"></a>Kaynağı giden trafik için hazırlayın

Tam bir test ortamı nın kurulumu nda size rehberlik edeceğiz. NAT ağ geçidini doğrulamak için açık kaynak araçlarını kullanarak bir test ayarlarsınız. Daha önce oluşturduğumuz NAT ağ geçidini kullanacak kaynakla başlayacağız.

### <a name="configure-virtual-network-for-source"></a>Kaynak için sanal ağı yapılandırma

Bir VM dağıtmadan ve NAT ağ geçidinizi test etmeden önce sanal ağı oluşturmamız gerekir.

az ağ Microsoft Azure Sanal Ağ oluşturmak kullanarak **myResourceGroupNAT** **mySubnetsource** adlı bir alt ağ ile **myVnetsource** adlı bir sanal [ağ oluşturun.](https://docs.microsoft.com/cli/azure/network/vnet)  Sanal ağ için IP adresi alanı **192.168.0.0/16'dır.** Sanal ağ içindeki alt ağ **192.168.0.0/24'dür.**

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupNAT \
    --location eastus2 \
    --name myVnetsource \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetsource \
    --subnet-prefix 192.168.0.0/24
```

### <a name="configure-nat-service-for-source-subnet"></a>Kaynak alt ağı için NAT hizmetini yapılandırma

Az ağ [Microsoft Azure Virtual Network alt ağ güncelleştirmesi](https://docs.microsoft.com/cli/azure/network/vnet/subnet)ile belirli bir NAT ağ geçidi kaynak **myNATgateway** kullanmak için sanal ağ **myVnetsource** kaynak **mySubnetsource** kaynak yapılandırın. Bu komut, belirtilen alt ağdaki NAT hizmetini etkinleştirir.

```azurecli-interactive
    az network vnet subnet update \
    --resource-group myResourceGroupNAT \
    --vnet-name myVnetsource \
    --name mySubnetsource \
    --nat-gateway myNATgateway
```

İnternet hedeflerine giden tüm giden trafik artık NAT hizmetini kullanıyor.  Bir UDR yapılandırmak için gerekli değildir.

NAT ağ geçidini test etmeden önce bir kaynak VM oluşturmamız gerekiyor.  Bu VM'ye dışarıdan erişmek için örnek düzeyinde bir genel IP kaynağı olarak ortak bir IP adresi kaynağı atayacağız. Bu adres yalnızca test için erişmek için kullanılır.  NAT hizmetinin diğer giden seçeneklerden nasıl öncelikli olduğunu göstereceğiz.

Ayrıca, bu VM'yi genel bir IP olmadan oluşturabilir ve bir egzersiz olarak genel IP'si olmadan atlama kutusu olarak kullanmak üzere başka bir VM oluşturabilirsiniz.

### <a name="create-public-ip-for-source-vm"></a>Kaynak VM için ortak IP oluşturma

Kaynak VM'ye erişmek için kullanılacak genel bir IP oluştururuz. **myResourceGroupNAT'ta** **myPublicIPsourceVM** adlı genel bir IP adresi kaynağı oluşturmak için [az ağ public-ip oluşturmayı](https://docs.microsoft.com/cli/azure/network/public-ip) kullanın.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupNAT \
    --name myPublicIPsourceVM \
    --sku standard
```

### <a name="create-an-nsg-for-source-vm"></a>Kaynak VM için NSG oluşturma

Standart genel IP adresleri 'varsayılan olarak güvenli' olduğundan, ssh erişimine izin vermek için bir NSG oluşturmamız gerekir.  Azure NAT hizmeti akış yönü farkındadır. NAT ağ geçidi aynı alt ağ üzerinde yapılandırıldığında bu NSG giden için kullanılmaz. **myResourceGroupNAT'ta** **myNSGsource** adlı bir NSG kaynağı oluşturmak için [az ağ nsg oluşturun.](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create)

```azurecli-interactive
  az network nsg create \
    --resource-group myResourceGroupNAT \
    --name myNSGsource 
```

### <a name="expose-ssh-endpoint-on-source-vm"></a>Kaynak VM'de SSH uç noktasını açığa çıkarma

NSG'de SSH'nin kaynak vm'ye erişimi için bir kural oluşturuyoruz. **ssh**adlı bir NSG kuralı oluşturmak için [az ağ nsg kuralı nı](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) kullanın. Bu kural kaynak grubunda **myNSGsource adlı NSG** oluşturulacaktır **myResourceGroupNAT**.

```azurecli-interactive
  az network nsg rule create \
    --resource-group myResourceGroupNAT \
    --nsg-name myNSGsource \
    --priority 100 \
    --name ssh \
    --description "SSH access" \
    --access allow \
    --protocol tcp \
    --direction inbound \
    --destination-port-ranges 22
```

### <a name="create-nic-for-source-vm"></a>Kaynak VM için NIC oluşturma

[Az network nic oluşturmak](/cli/azure/network/nic#az-network-nic-create) ve ortak IP adresi ve ağ güvenlik grubu ile ilişkilendirmek ile bir ağ arabirimi oluşturun. 

```azurecli-interactive
  az network nic create \
    --resource-group myResourceGroupNAT \
    --name myNicsource \
    --vnet-name myVnetsource \
    --subnet mySubnetsource \
    --public-ip-address myPublicIPSourceVM \
    --network-security-group myNSGsource
```

### <a name="create-a-source-vm"></a>Kaynak VM oluşturma

[Az vm oluşturmak](/cli/azure/vm#az-vm-create)ile sanal makine oluşturun.  Bu VM için ssh tuşları üretiyoruz ve daha sonra kullanmak üzere özel anahtarı saklıyoruz.

```azurecli-interactive
  az vm create \
    --resource-group myResourceGroupNAT \
    --name myVMsource \
    --nics myNicsource \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --no-wait
```

Komut hemen geri dönecek olsa da, VM'nin devreye salması birkaç dakika sürebilir.

## <a name="prepare-destination-for-outbound-traffic"></a>Giden trafik için hedef hazırlama

Şimdi, NAT hizmeti tarafından çevrilen giden trafik için bir hedef oluşturarak bunu test edebilirsiniz.

### <a name="configure-virtual-network-for-destination"></a>Hedef için sanal ağı yapılandırma

 Hedef sanal makinenin olacağı bir sanal ağ oluşturmamız gerekiyor.  Bu komutlar, hedef bitiş noktasını ortaya çıkarmak için küçük değişikliklerle kaynak VM ile aynı adımlardır.

az ağ Microsoft Azure Sanal Ağ oluşturma kullanarak **myResourceGroupNAT** **mySubnetdestination** adlı bir alt ağ ile **myVnetdestination** adlı bir sanal [ağ oluşturun.](https://docs.microsoft.com/cli/azure/network/vnet)  Sanal ağ için IP adresi alanı **192.168.0.0/16'dır.** Sanal ağ içindeki alt ağ **192.168.0.0/24'dür.**

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

Kaynak VM'ye erişmek için kullanılacak genel bir IP oluştururuz. **myResourceGroupNAT'ta** **myPublicIPdestinationVM** adlı genel bir IP adresi kaynağı oluşturmak için [az ağ public-ip oluşturmayı](https://docs.microsoft.com/cli/azure/network/public-ip) kullanın. 

```azurecli-interactive
  az network public-ip create \
  --resource-group myResourceGroupNAT \
  --name myPublicIPdestinationVM \
  --sku standard \
  --location westus
```

### <a name="create-an-nsg-for-destination-vm"></a>Hedef VM için bir NSG oluşturma

Standart Genel IP adresleri 'varsayılan olarak güvenli'dir, ssh için gelen erişime izin vermek için bir NSG oluşturmanız gerekir. Azure NAT hizmeti akış yönü farkındadır. NAT ağ geçidi aynı alt ağ üzerinde yapılandırıldığında bu NSG giden için kullanılmaz. **MyResourceGroupNAT'ta** **myNSGdestination** adlı bir NSG kaynağı oluşturmak için [az ağ nsg oluşturmayı](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create) kullanın.

```azurecli-interactive
    az network nsg create \
    --resource-group myResourceGroupNAT \
    --name myNSGdestination \
    --location westus
```

### <a name="expose-ssh-endpoint-on-destination-vm"></a>Hedef VM'de SSH uç noktasını açığa çıkarma

NSG'de Hedef vm'ye SSH erişimi için bir kural oluşturuyoruz. **ssh**adlı bir NSG kuralı oluşturmak için [az ağ nsg kuralı nı](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create) kullanın. Bu kural, **myResourceGroupNAT**kaynak grubunda **myNSGdestination adlı NSG** oluşturulur.

```azurecli-interactive
    az network nsg rule create \
    --resource-group myResourceGroupNAT \
    --nsg-name myNSGdestination \
    --priority 100 \
    --name ssh \
    --description "SSH access" \
    --access allow \
    --protocol tcp \
    --direction inbound \
    --destination-port-ranges 22
```

### <a name="expose-http-endpoint-on-destination-vm"></a>Http uç noktasını hedef VM'de pozlama

NSG'de http erişimi için hedef vm'ye bir kural oluşturuyoruz. **MyResourceGroupNAT'ta** **myNSGdestination** adlı NSG'de **http** adlı bir NSG kuralı oluşturmak için [az ağ nsg kuralını kullanın.](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create)

```azurecli-interactive
    az network nsg rule create \
    --resource-group myResourceGroupNAT \
    --nsg-name myNSGdestination \
    --priority 101 \
    --name http \
    --description "HTTP access" \
    --access allow \
    --protocol tcp \
    --direction inbound \
    --destination-port-ranges 80
```

### <a name="create-nic-for-destination-vm"></a>Hedef VM için NIC oluşturma

[Az network nic oluşturmak](/cli/azure/network/nic#az-network-nic-create) ve ortak IP adresi **myPublicIPdestinationVM** ve ağ güvenlik grubu **myNSGdestination**ile ilişkilendirmek ile bir ağ arabirimi oluşturun. 

```azurecli-interactive
    az network nic create \
    --resource-group myResourceGroupNAT \
    --name myNicdestination \
    --vnet-name myVnetdestination \
    --subnet mySubnetdestination \
    --public-ip-address myPublicIPdestinationVM \
    --network-security-group myNSGdestination \
    --location westus
```

### <a name="create-a-destination-vm"></a>Hedef VM oluşturma

[Az vm oluşturmak](/cli/azure/vm#az-vm-create)ile sanal makine oluşturun.  Bu VM için ssh tuşları üretiyoruz ve daha sonra kullanmak üzere özel anahtarı saklıyoruz.

 ```azurecli-interactive
    az vm create \
    --resource-group myResourceGroupNAT \
    --name myVMdestination \
    --nics myNicdestination \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --no-wait \
    --location westus
```
Komut hemen geri dönecek olsa da, VM'nin devreye salması birkaç dakika sürebilir.

## <a name="prepare-a-web-server-and-test-payload-on-destination-vm"></a>Hedef VM'de bir web sunucusu hazırlayın ve yükü test edin

Öncelikle hedef VM IP adresini keşfetmek gerekir.  Hedef VM'nin genel IP adresini almak için [az network public-ip show'u](/cli/azure/network/public-ip#az-network-public-ip-show)kullanın. 

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupNAT \
    --name myPublicIPdestinationVM \
    --query [ipAddress] \
    --output tsv
``` 

>[!IMPORTANT]
>Ortak IP adresini kopyalayın ve sonraki adımlarda kullanabilmeniz için not defterine yapıştırın. Bunun hedef sanal makine olduğunu belirtin.

### <a name="sign-in-to-destination-vm"></a>Hedef VM'de oturum açın

SSH kimlik bilgileri, önceki işlemdeki Bulut Kabuğunuzda depolanmalıdır.  Tarayıcınızda bir [Azure Bulut Kabuğu](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın. 

```bash
ssh <ip-address-destination>
```

Oturum aştıktan sonra aşağıdaki komutları kopyalayıp yapıştırın.  

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

Bu komutlar sanal makinenizi günceller, nginx yükler ve 100 KBytes dosyası oluşturur. Bu dosya NAT hizmeti kullanılarak kaynak VM'den alınacaktır.

Hedef VM ile SSH oturumunu kapatın.

## <a name="prepare-test-on-source-vm"></a>Kaynak VM üzerinde test hazırlama

Öncelikle kaynak VM IP adresini keşfetmek gerekir.  Kaynak VM'nin genel IP adresini almak için [az network public-ip show'u](/cli/azure/network/public-ip#az-network-public-ip-show)kullanın. 

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupNAT \
    --name myPublicIPsourceVM \
    --query [ipAddress] \
    --output tsv
``` 

>[!IMPORTANT]
>Ortak IP adresini kopyalayın ve sonraki adımlarda kullanabilmeniz için not defterine yapıştırın. Bunun kaynak sanal makine olduğunu belirtin.

### <a name="sign-in-to-source-vm"></a>Kaynak VM'de oturum açma

Yine, SSH kimlik bilgileri Bulut Kabuğu'nda depolanır. Tarayıcınızda Azure [Bulut Su şutu](https://shell.azure.com) için yeni bir sekme açın.  Sanal makineye SSH için önceki adımda alınan IP adresini kullanın. 

```bash
ssh <ip-address-source>
```

NAT hizmetini sınamaya hazırlanmak için aşağıdaki komutları kopyalayın ve yapıştırın.

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

Bu komut sanal makinenizi günceller, git'i yükler, GitHub'dan [yüklenir](https://github.com/rakyll/hey) ve kabuk ortamınızı günceller.

Artık NAT hizmetini test etmeye hazırsınız.

## <a name="validate-nat-service"></a>NAT hizmetini doğrulama

Kaynak VM'de oturum açtığınızda, hedef IP adresine istek oluşturmak için **curl** ve **hey'i** kullanabilirsiniz.

100-KBytes dosyasını almak için kıvırma kullanın.  Aşağıdaki örnekte ** \<ip adresi hedef>** daha önce kopyalamış olduğunuz hedef IP adresiyle değiştirin.  **--çıkış** parametresi, alınan dosyanın atılacağını gösterir.

```bash
curl http://<ip-address-destination>/100k --output /dev/null
```

Ayrıca **hey**kullanarak istekleri bir dizi oluşturabilirsiniz. Yine, ** \<ip adresi hedef>** daha önce kopyalamış olduğunuz hedef IP adresiyle değiştirin.

```bash
hey -n 100 -c 10 -t 30 --disable-keepalive http://<ip-address-destination>/100k
```

Bu komut, 30 saniyelik bir zaman aşımıyla 10 aynı anda 100 istek oluşturur. TCP bağlantısı yeniden kullanılmaz.  Her istek 100 Kbyte alır.  Çalışma sonunda, **hey** NAT hizmeti ne kadar iyi yaptığı hakkında bazı istatistikleri rapor edecektir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az grubu silme](/cli/azure/group#az-group-delete) komutunu kullanabilirsiniz.

```azurecli-interactive 
  az group delete --name myResourceGroupNAT
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir NAT ağ geçidi oluşturdunuz, bir kaynak ve hedef VM oluşturdunuz ve sonra NAT ağ geçidini test ettiniz.

NAT hizmetinizin çalıştığını görmek için Azure Monitor'da ölçümleri inceleyin. Kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanı.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi de eklenerek kolayca giderilir.

- Sanal [Ağ NAT](./nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [Azure CLI'yi kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-cli.md)dağıtmak için hızlı başlatın.
- [Azure PowerShell'i kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-powershell.md)dağıtmak için hızlı başlatın.
- [Azure portalını kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-portal.md)dağıtmak için hızlı başlatın.

> [!div class="nextstepaction"]

