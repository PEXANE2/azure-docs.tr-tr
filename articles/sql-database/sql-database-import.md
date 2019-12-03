---
title: Veritabanı oluşturmak için BACPAC dosyasını içeri aktarma
description: BACPAC dosyasını içeri aktararak newAzure SQL veritabanı oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 06/20/2019
ms.openlocfilehash: 4d6997475099420319c52abbbce34b2756e215ed
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74687658"
---
# <a name="quickstart-import-a-bacpac-file-to-a-database-in-azure-sql-database"></a>Hızlı başlangıç: BACPAC dosyasını Azure SQL veritabanı 'nda bir veritabanına aktarma

Bir SQL Server veritabanını, [bacpac](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) dosyasını kullanarak Azure SQL veritabanındaki bir veritabanına aktarabilirsiniz. Verileri Azure Blob depolama (yalnızca standart depolama) veya şirket içi bir konumdaki yerel depolama alanında depolanan bir `BACPAC` dosyasından içeri aktarabilirsiniz. Daha fazla ve daha hızlı kaynak sağlayarak içeri aktarma hızını en üst düzeye çıkarmak için, içeri aktarma işlemi sırasında veritabanınızı daha yüksek bir hizmet katmanına ve işlem boyutuna ölçeklendirin. Ardından içeri aktarma işlemi başarılı olduktan sonra ölçeği azaltabilirsiniz.

> [!NOTE]
> İçeri aktarılan veritabanının uyumluluk düzeyi, kaynak veritabanının uyumluluk düzeyini temel alır.

> [!IMPORTANT]
> Veritabanınızı içeri aktardıktan sonra veritabanını geçerli uyumluluk düzeyinde (AdventureWorks2008R2 veritabanı için Level 100) veya daha yüksek bir düzeyde çalıştırmayı seçebilirsiniz. Veritabanını belirli bir uyumluluk düzeyinde çalıştırmanın etkileri ve buna yönelik seçenekler hakkında daha fazla bilgi için, bkz. [Veritabanı Uyumluluk Düzeyini Değiştirme](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Uyumluluk düzeyleriyle ilgili ek veritabanı düzeyi ayarları hakkında bilgi için, ayrıca bkz. [VERİTABANI KAPSAMLI YAPILANDIRMAYI DEĞİŞTİRME](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql).

## <a name="using-azure-portal"></a>Azure portalını kullanma

Azure portal BACPAC dosyasından içeri aktarmayı öğrenmek için bu videoyu izleyin veya aşağıdaki okumaya devam edin:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Its-just-SQL-Restoring-a-database-to-Azure-SQL-DB-from-backup/player?WT.mc_id=dataexposed-c9-niner]

