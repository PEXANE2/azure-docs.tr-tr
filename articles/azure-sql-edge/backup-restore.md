---
title: Veritabanlarını yedekleme ve geri yükleme-Azure SQL Edge (Önizleme)
description: Azure SQL Edge 'de yedekleme ve geri yükleme özellikleri hakkında bilgi edinin (Önizleme)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 5c7bdbc49d8f1c6af7f38911919c660b03e1a37a
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596956"
---
# <a name="backup-and-restore-databases-in-azure-sql-edge-preview"></a>Azure SQL Edge 'de veritabanlarını yedekleme ve geri yükleme (Önizleme) 

Azure SQL Edge, Linux üzerinde Microsoft SQL Server veritabanı altyapısının en son sürümlerinde yerleşik olarak bulunur ve Linux üzerinde SQL Server ve kapsayıcılarda çalışan SQL Server benzer yedekleme ve geri yükleme veritabanı özellikleri sağlar. Yedekleme ve geri yükleme bileşeni, Azure SQL Edge veritabanlarınızda depolanan verileri korumak için önemli bir koruma sağlar. Veri kaybını tehlikeye atma riskini en aza indirmek için, verilerinizi düzenli aralıklarla yedeklemeniz önerilir. İyi planlanmış bir yedekleme ve geri yükleme stratejisi çeşitli sorunlardan kaynaklanan veri kaybına karşı veritabanlarını korumaya yardımcı olur. Bir grup yedeklemeyi geri yükleyerek ve ardından olağanüstü durumlara etkili bir şekilde yanıt vermeye hazırlanmak için veritabanınızı kurtararak stratejinizi test edin.

Yedeklemelerin neden önemli olduğu hakkında daha fazla bilgi edinmek için [SQL Server veritabanlarının yedeklenmesi ve geri yüklenmesi](/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases/)konusuna bakın.

Azure SQL Edge, yerel depolamadan veya Azure Bloblarından yedekleme ve geri yükleme desteği sağlar. Azure Blob depolama 'dan yedekleme ve geri yükleme hakkında daha fazla bilgi için, [Microsoft Azure Blob depolama hizmeti ile SQL Server Yedekleme ve geri yükleme](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service/) hakkında daha fazla bilgi edınmek ve [URL 'ye yedekleme SQL Server](/sql/relational-databases/backup-restore/sql-server-backup-to-url)

## <a name="backing-up-a-database-in-azure-sql-edge"></a>Azure SQL Edge 'de bir veritabanını yedekleme

Azure SQL Edge, SQL Server tarafından desteklenen Yedekleme türlerini destekler. SQL Server desteklenen yedekleme türlerinin tamamı listesi için [yedeklemeye genel bakış](/sql/relational-databases/backup-restore/backup-overview-sql-server/)bölümüne bakın.

> [!IMPORTANT] 
> Azure SQL Edge 'de oluşturulan veritabanları varsayılan olarak basit kurtarma modeli kullanır. Bu veritabanlarında bu tür günlük yedeklemeleri gerçekleştirilemez. Bu veritabanlarında günlük yedeklemeleri gerçekleştirmeye gerek varsa, yöneticinin veritabanı kurtarma modelini tam kurtarma modeliyle değiştirmesi gerekir. SQL Server tarafından desteklenen kurtarma modellerinin tüm listesi için, [kurtarma modeline genel bakış](/sql/relational-databases/backup-restore/recovery-models-sql-server#RMov)bölümüne bakın.

### <a name="backup-to-local-disk"></a>Yerel diske yedekleme

Aşağıda belirtilen örnekte, kapsayıcıda bir veritabanı yedeklemesi oluşturmak için BACKUP DATABASE Transact-SQL komutu kullanılır. Bu örneğin amacı için, yedekleme dosyalarını depolamak üzere "Backup" adlı yeni bir klasör oluşturulur.

1. Yedeklemeler için bir klasör oluşturun. Bu komutun, Azure SQL Edge kapsayıcının çalıştığı konakta yürütülmesi gerekir. Aşağıdaki komutta **<AzureSQLEdge_Container_Name>** , DAĞıTıMıNıZDAKI Azure SQL Edge kapsayıcısı adıyla değiştirin.

    ```bash
    sudo docker exec -it <AzureSQLEdge_Container_Name> mkdir /var/opt/mssql/backup
    ```

2. SQL Server Management Studio (SSMS) kullanarak veya Azure Data Studio (ADS) kullanarak Azure SQL Edge örneğine bağlanın ve Kullanıcı veritabanınızın yedeğini almak için Backup Database komutunu çalıştırın. Aşağıdaki örnekte, *IronOreSilicaPrediction* veritabanının yedeklemesi gerçekleştik.

    ```sql
    BACKUP DATABASE [IronOreSilicaPrediction] 
    TO DISK = N'/var/opt/mssql/backup/IronOrePredictDB.bak' 
    WITH NOFORMAT, NOINIT,  NAME = N'IronOreSilicaPrediction-Full Database Backup', 
    SKIP, NOREWIND, NOUNLOAD, COMPRESSION,  STATS = 10
    GO
    ```

