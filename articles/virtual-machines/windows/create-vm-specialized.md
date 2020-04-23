---
title: Azure'da özel bir VHD'den Windows VM oluşturma
description: Kaynak Yöneticisi dağıtım modelini kullanarak işletim sistemi diski olarak özel yönetilen bir disk ekleyerek yeni bir Windows VM oluşturun.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 10/10/2019
ms.author: cynthn
ms.openlocfilehash: 2939726898abc2abc0e62d0e36feedbfe7ba3645
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82086411"
---
# <a name="create-a-windows-vm-from-a-specialized-disk-by-using-powershell"></a>PowerShell kullanarak özel bir diskten Windows VM oluşturma

İşletim sistemi diski olarak özel yönetilen bir disk ekleyerek yeni bir VM oluşturun. Özelleştirilmiş disk, kullanıcı hesaplarını, uygulamaları ve orijinal VM'nizdeki diğer durum verilerini içeren varolan bir VM'den gelen sanal sabit diskin (VHD) kopyasıdır. 

Yeni bir VM oluşturmak için özel bir VHD kullandığınızda, yeni VM orijinal VM'nin bilgisayar adını korur. Bilgisayara özgü diğer bilgiler de tutulur ve bazı durumlarda bu yinelenen bilgiler sorunlara neden olabilir. Bir VM kopyalarken, uygulamalarınızın bilgisayara özgü bilgilere güvendiğinizde ne tür bilgilere güvendiğinizden haberdar olun.

