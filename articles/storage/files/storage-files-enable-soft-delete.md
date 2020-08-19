---
title: Geçici silme özelliğini etkinleştirme-Azure dosya paylaşımları
description: Veri kurtarma için Azure dosya paylaşımlarında geçici silme (Önizleme) özelliğini etkinleştirmeyi ve yanlışlıkla silmeyi engellemeyi öğrenin.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 05/28/2020
ms.author: rogarana
ms.subservice: files
services: storage
ms.openlocfilehash: 2d2a000879a95f86a6cdda3324add5b692476eee
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88590124"
---
# <a name="enable-soft-delete-on-azure-file-shares"></a>Azure dosya paylaşımlarında geçici silme özelliğini etkinleştirme

Azure depolama, bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından yanlışlıkla silindiğinde verilerinizi daha kolay kurtarabilmeniz için dosya paylaşımları (Önizleme) için geçici silme olanağı sağlar. Geçici silme hakkında daha fazla bilgi edinmek için bkz. [Azure dosya paylaşımlarının yanlışlıkla silinmesini engelleme](storage-files-prevent-file-share-deletion.md).

Aşağıdaki bölümlerde, var olan bir depolama hesabında Azure dosya paylaşımları için geçici silme 'nin nasıl etkinleştirileceği ve kullanılacağı gösterilmektedir:

# <a name="portal"></a>[Portal](#tab/azure-portal)

## <a name="getting-started"></a>Başlarken

1. [Azure portal](https://portal.azure.com/) oturum açın.
1. Depolama hesabınıza gidin ve **Dosya hizmeti**altında **geçici silme** ' yi seçin.
1. **Dosya paylaşma geçici silme**için **etkin** ' i seçin.
1. **Dosya paylaşma saklama süresini gün olarak** seçin ve seçtiğiniz bir sayıyı girin.
1. Veri saklama ayarlarınızı onaylamak için **Kaydet** ' i seçin.

:::image type="content" source="media/storage-how-to-recover-deleted-account/enable-soft-delete-files.png" alt-text="Depolama hesabı geçici silme ayarları bölmesinin ekran görüntüsü. Dosya paylaşımları bölümünü vurgulama, geçişi etkinleştirme, bekletme süresi ayarlama ve kaydetme. Bu, Depolama hesabınızdaki tüm dosya paylaşımları için geçici silme olanağı sağlar.":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

## <a name="prerequisite"></a>Önkoşul

Geçici silme cmdlet 'leri Şu anda yalnızca az. Storage modülünün [2.1.1-Preview](https://www.powershellgallery.com/packages/Az.Storage/2.1.1-preview) ve [2.3.1-Preview](https://www.powershellgallery.com/packages/Az.Storage/2.3.1-preview) sürümlerinde mevcuttur. 

## <a name="getting-started"></a>Başlarken

Geçici silme özelliğini etkinleştirmek için bir dosya istemcisinin hizmet özelliklerini güncelleştirmeniz gerekir. Aşağıdaki örnek, bir depolama hesabındaki tüm dosya paylaşımları için geçici silme imkanı sunar:

```azurepowershell-interactive
$rgName = "yourResourceGroupName"
$accountName = "yourStorageAccountName"

Update-AzStorageFileServiceProperty -ResourceGroupName $rgName -StorageAccountName $accountName -EnableShareDeleteRetentionPolicy $true -ShareRetentionDays 7
```

Geçici silmenin etkinleştirilip etkinleştirilmediğini doğrulayabilirsiniz ve bekletme ilkesini aşağıdaki komutla görüntüleyebilirsiniz:

```azurepowershell-interactive
Get-AzStorageFileServiceProperty -ResourceGroupName $rgName -StorageAccountName $accountName
```
---

## <a name="restore-soft-deleted-file-share"></a>Geçici olarak silinen dosya payını geri yükle

# <a name="portal"></a>[Portal](#tab/azure-portal)

Geçici olarak silinen bir dosya paylaşımının geri yüklenmesi için:

1. Depolama hesabınıza gidin ve **dosya paylaşımları**' nı seçin.
1. Dosya paylaşımı dikey penceresinde, geçici olarak silinen paylaşımları görüntülemek için **silinen paylaşımları göster** ' i etkinleştirin.

    Bu, şu anda **Silinmiş** durumdaki tüm paylaşımları görüntüler.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/undelete-file-share.png" alt-text="Durum sütunu, Ad sütununun yanındaki sütun silindi olarak ayarlandıysa, dosya paylaşımınız geçici olarak silinmiş durumda olur. Ve, belirtilen saklama süresinden sonra kalıcı olarak silinir.":::

1. Paylaşıma ve **silmeyi geri al**' ı seçtiğinizde, bu, paylaşımın geri yükleneceği

    Durumu **etkin**olarak geçiş yaptığından paylaşımın geri yüklendiğini doğrulayabilirsiniz.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/restored-file-share.png" alt-text="Durum sütunu, Ad sütununun yanındaki sütun etkin olarak ayarlanırsa, dosya paylaşımınız geri yüklendi.":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Geçici silme cmdlet 'leri, az. Storage modülünün 2.1.1-Preview sürümünde kullanılabilir. Geçici olarak silinen bir dosya paylaşımının geri yüklenmesi için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Restore-AzRmStorageShare -ResourceGroupName $rgname -StorageAccountName $accountName -DeletedShareVersion 01D5E2783BDCDA97
```
---

## <a name="disable-soft-delete"></a>Geçici silmeyi devre dışı bırak

Geçici silme kullanmayı durdurmak istiyorsanız veya bir dosya paylaşımının kalıcı olarak silinmesini istiyorsanız aşağıdaki yönergeleri izleyin:

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Depolama hesabınıza gidin ve **Ayarlar**altında **geçici silme** ' yi seçin.
1. Dosya **paylaşımları altında** **dosya paylaşımları Için geçici silme** **devre dışı** seçeneğini belirleyin.
1. Veri saklama ayarlarınızı onaylamak için **Kaydet** ' i seçin.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/disable-soft-delete-files.png" alt-text="Geçici silme devre dışı bırakıldığında, Depolama hesabınızdaki tüm dosya paylaşımlarını boş olarak hemen ve kalıcı olarak silebilirsiniz.":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Geçici silme cmdlet 'leri, az. Storage modülünün 2.1.1-Preview sürümünde kullanılabilir. Depolama hesabınızda geçici silme devre dışı bırakmak için aşağıdaki komutu kullanabilirsiniz:

```azurepowershell-interactive
Update-AzStorageFileServiceProperty -ResourceGroupName $rgName -StorageAccountName $accountName -EnableShareDeleteRetentionPolicy $false
```
---

## <a name="next-steps"></a>Sonraki adımlar

Başka bir veri koruma ve kurtarma biçimi hakkında bilgi edinmek için bkz. [Azure dosyaları için anlık görüntü paylaşma makalesine genel bakış](storage-snapshots-files.md).
