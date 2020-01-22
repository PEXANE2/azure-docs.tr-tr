---
title: Azure CLı ile Azure dosya paylaşımlarını geri yükleme
description: Azure CLı kullanarak kurtarma hizmetleri kasasındaki yedeklenen Azure dosya paylaşımlarını geri yükleme hakkında bilgi edinin
ms.topic: conceptual
ms.date: 01/16/2020
ms.openlocfilehash: 8d91031ff9ee0d4f32cc74ff7031d39ddab3e2b4
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294415"
---
# <a name="restore-azure-file-shares-with-cli"></a>CLı ile Azure dosya paylaşımlarını geri yükleme

Azure komut satırı arabirimi (CLı), Azure kaynaklarını yönetmek için bir komut satırı deneyimi sağlar. Azure kaynaklarını kullanmak için özel otomasyon oluşturmaya yönelik harika bir araçtır. Bu makalede, Azure CLı kullanılarak [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview) hizmeti tarafından oluşturulan bir geri yükleme noktasından tüm dosya paylaşımının veya belirli dosyaların nasıl geri yükleneceği açıklanmaktadır. Bu adımları [Azure PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-afs-automation) veya [Azure portalı](backup-afs.md) ile de gerçekleştirebilirsiniz.

Bu öğreticinin sonuna kadar, Azure CLı ile aşağıdaki işlemleri gerçekleştirmeyi öğreneceksiniz:

* Yedeklenen bir Azure dosya paylaşımının geri yükleme noktalarını görüntüleme
* Tam Azure dosya paylaşımının geri yüklenmesi
* Tek tek dosya veya klasörleri geri yükleme

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yı yerel ortamda yüklemek ve kullanmak için Azure CLI sürüm 2.0.18 veya üzeri çalıştırmanız gerekir. CLı sürümünü bulmak için `run az --version`. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yi yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, Azure Backup hizmeti tarafından yedeklenen bir Azure dosya paylaşımınız olduğu varsayılır. Bir tane yoksa, dosya paylaşımınızla ilgili yedeklemeyi yapılandırmak için bkz. [CLI Ile Azure dosya paylaşımlarını yedekleme](backup-afs-cli.md) . Bu makalede aşağıdaki kaynakları kullanacağız:

| Dosya Paylaşımı  | Depolama Hesabı | Bölge | Ayrıntılar                                                      |
| ----------- | --------------- | ------ | ------------------------------------------------------------ |
| *azurefiles*  | *afsaccount*      | Doğu ABD | Özgün kaynak Azure Backup kullanılarak yedeklendi                 |
| *azurefiles1* | *afaccount1*      | Doğu ABD | Alternatif konum kurtarma için kullanılan hedef kaynak |

Bu belgede açıklanan farklı geri yüklemeler türlerini denemek için dosya paylaşımlarınız için de benzer bir yapı kullanabilirsiniz.

## <a name="fetch-recovery-points-for-the-azure-file-share"></a>Azure dosya paylaşımının kurtarma noktalarını getir

Yedeklenen dosya paylaşımının tüm kurtarma noktalarını listelemek için [az Backup RecoveryPoint List](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-list) cmdlet 'ini kullanın.

Aşağıdaki örnek, *afsaccount* depolama hesabındaki *azurefiles* dosya paylaşımının kurtarma noktaları listesini getirir.

```azurecli-interactive
az backup recoverypoint list --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --backup-management-type azurestorage --item-name “AzureFileShare;azurefiles” --workload-type azurefileshare --out table
```

Ayrıca aşağıdaki iki ek parametreyi sağlayarak kapsayıcı ve öğe için "kolay ad" i kullanarak yukarıdaki cmdlet 'i çalıştırabilirsiniz:

* **--Backup-Management-Type**: *azurestorage*
* **--iş yükü-tür**: *azurefileshare*

```azurecli-interactive
az backup recoverypoint list --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --backup-management-type azurestorage --item-name azurefiles --workload-type azurefileshare --out table
```

Sonuç kümesi, her bir geri yükleme noktası için zaman ve tutarlılık ayrıntıları olan kurtarma noktalarının bir listesi olacaktır.

