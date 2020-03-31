---
title: Tek veya havuzlu bir veritabanını BACPAC dosyasına aktarma
description: Azure portalını kullanarak bir AZURE SQL veritabanını BACPAC dosyasına dışa aktarma
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 07/16/2019
ms.topic: conceptual
ms.openlocfilehash: 0bc72f0ad58829a3ff6545b5c4741ddc20916c31
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061623"
---
# <a name="export-an-azure-sql-database-to-a-bacpac-file"></a>Azure SQL Veritabanı’nı bir BACPAC dosyasına dışarı aktarma

Arşivleme veya başka bir platforma geçmek için bir veritabanı dışa aktarmanız gerektiğinde, veritabanı şemasını ve verilerini bir [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) dosyasına dışa aktarabilirsiniz. BACPAC dosyası, BIR SQL Server veritabanındaki meta verileri ve verileri içeren BACPAC uzantısı olan bir ZIP dosyasıdır. BACPAC dosyası Azure Blob depolama alanında veya yerel depolama alanında şirket içi bir konumda depolanabilir ve daha sonra Azure SQL Veritabanı'na veya bir SQL Server şirket içi yüklemeye geri aktarılabilir.

## <a name="considerations-when-exporting-an-azure-sql-database"></a>Azure SQL veritabanı dışa aktarırken dikkat edilmesi gerekenler