Birkaç seçeneğiniz vardır:
* [Varolan yönetilen bir disk kullanın.](#option-1-use-an-existing-disk) Bu seçenek, düzgün çalışmayan bir VM'niz varsa yararlıdır. VM'yi silebilir ve ardından yeni bir VM oluşturmak için yönetilen diski yeniden kullanabilirsiniz. 
* [VHD’yi karşıya yükleme](#option-2-upload-a-specialized-vhd) 
* [Anlık görüntüleri kullanarak varolan bir Azure VM'yi kopyalama](#option-3-copy-an-existing-azure-vm)

Azure portalını, özel [bir VHD'den yeni bir VM oluşturmak](create-vm-specialized-portal.md)için de kullanabilirsiniz.

Bu makalede, yönetilen disklerin nasıl kullanılacağı gösterilmektedir. Depolama hesabı kullanmayı gerektiren eski bir dağıtımınız varsa, [bkz.](sa-create-vm-specialized.md)

Eşzamanlı dağıtım sayısını tek bir VHD veya anlık görüntüden 20 VM ile sınırlamanızı öneririz. 

## <a name="option-1-use-an-existing-disk"></a>Seçenek 1: Varolan bir disk kullanma

Sildiğiniz bir VM'niz varsa ve yeni bir VM oluşturmak için işletim sistemi diskini yeniden kullanmak istiyorsanız [Get-AzDisk'i](https://docs.microsoft.com/powershell/module/az.compute/get-azdisk)kullanın.

```powershell
$resourceGroupName = 'myResourceGroup'
$osDiskName = 'myOsDisk'
$osDisk = Get-AzDisk `
-ResourceGroupName $resourceGroupName `
-DiskName $osDiskName
```
Artık bu diski işletim sistemi diski olarak yeni bir [VM'ye](#create-the-new-vm)ekleyebilirsiniz.

## <a name="option-2-upload-a-specialized-vhd"></a>Seçenek 2: Özel bir VHD yükleyin

VHD'yi Hyper-V gibi şirket içi sanallaştırma aracıyla oluşturulan özel bir VM'den veya başka bir buluttan dışa aktarılan bir VM'den yükleyebilirsiniz.

### <a name="prepare-the-vm"></a>VM’yi hazırlama
Yeni bir VM oluşturmak için VHD olarak kullanın. 
  
  * [Azure'a yüklemek için bir Windows VHD hazırlayın.](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Sysprep kullanarak VM genelleme **yapmayın.**
  * VM'de yüklü olan konuk sanallaştırma araçlarını ve aracılarını (VMware araçları gibi) kaldırın.
  * VM'nin IP adresini ve DNS ayarlarını DHCP'den alacak şekilde yapılandırıldığından emin olun. Bu, sunucunun başlatıldığında sanal ağ içinde bir IP adresi almasını sağlar. 


### <a name="upload-the-vhd"></a>VHD'yi yükleyin

Artık yönetilen bir diske doğrudan bir VHD yükleyebilirsiniz. Talimatlar için Azure [PowerShell kullanarak Azure'a VHD Yükle'ye](disks-upload-vhd-to-managed-disk-powershell.md)bakın.

## <a name="option-3-copy-an-existing-azure-vm"></a>Seçenek 3: Varolan bir Azure VM'yi kopyalama

VM'nin anlık görüntüsünü alarak yönetilen diskleri kullanan bir VM'nin kopyasını oluşturabilir ve ardından yeni yönetilen bir disk ve yeni bir VM oluşturmak için bu anlık görüntünün kullanılmasını sağlayabilirsiniz.

Varolan bir VM'yi başka bir bölgeye kopyalamak istiyorsanız, [başka bir bölgedeki bir diskin kopyasını oluşturmak](disks-upload-vhd-to-managed-disk-powershell.md#copy-a-managed-disk)için azcopy kullanmak isteyebilirsiniz. 

### <a name="take-a-snapshot-of-the-os-disk"></a>İşletim sistemi diskinin anlık görüntüsünü alın

Tüm VM'nin (tüm diskler dahil) veya tek bir diskin anlık görüntüsünü alabilirsiniz. Aşağıdaki adımlar, [Yeni-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot) cmdlet ile VM'nizin işletim sistemi diskinin anlık görüntüsünü nasıl çekebilirsiniz gösterir. 

İlk olarak, bazı parametrelerayarlayın. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

VM nesnesini alın.

```powershell
$vm = Get-AzVM -Name $vmName `
   -ResourceGroupName $resourceGroupName
```
İşletim sistemi disk adını alın.

 ```powershell
$disk = Get-AzDisk -ResourceGroupName $resourceGroupName `
   -DiskName $vm.StorageProfile.OsDisk.Name
```

Anlık görüntü yapılandırmasını oluşturun. 

 ```powershell
$snapshotConfig =  New-AzSnapshotConfig `
   -SourceUri $disk.Id `
   -OsType Windows `
   -CreateOption Copy `
   -Location $location 
```

Enstantaneyi al.

```powershell
$snapShot = New-AzSnapshot `
   -Snapshot $snapshotConfig `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName
```


Yüksek performanslı olması gereken bir VM oluşturmak için bu anlık `-AccountType Premium_LRS` görüntü kullanmak için, Yeni-AzSnapshotConfig komutuna parametre ekleyin. Bu parametre anlık görüntü, premium yönetilen disk olarak depolanır şekilde oluşturur. Premium Yönetilen Diskler Standart'tan daha pahalıdır, bu nedenle bu parametreyi kullanmadan önce Premium'a ihtiyacınız olacağından emin olun.

### <a name="create-a-new-disk-from-the-snapshot"></a>Anlık görüntüden yeni bir disk oluşturma

[New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/new-azdisk)kullanarak anlık görüntüden yönetilen bir disk oluşturun. Bu örnekte disk adı için *myOSDisk* kullanır.

Yeni VM için yeni bir kaynak grubu oluşturun.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

İşletim sistemi disk adını ayarlayın. 

```powershell
$osDiskName = 'myOsDisk'
```

Yönetilen diski oluşturun.

```powershell
$osDisk = New-AzDisk -DiskName $osDiskName -Disk `
    (New-AzDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a>Yeni VM'yi oluşturun 

Yeni VM tarafından kullanılacak ağ ve diğer VM kaynaklarını oluşturun.

### <a name="create-the-subnet-and-virtual-network"></a>Alt ağ ve sanal ağ oluşturma

VM için [sanal ağ](../../virtual-network/virtual-networks-overview.md) ve alt ağ oluşturun.

1. Alt ağı oluşturun. Bu örnek, *myDestinationResourceGroup*kaynak grubunda *mySubNet*adında bir alt ağ oluşturur ve alt net adresi önekini *10.0.0.0/24*olarak ayarlar.
   
    ```powershell
    $subnetName = 'mySubNet'
    $singleSubnet = New-AzVirtualNetworkSubnetConfig `
       -Name $subnetName `
       -AddressPrefix 10.0.0.0/24
    ```
    
2. Sanal ağı oluşturun. Bu örnek, sanal ağ adını *myVnetName'e,* *Batı ABD'ye*konum alave sanal ağ için adres önekini *10.0.0.0/16*olarak ayarlar. 
   
    ```powershell
    $vnetName = "myVnetName"
    $vnet = New-AzVirtualNetwork `
       -Name $vnetName -ResourceGroupName $destinationResourceGroup `
       -Location $location `
       -AddressPrefix 10.0.0.0/16 `
       -Subnet $singleSubnet
    ```    
    

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve RDP kuralı oluşturma
Uzak masaüstü protokolü (RDP) ile VM'nizde oturum açabilmek için, 3389 bağlantı noktasında RDP erişimine izin veren bir güvenlik kuralına sahip olmanız gerekir. Örneğimizde, yeni VM için VHD varolan özel bir VM oluşturuldu, böylece RDP için kaynak sanal makinede var olan bir hesabı kullanabilirsiniz.

Bu örnek, ağ güvenlik grubu (NSG) adını *myNsg'ye,* RDP kural adını *myRdpRule'e*ayarlar.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzNetworkSecurityGroup `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -Name $nsgName -SecurityRules $rdpRule
    
```

Uç noktalar ve NSG kuralları hakkında daha fazla bilgi için [PowerShell'i kullanarak Azure'da bir VM bağlantı noktasını açma'ya](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

### <a name="create-a-public-ip-address-and-nic"></a>Ortak bir IP adresi oluşturma ve NIC
Sanal ağdaki sanal makineyle iletişimi etkinleştirmek için ortak bir [IP adresine](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimine ihtiyacınız olacak.

1. Genel IP oluşturun. Bu örnekte, genel IP adresi adı *myIP*olarak ayarlanır.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzPublicIpAddress `
       -Name $ipName -ResourceGroupName $destinationResourceGroup `
       -Location $location `
       -AllocationMethod Dynamic
    ```       
    
2. NIC'yi oluşturun. Bu örnekte, NIC adı *myNicName*olarak ayarlanır.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzNetworkInterface -Name $nicName `
       -ResourceGroupName $destinationResourceGroup `
       -Location $location -SubnetId $vnet.Subnets[0].Id `
       -PublicIpAddressId $pip.Id `
       -NetworkSecurityGroupId $nsg.Id
    ```
    


### <a name="set-the-vm-name-and-size"></a>VM adını ve boyutunu ayarlama

Bu örnek, *MyVM'ye* VM adını ve *VM*boyutunu Standard_A2 olarak ayarlar.

```powershell
$vmName = "myVM"
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>NIC ekle
    
```powershell
$vm = Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a>İşletim sistemi diskini ekleme 

[Set-AzVMOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk)kullanarak yapılandırmaya işletim sistemi diskini ekleyin. Bu örnek, diskin boyutunu *128 GB* olarak ayarlar ve yönetilen diski *Windows* Os diski olarak bağlar.
 
```powershell
$vm = Set-AzVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType Standard_LRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a>VM'yi tamamlayın 

Yeni oluşturduğumuz yapılandırmalarla [Yeni AzVM'yi](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) kullanarak VM'yi oluşturun.

```powershell
New-AzVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Bu komut başarılı olursa, şu şekilde çıktı görürsünüz:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>VM'nin oluşturulduğunu doğrulama
Yeni oluşturulan VM'yi Sanal**makinelere** **Gözat** > altındaki [Azure portalında](https://portal.azure.com) veya aşağıdaki PowerShell komutlarını kullanarak görmeniz gerekir.

```powershell
$vmList = Get-AzVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenizde oturum açın. Daha fazla bilgi için [windows çalıştıran bir Azure sanal makinesine nasıl bağlanıp oturum açarken](connect-logon.md)göreceğiz.

