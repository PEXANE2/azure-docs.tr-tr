---
title: Blok Blobları için noktadan noktaya geri yükleme (Önizleme)
titleSuffix: Azure Storage
description: Blok Blobları için belirli bir noktaya geri yükleme, bir depolama hesabını belirli bir zamanda önceki durumuna geri yüklemenize olanak tanıyarak yanlışlıkla silinmeye veya bozulmalara karşı koruma sağlar.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 05/28/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: references_regions
ms.openlocfilehash: 513f0240296debb5e878461ed1ca7cffecad760a
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84463000"
---
# <a name="point-in-time-restore-for-block-blobs-preview"></a>Blok Blobları için noktadan noktaya geri yükleme (Önizleme)

Zaman içinde geri yükleme, Blok Blobu verilerini önceki bir duruma geri yüklemenize olanak tanıyarak yanlışlıkla silinmeye veya bozulmaya karşı koruma sağlar. Bir kullanıcının veya uygulamanın yanlışlıkla verileri sildiği veya bir uygulama hatasının verileri bozan senaryolarda, zaman içinde geri yükleme yararlı olur. Ayrıca, bir noktadan sonra geri yükleme, daha fazla testi çalıştırmadan önce bir veri kümesinin bilinen bir duruma geri döndürülmesi gereken test senaryolarına de olanak sağlar.

Bir depolama hesabı için zaman içinde geri yükleme yapmayı nasıl etkinleştireceğinizi öğrenmek için bkz. [blok Blobları (Önizleme) için noktadan noktaya geri yüklemeyi etkinleştirme ve yönetme](point-in-time-restore-manage.md).

## <a name="how-point-in-time-restore-works"></a>Zaman noktası geri yükleme nasıl çalışacaktır?

Noktadan noktaya geri yüklemeyi etkinleştirmek için, depolama hesabı için bir yönetim ilkesi oluşturup bir bekletme dönemi belirtirsiniz. Saklama süresi boyunca, blok bloblarını mevcut durumdan önceki bir zaman noktasına geri yükleyebilirsiniz.

Bir zaman noktası geri yüklemesi başlatmak için [BLOB aralıklarını geri yükleme](/rest/api/storagerp/storageaccounts/restoreblobranges) işlemini ÇAĞıRıN ve UTC zamanında bir geri yükleme noktası belirtin. Geri yüklemek için, kapsayıcı ve BLOB adlarının lexıgraf aralıklarını belirtebilir veya depolama hesabındaki tüm kapsayıcıları geri yüklemek için aralığı atlayabilirsiniz. Geri yükleme işlemi başına en fazla 10 lexıgraf aralığı desteklenir.

Azure depolama, istenen geri yükleme noktası arasındaki belirtilen bloblarda UTC saatine ve mevcut anda belirtilen tüm değişiklikleri analiz eder. Geri yükleme işlemi atomik olduğundan tüm değişiklikleri geri yükleme işleminde tamamen başarılı olur ya da başarısız olur. Geri yüklenemeyecek blob varsa, işlem başarısız olur ve etkilenen kapsayıcılar sürdürülmesine okuma ve yazma işlemleri devam ettirir.

Tek seferde bir depolama hesabında yalnızca bir geri yükleme işlemi çalıştırılabilir. Devam eden bir geri yükleme işlemi iptal edilemez, ancak ilk işlemi geri almak için ikinci bir geri yükleme işlemi gerçekleştirilebilir.

**BLOB aralıklarını geri yükleme** işlemi, işlemi benzersiz şekilde tanımlayan bir GERI yükleme kimliği döndürüyor. Bir zaman noktası geri yükleme işleminin durumunu denetlemek için, **BLOB aralıklarını geri** yükle işleminden döndürülen GERI yükleme kimliği Ile **geri yükleme durumunu Al** işlemini çağırın.

Geri yükleme işlemlerinde aşağıdaki sınırlamaları aklınızda bulundurun:

