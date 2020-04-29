---
title: Azure kurtarma hizmetleri kasalarını ve sunucularını yönetme
description: Bu makalede, kurtarma hizmetleri kasalarınızı izlemek ve yönetmek için kurtarma hizmetleri Kasası genel bakış panosunu nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 07/08/2019
ms.openlocfilehash: 1a4d23c157700f42422cfe7ca8fa1c49e2cf128a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80131969"
---
# <a name="monitor-and-manage-recovery-services-vaults"></a>Kurtarma Hizmetleri kasalarını izleme ve yönetme

Bu makalede, kurtarma hizmetleri kasalarınızı izlemek ve yönetmek için kurtarma hizmetleri Kasası **genel bakış** panosunun nasıl kullanılacağı açıklanmaktadır. Listeden bir kurtarma hizmetleri Kasası açtığınızda, seçili kasa için **genel bakış** panosu açılır. Pano, kasa hakkında çeşitli ayrıntılar sağlar. Şunları gösteren *Kutucuklar* vardır: kritik ve uyarı uyarılarının, devam eden ve başarısız yedekleme işlerinin durumu ve kullanılan yerel olarak yedekli depolama (LRS) ve coğrafi olarak yedekli depolama (GRS) miktarı. Azure VM 'lerini kasaya yedeklebiliyorsanız, [ **yedekleme ön denetim durumu** kutucuğunda tüm kritik veya uyarı öğeleri görüntülenir](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#backup-pre-check-status). Aşağıdaki görüntü, **contoso Kasası**Için **genel bakış** panosuyla bulunur. **Yedekleme öğeleri** kutucuğu, kasaya kayıtlı dokuz öğe olduğunu gösterir.

![Kurtarma Hizmetleri Kasası panosu](./media/backup-azure-manage-windows-server/rs-vault-blade.png)

Bu makaleye yönelik önkoşullar: bir Azure aboneliği, bir kurtarma hizmetleri kasası ve kasa için yapılandırılmış en az bir yedekleme öğesi vardır.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="open-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını açma

Uyarıları izlemek veya bir kurtarma hizmetleri kasasıyla ilgili yönetim verilerini görüntülemek için kasayı açın.

1. Azure aboneliğinizi kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.

2. Portalda **tüm hizmetler**' e tıklayın.

   ![Kurtarma Hizmetleri kasalarının listesini açın 1. adım](./media/backup-azure-manage-windows-server/open-rs-vault-list.png)

