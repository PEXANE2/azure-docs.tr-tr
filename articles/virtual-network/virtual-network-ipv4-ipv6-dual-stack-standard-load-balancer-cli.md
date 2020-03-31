---
title: IPv6 çift yığın uygulamasını dağıt - Standart Yük Dengeleyicisi - CLI
titlesuffix: Azure Virtual Network
description: Bu makalede, Azure CLI kullanarak Azure sanal ağında bir IPv6 çift yığın uygulamasınasıl dağıtılır gösterilmektedir.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/17/2019
ms.author: kumud
ms.openlocfilehash: fa895a294e26b6c74ab72afa3136feac2b2ec986
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240244"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure-virtual-network---cli-preview"></a>Azure sanal ağında bir IPv6 çift yığın uygulaması dağıtma - CLI (Önizleme)

Bu makalede, Azure'da çift yığın alt ağına sahip çift yığınlı sanal ağ, çift (IPv4 + IPv6) ön uç yapılandırmalı standart yük dengeleyicisi, VM'ler içeren Standart Yük Dengeleyici'yi kullanarak bir çift yığın (IPv4 + IPv6) uygulamasını nasıl dağıtabileceğinizi gösterir Çift IP yapılandırması, çift ağ güvenlik grubu kuralları ve çift ortak IP'ye sahip NIC'ler.

> [!Important]
> Azure Sanal Ağı için IPv6 çift yığını şu anda genel önizlemededir. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLI'yi yerel olarak yüklemeye ve kullanmaya karar verirseniz, bu hızlı başlangıç Azure CLI sürüm 2.0.49 veya sonraki sürümlerini kullanmanızı gerektirir. Yüklü sürümünüzü bulmak için `az --version`çalıştırın. Bilgileri yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli) bakın.

## <a name="prerequisites"></a>Ön koşullar
Azure sanal ağ için IPv6 özelliğini kullanmak için, aboneliğinizi Azure CLI'yi kullanarak aşağıdaki gibi yapılandırmanız gerekir:

```azurecli
az feature register --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
az feature register --name AllowIPv6CAOnStandardLB --namespace Microsoft.Network
```

Özellik kaydının tamamlanması 30 dakika kadar sürer. Aşağıdaki Azure CLI komutunu çalıştırarak kayıt durumunuzu kontrol edebilirsiniz:

```azurecli
az feature show --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
az feature show --name AllowIPv6CAOnStandardLB --namespace Microsoft.Network
```

Kayıt tamamlandıktan sonra aşağıdaki komutu çalıştırın:

```azurecli
az provider register --namespace Microsoft.Network
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Çift yığınlı sanal ağınızı oluşturmadan [önce, az grubu oluşturarak](/cli/azure/group)bir kaynak grubu oluşturmanız gerekir. Aşağıdaki örnek, *doğudaki* konumda *DsResourceGroup01* adlı bir kaynak grubu oluşturur:

```azurecli
az group create \
--name DsResourceGroup01 \
--location eastus
```

## <a name="create-ipv4-and-ipv6-public-ip-addresses-for-load-balancer"></a>Yük dengeleyicisi için IPv4 ve IPv6 genel IP adresleri oluşturma
Internet'teki IPv4 ve IPv6 uç noktalarınıza erişmek için yük dengeleyicisi için IPv4 ve IPv6 genel IP adreslerine ihtiyacınız vardır. [az network public-ip create](/cli/azure/network/public-ip) komutu ile bir genel IP adresi oluşturun. Aşağıdaki örnek, *DsResourceGroup01* kaynak grubunda *dsPublicIP_v4* ve *dsPublicIP_v6* adlı IPv4 ve IPv6 genel IP adresini oluşturur:

```azurecli
# Create an IPV4 IP address
az network public-ip create \
--name dsPublicIP_v4  \
--resource-group DsResourceGroup01  \
--location eastus  \
--sku STANDARD  \
--allocation-method static  \
--version IPv4

# Create an IPV6 IP address
az network public-ip create \
--name dsPublicIP_v6  \
--resource-group DsResourceGroup01  \
--location eastus \
--sku STANDARD  \
--allocation-method static  \
--version IPv6

```

## <a name="create-public-ip-addresses-for-vms"></a>VM'ler için genel IP adresleri oluşturma

VM'lerinize internetüzerinden uzaktan erişmek için VM'ler için IPv4 genel IP adreslerine ihtiyacınız vardır. [az network public-ip create](/cli/azure/network/public-ip) komutu ile bir genel IP adresi oluşturun.

```azurecli
az network public-ip create \
--name dsVM0_remote_access  \
--resource-group DsResourceGroup01 \
--location eastus  \
--sku Standard  \
--allocation-method static  \
--version IPv4

