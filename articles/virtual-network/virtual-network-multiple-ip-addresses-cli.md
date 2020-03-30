---
title: Azure CLI'yi kullanarak birden fazla IP adresine sahip VM
titlesuffix: Azure Virtual Network
description: Azure komut satırı arabirimini (CLI) kullanarak sanal bir makineye birden çok IP adresi atamayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: kumud
ms.openlocfilehash: 144f30463adb3dfbce1717e06548baccc8286f8b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240231"
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-cli"></a>Azure CLI'yi kullanarak sanal makinelere birden çok IP adresi atama

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Bu makalede, Azure CLI'yi kullanarak Azure Kaynak Yöneticisi dağıtım modeli aracılığıyla sanal makine (VM) nasıl oluşturulacak açıklanmaktadır. Klasik dağıtım modeli aracılığıyla oluşturulan kaynaklara birden çok IP adresi atanamaz. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için [dağıtım modellerini anlayın](../resource-manager-deployment-model.md) makalesini okuyun.

[!INCLUDE [virtual-network-multiple-ip-addresses-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name="create-a-vm-with-multiple-ip-addresses"></a><a name = "create"></a>Birden çok IP adresi ile VM oluşturma

İzleyen adımlar, senaryoda açıklandığı gibi, birden çok IP adresine sahip örnek bir sanal makinenin nasıl oluşturulacak olduğunu açıklar. Uygulamanız için "" ve IP adresi türlerinde değişken değerlerini gerektiği gibi değiştirin. 

1. Azure [CLI'yi](/cli/azure/install-azure-cli) zaten yüklü değilseniz yükleyin.
2. Linux VM'leri için SSH ortak ve özel anahtar çifti oluşturma adımlarını tamamlayarak Linux [VM'leri için bir SSH ortak ve özel anahtar çifti](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json)oluşturun.
3. Komut kabuğundan komutla giriş `az login` yapın ve kullandığınız aboneliği seçin.
4. Linux veya Mac bilgisayarında izleyen komut dosyasını çalıştırarak VM'yi oluşturun. Komut dosyası bir kaynak grubu, bir sanal ağ (VNet), üç IP yapılandırması ile bir NIC ve ona bağlı iki NIC ile bir VM oluşturur. NIC, genel IP adresi, sanal ağ ve VM kaynaklarının tümü aynı konumda ve abonelikte bulunmalıdır. Kaynakların tümü aynı kaynak grubunda var olmak zorunda olmasa da, aşağıdaki komut dosyasında varolurlar.

```bash
    
#!/bin/sh
    
RgName="myResourceGroup"
Location="westcentralus"
az group create --name $RgName --location $Location
    
# Create a public IP address resource with a static IP address using the `--allocation-method Static` option. If you
# do not specify this option, the address is allocated dynamically. The address is assigned to the resource from a pool
# of IP addresses unique to each Azure region. Download and view the file from
# https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists the ranges for each region.

PipName="myPublicIP"

# This name must be unique within an Azure location.
DnsName="myDNSName"

az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--dns-name $DnsName\
--allocation-method Static

# Create a virtual network with one subnet

VnetName="myVnet"
VnetPrefix="10.0.0.0/16"
VnetSubnetName="mySubnet"
VnetSubnetPrefix="10.0.0.0/24"

az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnetName \
--subnet-prefix $VnetSubnetPrefix

# Create a network interface connected to the subnet and associate the public IP address to it. Azure will create the
# first IP configuration with a static private IP address and will associate the public IP address resource to it.

NicName="MyNic1"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--private-ip-address 10.0.0.4
--vnet-name $VnetName \
--public-ip-address $PipName
    
# Create a second public IP address, a second IP configuration, and associate it to the NIC. This configuration has a
# static public IP address and a static private IP address.

az network public-ip create \
--resource-group $RgName \
--location $Location \
--name myPublicIP2 \
--dns-name mypublicdns2 \
--allocation-method Static

az network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--name IPConfig-2 \
--private-ip-address 10.0.0.5 \
--public-ip-name myPublicIP2

# Create a third IP configuration, and associate it to the NIC. This configuration has  static private IP address and   # no public IP address.

az network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--private-ip-address 10.0.0.6 \
--name IPConfig-3

# Note: Though this article assigns all IP configurations to a single NIC, you can also assign multiple IP configurations
# to any NIC in a VM. To learn how to create a VM with multiple NICs, read the Create a VM with multiple NICs 
# article: https://docs.microsoft.com/azure/virtual-network/virtual-network-deploy-multinic-arm-cli.

# Create a VM and attach the NIC.

VmName="myVm"

# Replace the value for the following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article. The script fails if the VM size
# is not supported in the location you select. Run the `azure vm sizes --location eastcentralus` command to get a full list
# of VMs in US West Central, for example.

VmSize="Standard_DS1"

# Replace the value for the OsImage variable value with a value for *urn* from the output returned by entering the
# `az vm image list` command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace the following value with the path to your public key file. If you're creating a Windows VM, remove the following
# line and you'll be prompted for the password you want to configure for the VM.

SshKeyValue="~/.ssh/id_rsa.pub"

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $NicName \
--admin-username $Username \
--ssh-key-value $SshKeyValue
```

Komut dosyası, 3 IP yapılandırmalı NIC'li bir VM oluşturmaya ek olarak şunları oluşturur:

- Varsayılan olarak tek bir premium yönetilen disk, ancak oluşturabileceğiniz disk türü için başka seçenekleriniz vardır. Ayrıntılar için [Azure CLI makalesini kullanarak Linux VM Oluştur'u](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) okuyun.
- Bir alt net ve iki genel IP adresi olan bir sanal ağ. Alternatif olarak, *varolan* sanal ağ, alt ağ, NIC veya genel IP adresi kaynaklarını kullanabilirsiniz. Ek kaynaklar oluşturmak yerine varolan ağ kaynaklarını `az vm create -h`nasıl kullanacağınızı öğrenmek için girin.

Genel IP adreslerinin nominal ücreti vardır. IP adresi fiyatlandırması hakkında daha fazla bilgi edinmek için [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfasını okuyun. Abonelikte kullanılabilecek genel IP adreslerinin sayısının bir sınırı vardır. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits) makalesini okuyun.

VM oluşturulduktan sonra NIC `az network nic show --name MyNic1 --resource-group myResourceGroup` yapılandırmasını görüntülemek için komutu girin. NIC `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` ile ilişkili IP yapılandırmalarının listesini görüntülemek için girin.

Bu makalenin Bir VM işletim sistemi bölümüne IP adresleri ekle işletim sisteminizin adımlarını tamamlayarak özel IP adreslerini [VM işletim sistemine](#os-config) ekleyin.

## <a name="add-ip-addresses-to-a-vm"></a><a name="add"></a>VM'ye IP adresleri ekleme

Aşağıdaki adımları tamamlayarak varolan bir Azure ağ arabirimine ek özel ve genel IP adresleri ekleyebilirsiniz. Örnekler, bu makalede açıklanan [senaryo](#scenario) üzerine inşa edin.

1. Bir komut kabuğu açın ve bu bölümdeki kalan adımları tek bir oturumda tamamlayın. Azure CLI'yi zaten yüklemediyseniz ve yapılandırmanız yoksa, [Azure CLI yükleme](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesindeki adımları tamamlayın `az-login` ve komutla Azure hesabınıza giriş yapın.

2. Gereksinimlerinize bağlı olarak aşağıdaki bölümlerden birinde adımları tamamlayın:

    **Özel IP adresi ekleme**
    
    NIC'e özel bir IP adresi eklemek için, aşağıdaki komutu kullanarak bir IP yapılandırması oluşturmanız gerekir. Statik IP adresi alt ağ için kullanılmayan bir adres olmalıdır.

    ```azurecli
    az network nic ip-config create \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --private-ip-address 10.0.0.7 \
    --name IPConfig-4
    ```
    
    Benzersiz yapılandırma adlarını ve özel IP adreslerini kullanarak (statik IP adreslerine sahip yapılandırmalar için) istediğiniz kadar yapılandırma oluşturun.

    **Herkese açık bir IP adresi ekleme**
    
    Genel bir IP adresi, yeni bir IP yapılandırması veya varolan bir IP yapılandırması ile ilişkilendirilerek eklenir. Aşağıdaki bölümlerden birinde, istediğiniz gibi adımları tamamlayın.

    Genel IP adreslerinin nominal ücreti vardır. IP adresi fiyatlandırması hakkında daha fazla bilgi edinmek için [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfasını okuyun. Abonelikte kullanılabilecek genel IP adreslerinin sayısının bir sınırı vardır. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits) makalesini okuyun.

    - **Kaynağı yeni bir IP yapılandırması ile ilişkilendirme**
    
        Yeni bir IP yapılandırmasında herkese açık bir IP adresi eklediğinizde, tüm IP yapılandırmalarının özel bir IP adresi olması gerektiğinden, özel bir IP adresi de eklemeniz gerekir. Varolan bir genel IP adresi kaynağı ekleyebilir veya yeni bir tane oluşturabilirsiniz. Yeni bir komut oluşturmak için aşağıdaki komutu girin:
    
        ```azurecli
        az network public-ip create \
        --resource-group myResourceGroup \
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3
        ```

        Statik özel IP adresi ve ilişkili *myPublicIP3* genel IP adresi kaynağı yla yeni bir IP yapılandırması oluşturmak için aşağıdaki komutu girin:

        ```azurecli
        az network nic ip-config create \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-5 \
        --private-ip-address 10.0.0.8
        --public-ip-address myPublicIP3
        ```

    - **Kaynağı varolan bir IP yapılandırmasına ilişkilendirme** Ortak BIR IP adresi kaynağı yalnızca zaten ilişkili olmayan bir IP yapılandırması ile ilişkilendirilebilir. Aşağıdaki komutu girerek bir IP yapılandırmasının ilişkili bir genel IP adresine sahip olup olmadığını belirleyebilirsiniz:

        ```azurecli
        az network nic ip-config list \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --query "[?provisioningState=='Succeeded'].{ Name: name, PublicIpAddressId: publicIpAddress.id }" --output table
        ```

        Döndürülen çıktı:
    
            Name        PublicIpAddressId
            
            ipconfig1   /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
            IPConfig-2  /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
            IPConfig-3

        *IpConfig-3* için **PublicIpAddressId** sütunu çıktıda boş olduğundan, şu anda bu sütunla ilişkili hiçbir genel IP adresi kaynağı bulunmamaktadır. IpConfig-3'e varolan bir genel IP adresi kaynağı ekleyebilir veya aşağıdaki komutu girerek bir tane oluşturabilirsiniz:

        ```azurecli
        az network public-ip create \
        --resource-group  myResourceGroup
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3 \
        --allocation-method Static
        ```
    
        Genel IP adresi kaynağını *IPConfig-3*adlı mevcut IP yapılandırmasına ilişkilendirmek için aşağıdaki komutu girin:
    
        ```azurecli
        az network nic ip-config update \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-3 \
        --public-ip myPublicIP3
        ```

3. Aşağıdaki komutu girerek NIC'e atanan özel IP adreslerini ve genel IP adresi kaynağı Kimliklerini görüntüleyin:

    ```azurecli
    az network nic ip-config list \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --query "[?provisioningState=='Succeeded'].{ Name: name, PrivateIpAddress: privateIpAddress, PrivateIpAllocationMethod: privateIpAllocationMethod, PublicIpAddressId: publicIpAddress.id }" --output table
    ```

    Döndürülen çıktı: <br>
    
        Name        PrivateIpAddress    PrivateIpAllocationMethod   PublicIpAddressId
        
        ipconfig1   10.0.0.4            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
        IPConfig-2  10.0.0.5            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
        IPConfig-3  10.0.0.6            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP3
    

4. BU makalenin Bir [VM işletim sistemi](#os-config) bölümüne IP adresleri ekle yönergeleriizek vm işletim sistemine NIC'ye eklediğiniz özel IP adreslerini ekleyin. Ortak IP adreslerini işletim sistemine eklemeyin.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
