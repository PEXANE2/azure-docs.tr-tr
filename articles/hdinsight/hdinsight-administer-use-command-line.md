---
title: Azure CLı kullanarak Azure HDInsight kümelerini yönetme
description: Azure HDInsight kümelerini yönetmek için Azure CLı 'yı nasıl kullanacağınızı öğrenin. Küme türleri Apache Hadoop, Spark, HBase, fırtınası, Kafka, Interactive Query ve ML hizmetlerini içerir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 02/26/2020
ms.openlocfilehash: 2c6495454e5ba2449d4b3c74a096681f74610813
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79272779"
---
# <a name="manage-azure-hdinsight-clusters-using-azure-cli"></a>Azure CLı kullanarak Azure HDInsight kümelerini yönetme

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure HDInsight kümelerini yönetmek için [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) 'yi nasıl kullanacağınızı öğrenin. Azure komut satırı arabirimi (CLI), Azure kaynaklarını yönetmek için Microsoft tarafından sunulan platformlar arası komut satırı deneyimidir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Azure CLı. Azure CLı 'yı yüklemediyseniz, adımlar için bkz. [Azure CLI 'Yi yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) .

* HDInsight üzerinde bir Apache Hadoop kümesi. Bkz. [Linux 'Ta HDInsight kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).

## <a name="connect-to-azure"></a>Azure'a Bağlanma

Azure aboneliğinizde oturum açın. Azure Cloud Shell kullanmayı planlıyorsanız, kod bloğunun sağ üst köşesinde bulunan **bunu dene** ' yi seçin. Aksi takdirde, aşağıdaki komutu girin:

```azurecli-interactive
az login

# If you have multiple subscriptions, set the one to use
# az account set --subscription "SUBSCRIPTIONID"
```

## <a name="list-clusters"></a>Kümeleri Listele

Kümeleri listelemek için [az HDInsight List](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-list) kullanın. `RESOURCE_GROUP_NAME` kaynak grubunuzun adıyla değiştirerek aşağıdaki komutları düzenleyin ve ardından komutları girin:

```azurecli-interactive
# List all clusters in the current subscription
az hdinsight list

# List only cluster name and its resource group
az hdinsight list --query "[].{Cluster:name, ResourceGroup:resourceGroup}" --output table

# List all cluster for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME

# List all cluster names for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME --query "[].{clusterName:name}" --output table
```

## <a name="show-cluster"></a>Kümeyi göster

Belirtilen bir kümenin bilgilerini göstermek için [az HDInsight Show](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-show) ' i kullanın. `RESOURCE_GROUP_NAME`değiştirerek aşağıdaki komutu düzenleyin ve ilgili bilgilerle `CLUSTER_NAME`, sonra şu komutu girin:

```azurecli-interactive
az hdinsight show --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

## <a name="delete-clusters"></a>Kümeleri Sil

Belirtilen kümeyi silmek için [az HDInsight Delete](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-delete) kullanın. `RESOURCE_GROUP_NAME`değiştirerek aşağıdaki komutu düzenleyin ve ilgili bilgilerle `CLUSTER_NAME`, sonra şu komutu girin:

```azurecli-interactive
az hdinsight delete --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

Kümeyi içeren kaynak grubunu silerek de bir kümeyi silebilirsiniz. Bu, varsayılan depolama hesabı da dahil olmak üzere gruptaki tüm kaynakların silineceğini aklınızda görürsünüz.

```azurecli-interactive
az group delete --name RESOURCE_GROUP_NAME
```

## <a name="scale-clusters"></a>Ölçek Kümeleri

Belirtilen HDInsight kümesini belirtilen boyuta göre yeniden boyutlandırmak için [az HDInsight Resize](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) ' i kullanın. `RESOURCE_GROUP_NAME`değiştirerek aşağıdaki komutu düzenleyin ve ilgili bilgilerle `CLUSTER_NAME`. `WORKERNODE_COUNT`, kümeniz için istenen çalışan düğüm sayısıyla değiştirin. Kümeleri ölçeklendirme hakkında daha fazla bilgi için bkz. [HDInsight kümelerini ölçeklendirme](./hdinsight-scaling-best-practices.md). Şu komutu girin:

```azurecli-interactive
az hdinsight resize --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME --workernode-count WORKERNODE_COUNT
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, farklı HDInsight kümesi yönetim görevlerinin nasıl gerçekleştirileceğini öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight 'ta Apache Hadoop kümelerini Azure portal kullanarak yönetin](hdinsight-administer-use-portal-linux.md)
* [Azure PowerShell kullanarak HDInsight 'ı yönetme](hdinsight-administer-use-powershell.md)
* [Azure HDInsight 'ı kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure CLI'yi kullanmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)
