---
title: Azure SQL veritabanı 'nda veritabanı oluşturmak için BACPAC dosyasını içeri aktarma
description: Bir BACPAC dosyasından Azure SQL veritabanı veya Azure SQL yönetilen örneği 'nde yeni bir veritabanı oluşturun.
services: sql-database
ms.service: sql-db-mi
ms.subservice: migrate
ms.custom: sqldbrb=1, devx-track-azurecli, devx-track-azurepowershell
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 06/20/2019
ms.openlocfilehash: 574bc4721f83d60fdd8c75b4fedb824522968822
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89070055"
---
# <a name="quickstart-import-a-bacpac-file-to-a-database-in-azure-sql-database-or-azure-sql-managed-instance"></a>Hızlı başlangıç: BACPAC dosyasını Azure SQL veritabanı veya Azure SQL yönetilen örneği 'nde bir veritabanına aktarma
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Bir SQL Server veritabanını, [bacpac](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) dosyasını kullanarak Azure SQL VERITABANı veya SQL yönetilen örneği 'ne aktarabilirsiniz. Verileri Azure Blob depolamada (yalnızca standart depolama) veya şirket içi konumdaki yerel depolama alanında bulunan bir BACPAC dosyasından içeri aktarabilirsiniz. Daha fazla ve daha hızlı kaynaklar sağlayarak içeri aktarma hızını en üst düzeye çıkarmak için içeri aktarma işlemi sırasında veritabanınızı daha yüksek bir hizmet katmanına ve işlem boyutuna sahip olacak şekilde ölçeklendirin. İçeri aktarma işlemi başarılı olduktan sonra ölçeği azaltabilirsiniz.

> [!NOTE]
> İçeri aktarılan veritabanının uyumluluk düzeyi, kaynak veritabanının uyumluluk düzeyini temel alır.

> [!IMPORTANT]
> Veritabanınızı içeri aktardıktan sonra veritabanını geçerli uyumluluk düzeyinde (AdventureWorks2008R2 veritabanı için Level 100) veya daha yüksek bir düzeyde çalıştırmayı seçebilirsiniz. Veritabanını belirli bir uyumluluk düzeyinde çalıştırmanın etkileri ve buna yönelik seçenekler hakkında daha fazla bilgi için, bkz. [Veritabanı Uyumluluk Düzeyini Değiştirme](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Uyumluluk düzeyleriyle ilgili ek veritabanı düzeyi ayarları hakkında bilgi için, ayrıca bkz. [VERİTABANI KAPSAMLI YAPILANDIRMAYI DEĞİŞTİRME](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql).

## <a name="using-azure-portal"></a>Azure portalını kullanma

Azure portal BACPAC dosyasından içeri aktarmayı öğrenmek için bu videoyu izleyin veya aşağıdaki okumaya devam edin:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Its-just-SQL-Restoring-a-database-to-Azure-SQL-DB-from-backup/player?WT.mc_id=dataexposed-c9-niner]

