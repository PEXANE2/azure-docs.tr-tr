---
title: Azure dosya paylaşımı yedeklemelerini Azure CLI ile yönetme
description: Azure Yedekleme tarafından desteklenen Azure dosya paylaşımlarını yönetmek ve izlemek için Azure CLI'yi nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: 44a49913abd99b285397b8b78ad9d4c0f9df52ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76934883"
---
# <a name="manage-azure-file-share-backups-with-the-azure-cli"></a>Azure dosya paylaşımı yedeklemelerini Azure CLI ile yönetme

Azure CLI, Azure kaynaklarını yönetmek için bir komut satırı deneyimi sağlar. Azure kaynaklarını kullanmak için özel otomasyon oluşturmak için harika bir araçtır. Bu makalede, [Azure Yedekleme](https://docs.microsoft.com/azure/backup/backup-overview)tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için görevlerin nasıl gerçekleştirililizleyeceği açıklanmaktadır. Bu adımları [Azure portalı](https://portal.azure.com/)ile de gerçekleştirebilirsiniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemek ve kullanmak için Azure CLI sürümünü 2.0.18 veya daha sonra çalıştırmanız gerekir. CLI sürümünü bulmak için şunu çalıştırın: `az --version`. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, [Azure Yedekleme](https://docs.microsoft.com/azure/backup/backup-overview)tarafından yedeklenmiş bir Azure dosya paylaşımızaten var varsayar. Yoksa, dosya paylaşımlarınızın yedeklemesini yapılandırmak için [Azure dosya paylaşımlarını CLI ile](backup-afs-cli.md) yedekle'ye bakın. Bu makale için aşağıdaki kaynakları kullanırsınız:

* **Kaynak grubu**: *azurefiles*
* **RecoveryServicesVault**: *azurefilesvault*
* **Depolama Hesabı**: *afsaccount*
* **Dosya Paylaş**: *azurefiles*

## <a name="monitor-jobs"></a>İşleri izleme

Yedekleme veya geri yükleme işlemlerini tetiklediğinizde, yedekleme hizmeti izleme için bir iş oluşturur. Tamamlanan veya şu anda çalışan işleri izlemek için [az yedekleme iş listesi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list) cmdlet kullanın. CLI ile, şu [anda çalışan bir işi askıya](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-stop) alabilir veya bir iş [bitene kadar bekleyebilirsiniz.](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-wait)

Aşağıdaki örnekte *azurefilesvault* Kurtarma Hizmetleri kasası için yedekleme işlerinin durumu görüntülenir:

```azurecli-interactive
az backup job list --resource-group azurefiles --vault-name azurefilesvault
```

```output
[
  {
    "eTag": null,
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/d477dfb6-b292-4f24-bb43-6b14e9d06ab5",
    "location": null,
    "name": "d477dfb6-b292-4f24-bb43-6b14e9d06ab5",
    "properties": {
      "actionsInfo": null,
      "activityId": "3cef43ed-0af4-43e2-b9cb-1322c496ccb4",
      "backupManagementType": "AzureStorage",
      "duration": "0:00:29.718011",
      "endTime": "2020-01-13T08:05:29.180606+00:00",
      "entityFriendlyName": "azurefiles",
      "errorDetails": null,
      "extendedInfo": null,
      "jobType": "AzureStorageJob",
      "operation": "Backup",
      "startTime": "2020-01-13T08:04:59.462595+00:00",
      "status": "Completed",
      "storageAccountName": "afsaccount",
      "storageAccountVersion": "MicrosoftStorage"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupJobs"
  },
  {
    "eTag": null,
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/1b9399bf-c23c-4caa-933a-5fc2bf884519",
    "location": null,
    "name": "1b9399bf-c23c-4caa-933a-5fc2bf884519",
    "properties": {
      "actionsInfo": null,
      "activityId": "2663449c-94f1-4735-aaf9-5bb991e7e00c",
      "backupManagementType": "AzureStorage",
      "duration": "0:00:28.145216",
      "endTime": "2020-01-13T08:05:27.519826+00:00",
      "entityFriendlyName": "azurefilesresource",
      "errorDetails": null,
      "extendedInfo": null,
      "jobType": "AzureStorageJob",
      "operation": "Backup",
      "startTime": "2020-01-13T08:04:59.374610+00:00",
      "status": "Completed",
      "storageAccountName": "afsaccount",
      "storageAccountVersion": "MicrosoftStorage"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupJobs"
  }
]
```

## <a name="modify-policy"></a>İlkeyi değiştir

Az yedekleme [öğesi ayar ilkesini](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-set-policy)kullanarak yedekleme sıklığını veya bekletme aralığını değiştirmek için bir yedekleme ilkesini değiştirebilirsiniz.

İlkeyi değiştirmek için aşağıdaki parametreleri tanımlayın:

* **--container-name**: Dosya paylaşımını barındıran depolama hesabının adı. Kapsayıcınızın **adını** veya **dostu adını** almak için az yedekleme [kapsayıcı listesi](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--name**: İlkeyi değiştirmek istediğiniz dosya paylaşımının adı. Yedeklenmiş öğenizin **adını** veya **dostu adını** almak için az yedekleme [öğesi listesi](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.
* **--ilke adı**: Dosya paylaşımınız için ayarlamak istediğiniz yedekleme ilkesinin adıdır. Kasanızın tüm ilkelerini görüntülemek için [az yedekleme ilkesi listesini](https://docs.microsoft.com/cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-list) kullanabilirsiniz.

Aşağıdaki örnekte, *afsaccount* depolama hesabında bulunan *azurefiles* dosya paylaşımı için *zamanlama2* yedekleme ilkesi belirlenir.

```azurecli-interactive
az backup item set-policy --policy-name schedule2 --name azurefiles --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --name "AzureFileShare;azurefiles" --backup-management-type azurestorage --out table
```

Ayrıca, aşağıdaki iki ek parametreyi sağlayarak kapsayıcı ve öğe için dost adlarını kullanarak önceki komutu çalıştırabilirsiniz:

* **--backup-management-type**: *azurestorage*
* **--iş yükü türü**: *azurefileshare*

```azurecli-interactive
az backup item set-policy --policy-name schedule2 --name azurefiles --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --name azurefiles --backup-management-type azurestorage --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
fec6f004-0e35-407f-9928-10a163f123e5  azurefiles
```

Çıktıdaki **Ad** özniteliği, değişiklik ilkesi işleminiz için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için az [yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

## <a name="stop-protection-on-a-file-share"></a>Dosya paylaşımı üzerindeki korumayı durdurma

Azure dosya paylaşımlarını korumayı durdurmanın iki yolu vardır:

* Gelecekteki tüm yedekleme işlerini durdurun ve tüm kurtarma noktalarını *silin.*
* Gelecekteki tüm yedekleme işlerini durdurun, ancak kurtarma noktalarını *bırakın.*

Azure Yedekleme tarafından oluşturulan temel anlık görüntüler korunacağı için, kurtarma noktalarını depolama alanında bırakmanın bir maliyeti olabilir. Kurtarma noktalarını bırakmanın yararı, isterseniz dosya paylaşımını daha sonra geri yükleme seçeneğidir. Kurtarma noktalarından çıkmanın maliyeti hakkında bilgi için [fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/storage/files)bakın. Tüm kurtarma noktalarını silmeyi seçerseniz, dosya paylaşımını geri yükleyemezsiniz.

Dosya paylaşımı için korumayı durdurmak için aşağıdaki parametreleri tanımlayın:

* **--container-name**: Dosya paylaşımını barındıran depolama hesabının adı. Kapsayıcınızın **adını** veya **dostu adını** almak için az yedekleme [kapsayıcı listesi](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--madde adı**: Korumayı durdurmak istediğiniz dosya paylaşımının adı. Yedeklenmiş öğenizin **adını** veya **dostu adını** almak için az yedekleme [öğesi listesi](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.

### <a name="stop-protection-and-retain-recovery-points"></a>Korumayı durdurun ve kurtarma noktalarını koruyun

Verileri tutarken korumayı durdurmak için [cmdlet'i devre dışı bırakın.](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-disable)

Aşağıdaki örnek, *azurefiles* dosya paylaşımı için korumayı durdurur, ancak tüm kurtarma noktalarını korur.

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name “AzureFileShare;azurefiles” --out table
```

Ayrıca, aşağıdaki iki ek parametreyi sağlayarak kapsayıcı ve öğe için dost adı kullanarak önceki komutu çalıştırabilirsiniz:

* **--backup-management-type**: *azurestorage*
* **--iş yükü türü**: *azurefileshare*

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --item-name azurefiles --workload-type azurefileshare --backup-management-type Azurestorage --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
fec6f004-0e35-407f-9928-10a163f123e5  azurefiles
```

Çıktıdaki **Ad** özniteliği, durdurma koruma işleminiz için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için az [yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

### <a name="stop-protection-without-retaining-recovery-points"></a>Kurtarma noktalarını tutmadan korumayı durdurma

Kurtarma noktalarını tutmadan korumayı durdurmak için, **silmek-yedekleme-veri** seçeneği ile cmdlet'i devre [dışı bırakma](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-disable) seçeneğini **kullanın.**

Aşağıdaki örnek, kurtarma noktaları tutmadan *azurefiles* dosya paylaşımı için korumayı durdurur.

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name “AzureFileShare;azurefiles” --delete-backup-data true --out table
```

Ayrıca, aşağıdaki iki ek parametreyi sağlayarak kapsayıcı ve öğe için dost adı kullanarak önceki komutu çalıştırabilirsiniz:

* **--backup-management-type**: *azurestorage*
* **--iş yükü türü**: *azurefileshare*

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --item-name azurefiles --workload-type azurefileshare --backup-management-type Azurestorage --delete-backup-data true --out table
```

## <a name="resume-protection-on-a-file-share"></a>Dosya paylaşımı üzerindeki korumayı sürdürme

Bir Azure dosya paylaşımı için korumayı durdurduysanız ancak kurtarma noktalarını koruduysanız, daha sonra korumaya devam edebilirsiniz. Kurtarma noktalarını korumazsanız, korunmaya devam edemezsiniz.

Dosya paylaşımı için korumayı sürdürmek için aşağıdaki parametreleri tanımlayın:

* **--container-name**: Dosya paylaşımını barındıran depolama hesabının adı. Kapsayıcınızın **adını** veya **dostu adını** almak için az yedekleme [kapsayıcı listesi](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--madde adı**: Korumayı sürdürmek istediğiniz dosya paylaşımının adı. Yedeklenmiş öğenizin **adını** veya **dostu adını** almak için az yedekleme [öğesi listesi](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.
* **--ilke adı**: Dosya paylaşımı için korumayı sürdürmek istediğiniz yedekleme ilkesinin adı.

Aşağıdaki örnekte, *zamanlama1* yedekleme ilkesini kullanarak *azurefiles* dosya paylaşımı için korumayı sürdürmek için [az yedekleme koruması devam](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-resume) cmdlet'i kullanır.

```azurecli-interactive
az backup protection resume --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --item-name “AzureFileShare;azurefiles” --policy-name schedule2 --out table
```

Ayrıca, aşağıdaki iki ek parametreyi sağlayarak kapsayıcı ve öğe için dost adı kullanarak önceki komutu çalıştırabilirsiniz:

* **--backup-management-type**: *azurestorage*
* **--iş yükü türü**: *azurefileshare*

```azurecli-interactive
az backup protection resume --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --item-name azurefiles --workload-type azurefileshare --backup-management-type Azurestorage --policy-name schedule2 --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
75115ab0-43b0-4065-8698-55022a234b7f  azurefiles
```

Çıktıdaki **Ad** özniteliği, özgeçmiş koruma işleminiz için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için az [yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

## <a name="unregister-a-storage-account"></a>Depolama hesabının kaydını açma

Farklı bir Kurtarma Hizmetleri kasası kullanarak belirli bir depolama hesabındaki dosya paylaşımlarınızı korumak istiyorsanız, önce bu depolama hesabındaki [tüm dosya paylaşımları için korumayı durdurun.](#stop-protection-on-a-file-share) Ardından, şu anda koruma için kullanılan Kurtarma Hizmetleri kasasından hesabı açın.

Depolama hesabının kaydını çıkarmak için bir kapsayıcı adı sağlamanız gerekir. Kabınızın **adını** veya **dostu adını** almak için az yedekleme kapsayıcı [listesi](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.

Aşağıdaki örnek, az yedekleme kapsayıcısı unregister cmdlet kullanarak *azurefilesvault* *gelen afsaccount* depolama hesabı nın [kaydını unregisters.](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-unregister)

```azurecli-interactive
az backup container unregister --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --out table
```

Ayrıca, aşağıdaki ek parametreyi sağlayarak kapsayıcının dostu adını kullanarak önceki cmdlet'i çalıştırabilirsiniz:

* **--backup-management-type**: *azurestorage*

```azurecli-interactive
az backup container unregister --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --backup-management-type azurestorage --out table
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için Bkz. [Sorun Gideri Azure dosya paylaşımları](troubleshoot-azure-files.md)yedeklemesi.
