---
title: Kapsayıcılar için geçici silme (Önizleme)
titleSuffix: Azure Storage
description: Kapsayıcılar için geçici silme (Önizleme), verilerinizi, bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından yanlışlıkla değiştirildiğinde veya silindiğinde verilerinizi daha kolay bir şekilde kurtarabilmeniz için korur.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 08/25/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: references_regions
ms.openlocfilehash: 60c7ac6a86c963a4a133f06ba6d9d602cb9090d0
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88854535"
---
# <a name="soft-delete-for-containers-preview"></a>Kapsayıcılar için geçici silme (Önizleme)

Kapsayıcılar için geçici silme (Önizleme), verilerinizin yanlışlıkla veya yanlışlıkla değiştirilmesini veya silinmesini önler. Bir depolama hesabı için kapsayıcı geçici silme etkinleştirildiğinde, silinen tüm kapsayıcı ve içerikleri, belirttiğiniz süre boyunca Azure Storage 'da tutulur. Saklama süresi boyunca, daha önce silinen kapsayıcıları ve bunların içindeki blob 'ları geri yükleyebilirsiniz.

Blob verilerinize yönelik uçtan uca koruma için, Microsoft aşağıdaki veri koruma özelliklerini etkinleştirmeyi önerir:

- Kapsayıcı geçici silme, bir kapsayıcının yanlışlıkla silinmesine veya üzerine yazılmasına karşı koruma için. Kapsayıcı geçici silmeyi etkinleştirme hakkında bilgi edinmek için bkz. [kapsayıcılar için geçici silmeyi etkinleştirme ve yönetme](soft-delete-container-enable.md).
- Blob geçici silme, tek bir Blobun yanlışlıkla silinmeye veya üzerine yazılmasına karşı koruma sağlamak için. Blob geçici silme özelliğini etkinleştirmeyi öğrenmek için bkz. [Bloblar Için geçici silme](soft-delete-blob-overview.md).
- Blob 'un önceki sürümlerini otomatik olarak sürdürmek için blob sürümü oluşturma (Önizleme). Blob sürümü oluşturma etkinleştirildiğinde, yanlışlıkla değiştirildiyse veya silinirse verilerinizi kurtarmak için bir Blobun önceki bir sürümünü geri yükleyebilirsiniz. Blob sürümü oluşturmayı nasıl etkinleştireceğinizi öğrenmek için bkz. [BLOB sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md).

> [!WARNING]
> Depolama hesabını silme işlemi geri alınamaz. Geçici silme, bir depolama hesabının silinmesine karşı koruma sağlamaz. Bir depolama hesabının yanlışlıkla silinmesini engellemek için, depolama hesabı kaynağında bir **Cannotdelete** kilidi yapılandırın. Azure kaynaklarını kilitleme hakkında daha fazla bilgi için, bkz. [beklenmeyen değişiklikleri engellemek için kaynakları kilitleme](../../azure-resource-manager/management/lock-resources.md).

## <a name="how-container-soft-delete-works"></a>Kapsayıcı geçici silme nasıl kullanılır?

Kapsayıcı geçici silmeyi etkinleştirdiğinizde, 1 ila 365 gün arasında silinen kapsayıcılar için bir saklama süresi belirtebilirsiniz. Varsayılan saklama süresi 7 gündür. Saklama süresi boyunca, silme **kapsayıcısını geri** alma işlemini çağırarak silinen kapsayıcıyı kurtarabilirsiniz.

Bir kapsayıcıyı geri yüklediğinizde, bu ad yeniden kullanılmazsa özgün adına geri yükleyebilirsiniz. Özgün kapsayıcı adı kullanılmışsa, kapsayıcıyı yeni bir adla geri yükleyebilirsiniz.

Saklama süresi dolduktan sonra kapsayıcı, Azure depolama 'dan kalıcı olarak silinir ve kurtarılamaz. Saat, kapsayıcının silindiği noktada bekletme döneminde başlar. Saklama süresini dilediğiniz zaman değiştirebilirsiniz, ancak güncelleştirilmiş bir bekletme döneminin yalnızca yeni silinen kapsayıcılar için geçerli olduğunu aklınızda bulundurun. Daha önce silinen kapsayıcılar, kapsayıcının silindiği sırada geçerli olan saklama süresine göre kalıcı olarak silinir.