[Azure Portal](https://portal.azure.com) *yalnızca* Azure SQL veritabanı 'Nda ve *yalnızca* Azure Blob depolamada depolanan bir bacpac dosyasından tek bir veritabanı oluşturulmasını destekler.

Bir veritabanını Azure PowerShell kullanılarak BACPAC dosyasından [yönetilen bir örneğe](sql-database-managed-instance.md) geçirme Şu anda desteklenmiyor. Bunun yerine SQL Server Management Studio veya SQLPackage kullanın.

> [!NOTE]
> Azure portal veya PowerShell aracılığıyla gönderilen içeri/dışarı aktarma isteklerini işleyen makineler, BACPAC dosyasını ve veri katmanı uygulama çerçevesi (DacFX) tarafından oluşturulan geçici dosyaları da depolaması gerekir. Gerekli disk alanı, aynı boyuta sahip veritabanları arasında önemli ölçüde farklılık gösterir ve veritabanı boyutunun 3 katına kadar disk alanı gerektirebilir. İçeri/dışarı aktarma isteği çalıştıran makinelerde yalnızca 450GB yerel disk alanı vardır. Sonuç olarak, bazı istekler `There is not enough space on the disk`hata vererek başarısız olabilir. Bu durumda geçici çözüm, SqlPackage. exe ' yi yeterli yerel disk alanına sahip bir makinede çalıştırmalıdır. Bu sorundan kaçınmak için, SQL paketini 150GB 'den büyük veritabanlarını içeri/dışarı aktarmak için kullanmayı öneririz.

1. Bir BACPAC dosyasından Azure portal kullanarak yeni bir tek veritabanına aktarmak için, uygun veritabanı sunucusu sayfasını açın ve ardından araç çubuğundan **veritabanını Içeri aktar**' ı seçin.  

   ![Veritabanı import1](./media/sql-database-import/import1.png)

1. BACPAC dosyası için depolama hesabını ve kapsayıcıyı seçin ve ardından İçeri aktarılacak BACPAC dosyasını seçin.

1. Yeni veritabanı boyutunu (genellikle Origin ile aynı) belirtin ve hedef SQL Server kimlik bilgilerini sağlayın. Yeni bir Azure SQL veritabanı için olası değerlerin bir listesi için, bkz. [veritabanı oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current).

   ![Veritabanı import2](./media/sql-database-import/import2.png)

1. **Tamam**’a tıklayın.

1. İçeri aktarmanın ilerlemesini izlemek için veritabanının sunucu sayfasını açın ve **Ayarlar**altında **Içeri/dışarı aktarma geçmişi**' ni seçin. Başarılı olduğunda, içeri aktarmanın **tamamlandı** durumu vardır.

   ![Veritabanı içeri aktarma durumu](./media/sql-database-import/import-status.png)

1. Veritabanının veritabanı sunucusunda canlı olduğunu doğrulamak için **SQL veritabanları** ' nı seçin ve yeni veritabanının **çevrimiçi**olduğunu doğrulayın.

## <a name="using-sqlpackage"></a>SqlPackage kullanma

[SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) komut satırı yardımcı programını kullanarak bir SQL Server veritabanını içeri aktarmak için, bkz. [içeri aktarma parametreleri ve özellikleri](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties). SqlPackage, [Visual Studio için](https://msdn.microsoft.com/library/mt204009.aspx)en son [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve SQL Server veri araçları sahiptir. En son [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) 'i Microsoft İndirme Merkezi ' nden de indirebilirsiniz.

Ölçek ve performans için, Azure portal kullanmak yerine, çoğu üretim ortamında SqlPackage kullanılması önerilir. `BACPAC` dosyalarını kullanarak geçirme hakkında bir SQL Server müşteri danışmanlık ekibi blogu için, bkz. [BACPAC dosyalarını kullanarak SQL Server 'Den Azure SQL veritabanı 'na geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Aşağıdaki SqlPackage komutu, **AdventureWorks2008R2** veritabanını yerel depolamadan **Mynewserver20170403**adlı bir Azure SQL veritabanı sunucusuna aktarır. **Premium** hizmet katmanı ve **P6** hizmeti hedefi ile **myMigratedDatabase** adlı yeni bir veritabanı oluşturur. Bu değerleri ortamınıza uygun şekilde değiştirin.

```cmd
sqlpackage.exe /a:import /tcs:"Data Source=<serverName>.database.windows.net;Initial Catalog=<migratedDatabase>;User Id=<userId>;Password=<password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Şirket güvenlik duvarının ardında tek bir veritabanını yöneten bir SQL veritabanı sunucusuna bağlanmak için, güvenlik duvarında 1433 numaralı bağlantı noktası açık olmalıdır. Yönetilen bir örneğe bağlanmak için, [Noktadan siteye bağlantı](sql-database-managed-instance-configure-p2s.md) veya Express Route bağlantısına sahip olmanız gerekir.

Bu örnek, Active Directory evrensel kimlik doğrulamasıyla SqlPackage kullanılarak bir veritabanının nasıl içeri aktarılacağını gösterir.

```cmd
sqlpackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="using-powershell"></a>PowerShell'i kullanma

> [!NOTE]
> [Yönetilen bir örnek](sql-database-managed-instance.md) Şu anda bir veritabanını Azure PowerShell kullanılarak bacpac dosyasından bir örnek veritabanına geçirmeyi desteklememektedir. Yönetilen bir örneğe aktarmak için SQL Server Management Studio veya SQLPackage kullanın.

> [!NOTE]
> Portal veya PowerShell aracılığıyla gönderilen içeri/dışarı aktarma isteklerini işleyen makinelerin bacpac dosyasını ve veri katmanı uygulaması çerçevesi (DacFX) tarafından oluşturulan geçici dosyaları depolaması gerekir. Gerekli disk alanı, aynı boyuta sahip olan veritabanları arasında önemli ölçüde farklılık gösterir ve veritabanı boyutunun 3 ' i kadar katına yararlanabilir. İçeri/dışarı aktarma isteği çalıştıran makinelerde yalnızca 450GB yerel disk alanı vardır. Sonuç olarak, bazı istekler "diskte yeterli alan yok" hatasını vererek başarısız olabilir. Bu durumda geçici çözüm, SqlPackage. exe ' yi yeterli yerel disk alanına sahip bir makinede çalıştırmalıdır. 150GB 'den büyük veritabanlarını içeri/dışarı aktarırken, bu sorundan kaçınmak için SqlPackage kullanın.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

> [!IMPORTANT]
> PowerShell Azure Resource Manager (RM) modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. AzureRM modülü, en az Aralık 2020 ' e kadar hata düzeltmeleri almaya devam edecektir.  Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumluluklarını hakkında daha fazla bilgi için bkz. [new Azure PowerShell konusuna giriş az Module](/powershell/azure/new-azureps-module-az).

Azure SQL veritabanı hizmetine bir veritabanı içeri aktarma isteği göndermek için [New-Azsqldatabaseımport](/powershell/module/az.sql/new-azsqldatabaseimport) cmdlet 'ini kullanın. Veritabanı boyutuna bağlı olarak, almanın tamamlanması biraz zaman alabilir.

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

Almanın ilerlemesini denetlemek için [Get-Azsqldatabaseımportexportstatus](/powershell/module/az.sql/get-azsqldatabaseimportexportstatus) cmdlet 'ini kullanabilirsiniz. İsteğin hemen ardından `Status: InProgress`döndüren cmdlet 'i çalıştırın. `Status: Succeeded`gördüğünüz zaman içeri aktarma tamamlanır.

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

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure SQL veritabanı hizmetine bir veritabanı içeri aktarma isteği göndermek için [az-SQL-DB-Import](/cli/azure/sql/db#az-sql-db-import) komutunu kullanın. Veritabanı boyutuna bağlı olarak, almanın tamamlanması biraz zaman alabilir.

```azure-cli
# get the storage account key
az storage account keys list --resource-group "<resourceGroupName>" --account-name "<storageAccountName>"

az sql db import --resource-group "<resourceGroupName>" --server "<serverName>" --name "<databaseName>" `
    --storage-key-type "StorageAccessKey" --storage-key "<storageAccountKey>" `
    --storage-uri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
    -u "<userId>" -p $(ConvertTo-SecureString -String "<password>" -AsPlainText -Force)
```

* * *

> [!TIP]
> Başka bir betik örneği için bkz. [BACPAC dosyasından bir veritabanını Içeri aktarma](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="limitations"></a>Sınırlamalar

Elastik havuzdaki bir veritabanına içeri aktarma desteklenmiyor. Verileri tek bir veritabanına aktarabilir ve sonra veritabanını elastik bir havuza taşıyabilirsiniz.

## <a name="import-using-wizards"></a>Sihirbazları kullanarak içeri aktarma

Bu sihirbazları da kullanabilirsiniz.

- [SQL Server Management Studio veri katmanı uygulaması Içeri aktarma Sihirbazı](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [SQL Server içeri ve dışarı aktarma Sihirbazı](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Sonraki adımlar

- İçeri aktarılan bir SQL veritabanına bağlanma ve sorgu yapma hakkında bilgi edinmek için bkz. [hızlı başlangıç: Azure SQL veritabanı: verileri bağlamak ve sorgulamak için SQL Server Management Studio kullanın](sql-database-connect-query-ssms.md).
- BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://techcommunity.microsoft.com/t5/DataCAT/Migrating-from-SQL-Server-to-Azure-SQL-Database-using-Bacpac/ba-p/305407).
- Performans önerileri de dahil olmak üzere tüm SQL Server veritabanı geçiş süreci hakkında bir tartışma için bkz. [Azure SQL veritabanı 'na SQL Server veritabanı geçişi](sql-database-single-database-migrate.md).
- Depolama anahtarlarını ve paylaşılan erişim imzalarını güvenli bir şekilde yönetme ve paylaşma hakkında bilgi edinmek için bkz. [Azure Storage Güvenlik Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
