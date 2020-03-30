---
title: 'SaaS uygulamaları: Olağanüstü durum kurtarma için coğrafi yedekli yedeklemeler'
description: Kesinti durumunda çok kiracılı bir SaaS uygulamasını kurtarmak için Azure SQL Veritabanı coğrafi yedekli yedeklemeleri kullanmayı öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: AyoOlubeko
ms.author: craigg
ms.reviewer: sstein
ms.date: 01/14/2019
ms.openlocfilehash: 270fc157fa14efa19ed30d35b614fb769804b72e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73826467"
---
# <a name="use-geo-restore-to-recover-a-multitenant-saas-application-from-database-backups"></a>Çok kiracılı bir SaaS uygulamasını veritabanı yedeklemelerinden kurtarmak için coğrafi geri yüklemeyi kullanma

Bu öğretici, kiracı modeli başına veritabanı ile uygulanan çok kiracılı Bir SaaS uygulaması için tam bir olağanüstü durum kurtarma senaryosunu inceler. Kataloğu ve kiracı veritabanlarını otomatik olarak tutulan coğrafi yedekyedeklemelerden alternatif bir kurtarma bölgesine kurtarmak için [coğrafi geri yükleme](sql-database-recovery-using-backups.md) kullanırsınız. Kesinti çözüldükten sonra, değiştirilen veritabanlarını özgün bölgelerine geri göndermek için [coğrafi çoğaltma](sql-database-geo-replication-overview.md) kullanırsınız.

