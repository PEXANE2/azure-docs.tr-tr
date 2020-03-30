---
title: DPM iş yükü olarak SQL Server'ı Azure'a yedekleme
description: Azure Yedekleme hizmetini kullanarak SQL Server veritabanlarını yedeklemeye giriş
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 8cbb8c833bc2933afac300bcc848fd50861011d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77505924"
---
# <a name="back-up-sql-server-to-azure-as-a-dpm-workload"></a>DPM iş yükü olarak SQL Server'ı Azure'a yedekleme

Bu makale, Azure Yedekleme'yi kullanarak SQL Server veritabanlarını yedeklemek için yapılandırma adımlarında size yol açar.

SQL Server veritabanlarını Azure'a yedeklemek için bir Azure hesabınız gerekir. Hesabınız yoksa, birkaç dakika içinde ücretsiz bir hesap oluşturabilirsiniz. Daha fazla bilgi için [bkz.](https://azure.microsoft.com/pricing/free-trial/)

BIR SQL Server veritabanını Azure'a yedeklemek ve Azure'dan kurtarmak için:

1. Azure'daki SQL Server veritabanlarını korumak için bir yedekleme ilkesi oluşturun.
1. Azure'da isteğe bağlı yedekleme kopyaları oluşturun.
1. Veritabanını Azure'dan kurtarın.

## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce, iş yüklerini korumak için Azure Yedekleme'yi kullanmanın [ön koşullarıyla](backup-azure-dpm-introduction.md#prerequisites-and-limitations) karşılaştığınıza emin olun. Ön koşul görevlerinden bazıları şunlardır: 
* Yedek kasa oluşturun.
* Kasa kimlik bilgilerini indirin. 
* Azure Yedekleme aracısını yükleyin.
* Sunucuyu kasaya kaydedin.

## <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma 

Azure'daki SQL Server veritabanlarını korumak için önce bir yedekleme ilkesi oluşturun:

1. Veri Koruma Yöneticisi (DPM) **sunucusunda, Koruma** çalışma alanını seçin.
1. Koruma grubu oluşturmak için **Yeni'yi** seçin.

    ![Koruma grubu oluşturma](./media/backup-azure-backup-sql/protection-group.png)
1. Başlangıç sayfasında, bir koruma grubu oluşturma yla ilgili kılavuzu gözden geçirin. Ardından **İleri'yi**seçin.
1. **Sunucuları**seçin.

    ![Sunucular koruma grubu türünü seçin](./media/backup-azure-backup-sql/pg-servers.png)
1. Yedeklemek istediğiniz veritabanlarının bulunduğu SQL Server makinesini genişletin. Bu sunucudan yedeklenebilen veri kaynaklarını görürsünüz. **Tüm SQL Paylaşımlarını** genişletin ve yedeklemek istediğiniz veritabanlarını seçin. Bu örnekte ReportServer$MSDPM2012 ve ReportServer$MSDPM2012TempDB'yi seçiyoruz. Ardından **İleri'yi**seçin.

    ![SQL Server veritabanı seçin](./media/backup-azure-backup-sql/pg-databases.png)
1. Koruma grubunu adlandırın ve sonra **çevrimiçi koruma istiyorum**seçin.

    ![Bir veri koruma yöntemi seçin - kısa süreli disk koruması veya çevrimiçi Azure koruması](./media/backup-azure-backup-sql/pg-name.png)
