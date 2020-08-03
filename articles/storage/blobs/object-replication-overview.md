---
title: Nesne çoğaltmasına genel bakış (Önizleme)
titleSuffix: Azure Storage
description: Nesne çoğaltma (Önizleme), blok bloblarını bir kaynak depolama hesabı ve hedef hesap arasında zaman uyumsuz olarak kopyalar. Okuma isteklerindeki gecikme süresini en aza indirmek için, işlem iş yüklerinin verimliliğini artırmak, veri dağıtımını iyileştirmek ve maliyetleri en aza indirmek için nesne çoğaltmasını kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 05/28/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-azurecli
ms.openlocfilehash: 097b3c71b01a8ad0e930d7aa0d7be46a1e835e1a
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87495174"
---
# <a name="object-replication-for-block-blobs-preview"></a>Blok Blobları için nesne çoğaltma (Önizleme)

Nesne çoğaltma (Önizleme), blok bloblarını bir kaynak depolama hesabı ve hedef hesap arasında zaman uyumsuz olarak kopyalar. Nesne çoğaltma tarafından desteklenen bazı senaryolar şunlardır:

- **En aza indirme gecikmesi.** Nesne çoğaltma, istemcilerin daha yakın bir fiziksel yakınlık alanında bulunan bir bölgeden veri tüketmesini sağlayarak okuma istekleri için gecikme süresini azaltabilir.
- **İşlem iş yükleri için verimliliği artırın.** İşlem iş yükleri, nesne çoğaltma ile farklı bölgelerde aynı blok blob kümelerini işleyebilir.
- **Veri dağıtımını iyileştirme.** Verileri tek bir konumda işleyebilir veya analiz edebilir ve sonra yalnızca sonuçları ek bölgelere çoğaltabilirsiniz.
- **Maliyetleri iyileştirme.** Verileriniz çoğaltıldıktan sonra, yaşam döngüsü yönetimi ilkelerini kullanarak bunu arşiv katmanına taşıyarak maliyetleri azaltabilirsiniz.

Aşağıdaki diyagramda, nesne çoğaltmanın, blok bloblarını bir bölgedeki kaynak depolama hesabından iki farklı bölgedeki hedef hesaplara nasıl çoğaltacağını gösterir.

:::image type="content" source="media/object-replication-overview/object-replication-diagram.svg" alt-text="Nesne çoğaltmasının nasıl çalıştığını gösteren diyagram":::

Nesne çoğaltmasını yapılandırma hakkında bilgi edinmek için bkz. [nesne çoğaltmasını yapılandırma (Önizleme)](object-replication-configure.md).

## <a name="object-replication-policies-and-rules"></a>Nesne çoğaltma ilkeleri ve kuralları

Nesne çoğaltmasını yapılandırdığınızda, kaynak depolama hesabı ve hedef hesap belirten bir çoğaltma ilkesi oluşturursunuz. Çoğaltma İlkesi, kaynak kapsayıcısını ve hedef kapsayıcıyı belirten bir veya daha fazla kural içerir ve kaynak kapsayıcısındaki hangi blok bloblarının çoğaltılacağı anlamına gelir.

Nesne çoğaltmasını yapılandırdıktan sonra Azure depolama, kaynak hesabın değişiklik akışını düzenli aralıklarla denetler ve yazma veya silme işlemlerini zaman uyumsuz olarak hedef hesaba çoğaltır. Çoğaltma gecikmesi, çoğaltılan blok Blobun boyutuna bağlıdır.

> [!IMPORTANT]
> Blok Blobu verileri zaman uyumsuz olarak çoğaltıldığından, kaynak hesap ve hedef hesap hemen eşitlenmiş durumda değildir. Şu anda, hedef hesaba veri çoğaltmak için ne kadar süreceğine ilişkin bir SLA yoktur.

### <a name="replications-policies"></a>Çoğaltmalar ilkeleri

Nesne çoğaltmasını yapılandırdığınızda, Azure depolama kaynak sağlayıcısı aracılığıyla hem kaynak hesapta hem de hedef hesapta bir çoğaltma ilkesi oluşturulur. Çoğaltma İlkesi bir ilke KIMLIĞI tarafından tanımlanır. Çoğaltmanın gerçekleşmesi için kaynak ve hedef hesapların ilke KIMLIĞI aynı olmalıdır.

Bir depolama hesabı, en fazla iki hedef hesap için kaynak hesap olarak görev yapabilir. Ve bir hedef hesabın ikiden fazla kaynak hesabı olamaz. Kaynak ve hedef hesaplar farklı bölgelerde bulunabilir. Hedef hesapların her birine veri çoğaltmak için ayrı çoğaltma ilkeleri yapılandırabilirsiniz.

### <a name="replication-rules"></a>Çoğaltma kuralları

Çoğaltma kuralları, Azure depolama 'nın blob 'ları bir kaynak kapsayıcısından hedef kapsayıcıya nasıl çoğaltacağınızı belirtir. Her çoğaltma ilkesi için en fazla 10 çoğaltma kuralı belirtebilirsiniz. Her kural tek bir kaynak ve hedef kapsayıcı tanımlar ve her kaynak ve hedef kapsayıcı yalnızca bir kuralda kullanılabilir.

Bir çoğaltma kuralı oluşturduğunuzda, varsayılan olarak yalnızca kaynak kapsayıcıya daha sonra eklenen yeni blok Blobları kopyalanır. Ayrıca, hem yeni hem de var olan blok bloblarının kopyalanmasını belirtebilir veya belirli bir zamandan itibaren oluşturulan blok bloblarını kopyalayan özel bir kopya kapsamı tanımlayabilirsiniz.

