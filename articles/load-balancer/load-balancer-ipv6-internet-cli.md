---
title: IPv6 ile genel yük dengeleyici oluşturma-Azure CLı
titleSuffix: Azure Load Balancer
description: Bu öğrenme yoluyla, Azure CLı kullanarak IPv6 ile genel yük dengeleyici oluşturmaya başlayın.
services: load-balancer
documentationcenter: na
author: asudbring
keywords: IPv6, Azure yük dengeleyici, çift yığın, genel IP, yerel IPv6, mobil, IoT
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/25/2018
ms.author: allensu
ms.openlocfilehash: 03bedba280fca4c051685eabdfa47eaaf00f05d3
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85963334"
---
# <a name="create-a-public-load-balancer-with-ipv6-using-azure-cli"></a>Azure CLı kullanarak IPv6 ile genel yük dengeleyici oluşturma

>[!NOTE] 
>Bu makalede, temel yük dengeleyiciler hem IPv4 hem de IPv6 bağlantısı sağlamasına izin veren bir giriş IPv6 özelliği açıklanır. Artık, sanal ağlarınızla IPv6 bağlantısını tümleştiren ve IPv6 ağ güvenlik grubu kuralları, IPv6 Kullanıcı tanımlı yönlendirme, IPv6 temel ve standart yük dengeleme gibi önemli özellikleri ve daha fazlasını içeren [Azure VNET 'ler için](../virtual-network/ipv6-overview.md) kapsamlı IPv6 bağlantısı kullanılabilir.  Azure sanal ağları için IPv6, Azure 'daki IPv6 uygulamaları için önerilen standarttır. Bkz. [Azure VNET PowerShell dağıtımı Için IPv6](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md) 

Azure Load Balancer bir Katman 4 (TCP, UDP) yük dengeleyicidir. Yük dengeleyiciler, gelen trafiği bulut hizmetlerindeki sağlıklı hizmet örnekleri veya bir yük dengeleyici kümesindeki sanal makineler arasında dağıtarak yüksek kullanılabilirlik sağlar. Yük dengeleyiciler Ayrıca bu hizmetleri birden çok bağlantı noktasında veya birden çok IP adresinde veya her ikisinde de sunabilir.

## <a name="example-deployment-scenario"></a>Örnek dağıtım senaryosu

Aşağıdaki diyagramda, bu makalede açıklanan örnek şablon kullanılarak dağıtılan yük dengeleme çözümü gösterilmektedir.

![Yük dengeleyici senaryosu](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

Bu senaryoda, aşağıdaki Azure kaynaklarını oluşturursunuz:

* İki sanal makine (VM)
* Her VM için hem IPv4 hem de IPv6 adresi atanmış bir sanal ağ arabirimi
* IPv4 ve IPv6 genel IP adresi olan bir genel yük dengeleyici
* İki VM 'yi içeren bir kullanılabilirlik kümesi
* Genel VIP 'leri özel uç noktalarla eşlemek için iki yük dengeleme kuralı

## <a name="deploy-the-solution-by-using-azure-cli"></a>Azure CLı kullanarak çözümü dağıtma

Aşağıdaki adımlarda, Azure CLı kullanarak nasıl ortak yük dengeleyici oluşturacağınız gösterilmektedir. CLı kullanarak her nesneyi ayrı ayrı oluşturup yapılandırıp, ardından bir kaynak oluşturmak için bunları bir araya getirin.

Yük dengeleyici dağıtmak için aşağıdaki nesneleri oluşturun ve yapılandırın:

* **Ön uç IP yapılandırması**: gelen ağ trafiği IÇIN genel IP adreslerini içerir.
* **Arka uç adres havuzu**: sanal makinelerin yük dengeleyiciden ağ trafiği alması için ağ arabirimlerini (NIC 'ler) içerir.
* **Yük Dengeleme kuralları**: yük dengeleyicideki genel bağlantı noktasını arka uç adres havuzundaki bir bağlantı noktasına eşleyen kuralları içerir.
* **Gelen NAT kuralları**: yük dengeleyicideki genel bağlantı noktasını arka uç adres havuzundaki belirli bir sanal makineye yönelik bir bağlantı noktasına eşleyen ağ adresi ÇEVIRISI (NAT) kurallarını içerir.
* **Yoklamalar**: arka uç adres havuzundaki sanal makine örneklerinin kullanılabilirliğini kontrol etmek için kullanılan durum araştırmalarını içerir.

## <a name="set-up-azure-cli"></a>Azure CLI’yı ayarlama

Bu örnekte, Azure CLı araçlarını bir PowerShell komut penceresinde çalıştırırsınız. Okunabilirliği ve yeniden kullanımını geliştirmek için Azure PowerShell cmdlet 'lerini değil, PowerShell 'in betik oluşturma yeteneklerini kullanın.

1. Bağlı makaledeki adımları izleyerek Azure [CLI 'Yı yükleyip yapılandırın](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve Azure hesabınızda oturum açın.

2. Azure CLı komutlarıyla kullanılmak üzere PowerShell değişkenlerini ayarlayın:

    ```powershell
    $subscriptionid = "########-####-####-####-############"  # enter subscription id
    $location = "southcentralus"
    $rgName = "pscontosorg1southctrlus09152016"
    $vnetName = "contosoIPv4Vnet"
    $vnetPrefix = "10.0.0.0/16"
    $subnet1Name = "clicontosoIPv4Subnet1"
    $subnet1Prefix = "10.0.0.0/24"
    $subnet2Name = "clicontosoIPv4Subnet2"
    $subnet2Prefix = "10.0.1.0/24"
    $dnsLabel = "contoso09152016"
    $lbName = "myIPv4IPv6Lb"
    ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Bir kaynak grubu, bir yük dengeleyici, bir sanal ağ ve alt ağ oluşturma

1. Kaynak grubu oluşturun:

    ```azurecli
    az group create --name $rgName --location $location
    ```

2. Yük dengeleyici oluştur:

    ```azurecli
    $lb = az network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. Sanal ağ oluşturma:

    ```azurecli
    $vnet = az network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

4. Bu sanal ağda iki alt ağ oluşturun:

    ```azurecli
    $subnet1 = az network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = az network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Ön uç havuzu için genel IP adresleri oluşturma

1. PowerShell değişkenlerini ayarlayın:

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. Ön uç IP havuzu için genel bir IP adresi oluşturun:

    ```azurecli
    $publicipV4 = az network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --version IPv4 --allocation-method Dynamic --dns-name $dnsLabel
    $publicipV6 = az network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --version IPv6 --allocation-method Dynamic --dns-name $dnsLabel
    ```

    > [!IMPORTANT]
    > Yük dengeleyici, tam etki alanı adı (FQDN) olarak genel IP 'nin etki alanı etiketini kullanır. Bu, bulut hizmeti adını yük dengeleyici FQDN olarak kullanan klasik dağıtımdan bir değişiklik.
    >
    > Bu örnekte, FQDN *contoso09152016.southcentralus.cloudapp.Azure.com*' dir.

## <a name="create-front-end-and-back-end-pools"></a>Ön uç ve arka uç havuzları oluşturma

Bu bölümde, aşağıdaki IP havuzlarını oluşturursunuz:
* Yük dengeleyicide gelen ağ trafiğini alan ön uç IP Havuzu.
* Ön uç havuzunun yük dengeli ağ trafiğini gönderdiği arka uç IP Havuzu.

1. PowerShell değişkenlerini ayarlayın:

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. Ön uç IP havuzu oluşturun ve önceki adımda ve yük dengeleyicide oluşturduğunuz ortak IP ile ilişkilendirin.

    ```azurecli
    $frontendV4 = az network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-address $publicIpv4Name --lb-name $lbName
    $frontendV6 = az network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-address $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = az network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = az network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-the-probe-nat-rules-and-load-balancer-rules"></a>Araştırma, NAT kuralları ve yük dengeleyici kuralları oluşturma

Bu örnek aşağıdaki nesneleri oluşturur:

* TCP bağlantı noktası 80 bağlantısını denetlemek için bir araştırma kuralı.
* 3389 numaralı bağlantı noktasındaki tüm gelen trafiği RDP için 3389 numaralı bağlantı noktasına çevirecek bir NAT kuralı.\*
* 3391 numaralı bağlantı noktasına gelen tüm trafiği Uzak Masaüstü Protokolü (RDP) için bağlantı noktası 3389 ' ye çevirecek bir NAT kuralı.\*
* 80 numaralı bağlantı noktasındaki tüm gelen trafiği arka uç havuzundaki adreslerde 80 numaralı bağlantı noktasına dengelemek için bir yük dengeleyici kuralı.

\*NAT kuralları yük dengeleyicinin arkasındaki belirli bir sanal makine örneğiyle ilişkilendirilir. 3389 numaralı bağlantı noktasına ulaşan ağ trafiği, NAT kuralıyla ilişkili belirli sanal makineye ve bağlantı noktasına gönderilir. NAT kuralı için bir protokol (UDP veya TCP) belirtmeniz gerekir. Her iki protokolü de aynı bağlantı noktasına atayamazsınız.

1. PowerShell değişkenlerini ayarlayın:

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. Araştırması oluşturun.

    Aşağıdaki örnek, her 15 saniyede bir arka uç TCP bağlantı noktası 80 bağlantısını denetleyen bir TCP araştırması oluşturur. Art arda iki hatadan sonra arka uç kaynağını kullanılamaz olarak işaretler.

    ```azurecli
    $probeV4V6 = az network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --threshold 2 --lb-name $lbName
    ```

3. Arka uç kaynaklarına RDP bağlantılarına izin veren gelen NAT kuralları oluşturun:

    ```azurecli
    $inboundNatRuleRdp1 = az network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = az network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. İsteği alan ön uca bağlı olarak, farklı arka uç bağlantı noktalarına trafik gönderen yük dengeleyici kuralları oluşturun.

    ```azurecli
    $lbruleIPv4 = az network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = az network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. Ayarlarınızı denetleyin:

    ```azurecli
    az network lb show --resource-group $rgName --name $lbName
    ```

    Beklenen çıktı:

    ```output
    info:    Executing command network lb show
    info:    Looking up the load balancer "myIPv4IPv6Lb"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
    data:    Name                            : myIPv4IPv6Lb
    data:    Type                            : Microsoft.Network/loadBalancers
    data:    Location                        : southcentralus
    data:    Provisioning state              : Succeeded
    data:
    data:    Frontend IP configurations:
    data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
    data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
    data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
    data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
    data:
    data:    Probes:
    data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
    data:    -------------------  ------------------  --------  ----  ----  --------  -----
    data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
    data:
    data:    Backend Address Pools:
    data:    Name             Provisioning state
    data:    ---------------  ------------------
    data:    BackendPoolIPv4  Succeeded
    data:    BackendPoolIPv6  Succeeded
    data:
    data:    Load Balancing Rules:
    data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
    data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
    data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
    data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
    data:
    data:    Inbound NAT Rules:
    data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
    data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
    data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
    data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
    info:    network lb show
    ```

## <a name="create-nics"></a>NIC’leri oluşturma

NIC 'ler oluşturun ve bunları NAT kuralları, yük dengeleyici kuralları ve araştırmalarla ilişkilendirin.

1. PowerShell değişkenlerini ayarlayın:

    ```powershell
    $nic1Name = "myIPv4IPv6Nic1"
    $nic2Name = "myIPv4IPv6Nic2"
    $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
    $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
    $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
    $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
    $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
    $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"
    ```

2. Her arka uç için bir NIC oluşturun ve bir IPv6 yapılandırması ekleyin:

    ```azurecli
    $nic1 = az network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-address-version "IPv4" --subnet $subnet1Id --lb-address-pools $backendAddressPoolV4Id --lb-inbound-nat-rules $natRule1V4Id
    $nic1IPv6 = az network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-address-version "IPv6" --lb-address-pools $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = az network nic create --name $nic2Name --resource-group $rgname --location $location --private-ip-address-version "IPv4" --subnet $subnet2Id --lb-address-pools $backendAddressPoolV4Id --lb-inbound-nat-rules $natRule2V4Id
    $nic2IPv6 = az network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-address-version "IPv6" --lb-address-pools $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Arka uç VM kaynakları oluşturma ve her NIC 'yi iliştirme

VM 'Ler oluşturmak için bir depolama hesabınız olmalıdır. Yük Dengeleme için VM 'Lerin bir kullanılabilirlik kümesinin üyesi olması gerekir. VM 'Ler oluşturma hakkında daha fazla bilgi için bkz. [PowerShell kullanarak Azure VM oluşturma](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

1. PowerShell değişkenlerini ayarlayın:

    ```powershell
    $availabilitySetName = "myIPv4IPv6AvailabilitySet"
    $vm1Name = "myIPv4IPv6VM1"
    $vm2Name = "myIPv4IPv6VM2"
    $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
    $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
    $imageurn = "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
    $vmUserName = "vmUser"
    $mySecurePassword = "PlainTextPassword*1"
    ```

    > [!WARNING]
    > Bu örnekte, sanal makinelerin Kullanıcı adı ve parolası, şifresiz metin olarak kullanılmaktadır. Bu kimlik bilgilerini düz metin olarak kullandığınızda uygun bir işlem yapın. PowerShell 'de kimlik bilgilerini işlemenin daha güvenli bir yöntemi için, cmdlet 'e bakın [`Get-Credential`](https://technet.microsoft.com/library/hh849815.aspx) .

2. Kullanılabilirlik kümesini oluşturun:

    ```azurecli
    $availabilitySet = az vm availability-set create --name $availabilitySetName --resource-group $rgName --location $location
    ```

3. İlişkili NIC 'ler ile sanal makineler oluşturun:

    ```azurecli
    az vm create --resource-group $rgname --name $vm1Name --image $imageurn --admin-username $vmUserName --admin-password $mySecurePassword --nics $nic1Id --location $location --availability-set $availabilitySetName --size "Standard_A1" 

    az vm create --resource-group $rgname --name $vm2Name --image $imageurn --admin-username $vmUserName --admin-password $mySecurePassword --nics $nic2Id --location $location --availability-set $availabilitySetName --size "Standard_A1" 
    ```