3. **Tüm hizmetler** Iletişim kutusunda **Kurtarma Hizmetleri**yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasaları** seçeneği göründüğünde, aboneliğinizdeki kurtarma hizmetleri kasalarının listesini açmak için tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Kasaların listesinden, **genel bakış** panosunu açmak için bir kasaya tıklayın.

    ![Kurtarma Hizmetleri Kasası panosu](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Genel Bakış Panosu, uyarılar ve yedekleme işi verileri sağlamak için kutucukları kullanır.

## <a name="monitor-backup-jobs-and-alerts"></a>Yedekleme işlerini ve uyarılarını izleme

Kurtarma Hizmetleri Kasası **genel bakış** panosu, Izleme ve kullanım bilgileri için kutucuk sağlar. Izleme bölümündeki kutucuklar kritik ve uyarı uyarılarını, devam eden ve başarısız işleri görüntüler. Bu iş veya uyarı için filtrelenmiş yedekleme uyarıları veya yedekleme Işleri menüsünü açmak için belirli bir uyarıya veya işe tıklayın.

![Yedekleme panosu görevleri](./media/backup-azure-manage-windows-server/monitor-dashboard-tiles-warning.png)

Izleme bölümünde, önceden tanımlanmış **yedekleme uyarıları** ve **yedekleme işleri** sorgularının sonuçları gösterilmektedir. Izleme kutucukları hakkında güncel bilgiler sağlar:

* Yedekleme işleri için kritik ve uyarı uyarıları (son 24 saat)
* Azure VM 'lerinin ön denetim durumu. Ön denetim durumu hakkında tam bilgi için bkz. [yedekleme ön denetim durumu](#backup-pre-check-status).
* Devam eden yedekleme işleri ve başarısız olan işler (son 24 saat içinde).

Kullanım kutucukları şunları sağlar:

* Kasa için yapılandırılmış yedekleme öğelerinin sayısı.
* Kasa tarafından tüketilen Azure depolama (LRS ve GRS ile ayrılmış).

İlişkili menüyü açmak için kutucuklara (yedekleme depolaması hariç) tıklayın. Yukarıdaki görüntüde, yedekleme uyarıları kutucuğunda üç kritik uyarı gösterilmektedir. Yedekleme uyarıları kutucuğunda kritik uyarılar satırına tıkladığınızda kritik uyarılar için filtrelenmiş yedekleme uyarıları açılır.

![Kritik uyarılar için filtrelenmiş yedekleme uyarıları menüsü](./media/backup-azure-manage-windows-server/critical-backup-alerts.png)

Yukarıdaki görüntüde bulunan yedekleme uyarıları menüsü, şu şekilde filtrelenmiştir: durum etkindir, önem derecesi kritik ve zaman, önceki 24 saattir.

### <a name="backup-pre-check-status"></a>Yedekleme ön denetim durumu

Yedekleme ön denetimleri, yedeklemeleri olumsuz etkileyebilecek sorunlar için sanal makinelerinizin yapılandırmasını denetler. Bu bilgileri, doğrudan kurtarma hizmetleri Kasası panosundan görüntüleyebilmeniz ve dosya tutarlı veya uygulamayla tutarlı yedeklemelerin başarılı olmasını sağlamak için düzeltici ölçümlere yönelik öneriler sağlayabilmeniz için toplar. Altyapı gerektirmez ve hiçbir ek ücret vermez.  

Yedekleme ön denetimleri, Azure VM 'leriniz için zamanlanmış yedekleme işlemlerinin bir parçası olarak çalışır. Aşağıdaki durumlardan biriyle sonuçlarlar:

* **Başarılı**: Bu durum, sanal makinenizin yapılandırmasının başarılı yedeklemelere neden olması gerektiğini ve bir düzeltici eylem yapılması gerektiğini gösterir.
* **Uyarı**: Bu durum, VM yapılandırmasındaki yedekleme *hatalarına yol açabilecek* bir veya daha fazla sorunu gösterir. Başarılı yedeklemeler sağlamak için *Önerilen* adımları sağlar. Örneğin, en son VM aracısının yüklü olmaması, yedeklemelerin aralıklı olarak başarısız olmasına neden olabilir. Bu durum, bir uyarı durumu sağlar.
* **Kritik**: Bu durum, VM yapılandırmasında yedekleme hatalarına yol *açacak* ve başarılı yedeklemeler sağlamak için *gerekli* adımları sağlayan bir veya daha fazla kritik sorunu belirtir. Örneğin, bir VM 'nin NSG kurallarına neden olan bir ağ sorunu, sanal makinenin Azure Backup hizmetiyle iletişim kurmasını önlediği için yedeklemelerin başarısız olmasına neden olur. Bu durum kritik bir durum sağlar.

Kurtarma Hizmetleri kasasında VM yedeklemeleri için yedekleme ön denetimleri tarafından bildirilen tüm sorunları çözmeye başlamak için aşağıdaki adımları izleyin.

* Kurtarma Hizmetleri Kasası panosunda **yedekleme ön denetim durumu (Azure VM 'ler)** kutucuğunu seçin.
* Yedekleme ön denetim durumu **kritik** veya **Uyarı**olan herhangi bir VM 'yi seçin. Bu eylem **VM ayrıntıları** bölmesini açar.
* Yapılandırma sorunu açıklaması ve düzeltme adımlarını göstermek için bölmenin üst kısmındaki bölme bildirimini seçin.

## <a name="manage-backup-alerts"></a>Yedekleme uyarılarını yönetme

Yedekleme uyarıları menüsüne erişmek için, kurtarma hizmetleri Kasası menüsünde **yedekleme uyarıları**' na tıklayın.

![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/backup-alerts-menu.png)

Yedekleme uyarıları raporu, kasadaki uyarıları listeler.

![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/backup-alerts.png)

### <a name="alerts"></a>Uyarılar

Yedekleme uyarıları listesi, filtrelenmiş uyarılarla ilgili seçili bilgileri görüntüler. Yedekleme uyarıları menüsünde, kritik veya uyarı uyarıları için filtre uygulayabilirsiniz.

| Uyarı Düzeyi | Uyarı üreten olaylar |
| ----------- | ----------- |
| Kritik | Yedekleme işleri başarısız olduğunda, kurtarma işleri başarısız olduğunda ve bir sunucuda korumayı durdurduğunuzda ancak verileri koruduğunuz zaman kritik uyarılar alırsınız.|
| Uyarı | Şu durumlarda uyarı uyarısı alırsınız: yedekleme işleri uyarılarla tamamlandı, örneğin, bozulma sorunları nedeniyle 100 'den az dosya yedeklenmemişse veya 1.000.000 ' den fazla dosya başarıyla yedekleniyorsa). |
| Bilgilendirici | Şu anda, hiçbir bilgilendirme uyarısı kullanımda değil. |

### <a name="viewing-alert-details"></a>Uyarı ayrıntılarını görüntüleme

Yedekleme uyarıları raporu her uyarı hakkındaki sekiz ayrıntıyı izler. Rapordaki ayrıntıları düzenlemek için **sütun Seç** düğmesini kullanın.

![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/backup-alerts.png)

Varsayılan olarak, **en son oluşum süresi**dışında tüm ayrıntılar raporda görüntülenir.

* Uyarı
* Yedekleme öğesi
* Korumalı sunucu
* Severity
* Süre
* Oluşturulma Zamanı
* Durum
* En son oluşum zamanı

### <a name="change-the-details-in-alerts-report"></a>Uyarılar raporundaki ayrıntıları değiştirme

1. Rapor bilgilerini değiştirmek için, **yedekleme uyarıları** menüsünde **Sütunları Seç**' e tıklayın.

   ![Yedekleme uyarıları](./media/backup-azure-manage-windows-server/alerts-menu-choose-columns.png)

   **Sütunları seçin** menüsü açılır.

2. **Sütunları seçin** menüsünde, raporda görünmesini istediğiniz ayrıntıları seçin.

    ![Sütunları seçin menüsü](./media/backup-azure-manage-windows-server/choose-columns-menu.png)

3. Değişikliklerinizi kaydetmek için **bitti** ' ye tıklayın ve sütunları Seç menüsünü kapatın.

   Değişiklik yaparsanız ancak değişiklikleri tutmak istemiyorsanız, seçili son yapılandırmaya dönmek için **Sıfırla** ' ya tıklayın.

### <a name="change-the-filter-in-alerts-report"></a>Uyarılar raporundaki filtreyi değiştirme

Uyarıların önem derecesini, durumunu, başlangıç saatini ve bitiş saatini değiştirmek için **filtre** menüsünü kullanın.

> [!NOTE]
> Yedekleme uyarıları filtresinin düzenlenebilmesi, kasa genel bakış panosundaki kritik veya uyarı uyarılarını değiştirmez.
>  

1. Yedekleme uyarıları filtresini değiştirmek için, yedekleme uyarıları menüsünde **filtre**' ye tıklayın.

   ![Filtre menüsünü seçin](./media/backup-azure-manage-windows-server/alerts-menu-choose-filter.png)

   Filtre menüsü görüntülenir.

   ![Filtre menüsünü seçin](./media/backup-azure-manage-windows-server/filter-alert-menu.png)

2. Önem derecesini, durumu, başlangıç saatini veya bitiş saatini düzenleyin ve yaptığınız değişiklikleri kaydetmek için **bitti** ' ye tıklayın.

## <a name="configuring-notifications-for-alerts"></a>Uyarılar için bildirimleri yapılandırma

Uyarı veya kritik bir uyarı oluştuğunda e-posta oluşturmak için bildirimleri yapılandırın. Her saat veya belirli bir uyarı oluştuğunda e-posta uyarıları gönderebilirsiniz.

   ![Uyarıları filtrele](./media/backup-azure-manage-windows-server/configure-notification.png)

Varsayılan olarak, e-posta bildirimleri **Açık**. E-posta bildirimlerini durdurmak için **Kapat** ' a tıklayın.

**Bildirim** denetiminde, gruplandırma istemiyorsanız veya uyarı oluşturabilen birçok öğe yoksa **Uyarı başına** ' yı seçin. Her uyarı tek bir bildirimde (varsayılan ayar) ve bir çözümleme e-postası anında gönderilir.

**Saatlik Özet**' i seçerseniz, son bir saat içinde oluşturulan çözümlenmemiş uyarıları açıklayan alıcılara bir e-posta gönderilir. Saatin sonunda bir çözüm e-postası gönderilir.

E-posta oluşturmak için kullanılan uyarı önem derecesini (kritik veya uyarı) seçin. Şu anda hiç bilgi uyarısı yok.

## <a name="manage-backup-items"></a>Yedekleme öğelerini yönetme

Kurtarma Hizmetleri Kasası birçok yedekleme verisi türünü tutar. Neleri yedekleyebileceğiniz hakkında [daha fazla bilgi edinin](backup-overview.md#what-can-i-back-up) . Çeşitli sunucuları, bilgisayarları, veritabanlarını ve iş yüklerini yönetmek için, kasanın içeriğini görüntülemek için **yedekleme öğeleri** kutucuğuna tıklayın.

![Yedekleme öğeleri kutucuğu](./media/backup-azure-manage-windows-server/backup-items.png)

Yedekleme yönetim türüne göre düzenlenen yedekleme öğelerinin listesi açılır.

![Yedekleme öğelerinin listesi](./media/backup-azure-manage-windows-server/list-backup-items.png)

Belirli bir korumalı örnek türünü araştırmak için yedekleme yönetimi türü sütunundaki öğeye tıklayın. Örneğin, yukarıdaki görüntüde, bu kasada korunan iki Azure sanal makinesi vardır. **Azure sanal makinesi**' ne tıklayarak bu kasadaki korumalı sanal makinelerin listesini açar.

![Yedekleme türü listesi](./media/backup-azure-manage-windows-server/list-of-protected-virtual-machines.png)

Sanal makinelerin listesi yardımcı verilere sahiptir: ilişkili kaynak grubu, önceki [Yedekleme öncesi denetim](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#backup-pre-check-status), son yedekleme durumu ve en son geri yükleme noktası tarihi. Son sütundaki üç nokta, ortak görevleri tetiklemek için menüyü açar. Sütunlarda belirtilen faydalı veriler her yedekleme türü için farklıdır.

![Yedekleme türü listesi](./media/backup-azure-manage-windows-server/ellipsis-menu.png)

## <a name="manage-backup-jobs"></a>Yedekleme işlerini yönetme

Kasa panosundaki **yedekleme işleri** kutucuğu, son 24 saat içinde sürmekte olan veya başarısız olan işlerin sayısını gösterir. Kutucuk, yedekleme Işleri menüsüne bir göz atmak sağlar.

![Öğeleri ayarlardan yedekleme](./media/backup-azure-manage-windows-server/backup-jobs-tile.png)

İşlerle ilgili ek ayrıntıları görmek için **devam** ' a tıklayın veya bu durum Için filtrelenmiş yedekleme işleri **menüsünü açamadı.**

### <a name="backup-jobs-menu"></a>Yedekleme işleri menüsü

**Yedekleme işleri** menüsünde öğe türü, Işlem, durum, başlangıç zamanı ve süre hakkında bilgiler görüntülenir.  

Yedekleme Işleri menüsünü açmak için kasanın ana menüsünde **yedekleme işleri**' ne tıklayın.

![Öğeleri ayarlardan yedekleme](./media/backup-azure-manage-windows-server/backup-jobs-menu-item.png)

Yedekleme işlerinin listesi açılır.

![Öğeleri ayarlardan yedekleme](./media/backup-azure-manage-windows-server/backup-jobs-list.png)

Yedekleme Işleri menüsü, son 24 saat için tüm yedekleme türlerinde tüm işlemlerin durumunu gösterir. Filtreleri değiştirmek için **filtreyi** kullanın. Filtreler aşağıdaki bölümlerde açıklanmıştır.

Filtreleri değiştirmek için:

1. Kasa yedekleme Işleri menüsünde **filtre**' ye tıklayın.

   ![Öğeleri ayarlardan yedekleme](./media/backup-azure-manage-windows-server/vault-backup-job-menu-filter.png)

    Filtre menüsü açılır.

   ![Öğeleri ayarlardan yedekleme](./media/backup-azure-manage-windows-server/filter-menu-backup-jobs.png)

2. Filtre ayarlarını seçin ve **bitti**' ye tıklayın. Filtrelenmiş liste, yeni ayarlara göre yenilenir.

#### <a name="item-type"></a>Öğe türü

Öğe türü, korunan örneğin yedekleme yönetim türüdür. Dört tür vardır; aşağıdaki listeye bakın. Tüm öğe türlerini veya bir öğe türünü görüntüleyebilirsiniz. İki veya üç öğe türü seçemezsiniz. Kullanılabilir öğe türleri şunlardır:

* Tüm öğe türleri
* Azure sanal makine
* Dosyalar ve klasörler
* Azure Storage
* Azure iş yükü

#### <a name="operation"></a>İşlem

Bir işlemi veya tüm işlemleri görüntüleyebilirsiniz. İki veya üç işlem seçemezsiniz. Kullanılabilir Işlemler şunlardır:

* Tüm İşlemler
* Kaydettir
* Yedeklemeyi yapılandırma
* Backup
* Geri Yükleme
* Yedeklemeyi devre dışı bırak
* Yedekleme verilerini silme

#### <a name="status"></a>Durum

Tüm durumu veya birini görüntüleyebilirsiniz. İki veya üç durumu seçemezsiniz. Kullanılabilir durumlar şunlardır:

* Tüm durum
* Tamamlandı
* Devam ediyor
* Başarısız
* İptal edildi
* Uyarılarla tamamlandı

#### <a name="start-time"></a>Başlangıç saati

Sorgunun başladığı gün ve saat. Varsayılan değer 24 saattir.

#### <a name="end-time"></a>Bitiş saati

Sorgunun bittiği gün ve saat.

### <a name="export-jobs"></a>İşleri dışarı aktar

Tüm Işler menü bilgilerini içeren bir elektronik tablo oluşturmak için **Işleri dışarı aktar** ' ı kullanın. Elektronik tabloda, tüm işlerin özetini içeren bir sayfa ve her iş için ayrı sayfalar bulunur.

İş bilgilerini bir elektronik tabloya aktarmak için **Işleri dışarı aktar**' a tıklayın. Hizmet, kasanın ve tarihin adını kullanarak bir elektronik tablo oluşturur, ancak adı değiştirebilirsiniz.

## <a name="monitor-backup-usage"></a>Yedekleme kullanımını izle

Panodaki yedekleme depolama kutucuğu, Azure 'da tüketilen depolamayı gösterir. İçin depolama kullanımı sağlanır:

* Kasayla ilişkili bulut LRS depolama kullanımı
* Kasayla ilişkili bulut GRS depolama alanı kullanımı

## <a name="troubleshooting-monitoring-issues"></a>İzleme sorunlarını giderme

**Sorun:** Azure Backup aracısındaki işler ve/veya uyarılar portalda görünmez.

**Sorun giderme adımları:** İşlem ```OBRecoveryServicesManagementAgent```, Azure Backup hizmetine iş ve uyarı verilerini gönderir. Bazen bu işlem, takılmış veya kapalı hale gelebilir.

1. İşlemin çalışmadığını doğrulamak için, **Görev Yöneticisi**'ni açın ve denetleyin ```OBRecoveryServicesManagementAgent``` ' ı çalıştırın.

2. İşlem çalışmıyorsa, **Denetim Masası**'nı açın ve hizmetler listesine gözatamazsınız. **Microsoft Azure Kurtarma Hizmetleri yönetim aracısını**başlatın veya yeniden başlatın.

    Daha fazla bilgi için şu adreste günlüklere gözatamazsınız:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Örneğin:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Sonraki adımlar

* [Windows Server veya Windows Istemcisini Azure 'dan geri yükleme](backup-azure-restore-windows-server.md)
* Azure Backup hakkında daha fazla bilgi edinmek için bkz. [Azure Backup genel bakış](backup-introduction-to-azure-backup.md)
