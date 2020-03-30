---
title: Birden fazla NIC kullanan Azure'da Windows VM'leri oluşturma ve yönetme
description: Azure PowerShell veya Resource Manager şablonlarını kullanarak birden fazla NIC'si olan bir Windows VM'yi nasıl oluşturup yöneteceğimiz öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/26/2017
ms.author: cynthn
ms.openlocfilehash: 20a595e1386a8d33c919ad4ff151d65e30b31eda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249990"
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a>Birden çok NIC'si olan bir Windows sanal makinesi oluşturma ve yönetme
Azure'daki sanal makinelerin (VM'ler) kendilerine birden çok sanal ağ arabirimi kartı (NIC) eklenebilir. Yaygın bir senaryo, ön uç ve arka uç bağlantısı için farklı alt ağlara sahip olmaktır. Bir VM'deki birden çok NIC'i birden çok alt ağla ilişkilendirebilirsiniz, ancak bu alt ağların tümü aynı sanal ağda (vNet) yer almalıdır. Bu makalede, birden çok NIC'si bağlı bir VM oluşturmak için nasıl ayrıntıları. NiC'lerin mevcut bir VM'den nasıl ekleyeceğinizi veya kaldırabileceğinizi de öğrenirsiniz. Farklı [VM boyutları,](sizes.md) farklı sayıda NIC'i destekler, bu nedenle VM'nizi buna göre boyutlandırın.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları *myResourceGroup,* *myVnet*ve *myVM'i*içerir.

 

## <a name="create-a-vm-with-multiple-nics"></a>Birden çok NIC ile VM oluşturma
İlk olarak, bir kaynak grubu oluşturun. Aşağıdaki örnek, *EastUs* konumunda *myResourceGroup* adında bir kaynak grubu oluşturur:

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a>Sanal ağ ve alt ağlar oluşturma
Yaygın bir senaryo, sanal bir ağın iki veya daha fazla alt ağa sahip olmasıdır. Bir alt ağ ön uç trafiği için, diğeri arka uç trafiği için olabilir. Her iki alt ağda da bağlanmak için VM'nizde birden çok NIC kullanırsınız.

1. [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig)ile iki sanal ağ alt ağı tanımlayın. Aşağıdaki örnek, *mySubnetFrontEnd* ve *mySubnetBackEnd*alt ağlarını tanımlar:

    ```powershell
    $mySubnetFrontEnd = New-AzVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork)ile sanal ağınızı ve alt ağlarınızı oluşturun. Aşağıdaki örnek *myVnet*adlı bir sanal ağ oluşturur:

    ```powershell
    $myVnet = New-AzVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a>Birden çok NIC oluşturma
[New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface)ile iki NIC oluşturun. Bir NIC'yi ön uç alt ağına, bir NIC'yi de arka uç alt ağına takın. Aşağıdaki örnek *myNic1* ve *myNic2*adlı NIC'ler oluşturur:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

Genellikle, ağ trafiğini VM'ye filtrelemek için bir [ağ güvenlik grubu](../../virtual-network/security-overview.md) ve trafiği birden çok VM'ye dağıtmak için bir yük [dengeleyicisi](../../load-balancer/load-balancer-overview.md) de oluşturursunuz.

### <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma
Şimdi VM yapılandırmanızı oluşturmaya başlayın. Her VM boyutunun, VM'ye ekleyebileceğiniz toplam NIC sayısı için bir sınırı vardır. Daha fazla bilgi için [Windows VM boyutlarına](sizes.md)bakın.

1. VM kimlik bilgilerinizi `$cred` aşağıdaki gibi değişkene ayarlayın:

    ```powershell
    $cred = Get-Credential
    ```

2. VM'inizi [New-AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig)ile tanımlayın. Aşağıdaki örnek, *myVM* adlı bir VM tanımlar ve ikiden fazla NIC'i *(Standard_DS3_v2)* destekleyen bir VM boyutu kullanır:

    ```powershell
    $vmConfig = New-AzVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. [Set-AzVMOperatingSystem](https://docs.microsoft.com/powershell/module/az.compute/set-azvmoperatingsystem) ve [Set-AzVMSourceImage](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsourceimage)ile VM yapılandırmanızın geri kalanını oluşturun. Aşağıdaki örnekte bir Windows Server 2016 VM oluşturulur:

    ```powershell
    $vmConfig = Set-AzVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. Daha önce [Add-AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface)ile oluşturduğunuz iki NIC'i ekleyin:

    ```powershell
    $vmConfig = Add-AzVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. [Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm)ile VM oluşturun:

    ```powershell
    New-AzVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

