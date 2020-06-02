---
title: 'Hızlı başlangıç: bir yedeklemeyi geri yükleme (SSMS)'
titleSuffix: Azure SQL SQL Managed Instance
description: Bu hızlı başlangıçta, bir veritabanı yedeklemesini SQL Server Management (SSMS) kullanarak Azure SQL SQL yönetilen örneğine geri yüklemeyi öğrenin.
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
ms.openlocfilehash: c750912e942d5dadeb97e6675427f1730912704a
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267588"
---
# <a name="quickstart-restore-a-database-to-an-azure-sql-managed-instance-with-ssms"></a>Hızlı başlangıç: SSMS ile bir veritabanını Azure SQL yönetilen örneğine geri yükleme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu hızlı başlangıçta, Azure Blob depolama 'dan [Azure SQL yönetilen örneğine](sql-managed-instance-paas-overview.md)bir veritabanını (geniş dünya Importers-standart yedekleme dosyası) geri yüklemek için SQL Server Management Studio (SSMS) kullanacaksınız.

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

> [!NOTE]
> Azure veritabanı geçiş hizmeti 'ni (DMS) kullanarak geçiş hakkında daha fazla bilgi için bkz. [DMS kullanarak SQL yönetilen örnek geçişi](../../dms/tutorial-sql-server-to-managed-instance.md).
> Çeşitli geçiş yöntemleri hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örneğine SQL Server geçişi](migrate-to-instance-from-sql-server.md).

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç:

- [SQL yönetilen örnek oluşturma](instance-create-quickstart.md) hızlı başlangıcı ' ndan kaynakları kullanır.
- [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) uygulamasının en son sürümünü gerektirir.
- SQL yönetilen örneğinize bağlanmak için SSMS kullanılması gerekir. Nasıl bağlanadığına ilişkin şu hızlı başlangıçlara bakın:
  - SQL yönetilen örneği üzerinde [genel uç noktayı etkinleştir](public-endpoint-configure.md) -Bu öğretici için önerilen yaklaşımdır.
  - [Azure VM 'den bir SQL yönetilen örneğine bağlanma](connect-vm-instance-configure.md)
  - [Şirket içinden BIR SQL yönetilen örneğine Noktadan siteye bağlantı yapılandırın](point-to-site-p2s-configure.md).

> [!NOTE]
> Azure Blob depolama ve [paylaşılan erişim imzası (SAS) anahtarı](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)kullanarak bir SQL Server veritabanını yedekleme ve geri yükleme hakkında daha fazla bilgi için bkz. [SQL Server yedekleme URL 'si](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2017).

## <a name="restore-from-a-backup-file"></a>Bir yedekleme dosyasından geri yükleme

SQL Server Management Studio (SSMS) içinde, Wide World Importers veritabanını SQL yönetilen örneğinize geri yüklemek için aşağıdaki adımları izleyin. Veritabanı yedekleme dosyası önceden yapılandırılmış bir Azure Blob depolama hesabında depolanır.

1. SSMS 'yi açın ve SQL yönetilen örneğinizi bağlayın.
2. **Nesne Gezgini**, SQL yönetilen örneğinizi sağ tıklayıp yeni **sorgu** ' yı seçerek yeni bir sorgu penceresi açın.
3. SQL yönetilen Örneğinizde [kimlik bilgileri oluşturmak](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) için önceden yapılandırılmış bir depolama HESABı ve SAS anahtarı kullanan aşağıdaki SQL betiğini çalıştırın.

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

6. Geri yüklemeniz durumunda olduğunuzu izlemek için aşağıdaki betiği çalıştırın.

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time
   FROM sys.dm_exec_requests r
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')
   ```

7. Geri yükleme tamamlandığında, veritabanını Nesne Gezgini görüntüleyin. Veritabanı geri yüklemenin, [sys. dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) View kullanarak tamamlandığını doğrulayabilirsiniz.

> [!NOTE]
> Veritabanı geri yükleme işlemi zaman uyumsuzdur ve yeniden denenebilir. Bağlantı kesildiyse veya bir zaman aşımı süresi dolarsa SQL Server Management Studio bir hata alabilirsiniz. Azure SQL veritabanı, arka planda veritabanını geri yüklemeye çalışmaya devam eder ve [sys. dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) ve [sys. dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) görünümlerini kullanarak geri yüklemenin ilerlemesini izleyebilirsiniz.
> Geri yükleme işleminin bazı aşamalarında, sistem görünümlerinde gerçek veritabanı adı yerine benzersiz tanımlayıcı görürsünüz. `RESTORE` [Burada](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#restore-statement)ekstre davranışı farklılıkları hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

- 5. adımda, bir veritabanı geri yükleme işlemi 22003 ileti KIMLIĞI ile sonlandıysa, yedekleme sağlama toplamlarını içeren yeni bir yedekleme dosyası oluşturun ve geri yüklemeyi yeniden gerçekleştirin. Bkz. [yedekleme veya geri yükleme sırasında yedekleme sağlaması sağlama veya devre dışı bırakma](https://docs.microsoft.com/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server).
- Bir URL 'de yedekleme sorunlarını gidermek için, bkz. [SQL Server yedekleme URL 'Si En Iyi uygulamalar ve sorun giderme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).
- Uygulama bağlantısı seçeneklerine genel bakış için bkz. [UYGULAMALARıNıZı SQL yönetilen örneğine bağlama](connect-application-instance.md).
- En sevdiğiniz araçları veya dilleri kullanarak sorgulamak için bkz. [hızlı başlangıç: Azure SQL veritabanı Connect ve Query](../database/connect-query-content-reference-guide.md).
