---
title: Azure Yedekleme Sunucusu ile Exchange sunucusuna yedekleme
description: Azure Yedekleme Sunucusu'ni kullanarak Exchange sunucusunun Azure Yedekleme'ye nasıl yedeklenebildiğini öğrenin
ms.reviewer: kasinh
ms.topic: conceptual
ms.date: 03/24/2017
ms.openlocfilehash: 1d7d28d813df82a5e1ea0fe424bba2ef5a9a2684
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421345"
---
# <a name="back-up-an-exchange-server-to-azure-with-azure-backup-server"></a>Azure Yedekleme Sunucusu ile Exchange sunucusunun Azure'a yedeklemi

Bu makalede, Bir Microsoft Exchange sunucusunun Azure'a yedeklenebilmek için Microsoft Azure Yedekleme Sunucusu'nun (MABS) nasıl yapılandırılabildiğini açıklanmaktadır.  

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce Azure Yedekleme Sunucusu'nun [yüklü olduğundan ve hazırlandığından](backup-azure-microsoft-azure-backup.md)emin olun.

## <a name="mabs-protection-agent"></a>MABS koruma maddesi

Exchange sunucusuna MABS koruma aracısını yüklemek için aşağıdaki adımları izleyin:

1. Güvenlik duvarlarının doğru şekilde yapılandırıldığından emin olun. Bkz. [Aracı için güvenlik duvarı özel durumlarını yapılandır.](https://docs.microsoft.com/system-center/dpm/configure-firewall-settings-for-dpm?view=sc-dpm-2019)
2. MABS Yönetici Konsoluna **Yönetim > Aracıları > yükleyin** tıklayarak Exchange sunucusuna aracıyı yükleyin. Bkz. Ayrıntılı adımlar için [MABS koruma aracısını yükleyin.](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent?view=sc-dpm-2019)

## <a name="create-a-protection-group-for-the-exchange-server"></a>Exchange sunucusu için bir koruma grubu oluşturma

1. MABS Yönetici Konsolunda, Yeni **Koruma Grubu Oluştur** sihirbazını açmak için **Koruma'yı**tıklatın ve ardından araç şeridinde **Yeni'yi** tıklatın.
2. Sihirbazın **Hoş Geldiniz** ekranında **İleri'yi**tıklatın.
3. Koruma **grubu türünü seç** ekranında **Sunucular'ı** seçin ve **İleri'yi**tıklatın.
4. Korumak istediğiniz Exchange sunucusu veritabanını seçin ve **İleri'yi**tıklatın.

   > [!NOTE]
   > Exchange 2013'ü koruyorsanız, [Exchange 2013 ön koşulları'nı](https://docs.microsoft.com/system-center/dpm/back-up-exchange?view=sc-dpm-2016)kontrol edin.
   >
   >

    Aşağıdaki örnekte, Exchange 2010 veritabanı seçilir.

    ![Grup üyelerini seçin](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Veri koruma yöntemini seçin.

    Koruma grubunu adlandırın ve ardından aşağıdaki seçeneklerden her ikisini de seçin:

   * Disk'i kullanarak kısa süreli koruma istiyorum.
   * Çevrimiçi koruma istiyorum.
6. **İleri**'ye tıklayın.
7. Exchange Server veritabanlarının bütünlüğünü denetlemek istiyorsanız veri bütünlüğünü denetlemek için **Eseutil'i çalıştır'ı** seçin.

    Bu seçeneği seçtikten sonra, Exchange sunucusunda **eseutil** komutunu çalıştırarak oluşturulan G/Ç trafiğini önlemek için MABS'de yedek tutarlılık denetimi çalıştırılacaktır.

   > [!NOTE]
   > Bu seçeneği kullanmak için, Ese.dll ve Eseutil.exe dosyalarını MABS sunucusundaki C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin dizinine kopyalamanız gerekir. Aksi takdirde, aşağıdaki hata tetiklenir:  
   > ![eseutil hatası](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. **İleri**'ye tıklayın.
9. **Yedeklemekopyala**veritabanını seçin ve sonra **İleri'yi**tıklatın.

   > [!NOTE]
   > Bir veritabanının en az bir DAG kopyası için "Tam yedekleme" seçeneğini seçmezseniz, günlükler kesildirilmeyecektir.
   >
   >
10. **Kısa Vadeli yedekleme**için hedefleri yapılandırın ve sonra **İleri'yi**tıklatın.
11. Kullanılabilir disk alanını gözden geçirin ve sonra **İleri'yi**tıklatın.
12. MABS Sunucusu'nun ilk çoğaltmayı oluşturacağı zamanı seçin ve sonra **İleri'yi**tıklatın.
13. Tutarlılık denetimi seçeneklerini seçin ve sonra **İleri'yi**tıklatın.
14. Azure'a yedeklemek istediğiniz veritabanını seçin ve sonra **İleri'yi**tıklatın. Örnek:

    ![Çevrimiçi koruma verilerini belirtin](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. **Azure Yedekleme**için zamanlamayı tanımlayın ve sonra **İleri'yi**tıklatın. Örnek:

    ![Çevrimiçi yedekleme zamanlamasını belirtin](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Not Çevrimiçi kurtarma noktaları ekspres tam kurtarma noktalarına dayanır. Bu nedenle, ekspres tam kurtarma noktası için belirtilen süreden sonra çevrimiçi kurtarma noktasını zamanlamanız gerekir.
    >
    >
16. **Azure Yedekleme**için bekletme ilkesini yapılandırın ve sonra **İleri'yi**tıklatın.
17. Çevrimiçi çoğaltma seçeneğini seçin ve **İleri'yi**tıklatın.

    Büyük bir veritabanınız varsa, ilk yedeklemenin ağ üzerinden oluşturulması uzun sürebilir. Bu sorunu önlemek için çevrimdışı yedekleme oluşturabilirsiniz.  

    ![Çevrimiçi bekletme ilkesini belirtin](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Ayarları onaylayın ve ardından **Grup Oluştur'u**tıklatın.
19. **Kapat'ı**tıklatın.

## <a name="recover-the-exchange-database"></a>Exchange veritabanını kurtarma

1. Exchange veritabanını kurtarmak için MABS Yönetici Konsolunda **Kurtarma'yı** tıklatın.
2. Kurtarmak istediğiniz Exchange veritabanını bulun.
3. *Kurtarma zamanı* açılır listesinden çevrimiçi bir kurtarma noktası seçin.
4. **Kurtarma Sihirbazı'nı**başlatmak için **Kurtar'ı** tıklatın.

Çevrimiçi kurtarma noktaları için beş kurtarma türü vardır:

* **Orijinal Exchange Server konumuna geri kazan:** Veriler özgün Exchange sunucusuna kurtarılır.
* **Exchange Sunucusu'ndaki başka bir veritabanına kurtarma:** Veriler başka bir Exchange sunucusunda başka bir veritabanına kurtarılır.
* **Kurtarma Veritabanına Kurtarma:** Veriler bir Exchange Recovery Database 'e (RDB) kurtarılır.
* **Ağ klasörüne kopyala:** Veriler bir ağ klasörüne kurtarılır.
* **Teyp için kopyala:** MABS'ye iliştirilmiş ve yapılandırılan bir teyp kitaplığınız veya tek başına bir teyp sürücün varsa, kurtarma noktası serbest teybe kopyalanır.

    ![Çevrimiçi çoğaltmayı seçin](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Yedekleme SSS](backup-azure-backup-faq.md)
