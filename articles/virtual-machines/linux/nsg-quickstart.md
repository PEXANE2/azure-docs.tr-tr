---
title: Azure CLI ile vm bağlantı noktalarını açın
description: Azure CLI'yi kullanarak VM'nize bir bağlantı noktası / bitiş noktası oluşturmayı öğrenin.
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.subservice: networking
ms.topic: article
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: c29fb075fc2d8b512070d7a6cf3fef949def5894
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066624"
---
# <a name="open-ports-and-endpoints-to-a-vm-with-the-azure-cli"></a>Azure CLI ile vm bağlantı noktalarını ve bitiş noktalarını açın

Bir alt ağ veya VM ağ arabirimi üzerinde ağ filtresi oluşturarak Azure'daki sanal makineye (VM) bir bağlantı noktası açar veya bir bitiş noktası oluşturursunuz. Hem gelen hem de giden trafiği kontrol eden bu filtreleri, trafiği alan kaynağa bağlı bir Ağ Güvenlik Grubu'na yerebilirsiniz. Bağlantı noktası 80'deki web trafiğinin yaygın bir örneğini kullanalım. Bu makalede, Azure CLI ile bir VM bağlantı noktasını nasıl açacağınızı gösterir. 


Bir Ağ Güvenlik Grubu ve kuralları oluşturmak için en son [Azure CLI'nin](/cli/azure/install-az-cli2) [az girişi](/cli/azure/reference-index)kullanarak bir Azure hesabına yüklenmesi ve oturum açması gerekir.

Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları *myResourceGroup,* *myNetworkSecurityGroup*ve *myVnet'i*içerir.


## <a name="quickly-open-a-port-for-a-vm"></a>VM için hızlı bir bağlantı noktası açın
Bir dev/test senaryosunda bir VM için bağlantı noktasını hızlı bir şekilde açmanız gerekiyorsa, [az vm açık bağlantı noktası](/cli/azure/vm) komutunu kullanabilirsiniz. Bu komut bir Ağ Güvenlik Grubu oluşturur, bir kural ekler ve bir VM veya alt ağa uygular. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myVM* adlı VM'de *80* no'l

```azurecli
az vm open-port --resource-group myResourceGroup --name myVM --port 80
```

Kaynak IP adresi aralığını tanımlama gibi kurallar üzerinde daha fazla denetim için bu makaledeki ek adımlarla devam edin.


## <a name="create-a-network-security-group-and-rules"></a>Ağ Güvenlik Grubu oluşturma ve kurallar
[Az ağ nsg oluşturmak](/cli/azure/network/nsg)ile ağ güvenlik grubu oluşturun. Aşağıdaki örnek, *eastus* konumda *myNetworkSecurityGroup* adlı bir ağ güvenlik grubu oluşturur:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Web sunucunuza HTTP trafiğine izin vermek için [az network nsg kuralı oluşturarak](/cli/azure/network/nsg/rule) bir kural ekleyin (veya SSH erişimi veya veritabanı bağlantısı gibi kendi senaryonuza göre ayarlayın). Aşağıdaki örnek, 80 no'l'daki bağlantı noktasında TCP trafiğine izin vermek için *myNetworkSecurityGroupRule* adlı bir kural oluşturur:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```


## <a name="apply-network-security-group-to-vm"></a>Ağ Güvenlik Grubunu VM'ye Uygula
Ağ Güvenlik Grubu'nu VM ağ arabirimi (NIC) ile [az network nic güncellemesi](/cli/azure/network/nic)ile ilişkilendirin. Aşağıdaki örnek, *myNetworkSecurityGroup*adlı Ağ Güvenlik Grubu ile *myNic* adlı varolan bir NIC ilişkilendirer:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Alternatif olarak, Ağ Güvenlik Grubunuzu tek bir VM'deki ağ arabirimine değil, [az ağ vnet alt ağı güncelleştirmesiyle](/cli/azure/network/vnet/subnet) sanal ağ alt ağıyla ilişkilendirebilirsiniz. Aşağıdaki örnek, *myVnet* sanal ağında *mySubnet* adlı varolan bir alt ağı *myNetworkSecurityGroup*adlı Ağ Güvenlik Grubu ile ilişkilendirer:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Ağ Güvenlik Grupları hakkında daha fazla bilgi
Buradaki hızlı komutlar, VM'nize akan trafikle çalışmaya devam etmenizi sağlar. Ağ Güvenlik Grupları, kaynaklarınıza erişimi denetlemek için birçok harika özellik ve ayrıntılılık sağlar. [Ağ Güvenlik Grubu ve ACL kuralları oluşturma](tutorial-virtual-network.md#secure-network-traffic)hakkında daha fazla bilgi buradan edinebilirsiniz.

Yüksek kullanılabilir web uygulamaları için, VM'lerinizi bir Azure Yük Dengeleyicisi'nin arkasına yerleştirmelisiniz. Yük dengeleyicisi, trafik filtreleme sağlayan bir Ağ Güvenlik Grubu ile Trafiği VM'lere dağıtır. Daha fazla bilgi için, [yüksek kullanılabilir bir uygulama oluşturmak için Azure'da Linux sanal makinelerinin bakiyesini nasıl yükleyin.](tutorial-load-balancer.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, HTTP trafiğine izin vermek için basit bir kural oluşturdunuz. Aşağıdaki makalelerde daha ayrıntılı ortamlar oluşturma hakkında bilgi bulabilirsiniz:

* [Azure Kaynak Yöneticisi'ne genel bakış](../../azure-resource-manager/management/overview.md)
* [Ağ Güvenlik Grubu (NSG) Nedir?](../../virtual-network/security-overview.md)
