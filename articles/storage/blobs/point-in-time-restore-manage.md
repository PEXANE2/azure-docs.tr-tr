---
title: Blok Blobları için noktadan noktaya geri yüklemeyi etkinleştirin ve yönetin (Önizleme)
titleSuffix: Azure Storage
description: Bir blok Blobları kümesini önceki bir duruma geri yüklemek için zaman noktası geri yükleme (Önizleme) özelliğini nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 09/11/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 140e1203a29dcebec9d6483e73e906591b2213fb
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90068542"
---
# <a name="enable-and-manage-point-in-time-restore-for-block-blobs-preview"></a>Blok Blobları için noktadan noktaya geri yüklemeyi etkinleştirin ve yönetin (Önizleme)

Blok Blobları kümesini önceki bir duruma geri yüklemek için zaman noktası geri yükleme (Önizleme) kullanabilirsiniz. Bu makalede, PowerShell ile bir depolama hesabı için noktadan noktaya geri yüklemenin nasıl etkinleştirileceği açıklanır. Ayrıca PowerShell ile geri yükleme işleminin nasıl gerçekleştirileceğini gösterir.

Daha fazla bilgi edinmek ve önizlemeye nasıl kaydolacağınızı öğrenmek için bkz. [blok Blobları Için noktadan noktaya geri yükleme (Önizleme)](point-in-time-restore-overview.md).

> [!CAUTION]
> Zaman içinde geri yükleme, yalnızca blok Bloblarındaki işlemleri geri yüklemeyi destekler. Kapsayıcılardaki işlemler geri yüklenemez. Bir kapsayıcıyı, zaman içinde geri yükleme önizlemesi sırasında [kapsayıcıyı silme](/rest/api/storageservices/delete-container) işlemini çağırarak depolama hesabından silerseniz, o kapsayıcı geri yükleme işlemiyle geri yüklenemez. Önizleme sırasında, bir kapsayıcıyı silmek, geri yüklemek istiyorsanız ayrı blob 'ları silin.

> [!IMPORTANT]
> Zaman içinde geri yükleme önizlemesi yalnızca üretim dışı kullanım için tasarlanmıştır.

## <a name="enable-and-configure-point-in-time-restore"></a>Zaman içinde nokta geri yüklemeyi etkinleştirme ve yapılandırma

Zaman içinde nokta geri yüklemeyi etkinleştirmeden ve yapılandırmadan önce, depolama hesabı için önkoşulları etkinleştirin: geçici silme, akış değiştirme ve BLOB sürümü oluşturma. Bu özelliklerin her birini etkinleştirme hakkında daha fazla bilgi için şu makalelere bakın:

