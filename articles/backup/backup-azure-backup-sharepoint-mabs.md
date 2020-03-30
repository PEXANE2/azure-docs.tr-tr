---
title: MABS ile SharePoint çiftlerini Azure'a yedekleme
description: SharePoint verilerinizi yedeklemek ve geri yüklemek için Azure Yedekleme Sunucusu'ni kullanın. Bu makalede, istenen verilerin Azure'da depolanabilmesi için SharePoint çiftliğinizi yapılandırmak için bilgiler sağlanmaktadır. Korunmuş SharePoint verilerini diskten veya Azure'dan geri yükleyebilirsiniz.
ms.topic: conceptual
ms.date: 06/08/2018
ms.openlocfilehash: 441a896f2faa67a1380007ebb9474d7c311a4842
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673133"
---
# <a name="back-up-a-sharepoint-farm-to-azure-with-mabs"></a>MABS ile SharePoint çiftlerini Azure'a yedekleme

Diğer veri kaynaklarını yedeklemek gibi Microsoft Azure Yedekleme Sunucusu'nu (MABS) kullanarak bir SharePoint çiftliğini Microsoft Azure'a yedeklersiniz. Azure Yedekleme, günlük, haftalık, aylık veya yıllık yedekleme noktaları oluşturmak için yedekleme zamanlamasında esneklik sağlar ve çeşitli yedekleme noktaları için bekletme ilkesi seçenekleri sunar. Ayrıca, yerel disk kopyalarını hızlı kurtarma zamanı hedefleri (RTO) için depolama ve kopyaları ekonomik ve uzun süreli bekletme için Azure'da depolama olanağı sağlar.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>SharePoint desteklenen sürümler ve ilgili koruma senaryoları

DPM için Azure Yedekleme aşağıdaki senaryoları destekler:

| İş yükü | Sürüm | SharePoint dağıtımı | Koruma ve kurtarma |
| --- | --- | --- | --- |
| SharePoint |SharePoint 2016, SharePoint 2013, SharePoint 2010, SharePoint 2007, SharePoint 3.0 |Fiziksel sunucu veya Hyper-V/VMware sanal makine olarak dağıtılan SharePoint <br> -------------- <br> SQL AlwaysOn | SharePoint Farm kurtarma seçeneklerini koruyun: Kurtarma çiftliği, veritabanı ve dosya veya liste öğesi disk kurtarma noktalarından.  Azure kurtarma noktalarından farm ve veritabanı kurtarma. |

## <a name="before-you-start"></a>Başlamadan önce

Bir SharePoint çiftliğini Azure'a yedeklemeden önce onaylamanız gereken birkaç şey vardır.

### <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce, iş yüklerini korumak için [Azure Yedekleme Sunucusu'nu yüklediğinizden ve hazırladığınıza](backup-azure-microsoft-azure-backup.md) emin olun.

### <a name="protection-agent"></a>Koruma aracısı

Azure Yedekleme aracısı SharePoint'i çalıştıran sunucuya, SQL Server çalıştıran sunuculara ve SharePoint çiftliğinin parçası olan diğer tüm sunuculara yüklenmiş olmalıdır. Koruma aracısını nasıl ayarlayıp kuracağız hakkında daha fazla bilgi için [Kurulum Koruma Aracısı'na](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent?view=sc-dpm-2019)bakın.  Bir istisna tek bir web ön uç (WFE) sunucusunda yalnızca aracı yüklemek olduğunu. Azure Backup Server'ın yalnızca koruma için giriş noktası olarak hizmet verebilmek için bir WFE sunucusundaki aracıya ihtiyacı vardır.

### <a name="sharepoint-farm"></a>SharePoint grubu

Çiftlikteki her 10 milyon öğe için, MABS klasörünün bulunduğu birimde en az 2 GB alan olmalıdır. Bu boşluk katalog oluşturma için gereklidir. MABS'nin belirli öğeleri (site koleksiyonları, siteler, listeler, belge kitaplıkları, klasörler, tek tek belgeler ve liste öğeleri) kurtarması için katalog oluşturma, her içerik veritabanında bulunan URL'lerin bir listesini oluşturur. MABS Yönetici Konsolu'nun **Kurtarma** görev alanında kurtarılabilir öğe bölmesinde URL listesini görüntüleyebilirsiniz.

### <a name="sql-server"></a>SQL Server

Azure Yedekleme Sunucusu, LocalSystem hesabı olarak çalışır. SQL Server veritabanlarını yedeklemek için, MABS'nin SQL Server'ı çalıştıran sunucu için bu hesapta sysadmin ayrıcalıklarına ihtiyacı vardır. NT AUTHORITY\SYSTEM'i yedeklemeden önce SQL Server çalıştıran sunucuda *sysadmin* olarak ayarlayın.

