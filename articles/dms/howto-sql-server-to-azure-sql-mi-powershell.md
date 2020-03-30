---
title: "PowerShell: SQL Server'ı SQL yönetilen örneğine geçirin"
titleSuffix: Azure Database Migration Service
description: Azure PowerShell ve Azure Veritabanı Geçiş Hizmeti'ni kullanarak şirket içi SQL Server'dan Azure SQL Veritabanı'na geçiş yapmayı öğrenin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 9ea9f55681b93e79eec836f5808d2c6feaa6bb29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650733"
---
# <a name="migrate-sql-server-to-sql-database-managed-instance-with-powershell--azure-database-migration-service"></a>PowerShell & Azure Veritabanı Geçiş Hizmeti ile SQL Server'ı SQL Veritabanı'na geçirin

Bu makalede, Sql Server 2005 veya üzeri bir şirket içi örneğine yüklenen **Adventureworks2016** veritabanını Microsoft Azure PowerShell kullanarak yönetilen bir Azure SQL Veritabanı'na geçirebilirsiniz. Microsoft Azure PowerShell'deki `Az.DataMigration` modülü kullanarak veritabanlarını şirket içi BIR SQL Server örneğinden Azure SQL Veritabanı yönetilen bir örneğe geçirebilirsiniz.

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
>
> * Bir kaynak grubu oluşturun.
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturun.
> * Azure Veritabanı Geçiş Hizmeti örneğinde bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makalede, hem çevrimiçi hem de çevrimdışı geçişlerin nasıl gerçekleştirilene kadar gerçekleştirilene ilişkin ayrıntılar yer almaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu adımları tamamlamak için şunları yapmanız gerekir:

* [SQL Server 2016 veya üzeri](https://www.microsoft.com/sql-server/sql-server-downloads) (herhangi bir sürüm).
* **AdventureWorks2016** veritabanının yerel bir kopyası, [buradan](https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017)indirebilirsiniz.
* SQL Server Express yüklemesi ile varsayılan olarak devre dışı bırakılan TCP/IP protokolünü etkinleştirmek için. [Sunucu Ağı Protokolünü Etkinleştir veya Devre Dışı Kılarak](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure)Makale'yi izleyerek TCP/IP protokolünü etkinleştirin.
* [Veritabanı altyapısına erişmek için Windows Güvenlik Duvarınızı yapılandırmak için.](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access)
* Azure aboneliği. Hesabınız yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
* Azure SQL Veritabanı yönetilen bir örnek. Bir Azure SQL Veritabanı yönetilen örnek oluştur makalede ayrıntıları izleyerek [bir Azure SQL Veritabanı yönetilen örnek](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started)oluşturabilirsiniz.
* Veri Geçiş [Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 veya daha sonra indirmek ve yüklemek için.
* Azure Veritabanı Geçiş Hizmeti'ne [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan Bir Microsoft Azure Sanal Ağı.
* SQL Server geçiş değerlendirmesi gerçekleştiren makalede açıklandığı gibi, Veri Geçişi Yardımcısı'nı kullanarak şirket içi veritabanınızın ve şema geçişinizin tamamlanmış [bir değerlendirmesi.](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem)
* `Az.DataMigration` [Install-Module PowerShell cmdlet](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1)kullanarak Modülü (sürüm 0.7.2 veya sonraki sürüm) PowerShell Galerisi'nden indirmek ve yüklemek için.
* Kaynak SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerinin [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) iznine sahip olduğundan emin olmak için.
* Hedeflenen Azure SQL Veritabanı yönetilen örneğine bağlanmak için kullanılan kimlik bilgilerinin hedef Azure SQL Veritabanı yönetilen örnek veritabanlarında CONTROL DATABASE iznine sahip olduğundan emin olmak için.

    > [!IMPORTANT]
    > Çevrimiçi geçişler için Azure Etkin Dizin kimlik bilgilerinizi zaten ayarlamış olmalısınız. Daha fazla bilgi için, [kaynaklara erişebilen bir Azure REKLAM uygulaması ve hizmet ilkesi oluşturmak için portalı kullan](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)makalesine bakın.

## <a name="sign-in-to-your-microsoft-azure-subscription"></a>Microsoft Azure aboneliğinizde oturum açın

PowerShell'i kullanarak Azure aboneliğinizde oturum açın. Daha fazla bilgi için [Azure PowerShell ile oturum aç](https://docs.microsoft.com/powershell/azure/authenticate-azureps)makalesini görün.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir kapsayıcıdır.

Komutu [`New-AzResourceGroup`](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) kullanarak bir kaynak grubu oluşturun.

Aşağıdaki örnek, *Doğu ABD* bölgesinde *myResourceGroup* adında bir kaynak grubu oluşturur.

```powershell
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

## <a name="create-an-instance-of-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti örneği oluşturma

Cmdlet'i kullanarak Azure Veritabanı Geçiş `New-AzDataMigrationService` Hizmeti'nin yeni örneğini oluşturabilirsiniz.
Bu cmdlet aşağıdaki gerekli parametreleri bekler:

* *Azure Kaynak Grubu adı.* Komutu, [`New-AzResourceGroup`](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) daha önce gösterildiği gibi bir Azure Kaynak grubu oluşturmak ve adını parametre olarak sağlamak için kullanabilirsiniz.
* *Servis adı*. Azure Veritabanı Geçiş Hizmeti için istenen benzersiz hizmet adına karşılık gelen dize.
* *Konum*. Hizmetin konumunu belirtir. Batı ABD veya Güneydoğu Asya gibi bir Azure veri merkezi konumu belirtin.
* *Sku*. Bu parametre DMS Sku adına karşılık gelir. Şu anda desteklenen Sku isimleri *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*.
* *Sanal Subnet Tanımlayıcısı*. Bir alt ağ oluşturmak [`New-AzVirtualNetworkSubnetConfig`](https://docs.microsoft.com//powershell/module/az.network/new-azvirtualnetworksubnetconfig) için cmdlet kullanabilirsiniz.

Aşağıdaki örnek, *MyVNET* adlı bir sanal ağ ve *MySubnet*adlı bir alt ağ kullanarak *Doğu ABD* bölgesinde bulunan kaynak grubu *MyDMS'de* *MyDMS* adlı bir hizmet oluşturur.

> [!IMPORTANT]
> Aşağıdaki kod snippet' i, Premium SKU'ya dayalı Azure Veritabanı Geçiş Hizmeti örneği gerektirmeyen çevrimdışı geçiş içindir. Çevrimiçi geçiş için -Sku parametresinin değeri premium SKU içermelidir.

```powershell
$vNet = Get-AzVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Bir Azure Veritabanı Geçiş Hizmeti örneği oluşturduktan sonra bir geçiş projesi oluşturun. Azure Veritabanı Geçiş Hizmeti projesi, hem kaynak hem de hedef örnekler için bağlantı bilgilerinin yanı sıra projenin bir parçası olarak geçirmek istediğiniz veritabanlarının listesini gerektirir.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Kaynak ve hedef bağlantılar için Veritabanı Bağlantı Bilgileri nesnesi oluşturma

Aşağıdaki parametreleri bekleyen cmdlet'i `New-AzDmsConnInfo` kullanarak bir Veritabanı Bağlantı Bilgisi nesnesi oluşturabilirsiniz:

* *ServerType*. Örneğin, SQL, Oracle veya MySQL gibi istenen veritabanı bağlantısı türü. SQL Server ve Azure SQL için SQL'i kullanın.
* *Veri Kaynağı*. Bir SQL Server örneğinin veya Azure SQL Veritabanı örneğinin adı veya IP'si.
* *AuthType*. SqlAuthentication veya WindowsAuthentication olabilir bağlantı için kimlik doğrulama türü.
* *TrustServerCertificate*. Bu parametre, güveni doğrulamak için sertifika zincirini yürürken kanalın şifrelenip şifrelenmediğini gösteren bir değer ayarlar. Değer olabilir `$true` veya `$false`.

Aşağıdaki örnek, sql kimlik doğrulaması kullanarak *MySourceSQLServer* adlı bir kaynak SQL Server için bir Bağlantı Bilgisi nesnesi oluşturur:

```powershell
$sourceConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Sonraki örnek, SQL kimlik doğrulaması kullanarak 'targetmanagedinstance.database.windows.net' adlı bir Azure SQL Veritabanı yönetilen örnek sunucusu için Bağlantı Bilgisi oluşturulmasını gösterir:

```powershell
$targetConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource "targetmanagedinstance.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Geçiş projesi için veritabanları sağlama

Projenin oluşturulması `AzDataMigrationDatabaseInfo` için parametre olarak sağlanabilecek Azure Veritabanı Geçiş Hizmeti projesinin bir parçası olarak veritabanlarını belirten nesnelerin bir listesini oluşturun. Oluşturmak için cmdlet `New-AzDataMigrationDatabaseInfo` `AzDataMigrationDatabaseInfo`kullanabilirsiniz.

Aşağıdaki örnek, `AzDataMigrationDatabaseInfo` **AdventureWorks2016** veritabanı için proje oluşturur ve proje oluşturma için parametre olarak sağlanacak listeye ekler.

```powershell
$dbInfo1 = New-AzDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Proje nesnesi oluşturma

Son olarak, Daha önce oluşturulmuş kaynak ve hedef bağlantıları ve geçiş `New-AzDataMigrationProject` için veritabanları listesini kullanarak *Doğu ABD'de* bulunan *MyDMSProject* adlı bir Azure Veritabanı Geçiş Hizmeti projesi oluşturabilirsiniz.

```powershell
$project = New-AzDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLMI `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfo $dbList
```

## <a name="create-and-start-a-migration-task"></a>Geçiş görevi oluşturma ve başlatma

Ardından, bir Azure Veritabanı Geçiş Hizmeti görevi oluşturun ve başlatın. Bu görev, hem kaynak hem de hedef için bağlantı kimlik bilgisinin yanı sıra geçirilecek veritabanı tablolarının listesini ve ön koşul olarak oluşturulan projeyle birlikte zaten sağlanan bilgileri gerektirir.

### <a name="create-credential-parameters-for-source-and-target"></a>Kaynak ve hedef için kimlik bilgisi parametreleri oluşturma

[PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesneolarak bağlantı güvenlik kimlik bilgileri oluşturun.

Aşağıdaki örnek, hem kaynak hem de hedef bağlantılar için *PSKdential* nesnelerin oluşturulmasını gösterir ve parolaları dize değişkenleri *olarak $sourcePassword* ve *$targetPassword*sağlar.

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-backup-fileshare-object"></a>Yedek FileShare nesnesi oluşturma

Artık, Azure Veritabanı Geçiş Hizmeti'nin cmdlet'i kullanarak kaynak veritabanı yedeklemelerini alabileceği yerel Kobİ ağ paylaşımını temsil eden bir FileShare nesnesi `New-AzDmsFileShare` oluşturun.

```powershell
$backupPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
$backupCred = New-Object System.Management.Automation.PSCredential ($backupUserName, $backupPassword)

$backupFileSharePath="\\10.0.0.76\SharedBackup"
$backupFileShare = New-AzDmsFileShare -Path $backupFileSharePath -Credential $backupCred
```

### <a name="create-selected-database-object"></a>Seçili veritabanı nesnesi oluşturma

Bir sonraki adım `New-AzDmsSelectedDB` cmdlet kullanarak kaynak ve hedef veritabanları seçmektir.

Aşağıdaki örnek, tek bir veritabanını SQL Server'dan Azure SQL Veritabanı yönetilen bir örneğe geçirmek içindir:

```powershell
$selectedDbs = @()
$selectedDbs += New-AzDmsSelectedDB -MigrateSqlServerSqlDbMi `
  -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -BackupFileShare $backupFileShare `
```

Tüm BIR SQL Server örneğinin Azure SQL Veritabanı yönetilen örneğine kaldırma ve kaydırma ihtiyacı varsa, kaynaktan tüm veritabanlarını almak için bir döngü aşağıda verilmiştir. Aşağıdaki örnekte, $Server, $SourceUserName ve $SourcePassword için kaynak SQL Server ayrıntılarınızı sağlayın.

```powershell
$Query = "(select name as Database_Name from master.sys.databases where Database_id>4)";
$Databases= (Invoke-Sqlcmd -ServerInstance "$Server" -Username $SourceUserName
-Password $SourcePassword -database master -Query $Query)
$selectedDbs=@()
foreach($DataBase in $Databases.Database_Name)
    {
      $SourceDB=$DataBase
      $TargetDB=$DataBase
      
$selectedDbs += New-AzureRmDmsSelectedDB -MigrateSqlServerSqlDbMi `
                                              -Name $SourceDB `
                                              -TargetDatabaseName $TargetDB `
                                              -BackupFileShare $backupFileShare
      }
```

### <a name="sas-uri-for-azure-storage-container"></a>Azure Depolama Konteyneri için SAS URI

Azure Veritabanı Geçiş Hizmeti'ne hizmetin yedekleme dosyalarını yüklediği depolama hesabı kapsayıcısına erişim sağlayan SAS URI içeren değişken oluşturun.

```powershell
$blobSasUri="https://mystorage.blob.core.windows.net/test?st=2018-07-13T18%3A10%3A33Z&se=2019-07-14T18%3A10%3A00Z&sp=rwdl&sv=2018-03-28&sr=c&sig=qKlSA512EVtest3xYjvUg139tYSDrasbftY%3D"
```

### <a name="additional-configuration-requirements"></a>Ek yapılandırma gereksinimleri

Ele almanız gereken birkaç ek gereksinim vardır, ancak bunlar çevrimdışı veya çevrimiçi geçiş gerçekleştirip gerçekleştirmediğinize bağlı olarak farklılık gösterir.

#### <a name="offline-migrations"></a>Çevrimdışı geçişler

Yalnızca çevrimdışı geçişler için aşağıdaki ek yapılandırma görevlerini gerçekleştirin.

* **Oturum açma ları seçin.** Aşağıdaki örnekte gösterildiği gibi geçirilecek giriş listesi oluşturun:

    ```powershell
    $selectedLogins = @("user1", "user2")
    ```

    > [!IMPORTANT]
    > Şu anda, Azure Veritabanı Geçiş Hizmeti yalnızca geçiş sql oturumlarını destekler.

* **Aracı işleri seçin.** Aşağıdaki örnekte gösterildiği gibi geçirilecek aracı işlerinin listesini oluşturun:

    ```powershell
    $selectedAgentJobs = @("agentJob1", "agentJob2")
    ```

    > [!IMPORTANT]
    > Şu anda Azure Veritabanı Geçiş Hizmeti yalnızca T-SQL alt sistem iş adımlarını içeren işleri destekler.

#### <a name="online-migrations"></a>Çevrimiçi geçişler

Yalnızca çevrimiçi geçişler için aşağıdaki ek yapılandırma görevlerini gerçekleştirin.

* **Azure Active Directory Uygulamasını Yapılandırma**

    ```powershell
    $pwd = "Azure App Key"
    $appId = "Azure App Id"
    $AppPasswd = ConvertTo-SecureString $pwd -AsPlainText -Force
    $app = New-AzDmsAdApp -ApplicationId $appId -AppKey $AppPasswd
    ```

* **Depolama Kaynağını Yapılandırma**

    ```powershell
    $storageResourceId = "Storage Resource Id"
    ```

### <a name="create-and-start-the-migration-task"></a>Geçiş görevini oluşturma ve başlatma

Geçiş `New-AzDataMigrationTask` görevi oluşturmak ve başlatmak için cmdlet'i kullanın.

#### <a name="specify-parameters"></a>Parametreleri belirtin

##### <a name="common-parameters"></a>Ortak Parametreler

Çevrimdışı veya çevrimiçi geçiş gerçekleştirip gerçekleştirmediğinize bakılmaksızın, `New-AzDataMigrationTask` cmdlet aşağıdaki parametreleri bekler:

* *Görev Türü*. SQL Server için oluşturulacak geçiş görevi nin türünden Sql SQL Veritabanı Yönetilen Örnek geçiş türü *nezimmeteSqlSqlSqlDbMi* bekleniyor. 
* *Kaynak Grup Adı*. Görevi oluşturacak azure kaynak grubunun adı.
* *ServiceName*. Görevin oluşturulabilmek için Azure Veritabanı Geçiş Hizmeti örneği.
* *Proje Adı*. Görevoluşturmak için Azure Veritabanı Geçiş Hizmeti projesinin adı. 
* *Görev Adı*. Oluşturulacak görevin adı. 
* *Kaynak Bağlantı*. Kaynak SQL Server bağlantısını temsil eden AzDmsConnInfo nesnesi.
* *TargetConnection*. Hedef Azure SQL Veritabanı Yönetilen Örnek bağlantısını temsil eden AzDmsConnInfo nesnesi.
* *Kaynak Cred*. Kaynak sunucuya bağlanmak için [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesne.
* *TargetCred*. Hedef sunucuya bağlanmak için [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesne.
* *SelectedDatabase*. Kaynak ve hedef veritabanı eşlemi temsil eden AzDataMigrationSelectedDB nesne.
* *BackupFileShare*. Azure Veritabanı Geçiş Hizmeti'nin kaynak veritabanı yedeklemelerini götürebileceği yerel ağ paylaşımını temsil eden FileShare nesnesi.
* *Yedek BlobSasUri*. Azure Veritabanı Geçiş Hizmeti'ne, hizmetin yedekleme dosyalarını yüklediği depolama hesabı kapsayıcısına erişim sağlayan SAS URI. Blob kapsayıcısı için SAS URI değerini almayı öğrenin.
* *SelectedLogins*. Geçirilecek seçili girişlerin listesi.
* *SelectedAgentİşler*. Geçirilecek seçili aracı işlerinin listesi.

##### <a name="additional-parameters"></a>Ek parametreler

Cmdlet `New-AzDataMigrationTask` ayrıca, gerçekleştirdiğiniz geçiş türüne, çevrimdışı veya çevrimiçi parametreleri de bekler.

* **Çevrimdışı geçişler.** Çevrimdışı geçişler için `New-AzDataMigrationTask` cmdlet de aşağıdaki parametreleri bekler:

  * *SelectedLogins*. Geçirilecek seçili girişlerin listesi.
  * *SelectedAgentİşler*. Geçirilecek seçili aracı işlerinin listesi.

* **Çevrimiçi geçişler**. Çevrimiçi geçişler için `New-AzDataMigrationTask` cmdlet de aşağıdaki parametreleri bekler.

* *AzureActiveDirectoryApp*. Aktif Dizin Uygulaması.
* *DepolamaKaynak KIMLIĞI*. Depolama Hesabı kaynak kimliği.

#### <a name="create-and-start-an-offline-migration-task"></a>Çevrimdışı geçiş görevi oluşturma ve başlatma

Aşağıdaki örnek **myDMSTask**adlı çevrimdışı bir geçiş görevi oluşturur ve başlatır:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDbMi `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -BackupFileShare $backupFileShare `
  -BackupBlobSasUri $blobSasUri `
  -SelectedLogins $selectedLogins `
  -SelectedAgentJobs $selectedJobs `
```

#### <a name="create-and-start-an-online-migration-task"></a>Çevrimiçi geçiş görevi oluşturma ve başlatma

Aşağıdaki örnek **myDMSTask**adlı bir çevrimiçi geçiş görevi oluşturur ve başlatır:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDbMiSync `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -BackupFileShare $backupFileShare `
  -SelectedDatabase $selectedDbs `
  -AzureActiveDirectoryApp $app `
  -StorageResourceId $storageResourceId
```

## <a name="monitor-the-migration"></a>Geçişi izleme

Geçişi izlemek için aşağıdaki görevleri gerçekleştirin.

1. Tüm geçiş ayrıntılarını $CheckTask adlı bir değişkende birleştirin.

    Geçişle ilişkili özellikler, durum ve veritabanı bilgileri gibi geçiş ayrıntılarını birleştirmek için aşağıdaki kod parçacıklarını kullanın:

    ```powershell
    $CheckTask= Get-AzDataMigrationTask     -ResourceGroupName myResourceGroup `
                                            -ServiceName $service.Name `
                                        -ProjectName $project.Name `
                                            -Name myDMSTask `
                                            -ResultType DatabaseLevelOutput `
                        -Expand 
    Write-Host ‘$CheckTask.ProjectTask.Properties.Output’
    ```

2. Geçiş `$CheckTask` görevinin geçerli durumunu almak için değişkeni kullanın.

    Geçiş görevinin geçerli durumunu almak için `$CheckTask` değişkeni kullanmak için, aşağıdaki örnekte gösterildiği gibi görevin durum özelliğini sorgulayarak çalışan geçiş görevini izleyebilirsiniz:

    ```powershell
    if (($CheckTask.ProjectTask.Properties.State -eq "Running") -or ($CheckTask.ProjectTask.Properties.State -eq "Queued"))
    {
      Write-Host "migration task running"
    }
    else if($CheckTask.ProjectTask.Properties.State -eq "Succeeded")
    { 
      Write-Host "Migration task is completed Successfully"
    }
    else if($CheckTask.ProjectTask.Properties.State -eq "Failed" -or $CheckTask.ProjectTask.Properties.State -eq "FailedInputValidation" -or $CheckTask.ProjectTask.Properties.State -eq "Faulted")
    { 
      Write-Host "Migration Task Failed"
    }
    ```

## <a name="performing-the-cutover-online-migrations-only"></a>Kesme gerçekleştirme (yalnızca çevrimiçi geçişler)

Çevrimiçi geçişle, tam bir yedekleme ve veritabanlarıgeri yükleme gerçekleştirilir ve ardından BackupFileShare'de depolanan İşlem Günlüklerini geri yükleme çalışmaları devam eder.

Azure SQL Veritabanı yönetilen bir örnekteki veritabanı en son verilerle güncelleştirildiğinde ve kaynak veritabanıyla eşitlendiğinde, bir kesme gerçekleştirebilirsiniz.

Aşağıdaki örnek, kesme\geçiştamamlayacaktır. Kullanıcılar kendi takdirine bağlı olarak bu komutu kullanırlar.

```powershell
$command = Invoke-AzDmsCommand -CommandType CompleteSqlMiSync `
                               -ResourceGroupName myResourceGroup `
                               -ServiceName $service.Name `
                               -ProjectName $project.Name `
                               -TaskName myDMSTask `
                               -DatabaseName "Source DB Name"
```

## <a name="deleting-the-instance-of-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti örneğini silme

Geçiş tamamlandıktan sonra Azure Veritabanı Geçiş Hizmeti örneğini silebilirsiniz:

```powershell
Remove-AzDms -ResourceGroupName myResourceGroup -ServiceName MyDMS
```

## <a name="additional-resources"></a>Ek kaynaklar

Ek geçiş senaryoları (kaynak/hedef çiftleri) hakkında bilgi için Microsoft [Veritabanı Geçiş Kılavuzu'na](https://datamigration.microsoft.com/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Makalede Azure Veritabanı Geçiş Hizmeti hakkında daha fazla bilgi [edinin.](https://docs.microsoft.com/azure/dms/dms-overview)
