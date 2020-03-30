---
title: Azure PowerShell'i kullanarak Azure'a vhd yükleme
description: Azure yönetilen bir diske vhd yüklemeyi ve azure PowerShell'i kullanarak bölgeler arasında yönetilen bir diski doğrudan yükleme yoluyla nasıl kopyalayamanızı öğrenin.
author: roygara
ms.author: rogarana
ms.date: 03/13/2020
ms.topic: article
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.subservice: disks
ms.openlocfilehash: 883fea1e25ded26c35e96d11edd8f417e96db30e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79369565"
---
# <a name="upload-a-vhd-to-azure-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure'a vhd yükleme

Bu makalede, yerel makinenizden Azure yönetilen bir diske vhd yükleme nin nasıl olduğu açıklanmaktadır. Daha önce, verilerinizi bir depolama hesabında düzenlemeyi ve bu depolama hesabını yönetmeyi içeren daha ilgili bir işlemi izlemeniz gerekiyordu. Şimdi, artık bir depolama hesabı yönetmek veya bir vhd yüklemek için veri sahne gerekir. Bunun yerine, boş yönetilen bir disk oluşturun ve doğrudan bir vhd yükleyin. Bu, şirket içi VM'lerin Azure'a yüklenmesini kolaylaştırır ve 32 TiB'ye kadar bir vhd'yi doğrudan yönetilen büyük bir diske yüklemenize olanak tanır.

Azure'daki IaaS VM'ler için bir yedekleme çözümü sağlıyorsanız, müşteri yedeklemelerini yönetilen disklere geri yüklemek için doğrudan yükleme kullanmanızı öneririz. Azure dışındaki bir makineden Bir VHD yüklüyorsanız, hızlar yerel bant genişliğinize bağlıdır. Azure VM kullanıyorsanız, bant genişliğiniz standart HDD'lerle aynı olacaktır.

Şu anda, doğrudan yükleme standart HDD, standart SSD ve premium SSD yönetilen diskler için desteklenir. Henüz ultra SSD'ler için desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

