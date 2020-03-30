---
title: Azure'da genelleştirilmiş bir VM'nin yönetilmeyen görüntüsünü oluşturma
description: Azure'da bir VM'nin birden çok kopyasını oluşturmak için kullanmak üzere genelleştirilmiş bir Windows VM'nin unmanged görüntüsünü oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: f25968fb74f0f10b1d498866c036dd04d4d5d134
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74073387"
---
# <a name="how-to-create-an-unmanaged-vm-image-from-an-azure-vm"></a>Azure VM'den yönetilmeyen bir VM görüntüsü oluşturma

Bu makale, depolama hesaplarının kullanılmasını kapsamaktadır. Depolama hesabı yerine yönetilen diskler ve yönetilen görüntüler kullanmanızı öneririz. Daha fazla bilgi için [azure'da genelleştirilmiş bir VM'nin yönetilen görüntüsünü yakalayın'](capture-image-resource.md)a bakın.

Bu makalede, bir depolama hesabı kullanarak genelleştirilmiş bir Azure VM görüntüsü oluşturmak için Azure PowerShell'i nasıl kullanacağınızı gösterilmektedir. Daha sonra başka bir VM oluşturmak için görüntüyü kullanabilirsiniz. Görüntü işletim sistemi diski ve sanal makineye bağlı veri diskleri içerir. Görüntü sanal ağ kaynaklarını içermez, bu nedenle yeni VM oluştururken bu kaynakları ayarlamanız gerekir. 

 

## <a name="generalize-the-vm"></a>VM'yi genelleştirin 
Bu bölümde, Windows sanal makinenizi görüntü olarak kullanılmak üzere nasıl genelleştirilebildiğiniz gösterilmektedir. VM genelleme, diğer şeylerin yanı sıra tüm kişisel hesap bilgilerinizi kaldırır ve makineyi görüntü olarak kullanılmak üzere hazırlar. Sysprep hakkındaki ayrıntılar için bkz. [Sysprep İşlemini Kullanma: Giriş](https://technet.microsoft.com/library/bb457073.aspx).

Makinede çalışan sunucu rollerinin Sysprep tarafından desteklendirildiklerinden emin olun. Daha fazla bilgi [için Sunucu Rolleri için Sysprep Desteği'ne](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) bakın

> [!IMPORTANT]
> VHD'nizi ilk kez Azure'a yüklüyorsanız, Sysprep'i çalıştırmadan önce [VM'inizi hazırladığınızdan](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) emin olun. 
> 
> 

Ayrıca bir Linux VM'yi `sudo waagent -deprovision+user` kullanarak genelleştirebilir ve VM'yi yakalamak için PowerShell'i kullanabilirsiniz. Bir VM yakalamak için CLI'yi kullanma hakkında bilgi için Azure [CLI'yi kullanarak bir Linux sanal makineyi nasıl genelleştirip yakalayabilirim](../linux/capture-image.md)bkz.


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

## <a name="log-in-to-azure-powershell"></a>Azure PowerShell'de oturum açma
1. Azure PowerShell'i açın ve Azure hesabınızda oturum açın.
   
    ```powershell
    Connect-AzAccount
    ```
   
    Azure hesap kimlik bilgilerinizi girmeniz için açılır pencere açılır.
2. Mevcut abonelikleriniz için abonelik tünalarını alın.
   
    ```powershell
    Get-AzSubscription
    ```
3. Abonelik kimliğini kullanarak doğru aboneliği ayarlayın.
   
    ```powershell
    Select-AzSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Deallocate VM ve genelleştirilmiş devlet ayarlayın

> [!IMPORTANT] 
> Genelleştirilmiş olarak işaretlendikten sonra VM'den etiket ekleyemez, kaldıramaz veya kaldıramazsınız. VM'ye bir etiket eklemek istiyorsanız, etiketleri genelleştirilmiş olarak işaretlemeden önce eklediğinizden emin olun.
> 

1. VM kaynaklarını niçin bulunun.
   
    ```powershell
    Stop-AzVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    Azure portalındaki *VM'nin Durumu* **Durduruldu'dan** **Durduruldu'ya (ayrılan)** değişir.
2. Sanal makinenin durumunu **Genelleştirilmiş**olarak ayarlayın. 
   
    ```powershell
    Set-AzVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. VM'nin durumunu kontrol edin. VM için **OSState/genelleştirilmiş** bölümde **DisplayStatus** **VM olarak genelleştirilmiş**olmalıdır.  
   
    ```powershell
    $vm = Get-AzVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Görüntü oluşturma

Bu komutu kullanarak hedef depolama kapsayıcısında yönetilmeyen bir sanal makine görüntüsü oluşturun. Görüntü, orijinal sanal makineyle aynı depolama hesabında oluşturulur. Parametre, `-Path` kaynak VM için JSON şablonunun bir kopyasını yerel bilgisayarınıza kaydeder. `-DestinationContainerName` Parametre, resimlerinizi tutmak istediğiniz kapsayıcının adıdır. Kapsayıcı yoksa, sizin için oluşturulur.
   
```powershell
Save-AzVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
Resminizin URL'sini JSON dosya şablonundan alabilirsiniz. Görüntünüzün tam yolu için **kaynak** > **depolamaProfil** > **osDisk** > **görüntü** > **uri** bölümüne gidin. Resmin URL'si `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`şöyle görünür: .
   
Portaldaki URI'yi de doğrulayabilirsiniz. Görüntü, depolama hesabınızdaki **sistem** adlı bir kapsayıcıya kopyalanır. 

## <a name="create-a-vm-from-the-image"></a>Görüntüden bir VM oluşturun

Artık yönetilmeyen görüntüden bir veya daha fazla VM oluşturabilirsiniz.

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
    $location = "West US"
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
Aşağıdaki PowerShell sanal makine yapılandırmalarını tamamlar ve yeni yüklemenin kaynağı olarak yönetilmeyen görüntü kullanır.

</br>

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

### <a name="verify-that-the-vm-was-created"></a>VM'nin oluşturulduğunu doğrulama
Tamamlandığında,**Sanal makinelere** **Gözat** > altındaki [Azure portalında](https://portal.azure.com) veya aşağıdaki PowerShell komutlarını kullanarak yeni oluşturulan VM'yi görmeniz gerekir:

```powershell
    $vmList = Get-AzVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenizi Azure PowerShell ile yönetmek için [Azure Kaynak Yöneticisi ve PowerShell'i kullanarak sanal makineleri yönet'e](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.