[Azure Portal](https://portal.azure.com) *yalnızca* Azure SQL veritabanı 'Nda ve *yalnızca* Azure Blob depolamada depolanan bir bacpac dosyasından tek bir veritabanı oluşturulmasını destekler.

Bir veritabanını BACPAC dosyasından [Azure SQL yönetilen örneği](../managed-instance/sql-managed-instance-paas-overview.md) 'ne geçirmek için, Azure portal veya Azure PowerShell ' i kullanarak SQL Server Management Studio veya SqlPackage kullanın.

> [!NOTE]
> Azure portal veya PowerShell aracılığıyla gönderilen içeri/dışarı aktarma isteklerini işleyen makineler, BACPAC dosyasını ve veri katmanı uygulama çerçevesi (DacFX) tarafından oluşturulan geçici dosyaları da depolaması gerekir. Gerekli disk alanı, aynı boyuta sahip veritabanları arasında önemli ölçüde farklılık gösterir ve veritabanı boyutunun 3 katına kadar disk alanı gerektirebilir. İçeri/dışarı aktarma isteği çalıştıran makinelerde yalnızca 450GB yerel disk alanı vardır. Sonuç olarak, bazı istekler hata vererek başarısız olabilir `There is not enough space on the disk` . Bu durumda, geçici çözüm yeterli yerel disk alanına sahip bir makinede sqlpackage.exe çalıştırmak olur. Bu sorundan kaçınmak için, SQL paketini 150GB 'den büyük veritabanlarını içeri/dışarı aktarmak için kullanmayı öneririz.

1. Bir BACPAC dosyasından Azure portal kullanarak yeni bir tek veritabanına aktarmak için, uygun sunucu sayfasını açın ve ardından araç çubuğundan **veritabanını Içeri aktar**' ı seçin.  

   ![Veritabanı import1](./media/database-import/sql-server-import-database.png)

1. BACPAC dosyası için depolama hesabını ve kapsayıcıyı seçin ve ardından İçeri aktarılacak BACPAC dosyasını seçin.

1. Yeni veritabanı boyutunu (genellikle Origin ile aynı) belirtin ve hedef SQL Server kimlik bilgilerini sağlayın. Azure SQL veritabanı 'nda yeni bir veritabanının olası değerlerinin listesi için bkz. [veritabanı oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current).

   ![Veritabanı import2](./media/database-import/sql-server-import-database-settings.png)

1. **Tamam**’a tıklayın.

1. İçeri aktarmanın ilerlemesini izlemek için veritabanının sunucu sayfasını açın ve **Ayarlar**altında **Içeri/dışarı aktarma geçmişi**' ni seçin. Başarılı olduğunda, içeri aktarmanın **tamamlandı** durumu vardır.

   ![Veritabanı içeri aktarma durumu](./media/database-import/sql-server-import-database-history.png)

1. Veritabanının sunucuda canlı olduğunu doğrulamak için **SQL veritabanları** ' nı seçin ve yeni veritabanının **çevrimiçi**olduğunu doğrulayın.

## <a name="using-sqlpackage"></a>SqlPackage kullanma

[SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) komut satırı yardımcı programını kullanarak bir SQL Server veritabanını içeri aktarmak için, bkz. [içeri aktarma parametreleri ve özellikleri](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties). [Visual Studio için](https://msdn.microsoft.com/library/mt204009.aspx) [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve SQL Server veri araçları SqlPackage içerir. En son [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) 'i Microsoft İndirme Merkezi ' nden de indirebilirsiniz.

Ölçek ve performans için, Azure portal kullanmak yerine, çoğu üretim ortamında SqlPackage kullanılması önerilir. Dosya kullanarak geçiş hakkında bir SQL Server müşteri danışmanlık ekibi blogu için `BACPAC` , bkz. [bacpac dosyalarını kullanarak SQL Server Azure SQL veritabanı 'na geçirme](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Aşağıdaki SqlPackage komutu, **AdventureWorks2008R2** veritabanını yerel depolamadan **mynewserver20170403**adlı bir mantıksal SQL Server 'a aktarır. **Premium** hizmet katmanı ve **P6** hizmeti hedefi ile **myMigratedDatabase** adlı yeni bir veritabanı oluşturur. Bu değerleri ortamınıza uygun şekilde değiştirin.

```cmd
sqlpackage.exe /a:import /tcs:"Data Source=<serverName>.database.windows.net;Initial Catalog=<migratedDatabase>;User Id=<userId>;Password=<password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Şirket güvenlik duvarının arkasındaki Azure SQL veritabanına bağlanmak için, güvenlik duvarında 1433 numaralı bağlantı noktası açık olmalıdır. SQL yönetilen örneğine bağlanmak için, [Noktadan siteye bağlantı](../managed-instance/point-to-site-p2s-configure.md) veya Express Route bağlantısına sahip olmanız gerekir.

Bu örnek, Active Directory evrensel kimlik doğrulamasıyla SqlPackage kullanılarak bir veritabanının nasıl içeri aktarılacağını gösterir.

```cmd
sqlpackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="using-powershell"></a>PowerShell’i kullanma

> [!NOTE]
> [SQL yönetilen örneği](../managed-instance/sql-managed-instance-paas-overview.md) şu anda Azure PowerShell kullanarak bir veritabanını bir bacpac dosyasından örnek veritabanına geçirmeyi desteklemez. SQL yönetilen örneği içine aktarmak için SQL Server Management Studio veya SQLPackage kullanın.

> [!NOTE]
> Portal veya PowerShell aracılığıyla gönderilen içeri/dışarı aktarma isteklerini işleyen makinelerin bacpac dosyasını ve veri katmanı uygulaması çerçevesi (DacFX) tarafından oluşturulan geçici dosyaları depolaması gerekir. Gerekli disk alanı, aynı boyuta sahip olan veritabanları arasında önemli ölçüde farklılık gösterir ve veritabanı boyutunun 3 ' i kadar katına yararlanabilir. İçeri/dışarı aktarma isteği çalıştıran makinelerde yalnızca 450GB yerel disk alanı vardır. Sonuç olarak, bazı istekler "diskte yeterli alan yok" hatasını vererek başarısız olabilir. Bu durumda, geçici çözüm yeterli yerel disk alanına sahip bir makinede sqlpackage.exe çalıştırmak olur. 150GB 'den büyük veritabanlarını içeri/dışarı aktarırken, bu sorundan kaçınmak için SqlPackage kullanın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

> [!IMPORTANT]
> PowerShell Azure Resource Manager (RM) modülü hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. AzureRM modülü, en az Aralık 2020 ' e kadar hata düzeltmeleri almaya devam edecektir.  Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumluluklarını hakkında daha fazla bilgi için bkz. [new Azure PowerShell konusuna giriş az Module](/powershell/azure/new-azureps-module-az).

Azure 'a bir veritabanı içeri aktarma isteği göndermek için [New-Azsqldatabaseımport](/powershell/module/az.sql/new-azsqldatabaseimport) cmdlet 'ini kullanın. Veritabanı boyutuna bağlı olarak, almanın tamamlanması biraz zaman alabilir.

```powershell
$importRequest = New-AzSqlDatabaseImport -ResourceGroupName "<resourceGroupName>" `
    -ServerName "<serverName>" -DatabaseName "<databaseName>" `
    -DatabaseMaxSizeBytes "<databaseSizeInBytes>" -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzStorageAccountKey `
        -ResourceGroupName "<resourceGroupName>" -StorageAccountName "<storageAccountName>").Value[0] `
        -StorageUri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
        -Edition "Standard" -ServiceObjectiveName "P6" `
        -AdministratorLogin "<userId>" `
        -AdministratorLoginPassword $(ConvertTo-SecureString -String "<password>" -AsPlainText -Force)
```

Almanın ilerlemesini denetlemek için [Get-Azsqldatabaseımportexportstatus](/powershell/module/az.sql/get-azsqldatabaseimportexportstatus) cmdlet 'ini kullanabilirsiniz. İsteğin genellikle döndürdüğü cmdlet 'i hemen çalıştırın `Status: InProgress` . Gördüğünüz zaman içeri aktarma tamamlanır `Status: Succeeded` .

```powershell
$importStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink

[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress") {
    $importStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}

[Console]::WriteLine("")
$importStatus
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure 'a bir veritabanı içeri aktarma isteği göndermek için [az-SQL-DB-Import](/cli/azure/sql/db#az-sql-db-import) komutunu kullanın. Veritabanı boyutuna bağlı olarak, almanın tamamlanması biraz zaman alabilir.

```azurecli
# get the storage account key
az storage account keys list --resource-group "<resourceGroup>" --account-name "<storageAccount>"

az sql db import --resource-group "<resourceGroup>" --server "<server>" --name "<database>" `
    --storage-key-type "StorageAccessKey" --storage-key "<storageAccountKey>" `
    --storage-uri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
    -u "<userId>" -p "<password>"
```

* * *

> [!TIP]
> Başka bir betik örneği için bkz. [BACPAC dosyasından bir veritabanını Içeri aktarma](scripts/import-from-bacpac-powershell.md).

## <a name="limitations"></a>Sınırlamalar

- Bir veritabanının elastik havuza aktarılması desteklenmez. Verileri tek bir veritabanına aktarıp ardından bu veritabanını elastik havuza taşıyabilirsiniz.
- Azure hizmetlerine erişime Izin ver kapalı olarak ayarlandığında içeri aktarma hizmeti kullanılamaz. Bununla birlikte, bir Azure VM 'den sqlpackage.exe el ile çalıştırarak veya DACFx API kullanarak doğrudan kodunuzda dışarı aktarmayı gerçekleştirerek soruna geçici bir çözüm bulabilirsiniz.

## <a name="import-using-wizards"></a>Sihirbazları kullanarak içeri aktarma

Bu sihirbazları da kullanabilirsiniz.

- [SQL Server Management Studio veri katmanı uygulaması Içeri aktarma Sihirbazı](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [SQL Server içeri ve dışarı aktarma Sihirbazı](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı 'nda bir veritabanına bağlanma ve veritabanını sorgulama hakkında bilgi edinmek için bkz. [hızlı başlangıç: Azure SQL veritabanı: ' a bağlanmak ve veri sorgulamak için SQL Server Management Studio kullanın](connect-query-ssms.md).
- BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://techcommunity.microsoft.com/t5/DataCAT/Migrating-from-SQL-Server-to-Azure-SQL-Database-using-Bacpac/ba-p/305407).
- Performans önerileri de dahil olmak üzere tüm SQL Server veritabanı geçiş süreci hakkında bir tartışma için bkz. [Azure SQL veritabanı 'na SQL Server veritabanı geçişi](migrate-to-database-from-sql-server.md).
- Depolama anahtarlarını ve paylaşılan erişim imzalarını güvenli bir şekilde yönetme ve paylaşma hakkında bilgi edinmek için bkz. [Azure Storage Güvenlik Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