- [AzCopy v10](../../storage/common/storage-use-azcopy-v10.md#download-and-install-azcopy)en son sürümünü indirin.
- [Azure PowerShell modüllerini yükleyin.](/powershell/azure/install-Az-ps)
- Şirket içinde bir VHD yüklemeyi planlıyorsanız: [Azure için hazırlanmış](prepare-for-upload-vhd-image.md)sabit boyutlu bir VHD, yerel olarak depolanır.
- Veya bir kopyalama eylemi gerçekleştirmek istiyorsanız Azure'da yönetilen bir disk.

## <a name="create-an-empty-managed-disk"></a>Boş yönetilen disk oluşturma

VHD'nizi Azure'a yüklemek için, bu yükleme işlemi için yapılandırılan boş bir yönetilen disk oluşturmanız gerekir. Bir tane oluşturmadan önce, bu diskler hakkında bilmeniz gereken bazı ek bilgiler vardır.

Yönetilen disk bu tür iki benzersiz durumları vardır:

- ReadToUpload, diskin yükleme almaya hazır olduğu ancak [güvenli erişim imzası](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) (SAS) oluşturulmadı anlamına gelir.
- ActiveUpload, bu da diskin yükleme almaya hazır olduğu ve SAS'ın oluşturulduğu anlamına gelir.

Bu durumların herhangi birinde, yönetilen disk, gerçek disk türüne bakılmaksızın [standart HDD fiyatlandırmasında](https://azure.microsoft.com/pricing/details/managed-disks/)faturalandırılır. Örneğin, Bir P10 S10 olarak faturalandırılır. Bu, diski `revoke-access` VM'ye takmak için gerekli olan yönetilen diske çağrılana kadar geçerli olacaktır.

Yüklemek için boş bir standart HDD oluşturmadan önce, yüklemek istediğiniz vhd'nin baytlarında dosya boyutuna ihtiyacınız vardır. Örnek kodu sizin için alacak ama, kendiniz kullanabilirsiniz `$vhdSizeBytes = (Get-Item "<fullFilePathHere>").length`bunu yapmak için: . **-UploadSizeBytes** parametresini belirtirken bu değer kullanılır.

Şimdi, yerel kabuğunuzda, **-CreateOption** parametresinde **Yükleme** ayarını ve [Yeni-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig?view=azps-1.8.0) cmdlet'teki **UploadSizeInBytes** parametresini belirterek yükleme için boş bir standart HDD oluşturun. Ardından diski oluşturmak için [New-AzDisk'i](https://docs.microsoft.com/powershell/module/az.compute/new-azdisk?view=azps-1.8.0) arayın:

```powershell
$vhdSizeBytes = (Get-Item "<fullFilePathHere>").length

$diskconfig = New-AzDiskConfig -SkuName 'Standard_LRS' -OsType 'Windows' -UploadSizeInBytes $vhdSizeBytes -Location 'West US' -CreateOption 'Upload'

New-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'myDiskName' -Disk $diskconfig
```

Premium SSD veya standart bir SSD yüklemek istiyorsanız, **Standard_LRS** **Premium_LRS** veya **StandardSSD_LRS**ile değiştirin. Ultra SSD henüz desteklenmedi.

Şimdi yükleme işlemi için yapılandırılan boş bir yönetilen disk oluşturdunuz. Diske bir vhd yüklemek için, yüklemeniz için hedef olarak başvuruda bulunabilmeniz için yazılabilir bir SAS'ye ihtiyacınız vardır.

Boş yönetilen diskinizin yazılabilir SAS'ını oluşturmak için aşağıdaki komutu kullanın:

```powershell
$diskSas = Grant-AzDiskAccess -ResourceGroupName 'myResouceGroup' -DiskName 'myDiskName' -DurationInSecond 86400 -Access 'Write'

$disk = Get-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'myDiskName'
```

## <a name="upload-vhd"></a>VHD yükle

Artık boş yönetilen diskiniz için bir SAS'ınız olduğuna göre, yönetilen diskinizi yükleme komutunuzun hedefi olarak ayarlamak için kullanabilirsiniz.

Oluşturduğunuz SAS URI'yi belirterek yerel VHD dosyanızı yönetilen bir diske yüklemek için AzCopy v10'u kullanın.

Bu yükleme eşdeğer [standart HDD](disks-types.md#standard-hdd)ile aynı üretim throughite sahiptir. Örneğin, S4'e eşit bir boyutunuz varsa, 60 MiB/s'ye kadar bir çıktınız olacaktır. Ancak, S70'e eşit bir boyuta sahipseniz, 500 MiB/s'ye kadar bir çıktınız olacaktır.

```
AzCopy.exe copy "c:\somewhere\mydisk.vhd" $diskSas.AccessSAS --blob-type PageBlob
```

Yükleme tamamlandıktan sonra ve artık diske daha fazla veri yazmanız gerekmez, SAS'ı iptal edin. SAS'ı iptal etmek yönetilen diskin durumunu değiştirir ve diski VM'ye eklemenize olanak sağlar.

```powershell
Revoke-AzDiskAccess -ResourceGroupName 'myResourceGroup' -DiskName 'myDiskName'
```

## <a name="copy-a-managed-disk"></a>Yönetilen diski çoğaltma

Doğrudan yükleme, yönetilen bir diski kopyalama işlemini de kolaylaştırır. Aynı bölge içinde veya bölgeler arası (başka bir bölgeye) kopyalayabilirsiniz.

Takip komut dosyası bunu sizin için yapar, işlem varolan bir diskle çalıştığınızdan beri bazı farklılıklarla birlikte daha önce açıklanan adımlara benzer.

> [!IMPORTANT]
> Azure'dan yönetilen bir diskin baytlarında disk boyutunu sağlarken 512'lik bir ofset eklemeniz gerekir. Bunun nedeni, Azure'un disk boyutunu döndürürken altbilgiden atlanmasıdır. Bunu yapmazsanız kopya başarısız olur. Aşağıdaki komut dosyası bunu zaten sizin için yapar.

Değerlerinizle `<sourceResourceGroupHere>` `<sourceDiskNameHere>`, `<targetDiskNameHere>` `<targetResourceGroupHere>`, `<yourOSTypeHere>` `<yourTargetLocationHere>` , , , ve (konum değeri örneği uswest2 olacaktır) değiştirin, ardından yönetilen bir diski kopyalamak için aşağıdaki komut dosyasını çalıştırın.

```powershell

$sourceRG = <sourceResourceGroupHere>
$sourceDiskName = <sourceDiskNameHere>
$targetDiskName = <targetDiskNameHere>
$targetRG = <targetResourceGroupHere>
$targetLocate = <yourTargetLocationHere>
#Expected value for OS is either "Windows" or "Linux"
$targetOS = <yourOSTypeHere>

$sourceDisk = Get-AzDisk -ResourceGroupName $sourceRG -DiskName $sourceDiskName

# Adding the sizeInBytes with the 512 offset, and the -Upload flag
$targetDiskconfig = New-AzDiskConfig -SkuName 'Standard_LRS' -osType $targetOS -UploadSizeInBytes $($sourceDisk.DiskSizeBytes+512) -Location $targetLocate -CreateOption 'Upload'

$targetDisk = New-AzDisk -ResourceGroupName $targetRG -DiskName $targetDiskName -Disk $targetDiskconfig

$sourceDiskSas = Grant-AzDiskAccess -ResourceGroupName $sourceRG -DiskName $sourceDiskName -DurationInSecond 86400 -Access 'Read'

$targetDiskSas = Grant-AzDiskAccess -ResourceGroupName $targetRG -DiskName $targetDiskName -DurationInSecond 86400 -Access 'Write'

azcopy copy $sourceDiskSas.AccessSAS $targetDiskSas.AccessSAS --blob-type PageBlob

Revoke-AzDiskAccess -ResourceGroupName $sourceRG -DiskName $sourceDiskName

Revoke-AzDiskAccess -ResourceGroupName $targetRG -DiskName $targetDiskName 
```

## <a name="next-steps"></a>Sonraki adımlar

Artık yönetilen bir diske başarılı bir vhd yüklediğinize göre, diskinizi bir VM'ye ekleyebilir ve kullanmaya başlayabilirsiniz.

VM'ye nasıl veri diski ekleyeceklerini öğrenmek için konuyla ilgili makalemize bakın: [PowerShell içeren bir Windows VM'ye veri diski takın.](attach-disk-ps.md) Diski işletim sistemi diski olarak kullanmak için [bkz.](create-vm-specialized.md#create-the-new-vm)
