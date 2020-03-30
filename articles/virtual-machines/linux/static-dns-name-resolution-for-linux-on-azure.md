---
title: Azure CLI ile VM ad çözümü için dahili DNS'yi kullanma
description: Azure CLI ile Azure'da sanal ağ arabirim kartları oluşturma ve Azure'da VM ad çözümü için dahili DNS nasıl kullanılır?
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 02/16/2017
ms.author: cynthn
ms.openlocfilehash: acfdfd4edf90b90998a913fa0c6479bedf0028b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034738"
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a>Azure'da sanal ağ arabirim kartları oluşturun ve VM ad çözümü için dahili DNS kullanın

Bu makalede, Azure CLI ile sanal ağ arabirim kartları (vNics) ve DNS etiket adları kullanarak Linux VM'ler için statik dahili DNS adları nasıl ayarlayabileceğinizgösterilmektedir. Statik DNS adları, bu belge için kullanılan Jenkins build sunucusu veya Git sunucusu gibi kalıcı altyapı hizmetleri için kullanılır.

Gereksinimler şunlardır:

* [bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/)
* [SSH ortak ve özel anahtar dosyaları](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Hızlı komutlar
Görevi hızlı bir şekilde gerçekleştirmeniz gerekiyorsa, aşağıdaki bölüm gereken komutları ayrıntılarıyla anlatır. Her adım için daha ayrıntılı bilgi ve bağlam [buradan başlayarak](#detailed-walkthrough)belgenin geri kalanında bulunabilir. Bu adımları gerçekleştirmek için, en son [Azure CLI'nin](/cli/azure/install-az-cli2) [az girişi](/cli/azure/reference-index)kullanarak bir Azure hesabına yüklenmesi ve oturum açması gerekir.

Ön Gereksinimler: Kaynak Grubu, sanal ağ ve alt ağ, SSH gelen ağ güvenlik grubu.

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a>Statik dahili DNS adı olan bir sanal ağ arabirim kartı oluşturma
az ağ nic oluşturmak ile vNic [oluşturun.](/cli/azure/network/nic) `--internal-dns-name` CLI bayrağı, sanal ağ arabirimi kartı (vNic) için statik DNS adını sağlayan DNS etiketini ayarlamak içindir. Aşağıdaki örnek, `myNic` `myVnet` adlı bir vNic oluşturur, sanal ağa bağlanır ve bir iç DNS ad kaydı oluşturur: `jenkins`

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-the-vnic"></a>Bir VM dağıt ve vNic'i bağlayın
[az vm create](/cli/azure/vm) ile bir VM oluşturun. Bayrak, Azure'a `--nics` dağıtım sırasında vNic'i VM'ye bağlar. Aşağıdaki örnek, Azure Yönetilen `myVM` Diskler ile birlikte bir VM `myNic` oluşturur ve önceki adımdan adlandırılmış vNic'i bağlar:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz

Azure'da tam sürekli tümleştirme ve sürekli dağıtım (CiCd) altyapısı, belirli sunucuların statik veya uzun ömürlü sunucular olmasını gerektirir. Sanal ağlar ve Ağ Güvenlik Grupları gibi Azure varlıklarının nadiren dağıtılan statik ve uzun ömürlü kaynaklar olması önerilir. Sanal ağ dağıtıldıktan sonra, altyapıya herhangi bir olumsuz etki yapmadan yeni dağıtımlar tarafından yeniden kullanılabilir. Daha sonra bir Git deposu sunucusu ekleyebilirsiniz veya bir Jenkins otomasyon sunucusu geliştirme veya test ortamlarınız için bu sanal ağa CiCd sağlar.  

Dahili DNS adları yalnızca bir Azure sanal ağında çözülebilir. DNS adları dahili olduğundan, dış internete göre çözülemez ler ve altyapıya ek güvenlik sağlarlar.

Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre `myResourceGroup`adları `myNic`, `myVM`ve .

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma
İlk olarak, az grubu oluşturmak ile kaynak grubu [oluşturun.](/cli/azure/group) Aşağıdaki örnek `westus` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

Bir sonraki adım içine VMs başlatmak için bir sanal ağ oluşturmaktır. Sanal ağ bu iziçin bir alt ağ içerir. Azure sanal ağları hakkında daha fazla bilgi için [bkz.](../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network) 

[Az ağ vnet oluşturmak](/cli/azure/network/vnet)ile sanal ağ oluşturun. Aşağıdaki örnek, adlı `myVnet` bir sanal ağ `mySubnet`ve alt ağ oluşturur:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-the-network-security-group"></a>Ağ Güvenlik Grubu Oluşturma
Azure Ağ Güvenlik Grupları, ağ katmanındaki bir güvenlik duvarına eşdeğerdir. Ağ Güvenlik Grupları hakkında daha fazla bilgi için [Azure CLI'da NSG'ler nasıl oluşturulur'](../../virtual-network/tutorial-filter-network-traffic-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)a bakın. 

[Az ağ nsg oluşturmak](/cli/azure/network/nsg)ile ağ güvenlik grubu oluşturun. Aşağıdaki örnek, adlı `myNetworkSecurityGroup`bir ağ güvenlik grubu oluşturur:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-to-allow-ssh"></a>SSH'ye izin vermek için gelen kural ekleme
[Az ağ nsg kuralı oluşturmak](/cli/azure/network/nsg/rule)ile ağ güvenlik grubu için gelen bir kural ekleyin. Aşağıdaki örnek, adlı `myRuleAllowSSH`bir kural oluşturur:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myRuleAllowSSH \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 22 \
    --access allow
```

## <a name="associate-the-subnet-with-the-network-security-group"></a>Alt ağı Ağ Güvenlik Grubu ile ilişkilendirme
Alt ağı Network Security Group ile ilişkilendirmek için [az network vnet subnet update'i](/cli/azure/network/vnet/subnet)kullanın. Aşağıdaki örnek, alt ağ `mySubnet` adını Ağ Güvenlik `myNetworkSecurityGroup`Grubu adlı ile ilişkilendirer:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-the-virtual-network-interface-card-and-static-dns-names"></a>Sanal ağ arabirim kartı ve statik DNS adlarını oluşturma
Azure çok esnektir, ancak VM ad çözümü için DNS adlarını kullanmak için, DNS etiketi içeren sanal ağ arabirimi kartları (vNics) oluşturmanız gerekir. vNics altyapı yaşam döngüsü üzerinde farklı VM'lere bağlayarak yeniden kullanabilirsiniz gibi önemlidir. Bu yaklaşım, VM'ler geçici olabilirken vNic'i statik bir kaynak olarak tutar. vNic'te DNS etiketlemesini kullanarak, VNet'teki diğer VM'lerden basit ad çözümlemesini etkinleştirebiliriz. Çözülebilir adlar kullanmak, diğer VM'lerin otomasyon sunucusuna DNS adı `Jenkins` `gitrepo`veya Git sunucusu olarak erişmesini sağlar.  

az ağ nic oluşturmak ile vNic [oluşturun.](/cli/azure/network/nic) Aşağıdaki `myNic`örnek, adlı bir vNic oluşturur, `myVnet` adlı `myVnet`sanal ağa bağlanır ve adlı `jenkins`bir iç DNS ad kaydı oluşturur:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>VM'yi sanal ağ altyapısına dağıtma
Artık sanal bir ağımız ve alt ağımız, ssh için 22 bağlantı noktası dışındaki tüm gelen trafiği engelleyerek alt ağımızı korumak için bir güvenlik duvarı görevi yapan bir Ağ Güvenlik Grubu ve bir vNic'imiz var. Artık bu varolan ağ altyapısının içine bir VM dağıtabilirsiniz.

[az vm create](/cli/azure/vm) ile bir VM oluşturun. Aşağıdaki örnek, Azure Yönetilen `myVM` Diskler ile birlikte bir VM `myNic` oluşturur ve önceki adımdan adlandırılmış vNic'i bağlar:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Varolan kaynakları çağırmak için CLI bayraklarını kullanarak, Azure'a VM'yi varolan ağın içinde dağıtmasını emrediyoruz. Yinelemek gerekirse, bir VNet ve alt ağ dağıtıldıktan sonra Azure bölgenizde statik veya kalıcı kaynaklar olarak bırakılabilirler.  

## <a name="next-steps"></a>Sonraki adımlar
* [Azure CLI komutlarını doğrudan kullanarak bir Linux VM için kendi özel ortamınızı oluşturun](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Şablonları kullanarak Azure'da Bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
