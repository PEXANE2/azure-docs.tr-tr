---
title: PowerShell ve Azure HDInsight'ı kullanarak Apache Sqoop işlerini çalıştırın
description: Apache Hadoop kümesi ile Azure SQL Veritabanı arasında Apache Sqoop alma ve dışa aktarma yı çalıştırmak için bir iş istasyonundan Azure PowerShell'i nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/10/2020
ms.openlocfilehash: f39b595adf249b7412cb9b6b48f86b6fbd2c5e1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76263413"
---
# <a name="run-apache-sqoop-jobs-by-using-azure-powershell-for-apache-hadoop-in-hdinsight"></a>HDInsight'ta Apache Hadoop için Azure PowerShell'i kullanarak Apache Sqoop işlerini çalıştırın

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Bir HDInsight kümesi ile Azure SQL Veritabanı veya SQL Server veritabanı arasında veri almak ve dışa aktarmak için Azure HDInsight'taki Apache Sqoop işlerini çalıştırmak için Azure PowerShell'i nasıl kullanacağınızı öğrenin.  Bu makale, [HDInsight Hadoop ile Kullanım Apache Sqoop](./hdinsight-use-sqoop.md)bir devamıdır.

## <a name="prerequisites"></a>Ön koşullar

* Azure PowerShell AZ [Modülü](https://docs.microsoft.com/powershell/azure/overview) yüklü bir iş istasyonu.

* [HDInsight'ta Hadoop ile Apache Sqoop'u Kullanın'dan](./hdinsight-use-sqoop.md) [test ortamını ayarlama.](./hdinsight-use-sqoop.md#create-cluster-and-sql-database)

* Sqoop'a aşinalık. Daha fazla bilgi için [Sqoop Kullanım Kılavuzu'na](https://sqoop.apache.org/docs/1.4.7/SqoopUserGuide.html)bakın.

## <a name="sqoop-export"></a>Sqoop ihracat

Hive'dan SQL Server'a.

Bu örnek, Hive `hivesampletable` tablosundaki `mobiledata` verileri SQL Veritabanı'ndaki tabloya aktarabilir. Aşağıdaki değişkenlerin değerlerini ayarlayın ve ardından komutu çalıştırın.

```powershell
$hdinsightClusterName = ""
$httpPassword = ''
$sqlDatabasePassword = ''

# These values only need to be changed if the template was not followed.
$httpUserName = "admin"
$sqlServerLogin = "sqluser"
$sqlServerName = $hdinsightClusterName + "dbserver"
$sqlDatabaseName = $hdinsightClusterName + "db"

$pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

# Connection string
$connectionString = "jdbc:sqlserver://$sqlServerName.database.windows.net;user=$sqlServerLogin@$sqlServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"

# start export
New-AzHDInsightSqoopJobDefinition `
    -Command "export --connect $connectionString --table mobiledata --hcatalog-table hivesampletable" `
    | Start-AzHDInsightJob `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential
```

### <a name="alternative-execution"></a>Alternatif yürütme

1. Aşağıdaki kod aynı dışa aktarma gerçekleştirir; ancak, çıktı günlüklerini okumak için bir yol sağlar. Dışa aktarmayı başlatmak için kodu çalıştırın.

    ```powershell
    $sqoopCommand = "export --connect $connectionString --table mobiledata --hcatalog-table hivesampletable"
    
    $sqoopDef = New-AzHDInsightSqoopJobDefinition `
        -Command $sqoopCommand
    
    $sqoopJob = Start-AzHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef
    ```

1. Aşağıdaki kod çıkış günlüklerini görüntüler. Aşağıdaki kodu çalıştırın:

    ```powershell
    Get-AzHDInsightJobOutput `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Get-AzHDInsightJobOutput `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    ```

Hata iletisini alırsanız, `The specified blob does not exist.`birkaç dakika sonra yeniden deneyin.

## <a name="sqoop-import"></a>Sqoop ithalat

SQL Server'dan Azure Depolama'ya. Bu örnek, SQL `mobiledata` Veritabanı'ndaki tablodaki `wasb:///tutorials/usesqoop/importeddata` verileri HDInsight dizinine aktarabilir. Verilerdeki alanlar bir sekme karakteriyle ayrılır ve satırlar yeni bir satır karakteri tarafından sonlandırılır. Bu örnek, önceki örneği tamamladığınızı varsayar.

```powershell
$sqoopCommand = "import --connect $connectionString --table mobiledata --target-dir wasb:///tutorials/usesqoop/importeddata --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1"


$sqoopDef = New-AzHDInsightSqoopJobDefinition `
    -Command $sqoopCommand

$sqoopJob = Start-AzHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef

Get-AzHDInsightJobOutput `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardError

Get-AzHDInsightJobOutput `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardOutput

```

## <a name="additional-sqoop-export-example"></a>Ek Sqoop ihracat örneği

Bu, varsayılan depolama hesabından `/tutorials/usesqoop/data/sample.log` veri aktarAn ve sonra sql server `log4jlogs` veritabanında adı verilen bir tabloya aktaran sağlam bir örnektir. Bu örnek önceki örneklere bağlı değildir.

Aşağıdaki PowerShell komut dosyası kaynak dosyayı önceden işler ve ardından `log4jlogs`tabloya azure SQL Veritabanına aktarır. `CLUSTERNAME`Değiştirin `CLUSTERPASSWORD`, `SQLPASSWORD` ve ön koşuldan kullandığınız değerlerle.

```powershell
<#------ BEGIN USER INPUT ------#>
$hdinsightClusterName = "CLUSTERNAME"
$httpUserName = "admin"  #default is admin, update as needed
$httpPassword = 'CLUSTERPASSWORD'
$sqlDatabasePassword = 'SQLPASSWORD'
<#------- END USER INPUT -------#>

# Other fixed variable that should be used as is
$sqlServerName = $hdinsightClusterName + "dbserver"
$sqlDatabaseName = $hdinsightClusterName + "db"
$tableName_log4j = "log4jlogs"
$exportDir_log4j = "/tutorials/usesqoop/data"
$sourceBlobName = "example/data/sample.log"
$destBlobName = "tutorials/usesqoop/data/sample.log"
$sqljdbcdriver = "/user/oozie/share/lib/sqoop/mssql-jdbc-7.0.0.jre8.jar"

$cluster = Get-AzHDInsightCluster -ClusterName $hdinsightClusterName
$defaultStorageAccountName = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
$defaultStorageContainer = $cluster.DefaultStorageContainer
$resourceGroup = $cluster.ResourceGroup

$sqlServer = Get-AzSqlServer -ResourceGroupName $resourceGroup -ServerName $sqlServerName
$sqlServerLogin = $sqlServer.SqlAdministratorLogin
$sqlServerFQDN = $sqlServer.FullyQualifiedDomainName

#Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
try{Get-AzContext}
catch{Connect-AzAccount}

#pre-process the source file
Write-Host "`nPreprocessing the source file ..." -ForegroundColor Green

# This procedure creates a new file with $destBlobName
# Define the connection string
$defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroup `
                                -Name $defaultStorageAccountName)[0].Value

# Create block blob objects referencing the source and destination blob.
$storageAccount = Get-AzStorageAccount `
    -ResourceGroupName $resourceGroup `
    -Name $defaultStorageAccountName

$storageContainer = ($storageAccount |Get-AzStorageContainer -Name $defaultStorageContainer).CloudBlobContainer

$sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
$destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

# Define a MemoryStream and a StreamReader for reading from the source file
$stream = New-Object System.IO.MemoryStream
$stream = $sourceBlob.OpenRead()
$sReader = New-Object System.IO.StreamReader($stream)

# Define a MemoryStream and a StreamWriter for writing into the destination file
$memStream = New-Object System.IO.MemoryStream
$writeStream = New-Object System.IO.StreamWriter $memStream

# Pre-process the source blob
$exString = "java.lang.Exception:"
while(-Not $sReader.EndOfStream){
    $line = $sReader.ReadLine()
    $split = $line.Split(" ")

    # remove the "java.lang.Exception" from the first element of the array
    # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
    if ($split[0] -eq $exString){
        #create a new ArrayList to remove $split[0]
        $newArray = [System.Collections.ArrayList] $split
        $newArray.Remove($exString)

        # update $split and $line
        $split = $newArray
        $line = $newArray -join(" ")
    }

    # remove the lines that has less than 7 elements
    if ($split.count -ge 7){
        write-host $line
        $writeStream.WriteLine($line)
    }
}

# Write to the destination blob
$writeStream.Flush()
$memStream.Seek(0, "Begin")
$destBlob.UploadFromStream($memStream)

#export the log file from the cluster to the SQL database
Write-Host "Exporting the log file ..." -ForegroundColor Green

$pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

# Connection string
$connectionString = "jdbc:sqlserver://$sqlServerFQDN;user=$sqlServerLogin@$sqlServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"

# Submit a Sqoop job
$sqoopDef = New-AzHDInsightSqoopJobDefinition `
    -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1" `
    -Files $sqljdbcdriver

$sqoopJob = Start-AzHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef

Wait-AzHDInsightJob `
    -ResourceGroupName $resourceGroup `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzHDInsightJobOutput `
    -ResourceGroupName $resourceGroup `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultStorageContainer `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardError

Write-Host "Standard Output" -BackgroundColor Green
Get-AzHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultStorageContainer `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardOutput
```

## <a name="limitations"></a>Sınırlamalar

Linux tabanlı HDInsight aşağıdaki sınırlamaları sunar:

* Toplu dışa aktarma: Verileri Microsoft SQL Server veya Azure SQL Veritabanı'na aktarmak için kullanılan Sqoop bağlayıcısı şu anda toplu ekleri desteklemiyor.

* Toplu İşlem: Sqoop, kesici uçlar gerçekleştirirken `-batch` anahtarı kullanarak, kesici uç işlemlerini toplu hale getirme yerine birden çok kesici uç gerçekleştirir.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Sqoop'u kullanmayı öğrendin. Daha fazla bilgi için bkz:

* [HDInsight ile Apache Oozie kullanın:](../hdinsight-use-oozie-linux-mac.md)Bir Oozie iş akışında Sqoop eylem kullanın.
* [HDInsight'a veri yükleme](../hdinsight-upload-data.md): HDInsight veya Azure Blob depolamasına veri yüklemek için başka yöntemler bulun.
