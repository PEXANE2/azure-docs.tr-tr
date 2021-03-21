---
title: Kapsayıcılar için geçici silme (Önizleme)
titleSuffix: Azure Storage
description: Kapsayıcılar için geçici silme (Önizleme), verilerinizi, bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından yanlışlıkla değiştirildiğinde veya silindiğinde verilerinizi daha kolay bir şekilde kurtarabilmeniz için korur.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 03/05/2021
ms.author: tamram
ms.subservice: blobs
ms.custom: references_regions
ms.openlocfilehash: f157b44e92289d0e9c5b88108550c144344c5206
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102211150"
---
# <a name="soft-delete-for-containers-preview"></a>Kapsayıcılar için geçici silme (Önizleme)

Kapsayıcılar için geçici silme (Önizleme), verilerinizin yanlışlıkla veya kötü amaçlı olarak silinmesini önler. Bir depolama hesabı için kapsayıcı geçici silme etkinleştirildiğinde, silinen tüm kapsayıcı ve içerikleri, belirttiğiniz süre boyunca Azure Storage 'da tutulur. Saklama süresi boyunca, daha önce silinen kapsayıcıları geri yükleyebilirsiniz. Kapsayıcının geri yüklenmesi, bu kapsayıcıdaki tüm Blobları silindiği zaman geri yükler.

Blob verilerinize yönelik uçtan uca koruma için, Microsoft aşağıdaki veri koruma özelliklerini etkinleştirmeyi önerir:

- Silinen bir kapsayıcıyı geri yüklemek için kapsayıcı geçici silme. Kapsayıcı geçici silmeyi etkinleştirme hakkında bilgi edinmek için bkz. [kapsayıcılar için geçici silmeyi etkinleştirme ve yönetme](soft-delete-container-enable.md).
- Blob 'un önceki sürümlerini otomatik olarak sürdürmek için blob sürümü oluşturma. Blob sürümü oluşturma etkinleştirildiğinde, yanlışlıkla değiştirildiyse veya silinirse verilerinizi kurtarmak için bir Blobun önceki bir sürümünü geri yükleyebilirsiniz. Blob sürümü oluşturmayı nasıl etkinleştireceğinizi öğrenmek için bkz. [BLOB sürüm oluşturmayı etkinleştirme ve yönetme](versioning-enable.md).
- Silinen bir Blobu veya sürümü geri yüklemek için blob geçici silme. Blob geçici silme özelliğini etkinleştirmeyi öğrenmek için bkz. [Bloblar için geçici silmeyi etkinleştirme ve yönetme](soft-delete-blob-enable.md).

> [!IMPORTANT]
> Kapsayıcı geçici silme Şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan yasal koşullara yönelik [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

## <a name="how-container-soft-delete-works"></a>Kapsayıcı geçici silme nasıl kullanılır?

Kapsayıcı geçici silmeyi etkinleştirdiğinizde, 1 ila 365 gün arasında silinen kapsayıcılar için bir saklama süresi belirtebilirsiniz. Varsayılan saklama süresi 7 gündür. Saklama süresi boyunca, **kapsayıcıyı geri yükleme** işlemini çağırarak silinen kapsayıcıyı kurtarabilirsiniz.

Bir kapsayıcıyı geri yüklediğinizde, kapsayıcının blob 'ları ve BLOB sürümleri de geri yüklenir. Ancak, kapsayıcının kendisi silinmişse Blobları geri yüklemek için yalnızca kapsayıcı geçici silme özelliğini kullanabilirsiniz. Silinen bir blobu, üst kapsayıcısı silinmediği zaman geri yüklemek için blob geçici silme veya blob sürümü oluşturma kullanmanız gerekir.

Aşağıdaki diyagramda, kapsayıcı geçici silme etkinken silinen kapsayıcının nasıl geri yüklenebildiğinden gösterilmektedir:

:::image type="content" source="media/soft-delete-container-overview/container-soft-delete-diagram.png" alt-text="Geçici olarak silinen kapsayıcının nasıl geri yükleneolabileceğini gösteren diyagram":::

Bir kapsayıcıyı geri yüklediğinizde, bu ad yeniden kullanılmazsa özgün adına geri yükleyebilirsiniz. Özgün kapsayıcı adı kullanılmışsa, kapsayıcıyı yeni bir adla geri yükleyebilirsiniz.

Saklama süresi dolduktan sonra kapsayıcı, Azure depolama 'dan kalıcı olarak silinir ve kurtarılamaz. Saat, kapsayıcının silindiği noktada bekletme döneminde başlar. Saklama süresini dilediğiniz zaman değiştirebilirsiniz, ancak güncelleştirilmiş bir bekletme döneminin yalnızca yeni silinen kapsayıcılar için geçerli olduğunu aklınızda bulundurun. Daha önce silinen kapsayıcılar, kapsayıcının silindiği sırada geçerli olan saklama süresine göre kalıcı olarak silinir.

Geçici silme kapsayıcısını devre dışı bırakmak, daha önce geçici olarak silinmiş kapsayıcıların kalıcı olarak silinmesine neden olmaz. Geçici olarak silinen kapsayıcılar, kapsayıcının silindiği sırada geçerli olan bekletme döneminin sona erdiğinde kalıcı olarak silinir.

> [!IMPORTANT]
> Kapsayıcı geçici silme, bir depolama hesabının silinmesine karşı koruma sağlamaz, ancak yalnızca söz konusu hesaptaki kapsayıcıların silinmesine karşı değildir. Bir depolama hesabını silinmeye karşı korumak için, depolama hesabı kaynağında bir kilit yapılandırın. Azure Resource Manager kaynaklarını kilitleme hakkında daha fazla bilgi için bkz. [kaynakları kilitleme, beklenmeyen değişiklikleri önleme](../../azure-resource-manager/management/lock-resources.md).

## <a name="about-the-preview"></a>Önizleme hakkında

Kapsayıcı geçici silme, tüm Azure bölgelerinde önizleme olarak kullanılabilir.

Azure Storage 'ın 2019-12-12 veya üzeri sürümleri, kapsayıcı geçici silmeyi destekler REST API.

### <a name="storage-account-support"></a>Depolama hesabı desteği

Kapsayıcı geçici silme, aşağıdaki depolama hesabı türleri için kullanılabilir:

- Genel amaçlı v2 ve v1 depolama hesapları
- BLOB depolama hesaplarını engelle
- Blob Storage hesapları

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
- [Blob sürümü oluşturma](versioning-overview.md)
