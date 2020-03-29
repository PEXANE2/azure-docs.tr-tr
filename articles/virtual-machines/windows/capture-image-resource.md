---
title: Azure'da yönetilen bir resim oluşturma
description: Azure'da genelleştirilmiş bir VM veya VHD'nin yönetilen görüntüsünü oluşturun. Görüntüler, yönetilen diskleri kullanan birden çok VM oluşturmak için kullanılabilir.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 09/27/2018
ms.author: cynthn
ms.openlocfilehash: 01619027ddc79530dc9541584efa9a3e518f5136
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74842067"
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a>Azure’da genelleştirilmiş bir VM’nin yönetilen görüntüsünü oluşturma

Yönetilen bir görüntü kaynağı, depolama hesabında yönetilen bir disk veya yönetilmeyen bir disk olarak depolanan genelleştirilmiş bir sanal makineden (VM) oluşturulabilir. Görüntü daha sonra birden çok VM oluşturmak için kullanılabilir. Yönetilen görüntülerin nasıl faturalandırıldırış edildiği hakkında bilgi için [Yönetilen Diskler](https://azure.microsoft.com/pricing/details/managed-disks/)fiyatlandırması'na bakın. 

 

## <a name="generalize-the-windows-vm-using-sysprep"></a>Sysprep kullanarak Windows VM'sini genelleştirme

Sysprep tüm kişisel hesap ve güvenlik bilgilerinizi kaldırır ve sonra makineyi görüntü olarak kullanılmak üzere hazırlar. Sysprep hakkında daha fazla bilgi için [Sysprep genel bakış](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)bilgisine bakın.

Makinede çalışan sunucu rollerinin Sysprep tarafından desteklendirildiklerinden emin olun. Daha fazla bilgi için sunucu rolleri ve [Desteklenmeyen senaryolar](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview#unsupported-scenarios) [için Sysprep desteğine](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep-support-for-server-roles) bakın.

> [!IMPORTANT]
> Sysprep'i vm üzerinde çalıştırdıktan sonra, vm *genelleştirilmiş* olarak kabul edilir ve yeniden başlatılamaz. VM’yi genelleştirme işlemi geri döndürülemez. Orijinal VM'nin çalışmasını tutmanız gerekiyorsa, [VM'nin](create-vm-specialized.md#option-3-copy-an-existing-azure-vm) bir kopyasını oluşturmalı ve kopyasını genellemeniz gerekir. 
>
> Sanal sabit diskinizi (VHD) Azure'a ilk kez yüklemeden önce Sysprep'i çalıştırmayı planlıyorsanız, [VM'nizi hazırladığınızdan](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)emin olun.  
> 
> 

Windows VM'nizi genelleştirmek için aşağıdaki adımları izleyin:

1. Windows VM'nizde oturum açın.
   
2. Yönetici olarak Komut İstemi pencerelerini açın. Dizin %windir%\system32\sysprep olarak değiştirin `sysprep.exe`ve çalıştırın.
   
3. Sistem **Hazırlama Aracı** iletişim kutusunda, **Kutudan Çıkma Sistemi Girin'i (OOBE)** ve **Genelleişaret** onay kutusunu seçin.
   
4. **Kapatma Seçenekleri**için **Kapatma'yı**seçin.
   
5. **Tamam'ı**seçin.
   
    ![Sysprep'i Başlat](./media/upload-generalized-managed/sysprepgeneral.png)

6. Sysprep tamamlandığında VM'yi kapatır. VM'yi yeniden başlatmayın.

> [!TIP]
> **İsteğe bağlı** Görüntünüzü optimize etmek ve VM'nizin ilk önyükleme süresini azaltmak için [DISM'yi](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-optimize-image-command-line-options) kullanın.
>
> Resminizi optimize etmek için, Windows gezgininde üzerine çift tıklayarak VHD'nizi `/optimize-image` monte edin ve parametreyle DISM çalıştırın.
>
> ```cmd
> DISM /image:D:\ /optimize-image /boot
> ```
> Nerede D: monte VHD yoludur.
>
> Çalışan `DISM /optimize-image` VHD yaptığınız son değişiklik olmalıdır. Dağıtımdan önce VHD'nizde herhangi bir değişiklik yaparsanız, `DISM /optimize-image` yeniden çalıştırmanız gerekir.

## <a name="create-a-managed-image-in-the-portal"></a>Portalda yönetilen bir görüntü oluşturma 

1. VM görüntüsünü yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.

2. VM'nizi listeden seçin.

3. VM için **Sanal makine** sayfasında, üst menüde, **Capture'i**seçin.

   **Resim oluştur** sayfası görüntülenir.

4. **Ad**için, önceden doldurulmuş adı kabul edin veya görüntü için kullanmak istediğiniz bir ad girin.

5. **Kaynak grubu için,** **yeni oluştur'u** seçin ve bir ad girin veya açılan listeden kullanılacak bir kaynak grubu seçin.

6. Görüntü oluşturulduktan sonra kaynak VM'yi silmek istiyorsanız, **görüntüyü oluşturduktan sonra bu sanal makineyi otomatik olarak sil'i**seçin.

7. Görüntüyü herhangi bir [kullanılabilirlik bölgesinde](../../availability-zones/az-overview.md)kullanabilme yeteneği istiyorsanız, Bölge esnekliği için **On'u**seçin. **On**

8. Görüntüyü oluşturmak için **Oluştur'u** seçin.

Görüntü oluşturulduktan sonra, kaynak grubundaki kaynaklar listesinde **görüntü** kaynağı olarak bulabilirsiniz.



## <a name="create-an-image-of-a-vm-using-powershell"></a>Powershell kullanarak VM görüntüsü oluşturma

 

Doğrudan VM'den bir görüntü oluşturmak, görüntünün Işletim Sistemi diski ve tüm veri diskleri de dahil olmak üzere VM ile ilişkili tüm diskleri içermesini sağlar. Bu örnek, yönetilen diskleri kullanan bir VM'den yönetilen bir görüntünün nasıl oluşturulabildiğini gösterir.

Başlamadan önce Azure PowerShell modülünün en son sürümüne sahip olduğunuzdan emin olun. Sürümü bulmak için `Get-Module -ListAvailable Az` PowerShell'de çalıştırın. Yükseltmeniz gerekiyorsa, [PowerShellGet ile Windows'ta Azure PowerShell'i Yükleyin'e](/powershell/azure/install-az-ps)bakın. PowerShell'i yerel olarak çalıştırıyorsanız, Azure ile bağlantı oluşturmak için çalıştırın. `Connect-AzAccount`


> [!NOTE]
> Resminizi bölge yedekli depolama alanında depolamak istiyorsanız, görüntünüzkullanılabilirlik [bölgelerini](../../availability-zones/az-overview.md) destekleyen bir bölgede `-ZoneResilient` oluşturmanız ve görüntü`New-AzImageConfig` yapılandırmasına (komut) parametreeklemeniz gerekir.

VM görüntüsü oluşturmak için aşağıdaki adımları izleyin:

1. Bazı değişkenler oluşturun.

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. VM'nin dağıtıldıkdığından emin olun.

    ```azurepowershell-interactive
    Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Sanal makinenin durumunu **Genelleştirilmiş**olarak ayarlayın. 
   
    ```azurepowershell-interactive
    Set-AzVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. Sanal makineyi alın. 

    ```azurepowershell-interactive
    $vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName
    ```

5. Görüntü yapılandırması oluşturun.

    ```azurepowershell-interactive
    $image = New-AzImageConfig -Location $location -SourceVirtualMachineId $vm.Id 
    ```
6. Görüntü oluşturun.

    ```azurepowershell-interactive
    New-AzImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 

## <a name="create-an-image-from-a-managed-disk-using-powershell"></a>PowerShell'i kullanarak yönetilen bir diskten görüntü oluşturma

Yalnızca işletim sistemi diskinin görüntüsünü oluşturmak istiyorsanız, yönetilen disk kimliğini işletim sistemi diski olarak belirtin:

    
1. Bazı değişkenler oluşturun. 

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```

2. VM'yi getirin.

   ```azurepowershell-interactive
   $vm = Get-AzVm -Name $vmName -ResourceGroupName $rgName
   ```

3. Yönetilen diskin kimliğini alın.

    ```azurepowershell-interactive
    $diskID = $vm.StorageProfile.OsDisk.ManagedDisk.Id
    ```
   
3. Görüntü yapılandırması oluşturun.

    ```azurepowershell-interactive
    $imageConfig = New-AzImageConfig -Location $location
    $imageConfig = Set-AzImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -ManagedDiskId $diskID
    ```
    
4. Görüntü oluşturun.

    ```azurepowershell-interactive
    New-AzImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 


## <a name="create-an-image-from-a-snapshot-using-powershell"></a>Powershell'i kullanarak anlık görüntü oluşturma

Genelleştirilmiş bir VM'nin anlık görüntüsünden yönetilen bir görüntü aşağıdaki adımları izleyerek oluşturabilirsiniz:

    
1. Bazı değişkenler oluşturun. 

    ```azurepowershell-interactive
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. Enstantaneyi al.

   ```azurepowershell-interactive
   $snapshot = Get-AzSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. Görüntü yapılandırması oluşturun.

    ```azurepowershell-interactive
    $imageConfig = New-AzImageConfig -Location $location
    $imageConfig = Set-AzImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. Görüntü oluşturun.

    ```azurepowershell-interactive
    New-AzImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 


## <a name="create-an-image-from-a-vm-that-uses-a-storage-account"></a>Depolama hesabı kullanan bir VM'den görüntü oluşturma

Yönetilen diskleri kullanmayan bir VM'den yönetilen bir görüntü oluşturmak için, depolama hesabında KI OS VHD'nin URI'sine aşağıdaki biçimde ihtiyacınız vardır: https://*mystorageaccount*.blob.core.windows.net/*vhdcontainer*/*vhdfilename.vhd*. Bu örnekte, VHD *mystorageaccount*, *vhdcontainer*adlı bir kapsayıcı , ve VHD dosya adı *vhdfilename.vhd*.


1.  Bazı değişkenler oluşturun.

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    $osVhdUri = "https://mystorageaccount.blob.core.windows.net/vhdcontainer/vhdfilename.vhd"
    ```
2. VM'yi durdurun/anlaşmalı.

    ```azurepowershell-interactive
    Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. VM'yi genelleştirilmiş olarak işaretleyin.

    ```azurepowershell-interactive
    Set-AzVm -ResourceGroupName $rgName -Name $vmName -Generalized  
    ```
4.  Genelleştirilmiş OS VHD'nizi kullanarak görüntüyü oluşturun.

    ```azurepowershell-interactive
    $imageConfig = New-AzImageConfig -Location $location
    $imageConfig = Set-AzImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

    
## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen bir görüntüden VM oluşturun.](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)    