- Bir dışa aktarmanın işlem açısından tutarlı olması için, dışa aktarma sırasında yazma etkinliği oluşmadığından veya Azure SQL veritabanınızın işlem açısından tutarlı bir [kopyasından](sql-database-copy.md) dışa aktardığınızdan emin olmalısınız.
- Blob depolamasına dışa aktarıyorsanız, BACPAC dosyasının maksimum boyutu 200 GB'dır. Daha büyük bir BACPAC dosyasını arşivlemek için yerel depolamaya aktarın.
- Bu makalede tartışılan yöntemleri kullanarak bir BACPAC dosyasını Azure premium depolamasına dışa aktarma desteklenmez.
- Güvenlik duvarının arkasındaki depolama şu anda desteklenmez.
- Azure SQL Veritabanı'ndan yapılan dışa aktarma işlemi 20 saati aşarsa, iptal edilebilir. Dışa aktarma sırasında performansı artırmak için şunları yapabilirsiniz:

  - İşlem boyutunuzgeçici olarak artırın.
  - İhracat sırasında tüm okuma ve yazma etkinliklerini durdurun.
  - Tüm büyük tablolarda null olmayan değerlere [sahip kümelenmiş](https://msdn.microsoft.com/library/ms190457.aspx) bir dizin kullanın. Kümelenmiş dizinler olmadan, 6-12 saatten uzun sürerse bir dışa aktarma başarısız olabilir. Bunun nedeni, dışa aktarma hizmetinin tüm tabloyu dışa aktarmak için bir tablo tayini tamamlaması gerektiğidir. Tablolarınızın dışa aktarım için optimize edilip edilemeyeceğini belirlemenin iyi bir yolu, **DBCC'yi SHOW_STATISTICS** çalıştırmak ve *RANGE_HI_KEY* null olmadığından ve değerinin iyi bir dağılıma sahip olduğundan emin olmaktır. Ayrıntılar için [DBCC SHOW_STATISTICS'](https://msdn.microsoft.com/library/ms174384.aspx)a bakın.

> [!NOTE]
> BACAD'lar yedekleme ve geri yükleme işlemleri için kullanılmak üzere tasarlanmamıştır. Azure SQL Veritabanı, her kullanıcı veritabanı için otomatik olarak yedekleme oluşturur. Ayrıntılar için [iş sürekliliğine genel bakış](sql-database-business-continuity.md) ve [SQL Veritabanı yedeklemelerine](sql-database-automated-backups.md)bakın.

## <a name="export-to-a-bacpac-file-using-the-azure-portal"></a>Azure portalını kullanarak bir BACPAC dosyasına dışa aktarma

Azure PowerShell kullanarak yönetilen bir [örnekten](sql-database-managed-instance.md) bir veritabanının BACPAC'ını dışa aktarma şu anda desteklenmez. Bunun yerine SQL Server Management Studio veya SQLPackage'ı kullanın.

> [!NOTE]
> Azure portalı veya PowerShell aracılığıyla gönderilen alma/dışa aktarma isteklerini işleyen makinelerin BACPAC dosyasını ve Veri Katmanı Uygulama Çerçevesi (DacFX) tarafından oluşturulan geçici dosyaları depolaması gerekir. Gerekli disk alanı aynı boyuttaki veritabanları arasında önemli ölçüde değişir ve veritabanının boyutunun 3 katına kadar disk alanı gerektirebilir. Alma/dışa aktarma isteğini çalıştıran makinelerde yalnızca 450GB yerel disk alanı bulunur. Sonuç olarak, bazı istekler hata `There is not enough space on the disk`ile başarısız olabilir. Bu durumda, geçici çözüm yeterli yerel disk alanı olan bir makinede sqlpackage.exe çalıştırmaktır. Bu sorunu önlemek için 150 GB'tan büyük veritabanlarını içe aktarmak/dışa aktarmak için [SqlPackage'ı](#export-to-a-bacpac-file-using-the-sqlpackage-utility) kullanmamızı öneririz.

1. [Azure portalını](https://portal.azure.com)kullanarak bir veritabanı dışa aktarmak için veritabanınızın sayfasını açın ve araç çubuğunda **Dışa Aktar'ı** tıklatın.

   ![Veritabanı dışa aktarma](./media/sql-database-export/database-export1.png)

2. BACPAC dosya adını belirtin, dışa aktarma için varolan bir Azure depolama hesabı ve kapsayıcısını seçin ve ardından kaynak veritabanına erişmek için uygun kimlik bilgilerini sağlayın. Azure yöneticisi olarak SQL Server yönetici izinlerine sahip olmak, SQL Server yönetici izinlerine sahip olmakla aynı şey olmadığı için, Azure yöneticisi olsan bile burada SQL **Server yönetici girişi** gereklidir.

    ![Veritabanı dışa aktarma](./media/sql-database-export/database-export2.png)

3. **Tamam**'a tıklayın.

4. Dışa aktarma işleminin ilerlemesini izlemek için, dışa aktarılan veritabanını içeren SQL Veritabanı sunucusu için sayfayı açın. **Ayarlar'ın** altında ve ardından **İçe/Dışa Aktar geçmişini**tıklatın.

   ![ihracat geçmişi](./media/sql-database-export/export-history.png)

## <a name="export-to-a-bacpac-file-using-the-sqlpackage-utility"></a>SQLPackage yardımcı programını kullanarak bir BACPAC dosyasına dışa aktarma

[SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) komut satırı yardımcı programını kullanarak bir SQL veritabanı dışa aktarmak için, [dışa aktarma parametreleri ve özellikleri](https://docs.microsoft.com/sql/tools/sqlpackage#export-parameters-and-properties)ne bakın. SQLPackage yardımcı programı, Visual [Studio](https://msdn.microsoft.com/library/mt238290.aspx) için SQL Server Management Studio ve SQL [Server Data Tools'un](https://msdn.microsoft.com/library/mt204009.aspx)en son sürümlerini kullanır veya [SqlPackage'ın](https://www.microsoft.com/download/details.aspx?id=53876) en son sürümünü doğrudan Microsoft indirme merkezinden indirebilirsiniz.

Çoğu üretim ortamlarında ölçek ve performans için SQLPackage yardımcı programının kullanılmasını öneririz. BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/20../../migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Bu örnek, Active Directory Universal Authentication ile SqlPackage.exe kullanarak bir veritabanının nasıl dışa aktarılabildiğini gösterir:

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=apptestserver.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="export-to-a-bacpac-file-using-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) kullanarak BACPAC dosyasına dışa aktarma

SQL Server Management Studio'nun en yeni sürümleri, Bir AZURE SQL veritabanını BACPAC dosyasına dışa aktarmak için bir sihirbaz sağlar. Veri [Katmanı Uygulamasını Dışa Aktar'a](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application)bakın.

## <a name="export-to-a-bacpac-file-using-powershell"></a>PowerShell kullanarak bir BACPAC dosyasına dışa aktarma

> [!NOTE]
> [Yönetilen bir örnek,](sql-database-managed-instance.md) şu anda Azure PowerShell kullanarak bir BACPAC dosyasına veritabanı dışa aktarmayı desteklemez. Yönetilen bir örneği BACPAC dosyasına aktarmak için SQL Server Management Studio veya SQLPackage'ı kullanın.

Azure SQL Veritabanı hizmetine bir dışa aktarma veritabanı isteği göndermek için [Yeni-AzSqlDatabaseExport](/powershell/module/az.sql/new-azsqldatabaseexport) cmdlet'i kullanın. Veritabanınızın boyutuna bağlı olarak, dışa aktarma işleminin tamamlanması biraz zaman alabilir.

```powershell
$exportRequest = New-AzSqlDatabaseExport -ResourceGroupName $ResourceGroupName -ServerName $ServerName `
  -DatabaseName $DatabaseName -StorageKeytype $StorageKeytype -StorageKey $StorageKey -StorageUri $BacpacUri `
  -AdministratorLogin $creds.UserName -AdministratorLoginPassword $creds.Password
```

Dışa aktarma isteğinin durumunu kontrol etmek için [Get-AzSqlDatabaseImportExportStatus](/powershell/module/az.sql/get-azsqldatabaseimportexportstatus) cmdlet'i kullanın. İstekten hemen sonra bu çalıştırma genellikle Durum döndürür: **InProgress**. **Durum gördüğünüzde: Başarılı** dışa aktarma tamamlandı.

```powershell
$exportStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
[Console]::Write("Exporting")
while ($exportStatus.Status -eq "InProgress")
{
    Start-Sleep -s 10
    $exportStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink
    [Console]::Write(".")
}
[Console]::WriteLine("")
$exportStatus
```

## <a name="next-steps"></a>Sonraki adımlar

- Arşiv amacıyla bir veritabanının dışa aktarılmasına alternatif olarak tek bir veritabanlarının ve birleştirilmiş veritabanlarının uzun vadeli yedekleme bekletme hakkında bilgi edinmek için, [bkz.](sql-database-long-term-retention.md) Sql Agent işlerini, yalnızca [kopya veritabanı yedeklemelerini](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) uzun süreli yedekleme bekletmeye alternatif olarak zamanlamak için kullanabilirsiniz.
- BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
- BIR BACPAC'ı SQL Server veritabanına alma hakkında bilgi edinmek [için](https://msdn.microsoft.com/library/hh710052.aspx)bkz.
- BIR SQL Server veritabanından BACPAC dışa aktarma hakkında bilgi edinmek için [bkz.](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application)
- Bir veritabanını geçirmek için Veri Geçiş Hizmeti'ni kullanma hakkında bilgi edinmek için, [DMS kullanarak SQL Server'ı Azure SQL Veritabanına çevrimdışı olarak geçir](../dms/tutorial-sql-server-to-azure-sql.md)'e bakın.
- SQL Server'dan Azure SQL Veritabanına geçiş için bir ön yol olarak dışa aktarıyorsanız, [bkz.](sql-database-single-database-migrate.md)
- Depolama anahtarlarını ve paylaşılan erişim imzalarını güvenli bir şekilde nasıl yönetip paylaşılamayı öğrenmek için [Azure Depolama Güvenlik Kılavuzu'na](https://docs.microsoft.com/azure/storage/common/storage-security-guide)bakın.