SharePoint farmalarında SQL Server diğer adlarıyla yapılandırılan SQL Server veritabanları varsa, MABS'nin koruyacağı ön uç Web sunucusuna SQL Server istemci sİstem bileşenlerini yükleyin.

### <a name="sharepoint-server"></a>SharePoint Server

Performans SharePoint çiftliğinin boyutu gibi birçok faktöre bağlı olsa da, genel kılavuz olarak bir MABS 25-TB SharePoint çiftliğini koruyabilir.

### <a name="whats-not-supported"></a>Desteklenmeyen durumlar

* SharePoint ekinlerini koruyan MABS, arama dizinlerini veya uygulama hizmeti veritabanlarını korumaz. Bu veritabanlarının korumasını ayrı olarak yapılandırmanız gerekir.
* MABS, ölçeklendirilen dosya sunucusu (SOFS) paylaşımlarında barındırılan SharePoint SQL Server veritabanlarının yedeklemesini sağlamaz.

## <a name="configure-sharepoint-protection"></a>SharePoint korumayı yapılandırma

SharePoint'i korumak için MABS'yi kullanabilmek için **önce, ConfigureSharePoint.exe'yi**kullanarak SharePoint VSS Writer hizmetini (WSS Writer service) yapılandırmanız gerekir.

**ConfigureSharePoint.exe'yi** ön uç web sunucusunda [MABS Yükleme Yolu]\bin klasöründe bulabilirsiniz. Bu araç, koruma aracısı SharePoint çiftliği için kimlik bilgileri sağlar. Tek bir WFE sunucusunda çalıştırın. Birden çok WFE sunucunuz varsa, bir koruma grubu yapılandırırken yalnızca birini seçin.

### <a name="to-configure-the-sharepoint-vss-writer-service"></a>SharePoint VSS Writer hizmetini yapılandırmak için

1. WFE sunucusunda, komut istemiyle [MABS yükleme konumu]\bin\
2. YapılandırmaSharePoint girin -EnableSharePointProtection.
3. Grup yöneticisi kimlik bilgilerini girin. Bu hesap, WFE sunucusunda yerel Yönetici grubunun bir üyesi olmalıdır. Grup yöneticisi yerel bir yönetici değilse WFE sunucusunda aşağıdaki izinleri verin:
   * WSS_Admin_WPG grubu tam denetimini DPM klasörüne (%Program Dosyaları%\Microsoft Azure Yedekleme\DPM) ver.
   * DPM Kayıt Defteri anahtarına (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Veri Koruma Yöneticisi) okuma WSS_Admin_WPG gruba erişim verin.

> [!NOTE]
> SharePoint çiftlik yöneticisi kimlik bilgilerinde bir değişiklik olduğunda ConfigureSharePoint.exe'yi yeniden çalıştırmanız gerekir.
>
>

## <a name="back-up-a-sharepoint-farm-by-using-mabs"></a>MABS kullanarak Bir SharePoint çiftliğini yedekleme

MABS ve SharePoint çiftliğini daha önce açıklandığı gibi yapılandırdıktan sonra, SharePoint MABS tarafından korunabilir.

### <a name="to-protect-a-sharepoint-farm"></a>SharePoint çiftliğini korumak için

1. MABS Yönetici Konsolunun **Koruma** sekmesinden **Yeni'yi**tıklatın.
    ![Yeni Koruma Sekmesi](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)
