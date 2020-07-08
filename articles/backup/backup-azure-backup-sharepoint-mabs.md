---
title: MABS ile bir SharePoint grubunu Azure 'a yedekleme
description: SharePoint verilerinizi yedeklemek ve geri yüklemek için Azure Backup Sunucusu kullanın. Bu makalede, SharePoint grubunuzu istenen verilerin Azure 'da depolanabilmesi için yapılandırma bilgileri sağlanmaktadır. Korumalı SharePoint verilerini diskten veya Azure 'dan geri yükleyebilirsiniz.
ms.topic: conceptual
ms.date: 04/26/2020
ms.openlocfilehash: 62fcb434ef00df43ce2950a5df569e346a06903a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84234798"
---
# <a name="back-up-a-sharepoint-farm-to-azure-with-mabs"></a>MABS ile bir SharePoint grubunu Azure 'a yedekleme

Microsoft Azure Backup sunucusu (MABS) kullanarak Microsoft Azure için bir SharePoint grubunu diğer veri kaynaklarını yedeklemekten çok aynı şekilde yedekleyerek. Azure Backup günlük, haftalık, aylık veya yıllık yedekleme noktaları oluşturmak için yedekleme zamanlamasında esneklik sağlar ve çeşitli yedekleme noktaları için bekletme ilkesi seçenekleri sunar. MABS, hızlı kurtarma zamanı hedeflerine (RTO) yönelik yerel disk kopyalarını depolama ve ekonomik, uzun süreli saklama için Azure 'a kopya depolama yeteneği sağlar.

MABS ile SharePoint 'ten Azure 'a yedekleme, SharePoint 'i (Data Protection Manager) yerel olarak yedeklemeye yönelik benzer bir işlemdir. Bu makalede Azure için belirli hususlar belirtilecektir.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>SharePoint desteklenen sürümleri ve ilgili koruma senaryoları

