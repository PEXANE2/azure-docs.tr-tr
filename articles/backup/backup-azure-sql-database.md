---
title: SQL Server veritabanlarını Azure'a yedekleme
description: Bu makalede, SQL Server'ın Azure'a nasıl yedeklenenekadar yedekleneme şekli açıklanmaktadır. Makalede ayrıca SQL Server kurtarma açıklar.
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: 537257733d7693598fd8007da6ce12c28fbeb02a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79408769"
---
# <a name="about-sql-server-backup-in-azure-vms"></a>Azure VM'lerindeki SQL Server Backup hakkında

[Azure Yedekleme,](backup-overview.md) Azure VM'lerde çalışan SQL Server'ı yedeklemek için akış tabanlı, özelleştirilmiş bir çözüm sunar. Bu çözüm, Azure Yedekleme'nin sıfır altyapı yedekleme, uzun süreli saklama ve merkezi yönetim avantajlarıyla uyumludur. Ayrıca, özellikle SQL Server için aşağıdaki avantajları sağlar:

1. Tüm yedekleme türlerini destekleyen iş yüküne duyarlı yedeklemeler - tam, diferansiyel ve günlük
2. Sık günlük yedeklemeleri ile 15-dk RPO (kurtarma noktası hedefi)
3. Saniyeye kadar zaman içinde iyileşme
4. Bireysel veritabanı düzeyi yedekleme ve geri yükleme

