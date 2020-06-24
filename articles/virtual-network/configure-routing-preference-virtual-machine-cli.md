---
title: VM için yönlendirme tercihini Yapılandırma-Azure CLı
description: Azure komut satırı arabirimi 'ni (CLı) kullanarak yönlendirme tercihi seçimiyle genel IP adresi ile bir VM oluşturmayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2020
ms.author: mnayak
ms.openlocfilehash: d5ea44e7059ae01204dbafd454c187e10f85e4e8
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84707557"
---
# <a name="configure-routing-preference-for-a-vm-using-azure-cli"></a>Azure CLı kullanarak bir VM için yönlendirme tercihini yapılandırma

Bu makalede, bir sanal makine için yönlendirme tercihini yapılandırma gösterilmektedir. Internet 'e ait trafiği yönlendirme tercih seçeneği olarak **Internet** ' i SEÇTIĞINIZDE, ISS ağı aracılığıyla yönlendirilecektir. Varsayılan yönlendirme, Microsoft Global ağı aracılığıyla yapılır.

Bu makalede, Azure CLı kullanarak genel İnternet üzerinden trafiği yönlendirmek üzere ayarlanmış bir genel IP ile sanal makine oluşturma işlemi gösterilmektedir.

> [!IMPORTANT]
> Yönlendirme tercihi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-feature-for-your-subscription"></a>Aboneliğiniz için özelliği kaydedin
Yönlendirme tercihi özelliği şu anda önizlemededir. Aboneliğiniz için özelliği aşağıdaki şekilde kaydedin:
```azurecli
az feature register --namespace Microsoft.Network --name AllowRoutingPreferenceFeature
```
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
1. Cloud Shell kullanılıyorsa adım 2 ' ye atlayın. Bir komut oturumu açın ve ile Azure 'da oturum açın `az login` .
2. [az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, Doğu ABD Azure bölgesinde bir kaynak grubu oluşturur:

    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma
Sanal makinelerinize Internet 'ten erişmek için genel bir IP adresi oluşturmanız gerekir. [az network public-ip create](/cli/azure/network/public-ip) komutu ile bir genel IP adresi oluşturun. Aşağıdaki örnek, *Doğu ABD* bölgesinde *Internet* yönlendirme tercihi türü olan genel bir IP oluşturur:

```azurecli
az network public-ip create \
--name MyRoutingPrefIP \
--resource-group MyResourceGroup \
--location eastus \
--ip-tags 'RoutingPreference=Internet' \
--sku STANDARD \
--allocation-method static \
--version IPv4
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

Bir VM 'yi dağıtmadan önce, destekleyici ağ kaynakları (ağ güvenlik grubu, sanal ağ ve sanal NIC) oluşturmanız gerekir.

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

[Az Network NSG Create](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create) ile VNET 'iniz içindeki gelen ve giden iletişimi yönetecek kurallar için bir ağ güvenlik grubu oluşturun

```azurecli
az network nsg create \
--name myNSG  \
--resource-group MyResourceGroup \
--location eastus
```

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-create) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek, alt ağ ile *Myvnet* adlı bir sanal ağ oluşturur *mysubnet*:

```azurecli
# Create a virtual network
az network vnet create \
--name myVNET \
--resource-group MyResourceGroup \
--location eastus

# Create a subnet
az network vnet subnet create \
--name mySubNet \
--resource-group MyResourceGroup \
--vnet-name myVNET \
--address-prefixes "10.0.0.0/24" \
--network-security-group myNSG
```

### <a name="create-a-nic"></a>NIC oluşturma

[Az Network NIC Create](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create)komutuyla VM için BIR sanal NIC oluşturun. Aşağıdaki örnek, VM 'ye eklenecek bir sanal NIC oluşturur.

```azurecli-interactive
# Create a NIC
az network nic create \
--name mynic  \
--resource-group MyResourceGroup \
--network-security-group myNSG  \
--vnet-name myVNET \
--subnet mySubNet  \
--private-ip-address-version IPv4 \
--public-ip-address MyRoutingPrefIP
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[az vm create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) ile bir VM oluşturun. Aşağıdaki örnekte, zaten mevcut değilse, bir Windows Server 2019 VM ve gerekli sanal ağ bileşenleri oluşturulur.

```azurecli
az vm create \
--name myVM \
--resource-group MyResourceGroup \
--nics mynic \
--size Standard_A2 \
--image MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest \
--admin-username myUserName
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

- [Genel IP adreslerinde yönlendirme tercihi](routing-preference-overview.md)hakkında daha fazla bilgi edinin.
- Azure 'da [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) hakkında daha fazla bilgi edinin.
- [Genel IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address)hakkında daha fazla bilgi edinin.
