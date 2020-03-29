---
title: SQL Data Sync sorunlarını giderme
description: Azure SQL Veri Eşitleme ile sık karşılaşılan sorunları nasıl gidereceklerini öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
ms.date: 12/20/2018
ms.openlocfilehash: 8e223d0c6243bfddc1e5a56867c4c69de5e2a62e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822447"
---
# <a name="troubleshoot-issues-with-sql-data-sync"></a>SQL Data Sync ile ilgili sorunları giderme

Bu makalede, Azure SQL Veri Eşitlemi ile bilinen sorunları nasıl gideriniz açıklanmaktadır. Bir sorun için bir çözüm varsa, burada sağlanır.

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md).

> [!IMPORTANT]
> Azure SQL Veri Eşitlemesi şu anda Azure SQL Veritabanı Yönetilen Örneği'ni **desteklemez.**

## <a name="sync-issues"></a>Eşitleme sorunları

- [Eşitleme istemci aracısı ile ilişkili şirket içi veritabanları için portal UI başarısız](#sync-fails)

- [Eşitleme grubum işlem durumunda kaldı](#sync-stuck)

- [Tablolarımda hatalı veriler görüyorum](#sync-baddata)

- [Başarılı bir eşitlemeden sonra tutarsız birincil anahtar verilerini görüyorum](#sync-pkdata)

- [Performansta önemli bir bozulma görüyorum.](#sync-perf)

- [Bu iletiyi görüyorum: "NULL değerini sütun \<sütununa>. Sütun nulls izin vermez." Bu ne anlama geliyor ve nasıl düzeltebilirim?](#sync-nulls)

- [Veri Eşitleme dairesel başvuruları nasıl işler? Diğer bir deyişle, aynı veriler birden çok eşitleme gruplarında eşitlendiğinde ve sonuç olarak değişmeye devam ettiğinde?](#sync-circ)

### <a name="sync-fails-in-the-portal-ui-for-on-premises-databases-that-are-associated-with-the-client-agent"></a><a name="sync-fails"></a>Eşitleme istemci aracısı ile ilişkili şirket içi veritabanları için portal UI başarısız

Eşitleme, istemci aracısıyla ilişkili şirket içi veritabanları için SQL Veri Eşitleme portalı UI'de başarısız olur. Aracıyı çalıştıran yerel bilgisayarda, Olay Günlüğü'ndeki System.IO.IOException hatalarını görürsünüz. Hatalar, diskin yeterli alana sahip olmadığını söylüyor.

- **Çünkü.** Sürücünün yeterli alanı yok.

- **Çözünürlük**. %TEMP% dizininin bulunduğu sürücüde daha fazla alan oluşturun.

### <a name="my-sync-group-is-stuck-in-the-processing-state"></a><a name="sync-stuck"></a>Eşitleme grubum işlem durumunda kaldı

SQL Veri Eşitleme'deki eşitleme grubu uzun zamandır işlem durumundadır. **Durdurma** komutuna yanıt vermez ve günlüklerde yeni girişler gösterilmez.

Aşağıdaki koşullardan herhangi biri, eşitleme grubunun işlem durumuna takılıp kalmasıyla sonuçlanabilir:

- **Çünkü.** İstemci aracısı çevrimdışı

- **Çözünürlük**. İstemci aracısının çevrimiçi olduğundan emin olun ve işlemi yeniden deneyin.

- **Çünkü.** İstemci aracısı yüklü değil veya eksik.

- **Çözünürlük**. İstemci aracısı yüklü değilse veya eksikse:

    1. Aracı XML dosyası mevcutsa, dosyayı SQL Data Sync yükleme klasöründen kaldırın.
    1. Aracıyı bir şirket içi bilgisayara yükleyin (aynı bilgisayar veya farklı bir bilgisayar olabilir). Ardından, portalda çevrimdışı olarak gösterilen aracı için oluşturulan aracı anahtarını gönderin.

- **Çünkü.** SQL Data Sync hizmeti durduruldu.

- **Çözünürlük**. SQL Veri Eşitleme hizmetini yeniden başlatın.

    1. **Başlat** **menüsünde, Hizmetleri**arayın.
    1. Arama sonuçlarında **Hizmetler'i**seçin.
    1. SQL **Veri Eşitleme** hizmetini bulun.
    1. Hizmet durumu **Durdurulursa,** hizmet adını sağ tıklatın ve ardından **Başlat'ı**seçin.

> [!NOTE]
> Önceki bilgiler eşitleme grubunuzu işleme durumunun dışına taşımazsa, Microsoft Destek eşitleme grubunuzun durumunu sıfırlayabilir. [Azure SQL Veritabanı forumunda](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted)eşitleme grubu durumunuzu sıfırlamak için bir gönderi oluşturun. Gönderiye, sıfırlaması gereken grup için abonelik kimliğinizi ve eşitgrup kimliğinizi ekleyin. Bir Microsoft Destek mühendisi gönderinize yanıt verir ve durum sıfırlandığında size haber verir.

### <a name="i-see-erroneous-data-in-my-tables"></a><a name="sync-baddata"></a>Tablolarımda hatalı veriler görüyorum

Aynı ada sahip ancak farklı veritabanı şemalarından gelen tablolar eşitleme de dahil edilirse, eşitlemeden sonra tablolarda hatalı veriler görürsünüz.

- **Çünkü.** SQL Veri Eşitleme sağlama işlemi, aynı ada sahip ancak farklı şemalarda bulunan tablolar için aynı izleme tablolarını kullanır. Bu nedenle, her iki tablodaki değişiklikler aynı izleme tablosuna yansıtılır. Bu eşitleme sırasında hatalı veri değişikliklerine neden olur.

- **Çözünürlük**. Tablolar veritabanındaki farklı şemalara ait olsa bile eşitlemeyle ilgili tabloların adlarının farklı olduğundan emin olun.

### <a name="i-see-inconsistent-primary-key-data-after-a-successful-sync"></a><a name="sync-pkdata"></a>Başarılı bir eşitlemeden sonra tutarsız birincil anahtar verilerini görüyorum

Eşitleme başarılı olarak bildirilir ve günlük başarısız veya atlanan satırlar göstermez, ancak birincil anahtar verilerinin eşitleme grubundaki veritabanları arasında tutarsız olduğunu gözlemlersiniz.

- **Çünkü.** Bu sonuç tasarım gereğidir. Herhangi bir birincil anahtar sütunundaki değişiklikler, birincil anahtarın değiştirildiği satırlarda tutarsız verilerle sonuçlanır.

- **Çözünürlük**. Bu sorunu önlemek için, birincil anahtar sütunundaki hiçbir verinin değiştirilmediğinden emin olun. Oluştuktan sonra bu sorunu gidermek için, eşitleme grubundaki tüm uç noktalardan tutarsız verileri olan satırı silin. Sonra satırı yeniden ekleyin.

### <a name="i-see-a-significant-degradation-in-performance"></a><a name="sync-perf"></a>Performansta önemli bir bozulma görüyorum.

Performansınız, büyük olasılıkla Veri Eşitleme UI'sini bile açamadığınız bir noktaya kadar önemli ölçüde düşer.

- **Çünkü.** En olası neden eşitleme döngüsüdür. Eşitleme döngüsü, eşitleme grubu A tarafından eşitlendiğinde, eşitleme grubu B tarafından eşitlendiğinde oluşur ve bu da eşitleme grubu A tarafından eşitleme tetikler. Gerçek durum daha karmaşık olabilir ve döngüde ikiden fazla eşitleme grubu içerebilir. Sorun, eşitleme gruplarının birbiriyle çakışması nedeniyle oluşan dairesel bir eşitleme tetikleyicisi olmasıdır.

- **Çözünürlük**. En iyi düzeltme önleme olduğunu. Eşitleme gruplarınızda dairesel başvuruların olmadığından emin olun. Bir eşitleme grubu tarafından eşitlenen herhangi bir satır başka bir eşitleme grubu tarafından eşitlenemez.

### <a name="i-see-this-message-cannot-insert-the-value-null-into-the-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-it"></a><a name="sync-nulls"></a>Bu iletiyi görüyorum: "NULL değerini sütun \<sütununa>. Sütun nulls izin vermez." Bu ne anlama geliyor ve nasıl düzeltebilirim? 
Bu hata iletisi, aşağıdaki iki sorundan birinin oluştuğunu gösterir:
-  Tablonun birincil anahtarı yoktur. Bu sorunu gidermek için, eşitlediğiniz tüm tablolara birincil anahtar ekleyin.
-  CREATE INDEX ekstrenizde WHERE yan tümcesi var. Veri Eşitleme bu durumu işlemez. Bu sorunu gidermek için WHERE yan tümcesini kaldırın veya tüm veritabanlarında değişiklikleri el ile yapın. 
 
### <a name="how-does-data-sync-handle-circular-references-that-is-when-the-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a><a name="sync-circ"></a>Veri Eşitleme dairesel başvuruları nasıl işler? Diğer bir deyişle, aynı veriler birden çok eşitleme gruplarında eşitlendiğinde ve sonuç olarak değişmeye devam ettiğinde?
Veri Eşitleme dairesel başvuruları işlemez. Onlardan uzak durun. 

## <a name="client-agent-issues"></a>İstemci aracısı sorunları

İstemci aracısıyla ilgili sorunları gidermek için [Veri Eşitleme Aracısı sorunlarını](sql-database-data-sync-agent.md#agent-tshoot)görün.

## <a name="setup-and-maintenance-issues"></a>Kurulum ve bakım sorunları

- ["Disk uzaydan çıktı" iletisi alıyorum](#setup-space)

- [Eşitleme grubumu silemişim](#setup-delete)

- [Şirket içi SQL Server veritabanını açamıyorum](#setup-unreg)

- [Sistem hizmetlerini başlatmak için yeterli ayrıcalığım yok](#setup-perms)

- [Veritabanının "Güncel Olmayan" durumu vardır](#setup-date)

- [Eşitleme grubunun "Güncel Olmayan" durumu vardır](#setup-date2)

- [Eşitleme grubu aracıyı yükledikten veya durdurdukten sonraki üç dakika içinde silinemez](#setup-delete2)

- [Kaybolan veya bozulmuş bir veritabanını geri yüklediğimde ne olur?](#setup-restore)

### <a name="i-get-a-disk-out-of-space-message"></a><a name="setup-space"></a>"Disk uzaydan çıktı" iletisi alıyorum

- **Çünkü.** Kalan dosyaların silinmesi gerekiyorsa "disk yer dışında" iletisi görünebilir. Bunun nedeni virüsten koruma yazılımı olabilir veya silme işlemleri denendiğinde dosyalar açık olabilir.

- **Çözünürlük**. %geçici% klasöründeki eşitleme dosyalarını el`del \*sync\* /s`ile silin ( ). Ardından, %geçici% klasöründeki alt dizinleri silin.

> [!IMPORTANT]
> Eşitleme devam ederken hiçbir dosyayı silmeyin.

### <a name="i-cant-delete-my-sync-group"></a><a name="setup-delete"></a>Eşitleme grubumu silemişim

Eşitleme grubunu silme girişiminiz başarısız olur. Aşağıdaki senaryolardan herhangi biri eşitleme grubunun silinememesine neden olabilir:

- **Çünkü.** İstemci aracısı çevrimdışı.

- **Çözünürlük**. İstemci aracısının çevrimiçi olduğundan emin olun ve sonra yeniden deneyin.

- **Çünkü.** İstemci aracısı yüklü değil veya eksik.

- **Çözünürlük**. İstemci aracısı yüklü değilse veya eksikse:  
    a. Aracı XML dosyası mevcutsa, dosyayı SQL Data Sync yükleme klasöründen kaldırın.  
    b. Aracıyı bir şirket içi bilgisayara yükleyin (aynı bilgisayar veya farklı bir bilgisayar olabilir). Ardından, portalda çevrimdışı olarak gösterilen aracı için oluşturulan aracı anahtarını gönderin.

- **Çünkü.** Veritabanı çevrimdışı.

- **Çözünürlük**. SQL veritabanlarınızın ve SQL Server veritabanlarınızın çevrimiçi olduğundan emin olun.

- **Çünkü.** Eşitleme grubu sağlama veya eşitleme.

- **Çözünürlük**. Sağlama veya eşitleme işlemi bitene kadar bekleyin ve ardından eşitleme grubunu yeniden silmeyi deneyin.

### <a name="i-cant-unregister-an-on-premises-sql-server-database"></a><a name="setup-unreg"></a>Şirket içi SQL Server veritabanını açamıyorum

- **Çünkü.** Büyük olasılıkla, zaten silinmiş bir veritabanının kaydını kaldırmaya çalışıyorsunuz.

- **Çözünürlük**. Şirket içi BIR SQL Server veritabanının kaydını çıkarmak için veritabanını seçin ve ardından **Zorla Sil'i**seçin.

  Bu işlem veritabanını eşitleme grubundan kaldıramazsa:

  1. İstemci aracısı ana bilgisayarı hizmetini durdurun ve yeniden başlatın:  
    a. **Başlat** menüsünü seçin.  
    b. Arama kutusuna **services.msc**girin.  
    c. Arama sonuçları bölmesinin **Programlar** bölümünde **Hizmetler'i**çift tıklatın.  
    d. SQL Veri **Eşitleme** hizmetine sağ tıklayın.  
    e. Hizmet çalışıyorsa, durdurun.  
    f. Hizmeti sağ tıklatın ve ardından **Başlat'ı**seçin.  
    g. Veritabanının hala kayıtlı olup olmadığını denetleyin. Artık kayıtlı değilse, bitti. Aksi takdirde, bir sonraki adımile devam edin.
  1. İstemci aracısı uygulamasını açın (SqlAzureDataSyncAgent).
  1. **Kimlik Bilgilerini Edit'i**seçin ve ardından veritabanının kimlik bilgilerini girin.
  1. Kayıt dışı ile devam edin.

### <a name="i-dont-have-sufficient-privileges-to-start-system-services"></a><a name="setup-perms"></a>Sistem hizmetlerini başlatmak için yeterli ayrıcalığım yok

- **Çünkü.** Bu hata iki durumda oluşur:
  -   Kullanıcı adı ve/veya parola yanlış.
  -   Belirtilen kullanıcı hesabının hizmet olarak oturum açmak için yeterli ayrıcalıkları yoktur.

- **Çözünürlük**. Hizmet olarak oturum açma kimlik bilgilerini kullanıcı hesabına verir:

  1. **Başlat** > **Denetim Masası** > Yönetim**Araçları** > **Yerel Güvenlik Politikası** > **Yerel İlke** > Kullanıcı Hakları**Yönetimi'ne**gidin.
  1. **Hizmet olarak Oturum Aç'ı**seçin.
  1. **Özellikler** iletişim kutusuna kullanıcı hesabını ekleyin.
  1. **Uygula**’yı ve sonra **Tamam**’ı seçin.
  1. Tüm pencereleri kapatın.

### <a name="a-database-has-an-out-of-date-status"></a><a name="setup-date"></a>Veritabanının "Güncel Olmayan" durumu vardır

- **Çünkü.** SQL Data Sync, 45 gün veya daha uzun süredir hizmetten çevrimdışı olan veritabanlarını kaldırır (veritabanının çevrimdışı duruma geldiği andan itibaren sayılır). Bir veritabanı 45 gün veya daha uzun bir süre çevrimdışıysa ve sonra tekrar çevrimiçi olursa, durumu **Güncel Değil.**

- **Çözünürlük**. Veritabanlarınızdan hiçbirinizin 45 gün veya daha uzun süre çevrimdışı kalmamasını sağlayarak **Güncel Olmayan** durumu önleyebilirsiniz.

  Bir veritabanının durumu **Güncel**değilse:

  1. Eşitleme grubundan **Güncel Olmayan** durum olan veritabanını kaldırın.
  1. Veritabanını eşitleme grubuna geri ekleyin.

  > [!WARNING]
  > Bu veritabanı çevrimdışıyken bu veritabanında yapılan tüm değişiklikleri kaybedersiniz.

### <a name="a-sync-group-has-an-out-of-date-status"></a><a name="setup-date2"></a>Eşitleme grubunun "Güncel Olmayan" durumu vardır

- **Çünkü.** 45 günlük tüm bekletme süresi için bir veya daha fazla değişiklik uygulanmazsa, eşitleme grubu eskihaline gelebilir.

- **Çözünürlük**. Eşitleme grubu için **Güncel Olmayan** durumlardan kaçınmak için, tarih görüntüleyicisindeki eşitleme işlerinizin sonuçlarını düzenli olarak inceleyin. Uygulanmayan değişiklikleri araştırın ve çözüme kavuşturun.

  Eşitleme grubunun durumu **Güncel Değilse,** eşitleme grubunu silin ve yeniden oluşturun.

### <a name="a-sync-group-cant-be-deleted-within-three-minutes-of-uninstalling-or-stopping-the-agent"></a><a name="setup-delete2"></a>Eşitleme grubu aracıyı yükledikten veya durdurdukten sonraki üç dakika içinde silinemez

İlişkili SQL Veri Eşitleme istemci aracısını kaldırmaveya durdurmadan sonraki üç dakika içinde bir eşitleme grubunu silemezsiniz.

- **Çözünürlük**.

  1. İlişkili eşitleme aracıları çevrimiçiyken eşitleme grubunu kaldırın (önerilir).
  1. Aracı çevrimdışıysa ancak yüklüyse, şirket içi bilgisayarda çevrimiçi duruma getirin. Aracının durumunun SQL Veri Eşitleme portalında **Çevrimiçi** olarak görünmesini bekleyin. Ardından eşitleme grubunu kaldırın.
  1. Aracı, kaldırıldığı için çevrimdışıysa:  
    a.  Aracı XML dosyası mevcutsa, dosyayı SQL Data Sync yükleme klasöründen kaldırın.  
    b.  Aracıyı bir şirket içi bilgisayara yükleyin (aynı bilgisayar veya farklı bir bilgisayar olabilir). Ardından, portalda çevrimdışı olarak gösterilen aracı için oluşturulan aracı anahtarını gönderin.  
    c. Eşitleme grubunu silmeye çalışın.

### <a name="what-happens-when-i-restore-a-lost-or-corrupted-database"></a><a name="setup-restore"></a>Kaybolan veya bozulmuş bir veritabanını geri yüklediğimde ne olur?

Kaybolan veya bozulmuş bir veritabanını bir yedeklemeden geri yüklerseniz, veritabanının ait olduğu eşitleme gruplarında veri yakınsamaz.

## <a name="next-steps"></a>Sonraki adımlar
SQL Veri Eşitlemi hakkında daha fazla bilgi için bkz:

-   Genel Bakış - [Azure SQL Veri Eşitlemi ile verileri birden çok bulut ve şirket içi veritabanları nda eşitleme](sql-database-sync-data.md)
-   Veri Eşitlemeyi Ayarlama
    - Portalda - [Öğretici: Azure SQL Veritabanı ve SQL Server arasında verileri şirket içinde eşitlemek için SQL Veri Eşitlemeyi'ni ayarlayın](sql-database-get-started-sql-data-sync.md)
    - PowerShell ile
        -  [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Veri Eşitleme Aracısı - [Azure SQL Veri Eşitleme için Veri Eşitleme Aracısı](sql-database-data-sync-agent.md)
-   En iyi uygulamalar - [Azure SQL Veri Eşitleme için en iyi uygulamalar](sql-database-best-practices-data-sync.md)
-   Monitör - [Azure Monitor günlükleriyle SQL Veri Eşitlemesini Izleyin](sql-database-sync-monitor-oms.md)
-   Eşitleme şeasını güncelleştirme
    -   Transact-SQL ile - [Azure SQL Veri Eşitleme'deki şema değişikliklerinin çoğaltılması otomatikleştirin](sql-database-update-sync-schema.md)
    -   PowerShell ile - [Varolan bir eşitleme grubunda eşitleme şemasını güncellemek için PowerShell'i kullanın](scripts/sql-database-sync-update-schema.md)

SQL Veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
-   [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
