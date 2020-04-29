---
title: Önyükleme kullanarak Azure HDInsight küme yapılandırmasını özelleştirme
description: .NET, PowerShell ve Kaynak Yöneticisi şablonlarını kullanarak HDInsight küme yapılandırmasını programlı bir şekilde özelleştirmeyi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/01/2020
ms.openlocfilehash: 796dbc53d1adf310028e06dea319b9a60d5cf54b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80529358"
---
# <a name="customize-hdinsight-clusters-using-bootstrap"></a>Önyükleme kullanarak HDInsight kümelerini özelleştirme

Önyükleme betikleri, Azure HDInsight 'taki bileşenleri programlı bir şekilde yüklemenize ve yapılandırmanıza olanak tanır.

HDInsight kümeniz oluşturulurken yapılandırma dosyası ayarlarını ayarlamak için üç yaklaşım vardır:

* Azure PowerShell kullanma
* .NET SDK kullanma
* Azure Resource Manager şablonu kullan

Örneğin, bu programlama yöntemlerini kullanarak bu dosyalardaki seçenekleri yapılandırabilirsiniz:

* Clusterıdentity. xml
* Core-site. xml
* Gateway. xml
* HBase-env. xml
* HBase-site. xml
* HDFS-site. xml
* Hive-env. xml
* Hive-site. xml
* mapred-site
* Oozie-site. xml
* Oozie-env. xml
* Storm-site. xml
* Tez-site. xml
* webhcat-site. xml
* Yarn-site. xml
* Server. Properties (Kafka-Broker yapılandırması)

Oluşturma süresi boyunca HDInsight kümesine ek bileşenler yükleme hakkında bilgi için bkz. [betik eylemi kullanarak HDInsight kümelerini özelleştirme (Linux)](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="prerequisites"></a>Ön koşullar

* PowerShell kullanıyorsanız [az Module](https://docs.microsoft.com/powershell/azure/overview)gerekecektir.

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Aşağıdaki PowerShell kodu bir [Apache Hive](https://hive.apache.org/) yapılandırmasını özelleştirir:

> [!IMPORTANT]  
> Parametrenin `Spark2Defaults` [Add-AzHDInsightConfigValue](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightconfigvalue)ile kullanılması gerekebilir. Aşağıdaki kod örneğinde gösterildiği gibi parametreye boş değerler geçirebilirsiniz.

```powershell
# hive-site.xml configuration
$hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90s" }

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValue `
        -HiveSite $hiveConfigValues `
        -Spark2Defaults @{}

New-AzHDInsightCluster `
    -ResourceGroupName $existingResourceGroupName `
    -ClusterName $clusterName `
    -Location $location `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -Config $config
```

Çalışan bir PowerShell betiği, [ek](#appendix-powershell-sample)içinde bulunabilir.

**Değişikliği doğrulamak için:**

1. Kümenizin adı `https://CLUSTERNAME.azurehdinsight.net/` olduğu `CLUSTERNAME` yere gidin.
1. Sol menüden **Hive** > **configs** > **İleri**' ye gidin.
1. **Gelişmiş Hive-site**' ı genişletin.
1. **Hive. metasarı. Client. Socket. Timeout** öğesini bulun ve değerin **90**olduğunu onaylayın.

Diğer yapılandırma dosyalarını özelleştirmeye ilişkin bazı örnekler:

```xml
# hdfs-site.xml configuration
$HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

# core-site.xml configuration
$CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

# mapred-site.xml configuration
$MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

# oozie-site.xml configuration
$OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120
```

## <a name="use-net-sdk"></a>.NET SDK kullanma

Bkz. [.net Için Azure HDıNSIGHT SDK](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet).

## <a name="use-resource-manager-template"></a>Resource Manager şablonu kullanma

Kaynak Yöneticisi şablonunda önyükleme kullanabilirsiniz:

```json
"configurations": {
    "hive-site": {
        "hive.metastore.client.connect.retry.delay": "5",
        "hive.execution.engine": "mr",
        "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
    }
}
```

![Hadoop, küme önyükleme Azure Resource Manager şablonunu özelleştirir](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)

Spark2 ' deki yapılandırmayı değiştirmek için örnek Kaynak Yöneticisi Şablon parçacığı, olay günlüklerini depolama alanından düzenli olarak temizler.  

```json
"configurations": {
    "spark2-defaults": {
        "spark.history.fs.cleaner.enabled": "true",
        "spark.history.fs.cleaner.interval": "7d",
        "spark.history.fs.cleaner.maxAge": "90d"
    }
}
```

## <a name="see-also"></a>Ayrıca bkz.

* [HDInsight 'ta Apache Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md) diğer özel seçenekleri kullanarak HDInsight kümesi oluşturma hakkında yönergeler sağlar.
* [HDInsight için betik eylem betikleri geliştirme](hdinsight-hadoop-script-actions-linux.md)
* [HDInsight kümelerinde Apache Spark yükleyip kullanma](spark/apache-spark-jupyter-spark-sql-use-portal.md)
* [HDInsight kümelerine Apache Giraph 'Yi yükleyip kullanın](hdinsight-hadoop-giraph-install.md).

## <a name="appendix-powershell-sample"></a>Ek: PowerShell örneği

Bu PowerShell betiği bir HDInsight kümesi oluşturur ve bir Hive ayarını özelleştirir. `$nameToken`, `$httpPassword`Ve `$sshPassword`değerlerini girdiğinizden emin olun.

```powershell
####################################
# Set these variables
####################################
#region - used for creating Azure service names
$nameToken = "<ENTER AN ALIAS>"
#endregion

#region - cluster user accounts
$httpUserName = "admin"  #HDInsight cluster username
$httpPassword = '<ENTER A PASSWORD>'

$sshUserName = "sshuser" #HDInsight ssh user name
$sshPassword = '<ENTER A PASSWORD>'
#endregion

####################################
# Service names and varialbes
####################################
#region - service names
$namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

$resourceGroupName = $namePrefix + "rg"
$hdinsightClusterName = $namePrefix + "hdi"
$defaultStorageAccountName = $namePrefix + "store"
$defaultBlobContainerName = $hdinsightClusterName

$location = "East US"
#endregion


####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

#endregion

#region - Create an HDInsight cluster
####################################
# Create dependent components
####################################
Write-Host "Creating a resource group ..." -ForegroundColor Green
New-AzResourceGroup `
    -Name  $resourceGroupName `
    -Location $location

Write-Host "Creating the default storage account and default blob container ..."  -ForegroundColor Green
New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableHttpsTrafficOnly 1

# Note: Storage account kind BlobStorage cannot be used as primary storage.

$defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value

$defaultStorageContext = New-AzStorageContext `
                                -StorageAccountName $defaultStorageAccountName `
                                -StorageAccountKey $defaultStorageAccountKey

New-AzStorageContainer `
    -Name $defaultBlobContainerName `
    -Context $defaultStorageContext #use the cluster name as the container name

####################################
# Create a configuration object
####################################
$hiveConfigValues = @{"hive.metastore.client.socket.timeout"="90s"}

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValue `
        -HiveSite $hiveConfigValues `
        -Spark2Defaults @{}

####################################
# Create an HDInsight cluster
####################################
$httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

$sshPW = ConvertTo-SecureString -String $sshPassword -AsPlainText -Force
$sshCredential = New-Object System.Management.Automation.PSCredential($sshUserName,$sshPW)

New-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -Location $location `
    -ClusterSizeInNodes 1 `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -Config $config

####################################
# Verify the cluster
####################################
Get-AzHDInsightCluster `
    -ClusterName $hdinsightClusterName

#endregion
```