Bugün desteklediğimiz yedekleme ve geri yükleme senaryolarını görüntülemek için [destek matrisine](sql-support-matrix.md#scenario-support)bakın.

## <a name="backup-process"></a>Yedekleme işlemi

Bu çözüm, SQL veritabanlarınızın yedeklerini almak için SQL yerel API'lerinden yararlanır.

* Korumak istediğiniz SQL Server VM'yi belirttiğinizde ve veritabanıiçin sorgu yaptığınızda, Azure Yedekleme hizmeti ad `AzureBackupWindowsWorkload` uzantısı ile VM'ye bir iş yükü yedekleme uzantısı yükler.
* Bu uzantı bir koordinatör ve bir SQL eklentisi oluşur. Koordinatör yedekleme, yedekleme ve geri yükleme yapılandırma gibi çeşitli işlemler için iş akışlarını tetiklemekten sorumlu olsa da, eklenti gerçek veri akışından sorumludur.
* Azure Yedekleme, bu VM'deki veritabanlarını bulabilmek `NT SERVICE\AzureWLBackupPluginSvc`için hesabı oluşturur. Bu hesap yedekleme ve geri yükleme için kullanılır ve SQL sysadmin izinleri gerektirir. Hesap `NT SERVICE\AzureWLBackupPluginSvc` bir [Sanal Hizmet Hesabıdır](https://docs.microsoft.com/windows/security/identity-protection/access-control/service-accounts#virtual-accounts)ve bu nedenle herhangi bir parola yönetimi gerektirmez. Azure Yedekleme veritabanı `NT AUTHORITY\SYSTEM` bulma/sorgulama için hesaptan yararlanır, bu nedenle bu hesabın SQL'de genel oturum açması gerekir. SQL Server VM'yi Azure Marketi'nden oluşturmadıysanız, **usererrorSQLNoSysadminMembership**hatası alabilirsiniz. Bu durumda [bu yönergeleri izleyin.](#set-vm-permissions)
* Seçili veritabanlarında yapılandırma korumasını tetikledikten sonra, yedekleme hizmeti koordinatörü yedekleme zamanlamaları ve uzantının VM'de yerel olarak önbelleğe aldığı diğer ilke ayrıntılarıyla ayarlar.
* Planlanan zamanda, koordinatör eklentiile iletişim kurar ve VDI kullanarak yedekleme verilerini SQL sunucusundan aktarmaya başlar.  
* Eklenti verileri doğrudan kurtarma hizmetleri kasasına gönderir ve böylece bir hazırlama konumuna duyulan gereksinimi ortadan kaldırır. Veriler, Azure Yedekleme hizmeti tarafından şifrelenir ve depolama hesaplarında depolanır.
* Veri aktarımı tamamlandığında, koordinatör yedekleme hizmetiyle commit onaylar.

  ![SQL Yedekleme mimarisi](./media/backup-azure-sql-database/backup-sql-overview.png)

## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce aşağıdakileri doğrulayın:

1. Azure'da çalışan bir SQL Server örneğinin olduğundan emin olun. Pazarda [hızlı bir şekilde bir SQL Server örneği oluşturabilirsiniz.](../virtual-machines/windows/sql/quickstart-sql-vm-create-portal.md)
2. Özellik [değerlendirmesini](sql-support-matrix.md#feature-consideration-and-limitations) ve [senaryo desteğini](sql-support-matrix.md#scenario-support)gözden geçirin.
3. Bu senaryoyla ilgili [sık sorulan soruları gözden geçirin.](faq-backup-sql-server.md)

## <a name="set-vm-permissions"></a>VM izinlerini ayarlama

  Bir SQL Server'da bulma çalıştırdığınızda, Azure Yedekleme aşağıdakileri yapar:

* AzureBackupWindowsİş uzantısını ekler.
* Sanal makinedeki veritabanlarını keşfetmek için bir NT SERVICE\AzureWLBackupPluginSvc hesabı oluşturur. Bu hesap yedekleme ve geri yükleme için kullanılır ve SQL sysadmin izinleri gerektirir.
* VM üzerinde çalışan veritabanlarını keşfeden Azure Yedekleme, NT AUTHORITY\SYSTEM hesabını kullanır. Bu hesap SQL'de genel oturum açma olmalıdır.

SQL Server VM'yi Azure Marketi'nde oluşturmadıysanız veya SQL 2008 ve 2008 R2'deyseniz, **UserErrorSQLNoSysadminMembership** hatası alabilirsiniz.

Windows 2008 R2'de çalışan **SQL 2008** ve **2008 R2** durumunda izin vermek için [buraya](#give-sql-sysadmin-permissions-for-sql-2008-and-sql-2008-r2)bakın.

Diğer tüm sürümler için izinleri aşağıdaki adımlarla düzeltin:

  1. SQL Server Management Studio 'da (SSMS) oturum açma izni olan bir hesap kullanın. Özel izinlere ihtiyacınız yoksa, Windows kimlik doğrulaması çalışması gerekir.
  2. SQL Server'da **Security/Logins** klasörünü açın.

      ![Hesapları görmek için Güvenlik/Giriş klasörünü açın](./media/backup-azure-sql-database/security-login-list.png)

  3. **Girişler** klasörüne sağ tıklayın ve **Yeni Giriş'i**seçin. **Giriş Yap - Yeni**, **Ara'yı**seçin.

      ![Giriş - Yeni iletişim kutusunda Ara'yı seçin](./media/backup-azure-sql-database/new-login-search.png)

  4. Windows sanal hizmet hesabı **NT SERVICE\AzureWLBackupPluginSvc** sanal makine kaydı ve SQL bulma aşamasında oluşturuldu. 'de gösterildiği gibi hesap **adını girin nesneyin adını seçin.** Adı çözmek için **Adları Denetle'yi** seçin. **Tamam**'a tıklayın.

      ![Bilinmeyen hizmet adını çözmek için Adları Denetle'yi seçin](./media/backup-azure-sql-database/check-name.png)

  5. **Sunucu**Rolleri'nde, **sysadmin** rolünün seçildiğinden emin olun. **Tamam**'a tıklayın. Gerekli izinler şimdi var olmalıdır.

      ![sysadmin sunucu rolünün seçildiğinden emin olun](./media/backup-azure-sql-database/sysadmin-server-role.png)

  6. Şimdi veritabanını Kurtarma Hizmetleri kasasıyla ilişkilendirin. Azure portalında, **Korumalı Sunucular** listesinde, **DB'leri Yeniden Keşfedin'i yeniden >** hata durumundaolan sunucuyu sağ tıklatın.

      ![Sunucunun uygun izinlere sahip olduğunu doğrulama](./media/backup-azure-sql-database/check-erroneous-server.png)

  7. **Bildirimler** alanında ilerlemeyi denetleyin. Seçili veritabanları bulunduğunda, bir başarı iletisi görüntülenir.

      ![Dağıtım başarı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

> [!NOTE]
> SQL Server'ınızda birden çok SQL Server örneği yüklüyse, tüm SQL örneklerine **NT Service\AzureWLBackupPluginSvc** hesabı için sysadmin izni eklemeniz gerekir.

### <a name="give-sql-sysadmin-permissions-for-sql-2008-and-sql-2008-r2"></a>SQL 2008 ve SQL 2008 R2 için SQL sysadmin izinleri verin

SQL Server **Instance'a NT AUTHORITY\SYSTEM** ve **NT Service\AzureWLBackupPluginSvc** girişlerini ekleyin:

1. Nesne gezgininde SQL Server Instance'a gidin.
2. Güvenlik -> Girişlerine Gidin
3. Girişler'e sağ tıklayın ve *Yeni Giriş'e tıklayın...*

    ![SSMS kullanarak Yeni Giriş](media/backup-azure-sql-database/sql-2k8-new-login-ssms.png)

4. Genel sekmesine gidin ve Giriş Adı olarak **NT AUTHORITY\SYSTEM** girin.

    ![SSMS için giriş adı](media/backup-azure-sql-database/sql-2k8-nt-authority-ssms.png)

5. Sunucu *Rolleri'ne* gidin ve *genel* ve *sysadmin* rollerini seçin.

    ![SSMS'te rolleri seçme](media/backup-azure-sql-database/sql-2k8-server-roles-ssms.png)

6. *Durum'a*git. Veritabanı altyapısına bağlanma izni ve *Etkinleştirilen*Olarak Giriş 'i *ver.*

    ![SSMS'de izin verme](media/backup-azure-sql-database/sql-2k8-grant-permission-ssms.png)

7. Tamam'a tıklayın.
8. SQL Server örneğine NT Service\AzureWLBackupPluginSvc girişi eklemek için aynı adım sırasını (yukarıda 1-7) yineleyin. Oturum açma zaten varsa, sysadmin sunucu rolüne sahip olduğundan emin olun ve Durum altında veritabanı altyapısına bağlanma izni verdive Etkinleştirildikçe Giriş Yapın.
9. İzin verdikten sonra, PORTALDA **DB'leri Yeniden Keşfedin:** Azure VM'de Vault **->** Backup Infrastructure **->** İş Yükü:

    ![Azure portalında DB'leri yeniden keşfedin](media/backup-azure-sql-database/sql-rediscover-dbs.png)

Alternatif olarak, aşağıdaki PowerShell komutlarını yönetici modunda çalıştırarak izinleri veren otomatikleştirebilirsiniz. Örnek adı varsayılan olarak MSSQLSERVER olarak ayarlanır. Gerekirse komut dosyasındaki örnek adı bağımsız değişkenini değiştirin:

```powershell
param(
    [Parameter(Mandatory=$false)]
    [string] $InstanceName = "MSSQLSERVER"
)
if ($InstanceName -eq "MSSQLSERVER")
{
    $fullInstance = $env:COMPUTERNAME   # In case it is the default SQL Server Instance
}
else
{
    $fullInstance = $env:COMPUTERNAME + "\" + $InstanceName   # In case of named instance
}
try
{
    sqlcmd.exe -S $fullInstance -Q "sp_addsrvrolemember 'NT Service\AzureWLBackupPluginSvc', 'sysadmin'" # Adds login with sysadmin permission if already not available
}
catch
{
    Write-Host "An error occurred:"
    Write-Host $_.Exception|format-list -force
}
try
{
    sqlcmd.exe -S $fullInstance -Q "sp_addsrvrolemember 'NT AUTHORITY\SYSTEM', 'sysadmin'" # Adds login with sysadmin permission if already not available
}
catch
{
    Write-Host "An error occurred:"
    Write-Host $_.Exception|format-list -force
}
```

## <a name="next-steps"></a>Sonraki adımlar

* SQL Server veritabanlarını yedekleme [hakkında bilgi edinin.](backup-sql-server-database-azure-vms.md)
* Yedeklenen SQL Server veritabanlarını geri alma [hakkında bilgi edinin.](restore-sql-database-azure-vm.md)
* Yedeklenmiş SQL Server veritabanlarını yönetme [hakkında bilgi edinin.](manage-monitor-sql-database-backup.md)
