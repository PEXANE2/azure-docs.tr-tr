---
title: Azure Backup Sunucusu kullanarak SQL Server yedekleme
description: Bu makalede, Microsoft Azure Backup sunucusu (MABS) kullanarak SQL Server veritabanlarının yedeklenme yapılandırmasını öğrenin.
ms.topic: conceptual
ms.date: 03/24/2017
ms.openlocfilehash: 9cd6a8b76e4618031f4d21dc04a82a78fad0076d
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "82159259"
---
# <a name="back-up-sql-server-to-azure-by-using-azure-backup-server"></a>Azure Backup Sunucusu kullanarak SQL Server Azure 'a yedekleme

Bu makale, Microsoft Azure Backup sunucusu (MABS) kullanarak SQL Server veritabanlarının yedeklerini ayarlamanıza yardımcı olur.

SQL Server veritabanını yedeklemek ve Azure 'dan kurtarmak için:

1. Azure 'da SQL Server veritabanlarını korumak için bir yedekleme ilkesi oluşturun.
1. Azure 'da isteğe bağlı yedekleme kopyaları oluşturun.
1. Azure 'da veritabanını kurtarın.

## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce [Azure Backup sunucusu yüklediğinizden ve hazırlandığınızdan](backup-azure-microsoft-azure-backup.md)emin olun.

## <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Azure 'da SQL Server veritabanlarını korumak için önce bir yedekleme ilkesi oluşturun:

1. Azure Backup Sunucusu, **koruma** çalışma alanını seçin.
1. Koruma grubu oluşturmak için **Yeni** ' yi seçin.

    ![Azure Backup Sunucusu bir koruma grubu oluşturun](./media/backup-azure-backup-sql/protection-group.png)
1. Başlangıç sayfasında, bir koruma grubu oluşturma kılavuzunu gözden geçirin. Ardından **İleri**' yi seçin.
1. Koruma grubu türü için **sunucular**' ı seçin.

    ![Sunucular koruma grubu türünü seçin](./media/backup-azure-backup-sql/pg-servers.png)
1. Yedeklemek istediğiniz veritabanlarının bulunduğu SQL Server makineyi genişletin. Bu sunucudan yedeklenebileceği veri kaynaklarını görürsünüz. **Tüm SQL paylaşımlarını** genişletin ve ardından yedeklemek istediğiniz veritabanlarını seçin. Bu örnekte, ReportServer $ MSDPM2012 ve ReportServer $ MSDPM2012TempDB ' ı seçeceğiz. **İleri**’yi seçin.

    ![SQL Server veritabanı seçin](./media/backup-azure-backup-sql/pg-databases.png)
1. Koruma grubunu adlandırın ve **çevrimiçi koruma**istiyorum ' u seçin.

    ![Veri koruma yöntemi seçin-kısa vadeli disk koruması veya çevrimiçi Azure koruması](./media/backup-azure-backup-sql/pg-name.png)
