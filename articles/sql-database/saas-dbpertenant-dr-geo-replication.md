---
title: Geo Replication ile SaaS uygulamaları için Olağanüstü Durum Kurtarma
description: Kesinti durumunda çok kiracılı bir SaaS uygulamasını kurtarmak için Azure SQL Veritabanı coğrafi yinelemelerini nasıl kullanacağınızı öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: AyoOlubeko
ms.author: craigg
ms.reviewer: sstein
ms.date: 01/25/2019
ms.openlocfilehash: 0668ccf5ceb972dd120e4e3f37be6d879a12d0a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73811717"
---
# <a name="disaster-recovery-for-a-multi-tenant-saas-application-using-database-geo-replication"></a>Veritabanı coğrafi çoğaltma kullanarak çok kiracılı SaaS uygulaması için olağanüstü durum kurtarma

Bu eğitimde, veritabanı başına kiracı modeli kullanılarak uygulanan çok kiracılı Bir SaaS uygulaması için tam bir olağanüstü durum kurtarma senaryosunu keşfe çıkıyorsunuz. Uygulamayı bir kesintiden korumak için, alternatif bir kurtarma bölgesinde katalog ve kiracı veritabanları için yinelemeler oluşturmak için [_coğrafi çoğaltmayı_](sql-database-geo-replication-overview.md) kullanırsınız. Bir kesinti oluşursa, normal iş işlemlerine devam etmek için bu yinelemeler üzerinde hızlı bir şekilde başarısız olur. Başarısız olunması üzerine, özgün bölgedeki veritabanları kurtarma bölgesindeki veritabanlarının ikincil yinelemeleri haline gelir. Bu yinelemeler çevrimiçi olduktan sonra otomatik olarak kurtarma bölgesindeki veritabanlarının durumuna yetişir. Kesinti çözüldükten sonra, özgün üretim bölgesindeki veritabanlarına geri dönmezsiniz.

Bu öğretici, hem failover hem de failback iş akışlarını inceler. Şunları öğrenirsiniz:
> [!div class="checklist"]
> 
> * Veritabanını ve esnek havuz yapılandırma bilgilerini kiracı kataloğuna senkronize edin
> * Uygulama, sunucular ve havuzlardan oluşan alternatif bir bölgede kurtarma ortamı ayarlama
> * Kataloğu ve kiracı veritabanlarını kurtarma bölgesine çoğaltmak için _coğrafi çoğaltmayı_ kullanın
> * Kurtarma bölgesine uygulama ve katalog ve kiracı veritabanları üzerinde başarısız 
> * Daha sonra, kesinti çözüldükten sonra uygulama, katalog ve kiracı veritabanları üzerinde orijinal bölgeye geri başarısız
> * Her kiracı veritabanı, her kiracının veritabanının birincil konumunu izlemek için başarısız olduğundan kataloğu güncelleştirin
> * Gecikme süresini azaltmak için uygulama nın ve birincil kiracı veritabanının her zaman aynı Azure bölgesinde bir konumda olduğundan emin olun  
 

