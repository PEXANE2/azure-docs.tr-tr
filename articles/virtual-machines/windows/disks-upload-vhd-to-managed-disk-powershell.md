---
title: Azure'a Bir VHD yükleme veya bölgeler arasında disk kopyalama - Azure PowerShell
description: Azure yönetilen bir diske Nasıl VHD yükleyip, azure PowerShell'i kullanarak bölgeler arasında yönetilen bir diski doğrudan yükleme yoluyla kopyalamayı öğrenin.
author: roygara
ms.author: rogarana
ms.date: 03/27/2020
ms.topic: article
ms.service: virtual-machines
ms.tgt_pltfrm: linux
ms.subservice: disks
ms.openlocfilehash: 7c66507989357569828d4ef933cfdca735f71570
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085420"
---
# <a name="upload-a-vhd-to-azure-or-copy-a-managed-disk-to-another-region---azure-powershell"></a>Azure'a Bir VHD yükleme veya yönetilen bir diski başka bir bölgeye kopyalama - Azure PowerShell

[!INCLUDE [disks-upload-vhd-to-disk-intro](../../../includes/disks-upload-vhd-to-disk-intro.md)]

## <a name="prerequisites"></a>Önkoşullar

- [AzCopy v10](../../storage/common/storage-use-azcopy-v10.md#download-and-install-azcopy)en son sürümünü indirin.
- [Azure PowerShell modüllerini yükleyin.](/powershell/azure/install-Az-ps)
- Şirket içinde bir VHD yüklemeyi planlıyorsanız: [Azure için hazırlanmış](prepare-for-upload-vhd-image.md)sabit boyutlu bir VHD, yerel olarak depolanır.
- Veya bir kopyalama eylemi gerçekleştirmek istiyorsanız Azure'da yönetilen bir disk.

## <a name="getting-started"></a>Kullanmaya başlama

Bir GUI üzerinden disk yüklemeyi tercih ederseniz, bunu Azure Depolama Gezgini'ni kullanarak yapabilirsiniz. Ayrıntılar için: [Azure yönetilen diskleri yönetmek için Azure Depolama Gezgini'ni kullanın](disks-use-storage-explorer-managed-disks.md)

VHD'nizi Azure'a yüklemek için, bu yükleme işlemi için yapılandırılan boş bir yönetilen disk oluşturmanız gerekir. Bir tane oluşturmadan önce, bu diskler hakkında bilmeniz gereken bazı ek bilgiler vardır.

Yönetilen disk bu tür iki benzersiz durumları vardır:

- ReadToUpload, diskin yükleme almaya hazır olduğu ancak [güvenli erişim imzası](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) (SAS) oluşturulmadı anlamına gelir.
- ActiveUpload, bu da diskin yükleme almaya hazır olduğu ve SAS'ın oluşturulduğu anlamına gelir.

