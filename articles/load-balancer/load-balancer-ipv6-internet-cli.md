---
title: IPv6 ile ortak yük dengeleyicisi oluşturma - Azure CLI
titleSuffix: Azure Load Balancer
description: Bu öğrenme yolu ile Azure CLI'yi kullanarak IPv6 ile ortak yük dengeleyici oluşturmaya başlayın.
services: load-balancer
documentationcenter: na
author: asudbring
keywords: ipv6, azure yük dengeleyici, çift yığın, genel ip, yerli ipv6, mobil, iot
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/25/2018
ms.author: allensu
ms.openlocfilehash: bff6a7ca6eb1a6859ec25d488f564c66946a780b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76045415"
---
# <a name="create-a-public-load-balancer-with-ipv6-using-azure-cli"></a>Azure CLI'yi kullanarak IPv6 ile ortak yük dengeleyicisi oluşturma

>[!NOTE] 
>Bu makalede, Temel Yük Dengeleyicileri hem IPv4 ve IPv6 bağlantısı sağlamak için izin vermek için bir giriş IPv6 özelliği açıklanır. IPv6 bağlantılarını Sanal Ağlarınızla birleştiren ve IPv6 Ağ Güvenlik Grubu kuralları, IPv6 Kullanıcı tanımlı yönlendirme, IPv6 Temel ve Standart yük dengeleme ve daha fazlası gibi temel özellikleri içeren [Azure VNET'ler için](../virtual-network/ipv6-overview.md) IPv6 ile kapsamlı IPv6 bağlantısı artık kullanılabilir.  Azure VNET'ler için IPv6, Azure'daki IPv6 uygulamaları için önerilen standarttır. [Azure VNET Powershell Dağıtımı için IPv6'ya](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md) bakın 

Azure Load Balancer bir Katman 4 (TCP, UDP) yük dengeleyicidir. Yük dengeleyicileri, gelen trafiği bulut hizmetlerinde veya sanal makinelerde bir yük dengeleyici kümesinde sağlıklı hizmet örnekleri arasında dağıtarak yüksek kullanılabilirlik sağlar. Yük dengeleyicileri bu hizmetleri birden çok bağlantı noktası veya birden çok IP adresi veya her ikisi üzerinde de sunabilir.

## <a name="example-deployment-scenario"></a>Örnek dağıtım senaryosu

Aşağıdaki diyagram, bu makalede açıklanan örnek şablonu kullanılarak dağıtılan yük dengeleme çözümlerini göstermektedir.

