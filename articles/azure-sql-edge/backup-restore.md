---
title: Veritabanlarını yedekleme ve geri yükleme-Azure SQL Edge (Önizleme)
description: Azure SQL Edge 'de yedekleme ve geri yükleme özellikleri hakkında bilgi edinin (Önizleme).
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 92a37babbcc0bbba3845267ca2eb0f95b9fceafa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84667871"
---
# <a name="back-up-and-restore-databases-in-azure-sql-edge-preview"></a>Azure SQL Edge 'de veritabanlarını yedekleme ve geri yükleme (Önizleme) 

Azure SQL Edge, Linux üzerinde Microsoft SQL Server veritabanı altyapısının en son sürümlerinde oluşturulmuştur. Linux üzerinde SQL Server ve kapsayıcılarda çalışan SQL Server, benzer yedekleme ve geri yükleme veritabanı işlevlerini sağlar. Yedekleme ve geri yükleme bileşeni, Azure SQL Edge veritabanlarınızda depolanan verileri korumak için önemli bir koruma sağlar. 

Veri kaybını tehlikeye atma riskini en aza indirmek için, verilerinizi düzenli aralıklarla yedeklemeniz gerekir. İyi planlanmış bir yedekleme ve geri yükleme stratejisi çeşitli sorunlardan kaynaklanan veri kaybına karşı veritabanlarını korumaya yardımcı olur. Bir yedekleme kümesini geri yükleyip veritabanınızı kurtararak, bir olağanüstü duruma etkin bir şekilde yanıt vermeye hazırlanmak için stratejinizi test edin.

Yedeklemelerin neden önemli olduğu hakkında daha fazla bilgi edinmek için [SQL Server veritabanlarının yedeklenmesi ve geri yüklenmesi](/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases/)konusuna bakın.