1. Kısa **Vadeli Hedefler belirt** sayfasında, diske yedekleme noktaları oluşturmak için gerekli girişleri ekleyin.

    Bu örnekte, **Bekletme aralığı** *5 gün*olarak ayarlanır. Yedekleme **Eşitleme frekansı** her *15 dakikada*bir olarak ayarlanır. **Express Full Backup** *20:00*olarak ayarlanır.

    ![Yedekleme koruması için kısa vadeli hedefler ayarlama](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > Bu örnekte, her gün saat 20:00'de bir yedekleme noktası oluşturulur. Önceki günün 20:00 yedekleme noktasından bu yana değiştirilen veriler aktarılır. Bu işlem **Express Full Backup**olarak adlandırılır. İşlem günlükleri her 15 dakikada bir eşitlenmiş olsa da, veritabanını 21:00'de kurtarmamız gerekirse, bu örnekte saat 20:00 olan son ekspres tam yedekleme noktasından günlükleri yeniden oynatılarak nokta oluşturulur.
   >
   >

1. **Sonraki'ni**seçin. DPM, kullanılabilir genel depolama alanını gösterir. Ayrıca potansiyel disk alanı kullanımını gösterir.

    ![Disk ayırmayı ayarlama](./media/backup-azure-backup-sql/pg-storage.png)

    Varsayılan olarak, DPM veri kaynağı başına bir birim (SQL Server veritabanı) oluşturur. Birim ilk yedekleme kopyası için kullanılır. Bu yapılandırmada, Mantıksal Disk Yöneticisi (LDM) DPM korumasını 300 veri kaynağıyla (SQL Server veritabanları) sınırlar. Bu sınırlamayı aşmak için **DPM Depolama Havuzu'nda Verileri Birlikte**Bul'u'yu seçin. Bu seçeneği kullanırsanız, DPM birden çok veri kaynağı için tek bir birim kullanır. Bu kurulum, DPM'nin 2.000'e kadar SQL Server veritabanını korumasına olanak tanır.

    **Birimleri otomatik olarak büyütürseniz,** üretim verileri büyüdükçe DPM artan yedekleme hacmini hesaplayabilir. Birimleri otomatik olarak **büyütmeyi**seçmezseniz, DPM yedekleme depolama alanını koruma grubundaki veri kaynaklarıyla sınırlar.

1. Yöneticiyseniz, bu ilk yedeklemeyi otomatik olarak **ağ üzerinden** aktarmayı ve aktarım saatini seçebilirsiniz. Veya yedeklemeyi **el ile** aktarmayı seçin. Ardından **İleri'yi**seçin.

    ![Çoğaltma oluşturma yöntemini seçin](./media/backup-azure-backup-sql/pg-manual.png)

    İlk yedekleme kopyası tüm veri kaynağının (SQL Server veritabanı) aktarılmasını gerektirir. Yedekleme verileri üretim sunucusundan (SQL Server makinesi) DPM sunucusuna taşınır. Bu yedekleme büyükse, verileri ağ üzerinden aktarmak bant genişliği nde tıkanıklık yaratabilir. Bu nedenle, yöneticiler ilk yedeklemeyi **el ile**aktarmak için çıkarılabilir ortam kullanmayı seçebilirler. Veya verileri belirli bir zamanda **ağ üzerinden otomatik olarak** aktarabilirler.

    İlk yedekleme bittikten sonra, yedeklemeler ilk yedekleme kopyasında artımlı olarak devam eder. Artımlı yedeklemeler küçük olma eğilimindedir ve ağ üzerinden kolayca aktarılır.
    
1. Tutarlılık denetiminin ne zaman çalıştırıleceğini zindan edin. Ardından **İleri'yi**seçin.

    ![Tutarlılık denetimini ne zaman çalıştırılacaklarını seçin](./media/backup-azure-backup-sql/pg-consistent.png)

    DPM, yedekleme noktasının bütünlüğü üzerinde tutarlılık denetimi çalıştırabilir. Üretim sunucusundaki yedekleme dosyasının denetimini (bu örnekteki SQL Server makinesi) ve DPM'deki dosyaiçin yedeklenmiş verileri hesaplar. Denetim çakışma bulursa, DPM'de yedeklenen dosyanın bozuk olduğu varsayılır. DPM, checksum uyuşmazlığına karşılık gelen blokları göndererek yedeklenen verileri düzeltir. Tutarlılık denetimi performans yoğun bir işlem olduğundan, yöneticiler tutarlılık denetimini zamanlamayı veya otomatik olarak çalıştırmayı seçebilir.

1. Azure'da korunmak için veri kaynaklarını seçin. Ardından **İleri'yi**seçin.

    ![Azure'da korunmak için veri kaynaklarını seçin](./media/backup-azure-backup-sql/pg-sqldatabases.png)
1. Yöneticiyseniz, kuruluşunuzun ilkelerine uygun yedekleme zamanlamaları ve bekletme ilkeleri seçebilirsiniz.

    ![Zamanlamaları ve bekletme ilkelerini seçme](./media/backup-azure-backup-sql/pg-schedule.png)

    Bu örnekte, yedeklemeler her gün 12:00 ve 20:00'de alınır.

    > [!TIP]
    > Hızlı kurtarma için diskinizde birkaç kısa vadeli kurtarma noktası tutun. Bu kurtarma noktaları operasyonel kurtarma için kullanılır. Azure, daha yüksek SLA'lar ve garantili kullanılabilirlik sağlayarak iyi bir iş yeri konumu olarak hizmet vermektedir.
    >
    > Yerel disk yedeklemeleri bittikten sonra Azure Yedeklemelerini zamanlamak için DPM'yi kullanın. Bu uygulamayı izlediğinizde, en son disk yedeklemesi Azure'a kopyalanır.
    >

1. Bekletme ilkesi zamanlamasını seçin. Bekletme ilkesinin nasıl çalıştığı hakkında daha fazla bilgi için [teyp altyapınızı değiştirmek için Azure Yedeklemesini Kullanın'a](backup-azure-backup-cloud-as-tape.md)bakın.

    ![Bekletme ilkesi seçin](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    Bu örnekte:

    * Yedekler her gün 12:00 ve 20:00'de alınır. 180 gün saklanır.
    * Cumartesi günü saat 12:00'de yedek 104 hafta saklanır.
    * Ayın son Cumartesi günü saat 12:00'deki yedeklemesi 60 ay boyunca saklanır.
    * Mart'ın son Cumartesi günü saat 12:00'deki yedek 10 yıl saklanır.
    
    Bekletme ilkesini seçtikten sonra **İleri'yi**seçin.

1. İlk yedekleme kopyasını Azure'a nasıl aktarırılabildiğini seçin.

    * **Ağ üzerinden Otomatikle** seçeneği, verileri Azure'a aktarmak için yedekleme zamanlamanızı izler.
    * **Çevrimdışı Yedekleme**hakkında daha fazla bilgi için çevrimdışı [yedeklemeye genel bakış](offline-backup-overview.md)bölümüne bakın.

    Aktarım mekanizmasını seçtikten sonra **İleri'yi**seçin.

1. **Özet** sayfasında, ilke ayrıntılarını gözden geçirin. Ardından **Grup Oluştur'u**seçin. **Kapat'ı** seçebilir ve **İzleme** çalışma alanında iş ilerlemesini izleyebilirsiniz.

    ![Koruma grubu oluşturmanın ilerlemesi](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="create-on-demand-backup-copies-of-a-sql-server-database"></a>SQL Server veritabanının isteğe bağlı yedekleme kopyalarını oluşturma

İlk yedekleme gerçekleştiğinde bir kurtarma noktası oluşturulur. Zamanlamanın çalışmasını beklemek yerine, bir kurtarma noktasının oluşturulmasını el ile tetikleyebilirsiniz:

1. Koruma grubunda, veritabanı durumunun **iyi**olduğundan emin olun.

    ![Veritabanı durumunu gösteren bir koruma grubu](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
1. Veritabanına sağ tıklayın ve ardından **kurtarma noktasını oluştur'u**seçin.

    ![Çevrimiçi kurtarma noktası oluşturmayı seçin](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
1. Açılan menüde **Çevrimiçi korumayı**seçin. Ardından Azure'da bir kurtarma noktası oluşturmaya başlamak için **Tamam'ı** seçin.

    ![Azure'da bir kurtarma noktası oluşturmaya başlayın](./media/backup-azure-backup-sql/sqlbackup-azure.png)
1. **İzleme** çalışma alanında iş ilerlemesini görüntüleyebilirsiniz.

    ![İzleme konsolunda iş ilerlemesini görüntüleme](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Azure'dan bir SQL Server veritabanı kurtarma

SQL Server veritabanı gibi korumalı bir varlığı Azure'dan kurtarmak için:

1. DPM sunucu yönetim konsolu açın. DPM'nin yedeklemesi sunucularını görmek için **Kurtarma** çalışma alanına gidin. Veritabanını seçin (bu örnekte, ReportServer$MSDPM2012). **Çevrimiçi**ile biten bir **Kurtarma süresi** seçin.

    ![Bir kurtarma noktası seçme](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
1. Veritabanı adını sağ tıklatın ve **Kurtar'ı**seçin.

    ![Azure'dan veritabanı kurtarma](./media/backup-azure-backup-sql/sqlbackup-recover.png)
1. DPM kurtarma noktasının ayrıntılarını gösterir. **Sonraki'ni**seçin. Veritabanının üzerine yazmak için, kurtarma türünü **SQL Server'ın özgün örneğine kurtar'ı**seçin. Ardından **İleri'yi**seçin.

    ![Veritabanını özgün konumuna kurtarma](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    Bu örnekte, DPM veritabanının başka bir SQL Server örneğine veya bağımsız bir ağ klasörüne kurtarılmasına izin verir.
1. Kurtarma **Seçeneklerini Belirt** sayfasında, kurtarma seçeneklerini seçebilirsiniz. Örneğin, kurtarmanın kullandığı bant genişliğini daraltmak için **Ağ bant genişliği kullanımını azaltmayı** seçebilirsiniz. Ardından **İleri'yi**seçin.
1. **Özet** sayfasında geçerli kurtarma yapılandırmasını görürsünüz. **Kurtar'ı**seçin.

    Kurtarma durumu, veritabanının kurtarıldığını gösterir. Sihirbazı kapatmak ve **İzleme** çalışma alanında ilerlemeyi görüntülemek için **Kapat'ı** seçebilirsiniz.

    ![Kurtarma işlemini başlatma](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Kurtarma tamamlandığında, geri yüklenen veritabanı uygulamayla tutarlıdır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [Azure Yedekleme SSS'si'ne](backup-azure-backup-faq.md)bakın.
