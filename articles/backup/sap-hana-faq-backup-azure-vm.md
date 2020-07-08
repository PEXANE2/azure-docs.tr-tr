---
title: SSS - Azure VM’lerindeki SAP HANA veritabanlarını yedekleme
description: Bu makalede, Azure Backup hizmetini kullanarak SAP HANA veritabanlarının yedeklenmesi hakkında sık sorulan soruların yanıtlarını bulun.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: ddc4af9a164de3a822e8aebd6c0a4db769ec62a0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85262591"
---
# <a name="frequently-asked-questions--back-up-sap-hana-databases-on-azure-vms"></a>Sık sorulan sorular – Azure VM 'lerinde SAP HANA veritabanlarını yedekleyin

Bu makalede, Azure Backup hizmetini kullanarak SAP HANA veritabanlarının yedeklenmesi hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="backup"></a>Backup

### <a name="how-many-full-backups-are-supported-per-day"></a>Gün başına kaç tam yedekleme destekleniyor?

Günde yalnızca bir tam yedeklemeyi destekliyoruz. Değişiklik yedeklemesi ve tam yedekleme aynı günde tetiklenemez.

### <a name="do-successful-backup-jobs-create-alerts"></a>Başarılı yedekleme işleri sonucunda uyarı oluşturulur mu?

Hayır. Başarılı yedekleme işleri uyarı oluşturmaz. Uyarılar yalnızca başarısız olan yedekleme işleri için gönderilir. Portal uyarıları için ayrıntılı davranış [burada](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor)belgelenmiştir. Ancak, başarılı işler için bile uyarı almak istiyorsanız [Azure İzleyicisi](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)'ni kullanabilirsiniz.

### <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>Yedekleme Işleri menüsünde zamanlanmış yedekleme işlerini görebilir miyim?

Yedekleme Işi menüsü, yalnızca geçici yedekleme işlerini gösterir. Zamanlanan işler için [Azure izleyici](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)'yi kullanın.

### <a name="are-future-databases-automatically-added-for-backup"></a>Gelecekteki veritabanları yedekleme için otomatik olarak eklenir mi?

Hayır, bu şu anda desteklenmiyor.

### <a name="if-i-delete-a-database-from-an-instance-what-will-happen-to-the-backups"></a>Bir veritabanını bir örnekten silersem, yedeklemelere ne olur?

Bir veritabanı SAP HANA örneğinden bırakılırsa, veritabanı yedeklemeleri yine de denenir. Bu, silinen veritabanının **yedekleme öğeleri** altında sağlıksız olarak gösterilmeye başladığı ve hala korunduğu anlamına gelir.
Bu veritabanını korumayı durdurmanın doğru yolu, bu veritabanındaki **silme verileriyle Yedeklemeyi Durdur** gerçekleştirmektir.

### <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-the-behavior-be"></a>Korunduktan sonra veritabanının adını değiştirdiğimde, davranış ne olur?

Yeniden adlandırılmış bir veritabanı yeni bir veritabanı olarak değerlendirilir. Bu nedenle, hizmet bu durumu veritabanında bulunamamıştır ve yedeklemelerle başarısız olur. Yeniden adlandırılan veritabanı yeni bir veritabanı olarak görünür ve koruma için yapılandırılması gerekir.

### <a name="what-are-the-prerequisites-to-back-up-sap-hana-databases-on-an-azure-vm"></a>Azure VM 'de SAP HANA veritabanlarını yedeklemek için Önkoşullar nelerdir?