az network public-ip create \
--name dsVM1_remote_access  \
--resource-group DsResourceGroup01  \
--location eastus  \
--sku Standard  \
--allocation-method static  \
--version IPv4
```

## <a name="create-standard-load-balancer"></a>Standart Load Balancer oluşturma

Bu bölümde, çift ön uç IP (IPv4 ve IPv6) ve yük dengeleyicisi için arka uç adresi havuzunu yapılandırıp bir Standart Yük Dengeleyicisi oluşturursunuz.

### <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

Az ağ lb ile Standart Yük Dengeleyici oluşturun **dsLbFrontEnd_v4**adlı bir ön uç havuzu içeren **dsLB** adlı [oluşturmak,](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) bir önceki adımda oluşturduğunuz iPv4 genel IP adresi ile ilişkili **dsLbBackEndPool_v4** adlı bir arka uç havuzu **dsPublicIP_v4.** 

```azurecli
az network lb create \
--name dsLB  \
--resource-group DsResourceGroup01 \
--sku Standard \
--location eastus \
--frontend-ip-name dsLbFrontEnd_v4  \
--public-ip-address dsPublicIP_v4  \
--backend-pool-name dsLbBackEndPool_v4
```

### <a name="create-ipv6-frontend"></a>IPv6 ön uç oluşturma

[Az ağ lb frontend-ip oluşturmak](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip?view=azure-cli-latest#az-network-lb-frontend-ip-create)ile bir IPV6 frontend IP oluşturun. Aşağıdaki örnek, *dsLbFrontEnd_v6* adında bir ön uç IP yapılandırması oluşturur ve *dsPublicIP_v6* adresi ne eklenir:

```azurepowershell-interactive
az network lb frontend-ip create \
--lb-name dsLB  \
--name dsLbFrontEnd_v6  \
--resource-group DsResourceGroup01  \
--public-ip-address dsPublicIP_v6

```

### <a name="configure-ipv6-back-end-address-pool"></a>IPv6 arka uç adres havuzunda yapılandırma

Az ağ lb adresi havuzu oluşturmak ile bir IPv6 arka uç adresi [havuzları oluşturun.](https://docs.microsoft.com/cli/azure/network/lb/address-pool?view=azure-cli-latest#az-network-lb-address-pool-create) Aşağıdaki örnek, IPv6 NIC yapılandırmalarına sahip VM'leri içerecek *şekilde dsLbBackEndPool_v6* adlı arka uç adres havuzu oluşturur:

```azurecli
az network lb address-pool create \
--lb-name dsLB  \
--name dsLbBackEndPool_v6  \
--resource-group DsResourceGroup01
```

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma
Sanal makinelerin durumunu izlemek için [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest) ile bir durum araştırması oluşturun. 

```azurecli
az network lb probe create -g DsResourceGroup01  --lb-name dsLB -n dsProbe --protocol tcp --port 3389
```

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. 

[az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create) komutuyla bir yük dengeleyici kuralı oluşturun. Aşağıdaki örnek, *dsLBrule_v4* ve *dsLBrule_v6* adlı yük dengeleyici kuralları oluşturur ve *TCP* bağlantı noktası *80'deki* trafiği IPv4 ve IPv6 frontend IP yapılandırmalarına dengeler:

```azurecli
az network lb rule create \
--lb-name dsLB  \
--name dsLBrule_v4  \
--resource-group DsResourceGroup01  \
--frontend-ip-name dsLbFrontEnd_v4  \
--protocol Tcp  \
--frontend-port 80  \
--backend-port 80  \
--probe-name dsProbe \
--backend-pool-name dsLbBackEndPool_v4


az network lb rule create \
--lb-name dsLB  \
--name dsLBrule_v6  \
--resource-group DsResourceGroup01 \
--frontend-ip-name dsLbFrontEnd_v6  \
--protocol Tcp  \
--frontend-port 80 \
--backend-port 80  \
--probe-name dsProbe \
--backend-pool-name dsLbBackEndPool_v6

```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma
Bazı Sanal M'leri dağıtmadan önce, kullanılabilirlik kümesi, ağ güvenlik grubu, sanal ağ ve sanal NIC'ler gibi destekleyici ağ kaynakları oluşturmanız gerekir. 
### <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma
Uygulamanızın kullanılabilirliğini artırmak için, VM'lerinizi bir kullanılabilirlik kümesine yerleştirin.

[az vm availability-set create](https://docs.microsoft.com/cli/azure/vm/availability-set?view=azure-cli-latest) komutunu kullanarak bir kullanılabilirlik kümesi oluşturun. Aşağıdaki örnek *dsAVset*adlı bir kullanılabilirlik kümesi oluşturur:

```azurecli
az vm availability-set create \
--name dsAVset  \
--resource-group DsResourceGroup01  \
--location eastus \
--platform-fault-domain-count 2  \
--platform-update-domain-count 2  
```

### <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma

VNet'inizde gelen ve giden iletişimi yönetecek kurallar için bir ağ güvenlik grubu oluşturun.

#### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

[Az ağ nsg oluşturmak](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create) ile bir ağ güvenlik grubu oluşturma


```azurecli
az network nsg create \
--name dsNSG1  \
--resource-group DsResourceGroup01  \
--location eastus

