---
title: 'PowerShell: SQL Server SQL veritabanına geçirme'
titleSuffix: Azure Database Migration Service
description: Azure veritabanı geçiş hizmeti ile Azure PowerShell kullanarak şirket içi SQL Server Azure SQL veritabanı 'na geçirmeyi öğrenin.
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
ms.openlocfilehash: 323d6b006d3e790a7517e90f34241976904b3ba3
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84021492"
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-database-using-azure-powershell"></a>Azure PowerShell kullanarak şirket içi SQL Server Azure SQL veritabanı 'na geçirme

Bu makalede, Microsoft Azure PowerShell kullanarak bir Azure SQL veritabanına SQL Server 2016 veya üzeri bir şirket içi örneğine geri yüklenen **Adventureworks2012** veritabanını geçirolursunuz. Microsoft Azure PowerShell modülünü kullanarak şirket içi SQL Server örneğinden veritabanlarını Azure SQL veritabanı 'na geçirebilirsiniz `Az.DataMigration` .

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
>
> * Bir kaynak grubu oluşturun.
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturma.
> * Azure veritabanı geçiş hizmeti örneğinde bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.

## <a name="prerequisites"></a>Ön koşullar

Bu adımları tamamlayabilmeniz için şunlar gerekir:

* [SQL Server 2016 veya üzeri](https://www.microsoft.com/sql-server/sql-server-downloads) (herhangi bir sürüm)
* SQL Server Express yükleme ile varsayılan olarak devre dışı bırakılan TCP/IP protokolünü etkinleştirmek için. [Sunucu ağ protokolünü etkinleştirme veya devre dışı bırakma](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure)MAKALESINI izleyerek TCP/IP protokolünü etkinleştirin.
* [Windows Güvenlik duvarınızı veritabanı altyapısı erişimi için](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access)yapılandırmak için.
* Bir Azure SQL Veritabanı örneği. [Azure Portal Azure SQL veritabanı oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)makalesindeki ayrıntıyı Izleyerek BIR Azure SQL veritabanı örneği oluşturabilirsiniz.
* [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) v 3.3 veya üzeri.
* Azure veritabanı geçiş hizmeti 'ni, [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınız için siteden siteye bağlantı ile sağlayan Azure Resource Manager dağıtım modelini kullanarak Microsoft Azure sanal ağ oluşturma.
* [SQL Server geçiş değerlendirmesi gerçekleştirme](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) makalesinde açıklandığı gibi Data Migration Yardımcısı kullanarak şirket içi veritabanınızın ve şema geçişinizin değerlendirmesini tamamlamış olması için
* [Install-Module PowerShell cmdlet 'ini](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1)kullanarak PowerShell Galerisi az. datamigration modülünü indirmek ve yüklemek için Yönetici olarak çalıştır 'ı kullanarak PowerShell komut penceresini açmayı unutmayın.
* Kaynak SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerinin [DENETIM sunucusu](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) iznine sahip olduğundan emin olmak için.
* Hedef Azure SQL DB örneğine bağlanmak için kullanılan kimlik bilgilerinin hedef Azure SQL veritabanı veritabanlarında DENETIM VERITABANı iznine sahip olduğundan emin olmak için.
* Azure aboneliği. Bir tane yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Microsoft Azure aboneliğinizde oturum açın

PowerShell kullanarak Azure aboneliğinizde oturum açmak için [Azure PowerShell Ile oturum](https://docs.microsoft.com/powershell/azure/authenticate-azureps) açma makalesindeki yönergeleri kullanın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makine oluşturabilmeniz için önce bir kaynak grubu oluşturun.

[New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) komutunu kullanarak bir kaynak grubu oluşturun.

Aşağıdaki örnek *EastUS* bölgesinde *myresourcegroup* adlı bir kaynak grubu oluşturur.

```powershell
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

## <a name="create-an-instance-of-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti örneği oluşturma

Cmdlet 'ini kullanarak Azure veritabanı geçiş hizmeti 'nin yeni bir örneğini oluşturabilirsiniz `New-AzDataMigrationService` . Bu cmdlet aşağıdaki gerekli parametreleri bekliyor:

* *Azure Kaynak grubu adı*. [Yeni-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) komutunu, daha önce gösterildiği gibi Azure Kaynak grubu oluşturmak ve adını bir parametre olarak sağlamak için kullanabilirsiniz.
* *Hizmet adı*. Azure veritabanı geçiş hizmeti için istenen benzersiz hizmet adına karşılık gelen dize 
* *Konum*. Hizmetin konumunu belirtir. Batı ABD veya Güneydoğu Asya gibi bir Azure veri merkezi konumu belirtin
* *SKU 'su*. Bu parametre, DMS SKU adına karşılık gelir. Şu anda desteklenen SKU adı *GeneralPurpose_4vCores*.
* *Sanal alt ağ tanımlayıcısı*. Bir alt ağ oluşturmak için [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) cmdlet 'ini kullanabilirsiniz. 

Aşağıdaki örnek, *Myvnet* adlı ve *mysubnet*adlı alt ağ adlı bir sanal ağ kullanarak *Doğu ABD* bölgesinde bulunan *Mydmsresourcegroup* kaynak grubunda *mydms* adlı bir hizmet oluşturur.

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

Azure veritabanı geçiş hizmeti örneği oluşturduktan sonra bir geçiş projesi oluşturun. Azure veritabanı geçiş hizmeti projesi hem kaynak hem de hedef örneklerin yanı sıra projenin bir parçası olarak geçirmek istediğiniz veritabanlarının listesini gerektirir.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Kaynak ve hedef bağlantıları için bir veritabanı bağlantı bilgileri nesnesi oluşturma

Cmdlet 'ini kullanarak bir veritabanı bağlantı bilgisi nesnesi oluşturabilirsiniz `New-AzDmsConnInfo` . Bu cmdlet şu parametreleri bekliyor:

* *SunucuTürü*. İstenen veritabanı bağlantısı türü (örneğin, SQL, Oracle veya MySQL). SQL Server ve Azure SQL için SQL kullanın.
* *Veri kaynağı*. Bir SQL Server örneğinin veya Azure SQL veritabanının adı veya IP 'si.
* *AuthType*. SqlAuthentication veya WindowsAuthentication olabilen bağlantı için kimlik doğrulaması türü.
* *TrustServerCertificate* parametresi, güveni doğrulamak üzere sertifika zincirini atlayarak kanalın şifrelenip şifrelenmeyeceğini belirten bir değer ayarlar. Değer true veya false olabilir.

Aşağıdaki örnek, SQL kimlik doğrulaması kullanarak Mysourcessqlserver adlı kaynak SQL Server için bağlantı bilgileri nesnesi oluşturur:

```powershell
$sourceConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Sonraki örnekte, SQL kimlik doğrulaması kullanarak SQLAzureTarget adlı bir sunucu için bağlantı bilgilerinin oluşturulması gösterilmektedir:

```powershell
$targetConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource "sqlazuretarget.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Geçiş projesi için veritabanları sağlama

`AzDataMigrationDatabaseInfo`Projeyi oluşturmak için parametre olarak sağlanmasında, Azure veritabanı geçiş projesinin bir parçası olarak veritabanlarını belirten nesnelerin bir listesini oluşturun. Cmdlet 'ı `New-AzDataMigrationDatabaseInfo` Azdatamigrationdatabaseınfo oluşturmak için kullanılabilir. 

Aşağıdaki örnek, `AzDataMigrationDatabaseInfo` **AdventureWorks2016** veritabanı için proje oluşturur ve proje oluşturmak için parametre olarak sağlanacak listeye ekler.

```powershell
$dbInfo1 = New-AzDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks2016
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Proje nesnesi oluştur

Son olarak, kullanarak *Doğu ABD* bulunan *Mydmsproject* adlı Azure veritabanı geçiş projesi oluşturabilir `New-AzDataMigrationProject` ve önceden oluşturulan kaynak ve hedef bağlantıları ve geçirilecek veritabanlarının listesini ekleyebilirsiniz.

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

Son olarak, Azure veritabanı geçiş görevi oluşturun ve başlatın. Azure veritabanı geçiş görevi, önkoşul olarak oluşturulan projede zaten sağlanmış olan bilgilere ek olarak geçirilecek kaynak ve hedef ve veritabanı tablolarının listesi için bağlantı kimlik bilgileri gerektirir.

### <a name="create-credential-parameters-for-source-and-target"></a>Kaynak ve hedef için kimlik bilgisi parametreleri oluşturma

Bağlantı güvenliği kimlik bilgileri, [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesnesi olarak oluşturulabilir.

Aşağıdaki örnek, *$sourcePassword* ve *$targetPassword*dize değişkenleri olarak parola sağlayan hem kaynak hem de hedef bağlantı için *PSCredential* nesnelerinin oluşturulmasını gösterir.

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-table-map-and-select-source-and-target-parameters-for-migration"></a>Geçiş için tablo haritası oluşturun ve kaynak ve hedef parametrelerini seçin

Geçiş için gereken başka bir parametre, kaynaktan hedefe geçirilecek tabloların eşleşmesini sağlar. Geçiş için kaynak ve hedef tablolar arasında eşleme sağlayan tablo sözlüğü oluşturun. Aşağıdaki örnekte, AdventureWorks 2016 veritabanı için kaynak ve hedef tabloları Insan kaynakları şeması arasındaki eşleme gösterilmektedir.

```powershell
$tableMap = New-Object 'system.collections.generic.dictionary[string,string]'
$tableMap.Add("HumanResources.Department", "HumanResources.Department")
$tableMap.Add("HumanResources.Employee","HumanResources.Employee")
$tableMap.Add("HumanResources.EmployeeDepartmentHistory","HumanResources.EmployeeDepartmentHistory")
$tableMap.Add("HumanResources.EmployeePayHistory","HumanResources.EmployeePayHistory")
$tableMap.Add("HumanResources.JobCandidate","HumanResources.JobCandidate")
$tableMap.Add("HumanResources.Shift","HumanResources.Shift")
```

Sonraki adım, kaynak ve hedef veritabanlarını seçmek ve `New-AzDmsSelectedDB` Aşağıdaki örnekte gösterildiği gibi cmdlet 'ini kullanarak bir parametre olarak geçirilecek tablo eşlemesi sağlamaktır:

```powershell
$selectedDbs = New-AzDmsSelectedDB -MigrateSqlServerSqlDb -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -TableMap $tableMap
```

### <a name="create-the-migration-task-and-start-it"></a>Geçiş görevini oluşturma ve başlatma

`New-AzDataMigrationTask`Bir geçiş görevi oluşturmak ve başlatmak için cmdlet 'ini kullanın. Bu cmdlet şu parametreleri bekliyor:

* *TaskType*. SQL Server Azure SQL veritabanı geçiş türü için oluşturulacak geçiş görevinin türü *Migratesqlserversqldb* bekleniyor. 
* *Kaynak grubu adı*. Görevin oluşturulacağı Azure Kaynak grubunun adı.
* *ServiceName*. Görevin oluşturulacağı Azure veritabanı geçiş hizmeti örneği.
* *ProjectName*. Görevin oluşturulacağı Azure veritabanı geçiş hizmeti projesinin adı. 
* *Görevadı*. Oluşturulacak görevin adı. 
* *SourceConnection*. Kaynak SQL Server bağlantısını temsil eden Azdmsconnınfo nesnesi.
* *TargetConnection*. Hedef Azure SQL veritabanı bağlantısını temsil eden Azdmsconnınfo nesnesi.
* *Sourcecred*. Kaynak sunucuya bağlanmak için [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesnesi.
* *Targetkimlik bilgileri*. Hedef sunucuya bağlanmak için [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesnesi.
* *Selecteddatabase*. Kaynak ve hedef veritabanı eşlemesini temsil eden AzDataMigrationSelectedDB nesnesi.
* *Schemavalidation*. (isteğe bağlı, anahtar parametresi) Geçişin ardından, kaynak ve hedef arasında şema bilgilerini karşılaştırma işlemini gerçekleştirir.
* *Dataintegrityvalidation*. (isteğe bağlı, anahtar parametresi) Geçişin ardından kaynak ve hedef arasında bir sağlama toplamı tabanlı veri bütünlüğü doğrulaması gerçekleştirir.
* *QueryAnalysisValidation*. (isteğe bağlı, anahtar parametresi) Geçişin ardından, kaynak veritabanından sorguları alarak ve bunları hedefte yürüterek hızlı ve akıllı bir sorgu çözümlemesi gerçekleştirir.

Aşağıdaki örnek, myDMSTask adlı bir geçiş görevi oluşturur ve başlatır:

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

Aşağıdaki örnek, yukarıdaki gibi aynı geçiş görevini oluşturur ve başlatır, ancak üç doğrulamayı de gerçekleştirir:

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

Aşağıdaki örnekte gösterildiği gibi görevin durum özelliğini sorgulayarak çalıştıran geçiş görevini izleyebilirsiniz:

```powershell
if (($mytask.ProjectTask.Properties.State -eq "Running") -or ($mytask.ProjectTask.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="deleting-the-dms-instance"></a>DMS örneğini silme

Geçiş işlemi tamamlandıktan sonra, Azure DMS örneğini silebilirsiniz:

```powershell
Remove-AzDms -ResourceGroupName myResourceGroup -ServiceName MyDMS
```

## <a name="next-step"></a>Sonraki adım

* Microsoft [veritabanı geçiş kılavuzu](https://datamigration.microsoft.com/)' nda geçiş kılavuzunu gözden geçirin.