Bu öğreticiye başlamadan önce, aşağıdaki ön koşulların tamamlandığından emin olun:
* Kiracı başına Wingtip Tickets SaaS veritabanı dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için, [kiracı uygulaması başına Wingtip Tickets SaaS veritabanını dağıt'a](saas-dbpertenant-get-started-deploy.md) bakın ve keşfedin  
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-geo-replication-recovery-pattern"></a>Coğrafi çoğaltma kurtarma desenine giriş

![Kurtarma Mimarisi](media/saas-dbpertenant-dr-geo-replication/recovery-architecture.png)
 
Olağanüstü durum kurtarma (DR) uyumluluk nedenleriyle veya iş sürekliliği için olsun, birçok uygulama için önemli bir husustur. Uzun süreli bir hizmet kesintisi olması halinde, iyi hazırlanmış bir DR planı iş aksamasını en aza indirebilir. Coğrafi çoğaltma kullanarak kısa sürede üzerinde başarısız olabilir bir kurtarma bölgesinde veritabanı yinelemeleri koruyarak en düşük RPO ve RTO sağlar.

Coğrafi çoğaltmaya dayalı bir DR planı üç ayrı bölümden oluşur:
* Kurulum - kurtarma ortamının oluşturulması ve bakımı
* Kurtarma - bir kesinti oluşursa uygulama ve veritabanlarının kurtarma ortamına başarısız olması,
* Geri dönüş - uygulama çözüldükten sonra uygulama nın ve veritabanlarının orijinal bölgeye geri gönderilmesi 

Tüm parçalar, özellikle ölçekte çalışıyorsa dikkatli bir şekilde düşünülmalıdır. Genel olarak, plan birkaç hedefe ulaşmak gerekir:

* Kurulum
    * Kurtarma bölgesinde bir ayna görüntüsü ortamı kurun ve koruyun. Elastik havuzları oluşturma ve bu kurtarma ortamındaki veritabanlarının çoğaltılamazlığı kurtarma bölgesindeki kapasiteyi ayırır. Bu ortamın korunması, yeni kiracı veritabanlarının sağlanırken çoğaltılmalarını içerir.  
* Kurtarma
    * Günlük maliyetleri en aza indirmek için küçültülmüş bir geri kazanım ortamının kullanıldığı durumlarda, havuzlar ve veritabanları kurtarma bölgesinde tam operasyonel kapasite elde etmek için ölçeklendirilmelidir
    * Kurtarma bölgesinde en kısa sürede yeni kiracı sağlama yı etkinleştirme  
    * Kiracıları öncelikli sıraya geri getirmek için optimize edin
    * Pratik yerlerde paralel adımlar atarak kiracıları mümkün olan en kısa sürede çevrimiçi duruma almak için optimize edin
    * Başarısızlığa karşı dirençli, yeniden başlatılabilir ve idempotent olun
    * Orijinal bölge tekrar devreye girdiğinde, uçuşun ortasında ki işlemi iptal etmek mümkün olabilir.
* Geri dönüş 
    * Kurtarma bölgesinden, kiracılar için en az etkiye sahip orijinal bölgedeki yinelemelere kadar veritabanları üzerinde başarısız olun: veri kaybı yok ve kiracı başına minimum çevrimdışı süre yok.   

Bu öğreticide, bu zorluklar Azure SQL Veritabanı ve Azure platformu özellikleri kullanılarak ele alınmıştır:

* [Azure Kaynak Yöneticisi,](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)gerekli tüm kapasiteyi mümkün olan en kısa sürede ayırmak için şablonlar. Azure Kaynak Yöneticisi şablonları, kurtarma bölgesindeki üretim sunucularının ve esnek havuzların ayna görüntüsünü sağlamak için kullanılır.
* [Coğrafi çoğaltma](sql-database-geo-replication-overview.md), tüm veritabanları için eş zamanlı olarak çoğaltılmış salt okunur ikincilikler oluşturmak için. Bir kesinti sırasında, kurtarma bölgesindeki yinelemeler üzerinde başarısız.  Kesinti çözüldükten sonra, veri kaybı olmadan özgün bölgedeki veritabanlarına geri dönmezsiniz.
* Çok sayıda veritabanının zaman içinde başarısız olmasını en aza indirmek için kiracı öncelikli sıraya gönderilen [eşzamanlı](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) hata lar.
* [Shard yönetimi kurtarma özellikleri,](sql-database-elastic-database-recovery-manager.md)kurtarma ve geri gönderme sırasında katalogveritabanı girişlerini değiştirmek için. Bu özellikler, uygulamanın konumune bakılmaksızın uygulamayı yeniden yapılandırmadan kiracı veritabanlarına bağlanmasını sağlar.
* [SQL sunucusu DNS diğer adları,](dns-alias-overview.md)uygulamanın hangi bölgede çalıştığına bakılmaksızın yeni kiracıların sorunsuz bir şekilde sağlanmasını sağlamak için. DNS diğer adları, katalog eşitleme işleminin konumune bakılmaksızın etkin kataloğerine bağlanmasına da izin vermek için kullanılır.

## <a name="get-the-disaster-recovery-scripts"></a>Olağanüstü durum kurtarma komut dosyalarını alın 

> [!IMPORTANT]
> Tüm Wingtip Tickets yönetim senaryoları gibi, DR komut örnekleri de örnek kalitededir ve üretimde kullanılmamalıdır. 

Bu öğretici ve Wingtip uygulama kaynak kodu kullanılan kurtarma komut kiracı [GitHub deposu başına Wingtip Biletleri SaaS veritabanında](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/)mevcuttur. Wingtip Tickets yönetim komut dosyalarını indirmek ve engelini kaldırmak için atılacak adımlar için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md) göz atın.

