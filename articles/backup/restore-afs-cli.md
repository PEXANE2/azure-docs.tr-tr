---
title: Azure CLI ile Azure dosya paylaşımlarını geri yükleme
description: Kurtarma Hizmetleri kasasında yedeklenmiş Azure dosya paylaşımlarını geri yüklemek için Azure CLI'yi nasıl kullanacağınızı öğrenin
ms.topic: conceptual
ms.date: 01/16/2020
ms.openlocfilehash: 980044011e3417a2aff8447a939e02299923da38
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80757100"
---
# <a name="restore-azure-file-shares-with-the-azure-cli"></a>Azure CLI ile Azure dosya paylaşımlarını geri yükleme

Azure CLI, Azure kaynaklarını yönetmek için bir komut satırı deneyimi sağlar. Azure kaynaklarını kullanmak için özel otomasyon oluşturmak için harika bir araçtır. Bu makalede, Azure CLI'yi kullanarak [Azure Yedekleme](https://docs.microsoft.com/azure/backup/backup-overview) tarafından oluşturulan bir geri yükleme noktasından tüm dosya paylaşımının veya belirli dosyaların nasıl geri yüklenir. Bu adımları [Azure PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-afs-automation) veya [Azure portalı](backup-afs.md) ile de gerçekleştirebilirsiniz.

Bu makalenin sonunda, Azure CLI ile aşağıdaki işlemleri nasıl gerçekleştireceğinizi öğreneceksiniz:

* Yedeklenmiş bir Azure dosya paylaşımı için geri yükleme noktalarını görüntüleyin.
* Tam bir Azure dosya paylaşımını geri yükleyin.
* Tek tek dosyaları veya klasörleri geri yükleyin.

>[!NOTE]
> Azure Yedekleme artık Azure CLI'yi kullanarak birden çok dosya veya klasörü özgün veya alternatif bir konuma geri getirmeyi destekler. Daha fazla bilgi edinmek için birden çok dosya veya klasörü bu belgenin [özgün veya alternatif konum bölümüne geri yükleyin.](#restore-multiple-files-or-folders-to-original-or-alternate-location)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yı yerel ortamda yüklemek ve kullanmak için Azure CLI sürüm 2.0.18 veya üzeri çalıştırmanız gerekir. CLI sürümünü bulmak için şunu çalıştırın: `az --version`. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, Azure Yedekleme tarafından yedeklenen bir Azure dosya paylaşımızaten var varsayar. Yoksa, dosya paylaşımınızın yedeklemesini yapılandırmak için [Azure dosya paylaşımlarını CLI ile](backup-afs-cli.md) yedekle'ye bakın. Bu makale için aşağıdaki kaynakları kullanırsınız:

| Dosya paylaşımı  | Depolama hesabı | Bölge | Ayrıntılar                                                      |
| ----------- | --------------- | ------ | ------------------------------------------------------------ |
| *azuredosyaları*  | *afsaccount*      | DoğuUS | Azure Yedekleme kullanılarak yedeklenen özgün kaynak                 |
| *azurefiles1* | *afaccount1*      | DoğuUS | Alternatif konum kurtarma için kullanılan hedef kaynağı |

Bu makalede açıklanan farklı geri yükleme türlerini denemek için dosya paylaşımlarınız için benzer bir yapı kullanabilirsiniz.

## <a name="fetch-recovery-points-for-the-azure-file-share"></a>Azure dosya paylaşımı için kurtarma noktaları getir