Geçici silme kapsayıcısını devre dışı bırakmak, daha önce geçici olarak silinmiş kapsayıcıların kalıcı olarak silinmesine neden olmaz. Geçici olarak silinen kapsayıcılar, kapsayıcının silindiği sırada geçerli olan bekletme döneminin sona erdiğinde kalıcı olarak silinir.

## <a name="about-the-preview"></a>Önizleme hakkında

Kapsayıcı geçici silme, önizleme aşamasında aşağıdaki bölgelerde kullanılabilir:

- Orta Fransa
- Doğu Kanada
- Orta Kanada

> [!IMPORTANT]
> Kapsayıcı geçici silme önizlemesi yalnızca üretim dışı kullanım için tasarlanmıştır. Üretim hizmet düzeyi sözleşmeleri (SLA 'Lar) Şu anda kullanılamıyor.

Azure depolama REST API sürüm 2019-12-12 ve üzeri, kapsayıcı geçici silmeyi destekler.

### <a name="storage-account-support"></a>Depolama hesabı desteği

Kapsayıcı geçici silme, aşağıdaki depolama hesabı türleri için kullanılabilir:

- Genel amaçlı v2 depolama hesapları
- BLOB depolama hesaplarını engelle
- Blob Storage hesapları

Depolama Hesabınız genel amaçlı bir v1 hesabıdır, genel amaçlı v2 hesabına yükseltmek için Azure portal kullanın. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md).

Azure Data Lake Storage 2. ile kullanım için etkinleştirilmiş hiyerarşik bir ad alanı olan depolama hesapları da desteklenir.

### <a name="register-for-the-preview"></a>Önizlemeye kaydolun

Kapsayıcı geçici silme önizlemesine kaydolmak için, özelliği aboneliğinize kaydetme isteği göndermek üzere PowerShell veya Azure CLı kullanın. İsteğiniz onaylandıktan sonra, yeni veya mevcut genel amaçlı v2, BLOB depolama veya Premium Blok Blobu depolama hesaplarıyla kapsayıcı geçici silmeyi etkinleştirebilirsiniz.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell 'e kaydolmak için [register-AzProviderFeature](/powershell/module/az.resources/register-azproviderfeature) komutunu çağırın.

```powershell
# Register for container soft delete (preview)
Register-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName ContainerSoftDelete

# Refresh the Azure Storage provider namespace
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile kaydolmak için [az Feature Register](/cli/azure/feature#az-feature-register) komutunu çağırın.

```azurecli
az feature register --namespace Microsoft.Storage --name ContainerSoftDelete
az provider register --namespace 'Microsoft.Storage'
```

---

### <a name="check-the-status-of-your-registration"></a>Kaydlarınızın durumunu denetleyin

Kaydlarınızın durumunu denetlemek için PowerShell veya Azure CLı kullanın.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Kayıt durumunuzu PowerShell 'e göre denetlemek için [Get-AzProviderFeature](/powershell/module/az.resources/get-azproviderfeature) komutunu çağırın.

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName ContainerSoftDelete
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile kaydlarınızın durumunu denetlemek için [az Feature](/cli/azure/feature#az-feature-show) komutunu çağırın.

```azurecli
az feature show --namespace Microsoft.Storage --name ContainerSoftDelete
```

---

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Kapsayıcı geçici silme özelliğini etkinleştirmek için ek ücret alınmaz. Geçici olarak silinen kapsayıcılardaki veriler, etkin verilerle aynı hızda faturalandırılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Kapsayıcıyı geçici silme yapılandırması](soft-delete-container-enable.md)
- [Bloblar için geçici silme](soft-delete-blob-overview.md)
- [Blob sürümü oluşturma (Önizleme)](versioning-overview.md)
