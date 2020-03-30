---
title: Azure Yığını'nda SQL Server iş yüklerini yedekleme
description: Bu makalede, Azure Yığını'ndaki SQL Server veritabanlarını korumak için Microsoft Azure Yedekleme Sunucusu'nun (MABS) nasıl yapılandırılabildiğini öğrenin.
ms.topic: conceptual
ms.date: 06/08/2018
ms.openlocfilehash: 03211e1147f96429a8406c4c95654161ed2bf308
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74172305"
---
# <a name="back-up-sql-server-on-azure-stack"></a>Azure Yığını'nda SQL Server'ı yedekleme

Azure Yığını'ndaki SQL Server veritabanlarını korumak için Microsoft Azure Yedekleme Sunucusu'nı (MABS) yapılandırmak için bu makaleyi kullanın.

SQL Server veritabanı yedeklemesinin Azure'a yönetimi ve Azure'dan kurtarma üç adım içerir:

1. SQL Server veritabanlarını korumak için yedekleme ilkesi oluşturma
2. İsteğe bağlı yedekleme kopyaları oluşturma
3. Veritabanını Diskler'den ve Azure'dan kurtarma

## <a name="before-you-start"></a>Başlamadan önce

[Azure Yedekleme Sunucusu'nu yükleyin ve hazırlayın.](backup-mabs-install-azure-stack.md)

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>SQL Server veritabanlarını Azure'da korumak için bir yedekleme ilkesi oluşturma

1. Azure Yedekleme Sunucusu UI'de **Koruma** çalışma alanını tıklatın.

2. Araç şeridinde yeni bir koruma grubu oluşturmak için **Yeni'yi** tıklatın.

    ![Koruma Grubu Oluştur](./media/backup-azure-backup-sql/protection-group.png)

    Azure Yedekleme Sunucusu, **koruma**grubu oluşturmanıza yol açan Koruma Grubu sihirbazını başlatır. **İleri**'ye tıklayın.

3. Koruma **Grubu Türünü Seç** ekranında **Sunucular'ı**seçin.

    ![Koruma Grubu Türünü Seçin - 'Sunucular'](./media/backup-azure-backup-sql/pg-servers.png)

4. Grup **Üyelerini Seç** ekranında, Kullanılabilir Üyeler listesi çeşitli veri kaynaklarını görüntüler. Bir **+** klasörü genişletmek ve alt klasörleri ortaya çıkarmak için tıklatın. Bir öğe seçmek için onay kutusunu tıklatın.

    ![SQL DB'yi seçin](./media/backup-azure-backup-sql/pg-databases.png)

    Seçili tüm öğeler Seçili üyeler listesinde görünür. Korumak istediğiniz sunucuları veya veritabanlarını seçtikten sonra **İleri'yi**tıklatın.

5. Veri **Koruma Yöntemini Seç** ekranında, koruma grubu için bir ad verin ve **Çevrimiçi Koruma** yı istiyorum onay kutusunu seçin.

    ![Veri Koruma Yöntemi - kısa süreli disk & Çevrimiçi Azure](./media/backup-azure-backup-sql/pg-name.png)

6. Kısa **Vadeli Hedefler belirt** ekranında, diske yedekleme noktaları oluşturmak için gerekli girişleri ekleyin ve **İleri'yi**tıklatın.

    Örnekte, **Bekletme aralığı** **5 gündür,** **Senkronizasyon frekansı** her **15 dakikada**bir , yedekleme frekansı dır. **Express Full Backup** **20:00**olarak ayarlanır.

    ![Kısa vadeli hedefler](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > Gösterilen örnekte, her gün saat 20:00'de, değiştirilen verileri önceki günün 20:00 yedekleme noktasından aktararak bir yedekleme noktası oluşturulur. Bu işlem **Express Full Backup**olarak adlandırılır. İşlem günlükleri her 15 dakikada bir eşitlenir. Veritabanını 21:00'de kurtarmanız gerekiyorsa, nokta son ekspres tam yedekleme noktasından (bu durumda 20:00) günlüklerden oluşturulur.
   >
   >

7. Gözden **Geçir disk ayırma** ekranında, kullanılabilir genel depolama alanını ve olası disk alanını doğrulayın. **İleri**'ye tıklayın.

8. Çoğaltma **Oluşturma Yöntemini**Seç'te, ilk kurtarma noktanızı nasıl oluştureceğinizi seçin. Bant genişliği tıkanıklığını önlemek için veya ağ üzerinden ilk yedeklemeyi el ile (ağ dışına) aktarabilirsiniz. İlk yedeklemeyi aktarmak için beklemeyi seçerseniz, ilk aktarım için gereken zamanı belirtebilirsiniz. **İleri**'ye tıklayın.

    ![İlk çoğaltma yöntemi](./media/backup-azure-backup-sql/pg-manual.png)

    İlk yedekleme kopyalama, tüm veri kaynağının (SQL Server veritabanı) üretim sunucusundan (SQL Server makinesi) Azure Yedekleme Sunucusu'na aktarılmasını gerektirir. Bu veriler büyük olabilir ve verileri ağ üzerinden aktarmak bant genişliğini aşabilir. Bu nedenle, ilk yedeklemeyi aktarmayı seçebilirsiniz: Bant genişliği tıkanıklığını önlemek için **el ile** (çıkarılabilir ortam kullanarak) veya ağ üzerinden **otomatik olarak** (belirli bir zamanda).

    İlk yedekleme tamamlandıktan sonra, yedeklerin geri kalanı ilk yedekleme kopyasında artımlı yedeklemelerdir. Artımlı yedeklemeler küçük olma eğilimindedir ve ağ üzerinden kolayca aktarılır.

9. Tutarlılık denetiminin ne zaman çalışmasını istediğinizi seçin ve **İleri'yi**tıklatın.

    ![Tutarlılık denetimi](./media/backup-azure-backup-sql/pg-consistent.png)

    Azure Yedekleme Sunucusu, yedekleme noktasının bütünlüğü üzerinde tutarlılık denetimi gerçekleştirir. Azure Yedekleme Sunucusu, üretim sunucusundaki yedekleme dosyasının denetimini (bu senaryoda SQL Server makinesi) ve bu dosyaiçin yedeklenmiş verileri hesaplar. Bir çakışma varsa, Azure Yedekleme Sunucusu'nda yedeklenen dosyanın bozuk olduğu varsayılır. Azure Yedekleme Sunucusu, checksum uyuşmazlığıyla eşleşen blokları göndererek yedeklenen verileri düzeltir. Tutarlılık denetimleri performans yoğun olduğundan, tutarlılık denetimini zamanlayabilir veya otomatik olarak çalıştırabilirsiniz.

10. Veri kaynaklarının çevrimiçi korumasını belirtmek için Azure'a korunacak veritabanlarını seçin ve **İleri'yi**tıklatın.

    ![Veri kaynaklarını seçin](./media/backup-azure-backup-sql/pg-sqldatabases.png)

11. Kuruluş ilkelerine uygun yedekleme zamanlamaları ve bekletme ilkeleri seçin.

    ![Zamanlama ve Bekletme](./media/backup-azure-backup-sql/pg-schedule.png)

    Bu örnekte, yedeklemeler günde bir kez 12:00 ve 20:00'de alınır (ekranın alt kısmı)

    > [!NOTE]
    > Hızlı kurtarma için diskte birkaç kısa vadeli kurtarma noktası olması iyi bir uygulamadır. Bu kurtarma noktaları operasyonel kurtarma için kullanılır. Azure, daha yüksek SLA'lar ve garantili kullanılabilirlik ile iyi bir iş yeri konumu olarak hizmet vermektedir.
    >
    >

    **En İyi Uygulama**: Yerel disk yedeklemeleri tamamlandıktan sonra başlamak üzere Azure'a yedekleme ler zamanlarsanız, en son disk yedeklemeleri her zaman Azure'a kopyalanır.

12. Bekletme ilkesi zamanlamasını seçin. Saklama ilkesinin nasıl çalıştığına ilişkin ayrıntılar, [teyp altyapısı makalenizi değiştirmek için Azure Yedekleme'yi kullanın'da](backup-azure-backup-cloud-as-tape.md)sağlanır.

    ![Bekletme İlkesi](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    Bu örnekte:

    * Yedekler günde bir kez 12:00 ve 20:00 'de (ekranın alt kısmı) alınır ve 180 gün boyunca saklanır.
    * Destek cumartesi günü saat 12:00'de. 104 hafta saklanır
    * Destek geçen Cumartesi saat 12:00'de. 60 ay saklanır
    * Destek, Mart'ın son Cumartesi günü saat 12:00'de. 10 yıl saklanır
13. **İleri'yi** tıklatın ve ilk yedekleme kopyasını Azure'a aktarmak için uygun seçeneği seçin. **Ağ üzerinden otomatik olarak** seçim yapabilirsiniz

14. **Özet** ekranındaki ilke ayrıntılarını gözden geçirdikten sonra, iş akışını tamamlamak için **Grubu Oluştur'u** tıklatın. Çalışma alanını izlemede **Kapat'ı** tıklatabilir ve iş ilerlemesini izleyebilirsiniz.

    ![Devam Ediyor Koruma Grubu Oluşturulması](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>SQL Server veritabanının isteğe bağlı yedeklemesi

Önceki adımlar bir yedekleme ilkesi oluşturulurken, yalnızca ilk yedekleme gerçekleştiğinde bir "kurtarma noktası" oluşturulur. Zamanlayıcının devreye alınmasını beklemek yerine, aşağıdaki adımlar el ile bir kurtarma noktası oluşturulmasını tetikler.

1. Kurtarma noktasını oluşturmadan önce koruma grubu durumunun veritabanı için **Tamam'ı** göstermesini bekleyin.

    ![Koruma Grubu Üyeleri](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Veritabanına sağ tıklayın ve **Kurtarma Noktası Oluştur'u**seçin.

    ![Çevrimiçi Kurtarma Noktası Oluşturma](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Açılan menüde **Çevrimiçi Koruma'yı** seçin ve Azure'da bir kurtarma noktası oluşturmaya başlamak için **Tamam'ı** tıklatın.

    ![Kurtarma noktası oluştur](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. **İzleme** çalışma alanında iş ilerlemesini görüntüleyin.

    ![İzleme konsolu](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Azure'dan bir SQL Server veritabanı kurtarma

Azure'dan korumalı bir varlık (SQL Server veritabanı) kurtarmak için aşağıdaki adımlar gereklidir.

1. Azure Yedekleme Sunucu Yönetim Konsolu'nu açın. Korumalı sunucuları görebileceğiniz **Kurtarma** çalışma alanına gidin. Gerekli veritabanına göz atın (bu durumda ReportServer$MSDPM2012). **Çevrimiçi** nokta olarak belirtilen zamanda bir **Kurtarma** seçin.

    ![Kurtarma noktasını seçin](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Veritabanı adını sağ tıklatın ve **Kurtar'ı**tıklatın.

    ![Azure'dan Kurtarma](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. MABS kurtarma noktasının ayrıntılarını gösterir. **İleri**'ye tıklayın. Veritabanının üzerine yazmak için, kurtarma türünü **SQL Server'ın özgün örneğine kurtar'ı**seçin. **İleri**'ye tıklayın.

    ![Orijinal Konuma Geri Kazan](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    Bu örnekte, MABS veritabanını başka bir SQL Server örneğine veya bağımsız bir ağ klasörüne kurtarır.

4. Kurtarma **seçeneklerini belirt** ekranında, kurtarma tarafından kullanılan bant genişliğini daraltmak için Ağ bant genişliği kullanımının daraltılması gibi kurtarma seçeneklerini seçebilirsiniz. **İleri**'ye tıklayın.

5. **Özet** ekranında, şimdiye kadar sağlanan tüm kurtarma yapılandırmalarını görürsünüz. **Kurtar'ı**tıklatın.

    Kurtarma durumu, veritabanının kurtarıldığını gösterir. Sihirbazı kapatmak ve **İzleme** çalışma alanında ilerlemeyi görüntülemek için **Kapat'ı** tıklatabilirsiniz.

    ![Kurtarma işlemini başlatma](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Kurtarma tamamlandıktan sonra, geri yüklenen veritabanı uygulama tutarlıdır.

## <a name="next-steps"></a>Sonraki adımlar

Yedekleme [dosyalarına ve uygulama](backup-mabs-files-applications-azure-stack.md) makalesine bakın.
Azure [Yığını makalesinde Yedekleme Paylaşım Noktası'na](backup-mabs-sharepoint-azure-stack.md) bakın.
