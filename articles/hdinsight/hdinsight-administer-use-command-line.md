---
title: Azure CLI'yi kullanarak Azure HDInsight kümelerini yönetme
description: Azure HDInsight kümelerini yönetmek için Azure CLI'yi nasıl kullanacağınızı öğrenin. Küme türleri Apache Hadoop, Spark, HBase, Storm, Kafka, Interactive Query ve ML Hizmetlerini içerir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 02/26/2020
ms.openlocfilehash: 2c6495454e5ba2449d4b3c74a096681f74610813
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272779"
---
# <a name="manage-azure-hdinsight-clusters-using-azure-cli"></a>Azure CLI'yi kullanarak Azure HDInsight kümelerini yönetme

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure HDInsight kümelerini yönetmek için [Azure CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) nasıl kullanacağınızı öğrenin. Azure komut satırı arabirimi (CLI), Azure kaynaklarını yönetmek için Microsoft tarafından sunulan platformlar arası komut satırı deneyimidir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure CLI. Azure CLI'yi yüklemediyseniz, adımlar için [Azure CLI'yi yükleyin'e](https://docs.microsoft.com/cli/azure/install-azure-cli) bakın.

* HDInsight'ta bir Apache Hadoop kümesi. [Linux'ta HDInsight ile başlayın](hadoop/apache-hadoop-linux-tutorial-get-started.md)bakın.

## <a name="connect-to-azure"></a>Azure'a Bağlanma

Azure aboneliğinizde oturum açın. Azure Bulut Su Şurası'nı kullanmayı planlıyorsanız, kod bloğunun sağ üst köşesinde **deneyin'i** seçin. Else, aşağıdaki komutu girin:

```azurecli-interactive
az login

# If you have multiple subscriptions, set the one to use
# az account set --subscription "SUBSCRIPTIONID"
```

## <a name="list-clusters"></a>Liste kümeleri

Kümeleri listelemek için [az hdinsight listesini](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-list) kullanın. Kaynak grubunuzun adını değiştirerek `RESOURCE_GROUP_NAME` aşağıdaki komutları düzenleme, ardından komutları girin:

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

Belirtilen kümenin bilgilerini göstermek için [az hdinsight göster'i](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-show) kullanın. Aşağıdaki komutu değiştirerek `RESOURCE_GROUP_NAME`ve `CLUSTER_NAME` ilgili bilgileri değiştirerek düzenleme yapın ve ardından komutu girin:

```azurecli-interactive
az hdinsight show --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

## <a name="delete-clusters"></a>Kümeleri silme

Belirtilen bir kümeyi silmek için [az hdinsight delete'yi](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-delete) kullanın. Aşağıdaki komutu değiştirerek `RESOURCE_GROUP_NAME`ve `CLUSTER_NAME` ilgili bilgileri değiştirerek düzenleme yapın ve ardından komutu girin:

```azurecli-interactive
az hdinsight delete --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

Kümeyi içeren kaynak grubunu silerek bir kümeyi de silebilirsiniz. Not, bu varsayılan depolama hesabı da dahil olmak üzere gruptaki tüm kaynakları siler.

```azurecli-interactive
az group delete --name RESOURCE_GROUP_NAME
```

## <a name="scale-clusters"></a>Ölçek kümeleri

Belirtilen HDInsight kümesini belirtilen boyuta yeniden boyutlandırmak için [az hdinsight yeniden boyutlandırmasını](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) kullanın. Aşağıdaki komutu değiştirerek `RESOURCE_GROUP_NAME`ve `CLUSTER_NAME` ilgili bilgileri değiştirerek düzenleme. Kümeniz için istenen sayıda alt düğümle değiştirin. `WORKERNODE_COUNT` Ölçekleme kümeleri hakkında daha fazla bilgi için, [Ölçek HDInsight kümelerine](./hdinsight-scaling-best-practices.md)bakın. Komutu girin:

```azurecli-interactive
az hdinsight resize --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME --workernode-count WORKERNODE_COUNT
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, farklı HDInsight küme yönetim görevlerini nasıl gerçekleştireceklerini öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure portalını kullanarak HDInsight'ta Apache Hadoop kümelerini yönetme](hdinsight-administer-use-portal-linux.md)
* [Azure PowerShell'i kullanarak HDInsight'ı yönetin](hdinsight-administer-use-powershell.md)
* [Azure HDInsight ile başlayın](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure CLI'yi kullanmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)
