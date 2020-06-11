---
title: 'Hızlı başlangıç: bir yedeklemeyi geri yükleme (SSMS)'
titleSuffix: Azure SQL Managed Instance
description: Bu hızlı başlangıçta, SQL Server Management Studio (SSMS) kullanarak bir veritabanı yedeklemesini Azure SQL yönetilen örneğine geri yüklemeyi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab, bonova
ms.date: 12/14/2018
ms.openlocfilehash: 23deba1dd2bbb706d1c76cb71cf1c08330049036
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84659318"
---
# <a name="quickstart-restore-a-database-to-azure-sql-managed-instance-with-ssms"></a>Hızlı başlangıç: SSMS ile bir veritabanını Azure SQL yönetilen örneği 'ne geri yükleme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu hızlı başlangıçta, Azure Blob depolama 'dan [Azure SQL yönetilen örneğine](sql-managed-instance-paas-overview.md)bir veritabanını (geniş dünya Importers-standart yedekleme dosyası) geri yüklemek için SQL Server Management Studio (SSMS) kullanacaksınız.

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

> [!NOTE]
> Azure veritabanı geçiş hizmeti 'ni kullanarak geçiş hakkında daha fazla bilgi için bkz. [veritabanı geçiş hizmeti kullanılarak SQL yönetilen örnek geçişi](../../dms/tutorial-sql-server-to-managed-instance.md).
> Çeşitli geçiş yöntemleri hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örneğine SQL Server geçişi](migrate-to-instance-from-sql-server.md).

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç:

- [Yönetilen örnek oluşturma](instance-create-quickstart.md) hızlı başlangıcı ' ndan kaynakları kullanır.
- Yüklü [SSMS](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) 'nin en son sürümünü gerektirir.
- SQL yönetilen örneğine bağlanmak için SSMS kullanılması gerekir. Nasıl bağlanadığına ilişkin şu hızlı başlangıçlara bakın:
  - SQL yönetilen örneği üzerinde [genel bir uç noktayı etkinleştirme](public-endpoint-configure.md) -Bu öğretici için önerilen yaklaşımdır.
  - [Azure VM 'den SQL yönetilen örneğine bağlanın](connect-vm-instance-configure.md).
  - [Şirket IÇINDEN SQL yönetilen örneği için Noktadan siteye bağlantı yapılandırın](point-to-site-p2s-configure.md).

> [!NOTE]
> Azure Blob depolama ve [paylaşılan erişim imzası (SAS) anahtarı](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)kullanarak bir SQL Server veritabanını yedekleme ve geri yükleme hakkında daha fazla bilgi için bkz. [SQL Server yedekleme URL 'si](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2017).

## <a name="restore-from-a-backup-file"></a>Bir yedekleme dosyasından geri yükleme

SQL Server Management Studio, geniş dünya Importers veritabanını SQL yönetilen örneğine geri yüklemek için aşağıdaki adımları izleyin. Veritabanı yedekleme dosyası önceden yapılandırılmış bir Azure Blob depolama hesabında depolanır.

1. SSMS 'yi açın ve yönetilen örneğinize bağlanın.
2. **Nesne Gezgini**, yönetilen örneğinize sağ tıklayıp yeni **sorgu** ' yı seçerek yeni bir sorgu penceresi açın.
3. Yönetilen örnekte [bir kimlik bilgisi oluşturmak](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) için önceden yapılandırılmış bir depolama HESABı ve SAS anahtarı kullanan aşağıdaki SQL betiğini çalıştırın.

   ```sql
   CREATE CREDENTIAL [https://mitutorials.blob.core.windows.net/databases]
   WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
   , SECRET = 'sv=2017-11-09&ss=bfqt&srt=sco&sp=rwdlacup&se=2028-09-06T02:52:55Z&st=2018-09-04T18:52:55Z&spr=https&sig=WOTiM%2FS4GVF%2FEEs9DGQR9Im0W%2BwndxW2CQ7%2B5fHd7Is%3D'
   ```

    ![kimlik bilgisi oluşturma](./media/restore-sample-database-quickstart/credential.png)

4. Kimlik bilgilerinizi denetlemek için, bir yedekleme dosyası listesi almak üzere bir [kapsayıcı](https://azure.microsoft.com/services/container-instances/) URL 'si kullanan aşağıdaki betiği çalıştırın.

   ```sql
   RESTORE FILELISTONLY FROM URL =
      'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![dosya listesi](./media/restore-sample-database-quickstart/file-list.png)

5. Wide World Importers veritabanını geri yüklemek için aşağıdaki betiği çalıştırın.

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![geri yükleme](./media/restore-sample-database-quickstart/restore.png)

6. Geri yüklemenin durumunu izlemek için aşağıdaki betiği çalıştırın.

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time
   FROM sys.dm_exec_requests r
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')
   ```

7. Geri yükleme tamamlandığında, veritabanını Nesne Gezgini görüntüleyin. Veritabanı geri yüklemenin, [sys. dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) görünümünü kullanarak tamamlandığını doğrulayabilirsiniz.

> [!NOTE]
> Bir veritabanı geri yükleme işlemi zaman uyumsuzdur ve yeniden denenebilir. Bağlantı kesmeleri veya zaman aşımı süresi dolarsa SQL Server Management Studio bir hata alabilirsiniz. Azure SQL veritabanı, arka planda veritabanını geri yüklemeye çalışmaya devam eder ve [sys. dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) ve [sys. dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) görünümlerini kullanarak geri yüklemenin ilerlemesini izleyebilirsiniz.
> Geri yükleme işleminin bazı aşamalarında, sistem görünümlerinde gerçek veritabanı adı yerine benzersiz bir tanımlayıcı görürsünüz. `RESTORE` [Burada](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#restore-statement)ekstre davranışı farklılıkları hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

- 5. adımda, bir veritabanı geri yükleme işlemi 22003 ileti KIMLIĞI ile sonlandıysa, yedekleme sağlama toplamlarını içeren yeni bir yedekleme dosyası oluşturun ve geri yüklemeyi yeniden gerçekleştirin. Bkz. [yedekleme veya geri yükleme sırasında yedekleme sağlaması sağlama veya devre dışı bırakma](https://docs.microsoft.com/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server).
- Bir URL 'de yedekleme sorunlarını gidermek için, bkz. [SQL Server yedekleme URL 'si en iyi uygulamalar ve sorun giderme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).
- Uygulama bağlantısı seçeneklerine genel bakış için bkz. [UYGULAMALARıNıZı SQL yönetilen örneğine bağlama](connect-application-instance.md).
- En sevdiğiniz araçları veya dilleri kullanarak sorgulamak için bkz. [hızlı başlangıç: Azure SQL veritabanı Connect ve Query](../database/connect-query-content-reference-guide.md).