3. Komutunu çalıştırdıktan sonra ve veritabanının yedeklemesi başarılı olursa SSMS veya ADS 'nin sonuçlar bölümünde aşağıdakine benzer iletiler görürsünüz.

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

### <a name="backup-to-url"></a>URL 'ye yedekleme

Azure SQL Edge, sayfa Blobları ve blok bloblarına yönelik yedeklemeleri destekler. Sayfa Blobları ve blok Blobları hakkında daha fazla bilgi için bkz. [Blok Blobu vs sayfa Blobuna yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-ver15#blockbloborpageblob). Aşağıdaki örnekte Database *IronOreSilicaPrediction* bir blok blobuna yedeklenir. 

1. Yedeklemeleri engellemek için yedeklemelerin yapılandırılmasında ilk adım, Azure SQL Edge 'de SQL Server kimlik bilgileri oluşturmak için kullanılabilecek bir paylaşılan erişim imzası (SAS) oluşturmaktır. Betik, depolanan bir erişim Ilkesiyle ilişkili bir paylaşılan erişim Imzası oluşturur. Daha fazla bilgi için bkz. [paylaşılan erişim imzaları, 1. Bölüm: SAS modelini anlama](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/). Betik Ayrıca SQL Server kimlik bilgisini oluşturmak için gereken T-SQL komutunu da yazar. Aşağıdaki komut dosyası, bir depolama hesabı ve yedeklemeler için bir depolama kapsayıcısı olan bir Azure aboneliğiniz olduğunu varsayar.

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

    Betiği başarıyla çalıştırdıktan sonra, KIMLIK BILGISI Oluştur komutunu bir sorgu aracına kopyalayın, bir SQL Server örneğine bağlanın ve paylaşılan erişim Imzasıyla kimlik bilgisini oluşturmak için komutunu çalıştırın.

2. SQL Server Management Studio (SSMS) kullanarak veya Azure Data Studio (ADS) kullanarak Azure SQL Edge örneğine bağlanın ve önceki adımda komutunu kullanarak kimlik bilgilerini oluşturun. KIMLIK BILGISI Oluştur komutunu önceki adımdaki gerçek çıkış ile değiştirdiğinizden emin olun.

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

## <a name="restoring-a-database-in-azure-sql-edge"></a>Azure SQL Edge 'de bir veritabanını geri yükleme

Azure SQL Edge, bir yerel disk, ağ konumu veya bir Azure Blob depolama hesabından geri yüklemeyi destekler. SQL Server ' de geri yükleme ve kurtarmaya genel bakış için [geri yükleme ve kurtarmaya genel bakış](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-and-recovery-overview-sql-server?view=sql-server-ver15)bölümüne bakın. SQL Server basit kurtarma modeline genel bir bakış için, [tüm veritabanı geri yüklemeleri (basit kurtarma modeli)](https://docs.microsoft.com/sql/relational-databases/backup-restore/complete-database-restores-simple-recovery-model?view=sql-server-ver15)konusuna bakın.

### <a name="restore-from-local-disk"></a>Yerel diskten geri yükleme

Bu örnek, önceki örnekte gerçekleştirilen *IronOreSilicaPrediction* yedeklemesini, farklı bir ada sahip yeni bir veritabanı olarak geri yüklemek için kullanır.

1. Veritabanı yedekleme dosyası kapsayıcıda zaten yoksa, dosyayı kapsayıcıya kopyalamak için aşağıdaki komutu kullanabilirsiniz. Aşağıdaki örnekte, yedekleme dosyasının yerel konakta mevcut olduğu ve/var/seçenek/MSSQL/Backup klasörüne SQL1 adlı bir Azure SQL Edge kapsayıcısına kopyalandığı varsayılır.

    ```bash
    sudo docker cp IronOrePredictDB.bak sql1:/var/opt/mssql/backup
    ```

2. Restore komutunu yürütmek için SQL Server Management Studio (SSMS) veya Azure Data Studio (ADS) kullanarak Azure SQL Edge örneğine bağlanın. Aşağıdaki örnekte, **ıronorepredictdb. bak** yeni bir veritabanı oluşturmak için geri yüklendi **IronOreSilicaPrediction_2**

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

Azure SQL Edge Ayrıca bir Azure Storage hesabından bir veritabanının geri yüklenmesini destekler. Geri yüklemeler, blok Blobları veya Sayfa Blobu yedeklemelerinden gerçekleştirilebilir. Aşağıda belirtilen örnekte, veritabanı *IronOreSilicaPrediction_3*oluşturmak için bir blok blobunun *IronOreSilicaPrediction_2020_04_16. bak* veritabanı yedekleme dosyası geri yüklenir.

```sql
RESTORE DATABASE IronOreSilicaPrediction_3
FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/IronOreSilicaPrediction_2020_04_16.bak'
WITH MOVE 'IronOreSilicaPrediction' TO '/var/opt/mssql/data/IronOreSilicaPrediction_Primary_3.mdf', 
MOVE 'IronOreSilicaPrediction_log' TO '/var/opt/mssql/data/IronOreSilicaPrediction_Primary_3.ldf',
STATS = 10;
```


