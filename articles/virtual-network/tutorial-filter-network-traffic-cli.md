---
title: Filtre ağ trafiği - Azure CLI | Microsoft Dokümanlar
description: Bu makalede, Azure CLI'yi kullanarak ağ trafiğini ağ güvenlik grubu yla bir alt ağa nasıl filtreleyiş olarak öğreneceksiniz.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to filter network traffic to virtual machines that perform similar functions, such as web servers.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/30/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 72c8b4d57b5064af34665cff1386179e62324938
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235069"
---
# <a name="filter-network-traffic-with-a-network-security-group-using-the-azure-cli"></a>Azure CLI'yi kullanarak ağ güvenlik grubuyla ağ trafiğini filtreleme

Bir sanal ağ alt ağına gelen ve sanal ağ alt ağından giden ağ trafiğini, bir ağ güvenlik grubu ile filtreleyebilirsiniz. Ağ güvenlik grupları, ağ trafiğini IP adresi, bağlantı noktası ve protokole göre filtreleyen güvenlik kuralları içerir. Güvenlik kuralları bir alt ağda dağıtılmış kaynaklara uygulanır. Bu makalede şunları öğreneceksiniz:

* Ağ güvenlik grubu ve güvenlik kuralları oluşturma
* Bir sanal ağ oluşturma ve ağ güvenlik grubunu alt ağ ile ilişkilendirme
* Sanal makineleri (VM) bir alt ağa dağıtma
* Trafik filtrelerini test etme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI sürümü 2.0.28 veya sonraki sürümlerini çalıştırdığınız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). 


## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Bir ağ güvenlik grubu, güvenlik kuralları içerir. Güvenlik kuralları, bir kaynak ve hedefi belirtir. Kaynaklar ve hedefler, uygulama güvenlik grupları olabilir.

### <a name="create-application-security-groups"></a>Uygulama güvenlik grupları oluşturma

Önce az grubu oluşturmak ile bu makalede oluşturulan tüm kaynaklar için bir kaynak grubu [oluşturun.](/cli/azure/group) Aşağıdaki örnekte *eastus* konumunda bir kaynak grubu oluşturulmaktadır: 

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

[az ağ asg oluşturmak](/cli/azure/network/asg)ile bir uygulama güvenlik grubu oluşturun. Uygulama güvenlik grubu, benzer bağlantı noktası filtreleme gereksinimlerine sahip sunucuları gruplandırmanızı sağlar. Aşağıdaki örnek iki uygulama güvenlik grubu oluşturur.

```azurecli-interactive
az network asg create \
  --resource-group myResourceGroup \
  --name myAsgWebServers \
  --location eastus

az network asg create \
  --resource-group myResourceGroup \
  --name myAsgMgmtServers \
  --location eastus
```

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

[az ağ nsg oluşturmak](/cli/azure/network/nsg)ile bir ağ güvenlik grubu oluşturun. Aşağıdaki örnek *myNsg* adlı bir ağ güvenlik grubu oluşturur: 

```azurecli-interactive 
# Create a network security group
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsg
```

### <a name="create-security-rules"></a>Güvenlik kuralları oluşturma

[az ağ nsg kuralı oluşturmak](/cli/azure/network/nsg/rule)ile bir güvenlik kuralı oluşturun. Aşağıdaki örnek, internetten gelen trafiğin 80 ve 443 numaralı bağlantı noktaları üzerinden *myWebServers* uygulama güvenlik grubuna gitmesine izin veren bir kural oluşturur:

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsg \
  --name Allow-Web-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-asgs "myAsgWebServers" \
  --destination-port-range 80 443
```

Aşağıdaki örnek, internetten gelen trafiğin bağlantı noktası 22 üzerinden *myMgmtServers* uygulama güvenlik grubuna bağlanmasına izin veren bir kural oluşturur:

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsg \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 110 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-asgs "myAsgMgmtServers" \
  --destination-port-range 22
```