Ayrıca, blok bloblarını ön eke göre filtrelemek için bir çoğaltma kuralının bir parçası olarak bir veya daha fazla filtre belirtebilirsiniz. Bir ön ek belirttiğinizde, yalnızca kaynak kapsayıcıda bu önekle eşleşen Bloblar hedef kapsayıcıya kopyalanacaktır.

Kaynak ve hedef kapsayıcıların her ikisi de bir kuralda belirtmeleri için mevcut olmalıdır. Çoğaltma ilkesini oluşturduktan sonra hedef kapsayıcı salt okunur duruma gelir. Hedef kapsayıcıya yazma girişimleri hata kodu 409 (Çakışma) vererek başarısız olur. Ancak, [BLOB katmanını ayarla](/rest/api/storageservices/set-blob-tier) işlemini arşiv katmanına taşımak için hedef kapsayıcıdaki bir blob üzerinde çağırabilirsiniz. Arşiv katmanı hakkında daha fazla bilgi için bkz. [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](storage-blob-storage-tiers.md#archive-access-tier).

## <a name="about-the-preview"></a>Önizleme hakkında

Nesne çoğaltma yalnızca genel amaçlı v2 depolama hesapları için desteklenir. Nesne çoğaltma, önizleme sürümünde aşağıdaki bölgelerde kullanılabilir:

- Orta Fransa
- Doğu Kanada
- Orta Kanada
- ABD Doğu 2
- ABD Orta

Kaynak ve hedef hesapların her ikisi de, nesne çoğaltmasını kullanmak için bu bölgelerden birinde bulunmalıdır. Hesaplar iki farklı bölgede olabilir.

Önizleme sırasında, depolama hesapları arasında veri çoğaltma ile ilişkili ek maliyet yoktur.

> [!IMPORTANT]
> Nesne çoğaltma önizlemesi yalnızca üretim dışı kullanım için tasarlanmıştır. Üretim hizmet düzeyi sözleşmeleri (SLA 'Lar) Şu anda kullanılamıyor.

### <a name="prerequisites-for-object-replication"></a>Nesne çoğaltma için önkoşullar

Nesne çoğaltma, aşağıdaki Azure depolama özelliklerinin etkinleştirilmesini gerektirir: 
- [Akışı Değiştir](storage-blob-change-feed.md)
- [Sürüm oluşturma](versioning-overview.md)

Nesne çoğaltmasını yapılandırmadan önce, önkoşullarını etkinleştirin. Kaynak hesapta değişiklik akışı etkinleştirilmelidir ve hem kaynak hem de hedef hesapta blob sürümü oluşturma etkinleştirilmiş olmalıdır. Bu özellikleri etkinleştirme hakkında daha fazla bilgi için şu makalelere bakın:

- [Değişiklik akışını etkinleştirme ve devre dışı bırakma](storage-blob-change-feed.md#enable-and-disable-the-change-feed)
- [Blob sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md)

Etkinleştirmeden önce değişiklik akışına ve BLOB sürüm önizlemesine kaydolduğunuzdan emin olun.

Değişiklik akışı ve BLOB sürümü oluşturma özelliğinin etkinleştirilmesi ek ücret ödemeniz gerekebilir. Daha fazla ayrıntı için [Azure Depolama fiyatlandırması sayfasına](https://azure.microsoft.com/pricing/details/storage/)bakın.

### <a name="register-for-the-preview"></a>Önizlemeye kaydolun

PowerShell veya Azure CLı kullanarak nesne çoğaltma önizlemesine kaydolabilirsiniz. Henüz yapmadıysanız değişiklik akışı ve BLOB sürüm oluşturma önizlemeleri için de kaydolduğunuzdan emin olun.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell ile önizlemeye kaydolmak için aşağıdaki komutları çalıştırın:

```powershell
# Register for the object replication preview
Register-AzProviderFeature -FeatureName AllowObjectReplication -ProviderNamespace Microsoft.Storage

# Register for change feed (preview)
Register-AzProviderFeature -FeatureName Changefeed -ProviderNamespace Microsoft.Storage

# Register for blob versioning (preview)
Register-AzProviderFeature -FeatureName Versioning -ProviderNamespace Microsoft.Storage

# Refresh the Azure Storage provider namespace
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile önizlemeye kaydolmak için aşağıdaki komutları çalıştırın:

```azurecli
az feature register --namespace Microsoft.Storage --name AllowObjectReplication
az feature register --namespace Microsoft.Storage --name Changefeed
az feature register --namespace Microsoft.Storage --name Versioning
az provider register --namespace 'Microsoft.Storage'
```

---

### <a name="check-registration-status"></a>Kayıt durumunu denetle

PowerShell veya Azure CLı kullanarak kayıt isteklerinizin durumunu kontrol edebilirsiniz.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell kullanarak kayıt isteklerinizin durumunu denetlemek için aşağıdaki komutları çalıştırın:

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowObjectReplication

Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Changefeed

Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Versioning
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı kullanarak kayıt isteklerinizin durumunu denetlemek için aşağıdaki komutları çalıştırın:

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/AllowObjectReplication')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/Changefeed')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.Storage/Versioning')].{Name:name,State:properties.state}"
```

---

## <a name="ask-questions-or-provide-feedback"></a>Soru sorun veya geri bildirim sağlayın

Nesne çoğaltma önizlemesi hakkında sorular sormak veya geri bildirim sağlamak için, Microsoft ile iletişim kurun AzureStorageFeedback@microsoft.com . Azure depolama ile ilgili fikirler ve öneriler [Azure Storage geri bildirim Forumu](https://feedback.azure.com/forums/217298-storage)' nda her zaman sağlanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Nesne çoğaltmasını yapılandırma (Önizleme)](object-replication-configure.md)
- [Azure Blob depolamada akış desteğini değiştirme (Önizleme)](storage-blob-change-feed.md)
- [Blob sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md)
