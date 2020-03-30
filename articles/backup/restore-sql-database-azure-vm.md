---
title: Sql Server veritabanlarını Azure VM'de geri yükleme
description: Bu makalede, Azure VM'de çalışan ve Azure Yedekleme ile yedeklenen SQL Server veritabanlarının nasıl geri yüklenilen açıklanmaktadır.
ms.topic: conceptual
ms.date: 05/22/2019
ms.openlocfilehash: 642476c98ca223da01bda5c6eb79ee9b53732468
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252460"
---
# <a name="restore-sql-server-databases-on-azure-vms"></a>Azure VM'lerde SQL Server veritabanlarını geri yükleme

Bu makalede, [Azure Yedekleme](backup-overview.md) hizmetinin Azure Yedekleme Kurtarma Hizmetleri kasasına yedeklediği bir Azure sanal makinesinde (VM) çalışan bir SQL Server veritabanının nasıl geri yüklenilen açıklanmaktadır.

Bu makalede, SQL Server veritabanlarının nasıl geri yüklenir. Daha fazla bilgi için Azure [VM'lerde SQL Server veritabanlarını yedekleyin.](backup-azure-sql-database.md)

## <a name="restore-to-a-time-or-a-recovery-point"></a>Bir zamana veya kurtarma noktasına geri yükleme

Azure Yedekleme, Azure VM'lerinde çalışan SQL Server veritabanlarını aşağıdaki gibi geri yükleyebilir:

- İşlem günlüğü yedeklemelerini kullanarak belirli bir tarihe veya saate (ikinciye) geri yükleyin. Azure Yedekleme, uygun tam diferansiyel yedeklemeyi ve seçilen zamana bağlı olarak geri yüklemesi gereken günlük yedeklemeleri zincirini otomatik olarak belirler.
- Belirli bir kurtarma noktasına geri yüklemek için belirli bir tam veya diferansiyel yedekleme geri yükleyin.

## <a name="prerequisites"></a>Ön koşullar

Bir veritabanını geri yüklemeden önce aşağıdakileri unutmayın:

- Veritabanını aynı Azure bölgesinde bir SQL Server örneğine geri yükleyebilirsiniz.
- Hedef sunucu, kaynakla aynı kasaya kaydedilmelidir.
- TDE şifreli bir veritabanını başka bir SQL Server'a geri yüklemek için [önce sertifikayı hedef sunucuya geri yüklemeniz](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017)gerekir.
- "Ana" veritabanını geri yüklemeden önce, başlangıç seçeneği **-m AzureWorkloadBackup'ı**kullanarak SQL Server örneğini tek kullanıcı modunda başlatın.
  - **-m** değeri istemcinin adıdır.
  - Bağlantıyı yalnızca belirtilen istemci adı açabilir.
- Tüm sistem veritabanları (model, master, msdb) için, geri yüklemeyi tetiklemeden önce SQL Server Agent hizmetini durdurun.
- Bu veritabanlarından herhangi biri için bağlantı almaya çalışabilecek uygulamaları kapatın.
- Bir sunucuda çalışan birden çok örneğiniz varsa, tüm örneklerin çalışır durumda olması gerekir, aksi takdirde sunucu veritabanını geri yüklemeniz için hedef sunucular listesinde görünmez.

## <a name="restore-a-database"></a>Veritabanını geri yükleme

Geri yüklemek için aşağıdaki izinlere ihtiyacınız vardır:

- **Yedekleme Operatörü** geri yükleme yaptığınız kasada izin verir.
- **Katılımcı (yazma)** yedeklenen kaynak VM'ye erişim.
- **Katılımcı (yazma)** hedef VM erişimi:
  - Aynı VM'ye geri geri biniyorsanız, kaynak VM budur.
  - Alternatif bir konuma geri darıyorsanız, bu yeni hedef VM'dir.

Aşağıdaki gibi geri yükleyin:

1. SQL Server VM'nin kayıtlı olduğu kasayı açın.
2. Tonoz panosunda, **Kullanım**altında **Yedek Öğeler'i**seçin.
3. **Yedekleme Öğeleri'nde**, **Yedekleme Yönetim Türü**altında, Azure **VM'de SQL'i**seçin.

    ![Azure VM'de SQL'i seçin](./media/backup-azure-sql-database/sql-restore-backup-items.png)

4. Geri yüklemek için veritabanını seçin.

    ![Geri yüklemek için veritabanını seçin](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

5. Veritabanı menüsünü gözden geçirin. Veritabanı yedeklemesi hakkında aşağıdakiler de dahil olmak üzere bilgi sağlar:

    - En eski ve en son geri yükleme noktaları.
    - Tam ve toplu günlüğe kaydedilmiş kurtarma modunda olan ve işlem günlüğü yedeklemeleri için yapılandırılan veritabanları için son 24 saate ait günlük yedekleme durumu.

6. **Geri Yükle'yi**seçin.

    ![Geri Yükle'yi Seçin](./media/backup-azure-sql-database/restore-db.png)

7. **Yapılandırmayı Geri**Yükle'de, verileri nerede (veya nasıl) geri yükleyebileceğinizi belirtin:
   - **Alternatif Konum**: Veritabanını alternatif bir konuma geri yükleyin ve özgün kaynak veritabanını tutun.
   - **Overwrite DB**: Verileri orijinal kaynakla aynı SQL Server örneğine geri yükleyin. Bu seçenek özgün veritabanının üzerine yazar.

    > [!IMPORTANT]
    > Seçili veritabanı Her Zaman Kullanılabilirlik grubuna aitse, SQL Server veritabanının üzerine yazılmasına izin vermez. Yalnızca **Alternatif Konum** kullanılabilir.
    >
   - **Dosya olarak geri yükleme**: Veritabanı olarak geri yüklemek yerine, sql server management studio kullanarak dosyaların bulunduğu herhangi bir makinede daha sonra veritabanı olarak kurtarılabilen yedekleme dosyalarını geri yükleyin.
     ![Yapılandırma menüsünü geri yükle](./media/backup-azure-sql-database/restore-configuration.png)

### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

1. **Yapılandırmayı Geri Yükle** menüsünde, **Geri Yüklenilen Yer'in** **altında, Alternatif Konum'u**seçin.
2. Veritabanını geri yüklemek istediğiniz SQL Server adını ve örneğini seçin.
3. Geri **yüklenen DB Adı** kutusuna hedef veritabanının adını girin.
4. Varsa, **seçili SQL örneğinde aynı ada sahip DB zaten varsa Overwrite'ı**seçin.
5. **Tamam'ı**seçin.

    ![Yapılandırmayı Geri Yükle menüsü için değerler sağlama](./media/backup-azure-sql-database/restore-configuration.png)

6. **Geri Yükleme noktasını seç'te,** zaman içinde belirli bir noktaya geri mi yoksa belirli [bir](#restore-to-a-specific-point-in-time) [kurtarma noktasına geri](#restore-to-a-specific-restore-point)yüklemek mi gerektiğini seçin.

    > [!NOTE]
    > Zamanında geri yükleme, yalnızca tam ve toplu günlüğe kaydedilmiş kurtarma modunda olan veritabanları için günlük yedeklemeleri için kullanılabilir.

### <a name="restore-and-overwrite"></a>Geri yükleme ve üzerine yazma

1. **Yapılandırmayı Geri Yükle** menüsünde, **Geri Yüklenilen Yer'in**altında, DB > **Ok'u** **Üzerine Yaz'ı**seçin.

    ![DB Üzerine Yaz'ı Seçin](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

2. **Geri Yükleme noktasını seç'te,** belirli bir noktaya zaman içinde geri yüklemek için **Günlükleri (Point in Time)** seçeneğini [belirleyin.](#restore-to-a-specific-point-in-time) Veya belirli bir [kurtarma noktasına](#restore-to-a-specific-restore-point)geri yüklemek için Tam **& Diferansiyel'i** seçin.

    > [!NOTE]
    > Zamanında geri yükleme, yalnızca tam ve toplu günlüğe kaydedilmiş kurtarma modunda olan veritabanları için günlük yedeklemeleri için kullanılabilir.

### <a name="restore-as-files"></a>Dosya olarak geri yükleme

Yedekleme verilerini veritabanı yerine .bak dosyaları olarak geri yüklemek için **Dosyalar Olarak Geri Yükle'yi**seçin. Dosyalar belirli bir yola atıldıktan sonra, bu dosyaları veritabanı olarak geri yüklemek istediğiniz herhangi bir makineye götürebilirsiniz. Bu dosyaları herhangi bir makineye taşıyabilme sayesinde, artık verileri abonelikler ve bölgeler arasında geri yükleyebilirsiniz.

1. **Yapılandırmayı Geri Yükle** menüsünde, **Geri Yüklenilen Yerler'in** **altında, dosya olarak geri yükle'yi**seçin.
2. Yedekleme dosyalarını geri yüklemek istediğiniz SQL Server adını seçin.
3. **Sunucudaki Hedef yolunda,** adım 2'de seçilen sunucudaki klasör yolunu girdi. Bu, hizmetin gerekli tüm yedekleme dosyalarını atacağı yerdir. Genellikle, hedef yol olarak belirtildiğinde bir ağ paylaşım yolu veya atlı Azure dosya paylaşımı yolu, bu dosyalara aynı ağdaki diğer makineler veya üzerlerine monte edilmiş aynı Azure dosya paylaşımıyla daha kolay erişilmesini sağlar.<BR>

    >Hedef kayıtlı VM'ye monte edilmiş bir Azure Dosya Paylaşımı'ndaki veritabanı yedekleme dosyalarını geri yüklemek için NT AUTHORITY\SYSTEM'in dosya paylaşımına erişebilmesini unutmayın. VM'ye monte edilen AFS'ye okuma/yazma izinlerini vermek için aşağıda verilen adımları gerçekleştirebilirsiniz:
    >
    >- NT AUTHORITY\SYSTEM kabuğuna girmek için çalıştırın `PsExec -s cmd`
    >   - `cmdkey /add:<storageacct>.file.core.windows.net /user:AZURE\<storageacct> /pass:<storagekey>` yürütme
    >   - Erişimi doğrula`dir \\<storageacct>.file.core.windows.net\<filesharename>`
    >- Yedekleme Kasasından yol `\\<storageacct>.file.core.windows.net\<filesharename>` olarak dosya olarak geri yükleme başlatın<BR>
    Psexec'i<https://docs.microsoft.com/sysinternals/downloads/psexec>

4. **Tamam'ı**seçin.

    ![Dosya Olarak Geri Yükle'yi seçin](./media/backup-azure-sql-database/restore-as-files.png)

5. Kullanılabilir tüm .bak dosyalarının geri yüklendiği **Geri Yükleme Noktasını** seçin.

    ![Geri Yükleme Noktası'nı seçin](./media/backup-azure-sql-database/restore-point.png)

6. Seçili kurtarma noktasıyla ilişkili tüm yedekleme dosyaları hedef yola atılır. SQL Server Management Studio'yu kullanarak dosyaları mevcut olan herhangi bir makinede veritabanı olarak geri yükleyebilirsiniz.

    ![Hedef Yolunda Geri Yüklenen Yedekleme Dosyaları](./media/backup-azure-sql-database/sql-backup-files.png)

### <a name="restore-to-a-specific-point-in-time"></a>Zaman içinde belirli bir noktaya geri yükleme

Geri yükleme türü olarak **Günlükleri (Point in Time)** seçtiyseniz, aşağıdakileri yapın:

1. **Tarihi/Saati Geri Yükle**altında, takvimi açın. Takvimde, kurtarma noktaları olan tarihler kalın yazıyla görüntülenir ve geçerli tarih vurgulanır.
1. Kurtarma noktaları olan bir tarih seçin. Kurtarma noktası olmayan tarihleri seçemezsiniz.

    ![Takvimi açma](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

1. Bir tarih seçtikten sonra, zaman çizelgesi grafiği kullanılabilir kurtarma noktalarını sürekli aralıkta görüntüler.
1. Zaman çizelgesi grafiğinde kurtarma zamanı belirtin veya bir saat seçin. Sonra **Tamam**’ı seçin.

    ![Geri yükleme süresi seçin](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

1. Gelişmiş **Yapılandırma** menüsünde, veritabanını geri yüklemeden sonra çalışır durumda tutmak istiyorsanız, **NORECOVERY ile Geri Yükle'yi**etkinleştirin.
1. Hedef sunucudaki geri yükleme konumunu değiştirmek istiyorsanız, yeni bir hedef yolu girin.
1. **Tamam'ı**seçin.

    ![Gelişmiş yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

1. Geri **Yükle** menüsünde, geri yükleme işini başlatmak için **Geri Yükle'yi** seçin.
1. **Bildirimler** alanındaki geri yükleme ilerlemesini izleyin veya veritabanı menüsünde **işleri geri yükle'yi** seçerek izleyin.

    ![İş ilerlemesini geri yükleme](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-to-a-specific-restore-point"></a>Belirli bir geri yükleme noktasına geri yükleme

Geri yükleme türü olarak **Tam & Diferansiyel'i** seçtiyseniz, aşağıdakileri yapın:

1. Listeden bir kurtarma noktası seçin ve geri yükleme noktası yordamını tamamlamak için **Tamam'ı** seçin.

    ![Tam bir kurtarma noktası seçin](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

    >[!NOTE]
    > Varsayılan olarak, son 30 gündeki kurtarma noktaları görüntülenir. **Filtre'yi** tıklatıp özel bir aralık seçerek 30 günden eski kurtarma noktalarını görüntüleyebilirsiniz.

1. Gelişmiş **Yapılandırma** menüsünde, veritabanını geri yüklemeden sonra çalışır durumda tutmak istiyorsanız, **NORECOVERY ile Geri Yükle'yi**etkinleştirin.
1. Hedef sunucudaki geri yükleme konumunu değiştirmek istiyorsanız, yeni bir hedef yolu girin.
1. **Tamam'ı**seçin.

    ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

1. Geri **Yükle** menüsünde, geri yükleme işini başlatmak için **Geri Yükle'yi** seçin.
1. **Bildirimler** alanındaki geri yükleme ilerlemesini izleyin veya veritabanı menüsünde **işleri geri yükle'yi** seçerek izleyin.

    ![İş ilerlemesini geri yükleme](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-databases-with-large-number-of-files"></a>Çok sayıda dosya içeren veritabanlarını geri yükleme

Bir veritabanındaki dosyaların toplam dize boyutu belirli bir [sınırdan](backup-sql-server-azure-troubleshoot.md#size-limit-for-files)büyükse, Azure Yedekleme veritabanı dosyalarının listesini farklı bir çukur bileşeninde saklar ve geri yükleme işlemi sırasında hedef geri yükleme yolunu ayarlayamayacaktır. Dosyalar bunun yerine SQL varsayılan yoluna geri yüklenir.

  ![Büyük dosyayla Veritabanını Geri Yükle](./media/backup-azure-sql-database/restore-large-files.jpg)

## <a name="next-steps"></a>Sonraki adımlar

[Yönetme ve izleme](manage-monitor-sql-database-backup.md) Azure Yedekleme tarafından yedeklenen SQL Server veritabanları.