Bu makalede, SSH (bağlantı noktası 22) *myAsgMgmtServers* VM için internete maruz kalmaktadır. Üretim ortamları için, bağlantı noktası 22'yi internete maruz bırakmak yerine, [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [özel](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ağ bağlantısı kullanarak yönetmek istediğiniz Azure kaynaklarına bağlanmanız önerilir.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[az network vnet create](/cli/azure/network/vnet) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek *myVirtualNetwork* adlı bir sanal ağ oluşturur:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16
```

[Az ağ vnet subnet oluşturmak](/cli/azure/network/vnet/subnet)ile sanal ağa bir alt ağ ekleyin. Aşağıdaki örnek, sanal ağa *mySubnet* adlı bir alt ağ ekler ve *myNsg* ağ güvenlik grubunu onunla ilişkilendirir:

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name mySubnet \
  --address-prefix 10.0.0.0/24 \
  --network-security-group myNsg
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Daha sonraki bir adımda trafik filtrelemesini doğrulayabilmek için sanal ağda iki VM oluşturun. 

[az vm create](/cli/azure/vm) ile bir VM oluşturun. Aşağıdaki örnek, web sunucusu olarak görev yapacak bir VM oluşturur. Bu `--asgs myAsgWebServers` seçenek, Azure'un VM için oluşturduğu ağ arabirimini *myAsgWebServers* uygulama güvenlik grubunun bir üyesi haline getirmesine neden olur.

Bu `--nsg ""` seçenek, Azure'un VM'yi oluştururken oluşturduğu ağ arabirimi için varsayılan ağ güvenlik grubu oluşturmasını önlemek için belirtilir. Bu makaleyi kolaylaştırmak için bir parola kullanılır. Anahtarlar genellikle üretim dağıtımlarında kullanılır. Anahtarları kullanıyorsanız, kalan adımlar için SSH aracısını da yapılandırmanız gerekir. Daha fazla bilgi için SSH istemcinizin belgelerine bakın. Aşağıdaki `<replace-with-your-password>` komutu seçtiğiniz bir parolayla değiştirin.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmWeb \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet mySubnet \
  --nsg "" \
  --asgs myAsgWebServers \
  --admin-username azureuser \
  --admin-password $adminPassword
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra, aşağıdaki örneğe benzer çıktı döndürülür: 

```output
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmWeb",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

**publicIpAddress** değerini not alın. Bu adres, daha sonraki bir adımda VM'ye internetten erişmek için kullanılır.  Yönetim sunucusu olarak görev yapacak bir VM oluşturun:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmMgmt \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet mySubnet \
  --nsg "" \
  --asgs myAsgMgmtServers \
  --admin-username azureuser \
  --admin-password $adminPassword
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra, döndürülen çıktıdaki **publicIpAddress'e** dikkat edin. Bu adres, bir sonraki adımda VM'ye erişmek için kullanılır. Azure VM oluşturma işlemini tamamlayana kadar sonraki adıma geçmeyin.

## <a name="test-traffic-filters"></a>Trafik filtrelerini test etme

*myVmMgmt* VM ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. * \<PublicIpAddress>'yi* VM'nizin genel IP adresiyle değiştirin. Yukarıdaki örnekte, IP adresi *13.90.242.231'dir.*

```bash 
ssh azureuser@<publicIpAddress>
```

Parola istendiğinde, [VM oluştur'a](#create-virtual-machines)girdiğiniz parolayı girin.

Bağlantı başarılı olur, çünkü bağlantı 22 internetten *myVmMgmt* VM'ye bağlı ağ arabiriminin içinde olduğu *myAsgMgmtServers* uygulama güvenlik grubuna giriş izni verilir.

*myVmMgmt* VM'den *myVmWeb* VM'ye SSH için aşağıdaki komutu kullanın:

```bash 
ssh azureuser@myVmWeb
```

Her bir ağ güvenlik grubu içindeki varsayılan güvenlik kuralı bir sanal ağ içindeki tüm IP adresleri arasında tüm bağlantı noktaları üzerinden trafiğe izin verdiği için bağlantı başarılı olur. *MyAsgWebServers* için güvenlik kuralı bağlantı noktası 22 Internet'ten gelen izin vermez, çünkü Internet'ten *myVmWeb* VM SSH olamaz.

*myVmWeb* VM nginx web sunucusu yüklemek için aşağıdaki komutları kullanın:

```bash 
# Update package source
sudo apt-get -y update

# Install NGINX
sudo apt-get -y install nginx
```

Varsayılan güvenlik kuralı Internet'e giden tüm trafiğe izin verdiğinden, *myVmWeb* VM'nin nginx'i almak için Internet'e giden bir şekilde çıkmasına izin verilir. *MyVmWeb* SSH oturumundan çıkın, bu `username@myVmMgmt:~$` da sizi *myVmMgmt VM'nin* istemanında bırakır. *myVmWeb* VM'den nginx karşılama ekranını almak için aşağıdaki komutu girin:

```bash
curl myVmWeb
```

*myVmMgmt* VM'nin oturumu. *MyVmWeb* web sunucusuna Azure dışından erişebileceğinizi doğrulamak `curl <publicIpAddress>` için kendi bilgisayarınızdan girin. Bağlantı başarılı olur, çünkü bağlantı noktası 80 *myVmWeb* VM bağlı ağ arabirimi *myAsgWebServers* uygulama güvenlik grubuna Internet'ten gelen izin verilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az grubu silme'yi](/cli/azure/group) kullanın.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ağ güvenlik grubu oluşturdunuz ve sanal ağ alt ağıyla ilişkilendirdin. Ağ güvenlik grupları hakkında daha fazla bilgi edinmek bkz. [Ağ güvenlik grubuna genel bakış](security-overview.md) ve [Ağ güvenlik grubunu yönetme](manage-network-security-group.md).

Azure, varsayılan olarak trafiği alt ağlar arasında yönlendirir. Bunun yerine, alt ağlar arasındaki trafiği, örneğin, güvenlik duvarı olarak görev yapan bir VM aracılığıyla yönlendirmeyi seçebilirsiniz. Nasıl yapılacağını öğrenmek için [bkz.](tutorial-create-route-table-cli.md)
