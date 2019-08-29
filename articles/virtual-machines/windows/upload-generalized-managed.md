---
title: Genelleştirilmiş şirket içi bir VHD 'den yönetilen bir Azure VM oluşturun | Microsoft Docs
description: Genelleştirilmiş bir VHD 'yi Azure 'a yükleyin ve Kaynak Yöneticisi dağıtım modelinde yeni VM 'Ler oluşturmak için kullanın.
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
ms.date: 09/25/2018
ms.author: cynthn
ms.openlocfilehash: be3ccfd0c562763d0968398ddb042dc5f07dbdcf
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70101571"
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Genelleştirilmiş bir VHD 'YI karşıya yükleyin ve Azure 'da yeni VM 'Ler oluşturmak için kullanın

Bu makalede, PowerShell kullanarak genelleştirilmiş bir sanal makinenin VHD 'sini Azure 'a yükleme, VHD 'den bir görüntü oluşturma ve bu görüntüden yeni bir VM oluşturma işlemleri gösterilmektedir. Şirket içi bir sanallaştırma aracından veya başka bir buluttan aktarılmış bir VHD 'yi karşıya yükleyebilirsiniz. Yeni VM için [yönetilen disklerin](managed-disks-overview.md) kullanılması VM yönetimini BASITLEŞTIRIR ve VM bir kullanılabilirlik kümesine yerleştirildiğinde daha iyi kullanılabilirlik sağlar. 

