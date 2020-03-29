---
title: Yüklenen genelleştirilmiş VHD'den VM oluşturma
description: Genelleştirilmiş bir VHD'yi Azure'a yükleyin ve Kaynak Yöneticisi dağıtım modelinde yeni VM'ler oluşturmak için kullanın.
services: virtual-machines-windows
author: cynthn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 12/12/2019
ms.author: cynthn
ms.openlocfilehash: 3c482caf2407c89ffdb6c55c9184c31e2e3197c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464943"
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Genelleştirilmiş bir VHD'yi karşıya yükleme ve Azure’da yeni VM’ler oluştururken kullanma

Bu makale, Genelleştirilmiş bir VM'nin VHD'sini Azure'a yüklemek, VHD'den bir görüntü oluşturmak ve bu görüntüden yeni bir VM oluşturmak için PowerShell'i kullanmanız için size yol sunar. Şirket içi sanallaştırma aracından veya başka bir buluttan dışa aktarılan bir VHD yükleyebilirsiniz. Yeni VM için [Yönetilen Diskler'in](managed-disks-overview.md) kullanılması VM yönetimini basitleştirir ve VM kullanılabilirlik kümesine yerleştirildiğinde daha iyi kullanılabilirlik sağlar. 

Örnek bir komut dosyası için, [Azure'a VHD yüklemek ve yeni bir VM oluşturmak için Örnek komut dosyasına](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)bakın.

## <a name="before-you-begin"></a>Başlamadan önce

- Azure'a herhangi bir VHD yüklemeden önce, [Azure'a yüklemek için Windows VHD veya VHDX Hazırla'yı](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)izlemeniz gerekir.
- Yönetilen [Disklere geçişinizi](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) başlatmadan önce Yönetilen Disklere geçiş için Planı gözden [geçirin.](managed-disks-overview.md)

 
## <a name="generalize-the-source-vm-by-using-sysprep"></a>Sysprep kullanarak kaynak VM genelleştirin

Henüz yapmadıysanız, VHD'yi Azure'a yüklemeden önce VM'yi Sysprep yapmanız gerekir. Sysprep diğer öğelerin yanı sıra tüm kişisel hesap bilgilerinizi kaldırır ve makineyi bir görüntü olarak kullanılacak şekilde hazırlar. Sysprep hakkında ayrıntılı bilgi için [Sysprep Genel Bakış'a](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)bakın.

Makinede çalışan sunucu rollerinin Sysprep tarafından desteklendirildiklerinden emin olun. Daha fazla bilgi [için Sunucu Rolleri için Sysprep Desteği'ne](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)bakın.

> [!IMPORTANT]
> VHD'nizi Azure'a ilk kez yüklemeden önce Sysprep'i çalıştırmayı planlıyorsanız, [VM'nizi hazırladığınızdan](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)emin olun. 
> 
> 

1. Windows sanal makinesinde oturum açın.
2. Yönetici olarak Komut İstemi penceresini açın. Dizin %windir%\system32\sysprep olarak değiştirin `sysprep.exe`ve çalıştırın.
3. Sistem **Hazırlama Aracı** iletişim kutusunda, **Kutudan Çıkma Sistemi Girin Deneyimi 'ni (OOBE)** seçin ve **Genelleişaret** onay kutusunun etkinleştirildiğinden emin olun.
4. **Kapatma Seçenekleri**için **Kapatma'yı**seçin.
5. **Tamam'ı**seçin.
   
    ![Sysprep'i Başlat](./media/upload-generalized-managed/sysprepgeneral.png)
6. Sysprep bittiğinde, sanal makineyi kapatır. VM'yi yeniden başlatmayın.


## <a name="upload-the-vhd"></a>VHD'yi yükleyin 

Artık yönetilen bir diske doğrudan bir VHD yükleyebilirsiniz. Talimatlar için Azure [PowerShell kullanarak Azure'a VHD Yükle'ye](disks-upload-vhd-to-managed-disk-powershell.md)bakın.



VHD yönetilen diske yüklendikten sonra yönetilen diski almak için [Get-AzDisk'i](https://docs.microsoft.com/powershell/module/az.compute/get-azdisk) kullanmanız gerekir.

```azurepowershell-interactive
$disk = Get-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'myDiskName'
```

## <a name="create-the-image"></a>Görüntü oluşturma
Genelleştirilmiş işletim sistemi yönetilen diskinizden yönetilen bir görüntü oluşturun. Aşağıdaki değerleri kendi bilgilerinizle değiştirin.

İlk olarak, bazı değişkenler ayarlayın:

```powershell
$location = 'East US'
$imageName = 'myImage'
$rgName = 'myResourceGroup'
```

Yönetilen diskinizi kullanarak görüntüyü oluşturun.

```azurepowershell-interactive
$imageConfig = New-AzImageConfig `
   -Location $location
$imageConfig = Set-AzImageOsDisk `
   -Image $imageConfig `
   -OsState Generalized `
   -OsType Windows `
   -ManagedDiskId $disk.Id
```

Görüntü oluşturun.

```azurepowershell-interactive
$image = New-AzImage `
   -ImageName $imageName `
   -ResourceGroupName $rgName `
   -Image $imageConfig
```

## <a name="create-the-vm"></a>Sanal makine oluşturma

Artık bir görüntünüz olduğuna göre, görüntüden bir veya daha fazla yeni VM oluşturabilirsiniz. Bu *örnekmyImage* *myVM* adlı bir VM oluşturur , *myResourceGroup.*


```powershell
New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Image $image.Id `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNSG" `
    -PublicIpAddressName "myPIP" `
    -OpenPorts 3389
```


## <a name="next-steps"></a>Sonraki adımlar

Yeni sanal makinenizde oturum açın. Daha fazla bilgi için [windows çalıştıran bir Azure sanal makinesine nasıl bağlanıp oturum açarken](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)göreceğiz. 

