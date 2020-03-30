---
title: MARS aracısını kullanarak Windows makinelerini yedekleme
description: Windows makinelerini yedeklemek için Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını kullanın.
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: 12463f33a6fa97b33e70b77fb2fcf6b0a27b5790
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79408921"
---
# <a name="back-up-windows-machines-by-using-the-azure-backup-mars-agent"></a>Azure Yedekleme MARS aracısını kullanarak Windows makinelerini yedekleme

Bu makalede, [Azure Yedekleme](backup-overview.md) hizmeti ve Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını kullanarak Windows makinelerinin nasıl yedeklenenekadar yedekleneneaçıklanmaktadır. MARS, Azure Yedekleme aracısı olarak da bilinir.

Bu makalede, nasıl öğreneceksiniz:

> [!div class="checklist"]
>
> * Ön koşulları doğrulayın
> * Bir yedekleme ilkesi ve zamanlama oluşturun.
> * İsteğe bağlı yedekleme gerçekleştirin.

## <a name="before-you-start"></a>Başlamadan önce

* Azure [Yedekleme'nin Windows makinelerini yedeklemek için MARS aracısını](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders)nasıl kullandığını öğrenin.
* Mars aracısını ikincil bir MABS veya Veri Koruma Yöneticisi sunucusunda çalıştıran [yedekleme mimarisi](backup-architecture.md#architecture-back-up-to-dpmmabs) hakkında bilgi edinin.
* Mars ajanı tarafından [desteklenenleri ve destekleyebildiklerinizi](backup-support-matrix-mars-agent.md) gözden geçirin.
* Yedeklemek istediğiniz makinelerdeki [internet erişimini doğrulayın.](install-mars-agent.md#verify-internet-access)
* MARS aracısı yüklenmediyse, [buraya](install-mars-agent.md)nasıl yükleyin için çalıştığını öğrenin.

## <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme ilkesi, kurtarma noktaları oluşturmak için verilerin anlık görüntülerini ne zaman alacağını belirtir. Ayrıca, kurtarma noktalarının ne kadar uzun süre sakolacağını da belirtir. Bir yedekleme ilkesini yapılandırmak için MARS aracısını kullanırsınız.

Azure Yedekleme, gün ışığından yararlanma saatini (DST) otomatik olarak hesaba katmaz. Bu varsayılan gerçek saat ve zamanlanan yedekleme zamanı arasında bazı tutarsızlıklar neden olabilir.

Yedekleme ilkesi oluşturmak için:

1. MARS aracısını indirip kaydettikten sonra aracı konsolunu açın. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.  

1. **Eylemler**altında, **Yedekleme Zamanla'yı**seçin.

    ![Windows Server yedeklemesini zamanlama](./media/backup-configure-vault/schedule-first-backup.png)
1. Zamanlama Yedekleme Sihirbazı'nda, >  **Sonraki'ne Başlar'ı**seçin.**Next**
1. **Yedeklemek Için Öğeleri Seç'in**altında, **Öğe Ekle'yi**seçin.

    ![Yedeklemek için öğe ekleme](./media/backup-azure-manage-mars/select-item-to-backup.png)

1. Öğeleri **Seç** kutusunda, yedeklenebilmek için öğeleri seçin ve ardından **Tamam'ı**seçin.

    ![Yedeklenebilmek için öğeleri seçme](./media/backup-azure-manage-mars/selected-items-to-backup.png)

1. Yukarı **Yassı'nı Seç** sayfasında **İleri'yi**seçin.
1. Yedekleme **Çizelgesi Belirt** sayfasında, günlük veya haftalık yedeklemelerin ne zaman alınca ya da ne zaman alıncaya kadar belirtin. Ardından **İleri'yi**seçin.

    * Yedekleme alındığında bir kurtarma noktası oluşturulur.
    * Ortamınızda oluşturulan kurtarma noktalarının sayısı yedekleme zamanlamanıza bağlıdır.
    * Günde en fazla üç günlük yedekleme zamanlayabilirsiniz. Aşağıdaki örnekte, biri gece yarısı, diğeri 18:00'de olmak üzere iki günlük yedekleme oluşur.

        ![Günlük yedekleme zamanlaması ayarlama](./media/backup-configure-vault/day-schedule.png)

    * Haftalık yedeklemeleri de çalıştırabilirsiniz. Aşağıdaki örnekte, yedeklemeler her alternatif Pazar ve Çarşamba günleri 09:30 ve 01:00'de alınır.

        ![Haftalık yedekleme zamanlaması ayarlama](./media/backup-configure-vault/week-schedule.png)

1. **Bekletme İlkesi Seç** sayfasında, verilerinizin geçmiş kopyalarını nasıl depolayacaklarını belirtin. Ardından **İleri'yi**seçin.

    * Bekletme ayarları, hangi kurtarma noktalarının depolanılsüreceğini ve ne kadar süreyle depolanacaklarını belirtir.
    * Günlük bekletme ayarı için, günlük bekletme için belirtilen zamanda, en son kurtarma noktasının belirtilen gün sayısı için tutulacağını belirtirsiniz. Veya her ayın 30'unda oluşturulan kurtarma noktasının 12 ay boyunca saklanması gerektiğini belirtmek için bir aylık bekletme ilkesi belirtebilirsiniz.
    * Günlük ve haftalık kurtarma noktalarıiçin bekletme genellikle yedekleme zamanlaması ile çakışıyor. Bu nedenle zamanlama bir yedekleme tetiklediğinde, yedeklemenin oluşturduğu kurtarma noktası günlük veya haftalık bekletme ilkesinin belirttiği süre boyunca depolanır.
    * Aşağıdaki örnekte:

        * Gece yarısı ve 18:00'de günlük yedekler yedi gün bekletilir.
        * Cumartesi gece yarısı ve 18:00'de alınan yedekler dört hafta boyunca saklanır.
        * Ayın son Cumartesi günü gece yarısı ve 18:00'de alınan yedekler 12 ay boyunca saklanır.
        * Mart'ın son Cumartesi günü alınan yedekler 10 yıl saklanır.

        ![Bekletme ilkesi örneği](./media/backup-configure-vault/retention-example.png)

1. İlk **Yedekleme Türünü Seç** sayfasında, ilk yedeklemeyi ağ üzerinden mi almak istediğinize veya çevrimdışı yedeklemeyi kullanmak isteyip istemediğinizi belirleyin. İlk yedeklemeyi ağ üzerinden almak**için, Sonraki** **ağ** > üzerinden Otomatik olarak ''yi seçin.

    Çevrimdışı yedekleme hakkında daha fazla bilgi için, [çevrimdışı yedekleme için Azure Veri Kutusu'yu kullanın'a](offline-backup-azure-data-box.md)bakın.

    ![İlk yedekleme türünü seçme](./media/backup-azure-manage-mars/choose-initial-backup-type.png)

1. **Onay** sayfasında bilgileri gözden geçirin ve **ardından Bitir'i**seçin.

    ![Yedekleme türünü onaylama](./media/backup-azure-manage-mars/confirm-backup-type.png)

1. Sihirbaz yedekleme zamanlamasını oluşturmayı bitirdikten sonra **Kapat'ı**seçin.

    ![Yedekleme zamanlama ilerlemesini görüntüleme](./media/backup-azure-manage-mars/confirm-modify-backup-process.png)

Aracının yüklü olduğu her makinede bir ilke oluşturun.

### <a name="do-the-initial-backup-offline"></a>İlk yedeklemeyi çevrimdışı yapma

İlk yedeklemeyi ağ üzerinden otomatik olarak çalıştırabilir veya çevrimdışı yedekleyebilirsiniz. İlk yedekleme için çevrimdışı tohumlama, aktarım için çok fazla ağ bant genişliği gerektirecek büyük miktarda veriniz varsa yararlıdır.

Çevrimdışı aktarım yapmak için:

1. Yedekleme verilerini bir hazırlama konumuna yazın.
1. Verileri hazırlama konumundan bir veya daha fazla SATA diske kopyalamak için AzureOfflineBackupDiskPrep aracını kullanın.

    Araç bir Azure İçe Aktarma işi oluşturur. Daha fazla bilgi için Azure [İçe Alma/Dışa Aktarma hizmeti nedir'e](https://docs.microsoft.com/azure/storage/common/storage-import-export-service)bakın.
1. SATA disklerini azure veri merkezine gönderin.

    Veri merkezinde, disk verileri bir Azure depolama hesabına kopyalanır. Azure Yedekleme, depolama hesabındaki verileri kasaya kopyalar ve artımlı yedeklemeler zamanlanır.

Çevrimdışı tohumlama hakkında daha fazla bilgi için, [çevrimdışı yedekleme için Azure Veri Kutusu'yu kullanın'a](offline-backup-azure-data-box.md)bakın.

### <a name="enable-network-throttling"></a>Ağ azaltmayı etkinleştirme

Mars aracısının ağ azaltmayı etkinleştirerek ağ bant genişliğini nasıl kullandığını denetleyebilirsiniz. Çalışma saatleri içinde verileri yedeklemeniz gerekiyorsa ancak yedekleme ve etkinliği geri yüklemenin ne kadar bant genişliği kullandığını denetlemek istediğinizde azaltma yararlıdır.

Azure Yedekleme'de ağ azaltma, yerel işletim sisteminde [Hizmet Kalitesi 'ni (QoS)](https://docs.microsoft.com/windows-server/networking/technologies/qos/qos-policy-top) kullanır.

Yedeklemeler için ağ azaltma, Windows Server 2012 ve sonraki saatlerde, Windows 8 ve sonraki saatlerde kullanılabilir. İşletim sistemleri en son servis paketlerini çalıştırıyor olmalıdır.

Ağ azaltmayı etkinleştirmek için:

1. MARS aracısında **Özellikleri Değiştir'i**seçin.
1. **Azaltma** sekmesinde, **yedekleme işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir'i**seçin.

    ![Yedekleme işlemleri için ağ azaltmayı ayarlama](./media/backup-configure-vault/throttling-dialog.png)
1. Çalışma saatleri ve çalışma dışı saatlerde izin verilen bant genişliğini belirtin. Bant genişliği değerleri 512 Kbps'den başlar ve 1.023 MBp'ye kadar çıkar. Sonra **Tamam**’ı seçin.

## <a name="run-an-on-demand-backup"></a>İsteğe bağlı yedekleme çalıştırma

1. MARS ajanında, **Şimdi Yukarı'yı**Yedekle'yi seçin.

    ![Windows Server'da şimdi yedekleme](./media/backup-configure-vault/backup-now.png)

1. MARS aracısı sürümü 2.0.9169.0 veya daha yeniyse, özel bir bekletme tarihi ayarlayabilirsiniz. Yedekleme **Kasasını Koru** bölümünde, takvimden bir tarih seçin.

   ![Bekletme tarihini özelleştirmek için takvimi kullanma](./media/backup-configure-vault/mars-ondemand.png)

1. **Onay** sayfasında, ayarları gözden geçirin ve **Yukarı'yı seçin.**
1. Sihirbazı kapatmak için **Kapat'ı** seçin. Yedekleme tamamlanmadan önce sihirbazı kapatırsanız, sihirbaz arka planda koşmaya devam eder.

İlk yedekleme tamamlandıktan **sonra, İş tamamlanan** durum Yedekleme konsolunda görünür.

## <a name="set-up-on-demand-backup-policy-retention-behavior"></a>İsteğe bağlı yedekleme ilkesi tutma davranışını ayarlama

> [!NOTE]
> Bu bilgiler yalnızca 2.0.9169.0'dan eski MARS aracısürümleri için geçerlidir.
>

| Yedekleme zamanlama seçeneği | Veri saklama süresi
| -- | --
| Gün | **Varsayılan bekletme**: "Günlük yedeklemeler için gün içinde bekletme" eşdeğeridir. <br/><br/> **Özel Durum**: Uzun süreli bekletme (haftalar, aylar veya yıllar) için ayarlanmış günlük zamanlanmış yedekleme başarısız olursa, hatadan hemen sonra tetiklenen isteğe bağlı yedekleme uzun süreli bekletme için kabul edilir. Aksi takdirde, bir sonraki zamanlanmış yedekleme uzun süreli bekletme için kabul edilir.<br/><br/> **Örnek senaryo**: Perşembe günü saat 08:00'de zamanlanan yedekleme başarısız oldu. Bu yedekleme haftalık, aylık veya yıllık bekletme için dikkate alınacaktı. Bu nedenle, Cuma günü saat 08:00'de bir sonraki zamanlanmış yedeklemeden önce tetiklenen ilk isteğe bağlı yedekleme, haftalık, aylık veya yıllık bekletme için otomatik olarak etiketlenir. Bu yedek, Perşembe 8:00 yedeklemesinin yerine geçer.
| Hafta | **Varsayılan bekletme**: Bir gün. Haftalık yedekleme ilkesi olan bir veri kaynağı için alınan isteğe bağlı yedeklemeler ertesi gün silinir. Veri kaynağının en son yedekleri olsalar bile silinirler. <br/><br/> **Özel Durum**: Uzun süreli bekletme (haftalar, aylar veya yıllar) için ayarlanmış haftalık zamanlanmış yedekleme başarısız olursa, hatadan hemen sonra tetiklenen isteğe bağlı yedekleme uzun süreli bekletme için kabul edilir. Aksi takdirde, bir sonraki zamanlanmış yedekleme uzun süreli bekletme için kabul edilir. <br/><br/> **Örnek senaryo**: Perşembe günü saat 08:00'de zamanlanan yedekleme başarısız oldu. Bu yedekleme aylık veya yıllık bekletme için dikkate alınması gerekiyordu. Bu nedenle, Perşembe günü saat 8:00'de bir sonraki planlanan yedeklemeden önce tetiklenen ilk isteğe bağlı yedekleme, aylık veya yıllık bekletme için otomatik olarak etiketlenir. Bu yedek, Perşembe 8:00 yedeklemesinin yerine geçer.

Daha fazla bilgi için [bkz.](#create-a-backup-policy)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da dosyaları](backup-azure-restore-windows-server.md)nasıl geri yükleyin öğrenin.
* [Dosya ve klasörleri yedekleme yle ilgili sık sorulan soruları](backup-azure-file-folder-backup-faq.md) bulma