6. Birden çok NIC için işletim sistemini yapılandırma adımlarını tamamlayarak işletim sistemine ikincil [NIC'ler için rotalar](#configure-guest-os-for-multiple-nics)ekleyin.

## <a name="add-a-nic-to-an-existing-vm"></a>Varolan bir VM'ye NIC ekleme
Varolan bir VM'ye sanal NIC eklemek için, VM'yi konuma getirmek, sanal NIC'i eklemek ve ardından VM'yi başlatabilirsiniz. Farklı [VM boyutları,](sizes.md) farklı sayıda NIC'i destekler, bu nedenle VM'nizi buna göre boyutlandırın. Gerekirse, bir [VM yeniden boyutlandırabilirsiniz.](resize-vm.md)

1. [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm)ile VM'yi bulun. Aşağıdaki örnek, *myResourceGroup'taki* *myVM* adlı VM'yi yerle bir eder:

    ```powershell
    Stop-AzVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. [Get-AzVm](https://docs.microsoft.com/powershell/module/az.compute/get-azvm)ile VM'nin mevcut yapılandırmasını alın. Aşağıdaki örnek *myResourceGroup* *myVM* adlı VM için bilgi alır:

    ```powershell
    $vm = Get-AzVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Aşağıdaki örnek *mySubnetBackEnd*bağlı *myNic3* adlı [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) ile sanal bir NIC oluşturur. Sanal NIC sonra [Add-AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface)ile *myResourceGroup* *myVM* adlı VM eklenir:

    ```powershell
    # Get info for the back end subnet
    $myVnet = Get-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get the ID of the new virtual NIC and add to VM
    $nicId = (Get-AzNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzVMNetworkInterface -VM $vm -Id $nicId | Update-AzVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a>Birincil sanal NIC'ler
    Çoklu NİşANLI VM'deki NIC'lerden birinin birincil olması gerekir. VM'deki varolan sanal NIC'lerden biri zaten birincil olarak ayarlanmışsa, bu adımı atlayabilirsiniz. Aşağıdaki örnek, iki sanal NIC'nin artık bir VM'de bulunduğunu varsayar`[0]`ve birincil olarak ilk NIC () eklemek istersiniz:
        
    ```powershell
    # List existing NICs on the VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 to be primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update the VM state in Azure
    Update-AzVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. [Start-AzVm](https://docs.microsoft.com/powershell/module/az.compute/start-azvm)ile VM başlatın:

    ```powershell
    Start-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

5. Birden çok NIC için işletim sistemini yapılandırma adımlarını tamamlayarak işletim sistemine ikincil [NIC'ler için rotalar](#configure-guest-os-for-multiple-nics)ekleyin.

## <a name="remove-a-nic-from-an-existing-vm"></a>Varolan bir VM'den NIC kaldırma
Sanal bir NIC'i varolan bir VM'den kaldırmak için, VM'yi konuma getirmek, sanal NIC'i kaldırmak ve ardından VM'yi başlatabilirsiniz.

1. [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm)ile VM'yi bulun. Aşağıdaki örnek, *myResourceGroup'taki* *myVM* adlı VM'yi yerle bir eder:

    ```powershell
    Stop-AzVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. [Get-AzVm](https://docs.microsoft.com/powershell/module/az.compute/get-azvm)ile VM'nin mevcut yapılandırmasını alın. Aşağıdaki örnek *myResourceGroup* *myVM* adlı VM için bilgi alır:

    ```powershell
    $vm = Get-AzVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. [Get-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkinterface)ile NIC kaldırmak hakkında bilgi alın. Aşağıdaki örnek *myNic3*hakkında bilgi alır:

    ```powershell
    # List existing NICs on the VM if you need to determine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. [Kaldır-AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmnetworkinterface) ile NIC kaldırın ve sonra [Update-AzVm](https://docs.microsoft.com/powershell/module/az.compute/update-azvm)ile VM güncelleştirin. Aşağıdaki örnek, önceki `$nicId` adımda elde edilen *myNic3* kaldırır:

    ```powershell
    Remove-AzVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzVm -ResourceGroupName "myResourceGroup"
    ```   

5. [Start-AzVm](https://docs.microsoft.com/powershell/module/az.compute/start-azvm)ile VM başlatın:

    ```powershell
    Start-AzVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a>Şablonlarla birden çok NIC oluşturma
Azure Kaynak Yöneticisi şablonları, dağıtım sırasında birden çok NIC oluşturma gibi birden çok kaynak örneği oluşturmanın bir yolunu sağlar. Kaynak Yöneticisi şablonları, ortamınızı tanımlamak için bildirimsel JSON dosyalarını kullanır. Daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](../../azure-resource-manager/management/overview.md)bölümüne bakın. Oluşturmak için örnek sayısını belirtmek için *kopya* kullanabilirsiniz:

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

Daha fazla bilgi [ *için, kopyayı*kullanarak birden çok örnek oluşturmaya](../../resource-group-create-multiple.md)bakın. 

Bir sayıyı `copyIndex()` kaynak adına eklemek için de kullanabilirsiniz. Daha sonra *myNic1*oluşturabilirsiniz, *MyNic2* ve benzeri. Aşağıdaki kod, dizin değerini ekleyen bir örnek gösterir:

```json
"name": "[concat('myNic', copyIndex())]", 
```

[Kaynak Yöneticisi şablonlarını kullanarak birden çok NIC oluşturmanın](../../virtual-network/template-samples.md)tam bir örneğini okuyabilirsiniz.

Birden çok NIC için işletim sistemini yapılandırma adımlarını tamamlayarak işletim sistemine ikincil [NIC'ler için rotalar](#configure-guest-os-for-multiple-nics)ekleyin.

## <a name="configure-guest-os-for-multiple-nics"></a>Birden çok NIC için konuk işletim sistemi yapılandırma

Azure, sanal makineye bağlı ilk (birincil) ağ arabirimine varsayılan ağ geçidi atar. Azure, bir sanal makineye bağlı ek (ikincil) ağ arabirimlerine varsayılan ağ geçidi atamaz. Bu nedenle varsayılan olarak, alt ağın dışında kalan ve ikincil bir ağ arabirimi içeren kaynaklarla iletişim kurulamaz. Ancak ikincil ağ arabirimleri, iletişimi etkinleştirme adımları farklı işletim sistemleri için farklı olsa da, alt ağları dışındaki kaynaklarla iletişim kurabilir.

1. Windows komut isteminden, `route print` iki bağlı ağ arabirimine sahip sanal bir makine için aşağıdaki çıktıya benzer çıktı döndüren komutu çalıştırın:

    ```
    ===========================================================================
    Interface List
    3...00 0d 3a 10 92 ce ......Microsoft Hyper-V Network Adapter #3
    7...00 0d 3a 10 9b 2a ......Microsoft Hyper-V Network Adapter #4
    ===========================================================================
    ```
 
    Bu örnekte, **Microsoft Hyper-V Ağ Bağdaştırıcısı #4** (arabirim 7), varsayılan ağ geçidi olmayan ikincil ağ arabirimidir.

2. Komut isteminden, ikincil `ipconfig` ağ arabirimine hangi IP adresinin atandığını görmek için komutu çalıştırın. Bu örnekte, 192.168.2.4 arabirimi 7 atanır. İkincil ağ arabirimi için varsayılan ağ geçidi adresi döndürülür.

3. İkincil ağ arabiriminin alt ağı dışındaki adresler için ayrılan tüm trafiği alt ağ için ağ geçidine yönlendirmek için aşağıdaki komutu çalıştırın:

    ```
    route add -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5015 IF 7
    ```

    Alt ağ geçidi adresi, alt ağ için tanımlanan adres aralığındaki ilk IP adresidir (0,1 ile biten). Tüm trafiği alt ağ dışına yönlendirmek istemiyorsanız, bunun yerine belirli hedeflere tek tek rotalar ekleyebilirsiniz. Örneğin, trafiği yalnızca ikincil ağ arabiriminden 192.168.3.0 ağına yönlendirmek istiyorsanız, komutu girersiniz:

      ```
      route add -p 192.168.3.0 MASK 255.255.255.0 192.168.2.1 METRIC 5015 IF 7
      ```
  
4. Örneğin, 192.168.3.0 ağındaki bir kaynakla başarılı iletişimi onaylamak için, arabirim 7 (192.168.2.4) kullanarak 192.168.3.4 ping'i girmek için aşağıdaki komutu girin:

    ```
    ping 192.168.3.4 -S 192.168.2.4
    ```

    Ping yaptığınız aygıtın Windows güvenlik duvarı aracılığıyla ICMP'yi açmanız gerekebilir:
  
      ```
      netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
      ```
  
5. Eklenen rotanın rota tablosunda olduğunu doğrulamak `route print` için, çıktıyı aşağıdaki metne benzer olarak döndüren komutu girin:

    ```
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4     15
              0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.4   5015
    ```

    **Ağ Geçidi**altında *192.168.1.1* ile listelenen rota, birincil ağ arabirimi için varsayılan olarak orada olan yoldur. **Ağ Geçidi**altında *192.168.2.1* ile rota , eklediğiniz rotadır.

## <a name="next-steps"></a>Sonraki adımlar
Birden çok NIC'si olan bir VM oluşturmaya çalışırken [Windows VM boyutlarını](sizes.md) gözden geçirin. Her VM boyutunun desteklediği maksimum NIC sayısına dikkat edin. 


