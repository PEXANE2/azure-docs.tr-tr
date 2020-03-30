---
title: System Center DPM ile Exchange sunucusuyedekleme
description: System Center 2012 R2 DPM'yi kullanarak Exchange sunucusunun Azure Yedekleme'ye nasıl yedeklenebildiğini öğrenin
ms.reviewer: kasinh
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 389713767409ff49c6fc83a4d6e8dc3c87272fc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77614363"
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-system-center-2012-r2-dpm"></a>System Center 2012 R2 DPM ile Azure Backup’a Exchange sunucusu yedekleme

Bu makalede, bir Microsoft Exchange sunucusunun Azure Yedekleme'ye yedeklemesi için Bir System Center 2012 R2 Data Protection Manager (DPM) sunucusunun nasıl yapılandırılabildiğini açıklanmaktadır.  

## <a name="updates"></a>Güncelleştirmeler

DPM sunucusunu Azure Yedekleme ile başarıyla kaydetmek için System Center 2012 R2 DPM için en son güncelleştirme toplamayı ve Azure Yedekleme Aracısı'nın en son sürümünü yüklemeniz gerekir. [Microsoft Kataloğu'ndan](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager)en son güncelleme toplama alın.

> [!NOTE]
> Bu makaledeki örnekler için Azure Yedekleme Aracısı'nın 2.0.8719.0 sürümü yüklenir ve System Center 2012 R2 DPM'ye Güncelleştirme Rollup 6 yüklenir.
>
>

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce, iş yüklerini korumak için Microsoft Azure Yedekleme'yi kullanmanın tüm [ön koşullarının](backup-azure-dpm-introduction.md#prerequisites-and-limitations) yerine getirildiğinden emin olun. Bu önkoşullar şunlardır:

* Azure sitesinde yedek kasa oluşturuldu.
* Aracı ve kasa kimlik bilgileri DPM sunucusuna indirildi.
* Aracı DPM sunucusuna yüklenir.
* Kasa kimlik bilgileri DPM sunucusunu kaydetmek için kullanıldı.
* Exchange 2016'yı koruyorsanız, lütfen DPM 2012 R2 UR9 veya sonraki bir

## <a name="dpm-protection-agent"></a>DPM koruma aracısı

Exchange sunucusuna DPM koruma aracısını yüklemek için aşağıdaki adımları izleyin:

1. Güvenlik duvarlarının doğru şekilde yapılandırıldığından emin olun. Bkz. [Aracı için güvenlik duvarı özel durumlarını yapılandır.](https://docs.microsoft.com/system-center/dpm/configure-firewall-settings-for-dpm?view=sc-dpm-2019)
2. DPM Yönetici Konsoluna **Yükleme > Yönetim > Aracıları'nı** tıklatarak aracıyı Exchange sunucusuna yükleyin. Ayrıntılı adımlar için [DPM koruma aracısını yükleyin'](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent?view=sc-dpm-2019) e bakın.

## <a name="create-a-protection-group-for-the-exchange-server"></a>Exchange sunucusu için bir koruma grubu oluşturma

1. DPM Yönetici Konsolunda, Yeni **Koruma Grubu Oluştur** sihirbazını açmak için **Koruma'yı**tıklatın ve ardından araç şeridinde **Yeni'yi** tıklatın.
2. Sihirbazın **Hoş Geldiniz** ekranında **İleri'yi**tıklatın.
3. Koruma **grubu türünü seç** ekranında **Sunucular'ı** seçin ve **İleri'yi**tıklatın.
4. Korumak istediğiniz Exchange sunucusu veritabanını seçin ve **İleri'yi**tıklatın.

   > [!NOTE]
   > Exchange 2013'ü koruyorsanız, [Exchange 2013 ön koşulları'nı](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/dn751029(v=sc.12))kontrol edin.
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

    Bu seçeneği seçtikten sonra, Exchange sunucusunda **eseutil** komutunu çalıştırarak oluşturulan G/Ç trafiğini önlemek için dpm sunucusunda yedek tutarlılık denetimi çalıştırılacaktır.

   > [!NOTE]
   > Bu seçeneği kullanmak için, Ese.dll ve Eseutil.exe dosyalarını C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin dizinine kopyalamanız gerekir. Aksi takdirde, aşağıdaki hata tetiklenir:  
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
12. DPM sunucusunun ilk çoğaltmayı oluşturacağı zamanı seçin ve sonra **İleri'yi**tıklatın.
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

1. Exchange veritabanını kurtarmak için DPM Yönetici Konsolunda **Kurtarma'yı** tıklatın.
2. Kurtarmak istediğiniz Exchange veritabanını bulun.
3. *Kurtarma zamanı* açılır listesinden çevrimiçi bir kurtarma noktası seçin.
4. **Kurtarma Sihirbazı'nı**başlatmak için **Kurtar'ı** tıklatın.

Çevrimiçi kurtarma noktaları için beş kurtarma türü vardır:

* **Orijinal Exchange Server konumuna geri kazan:** Veriler özgün Exchange sunucusuna kurtarılır.
* **Exchange Sunucusu'ndaki başka bir veritabanına kurtarma:** Veriler başka bir Exchange sunucusunda başka bir veritabanına kurtarılır.
* **Kurtarma Veritabanına Kurtarma:** Veriler bir Exchange Recovery Database 'e (RDB) kurtarılır.
* **Ağ klasörüne kopyala:** Veriler bir ağ klasörüne kurtarılır.
* **Teyp için kopyala:** DPM sunucusuna iliştirilmiş ve yapılandırılan bir teyp kitaplığınız veya tek başına bir teyp sürücün varsa, kurtarma noktası serbest teybe kopyalanır.

    ![Çevrimiçi çoğaltmayı seçin](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Yedekleme SSS](backup-azure-backup-faq.md)