- [URL 'den](/rest/api/storageservices/put-block-from-url) [PUT bloğu veya](/rest/api/storageservices/put-block) put bloğu aracılığıyla karşıya yüklenen ancak [PUT blok listesi](/rest/api/storageservices/put-block-list)aracılığıyla kaydedilmemiş bir blok, blob 'un bir parçası değildir ve bu nedenle geri yükleme işleminin bir parçası olarak geri yüklenmez.
- Etkin kiralamaya sahip bir blob geri yüklenemez. Etkin bir kiralamaya sahip bir blob geri yüklenecek blob aralığına dahil edilmezse geri yükleme işlemi otomatik olarak başarısız olur.
- Anlık görüntüler, geri yükleme işleminin bir parçası olarak oluşturulmaz veya silinmez. Yalnızca temel blob önceki durumuna geri yüklendi.
- Bir blob, mevcut bir süre ve geri yükleme noktası arasındaki dönemdeki sık ve seyrek katmanlar arasında taşınırsa, blob önceki katmanına geri yüklenir. Ancak, arşiv katmanına taşınan bir blob geri yüklenmez.

> [!IMPORTANT]
> Geri yükleme işlemi gerçekleştirdiğinizde, Azure depolama, işlem süresince geri yüklenen aralıklardaki bloblarda veri işlemlerini engeller. Birincil konumda okuma, yazma ve silme işlemleri engellenir. Bu nedenle, Azure portal kapsayıcıları gibi işlemler geri yükleme işlemi devam ederken beklendiği gibi gerçekleştirilemeyebilir.
>
> Depolama hesabı coğrafi olarak çoğaltılırsa, ikincil konumdaki okuma işlemleri geri yükleme işlemi sırasında devam edebilir.

> [!CAUTION]
> Zaman içinde geri yükleme, yalnızca blok Bloblarındaki işlemleri geri yüklemeyi destekler. Kapsayıcılardaki işlemler geri yüklenemez. Bir kapsayıcıyı, zaman içinde geri yükleme önizlemesi sırasında [kapsayıcıyı silme](/rest/api/storageservices/delete-container) işlemini çağırarak depolama hesabından silerseniz, o kapsayıcı geri yükleme işlemiyle geri yüklenemez. Önizleme sırasında, bir kapsayıcıyı silmek, geri yüklemek istiyorsanız ayrı blob 'ları silin.

### <a name="prerequisites-for-point-in-time-restore"></a>Zaman içinde nokta geri yükleme önkoşulları

Zaman içinde geri yükleme, aşağıdaki Azure depolama özelliklerinin etkinleştirilmesini gerektirir:

- [Geçici silme](soft-delete-overview.md)
- [Akışı Değiştir (Önizleme)](storage-blob-change-feed.md)
- [Blob sürümü oluşturma (Önizleme)](versioning-overview.md)

Noktadan noktaya geri yüklemeyi etkinleştirmeden önce depolama hesabı için bu özellikleri etkinleştirin. Etkinleştirmeden önce değişiklik akışına ve BLOB sürüm önizlemesine kaydolduğunuzdan emin olun.

### <a name="retention-period-for-point-in-time-restore"></a>Zaman içinde bir noktaya geri yükleme için bekletme süresi

Bir depolama hesabı için noktadan noktaya geri yüklemeyi etkinleştirdiğinizde bir bekletme dönemi belirtirsiniz. Depolama hesabınızdaki blok Blobları Bekletme dönemi sırasında geri yüklenebilir.

Bekletme süresi, zaman içinde geri yüklemeyi etkinleştirdiğinizde başlar. Blob 'ları bekletme döneminin başlangıcından önceki bir duruma geri yükleyemezsiniz. Örneğin, 1 Mayıs 'ta 30 günlük bir bekletme tarihi ile bir süre sonra geri yükleme etkinleştirdiyseniz, 15 Mayıs 'ta en fazla 15 güne geri yükleme yapabilirsiniz. 1 Haziran 'da verileri 1 ila 30 gün içinde geri yükleyebilirsiniz.

Belirli bir noktaya geri yükleme için bekletme süresi, geçici silme için belirtilen saklama süresinden en az bir gün daha az olmalıdır. Örneğin, geçici silme Bekletme dönemi 7 gün olarak ayarlandıysa, zaman içinde geri yükleme saklama süresi 1 ile 6 gün arasında olabilir.

### <a name="permissions-for-point-in-time-restore"></a>Noktadan noktaya geri yükleme izinleri

Geri yükleme işlemini başlatmak için, bir istemcinin depolama hesabındaki tüm kapsayıcılar için yazma izinlerine sahip olması gerekir. Azure Active Directory (Azure AD) ile bir geri yükleme işlemini yetkilendirmek için izin vermek üzere depolama hesabı, kaynak grubu veya abonelik düzeyinde güvenlik sorumlusuna **depolama hesabı katılımcısı** rolünü atayın.

## <a name="about-the-preview"></a>Önizleme hakkında

Tek noktaya geri yükleme yalnızca genel amaçlı v2 depolama hesapları için desteklenir. Yalnızca sık ve Seyrek Erişimli katmanlardaki veriler, zaman içinde geri yükleme ile geri yüklenebilir.

Aşağıdaki bölgeler önizlemede bir noktaya geri yüklemeyi destekler:

- Orta Kanada
- Doğu Kanada
- Orta Fransa

Önizleme aşağıdaki sınırlamaları içerir:

- Premium blok bloblarının geri yüklenmesi desteklenmez.
- Arşiv katmanındaki blobları geri yükleme desteklenmez. Örneğin, sık erişim katmanındaki bir blob iki gün önce arşiv katmanına taşınmışsa ve geri yükleme işlemi üç gün önceki bir noktaya geri yüklüyorsa blob sık erişim katmanına geri yüklenmez.
- Azure Data Lake Storage 2. düz ve hiyerarşik ad alanlarını geri yükleme desteklenmez.
- Müşteri tarafından belirtilen anahtarlar kullanılarak depolama hesaplarının geri yüklenmesi desteklenmez.

> [!IMPORTANT]
> Zaman içinde geri yükleme önizlemesi yalnızca üretim dışı kullanım için tasarlanmıştır. Üretim hizmet düzeyi sözleşmeleri (SLA 'Lar) Şu anda kullanılamıyor.

### <a name="register-for-the-preview"></a>Önizlemeye kaydolun

Önizlemeye kaydolmak için aşağıdaki komutları çalıştırın:

# <a name="powershell"></a>[PowerShell](#tab/powershell)
```powershell
# Register for the point-in-time restore preview
Register-AzProviderFeature -FeatureName RestoreBlobRanges -ProviderNamespace Microsoft.Storage

# Register for change feed (preview)
Register-AzProviderFeature -FeatureName Changefeed -ProviderNamespace Microsoft.Storage

# Register for blob versioning (preview)
Register-AzProviderFeature -FeatureName Versioning -ProviderNamespace Microsoft.Storage

# Refresh the Azure Storage provider namespace
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```
# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
az feature register --namespace Microsoft.Storage --name RestoreBlobRanges
az feature register --namespace Microsoft.Storage --name Changefeed
az feature register --namespace Microsoft.Storage --name Versioning
az provider register --namespace 'Microsoft.Storage'
```

---

### <a name="check-registration-status"></a>Kayıt durumunu denetle

Zaman içinde nokta geri yükleme kaydı otomatiktir ve 10 dakikadan kısa sürer. Kaydlarınızın durumunu denetlemek için aşağıdaki komutları çalıştırın:

# <a name="powershell"></a>[PowerShell](#tab/powershell)
```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName RestoreBlobRanges

Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Changefeed
    
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Versioning
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/RestoreBlobRanges')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/Changefeed')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/Versioning')].{Name:name,State:properties.state}"
```

---


## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Bir noktadan noktaya geri yükleme için faturalandırma, geri yükleme işlemini gerçekleştirmek için işlenen veri miktarına bağlıdır. İşlenen veri miktarı, geri yükleme noktası ve var olan süre arasında gerçekleşen değişiklik sayısına göre belirlenir. Örneğin, bir depolama hesabındaki blob verilerini engellemek için görece sabit bir değişiklik oranının olduğunu varsayarsak, 1 gün içinde geri kalan geri yükleme işlemi 10 gün sonra geri kalan bir geri yükleme maliyeti 1/10 ' dur.

Geri yükleme işleminin maliyetini tahmin etmek için, geri yükleme döneminde değiştirilen veri miktarını tahmin etmek üzere değişiklik akışı günlüğünü gözden geçirin. Örneğin, değişiklik akışı için bekletme süresi 30 gün ise ve değişiklik akışı boyutu 10 MB ise, daha önce bir noktaya geri yükleme, o bölgedeki bir LRS hesabı için listelenen fiyatın yaklaşık bir üçte biri üzerinden ücretlendirilir. Daha önce 27 gün olan bir noktaya geri yükleme, listelenen fiyatın yaklaşık dokuz-onmalinin maliyetlidir.

Zaman içinde nokta geri yükleme fiyatlandırması hakkında daha fazla bilgi için bkz. [Blok Blobu fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="ask-questions-or-provide-feedback"></a>Soru sorun veya geri bildirim sağlayın

Zaman içinde geri yükleme önizlemesi hakkında sorular sormak veya geri bildirim sağlamak için, Microsoft ile iletişim kurun pitrdiscussion@microsoft.com .

## <a name="next-steps"></a>Sonraki adımlar

- [Blok Blobları için noktadan noktaya geri yüklemeyi etkinleştirin ve yönetin (Önizleme)](point-in-time-restore-manage.md)
- [Azure Blob depolamada akış desteğini değiştirme (Önizleme)](storage-blob-change-feed.md)
- [Bloblar için geçici silmeyi etkinleştir](soft-delete-enable.md)
- [Blob sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md)