![Coğrafi geri yükleme-mimari](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Coğrafi geri yükleme, Azure SQL Veritabanı için en düşük maliyetli olağanüstü durum kurtarma çözümüdür. Ancak, coğrafi yedekli yedeklemelerden geri almak, bir saate kadar veri kaybına neden olabilir. Her veritabanının boyutuna bağlı olarak önemli ölçüde zaman alabilir. 

> [!NOTE]
> Olası en düşük RPO ve RTO ile uygulamaları, coğrafi geri yükleme yerine coğrafi çoğaltma kullanarak kurtarın.

Bu öğretici, hem geri yükleme hem de geri dönüş iş akışlarını inceler. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:
> [!div class="checklist"]
> 
> * Veritabanını ve esnek havuz yapılandırma bilgilerini kiracı kataloğuna eşitleyin.
> * Uygulama, sunucular ve havuzlar içeren bir kurtarma bölgesinde bir ayna görüntüsü ortamı ayarlayın.   
> * Coğrafi geri yükleme kullanarak katalog ve kiracı veritabanlarını kurtarın.
> * Kesinti çözüldükten sonra kiracı kataloğunu ve değiştirilen kiracı veritabanlarını geri göndermek için coğrafi çoğaltmayı kullanın.
> * Her kiracının veritabanının etkin kopyasının geçerli konumunu izlemek için her veritabanı geri yüklenirken (veya iade edilirken) kataloğu güncelleştirin.
> * Gecikme süresini azaltmak için uygulama ve kiracı veritabanının her zaman aynı Azure bölgesinde birlikte bulunduğundan emin olun. 
 

Bu öğreticiye başlamadan önce aşağıdaki ön koşulları tamamlayın:
* Kiracı uygulaması başına Wingtip Tickets SaaS veritabanını dağıtın. Beş dakikadan kısa bir süre içinde dağıtmak için, [bkz.](saas-dbpertenant-get-started-deploy.md) 
* Azure PowerShell'i yükleyin. Ayrıntılar için Azure [PowerShell ile başlarken](https://docs.microsoft.com/powershell/azure/get-started-azureps)bakın.

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Coğrafi geri yükleme kurtarma desenine giriş

Olağanüstü durum kurtarma (DR) uyumluluk nedenleriyle veya iş sürekliliği için olsun, birçok uygulama için önemli bir husustur. Uzun süreli bir hizmet kesintisi varsa, iyi hazırlanmış bir DR planı iş aksamalarını en aza indirebilir. Coğrafi geri yüklemeye dayalı bir DR planı birkaç hedefe ulaşmalıdır:
 * Kiracı veritabanlarını geri yüklemek için kullanılabilir olduğundan emin olmak için seçilen kurtarma bölgesinde gereken tüm kapasiteyi mümkün olan en kısa sürede rezerve edin.
 * Özgün havuz ve veritabanı yapılandırmasını yansıtan bir ayna görüntüsü kurtarma ortamı kurun. 
 * Orijinal bölge yeniden devreye girdiğinde, uçuşun ortasında geri yükleme işleminin iptaline izin verin.
 * Yeni kiracının mümkün olan en kısa sürede yeniden başlatabilmesi için kiracı sağlamayı hızlı bir şekilde etkinleştirin.
 * Kiracıları öncelikli sırada geri yüklemek için optimize edin.
 * Pratik olduğu durumlarda paralel adımlar atarak kiracıları mümkün olan en kısa sürede çevrimiçi duruma getirmek için optimize edin.
 * Başarısızlığa, yeniden başlatılabilir ve idempotent'e karşı dirençli olun.
 * Kesinti çözüldüğünde, veritabanlarını kiracılara en az etkiyle orijinal bölgelerine geri gönderin.  

> [!NOTE]
> Uygulama, uygulamanın dağıtıldığı bölgenin eşleşmiş bölgesine kurtarılır. Daha fazla bilgi için Azure [eşleştirilmiş bölgelere](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)bakın.   

Bu öğretici, aşağıdaki zorlukları gidermek için Azure SQL Veritabanı ve Azure platformunun özelliklerini kullanır:

* [Azure Kaynak Yöneticisi,](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)gerekli tüm kapasiteyi mümkün olan en kısa sürede ayırmak için şablonlar. Azure Kaynak Yöneticisi şablonları, kurtarma bölgesindeki özgün sunucuların ve esnek havuzların ayna görüntüsünü sağlamak için kullanılır. Yeni kiracıların sağlanması için ayrı bir sunucu ve havuz da oluşturulur.
* [Elastik Veritabanı İstemci Kitaplığı](sql-database-elastic-database-client-library.md) (EDCL), bir kiracı veritabanı kataloğu oluşturmak ve korumak için. Genişletilmiş katalog, periyodik olarak yenilenen havuz ve veritabanı yapılandırma bilgilerini içerir.
* Kurtarma ve geri gönderme sırasında katalogdaki veritabanı konum girişlerini korumak için EDCL'nin [parça yönetimi kurtarma özellikleri.](sql-database-elastic-database-recovery-manager.md)  
* [Coğrafi geri yükleme,](sql-database-disaster-recovery.md)kataloğu ve kiracı veritabanlarını otomatik olarak tutulan coğrafi yedekyedeklemelerden kurtarmak için. 
* Kiracı öncelikli sırada gönderilen [eşkenar dörtel geri yükleme işlemleri,](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations)sistem tarafından her havuz için sıraya alınır ve havuzun aşırı yüklenmemesi için toplu olarak işlenir. Bu işlemler, gerekirse yürütmeden önce veya sırasında iptal edilebilir.   
* [Coğrafi çoğaltma](sql-database-geo-replication-overview.md), kesintiden sonra orijinal bölgeye veritabanları nı iade etmek için. Coğrafi çoğaltma kullandığınızda hiçbir veri kaybı ve kiracı üzerinde en az etkisi yoktur.
* [SQL sunucusu DNS diğer adları,](dns-alias-overview.md)katalog eşitleme işleminin konumune bakılmaksızın etkin kataloğerine bağlanmasına izin verir.  

## <a name="get-the-disaster-recovery-scripts"></a>Olağanüstü durum kurtarma komut dosyalarını alın

Bu eğitimde kullanılan DR komut [dosyaları, kiracı GitHub deposu başına Wingtip Tickets SaaS veritabanında](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant)mevcuttur. Wingtip Tickets yönetim komut dosyalarını indirmek ve engelini kaldırmak için atılacak adımlar için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md) göz atın.

> [!IMPORTANT]
> Tüm Wingtip Tickets yönetim senaryoları gibi, DR komut örnekleri de örnek kalitededir ve üretimde kullanılmamalıdır.

## <a name="review-the-healthy-state-of-the-application"></a>Uygulamanın sağlıklı durumunu gözden geçirin
Kurtarma işlemini başlatmadan önce, uygulamanın normal sağlıklı durumunu gözden geçirin.

