---
title: Azure CLI'yi kullanarak Azure Veri Gölü Analizini yönetme
description: Bu makalede, Veri Gölü Analizi işlerini, veri kaynaklarını & kullanıcıları yönetmek için Azure CLI'nin nasıl kullanılacağı açıklanmaktadır.
services: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: 69a48952ef273acb8cf7eb0ec5968e12b962b622
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454372"
---
# <a name="manage-azure-data-lake-analytics-using-the-azure-command-line-interface-cli"></a>Azure Komut Satırı Arabirimini (CLI) kullanarak Azure Veri Gölü Analizini yönetme

[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure CLI'yi kullanarak Azure Veri Gölü Analizi hesaplarını, veri kaynaklarını, kullanıcıları ve işleri nasıl yönettiğinizi öğrenin. Diğer araçları kullanarak yönetim konularını görmek için yukarıdaki seçsekmesini tıklatın.


**Ön koşullar**

Bu öğreticiye başlamadan önce aşağıdaki kaynaklara sahip olmalısınız:

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* Azure CLI. Bkz. [Azure CLI'yı yükleme ve yapılandırma](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

   * Bu demoyu tamamlamak için **yayın öncesi sürüm** [Azure CLI araçlarını](https://github.com/MicrosoftBigData/AzureDataLake/releases) indirip yükleyin.

* Komutu `az login` kullanarak kimlik doğrulaması ve kullanmak istediğiniz aboneliği seçin. Bir iş veya okul hesabı kullanarak kimlik doğrulama gerçekleştirme konusunda daha fazla bilgi için bkz. [Azure CLI'dan Azure aboneliğine bağlanma](/cli/azure/authenticate-azure-cli).

   ```azurecli
   az login
   az account set --subscription <subscription id>
   ```

   Artık Veri Gölü Analizi ve Veri Gölü Deposu komutlarına erişebilirsiniz. Veri Gölü Deposu ve Veri Gölü Analizi komutlarını listelemek için aşağıdaki komutu çalıştırın:

   ```azurecli
   az dls -h
   az dla -h
   ```

## <a name="manage-accounts"></a>Hesapları yönetme

Herhangi bir Data Lake Analytics işini çalıştırmadan önce bir Data Lake Analytics hesabınız olması gerekir. Azure HDInsight'ın aksine, bir Analytics hesabı çalışmıyorsa ödeme yapmazsınız. Sadece bir işi çalıştırırken ki süreyi ödlersin.  Daha fazla bilgi için Azure [Veri Gölü Analizine Genel Bakış](data-lake-analytics-overview.md)bölümüne bakın.  

### <a name="create-accounts"></a>Hesap oluşturma

Veri Gölü hesabı oluşturmak için aşağıdaki komutu çalıştırın, 

   ```azurecli
   az dla account create --account "<Data Lake Analytics account name>" --location "<Location Name>" --resource-group "<Resource Group Name>" --default-data-lake-store "<Data Lake Store account name>"
   ```

### <a name="update-accounts"></a>Hesapları güncelleştir

Aşağıdaki komut, varolan bir Veri Gölü Analizi Hesabının özelliklerini güncelleştirir

   ```azurecli
   az dla account update --account "<Data Lake Analytics Account Name>" --firewall-state "Enabled" --query-store-retention 7
   ```

### <a name="list-accounts"></a>Hesapları listele

Belirli bir kaynak grubundaki Veri Gölü Analizi hesaplarını listele

   ```azurecli
   az dla account list "<Resource group name>"
   ```

## <a name="get-details-of-an-account"></a>Bir hesabın ayrıntılarını alın

   ```azurecli
   az dla account show --account "<Data Lake Analytics account name>" --resource-group "<Resource group name>"
   ```

### <a name="delete-an-account"></a>Hesap silme

   ```azurecli
   az dla account delete --account "<Data Lake Analytics account name>" --resource-group "<Resource group name>"
   ```

## <a name="manage-data-sources"></a>Veri kaynaklarını yönetme

Data Lake Analytics şu anda aşağıdaki iki veri kaynağını destekler:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Depolama](../storage/common/storage-introduction.md)

Bir Analytics hesabı oluşturduğunuzda, varsayılan depolama hesabı olarak bir Azure Veri Gölü Depolama hesabı belirlemeniz gerekir. Varsayılan Veri Gölü depolama hesabı, iş meta verilerini ve iş denetim günlüklerini depolamak için kullanılır. Bir Analytics hesabı oluşturduktan sonra, ek Veri Gölü Depolama hesapları ve/veya Azure Depolama hesabı ekleyebilirsiniz. 

### <a name="find-the-default-data-lake-store-account"></a>Varsayılan Veri Gölü Deposu hesabını bulma

`az dla account show` Komutu çalıştırarak kullanılan varsayılan Data Lake Store hesabını görüntüleyebilirsiniz. Varsayılan hesap adı, varsayılan DataLakeStoreAccount özelliği altında listelenir.

   ```azurecli
   az dla account show --account "<Data Lake Analytics account name>"
   ```

### <a name="add-additional-blob-storage-accounts"></a>Ek Blob depolama hesapları ekleme

   ```azurecli
   az dla account blob-storage add --access-key "<Azure Storage Account Key>" --account "<Data Lake Analytics account name>" --storage-account-name "<Storage account name>"
   ```

> [!NOTE]
> Yalnızca Blob depolama kısa adları desteklenir. Örneğin "myblob.blob.core.windows.net" gibi FQDN kullanmayın.
> 

### <a name="add-additional-data-lake-store-accounts"></a>Ek Veri Gölü Deposu hesapları ekleme

Aşağıdaki komut, belirtilen Data Lake Analytics hesabını ek bir Data Lake Store hesabıyla güncelleştirir:

   ```azurecli
   az dla account data-lake-store add --account "<Data Lake Analytics account name>" --data-lake-store-account-name "<Data Lake Store account name>"
   ```

### <a name="update-existing-data-source"></a>Varolan veri kaynağını güncelleştirme

Varolan bir Blob depolama hesabı anahtarını güncelleştirmek için:

   ```azurecli
   az dla account blob-storage update --access-key "<New Blob Storage Account Key>" --account "<Data Lake Analytics account name>" --storage-account-name "<Data Lake Store account name>"
   ```

### <a name="list-data-sources"></a>Veri kaynaklarını listele:

Data Lake Store hesaplarını listelemek için:

   ```azurecli
   az dla account data-lake-store list --account "<Data Lake Analytics account name>"
   ```

Blob depolama hesabını listelemek için:

   ```azurecli
   az dla account blob-storage list --account "<Data Lake Analytics account name>"
   ```

![Data Lake Analytics liste veri kaynağı](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Veri kaynaklarını silme:
Bir Data Lake Store hesabını silmek için:

   ```azurecli
   az dla account data-lake-store delete --account "<Data Lake Analytics account name>" --data-lake-store-account-name "<Azure Data Lake Store account name>"
   ```

Blob depolama hesabını silmek için:

   ```azurecli
   az dla account blob-storage delete --account "<Data Lake Analytics account name>" --storage-account-name "<Data Lake Store account name>"
   ```

## <a name="manage-jobs"></a>İşleri yönetme
Bir iş oluşturabiliyor olmak için bir Data Lake Analytics hesabınız olması gerekir.  Daha fazla bilgi için [bkz.](#manage-accounts)

### <a name="list-jobs"></a>İşleri listele

   ```azurecli
   az dla job list --account "<Data Lake Analytics account name>"
   ```

   ![Data Lake Analytics liste veri kaynağı](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>İş ayrıntılarını alın

   ```azurecli
   az dla job show --account "<Data Lake Analytics account name>" --job-identity "<Job Id>"
   ```

### <a name="submit-jobs"></a>İş gönderme

> [!NOTE]
> Bir işin varsayılan önceliği 1000'dir ve bir iş için varsayılan paralellik derecesi 1'dir.
> 
>    ```azurecli
>    az dla job submit --account "<Data Lake Analytics account name>" --job-name "<Name of your job>" --script "<Script to submit>"
>    ```

### <a name="cancel-jobs"></a>İşleri iptal etme
İş kimliğini bulmak için liste komutunu kullanın ve sonra işi iptal etmek için iptal et'i kullanın.

   ```azurecli
   az dla job cancel --account "<Data Lake Analytics account name>" --job-identity "<Job Id>"
   ```

## <a name="pipelines-and-recurrences"></a>İşlem hatları ve tekrarlar

**İşlem hatları ve tekrarlar hakkında bilgi edinin**

Önceden gönderilmiş işler hakkında işlem hatları bilgilerini görmek için `az dla job pipeline` komutlarını kullanın.

```azurecli
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

Önceden gönderilmiş işler hakkında tekrar bilgilerini görmek için `az dla job recurrence` komutlarını kullanın.

```azurecli
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="see-also"></a>Ayrıca bkz.
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure portalLarını kullanarak Data Lake Analytics ile başlayın](data-lake-analytics-get-started-portal.md)
* [Azure portalLarını kullanarak Azure Veri Gölü Analizini yönetme](data-lake-analytics-manage-use-portal.md)
* [Azure portalLarını kullanarak Azure Veri Gölü Analizi işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