Desteklenen SharePoint sürümlerinin ve bunları yedeklemek için gereken MABS sürümlerinin listesi için bkz [. mabs koruma matrisi](https://docs.microsoft.com/azure/backup/backup-mabs-protection-matrix)

## <a name="before-you-start"></a>Başlamadan önce

Bir SharePoint grubunu Azure 'a yedeklemebilmeniz için öncelikle onaylamanız gereken birkaç nokta vardır.

### <a name="whats-not-supported"></a>Desteklenmeyen durumlar

* SharePoint grubunu koruyan MABS, arama dizinlerini veya uygulama hizmeti veritabanlarını korumaz. Bu veritabanlarının korumasını ayrı olarak yapılandırmanız gerekir.

* MABS, genişleme dosya sunucusu (SOFS) paylaşımlarında barındırılan SharePoint SQL Server veritabanlarının yedeklemesini sağlamıyor.

### <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce, iş yüklerini korumak için [Microsoft Azure Backup kullanmaya yönelik tüm önkoşulları](backup-azure-dpm-introduction.md#prerequisites-and-limitations) karşıladığınızdan emin olun. Önkoşullar için bazı görevler şunlardır: bir yedekleme Kasası oluşturun, kasa kimlik bilgilerini indirin, Azure Backup Aracısı yükleyin ve Azure Backup Sunucusu kasaya kaydedin.

Ek Önkoşullar ve sınırlamalar:

* SharePoint 'i koruduğunuzda varsayılan olarak tüm içerik veritabanları (ve SharePoint_Config ve SharePoint_AdminContent * veritabanları) korunur. Arama dizinleri, şablonları veya uygulama hizmeti veritabanları veya kullanıcı profili hizmeti gibi özelleştirmeler eklemek istiyorsanız, bunların korunmasını ayrı yapılandırmanız gerekir. Bu türde özellikler veya özelleştirme dosyaları içeren tüm klasörlerde korumayı etkinleştirdiğinizden emin olun.

* SharePoint veritabanlarını SQL Server veri kaynağı olarak koruyamazsınız. Bir grup yedeğinden ayrı ayrı veritabanlarını kurtarabilirsiniz.

* MABS 'lerin **yerel sistem**olarak çalıştığını ve SQL Server veritabanlarının YEDEKLENMESINI ve SQL Server için bu hesapta sysadmin ayrıcalıklarına ihtiyacı olduğunu unutmayın. Yedeklemek istediğiniz SQL Server, NT AUTHORITY\SYSTEM ' ı **sysadmin**olarak ayarlayın.

* Gruptaki her 10.000.000 öğe için, MABS klasörünün bulunduğu birimde en az 2 GB alan olması gerekir. Bu boşluk katalog oluşturma için gereklidir. MABS 'yi, öğelerin (site koleksiyonları, siteler, listeler, belge kitaplıkları, klasörler, bireysel belgeler ve liste öğeleri) belirli bir kurtarmasını gerçekleştirmek üzere kullanmanıza olanak tanımak için, Katalog oluşturma her bir içerik veritabanında bulunan URL 'lerin bir listesini oluşturur. URL 'lerin listesini, MABS Yönetici Konsolu Kurtarma görev alanındaki kurtarılabilir öğe bölmesinde görüntüleyebilirsiniz.

* SharePoint grubunda, SQL Server diğer adlarla yapılandırılmış SQL Server veritabanlarına sahipseniz, SQL Server istemci bileşenlerini MABS 'nin koruduğu ön uç Web sunucusuna yükleyebilirsiniz.

* Uygulama deposu öğelerinin korumak SharePoint 2013 ile desteklenmemektedir.

* MABS, uzak FıLESTREAM korumasını desteklemiyor. FILESTREAM, veritabanına ait olmalıdır.

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma

SharePoint grubunu yedeklemek için ConfigureSharePoint.exe kullanarak SharePoint korumasını yapılandırın ve ardından MABS içinde bir koruma grubu oluşturun.

1. **ConfigureSharePoint.exe’yi çalıştırın** - Bu araç SharePoint VSS Yazıcı hizmetini \(WSS\) yapılandırır ve koruma aracısını SharePoint grubuna yönelik kimlik bilgileriyle sağlar. Koruma aracısını dağıttıktan sonra, ConfigureSharePoint.exe dosyası `<MABS Installation Path\>\bin` ön \- uç Web sunucusundaki klasöründe bulunabilir.  Birden çok WFE sunucunuz varsa, bunu yalnızca birine yüklemeniz gerekir. Aşağıdaki gibi çalıştırın:

    * WFE sunucusunda, komut isteminde şuraya gidin `\<MABS installation location\>\\bin\\` ve çalıştırın; `ConfigureSharePoint \[\-EnableSharePointProtection\] \[\-EnableSPSearchProtection\] \[\-ResolveAllSQLAliases\] \[\-SetTempPath <path>\]` burada:

        * **EnableSharePointProtection** , SharePoint grubunun korunmasını sağlar, VSS yazıcısını sağlar ve kimlik bilgileri bu seçenekle girilen bir kullanıcı olarak çalıştırılmak üzere Wsscmdletswswber DCOM uygulamasının kimliğini kaydeder. Bu hesabın ön\-uç Web Sunucusunda grup yöneticisi ve ayrıca yerel yönetici olması gerekir.

        * **EnableSPSearchProtection** ön\-uç Web sunucusu üzerindeki HKLM\\Yazılım\\Microsoft\\ Microsoft Data Protection Manager\\Aracı\\2.0\\ altında bulunan SharePointSearchEnumerationEnabled kayıt defteri anahtarını kullanarak WSS 3.0 SP Araması’nın korunmasını etkinleştirir ve DCOM uygulaması WssCmdletsWrapper’ın bu seçenekle kimlik bilgileri girilen kullanıcı olarak belirlenmesini kaydeder. Bu hesabın ön\-uç Web Sunucusunda grup yöneticisi ve ayrıca yerel yönetici olması gerekir.

        * **ResolveAllSQLAliases** SharePoint VSS yazıcı tarafından bildirilen tüm diğer adları gösterir ve ilgili SQL Server örneğinde çözümler. Ayrıca çözümlenmiş örnek adlarını gösterir. Sunucular yansıtılırsa yansıtılmış sunucuyu da gösterir. Çözümlenmeyen tüm diğer adları bir SQL Server rapor edin.

        * **SetTempPath** TEMP ve TMP ortam değişkenini belirtilen yola ayarlar. Büyük bir site koleksiyonu, site, liste veya öğe kurtarılıyorsa ve Grup Yöneticisi geçici klasöründe yeterli alan yoksa öğe düzeyinde kurtarma başarısız olur. Bu seçenek geçici dosyaların klasör yolunu, kurtarılmakta olan site koleksiyonunu veya siteyi depolamaya yetecek alana sahip bir birimle değiştirmenize imkan tanır.

    * Grup yöneticisi kimlik bilgilerini girin. Bu hesap, WFE sunucusunda yerel Yönetici grubunun bir üyesi olmalıdır. Grup Yöneticisi yerel bir yönetici değilse WFE sunucusunda aşağıdaki izinleri verin:

        * MABS klasörüne () **WSS_Admin_WPG** grubuna Tam Denetim izni verin `%Program Files%\Data Protection Manager\DPM\` .

        * MABS kayıt defteri anahtarına () **WSS_Admin_WPG** grubuna Okuma erişimi verin `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager` .

        ConfigureSharePoint.exe çalıştırdıktan sonra, SharePoint grubu yönetici kimlik bilgilerinde bir değişiklik olursa yeniden çalıştırmanız gerekir.

1. Koruma grubu oluşturmak **için koruma**  >  **Eylemler**  >  **koruma grubu oluştur** ' a tıklayarak mabs konsolundaki **yeni koruma grubu oluşturma** Sihirbazı ' nı açın.

1. **Koruma grubu türünü seçin**içinde **sunucular**' ı seçin.

1. **Grup üyelerini seçin**içinde, WFE rolünü tutan sunucuyu genişletin. Birden çok WFE sunucusu varsa ConfigureSharePoint.exe yüklediğiniz birini seçin.

    SharePoint Server MABS sorguları VSS 'yi genişleterek, MABS 'lerin neleri koruyabileceği hakkında bilgi alın.  SharePoint veritabanı uzak ise, MABS buna bağlanır. SharePoint veri kaynakları görünmezse, VSS yazıcısının SharePoint sunucusunda ve uzak SQL Server çalışıp çalışmadığını ve MABS aracısının hem SharePoint sunucusunda hem de uzak SQL Server yüklü olduğundan emin olun. Ayrıca SharePoint veritabanlarının başka bir yerde SQL Server veritabanları olarak korunmadığından emin olun.

1. **Veri koruma yöntemini seçin**içinde, kısa ve uzun süreli yedeklemeyi nasıl işlemek istediğinizi belirtin \- . Kısa\-süreli yedekleme Azure yedekleme ile diskten Azure buluta \(kısa veya uzun\-süreli için\) yedekleme seçeneğiyle her zaman ilk olarak diske yapılır.

1. **Kısa \- vadeli hedefleri seçin**bölümünde diskte kısa süreli depolama için nasıl yedekleme yapmak istediğinizi belirtin \- .   **Bekletme aralığı** kısmında verileri disk üzerinde ne kadar saklamak istediğinizi belirtirsiniz. **Eşitleme sıklığı**' nda, diske artımlı yedeklemeyi ne sıklıkta çalıştırmak istediğinizi belirtirsiniz. Bir yedekleme aralığı ayarlamak istemiyorsanız, MABS 'lerin her bir kurtarma noktası zamanlanmadan hemen önce hızlı tam yedekleme çalıştırmaları için bir kurtarma noktasının hemen öncesine bakabilirsiniz.

1. Disk ayırmayı ıncele sayfasında, koruma grubu için ayrılmış depolama havuzu disk alanını gözden geçirin.

    **Toplam veri boyutu** , yedeklemek istediğiniz verilerin boyutudur ve **mabs üzerinde sağlanacak disk alanı** , mabs 'nin koruma grubu için önerdiği alandır. MABS, ayarlara bağlı olarak ideal yedekleme birimini seçer. Ancak, yedekleme birimi seçeneklerini **Disk ayırma ayrıntıları**'nda düzenleyebilirsiniz. İş yükleri için açılan menüden tercih edilen depolamayı seçin. Düzenlemeleriniz, **Kullanılabilir Disk Depolaması** bölmesindeki **Toplam Depolama** ve **Boş Depolama** değerlerini değiştirir. Yetersiz sağlanan alan, daha sonra yedeklemeler ile devam etmek için, MABS 'ların birime eklemenizi önerdiği depolama miktarıdır.

1. **Çoğaltma oluşturma yöntemini seçin**kısmında ilk tam veri çoğaltmasını nasıl işlemek istediğinizi seçin.  Ağ üzerinden çoğaltmayı seçerseniz, yoğun olmayan bir zaman seçmeniz önerilir. Büyük miktarlarda veri veya en iyi durumda olmayan ağ koşulları için, verileri çıkarılabilir medya kullanarak çevrimdışı olarak çoğaltmayı düşünebilirsiniz.

1. **Tutarlılık Denetimi Seçenekleri** kısmında, tutarlılık denetimlerinin nasıl otomatikleştirilmesini istediğinizi seçin. Veri çoğaltma tutarsız hale geldiğinde veya bir zamanlamaya göre çalıştırmak için bir denetim etkinleştirebilirsiniz. Otomatik tutarlılık denetimini yapılandırmak istemiyorsanız, MABS konsolunun **koruma** alanındaki koruma grubuna sağ tıklayarak ve **tutarlılık denetimi gerçekleştir**' i seçerek istediğiniz zaman el ile denetim gerçekleştirebilirsiniz.

1. Azure Backup ile buluta yedekleme seçtiyseniz, **Çevrimiçi koruma verilerini belirtin** sayfasında Azure'a yedeklemek istediğiniz iş yüklerinin seçili olduğundan emin olun.

1. **Çevrimiçi yedekleme zamanlamasını belirtin**kısmında, Azure 'a artımlı yedeklemelerin ne sıklıkta gerçekleşeceğini belirtin. Yedeklemeleri her gün/hafta/ay/yıl yapılacak şekilde ve çalışacakları saat/tarihi belirterek zamanlayabilirsiniz. Yedeklemeler günde iki kez gerçekleşebilir. Yedekleme her çalıştığında, Azure 'da MABS diskinde depolanan yedeklenen verilerin kopyasından bir veri kurtarma noktası oluşturulur.

1. **Çevrimiçi saklama Ilkesini belirtin**kısmında günlük/haftalık/aylık/yıllık yedeklerden oluşturulan kurtarma noktalarının Azure 'da nasıl korunacağını belirtebilirsiniz.

1. **Çevrimiçi çoğaltma Seç**' de, verilerin ilk tam çoğaltmasının nasıl yapılacağını belirtin. Ağ üzerinden çoğaltma veya çevrimdışı yedekleme (çevrimdışı dengeli dağıtım) yapabilirsiniz. Çevrimdışı yedekleme Azure İçe Aktarma özelliğini kullanır. [Daha fazla bilgi edinin](https://azure.microsoft.com/documentation/articles/backup-azure-backup-import-export/).

1. **Özet** sayfasında, ayarlarınızı gözden geçirin. **Grup Oluştur**' a tıkladıktan sonra, verilerin ilk çoğaltması oluşur. İşlem tamamlandığında, koruma grubunun durumu **durum** sayfasında **Tamam** olarak gösterilir. Ardından, koruma grupları ayarlarına uygun olarak yedekleme gerçekleşir.

## <a name="monitoring"></a>İzleme

Koruma grubu oluşturulduktan sonra ilk çoğaltma gerçekleşir ve MABS, SharePoint verilerini yedeklemeye ve eşitlemeye başlar. MABS, ilk eşitlemeyi ve sonraki yedeklemeleri izler.  SharePoint verilerini çeşitli şekillerde izleyebilirsiniz:

* Varsayılan MABS izlemeyi kullanarak, uyarıları yayımlayarak ve bildirimleri yapılandırarak proaktif izleme için bildirimler ayarlayabilirsiniz. Kritik, uyarı veya bilgilendirme amaçlı uyarılar için ve başlatılan kurtarma işlemlerinin durumu için e-posta ile bildirim gönderebilirsiniz.

* Operations Manager kullanıyorsanız, merkezi olarak uyarılar yayımlayabilirsiniz.

### <a name="set-up-monitoring-notifications"></a>Bildirimleri izleme işlevini ayarlama

1. Mabs Yönetici Konsolu, **izleme**  >  **eylemi**  >  **seçenekleri**' ne tıklayın.

2. **SMTP Sunucusu**'nu tıklayın; sunucu adını, bağlantı noktasını ve bildirimlerin çıkacağı e-posta adresini yazın. Adres geçerli olmalıdır.

3. **Kimliği DOĞRULANMıŞ SMTP sunucusu**alanına bir Kullanıcı adı ve parola yazın. Kullanıcı adı ve parola, "Kimden" adresi önceki adımda açıklanan kişinin etki alanı hesap adı olmalıdır. Aksi takdirde, bildirim teslimi başarısız olur.

4. SMTP sunucusu ayarlarını test etmek için **Test e-postası gönder**' e tıklayın, mabs 'nin test iletisini göndermesini istediğiniz e-posta adresini yazın ve ardından **Tamam**' a tıklayın. **Seçenekler**  >  **Bildirimler** ' e tıklayın ve alıcıların bildirim almak istediği uyarı türlerini seçin. **Alıcılar** ' de, mabs ' ın bildirimlerin kopyalarını göndermesini istediğiniz her alıcı için e-posta adresini yazın.

### <a name="publish-operations-manager-alerts"></a>Operations Manager uyarıları yayınlama

1. Mabs Yönetici Konsolu **izleme**  >  **eylemi**  >  **seçenekleri**  >  **uyarı yayımlama**  >  **Etkin uyarıları Yayımla** ' ya tıklayın.

2. **Uyarı yayımlamayı**etkinleştirdikten sonra, bir kullanıcı eylemi gerektiren tüm mevcut mabs uyarıları, **mabs uyarıları** olay günlüğüne yayımlanır. MABS sunucusunda yüklü olan Operations Manager Aracısı daha sonra bu uyarıları Operations Manager yayımlar ve yeni uyarılar oluşturulduğundan konsolu güncelleştirmeye devam eder.

## <a name="restore-a-sharepoint-item-from-disk-by-using-mabs"></a>MABS kullanarak bir SharePoint öğesini diskten geri yükleme

Aşağıdaki örnekte, *Kurtarma SharePoint öğesi* yanlışlıkla silinmiş ve kurtarılması gerekiyor.
![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)

1. **Mabs Yönetici Konsolu**açın. MABS tarafından korunan tüm SharePoint grupları, **koruma** sekmesinde gösterilir.

    ![MABS SharePoint Protection3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. Öğeyi kurtarmaya başlamak için **Kurtarma** sekmesini seçin.

    ![MABS SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. Bir kurtarma noktası aralığı içinde joker karakter tabanlı bir arama kullanarak *SharePoint öğesini kurtarmak* için SharePoint 'te arama yapabilirsiniz.

    ![MABS SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. Arama sonuçlarından uygun kurtarma noktasını seçin, öğeye sağ tıklayın ve ardından **kurtar**' ı seçin.
5. Ayrıca, çeşitli kurtarma noktalarına göz atabilir ve kurtarılacak bir veritabanı veya öğe seçebilirsiniz. **Tarih > kurtarma zamanı**' nı seçin ve ardından **> SharePoint grubu > kurtarma noktası > öğesi doğru veritabanını**seçin.

    ![MABS SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. Öğeye sağ tıklayın ve ardından **Kurtarma Sihirbazı**'nı açmak için **kurtar** ' ı seçin. **İleri**’ye tıklayın.

    ![Kurtarma seçimini İncele](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. Gerçekleştirmek istediğiniz kurtarma türünü seçin ve ardından **İleri**' ye tıklayın.

    ![Kurtarma türü](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > Örnekteki **orijinale kurtar** seçimi, öğeyi özgün SharePoint sitesine kurtarır.
   >
   >
8. Kullanmak istediğiniz **kurtarma işlemini** seçin.

   * SharePoint grubu değiştirilmediyse ve geri yüklenmekte olan kurtarma noktasıyla aynıysa, **Kurtarma grubu kullanmadan kurtar** ' ı seçin.
   * Kurtarma noktası oluşturulduktan sonra SharePoint grubu değiştiyse **Kurtarma grubu kullanarak kurtar** ' ı seçin.

     ![Kurtarma İşlemi](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. Veritabanını geçici olarak kurtarmak için bir hazırlama SQL Server örnek konumu sağlayın ve MABS üzerinde bir hazırlama dosyası paylaşma ve öğeyi kurtarmak için SharePoint çalıştıran sunucu sağlayın.

    ![Hazırlama location1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    MABS, SharePoint öğesini barındıran içerik veritabanını geçici SQL Server örneğine iliştirir. İçerik veritabanından, öğeyi kurtarır ve MABS üzerindeki hazırlama dosyası konumuna koyar. Hazırlama konumunda bulunan kurtarılan öğenin artık SharePoint grubundaki hazırlama konumuna aktarılması gerekir.

    ![Hazırlama Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. **Kurtarma seçeneklerini belirtin**' i seçin ve güvenlik ayarlarını SharePoint grubuna uygulayın veya kurtarma noktasının güvenlik ayarlarını uygulayın. **İleri**’ye tıklayın.

    ![Kurtarma seçenekleri](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > Ağ bant genişliği kullanımını azaltmayı seçebilirsiniz. Bu, üretim saatleri sırasında üretim sunucusuna etkisini en aza indirir.
    >
    >
11. Özet bilgilerini gözden geçirin ve ardından dosyayı kurtarmaya başlamak için **kurtar** ' ı tıklatın.

    ![Kurtarma Özeti](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. Şimdi kurtarma **durumunu** görüntülemek Için **Mabs Yönetici Konsolu** **izleme** sekmesini seçin.

    ![Kurtarma durumu](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > Dosya şimdi geri yüklendi. Geri yüklenen dosyayı denetlemek için SharePoint sitesini yenileyebilirsiniz.
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-mabs"></a>MABS kullanarak bir SharePoint veritabanını Azure 'dan geri yükleme

1. Bir SharePoint içerik veritabanını kurtarmak için, çeşitli kurtarma noktalarına göz atarak (daha önce gösterildiği gibi) ve geri yüklemek istediğiniz kurtarma noktasını seçin.

    ![MABS SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. SharePoint kurtarma noktasına çift tıklayarak kullanılabilir SharePoint kataloğu bilgilerini görüntüleyin.

   > [!NOTE]
   > SharePoint grubu Azure 'da uzun süreli saklama için korunduğundan, MABS sunucusunda hiçbir katalog bilgisi (meta veri) yok. Sonuç olarak, her zaman bir noktadan SharePoint içerik veritabanının kurtarılması gerektiğinde SharePoint grubunu yeniden kataloglanmalıdır.
   >
   >
3. **Yeniden katalog**' a tıklayın.

    ![MABS SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    **Bulut yeniden Kataloglandırma** durumu penceresi açılır.

    ![MABS SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    Kataloglama tamamlandıktan sonra durum *başarılı*olarak değişir. **Kapat**' a tıklayın.

    ![MABS SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. İçerik veritabanı yapısını almak için MABS **Kurtarma** sekmesinde gösterilen SharePoint nesnesine tıklayın. Öğeye sağ tıklayın ve ardından **kurtar**' a tıklayın.

    ![MABS SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Bu noktada, bir SharePoint içerik veritabanını diskten kurtarmak için bu makalenin önceki kısımlarında yer alarak kurtarma adımlarını izleyin.

## <a name="switching-the-front-end-web-server"></a>Ön uç Web sunucusunu değiştirme

Birden fazla ön uç Web sunucunuz varsa ve MABS 'nin grubu korumak için kullandığı sunucuyu değiştirmek istiyorsanız, yönergeleri izleyin:

Aşağıdaki yordam, *Sunucu1* ve *Sunucu2*olmak üzere iki ön uç Web sunucusuyla sunucu grubu örneğini kullanır. MABS, grubu korumak için *Sunucu1* kullanır. MABS 'nin, grubundan *Sunucu1* 'yi kaldırabilmeniz için bu ön uç Web sunucusunu *Sunucu2* olarak değiştirin.

> [!NOTE]
> MABS 'nin grubu korumak için kullandığı ön uç Web sunucusu kullanılamıyorsa, 4. adımdan başlayarak ön uç Web sunucusunu değiştirmek için aşağıdaki yordamı kullanın.

### <a name="to-change-the-front-end-web-server-that-mabs-uses-to-protect-the-farm"></a>MABS 'nin grubu korumak için kullandığı ön uç Web sunucusunu değiştirmek için

1. Bir komut isteminde aşağıdaki komutu çalıştırarak *Sunucu1* ÜZERINDE SharePoint VSS Yazıcı hizmetini durdurun:

    ```CMD
    stsadm -o unregisterwsswriter
    ```

1. *Sunucu1*'de, kayıt defteri düzenleyicisini açın ve aşağıdaki anahtara gidin:

   **HKLM\System\CCS\Services\VSS\VssAccessControl**

1. VssAccessControl alt anahtarında listelenen tüm değerleri kontrol edin. Herhangi bir girdinin değer verisi 0 ise ve ilişkili hesap kimlik bilgileri altında başka bir VSS yazıcısı çalışıyorsa, değer verisini 1 olarak değiştirin.

1. *Sunucu2*üzerine bir koruma Aracısı yükler.

   > [!WARNING]
   > Web ön uç sunucularını yalnızca her iki sunucu da aynı etki alanında olduğunda değiştirebilirsiniz.

1. *Sunucu2*'de, bir komut isteminde dizinini olarak değiştirin `_MABS installation location_\bin\` ve **ConfigureSharePoint**çalıştırın. ConfigureSharePoint hakkında daha fazla bilgi için bkz. [yedeklemeyi yapılandırma](#configure-backup).

1. Sunucu grubunun ait olduğu koruma grubunu seçin ve ardından **koruma grubunu değiştir**' e tıklayın.

1. Grubu Değiştir sihirbazında, **Grup üyelerini seçin** sayfasında, *Sunucu2* ' i genişletin ve sunucu grubunu seçin ve Sihirbazı doldurun.

   Tutarlılık denetimi başlar.

1. 6. adımı gerçekleştirdiyseniz, artık birimi koruma grubundan kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. [Exchange Server 'ı yedekleme](backup-azure-exchange-mabs.md) makalesi.
Bkz. [yedekleme SQL Server](backup-azure-sql-mabs.md) makalesi.