1. Web tarayıcınızda Wingtip Tickets etkinlik merkezinihttp://events.wingtip-dpt.&ltaçın&gt;(;kullanıcı &lt;&gt; .trafficmanager.net, kullanıcıyı dağıtımınızın kullanıcı değeriyle değiştirin).
    
   Sayfanın altına gidin ve altbilgideki katalog sunucusu adını ve konumunu fark edin. Konum, uygulamayı dağıttığınız bölgedir.    

   > [!TIP]
   > Ekranı büyütmek için fareyi konumun üzerine tover edin.

   ![Özgün bölgede olaylar merkezi sağlıklı durum](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

2. Contoso Konser Salonu kiracısını seçin ve etkinlik sayfasını açın.

   Altbilgide, kiracının sunucu adını fark edin. Konum, katalog sunucusunun konumuyla aynıdır.

   ![Contoso Konser Salonu orijinal bölge](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png) 

3. Azure [portalında](https://portal.azure.com)uygulamayı dağıttığınız kaynak grubunu inceleyin ve açın.

   Kaynaklarına ve uygulama hizmeti bileşenlerinin ve SQL Veritabanı sunucularının dağıtıldığı bölgeye dikkat edin.

## <a name="sync-the-tenant-configuration-into-the-catalog"></a>Kiracı yapılandırmasını katalogda eşitleme

Bu görevde, sunucuların, elastik havuzların ve veritabanlarının yapılandırmasını kiracı kataloğuna eşitlemek için bir işlem başlamış olursunuz. Bu bilgiler daha sonra kurtarma bölgesindeki bir ayna görüntüsü ortamını yapılandırmak için kullanılır.

> [!IMPORTANT]
> Basitlik için, senkronizasyon işlemi ve diğer uzun süren kurtarma ve geri gönderme işlemleri bu örneklerde istemci kullanıcı oturumunuzda çalışan yerel PowerShell işleri veya oturumları olarak uygulanır. Oturum açtığınızda verilen kimlik doğrulama belirteçleri birkaç saat sonra sona erer ve işler başarısız olur. Bir üretim senaryosunda, uzun süren işlemler bir hizmet ilkesi altında çalışan bir tür güvenilir Azure hizmeti olarak uygulanmalıdır. Bkz. [Sertifikası olan bir hizmet sorumlusu oluşturmak için Azure PowerShell'i kullanın.](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal) 

1. PowerShell ISE'de ...\Öğrenme Modülleri\UserConfig.psm1 dosyasını açın. Uygulamayı `<resourcegroup>` `<user>` dağıtırken kullanılan değerle 10 ve 11 satırlarını değiştirin. Dosyayı kaydedin.

2. PowerShell ISE'de ...\Öğrenme Modüllerini\İş Sürekliliği ve Olağanüstü Durum Kurtarma\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyasını açın.

    Bu öğreticide, bu PowerShell komut dosyasındaki senaryoların her birini çalıştırın, bu nedenle bu dosyayı açık tutun.

3. Aşağıdakileri ayarlayın:

    $DemoScenario = 1: Kiracı sunucusunu ve havuz yapılandırma bilgilerini katalogda eşitleyen bir arka plan işi başlatın.

4. Eşitleme komut dosyasını çalıştırmak için F5'i seçin. 

    Bu bilgiler daha sonra kurtarma sunucuları, havuzları ve kurtarma bölgesindeki veritabanlarının bir ayna görüntüsü oluşturur sağlamak için kullanılır.  

    ![Eşitleme işlemi](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

PowerShell penceresini arka planda çalıştırın ve bu eğitimin geri kalanıyla devam edin.

> [!NOTE]
> Eşitleme işlemi, bir DNS diğer adı aracılığıyla kataloğa basılanır. Takma ad, etkin kataloğu göndermek için geri yükleme ve geri dönüşsırasında değiştirilir. Eşitleme işlemi, kataloğu kurtarma bölgesinde yapılan herhangi bir veritabanı veya havuz yapılandırma değişikliğiyle güncel tutar. Geri dönüş sırasında, bu değişiklikler orijinal bölgedeki eşdeğer kaynaklara uygulanır.

## <a name="geo-restore-recovery-process-overview"></a>Coğrafi geri yükleme kurtarma sürecine genel bakış

Coğrafi geri yükleme kurtarma işlemi uygulamayı dağıtır ve veritabanlarını yedeklemelerden kurtarma bölgesine geri yükler.

Kurtarma işlemi aşağıdakileri yapar:

1. Orijinal bölgedeki web uygulaması için Azure Trafik Yöneticisi bitiş noktasını devre dışı kılabilir. Bitiş noktasının devre dışı bırakılması, özgün bölgenin kurtarma sırasında çevrimiçi olması durumunda kullanıcıların geçersiz bir durumda uygulamaya bağlanmasını engeller.

2. Kurtarma bölgesindeki bir kurtarma kataloğu sunucusunu karşılar, katalog veritabanını coğrafi olarak geri yükler ve etkin katalog diğer adını geri yüklenen katalog sunucusuna işaret etmek için güncelleştirir. Katalog takma adını değiştirmek, katalog eşitleme işleminin her zaman etkin katalogla eşitlemesini sağlar.

3. Geri yüklenmeden önce kiracı veritabanlarına erişimi önlemek için kurtarma kataloğundaki tüm varolan kiracıları çevrimdışı olarak işaretler.

4. Uygulamanın kurtarma bölgesindeki bir örneğini karşılar ve bu bölgede geri yüklenen kataloğu kullanacak şekilde yapılandırır. Gecikme süresini minimumda tutmak için, örnek uygulama her zaman aynı bölgedeki bir kiracı veritabanına bağlanmak üzere tasarlanmıştır.

5. Yeni kiracıların sağlandığı bir sunucu ve elastik havuz sağlar. Bu kaynakların oluşturulması, yeni kiracıların sağlanmasının varolan kiracıların kurtarılmasını engellememesini sağlar.

6. Kurtarma bölgesindeki yeni kiracı veritabanları için sunucuyu işaret etmek için yeni kiracı takma adını güncelleştirir. Bu diğer adın değiştirilmesi, kurtarma bölgesinde yeni kiracılar için veritabanlarının sağlanmasını sağlar.
        
7. Kiracı veritabanlarını geri getirmek için kurtarma bölgesindeki hükümler sunucuları ve esnek havuzları. Bu sunucular ve havuzlar, özgün bölgedeki yapılandırmanın ayna görüntüsü. Ön havuzları sağlama, tüm veritabanlarını geri yüklemek için gereken kapasiteyi saklı tutar.

    Bir bölgedeki kesinti, eşleştirilmiş bölgedeki kaynaklar üzerinde önemli bir baskı sağlayabilir. DR için coğrafi geri yüklemeye güveniyorsanız, kaynakları hızla ayırmanız önerilir. Bir uygulamanın belirli bir bölgede kurtarılması kritik se, coğrafi çoğaltmayı göz önünde bulundurun. 

8. Kurtarma bölgesindeki web uygulaması için Trafik Yöneticisi bitiş noktasını etkinleştirir. Bu bitiş noktasının etkinleştirilmesi, uygulamanın yeni kiracılar sağlamasına olanak tanır. Bu aşamada, varolan kiracılar hala çevrimdışı.

9. Veritabanlarını öncelik sırasına göre geri yüklemek için toplu istekler gönderir. 

    * Toplu iş, veritabanlarının tüm havuzlarda paralel olarak geri yüklenmeleri için düzenlenir.  

    * Geri yükleme istekleri eşsenkronize olarak gönderilir, böylece hızlı bir şekilde gönderilir ve her havuzda yürütülmesi için sıraya alınır.

    * Geri yükleme istekleri tüm havuzlarda paralel olarak işlendiğinden, önemli kiracıları birçok havuza dağıtmak daha iyidir. 

10. Veritabanlarının ne zaman geri yüklendiğini belirlemek için SQL Veritabanı hizmetini izler. Kiracı veritabanı geri yüklendikten sonra, katalogda çevrimiçi olarak işaretlenir ve kiracı veritabanı için satır çevirme toplamı kaydedilir. 

    * Kiracı veritabanlarına, katalogda çevrimiçi olarak işaretlenir lenmez uygulama tarafından erişilebilir.

    * Kiracı veritabanındaki satır oluşturma değerlerinin toplamı katalogda depolanır. Bu toplam, veritabanının kurtarma bölgesinde güncelleştirilip güncelleştirilemediğini belirlemek için geri gönderme işlemine olanak tanıyan bir parmak izi görevi görür.       

## <a name="run-the-recovery-script"></a>Kurtarma komut dosyasını çalıştırma

> [!IMPORTANT]
> Bu öğretici, coğrafi yedekli yedeklemelerden veritabanlarını geri yükler. Bu yedeklemeler genellikle 10 dakika içinde kullanılabilir olsa da, bir saat kadar sürebilir. Komut dosyası kullanılabilir olana kadar duraklar.

Uygulamanın dağıtıldığı bölgede bir kesinti olduğunu düşünün ve kurtarma komut dosyasını çalıştırın:

1. PowerShell ISE'de,...\Öğrenme Modülleri\İş Sürekliliği ve Olağanüstü Durum Kurtarma\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyasında aşağıdaki değeri ayarlayın:

    $DemoScenario = 2: Coğrafi yedekli yedeklemelerden geri kazanarak uygulamayı kurtarma bölgesine geri dönüştürün.

2. Komut dosyasını çalıştırmak için F5'i seçin.  

    * Komut dosyası yeni bir PowerShell penceresinde açılır ve ardından paralel çalışan bir powershell işleri kümesi başlar. Bu işler sunucuları, havuzları ve veritabanlarını kurtarma bölgesine geri yükler.

    * Kurtarma bölgesi, uygulamayı dağıttığınız Azure bölgesiyle ilişkili eşleştirilmiş bölgedir. Daha fazla bilgi için Azure [eşleştirilmiş bölgelere](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)bakın. 

3. PowerShell penceresinde kurtarma işleminin durumunu izleyin.

    ![Kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

> [!NOTE]
> Kurtarma işlerinin kodunu keşfetmek için ...\Learning Modules\Business Continuity and Disaster Recovery\DR-RestoreFromBackup\RecoveryJobs klasöründeki PowerShell komut dosyalarını inceleyin.

## <a name="review-the-application-state-during-recovery"></a>Kurtarma sırasında uygulama durumunu gözden geçirme
Uygulama bitiş noktası Trafik Yöneticisi'nde devre dışı bırakılırken, uygulama kullanılamıyor. Katalog geri yüklenir ve tüm kiracılar çevrimdışı olarak işaretlenir. Kurtarma bölgesindeki uygulama bitiş noktası etkinleştirilir ve uygulama yeniden çevrimiçi olur. Uygulama kullanılabilir olmasına rağmen, kiracılar veritabanları geri yüklenene kadar olaylar hub'ında çevrimdışı görünür. Uygulamanızı çevrimdışı kiracı veritabanlarını işlemek için tasarlamak önemlidir.

* Katalog veritabanı kurtarıldıktan sonra ancak kiracılar tekrar çevrimiçi olmadan önce web tarayıcınızdaki Wingtip Biletleri etkinlik merkezini yenileyin.

  * Altbilgide, katalog sunucu adının artık bir kurtarma soneki olduğunu ve kurtarma bölgesinde bulunduğunu unutmayın.

  * Henüz geri yüklenmeyen kiracıların çevrimdışı olarak işaretlendiğini ve seçilemez durumda olmadığını unutmayın.   
 
    ![Kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

  * Kiracı çevrimdışıyken kiracının etkinlik sayfasını doğrudan açarsanız, sayfa bir kiracı çevrimdışı bildirimi görüntüler. Örneğin, Contoso Konser Salonu çevrimdışıysa ;kullanıcı http://events.wingtip-dpt.&lt&gt;.trafficmanager.net/contosoconcerthall'u açmayı deneyin.

    ![Kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png)

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Kurtarma bölgesinde yeni bir kiracı sağlama
Kiracı veritabanları geri yüklenmeden önce bile, kurtarma bölgesinde yeni kiracılar sağlayabilirsiniz. Kurtarma bölgesinde sağlanan yeni kiracı veritabanları daha sonra kurtarılan veritabanları ile iade edilir.   

1. PowerShell ISE'de,...\Öğrenme Modülleri\İş Sürekliliği ve Olağanüstü Durum Kurtarma\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyasında aşağıdaki özelliği ayarlayın:

    $DemoScenario = 3: Kurtarma bölgesinde yeni bir kiracı sağlama.

2. Komut dosyasını çalıştırmak için F5'i seçin.

3. Hawthorn Hall etkinlikleri sayfası, sağlama bitince tarayıcıda açılır. 

    Hawthorn Hall veritabanının kurtarma bölgesinde bulunduğuna dikkat edin.

    ![Hawthorn Hall kurtarma bölgesinde sağlanan](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

4. Tarayıcıda, Hawthorn Hall'u görmek için Wingtip Tickets etkinlik merkezi sayfasını yenileyin. 

    Diğer kiracıların geri gelmesini beklemeden Hawthorn Hall'u desteklediyseniz, diğer kiracılar hala çevrimdışı olabilir.

## <a name="review-the-recovered-state-of-the-application"></a>Uygulamanın kurtarılan durumunu gözden geçirme

Kurtarma işlemi sona erdiğinde, uygulama ve tüm kiracılar kurtarma bölgesinde tamamen işlevseldir. 

1. PowerShell konsol penceresindeki ekran tüm kiracıların kurtarıldığını gösterirsonra, olaylar merkezini yenileyin. 

    Kiracıların hepsi internette görünüyor, yeni kiracı Hawthorn Hall da dahil.

    ![Olaylar merkezinde kurtarılan ve yeni kiracılar](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

2. Contoso Konser Salonu'na tıklayın ve etkinlik sayfasını açın. 

    Altbilgide, veritabanının kurtarma bölgesinde bulunan kurtarma sunucusunda bulunduğuna dikkat edin.

    ![Kurtarma bölgesinde Contoso](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

3. Azure [portalında](https://portal.azure.com)kaynak grupları listesini açın.  

    Dağıttığınız kaynak grubuna ve kurtarma kaynak grubuna dikkat edin. Kurtarma kaynak grubu, kurtarma işlemi sırasında oluşturulan tüm kaynakları ve kesinti sırasında oluşturulan yeni kaynakları içerir. 

4. Kurtarma kaynak grubunu açın ve aşağıdaki öğeleri fark edin:

   * Kataloğun ve kiracıların kurtarma sürümleri1 sunucuları, -kurtarma soneki ile. Bu sunucularda geri yüklenen katalog ve kiracı veritabanlarının tümü özgün bölgede kullanılan adlara sahiptir.

   * Kiracı2-dpt-&lt;kullanıcı&gt;-kurtarma SQL sunucusu. Bu sunucu, kesinti sırasında yeni kiracılar ın sağlanması için kullanılır.

   * Uygulama hizmeti olaylar-wingtip-dpt-&lt;kurtarma&gt;-&lt;&gt;bölgesi kullanıcı adlı , hangi olaylar uygulamasının kurtarma örneğidir.

     ![Kurtarma bölgesindeki Contoso kaynakları](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png) 
    
5. Tenants2-dpt-&lt;kullanıcı&gt;kurtarma SQL sunucusunu açın. Veritabanı hawthornhall ve elastik havuz Pool1 içerdiğini unutmayın. Hawthornhall veritabanı Pool1 elastik havuzda elastik bir veritabanı olarak yapılandırılmıştır.

## <a name="change-the-tenant-data"></a>Kiracı verilerini değiştirme 
Bu görevde, geri yüklenen kiracı veritabanlarından birini güncelleştirmiş siniz. Geri gönderme işlemi, özgün bölge olarak değiştirilen veritabanlarını kopyalar. 

1. Tarayıcınızda Contoso Konser Salonu'nun etkinlik listesini bulun, etkinliklerarasında ilerleyin ve son olay olan Seriously Strauss'u fark edin.

2. PowerShell ISE'de,...\Öğrenme Modülleri\İş Sürekliliği ve Olağanüstü Durum Kurtarma\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyasında aşağıdaki değeri ayarlayın:

    $DemoScenario = 4: Kurtarma bölgesindeki bir kiracıdan bir olayı silin.

3. Komut dosyasını çalıştırmak için F5'i seçin.

4. Contoso Konser Salonu etkinlikhttp://events.wingtip-dpt.&ltsayfasını&gt;(;kullanıcı .trafficmanager.net/contosoconcerthall) yenileyin ve olayın Ciddi Strauss'un eksik olduğunu fark edin.

Öğreticinin bu noktasında, şimdi kurtarma bölgesinde çalışan uygulamayı kurtardınız. Kurtarma bölgesinde yeni bir kiracı ve geri yüklenen kiracılardan birinin verilerini değiştirdiniz.  

> [!NOTE]
> Örnekteki diğer öğreticiler, kurtarma durumunda uygulamayla birlikte çalışacak şekilde tasarlanmaz. Diğer öğreticileri keşfetmek istiyorsanız, önce uygulamayı iade ettiğinizden emin olun.

## <a name="repatriation-process-overview"></a>Geri dönüş sürecine genel bakış

Geri gönderme işlemi, bir kesinti çözüldükten sonra uygulamayı ve veritabanlarını özgün bölgesine geri döndürür.

![Coğrafi geri dönüş](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 

İşlem:

1. Devam eden geri yükleme etkinliğini durdurur ve bekleyen veya uçuş içi veritabanı geri yükleme isteklerini iptal eder.

2. Kesintiden bu yana değiştirilmeyen özgün bölge kiracı veritabanlarında yeniden etkinleştirilir. Bu veritabanları, henüz kurtarılmayanlar ve kurtarılanancak daha sonra değiştirilemeyenleri içerir. Yeniden etkinleştirilen veritabanları tam olarak en son kiracıları tarafından erişilir.

3. Hükümler orijinal bölgede yeni kiracı sunucusu ve elastik havuzubir ayna görüntüsü. Bu eylem tamamlandıktan sonra, yeni kiracı diğer adı bu sunucuyu işaret etmek için güncelleştirilir. Takma adın güncelleştirilmesi, kurtarma bölgesi yerine özgün bölgede yeni kiracı onboarding oluşmasına neden olur.

3. Kataloğu kurtarma bölgesinden özgün bölgeye taşımak için coğrafi çoğaltma kullanır.

4. Kesinti sırasında kurtarma bölgesinde yapılan değişikliklerle tutarlı olduğundan, özgün bölgedeki havuz yapılandırmasını güncelleştirir.

5. Kesinti sırasında oluşturulan yeni veritabanlarını barındırmak için gerekli sunucuları ve havuzları oluşturur.

6. Geri yükleme sonrası güncelleştirilen geri yüklenen kiracı veritabanlarını ve kesinti sırasında sağlanan tüm yeni kiracı veritabanlarını geri göndermek için coğrafi çoğaltmayı kullanır. 

7. Geri yükleme işlemi sırasında kurtarma bölgesinde oluşturulan kaynakları temizler.

Geri gönderilmesi gereken kiracı veritabanlarının sayısını sınırlamak için 1 ile 3 arasında adımlar derhal yapılır.  

Adım 4, yalnızca kurtarma bölgesindeki katalog kesinti sırasında değiştirilmişse yapılır. Yeni kiracılar oluşturulursa veya kurtarma bölgesinde herhangi bir veritabanı veya havuz yapılandırması değiştirilirse katalog güncelleştirilir.

Adım 7'nin kiracılarda en az kesintiye neden olması ve hiçbir veri kaybolmaması önemlidir. Bu amaca ulaşmak için, işlem coğrafi çoğaltma kullanır.

Her veritabanı coğrafi çoğaltılmadan önce, özgün bölgedeki ilgili veritabanı silinir. Kurtarma bölgesindeki veritabanı daha sonra coğrafi olarak çoğaltılarak özgün bölgede ikincil bir yineleme oluşturur. Çoğaltma tamamlandıktan sonra, kiracı kurtarma bölgesindeki veritabanına herhangi bir bağlantı tatili kataloğunda çevrimdışı olarak işaretlenir. Veritabanı daha sonra başarısız olur ve bekleyen işlemlerin ikincil işlemde işlemesine neden olur, böylece hiçbir veri kaybolmaz. 

Başarısız olunması üzerine, veritabanı rolleri tersine çevrilir. Özgün bölgedeki ikincil veritabanı birincil okuma yazma veritabanı olur ve kurtarma bölgesindeki veritabanı salt okunur ikincil olur. Katalogdaki kiracı girişi, özgün bölgedeki veritabanına başvurmak üzere güncelleştirilir ve kiracı çevrimiçi olarak işaretlenir. Bu noktada, veritabanının iadesi tamamlandı. 

Uygulamalar, bağlantılar koptuğunda otomatik olarak yeniden bağlanmalarını sağlamak için yeniden deneme mantığıyla yazılmalıdır. Yeniden bağlantıya aracılık etmek için kataloğu kullandıklarında, orijinal bölgedeki iade edilen veritabanına bağlanır. Kısa bağlantı kesilmesi genellikle fark edilmese de, veritabanlarını çalışma saatleri dışında geri göndermeyi seçebilirsiniz.

Veritabanı geri gönderildikten sonra, kurtarma bölgesindeki ikincil veritabanı silinebilir. Özgün bölgedeki veritabanı daha sonra DR koruması için yeniden coğrafi geri yüklemeye dayanır.

8. adımda, kurtarma sunucuları ve havuzları da dahil olmak üzere kurtarma bölgesindeki kaynaklar silinir.

## <a name="run-the-repatriation-script"></a>Geri dönüş komut dosyasını çalıştırma
Kesintinin çözüldüğünü düşünelim ve geri dönüş senaryosunu çalıştıralım.

Öğreticiyi takip ettiyseniz, komut dosyası fabrikam Jazz Club ve Dogwood Dojo'yu orijinal bölgede yeniden etkinleştirir çünkü bunlar değişmez. Daha sonra yeni kiracı, Hawthorn Hall ve Contoso Konser Salonu'nu iade eder, çünkü değiştirilmiştir. Script ayrıca Hawthorn Hall'un sağlanması sırasında güncellenen kataloğu da iade eder.
  
1. PowerShell ISE'de,...\Öğrenme Modülleri\İş Sürekliliği ve Olağanüstü Durum Kurtarma\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyasında, Katalog Eşitleme işleminin PowerShell örneğinde çalışmaya devam ettiğini doğrulayın. Gerekirse, ayarlayarak yeniden başlatın:

    $DemoScenario = 1: Kiracı sunucusu, havuz ve veritabanı yapılandırma bilgilerini katalogda eşitlemeye başlayın.

    Komut dosyasını çalıştırmak için F5'i seçin.

2.  Sonra geri dönüş sürecini başlatmak için, ayarlayın:

    $DemoScenario = 5: Uygulamayı orijinal bölgesine geri gönderin.

    Kurtarma komut dosyasını yeni bir PowerShell penceresinde çalıştırmak için F5'i seçin. Geri dönüş birkaç dakika sürer ve PowerShell penceresindeizden izlenebilir.

3. Komut dosyası çalışırken, olaylar hub sayfasınıhttp://events.wingtip-dpt.&lt(;kullanıcı&gt;.trafficmanager.net) yenileyin.

    Tüm kiracıların çevrimiçi olduğunu ve bu işlem boyunca erişilebilir olduğuna dikkat edin.

4. Açmak için Fabrikam Jazz Club'ı seçin. Bu kiracıyı değiştirmediyseniz, altbilgiden sunucunun zaten özgün sunucuya geri döndürüldene dikkat edin.

5. Contoso Konser Salonu etkinlikleri sayfasını açın veya yenileyin. Altbilgiden, başlangıçta veritabanının hala kurtarma sunucusunda olduğuna dikkat edin. 

6. Geri dönüş işlemi bittiğinde Contoso Concert Hall etkinlikleri sayfasını yenileyin ve veritabanının artık orijinal bölgenizde olduğunu unutmayın.

7. Etkinlik merkezini yeniden yenileyin ve Hawthorn Hall'u açın. Veritabanının orijinal bölgede de bulunduğuna dikkat edin. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Geri döndükten sonra kurtarma bölgesi kaynaklarını temizleme
Geri dönüş tamamlandıktan sonra kurtarma bölgesindeki kaynakları silmek güvenlidir. 

> [!IMPORTANT]
> Bunlar için tüm faturalandırmayı durdurmak için bu kaynakları hemen silin.

Geri yükleme işlemi, kurtarma kaynak grubundaki tüm kurtarma kaynaklarını oluşturur. Temizleme işlemi bu kaynak grubunu siler ve kaynaklara yapılan tüm başvuruları katalogdan kaldırır. 

1. PowerShell ISE'de,...\Öğrenme Modülleri\İş Sürekliliği ve Olağanüstü Durum Kurtarma\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyasında, ayarlayın:
    
    $DemoScenario = 6: Kurtarma bölgesinden eski kaynakları silin.

2. Komut dosyasını çalıştırmak için F5'i seçin.

Komut dosyalarını temizledikten sonra, uygulama başladığı yere geri döndü. Bu noktada, komut dosyasını yeniden çalıştırabilir veya diğer öğreticileri deneyebilirsiniz.

## <a name="designing-the-application-to-ensure-that-the-app-and-the-database-are-co-located"></a>Uygulamanın ve veritabanının birlikte bulunduğundan emin olmak için uygulamanın tasarlanması 
Uygulama her zaman kiracıveritabanı ile aynı bölgede bir örnek bağlanmak için tasarlanmıştır. Bu tasarım, uygulama ve veritabanı arasındaki gecikmesüresini azaltır. Bu optimizasyon, uygulamadan veritabanına etkileşimin kullanıcıdan uygulamaya etkileşimden daha geveze olduğunu varsayar.  

Kiracı veritabanları geri dönüş sırasında bir süre için kurtarma ve özgün bölgeler arasında yayılabilir. Her veritabanı için uygulama, kiracı sunucu adına bir DNS araması yaparak veritabanının bulunduğu bölgeyi arar. SQL Veritabanı'nda sunucu adı bir diğer addır. Diğer adı alan sunucu adı bölge adını içerir. Uygulama veritabanıyla aynı bölgede değilse, veritabanı sunucusuyla aynı bölgedeki örne yönlendirir. Veritabanı yla aynı bölgedeki örne yönlendirme, uygulama ve veritabanı arasındaki gecikmeyi en aza indirir.  

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> 
> * Başka bir bölgede ayna görüntüsü kurtarma ortamı oluşturulmasına olanak tanıyan, düzenli olarak yenilenmiş yapılandırma bilgilerini tutmak için kiracı kataloğunu kullanın.
> * Azure SQL veritabanlarını coğrafi geri yükleme kullanarak kurtarma bölgesine kurtarın.
> * Geri yüklenen kiracı veritabanı konumlarını yansıtacak şekilde kiracı kataloğunu güncelleştirin. 
> * Bir uygulamanın yeniden yapılandırma olmadan kiracı kataloğuna bağlanmasını sağlamak için bir DNS takma adı kullanın.
> * Bir kesinti çözüldükten sonra kurtarılan veritabanlarını orijinal bölgelerine geri göndermek için coğrafi çoğaltmayı kullanın.

Büyük ölçekli çok kiracılı bir uygulamayı kurtarmak için gereken süreyi önemli ölçüde azaltmak için coğrafi çoğaltmanın nasıl kullanılacağını öğrenmek için veritabanı coğrafi çoğaltma öğreticisini [kullanarak çok kiracılı bir SaaS uygulaması](saas-dbpertenant-dr-geo-replication.md) için Olağanüstü Durum kurtarmayı deneyin.

## <a name="additional-resources"></a>Ek kaynaklar

[Wingtip SaaS uygulaması üzerine inşa ek öğreticiler](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
