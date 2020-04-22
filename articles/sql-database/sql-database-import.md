---
title: Veritabanı oluşturmak için bacpac dosyasını alma
description: BACPAC dosyalarını içe aktararak yeni bir Azure SQL veritabanı oluşturun.
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
ms.openlocfilehash: 7db3f6f50745526876ef2ca6e3253f1931420f0f
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81683257"
---
# <a name="quickstart-import-a-bacpac-file-to-a-database-in-azure-sql-database"></a>Quickstart: BACPAC dosyasını Azure SQL Veritabanı'ndaki bir veritabanına aktarma

[BIR BACPAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) dosyakullanarak Azure SQL Veritabanı'ndaki bir veritabanına SQL Server veritabanı nı aktarabilirsiniz. Verileri Azure Blob `BACPAC` depolama alanında depolanan bir dosyadan (yalnızca standart depolama alanı) veya şirket içi bir konumdaki yerel depolama alanından içe aktarabilirsiniz. Daha fazla ve daha hızlı kaynak sağlayarak alma hızını en üst düzeye çıkarmak için veritabanınızı alma işlemi sırasında daha yüksek bir hizmet katmanına ve bilgi işlem boyutuna ölçeklendirin. Daha sonra alma başarılı olduktan sonra küçültebilirsiniz.

> [!NOTE]
> İçe aktarılan veritabanının uyumluluk düzeyi kaynak veritabanının uyumluluk düzeyini temel almaktadır.

> [!IMPORTANT]
> Veritabanınızı aldıktan sonra, veritabanını geçerli uyumluluk düzeyinde (AdventureWorks2008R2 veritabanı için seviye 100) veya daha yüksek bir düzeyde çalıştırmayı seçebilirsiniz. Veritabanını belirli bir uyumluluk düzeyinde çalıştırmanın etkileri ve buna yönelik seçenekler hakkında daha fazla bilgi için, bkz. [Veritabanı Uyumluluk Düzeyini Değiştirme](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Uyumluluk düzeyleriyle ilgili ek veritabanı düzeyi ayarları hakkında bilgi için, ayrıca bkz. [VERİTABANI KAPSAMLI YAPILANDIRMAYI DEĞİŞTİRME](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql).

## <a name="using-azure-portal"></a>Azure portalını kullanma

Azure portalındaki bir BACPAC dosyasından nasıl içe aktarılabildiğini görmek veya aşağıdakileri okumaya devam etmek için bu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Its-just-SQL-Restoring-a-database-to-Azure-SQL-DB-from-backup/player?WT.mc_id=dataexposed-c9-niner]

