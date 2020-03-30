---
title: Çok kiracılı bir SaaS uygulamasında veritabanını geri yükleme
description: Verileri yanlışlıkla siledikten sonra tek bir kiracının SQL veritabanını nasıl geri yükleyin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
ms.date: 12/04/2018
ms.openlocfilehash: a54e8e5629f6f8ad688b6fe11bbf50fc038095bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73818521"
---
# <a name="restore-a-single-tenant-with-a-database-per-tenant-saas-application"></a>Veritabanı başına kiracı Başına SaaS uygulamasıyla tek bir kiracıyı geri yükleme

Kiracı başına veritabanı modeli, diğer kiracıları etkilemeden tek bir kiracıyı önceki bir noktaya geri yüklemeyi kolaylaştırır.

Bu öğreticide, iki veri kurtarma deseni öğrenirsiniz:

> [!div class="checklist"]
> * Veritabanını paralel bir veritabanına geri yükleme (yan yana).
> * Varolan veritabanını değiştirerek bir veritabanını yerinde geri yükleyin.

|||
|:--|:--|
| Paralel veritabanına geri yükleme | Bu desen, bir kiracının verilerini daha önceki bir noktadan denetlemesine izin vermek için gözden geçirme, denetleme ve uyumluluk gibi görevler için kullanılabilir. Kiracının geçerli veritabanı çevrimiçi kalır ve değişmeden kalır. |
| Yerinde geri yükleme | Bu desen genellikle, kiracı yanlışlıkla verileri siler veya bozar sonra, daha önceki bir noktaya kiracı kurtarmak için kullanılır. Özgün veritabanı çevrimdışı duruma alınır ve geri yüklenen veritabanıyla değiştirilir. |
|||

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip SaaS uygulaması dağıtıldı. Beş dakikadan kısa bir süre içinde dağıtmak için [Bkz.](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için Azure [PowerShell ile başlayın'](https://docs.microsoft.com/powershell/azure/get-started-azureps)a bakın.

## <a name="introduction-to-the-saas-tenant-restore-patterns"></a>SaaS kiracı geri yükleme desenleri giriş

Tek bir kiracının verilerini geri bulmak için iki basit desen vardır. Kiracı veritabanları birbirinden izole olduğundan, bir kiracının geri dinlenmesi diğer kiracıların verileri üzerinde hiçbir etkiye sahip değildir. Azure SQL Veritabanı nokta-in-time geri yükleme (PITR) özelliği her iki desen kullanılır. PITR her zaman yeni bir veritabanı oluşturur.

* **Paralel Geri Yükleme**: İlk desende, kiracının geçerli veritabanının yanında yeni bir paralel veritabanı oluşturulur. Kiracıya daha sonra geri yüklenen veritabanına salt okunur erişim verilir. Geri yüklenen veriler gözden geçirilebilir ve geçerli veri değerlerinin üzerine yazmak için kullanılabilir. Kiracının geri yüklenen veritabanına nasıl erişip kurtarma seçenekleri nin sağlandığını belirlemek uygulama tasarımcısına kalmaktadır. Kiracının verilerini daha erken bir noktada gözden geçirmesine izin vermek, bazı senaryolarda gereken tek şey olabilir.

* **Yerinde Geri Yükleme**: Veriler kaybolduysa veya bozuksa ve kiracı daha önceki bir noktaya dönmek istiyorsa ikinci desen yararlıdır. Veritabanı geri yüklenirken kiracı çevrimdışı duruma getirilir. Özgün veritabanı silinir ve geri yüklenen veritabanı nın adı yeniden adlandırılır. Özgün veritabanının yedekleme zinciri silindikten sonra erişilebilir kalır, böylece gerekirse veritabanını daha önceki bir noktaya geri yükleyebilirsiniz.

Veritabanı etkin [coğrafi çoğaltma](sql-database-active-geo-replication.md) ve paralel olarak geri yükleme kullanıyorsa, geri yüklenen kopyadan gerekli verileri özgün veritabanına kopyalamanızı öneririz. Özgün veritabanını geri yüklenen veritabanıyla değiştirirseniz, coğrafi çoğaltmayı yeniden yapılandırmanız ve yeniden eşitlemeniz gerekir.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip Tickets SaaS veritabanı-kiracı başına uygulama komut dosyalarını edinin

Wingtip Biletleri SaaS Multitenant Veritabanı komut dosyaları ve uygulama kaynak kodu [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub repo mevcuttur. Wingtip Tickets SaaS komut dosyalarını indirmek ve engelini kaldırmak için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md)bakın.

## <a name="before-you-start"></a>Başlamadan önce

Bir veritabanı oluşturulduğunda, ilk tam yedeklemenin geri yüklenmeden önce 10 ila 15 dakika sürebilir. Uygulamayı yeni yüklediyseniz, bu senaryoyu denemeden önce birkaç dakika beklemeniz gerekebilir.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Verileri yanlışlıkla silen kiracıyı simüle etme

Bu kurtarma senaryolarını göstermek için, ilk önce kiracı veritabanlarından birinde bir olayı "yanlışlıkla" silin. 

### <a name="open-the-events-app-to-review-the-current-events"></a>Güncel olayları incelemek için Etkinlikler uygulamasını açın

1. Etkinlikler Hub'ınıhttp://events.wtp.&ltaçın&gt;( ;kullanıcı .trafficmanager.net) ve **Contoso Konser Salonu'nu**seçin.

   ![Etkinlikler Merkezi](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

2. Olaylar listesini kaydırın ve listedeki son olayın notunu alın.

   ![Son olay görünür](media/saas-dbpertenant-restore-single-tenant/last-event.png)

### <a name="accidentally-delete-the-last-event"></a>"Yanlışlıkla" son olayı silme

1. PowerShell ISE, açık ... \\Öğrenme\\Modülleri İş Sürekliliği\\ve\\Afet Kurtarma RestoreTenant*Demo-RestoreTenant.ps1*, ve aşağıdaki değeri ayarlayın:

   * **$DemoScenario** = **1**, Son olayı *silin (bilet satışı olmadan)*.
2. Komut dosyasını çalıştırmak ve son olayı silmek için F5 tuşuna basın. Aşağıdaki onay iletisi görüntülenir:

   ```Console
   Deleting last unsold event from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

3. Contoso olaylar sayfası açılır. Aşağı kaydırın ve olayın gittiğini doğrulayın. Olay listede yse, **Yenile'yi** seçin ve gittiğini doğrulayın.
   ![Son olay kaldırıldı](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)

## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Üretim veritabanına paralel olarak kiracı veritabanını geri yükleme

Bu alıştırma, Contoso Concert Hall veritabanını olay silinmeden önce bir noktaya geri yüklenir. Bu senaryo, silinen verileri paralel bir veritabanında gözden geçirmek istediğinizi varsayar.

 *Geri Yükleme-TenantInParallel.ps1* komut dosyası, paralel katalog girişi ile *ContosoConcertHall\_eski*adında paralel bir kiracı veritabanı oluşturur. Bu geri yükleme deseni, küçük bir veri kaybından kurtarmak için en uygun modeldir. Verileri uyumluluk veya denetim amacıyla gözden geçirmeniz gerekiyorsa da bu deseni kullanabilirsiniz. [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md)kullandığınızda önerilen yaklaşımdır.

1. Yanlışlıkla [veri silme bir kiracı benzetme](#simulate-a-tenant-accidentally-deleting-data) tamamlayın.
2. PowerShell ISE, açık ... \\Öğrenme\\Modülleri İş Sürekliliği\\ve\\Afet Kurtarma RestoreTenant_Demo-RestoreTenant.ps1_.
3. 2 **$DemoScenario** = ayarlayın **,** *Kiracıyı paralel olarak geri yükleyin.*
4. Komut dosyasını çalıştırmak için F5 tuşuna basın.

Komut dosyası, kiracı veritabanını, olayı silmeden önce bir noktaya geri yükler. Veritabanı _\_ContosoConcertHall eski_adlı yeni bir veritabanına geri yüklenir. Bu geri yüklenen veritabanında bulunan katalog meta verileri silinir ve *\_contosoConcertHall eski* adından oluşturulmuş bir anahtar kullanılarak veritabanı kataloga eklenir.

Demo komut dosyası tarayıcınızdaki bu yeni kiracı veritabanı için etkinlikler sayfasını açar. URL'den ```http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` bu sayfanın, ada *_old* eklendiği geri yüklenen veritabanından gelen verileri gösterdiğini unutmayın.

Önceki bölümde silinen olayın geri yükildiğini doğrulamak için tarayıcıda listelenen olayları kaydırın.

Geri yüklenen kiracıyı kendi Etkinlikler uygulamasıyla ek bir kiracı olarak ortaya çıkarmak, geri yüklenen verilere kiracı erişimi sağlama nız olası değildir. Geri yükleme deseni göstermek için hizmet vermektedir. Genellikle, eski verilere salt okunur erişim verir ve geri yüklenen veritabanını belirli bir süre boyunca saklarsınız. Örnekte, geri yüklenen kiracı senaryosunu _kaldır'ı_ çalıştırarak, bittikten sonra geri yüklenen kiracı girişini silebilirsiniz.

1. **4** **$DemoScenario** = ayarlayın , Geri *yüklenen kiracıyı kaldırın.*
2. Komut dosyasını çalıştırmak için F5 tuşuna basın.
3. *\_ContosoConcertHall eski* girişi artık katalogdan silinir. Tarayıcınızdaki bu kiracının etkinlikler sayfasını kapatın.

## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Varolan kiracı veritabanını değiştirerek kiracıyı yerinde geri yükleme

Bu alıştırma, Contoso Konser Salonu kiracısını etkinlik silinmeden önce bir noktaya geri yükler. *Geri Yükleme-TenantInPlace* komut dosyası, kiracı veritabanını yeni bir veritabanına geri yükler ve orijinalini siler. Bu geri yükleme deseni, ciddi veri bozulmasını atlatmak için en uygun modeldir ve kiracının önemli veri kaybına uyum sağlaması gerekebilir.

1. PowerShell ISE'de **Demo-RestoreTenant.ps1** dosyasını açın.
2. Set **$DemoScenario** = **5**, Yerinde kiracı *geri yükleyin.*
3. Komut dosyasını çalıştırmak için F5 tuşuna basın.

Komut dosyası, olay silinmeden önce kiracı veritabanını bir noktaya geri yükler. İlk contoso Konser Salonu kiracı daha fazla güncellemeleri önlemek için hat kapalı alır. Daha sonra, geri yükleme noktasından geri yükleyerek paralel bir veritabanı oluşturulur. Geri yüklenen veritabanı, veritabanı adının varolan kiracı veritabanı adıyla çakışmadığından emin olmak için bir zaman damgasıyla adlandırılır. Ardından, eski kiracı veritabanı silinir ve geri yüklenen veritabanı özgün veritabanı adı yla yeniden adlandırılır. Son olarak, Contoso Konser Salonu, uygulamanın geri yüklenen veritabanına erişmesine izin vermek için çevrimiçi olarak getirilir.

Veritabanını, olay silinmeden önce bir noktaya başarıyla geri yükledin. **Etkinlikler** sayfası açıldığında, son olayın geri yüklendiğinden onaylayın.

Veritabanını geri yükledikten sonra, ilk tam yedeklemenin yeniden geri yüklenebilmeleri için 10 ila 15 dakika daha sürer.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Veritabanını paralel bir veritabanına geri yükleme (yan yana).
> * Bir veritabanını yerinde geri yükleyin.

Kiracı [veritabanı şema öğreticisini yönet'i](saas-tenancy-schema-management.md) deneyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip SaaS uygulaması üzerine inşa ek öğreticiler](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure SQL Veritabanı'nda iş sürekliliğine genel bakış](sql-database-business-continuity.md)
* [SQL Veritabanı yedeklemeleri hakkında bilgi edinin](sql-database-automated-backups.md)
