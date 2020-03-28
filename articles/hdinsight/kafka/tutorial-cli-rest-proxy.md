---
title: "Öğretici: Azure CLI kullanarak HDInsight'ta Apache Kafka REST proxy etkin küme oluşturma"
description: Azure HDInsight'ta Kafka REST proxy'sini kullanarak Apache Kafka işlemlerini nasıl gerçekleştireceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: tutorial
ms.date: 02/27/2020
ms.openlocfilehash: 6ba5e433839d1f27c9522749fd7a8831c7243aae
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78201888"
---
# <a name="tutorial-create-an-apache-kafka-rest-proxy-enabled-cluster-in-hdinsight-using-azure-cli"></a>Öğretici: Azure CLI kullanarak HDInsight'ta Apache Kafka REST proxy etkin küme oluşturma

Bu eğitimde, Azure komut satırı arabirimini (CLI) kullanarak Azure HDInsight'ta Apache Kafka [REST proxy etkin](./rest-proxy.md) küme oluşturmayı öğrenirsiniz. Azure HDInsight kuruluşlara yönelik, yönetilen, tam spektrumlu ve açık kaynaklı bir analiz hizmetidir. Apache Kafka açık kaynaklı, dağıtılmış bir akış platformudur. Yayımla-abone ol ileti kuyruğuna benzer işlevler sağladığı için genellikle ileti aracısı olarak kullanılır. Kafka REST Proxy, HTTP üzerinden [bir REST API](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy/) üzerinden Kafka kümenizle etkileşimkurmanızı sağlar. Azure CLI, Azure kaynaklarını yönetmek için Microsoft tarafından sunulan platformlar arası komut satırı deneyimidir.

Apache Kafka API'sine yalnızca aynı sanal ağ içindeki kaynaklar tarafından erişilebilir. SSH kullanarak kümeye doğrudan erişebilirsiniz. Diğer hizmetleri, ağları veya sanal makineleri Apache Kafka'ya bağlamak için önce bir sanal ağ oluşturmanız e sonra ağ içinde kaynakları oluşturmanız gerekir. Daha fazla bilgi için sanal [ağ kullanarak Apache Kafka'ya Bağlan'a](./apache-kafka-connect-vpn-gateway.md)bakın.

Bu öğreticide şunları öğreniyorsunuz:

> [!div class="checklist"]
> * Kafka REST vekili için ön koşullar
> * Azure CLI'yi kullanarak Bir Apache Kafka kümesi oluşturma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure AD'ye kayıtlı bir uygulama. Kafka REST proxy ile etkileşim kurmak için yazdığınız istemci uygulamaları, Azure'a kimlik doğrulamak için bu uygulamanın kimliğini ve gizli sini kullanır. Daha fazla bilgi için [bkz.](../../active-directory/develop/quickstart-register-app.md)

* Üye olarak kayıtlı uygulamanıza sahip bir Azure REKLAM güvenlik grubu. Bu güvenlik grubu, hangi uygulamaların REST proxy ile etkileşime girebilmek için izin verildiğini denetlemek için kullanılır. Azure REKLAM grupları oluşturma hakkında daha fazla bilgi için temel [bir grup oluştur ve Azure Etkin Dizini'ni kullanarak üye ekleyin'](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)e bakın.