Yedeklenmiş dosya paylaşımı için tüm kurtarma noktalarını listelemek için [az yedekleme kurtarma noktası cmdlet'ini](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-list) kullanın.

Aşağıdaki örnek, *afsaccount* depolama hesabında *azurefiles* dosya paylaşımı için kurtarma noktaları listesini getirir.

```azurecli-interactive
az backup recoverypoint list --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --backup-management-type azurestorage --item-name "AzureFileShare;azurefiles" --workload-type azurefileshare --out table
```

Ayrıca, aşağıdaki iki ek parametre yi sağlayarak, kapsayıcı ve öğe için uygun adı kullanarak önceki cmdlet'i çalıştırabilirsiniz:

* **--backup-management-type**: *azurestorage*
* **--iş yükü türü**: *azurefileshare*

```azurecli-interactive
az backup recoverypoint list --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --backup-management-type azurestorage --item-name azurefiles --workload-type azurefileshare --out table
```

Sonuç kümesi, her geri yükleme noktası için zaman ve tutarlılık ayrıntıları içeren kurtarma noktalarının listesidir.

```output
Name                Time                        Consistency
------------------  -------------------------   --------------------
932887541532871865  2020-01-05T07:08:23+00:00   FileSystemConsistent
932885927361238054  2020-01-05T07:08:10+00:00   FileSystemConsistent
932879614553967772  2020-01-04T21:33:04+00:00   FileSystemConsistent
```

Çıktıdaki **Ad** özniteliği, kurtarma işlemlerinde **--rp-name** parametresi için bir değer olarak kullanılabilecek kurtarma noktası adı ile karşılık gelir.

## <a name="full-share-recovery-by-using-the-azure-cli"></a>Azure CLI'yi kullanarak tam paylaşım kurtarma

Bu geri yükleme seçeneğini, özgün veya alternatif bir konumda dosya paylaşımının tamamını geri yüklemek için kullanabilirsiniz.

Geri yükleme işlemleri gerçekleştirmek için aşağıdaki parametreleri tanımlayın:

* **--container-name**: Yedeklenmiş orijinal dosya paylaşımını barındıran depolama hesabının adı. Kapsayıcınızın adını veya dostu adını almak için [az yedekleme kapsayıcı listesi](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--madde adı**: Geri yükleme işlemi için kullanmak istediğiniz yedeklenmiş özgün dosya paylaşımının adı. Yedeklenmiş öğenizin adını veya dostu adını almak için [az yedekleme öğesi listesi](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.

### <a name="restore-a-full-share-to-the-original-location"></a>Tam paylaşımı özgün konuma geri yükleme

Özgün bir konuma geri yüklediğinizde, hedefle ilgili parametrelerbelirtmeniz gerekmez. Yalnızca **Çözüm Çakışması** sağlanmalıdır.

Aşağıdaki örnekte, *azurefiles* dosya paylaşımını özgün konumda geri yüklemek için *özgün konuma* ayarlanmış geri yükleme moduyla az yedekleme geri [yükleme azurefileshare](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefileshare) cmdlet kullanır. [Azure dosya paylaşımı için Kurtarma noktalarını getir'de](#fetch-recovery-points-for-the-azure-file-share)elde ettiğiniz 932883129628959823 kurtarma noktasını kullanırsınız:

```azurecli-interactive
az backup restore restore-azurefileshare --vault-name azurefilesvault --resource-group azurefiles --rp-name 932887541532871865   --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name "AzureFileShare;azurefiles" --restore-mode originallocation --resolve-conflict overwrite --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
6a27cc23-9283-4310-9c27-dcfb81b7b4bb  azurefiles
```

Çıktıdaki **Ad** özniteliği, geri yükleme işleminiz için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için az [yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

### <a name="restore-a-full-share-to-an-alternate-location"></a>Tam paylaşımı alternatif bir konuma geri yükleme

Bu seçeneği, dosya paylaşımını alternatif bir konuma geri yüklemek ve özgün dosya paylaşımını olduğu gibi tutmak için kullanabilirsiniz. Alternatif konum kurtarma için aşağıdaki parametreleri belirtin:

* **--hedef-depolama hesabı**: Yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **--hedef-dosya paylaşımı**: Yedeklenen içeriğin geri yüklendiği hedef depolama hesabı içindeki dosya paylaşımı.
* **--hedef klasör**: Verilerin geri yüklendiği dosya paylaşımının altındaki klasör. Yedeklenen içerik bir kök klasörüne geri yüklenecekse, hedef klasör değerlerini boş bir dize olarak verin.
* **--resolve-conflict**: Geri yüklenen verilerle çakışması varsa yönerge. **Overwrite** veya **Skip'i**kabul eder.

Aşağıdaki örnekte, *afsaccount* depolama hesabındaki azurefiles dosya paylaşımını *afaccount1* depolama hesabındaki *azurefiles1"* dosya paylaşımına geri yüklemek için *alternatif konum* olarak geri yükleme moduile [az yedekleme geri yükleme-azurefileshare](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefileshare) kullanır. *azurefiles*

```azurecli-interactive
az backup restore restore-azurefileshare --vault-name azurefilesvault --resource-group azurefiles --rp-name 932883129628959823 --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name "AzureFileShare;azurefiles" --restore-mode alternatelocation --target-storage-account afaccount1 --target-file-share azurefiles1 --target-folder restoredata --resolve-conflict overwrite --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
babeb61c-d73d-4b91-9830-b8bfa83c349a  azurefiles
```

Çıktıdaki **Ad** özniteliği, geri yükleme işleminiz için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için az [yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

## <a name="item-level-recovery"></a>Öğe düzeyinde kurtarma

Özgün veya alternatif bir konumda tek tek dosyaları veya klasörleri geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

Geri yükleme işlemleri gerçekleştirmek için aşağıdaki parametreleri tanımlayın:

* **--container-name**: Yedeklenmiş orijinal dosya paylaşımını barındıran depolama hesabının adı. Kapsayıcınızın adını veya dostu adını almak için [az yedekleme kapsayıcı listesi](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--madde adı**: Geri yükleme işlemi için kullanmak istediğiniz yedeklenmiş özgün dosya paylaşımının adı. Yedeklenmiş öğenizin adını veya dostu adını almak için [az yedekleme öğesi listesi](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.

Kurtarmak istediğiniz maddeler için aşağıdaki parametreleri belirtin:

* **SourceFilePath**: Dosyanın mutlak yolu, dosya payı içinde geri yüklenecek, bir dize olarak. Bu yol [az depolama dosyası indirme](https://docs.microsoft.com/cli/azure/storage/file?view=azure-cli-latest#az-storage-file-download) veya [az depolama dosyası CLI](https://docs.microsoft.com/cli/azure/storage/file?view=azure-cli-latest#az-storage-file-show) komutları göstermek kullanılan aynı yoldur.
* **SourceFileType**: Dizin veya dosya seçilip seçilmediğini seçin. **Dizin** veya **Dosya**kabul eder.
* **Çözüm:** Geri yüklenen verilerle çakışması varsa yönerge. **Overwrite** veya **Skip'i**kabul eder.

### <a name="restore-individual-files-or-folders-to-the-original-location"></a>Tek tek dosyaları veya klasörleri özgün konuma geri yükleme

Belirli dosya veya klasörleri özgün konumlarına geri yüklemek için *özgün konuma* ayarlanmış geri yükleme moduyla az yedekleme [geri yükleme](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefiles) dosyaları cmdlet'i kullanın.

Aşağıdaki örnek, Özgün konumunda *ki Geri Yükleme Testi.txt* dosyasını geri yükler: *azurefiles* dosya paylaşımı.

```azurecli-interactive
az backup restore restore-azurefiles --vault-name azurefilesvault --resource-group azurefiles --rp-name 932881556234035474 --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name "AzureFileShare;azurefiles" --restore-mode originallocation  --source-file-type file --source-file-path "Restore/RestoreTest.txt" --resolve-conflict overwrite  --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
df4d9024-0dcb-4edc-bf8c-0a3d18a25319  azurefiles
```

Çıktıdaki **Ad** özniteliği, geri yükleme işleminiz için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için az [yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

### <a name="restore-individual-files-or-folders-to-an-alternate-location"></a>Tek tek dosyaları veya klasörleri alternatif bir konuma geri yükleme

Belirli dosya veya klasörleri alternatif bir konuma geri yüklemek için, *alternatif konum* için ayarlanmış geri yükleme moduyla az yedekleme geri [yükleme dosyaları](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurefiles) cmdlet'i kullanın ve aşağıdaki hedefle ilgili parametreleri belirtin:

* **--hedef-depolama hesabı**: Yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **--hedef-dosya paylaşımı**: Yedeklenen içeriğin geri yüklendiği hedef depolama hesabı içindeki dosya paylaşımı.
* **--hedef klasör**: Verilerin geri yüklendiği dosya paylaşımının altındaki klasör. Yedeklenen içerik bir kök klasörüne geri yüklenecekse, hedef klasörün değerini boş bir dize olarak verin.

Aşağıdaki örnek, *azurefiles* dosyası paylaşımında başlangıçta bulunan *RestoreTest.txt* dosyasını alternatif bir konuma geri yükler: *azurefiles1* dosya paylaşımındaki *geri yükleme veri* klasörü *afaccount1* depolama hesabında barındırılan.

```azurecli-interactive
az backup restore restore-azurefiles --vault-name azurefilesvault --resource-group azurefiles --rp-name 932881556234035474 --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name "AzureFileShare;azurefiles" --restore-mode alternatelocation --target-storage-account afaccount1 --target-file-share azurefiles1 --target-folder restoredata --resolve-conflict overwrite --source-file-type file --source-file-path "Restore/RestoreTest.txt" --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
df4d9024-0dcb-4edc-bf8c-0a3d18a25319  azurefiles
```

Çıktıdaki **Ad** özniteliği, geri yükleme işleminiz için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için az [yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

## <a name="restore-multiple-files-or-folders-to-original-or-alternate-location"></a>Birden çok dosya veya klasörü özgün veya alternatif konuma geri yükleme

Birden çok öğe için geri yükleme gerçekleştirmek için, geri yüklemek istediğiniz tüm dosya veya klasörlerin **boşluk ayrılmış** yolları olarak **kaynak dosya-yol** parametresinin değerini geçirin.

Aşağıdaki örnek, *Geri Yükleme.txt* ve *AFS test Report.docx* dosyalarını özgün konumlarında geri yükler.

```azurecli-interactive
az backup restore restore-azurefiles --vault-name azurefilesvault --resource-group azurefiles --rp-name 932889937058317910 --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name "AzureFileShare;azurefiles" --restore-mode originallocation  --source-file-type file --source-file-path "Restore Test.txt" "AFS Testing Report.docx" --resolve-conflict overwrite  --out table
```

Çıktı şununla benzerlik gösterecektir:

```output
Name                                          ResourceGroup
------------------------------------          ---------------
649b0c14-4a94-4945-995a-19e2aace0305          azurefiles
```

Çıktıdaki **Ad** özniteliği, geri yükleme işleminiz için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için az [yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

Birden çok öğeyi alternatif bir konuma geri yüklemek istiyorsanız, tek [tek dosyaları veya klasörleri alternatif bir konum bölümüne geri yükle'de](#restore-individual-files-or-folders-to-an-alternate-location) açıklandığı gibi hedefle ilgili parametreleri belirterek yukarıdaki komutu kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure CLI ile Azure dosya paylaşım yedeklemelerini](manage-afs-backup-cli.md)nasıl yönetebilirsiniz öğrenin.
