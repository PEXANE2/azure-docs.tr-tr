---
title: Azure CLı ile Azure dosya paylaşımlarını geri yükleme
description: Azure CLı kullanarak kurtarma hizmetleri kasasındaki yedeklenen Azure dosya paylaşımlarını geri yükleme hakkında bilgi edinin
ms.topic: conceptual
ms.date: 01/16/2020
ms.openlocfilehash: 63b2be2fe24c1274ed1581b7b849de578c978842
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76931042"
---
# <a name="restore-azure-file-shares-with-the-azure-cli"></a>Azure CLı ile Azure dosya paylaşımlarını geri yükleme

Azure CLı, Azure kaynaklarını yönetmek için bir komut satırı deneyimi sağlar. Azure kaynaklarını kullanmak için özel otomasyon oluşturmaya yönelik harika bir araçtır. Bu makalede, Azure CLı kullanılarak [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview) tarafından oluşturulan bir geri yükleme noktasından tüm dosya paylaşımının veya belirli dosyaların nasıl geri yükleneceği açıklanmaktadır. Bu adımları [Azure PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-afs-automation) veya [Azure portalı](backup-afs.md) ile de gerçekleştirebilirsiniz.

Bu makalenin sonuna kadar, Azure CLı ile aşağıdaki işlemleri gerçekleştirmeyi öğreneceksiniz:

* Yedeklenen bir Azure dosya paylaşımının geri yükleme noktalarını görüntüleyin.
* Tam bir Azure dosya paylaşımının geri yüklenmesi.
* Dosya veya klasörleri tek tek geri yükleyin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yı yerel ortamda yüklemek ve kullanmak için Azure CLI sürüm 2.0.18 veya üzeri çalıştırmanız gerekir. CLI sürümünü bulmak için şunu çalıştırın: `az --version`. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yi yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, zaten Azure Backup tarafından yedeklenen bir Azure dosya paylaşımınız olduğu varsayılır. Bir tane yoksa, dosya paylaşımınızla ilgili yedeklemeyi yapılandırmak için bkz. [CLI Ile Azure dosya paylaşımlarını](backup-afs-cli.md) yedekleme. Bu makalede aşağıdaki kaynakları kullanırsınız:

| Dosya paylaşımı  | Depolama hesabı | Bölge | Ayrıntılar                                                      |
| ----------- | --------------- | ------ | ------------------------------------------------------------ |
| *azurefiles*  | *afsaccount*      | EastUS | Azure Backup kullanılarak Yedeklenen özgün kaynak                 |
| *azurefiles1* | *afaccount1*      | EastUS | Alternatif konum kurtarma için kullanılan hedef kaynak |

Bu makalede açıklanan farklı geri yükleme türlerini denemek için dosya paylaşımlarınız için benzer bir yapı kullanabilirsiniz.

## <a name="fetch-recovery-points-for-the-azure-file-share"></a>Azure dosya paylaşımının kurtarma noktalarını getir

Yedeklenen dosya paylaşımının tüm kurtarma noktalarını listelemek için [az Backup RecoveryPoint List](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-list) cmdlet 'ini kullanın.

Aşağıdaki örnek, *afsaccount* depolama hesabındaki *azurefiles* dosya paylaşımının kurtarma noktaları listesini getirir.

```azurecli-interactive
az backup recoverypoint list --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --backup-management-type azurestorage --item-name “AzureFileShare;azurefiles” --workload-type azurefileshare --out table
```

Önceki cmdlet 'i, kapsayıcı ve öğe için kolay adı kullanarak aşağıdaki iki ek parametreyi sağlayarak da çalıştırabilirsiniz:

* **--Backup-Management-Type**: *azurestorage*
* **--iş yükü-tür**: *azurefileshare*

```azurecli-interactive
az backup recoverypoint list --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --backup-management-type azurestorage --item-name azurefiles --workload-type azurefileshare --out table
```

Sonuç kümesi, her geri yükleme noktası için zaman ve tutarlılık ayrıntılarının bulunduğu kurtarma noktalarının bir listesidir.

```output
Name                Time                        Consistency
------------------  -------------------------   --------------------
932887541532871865  2020-01-05T07:08:23+00:00   FileSystemConsistent
932885927361238054  2020-01-05T07:08:10+00:00   FileSystemConsistent
932879614553967772  2020-01-04T21:33:04+00:00   FileSystemConsistent
```

