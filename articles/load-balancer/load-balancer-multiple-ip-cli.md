---
title: Azure CLI kullanarak birden çok IP yapılandırması üzerinde yük dengeleme
titleSuffix: Azure Load Balancer
description: Azure CLI'yi kullanarak sanal bir makineye birden çok IP adresi atamayı öğrenin.
services: virtual-network
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/25/2018
ms.author: allensu
ms.openlocfilehash: 69d324647af014a5122c404929c104a9077d5f13
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74225299"
---
# <a name="load-balancing-on-multiple-ip-configurations-using-azure-cli"></a>Azure CLI kullanarak birden çok IP yapılandırması üzerinde yük dengeleme

Bu makalede, ikincil bir ağ arabiriminde (NIC) birden çok IP adresi olan Azure Yük Dengeleyicisi'nin nasıl kullanılacağı açıklanmaktadır. Bu senaryo için, windows çalıştıran iki VM'miz vardır, her biri birincil ve ikincil bir NIC'si vardır. İkincil NIC'lerin her birinin iki IP yapılandırması vardır. Her VM, contoso.com ve fabrikam.com web sitelerini de barındırar. Her web sitesi ikincil NIC'deki IP yapılandırmalarından birine bağlıdır. Trafiği web sitesi için ilgili IP yapılandırmasına dağıtmak için her web sitesi için bir tane olmak üzere iki ön uç IP adresini ortaya çıkarmak için Azure Yük Dengeleyicisi'ni kullanırız. Bu senaryo, her iki ön uçta da aynı bağlantı noktası numarasını ve her iki arka uç ip adresini kullanır.