```output
Name                Time                        Consistency
------------------  -------------------------   --------------------
932887541532871865  2020-01-05T07:08:23+00:00   FileSystemConsistent
932885927361238054  2020-01-05T07:08:10+00:00   FileSystemConsistent
932879614553967772  2020-01-04T21:33:04+00:00   FileSystemConsistent
```

Çıktıdaki **ad** özniteliği, kurtarma işlemlerinde **--RP-Name** parametresi için değer olarak kullanılabilecek kurtarma noktası adına karşılık gelir.

## <a name="full-share-recovery-using-azure-cli"></a>Azure CLı kullanarak tam paylaşma kurtarması

Özgün dosya paylaşımının tamamını veya alternatif bir konuma geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

Geri yükleme işlemlerini gerçekleştirmek için aşağıdaki parametreleri tanımlayın:

* **--kapsayıcı-adı** , yedeklenen özgün dosya paylaşımının barındırıldığı depolama hesabının adıdır. Kapsayıcının **adını** veya **kolay adını** almak için [az Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--öğe-adı** , geri yükleme işlemi için kullanmak istediğiniz yedeklenen orijinal dosya paylaşımının adıdır. Yedeklenen öğenin **adını** veya **kolay adını** almak için [az Backup Item List](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.

### <a name="restore-full-share-to-the-original-location"></a>Tam paylaşımın özgün konumuna geri yükleme

Özgün bir konuma geri yüklediğinizde, hedefle ilgili parametreleri belirtmeniz gerekmez. Yalnızca **çakışmayı çözümle** sağlanmalıdır.

Aşağıdaki örnek, [Azure dosya paylaşımının dağıtım noktalarını getir](#fetch-recovery-points-for-the-azure-file-share)ile elde ettiğimiz 932883129628959823 kurtarma noktasını kullanarak, *azurefiles* dosya paylaşımının özgün konumda geri *yüklenmesi için geri* yükleme modu ile [az Backup Restore restore-azurefileshare](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefileshare) cmdlet 'ini kullanır:

```azurecli-interactive
az backup restore restore-azurefileshare --vault-name azurefilesvault --resource-group azurefiles --rp-name 932887541532871865   --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --item-name “AzureFileShare;azurefiles” --restore-mode originallocation --resolve-conflict overwrite --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
6a27cc23-9283-4310-9c27-dcfb81b7b4bb  azurefiles
```

Çıkışdaki **ad** özniteliği, "geri yükleme" işlemi için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

### <a name="restore-full-share-to-an-alternate-location"></a>Tam paylaşımdan farklı bir konuma geri yükleme

Bu seçeneği, bir dosya paylaşımının alternatif bir konuma geri yüklenmesi ve özgün dosya paylaşımının olduğu gibi tutmanız için kullanabilirsiniz. Alternatif konum kurtarma için aşağıdaki parametreleri belirtin:

* **--target-Storage-Account**: yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **--target-File-Share**: yedeklenen içeriğin geri yüklendiği hedef depolama hesabı içindeki dosya paylaşma.
* **--target-Folder**: verilerin geri yüklendiği dosya paylaşımının altındaki klasör. Yedeklenen içerik bir kök klasöre geri yüklenirse, hedef klasör değerlerini boş bir dize olarak verin.
* **--Resolve-Conflict**: geri yüklenen verilerle bir çakışma varsa yönerge. **Overwrite** veya **Skip**kabul eder.

Aşağıdaki örnekte, *afsaccount* depolama hesabındaki *azurefiles* dosya paylaşımının *afaccount1* depolama hesabındaki *azurefiles1 "* dosya paylaşımında geri yüklenmesi için [az Backup Restore restore-azurefileshare](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefileshare) WITH restore Mode as *alternatelocation* olarak kullanılmaktadır.

```azurecli-interactive
az backup restore restore-azurefileshare --vault-name azurefilesvault --resource-group azurefiles --rp-name 932883129628959823 --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --item-name “AzureFileShare;azurefiles” --restore-mode alternatelocation --target-storage-account afaccount1 --target-file-share azurefiles1 --target-folder restoredata --resolve-conflict overwrite --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
babeb61c-d73d-4b91-9830-b8bfa83c349a  azurefiles
```

Çıkışdaki **ad** özniteliği, "geri yükleme" işlemi için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

## <a name="item-level-recovery"></a>Öğe düzeyinde kurtarma

Özgün veya alternatif konumdaki tek tek dosyaları veya klasörleri geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

Geri yükleme işlemlerini gerçekleştirmek için aşağıdaki parametreleri tanımlayın:

* **--kapsayıcı-adı** , yedeklenen özgün dosya paylaşımının barındırıldığı depolama hesabının adıdır. Kapsayıcının **adını** veya **kolay adını** almak için [az Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--öğe-adı** , geri yükleme işlemi için kullanmak istediğiniz Yedeklenen özgün dosya paylaşımının adıdır. Yedeklenen öğenin **adını** veya **kolay adını** almak için [az Backup Item List](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.

Kurtarmak istediğiniz öğeler için aşağıdaki parametreleri belirtin:

* **SourceFilePath**: dosya paylaşımında, bir dize olarak geri yüklenecek dosyanın mutlak yolu. Bu yol, [az Storage File Download](https://docs.microsoft.com/cli/azure/storage/file?view=azure-cli-latest#az-storage-file-download) veya [az Storage File CLI Show](https://docs.microsoft.com/cli/azure/storage/file?view=azure-cli-latest#az-storage-file-show) CLI komutlarında kullanılan yoldur.
* **Sourcefiletype**: bir dizin veya dosya seçili olup olmadığını seçin. **Dizini** veya **dosyayı**kabul eder.
* **ResolveConflict**: geri yüklenen verilerle bir çakışma varsa yönerge. **Overwrite** veya **Skip**kabul eder.

### <a name="restore-individual-filesfolders-to-the-original-location"></a>Dosyaları/klasörleri özgün konuma geri yükleme

Belirli dosyaları/klasörleri orijinal konumlarına geri yüklemek için, geri yükleme modu ile [az Backup Restore restore-azurefiles](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefiles) cmdlet 'ini, *originallocation* olarak ayarlayın.

Aşağıdaki örnek özgün konumdaki *"Restoretest. txt"* dosyasını geri yükler: *azurefiles* FileShare.

```azurecli-interactive
az backup restore restore-azurefiles --vault-name azurefilesvault --resource-group azurefiles --rp-name 932881556234035474 --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --item-name “AzureFileShare;azurefiles” --restore-mode originallocation  --source-file-type file --source-file-path "Restore/RestoreTest.txt" --resolve-conflict overwrite  --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
df4d9024-0dcb-4edc-bf8c-0a3d18a25319  azurefiles
```

Çıkışdaki **ad** özniteliği, "geri yükleme" işlemi için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

### <a name="restore-individual-filesfolders-to-an-alternate-location"></a>Tek tek dosyaları/klasörleri alternatif bir konuma geri yükleme

Belirli dosyaları/klasörleri alternatif bir konuma geri yüklemek için, [az Backup Restore restore-azurefiles](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefiles) cmdlet 'ini geri yükleme modu *alternatelocation* olarak ayarlayın ve hedefle ilgili aşağıdaki parametreleri belirtin:

* **--target-Storage-Account**: yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **--target-File-Share**: yedeklenen içeriğin geri yüklendiği hedef depolama hesabı içindeki dosya paylaşma.
* **--target-Folder**: verilerin geri yüklendiği dosya paylaşımının altındaki klasör. Yedeklenen içerik bir kök klasöre geri yüklenirse, hedef klasörün değerini boş bir dize olarak verin.

Aşağıdaki örnek, *azurefiles* FileShare içinde özgün olarak bulunan *restoretest. txt* dosyasını alternatif bir konuma geri yükler: *afaccount1* depolama hesabında barındırılan *azurefiles1* FileShare içindeki *restoredata* klasörü.

```azurecli-interactive
az backup restore restore-azurefiles --vault-name azurefilesvault --resource-group azurefiles --rp-name 932881556234035474 --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --item-name “AzureFileShare;azurefiles” --restore-mode alternatelocation --target-storage-account afaccount1 --target-file-share azurefiles1 --target-folder restoredata --resolve-conflict overwrite --source-file-type file --source-file-path "Restore/RestoreTest.txt" --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
df4d9024-0dcb-4edc-bf8c-0a3d18a25319  azurefiles
```

Çıkışdaki **ad** özniteliği, "geri yükleme" işlemi için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Azure [CLI Ile Azure dosya paylaşımının yedeklerini yönetme](manage-afs-backup-cli.md) hakkında bilgi edinin
