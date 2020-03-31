---
title: "Powershell: SQL Server'ı SQL Veritabanına Geçirin"
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti ile Azure PowerShell'i kullanarak şirket içi SQL Server'dan Azure SQL Veritabanı'na geçiş yapmayı öğrenin.
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
ms.openlocfilehash: f63f79402b457017257f1762c6ddc7e04c0ee1af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650699"
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-database-using-azure-powershell"></a>SQL Server'ı Azure PowerShell kullanarak azure SQL Veritabanına şirket içinde geçirin

Bu makalede, Microsoft Azure PowerShell'i kullanarak SQL Server 2016 veya üzeri bir şirket içi örneğine yüklenen **Adventureworks2012** veritabanını Azure SQL Veritabanına geçirebilirsiniz. Microsoft Azure PowerShell'deki `Az.DataMigration` modülü kullanarak veritabanlarını şirket içi BIR SQL Server örneğinden Azure SQL Veritabanına geçirebilirsiniz.

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
>
> * Bir kaynak grubu oluşturun.
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
> * Azure Veritabanı Geçiş Hizmeti örneğinde bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.

## <a name="prerequisites"></a>Ön koşullar

Bu adımları tamamlamak için şunları yapmanız gerekir:

* [SQL Server 2016 ve üzeri](https://www.microsoft.com/sql-server/sql-server-downloads) (herhangi bir sürüm)
* SQL Server Express yüklemesi ile varsayılan olarak devre dışı bırakılan TCP/IP protokolünü etkinleştirmek için. [Sunucu Ağı Protokolünü Etkinleştir veya Devre Dışı Kılarak](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure)Makale'yi izleyerek TCP/IP protokolünü etkinleştirin.
* [Veritabanı altyapısına erişmek için Windows Güvenlik Duvarınızı yapılandırmak için.](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access)
* Bir Azure SQL Veritabanı örneği. [Azure portalında Bir Azure SQL veritabanı oluştur](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)makaledeki ayrıntıları izleyerek bir Azure SQL Veritabanı örneği oluşturabilirsiniz.
* [Veri Geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 veya daha sonra.
* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak Azure Veritabanı Geçiş Hizmeti'ni şirket içi kaynak sunucularınıza site-to-site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak bir Microsoft Azure Sanal Ağı oluşturmuş olmak.
* [SQL Server geçiş değerlendirmesi gerçekleştirme](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) makalesinde açıklandığı gibi Veri Geçişi Yardımcısı'nı kullanarak şirket içi veritabanınızın ve şema geçişinizin değerlendirmesini tamamlamış olmak
* Az.DataMigration modüllerini PowerShell Galerisi'nden [Install-Module PowerShell cmdlet](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1)kullanarak indirmek ve yüklemek için; Administrator olarak çalıştır'ı kullanarak powershell komut penceresini açtığından emin olun.
* Kaynak SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerinin [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) iznine sahip olduğundan emin olmak için.
* Hedef Azure SQL DB örneğine bağlanmak için kullanılan kimlik bilgilerinin hedef Azure SQL Veritabanı veritabanlarında CONTROL DATABASE iznine sahip olduğundan emin olmak için.
* Azure aboneliği. Hesabınız yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Microsoft Azure aboneliğinizde oturum açın

PowerShell'i kullanarak Azure aboneliğinizde oturum açmak için Azure [PowerShell ile oturum aç'ın](https://docs.microsoft.com/powershell/azure/authenticate-azureps) makaledeki yönergeleri kullanın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Sanal bir makine oluşturmadan önce bir kaynak grubu oluşturun.

[Yeni Kaynak Grubu](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) komutunu kullanarak bir kaynak grubu oluşturun.

Aşağıdaki örnek, *EastUS* bölgesinde *myResourceGroup* adında bir kaynak grubu oluşturur.

```powershell
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

## <a name="create-an-instance-of-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti örneği oluşturma

Cmdlet'i kullanarak Azure Veritabanı Geçiş `New-AzDataMigrationService` Hizmeti'nin yeni örneğini oluşturabilirsiniz. Bu cmdlet aşağıdaki gerekli parametreleri bekler:

* *Azure Kaynak Grubu adı.* Daha önce gösterildiği gibi Azure Kaynak grubu oluşturmak ve adını parametre olarak sağlamak için [Yeni Kaynak Grubu](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) komutunu kullanabilirsiniz.
* *Servis adı*. Azure Veritabanı Geçiş Hizmeti için istenen benzersiz hizmet adına karşılık gelen dize 
* *Konum*. Hizmetin konumunu belirtir. Batı ABD veya Güneydoğu Asya gibi bir Azure veri merkezi konumu belirtin
* *Sku*. Bu parametre DMS Sku adına karşılık gelir. Şu anda desteklenen Sku adı *GeneralPurpose_4vCores.*
* *Sanal Subnet Tanımlayıcısı*. Bir alt ağ oluşturmak için cmdlet [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) kullanabilirsiniz. 

Aşağıdaki örnek, *MyVNET* adlı sanal ağ ve *MySubnet*adlı alt ağ kullanarak *Doğu ABD* bölgesinde bulunan kaynak grubu *MyDMSGroup'ta MyDMS* adlı bir hizmet oluşturur. *MyDMS*

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

Cmdlet'i kullanarak veritabanı bağlantı `New-AzDmsConnInfo` bilgisi nesnesi oluşturabilirsiniz. Bu cmdlet aşağıdaki parametreleri bekler:

* *ServerType*. Örneğin, SQL, Oracle veya MySQL gibi istenen veritabanı bağlantısı türü. SQL Server ve Azure SQL için SQL'i kullanın.
* *Veri Kaynağı*. Bir SQL Server örneğinin veya Azure SQL veritabanının adı veya IP'si.
* *AuthType*. SqlAuthentication veya WindowsAuthentication olabilir bağlantı için kimlik doğrulama türü.
* *TrustServerCertificate* parametresi, güveni doğrulamak için sertifika zincirini yürürken kanalın şifrelenip şifrelenmediğini gösteren bir değer ayarlar. Değer doğru veya yanlış olabilir.

Aşağıdaki örnek, sql kimlik doğrulaması kullanarak MySourceSQLServer adlı kaynak SQL Server için Bağlantı Bilgisi nesnesi oluşturur:

```powershell
$sourceConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Sonraki örnek, SQLAzureTarget adlı bir Azure SQL veritabanı sunucusu için Bağlantı Bilgisi oluşturulmasını sql kimlik doğrulaması kullanarak gösterir:

```powershell
$targetConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource "sqlazuretarget.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Geçiş projesi için veritabanları sağlama

Projenin oluşturulması `AzDataMigrationDatabaseInfo` için parametre olarak sağlanabilecek Azure Veritabanı Geçişi projesinin bir parçası olarak veritabanlarını belirten nesnelerin bir listesini oluşturun. Cmdlet `New-AzDataMigrationDatabaseInfo` AzDataMigrationDatabaseInfo oluşturmak için kullanılabilir. 

Aşağıdaki örnek, `AzDataMigrationDatabaseInfo` **AdventureWorks2016** veritabanı için proje oluşturur ve proje oluşturma için parametre olarak sağlanacak listeye ekler.

```powershell
$dbInfo1 = New-AzDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks2016
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Proje nesnesi oluşturma

Son olarak, daha önce oluşturulmuş kaynak ve hedef bağlantıları `New-AzDataMigrationProject` ve geçirilen veritabanları listesini kullanarak ve ekleyerek *Doğu ABD'de* bulunan *MyDMSProject* adlı Azure Veritabanı Geçişi projesini oluşturabilirsiniz.

```powershell
$project = New-AzDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLDB `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfo $dbList
```

## <a name="create-and-start-a-migration-task"></a>Geçiş görevi oluşturma ve başlatma

Son olarak, Azure Veritabanı Geçişi görevini oluşturun ve başlatın. Azure Veritabanı Geçişi görevi, ön koşul olarak oluşturulan projede zaten sağlanan bilgilere ek olarak, hem kaynak hem de hedef ve veritabanı tablolarının listesiiçin bağlantı kimlik bilgisi nin geçirilmelerini gerektirir.

### <a name="create-credential-parameters-for-source-and-target"></a>Kaynak ve hedef için kimlik bilgisi parametreleri oluşturma

Bağlantı güvenlik kimlik bilgileri [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesne olarak oluşturulabilir.

Aşağıdaki örnek, string değişkenleri *$sourcePassword* ve *$targetPassword*olarak parola sağlayan hem kaynak hem de hedef bağlantılar için *PSCredential* nesnelerin oluşturulmasını gösterir.

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-table-map-and-select-source-and-target-parameters-for-migration"></a>Bir tablo haritası oluşturun ve geçiş için kaynak ve hedef parametreleri seçin

Geçiş için gereken bir diğer parametre de, tabloların kaynaktan hedefe geçirilecek şekilde eşlemeedilmesidir. Geçiş için kaynak ve hedef tablolar arasında eşleme sağlayan tablolar sözlüğü oluşturun. Aşağıdaki örnek, AdventureWorks 2016 veritabanı için kaynak ve hedef tablolar Arasındaki İnsan Kaynakları şemasını göstermektedir.

```powershell
$tableMap = New-Object 'system.collections.generic.dictionary[string,string]'
$tableMap.Add("HumanResources.Department", "HumanResources.Department")
$tableMap.Add("HumanResources.Employee","HumanResources.Employee")
$tableMap.Add("HumanResources.EmployeeDepartmentHistory","HumanResources.EmployeeDepartmentHistory")
$tableMap.Add("HumanResources.EmployeePayHistory","HumanResources.EmployeePayHistory")
$tableMap.Add("HumanResources.JobCandidate","HumanResources.JobCandidate")
$tableMap.Add("HumanResources.Shift","HumanResources.Shift")
```

Bir sonraki adım kaynak ve hedef veritabanları seçmek ve aşağıdaki örnekte gösterildiği gibi `New-AzDmsSelectedDB` cmdlet kullanarak bir parametre olarak geçiş için tablo eşleme sağlamaktır:

```powershell
$selectedDbs = New-AzDmsSelectedDB -MigrateSqlServerSqlDb -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -TableMap $tableMap
```

### <a name="create-the-migration-task-and-start-it"></a>Geçiş görevini oluşturun ve başlatın

Geçiş `New-AzDataMigrationTask` görevi oluşturmak ve başlatmak için cmdlet'i kullanın. Bu cmdlet aşağıdaki parametreleri bekler:

* *Görev Türü*. SQL Server için oluşturulacak geçiş görevi nin Sql SQL Veritabanı geçiş *türüne geçirinSqlSqlSqlDb* olması bekleniyor. 
* *Kaynak Grup Adı*. Görevi oluşturacak azure kaynak grubunun adı.
* *ServiceName*. Görevin oluşturulabilmek için Azure Veritabanı Geçiş Hizmeti örneği.
* *Proje Adı*. Görevoluşturmak için Azure Veritabanı Geçiş Hizmeti projesinin adı. 
* *Görev Adı*. Oluşturulacak görevin adı. 
* *Kaynak Bağlantı*. Kaynak SQL Server bağlantısını temsil eden AzDmsConnInfo nesnesi.
* *TargetConnection*. Hedef Azure SQL Veritabanı bağlantısını temsil eden AzDmsConnInfo nesnesi.
* *Kaynak Cred*. Kaynak sunucuya bağlanmak için [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesne.
* *TargetCred*. Hedef sunucuya bağlanmak için [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesne.
* *SelectedDatabase*. Kaynak ve hedef veritabanı eşlemi temsil eden AzDataMigrationSelectedDB nesne.
* *ŞemaDoğrulama*. (isteğe bağlı, anahtar parametresi) Geçişten sonra, kaynak ve hedef arasındaki şema bilgilerinin karşılaştırılmasını gerçekleştirir.
* *DataIntegrityValidation*. (isteğe bağlı, anahtar parametresi) Geçişten sonra, kaynak ve hedef arasında denetime dayalı veri bütünlüğü doğrulaması gerçekleştirir.
* *QueryAnalysisValidation*. (isteğe bağlı, anahtar parametresi) Geçişin ardından, sorguları kaynak veritabanından alarak hızlı ve akıllı bir sorgu çözümlemesi gerçekleştirir ve bunları hedefte gerçekleştirir.

Aşağıdaki örnek myDMSTask adlı bir geçiş görevi oluşturur ve başlatır:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDb `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
```

Aşağıdaki örnek, yukarıdaki gibi aynı geçiş görevi oluşturur ve başlatır, ancak aynı zamanda üç doğrulama gerçekleştirir:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDb `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -SchemaValidation `
  -DataIntegrityValidation `
  -QueryAnalysisValidation `
```

## <a name="monitor-the-migration"></a>Geçişi izleme

Aşağıdaki örnekte gösterildiği gibi görevin durum özelliğini sorgulayarak çalışan geçiş görevini izleyebilirsiniz:

```powershell
if (($mytask.ProjectTask.Properties.State -eq "Running") -or ($mytask.ProjectTask.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="deleting-the-dms-instance"></a>DMS örneğini silme

Geçiş tamamlandıktan sonra Azure DMS örneğini silebilirsiniz:

```powershell
Remove-AzDms -ResourceGroupName myResourceGroup -ServiceName MyDMS
```

## <a name="next-step"></a>Sonraki adım

* Microsoft [Veritabanı Geçiş Kılavuzu'ndaki](https://datamigration.microsoft.com/)geçiş kılavuzunu gözden geçirin.