Azure SQL Edge, hem yerel depolama hem de Azure Bloblarından yedekleme ve geri yükleme yapmanızı sağlar. Daha fazla bilgi için bkz. [Azure Blob depolama ile yedekleme ve geri yükleme SQL Server](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service/) ve [URL 'ye yedekleme SQL Server](/sql/relational-databases/backup-restore/sql-server-backup-to-url).

## <a name="back-up-a-database-in-azure-sql-edge"></a>Azure SQL Edge 'de bir veritabanını yedekleme

Azure SQL Edge, SQL Server aynı Yedekleme türlerini destekler. Tüm liste için bkz. [yedeklemeye genel bakış](/sql/relational-databases/backup-restore/backup-overview-sql-server/).

> [!IMPORTANT] 
> Azure SQL Edge 'de oluşturulan veritabanları, varsayılan olarak basit kurtarma modelini kullanır. Bu nedenle, bu veritabanlarında günlük yedeklemeleri gerçekleştiremezsiniz. Bunu yapmanız gerekirse, veritabanı kurtarma modelini tam kurtarma modeliyle değiştirmek için bir yöneticiye ihtiyacınız vardır. SQL Server tarafından desteklenen kurtarma modellerinin tüm listesi için bkz. [kurtarma modeline genel bakış](/sql/relational-databases/backup-restore/recovery-models-sql-server#RMov).

### <a name="back-up-to-local-disk"></a>Yerel diske Yedekle

Aşağıdaki örnekte, `BACKUP DATABASE` kapsayıcıda bir veritabanı yedeklemesi oluşturmak için Transact-SQL komutunu kullanırsınız. Bu örneğin amacı doğrultusunda yedekleme dosyalarını depolamak için *yedekleme* adlı yeni bir klasör oluşturursunuz.

1. Yedeklemeler için bir klasör oluşturun. Azure SQL Edge kapsayıcının çalıştığı konakta bu komutu çalıştırın. Aşağıdaki komutta **<AzureSQLEdge_Container_Name>** , DAĞıTıMıNıZDAKI Azure SQL Edge kapsayıcısının adıyla değiştirin.

    ```bash
    sudo docker exec -it <AzureSQLEdge_Container_Name> mkdir /var/opt/mssql/backup
    ```

2. SQL Server Management Studio (SSMS) kullanarak veya Azure Data Studio kullanarak Azure SQL Edge örneğine bağlanın. `BACKUP DATABASE`Kullanıcı veritabanınızın yedeğini almak için komutunu çalıştırın. Aşağıdaki örnekte, *IronOreSilicaPrediction* veritabanının yedeğini alıyorsunuz demektir.

    ```sql
    BACKUP DATABASE [IronOreSilicaPrediction] 
    TO DISK = N'/var/opt/mssql/backup/IronOrePredictDB.bak' 
    WITH NOFORMAT, NOINIT,  NAME = N'IronOreSilicaPrediction-Full Database Backup', 
    SKIP, NOREWIND, NOUNLOAD, COMPRESSION,  STATS = 10
    GO
    ```

3. Komutunu çalıştırdıktan sonra, veritabanının yedeklemesi başarılı olursa SSMS veya Azure Data Studio sonuçları bölümünde aşağıdakine benzer iletiler görürsünüz.

    ```txt
    10 percent processed.
    20 percent processed.
    30 percent processed.
    40 percent processed.
    50 percent processed.
    60 percent processed.
    70 percent processed.
    80 percent processed.
    90 percent processed.
    100 percent processed.
    Processed 51648 pages for database 'IronOreSilicaPrediction', file 'IronOreSilicaPrediction' on file 1.
    Processed 2 pages for database 'IronOreSilicaPrediction', file 'IronOreSilicaPrediction_log' on file 1.
    BACKUP DATABASE successfully processed 51650 pages in 3.588 seconds (112.461 MB/sec).

    Completion time: 2020-04-09T23:54:48.4957691-07:00
    ```

### <a name="back-up-to-url"></a>URL 'ye yedekleme

Azure SQL Edge, sayfa Blobları ve blok bloblarına yönelik yedeklemeleri destekler. Daha fazla bilgi için bkz. [Blok Blobu vs Sayfa Blobu 'Na yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-ver15#blockbloborpageblob). Aşağıdaki örnekte veritabanı *IronOreSilicaPrediction* bir blok blobuna yedeklenir. 

1. Yedeklemeleri bloblara engelleyecek şekilde yapılandırmak için, önce Azure SQL Edge 'de SQL Server kimlik bilgileri oluşturmak üzere kullanabileceğiniz bir paylaşılan erişim imzası (SAS) oluşturun. Betik, depolanan bir erişim ilkesiyle ilişkili bir SAS oluşturur. Daha fazla bilgi için bkz. [paylaşılan erişim imzaları, 1. Bölüm: SAS modelini anlama](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/). Betik Ayrıca SQL Server kimlik bilgisini oluşturmak için gereken T-SQL komutunu da yazar. Aşağıdaki betik, bir depolama hesabı olan bir Azure aboneliğiniz olduğunu ve yedeklemeler için bir depolama kapsayıcısı olduğunu varsayar.

    ```PowerShell
    # Define global variables for the script  
    $subscriptionName='<your subscription name>'   # the name of subscription name you will use
    $resourcegroupName = '<your resource group name>' # the name of resource group you will use
    $storageAccountName= '<your storage account name>' # the storage account name you will use for backups
    $containerName= '<your storage container name>'  # the storage container name to which you will attach the SAS policy with its SAS token  
    $policyName = 'SASPolicy' # the name of the SAS policy  

    # adds an authenticated Azure account for use in the session
    Login-AzAccount

    # set the tenant, subscription and environment for use in the rest of
    Select-AzSubscription -Subscription $subscriptionName

    # Generate the SAS token
    $sa = Get-AzStorageAccount -ResourceGroupName $resourcegroupName -Name $storageAccountName 
    $storagekey = Get-AzStorageAccountKey -ResourceGroupName $resourcegroupName -Name $storageAccountName 
    $storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storagekey[0].Value
    $cbc = Get-AzStorageContainer -Name $containerName -Context $storageContext
    $policy = New-AzStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission "rwld"
    $sas = New-AzStorageContainerSASToken -Policy $policyName -Context $storageContext -Container $containerName
    Write-Host 'Shared Access Signature= '$($sas.Substring(1))''

    # Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
    Write-Host 'Credential T-SQL'  
    $tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.CloudBlobContainer.Uri.AbsoluteUri,$sas.Substring(1)
    $tSql | clip  
    Write-Host $tSql
    ```

    Betiği başarıyla çalıştırdıktan sonra, `CREATE CREDENTIAL` komutu bir sorgu aracına kopyalayın. Sonra bir SQL Server örneğine bağlanın ve, SAS ile kimlik bilgilerini oluşturmak için komutunu çalıştırın.

2. SSMS veya Azure Data Studio kullanarak Azure SQL Edge örneğine bağlanın ve önceki adımda komutunu kullanarak kimlik bilgilerini oluşturun. `CREATE CREDENTIAL`Komutu önceki adımdaki gerçek çıktıyla değiştirdiğinizden emin olun.

    ```sql
    IF NOT EXISTS  
    (SELECT * FROM sys.credentials
    WHERE name = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>')  
    CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>]
       WITH IDENTITY = 'SHARED ACCESS SIGNATURE',  
       SECRET = '<SAS_TOKEN>';
    ```

3. Aşağıdaki komut, Azure depolama kapsayıcısına *IronOreSilicaPrediction* 'in bir yedeğini alır.

    ```sql
    BACKUP DATABASE IronOreSilicaPrediction
    TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/IronOreSilicaPrediction.bak'
    With MAXTRANSFERSIZE = 4194304,BLOCKSIZE=65536;  
    GO
    ```

## <a name="restore-a-database-in-azure-sql-edge"></a>Azure SQL Edge 'de bir veritabanını geri yükleme

Azure SQL Edge 'de yerel bir diskten, ağ konumundan veya Azure Blob Storage hesabından geri yükleme yapabilirsiniz. SQL Server ' de geri yükleme ve kurtarma hakkında daha fazla bilgi için bkz. [geri yükleme ve kurtarmaya genel bakış](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-and-recovery-overview-sql-server?view=sql-server-ver15). SQL Server basit kurtarma modeline genel bir bakış için bkz. [veritabanını geri yükleme (basit kurtarma modeli)](https://docs.microsoft.com/sql/relational-databases/backup-restore/complete-database-restores-simple-recovery-model?view=sql-server-ver15).

### <a name="restore-from-a-local-disk"></a>Yerel diskten geri yükleme

Bu örnek, önceki örnekte yaptığınız *IronOreSilicaPrediction* yedeklemesini kullanır. Şimdi, bu dosyayı farklı bir adla yeni bir veritabanı olarak geri yükleyeceksiniz.

1. Veritabanı yedekleme dosyası kapsayıcıda zaten yoksa, dosyayı kapsayıcıya kopyalamak için aşağıdaki komutu kullanabilirsiniz. Aşağıdaki örnek, yedekleme dosyasının yerel konakta bulunduğunu ve/var/seçenek/MSSQL/Backup klasörüne *SQL1*adlı BIR Azure SQL Edge kapsayıcısına kopyalandığını varsayar.

    ```bash
    sudo docker cp IronOrePredictDB.bak sql1:/var/opt/mssql/backup
    ```

2. Restore komutunu çalıştırmak için SSMS veya Azure Data Studio kullanarak Azure SQL Edge örneğine bağlanın. Aşağıdaki örnekte, **IronOreSilicaPrediction_2**yeni bir veritabanı oluşturmak Için **ıronorepredictdb. bak** geri yüklendi.

    ```sql
    Restore FilelistOnly from disk = N'/var/opt/mssql/backup/IronOrePredictDB.bak'

    Restore Database IronOreSilicaPrediction_2
    From disk = N'/var/opt/mssql/backup/IronOrePredictDB.bak'
    WITH MOVE 'IronOreSilicaPrediction' TO '/var/opt/mssql/data/IronOreSilicaPrediction_Primary_2.mdf',
    MOVE 'IronOreSilicaPrediction_log' TO '/var/opt/mssql/data/IronOreSilicaPrediction_Primary_2.ldf'
    ```

3. Restore komutunu çalıştırdıktan sonra geri yükleme işlemi başarılı olduysa, çıkış penceresinde aşağıdakine benzer iletiler görürsünüz. 

    ```txt
    Processed 51648 pages for database 'IronOreSilicaPrediction_2', file 'IronOreSilicaPrediction' on file 1.
    Processed 2 pages for database 'IronOreSilicaPrediction_2', file 'IronOreSilicaPrediction_log' on file 1.
    RESTORE DATABASE successfully processed 51650 pages in 6.543 seconds (61.670 MB/sec).

    Completion time: 2020-04-13T23:49:21.1600986-07:00
    ```

### <a name="restore-from-url"></a>URL 'den geri yükle

Azure SQL Edge Ayrıca bir Azure Storage hesabından bir veritabanının geri yüklenmesini destekler. Blok Blobları veya Sayfa Blobu yedeklemelerinden geri yükleme yapabilirsiniz. Aşağıdaki örnekte, veritabanı oluşturmak için bir blok blobunun *IronOreSilicaPrediction_2020_04_16. bak* veritabanı yedekleme dosyası geri yüklenir, *IronOreSilicaPrediction_3*.

```sql
RESTORE DATABASE IronOreSilicaPrediction_3
FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/IronOreSilicaPrediction_2020_04_16.bak'
WITH MOVE 'IronOreSilicaPrediction' TO '/var/opt/mssql/data/IronOreSilicaPrediction_Primary_3.mdf', 
MOVE 'IronOreSilicaPrediction_log' TO '/var/opt/mssql/data/IronOreSilicaPrediction_Primary_3.ldf',
STATS = 10;
```


