---
title: Azure sanal makineleri için birden çok IP adresi - PowerShell | Microsoft Dokümanlar
description: PowerShell'i kullanarak sanal bir makineye birden çok IP adresi atamayı öğrenin. | Kaynak Yöneticisi
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: c44ea62f-7e54-4e3b-81ef-0b132111f1f8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/24/2017
ms.author: kumud
ms.reviewer: annahar
ms.openlocfilehash: a8bd4e4779d94cfc22ac7726c9746fe755764033
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279578"
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-powershell"></a>PowerShell'i kullanarak sanal makinelere birden fazla IP adresi atama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Bu makalede, PowerShell'i kullanarak Azure Kaynak Yöneticisi dağıtım modeli aracılığıyla sanal makine (VM) nasıl oluşturulacak açıklanmaktadır. Klasik dağıtım modeli aracılığıyla oluşturulan kaynaklara birden çok IP adresi atanamaz. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için [dağıtım modellerini anlayın](../resource-manager-deployment-model.md) makalesini okuyun.

[!INCLUDE [virtual-network-multiple-ip-addresses-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name="create-a-vm-with-multiple-ip-addresses"></a><a name = "create"></a>Birden çok IP adresi ile VM oluşturma

İzleyen adımlar, senaryoda açıklandığı gibi, birden çok IP adresine sahip bir örnek VM'nin nasıl oluşturulabildiğini açıklar. Uygulamanız için gereken değişken değerlerini değiştirin.

1. Bir PowerShell komut istemi açın ve bu bölümdeki kalan adımları tek bir PowerShell oturumu içinde tamamlayın. PowerShell'in yüklü ve yapılandırılmış olması yoksa, Azure PowerShell makalesini [yükleme ve yapılandırma adımlarını](/powershell/azure/overview) tamamlayın.
2. `Connect-AzAccount` Komutile hesabınıza giriş yapın.
3. *MyResourceGroup* ve *westus'u* seçtiğiniz bir ad ve konumla değiştirin. Bir kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

   ```powershell
   $RgName   = "MyResourceGroup"
   $Location = "westus"

   New-AzResourceGroup `
   -Name $RgName `
   -Location $Location
   ```

4. Kaynak grubuyla aynı konumda sanal ağ (VNet) ve alt ağ oluşturun:

   ```powershell

   # Create a subnet configuration
   $SubnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name MySubnet `
   -AddressPrefix 10.0.0.0/24

   # Create a virtual network
   $VNet = New-AzVirtualNetwork `
   -ResourceGroupName $RgName `
   -Location $Location `
   -Name MyVNet `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $subnetConfig

   # Get the subnet object
   $Subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
   ```

5. Bir ağ güvenlik grubu (NSG) ve bir kural oluşturun. NSG, gelen ve giden kuralları kullanarak VM'yi güvence altına alar. Bu durumda, bağlantı noktası 3389 için gelen masaüstü bağlantılarına izin veren bir gelen kuralı oluşturulur.

    ```powershell
    
    # Create an inbound network security group rule for port 3389

    $NSGRule = New-AzNetworkSecurityRuleConfig `
    -Name MyNsgRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow
    
    # Create a network security group
    $NSG = New-AzNetworkSecurityGroup `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyNetworkSecurityGroup `
    -SecurityRules $NSGRule
    ```

6. NIC için birincil IP yapılandırmasını tanımlayın. 10.0.0.4'ü oluşturduğunuz alt ağdaki geçerli bir adrese değiştirin, daha önce tanımlanan değeri kullanmadıysanız. Statik bir IP adresi atamadan önce, öncelikle kullanımda olmadığını onaylamanız önerilir. Komutu `Test-AzPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`girin. Adres varsa, çıktı *True*döndürür. Kullanılamıyorsa, çıktı *False'u* ve kullanılabilen adreslerin listesini döndürür. 

    Aşağıdaki komutlarda, **kullanılacak \<benzersiz DNS adı ile> değiştirin.** Ad, bir Azure bölgesindeki tüm genel IP adreslerinde benzersiz olmalıdır. Bu isteğe bağlı bir parametredir. Yalnızca ortak IP adresini kullanarak VM'ye bağlanmak istiyorsanız kaldırılabilir.

    ```powershell
    
    # Create a public IP address
    $PublicIP1 = New-AzPublicIpAddress `
    -Name "MyPublicIP1" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -DomainNameLabel <replace-with-your-unique-name> `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP address to it
    $IpConfigName1 = "IPConfig-1"
    $IpConfig1     = New-AzNetworkInterfaceIpConfig `
    -Name $IpConfigName1 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.4 `
    -PublicIpAddress $PublicIP1 `
    -Primary
    ```

    Bir NIC'e birden çok IP yapılandırması atadığınızda, bir yapılandırma *-Birincil*olarak atanmalıdır.

    > [!NOTE]
    > Genel IP adreslerinin nominal ücreti vardır. IP adresi fiyatlandırması hakkında daha fazla bilgi edinmek için [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfasını okuyun. Abonelikte kullanılabilecek genel IP adreslerinin sayısının bir sınırı vardır. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits) makalesini okuyun.

7. NIC için ikincil IP yapılandırmalarını tanımlayın. Gerektiğinde yapılandırmalar ekleyebilir veya kaldırabilirsiniz. Her IP yapılandırması özel bir IP adresi atanmış olmalıdır. Her yapılandırma isteğe bağlı olarak bir genel IP adresi atanmış olabilir.

    ```powershell
    
    # Create a public IP address
    $PublicIP2 = New-AzPublicIpAddress `
    -Name "MyPublicIP2" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP address to it
    $IpConfigName2 = "IPConfig-2"
    $IpConfig2     = New-AzNetworkInterfaceIpConfig `
    -Name $IpConfigName2 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.5 `
    -PublicIpAddress $PublicIP2
        
    $IpConfigName3 = "IpConfig-3"
    $IpConfig3 = New-AzNetworkInterfaceIpConfig `
    -Name $IPConfigName3 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.6
    ```

8. NIC'i oluşturun ve üç IP yapılandırmasını ilişkilendirin:

   ```powershell
   $NIC = New-AzNetworkInterface `
   -Name MyNIC `
   -ResourceGroupName $RgName `
   -Location $Location `
   -NetworkSecurityGroupId $NSG.Id `
   -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
   ```

   >[!NOTE]
   >Bu makalede tüm yapılandırmalar bir NIC'e atanmış olsa da, VM'ye bağlı her NIC'ye birden çok IP yapılandırması atayabilirsiniz. Birden çok NIC'li bir VM nasıl oluşturulacak öğrenmek için, [birden çok NIC'li VM](../virtual-machines/windows/multiple-nics.md) makalesini okuyun.

9. Aşağıdaki komutları girerek VM oluşturun:

    ```powershell
    
    # Define a credential object. When you run these commands, you're prompted to enter a username and password for the VM you're creating.
    $cred = Get-Credential
    
    # Create a virtual machine configuration
    $VmConfig = New-AzVMConfig `
    -VMName MyVM `
    -VMSize Standard_DS1_v2 | `
    Set-AzVMOperatingSystem -Windows `
    -ComputerName MyVM `
    -Credential $cred | `
    Set-AzVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
    Add-AzVMNetworkInterface `
    -Id $NIC.Id
    
    # Create the VM
    New-AzVM `
    -ResourceGroupName $RgName `
    -Location $Location `
    -VM $VmConfig
    ```

10. Bu makalenin Bir VM işletim sistemi bölümüne IP adresleri ekle işletim sisteminizin adımlarını tamamlayarak özel IP adreslerini [VM işletim sistemine](#os-config) ekleyin. Ortak IP adreslerini işletim sistemine eklemeyin.

## <a name="add-ip-addresses-to-a-vm"></a><a name="add"></a>VM'ye IP adresleri ekleme

Aşağıdaki adımları tamamlayarak Azure ağ arabirimine özel ve genel IP adresleri ekleyebilirsiniz. Aşağıdaki bölümlerdeki örnekler, bu makalede [senaryoda](#scenario) açıklanan üç IP yapılandırması ile zaten bir VM'niz olduğunu varsayar, ancak bunu yapmanız gerekmez.

1. Bir PowerShell komut istemi açın ve bu bölümdeki kalan adımları tek bir PowerShell oturumu içinde tamamlayın. PowerShell'in yüklü ve yapılandırılmış olması yoksa, Azure PowerShell makalesini [yükleme ve yapılandırma adımlarını](/powershell/azure/overview) tamamlayın.
2. Aşağıdaki $Variables "değerlerini" IP adresi eklemek istediğiniz NIC'nin adıyla ve NIC'nin var olduğu kaynak grubu ve konumuyla değiştirin:

   ```powershell
   $NicName  = "MyNIC"
   $RgName   = "MyResourceGroup"
   $Location = "westus"
   ```

   Değiştirmek istediğiniz NIC'nin adını bilmiyorsanız, aşağıdaki komutları girin ve önceki değişkenlerin değerlerini değiştirin:

   ```powershell
   Get-AzNetworkInterface | Format-Table Name, ResourceGroupName, Location
   ```

3. Bir değişken oluşturun ve aşağıdaki komutu yazarak varolan NIC'ye ayarlayın:

   ```powershell
   $MyNIC = Get-AzNetworkInterface -Name $NicName -ResourceGroupName $RgName
   ```

4. Aşağıdaki komutlarda *MyVNet* ve *MySubnet'i* NIC'nin bağlı olduğu VNet ve alt ağ adlarıyla değiştirin. NIC'nin bağlı olduğu VNet ve alt ağ nesnelerini almak için komutları girin:

   ```powershell
   $MyVNet = Get-AzVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
   $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
   ```

   NIC'nin bağlı olduğu VNet veya alt ağ adını bilmiyorsanız aşağıdaki komutu girin:

   ```powershell
   $MyNIC.IpConfigurations
   ```

   Çıktıda, aşağıdaki örnek çıktıya benzer bir metin arayın:

   ```
   "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
   ```

    Bu çıktıda, *MyVnet* VNet ve *MySubnet* NIC bağlı alt ağdır.

5. Gereksinimlerinize bağlı olarak aşağıdaki bölümlerden birinde adımları tamamlayın:

   **Özel IP adresi ekleme**

   NIC'e özel bir IP adresi eklemek için bir IP yapılandırması oluşturmanız gerekir. Aşağıdaki komut, statik IP adresi 10.0.0.7 olan bir yapılandırma oluşturur. Statik bir IP adresi belirtirken, alt ağ için kullanılmayan bir adres olmalıdır. Komutu girerek `Test-AzPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` kullanılabilir olduğundan emin olmak için önce adresi test etmeniz önerilir. IP adresi varsa, çıktı *True*döndürür. Kullanılamıyorsa, çıktı *False'u*ve kullanılabilen adreslerin listesini döndürür.

   ```powershell
   Add-AzNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
   $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
   ```

   Benzersiz yapılandırma adlarını ve özel IP adreslerini kullanarak (statik IP adreslerine sahip yapılandırmalar için) istediğiniz kadar yapılandırma oluşturun.

   Bu makalenin Bir VM işletim sistemi bölümüne IP adresleri ekle işletim sisteminizin adımlarını tamamlayarak Özel IP adresini [VM işletim sistemine](#os-config) ekleyin.

   **Herkese açık bir IP adresi ekleme**

   Ortak BIR IP adresi, ortak bir IP adresi kaynağını yeni bir IP yapılandırmasına veya varolan bir IP yapılandırmasına ilişkilendirerek eklenir. Aşağıdaki bölümlerden birinde, istediğiniz gibi adımları tamamlayın.

   > [!NOTE]
   > Genel IP adreslerinin nominal ücreti vardır. IP adresi fiyatlandırması hakkında daha fazla bilgi edinmek için [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfasını okuyun. Abonelikte kullanılabilecek genel IP adreslerinin sayısının bir sınırı vardır. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits) makalesini okuyun.
   >

   **Ortak IP adresi kaynağını yeni bir IP yapılandırması ile ilişkilendirme**

   Yeni bir IP yapılandırmasında herkese açık bir IP adresi eklediğinizde, tüm IP yapılandırmalarının özel bir IP adresi olması gerektiğinden, özel bir IP adresi de eklemeniz gerekir. Varolan bir genel IP adresi kaynağı ekleyebilir veya yeni bir tane oluşturabilirsiniz. Yeni bir komut oluşturmak için aşağıdaki komutu girin:

   ```powershell
   $myPublicIp3 = New-AzPublicIpAddress `
   -Name "myPublicIp3" `
   -ResourceGroupName $RgName `
   -Location $Location `
   -AllocationMethod Static
   ```

   Statik özel IP adresi ve ilişkili *myPublicIp3* genel IP adresi kaynağı yla yeni bir IP yapılandırması oluşturmak için aşağıdaki komutu girin:

   ```powershell
   Add-AzNetworkInterfaceIpConfig `
   -Name IPConfig-4 `
   -NetworkInterface $myNIC `
   -Subnet $Subnet `
   -PrivateIpAddress 10.0.0.7 `
   -PublicIpAddress $myPublicIp3
   ```

   **Ortak IP adresi kaynağını varolan bir IP yapılandırması ile ilişkilendirme**

   Ortak BIR IP adresi kaynağı yalnızca zaten ilişkili olmayan bir IP yapılandırması ile ilişkilendirilebilir. Aşağıdaki komutu girerek bir IP yapılandırmasının ilişkili bir genel IP adresine sahip olup olmadığını belirleyebilirsiniz:

   ```powershell
   $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
   ```

   Aşağıdakine benzer bir çıktı görürsünüz:

   ```
   Name       PrivateIpAddress PublicIpAddress                                           Primary

   IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
   IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
   IpConfig-3 10.0.0.6                                                                     False
   ```

   *IpConfig-3* için **PublicIpAddress** sütunu boş olduğundan, şu anda ortak IP adresi kaynağı ilişkili değildir. IpConfig-3'e varolan bir genel IP adresi kaynağı ekleyebilir veya aşağıdaki komutu girerek bir tane oluşturabilirsiniz:

   ```powershell
   $MyPublicIp3 = New-AzPublicIpAddress `
   -Name "MyPublicIp3" `
   -ResourceGroupName $RgName `
   -Location $Location -AllocationMethod Static
   ```

   Genel IP adresi kaynağını *IpConfig-3*adlı mevcut IP yapılandırmasına ilişkilendirmek için aşağıdaki komutu girin:

   ```powershell
   Set-AzNetworkInterfaceIpConfig `
   -Name IpConfig-3 `
   -NetworkInterface $mynic `
   -Subnet $Subnet `
   -PublicIpAddress $myPublicIp3
   ```

6. Aşağıdaki komutu girerek NIC'yi yeni IP yapılandırmasıyla ayarlayın:

   ```powershell
   Set-AzNetworkInterface -NetworkInterface $MyNIC
   ```

7. Aşağıdaki komutu girerek NIC'e atanan özel IP adreslerini ve genel IP adresi kaynaklarını görüntüleyin:

   ```powershell
   $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
   ```

8. Bu makalenin Bir VM işletim sistemi bölümüne IP adresleri ekle işletim sisteminizin adımlarını tamamlayarak Özel IP adresini [VM işletim sistemine](#os-config) ekleyin. İşletim sistemine genel IP adresini eklemeyin.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]