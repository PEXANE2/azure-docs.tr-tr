---
title: Azure'da birden çok VM oluşturmak için genelleme vhd yükleme
description: Kaynak Yöneticisi dağıtım modelinde kullanılacak bir Windows VM oluşturmak için genelleştirilmiş bir VHD'yi Azure depolama hesabına yükleyin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/18/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 7a5aa05a9045548e15aba667fdcdbd14fc8990e6
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460316"
---
# <a name="upload-a-generalized-vhd-to-azure-to-create-a-new-vm"></a>Yeni bir VM oluşturmak için Azure'a genelleştirilmiş bir VHD yükleme

Bu konu, genelleştirilmiş yönetilmeyen bir diski depolama hesabına yüklemeyi ve yüklenen diski kullanarak yeni bir VM oluşturmayı kapsar. Genelleştirilmiş bir VHD görüntüsü, Sysprep kullanılarak tüm kişisel hesap bilgilerinizin kaldırılmıştır. 

Bir depolama hesabında özel bir VHD'den bir VM oluşturmak istiyorsanız, [bkz.](sa-create-vm-specialized.md)

Bu konu depolama hesaplarını kullanmayı kapsar, ancak müşterilerin bunun yerine Yönetilen Diskleri kullanmaya geçmelerini öneririz. Yönetilen diskleri kullanarak yeni bir VM'yi nasıl hazırlayacağı, yükleyip oluşturlayacağınıza ilgili tam [bir](upload-generalized-managed.md)gözden geçirme için bkz.

 

## <a name="prepare-the-vm"></a>VM’yi hazırlama