2. Yeni **Koruma Grubu** **Oluştur** sihirbazının Koruma Grubu Türü Seç sayfasında **Sunucular'ı**seçin ve **ardından İleri'yi**tıklatın.

    ![Koruma Grubu türünü seçin](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. Grup **Üyelerini Seç** ekranında, korumak istediğiniz SharePoint sunucusunun onay kutusunu seçin ve **İleri'yi**tıklatın.

    ![Grup üyelerini seçin](./media/backup-azure-backup-sharepoint/select-group-members2.png)

   > [!NOTE]
   > Koruma aracısı yüklü olduğu için, sunucuyu sihirbazda görebilirsiniz. MABS da yapısını gösterir. ConfigureSharePoint.exe'yi çalıştırdığınız için, MABS SharePoint VSS Writer hizmeti ve ilgili SQL Server veritabanları ile iletişim kurar ve SharePoint farm yapısını, ilişkili içerik veritabanlarını ve ilgili öğeleri tanır.
   >
   >
4. Veri **Koruma Yöntemini Seç** sayfasında, **Koruma Grubu'nun**adını girin ve tercih ettiğiniz *koruma yöntemlerini*seçin. **İleri**'ye tıklayın.

    ![Veri koruma yöntemini seçme](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

   > [!NOTE]
   > Disk koruma yöntemi, kısa kurtarma süresi hedeflerini karşılamaya yardımcı olur.
   >
   >
5. Kısa **Vadeli Hedefler Belirt** sayfasında, tercih ettiğiniz **Bekletme aralığını** seçin ve yedeklemelerin ne zaman gerçekleşmesini istediğinizi belirleyin.

    ![Kısa vadeli hedefler belirtin](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

   > [!NOTE]
   > Kurtarma genellikle beş günden eski veriler için gerekli olduğundan, diskte beş günlük bir bekletme aralığı seçtik ve bu örnekte, yedeklemenin üretim dışı saatlerde gerçekleşmesini sağladık.
   >
   >
6. Koruma grubu için ayrılan depolama havuzu disk alanını gözden geçirin ve **sonra İleri'yi**tıklatın.
7. Her koruma grubu için MABS, yinelemeleri depolamak ve yönetmek için disk alanı ayırır. Bu noktada, MABS seçili verilerin bir kopyasını oluşturmalıdır. Yinelemenin nasıl ve ne zaman oluşturulmasını istediğinizi seçin ve sonra **İleri'yi**tıklatın.

    ![Yineleme oluşturma yöntemini seçin](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

   > [!NOTE]
   > Ağ trafiğinin etkilenmediğinden emin olmak için üretim saatleri dışında bir süre seçin.
   >
   >
8. MABS çoğaltma üzerinde tutarlılık denetimleri gerçekleştirerek veri bütünlüğü sağlar. İki seçenek vardır. Tutarlılık denetimleri çalıştırmak için bir zamanlama tanımlayabilirsiniz veya DPM tutarsız olduğunda yineleme üzerinde otomatik olarak tutarlılık denetimleri çalıştırabilirsiniz. Tercih ettiğiniz seçeneği seçin ve **sonra İleri'yi**tıklatın.

    ![Tutarlılık Denetimi](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. Çevrimiçi **Koruma Verilerini Belirt** sayfasında, korumak istediğiniz SharePoint çiftliğini seçin ve sonra **İleri'yi**tıklatın.

    ![DPM SharePoint Protection1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. Çevrimiçi **Yedekleme Çizelgesi'ni belirt** sayfasında, tercih ettiğiniz programı seçin ve sonra **İleri'yi**tıklatın.

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    > [!NOTE]
    > MABS, kullanılabilir en son disk yedekleme noktasından Azure'a en fazla iki günlük yedekleme sağlar. Azure Yedekleme [ayrıca, Azure Yedekleme ağ azaltmayı](backup-windows-with-mars-agent.md#enable-network-throttling)kullanarak yoğun ve yoğun olmayan saatlerde yedeklemeler için kullanılabilecek WAN bant genişliği miktarını da denetleyebilir.
    >
    >
11. Seçtiğiniz yedekleme zamanlamasına bağlı olarak, **Çevrimiçi Bekletme İlkesi Belirt** sayfasında günlük, haftalık, aylık ve yıllık yedekleme noktaları için bekletme ilkesini seçin.

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    > [!NOTE]
    > MABS, farklı yedekleme noktaları için farklı bir bekletme ilkesinin seçilebildiği bir büyükbaba-baba-oğul tutma şeması kullanır.
    >
    >
12. Diske benzer şekilde, Azure'da da bir ilk başvuru noktası yinelemesi oluşturulması gerekir. Azure'a ilk yedek kopya oluşturmak için tercih ettiğiniz seçeneği seçin ve sonra **İleri'yi**tıklatın.

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. **Özet** sayfasında seçtiğiniz ayarları gözden geçirin ve ardından **Grup Oluştur'u**tıklatın. Koruma grubu oluşturulduktan sonra bir başarı iletisi görürsünüz.

    ![Özet](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-mabs"></a>MABS kullanarak diskten SharePoint öğeyi geri yükleme

Aşağıdaki örnekte, *SharePoint öğesini kurtarma* yanlışlıkla silinmiş ve kurtarılması gerekir.
![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)

1. **DPM Yönetici Konsolu'nu**açın. DPM tarafından korunan tüm SharePoint çiftlikleri **Koruma** sekmesinde gösterilir.

    ![MABS SharePoint Koruma3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. Öğeyi kurtarmaya başlamak için **Kurtarma** sekmesini seçin.

    ![MABS SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. Kurtarma noktası aralığında joker karakter tabanlı bir arama kullanarak *SharePoint öğesini kurtarmak* için SharePoint'te arama yapabilirsiniz.

    ![MABS SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. Arama sonuçlarından uygun kurtarma noktasını seçin, öğeyi sağ tıklatın ve sonra **Kurtar'ı**seçin.
5. Ayrıca, çeşitli kurtarma noktalarına göz atabilir ve kurtarmak için bir veritabanı veya öğe seçebilirsiniz. **Kurtarma zamanı > Tarih'i**seçin ve ardından Doğru Veritabanı > **SharePoint > Öğe> Kurtarma noktasını**seçin.

    ![MABS SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. Öğeyi sağ tıklatın ve sonra Kurtarma **Sihirbazı'nı**açmak için **Kurtar'ı** seçin. **İleri**'ye tıklayın.

    ![Kurtarma Seçimini Gözden Geçirin](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. Gerçekleştirmek istediğiniz kurtarma türünü seçin ve sonra **İleri'yi**tıklatın.

    ![Kurtarma Türü](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > Örnekte **özgün recover** seçimi, öğeyi özgün SharePoint sitesine kurtarır.
   >
   >
8. Kullanmak istediğiniz **Kurtarma İşlemi'ni** seçin.

   * SharePoint çiftliği değişmediyse ve geri yüklenen kurtarma noktasıyla aynıysa, **kurtarma ekilisini kullanmadan Kurtar'ı** seçin.
   * Kurtarma noktası oluşturulduğundan beri SharePoint çiftliği değiştiyse **kurtarma çiftliğikullanarak Kurtarma'yı** seçin.

     ![Kurtarma İşlemi](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. Veritabanını geçici olarak kurtarmak için bir evreleme SQL Server örnek konumu sağlayın ve öğeyi kurtarmak için SharePoint'i çalıştıran Sunucu ve MABS'de bir hazırlama dosyası paylaşımı sağlayın.

    ![Evreleme Yeri1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    MABS, SharePoint öğesini barındıran içerik veritabanını geçici SQL Server örneğine bağlar. İçerik veritabanından öğeyi kurtarır ve MABS'deki hazırlama dosyası konumuna koyar. Evreleme konumunda ki kurtarılan öğenin artık SharePoint çiftliğindeki hazırlama konumuna dışa aktarılması gerekiyor.

    ![Evreleme Yeri2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. **Kurtarma seçeneklerini belirt'i**seçin ve SharePoint ekine güvenlik ayarlarını uygulayın veya kurtarma noktasının güvenlik ayarlarını uygulayın. **İleri**'ye tıklayın.

    ![Kurtarma Seçenekleri](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > Ağ bant genişliği kullanımını azaltmayı seçebilirsiniz. Bu, üretim saatlerinde üretim sunucusuna etkisini en aza indirir.
    >
    >
11. Özet bilgileri gözden geçirin ve dosyayı kurtarmak için **Kurtar'ı** tıklatın.

    ![Kurtarma özeti](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. Şimdi kurtarma **durumunu** görüntülemek için **MABS Yönetici** Konsolu'ndaki **İzleme** sekmesini seçin.

    ![Kurtarma Durumu](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > Dosya şimdi geri yüklendi. Geri yüklenen dosyayı denetlemek için SharePoint sitesini yenileyebilirsiniz.
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>DPM'yi kullanarak Azure'dan SharePoint veritabanını geri yükleme

1. Bir SharePoint içerik veritabanını kurtarmak için çeşitli kurtarma noktalarına (daha önce gösterildiği gibi) göz atın ve geri yüklemek istediğiniz kurtarma noktasını seçin.

    ![MABS SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. Kullanılabilir SharePoint katalog bilgilerini göstermek için SharePoint kurtarma noktasını çift tıklatın.

   > [!NOTE]
   > SharePoint çiftliği Azure'da uzun süreli bekletme için korunduğundan, MABS'de katalog bilgisi (meta veri) bulunmaz. Sonuç olarak, zaman içinde bir SharePoint içerik veritabanının kurtarılması gerektiğinde, SharePoint farm'ını yeniden kataloglamanız gerekir.
   >
   >
3. **Yeniden Katalog'u**tıklatın.

    ![MABS SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    **Bulut Yeniden Katalog** durum penceresi açılır.

    ![MABS SharePoint Koruma11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    Kataloglama tamamlandıktan sonra durum *Başarı*olarak değişir. **Kapat'ı**tıklatın.

    ![MABS SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. İçerik veritabanı yapısını almak için MABS **Kurtarma** sekmesinde gösterilen SharePoint nesnesini tıklatın. Öğeyi sağ tıklatın ve sonra **Kurtar'ı**tıklatın.

    ![MABS SharePoint Koruma13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Bu noktada, bir SharePoint içerik veritabanını diskten kurtarmak için bu makalenin önceki kurtarma adımlarını izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Exchange sunucusu makalesini [yedekle](backup-azure-exchange-mabs.md) makaleye bakın.
SQL [Server'ı Yedekle](backup-azure-sql-mabs.md) makalesine bakın.
