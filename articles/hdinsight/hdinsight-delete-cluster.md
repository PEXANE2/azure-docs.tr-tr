---
title: HDInsight kümesi nasıl silinir - Azure
description: Azure HDInsight kümesini silmenin çeşitli yolları hakkında bilgi
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.date: 11/29/2019
ms.openlocfilehash: 2912777c001a36ec913e2cfd618091ada5aa107a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74807145"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Tarayıcınızı, PowerShell'i veya Azure CLI'nizi kullanarak bir HDInsight kümesini silme

HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Faturalandırma dakikada eşit olarak değerlendirildiğinde, artık kullanılmadığında kümenizi her zaman silmeniz gerekir. Bu belgede, [Azure portalı,](https://portal.azure.com) [Azure PowerShell Az modülü](https://docs.microsoft.com/powershell/azure/overview)ve Azure [CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)kullanarak bir kümeyi nasıl sildiğinizi öğrenirsiniz.

> [!IMPORTANT]  
> HDInsight kümesini silmek, kümeyle ilişkili Azure Depolama hesaplarını veya Veri Gölü Depolamasını silmez. Gelecekte bu hizmetlerde depolanan verileri yeniden kullanabilirsiniz.

## <a name="azure-portal"></a>Azure portalında

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Sol menüden Tüm **hizmetler** > **Analytics** > **HDInsight kümelerine** gidin ve kümenizi seçin.

3. Varsayılan görünümden **Sil** simgesini seçin. Kümenizi silmek için komut istemini izleyin.

    ![HDInsight sil küme düğmesi](./media/hdinsight-delete-cluster/hdinsight-delete-cluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki `CLUSTERNAME` kodda HDInsight kümenizin adıyla değiştirin. PowerShell komut isteminden kümeyi silmek için aşağıdaki komutu girin:

```powershell
Remove-AzHDInsightCluster -ClusterName CLUSTERNAME
```

## <a name="azure-cli"></a>Azure CLI

HDInsight kümenizin adıyla ve `CLUSTERNAME` `RESOURCEGROUP` aşağıdaki koddaki kaynak grubunuzun adı ile değiştirin.  Komut isteminden, kümeyi silmek için aşağıdakileri girin:

```azurecli
az hdinsight delete --name CLUSTERNAME --resource-group RESOURCEGROUP
```