- [Bloblar için geçici silmeyi etkinleştirme](soft-delete-enable.md)
- [Değişiklik akışını etkinleştirme ve devre dışı bırakma](storage-blob-change-feed.md#enable-and-disable-the-change-feed)
- [Blob sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md)

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Azure portal zaman içinde nokta geri yüklemeyi yapılandırmak için aşağıdaki adımları izleyin:

1. Azure portalda depolama hesabınıza gidin.
1. **Ayarlar**altında **veri koruma**' yı seçin.
1. Belirli bir noktaya geri yüklemeyi **Aç ' ı** seçin. Bu seçeneği belirlediğinizde Bloblar, sürüm oluşturma ve değişiklik akışı için geçici silme de etkinleştirilir.
1. Zaman içinde bir noktaya geri yükleme için en fazla geri yükleme noktasını gün cinsinden ayarlayın. Bu sayı, blob geçici silme için belirtilen saklama süresinden en az bir gün daha az olmalıdır.
1. Yaptığınız değişiklikleri kaydedin.

Aşağıdaki görüntüde, yedi gün önce geri yükleme noktası içeren bir zaman noktası geri yüklemesi için yapılandırılmış bir depolama hesabı ve BLOB geçici silme işlemi için 14 günlük bir bekletme dönemi gösterilmektedir.

:::image type="content" source="media/point-in-time-restore-manage/configure-point-in-time-restore-portal.png" alt-text="Azure portal zaman içinde nokta geri yükleme 'nin nasıl yapılandırılacağını gösteren ekran görüntüsü":::

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell ile noktadan noktaya geri yüklemeyi yapılandırmak için önce az. Storage Preview Module sürüm 1.14.1-Preview veya Preview modülünün sonraki bir sürümünü yükleyin. Az. Storage modülünün diğer sürümlerini kaldırın.

PowerShellGet 'in 2.2.4.1 veya sonraki bir sürümünü yüklediğinizden emin olun. Şu anda yüklü olan sürümü belirlemek için aşağıdaki komutu çalıştırın:

```powershell
Get-InstalledModule PowerShellGet
```

Sonra, az. Storage Preview modülünü Install. Aşağıdaki komut, az. Storage modülünün sürüm [2.5.2 Parallel sections-Preview](https://www.powershellgallery.com/packages/Az.Storage/2.5.2-preview) sürümünü yüklüyor:

```powershell
Install-Module -Name Az.Storage -RequiredVersion 2.5.2-preview -AllowPrerelease
```

Azure PowerShell yükleme hakkında daha fazla bilgi için bkz. [PowerShellGet](/powershell/scripting/gallery/installing-psget) ve [Install Azure PowerShell PowerShellGet ile](/powershell/azure/install-az-ps)yükleme.

Azure 'un anında yüklemesini PowerShell ile yapılandırmak için Enable-AzStorageBlobRestorePolicy komutunu çağırın. Aşağıdaki örnek, geçici silme ve geçici silme bekletme süresini ayarlar, akışı Değiştir ' i ve sonra da zaman içinde geri yüklemeyi sunar. Örneği çalıştırmadan önce, blob sürüm oluşturmayı etkinleştirmek için Azure portal veya bir Azure Resource Manager şablonunu kullanın.

Örneği çalıştırırken, açılı ayraçlar içindeki değerleri kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
# Sign in to your Azure account.
Connect-AzAccount

# Set resource group and account variables.
$rgName = "<resource-group>"
$accountName = "<storage-account>"

# Enable soft delete with a retention of 14 days.
Enable-AzStorageBlobDeleteRetentionPolicy -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -RetentionDays 14

# Enable change feed.
Update-AzStorageBlobServiceProperty -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -EnableChangeFeed $true

# Enable point-in-time restore with a retention period of 7 days.
# The retention period for point-in-time restore must be at least
# one day less than that set for soft delete.
Enable-AzStorageBlobRestorePolicy -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -RestoreDays 7

# View the service settings.
Get-AzStorageBlobServiceProperty -ResourceGroupName $rgName `
    -StorageAccountName $accountName
```

---

## <a name="perform-a-restore-operation"></a>Geri yükleme işlemi gerçekleştirme

Geri yükleme işlemi gerçekleştirdiğinizde geri yükleme noktasını UTC **Tarih saat** değeri olarak belirtmeniz gerekir. Kapsayıcılar ve Bloblar o gün ve zamanda durumlarına geri yüklenecek. Geri yükleme işleminin tamamlanması birkaç dakika sürebilir.

Depolama hesabındaki tüm kapsayıcıları geri yükleyebilir veya bir veya daha fazla kapsayıcıda bir dizi blobu geri yükleyebilirsiniz. Bir dizi blob, sözlük düzeninde sözcüıgrafik olarak tanımlanır. Her geri yükleme işlemi için On adede kadar lexıgraf aralığı desteklenir. Aralığın başlangıcı dahil değildir ve aralığın sonu hariç olur.

Başlangıç aralığı ve bitiş aralığı için belirtilen kapsayıcı deseninin en az üç karakter içermesi gerekir. Bir blob adından kapsayıcı adını ayırmak için kullanılan eğik çizgi (/), bu en düşük değere doğru sayılmaz.

Bir lexıgraf aralığında joker karakterler desteklenmez. Herhangi bir joker karakter standart karakter olarak değerlendirilir.

`$root` `$web` Geri yükleme işlemine geçirilen bir aralığa açıkça belirtilerek ve kapsayıcılardaki Blobları geri yükleyebilirsiniz. `$root`Ve `$web` kapsayıcıları yalnızca açık olarak belirtilmişse geri yüklenir. Diğer sistem kapsayıcıları geri yüklenemez.

Yalnızca blok Blobları geri yüklenir. Sayfa Blobları ve ekleme Blobları geri yükleme işlemine dahil edilmez. Ekleme Blobları ile ilgili sınırlamalar hakkında daha fazla bilgi için bkz. [bilinen sorunlar](#known-issues).

> [!IMPORTANT]
> Geri yükleme işlemi gerçekleştirdiğinizde, Azure depolama, işlem süresince geri yüklenen aralıklardaki bloblarda veri işlemlerini engeller. Birincil konumda okuma, yazma ve silme işlemleri engellenir. Bu nedenle, Azure portal kapsayıcıları gibi işlemler geri yükleme işlemi devam ederken beklendiği gibi gerçekleştirilemeyebilir.
>
> Depolama hesabı coğrafi olarak çoğaltılırsa, ikincil konumdaki okuma işlemleri geri yükleme işlemi sırasında devam edebilir.

### <a name="restore-all-containers-in-the-account"></a>Hesaptaki tüm kapsayıcıları geri yükle

Depolama hesabındaki tüm kapsayıcıları geri yükleyerek belirli bir zaman noktasında bunları önceki durumlarına getirebilirsiniz.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Depolama hesabındaki tüm kapsayıcıları ve Blobları Azure portal geri yüklemek için şu adımları izleyin:

1. Depolama hesabınızın kapsayıcı listesine gidin.
1. Araç çubuğunda **kapsayıcıları geri yükle**' yi ve ardından **Tümünü geri yükle**' yi seçin.
1. **Tüm kapsayıcıları geri yükle** bölmesinde, bir tarih ve saat sağlayarak geri yükleme noktasını belirtin.
1. Kutuyu işaretleyerek devam etmek istediğinizi onaylayın.
1. Geri yükleme işlemini başlatmak için **geri yükle** ' yi seçin.

    :::image type="content" source="media/point-in-time-restore-manage/restore-all-containers-portal.png" alt-text="Tüm kapsayıcıların belirtilen geri yükleme noktasına nasıl geri yükleneceğini gösteren ekran görüntüsü":::

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Depolama hesabındaki tüm kapsayıcıları ve Blobları PowerShell ile geri yüklemek için, parametresini atlayarak **restore-AzStorageBlobRange** komutunu çağırın `-BlobRestoreRange` . Aşağıdaki örnek, depolama hesabındaki kapsayıcıları, mevcut andan önceki durumuna 12 saat öncesine geri yükler:

```powershell
# Specify -TimeToRestore as a UTC value
Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddHours(-12)
```

---

### <a name="restore-ranges-of-block-blobs"></a>Blok bloblarının aralıklarını geri yükleme

Tek bir kapsayıcı içinde veya birden çok kapsayıcı içindeki blob türlerini bir veya daha fazla geri yükleyebilir ve bu Blobları belirli bir noktada önceki durumlarına döndürebilirsiniz.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Azure portal bir veya daha fazla kapsayıcıda bir blob aralığını geri yüklemek için şu adımları izleyin:

1. Depolama hesabınızın kapsayıcı listesine gidin.
1. Geri yüklenecek kapsayıcıyı veya kapsayıcıları seçin.
1. Araç çubuğunda **kapsayıcıları geri yükle**' yi seçin ve ardından seçili ' ı **geri yükleyin**.
1. **Seçili kapsayıcıları geri yükle** bölmesinde, bir tarih ve saat sağlayarak geri yükleme noktasını belirtin.
1. Geri yüklenecek aralıkları belirtin. Blob önekinden kapsayıcı adını belirtmek için bir eğik çizgi (/) kullanın.
1. Varsayılan olarak, **Seçili kapsayıcıları geri yükle** bölmesi, kapsayıcıdaki tüm Blobları içeren bir aralığı belirtir. Kapsayıcının tamamını geri yüklemek istemiyorsanız bu aralığı silin. Varsayılan Aralık aşağıdaki görüntüde gösterilmiştir.

    :::image type="content" source="media/point-in-time-restore-manage/delete-default-blob-range.png" alt-text="Özel Aralık belirtmeden önce silinecek varsayılan blob aralığını gösteren ekran görüntüsü":::

1. Kutuyu işaretleyerek devam etmek istediğinizi onaylayın.
1. Geri yükleme işlemini başlatmak için **geri yükle** ' yi seçin.

Aşağıdaki görüntüde bir dizi Aralık üzerinde geri yükleme işlemi gösterilmektedir.

:::image type="content" source="media/point-in-time-restore-manage/restore-multiple-container-ranges-portal.png" alt-text="Bir veya daha fazla kapsayıcıda blob aralıklarının nasıl geri yükleneceğini gösteren ekran görüntüsü":::

Görüntüde gösterilen geri yükleme işlemi aşağıdaki eylemleri gerçekleştirir:

- *Kapsayıcı1*'in tüm içeriğini geri yükler.
- *Container2*içindeki *blob5* ile lexıgraf Range *blob1* ile Blobları geri yükler. Bu Aralık, *blob1*, *blob11*, *blob100*, *blob2*vb. gibi adlara sahip Blobları geri yükler. Aralığın sonu dışlamalı olduğu için, adları *blob4*ile başlayan Blobları geri yükler, ancak adları *blob5*ile başlayan Blobları geri yüklemez.
- *Container3* ve *container4*içindeki tüm Blobları geri yükler. Aralığın sonu dışlamalı olduğundan, bu Aralık *container5*geri yüklemez.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Tek bir blob aralığını geri yüklemek için **restore-AzStorageBlobRange** komutunu çağırın ve parametresi için bir lexıgraf kapsayıcı ve BLOB adı belirtin `-BlobRestoreRange` . Örneğin, blob 'ları *örnek kapsayıcı*adlı tek bir kapsayıcıda geri yüklemek için *örnek kapsayıcla* başlayan ve *örnek-kapsayıcı1*ile biten bir Aralık belirtebilirsiniz. Başlangıç ve bitiş aralıklarında adlı kapsayıcılar için bir gereksinim yok. Aralığın sonu özel olduğundan, depolama hesabı *Sample-kapsayıcı1*adlı bir kapsayıcı içerse bile, yalnızca *örnek kapsayıcı* adlı kapsayıcı geri yüklenecek:

```powershell
$range = New-AzStorageBlobRangeToRestore -StartRange sample-container `
    -EndRange sample-container1
```

Geri yüklenecek bir kapsayıcıdaki Blobların bir alt kümesini belirtmek için, blob öneki düzeninden kapsayıcı adını ayırmak için bir eğik çizgi (/) kullanın. Örneğin, aşağıdaki Aralık, adları *d* ile *f*ile başlayan tek bir kapsayıcıdaki Blobları seçer:

```powershell
$range = New-AzStorageBlobRangeToRestore -StartRange sample-container/d `
    -EndRange sample-container/g
```

Ardından, **geri yükleme-AzStorageBlobRange** komutuna olan aralığı belirtin. Parametre için bir UTC **Tarih saat** değeri sağlayarak geri yükleme noktasını belirtin `-TimeToRestore` . Aşağıdaki örnek, belirtilen aralıktaki Blobları mevcut andan 3 güne geri yükler:

```powershell
# Specify -TimeToRestore as a UTC value
Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -BlobRestoreRange $range `
    -TimeToRestore (Get-Date).AddDays(-3)
```

Blok bloblarının birden çok aralığını geri yüklemek için, parametre için bir Aralık dizisi belirtin `-BlobRestoreRange` . Aşağıdaki örnek, *kapsayıcı1* ve *container4*' nin tüm içeriğini geri yüklemek için iki aralığı belirtir:

```powershell
# Specify a range that includes the complete contents of container1.
$range1 = New-AzStorageBlobRangeToRestore -StartRange container1 `
    -EndRange container2
# Specify a range that includes the complete contents of container4.
$range2 = New-AzStorageBlobRangeToRestore -StartRange container4 `
    -EndRange container5

Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddMinutes(-30) `
    -BlobRestoreRange @($range1, $range2)
```

---

### <a name="restore-block-blobs-asynchronously-with-powershell"></a>Blok bloblarını PowerShell ile zaman uyumsuz olarak geri yükleme

Geri yükleme işlemini zaman uyumsuz olarak çalıştırmak için, `-AsJob` **geri yükleme-AzStorageBlobRange** öğesine yapılan çağrıya parametreyi ekleyin ve çağrının sonucunu bir değişkende depolayın. **Restore-AzStorageBlobRange** komutu, **AzureLongRunningJob**türünde bir nesne döndürür. Geri yükleme işleminin tamamlanıp tamamlanmadığını anlamak için bu nesnenin **durum** özelliğini kontrol edebilirsiniz. **Durum** özelliğinin değeri **çalışıyor** veya **tamamlanmış**olabilir.

Aşağıdaki örnek, bir geri yükleme işleminin zaman uyumsuz olarak nasıl çağrılacağını göstermektedir:

```powershell
$job = Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddMinutes(-5) `
    -AsJob

# Check the state of the job.
$job.State
```

Geri yükleme işleminin tamamlanmasından sonra tamamlanmasını beklemek için, aşağıdaki örnekte gösterildiği gibi [wait-Job](/powershell/module/microsoft.powershell.core/wait-job) komutunu çağırın:

```powershell
$job | Wait-Job
```

## <a name="known-issues"></a>Bilinen sorunlar

Ekleme Blobları bulunan geri yükleme işlemlerinin bir alt kümesi için geri yükleme işlemi başarısız olur. Microsoft hesapta ekleme Blobları varsa, önizleme sırasında bir zaman içinde geri yükleme gerçekleştirmenizi önerir.

## <a name="next-steps"></a>Sonraki adımlar

- [Blok Blobları için noktadan noktaya geri yükleme (Önizleme)](point-in-time-restore-overview.md)
- [Geçici silme](soft-delete-overview.md)
- [Akışı Değiştir (Önizleme)](storage-blob-change-feed.md)
- [Blob sürümü oluşturma](versioning-overview.md)