```

#### <a name="create-a-network-security-group-rule-for-inbound-and-outbound-connections"></a>Gelen ve giden bağlantılar için ağ güvenlik grubu kuralı oluşturma

RdP bağlantılarını bağlantı noktası 3389 üzerinden, bağlantı noktası 80 üzerinden internet bağlantısına ve [az network nsg kuralına](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create)sahip giden bağlantılara izin vermek için bir ağ güvenlik grubu kuralı oluşturun.

```azurecli
# Create inbound rule for port 3389
az network nsg rule create \
--name allowRdpIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 100  \
--description "Allow Remote Desktop In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges 3389

# Create inbound rule for port 80
az network nsg rule create \
--name allowHTTPIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 200  \
--description "Allow HTTP In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges 80  \
--destination-address-prefixes "*"  \
--destination-port-ranges 80

# Create outbound rule

az network nsg rule create \
--name allowAllOut  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 300  \
--description "Allow All Out"  \
--access Allow  \
--protocol "*"  \
--direction Outbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges "*"
```


### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-create) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek, *dsVNET* adlı bir sanal ağ oluşturur alt ağları *dsSubNET_v4* ve *dsSubNET_v6:*

```azurecli
# Create the virtual network
az network vnet create \
--name dsVNET \
--resource-group DsResourceGroup01 \
--location eastus  \
--address-prefixes "10.0.0.0/16" "ace:cab:deca::/48"

# Create a single dual stack subnet

az network vnet subnet create \
--name dsSubNET \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--address-prefixes "10.0.0.0/24" "ace:cab:deca:deed::/64" \
--network-security-group dsNSG1
```

### <a name="create-nics"></a>NIC’leri oluşturma

Az ağ nic oluşturmak ile her VM için sanal NIC'ler [oluşturun.](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) Aşağıdaki örnek, her VM için sanal bir NIC oluşturur. Her NIC iki IP yapılandırması (1 IPv4 config, 1 IPv6 config) vardır. [Az network nic ip-config oluşturmak](https://docs.microsoft.com/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-create)ile IPV6 yapılandırmaoluşturun.
 
```azurecli
# Create NICs
az network nic create \
--name dsNIC0  \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1  \
--vnet-name dsVNET  \
--subnet dsSubNet  \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4  \
--lb-name dsLB  \
--public-ip-address dsVM0_remote_access

az network nic create \
--name dsNIC1 \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4 \
--lb-name dsLB \
--public-ip-address dsVM1_remote_access

# Create IPV6 configurations for each NIC

az network nic ip-config create \
--name dsIp6Config_NIC0  \
--nic-name dsNIC0  \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB

az network nic ip-config create \
--name dsIp6Config_NIC1 \
--nic-name dsNIC1 \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

az vm oluşturmak ile [VM'ler oluşturun.](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) Aşağıdaki örnek, zaten yoksa iki VM ve gerekli sanal ağ bileşenleri oluşturur. 

Aşağıdaki gibi sanal makine *dsVM0* oluşturun:

```azurecli
 az vm create \
--name dsVM0 \
--resource-group DsResourceGroup01 \
--nics dsNIC0 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest  
```

Aşağıdaki gibi sanal makine *dsVM1* oluşturun:

```azurecli
az vm create \
--name dsVM1 \
--resource-group DsResourceGroup01 \
--nics dsNIC1 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest 
```

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>Azure portalında IPv6 çift yığın sanal ağı görüntüleyin
Azure portalındaki IPv6 çift yığın sanal ağını aşağıdaki gibi görüntüleyebilirsiniz:
1. Portalın arama çubuğuna *dsVnet*girin.
2. Arama sonuçlarında **myVirtualNetwork** göründüğünde seçin. Bu *dsVnet*adlı çift yığını sanal ağ **Genel Bakış** sayfası başlattı. Çift yığın sanal ağ *dsSubnet*adlı çift yığın alt ağında bulunan hem IPv4 ve IPv6 yapılandırmaları ile iki NIC gösterir.

  ![Azure'da IPv6 çift yığın sanal ağ](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-vnet.png)

> [!NOTE]
> Azure için IPv6 sanal ağı, bu önizleme sürümü için salt okunur olarak Azure portalında kullanılabilir.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurecli
 az group delete --name DsResourceGroup01
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, çift frontend IP yapılandırmasına (IPv4 ve IPv6) sahip bir Standart Yük Dengeleyicisi oluşturdunuz. Ayrıca, yük dengeleyicisinin arka uç havuzuna eklenen çift IP yapılandırmalı (IPV4 + IPv6) NIC'leri içeren iki sanal makine oluşturdunuz. Azure sanal ağlarında IPv6 desteği hakkında daha fazla bilgi edinmek için Azure [Sanal Ağı için IPv6 nedir'](ipv6-overview.md) e bakın?