1. **Kısa dönem hedeflerini belirtin** sayfasında, diske yedekleme noktaları oluşturmak için gerekli girişleri ekleyin.

    Bu örnekte, **bekletme aralığı** *5 güne*ayarlanır. Yedekleme **eşitleme sıklığı** her *15 dakikada*bir olarak ayarlanır. **Hızlı tam yedekleme** *8:00 PM*olarak ayarlanmıştır.

    ![Yedekleme koruması için kısa vadeli hedefler ayarlama](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > Bu örnekte, her gün 8:00 PM 'de bir yedekleme noktası oluşturulur. Önceki güne ait 8:00 PM yedekleme noktası aktarıldıktan sonra değiştirilen veriler. Bu işlem **Hızlı tam yedekleme**olarak adlandırılır. İşlem günlükleri 15 dakikada bir eşitlense de veritabanını 9:00 PM 'de kurtarmamız gerekiyorsa, bu örnekte bu durumda 8:00 PM olan son hızlı tam yedekleme noktasından Günlükler yeniden çalıştırılarak oluşturulur.
   >
   >

1. **İleri**’yi seçin. MABS, kullanılabilir genel depolama alanını gösterir. Ayrıca, olası disk alanı kullanımını da gösterir.

    ![MABS 'de disk ayırmayı ayarlama](./media/backup-azure-backup-sql/pg-storage.png)

    Varsayılan olarak, MABS veri kaynağı başına bir birim (SQL Server veritabanı) oluşturur. Birim, ilk yedekleme kopyası için kullanılır. Bu yapılandırmada, mantıksal disk Yöneticisi (LDM) MABS korumasını 300 veri kaynağı (SQL Server veritabanları) ile sınırlandırır. Bu kısıtlamayı geçici olarak çözmek için **DPM depolama havuzundaki verileri birlikte Konumlandır**' ı seçin. Bu seçeneği kullanırsanız, MABS birden çok veri kaynağı için tek bir birim kullanır. Bu kurulum, MABS 'in 2.000 SQL Server veritabanlarının korunmasını sağlar.

    **Birimleri otomatik olarak Büyüt**' i seçerseniz, mabs 'ler, üretim verileri büyüdükçe artan yedekleme birimi için hesap oluşturabilir. **Birimleri otomatik olarak Büyüt**' i seçmezseniz mabs, yedekleme depolama alanını koruma grubundaki veri kaynaklarıyla sınırlandırır.
1. Yöneticiyseniz, bu ilk yedeklemeyi **ağ üzerinden otomatik olarak** aktarmayı ve aktarım zamanını seçmenizi seçebilirsiniz. Ya da yedeklemeyi **el ile** aktarmayı seçebilirsiniz. Ardından **İleri**' yi seçin.

    ![MABS 'te çoğaltma oluşturma yöntemi seçme](./media/backup-azure-backup-sql/pg-manual.png)

    İlk yedekleme kopyası, tüm veri kaynağının (SQL Server veritabanı) aktarılmasını gerektirir. Yedekleme verileri üretim sunucusundan (SQL Server makineden) MABS 'ye gider. Bu yedekleme büyükse, verilerin ağ üzerinden aktarılması bant genişliği tıkanıklığı sağlayabilir. Bu nedenle, Yöneticiler ilk yedeklemenin **el ile**aktarılması için çıkarılabilir medya kullanmayı seçebilir. Ya da verileri belirli bir zamanda **ağ üzerinden otomatik olarak** aktarabilirler.

    İlk yedekleme tamamlandıktan sonra yedeklemeler ilk yedekleme kopyasında artımlı olarak devam eder. Artımlı yedeklemeler küçük olma eğilimindedir ve ağ üzerinden kolayca aktarılır.
1. Tutarlılık denetiminin ne zaman çalıştırılacağını seçin. Ardından **İleri**' yi seçin.

    ![Tutarlılık denetiminin ne zaman çalıştırılacağını seçin](./media/backup-azure-backup-sql/pg-consistent.png)

    MABS, yedekleme noktasının bütünlüğü üzerinde bir tutarlılık denetimi çalıştırabilir. Üretim sunucusundaki (Bu örnekteki SQL Server makine) yedekleme dosyasının sağlama toplamını ve bu dosya için MABS içindeki yedeklenen verileri hesaplar. Denetim bir çakışma bulursa, MABS içindeki yedeklenen dosyanın bozuk olduğu varsayılır. MABS, sağlama toplamı uyuşmazlığına karşılık gelen blokları göndererek yedeklenen verileri düzeltir. Tutarlılık denetimi performansı yoğun bir işlem olduğundan, Yöneticiler tutarlılık denetimini zamanlamayı veya otomatik olarak çalıştırmayı seçebilirler.
1. Azure 'da korunacak veri kaynaklarını seçin. Ardından **İleri**' yi seçin.

    ![Azure 'da korunacak veri kaynaklarını seçin](./media/backup-azure-backup-sql/pg-sqldatabases.png)
1. Yöneticiyseniz, kuruluşunuzun ilkelerine uygun olan yedekleme zamanlamaları ve bekletme ilkeleri ' ni seçebilirsiniz.

    ![Zamanlamaları ve bekletme ilkelerini seçin](./media/backup-azure-backup-sql/pg-schedule.png)

    Bu örnekte, yedeklemeler günlük olarak 12:00 PM ve 8:00 PM üzerinden alınır.

    > [!TIP]
    > Hızlı kurtarma için, diskinizde birkaç kısa süreli kurtarma noktası saklayın. Bu kurtarma noktaları, işletimsel kurtarma için kullanılır. Azure, daha yüksek SLA 'Lar ve garantili kullanılabilirlik sağlamak için iyi bir site dışı konum işlevi görür.
    >
    > Yerel Disk yedeklemeleri bittikten sonra Azure yedeklemeleri zamanlamak için Data Protection Manager (DPM) kullanın. Bu uygulamayı izlediğinizde, en son disk yedeklemesi Azure 'a kopyalanır.
    >

1. Bekletme ilkesi zamanlaması ' nı seçin. Saklama ilkesinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Backup kullanarak bant altyapınızı değiştirme](backup-azure-backup-cloud-as-tape.md).

    ![MABS 'te bir bekletme ilkesi seçin](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    Bu örnekte:

    * Yedeklemeler günlük olarak 12:00 PM ve 8:00 PM üzerinden alınır. Bunlar 180 gün boyunca tutulur.
    * 12:00 ' de Cumartesi Cumartesi günü 104 hafta boyunca saklanır.
    * 12:00 saat, ayın son Cumartesi günü 60 ay boyunca tutulur.
    * Mart 12:00 ' nin son Cumartesi günü 10 yıl süreyle tutulur.

    Bir bekletme ilkesi seçtikten sonra **İleri**' yi seçin.
1. İlk yedekleme kopyasının Azure 'a nasıl aktarılacağı seçin.

    * **Ağ üzerinden otomatik olarak** seçeneği, verileri Azure 'a aktarmak için yedekleme zamanlamanızı izler.
    * **Çevrimdışı yedekleme**hakkında daha fazla bilgi için bkz. [çevrimdışı yedeklemeye genel bakış](offline-backup-overview.md).

    Bir aktarım mekanizması seçtikten sonra **İleri**' yi seçin.
1. **Özet** sayfasında, ilke ayrıntılarını gözden geçirin. Ardından **Grup Oluştur**' u seçin. **Kapat** ' ı seçebilir ve **izleme** çalışma alanında işin ilerlemesini izleyebilirsiniz.

    ![Koruma grubu oluşturma işleminin ilerlemesi](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="create-on-demand-backup-copies-of-a-sql-server-database"></a>SQL Server veritabanının isteğe bağlı yedek kopyalarını oluşturma

İlk yedekleme gerçekleştiğinde bir kurtarma noktası oluşturulur. Zamanlamanın çalışmasını beklemek yerine, bir kurtarma noktasının oluşturulmasını el ile tetikleyebilirsiniz:

1. Koruma grubunda, veritabanının durumunun **Tamam**olduğundan emin olun.

    ![Veritabanı durumunu gösteren bir koruma grubu](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
1. Veritabanına sağ tıklayın ve ardından **Kurtarma noktası oluştur**' u seçin.

    ![Çevrimiçi kurtarma noktası oluşturmayı seçin](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
1. Açılan menüde **çevrimiçi koruma**' yı seçin. Ardından, Azure 'da bir kurtarma noktası oluşturmayı başlatmak için **Tamam** ' ı seçin.

    ![Azure 'da bir kurtarma noktası oluşturmaya başlayın](./media/backup-azure-backup-sql/sqlbackup-azure.png)
1. İş ilerleme durumunu **izleme** çalışma alanında görüntüleyebilirsiniz.

    ![Izleme konsolunda iş ilerlemesini görüntüleme](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Azure 'dan SQL Server veritabanını kurtarma

Azure 'dan SQL Server veritabanı gibi korumalı bir varlığı kurtarmak için:

1. DPM sunucusu yönetim konsolunu açın. DPM 'nin yedeklediği sunucuları görmek için **Kurtarma** çalışma alanına gidin. Veritabanını seçin (Bu örnekte ReportServer $ MSDPM2012). **Çevrimiçi**ile biten bir **Kurtarma süresi** seçin.

    ![Bir kurtarma noktası seçme](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
1. Veritabanı adına sağ tıklayın ve **kurtar**' ı seçin.

    ![Azure 'dan bir veritabanını kurtarma](./media/backup-azure-backup-sql/sqlbackup-recover.png)
1. DPM, kurtarma noktasının ayrıntılarını gösterir. **İleri**’yi seçin. Veritabanının üzerine yazmak için, **SQL Server özgün örneğine kurtar**kurtarma türünü seçin. Ardından **İleri**' yi seçin.

    ![Veritabanını özgün konumuna kurtar](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    Bu örnekte DPM, veritabanının başka bir SQL Server örneğine veya tek başına bir ağ klasörüne kurtarılmasını sağlar.
1. **Kurtarma seçeneklerini belirtin** sayfasında kurtarma seçeneklerini belirleyebilirsiniz. Örneğin, kurtarmanın kullandığı bant genişliğini azaltmak için **ağ bant genişliği kullanımını azaltmayı** seçebilirsiniz. Ardından **İleri**' yi seçin.
1. **Özet** sayfasında, geçerli kurtarma yapılandırmasını görürsünüz. **Kurtar**' ı seçin.

    Kurtarma durumu kurtarılan veritabanını gösterir. Sihirbazı kapatmak için **Kapat** ' ı seçebilirsiniz ve ilerlemeyi **izleme** çalışma alanında görüntüleyebilirsiniz.

    ![Kurtarma işlemini Başlat](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Kurtarma tamamlandığında, geri yüklenen veritabanı uygulamayla tutarlıdır.

### <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure Backup SSS](backup-azure-backup-faq.md).