![LB senaryo görüntüsü](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Birden çok IP yapılandırmasına bakiye yüklemek için adımlar

Bu makalede özetlenen senaryoyu elde etmek için aşağıdaki adımları tamamlayın:

1. Bağlı makaledeki adımları izleyerek [Azure CLI'yi yükleyin ve yapılandırın](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve Azure hesabınızda oturum açın.
2. *Contosofabrikam* adlı bir kaynak grubu aşağıdaki gibi [oluşturun:](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-group)

    ```azurecli
    az group create contosofabrikam westcentralus
    ```

3. İki VM için [ayarlanmış bir kullanılabilirlik oluşturun.](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-an-availability-set) Bu senaryo için aşağıdaki komutu kullanın:

    ```azurecli
    az vm availability-set create --resource-group contosofabrikam --location westcentralus --name myAvailabilitySet
    ```

4. *myVNet* adında bir sanal ağ ve *mySubnet*adında bir alt ağ [oluşturun:](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet)

    ```azurecli
    az network vnet create --resource-group contosofabrikam --name myVnet --address-prefixes 10.0.0.0/16  --location westcentralus --subnet-name MySubnet --subnet-prefix 10.0.0.0/24

    ```

5. *mylb*adlı [yük dengeleyicioluşturun:](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

    ```azurecli
    az network lb create --resource-group contosofabrikam --location westcentralus --name mylb
    ```

6. Yük bakiyenizin ön uç IP yapılandırmaları için iki dinamik genel IP adresi oluşturun:

    ```azurecli
    az network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp1 --domain-name-label contoso --allocation-method Dynamic

    az network public-ip create --resource-group contosofabrikam --location westcentralus --name PublicIp2 --domain-name-label fabrikam --allocation-method Dynamic
    ```

7. Sırasıyla iki frontend IP yapılandırmaları, *contosofe* ve *fabrikamfe* oluşturun:

    ```azurecli
    az network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp1 --name contosofe
    az network lb frontend-ip create --resource-group contosofabrikam --lb-name mylb --public-ip-name PublicIp2 --name fabrkamfe
    ```

8. Arka uç adres havuzlarınızı oluşturun - *contosopool* ve *fabrikampool,* bir [sonda](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) - *HTTP*, ve yük dengeleme kuralları - *HTTPc* ve *HTTPf*:

    ```azurecli
    az network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name contosopool
    azure network lb address-pool create --resource-group contosofabrikam --lb-name mylb --name fabrikampool

    az network lb probe create --resource-group contosofabrikam --lb-name mylb --name HTTP --protocol "http" --interval 15 --count 2 --path index.html

    az network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPc --protocol tcp --probe-name http--frontend-port 5000 --backend-port 5000 --frontend-ip-name contosofe --backend-address-pool-name contosopool
    az network lb rule create --resource-group contosofabrikam --lb-name mylb --name HTTPf --protocol tcp --probe-name http --frontend-port 5000 --backend-port 5000 --frontend-ip-name fabrkamfe --backend-address-pool-name fabrikampool
    ```

9. Aşağıdaki komutu çalıştırarak [yük dengeleyicinizin](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json) doğru şekilde oluşturulduğunu doğrulamak için çıktıyı kontrol edin:

    ```azurecli
    az network lb show --resource-group contosofabrikam --name mylb
    ```

10. Aşağıdaki gibi ilk sanal makine VM1 için bir kamu IP , *myPublicIp*, ve [depolama hesabı](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json), *mystorageaccont1* [oluşturun:](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-public-ip-address)

    ```azurecli
    az network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP --domain-name-label mypublicdns345 --allocation-method Dynamic

    az storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount1
    ```

11. VM1 için [ağ arabirimleri oluşturun](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-nic) ve ikinci bir IP yapılandırması olan *VM1-ipconfig2'yi*ekleyin ve [VM'yi](../virtual-machines/linux/create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-vm) aşağıdaki gibi oluşturun:

    ```azurecli
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic1 --ip-config-name NIC1-ipconfig1
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM1Nic2 --ip-config-name VM1-ipconfig1 --public-ip-name myPublicIP --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    az network nic ip-config create --resource-group contosofabrikam --nic-name VM1Nic2 --name VM1-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    az vm create --resource-group contosofabrikam --name VM1 --location westcentralus --os-type linux --nic-names VM1Nic1,VM1Nic2  --vnet-name VNet1 --vnet-subnet-name Subnet1 --availability-set myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount1 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

12. İkinci VM'iniz için 10-11 adımlarını tekrarlayın:

    ```azurecli
    az network public-ip create --resource-group contosofabrikam --location westcentralus --name myPublicIP2 --domain-name-label mypublicdns785 --allocation-method Dynamic
    az storage account create --location westcentralus --resource-group contosofabrikam --kind Storage --sku-name GRS mystorageaccount2
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic1
    az network nic create --resource-group contosofabrikam --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name VM2Nic2 --ip-config-name VM2-ipconfig1 --public-ip-name myPublicIP2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/contosopool"
    az network nic ip-config create --resource-group contosofabrikam --nic-name VM2Nic2 --name VM2-ipconfig2 --lb-address-pool-ids "/subscriptions/<your subscription ID>/resourceGroups/contosofabrikam/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/fabrikampool"
    az vm create --resource-group contosofabrikam --name VM2 --location westcentralus --os-type linux --nic-names VM2Nic1,VM2Nic2 --vnet-name VNet1 --vnet-subnet-name Subnet1 --availability-set myAvailabilitySet --vm-size Standard_DS3_v2 --storage-account-name mystorageaccount2 --image-urn canonical:UbuntuServer:16.04.0-LTS:latest --admin-username <your username>  --admin-password <your password>
    ```

13. Son olarak, DNS kaynak kayıtlarını Yük Dengeleyicisinin ilgili ön uç IP adresini işaret etmek üzere yapılandırmanız gerekir. Etki alanlarınızı Azure DNS'de barındırabilirsiniz. Yük Bakiyesi ile Azure DNS kullanma hakkında daha fazla bilgi [için](../dns/dns-for-azure-services.md)bkz.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure'da yük dengeleme hizmetlerini kullanmada](../traffic-manager/traffic-manager-load-balancing-azure.md)Azure'da yük dengeleme hizmetlerini nasıl birleştirebilirsiniz hakkında daha fazla bilgi edinin.
- [Azure Yük Dengeleyicisi için Günlük analitiğinde](../load-balancer/load-balancer-monitor-log.md)yük dengeleyicisini yönetmek ve sorun gidermek için Azure'da farklı türde günlükleri nasıl kullanabileceğinizi öğrenin.
