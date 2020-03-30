---
title: PowerShell ile Apache Hadoop kümelerini yönetme - Azure HDInsight
description: Azure PowerShell'i kullanarak HDInsight'taki Apache Hadoop kümeleri için yönetim görevlerini nasıl gerçekleştireceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/13/2020
ms.openlocfilehash: 104975e6424ed96d43434a588997957033c31d93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77560363"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak Apache Hadoop kümelerini HDInsight'ta yönetin

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell, Azure'daki iş yüklerinizin dağıtımını ve yönetimini denetlemek ve otomatikleştirmek için kullanılabilir. Bu makalede, Azure PowerShell Az modüllerini kullanarak Azure HDInsight'ta [Apache Hadoop](https://hadoop.apache.org/) kümelerini nasıl yöneteceğinizi öğreneceksiniz. HDInsight PowerShell cmdlets listesi için [Az.HDInsight referansına](https://docs.microsoft.com/powershell/module/az.hdinsight)bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell [Az Modülü](https://docs.microsoft.com/powershell/azure/overview) kuruldu.

## <a name="create-clusters"></a>Küme oluşturma

[Azure PowerShell kullanarak HDInsight'ta Linux tabanlı kümeler oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Liste kümeleri

Geçerli abonelikteki tüm kümeleri listelemek için aşağıdaki komutu kullanın:

```powershell
Get-AzHDInsightCluster
```

## <a name="show-cluster"></a>Kümeyi göster

Geçerli abonelikteki belirli bir kümenin ayrıntılarını göstermek için aşağıdaki komutu kullanın:

```powershell
Get-AzHDInsightCluster -ClusterName <Cluster Name>
```

## <a name="delete-clusters"></a>Kümeleri silme

Bir kümeyi silmek için aşağıdaki komutu kullanın:

```powershell
Remove-AzHDInsightCluster -ClusterName <Cluster Name>
```

Kümeyi içeren kaynak grubunu kaldırarak bir kümeyi de silebilirsiniz. Kaynak grubunu silmek, varsayılan depolama hesabı da dahil olmak üzere gruptaki tüm kaynakları siler.

```powershell
Remove-AzResourceGroup -Name <Resource Group Name>
```

## <a name="scale-clusters"></a>Ölçek kümeleri

Küme ölçekleme özelliği, kümeyi yeniden oluşturmak zorunda kalmadan Azure HDInsight'ta çalışan bir küme tarafından kullanılan alt düğüm sayısını değiştirmenize olanak tanır. Azure PowerShell'i kullanarak Hadoop küme boyutunu değiştirmek için istemci makinesinden aşağıdaki komutu çalıştırın:

```powershell
Set-AzHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
```

 Ölçekleme kümeleri hakkında daha fazla bilgi için, [Ölçek HDInsight kümelerine](./hdinsight-scaling-best-practices.md)bakın.

## <a name="update-http-user-credentials"></a>HTTP kullanıcı kimlik bilgilerini güncelleştirme

[Set-AzHDInsightGatewayCredential,](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightgatewaycredential) bir Azure HDInsight kümesinin giriş geçidi http kimlik bilgilerini ayarlar.

```powershell
$clusterName = "CLUSTERNAME"
$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"

Set-AzHDInsightGatewayCredential -ClusterName $clusterName -HttpCredential $credential
```

## <a name="find-the-default-storage-account"></a>Varsayılan depolama hesabını bulma

Aşağıdaki PowerShell komut dosyası, varsayılan depolama hesabı adının ve ilgili bilgilerin nasıl alınabildiğini gösterir:

```powershell
#Connect-AzAccount
$clusterName = "<HDInsight Cluster Name>"

$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$storageInfo = $clusterInfo.DefaultStorageAccount.split('.')
$defaultStoreageType = $storageInfo[1]
$defaultStorageName = $storageInfo[0]

echo "Default Storage account name: $defaultStorageName"
echo "Default Storage account type: $defaultStoreageType"

if ($defaultStoreageType -eq "blob")
{
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

    echo "Default Blob container name: $defaultBlobContainerName"
    echo "Default Storage account key: $defaultStorageAccountKey"
}
```

## <a name="find-the-resource-group"></a>Kaynak grubunu bulma

Kaynak Yöneticisi modunda, her HDInsight kümesi bir Azure kaynak grubuna aittir.  Kaynak grubunu bulmak için:

```powershell
$clusterName = "<HDInsight Cluster Name>"

$cluster = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $cluster.ResourceGroup
```

## <a name="submit-jobs"></a>İş gönderme

**MapReduce işlerini göndermek için**

Bkz. [HDInsight'ta yer alan MapReduce örneklerini çalıştırın.](hadoop/apache-hadoop-run-samples-linux.md)

**Apache Hive işlerini göndermek için**

Bkz. [PowerShell kullanarak Apache Hive sorgularını çalıştırın.](hadoop/apache-hadoop-use-hive-powershell.md)

**Apache Sqoop işleri göndermek için**

Bkz. [HDInsight ile Apache Sqoop kullanın.](hadoop/hdinsight-use-sqoop.md)

**Apache Oozie işleri göndermek için**

HdInsight'ta [bir iş akışını tanımlamak ve çalıştırmak için Apache Oozie'yi Apache Hadoop ile kullanın'](hdinsight-use-oozie-linux-mac.md)ı görün.

## <a name="upload-data-to-azure-blob-storage"></a>Azure Blob depolama alanına veri yükleme

Bkz. [HDInsight'a veri yükleme](hdinsight-upload-data.md).

## <a name="see-also"></a>Ayrıca Bkz.

* [Az.HDInsight cmdlets](https://docs.microsoft.com/powershell/module/az.hdinsight/?view=azps-3.1.0#hdinsight)
* [Azure portalını kullanarak HDInsight'ta Apache Hadoop kümelerini yönetme](hdinsight-administer-use-portal-linux.md)
* [Komut satırı arabirimi kullanarak HDInsight'ı yönetme](hdinsight-administer-use-command-line.md)
* [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md)
* [Apache Hadoop işlerini programlı olarak gönderin](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Azure HDInsight ile başlayın](hadoop/apache-hadoop-linux-tutorial-get-started.md)
