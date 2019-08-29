---
title: Azure 'da özelleştirilmiş bir VHD 'den Windows sanal makinesi oluşturma | Microsoft Docs
description: Kaynak Yöneticisi dağıtım modelini kullanarak işletim sistemi diski olarak özelleştirilmiş bir yönetilen disk ekleyerek yeni bir Windows sanal makinesi oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 10/10/2018
ms.author: cynthn
ms.openlocfilehash: 5dde098277b16c7ec5339aa6b963b04dd608c8ac
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70079663"
---
# <a name="create-a-windows-vm-from-a-specialized-disk-by-using-powershell"></a>PowerShell kullanarak özel bir diskten Windows sanal makinesi oluşturma

Özel bir yönetilen diski işletim sistemi diski olarak ekleyerek yeni bir VM oluşturun. Özel bir disk, var olan bir VM 'den bir sanal sabit diskin (VHD) bir kopyasıdır ve bu, özgün VM 'nizden Kullanıcı hesaplarını, uygulamaları ve diğer durum verilerini içerir. 

Yeni bir VM oluşturmak için özelleştirilmiş bir VHD kullandığınızda, yeni VM orijinal sanal makinenin bilgisayar adını korur. Bilgisayara özgü diğer bilgiler de tutulur ve bazı durumlarda bu yinelenen bilgiler sorunlara yol açabilir. Bir VM 'yi kopyalarken, uygulamalarınızın kullandığı bilgisayara özgü bilgi türlerini unutmayın.

