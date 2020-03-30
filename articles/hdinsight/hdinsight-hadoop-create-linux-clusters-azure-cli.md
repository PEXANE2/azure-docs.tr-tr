---
title: Azure CLI kullanarak Apache Hadoop kümeleri oluşturun - Azure HDInsight
description: Platformlar arası Azure CLI'yi kullanarak Azure HDInsight kümelerini nasıl oluşturabilirsiniz öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/03/2020
ms.openlocfilehash: b9d935e72c67b78484337e39e0897d4962340636
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77199050"
---
# <a name="create-hdinsight-clusters-using-the-azure-cli"></a>Azure CLI'yi kullanarak HDInsight kümeleri oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bu belgedeki adımlar, Azure CLI'yi kullanarak bir HDInsight 3.6 kümesi oluşturmada geçer.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Azure CLI. Azure CLI'yi yüklemediyseniz, adımlar için [Azure CLI'yi yükleyin'e](https://docs.microsoft.com/cli/azure/install-azure-cli) bakın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-cluster"></a>Küme oluşturma

1. Azure aboneliğinizde oturum açın. Azure Bulut Su Şurası'nı kullanmayı planlıyorsanız, kod bloğunun sağ üst köşesinde **deneyin'i** seçin. Else, aşağıdaki komutu girin:

    ```azurecli-interactive
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription "SUBSCRIPTIONID"
    ```

2. Ortam değişkenlerini ayarlayın. Bu makalede değişkenlerin kullanımı Bash dayanmaktadır. Diğer ortamlar için küçük varyasyonlar gerekecektir. Küme oluşturma için olası parametrelerin tam listesi için [az-hdinsight-create'a](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) bakın.

    |Parametre | Açıklama |
    |---|---|
    |`--workernode-count`| Kümedeki alt düğüm sayısı. Bu makalede, `clusterSizeInNodes` `--workernode-count`'ye geçirilen değer olarak değişken kullanır. |
    |`--version`| HDInsight küme sürümü. Bu makalede, `clusterVersion` `--version`'ye geçirilen değer olarak değişken kullanır. Ayrıca bakınız: [Desteklenen HDInsight sürümleri.](./hdinsight-component-versioning.md#supported-hdinsight-versions)|
    |`--type`| HDInsight küme türü, gibi: hadoop, interaktif hive, hbase, kafka, fırtına, kıvılcım, rserver, mlservices.  Bu makalede, `clusterType` `--type`'ye geçirilen değer olarak değişken kullanır. Ayrıca bakınız: [Küme türleri ve yapılandırma.](./hdinsight-hadoop-provision-linux-clusters.md#cluster-type)|
    |`--component-version`|Çeşitli Hadoop bileşenlerinin 'component=version' formatında uzaya ayrılmış versiyonları. Bu makalede, `componentVersion` `--component-version`'ye geçirilen değer olarak değişken kullanır. Ayrıca bakınız: [Hadoop bileşenleri.](./hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions)|

    Değiştir `RESOURCEGROUPNAME` `LOCATION`, `CLUSTERNAME` `STORAGEACCOUNTNAME`, `PASSWORD` , , ve istenilen değerlerle. Diğer değişkenlerin değerlerini istediğiniz gibi değiştirin. Ardından CLI komutlarını girin.

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
    export clusterType=hadoop
    export componentVersion=Hadoop=2.7
    ```

3. Aşağıdaki komutu girerek [kaynak grubu oluşturun:](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create)

    ```azurecli-interactive
    az group create \
        --location $location \
        --name $resourceGroupName
    ```

    Geçerli konumların listesi için `az account list-locations` komutu kullanın ve ardından `name` değerden konumlardan birini kullanın.

4. Aşağıdaki komutu girerek [bir Azure Depolama hesabı oluşturun:](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create)

    ```azurecli-interactive
    # Note: kind BlobStorage is not available as the default storage account.
    az storage account create \
        --name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --https-only true \
        --kind StorageV2 \
        --location $location \
        --sku Standard_LRS
    ```

5. [Birincil anahtarı Azure Depolama hesabından çıkarın](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest#az-storage-account-keys-list) ve aşağıdaki komutu girerek bir değişkende saklayın:

    ```azurecli-interactive
    export AZURE_STORAGE_KEY=$(az storage account keys list \
        --account-name $AZURE_STORAGE_ACCOUNT \
        --resource-group $resourceGroupName \
        --query [0].value -o tsv)
    ```

6. Aşağıdaki komutu girerek [bir Azure Depolama kapsayıcısı oluşturun:](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create)

    ```azurecli-interactive
    az storage container create \
        --name $AZURE_STORAGE_CONTAINER \
        --account-key $AZURE_STORAGE_KEY \
        --account-name $AZURE_STORAGE_ACCOUNT
    ```

7. Aşağıdaki komutu girerek [HDInsight kümesini oluşturun:](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create)

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

    > [!IMPORTANT]  
    > HDInsight kümeleri, kümenin ayarlandığı iş yüküne veya teknolojiye karşılık gelen çeşitli türlerde gelir. Bir kümede Fırtına ve HBase gibi birden çok türü birleştiren bir küme oluşturmak için desteklenen bir yöntem yoktur.

    Küme oluşturma işleminin tamamlanması birkaç dakika sürebilir. Genellikle 15 civarı.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Makaleyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

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

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](./hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI'yi kullanarak bir HDInsight kümesini başarıyla oluşturduğunuza göre, kümenizle nasıl çalışacağınızı öğrenmek için aşağıdakileri kullanın:

### <a name="apache-hadoop-clusters"></a>Apaçi Hadoop kümeleri

* [HDInsight ile Apache Hive'ı kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile MapReduce'ı kullanın](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase kümeleri

* [HDInsight'ta Apache HBase ile başlayın](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight'ta Apache HBase için Java uygulamaları geliştirin](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apaçi Fırtına kümeleri

* [HDInsight'ta Apache Storm için Java topolojileri geliştirin](storm/apache-storm-develop-java-topology.md)
* [HDInsight'ta Apache Storm'da Python bileşenlerini kullanma](storm/apache-storm-develop-python-topology.md)
* [HDInsight'ta Apache Storm ile topolojileri dağıtın ve izleyin](storm/apache-storm-deploy-monitor-topology-linux.md)