![Yük dengeleyici senaryosu](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

Bu senaryoda, aşağıdaki Azure kaynaklarını oluşturursunuz:

* İki sanal makine (VM)
* Hem IPv4 hem de IPv6 adresleri atanmış her VM için sanal ağ arabirimi
* IPv4 ve IPv6 genel IP adresi ne sahip bir genel yük dengeleyicisi
* İki VM içeren bir kullanılabilirlik kümesi
* Kamu VIP'lerini özel uç noktalarıyla eşlemek için iki yük dengeleme kuralı

## <a name="deploy-the-solution-by-using-azure-cli"></a>Azure CLI'yi kullanarak çözümü dağıtma

Aşağıdaki adımlar, Azure CLI'yi kullanarak ortak yük dengeleyicisinin nasıl oluşturulacağımı gösterir. CLI'yi kullanarak, her nesneyi ayrı ayrı oluşturur ve yapılandırır sınız ve sonra bunları bir kaynak oluşturmak için bir araya getirirsiniz.

Bir yük dengeleyicisi dağıtmak için aşağıdaki nesneleri oluşturun ve yapılandırın:

* **Ön uç IP yapılandırması**: Gelen ağ trafiği için ortak IP adreslerini içerir.
* **Arka uç adres havuzu**: Sanal makinelerin yük bakiyesinden ağ trafiği alması için ağ arabirimleri (NIC' ler) içerir.
* **Yük dengeleme kuralları**: Yük dengeleyicisindeki ortak bağlantı noktasını arka uç adres havuzundaki bir bağlantı noktasına eşleyen kuralları içerir.
* **Gelen NAT kuralları**: Arka uç adres havuzunda belirli bir sanal makine için yük dengeleyicisindeki ortak bağlantı noktasını bir bağlantı noktasına eşleyen ağ adresi çevirisi (NAT) kurallarını içerir.
* **Problar**: Arka uç adres havuzundaki sanal makine örneklerinin kullanılabilirliğini denetlemek için kullanılan sistem sondalarını içerir.

## <a name="set-up-azure-cli"></a>Azure CLI’yı ayarlama

Bu örnekte, PowerShell komut penceresinde Azure CLI araçlarını çalıştırın. Okunabilirliği ve yeniden kullanımı artırmak için Azure PowerShell cmdlets'i değil, PowerShell'in komut dosyası özelliklerini kullanırsınız.

1. Bağlı makaledeki adımları izleyerek [Azure CLI'yi yükleyin ve yapılandırın](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve Azure hesabınızda oturum açın.

2. Azure CLI komutlarıyla kullanılmak üzere PowerShell değişkenlerini ayarlayın:

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

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Kaynak grubu, yük dengeleyicisi, sanal ağ ve alt ağlar oluşturma

1. Kaynak grubu oluşturun:

    ```azurecli
    az group create --name $rgName --location $location
    ```

2. Yük dengeleyicisi oluşturun:

    ```azurecli
    $lb = az network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. Sanal ağ oluşturma:

    ```azurecli
    $vnet = az network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

4. Bu sanal ağda, iki alt ağ oluşturun:

    ```azurecli
    $subnet1 = az network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = az network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Ön uç havuzu için ortak IP adresleri oluşturma

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
    > Yük dengeleyicisi, tam nitelikli etki alanı adı (FQDN) olarak genel IP'nin etki alanı etiketini kullanır. Bu, yük dengeleyicisi FQDN olarak bulut hizmet adını kullanan klasik dağıtımdan bir değişiklik.
    >
    > Bu örnekte, FQDN *contoso09152016.southcentralus.cloudapp.azure.com.*

## <a name="create-front-end-and-back-end-pools"></a>Ön ve arka uç havuzları oluşturma

Bu bölümde, aşağıdaki IP havuzlarını oluşturursunuz:
* Yük bakiyesi üzerinde gelen ağ trafiğini alan ön uç IP havuzu.
* Ön uç havuzunun yük dengeli ağ trafiğini gönderdiği arka uç IP havuzu.

1. PowerShell değişkenlerini ayarlayın:

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. Bir ön uç IP havuzu oluşturun ve önceki adımda oluşturduğunuz genel IP ve yük dengeleyicisi ile ilişkilendirin.

    ```azurecli
    $frontendV4 = az network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-address $publicIpv4Name --lb-name $lbName
    $frontendV6 = az network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-address $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = az network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = az network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-the-probe-nat-rules-and-load-balancer-rules"></a>Sonda, NAT kuralları ve yük dengeleyici kuralları oluşturma

Bu örnek aşağıdaki nesneleri oluşturur:

* TCP bağlantı noktası 80'e bağlantı olup olmadığını denetlemek için bir sonda kuralı.
* RdP için 3389 portundaki tüm gelen trafiği 3389 bağlantı noktasına çevirmek için bir NAT kuralı.\*
* Uzak masaüstü protokolü (RDP) için 3391 bağlantı noktasındaki tüm gelen trafiği bağlantı noktası 3389'a çevirmek için bir NAT kuralı.\*
* Arka uç havuzundaki adreslerde 80 portundaki tüm gelen trafiği dengelemek için bir yük dengeleyici kuralı.

\*NAT kuralları, yük dengeleyicisinin arkasındaki belirli bir sanal makine örneğiyle ilişkilidir. 3389 portuna gelen ağ trafiği, NAT kuralıyla ilişkili belirli sanal makine ye ve bağlantı noktasına gönderilir. NAT kuralı için bir protokol (UDP veya TCP) belirtmeniz gerekir. Her iki protokolü de aynı bağlantı noktasına atayamazsınız.

1. PowerShell değişkenlerini ayarlayın:

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. Sondayı oluşturun.

    Aşağıdaki örnek, arka uç TCP bağlantı noktasına bağlantı için her 15 saniyede 80'i kontrol eden bir TCP sondası oluşturur. Art arda iki hatadan sonra, arka uç kaynağı kullanılamaz olarak işaretler.

    ```azurecli
    $probeV4V6 = az network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --threshold 2 --lb-name $lbName
    ```

3. RDP bağlantılarının arka uç kaynaklarına bağlanmasına izin veren gelen NAT kuralları oluşturun:

    ```azurecli
    $inboundNatRuleRdp1 = az network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = az network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. İsteği alan ön uca bağlı olarak trafiği farklı arka uç bağlantı noktalarına gönderen yük dengeleyici kuralları oluşturun.

    ```azurecli
    $lbruleIPv4 = az network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = az network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. Ayarlarınızı kontrol edin:

    ```azurecli
    az network lb show --resource-group $rgName --name $lbName
    ```

    Beklenen çıktı:

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

## <a name="create-nics"></a>NIC’leri oluşturma

NIC'ler oluşturun ve bunları NAT kuralları, yük dengeleyici kuralları ve sondalarla ilişkilendirin.

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

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Arka uç VM kaynaklarını oluşturun ve her NIC

VM'ler oluşturmak için bir depolama hesabınız olması gerekir. Yük dengeleme için, VM'lerin bir kullanılabilirlik kümesinin üyeleri olması gerekir. VM oluşturma hakkında daha fazla bilgi için [PowerShell'i kullanarak Azure VM oluştur'a](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)bakın.

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
    > Bu örnek, VM'lerin kullanıcı adını ve parolasını açık metin olarak kullanır. Bu kimlik bilgilerini açık metin olarak kullandığınızda uygun özeni göz önünde n için kullanın. PowerShell'de kimlik bilgilerini işlemenin daha [`Get-Credential`](https://technet.microsoft.com/library/hh849815.aspx) güvenli bir yöntemi için cmdlet'e bakın.

2. Kullanılabilirlik kümesini oluşturun:

    ```azurecli
    $availabilitySet = az vm availability-set create --name $availabilitySetName --resource-group $rgName --location $location
    ```

3. İlişkili NIC'lerle sanal makineler oluşturun:

    ```azurecli
    az vm create --resource-group $rgname --name $vm1Name --image $imageurn --admin-username $vmUserName --admin-password $mySecurePassword --nics $nic1Id --location $location --availability-set $availabilitySetName --size "Standard_A1" 

    az vm create --resource-group $rgname --name $vm2Name --image $imageurn --admin-username $vmUserName --admin-password $mySecurePassword --nics $nic2Id --location $location --availability-set $availabilitySetName --size "Standard_A1" 
    ```


