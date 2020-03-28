---
title: 'Azure HDInsight: Azure CLI örnekleri'
description: Azure HDInsight'ta sık kullanılan görevler için Azure CLI örnekleri.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: sample
ms.date: 09/23/2019
ms.openlocfilehash: cbbb38c645e56b2e7b8c70b437b1e5158b09a50b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78207107"
---
# <a name="azure-hdinsight-azure-cli-samples"></a>Azure HDInsight: Azure CLI örnekleri

> [!div class="op_single_selector"]
> [.NET Örnekleri](hdinsight-sdk-dotnet-samples.md)
> [Python Örnekleri](hdinsight-sdk-python-samples.md)
> [Java Örnekleri](hdinsight-sdk-java-samples.md)

Bu makalede, ortak görevler için örnek komut dosyaları sağlar. Her örnek için, değişkenleri uygun değerlerle güncelleştirin ve ardından komutu çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

* Azure CLI. Adımlar için [Azure CLI'yi yükleyin'](https://docs.microsoft.com/cli/azure/install-azure-cli) e bakın.

* İsteğe bağlı: Bash. Bu makaledeki örnekler, Windows 10'daki Bash kabuğunu kullanır. Yükleme adımları [için Windows 10 için Linux Yükleme Kılavuzu](https://docs.microsoft.com/windows/wsl/install-win10) için Windows Alt Sistemi'ne bakın.  Örnekler, bazı küçük değişikliklerle bir Windows Komut istemi çalışır.

## <a name="az-login"></a>az login

[Azure'da oturum açın.](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login)

```azurecli
az login

# If you have multiple subscriptions, set the one to use
# az account set --subscription "SUBSCRIPTIONID"
```

## <a name="az-hdinsight-create"></a>az hdinsight oluşturmak

[Yeni bir küme oluşturur.](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create)

### <a name="create-a-cluster-with-an-existing-storage-account"></a>Varolan bir depolama hesabıyla küme oluşturma

```azurecli
# set variables
export clusterName=CLUSTERNAME
export resourceGroupName=RESOURCEGROUPNAME
export clusterType=hadoop
export httpCredential='PASSWORD'
export AZURE_STORAGE_ACCOUNT=STORAGEACCOUNTNAME

az hdinsight create \
    --name $clusterName \
    --resource-group $resourceGroupName \
    --type $clusterType \
    --http-password $httpCredential \
    --storage-account $AZURE_STORAGE_ACCOUNT
```

### <a name="create-a-cluster-with-the-enterprise-security-package-esp"></a>Kurumsal Güvenlik Paketi (ESP) ile küme oluşturma

```azurecli
export clusterName=CLUSTERNAME
export resourceGroupName=RESOURCEGROUPNAME
export clusterType=spark
export httpCredential='PASSWORD'
export AZURE_STORAGE_ACCOUNT=STORAGEACCOUNTNAME
export subnet="/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworks/MyVnet/subnets/subnet1"
export domain="/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyRG/providers/Microsoft.AAD/domainServices/MyDomain.onmicrosoft.com"
export userAssignedIdentity="/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/MyMsiRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/MyMSI"
export domainAccount=MyAdminAccount@MyDomain.onmicrosoft.com
export groupDNS=MyGroup

az hdinsight create \
    --esp \
    --name $clusterName \
    --resource-group $resourceGroupName \
    --type $clusterType \
    --http-password $httpCredential \
    --storage-account $AZURE_STORAGE_ACCOUNT \
    --subnet $subnet \
    --domain $domain \
    --assign-identity $userAssignedIdentity \
    --cluster-admin-account $domainAccount \
    --cluster-users-group-dns $groupDNS
```

### <a name="create-a-kafka-cluster-with-disk-encryption"></a>[Disk şifrelemeli](./disk-encryption.md) kafka kümesi oluşturma

```azurecli
export clusterName=CLUSTERNAME
export resourceGroupName=RESOURCEGROUPNAME
export clusterType=kafka
export httpCredential='PASSWORD'
export AZURE_STORAGE_ACCOUNT=STORAGEACCOUNTNAME
export encryptionKeyName=kafkaClusterKey
export encryptionKeyVersion=00000000000000000000000000000000
export encryptionVaultUri=https://MyKeyVault.vault.azure.net
export userAssignedIdentity="/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/MyMsiRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/MyMSI"

az hdinsight create \
    --name $clusterName \
    --resource-group $resourceGroupName \
    --type $clusterType \
    --http-password $httpCredential \
    --storage-account $AZURE_STORAGE_ACCOUNT \
    --workernode-data-disks-per-node 2 \
    --encryption-key-name $encryptionKeyName \
    --encryption-key-version $encryptionKeyVersion \
    --encryption-vault-uri $encryptionVaultUri \
    --assign-identity $userAssignedIdentity
```

### <a name="create-a-cluster-with-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2 ile küme oluşturma

```azurecli
export clusterName=CLUSTERNAME
export resourceGroupName=RESOURCEGROUPNAME
export clusterType=spark
export httpCredential='PASSWORD'
export adlgen2=MyStorageAccount
export sami=MyMSI

az hdinsight create \
    --name $clusterName \
    --resource-group $resourceGroupName \
    --type $clusterType \
    --http-password $httpCredential \
    --storage-account $adlgen2 \
    --storage-account-managed-identity $sami
```

### <a name="create-a-cluster-with-configuration-from-json-string"></a>JSON dizesinden yapılandırma içeren bir küme oluşturma

```azurecli
export clusterName=CLUSTERNAME
export resourceGroupName=RESOURCEGROUPNAME
export clusterType=spark
export httpCredential='PASSWORD'
export AZURE_STORAGE_ACCOUNT=STORAGEACCOUNTNAME
export clusterConfiguration="{'gateway':{'restAuthCredential.username':'admin'}}"

az hdinsight create \
    --name $clusterName \
    --resource-group $resourceGroupName \
    --type $clusterType \
    --http-password $httpCredential \
    --storage-account $AZURE_STORAGE_ACCOUNT \
    --cluster-configuration $clusterConfiguration
```

### <a name="create-a-cluster-with-configuration-from-a-local-file"></a>Yerel bir dosyadan yapılandırma içeren bir küme oluşturma

```azurecli
export clusterName=CLUSTERNAME
export resourceGroupName=RESOURCEGROUPNAME
export clusterType=spark
export httpCredential='PASSWORD'
export AZURE_STORAGE_ACCOUNT=STORAGEACCOUNTNAME
export clusterConfiguration=@/mnt/c/HDI/config.json

az hdinsight create \
    --name $clusterName \
    --resource-group $resourceGroupName \
    --type $clusterType \
    --http-password $httpCredential \
    --storage-account $AZURE_STORAGE_ACCOUNT \
    --cluster-configuration $clusterConfiguration
```

## <a name="az-hdinsight-application-create"></a>az hdinsight uygulaması oluşturmak

[HDInsight kümesi için bir uygulama oluşturun.](https://docs.microsoft.com/cli/azure/hdinsight/application?view=azure-cli-latest#az-hdinsight-application-create)

### <a name="create-an-application-with-a-script-uri"></a>Komut dosyası URI ile uygulama oluşturma

```azurecli
export resourceGroupName=RESOURCEGROUPNAME
export applicationName=MyApplication
export clusterName=CLUSTERNAME
export scriptURI=https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
export scriptActionName=MyScriptAction
export scriptParameters='"-version latest -port 20000"'

az hdinsight application create \
    --resource-group $resourceGroupName \
    --name $applicationName \
    --cluster-name $clusterName \
    --script-uri $scriptURI  \
    --script-action-name $scriptActionName \
    --script-parameters "$scriptParameters"
```

### <a name="create-an-application-with-a-script-uri-and-specified-edge-node-size"></a>Komut dosyası URI ve belirtilen kenar düğümü boyutuna sahip bir uygulama oluşturma

```azurecli
export resourceGroupName=RESOURCEGROUPNAME
export applicationName=MyApplication
export clusterName=CLUSTERNAME
export scriptURI=https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
export scriptActionName=MyScriptAction
export scriptParameters='"-version latest -port 20000"'
export edgenodeSize=Standard_D4_v2

az hdinsight application create \
    --resource-group $resourceGroupName \
    --name $applicationName \
    --cluster-name $clusterName \
    --script-uri $scriptURI  \
    --script-action-name $scriptActionName \
    --script-parameters "$scriptParameters" \
    --edgenode-size $edgenodeSize
```

### <a name="create-an-application-with-https-endpoint"></a>HTTPS Endpoint ile bir uygulama oluşturma

```azurecli
export resourceGroupName=RESOURCEGROUPNAME
export applicationName=MyApplication
export clusterName=CLUSTERNAME
export scriptURI=https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
export scriptActionName=MyScriptAction
export scriptParameters='"-version latest -port 20000"'
export destinationPort=8888
export subDomainSuffix=was

az hdinsight application create \
    --resource-group $resourceGroupName \
    --name $applicationName \
    --cluster-name $clusterName \
    --script-uri $scriptURI  \
    --script-action-name $scriptActionName \
    --script-parameters "$scriptParameters" \
    --destination-port $destinationPort \
    --sub-domain-suffix $subDomainSuffix
```

## <a name="az-hdinsight-script-action-execute"></a>az hdinsight komut dosyası-eylem yürütmek

[Belirtilen HDInsight kümesinde komut dosyası eylemlerini yürütün.](https://docs.microsoft.com/cli/azure/hdinsight/script-action?view=azure-cli-latest#az-hdinsight-script-action-execute)

### <a name="execute-a-script-action-and-persist-on-success"></a>Komut dosyası eylemini yürütme ve başarıya devam etme

```azurecli
export resourceGroupName=RESOURCEGROUPNAME
export clusterName=CLUSTERNAME
export scriptActionName=MyScriptAction
export scriptURI=https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
export roles="headnode workernode"

az hdinsight script-action execute \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --name $scriptActionName \
    --script-uri $scriptURI  \
    --roles $roles \
    --persist-on-success
```
