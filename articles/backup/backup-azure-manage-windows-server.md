---
title: Azure Kurtarma Hizmetleri kasalarını ve sunucularını yönetme
description: Bu makalede, Kurtarma Hizmetleri kasalarınızı izlemek ve yönetmek için Kurtarma Hizmetleri kasasına Genel Bakış panosunu nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 07/08/2019
ms.openlocfilehash: 1a4d23c157700f42422cfe7ca8fa1c49e2cf128a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131969"
---
# <a name="monitor-and-manage-recovery-services-vaults"></a>Kurtarma Hizmetleri kasalarını izleme ve yönetme

Bu makalede, Kurtarma Hizmetleri kasalarınızı izlemek ve yönetmek için Kurtarma Hizmetleri kasasına **Genel Bakış** panosunun nasıl kullanılacağı açıklanmaktadır. Listeden bir Kurtarma Hizmetleri kasası açtığınızda, seçili kasanın **Genel Bakış** panosu açılır. Pano kasa hakkında çeşitli ayrıntılar sağlar. Gösteren *kutucuklar* vardır: kritik ve uyarı uyarılarının durumu, devam eden ve başarısız yedekleme işleri ve kullanılan yerel yedekdepolama (LRS) ve coğrafi yedekli depolama (GRS) miktarı. Azure VM'leri kasaya yedeklerseniz, [ **Yedek Ön Denetim Durumu** döşemesi kritik veya uyarı öğelerini görüntüler.](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#backup-pre-check-status) Aşağıdaki resim **Contoso-vault**için **Genel Bakış** panosudur. **Yedek Öğeler** döşemesi kasaya kayıtlı dokuz öğe olduğunu gösterir.

![kurtarma hizmetleri kasa panosu](./media/backup-azure-manage-windows-server/rs-vault-blade.png)

Bu makalenin ön koşulları şunlardır: Bir Azure aboneliği, Kurtarma Hizmetleri kasası ve kasa için yapılandırılan en az bir yedek öğe vardır.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="open-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası açma

Uyarıları izlemek veya Kurtarma Hizmetleri kasası hakkındaki yönetim verilerini görüntülemek için kasayı açın.

1. Azure aboneliğinizi kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.

2. Portalda Tüm **hizmetler'i**tıklatın.

   ![Kurtarma Hizmetleri tonozları adım 1 açık listesi](./media/backup-azure-manage-windows-server/open-rs-vault-list.png)