## <a name="tutorial-overview"></a>Öğreticiye genel bakış
Bu eğitimde, ilk olarak Wingtip Tickets uygulamasının ve veritabanlarının farklı bir bölgedeki kopyalarını oluşturmak için coğrafi çoğaltmayı kullanırsınız. Daha sonra, bir kesintikurtarma simüle etmek için bu bölgeye üzerinde başarısız. Tamamlandığında, uygulama kurtarma bölgesinde tamamen işlevseldir.

Daha sonra, ayrı bir geri gönderme adımında, kurtarma bölgesindeki katalog ve kiracı veritabanları üzerinde özgün bölgeye başarısız oluyorsunuz. Uygulama ve veritabanları geri dönüş boyunca kullanılabilir kalır. Tamamlandığında, uygulama orijinal bölgede tamamen işlevseldir.

> [!Note]
> Uygulama, uygulamanın _dağıtıldığı bölgenin eşleşmiş bölgesine_ kurtarılır. Daha fazla bilgi için Azure [eşleştirilmiş bölgelere](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)bakın.

## <a name="review-the-healthy-state-of-the-application"></a>Uygulamanın sağlıklı durumunu gözden geçirin

Kurtarma işlemini başlatmadan önce, uygulamanın normal sağlıklı durumunu gözden geçirin.
1. Web tarayıcınızda Wingtip Tickets Events Hub'ı açın (http://events.wingtip-dpt.&lt;kullanıcı&gt;.trafficmanager.net - kullanıcıyı &lt;&gt; dağıtımınızın kullanıcı değeriyle değiştirin).
    * Sayfanın altına gidin ve altbilgideki katalog sunucusu adını ve konumunu fark edin. Konum, uygulamayı dağıttığınız bölgedir.
    *İpucu: Ekranı büyütmek için fareyi konumun üzerine titretin.* 
    Özgün bölgede olaylar merkezi sağlıklı ![durum](media/saas-dbpertenant-dr-geo-replication/events-hub-original-region.png)

2. Contoso Konser Salonu kiracı tıklayın ve etkinlik sayfasını açın.
    * Altbilgide, kiracı sunucu adını fark edin. Konum, katalog sunucusunun konumuyla aynı olacaktır.