[Azure portalı](https://portal.azure.com) *yalnızca* Azure SQL Veritabanı'nda tek bir veritabanı oluşturmayı ve *yalnızca* Azure Blob depolama alanında depolanan bir BACPAC dosyasından oluşturmayı destekler.

Azure PowerShell kullanarak bir BACPAC dosyasından yönetilen bir [örneğe](sql-database-managed-instance.md) veritabanını geçirmek şu anda desteklenmez. Bunun yerine SQL Server Management Studio veya SQLPackage'ı kullanın.

> [!NOTE]
> Azure portalı veya PowerShell aracılığıyla gönderilen alma/dışa aktarma isteklerini işleyen makinelerin BACPAC dosyasını ve Veri Katmanı Uygulama Çerçevesi (DacFX) tarafından oluşturulan geçici dosyaları depolaması gerekir. Gerekli disk alanı aynı boyuttaki veritabanları arasında önemli ölçüde değişir ve veritabanının boyutunun 3 katına kadar disk alanı gerektirebilir. Alma/dışa aktarma isteğini çalıştıran makinelerde yalnızca 450GB yerel disk alanı bulunur. Sonuç olarak, bazı istekler hata `There is not enough space on the disk`ile başarısız olabilir. Bu durumda, geçici çözüm yeterli yerel disk alanı olan bir makinede sqlpackage.exe çalıştırmaktır. Bu sorunu önlemek için 150 GB'tan büyük veritabanlarını içe aktarmak/dışa aktarmak için SqlPackage'ı kullanmamızı öneririz.

1. Azure portalını kullanarak bir BACPAC dosyasından yeni bir veritabanına aktetmek için, uygun veritabanı sunucusu sayfasını açın ve araç çubuğunda **Veritabanını Aktar'ı**seçin.  

   ![Veritabanı alma1](./media/sql-database-import/sql-server-import-database.png)

1. BACPAC dosyasının depolama hesabını ve kapsayıcısını seçin ve sonra içe aktarılan BACPAC dosyasını seçin.

1. Yeni veritabanı boyutunu (genellikle başlangıçla aynı) belirtin ve hedef SQL Server kimlik bilgilerini sağlayın. Yeni bir Azure SQL veritabanı için olası değerlerin listesi için veritabanı [oluştur'a](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current)bakın.

   ![Veritabanı alma2](./media/sql-database-import/sql-server-import-database-settings.png)

1. **Tamam**'a tıklayın.

1. Bir alma nın ilerlemesini izlemek için veritabanının sunucu sayfasını açın ve **Ayarlar**altında **İçe/Dışa Aktar geçmişini**seçin. Başarılı olduğunda, içe aktarma nın **Tamamlanmış** durumu vardır.

   ![Veritabanı alma durumu](./media/sql-database-import/sql-server-import-database-history.png)

1. Veritabanının veritabanı sunucusunda canlı olduğunu doğrulamak için **SQL veritabanlarını** seçin ve yeni veritabanının **Çevrimiçi**olduğunu doğrulayın.

## <a name="using-sqlpackage"></a>SqlPackage kullanma

[SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) komut satırı yardımcı programını kullanarak bir SQL Server veritabanı almak için [alma parametrelerini ve özelliklerini](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties)görün. SqlPackage Visual [Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) için en son SQL Server Management Studio ve [SQL Server Veri Araçları](https://msdn.microsoft.com/library/mt204009.aspx)vardır. En son [SqlPackage'ı](https://www.microsoft.com/download/details.aspx?id=53876) Microsoft indirme merkezinden de indirebilirsiniz.

Ölçek ve performans için, Azure portalını kullanmak yerine çoğu üretim ortamlarında SqlPackage kullanmanızı öneririz. Dosyaları kullanarak `BACPAC` geçiş hakkında bir SQL Server Müşteri Danışma Ekibi blogunda [BACPAC Dosyalarını kullanarak SQL Server'dan Azure SQL Veritabanına geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/)bkz.

Aşağıdaki SqlPackage komutu **AdventureWorks2008R2** veritabanını yerel depolamadan **mynewserver20170403**adlı bir Azure SQL Veritabanı sunucusuna alır. **Premium** hizmet katmanı ve **P6** Hizmet Hedefi ile **myMigratedDatabase** adında yeni bir veritabanı oluşturur. Bu değerleri ortamınız için uygun şekilde değiştirin.

```cmd
sqlpackage.exe /a:import /tcs:"Data Source=<serverName>.database.windows.net;Initial Catalog=<migratedDatabase>;User Id=<userId>;Password=<password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Kurumsal bir güvenlik duvarının arkasından tek bir veritabanını yöneten bir SQL Veritabanı sunucusuna bağlanmak için güvenlik duvarının 1433 portu açık olması gerekir. Yönetilen bir örne bağlanmak için noktadan [siteye bağlantınız](sql-database-managed-instance-configure-p2s.md) veya açık bir rota bağlantınız olması gerekir.

Bu örnek, Active Directory Universal Authentication ile SqlPackage kullanarak bir veritabanının nasıl içe aktarılabildiğini gösterir.

```cmd
sqlpackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="using-powershell"></a>PowerShell’i kullanma

> [!NOTE]
> [Yönetilen bir örnek,](sql-database-managed-instance.md) azure PowerShell kullanarak bir BACPAC dosyasından bir veritabanına geçişi şu anda desteklemez. Yönetilen bir örne almak için SQL Server Management Studio veya SQLPackage'ı kullanın.

> [!NOTE]
> Portal veya Powershell aracılığıyla gönderilen alma/dışa aktarma isteklerini işleyen makinelerin bacpac dosyasını ve Veri Katmanı Uygulama Çerçevesi (DacFX) tarafından oluşturulan geçici dosyaları depolaması gerekir. Gerekli disk alanı aynı boyuttaki DB'ler arasında önemli ölçüde değişir ve veritabanı boyutunun 3 katına kadar sürebilir. Alma/dışa aktarma isteğini çalıştıran makinelerde yalnızca 450GB yerel disk alanı bulunur. Sonuç olarak, bazı istekler "Diskte yeterli alan yok" hatasıyla başarısız olabilir. Bu durumda, geçici çözüm yeterli yerel disk alanı olan bir makinede sqlpackage.exe çalıştırmaktır. 150 GB'dan büyük veritabanlarını içe aktarma/dışa aktarma yaparken, bu sorunu önlemek için SqlPackage'ı kullanın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi (RM) modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirmeler Az.Sql modülü içindir. AzureRM modülü en az Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecektir.  Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumlulukları hakkında daha fazla bilgi için [yeni Azure PowerShell Az modüllerini tanıtın.](/powershell/azure/new-azureps-module-az)

Azure SQL Veritabanı hizmetine bir alma veritabanı isteği göndermek için [Yeni-AzSqlDatabaseImport](/powershell/module/az.sql/new-azsqldatabaseimport) cmdlet'i kullanın. Veritabanı boyutuna bağlı olarak, içe aktarmanın tamamlanması biraz zaman alabilir.

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

Alma nın ilerlemesini kontrol etmek için [Get-AzSqlDatabaseImportExportStatus](/powershell/module/az.sql/get-azsqldatabaseimportexportstatus) cmdlet'i kullanabilirsiniz. İstek ten hemen sonra cmdlet `Status: InProgress`çalıştıran genellikle döner . Bunu gördüğünüzde `Status: Succeeded`alma işlemi tamamlanır.

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

Azure SQL Veritabanı hizmetine bir alma veritabanı isteği göndermek için [az-sql-db-içe aktarma](/cli/azure/sql/db#az-sql-db-import) komutunu kullanın. Veritabanı boyutuna bağlı olarak, içe aktarmanın tamamlanması biraz zaman alabilir.

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
> Başka [bir](scripts/sql-database-import-from-bacpac-powershell.md)komut dosyası örneği için bkz.

## <a name="limitations"></a>Sınırlamalar

- Elastik havuzda bir veritabanına alma desteklenmez. Verileri tek bir veritabanına aktarAbilir ve veritabanını esnek bir havuza taşıyabilirsiniz.
- Azure hizmetlerine erişime izin ver,KAPALI olarak ayarlandığında Dışa Aktarma Hizmeti çalışmaz. Ancak, bir Azure VM'den sqlpackage.exe'yi el ile çalıştırarak veya DACFx API'sini kullanarak dışa aktarmayı doğrudan kodunuzda gerçekleştirerek sorunu çözebilirsiniz.

## <a name="import-using-wizards"></a>Sihirbazları kullanarak alma

Bu sihirbazları da kullanabilirsiniz.

- [SQL Server Management Studio'da Veri katmanı Uygulama Sihirbazı'nı aktarın.](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard)
- [SQL Server Alma ve Dışa Aktarma Sihirbazı](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Sonraki adımlar

- İçe aktarılan bir SQL Veritabanına nasıl bağlanıp sorgulayabilirsiniz öğrenmek [için, bkz.](sql-database-connect-query-ssms.md)
- BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://techcommunity.microsoft.com/t5/DataCAT/Migrating-from-SQL-Server-to-Azure-SQL-Database-using-Bacpac/ba-p/305407).
- Performans önerileri de dahil olmak üzere tüm SQL Server veritabanı geçiş işleminin tartışılması [için](sql-database-single-database-migrate.md)bkz.
- Depolama anahtarlarını ve paylaşılan erişim imzalarını güvenli bir şekilde nasıl yönetip paylaşılamayı öğrenmek için [Azure Depolama Güvenlik Kılavuzu'na](https://docs.microsoft.com/azure/storage/common/storage-security-guide)bakın.
