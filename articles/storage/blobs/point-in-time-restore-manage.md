---
title: Blok Blobları için noktadan noktaya geri yüklemeyi etkinleştirin ve yönetin (Önizleme)
titleSuffix: Azure Storage
description: Blok bloblarını zaman içinde daha önceki bir noktada bir duruma geri yüklemek için zaman noktası geri yükleme (Önizleme) özelliğini nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/28/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: fe98e04c37172dc6b91c86fab8200022ed860d4f
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170112"
---
# <a name="enable-and-manage-point-in-time-restore-for-block-blobs-preview"></a>Blok Blobları için noktadan noktaya geri yüklemeyi etkinleştirin ve yönetin (Önizleme)

Blok bloblarını zaman içinde daha önceki bir noktadaki durumlarına geri yüklemek için zaman noktası geri yükleme (Önizleme) kullanabilirsiniz. Bu makalede, PowerShell ile bir depolama hesabı için noktadan noktaya geri yüklemenin nasıl etkinleştirileceği açıklanır. Ayrıca PowerShell ile geri yükleme işleminin nasıl gerçekleştirileceğini gösterir.

Daha fazla bilgi edinmek ve önizlemeye nasıl kaydolacağınızı öğrenmek için bkz. [blok Blobları Için noktadan noktaya geri yükleme (Önizleme)](point-in-time-restore-overview.md).

> [!CAUTION]
> Zaman içinde geri yükleme, yalnızca blok Bloblarındaki işlemleri geri yüklemeyi destekler. Kapsayıcılardaki işlemler geri yüklenemez. Bir kapsayıcıyı, zaman içinde geri yükleme önizlemesi sırasında [kapsayıcıyı silme](/rest/api/storageservices/delete-container) işlemini çağırarak depolama hesabından silerseniz, o kapsayıcı geri yükleme işlemiyle geri yüklenemez. Önizleme sırasında, bir kapsayıcıyı silmek, geri yüklemek istiyorsanız ayrı blob 'ları silin.