Örnek betik için bkz. [Azure 'a BIR VHD yüklemek ve yeni BIR VM oluşturmak Için örnek betik](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md).

## <a name="before-you-begin"></a>Başlamadan önce

- Herhangi bir VHD 'yi Azure 'a yüklemeden önce, [Azure 'a yüklemek için bir WINDOWS VHD veya vhdx hazırlamanızı](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)izlemelisiniz.
- [Yönetilen disklere](managed-disks-overview.md)geçişinizi başlatmadan önce [yönetilen disklere geçiş planını](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) gözden geçirin.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]


## <a name="generalize-the-source-vm-by-using-sysprep"></a>Sysprep kullanarak kaynak VM 'yi Genelleştirme

Sysprep diğer öğelerin yanı sıra tüm kişisel hesap bilgilerinizi kaldırır ve makineyi bir görüntü olarak kullanılacak şekilde hazırlar. Sysprep hakkında daha fazla bilgi için bkz. [Sysprep genel bakış](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

Makinede çalışan sunucu rollerinin Sysprep tarafından desteklendiğinden emin olun. Daha fazla bilgi için bkz. [sunucu rolleri Için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [!IMPORTANT]
> VHD 'nizi Azure 'a ilk kez yüklemeden önce Sysprep 'i çalıştırmayı planlıyorsanız, [VM 'nizi hazırladığınızdan](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)emin olun. 
> 
> 

1. Windows sanal makinesinde oturum açın.
2. Yönetici olarak Komut İstemi penceresini açın. Dizini%windir%\system32\sysprep olarak değiştirip komutunu çalıştırın `sysprep.exe`.
3. **Sistem Hazırlama Aracı** iletişim kutusunda, **sistem kutudan çıkar deneyimi (OOBE)** seçeneğini belirleyin ve **Genelleştir** onay kutusunun etkinleştirildiğinden emin olun.
4. **Kapalı seçenekleri**Için, **kapanıyor**' ı seçin.
5. **Tamam**’ı seçin.
   
    ![Sysprep 'ı Başlat](./media/upload-generalized-managed/sysprepgeneral.png)
6. Sysprep tamamlandığında, sanal makineyi kapatır. VM 'yi yeniden başlatmayın.


## <a name="get-a-storage-account"></a>Depolama hesabı al

Karşıya yüklenen VM görüntüsünü depolamak için Azure 'da bir depolama hesabı gerekir. Var olan bir depolama hesabı kullanabilir veya yeni bir tane oluşturabilirsiniz. 

Bir VM için yönetilen disk oluşturmak üzere VHD 'yi kullanacaksanız, depolama hesabı konumu VM 'yi oluşturacağınız konumla aynı olmalıdır.

Kullanılabilir depolama hesaplarını göstermek için şunu girin:

```azurepowershell
Get-AzStorageAccount | Format-Table
```

## <a name="upload-the-vhd-to-your-storage-account"></a>VHD 'YI depolama hesabınıza yükleyin

VHD 'yi Depolama hesabınızdaki bir kapsayıcıya yüklemek için [Add-AzVhd](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd) cmdlet 'ini kullanın. Bu örnek, *myvhd. vhd* dosyasını *c:\users\ortak\belgelerim\sanal sabit disklerinden\\*  myresourcegroup kaynak grubundaki *mystorageaccount* adlı bir depolama hesabına yükler. Dosya *myContainer* adlı kapsayıcıya yerleştirilecek ve yeni dosya adı *Myuploadedvhd. vhd*olacaktır.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Başarılı olursa şuna benzer bir yanıt alırsınız:

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

### <a name="other-options-for-uploading-a-vhd"></a>Bir VHD yüklemek için diğer seçenekler
 
Ayrıca, aşağıdakilerden birini kullanarak depolama hesabınıza bir VHD yükleyebilirsiniz:

- [AzCopy](https://aka.ms/downloadazcopy)
- [Azure depolama kopya blobu API 'SI](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Blobları karşıya yükleme Azure Depolama Gezgini](https://azurestorageexplorer.codeplex.com/)
- [Depolama Içeri/dışarı aktarma hizmeti REST API başvurusu](https://msdn.microsoft.com/library/dn529096.aspx)
-   Tahmini karşıya yükleme süresi yedi günden uzunsa Içeri/dışarı aktarma hizmetini kullanmanızı öneririz. Veri boyutu ve aktarım biriminden süreyi tahmin etmek için [Datatransferspeedhesaplayıcı](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) ' yı kullanabilirsiniz. 
    İçeri/dışarı aktarma, standart bir depolama hesabına kopyalamak için kullanılabilir. AzCopy gibi bir araç kullanarak standart depolamadan Premium depolama hesabına kopyalamanız gerekir.

> [!IMPORTANT]
> VHD 'nizi Azure 'a yüklemek için AzCopy kullanıyorsanız, yükleme betiğinizi çalıştırmadan önce [ **/Blobtype: Page**](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-blobs#upload-a-file) ' i ayarladığınızdan emin olun. Hedef bir blob ise ve bu seçenek belirtilmemişse, varsayılan AzCopy bir Blok Blobu oluşturur.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Karşıya yüklenen VHD 'den yönetilen bir görüntü oluşturma 

Genelleştirilmiş işletim sistemi VHD 'nizden yönetilen bir görüntü oluşturun. Aşağıdaki değerleri kendi bilgileriniz ile değiştirin.


İlk olarak, bazı parametreleri ayarlayın:

```powershell
$location = "East US" 
$imageName = "myImage"
```

Genelleştirilmiş işletim sistemi VHD 'nizi kullanarak görüntü oluşturun.

```powershell
$imageConfig = New-AzImageConfig `
   -Location $location
$imageConfig = Set-AzImageOsDisk `
   -Image $imageConfig `
   -OsType Windows `
   -OsState Generalized `
   -BlobUri $urlOfUploadedImageVhd `
   -DiskSizeGB 20
New-AzImage `
   -ImageName $imageName `
   -ResourceGroupName $rgName `
   -Image $imageConfig
```


## <a name="create-the-vm"></a>Sanal makine oluşturma

Artık bir görüntünüz olduğuna göre, görüntüden bir veya daha fazla yeni VM oluşturabilirsiniz. Bu örnek Myresourcegroup içindeki Myvm adlı birVM oluşturur.


```powershell
New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -ImageName $imageName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNSG" `
    -PublicIpAddressName "myPIP" `
    -OpenPorts 3389
```


## <a name="next-steps"></a>Sonraki adımlar

Yeni sanal makinenizde oturum açın. Daha fazla bilgi için bkz. [Windows çalıştıran bir Azure sanal makinesine bağlanma ve oturum](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)açma. 

