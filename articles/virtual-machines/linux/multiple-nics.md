---
title: Azure'da birden çok NIC içeren bir Linux VM oluşturma
description: Azure CLI veya Kaynak Yöneticisi şablonlarını kullanarak birden fazla NIC'si olan bir Linux VM'yi nasıl oluşturabilirsiniz öğrenin.
author: cynthn
ms.service: virtual-machines-linux
ms.subservice: networking
ms.topic: article
ms.workload: infrastructure
ms.date: 06/07/2018
ms.author: cynthn
ms.openlocfilehash: ecbff4beadd9d10a8489c89cc322c0bb67ec5f40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267189"
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Azure'da birden fazla ağ arabirimi kartı yla bir Linux sanal makinesi nasıl oluşturulur?


Bu makalede, Azure CLI ile birden çok NIC içeren bir VM nasıl oluşturulacak ayrıntılı olarak açıklanmaktadır.

## <a name="create-supporting-resources"></a>Destekleyici kaynaklar oluşturma
En son [Azure CLI'sini](/cli/azure/install-az-cli2) yükleyin ve az giriş 'i kullanarak bir Azure [hesabına](/cli/azure/reference-index)giriş yapın.

Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları *myResourceGroup,* *mystorageaccount*ve *myVM'i*içeriyordu.

Öncelikle [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

[Az ağ vnet oluşturmak](/cli/azure/network/vnet)ile sanal ağ oluşturun. Aşağıdaki örnek *myVnet* adlı bir sanal ağ oluşturur ve *mySubnetFrontEnd*adlı alt ağ:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 10.0.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 10.0.1.0/24
```

[Az network vnet subnet oluşturmak](/cli/azure/network/vnet/subnet)ile arka uç trafiği için bir alt ağ oluşturun. Aşağıdaki *örnekmySubnetBackEnd*adlı bir alt ağ oluşturur:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 10.0.2.0/24
```

[az ağ nsg oluşturmak](/cli/azure/network/nsg)ile bir ağ güvenlik grubu oluşturun. Aşağıdaki örnek *myNetworkSecurityGroup* adında bir ağ güvenlik grubu oluşturur:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Birden çok NIC oluşturma ve yapılandırma
Az ağ nic oluşturmak ile iki NIC [oluşturun.](/cli/azure/network/nic) Aşağıdaki örnek, *myNic1* ve *myNic2*adlı iki NIC'oluşturur, ağ güvenlik grubunu birbirine bağlar ve her alt ağa bir NIC bağlanır:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a>Bir VM oluşturun ve NIC'leri takın
VM'yi oluşturduğunuzda, oluşturduğunuz NIC'leri belirtin. `--nics` VM boyutunu seçerken de dikkatli olmanız gerekir. VM'ye ekleyebileceğiniz toplam NIC sayısı için sınırlar vardır. [Linux VM boyutları](sizes.md)hakkında daha fazla bilgi edinin.

[az vm create](/cli/azure/vm) ile bir VM oluşturun. Aşağıdaki örnek *myVM* adlı bir VM oluşturur:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

Birden çok NIC için konuk işletim sistemi [yapılandırma](#configure-guest-os-for-multiple-nics)adımlarını tamamlayarak konuk işletim sistemi yönlendirme tabloları ekleyin.

## <a name="add-a-nic-to-a-vm"></a>VM'ye NIC ekleme
Önceki adımlar, birden çok NIC içeren bir VM oluşturdu. Azure CLI ile varolan bir VM'ye NIC'ler de ekleyebilirsiniz. Farklı [VM boyutları,](sizes.md) farklı sayıda NIC'i destekler, bu nedenle VM'nizi buna göre boyutlandırın. Gerekirse, bir [VM yeniden boyutlandırabilirsiniz.](change-vm-size.md)

Az ağ nic oluşturmak ile başka bir NIC [oluşturun.](/cli/azure/network/nic) Aşağıdaki örnek, önceki adımlarda oluşturulan arka uç alt ağına ve ağ güvenlik grubuna bağlı *myNic3* adlı bir NIC oluşturur:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

Varolan bir VM'ye NIC eklemek için, önce [az vm deallocate](/cli/azure/vm)ile VM'yi bulun. Aşağıdaki örnek, *myVM*adlı VM'yi yerlebir eder:


```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

[az vm nic ekle](/cli/azure/vm/nic)nic ekleyin. Aşağıdaki örnek *myVM* *myNic3* ekler:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

[Az vm start](/cli/azure/vm)ile VM başlatın:

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

Birden çok NIC için konuk işletim sistemi [yapılandırma](#configure-guest-os-for-multiple-nics)adımlarını tamamlayarak konuk işletim sistemi yönlendirme tabloları ekleyin.

## <a name="remove-a-nic-from-a-vm"></a>VM'den NIC kaldırma
Varolan bir VM'den NIC'yi kaldırmak için, ilk olarak [Az vm deallocate](/cli/azure/vm)ile VM'yi bulun. Aşağıdaki örnek, *myVM*adlı VM'yi yerlebir eder:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

[Az vm nic kaldır](/cli/azure/vm/nic)ile NIC kaldırın. Aşağıdaki örnek *myVM* *myNic3* kaldırır:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

[Az vm start](/cli/azure/vm)ile VM başlatın:

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarını kullanarak birden çok NIC oluşturma
Azure Kaynak Yöneticisi şablonları, ortamınızı tanımlamak için bildirimsel JSON dosyalarını kullanır. Azure Kaynak [Yöneticisi'ne genel bir bakış](../../azure-resource-manager/management/overview.md)okuyabilirsiniz. Kaynak Yöneticisi şablonları, dağıtım sırasında birden çok NIC oluşturma gibi birden çok kaynak örneği oluşturmanın bir yolunu sağlar. Oluşturmak için örnek sayısını belirtmek için *kopya* kullanın:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

[ *Kopyakullanarak*birden çok örnek oluşturma](../../resource-group-create-multiple.md)hakkında daha fazla bilgi edinin. 

Ayrıca bir `copyIndex()` sayıyı kaynak adına eklemek için de kullanabilirsiniz, `myNic1` `myNic2`bu da , vb. oluşturmanıza olanak tanır. Aşağıdaki dizin değeri ekleyen bir örnek gösterir:

```json
"name": "[concat('myNic', copyIndex())]", 
```

[Kaynak Yöneticisi şablonlarını kullanarak birden çok NIC oluşturmanın](../../virtual-network/template-samples.md)tam bir örneğini okuyabilirsiniz.

Birden çok NIC için konuk işletim sistemi [yapılandırma](#configure-guest-os-for-multiple-nics)adımlarını tamamlayarak konuk işletim sistemi yönlendirme tabloları ekleyin.

## <a name="configure-guest-os-for-multiple-nics"></a>Birden çok NIC için konuk işletim sistemi yapılandırma

Önceki adımlar sanal ağ ve alt ağ, bağlı NIC'ler, sonra bir VM oluşturdu. SSH trafiğine izin veren genel bir IP adresi ve ağ güvenlik grubu kuralları oluşturulmamada. Konuk işletim sistemi birden çok NIC için yapılandırmak için, uzak bağlantılara izin vermeniz ve VM'de yerel komutları çalıştırmanız gerekir.

SSH trafiğine izin vermek için, [az ağ nsg kuralıaşağıdaki](/cli/azure/network/nsg/rule#az-network-nsg-rule-create) gibi oluşturmak ile bir ağ güvenlik grubu kuralı oluşturun:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name allow_ssh \
    --priority 101 \
    --destination-port-ranges 22
```

[az ağ public-ip oluşturmak](/cli/azure/network/public-ip#az-network-public-ip-create) ile ortak bir IP adresi oluşturun ve az ağ nic [ip-config güncelleştirmesi](/cli/azure/network/nic/ip-config#az-network-nic-ip-config-update)ile ilk NIC atamak:

```azurecli
az network public-ip create --resource-group myResourceGroup --name myPublicIP

az network nic ip-config update \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --name ipconfig1 \
    --public-ip myPublicIP
```

VM'nin genel IP adresini görüntülemek için [az vm show'u](/cli/azure/vm#az-vm-show) aşağıdaki gibi kullanın:

```azurecli
az vm show --resource-group myResourceGroup --name myVM -d --query publicIps -o tsv
```

Şimdi SSH VM'nizin genel IP adresine. Önceki adımda sağlanan varsayılan kullanıcı adı *azureuser*oldu. Kendi kullanıcı adınızı ve genel IP adresinizi girin:

```bash
ssh azureuser@137.117.58.232
```

İkincil bir ağ arabirimine veya gönderenler için, her ikincil ağ arabirimi için işletim sistemine kalıcı yolları el ile eklemeniz gerekir. Bu makalede, *eth1* ikincil arabirimidir. İşletim sistemine kalıcı rotalar ekleme yönergeleri dağıtıma göre değişir. Talimatlar için dağıtım belgelerinize bakın.

İşletim sistemine rota eklerken, ağ arabiriminin içinde olduğu alt ağ için ağ geçidi adresi *0,1'dir.* Örneğin, ağ arabirimine *10.0.2.4*adresi atanırsa, rota için belirttiğiniz ağ geçidi *10.0.2.1'dir.* Rotanın hedefi için belirli bir ağ tanımlayabilirsiniz veya arabirimiçin tüm trafiğin belirtilen ağ geçidinden gitmesini istiyorsanız *0.0.0.0*hedefi belirtebilirsiniz. Her alt ağ için ağ geçidi sanal ağ tarafından yönetilir.

İkincil arabirimin rotasını ekledikten sonra, rotanın rota tablonuzda ' ile `route -n`birlikte olduğunu doğrulayın. Aşağıdaki örnek çıktı, bu makalede VM'ye iki ağ arabirimi eklenen rota tablosu içindir:

```bash
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         10.0.1.1        0.0.0.0         UG    0      0        0 eth0
0.0.0.0         10.0.2.1        0.0.0.0         UG    0      0        0 eth1
10.0.1.0        0.0.0.0         255.255.255.0   U     0      0        0 eth0
10.0.2.0        0.0.0.0         255.255.255.0   U     0      0        0 eth1
168.63.129.16   10.0.1.1        255.255.255.255 UGH   0      0        0 eth0
169.254.169.254 10.0.1.1        255.255.255.255 UGH   0      0        0 eth0
```

Eklediğiniz rotanın yeniden başlatmadan sonra rota tablonuzu tekrar kontrol ederek yeniden başlatmalarda devam ettiğini doğrulayın. Bağlantıyı test etmek için, *örneğin eth1'in* ikincil bir ağ arabiriminin adı olduğu aşağıdaki komutu girebilirsiniz:

```bash
ping bing.com -c 4 -I eth1
```

## <a name="next-steps"></a>Sonraki adımlar
Birden çok NIC'li bir VM oluşturmaya çalışırken [Linux VM boyutlarını](sizes.md) gözden geçirin. Her VM boyutunun desteklediği maksimum NIC sayısına dikkat edin.

VM'lerinizi daha da güvenli hale getirmek için, tam zamanında VM erişimini kullanın. Bu özellik, gerektiğinde SSH trafiği için ve belirli bir süre için ağ güvenliği grubu kurallarını açar. Daha fazla bilgi için bkz. [Tam zamanında özelliğini kullanarak sanal makine erişimini yönetme](../../security-center/security-center-just-in-time.md).
