---
title: SSS - Azure VM’lerindeki SAP HANA veritabanlarını yedekleme
description: Bu makalede, Azure Yedekleme hizmetini kullanarak SAP HANA veritabanlarını yedeklemeyle ilgili sık sorulan soruların yanıtlarını keşfedin.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: a46c4d6cccc00452a56567880400ef5779e6aed4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80155401"
---
# <a name="frequently-asked-questions--back-up-sap-hana-databases-on-azure-vms"></a>Sık sorulan sorular – Azure VM'lerde SAP HANA veritabanlarını yedekleme

Bu makalede, Azure Yedekleme hizmetini kullanarak SAP HANA veritabanlarını yedekleme yle ilgili sık sorulan sorular yanıtlanmaktadır.

## <a name="backup"></a>Backup

### <a name="how-many-full-backups-are-supported-per-day"></a>Günde kaç tam yedekleme desteklenir?

Günde sadece bir tam yedekleme yi destekliyoruz. Aynı gün diferansiyel yedekleme ve tam yedekleme tetiklenemez.

### <a name="do-successful-backup-jobs-create-alerts"></a>Başarılı yedekleme işleri sonucunda uyarı oluşturulur mu?

Hayır. Başarılı yedekleme işleri uyarı oluşturmaz. Uyarılar yalnızca başarısız olan yedekleme işleri için gönderilir. Portal uyarıları için ayrıntılı davranış [burada](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor)belgelenmiştir. Ancak, başarılı işler için bile uyarılara sahip olmak istiyorsanız, [Azure Monitor'u](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)kullanabilirsiniz.

### <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>Yedek İşler menüsünde zamanlanmış yedekleme işleri görebilir miyim?

Yedekleme İşmenüsü yalnızca geçici yedekleme işleri gösterir. Zamanlanmış işler için [Azure Monitör'ü](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)kullanın.

### <a name="are-future-databases-automatically-added-for-backup"></a>Gelecekteki veritabanları yedekleme için otomatik olarak eklenir mi?

Hayır, bu şu anda desteklenmiyor.

### <a name="if-i-delete-a-database-from-an-instance-what-will-happen-to-the-backups"></a>Bir veritabanını bir örnekten silersem, yedeklemelere ne olur?

Bir veritabanı SAP HANA örneğinden bırakılırsa, veritabanı yedeklemeleri yine de denenir. Bu, silinen veritabanının **Yedekleme Öğeleri** altında sağlıksız olarak görünmeye başladığı ve hala korunduğu anlamına gelir.
Bu veritabanını korumayı durdurmanın en doğru yolu, bu veritabanındaki **silme verileriyle Yedeklemeyi Durdur'u** gerçekleştirmektir.

### <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-the-behavior-be"></a>Veritabanı nın adını korumaaltına alındıktan sonra değiştirirsem, davranış ne olur?

Yeniden adlandırılmış bir veritabanı yeni bir veritabanı olarak kabul edilir. Bu nedenle, hizmet veritabanı bulunamadı ve başarısız yedekleri ile bu durumu ele alacaktır. Yeniden adlandırılmış veritabanı yeni bir veritabanı olarak görünür ve koruma için yapılandırılmalıdır.

### <a name="what-are-the-prerequisites-to-back-up-sap-hana-databases-on-an-azure-vm"></a>SAP HANA veritabanlarını Azure VM'de yedeklemenin ön koşulları nelerdir?

[Ön koşullara](tutorial-backup-sap-hana-db.md#prerequisites) ve ön kayıt komut dosyasının bölümleri [ne işe yaradığına](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) bakın.

### <a name="what-permissions-should-be-set-for-azure-to-be-able-to-back-up-sap-hana-databases"></a>AZURE'un SAP HANA veritabanlarını yedeklemesi için hangi izinler ayarlanmalıdır?

Ön kayıt komut dosyasının çalıştırılması, Azure'un SAP HANA veritabanlarını yedeklemesine izin vermek için gerekli izinleri ayarlar. Ön kayıt komut dosyasının ne yaptığını [burada](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does)bulabilirsiniz.

### <a name="will-backups-work-after-migrating-sap-hana-from-10-to-20"></a>Yedeklemeler SAP HANA'yı 1,0'dan 2,0'a geçtikten sonra çalışır mı?

Sorun giderme kılavuzunun [bu bölümüne](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#upgrading-from-sap-hana-10-to-20) bakın.

### <a name="can-azure-hana-backup-be-set-up-against-a-virtual-ip-load-balancer-and-not-a-virtual-machine"></a>Azure HANA Yedekleme sanal bir makineye değil, sanal bir IP 'ye (yük dengeleyicisi) karşı ayarlanabilir mi?

Şu anda tek başına sanal bir IP karşı çözüm kurmak için yeteneği yok. Çözümü uygulamak için sanal bir makineye ihtiyacımız var.

### <a name="i-have-a-sap-hana-system-replication-hsr-how-should-i-configure-backup-for-this-setup"></a>Ben bir SAP HANA Sistem Çoğaltma (HSR) var, nasıl bu kurulum için yedekleme yapılandırmak gerekir?

HSR'nin birincil ve ikincil düğümleri iki ayrı, ilişkisi zıddı ile ilişkili olmayan VM olarak ele alınacaktır. Yedeklemeyi birincil düğümde yapılandırmanız gerekir ve başarısız lık gerçekleştiğinde, yedeklemeyi ikincil düğümde yapılandırmanız gerekir (şimdi birincil düğüm olur). Diğer düğüme yedeklemenin otomatik 'fail-over'ı yoktur.

## <a name="restore"></a>Geri Yükleme

### <a name="why-cant-i-see-the-hana-system-i-want-my-database-to-be-restored-to"></a>Veritabanımın geri yüklenmesini istediğim HANA sistemini neden göremiyorum?

SAP HANA örneğini hedeflemek için geri yükleme için tüm ön koşulların karşılanıp karşılanmad›r olmadığını kontrol edin. Daha fazla bilgi için [Önkoşullar - Azure VM'deki SAP HANA veritabanlarını geri yükleme](https://docs.microsoft.com/azure/backup/sap-hana-db-restore#prerequisites)' ye bakın.

### <a name="why-is-the-overwrite-db-restore-failing-for-my-database"></a>Overwrite DB geri yüklemeveritabanım için neden başarısız oluyor?

Geri alırken **Force Overwrite** seçeneğinin seçildiğinden emin olun.

### <a name="why-do-i-see-the-source-and-target-systems-for-restore-are-incompatible-error"></a>Neden "Geri yükleme için kaynak ve hedef sistemleri uyumsuzdur" hatasını görüyorum?

Geri yükleme türlerinin şu anda desteklenmelerini görmek için SAP HANA Note [1642148'e](https://launchpad.support.sap.com/#/notes/1642148) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM'lerde çalışan [SAP HANA veritabanlarını](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database) nasıl yedekleyeceğimiz öğrenin.
