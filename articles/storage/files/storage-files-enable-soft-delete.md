---
title: Geçici silme özelliğini etkinleştirme-Azure dosya paylaşımları
description: Veri kurtarma için Azure dosya paylaşımlarında geçici silme özelliğini etkinleştirmeyi ve yanlışlıkla silmeyi engellemeyi öğrenin.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 05/28/2020
ms.author: rogarana
ms.subservice: files
services: storage
ms.openlocfilehash: 7defa8611080027a67a0d1db1daa4c4a9d44edfe
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93126150"
---
# <a name="enable-soft-delete-on-azure-file-shares"></a>Azure dosya paylaşımlarında geçici silme özelliğini etkinleştirme

Azure depolama, dosya paylaşımları için geçici silme olanağı sunarak, verileri yanlışlıkla bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından daha kolay kurtarabilirsiniz. Geçici silme hakkında daha fazla bilgi edinmek için bkz. [Azure dosya paylaşımlarının yanlışlıkla silinmesini engelleme](storage-files-prevent-file-share-deletion.md).

Aşağıdaki bölümlerde, var olan bir depolama hesabında Azure dosya paylaşımları için geçici silme 'nin nasıl etkinleştirileceği ve kullanılacağı gösterilmektedir:

# <a name="portal"></a>[Portal](#tab/azure-portal)

## <a name="getting-started"></a>Başlarken

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. Depolama hesabınıza gidin ve **Dosya hizmeti** altında **geçici silme** ' yi seçin.
1. **Dosya paylaşma geçici silme** için **etkin** ' i seçin.
1. **Dosya paylaşma saklama süresini gün olarak** seçin ve seçtiğiniz bir sayıyı girin.
1. Veri saklama ayarlarınızı onaylamak için **Kaydet** ' i seçin.

:::image type="content" source="media/storage-how-to-recover-deleted-account/enable-soft-delete-files.png" alt-text="Depolama hesabı geçici silme ayarları bölmesinin ekran görüntüsü. Dosya paylaşımları bölümünü vurgulama, geçişi etkinleştirme, bekletme süresi ayarlama ve kaydetme. Bu, Depolama hesabınızdaki tüm dosya paylaşımları için geçici silme olanağı sağlar.":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

## <a name="prerequisite"></a>Önkoşul

Geçici silme cmdlet 'leri az. Storage modülünün [3.0.0](https://www.powershellgallery.com/packages/Az.Storage/3.0.0) sürümünde kullanılabilir. 

## <a name="getting-started-with-powershell"></a>PowerShell ile çalışmaya başlayın

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

1. Depolama hesabınıza gidin ve **dosya paylaşımları** ' nı seçin.
1. Dosya paylaşımı dikey penceresinde, geçici olarak silinen paylaşımları görüntülemek için **silinen paylaşımları göster** ' i etkinleştirin.

    Bu, şu anda **Silinmiş** durumdaki tüm paylaşımları görüntüler.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/undelete-file-share.png" alt-text="Depolama hesabı geçici silme ayarları bölmesinin ekran görüntüsü. Dosya paylaşımları bölümünü vurgulama, geçişi etkinleştirme, bekletme süresi ayarlama ve kaydetme. Bu, Depolama hesabınızdaki tüm dosya paylaşımları için geçici silme olanağı sağlar.":::

1. Paylaşıma ve **silmeyi geri al** ' ı seçtiğinizde, bu, paylaşımın geri yükleneceği

    Durumu **etkin** olarak geçiş yaptığından paylaşımın geri yüklendiğini doğrulayabilirsiniz.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/restored-file-share.png" alt-text="Depolama hesabı geçici silme ayarları bölmesinin ekran görüntüsü. Dosya paylaşımları bölümünü vurgulama, geçişi etkinleştirme, bekletme süresi ayarlama ve kaydetme. Bu, Depolama hesabınızdaki tüm dosya paylaşımları için geçici silme olanağı sağlar.":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Geçici silme cmdlet 'leri az. Storage modülünün 3.0.0 sürümünde kullanılabilir. Geçici olarak silinen bir dosya paylaşımının geri yüklenmesi için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Restore-AzRmStorageShare -ResourceGroupName $rgname -StorageAccountName $accountName -DeletedShareVersion 01D5E2783BDCDA97
```
---

## <a name="disable-soft-delete"></a>Geçici silmeyi devre dışı bırak

Geçici silme kullanmayı durdurmak istiyorsanız veya bir dosya paylaşımının kalıcı olarak silinmesini istiyorsanız aşağıdaki yönergeleri izleyin:

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Depolama hesabınıza gidin ve **Ayarlar** altında **geçici silme** ' yi seçin.
1. Dosya **paylaşımları altında** **dosya paylaşımları Için geçici silme** **devre dışı** seçeneğini belirleyin.
1. Veri saklama ayarlarınızı onaylamak için **Kaydet** ' i seçin.

    :::image type="content" source="media/storage-how-to-recover-deleted-account/disable-soft-delete-files.png" alt-text="Depolama hesabı geçici silme ayarları bölmesinin ekran görüntüsü. Dosya paylaşımları bölümünü vurgulama, geçişi etkinleştirme, bekletme süresi ayarlama ve kaydetme. Bu, Depolama hesabınızdaki tüm dosya paylaşımları için geçici silme olanağı sağlar.":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Geçici silme cmdlet 'leri az. Storage modülünün 3.0.0 sürümünde kullanılabilir. Depolama hesabınızda geçici silme devre dışı bırakmak için aşağıdaki komutu kullanabilirsiniz:

```azurepowershell-interactive
Update-AzStorageFileServiceProperty -ResourceGroupName $rgName -StorageAccountName $accountName -EnableShareDeleteRetentionPolicy $false
```
---

## <a name="next-steps"></a>Sonraki adımlar

Başka bir veri koruma ve kurtarma biçimi hakkında bilgi edinmek için bkz. [Azure dosyaları için anlık görüntü paylaşma makalesine genel bakış](storage-snapshots-files.md).
