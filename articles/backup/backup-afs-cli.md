---
title: Azure CLI ile Azure dosya paylaşımlarını yedekleme
description: Kurtarma Hizmetleri Kasası'ndaki Azure dosya paylaşımlarını yedeklemek için Azure CLI'yi nasıl kullanacağınızı öğrenin
ms.topic: conceptual
ms.date: 01/14/2020
ms.openlocfilehash: ff1d8c6245521d2d0262b0440177d65713058742
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844050"
---
# <a name="back-up-azure-file-shares-with-cli"></a>Azure dosya paylaşımlarını CLI ile yedekleme

Azure komut satırı arabirimi (CLI), Azure kaynaklarını yönetmek için komut satırı deneyimi sağlar. Azure kaynaklarını kullanmak için özel otomasyon oluşturmak için harika bir araçtır. Bu makalede, Azure CLI ile Azure dosya paylaşımları nasıl yedekleyinizi ayrıntılarıyla açıklanmaktadır. Bu adımları [Azure PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-afs-automation) veya [Azure portalı](backup-afs.md) ile de gerçekleştirebilirsiniz.

Bu eğitimin sonunda, Azure CLI ile aşağıdaki işlemleri nasıl gerçekleştireceğinizi öğreneceksiniz:

* Kurtarma hizmetleri kasası oluşturma
* Azure dosya paylaşımları için yedeklemeyi etkinleştirme
* Dosya paylaşımları için isteğe bağlı yedeklemeyi tetikleme

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yı yerel ortamda yüklemek ve kullanmak için Azure CLI sürüm 2.0.18 veya üzeri çalıştırmanız gerekir. CLI sürümünü bulmak `run az --version`için, . Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri Kasası Oluşturma

Kurtarma hizmeti kasası, tüm yedekleme öğelerinde birleştirilmiş görünüm ve yönetim yeteneği sağlayan bir varlıktır. Koruma altındaki bir kaynak için yedekleme işi çalıştığında Kurtarma Hizmetleri kasasının içinde bir kurtarma noktası oluşturulur. Daha sonra bu kurtarma noktalarından birini kullanarak verileri dilediğiniz zaman geri yükleyebilirsiniz.

Kurtarma hizmetleri kasası oluşturmak için aşağıdaki adımları izleyin:

1. Bir kaynak grubuna kasa yerleştirilir. Varolan bir kaynak grubunuz yoksa, az grubu oluşturmak için yeni bir tane [oluşturun.](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) Bu eğitimde, Doğu ABD bölgesinde yeni kaynak grubu *azurefilesi* oluşturuyoruz.

    ```azurecli-interactive
    az group create --name AzureFiles --location eastus --output table
    ```

    ```output
    Location    Name
    ----------  ----------
    eastus      AzureFiles
    ```