3. Tüm **hizmetler** iletişim kutusunda Kurtarma **Hizmetleri**yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Kurtarma **Hizmetleri kasaları** seçeneği göründüğünde, aboneliğinizdeki Kurtarma Hizmetleri kasalarının listesini açmak için bu seçeneği tıklatın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Kasa listesinden, **Genel Bakış** panosunu açmak için bir kasaya tıklayın.

    ![kurtarma hizmetleri kasa panosu](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Genel Bakış panosu, uyarıları ve yedekleme iş verilerini sağlamak için kutucukları kullanır.

## <a name="monitor-backup-jobs-and-alerts"></a>Yedekleme işlerini ve uyarıları izleme

Kurtarma Hizmetleri kasasına **Genel Bakış** panosu, İzleme ve Kullanım bilgileri için kutucuklar sağlar. İzleme bölümündeki kutucuklar Kritik ve Uyarı uyarılarını görüntüler ve devam eden ve başarısız olan işlerde. Bu iş veya uyarı için filtre uygulanmış Yedekleme Uyarıları veya Yedek İşler menüsünü açmak için belirli bir uyarıyı veya işi tıklatın.

![Yedekleme panosu görevleri](./media/backup-azure-manage-windows-server/monitor-dashboard-tiles-warning.png)

İzleme bölümü önceden tanımlanmış Yedekleme **Uyarıları** ve **Yedekleme İşleri** sorgularının sonuçlarını gösterir. İzleme kutucukları aşağıdakiler hakkında güncel bilgiler sağlar:

* Yedekleme işleri için kritik ve Uyarı uyarıları (son 24 saat içinde)
* Azure VM'leri için ön kontrol durumu. Ön kontrol durumu hakkında tam bilgi için [Yedek Ön Kontrol Durumu'na](#backup-pre-check-status)bakın.
* Yedekleme işleri devam ediyor ve başarısız olan işler (son 24 saat içinde).

Kullanım kutucukları şunları sağlar:

* Kasa için yapılandırılan Yedekleme öğelerinin sayısı.
* Kasa tarafından tüketilen Azure depolama (LRS ve GRS ile ayrılmıştır).

İlişkili menüyü açmak için kutucukları (Yedek Depolama hariç) tıklatın. Yukarıdaki resimde, Yedekleme Uyarıları döşemesi üç Kritik uyarı gösterir. Yedekleme Uyarıları döşemesinde Kritik uyarılar satırını tıklattığınızda, Kritik uyarılar için filtre uygulanmış Yedekleme Uyarıları açılır.

![Kritik uyarılar için filtreuygulanmış yedekleme uyarıları menüsü](./media/backup-azure-manage-windows-server/critical-backup-alerts.png)

Yukarıdaki resimde Yedek Uyarılar menüsü şu şekilde filtrelenir: Durum Etkin, Önem Önemli ve zaman önceki 24 saattir.

### <a name="backup-pre-check-status"></a>Yedekleme Ön Kontrol Durumu

Yedek Ön Denetimler, yedeklemeleri olumsuz etkileyebilecek sorunlar için VM'lerinizin yapılandırmalarını denetler. Bu bilgileri, doğrudan Kurtarma Hizmetleri Vault panosundan görüntüleyebilmeniz ve başarılı dosya tutarlısı veya uygulama tutarlı yedeklemeler sağlamak için düzeltici önlemler için öneriler sunabilmeniz için toplarlar. Altyapı gerektirmezler ve ek bir maliyeti yoktur.  

Yedekleme Ön Denetimleri, Azure VM'leriniz için zamanlanmış yedekleme işlemlerinin bir parçası olarak çalıştırın. Aşağıdaki devletlerden biriyle sonuçlandırırlar:

* **Geçti**: Bu durum, VM yapılandırmanızın başarılı yedeklemelere yol açması gerektiğini ve düzeltici eylemin yapılması gerekmediğini gösterir.
* **Uyarı**: Bu durum, VM yapılandırmasında yedekleme hatalarına *yol* açabilecek bir veya daha fazla sorunu gösterir. Başarılı yedeklemeler sağlamak için *önerilen* adımları sağlar. Örneğin, en son VM Aracısının yüklü olmaması yedeklemelerin zaman zaman başarısız olmasına neden olabilir. Bu durum bir uyarı durumu sağlayacaktır.
* **Kritik**: Bu durum, VM yapılandırmasında yedekleme hatalarına yol *açacak* ve başarılı yedeklemeler sağlamak için *gerekli* adımları sağlayacak bir veya daha fazla kritik sorunları gösterir. Örneğin, bir VM'nin NSG kurallarına yapılan bir güncelleştirmeden kaynaklanan bir ağ sorunu, VM'nin Azure Yedekleme hizmetiyle iletişim kurmasını engellediği için yedeklemelerin başarısız olmasına neden olur. Bu durum kritik bir durum sağlayacaktır.

Kurtarma Hizmetleri Kasası'nızdaki VM yedeklemeleri için Yedekleme Ön Denetimleri tarafından bildirilen sorunları çözmeye başlamak için aşağıdaki adımları izleyin.

* Kurtarma Hizmetleri Kasası panosunda **Yedek Ön Kontrol Durumu (Azure VM' ler)** döşemesini seçin.
* **Kritik** veya **Uyarı**yedekleme ön kontrol durumuna sahip herhangi bir VM seçin. Bu eylem **VM ayrıntıları** bölmesini açar.
* Yapılandırma sorunu açıklamasını ve düzeltici adımları ortaya çıkarmak için bölmenin üst kısmındaki bölme bildirimini seçin.

## <a name="manage-backup-alerts"></a>Yedekleme uyarılarını yönetme

Yedekleme Uyarıları menüsüne erişmek için, Kurtarma Hizmetleri kasası menüsünde **Yedek Uyarıları'nı**tıklatın.

![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/backup-alerts-menu.png)

Yedekleme Uyarıları raporu kasa için uyarıları listeler.

![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/backup-alerts.png)

### <a name="alerts"></a>Uyarılar

Yedek Uyarılar listesi, filtre uygulanmış uyarılar için seçili bilgileri görüntüler. Yedekleme Uyarıları menüsünde Kritik veya Uyarı uyarılarına filtre uygulayabilirsiniz.

| Uyarı Düzeyi | Uyarı oluşturan olaylar |
| ----------- | ----------- |
| Kritik | Kritik uyarıları alırsınız: Yedekleme işleri başarısız oldu, kurtarma işleri başarısız oldu ve sunucudaki korumayı durdurduğunuzda, ancak verileri korudunuz.|
| Uyarı | Uyarı uyarıları alırsınız: Örneğin, yolsuzluk sorunları nedeniyle 100'den az dosya yedeklenmediğinde veya 1.000.000'den fazla dosya başarıyla yedeklendiğinde uyarılarla birlikte yedekleme işleri alırsınız). |
| Bilgilendirici | şu anda, hiçbir bilgilendirme uyarıları kullanımdadır. |

### <a name="viewing-alert-details"></a>Uyarı ayrıntılarını görüntüleme

Yedekleme Uyarıları raporu, her bir uyarıyla ilgili sekiz ayrıntıyı izler. Rapordaki ayrıntıları sıralamak için **sütunları seç** düğmesini kullanın.

![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/backup-alerts.png)

Varsayılan olarak, Son **Oluşum Süresi**dışındaki tüm ayrıntılar raporda görünür.

* Uyarı
* Yedek Öğe
* Korumalı Sunucu
* Severity
* Süre
* Yaratılış Zamanı
* Durum
* En Son Oluşum Zamanı

### <a name="change-the-details-in-alerts-report"></a>Uyarılar raporundaki ayrıntıları değiştirme

1. Rapor bilgilerini değiştirmek için **Yedekleme Uyarıları** menüsünde **sütunları seç'i**tıklatın.

   ![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/alerts-menu-choose-columns.png)

   **Sütunları Seç** menüsü açılır.

2. **Sütunları Seç** menüsünde, raporda görünmesini istediğiniz ayrıntıları seçin.

    ![Sütunlar menüsünü seçin](./media/backup-azure-manage-windows-server/choose-columns-menu.png)

3. Değişikliklerinizi kaydetmek ve sütunları seç menüsünü kapatmak için **Bitti'yi** tıklatın.

   Değişiklik yapar, ancak değişiklikleri tutmak istemezseniz, seçili yapılandırmayı son kaydedilen yapılandırmaya döndürmek için **Sıfırla'yı** tıklatın.

### <a name="change-the-filter-in-alerts-report"></a>Uyarılar raporundafiltreyi değiştirme

Uyarıların Önem, Durum, Başlangıç saati ve Bitiş saatini değiştirmek için **Filtre** menüsünü kullanın.

> [!NOTE]
> Yedekleme Uyarıları filtresinin düzenlenmesi, kasa genel bakış panosundaki Kritik veya Uyarı uyarılarını değiştirmez.
>  

1. Yedekleme Uyarıları filtresini değiştirmek için, Yedekleme Uyarıları menüsünde **Filtre'yi**tıklatın.

   ![Filtre menüsünü seçin](./media/backup-azure-manage-windows-server/alerts-menu-choose-filter.png)

   Filtre menüsü görüntülenir.

   ![Filtre menüsünü seçin](./media/backup-azure-manage-windows-server/filter-alert-menu.png)

2. Önem, Durum, Başlangıç zamanı veya Bitiş saatini edin ve değişikliklerinizi kaydetmek için **Bitti'yi** tıklatın.

## <a name="configuring-notifications-for-alerts"></a>Uyarılar için bildirimleri yapılandırma

Uyarı veya Kritik uyarı oluştuğunda bildirimleri e-posta oluşturacak şekilde yapılandırın. Her saat veya belirli bir uyarı oluştuğunda e-posta uyarıları gönderebilirsiniz.

   ![Filtre uyarıları](./media/backup-azure-manage-windows-server/configure-notification.png)

Varsayılan olarak, E-posta bildirimleri **Açıktır.** E-posta bildirimlerini durdurmak için **Kapat'ı** tıklatın.

**Bildirim** denetiminde, gruplandırma istemiyorsanız veya uyarı oluşturabilecek çok sayıda öğeniz yoksa **Uyarı Başına'yı** seçin. Her uyarı bir bildirimle (varsayılan ayar) sonuçlanır ve hemen bir çözüm e-postası gönderilir.

**Saatlik Özet'i**seçerseniz, alıcılara son bir saat içinde oluşturulan çözülmemiş uyarıları açıklayan bir e-posta gönderilir. Bir çözüm e-postası saatin sonunda gönderilir.

E-posta oluşturmak için kullanılan uyarı önem derecesini (Kritik veya Uyarı) seçin. Şu anda bilgi uyarısı yok.

## <a name="manage-backup-items"></a>Yedekleme öğelerini yönetme

Kurtarma Hizmetleri kasası birçok yedekleme verisi türüne sahiptir. Neleri yedekleyebilirsiniz hakkında [daha fazla bilgi edinin.](backup-overview.md#what-can-i-back-up) Çeşitli sunucuları, bilgisayarları, veritabanlarını ve iş yüklerini yönetmek için kasanın içeriğini görüntülemek için **Yedek Öğeler** döşemesini tıklatın.

![Yedek öğeler döşemesi](./media/backup-azure-manage-windows-server/backup-items.png)

Yedekleme Yönetimi Türü tarafından düzenlenen Yedekleme Öğeleri listesi açılır.

![Yedekleme öğeleri listesi](./media/backup-azure-manage-windows-server/list-backup-items.png)

Belirli bir korumalı örnek türünü keşfetmek için Yedekleme Yönetimi Türü sütunundaki öğeyi tıklatın. Örneğin, yukarıdaki resimde, bu kasada korunan iki Azure sanal makinesi vardır. Azure **Sanal Makine'yi**tıklattığınızda, bu kasada korumalı sanal makinelerin listesini açar.

![Yedekleme türü listesi](./media/backup-azure-manage-windows-server/list-of-protected-virtual-machines.png)

Sanal makinelerin listesi yararlı verilere sahiptir: ilişkili Kaynak Grubu, önceki [Yedekleme Ön Denetimi,](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#backup-pre-check-status)Son Yedekleme Durumu ve en son Geri Yükleme Noktası'nın tarihi. Elipsler, son sütunda, ortak görevleri tetiklemek için menüyü açar. Sütunlarda sağlanan yararlı veriler, her yedekleme türü için farklıdır.

![Yedekleme türü listesi](./media/backup-azure-manage-windows-server/ellipsis-menu.png)

## <a name="manage-backup-jobs"></a>Yedekleme işlerini yönetme

Kasa panosundaki **Yedekleme İşleri** döşemesi, son 24 saat içinde devam eden veya başarısız olan iş sayısını gösterir. Döşeme, Yedekleme İşleri menüsüne bir bakış sağlar.

![Ayarlardan öğeleri yedekleme](./media/backup-azure-manage-windows-server/backup-jobs-tile.png)

İşler le ilgili ek ayrıntıları görmek için Devam **Eden** veya Bu durum için filtre uygulanmış Yedek İşler menüsünü açmak için **Başarısız'ı** tıklatın.

### <a name="backup-jobs-menu"></a>Yedek işler menüsü

**Yedek İşler** menüsü, Madde türü, Çalışma, Durum, Başlangıç Saati ve Süre hakkındaki bilgileri görüntüler.  

Yedek İşler menüsünü açmak için kasanın ana menüsünde **Yedek İşler'i**tıklatın.

![Ayarlardan öğeleri yedekleme](./media/backup-azure-manage-windows-server/backup-jobs-menu-item.png)

Yedekleme işlerinin listesi açılır.

![Ayarlardan öğeleri yedekleme](./media/backup-azure-manage-windows-server/backup-jobs-list.png)

Yedek İşler menüsü, son 24 saat için tüm yedekleme türlerinde tüm işlemlerin durumunu gösterir. Filtreleri değiştirmek için **Filtre'yi** kullanın. Filtreler aşağıdaki bölümlerde açıklanmıştır.

Filtreleri değiştirmek için:

1. Kasa Yedek İşler menüsünde **Filtre'yi**tıklatın.

   ![Ayarlardan öğeleri yedekleme](./media/backup-azure-manage-windows-server/vault-backup-job-menu-filter.png)

    Filtre menüsü açılır.

   ![Ayarlardan öğeleri yedekleme](./media/backup-azure-manage-windows-server/filter-menu-backup-jobs.png)

2. Filtre ayarlarını seçin ve **Bitti'yi**tıklatın. Filtre uygulanmış liste yeni ayarlara göre yenilenir.

#### <a name="item-type"></a>Madde türü

Madde türü, korumalı örneğin yedekleme yönetim türüdür. Dört türü vardır; aşağıdaki listeye bakın. Tüm madde türlerini veya bir öğe türünü görüntüleyebilirsiniz. İki veya üç öğe türü seçemezsiniz. Kullanılabilir Madde türleri şunlardır:

* Tüm madde türleri
* Azure sanal makine
* Dosyalar ve klasörler
* Azure Storage
* Azure iş yükü

#### <a name="operation"></a>İşlem

Bir işlemi veya tüm işlemleri görüntüleyebilirsiniz. İki veya üç işlem seçemezsiniz. Kullanılabilir İşlemler şunlardır:

* Tüm İşlemler
* Kaydettir
* Yedeklemeyi yapılandırma
* Backup
* Geri Yükleme
* Yedeklemeyi devre dışı
* Yedekleme verilerini silme

#### <a name="status"></a>Durum

Tüm Durum veya bir görüntüleyebilirsiniz. İki veya üç durum seçemezsiniz. Kullanılabilir durumlar şunlardır:

* Tüm Durum
* Tamamlandı
* Devam ediyor
* Başarısız
* İptal edildi
* Uyarılarla tamamlandı

#### <a name="start-time"></a>Başlangıç saati

Sorgunun başladığı gün ve saat. Varsayılan değer 24 saatlik bir dönemdir.

#### <a name="end-time"></a>Bitiş saati

Sorgunun sona erdince gün ve saat.

### <a name="export-jobs"></a>Dışa aktarma işleri

Tüm İşler menüsü bilgilerini içeren bir elektronik tablo oluşturmak için **Dışa** Aktarma işlerini kullanın. Elektronik tablo, tüm işlerin bir özetini ve her iş için tek tek sayfaları tutan bir sayfaya sahiptir.

İş bilgilerini elektronik tabloya aktarmak **için, işleri dışa**aktarma'yı tıklatın. Hizmet kasa ve tarih adını kullanarak bir elektronik tablo oluşturur, ancak adı değiştirebilirsiniz.

## <a name="monitor-backup-usage"></a>Yedekleme kullanımını izleyin

Panodaki Yedek Depolama döşemesi, Azure'da tüketilen depolama alanını gösterir. Depolama alanı kullanımı için sağlanır:

* Kasayla ilişkili Bulut LRS depolama alanı kullanımı
* Kasa ile ilişkili Cloud GRS depolama alanı kullanımı

## <a name="troubleshooting-monitoring-issues"></a>Sorun giderme izleme sorunları

**Sorun:** Azure Yedekleme aracısının işleri ve/veya uyarıları portalda görünmez.

**Sorun giderme adımları:** İşlem, ```OBRecoveryServicesManagementAgent```işi gönderir ve verileri Azure Yedekleme hizmetine uyarır. Bazen bu işlem sıkışmış veya kapatma olabilir.

1. İşlemin çalışmadığını doğrulamak için **Görev Yöneticisi'ni**açın ve denetim ```OBRecoveryServicesManagementAgent``` çalışıyor.

2. İşlem çalışmıyorsa, Denetim **Masası'nı**açın ve hizmet listesine göz atın. Microsoft Azure **Kurtarma Hizmetleri Yönetim**Aracısı'nı başlatın veya yeniden başlatın.

    Daha fazla bilgi için günlüklere şu şekilde göz atın:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Örneğin:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'dan Windows Server veya Windows İstemci Geri Yükleme](backup-azure-restore-windows-server.md)
* Azure Yedekleme hakkında daha fazla bilgi edinmek için Azure [Yedekleme genel bakış](backup-introduction-to-azure-backup.md)
