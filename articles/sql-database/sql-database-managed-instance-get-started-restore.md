---
title: Yönetilen örneğe yedekleme geri yükleme
description: SSMS kullanarak veritabanı yedeklemesini Azure SQL Veritabanı Yönetilen Örneğine geri yükleyin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab, bonova
ms.date: 12/14/2018
ms.openlocfilehash: 3b12aa860bee189f706bd98df63fc194a3a0cc71
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874713"
---
# <a name="quickstart-restore-a-database-to-a-managed-instance"></a>Hızlı başlatma: Veritabanını Yönetilen Örneğe geri yükleme

Bu hızlı başlangıçta, Azure Blob depolamaalanından azure SQL Veritabanı [Yönetilen Örneği'ne](sql-database-managed-instance.md)bir veritabanını (Geniş Dünya İthalatçıları - Standart yedekleme dosyası) geri yüklemek için SQL Server Management Studio'yu (SSMS) kullanacaksınız.

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

> [!NOTE]
> Azure Veritabanı Geçiş Hizmeti'ni (DMS) kullanarak geçiş hakkında daha fazla bilgi için Bkz. [DMS kullanarak Yönetilen Örnek geçişi.](../dms/tutorial-sql-server-to-managed-instance.md)
> Çeşitli geçiş yöntemleri hakkında daha fazla bilgi için, [SQL Server örnek geçişiiçin Azure SQL Veritabanı Yönetilen Örneği'ne](sql-database-managed-instance-migrate.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç:

- Yönetilen Örnek [Oluşturma](sql-database-managed-instance-get-started.md) hızlı başlangıcından kaynakları kullanır.
- Bilgisayarınızın en son [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) yüklü olması gerekir.
- Yönetilen Örneğinize bağlanmak için SSMS kullanılması nı gerektirir. Nasıl bağlanılailgili şu hızlı başlangıçlara bakın:
  - Yönetilen [Örnek'te ortak bitiş noktasını etkinleştirin](sql-database-managed-instance-public-endpoint-configure.md) - bu öğretici için önerilen yaklaşımdır.
  - [Bir Azure VM'den bir Azure SQL Veritabanı Yönetilen Örneğine bağlanma](sql-database-managed-instance-configure-vm.md)
  - Şirket içinde [bir Azure SQL Veritabanı Yönetilen Örneği'ne noktadan siteye bağlantı yapılandırın.](sql-database-managed-instance-configure-p2s.md)

> [!NOTE]
> Azure Blob depolama ve [Paylaşılan Erişim İmzası (SAS) anahtarını](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)kullanarak bir SQL Server veritabanını yedekleme ve geri kurma hakkında daha fazla bilgi [için, SQL Server Backup to URL'ye](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2017)bakın.

## <a name="restore-the-database-from-a-backup-file"></a>Veritabanını yedek bir dosyadan geri yükleme

SSMS'te, Geniş Dünya İthalatçılar veritabanını Yönetilen Örneğinize geri yüklemek için aşağıdaki adımları izleyin. Veritabanı yedekleme dosyası önceden yapılandırılmış bir Azure Blob depolama hesabında depolanır.

1. SSMS'leri açın ve Yönetilen Örneğinize bağlanın.
2. Sol menüden Yönetilen Örnek'e sağ tıklayın ve yeni bir sorgu penceresi açmak için **Yeni Sorgu'yu** seçin.
3. Yönetilen Örneğinizde [bir kimlik bilgisi oluşturmak](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) için önceden yapılandırılmış bir depolama hesabı ve SAS anahtarı kullanan aşağıdaki SQL komut dosyasını çalıştırın.

   ```sql
   CREATE CREDENTIAL [https://mitutorials.blob.core.windows.net/databases]
   WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
   , SECRET = 'sv=2017-11-09&ss=bfqt&srt=sco&sp=rwdlacup&se=2028-09-06T02:52:55Z&st=2018-09-04T18:52:55Z&spr=https&sig=WOTiM%2FS4GVF%2FEEs9DGQR9Im0W%2BwndxW2CQ7%2B5fHd7Is%3D'
   ```

    ![kimlik bilgisi oluşturma](./media/sql-database-managed-instance-get-started-restore/credential.png)

4. Kimlik bilgilerinizi denetlemek için, yedek dosya listesi almak için [kapsayıcı](https://azure.microsoft.com/services/container-instances/) URL'sini kullanan aşağıdaki komut dosyasını çalıştırın.

   ```sql
   RESTORE FILELISTONLY FROM URL =
      'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![dosya listesi](./media/sql-database-managed-instance-get-started-restore/file-list.png)

5. Geniş Dünya İthalatçılar veritabanını geri yüklemek için aşağıdaki komut dosyasını çalıştırın.

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![geri yükleme](./media/sql-database-managed-instance-get-started-restore/restore.png)

6. Geri yükleme nizin durumunu izlemek için aşağıdaki komut dosyasını çalıştırın.

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time
   FROM sys.dm_exec_requests r
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')
   ```

7. Geri yükleme tamamlandığında, Nesne Gezgini'nde veritabanını görüntüleyin. Veritabanı geri [yüklemesi sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) görünümünü kullanarak tamamlandığını doğrulayabilirsiniz.

> [!NOTE]
> Veritabanı geri yükleme işlemi asynchronous ve geri alınamaz. Bağlantı nın kesilmesi veya zaman aşımı süresinin dolması durumunda sql server management studio bazı hata alabilirsiniz. Azure SQL Veritabanı arka planda veritabanını geri yüklemeye devam eder ve [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) ve [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) görünümlerini kullanarak geri yükleme nin ilerlemesini izleyebilirsiniz.
> Geri yükleme işleminin bazı aşamalarında, sistem görünümlerinde gerçek veritabanı adı yerine benzersiz tanımlayıcı görürsünüz. İfade `RESTORE` davranış farklılıkları hakkında [bilgi edinin.](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#restore-statement)

## <a name="next-steps"></a>Sonraki adımlar

- Bir URL'nin yedeklemesini sorun giderme için, [SQL Server Backup to URL En İyi Uygulamalar ve Sorun Giderme'ye](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting)bakın.
- Uygulama bağlantısı seçeneklerine genel bakış için [bkz.](sql-database-managed-instance-connect-app.md)
- Sık kullandığınız araçları veya dilleri kullanarak sorgu yapmak için [Hızlı Başlangıçlar: Azure SQL Veritabanı Bağlantısı ve Sorgu'ya](sql-database-connect-query.md)bakın.
