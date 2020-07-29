---
title: Öğretici-Azure Backup bir VM diskini geri yükleme
description: Yedekleme ve Kurtarma Hizmetleri ile Azure’da bir diskin nasıl geri yükleneceğini ve kurtarılan bir sanal makinenin nasıl oluşturulacağını öğrenin.
ms.topic: tutorial
ms.date: 01/31/2019
ms.custom: mvc
ms.openlocfilehash: efad97c3668c50669be89e6eccaadb26cb313e81
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289474"
---
# <a name="restore-a-disk-and-create-a-recovered-vm-in-azure"></a>Azure’da bir diski geri yükleme ve kurtarılan bir VM oluşturma

Azure Backup, coğrafi olarak yedekli kurtarma kasalarında depolanan kurtarma noktaları oluşturur. Bir kurtarma noktasından geri yüklediğinizde, tüm sanal makineyi veya tek tek dosyaları geri yükleyebilirsiniz. Bu makalede, CLI kullanarak tam bir sanal makinenin nasıl geri yükleneceği açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Kurtarma noktalarını listeleme ve seçme
> * Bir kurtarma noktasından diski geri yükleme
> * Geri yüklenen diskten sanal makine oluşturma

Disk geri yüklemek ve kurtarılmış bir VM oluşturmak üzere PowerShell kullanmayla ilgili bilgi edinmek için bkz. [PowerShell ile Azure VM’lerini yedekleme ve geri yükleme](backup-azure-vms-automation.md#restore-an-azure-vm).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.18 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için Azure Backup ile korunmuş olan bir Linux sanal makinesi gerekir. Yanlışlıkla bir sanal makineyi silme ve kurtarma işleminin benzetimini yapmak için, bir kurtarma noktasındaki diskten bir sanal makine oluşturursunuz. Azure Backup ile korunan bir Linux sanal makinesine ihtiyacınız varsa bkz. [CLI ile Azure’da bir sanal makineyi yedekleme](quick-backup-vm-cli.md).

## <a name="backup-overview"></a>Backup’a genel bakış

Azure bir yedekleme başlattığında sanal makinedeki yedekleme uzantısı, belirli bir noktanın anlık görüntüsünü alır. İlk yedekleme istendiğinde sanal makineye yedekleme uzantısı yüklenir. Azure Backup, yedekleme gerçekleştiğinde sanal makine çalışmıyorsa temel depolamanın anlık görüntüsünü de alabilir.

Varsayılan olarak Azure Backup, bir dosya sisteminin tutarlı yedeklemesini alır. Azure Backup, anlık görüntüyü aldığında veriler Kurtarma Hizmetleri kasasına aktarılır. Verimliliği en üst düzeye çıkarmak için Azure Backup yalnızca önceki yedeklemeden itibaren değişmiş olan veri bloklarını belirler ve aktarır.

Veri aktarımı tamamlandığında, anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.

## <a name="list-available-recovery-points"></a>Kullanılabilir kurtarma noktalarını listeleme

Bir diski geri yüklemek için, kurtarma verileri kaynağı olarak bir kurtarma noktası seçersiniz. Varsayılan ilke her gün bir kurtarma noktası oluşturup 30 gün boyunca bunları beklettiğinden, kurtarma için belirli bir nokta seçmenize olanak sağlayan bir kurtarma noktaları kümesini tutabilirsiniz.

Kullanılabilir kurtarma noktalarının listesini görmek için [az backup recoverypoint list](/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-list) komutunu kullanın. Diskleri kurtarmak için kurtarma noktası **adı** kullanılır. Bu öğreticide, kullanılabilir en son kurtarma noktasını istiyoruz. `--query [0].name` parametresi aşağıdaki şekilde en son kurtarma noktası adını seçer:

```azurecli-interactive
az backup recoverypoint list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --backup-management-type AzureIaasVM
    --container-name myVM \
    --item-name myVM \
    --query [0].name \
    --output tsv
```

## <a name="restore-a-vm-disk"></a>Sanal makine diskini geri yükleme

> [!IMPORTANT]
> Yönetilen disk geri yükleme dahil olmak üzere hızlı geri yükleme avantajlarından yararlanmak için az CLı Version 2.0.74 veya üzeri kullanılması önemle önerilir. Kullanıcı her zaman en son sürümü kullanıyorsa, bu en iyisidir.

### <a name="managed-disk-restore"></a>Yönetilen disk geri yükleme

Yedeklenen sanal makinede yönetilen diskler varsa ve kurtarma noktasından yönetilen diskleri geri yüklemek istiyorsanız, önce bir Azure depolama hesabı sağlarsınız. Bu depolama hesabı, VM yapılandırmasını ve daha sonra VM 'yi geri yüklenen disklerden dağıtmak için kullanılabilecek Dağıtım şablonunu depolamak için kullanılır. Daha sonra, yönetilen disklerin içine geri yükleneceği bir hedef kaynak grubu da sağlarsınız.

1. Depolama hesabı oluşturmak için [az storage account create](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) komutunu kullanın. Depolama hesabı adı tamamen küçük harflerden oluşmalı ve genel olarak benzersiz olmalıdır. *mystorageaccount* değerini kendi benzersiz adınızla değiştirin:

    ```azurecli-interactive
    az storage account create \
        --resource-group myResourceGroup \
        --name mystorageaccount \
        --sku Standard_LRS
    ```

2. [az backup restore restore-disks](/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-disks) komutuyla kurtarma noktanızdan diski geri yükleyin. *mystorageaccount* değerini, önceki komutta oluşturduğunuz depolama hesabının adıyla değiştirin. *Myrecoverypointname* değerini, önceki [az Backup RecoveryPoint List](/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-list) komutunun çıktısında edindiğiniz kurtarma noktası adıyla değiştirin. ***Ayrıca, yönetilen disklerin geri yüklendiği hedef kaynak grubunu da sağlayın***.

    ```azurecli-interactive
    az backup restore restore-disks \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --storage-account mystorageaccount \
        --rp-name myRecoveryPointName \
        --target-resource-group targetRG
    ```

    > [!WARNING]
    > Target-Resource-Group sağlanmazsa, yönetilen diskler, belirtilen depolama hesabına yönetilmeyen diskler olarak geri yüklenir. Diskleri geri yüklemek için geçen süre tamamen verilen depolama hesabına bağlı olduğundan, bu geri yükleme saatine önemli sonuçlara sahip olur. Müşteriler, yalnızca Target-Resource-Group parametresi verildiğinde anında geri yükleme avantajına sahip olur. Yönetilen diskleri yönetilmeyen olarak geri yüklemek için amaç, hedef-kaynak grubu parametresini sağlamamıştır ve bunun yerine, aşağıda gösterildiği gibi, yönetilmeyen disk parametresini geri yükle parametresini sağlar. Bu parametre az 3.4.0 onenlerden kullanılabilir.

    ```azurecli-interactive
    az backup restore restore-disks \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --storage-account mystorageaccount \
    --rp-name myRecoveryPointName \
    --restore-as-unmanaged-disk
    ```

Bu işlem, yönetilen diskleri, belirtilen depolama hesabına yönetilmeyen diskler olarak geri yükler ve ' anlık ' geri yükleme işlevlerinden yararlanmaz. CLı 'nın gelecek sürümlerinde, hedef kaynak grubu parametresini veya ' yönetilmeyen--------------------------

### <a name="unmanaged-disks-restore"></a>Yönetilmeyen diskler geri yükleme

Yedeklenen sanal makinede yönetilmeyen diskler varsa ve diskleri kurtarma noktasından geri yüklemek istiyorsanız, önce bir Azure depolama hesabı sağlarsınız. Bu depolama hesabı, VM yapılandırmasını ve daha sonra VM 'yi geri yüklenen disklerden dağıtmak için kullanılabilecek Dağıtım şablonunu depolamak için kullanılır. Varsayılan olarak, yönetilmeyen diskler özgün depolama hesaplarına geri yüklenir. Kullanıcı tüm yönetilmeyen diskleri tek bir yere geri yüklemeyi istiyorsa, söz konusu depolama hesabı da bu disklere yönelik hazırlama konumu olarak da kullanılabilir.

Ek adımlarda, sanal makine oluşturmak için geri yüklenen disk kullanılır.

1. Depolama hesabı oluşturmak için [az storage account create](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) komutunu kullanın. Depolama hesabı adı tamamen küçük harflerden oluşmalı ve genel olarak benzersiz olmalıdır. *mystorageaccount* değerini kendi benzersiz adınızla değiştirin:

    ```azurecli-interactive
    az storage account create \
        --resource-group myResourceGroup \
        --name mystorageaccount \
        --sku Standard_LRS
    ```

2. [az backup restore restore-disks](/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-disks) komutuyla kurtarma noktanızdan diski geri yükleyin. *mystorageaccount* değerini, önceki komutta oluşturduğunuz depolama hesabının adıyla değiştirin. *myRecoveryPointName* değerini, önceki [az backup recoverypoint list](/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-list) komutunun çıktısından elde ettiğiniz kurtarma noktası adıyla değiştirin:

    ```azurecli-interactive
    az backup restore restore-disks \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --storage-account mystorageaccount \
        --rp-name myRecoveryPointName
    ```

Yukarıda belirtildiği gibi, yönetilmeyen diskler özgün depolama hesaplarına geri yüklenir. Bu, en iyi geri yükleme performansını sağlar. Ancak, tüm yönetilmeyen disklerin verilen depolama hesabına geri yüklenmesi gerekiyorsa, aşağıda gösterildiği gibi ilgili bayrağı kullanın.

```azurecli-interactive
    az backup restore restore-disks \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --storage-account mystorageaccount \
        --rp-name myRecoveryPointName \
        --restore-to-staging-storage-account
    ```

## Monitor the restore job

To monitor the status of restore job, use [az backup job list](/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list):

```azurecli-interactive
az backup job list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --output table
```

Çıktı, geri yükleme işinin *İlerliyor* durumunda olduğunu gösteren aşağıdaki örneğe benzer olacaktır:

```output
Name      Operation        Status      Item Name    Start Time UTC       Duration
--------  ---------------  ----------  -----------  -------------------  --------------
7f2ad916  Restore          InProgress  myvm         2017-09-19T19:39:52  0:00:34.520850
a0a8e5e6  Backup           Completed   myvm         2017-09-19T03:09:21  0:15:26.155212
fe5d0414  ConfigureBackup  Completed   myvm         2017-09-19T03:03:57  0:00:31.191807
```

Geri yükleme işi raporlarının *durumu* *tamamlandığında*, gerekli bilgiler (VM yapılandırması ve dağıtım şablonu) depolama hesabına geri yüklendi.

## <a name="create-a-vm-from-the-restored-disk"></a>Geri yüklenen diskten sanal makine oluşturma

Son adım geri yüklenen disklerden bir VM oluşturmaktır. VM oluşturmak için verilen depolama hesabına indirilen Dağıtım şablonunu kullanabilirsiniz.

### <a name="fetch-the-job-details"></a>Iş ayrıntılarını getir

Sonuç iş ayrıntıları, sorgulanabilen ve dağıtılabilen şablon URI 'SI sağlar. Tetiklenen geri yüklenen iş hakkında daha fazla bilgi edinmek için iş göster komutunu kullanın.

```azurecli-interactive
az backup job show \
    -v myRecoveryServicesVault \
    -g myResourceGroup \
    -n 1fc2d55d-f0dc-4ca6-ad48-aca0fe5d0414
```

Bu sorgunun çıktısı tüm ayrıntıları verir, ancak yalnızca depolama hesabı içeriklerinde ilgileniyoruz. Azure CLı 'nin [sorgu özelliğini](/cli/azure/query-azure-cli?view=azure-cli-latest) kullanarak ilgili ayrıntıları getirebilirsiniz

```azurecli-interactive
az backup job show \
    -v myRecoveryServicesVault \
    -g myResourceGroup \
    -n 1fc2d55d-f0dc-4ca6-ad48-aca0fe5d0414 \
    --query properties.extendedInfo.propertyBag

{
  "Config Blob Container Name": "myVM-daa1931199fd4a22ae601f46d8812276",
  "Config Blob Name": "config-myVM-1fc2d55d-f0dc-4ca6-ad48-aca0fe5d0414.json",
  "Config Blob Uri": "https://mystorageaccount.blob.core.windows.net/myVM-daa1931199fd4a22ae601f46d8812276/config-appvm8-1fc2d55d-f0dc-4ca6-ad48-aca0519c0232.json",
  "Job Type": "Recover disks",
  "Recovery point time ": "12/25/2019 10:07:11 PM",
  "Target Storage Account Name": "mystorageaccount",
  "Target resource group": "mystorageaccountRG",
  "Template Blob Uri": "https://mystorageaccount.blob.core.windows.net/myVM-daa1931199fd4a22ae601f46d8812276/azuredeploy1fc2d55d-f0dc-4ca6-ad48-aca0519c0232.json"
}
```

### <a name="fetch-the-deployment-template"></a>Dağıtım şablonunu getir

Bir müşterinin depolama hesabı ve verilen kapsayıcı altında olduğundan, şablon doğrudan erişilebilir değildir. Bu şablona erişmek için URL 'nin tamamı (geçici bir SAS belirteci ile birlikte) gereklidir.

İlk olarak, iş ayrıntılarından şablon blobu URI 'sini ayıklayın

```azurecli-interactive
az backup job show \
    -v myRecoveryServicesVault \
    -g myResourceGroup \
    -n 1fc2d55d-f0dc-4ca6-ad48-aca0fe5d0414 \
    --query properties.extendedInfo.propertyBag."""Template Blob Uri"""

"https://mystorageaccount.blob.core.windows.net/myVM-daa1931199fd4a22ae601f46d8812276/azuredeploy1fc2d55d-f0dc-4ca6-ad48-aca0519c0232.json"
```

Şablon blob URI 'Si bu biçimde olur ve şablon adını ayıklar

```https
https://<storageAccountName.blob.core.windows.net>/<containerName>/<templateName>
```

Bu nedenle, yukarıdaki örnekteki şablon adı ```azuredeploy1fc2d55d-f0dc-4ca6-ad48-aca0519c0232.json``` ve kapsayıcı adı```myVM-daa1931199fd4a22ae601f46d8812276```

Şimdi bu kapsayıcı ve şablon için SAS belirtecini [burada](../azure-resource-manager/templates/secure-template-with-sas-token.md?tabs=azure-cli#provide-sas-token-during-deployment) açıklandığı gibi alın

```azurecli-interactive
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group mystorageaccountRG \
    --name mystorageaccount \
    --query connectionString)
token=$(az storage blob generate-sas \
    --container-name myVM-daa1931199fd4a22ae601f46d8812276 \
    --name azuredeploy1fc2d55d-f0dc-4ca6-ad48-aca0519c0232.json \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
   --container-name myVM-daa1931199fd4a22ae601f46d8812276 \
    --name azuredeploy1fc2d55d-f0dc-4ca6-ad48-aca0519c0232.json \
    --output tsv \
    --connection-string $connection)
```

### <a name="deploy-the-template-to-create-the-vm"></a>VM 'yi oluşturmak için şablonu dağıtma

Şimdi [burada](../azure-resource-manager/templates/deploy-cli.md)açıklandığı gibi, VM 'yi oluşturmak için şablonu dağıtın.

```azurecli-interactive
az group deployment create \
  --resource-group ExampleGroup \
  --template-uri $url?$token
```

Kurtarılan diskinizden sanal makinenizin oluşturulduğunu onaylamak için [az vm list](/cli/azure/vm?view=azure-cli-latest#az-vm-list) komutuyla aşağıdaki şekilde kaynak grubunuzdaki sanal makineleri listeleyin:

```azurecli-interactive
az vm list --resource-group myResourceGroup --output table
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir kurtarma noktasından bir diski geri yüklediniz ve sonra diskten bir sanal makine oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
>
> * Kurtarma noktalarını listeleme ve seçme
> * Bir kurtarma noktasından diski geri yükleme
> * Geri yüklenen diskten sanal makine oluşturma

Bir kurtarma noktasından tek tek dosyaları geri yükleme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure’da dosyaları sanal makineye geri yükleme](tutorial-restore-files.md)