Çıktıdaki **ad** özniteliği, kurtarma işlemlerinde **--RP-Name** parametresi için değer olarak kullanılabilecek kurtarma noktası adına karşılık gelir.

## <a name="full-share-recovery-by-using-the-azure-cli"></a>Azure CLı kullanarak tam paylaşma kurtarması

Özgün dosya paylaşımının tamamını veya alternatif bir konuma geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

Geri yükleme işlemlerini gerçekleştirmek için aşağıdaki parametreleri tanımlayın:

* **--Container-Name**: yedeklenen orijinal dosya paylaşımından barındıran depolama hesabının adı. Kapsayıcının adını veya kolay adını almak için [az Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--öğe-adı**: geri yükleme işlemi için kullanmak istediğiniz yedeklenen orijinal dosya paylaşımının adı. Yedeklenen öğenin adını veya kolay adını almak için [az Backup Item List](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.

### <a name="restore-a-full-share-to-the-original-location"></a>Tam bir paylaşımın özgün konumuna geri yükleme

Özgün bir konuma geri yüklediğinizde, hedefle ilgili parametreleri belirtmeniz gerekmez. Yalnızca **çakışmayı çözümle** sağlanmalıdır.

Aşağıdaki örnek, özgün konumdaki *azurefiles* dosya paylaşımının geri yüklenmesi için geri yükleme modu ile [az Backup Restore restore-azurefileshare](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefileshare) cmdlet 'ini, *originallocation* olarak ayarlanmış şekilde kullanır. [Azure dosya paylaşımının kurtarma noktalarını getir](#fetch-recovery-points-for-the-azure-file-share)bölümünde elde ettiğiniz 932883129628959823 kurtarma noktasını kullanırsınız:

```azurecli-interactive
az backup restore restore-azurefileshare --vault-name azurefilesvault --resource-group azurefiles --rp-name 932887541532871865   --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --item-name “AzureFileShare;azurefiles” --restore-mode originallocation --resolve-conflict overwrite --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
6a27cc23-9283-4310-9c27-dcfb81b7b4bb  azurefiles
```

Çıkışdaki **ad** özniteliği, geri yükleme işlemi için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

### <a name="restore-a-full-share-to-an-alternate-location"></a>Tam bir paylaşıma alternatif bir konuma geri yükleme

Bu seçeneği, bir dosya paylaşımının alternatif bir konuma geri yüklenmesi ve özgün dosya paylaşımının olduğu gibi tutmanız için kullanabilirsiniz. Alternatif konum kurtarma için aşağıdaki parametreleri belirtin:

* **--target-Storage-Account**: yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **--target-File-Share**: yedeklenen içeriğin geri yüklendiği hedef depolama hesabı içindeki dosya paylaşma.
* **--target-Folder**: verilerin geri yüklendiği dosya paylaşımının altındaki klasör. Yedeklenen içerik bir kök klasöre geri yüklenirse, hedef klasör değerlerini boş bir dize olarak verin.
* **--Resolve-Conflict**: geri yüklenen verilerle bir çakışma varsa yönerge. **Overwrite** veya **Skip**kabul eder.

Aşağıdaki örnek, *afsaccount* depolama hesabındaki *azurefiles* dosya paylaşımının *afaccount1* depolama hesabındaki *azurefiles1 "* dosya paylaşımında geri yüklenmesi için [az Backup Restore restore-azurefileshare](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefileshare) WITH restore Mode with *alternatelocation* olarak kullanır.

```azurecli-interactive
az backup restore restore-azurefileshare --vault-name azurefilesvault --resource-group azurefiles --rp-name 932883129628959823 --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --item-name “AzureFileShare;azurefiles” --restore-mode alternatelocation --target-storage-account afaccount1 --target-file-share azurefiles1 --target-folder restoredata --resolve-conflict overwrite --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
babeb61c-d73d-4b91-9830-b8bfa83c349a  azurefiles
```

Çıkışdaki **ad** özniteliği, geri yükleme işlemi için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

## <a name="item-level-recovery"></a>Öğe düzeyinde kurtarma

Özgün veya alternatif konumdaki tek tek dosyaları veya klasörleri geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

Geri yükleme işlemlerini gerçekleştirmek için aşağıdaki parametreleri tanımlayın:

* **--Container-Name**: yedeklenen orijinal dosya paylaşımından barındıran depolama hesabının adı. Kapsayıcının adını veya kolay adını almak için [az Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--öğe-adı**: geri yükleme işlemi için kullanmak istediğiniz yedeklenen orijinal dosya paylaşımının adı. Yedeklenen öğenin adını veya kolay adını almak için [az Backup Item List](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.

Kurtarmak istediğiniz öğeler için aşağıdaki parametreleri belirtin:

* **SourceFilePath**: dosya paylaşımında, bir dize olarak geri yüklenecek dosyanın mutlak yolu. Bu yol, [az Storage File Download](https://docs.microsoft.com/cli/azure/storage/file?view=azure-cli-latest#az-storage-file-download) veya [az Storage File CLI Show](https://docs.microsoft.com/cli/azure/storage/file?view=azure-cli-latest#az-storage-file-show) CLI komutlarında kullanılan yoldur.
* **Sourcefiletype**: bir dizin veya dosya seçili olup olmadığını seçin. **Dizini** veya **dosyayı**kabul eder.
* **ResolveConflict**: geri yüklenen verilerle bir çakışma varsa yönerge. **Overwrite** veya **Skip**kabul eder.

### <a name="restore-individual-files-or-folders-to-the-original-location"></a>Tek tek dosyaları veya klasörleri özgün konumlarına geri yükleme

Belirli dosyaları veya klasörleri özgün konumlarına geri yüklemek için, geri yükleme modu ile [az Backup Restore restore-azurefiles](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefiles) cmdlet 'ini, *originallocation* olarak ayarlayın.

Aşağıdaki örnek, *Restoretest. txt* dosyasını özgün konumuna geri yükler: *azurefiles* dosya paylaşma.

```azurecli-interactive
az backup restore restore-azurefiles --vault-name azurefilesvault --resource-group azurefiles --rp-name 932881556234035474 --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --item-name “AzureFileShare;azurefiles” --restore-mode originallocation  --source-file-type file --source-file-path "Restore/RestoreTest.txt" --resolve-conflict overwrite  --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
df4d9024-0dcb-4edc-bf8c-0a3d18a25319  azurefiles
```

Çıkışdaki **ad** özniteliği, geri yükleme işlemi için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

### <a name="restore-individual-files-or-folders-to-an-alternate-location"></a>Tek tek dosyaları veya klasörleri alternatif bir konuma geri yükleme

Belirli dosya veya klasörleri alternatif bir konuma geri yüklemek için, [az Backup Restore restore-azurefiles](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefiles) cmdlet 'ini restore modu *alternatelocation* olarak ayarlayın ve hedefle ilgili aşağıdaki parametreleri belirtin:

* **--target-Storage-Account**: yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **--target-File-Share**: yedeklenen içeriğin geri yüklendiği hedef depolama hesabı içindeki dosya paylaşma.
* **--target-Folder**: verilerin geri yüklendiği dosya paylaşımının altındaki klasör. Yedeklenen içerik bir kök klasöre geri yüklenirse, hedef klasörün değerini boş bir dize olarak verin.

Aşağıdaki örnek, *azurefiles* dosya paylaşımında özgün olarak bulunan *restoretest. txt* dosyasını alternatif bir konuma geri yükler: *afaccount1* depolama hesabında barındırılan *azurefiles1* dosya paylaşımındaki *restoredata* klasörü.

```azurecli-interactive
az backup restore restore-azurefiles --vault-name azurefilesvault --resource-group azurefiles --rp-name 932881556234035474 --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --item-name “AzureFileShare;azurefiles” --restore-mode alternatelocation --target-storage-account afaccount1 --target-file-share azurefiles1 --target-folder restoredata --resolve-conflict overwrite --source-file-type file --source-file-path "Restore/RestoreTest.txt" --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
df4d9024-0dcb-4edc-bf8c-0a3d18a25319  azurefiles
```

Çıkışdaki **ad** özniteliği, geri yükleme işlemi için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Azure [CLI Ile Azure dosya paylaşımının yedeklerini yönetme](manage-afs-backup-cli.md)hakkında bilgi edinin.