3. Azure [portalında,](https://portal.azure.com)uygulamanın dağıtıldığı kaynak grubunu açın
    * Sunucuların dağıtıldığı bölgeye dikkat edin. 

## <a name="sync-tenant-configuration-into-catalog"></a>Kiracı yapılandırmayı katalogolarak eşitleme

Bu görevde, sunucuların, elastik havuzların ve veritabanlarının yapılandırmasını kiracı kataloğuna eşitleyen bir işlem başlatın. İşlem, bu bilgileri katalogda güncel tutar.  İşlem, ister özgün bölgede ister kurtarma bölgesinde olsun, etkin katalogla çalışır. Yapılandırma bilgileri, kurtarma ortamının özgün ortamla tutarlı olmasını sağlamak için kurtarma işleminin bir parçası olarak ve daha sonra geri dönüş sırasında orijinal bölgenin kurtarma ortamı. Katalog, kiracı kaynaklarının kurtarma durumunu izlemek için de kullanılır

> [!IMPORTANT]
> Basitlik için, senkronizasyon işlemi ve diğer uzun süren kurtarma ve geri gönderme işlemleri bu öğreticilerde istemci kullanıcı oturumunuz altında çalışan yerel PowerShell işleri veya oturumları olarak uygulanır. Oturum açtığınızda verilen kimlik doğrulama belirteçleri birkaç saat sonra sona erer ve işler başarısız olur. Bir üretim senaryosunda, uzun süren işlemler bir hizmet ilkesi altında çalışan bir tür güvenilir Azure hizmeti olarak uygulanmalıdır. Bkz. [Sertifikası olan bir hizmet sorumlusu oluşturmak için Azure PowerShell'i kullanın.](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)

1. _PowerShell ISE'de_...\Öğrenme Modülleri\UserConfig.psm1 dosyasını açın. Uygulamayı `<resourcegroup>` `<user>` dağıtırken kullanılan değerle 10 ve 11 satırlarını değiştirin.  Dosyayı kaydedin!

2. *PowerShell ISE,* açın ...\Öğrenme Modülleri\İş Sürekliliği ve Olağanüstü Durum Kurtarma\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyası ve set:
    * **$DemoScenario = 1**, Kiracı sunucusunu eşitleyen bir arka plan işi başlatma ve yapılandırma bilgilerini katalogda havuza ekleme

3. Eşitleme komut dosyasını çalıştırmak için **F5** tuşuna basın. Kiracı kaynaklarının yapılandırmasını eşitlemek için yeni bir PowerShell oturumu açılır.
![Eşitleme işlemi](media/saas-dbpertenant-dr-geo-replication/sync-process.png)

PowerShell penceresini arka planda çalıştırın ve eğitimin geri kalanıyla devam edin. 

> [!Note]
> Eşitleme işlemi, bir DNS diğer adı aracılığıyla kataloğa basılanır. Bu diğer ad, etkin kataloğu göndermek için geri yükleme ve geri dönüşsırasında değiştirilir. Eşitleme işlemi, kataloğu kurtarma bölgesinde yapılan herhangi bir veritabanı veya havuz yapılandırma değişiklikleriyle güncel tutar.  Geri dönüş sırasında, bu değişiklikler orijinal bölgedeki eşdeğer kaynaklara uygulanır.

## <a name="create-secondary-database-replicas-in-the-recovery-region"></a>Kurtarma bölgesinde ikincil veritabanı yinelemeleri oluşturma

Bu görevde, yinelenen bir uygulama örneğini dağıtan ve kataloğu ve tüm kiracı veritabanlarını kurtarma bölgesine çoğaltır bir işlem başlatırsınız.

> [!Note]
> Bu öğretici Wingtip Tickets örnek uygulamasına coğrafi çoğaltma koruması ekler. Coğrafi çoğaltma kullanan bir uygulama için bir üretim senaryosunda, her kiracı başından itibaren coğrafi olarak çoğaltılmış bir veritabanıyla birlikte sağlanacaktır. Bkz. [Azure SQL Veritabanı'nı kullanarak yüksek kullanılabilirlikte hizmet tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)

1. *PowerShell ISE,* açın ...\Öğrenme Modülleri\İş Sürekliliği ve Olağanüstü Durum Kurtarma\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyası ve aşağıdaki değerleri ayarlayın:
    * **$DemoScenario = 2**, Ayna görüntü kurtarma ortamı oluşturma ve katalog ve kiracı veritabanları çoğaltmak

2. Betiği çalıştırmak için **F5**'e basın. Kopyaları oluşturmak için yeni bir PowerShell oturumu açılır.
![Eşitleme işlemi](media/saas-dbpertenant-dr-geo-replication/replication-process.png)  

## <a name="review-the-normal-application-state"></a>Normal uygulama durumunu gözden geçirme

Bu noktada, uygulama normal olarak özgün bölgede çalışıyor ve şimdi coğrafi çoğaltma ile korunuyor.  Salt okunur ikincil yinelemeler, tüm veritabanları için kurtarma bölgesinde bulunur. 

1. Azure portalında, kaynak gruplarınıza bakın ve kurtarma bölgesinde kurtarma soneki yle bir kaynak grubu oluşturulduğunu unutmayın. 

2. Kurtarma kaynak grubundaki kaynakları keşfedin.  

3. _Kiracılar1-dpt-&lt;&gt;kullanıcı kurtarma_ sunucusundaki Contoso Concert Hall veritabanına tıklayın.  Sol taraftaki Coğrafi Çoğaltma'ya tıklayın. 

    ![Contoso Konser coğrafi çoğaltma bağlantısı](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication.png) 

Azure bölgeleri haritasında, özgün bölgedeki birincil ile kurtarma bölgesindeki ikincil alan arasındaki coğrafi çoğaltma bağlantısını unutmayın.  

## <a name="fail-over-the-application-into-the-recovery-region"></a>Kurtarma bölgesine uygulama üzerinde başarısız

### <a name="geo-replication-recovery-process-overview"></a>Coğrafi çoğaltma kurtarma sürecine genel bakış

Kurtarma komut dosyası aşağıdaki görevleri gerçekleştirir:

1. Orijinal bölgedeki web uygulaması için Trafik Yöneticisi bitiş noktasını devre dışı kılabilir. Bitiş noktasının devre dışı bırakılması, özgün bölgenin kurtarma sırasında çevrimiçi olması durumunda kullanıcıların geçersiz bir durumda uygulamaya bağlanmasını engeller.

1. Birincil veritabanı yapmak için kurtarma bölgesindeki katalog veritabanının bir kuvvet başarısızlığı kullanır ve kurtarma kataloğu sunucusuna işaret etmek için _etkin katalog_ takma güncelleştirir.

1. Kurtarma bölgesindeki kiracı sunucuyu işaret etmek için _yeni gelen kiracı_ takma adını güncelleştirir. Bu diğer adın değiştirilmesi, kurtarma bölgesinde yeni kiracıların veritabanlarının sağlanmasını sağlar. 

1. Kurtarma kataloğundaki varolan tüm kiracıları, başarısız olmadan önce kiracı veritabanlarına erişimi önlemek için çevrimdışı olarak işaretler.

1. Özgün bölgedeki yapılandırmalarını yansıtmak için tüm esnek havuzların ve kurtarma bölgesindeki çoğaltılan tek veritabanlarının yapılandırmasını güncelleştirir. (Bu görev yalnızca kurtarma ortamındaki havuzlar veya çoğaltılan veritabanları maliyetleri azaltmak için normal işlemler sırasında küçültüldüyse gereklidir).

1. Kurtarma bölgesindeki web uygulaması için Trafik Yöneticisi bitiş noktasını etkinleştirir. Bu bitiş noktasının etkinleştirilmesi, uygulamanın yeni kiracılar sağlamasına olanak tanır. Bu aşamada, varolan kiracılar hala çevrimdışı.

1. Öncelik sırasına göre veritabanları üzerinde başarısız zorlama isteği toplu gönderir.
    * Toplu iş, veritabanlarının tüm havuzlarda paralel olarak başarısız olması için düzenlenir.
    * Failover istekleri eşzamanlı işlemler kullanılarak gönderilir, böylece hızlı bir şekilde gönderilir ve birden çok istek aynı anda işlenebilir.

   > [!Note]
   > Bir kesinti senaryosunda, özgün bölgedeki birincil veritabanları çevrimdışıdır.  Herhangi bir artık sıralı hareketleri uygulamak için denemeden birincil bağlantı ikincil tatili üzerinde kuvvet başarısız. Bu öğretici gibi bir DR matkap senaryoda, başarısız olduğu anda herhangi bir güncelleştirme etkinliği varsa bazı veri kaybı olabilir. Daha sonra, geri dönüş sırasında, kurtarma bölgesindeki veritabanları üzerinde başarısız olduğunuzda, veri kaybı olmadığından emin olmak için normal bir hata kullanılır.

1. Veritabanları üzerinde başarısız olduğunda belirlemek için SQL veritabanı hizmetini izler. Kiracı veritabanı başarısız olduğunda, kiracı veritabanının kurtarma durumunu kaydetmek ve kiracıyı çevrimiçi olarak işaretlemek için kataloğu güncelleştirir.
    * Kiracı veritabanlarına, katalogda çevrimiçi olarak işaretlenir lenmez uygulama tarafından erişilebilir.
    * Kiracı veritabanındaki satır oluşturma değerlerinin toplamı katalogda depolanır. Bu değer, geri gönderme işleminin veritabanının kurtarma bölgesinde güncelleştirilip güncelleştirilemediğini belirlemesine olanak tanıyan bir parmak izi görevi görür.

### <a name="run-the-script-to-fail-over-to-the-recovery-region"></a>Komut dosyasını kurtarma bölgesine başarısız olması için çalıştırma

Şimdi, uygulamanın dağıtıldığı bölgede bir kesinti olduğunu ve kurtarma komut dosyasını çalıştırın:

1. *PowerShell ISE,* açın ...\Öğrenme Modülleri\İş Sürekliliği ve Olağanüstü Durum Kurtarma\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyası ve aşağıdaki değerleri ayarlayın:
    * **$DemoScenario = 3**, Kopyaları üzerinde başarısız olarak bir kurtarma bölgesine uygulama kurtarma

2. Betiği çalıştırmak için **F5**'e basın.  
    * Komut dosyası yeni bir PowerShell penceresinde açılır ve daha sonra paralel çalışan powershell işleri bir dizi başlar. Bu işler kurtarma bölgesine kiracı veritabanları üzerinde başarısız.
    * Kurtarma bölgesi, uygulamayı dağıttığınız Azure bölgesiyle ilişkili _eşleştirilmiş bölgedir._ Daha fazla bilgi için Azure [eşleştirilmiş bölgelere](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)bakın. 

3. PowerShell penceresinde kurtarma işleminin durumunu izleyin.
    ![başarısız süreci](media/saas-dbpertenant-dr-geo-replication/failover-process.png)

> [!Note]
> Kurtarma işlerinin kodunu keşfetmek için ...\Learning Modules\Business Continuity and Disaster Recovery\DR-FailoverToReplica\RecoveryJobs klasöründeki PowerShell komut dosyalarını inceleyin.

### <a name="review-the-application-state-during-recovery"></a>Kurtarma sırasında uygulama durumunu gözden geçirme

Uygulama bitiş noktası Trafik Yöneticisi'nde devre dışı bırakılırken, uygulama kullanılamıyor. Katalog kurtarma bölgesine başarısız olduktan ve tüm kiracılar çevrimdışı işaretlendikten sonra, uygulama yeniden çevrimiçi duruma getirilir. Uygulama kullanılabilir olmasına rağmen, veritabanı başarısız olana kadar her kiracı olaylar hub'ında çevrimdışı görünür. Uygulamanızı çevrimdışı kiracı veritabanlarını işlemek için tasarlamak önemlidir.

1. Katalog veritabanı kurtarıldıktan hemen sonra, web tarayıcınızdaki Wingtip Tickets Events Hub'ını yenileyin.
   * Altbilgide, katalog sunucu adının artık bir _kurtarma_ soneki olduğunu ve kurtarma bölgesinde bulunduğunu unutmayın.
   * Henüz geri yüklenmemiş, çevrimdışı olarak işaretlenmiş ve seçilemez durumda olmayan kiracılara dikkat edin.  

     > [!Note]
     > Kurtarılacak yalnızca birkaç veritabanları yla, kurtarma tamamlanmadan önce tarayıcıyı yenileyemeyebilirsiniz, bu nedenle kiracıları çevrimdışıyken göremeyebilirsiniz. 
 
     ![Olaylar merkezi çevrimdışı](media/saas-dbpertenant-dr-geo-replication/events-hub-offlinemode.png) 

   * Çevrimdışı bir kiracının Etkinlikler sayfasını doğrudan açarsanız, 'kiracı çevrimdışı' bildirimi görüntüler. Örneğin, Contoso Konser Salonu çevrimdışıysa, ;kullanıcı http://events.wingtip-dpt.&lt&gt;.trafficmanager.net/contosoconcerthall ![Contoso Çevrimdışı sayfasını açmayı deneyin](media/saas-dbpertenant-dr-geo-replication/dr-in-progress-offline-contosoconcerthall.png) 

### <a name="provision-a-new-tenant-in-the-recovery-region"></a>Kurtarma bölgesinde yeni bir kiracı sağlama
Varolan tüm kiracı veritabanları başarısız olmadan önce bile, kurtarma bölgesinde yeni kiracılar sağlayabilirsiniz.  

1. *PowerShell ISE,* açın ...\Öğrenme Modülleri\İş Sürekliliği ve Olağanüstü Durum Kurtarma\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyası ve aşağıdaki özelliği ayarlayın:
    * **$DemoScenario = 4**, Kurtarma bölgesinde yeni bir kiracı sağlama

2. Komut dosyasını çalıştırmak ve yeni kiracıyı sağlamak için **F5** tuşuna basın. 

3. Hawthorn Hall etkinlikleri sayfası tamamlandığında tarayıcıda açılır. Altbilgiden Hawthorn Hall veritabanının kurtarma bölgesinde sağlanmış olduğunu unutmayın.
    ![Hawthorn Hall Etkinlikler Sayfası](media/saas-dbpertenant-dr-geo-replication/hawthornhallevents.png) 

4. Tarayıcıda, Hawthorn Hall'u görmek için Wingtip Tickets Events Hub sayfasını yenileyin. 
    * Diğer kiracıların geri gelmesini beklemeden Hawthorn Hall'u desteklediyseniz, diğer kiracılar hala çevrimdışı olabilir.


## <a name="review-the-recovered-state-of-the-application"></a>Uygulamanın kurtarılan durumunu gözden geçirme

Kurtarma işlemi tamamlandığında, uygulama ve tüm kiracılar kurtarma bölgesinde tamamen işlevseldir. 

1. PowerShell konsol penceresindeki ekran tüm kiracıların kurtarıldığını gösterdiğinde, Etkinlikler Hub'ını yenileyin.  Kiracıların hepsi internette görünecek, yeni kiracı Hawthorn Hall da dahil.

    ![olaylar merkezinde kurtarılan ve yeni kiracılar](media/saas-dbpertenant-dr-geo-replication/events-hub-with-hawthorn-hall.png)

2. Azure [portalında](https://portal.azure.com)kaynak grupları listesini açın.  
    * Dağıttığınız kaynak grubuna ve kurtarma kaynak grubuna _dikkat_ edin.  Kurtarma kaynak grubu, kurtarma işlemi sırasında oluşturulan tüm kaynakları ve kesinti sırasında oluşturulan yeni kaynakları içerir.  

3. Kurtarma kaynak grubunu açın ve aşağıdaki öğeleri fark edin:
   * Kataloğun ve kiracıların kurtarma sürümleri1 sunucuları, _-kurtarma_ soneki ile.  Bu sunucularda geri yüklenen katalog ve kiracı veritabanlarının tümü özgün bölgede kullanılan adlara sahiptir.

   * _Kiracı2-dpt-&lt;&gt;kullanıcı -kurtarma_ SQL sunucusu.  Bu sunucu, kesinti sırasında yeni kiracılar ın sağlanması için kullanılır.
   * Adlandırılmış App Service, _olaylar-wingtip-dpt-&lt;recoveryregion&gt;-&lt;kullanıcı&gt_& 

     ![Azure kurtarma kaynakları](media/saas-dbpertenant-dr-geo-replication/resources-in-recovery-region.png) 
    
4. _&lt;Tenants2-dpt- kullanıcı&gt;kurtarma_ SQL sunucusunu açın.  Veritabanı _hawthornhall_ ve elastik havuz, _Pool1_içerir dikkat edin.  _Hawthornhall_ veritabanı _Pool1_ elastik havuzda elastik bir veritabanı olarak yapılandırılmıştır.

5. Kaynak grubuna geri gidin ve _kiracılar1-dpt-&lt;&gt;kullanıcı kurtarma_ sunucusundaki Contoso Concert Hall veritabanına tıklayın. Sol taraftaki Coğrafi Çoğaltma'ya tıklayın.
    
    ![Failover sonra Contoso veritabanı](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication-after-failover.png)

## <a name="change-tenant-data"></a>Kiracı verilerini değiştirme 
Bu görevde, kiracı veritabanlarından birini güncelleştirmişsiniz. 

1. Tarayıcınızda Contoso Konser Salonu'nun etkinlik listesini bulun ve son etkinlik adını not edin.
2. *PowerShell ISE,* içinde ...\Öğrenme Modülleri \İş Sürekliliği ve Afet Kurtarma\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyası, aşağıdaki değeri ayarlayın:
    * **$DemoScenario = 5** Kurtarma bölgesindeki bir kiracıdan bir olayı silme
3. Komut dosyasını çalıştırmak için **F5** tuşuna basın
4. Contoso Konser Salonu etkinliklerihttp://events.wingtip-dpt.&ltsayfasını&gt;yenileyin ( &lt;&gt; ;kullanıcı .trafficmanager.net/contosoconcerthall - dağıtımınızın kullanıcı değeriyle yedek kullanıcı) ve son etkinliğin silindiğini fark edin.

## <a name="repatriate-the-application-to-its-original-production-region"></a>Uygulamayı orijinal üretim bölgesine geri gönderme

Bu görev, uygulamayı özgün bölgesine geri döndürer. Gerçek bir senaryoda, kesinti çözüldüğünde geri dönüş başlatın.

### <a name="repatriation-process-overview"></a>Geri dönüş sürecine genel bakış

![Geri Dönüş Mimarisi](media/saas-dbpertenant-dr-geo-replication/repatriation-architecture.png)

Geri dönüş süreci:
1. Bekleyen veya uçuş içi veritabanı geri yükleme isteklerini iptal eder.
2. Yeni _kiracı_ takma adını, kiracıların başlangıç bölgesindeki sunucusunu işaret etmek için güncelleştirir. Bu diğer adın değiştirilmesi, yeni kiracıların veritabanlarının artık kaynak bölgede sağlanmasını sağlar.
3. Değiştirilen kiracı verilerini orijinal bölgeye tohumlar
4. Öncelikli sırada kiracı veritabanları üzerinde başarısız olur.

Failover veritabanını etkili bir şekilde özgün bölgeye taşır. Veritabanı üzerinde başarısız olduğunda, tüm açık bağlantılar bırakılır ve veritabanı birkaç saniye için kullanılamaz. Uygulamalar yeniden bağlanmalarını sağlamak için yeniden deneme mantığıyla yazılmalıdır.  Bu kısa bağlantı kesilmesi genellikle fark edilmese de, veritabanlarını çalışma saatleri dışında geri göndermeyi seçebilirsiniz. 


### <a name="run-the-repatriation-script"></a>Geri dönüş komut dosyasını çalıştırma
Şimdi kesintinin çözüldüğünü düşünelim ve geri dönüş senaryosunu çalıştıralım.

1. *PowerShell ISE*, içinde ...\Öğrenme Modülleri \İş Sürekliliği ve Afet Kurtarma\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyası.

2. Katalog Eşitleme işleminin PowerShell örneğinde çalışmaya devam ettiğini doğrulayın.  Gerekirse, ayarlayarak yeniden başlatın:
    * **$DemoScenario = 1**, Kiracı sunucusu, havuz ve veritabanı yapılandırma bilgilerini katalogda eşitlemeye başlayın
    * Betiği çalıştırmak için **F5**'e basın.

3.  Sonra geri dönüş sürecini başlatmak için, ayarlayın:
    * **$DemoScenario = 6**, Uygulamayı orijinal bölgesine geri gönder
    * Kurtarma komut dosyasını yeni bir PowerShell penceresinde çalıştırmak için **F5** tuşuna basın.  Geri dönüş birkaç dakika sürer ve PowerShell penceresindeizden izlenebilir.
    ![Geri dönüş süreci](media/saas-dbpertenant-dr-geo-replication/repatriation-process.png)

4. Komut dosyası çalışırken, Events Hub sayfasınıhttp://events.wingtip-dpt.&lt(;kullanıcı&gt;.trafficmanager.net) yenileyin
    * Tüm kiracıların çevrimiçi olduğunu ve bu işlem boyunca erişilebilir olduğuna dikkat edin.

5. Geri dönüş tamamlandıktan sonra Etkinlikler merkezini yenileyin ve Hawthorn Hall için etkinlik sayfasını açın. Bu veritabanının özgün bölgeye geri gönderildiğine dikkat edin.
    ![Etkinlikler merkezi iade edildi](media/saas-dbpertenant-dr-geo-replication/events-hub-repatriated.png)


## <a name="designing-the-application-to-ensure-app-and-database-are-colocated"></a>Uygulama ve veritabanının bir konuma ilmesini sağlamak için uygulamanın tasarlanması 
Uygulama, her zaman kiracı veritabanı ile aynı bölgedeki bir örnekten bağlanacak şekilde tasarlanmıştır. Bu tasarım, uygulama ve veritabanı arasındaki gecikmesüresini azaltır. Bu optimizasyon, uygulamadan veritabanına etkileşimin kullanıcıdan uygulamaya etkileşimden daha geveze olduğunu varsayar.  

Kiracı veritabanları geri dönüş sırasında bir süre için kurtarma ve özgün bölgeler arasında yayılabilir. Her veritabanı için uygulama, kiracı sunucu adına bir DNS araması yaparak veritabanının bulunduğu bölgeyi arar. SQL Veritabanı'nda sunucu adı bir diğer addır. Diğer adı alan sunucu adı bölge adını içerir. Uygulama veritabanıyla aynı bölgede değilse, veritabanı sunucusuyla aynı bölgedeki örne yönlendirir.  Veritabanı yla aynı bölgedeki örne yönlendirme, uygulama ve veritabanı arasındaki gecikmeyi en aza indirir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> 
> * Veritabanını ve esnek havuz yapılandırma bilgilerini kiracı kataloğuna senkronize edin
> * Uygulama, sunucular ve havuzlardan oluşan alternatif bir bölgede kurtarma ortamı ayarlama
> * Kataloğu ve kiracı veritabanlarını kurtarma bölgesine çoğaltmak için _coğrafi çoğaltmayı_ kullanın
> * Kurtarma bölgesine uygulama ve katalog ve kiracı veritabanları üzerinde başarısız 
> * Kesinti çözüldükten sonra uygulama, katalog ve kiracı veritabanlarını özgün bölgeye geri verme

Azure SQL veritabanının [İş Sürekliliğine Genel Bakış](sql-database-business-continuity.md) belgelerinde iş sürekliliğini etkinleştirmek için sağladığı teknolojiler hakkında daha fazla bilgi edinebilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip SaaS uygulaması üzerine inşa ek öğreticiler](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
