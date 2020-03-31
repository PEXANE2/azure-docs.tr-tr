---
title: Rota ağ trafiği - Azure CLI | Microsoft Dokümanlar
description: Bu makalede, Azure CLI'yi kullanarak ağ trafiğini bir rota tablosuyla nasıl yönlendireceklerini öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 5fa94b93e081ab6334c39b848068f50682f5f1f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235062"
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Azure CLI'yi kullanarak ağ trafiğini bir rota tablosuyla yönlendirme

Azure varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında gerçekleşen trafiği otomatik olarak yönlendirir. Azure’ın varsayılan yönlendirmesini geçersiz kılmak için kendi yönlendirmelerinizi oluşturabilirsiniz. Örneğin, bir ağ sanal gereci üzerinden alt ağlar arasındaki trafiği yönlendirmek isteyebilirsiniz. Bu makalede şunları öğreneceksiniz:

* Yönlendirme tablosu oluşturma
* Yönlendirme oluşturma
* Birden fazla alt ağa sahip bir sanal ağ oluşturma
* Yönlendirme tablosunu bir alt ağ ile ilişkilendirme
* Trafiği yönlendiren bir NVA oluşturma
* Sanal makineleri (VM) farklı alt ağlara dağıtma
* NVA aracılığıyla trafiği bir alt ağdan başka birine yönlendirme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.28 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). 

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Bir rota tablosu oluşturmadan önce, bu makalede oluşturulan tüm kaynaklar için [az grubu oluşturduğu](/cli/azure/group) bir kaynak grubu oluşturun. 

```azurecli-interactive
# Create a resource group.
az group create \
  --name myResourceGroup \
  --location eastus
```

