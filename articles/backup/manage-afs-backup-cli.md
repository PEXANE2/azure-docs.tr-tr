---
title: CLı ile Azure dosya paylaşma yedeklemelerini yönetme
description: Azure CLı kullanarak Azure Backup hizmeti tarafından yedeklenen Azure dosya paylaşımlarını yönetme ve izleme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: bf824b1868ad7c9e4df2cceeca101d82272e18d6
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294480"
---
# <a name="manage-azure-file-share-backups-with-azure-cli"></a>Azure CLı ile Azure dosya paylaşma yedeklemelerini yönetme

Azure komut satırı arabirimi (CLı), Azure kaynaklarını yönetmek için bir komut satırı deneyimi sağlar. Azure kaynaklarını kullanmak için özel otomasyon oluşturmaya yönelik harika bir araçtır. Bu makalede, [Azure Backup hizmeti](https://docs.microsoft.com/azure/backup/backup-overview)tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için aşağıdaki görevlerin nasıl gerçekleştirileceği açıklanmaktadır. Ayrıca [Azure Portal](https://portal.azure.com/)bu adımları uygulayabilirsiniz.

* [Işleri izleme](#monitor-jobs)
* [Ilkeyi Değiştir](#modify-policy)
* [Bir dosya paylaşımında Korumayı Durdur](#stop-protection-on-a-file-share)
* [Bir dosya paylaşımında korumayı sürdürür](#resume-protection-on-a-file-share)
* [Depolama hesabının kaydını silme](#unregister-a-storage-account)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yı yerel ortamda yüklemek ve kullanmak için Azure CLI sürüm 2.0.18 veya üzeri çalıştırmanız gerekir. CLI sürümünü bulmak için şunu çalıştırın: `az --version`. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yi yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide, [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview) hizmeti tarafından yedeklenen bir Azure dosya paylaşımınız olduğu varsayılır. Bunlardan birine sahip değilseniz dosya paylaşımlarınız için yedeklemeyi yapılandırmak üzere [CLI Ile Azure dosya paylaşımlarını yedekleme](backup-afs-cli.md) bölümüne bakın. Bu makalede, aşağıdaki kaynakları kullanacağız:

* **Kaynak grubu**: *azurefiles*
* **Recoveryserviceskasası**: *azurefilesvault*
* **Depolama hesabı**: *afsaccount*
* **Dosya paylaşma**: *azurefiles*

## <a name="monitor-jobs"></a>İşleri İzleme

Yedekleme veya geri yükleme işlemlerini tetiklemeniz durumunda, yedekleme hizmeti izleme için bir iş oluşturur. Tamamlanmış veya çalışmakta olan işleri izlemek için [az Backup Job List](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list) cmdlet 'ini kullanın. CLı, çalışmakta [olan bir işi askıya](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-stop) almanıza veya [bir iş tamamlanana kadar beklemeniz](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-wait)de sağlar.

Aşağıdaki örnek, *azurefilesvault* kurtarma hizmetleri Kasası için yedekleme işlerinin durumunu gösterir:

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

## <a name="modify-policy"></a>İlkeyi Değiştir

Yedekleme sıklığını veya bekletme aralığını değiştirmek için [az Backup Item Set-Policy](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-set-policy)' i kullanarak bir yedekleme ilkesini değiştirebilirsiniz.

İlkeyi değiştirmek için aşağıdaki parametreleri tanımlayın:

* **--Container-Name** , dosya paylaşımının barındırıldığı depolama hesabının adıdır. Kapsayıcının **adını** veya **kolay adını** almak için [az Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--ad** , ilkeyi değiştirmek istediğiniz dosya paylaşımının adıdır. Yedeklenen öğenin **adını** veya **kolay adını** almak için [az Backup Item List](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.
* **--Policy-Name** , dosya paylaşımınız için ayarlamak istediğiniz yedekleme ilkesinin adıdır. Kasanıza yönelik tüm ilkeleri görüntülemek için [az Backup Policy List](https://docs.microsoft.com/cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-list) ' i kullanabilirsiniz.

Aşağıdaki örnek, *afsaccount* depolama hesabında bulunan *azurefiles* dosya paylaşımının *schedule2* yedekleme ilkesini ayarlar.

```azurecli-interactive
az backup item set-policy --policy-name schedule2 --name azurefiles --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --name "AzureFileShare;azurefiles" --backup-management-type azurestorage --out table
```

Ayrıca, aşağıdaki iki ek parametreyi sağlayarak kapsayıcı ve öğe için "kolay adlar" i kullanarak yukarıdaki komutu çalıştırabilirsiniz:

* **--Backup-Management-Type**: *azurestorage*
* **--iş yükü-tür**: *azurefileshare*

```azurecli-interactive
az backup item set-policy --policy-name schedule2 --name azurefiles --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --name azurefiles --backup-management-type azurestorage --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
fec6f004-0e35-407f-9928-10a163f123e5  azurefiles
```

Çıkışdaki **ad** özniteliği, "ilkeyi Değiştir" işlemi için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

## <a name="stop-protection-on-a-file-share"></a>Bir dosya paylaşımında Korumayı Durdur

Azure dosya paylaşımlarını korumayı durdurmanın iki yolu vardır:

* Gelecekteki tüm yedekleme işlerini Durdur ve tüm kurtarma noktalarını *Sil*
* Gelecekteki tüm yedekleme işlerini durdur, ancak kurtarma noktalarını *bırak*

Azure Backup tarafından oluşturulan temeldeki anlık görüntüler bekletileceği için, kurtarma noktalarını depolamada bırakma ile ilişkili bir maliyet olabilir. Ancak, kurtarma noktalarından ayrılmasının avantajı, daha sonra isterseniz dosya paylaşımının geri yükleme seçeneğidir. Kurtarma noktalarından ayrılma maliyeti hakkında daha fazla bilgi için bkz. [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/storage/files). Tüm kurtarma noktalarını silmeyi seçerseniz, Dosya paylaşımını geri yükleyemezsiniz.

Dosya paylaşımının korumasını durdurmak için aşağıdaki parametreleri tanımlayın:

* **--Container-Name** , dosya paylaşımının barındırıldığı depolama hesabının adıdır. Kapsayıcının **adını** veya **kolay adını** almak için [az Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--öğe-adı** , korumasını durdurmak istediğiniz dosya paylaşımının adıdır. Yedeklenen öğenin **adını** veya **kolay adını** almak için [az Backup Item List](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.

### <a name="stop-protection-and-retain-recovery-points"></a>Korumayı durdurun ve kurtarma noktalarını koruyun

Verileri korurken korumayı durdurmak için [az Backup Protection Disable](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-disable) cmdlet 'ini kullanın.

Aşağıdaki örnek, *azurefiles* dosya paylaşımının korumasını durduruyor ancak tüm kurtarma noktalarını korur.

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name “AzureFileShare;azurefiles” --out table
```

Ayrıca, aşağıdaki iki ek parametreyi sağlayarak kapsayıcı ve öğe için "kolay ad" nı kullanarak yukarıdaki komutu çalıştırabilirsiniz:

* **--Backup-Management-Type**: *azurestorage*
* **--iş yükü-tür**: *azurefileshare*

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --item-name azurefiles --workload-type azurefileshare --backup-management-type Azurestorage --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
fec6f004-0e35-407f-9928-10a163f123e5  azurefiles
```

Çıktıda **ad** özniteliği, "korumayı Durdur" işleminin yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

### <a name="stop-protection-without-retaining-recovery-points"></a>Kurtarma noktalarını korumadan korumayı durdur

Kurtarma noktalarını korumadan korumayı durdurmak için, [az Backup Protection Disable](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-disable) cmdlet 'ini **kullanın.**

Aşağıdaki örnek, kurtarma noktalarını korumadan *azurefiles* dosya paylaşımının korumasını durduruyor:

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name “AzureFileShare;azurefiles” --delete-backup-data true --out table
```

Ayrıca, aşağıdaki iki ek parametreyi sağlayarak, kapsayıcı ve öğe için "kolay ad" i kullanarak yukarıdaki komutu çalıştırabilirsiniz:

* **--Backup-Management-Type**: *azurestorage*
* **--iş yükü-tür**: *azurefileshare*

```azurecli-interactive
az backup protection disable --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --item-name azurefiles --workload-type azurefileshare --backup-management-type Azurestorage --delete-backup-data true --out table
```

## <a name="resume-protection-on-a-file-share"></a>Bir dosya paylaşımında korumayı sürdürür

Azure dosya paylaşımının korumasını durdurduysanız ancak kurtarma noktalarını koruduysanız, korumayı daha sonra sürdürebilirsiniz. Kurtarma noktalarını korumazsanız korumaya devam edebilirsiniz.

Dosya paylaşımının korumasını sürdürmesini sağlamak için aşağıdaki parametreleri tanımlayın:

* **--Container-Name** , dosya paylaşımının barındırıldığı depolama hesabının adıdır. Kapsayıcının **adını** veya **kolay adını** almak için [az Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--öğe-adı** , korumayı yeniden başlatmak istediğiniz dosya paylaşımının adıdır. Yedeklenen öğenin **adını** veya **kolay adını** almak için [az Backup Item List](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.
* **--Policy-Name** , dosya paylaşımının korumasını sürdürmesini sağlamak istediğiniz yedekleme ilkesinin adıdır.

Aşağıdaki örnek, *schedule1* yedekleme ilkesini kullanarak *azurefiles* dosya paylaşımının korumasını sürdürmeniz için [az Backup Protection özgeçmişi](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-resume) cmdlet 'ini kullanır.

```azurecli-interactive
az backup protection resume --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount” --item-name “AzureFileShare;azurefiles” --policy-name schedule2 --out table
```

Ayrıca, aşağıdaki iki ek parametreyi sağlayarak, kapsayıcı ve öğe için "kolay ad" i kullanarak yukarıdaki komutu çalıştırabilirsiniz:

* **--Backup-Management-Type**: *azurestorage*
* **--iş yükü-tür**: *azurefileshare*

```azurecli-interactive
az backup protection resume --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --item-name azurefiles --workload-type azurefileshare --backup-management-type Azurestorage --policy-name schedule2 --out table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
75115ab0-43b0-4065-8698-55022a234b7f  azurefiles
```

Çıkışdaki **ad** özniteliği, "korumayı yeniden başlatma" işlemi için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

## <a name="unregister-a-storage-account"></a>Depolama hesabının kaydını silme

Farklı bir kurtarma hizmetleri Kasası kullanarak belirli bir depolama hesabındaki dosya paylaşımlarınızı korumak istiyorsanız, önce bu depolama hesabındaki [tüm dosya paylaşımları için korumayı durdurun](#stop-protection-on-a-file-share) . Daha sonra, koruma için şu anda kullanılan kurtarma hizmetleri kasasından hesabın kaydını silin.

Depolama hesabının kaydını silmek için bir kapsayıcı adı sağlamanız gerekir. Kapsayıcının **adını** veya **kolay adını** almak için [az Backup Container List](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.

Aşağıdaki örnek, [az Backup Container Unregister](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-unregister) cmdlet 'ini kullanarak *afsaccount* depolama hesabının *azurefilesvault* öğesinden kaydını siler.

```azurecli-interactive
az backup container unregister --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --out table
```

Ayrıca, aşağıdaki ek parametreyi sağlayarak kapsayıcı için "kolay ad" ile cmdlet 'ini çalıştırabilirsiniz:

* **--Backup-Management-Type**: *azurestorage*

```azurecli-interactive
az backup container unregister --vault-name azurefilesvault --resource-group azurefiles --container-name afsaccount --backup-management-type azurestorage --out table
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure dosya paylaşımları Için yedekleme/geri yükleme hatalarıyla Ilgili sorunları giderme](troubleshoot-azure-files.md)