> [!NOTE]
> Bu durumların herhangi birinde, yönetilen disk, gerçek disk türüne bakılmaksızın [standart HDD fiyatlandırmasında](https://azure.microsoft.com/pricing/details/managed-disks/)faturalandırılır. Örneğin, Bir P10 S10 olarak faturalandırılır. Bu, diski `revoke-access` VM'ye takmak için gerekli olan yönetilen diske çağrılana kadar geçerli olacaktır.

## <a name="create-an-empty-managed-disk"></a>Boş yönetilen disk oluşturma

Yükleme için boş bir standart HDD oluşturabilmeniz için, yüklemek istediğiniz VHD'nin baytlar halindeki dosya boyutuna ihtiyacınız vardır. Örnek kodu sizin için alacak ama, kendiniz kullanabilirsiniz `$vhdSizeBytes = (Get-Item "<fullFilePathHere>").length`bunu yapmak için: . **-UploadSizeBytes** parametresini belirtirken bu değer kullanılır.

Şimdi, yerel kabuğunuzda, **-CreateOption** parametresinde **Yükleme** ayarını ve [Yeni-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig?view=azps-1.8.0) cmdlet'teki **UploadSizeInBytes** parametresini belirterek yükleme için boş bir standart HDD oluşturun. Ardından diski oluşturmak için [New-AzDisk'i](https://docs.microsoft.com/powershell/module/az.compute/new-azdisk?view=azps-1.8.0) arayın.

Değiştir `<yourdiskname>` `<yourresourcegroupname>`, `<yourregion>` ve sonra aşağıdaki komutları çalıştırın:

```powershell
$vhdSizeBytes = (Get-Item "<fullFilePathHere>").length

$diskconfig = New-AzDiskConfig -SkuName 'Standard_LRS' -OsType 'Windows' -UploadSizeInBytes $vhdSizeBytes -Location '<yourregion>' -CreateOption 'Upload'

New-AzDisk -ResourceGroupName '<yourresourcegroupname' -DiskName '<yourdiskname>' -Disk $diskconfig
```

Premium SSD veya standart bir SSD yüklemek istiyorsanız, **Standard_LRS** **Premium_LRS** veya **StandardSSD_LRS**ile değiştirin. Ultra diskler henüz desteklenmez.

Artık yükleme işlemi için yapılandırılan boş yönetilen bir disk oluşturduğunuza göre, bu diske bir VHD yükleyebilirsiniz. Diske bir VHD yüklemek için, yüklemeniz için hedef olarak başvuruda bulunabilmeniz için yazılabilir bir SAS'ye ihtiyacınız vardır.

Boş yönetilen diskinizin yazılabilir Bir SAS'ı oluşturmak için aşağıdaki komutları değiştirin `<yourdiskname>`ve `<yourresourcegroupname>`ardından kullanın:

```powershell
$diskSas = Grant-AzDiskAccess -ResourceGroupName '<yourresourcegroupname>' -DiskName '<yourdiskname>' -DurationInSecond 86400 -Access 'Write'

$disk = Get-AzDisk -ResourceGroupName '<yourresourcegroupname>' -DiskName '<yourdiskname>'
```

## <a name="upload-a-vhd"></a>VHD’yi karşıya yükleme

Artık boş yönetilen diskiniz için bir SAS'ınız olduğuna göre, yönetilen diskinizi yükleme komutunuzun hedefi olarak ayarlamak için kullanabilirsiniz.

Oluşturduğunuz SAS URI'yi belirterek yerel VHD dosyanızı yönetilen bir diske yüklemek için AzCopy v10'u kullanın.

Bu yükleme eşdeğer [standart HDD](disks-types.md#standard-hdd)ile aynı üretim throughite sahiptir. Örneğin, S4'e eşit bir boyutunuz varsa, 60 MiB/s'ye kadar bir çıktınız olacaktır. Ancak, S70'e eşit bir boyuta sahipseniz, 500 MiB/s'ye kadar bir çıktınız olacaktır.

```
AzCopy.exe copy "c:\somewhere\mydisk.vhd" $diskSas.AccessSAS --blob-type PageBlob
```

Yükleme tamamlandıktan sonra ve artık diske daha fazla veri yazmanız gerekmez, SAS'ı iptal edin. SAS'ı iptal etmek yönetilen diskin durumunu değiştirir ve diski VM'ye eklemenize olanak sağlar.

`<yourdiskname>`Değiştirin `<yourresourcegroupname>`ve ardından aşağıdaki komutu çalıştırın:

```powershell
Revoke-AzDiskAccess -ResourceGroupName '<yourresourcegroupname>' -DiskName '<yourdiskname>'
```

## <a name="copy-a-managed-disk"></a>Yönetilen diski çoğaltma

Doğrudan yükleme, yönetilen bir diski kopyalama işlemini de kolaylaştırır. Aynı bölge içinde kopyalayabilir veya yönetilen diskinizi başka bir bölgeye kopyalayabilirsiniz.

Takip komut dosyası bunu sizin için yapar, işlem varolan bir diskle çalıştığınızdan, bazı farklılıklarla birlikte daha önce açıklanan adımlara benzer.

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

Artık yönetilen bir diske başarıyla bir VHD yüklediğinize göre, diskinizi bir VM'ye ekleyebilir ve kullanmaya başlayabilirsiniz.

VM'ye nasıl veri diski ekleyeceklerini öğrenmek için konuyla ilgili makalemize bakın: [PowerShell içeren bir Windows VM'ye veri diski takın.](attach-disk-ps.md) Diski işletim sistemi diski olarak kullanmak için [bkz.](create-vm-specialized.md#create-the-new-vm)