[Önkoşulları](tutorial-backup-sap-hana-db.md#prerequisites) ve [kayıt öncesi betiğin bölümler olduğunu](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) inceleyin.

### <a name="what-permissions-should-be-set-so-azure-can-back-up-sap-hana-databases"></a>Azure 'un SAP HANA veritabanlarını yedekleyebilmesi için hangi izinlerin ayarlanması gerekir?

Ön kayıt betiğini çalıştırmak, Azure 'un SAP HANA veritabanlarını yedeklemesini sağlamak için gerekli izinleri ayarlar. Ön kayıt betiğini [buradan](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does)daha fazla bulabilirsiniz.

### <a name="will-backups-work-after-migrating-sap-hana-from-sdc-to-mdc"></a>SAP HANA SDC 'den MDC 'ye geçirdikten sonra yedeklemeler çalışır mi?

Sorun giderme kılavuzunun [Bu bölümüne](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#sdc-to-mdc-upgrade-with-a-change-in-sid) bakın.

### <a name="can-azure-hana-backup-be-set-up-against-a-virtual-ip-load-balancer-and-not-a-virtual-machine"></a>Azure HANA yedekleme, sanal bir makine değil sanal IP 'ye (yük dengeleyici) göre ayarlanabilir mi?

Şu anda çözümü yalnızca bir sanal IP 'ye karşı ayarlama yeteneği yoktur. Çözümü yürütebilmemiz için bir sanal makineye ihtiyacımız var.

### <a name="how-can-i-move-an-on-demand-backup-to-the-local-file-system-instead-of-the-azure-vault"></a>İsteğe bağlı bir yedeklemeyi Azure Kasası yerine yerel dosya sistemine nasıl taşıyabilirim?

1. Şu anda çalışmakta olan yedeklemenin istenen veritabanında tamamlanmasını bekle (Studio 'nun tamamlanmasını denetle)
1. Aşağıdaki adımları kullanarak, günlük yedeklemelerini devre dışı bırakın ve istenen VERITABANı için Katalog yedeklemesini **dosya sistemine** ayarlayın:
1. **SystemDB**  ->  **yapılandırma**  ->  **veritabanı**  ->  **filtresi Seç (günlük)** öğesine çift tıklayın
    1. Enable_auto_log_backup **Hayır** olarak ayarla
    1. Log_backup_using_backint **false** olarak ayarla
1. İstenen veritabanında isteğe bağlı bir yedekleme yapın ve yedeklemenin ve kataloğun yedeğinin tamamlanmasını bekleyin.
1. Yedeklemelerin Azure kasasına akmasını sağlamak için önceki ayarlara geri dönün:
    1. Enable_auto_log_backup **Evet** olarak ayarla
    1. Log_backup_using_backint **true** olarak ayarla

### <a name="how-can-i-use-sap-hana-backup-with-my-hana-replication-set-up"></a>HANA çoğaltma ayarlarım SAP HANA yedeklemeyi nasıl kullanabilirim?

Şu anda Azure Backup bir HSR kurulumunu anlama yeteneği yoktur. Bu, HSR 'nin birincil ve ikincil düğümlerinin iki bireysel, ilişkisiz VM olarak kabul edildiği anlamına gelir. Öncelikle birincil düğümde yedekleme 'yi yapılandırmanız gerekir. Yük devretme gerçekleştiğinde, ikincil düğümde (şimdi birincil düğüm haline gelir) yedeklemenin yapılandırılması gerekir. Diğer düğüme yedeklemenin otomatik olarak devredilme işlemi yapılmaz.

Belirli bir zaman noktasında etkin (birincil) düğümden verileri yedeklemek için korumayı yük devretme sonrasında birincil olan ikincil düğüme **geçirebilirsiniz** .

Bu **anahtar korumasını**gerçekleştirmek için şu adımları izleyin:

- Birincil üzerinde [Korumayı Durdur](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database) (verileri koruyun)
- İkincil düğümde [ön kayıt betiğini](https://aka.ms/scriptforpermsonhana) çalıştırma
- İkincil düğümdeki [veritabanlarını bulun](tutorial-backup-sap-hana-db.md#discover-the-databases) ve bunların üzerinde [yedeklemeleri yapılandırın](tutorial-backup-sap-hana-db.md#configure-backup)

Bu adımların her yük devretme sonrasında el ile gerçekleştirilmesi gerekir. Bu adımları, Azure portal ek olarak komut satırı/HTTP REST aracılığıyla gerçekleştirebilirsiniz. Bu adımları otomatik hale getirmek için bir Azure runbook 'u kullanabilirsiniz.

Aşağıda, **anahtar korumasının** nasıl gerçekleştirilmesi gerektiğini gösteren ayrıntılı bir örnek verilmiştir:

Bu örnekte, HSR kurulumu 'nda düğüm 1 (birincil) ve düğüm 2 (ikincil) olmak üzere iki düğüm vardır.  Yedeklemeler, düğüm 1 ' de yapılandırılır. Yukarıda belirtildiği gibi, düğüm 2 ' de yedeklemeleri yapılandırmaya çalışmayın.

İlk yük devretme gerçekleştiğinde, düğüm 2 birincil olur. Ni

1. Verileri sakla seçeneğiyle, düğüm 1 ' in (önceki birincil) korunmasını durdurun.
1. Düğüm 2 ' de (artık birincil) kayıt öncesi betiği çalıştırın.
1. Düğüm 2 ' de veritabanlarını bulun, yedekleme ilkesi atayın ve yedeklemeleri yapılandırın.

Ardından, düğüm 2 ' de ilk tam yedekleme tetiklenir ve bu işlem tamamlandıktan sonra günlük yedeklemeleri başlar.

Sonraki yük devretme gerçekleştiğinde, 1. düğüm birincil ve düğüm 2 ikincil hale gelir. Şimdi işlemi yineleyin:

1. Verileri sakla seçeneğiyle node 2 korumasını durdurun.
1. Düğüm 1 ' de ön kayıt betiğini çalıştırın (Bu, birincili olur)
1. Ardından, düğüm 1 ' de [yedekleme işlemini](sap-hana-db-manage.md#resume-protection-for-an-sap-hana-database) gerekli ilkeyle (yedekleme, düğüm 1 ' de daha önce durdurulduğundan) kapatın.

Ardından, düğüm 1 ' de tam yedekleme tetiklenir ve bu işlem tamamlandıktan sonra günlük yedeklemeleri başlatılır.

## <a name="restore"></a>Geri Yükleme

### <a name="why-cant-i-see-the-hana-system-i-want-my-database-to-be-restored-to"></a>My Database 'in geri yüklenmesini istiyorum HANA sistemini neden göremiyorum?

Hedef SAP HANA örneğine geri yükleme için tüm önkoşulların karşılanıp karşılanmadığını denetleyin. Daha fazla bilgi için bkz. [Önkoşullar-Azure VM 'de SAP HANA veritabanlarını geri yükleme](https://docs.microsoft.com/azure/backup/sap-hana-db-restore#prerequisites).

### <a name="why-is-the-overwrite-db-restore-failing-for-my-database"></a>Veritabanım için üzerine yazma DB geri yüklemesi neden başarısız oluyor?

Geri yükleme sırasında **üzerine yazmayı zorla** seçeneğinin seçildiğinden emin olun.

### <a name="why-do-i-see-the-source-and-target-systems-for-restore-are-incompatible-error"></a>Neden "geri yükleme için kaynak ve hedef sistemler uyumsuz" hatası görüyorum?

Şu anda hangi geri yükleme türlerinin desteklendiğini görmek için SAP HANA Note [1642148](https://launchpad.support.sap.com/#/notes/1642148) bölümüne bakın.

### <a name="can-i-use-a-backup-of-a-database-running-on-sles-to-restore-to-an-rhel-hana-system-or-vice-versa"></a>SLES üzerinde çalışan bir veritabanının yedeklemesini bir RHEL HANA sistemine geri yüklemek veya bunun tersini yapabilir miyim?

Evet, SLES üzerinde çalışan bir HANA veritabanında tetiklenen akış yedeklemelerini kullanarak bir RHEL HANA sistemine geri yükleyebilir ve bunun tersini yapabilirsiniz. Diğer bir deyişle, akış yedeklemeleri kullanılarak platformlar arası geri yükleme mümkündür. Ancak, geri yüklemek istediğiniz HANA sisteminin ve geri yükleme için kullanılan HANA sisteminin hem SAP 'ye göre geri yükleme için uyumlu olduğundan emin olmanız gerekir. Hangi geri yükleme türlerinin uyumlu olduğunu görmek için SAP HANA Note [1642148](https://launchpad.support.sap.com/#/notes/1642148) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM 'lerinde çalışan [SAP HANA veritabanlarını nasıl yedekleyeceğinizi](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database) öğrenin.
