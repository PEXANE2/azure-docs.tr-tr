---
title: HDInsight kümesini silme-Azure
description: Azure HDInsight kümesini silebilmeniz için çeşitli yollarla ilgili bilgiler
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.date: 11/29/2019
ms.openlocfilehash: 2912777c001a36ec913e2cfd618091ada5aa107a
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74807145"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Tarayıcınızı, PowerShell 'i veya Azure CLı 'yı kullanarak bir HDInsight kümesini silme

HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Faturalandırma, dakika başına dağıtılır, bu nedenle artık kullanımda olmadığında kümenizi her zaman silmeniz gerekir. Bu belgede [Azure Portal](https://portal.azure.com), [Azure PowerShell az Module](https://docs.microsoft.com/powershell/azure/overview)ve [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)kullanarak bir kümeyi silmeyi öğreneceksiniz.

> [!IMPORTANT]  
> HDInsight kümesini silmek, kümeyle ilişkili Azure depolama hesaplarını veya Data Lake Storage silmez. Gelecekte bu hizmetlerde depolanan verileri yeniden kullanabilirsiniz.

## <a name="azure-portal"></a>Azure portalı

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. Sol menüden **tüm hizmetler** > **analiz** > **HDInsight kümelerine** gidin ve kümenizi seçin.

3. Varsayılan görünümden **Sil** simgesini seçin. Kümenizi silmek için istemi izleyin.

    ![HDInsight küme silme düğmesi](./media/hdinsight-delete-cluster/hdinsight-delete-cluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

`CLUSTERNAME` aşağıdaki koddaki HDInsight kümenizin adıyla değiştirin. Bir PowerShell isteminden, kümeyi silmek için aşağıdaki komutu girin:

```powershell
Remove-AzHDInsightCluster -ClusterName CLUSTERNAME
```

## <a name="azure-cli"></a>Azure CLI

`CLUSTERNAME`, HDInsight kümenizin adıyla değiştirin ve aşağıdaki kodda kaynak grubunuzun adı ile `RESOURCEGROUP`.  Bir komut isteminden, kümeyi silmek için aşağıdakileri girin:

```azurecli
az hdinsight delete --name CLUSTERNAME --resource-group RESOURCEGROUP
```