2. Tonoz oluşturmak için [az yedek tonoz oluşturmak](https://docs.microsoft.com/cli/azure/backup/vault?view=azure-cli-latest#az-backup-vault-create) cmdlet kullanın. Kaynak grubu için kullanılan kasa için aynı konumu belirtin.

    Aşağıdaki örnek, Doğu ABD bölgesinde *azurefilesvault* adlı bir kurtarma hizmetleri kasası oluşturur.

    ```azurecli-interactive
    az backup vault create --resource-group azurefiles --name azurefilesvault --location eastus --output table
    ```

    ```output
    Location    Name                ResourceGroup
    ----------  ----------------    ---------------
    eastus      azurefilesvault     azurefiles
    ```

3. Kasa depolamasında kullanılacak artıklık türünü belirtin. [Yerel olarak yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs) veya [coğrafi yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs)kullanabilirsiniz.

    Aşağıdaki örnekte, [az yedekleme tonoz yedekleme özellikleri kümesi](https://docs.microsoft.com/cli/azure/backup/vault/backup-properties?view=azure-cli-latest#az-backup-vault-backup-properties-set) cmdlet kullanarak *Azurefilesvault'tan* **Georedundant'a** depolama artıklığı seçeneği ayarlanır.

    ```azurecli-interactive
    az backup vault backup-properties set --name azurefilesvault --resource-group azurefiles --backup-storage-redundancy Georedundant
    ```

    Kasanın başarılı bir şekilde oluşturulıp oluşturulmamasını kontrol etmek için, kasanızın ayrıntılarını almak için [az yedekleme tonoz gösterisi](https://docs.microsoft.com/cli/azure/backup/vault?view=azure-cli-latest#az-backup-vault-show) cmdlet'i kullanabilirsiniz. Aşağıdaki örnekte, yukarıdaki adımlarda oluşturduğumuz *azurefilesvault'un* ayrıntılarını görüntüler.

    ```azurecli-interactive
    az backup vault show --name azurefilesvault --resource-group azurefiles --output table
    ```

    Çıktı aşağıdaki yanıta benzer olacaktır:

    ```output
    Location     Name               ResourceGroup
    ----------   ---------------    ---------------
    eastus       azurefilesvault    azurefiles
    ```

## <a name="enable-backup-for-azure-file-shares"></a>Azure dosya paylaşımları için yedeklemeyi etkinleştirme

Bu bölüm, yedeklemeyapılandırmak istediğiniz bir Azure dosya paylaşımının zaten olduğunu varsayar. Yoksa, az depolama paylaşımı oluşturma komutunu kullanarak bir Azure dosya [paylaşımı oluşturun.](https://docs.microsoft.com/cli/azure/storage/share?view=azure-cli-latest#az-storage-share-create)

Dosya paylaşımları için yedeklemeyi etkinleştirmek için, yedekleme işinin ne zaman çalıştığını ve kurtarma noktalarının ne kadar süre depolandığını tanımlayan bir koruma ilkesi oluşturmanız gerekir. [Az yedekleme ilkesi create](https://docs.microsoft.com/cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-create) cmdlet'i kullanarak bir yedekleme ilkesi oluşturabilirsiniz.

Aşağıdaki örnekte, *zamanlama 1* yedekleme ilkesini kullanarak *afsaccount* depolama *hesabındaki azurefiles* dosya paylaşımıiçin yedeklemeyi etkinleştirmek [için az yedekleme koruması azurefileshare](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-enable-for-azurefileshare) için etkinleştirme cmdlet kullanır:

```azurecli-interactive
az backup protection enable-for-azurefileshare --vault-name azurefilesvault --resource-group  azurefiles --policy-name schedule1 --storage-account afsaccount --azure-file-share azurefiles  --output table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
0caa93f4-460b-4328-ac1d-8293521dd928  azurefiles
```

Çıktıdaki **Ad** özniteliği, **yedekleme** işleminiz için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. İşin durumunu izlemek [için, az yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

## <a name="trigger-an-on-demand-backup-for-file-share"></a>Dosya paylaşımı için isteğe bağlı yedeklemeyi tetikleme

Yedekleme ilkesinin işi zamanlanan zamanda çalıştırmasını beklemek yerine dosya paylaşımınız için isteğe bağlı yedeklemeyi tetiklemek istiyorsanız, [az yedekleme yedekleme-şimdi](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-backup-now) cmdlet'ini kullanın.

İsteğe bağlı yedeklemeyi tetiklemek için aşağıdaki parametreleri tanımlamanız gerekir:

* **--kapsayıcı adı,** dosya paylaşımını barındıran depolama hesabının adıdır. Kapsayıcınızın **adını** veya **dostu adını** almak için az yedekleme [kapsayıcı listesi](/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) komutunu kullanın.
* **--madde adı,** isteğe bağlı yedeklemeyi tetiklemek istediğiniz dosya paylaşımının adıdır. Yedeklenmiş öğenizin **adını** veya **dostu adını** almak için az yedekleme [öğesi listesi](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list) komutunu kullanın.
* **--tutma-kadar** kurtarma noktası korumak istediğiniz zamana kadar tarih belirtir. Değer UTC zaman biçiminde ayarlanmalıdır (dd-mm-yyyy).

Aşağıdaki örnek, *20-01-2020*yılına kadar bekletilen *afsaccount* depolama *hesabındaki azuresfiles* fileshare için isteğe bağlı yedeklemeyi tetikler.

```azurecli-interactive
az backup protection backup-now --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name "AzureFileShare;azurefiles" --retain-until 20-01-2020 --output table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
9f026b4f-295b-4fb8-aae0-4f058124cb12  azurefiles
```

Çıktıdaki **Ad** özniteliği, "isteğe bağlı yedekleme" işleminiz için yedekleme hizmeti tarafından oluşturulan işin adına karşılık gelir. Bir işin durumunu izlemek için [az yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [CLI ile Azure dosya paylaşımlarını](restore-afs-cli.md) nasıl geri yükleyebilirsiniz öğrenin
* [CLI ile Azure dosya paylaşımı ackup'larını](manage-afs-backup-cli.md) nasıl yönetebilirsiniz öğrenin