Birkaç seçeneğiniz vardır:
* [Mevcut bir yönetilen disk kullanın](#option-1-use-an-existing-disk). Doğru şekilde çalışmayan bir sanal makineniz varsa bu seçenek faydalıdır. VM 'yi silebilir ve ardından yeni bir VM oluşturmak için yönetilen diski yeniden kullanabilirsiniz. 
* [Bir VHD’yi karşıya yükleme](#option-2-upload-a-specialized-vhd) 
* [Anlık görüntüler kullanarak var olan bir Azure VM 'yi kopyalama](#option-3-copy-an-existing-azure-vm)

Ayrıca, [özelleştirilmiş BIR VHD 'den yeni BIR VM oluşturmak](create-vm-specialized-portal.md)için Azure Portal de kullanabilirsiniz.

Bu makalede, yönetilen disklerin nasıl kullanılacağı gösterilir. Depolama hesabı kullanılmasını gerektiren eski bir dağıtıma sahipseniz, bkz. [depolama hesabındaki özelleştirilmiş BIR VHD 'den VM oluşturma](sa-create-vm-specialized.md).

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="option-1-use-an-existing-disk"></a>Seçenek 1: Var olan bir diski kullan

Sildiğiniz bir sanal makine varsa ve yeni bir VM oluşturmak için işletim sistemi diskini yeniden kullanmak istiyorsanız [Get-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/get-azdisk)' i kullanın.

```powershell
$resourceGroupName = 'myResourceGroup'
$osDiskName = 'myOsDisk'
$osDisk = Get-AzDisk `
-ResourceGroupName $resourceGroupName `
-DiskName $osDiskName
```
Artık bu diski işletim sistemi diski olarak [Yeni BIR VM](#create-the-new-vm)'ye ekleyebilirsiniz.

## <a name="option-2-upload-a-specialized-vhd"></a>Seçenek 2: Özelleştirilmiş bir VHD 'YI karşıya yükleme

VHD 'yi Hyper-V gibi şirket içi bir sanallaştırma aracıyla oluşturulan özel bir VM 'den veya başka bir buluttan aktarılmış bir VM 'den karşıya yükleyebilirsiniz.

### <a name="prepare-the-vm"></a>VM’yi hazırlama
Yeni bir VM oluşturmak için VHD 'YI olduğu gibi kullanın. 
  
  * [Bir WINDOWS VHD 'Yi Azure 'a yüklemek Için hazırlayın](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Sysprep 'i kullanarak VM 'yi genelleştirmeyin.
  * VM 'de yüklü olan tüm konuk sanallaştırma araçlarını ve aracılarını (VMware araçları gibi) kaldırın.
  * VM 'nin DHCP 'den IP adresi ve DNS ayarlarını almak için yapılandırıldığından emin olun. Bu, sunucunun başladığında sanal ağ içinde bir IP adresi almasını sağlar. 


### <a name="get-the-storage-account"></a>Depolama hesabını al
Karşıya yüklenen VHD 'YI depolamak için Azure 'da bir depolama hesabı gerekir. Var olan bir depolama hesabı kullanabilir veya yeni bir tane oluşturabilirsiniz. 

Kullanılabilir depolama hesaplarını gösterin.

```powershell
Get-AzStorageAccount
```

Mevcut bir depolama hesabını kullanmak için [VHD 'Yi karşıya yükleme](#upload-the-vhd-to-your-storage-account) bölümüne ilerleyin.

Bir depolama hesabı oluşturun.

1. Depolama hesabının oluşturulacağı kaynak grubunun adı gerekir. Get-AzResourceGroup komutunu, aboneliğinizdeki tüm kaynak gruplarını görüntüleyin.
   
    ```powershell
    Get-AzResourceGroup
    ```

    *Batı ABD* bölgesinde *myresourcegroup* adlı bir kaynak grubu oluşturun.

    ```powershell
    New-AzResourceGroup `
       -Name myResourceGroup `
       -Location "West US"
    ```

2. New [-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) cmdlet 'ini kullanarak yeni kaynak grubunda *mystorageaccount* adlı bir depolama hesabı oluşturun.
   
    ```powershell
    New-AzStorageAccount `
       -ResourceGroupName myResourceGroup `
       -Name mystorageaccount `
       -Location "West US" `
       -SkuName "Standard_LRS" `
       -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a>VHD 'YI depolama hesabınıza yükleyin 
VHD 'yi Depolama hesabınızdaki bir kapsayıcıya yüklemek için [Add-AzVhd](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd) cmdlet 'ini kullanın. Bu örnek, *myvhd. vhd* dosyasını "c:\users\public\belgelerim\sanal sabit disklerinden\" myresourcegroup kaynak grubundaki *mystorageaccount* adlı bir depolama hesabına yükler . Dosya *myContainer* adlı kapsayıcıda depolanır ve yeni dosya adı *Myuploadedvhd. vhd*olacaktır.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzVhd -ResourceGroupName $resourceGroupName `
   -Destination $urlOfUploadedVhd `
   -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Komutlar başarılı olursa şuna benzer bir yanıt alırsınız:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Bu komutun tamamlanması, ağ bağlantınıza ve VHD dosyanızın boyutuna bağlı olarak biraz zaman alabilir.

### <a name="create-a-managed-disk-from-the-vhd"></a>VHD 'den yönetilen disk oluşturma

[New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/new-azdisk)' i kullanarak depolama HESABıNıZDAKI özelleştirilmiş VHD 'den yönetilen disk oluşturun. Bu örnek, disk adı için *myOSDisk1* kullanır, diski *Standard_LRS* Storage 'a koyar ve kaynak VHD için *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* URI olarak kullanır.

Yeni VM için yeni bir kaynak grubu oluşturun.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Karşıya yüklenen VHD 'den yeni işletim sistemi diski oluşturun. 

```powershell
$sourceUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'
$osDiskName = 'myOsDisk'
$osDisk = New-AzDisk -DiskName $osDiskName -Disk `
    (New-AzDiskConfig -AccountType Standard_LRS  `
    -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-3-copy-an-existing-azure-vm"></a>Seçenek 3: Mevcut bir Azure VM 'yi kopyalama

VM 'nin bir anlık görüntüsünü alarak yönetilen diskleri kullanan bir VM 'nin kopyasını oluşturabilir ve ardından yeni bir yönetilen disk ve yeni bir VM oluşturmak için bu anlık görüntüyü kullanabilirsiniz.


### <a name="take-a-snapshot-of-the-os-disk"></a>İşletim sistemi diskinin anlık görüntüsünü alın

Tüm VM 'nin (tüm diskler dahil) veya yalnızca tek bir diskin anlık görüntüsünü alabilirsiniz. Aşağıdaki adımlarda, [New-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot) cmdlet 'ı ile sanal makinenizin yalnızca işletim sistemi diskinin anlık görüntüsünü alma işlemleri gösterilmektedir. 

İlk olarak, bazı parametreleri ayarlayın. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

VM nesnesini Al.

```powershell
$vm = Get-AzVM -Name $vmName `
   -ResourceGroupName $resourceGroupName
```
İşletim sistemi diski adını alın.

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

Anlık görüntüyü alın.

```powershell
$snapShot = New-AzSnapshot `
   -Snapshot $snapshotConfig `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName
```


Bu anlık görüntüyü, yüksek performanslı olması gereken bir VM oluşturmak üzere kullanmak için, New-azsnapshotconfig komutuna parametresini `-AccountType Premium_LRS` ekleyin. Bu parametre, anlık görüntüyü, Premium yönetilen disk olarak depolanacak şekilde oluşturur. Premium yönetilen diskler standart 'tan daha pahalıdır, bu nedenle bu parametreyi kullanmadan önce Premium 'A ihtiyacınız olduğundan emin olun.

### <a name="create-a-new-disk-from-the-snapshot"></a>Anlık görüntüden yeni bir disk oluştur

[New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/new-azdisk)kullanarak anlık görüntüden yönetilen disk oluşturun. Bu örnek, disk adı için *Myosdisk* kullanır.

Yeni VM için yeni bir kaynak grubu oluşturun.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

İşletim sistemi diski adını ayarlayın. 

```powershell
$osDiskName = 'myOsDisk'
```

Yönetilen disk oluşturun.

```powershell
$osDisk = New-AzDisk -DiskName $osDiskName -Disk `
    (New-AzDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a>Yeni VM oluşturma 

Yeni VM tarafından kullanılacak ağ ve diğer VM kaynaklarını oluşturun.

### <a name="create-the-subnet-and-virtual-network"></a>Alt ağ ve sanal ağ oluşturma

VM için [sanal ağ](../../virtual-network/virtual-networks-overview.md) ve alt ağ oluşturun.

1. Alt ağı oluşturun. Bu örnekte, *Mydestinationresourcegroup*kaynak grubunda *mysubnet*adlı bir alt ağ oluşturulur ve alt ağ adres öneki *10.0.0.0/24*olarak ayarlanır.
   
    ```powershell
    $subnetName = 'mySubNet'
    $singleSubnet = New-AzVirtualNetworkSubnetConfig `
       -Name $subnetName `
       -AddressPrefix 10.0.0.0/24
    ```
    
2. Sanal ağı oluşturun. Bu örnek, sanal ağ adını *Myvağadı*, *Batı ABD*konumu ve sanal ağın adres önekini *10.0.0.0/16*olarak ayarlar. 
   
    ```powershell
    $vnetName = "myVnetName"
    $vnet = New-AzVirtualNetwork `
       -Name $vnetName -ResourceGroupName $destinationResourceGroup `
       -Location $location `
       -AddressPrefix 10.0.0.0/16 `
       -Subnet $singleSubnet
    ```    
    

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve RDP kuralı oluşturma
SANAL makinenizde Uzak Masaüstü Protokolü (RDP) ile oturum açabilmeniz için, 3389 numaralı bağlantı noktasında RDP erişimine izin veren bir güvenlik kuralına sahip olmanız gerekir. Örneğimizde, yeni VM için VHD, mevcut bir özelleştirilmiş VM 'den oluşturulmuştur, bu nedenle RDP için kaynak sanal makinede bulunan bir hesabı kullanabilirsiniz.

Bu örnek, ağ güvenlik grubu (NSG) adını *Mynsg* olarak ve RDP kuralı adını *Myrdprule*olarak ayarlar.

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

Uç noktalar ve NSG kuralları hakkında daha fazla bilgi için bkz. [PowerShell kullanarak Azure 'DA VM 'ye bağlantı noktalarını açma](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Genel IP adresi ve NIC oluşturma
Sanal ağdaki sanal makineyle iletişimi etkinleştirmek için [Genel BIR IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gerekir.

1. Genel IP 'yi oluşturun. Bu örnekte, genel IP adresi adı *MYIP*olarak ayarlanır.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzPublicIpAddress `
       -Name $ipName -ResourceGroupName $destinationResourceGroup `
       -Location $location `
       -AllocationMethod Dynamic
    ```       
    
2. NIC 'yi oluşturun. Bu örnekte, NIC adı *Mynicname*olarak ayarlanır.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzNetworkInterface -Name $nicName `
       -ResourceGroupName $destinationResourceGroup `
       -Location $location -SubnetId $vnet.Subnets[0].Id `
       -PublicIpAddressId $pip.Id `
       -NetworkSecurityGroupId $nsg.Id
    ```
    


### <a name="set-the-vm-name-and-size"></a>VM adı ve boyutunu ayarla

Bu örnek, VM adını *myvm* ve VM boyutu olarak *Standard_A2*olarak ayarlar.

```powershell
$vmName = "myVM"
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>NIC 'yi ekleme
    
```powershell
$vm = Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a>İşletim sistemi diskini ekleme 

[Set-AzVMOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk)kullanarak işletim sistemi diskini yapılandırmaya ekleyin. Bu örnek, diskin boyutunu *128 GB* olarak ayarlar ve yönetilen diski bir *Windows* işletim sistemi diski olarak ekler.
 
```powershell
$vm = Set-AzVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType Standard_LRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a>VM 'yi doldurun 

Yeni oluşturduğumuz yapılandırmalara sahip [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) kullanarak VM 'yi oluşturun.

```powershell
New-AzVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Bu komut başarılı olursa aşağıdakine benzer bir çıktı görürsünüz:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>VM 'nin oluşturulduğunu doğrulama
Yeni oluşturulan VM 'yi,**sanal makinelere** **gözatada** >  [Azure Portal](https://portal.azure.com) veya aşağıdaki PowerShell komutlarını kullanarak görmeniz gerekir.

```powershell
$vmList = Get-AzVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenizde oturum açın. Daha fazla bilgi için bkz. [Windows çalıştıran bir Azure sanal makinesine bağlanma ve oturum](connect-logon.md)açma.

