---
title: HDInsight kümesini silme-Azure
description: Azure HDInsight kümesini silebilmeniz için çeşitli yollarla ilgili bilgiler
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.date: 11/29/2019
ms.openlocfilehash: 60f3ce0ad4f6ee6b1a38130867e0bcd1420620ef
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086373"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Tarayıcınızı, PowerShell 'i veya Azure CLı 'yı kullanarak bir HDInsight kümesini silme

HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Faturalandırma, dakika başına dağıtılır, bu nedenle artık kullanımda olmadığında kümenizi her zaman silmeniz gerekir. Bu belgede [Azure Portal](https://portal.azure.com), [Azure PowerShell az Module](https://docs.microsoft.com/powershell/azure/overview)ve [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)kullanarak bir kümeyi silmeyi öğreneceksiniz.

> [!IMPORTANT]  
> HDInsight kümesini silmek, kümeyle ilişkili Azure depolama hesaplarını veya Data Lake Storage silmez. Gelecekte bu hizmetlerde depolanan verileri yeniden kullanabilirsiniz.

## <a name="azure-portal"></a>Azure portal

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. Sol menüden **Tüm Service**  >  **Analytics**  >  **HDInsight kümelerine** gidin ve kümenizi seçin.

3. Varsayılan görünümden **Sil** simgesini seçin. Kümenizi silmek için istemi izleyin.

    ![HDInsight küme silme düğmesi](./media/hdinsight-delete-cluster/hdinsight-delete-cluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

`CLUSTERNAME`Aşağıdaki koddaki HDInsight kümenizin adıyla değiştirin. Bir PowerShell isteminden, kümeyi silmek için aşağıdaki komutu girin:

```powershell
Remove-AzHDInsightCluster -ClusterName CLUSTERNAME
```

## <a name="azure-cli"></a>Azure CLI

`CLUSTERNAME`' Yi HDInsight kümenizin adıyla ve `RESOURCEGROUP` kaynak grubunuzun adı ile aşağıdaki kodda olacak şekilde değiştirin.  Bir komut isteminden, kümeyi silmek için aşağıdakileri girin:

```azurecli
az hdinsight delete --name CLUSTERNAME --resource-group RESOURCEGROUP
```