* Azure CLI. En az sürüm 2.0.79 olduğundan emin olun. Bkz. [Azure CLI'yi yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli)

## <a name="create-an-apache-kafka-cluster"></a>Apache Kafka kümesi oluşturma

1. Azure aboneliğinizde oturum açın.

    ```azurecli
    az login
    
    # If you have multiple subscriptions, set the one to use
    # az account set --subscription "SUBSCRIPTIONID"
    ```

1. Ortam değişkenlerini ayarlayın. Bu öğreticideğişkenlerin kullanımı Bash dayanmaktadır. Diğer ortamlar için küçük varyasyonlar gerekecektir.

    |Değişken | Açıklama |
    |---|---|
    |resourceGroupName|RESOURCEGROUPNAME'yi yeni kaynak grubunuzun adı ile değiştirin.|
    |location|KONUM'u kümenin oluşturulacağı bir bölgeyle değiştirin. Geçerli konumların listesi için, `az account list-locations` komutu kullanın|
    |clusterName|CLUSTERNAME'yi yeni kümeniz için genel olarak benzersiz bir adla değiştirin.|
    |storageAccount|STORAGEACCOUNTNAME'i yeni depolama hesabınız için bir adla değiştirin.|
    |httpŞifre|Küme giriş için bir şifre ile PASSWORD değiştirin, **admin**.|
    |sshPassword|Güvenli kabuk kullanıcı adı, sshuser için bir şifre ile PASSWORD **değiştirin.**|
    |securityGroupName|SECURITYGROUPNAME'yi Kafka Rest Proxy için istemci AAD güvenlik grubu adı ile değiştirin. Değişken için `--kafka-client-group-name` parametreye `az-hdinsight-create`geçirilecektir.|
    |securityGroupID|SecurityGROUPID'yi Kafka Rest Proxy için istemci AAD güvenlik grubu kimliğiyle değiştirin. Değişken için `--kafka-client-group-id` parametreye `az-hdinsight-create`geçirilecektir.|
    |depolamaKonteyner|Kümenin kullanacağı depolama kapsayıcısı, bu öğretici için olduğu gibi bırakın. Bu değişken kümenin adı ile ayarlanır.|
    |işçi nodeSayısı|Kümedeki işçi düğümlerinin sayısı, bu öğretici için olduğu gibi bırakın. Yüksek kullanılabilirliği garanti etmek için, Kafka en az 3 işçi düğümü gerektirir|
    |clusterType|HDInsight küme türü, bu öğretici için olduğu gibi bırakın.|
    |clusterVersion|HDInsight küme sürümü, bu öğretici için olduğu gibi bırakın. Kafka Rest Proxy 4.0 minimum küme sürümü gerektirir.|
    |componentVersion|Kafka sürümü, bu öğretici için olduğu gibi bırakın. Kafka Rest Proxy 2.1 minimum bileşen sürümü gerektirir.|

    Değişkenleri istenilen değerlerle güncelleştirin. Ardından ortam değişkenlerini ayarlamak için CLI komutlarını girin.

    ```azurecli
    export resourceGroupName=RESOURCEGROUPNAME
    export location=LOCATION
    export clusterName=CLUSTERNAME
    export storageAccount=STORAGEACCOUNTNAME
    export httpPassword='PASSWORD'
    export sshPassword='PASSWORD'
    export securityGroupName=SECURITYGROUPNAME
    export securityGroupID=SECURITYGROUPID

    export storageContainer=$(echo $clusterName | tr "[:upper:]" "[:lower:]")
    export workernodeCount=3
    export clusterType=kafka
    export clusterVersion=4.0
    export componentVersion=kafka=2.1
    ```

1. Aşağıdaki komutu girerek [kaynak grubu oluşturun:](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create)

    ```azurecli
     az group create \
        --location $location \
        --name $resourceGroupName
    ```

1. Aşağıdaki komutu girerek [bir Azure Depolama hesabı oluşturun:](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create)

    ```azurecli
    # Note: kind BlobStorage is not available as the default storage account.
    az storage account create \
        --name $storageAccount \
        --resource-group $resourceGroupName \
        --https-only true \
        --kind StorageV2 \
        --location $location \
        --sku Standard_LRS
    ```

1. [Birincil anahtarı](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest#az-storage-account-keys-list) Azure Depolama hesabından çıkarın ve aşağıdaki komutu girerek bir değişkende saklayın:

    ```azurecli
    export storageAccountKey=$(az storage account keys list \
        --account-name $storageAccount \
        --resource-group $resourceGroupName \
        --query [0].value -o tsv)
    ```

1. Aşağıdaki komutu girerek [bir Azure Depolama kapsayıcısı oluşturun:](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create)

    ```azurecli
    az storage container create \
        --name $storageContainer \
        --account-key $storageAccountKey \
        --account-name $storageAccount
    ```

1. [HDInsight kümesini oluşturun.](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) Komutu girmeden önce aşağıdaki parametrelere dikkat edin:

    1. Kafka kümeleri için gerekli parametreler:

        |Parametre | Açıklama|
        |---|---|
        |--türü|Değeri **Kafka**olmalı.|
        |--işçi düğümü-veri-diskler-düğüm başına|Alt düğüm başına kullanılacak veri disklerinin sayısı. HDInsight Kafka yalnızca veri diskleri ile desteklenir. Bu öğretici **2**değerini kullanır.|

    1. Kafka REST proxy için gerekli parametreler:

        |Parametre | Açıklama|
        |---|---|
        |--kafka-yönetim-düğüm boyutu|Düğümün boyutu. Bu öğretici değeri **Standard_D4_v2**kullanır.|
        |--kafka-istemci-grup-id|Kafka Rest Proxy için istemci AAD güvenlik grubu kimliği. Değer **$securityGroupID**değişkeninden geçirilir.|
        |--kafka-istemci-grup adı|Kafka Rest Proxy için istemci AAD güvenlik grubu adı. Değer **$securityGroupName**değişkeninden geçirilir.|
        |--sürüm|HDInsight küme sürümü en az 4.0 olmalıdır. Değer **$clusterVersion**değişkeninden geçirilir.|
        |--bileşen sürümü|Kafka versiyonu en az 2.1 olmalıdır. Değer **$componentVersion**değişkeninden geçirilir.|
    
        Rest proxy olmadan küme oluşturmak istiyorsanız, `--kafka-management-node-size`ortadan `--kafka-client-group-id`kaldırmak `--kafka-client-group-name` , `az hdinsight create` ve komut.

    1. Varolan bir sanal ağınız varsa, parametreleri `--vnet-name` ve `--subnet`değerlerini ekleyin.

    Küme oluşturmak için aşağıdaki komutu girin:

    ```azurecli
    az hdinsight create \
        --name $clusterName \
        --resource-group $resourceGroupName \
        --type $clusterType \
        --component-version $componentVersion \
        --http-password $httpPassword \
        --http-user admin \
        --location $location \
        --ssh-password $sshPassword \
        --ssh-user sshuser \
        --storage-account $storageAccount \
        --storage-account-key $storageAccountKey \
        --storage-container $storageContainer \
        --version $clusterVersion \
        --workernode-count $workernodeCount \
        --workernode-data-disks-per-node 2 \
        --kafka-management-node-size "Standard_D4_v2" \
        --kafka-client-group-id $securityGroupID \
        --kafka-client-group-name "$securityGroupName"
    ```

    Küme oluşturma işleminin tamamlanması birkaç dakika sürebilir. Genellikle 15 civarı.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Makaleyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile verileriniz Azure Depolama'da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Kullanılmamış olsa bile bir HDInsight kümesi için de ücretlendirilirsiniz. Küme ücretleri depolama ücretlerinden kat kat daha fazla olduğundan, kümeleri kullanılmadıklarında silmek ekonomik açıdan mantıklıdır.

Kaynakları kaldırmak için aşağıdaki komutların tümlerini veya bazılarını girin:

```azurecli
# Remove cluster
az hdinsight delete \
    --name $clusterName \
    --resource-group $resourceGroupName

# Remove storage container
az storage container delete \
    --account-name $storageAccount  \
    --name $storageContainer

# Remove storage account
az storage account delete \
    --name $storageAccount  \
    --resource-group $resourceGroupName

# Remove resource group
az group delete \
    --name $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI'yi kullanarak Azure HDInsight'ta bir Apache Kafka REST proxy etkin kümesini başarıyla oluşturduğunuza göre, REST proxy ile etkileşimkurmak için Python kodunu kullanın:

> [!div class="nextstepaction"]
> [Örnek uygulama oluşturma](./rest-proxy.md#client-application-sample)