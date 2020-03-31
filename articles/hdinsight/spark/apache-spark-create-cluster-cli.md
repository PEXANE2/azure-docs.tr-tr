---
title: 'Hızlı başlangıç: Azure CLI ile Apache Spark kümeleri - Azure HDInsight'
description: Bu hızlı başlangıç, Azure HDInsight'ta Bir Apache Spark kümesi oluşturmak için Azure CLI'nin nasıl kullanılacağını gösterir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 02/03/2020
ms.openlocfilehash: e4679d5a04be7b8c0145fd93818e4187170b4194
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77049674"
---
# <a name="quickstart-create-apache-spark-cluster-in-azure-hdinsight-using-azure-cli"></a>Hızlı başlangıç: Azure CLI'yi kullanarak Azure HDInsight'ta Apache Spark kümesi oluşturma

Bu hızlı başlangıçta, Azure komut satırı arabirimini (CLI) kullanarak Azure HDInsight'ta Apache Spark kümesioluşturmayı öğrenirsiniz. Azure HDInsight kuruluşlara yönelik, yönetilen, tam spektrumlu ve açık kaynaklı bir analiz hizmetidir. HDInsight için Apache Spark çerçevesi, bellek içi işleme yi kullanarak hızlı veri analitiği ve küme hesaplaması sağlar. Azure CLI, Azure kaynaklarını yönetmek için Microsoft tarafından sunulan platformlar arası komut satırı deneyimidir.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Azure CLI,](https://docs.microsoft.com/cli/azure/install-azure-cli)Azure Bulut Uyp'ı kullanmak istemiyorsanız.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-an-apache-spark-cluster"></a>Apache Spark kümesi oluşturma

1. Azure aboneliğinizde oturum açın. Azure Bulut Su Şurası'nı kullanmayı planlıyorsanız, aşağıdaki kod bloğunun sağ üst köşesinde **deneyin'i** seçin. Else, aşağıdaki komutu girin:

    ```azurecli-interactive
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription "SUBSCRIPTIONID"
    ```

2. Ortam değişkenlerini ayarlayın. Bu hızlı başlatmada değişkenlerin kullanımı Bash'e dayanır. Diğer ortamlar için küçük varyasyonlar gerekecektir. Aşağıdaki kod snippetinde KAYNAKGROUPNAME, YER, KÜME ADI, STORAGEACCOUNTNAME ve PASSWORD'i istenilen değerlerle değiştirin. Ardından ortam değişkenlerini ayarlamak için CLI komutlarını girin.

    ```azurecli-interactive
    export resourceGroupName=RESOURCEGROUPNAME
    export location=LOCATION
    export clusterName=CLUSTERNAME
    export AZURE_STORAGE_ACCOUNT=STORAGEACCOUNTNAME
    export httpCredential='PASSWORD'
    export sshCredentials='PASSWORD'
    
    export AZURE_STORAGE_CONTAINER=$clusterName
    export clusterSizeInNodes=1
    export clusterVersion=3.6
    export clusterType=spark
    export componentVersion=Spark=2.3
    ```

3. Aşağıdaki komutu girerek kaynak grubu oluşturun:

    ```azurecli-interactive
    az group create \
        --location $location \
        --name $resourceGroupName
    ```

4. Aşağıdaki komutu girerek bir Azure depolama hesabı oluşturun:

    ```azurecli-interactive
    az storage account create \
        --name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --https-only true \
        --kind StorageV2 \
        --location $location \
        --sku Standard_LRS
    ```

5. Birincil anahtarı Azure depolama hesabından çıkarın ve aşağıdaki komutu girerek bir değişkende saklayın:

    ```azurecli-interactive
    export AZURE_STORAGE_KEY=$(az storage account keys list \
        --account-name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --query [0].value -o tsv)
    ```

6. Aşağıdaki komutu girerek bir Azure depolama kapsayıcısı oluşturun:

    ```azurecli-interactive
    az storage container create \
        --name $AZURE_STORAGE_CONTAINER \
        --account-key $AZURE_STORAGE_KEY \
        --account-name $AZURE_STORAGE_ACCOUNT
    ```

7. Aşağıdaki komutu girerek Apache Spark kümesini oluşturun:

    ```azurecli-interactive
    az hdinsight create \
        --name $clusterName \
        --resource-group $resourceGroupName \
        --type $clusterType \
        --component-version $componentVersion \
        --http-password $httpCredential \
        --http-user admin \
        --location $location \
        --workernode-count $clusterSizeInNodes \
        --ssh-password $sshCredentials \
        --ssh-user sshuser \
        --storage-account $AZURE_STORAGE_ACCOUNT \
        --storage-account-key $AZURE_STORAGE_KEY \
        --storage-container $AZURE_STORAGE_CONTAINER \
        --version $clusterVersion
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladıktan sonra, kümeyi silmek isteyebilirsiniz. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

Kaynakları kaldırmak için aşağıdaki komutların tümlerini veya bazılarını girin:

```azurecli-interactive
# Remove cluster
az hdinsight delete \
    --name $clusterName \
    --resource-group $resourceGroupName

# Remove storage container
az storage container delete \
    --account-name $AZURE_STORAGE_ACCOUNT \
    --name $AZURE_STORAGE_CONTAINER

# Remove storage account
az storage account delete \
    --name $AZURE_STORAGE_ACCOUNT \
    --resource-group $resourceGroupName

# Remove resource group
az group delete \
    --name $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure CLI'yi kullanarak Azure HDInsight'ta Bir Apache Spark kümesi oluşturmayı öğrendiniz.  Örnek verilerüzerinde etkileşimli sorguları çalıştırmak için HDInsight kümesini nasıl kullanacağınızı öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Apache Spark'ta etkileşimli sorgular çalıştırma](./apache-spark-load-data-run-query.md)