az ağ rota tablosu oluşturmak ile bir rota [tablosu oluşturun.](/cli/azure/network/route-table#az-network-route-table-create) Aşağıdaki örnek, *myRouteTablePublic*adlı bir rota tablosu oluşturur. 

```azurecli-interactive
# Create a route table
az network route-table create \
  --resource-group myResourceGroup \
  --name myRouteTablePublic
```

## <a name="create-a-route"></a>Yönlendirme oluşturma

Az ağ rota tablosu rotası oluşturmak ile rota tablosunda bir rota [oluşturun.](/cli/azure/network/route-table/route#az-network-route-table-route-create) 

```azurecli-interactive
az network route-table route create \
  --name ToPrivateSubnet \
  --resource-group myResourceGroup \
  --route-table-name myRouteTablePublic \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.2.4
```

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Bir rota tablosunu bir alt ağla ilişkilendirebilmek için sanal bir ağ ve alt ağ oluşturmanız gerekir. [Az ağ vnet oluşturmak](/cli/azure/network/vnet)ile bir alt net ile sanal ağ oluşturun.

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

[az ağ vnet subnet oluşturmak](/cli/azure/network/vnet/subnet)ile iki ek alt ağ oluşturun.

```azurecli-interactive
# Create a private subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24

# Create a DMZ subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name DMZ \
  --address-prefix 10.0.2.0/24
```

*MyRouteTablePublic* rota tablosunu *az* network [vnet subnet güncellemesi](/cli/azure/network/vnet/subnet)ile Genel alt netle ilişkilendirin.

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

## <a name="create-an-nva"></a>NVA oluşturma

NVA; yönlendirme, güvenlik duvarı oluşturma veya WAN iyileştirmesi gibi ağ işlevlerini gerçekleştiren bir VM'dir.

[Az vm oluşturmak](/cli/azure/vm)ile *DMZ* alt net bir NVA oluşturun. Bir VM oluşturduğunuzda, Azure varsayılan olarak VM'ye genel bir IP adresi oluşturur ve atar. Parametre, `--public-ip-address ""` VM'nin internetten bağlanması gerekmediği için Azure'a vm'ye genel bir IP adresi oluşturmamasını ve atamasını bildirir. SSH anahtarları, varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmNva \
  --image UbuntuLTS \
  --public-ip-address "" \
  --subnet DMZ \
  --vnet-name myVirtualNetwork \
  --generate-ssh-keys
```

Sanal makinenin oluşturulması birkaç dakika sürer. Azure VM'yi oluşturmayı bitirip VM ile ilgili çıktıyı döndürene kadar bir sonraki adıma devam etmeyin. 

Bir ağ arabiriminin kendi IP adresini hedeflemeden kendisine gönderilen ağ trafiğini iletebilmesi için, ağ arabiriminde IP iletme özelliğinin etkinleştirilmiş olması gerekir. [Az network nic güncellemesi](/cli/azure/network/nic)ile ağ arabirimi için IP iletmesini etkinleştirin.

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

VM içindeki işletim sistemi veya VM içinde çalışan bir uygulama da ağ trafiğini iletebilmelidir. [Az vm uzatma seti](/cli/azure/vm/extension)ile VM işletim sistemi içinde IP iletme etkinleştirin:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVmNva \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
```

Komutun yürütülmesi bir dakika kadar sürebilir.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Daha sonraki bir adımda *NVA* üzerinden Ortak alt ağdan gelen trafiğin *Özel* alt ağına yönlendirildiğini doğrulamak için sanal ağda iki VM oluşturun. 

Az vm oluşturmak ile *Ortak* alt net bir [VM oluşturun.](/cli/azure/vm) Parametre, Azure'un `--no-wait` arka planda komutu yürütmesini sağlar, böylece bir sonraki komuta devam edebilirsiniz. Bu makaleyi kolaylaştırmak için bir parola kullanılır. Anahtarlar genellikle üretim dağıtımlarında kullanılır. Anahtarları kullanıyorsanız, SSH aracıyönlendirmeyi de yapılandırmanız gerekir. Daha fazla bilgi için SSH istemcinizin belgelerine bakın. Aşağıdaki `<replace-with-your-password>` komutu seçtiğiniz bir parolayla değiştirin.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmPublic \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

*Özel* alt ağda bir VM oluşturun.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --admin-username azureuser \
  --admin-password $adminPassword
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra, Azure CLI aşağıdaki örneğe benzer bilgileri gösterir: 

```output
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmPrivate",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

**publicIpAddress** değerini not alın. Bu adres, daha sonraki bir adımda VM'ye internetten erişmek için kullanılır.

## <a name="route-traffic-through-an-nva"></a>Trafiği NVA üzerinden yönlendirme

*myVmPrivate* VM ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. * \<PublicIpAddress>'yi* VM'nizin genel IP adresiyle değiştirin. Yukarıdaki örnekte, IP adresi *13.90.242.231'dir.*

```bash
ssh azureuser@<publicIpAddress>
```

Parola istendiğinde, seçtiğiniz parolayı sanal [makineler oluştur'a](#create-virtual-machines)girin.

*myVmPrivate* VM'ye izleme rotası yüklemek için aşağıdaki komutu kullanın:

```bash
sudo apt-get install traceroute
```

*MyVmPrivate* VM'den *myVmPublic* VM'ye ağ trafiği için yönlendirmeyi test etmek için aşağıdaki komutu kullanın.

```bash
traceroute myVmPublic
```

Yanıt aşağıdaki örneğe benzer:

```output
traceroute to myVmPublic (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

Trafiğin *myVmPrivate* sanal makinesinden *myVmPublic* sanal makinesine doğrudan yönlendirildiğini görebilirsiniz. Azure'un varsayılan rotaları, trafiği doğrudan alt ağlar arasında yönlendirin. 

*myVmPrivate* VM'den *myVmPublic* VM'ye SSH için aşağıdaki komutu kullanın:

```bash
ssh azureuser@myVmPublic
```

*myVmPublic* VM'ye izleme rotası yüklemek için aşağıdaki komutu kullanın:

```bash
sudo apt-get install traceroute
```

*MyVmPublic* VM'den *myVmPrivate* VM'ye ağ trafiği için yönlendirmeyi test etmek için aşağıdaki komutu kullanın.

```bash
traceroute myVmPrivate
```

Yanıt aşağıdaki örneğe benzer:

```output
traceroute to myVmPrivate (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

İlk atlamanın, NVA özel IP adresi olan 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama ise *myVmPrivate* sanal makinesinin özel IP adresi olan 10.0.1.4’tür. *myRouteTablePublic* yönlendirme tablosuna eklenip *Genel* alt ağ ile ilişkilendirilen yönlendirme, Azure’un trafiği doğrudan *Özel* alt ağına yönlendirmek yerine NVA aracılığıyla yönlendirmesine neden olmuştur.

SSH oturumlarını hem *myVmPublic* hem de *myVmPrivate* VM'lere kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az grubu silme'yi](/cli/azure/group) kullanın.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir rota tablosu oluşturdunuz ve bir alt ağla ilişkilendirdin. Bir genel alt ağdan özel alt ağa trafiği yönlendiren basit bir NVA oluşturdunuz. [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking)’ten güvenlik duvarı ve WAN iyileştirme gibi ağ işlevleri gerçekleştiren, önceden yapılandırılmış çeşitli NVA’lar dağıtın. Yönlendirme hakkında daha fazla bilgi için bkz. [Yönlendirmeye genel bakış](virtual-networks-udr-overview.md) ve [Yönlendirme tablosunu yönetme](manage-route-table.md).

Bir sanal ağ içinde çok sayıda Azure kaynağına dağıtabilmenize karşın, bazı Azure PaaS hizmetlerinin kaynakları bir sanal ağa dağıtılamaz. Yine de, bazı Azure PaaS hizmetlerinin kaynaklarına erişimi yalnızca bir sanal ağ alt ağından gelecek trafikle kısıtlayabilirsiniz. Nasıl yapılacağını öğrenmek [için](tutorial-restrict-network-access-to-resources-cli.md)bkz.