> [!IMPORTANT]
> Zaman içinde geri yükleme önizlemesi yalnızca üretim dışı kullanım için tasarlanmıştır. Üretim hizmet düzeyi sözleşmeleri (SLA 'Lar) Şu anda kullanılamıyor.

## <a name="install-the-preview-module"></a>Önizleme modülünü yükler

Azure 'un anında yükleme zaman yüklemesini PowerShell ile yapılandırmak için, ilk yükleme sürümü [1.14.1-](https://www.powershellgallery.com/packages/Az.Storage/1.14.1-preview) daha az. Storage PowerShell modülünün önizlemesini yükleyin. Önizleme modülünü yüklemek için aşağıdaki adımları izleyin:

1. **Ayarlar**altındaki **uygulamalar & Özellikler** ayarını kullanarak Windows 'un önceki Azure PowerShell yüklemelerini kaldırın.

1. PowerShellGet 'in en son sürümüne sahip olduğunuzdan emin olun. Bir Windows PowerShell penceresi açın ve en son sürümü yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

1. PowerShellGet yükledikten sonra PowerShell penceresini kapatın ve yeniden açın.

1. Azure PowerShell en son sürümünü yükler:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Az. Storage Preview modülünü Install:

    ```powershell
    Install-Module Az.Storage -Repository PSGallery -RequiredVersion 1.14.1-preview -AllowPrerelease -AllowClobber -Force
    ```

Azure PowerShell yükleme hakkında daha fazla bilgi için bkz. [PowerShellGet Ile yükleme Azure PowerShell](/powershell/azure/install-az-ps).

## <a name="enable-and-configure-point-in-time-restore"></a>Zaman içinde nokta geri yüklemeyi etkinleştirme ve yapılandırma

Zaman içinde nokta geri yüklemeyi etkinleştirmeden ve yapılandırmadan önce, önkoşullarını etkinleştirin: geçici silme, akış değiştirme ve BLOB sürümü oluşturma. Bu özelliklerin her birini etkinleştirme hakkında daha fazla bilgi için şu makalelere bakın:

- [Bloblar için geçici silmeyi etkinleştir](soft-delete-enable.md)
- [Değişiklik akışını etkinleştirme ve devre dışı bırakma](storage-blob-change-feed.md#enable-and-disable-the-change-feed)
- [Blob sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md)

Azure 'un anında yüklemesini PowerShell ile yapılandırmak için Enable-AzStorageBlobRestorePolicy komutunu çağırın. Aşağıdaki örnek, geçici silme ve geçici silme bekletme süresini ayarlar, akışı Değiştir ' i ve sonra da zaman içinde geri yüklemeyi sunar. Örneği çalıştırmadan önce, blob sürüm oluşturmayı etkinleştirmek için Azure portal veya bir Azure Resource Manager şablonunu kullanın.

Örneği çalıştırırken, açılı ayraçlar içindeki değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
# Sign in to your Azure account.
Connect-AzAccount

# Set resource group and account variables.
$rgName = "<resource-group>"
$accountName = "<storage-account>"

# Enable soft delete with a retention of 6 days.
Enable-AzStorageBlobDeleteRetentionPolicy -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -RetentionDays 6

# Enable change feed.
Update-AzStorageBlobServiceProperty -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -EnableChangeFeed $true

# Enable point-in-time restore with a retention period of 5 days.
# The retention period for point-in-time restore must be at least one day less than that set for soft delete.
Enable-AzStorageBlobRestorePolicy -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -RestoreDays 5

# View the service settings.
Get-AzStorageBlobServiceProperty -ResourceGroupName $rgName `
    -StorageAccountName $accountName
```

## <a name="perform-a-restore-operation"></a>Geri yükleme işlemi gerçekleştirme

Geri yükleme işlemini başlatmak için restore-AzStorageBlobRange komutunu çağırın ve geri yükleme noktasını UTC **Tarih saat** değeri olarak belirtin. Geri yüklemek için lexıgrafik aralıklarını belirtebilir veya depolama hesabındaki tüm kapsayıcılardaki tüm Blobları geri yüklemek için bir aralığı atlayabilirsiniz. Geri yükleme işlemi başına en fazla 10 lexıgraf aralığı desteklenir. Geri yükleme işleminin tamamlanması birkaç dakika sürebilir.

Geri yüklenecek bir blob aralığı belirtirken aşağıdaki kuralları aklınızda bulundurun:

- Başlangıç aralığı ve bitiş aralığı için belirtilen kapsayıcı deseninin en az üç karakter içermesi gerekir. Bir blob adından kapsayıcı adını ayırmak için kullanılan eğik çizgi (/), bu en düşük değere doğru sayılmaz.
- Geri yükleme işlemi başına en fazla 10 Aralık belirtilebilir.
- Joker karakterler desteklenmez. Bunlar standart karakter olarak kabul edilir.
- `$root` `$web` Geri yükleme işlemine geçirilen bir aralığa açıkça belirtilerek ve kapsayıcılardaki Blobları geri yükleyebilirsiniz. `$root`Ve `$web` kapsayıcıları yalnızca açık olarak belirtilmişse geri yüklenir. Diğer sistem kapsayıcıları geri yüklenemez.

> [!IMPORTANT]
> Geri yükleme işlemi gerçekleştirdiğinizde, Azure depolama, işlem süresince geri yüklenen aralıklardaki bloblarda veri işlemlerini engeller. Birincil konumda okuma, yazma ve silme işlemleri engellenir. Bu nedenle, Azure portal kapsayıcıları gibi işlemler geri yükleme işlemi devam ederken beklendiği gibi gerçekleştirilemeyebilir.
>
> Depolama hesabı coğrafi olarak çoğaltılırsa, ikincil konumdaki okuma işlemleri geri yükleme işlemi sırasında devam edebilir.

### <a name="restore-all-containers-in-the-account"></a>Hesaptaki tüm kapsayıcıları geri yükle

Depolama hesabındaki tüm kapsayıcıları ve Blobları geri yüklemek için, parametresini atlayarak restore-AzStorageBlobRange komutunu çağırın `-BlobRestoreRange` . Aşağıdaki örnek, depolama hesabındaki kapsayıcıları, mevcut andan önceki durumuna 12 saat öncesine geri yükler:

```powershell
# Specify -TimeToRestore as a UTC value
Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddHours(-12)
```

### <a name="restore-a-single-range-of-block-blobs"></a>Blok bloblarının tek bir aralığını geri yükleme

Blob 'ların bir aralığını geri yüklemek için restore-AzStorageBlobRange komutunu çağırın ve parametresi için bir lexicografik aralığı ve BLOB adı belirtin `-BlobRestoreRange` . Aralığın başlangıcı dahil değildir ve aralığın sonu özeldir.

Örneğin, blob 'ları *örnek kapsayıcı*adlı tek bir kapsayıcıda geri yüklemek için *örnek kapsayıcla* başlayan ve *örnek-kapsayıcı1*ile biten bir Aralık belirtebilirsiniz. Başlangıç ve bitiş aralıklarında adlı kapsayıcılar için bir gereksinim yok. Aralığın sonu özel olduğundan, depolama hesabı *Sample-kapsayıcı1*adlı bir kapsayıcı içerse bile, yalnızca *örnek kapsayıcı* adlı kapsayıcı geri yüklenecek:

```powershell
$range = New-AzStorageBlobRangeToRestore -StartRange sample-container -EndRange sample-container1
```

Geri yüklenecek bir kapsayıcıdaki Blobların bir alt kümesini belirtmek için, blob düzeninden kapsayıcı adını ayırmak için eğik çizgi (/) kullanın. Örneğin, aşağıdaki Aralık, adları *d* ile *f*ile başlayan tek bir kapsayıcıdaki Blobları seçer:

```powershell
$range = New-AzStorageBlobRangeToRestore -StartRange sample-container/d -EndRange sample-container/g
```

Ardından, geri yükleme-AzStorageBlobRange komutuna olan aralığı belirtin. Parametre için bir UTC **Tarih saat** değeri sağlayarak geri yükleme noktasını belirtin `-TimeToRestore` . Aşağıdaki örnek, belirtilen aralıktaki Blobları mevcut andan 3 güne geri yükler:

```powershell
# Specify -TimeToRestore as a UTC value
Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -BlobRestoreRange $range `
    -TimeToRestore (Get-Date).AddDays(-3)
```

### <a name="restore-multiple-ranges-of-block-blobs"></a>Blok bloblarının birden çok aralığını geri yükleme

Blok bloblarının birden çok aralığını geri yüklemek için, parametre için bir Aralık dizisi belirtin `-BlobRestoreRange` . Geri yükleme işlemi başına en fazla 10 Aralık desteklenir. Aşağıdaki örnek, *kapsayıcı1* ve *container4*' nin tüm içeriğini geri yüklemek için iki aralığı belirtir:

```powershell
$range1 = New-AzStorageBlobRangeToRestore -StartRange container1 -EndRange container2
$range2 = New-AzStorageBlobRangeToRestore -StartRange container4 -EndRange container5

Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddMinutes(-30) `
    -BlobRestoreRange @($range1, $range2)
```

## <a name="next-steps"></a>Sonraki adımlar

- [Blok Blobları için noktadan noktaya geri yükleme (Önizleme)](point-in-time-restore-overview.md)
- [Geçici silme](soft-delete-overview.md)
- [Akışı Değiştir (Önizleme)](storage-blob-change-feed.md)
- [Blob sürümü oluşturma (Önizleme)](versioning-overview.md)
