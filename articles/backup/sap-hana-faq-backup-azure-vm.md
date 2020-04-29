---
title: SSS - Azure VM’lerindeki SAP HANA veritabanlarını yedekleme
description: Bu makalede, Azure Backup hizmetini kullanarak SAP HANA veritabanlarının yedeklenmesi hakkında sık sorulan soruların yanıtlarını bulun.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: a46c4d6cccc00452a56567880400ef5779e6aed4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80155401"
---
# <a name="frequently-asked-questions--back-up-sap-hana-databases-on-azure-vms"></a>Sık sorulan sorular – Azure VM 'lerinde SAP HANA veritabanlarını yedekleyin

Bu makalede, Azure Backup hizmetini kullanarak SAP HANA veritabanlarının yedeklenmesi hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="backup"></a>Backup

### <a name="how-many-full-backups-are-supported-per-day"></a>Gün başına kaç tam yedekleme destekleniyor?

Günde yalnızca bir tam yedeklemeyi destekliyoruz. Değişiklik yedeklemesi ve tam yedekleme aynı günde tetiklenemez.

### <a name="do-successful-backup-jobs-create-alerts"></a>Başarılı yedekleme işleri sonucunda uyarı oluşturulur mu?

Hayır. Başarılı yedekleme işleri uyarı oluşturmaz. Uyarılar yalnızca başarısız olan yedekleme işleri için gönderilir. Portal uyarıları için ayrıntılı davranış [burada](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor)belgelenmiştir. Ancak, başarılı işler için bile uyarı yaşamaya ilgileniyorsanız [Azure izleyici](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)'yi kullanabilirsiniz.

### <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>Yedekleme Işleri menüsünde zamanlanmış yedekleme işlerini görebilir miyim?

Yedekleme Işi menüsü, yalnızca geçici yedekleme işlerini gösterir. Zamanlanan işler için [Azure izleyici](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)'yi kullanın.

### <a name="are-future-databases-automatically-added-for-backup"></a>Gelecekteki veritabanları yedekleme için otomatik olarak eklenir mi?

Hayır, bu, şu anda desteklenmiyor.

### <a name="if-i-delete-a-database-from-an-instance-what-will-happen-to-the-backups"></a>Bir veritabanını bir örnekten silersem, yedeklemelere ne olur?

Bir veritabanı SAP HANA örneğinden bırakılırsa, veritabanı yedeklemeleri yine de denenir. Bu, silinen veritabanının **yedekleme öğeleri** altında sağlıksız olarak gösterilmeye başladığı ve hala korunduğu anlamına gelir.
Bu veritabanını korumayı durdurmanın doğru yolu, bu veritabanındaki **silme verileriyle Yedeklemeyi Durdur** gerçekleştirmektir.

### <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-the-behavior-be"></a>Korunduktan sonra veritabanının adını değiştirdiğimde, davranış ne olur?

Yeniden adlandırılmış bir veritabanı yeni bir veritabanı olarak değerlendirilir. Bu nedenle, hizmet bu durumu veritabanı bulunamamıştır ve yedeklemelerle başarısız olur. Yeniden adlandırılan veritabanı yeni bir veritabanı olarak görünür ve koruma için yapılandırılması gerekir.

### <a name="what-are-the-prerequisites-to-back-up-sap-hana-databases-on-an-azure-vm"></a>Azure VM 'de SAP HANA veritabanlarını yedeklemek için Önkoşullar nelerdir?

[Önkoşulları](tutorial-backup-sap-hana-db.md#prerequisites) ve [kayıt öncesi betiğin bölümler olduğunu](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) inceleyin.

### <a name="what-permissions-should-be-set-for-azure-to-be-able-to-back-up-sap-hana-databases"></a>Azure 'un SAP HANA veritabanlarını yedekleyebilmesi için hangi izinlerin ayarlanması gerekir?

Ön kayıt betiğini çalıştırmak, Azure 'un SAP HANA veritabanlarını yedeklemesini sağlamak için gerekli izinleri ayarlar. Ön kayıt betiğini [buradan](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does)daha fazla bulabilirsiniz.

### <a name="will-backups-work-after-migrating-sap-hana-from-10-to-20"></a>SAP HANA 1,0 ' den 2,0 ' e geçirildikten sonra yedeklemeler çalışır mi?

Sorun giderme kılavuzunun [Bu bölümüne](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#upgrading-from-sap-hana-10-to-20) bakın.

### <a name="can-azure-hana-backup-be-set-up-against-a-virtual-ip-load-balancer-and-not-a-virtual-machine"></a>Azure HANA yedekleme, sanal bir makine değil sanal IP 'ye (yük dengeleyici) göre ayarlanabilir mi?

Şu anda çözümü yalnızca bir sanal IP 'ye karşı ayarlama yeteneği yoktur. Çözümü yürütebilmemiz için bir sanal makineye ihtiyacımız var.

### <a name="i-have-a-sap-hana-system-replication-hsr-how-should-i-configure-backup-for-this-setup"></a>SAP HANA sistem çoğalttım var (HSR), bu kurulum için yedeklemeyi nasıl yapılandırmalıyım?

HSR 'nin birincil ve ikincil düğümleri iki bireysel, ilişkili olmayan VM olarak değerlendirilir. Birincil düğümde yedeklemeyi yapılandırmanız gerekir ve yük devretme gerçekleştiğinde, ikincil düğümde (bundan böyle birincil düğüm haline gelir) yedeklemeyi yapılandırmanız gerekir. Diğer düğüme yedekleme için otomatik olarak ' yük devretme ' yok.

## <a name="restore"></a>Geri Yükleme

### <a name="why-cant-i-see-the-hana-system-i-want-my-database-to-be-restored-to"></a>My Database 'in geri yüklenmesini istiyorum HANA sistemini neden göremiyorum?

Hedef SAP HANA örneğine geri yükleme için tüm önkoşulların karşılanıp karşılanmadığını denetleyin. Daha fazla bilgi için bkz. [Önkoşullar-Azure VM 'de SAP HANA veritabanlarını geri yükleme](https://docs.microsoft.com/azure/backup/sap-hana-db-restore#prerequisites).

### <a name="why-is-the-overwrite-db-restore-failing-for-my-database"></a>Veritabanım için üzerine yazma DB geri yüklemesi neden başarısız oluyor?

Geri yükleme sırasında **üzerine yazmayı zorla** seçeneğinin seçildiğinden emin olun.

### <a name="why-do-i-see-the-source-and-target-systems-for-restore-are-incompatible-error"></a>Neden "geri yükleme için kaynak ve hedef sistemler uyumsuz" hatası görüyorum?

Şu anda hangi geri yükleme türlerinin desteklendiğini görmek için SAP HANA Note [1642148](https://launchpad.support.sap.com/#/notes/1642148) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM 'lerinde çalışan [SAP HANA veritabanlarını nasıl yedekleyeceğinizi](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database) öğrenin.
