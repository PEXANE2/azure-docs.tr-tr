---
title: 'Hızlı başlangıç: Azure CLı kullanarak SYNAPSE çalışma alanı oluşturma'
description: Bu kılavuzdaki adımları izleyerek Azure CLı kullanarak bir Azure SYNAPSE çalışma alanı oluşturun.
services: synapse-analytics
author: alehall
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 08/25/2020
ms.author: alehall
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 7f0bf7d409c5e47abaaa5b59271f55952d8ccff4
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88871622"
---
# <a name="quickstart-create-an-azure-synapse-workspace-with-azure-cli"></a>Hızlı başlangıç: Azure CLı ile Azure SYNAPSE çalışma alanı oluşturma

Azure CLI, Azure kaynaklarını yönetmek için Azure tarafından sunulan komut satırı deneyimidir. Azure Cloud Shell ile kendi tarayıcınızda da kullanabilirsiniz. Dilerseniz macOS, Linux veya Windows’a yükleyebilir ve komut satırından çalıştırabilirsiniz.

Bu hızlı başlangıçta, Azure CLı kullanarak bir Synapse çalışma alanı oluşturmayı öğreneceksiniz.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

- Hafif ve esnek bir komut satırı JSON işlemcisi olan [JQ](https://stedolan.github.io/jq/download/)'yi indirin ve yükleyin
- [Azure Data Lake Storage 2. depolama hesabı](../storage/common/storage-account-create.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

    > [!IMPORTANT]
    > Azure SYNAPSE çalışma alanının seçili ADLS 2. hesabını okuyup yazabilbilmesi gerekir. Bunlara ek olarak, birincil depolama hesabı olarak bağlanan herhangi bir depolama hesabı için depolama hesabı [oluşturma](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account) bölümünde açıklandığı gibi depolama hesabı oluşturulurken **hiyerarşik ad alanını** etkinleştirmiş olmanız gerekir. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="install-the-azure-cli-locally"></a>Azure CLı 'yı yerel olarak yükler

Azure CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli).

Azure CLı 'yi yerel olarak çalıştırıyorsanız, oturum açıp kimlik doğrulaması yapmanız gerekir. Azure Cloud Shell kullanıyorsanız bu adım gerekli değildir. Azure CLı 'da oturum açmak için `az login` tarayıcı penceresinde çalıştırın ve kimlik doğrulaması yapın:

```azurecli
az login
```

Azure CLı ile kimlik doğrulama hakkında daha fazla bilgi için bkz. [Azure CLI Ile oturum açma](/cli/azure/authenticate-azure-cli).

## <a name="install-azure-synapse-extension-for-azure-cli"></a>Azure CLı için Azure SYNAPSE uzantısını yükler

```azurecli
az extension add --name synapse
```

> [!WARNING]
> Azure CLı için Azure SYNAPSE uzantısı önizleme aşamasındadır.

## <a name="create-an-azure-synapse-workspace-using-the-azure-cli"></a>Azure CLı kullanarak Azure SYNAPSE çalışma alanı oluşturma

1. Azure SYNAPSE çalışma alanı için kaynak oluşturmak üzere gerekli ortam değişkenlerini tanımlayın.

    | Ortam Değişkeni Adı | Açıklama |
    |---|---|---|
    |StorageAccountName| Mevcut ADLS 2. depolama hesabınızın adı.|
    |StorageAccountResourceGroup| Mevcut ADLS 2. depolama hesabı kaynak grubunuzun adı. |
    |FileShareName| Mevcut depolama dosya sisteminizin adı.|
    |SynapseResourceGroup| Azure SYNAPSE kaynak grubunuz için yeni bir ad seçin. |
    |Region| [Azure bölgelerinden](https://azure.microsoft.com/global-infrastructure/geographies/#overview)birini seçin. |
    |SynapseWorkspaceName| Yeni Azure SYNAPSE çalışma alanınız için benzersiz bir ad seçin. |
    |SqlUser| Yeni Kullanıcı adı için bir değer seçin.|
    |SQLPassword de belirtilmelidir| Güvenli bir parola seçin.|
    |||

2. Azure SYNAPSE çalışma alanınız için kapsayıcı olarak bir kaynak grubu oluşturun:
    ```azurecli
    az group create --name $SynapseResourceGroup --location $Region
    ```
3. ADLS Gen 2 depolama hesabı anahtarını alın:
    ```azurecli
    StorageAccountKey=$(az storage account keys list \
      --account-name $StorageAccountName \
      | jq -r '.[0] | .value')
    ```
4. ADLS Gen 2 depolama uç noktası URL 'sini alın:
    ```azurecli
    StorageEndpointUrl=$(az storage account show \
      --name $StorageAccountName \
      --resource-group $StorageAccountResourceGroup \
      | jq -r '.primaryEndpoints | .dfs')
    ```

5. Seçim ADLS 2. depolama hesabı anahtarınızın ve uç noktasının ne olduğunu her zaman kontrol edebilirsiniz:
    ```azurecli
    echo "Storage Account Key: $StorageAccountKey"
    echo "Storage Endpoint URL: $StorageEndpointUrl"
    ```

6. Azure SYNAPSE çalışma alanı oluşturma:
    ```azurecli
    az synapse workspace create \
      --name $SynapseWorkspaceName \
      --resource-group $SynapseResourceGroup \
      --storage-account $StorageAccountName \
      --file-system $FileShareName \
      --sql-admin-login-user $SqlUser \
      --sql-admin-login-password $SqlPassword \
      --location $Region
    ```

7. Azure SYNAPSE çalışma alanı için Web ve dev URL 'sini alın:
    ```azurecli
    WorkspaceWeb=$(az synapse workspace show --name $SynapseWorkspaceName --resource-group $SynapseResourceGroup | jq -r '.connectivityEndpoints | .web')

    WorkspaceDev=$(az synapse workspace show --name $SynapseWorkspaceName --resource-group $SynapseResourceGroup | jq -r '.connectivityEndpoints | .dev')
    ```

8. Makinenizden Azure SYNAPSE çalışma alanına erişmenize izin vermek için bir güvenlik duvarı kuralı oluşturun:

    ```azurecli
    ClientIP=$(curl -sb -H "Accept: application/json" "$WorkspaceDev" | jq -r '.message')
    ClientIP=${ClientIP##'Client Ip address : '}
    echo "Creating a firewall rule to enable access for IP address: $ClientIP"

    az synapse workspace firewall-rule create --end-ip-address $ClientIP --start-ip-address $ClientIP --name "Allow Client IP" --resource-group $SynapseResourceGroup --workspace-name $SynapseWorkspaceName
    ```

9. Çalışma alanınıza erişmek için ortam değişkeninde depolanan Azure SYNAPSE Workspace Web URL adresini açın `WorkspaceWeb` :

    ```azurecli
    echo "Open your Azure Synapse Workspace Web URL in the browser: $WorkspaceWeb"
    ```
    
    [![Azure SYNAPSE çalışma alanı Web ](media/quickstart-create-synapse-workspace-cli/create-workspace-cli-1.png)](media/quickstart-create-synapse-workspace-cli/create-workspace-cli-1.png#lightbox)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure SYNAPSE çalışma alanını silmek için aşağıdaki adımları izleyin.
> [!WARNING]
> Azure SYNAPSE çalışma alanını silmek, bulunan SQL havuzlarının ve çalışma alanı meta verilerinin veritabanında depolanan analiz altyapılarını ve verileri kaldırır. SQL veya Apache Spark uç noktalarına bağlanmak artık mümkün olmayacaktır. Tüm kod yapıtları silinecek (sorgular, Not defterleri, iş tanımları ve işlem hatları).
>
> Çalışma alanını silmek, çalışma alanına bağlı Data Lake Store Gen2 **verileri etkilemez.**

Azure SYNAPSE çalışma alanını silmek istiyorsanız aşağıdaki komutu doldurun:

```azurecli
az synapse workspace delete --name $SynapseWorkspaceName --resource-group $SynapseResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra, verilerinizi analiz etmeye ve keşfetmeye başlamak için [SQL havuzları oluşturabilir](quickstart-create-sql-pool-studio.md) veya [Apache Spark havuzları oluşturabilirsiniz](quickstart-create-apache-spark-pool-studio.md) .