Genelleştirilmiş bir VHD, Sysprep kullanarak tüm kişisel hesap bilgilerinizin silindiğini göstermiştir. VHD'yi yeni VM'ler oluşturmak için bir görüntü olarak kullanmayı planlıyorsanız, şunları yapmalısınız:
  
  * [Azure'a yüklemek için bir Windows VHD hazırlayın.](prepare-for-upload-vhd-image.md) 
  * Sysprep kullanarak sanal makineyi genelleştirin

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Sysprep kullanarak Bir Windows sanal makine genelleme
Bu bölümde, Windows sanal makinenizi görüntü olarak kullanılmak üzere nasıl genelleştirilebildiğiniz gösterilmektedir. Sysprep diğer öğelerin yanı sıra tüm kişisel hesap bilgilerinizi kaldırır ve makineyi bir görüntü olarak kullanılacak şekilde hazırlar. Sysprep hakkındaki ayrıntılar için bkz. [Sysprep İşlemini Kullanma: Giriş](https://technet.microsoft.com/library/bb457073.aspx).

Makinede çalışan sunucu rollerinin Sysprep tarafından desteklendirildiklerinden emin olun. Daha fazla bilgi [için Sunucu Rolleri için Sysprep Desteği'ne](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) bakın

> [!IMPORTANT]
> VHD'nizi Azure'a ilk kez yüklemeden önce Sysprep çalıştırıyorsanız, Sysprep'i çalıştırmadan önce [VM'inizi hazırladığınızdan](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) emin olun. 
> 
> 

1. Windows sanal makinesinde oturum açın.
2. Yönetici olarak Komut İstemi penceresini açın. Dizin **değiştirin %windir%\system32\sysprep**, `sysprep.exe`ve sonra çalıştırın .
3. **Sistem Hazırlama Aracı** iletişim kutusunda ** Sistem İlk Çalıştırma Deneyimi (OOBE) Moduna Gir**'i seçin ve **Genelleştir** onay kutusunun seçili olduğundan emin olun.
4. **Kapatma Seçenekleri'nde** **Kapatma'yı**seçin.
5. **Tamam**'a tıklayın.
   
    ![Sysprep'i Başlat](./media/upload-generalized-managed/sysprepgeneral.png)
6. Sysprep tamamlandığında, sanal makineyi kapatır. 

> [!IMPORTANT]
> VHD'yi Azure'a yüklemeyi veya VM'den bir resim oluşturmayı bitirene kadar VM'yi yeniden başlatmayın. VM yanlışlıkla yeniden başlatılırsa, yeniden genelleştirmek için Sysprep çalıştırın.
> 
> 


## <a name="upload-the-vhd"></a>VHD'yi yükleyin

VHD'yi bir Azure depolama hesabına yükleyin.

### <a name="log-in-to-azure"></a>Azure'da oturum açma
PowerShell sürüm 1.4 veya üzeri yüklü değilseniz, [Azure PowerShell'i nasıl yükleyip yapılandırabileceğinizi](/powershell/azure/overview)okuyun.

1. Azure PowerShell'i açın ve Azure hesabınızda oturum açın. Azure hesap kimlik bilgilerinizi girmeniz için açılır pencere açılır.
   
    ```powershell
    Connect-AzAccount
    ```
2. Mevcut abonelikleriniz için abonelik tünalarını alın.
   
    ```powershell
    Get-AzSubscription
    ```
3. Abonelik kimliğini kullanarak doğru aboneliği ayarlayın. Doğru `<subscriptionID>` aboneliğin kimliğiyle değiştirin.
   
    ```powershell
    Select-AzSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-the-storage-account"></a>Depolama hesabını alma
Yüklenen VM görüntüsünü depolamak için Azure'da bir depolama hesabına ihtiyacınız vardır. Varolan bir depolama hesabı kullanabilir veya yeni bir depo oluşturabilirsiniz. 

Kullanılabilir depolama hesaplarını göstermek için şunları yazın:

```powershell
Get-AzStorageAccount
```

Varolan bir depolama hesabı kullanmak istiyorsanız, VM resim yükleme bölümüne gidin.

Bir depolama hesabı oluşturmanız gerekiyorsa, aşağıdaki adımları izleyin:

1. Depolama hesabının oluşturulması gereken kaynak grubunun adına ihtiyacınız var. Aboneliğinizde bulunan tüm kaynak gruplarını bulmak için şunları yazın:
   
    ```powershell
    Get-AzResourceGroup
    ```

    **Batı ABD** bölgesinde **myResourceGroup** adında bir kaynak grubu oluşturmak için şunları yazın:

    ```powershell
    New-AzResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. [Yeni Depolama Hesabı](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) cmdlet'ini kullanarak bu kaynak grubunda **mystorageaccount** adında bir depolama hesabı oluşturun:
   
    ```powershell
    New-AzStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-the-upload"></a>Yüklemeyi başlatın 

Görüntüyü depolama hesabınızdaki bir kapsayıcıya yüklemek için [Add-AzVhd](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd) cmdlet'i kullanın. Bu örnek, **myVHD.vhd** dosyasını `"C:\Users\Public\Documents\Virtual hard disks\"` **myResourceGroup** kaynak grubunda **mystorageaccount** adlı bir depolama hesabına yükler. Dosya **mycontainer** adlı konteyner içine yerleştirilir ve yeni dosya adı **myUploadedVHD.vhd**olacaktır.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Başarılı olursa, buna benzer bir yanıt alırsınız:

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

Ağ bağlantınıza ve VHD dosyanızın boyutuna bağlı olarak, bu komutun tamamlanması biraz zaman alabilir.


## <a name="create-a-new-vm"></a>Yeni bir VM oluşturun 

Artık yüklenen VHD'yi yeni bir VM oluşturmak için kullanabilirsiniz. 

### <a name="set-the-uri-of-the-vhd"></a>VHD'nin URI'sini ayarlayın

VHD kullanmak için URI biçimindedir:**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName .vhd**https://. Bu örnekte **myVHD adlı VHD,** **mycontainer'taki** **depolama hesabımystorage hesabındadır.**

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
[Sanal ağın](../../virtual-network/virtual-networks-overview.md)vNet ve alt netini oluşturun.

1. Alt ağı oluşturun. Aşağıdaki örnek, kaynak grubu **myResourceGroup'ta** **10.0.0.0/24**adres önekiyle **mySubnet** adında bir alt ağ oluşturur.  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Sanal ağı oluşturun. Aşağıdaki örnek, **10.0.0.0/16**adres öneki ile **Batı ABD** konumunda **myVnet** adlı bir sanal ağ oluşturur.  
   
    ```powershell
    $location = "WestUS"
    $vnetName = "myVnet"
    $vnet = New-AzVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Ortak IP adresi ve ağ arabirimi oluşturma
Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

1. Herkese açık bir IP adresi oluşturun. Bu örnek, **myPip**adlı genel bir IP adresi oluşturur. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. NIC'yi oluşturun. Bu örnek, **myNic**adlı bir NIC oluşturur. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve RDP kuralı oluşturma
RDP kullanarak VM'nizde oturum açabilmek için, 3389 bağlantı noktasında RDP erişimine izin veren bir güvenlik kuralına sahip olmanız gerekir. 

Bu örnek, **myNsg** adlı bir NSG oluşturarak 3389 no'c bağlantı noktası üzerinde RDP trafiğine izin veren **myRdpRule** adlı bir kural içerir. NSG'ler hakkında daha fazla bilgi için [PowerShell kullanarak Azure'da bir VM](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bağlantı noktası açma'ya bakın.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a>Sanal ağ için değişken oluşturma
Tamamlanan sanal ağ için bir değişken oluşturun. 

```powershell
$vnet = Get-AzVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-the-vm"></a>Sanal makine oluşturma
Aşağıdaki PowerShell komut dosyası, sanal makine yapılandırmalarının nasıl ayarlanış yapılacağını ve yüklenen VM görüntüsünü yeni yüklemenin kaynağı olarak nasıl kullanacağını gösterir.



```powershell
# Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential

    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"

    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"

    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set the VM name and size
    $vmConfig = New-AzVMConfig -VMName $vmName -VMSize $vmSize

    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzVMNetworkInterface -VM $vm -Id $nic.Id

    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create the new VM
    New-AzVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>VM'nin oluşturulduğunu doğrulama
Tamamlandığında,**Sanal makinelere** **Gözat** > altındaki [Azure portalında](https://portal.azure.com) veya aşağıdaki PowerShell komutlarını kullanarak yeni oluşturulan VM'yi görmeniz gerekir:

```powershell
    $vmList = Get-AzVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenizi Azure PowerShell ile yönetmek için [Azure Kaynak Yöneticisi ve PowerShell'i kullanarak sanal makineleri yönet'e](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.


