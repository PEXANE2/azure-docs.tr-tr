---
title: Olağanüstü durum kurtarma çözümleri tasarla
description: Doğru başarısız desen seçerek olağanüstü durum kurtarma için bulut çözümünüzü nasıl tasarlarınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
ms.date: 01/25/2019
ms.openlocfilehash: 4eeaa187142a6d0d97b12f685ebc455f3844606f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73825868"
---
# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pools"></a>SQL Veritabanı elastik havuzları kullanan uygulamalar için olağanüstü durum kurtarma stratejileri

Yıllar geçtikçe bulut hizmetlerinin kusursuz olmadığını ve felaket olayların yaşandığını öğrendik. SQL Veritabanı, bu olaylar meydana geldiğinde uygulamanızın iş sürekliliğini sağlamak için çeşitli özellikler sağlar. [Elastik havuzlar](sql-database-elastic-pool.md) ve tek veritabanları aynı tür olağanüstü durum kurtarma (DR) özelliklerini destekler. Bu makalede, bu SQL Veritabanı iş sürekliliği özelliklerinden yararlanan esnek havuzlar için çeşitli DR stratejileri açıklanmaktadır.

Bu makalede, aşağıdaki kanonik SaaS ISV uygulama deseni kullanır:

Modern bulut tabanlı web uygulaması, her son kullanıcı için bir SQL veritabanı sağlar. ISV'nin birçok müşterisi vardır ve bu nedenle kiracı veritabanları olarak bilinen birçok veritabanları nı kullanır. Kiracı veritabanları genellikle öngörülemeyen etkinlik desenleri olduğundan, ISV veritabanı maliyetini uzun süreler boyunca çok öngörülebilir hale getirmek için elastik bir havuz kullanır. Elastik havuz, kullanıcı etkinliği arttığında performans yönetimini de kolaylaştırır. Kiracı veritabanlarına ek olarak uygulama da kullanıcı profilleri, güvenlik, kullanım desenleri vb toplamak yönetmek için çeşitli veritabanları kullanır. Tek tek kiracıların kullanılabilirliği, uygulamanın bir bütün olarak kullanılabilirliğini etkilemez. Ancak, yönetim veritabanlarının kullanılabilirliği ve performansı uygulamanın işlevi için önemlidir ve yönetim veritabanları çevrimdışıysa tüm uygulama çevrimdışıdır.

Bu makalede, maliyete duyarlı başlangıç uygulamalarından sıkı kullanılabilirlik gereksinimlerine sahip olanlara kadar çeşitli senaryoları kapsayan DR stratejileri tartışılmaktadır.

> [!NOTE]
> Premium veya İş Açısından Kritik veritabanları nı ve esnek havuzları kullanıyorsanız, bunları bölge gereksiz dağıtım yapılandırmasına dönüştürerek bunları bölgesel kesintilere karşı esnek hale getirebilirsiniz. Bkz. [Bölge yedekli veritabanları.](sql-database-high-availability.md)

## <a name="scenario-1-cost-sensitive-startup"></a>Senaryo 1. Maliyete duyarlı başlangıç

Ben bir başlangıç iş ve son derece maliyet duyarlı duyuyorum.  Uygulamanın dağıtımını ve yönetimini basitleştirmek istiyorum ve bireysel müşteriler için sınırlı bir SLA'ya sahip olabilirim. Ama bir bütün olarak uygulama çevrimdışı asla emin olmak istiyorum.

Basitlik gereksinimini karşılamak için, tüm kiracı veritabanlarını seçtiğiniz Azure bölgesinde tek bir elastik havuza dağıtın ve yönetim veritabanlarını coğrafi olarak çoğaltılmış tek veritabanları olarak dağıtın. Kiracıların olağanüstü durum kurtarma için, hiçbir ek ücret ödemeden gelen coğrafi geri yükleme kullanın. Yönetim veritabanlarının kullanılabilirliğini sağlamak için, otomatik hata grubu (adım 1) kullanarak bunları başka bir bölgeye coğrafi olarak kopyalayın. Bu senaryoda olağanüstü durum kurtarma yapılandırmasının devam eden maliyeti, ikincil veritabanlarının toplam maliyetine eşittir. Bu yapılandırma sonraki diyagramda gösterilmiştir.

![Şekil 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Birincil bölgede bir kesinti oluşursa, uygulamanızı çevrimiçi duruma getirmek için kurtarma adımları bir sonraki diyagramda gösterilmiştir.

* Failover grubu, yönetim veritabanının DR bölgesine otomatik olarak başarısız olmasını başlatır. Uygulama otomatik olarak yeni birincil ve tüm yeni hesaplar ve kiracı veritabanları DR bölgede oluşturulur yeniden bağlanır. Varolan müşteriler verilerinin geçici olarak kullanılamada olduğunu görür.
* Orijinal havuzla aynı yapılandırmaya sahip elastik havuzu oluşturun (2).
* Kiracı veritabanlarının kopyalarını oluşturmak için coğrafi geri yükleme'yi kullanın (3). Tek tek geri yüklemeleri son kullanıcı bağlantıları tarafından tetiklemeyi veya uygulamaya özgü başka bir öncelik düzeni kullanmayı düşünebilirsiniz.

Bu noktada uygulamanız DR bölgesinde yeniden çevrimiçi dir, ancak bazı müşteriler verilerine erişirken gecikme yaşar.

![Şekil 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Kesinti geçiciyse, tüm veritabanı geri yüklemeleri DR bölgesinde tamamlanmadan önce birincil bölgenin Azure tarafından kurtarılmış olması mümkündür. Bu durumda, uygulamayı birincil bölgeye geri taşımayı düzenleyin. İşlem, bir sonraki diyagramda gösterilen adımları alır.

* Bekleyen tüm coğrafi geri yükleme isteklerini iptal edin.
* Birincil bölgeye yönetim veritabanları üzerinde başarısız (5). Bölgenin iyileşmesinden sonra, eski ön seçimler otomatik olarak ikinci sıraya dönüştü. Şimdi tekrar rolleri değiştiriyorlar.
* Birincil bölgeye geri dönmek için uygulamanın bağlantı dizesini değiştirin. Şimdi tüm yeni hesaplar ve kiracı veritabanları birincil bölgede oluşturulur. Bazı varolan müşteriler verilerinin geçici olarak kullanılamada olduğunu görür.
* DR havuzundaki tüm veritabanlarını, DR bölgesinde değiştirilemediğinden emin olmak için salt okunur şekilde ayarlayın (6).
* Kurtarma, yeniden adlandırma veya birincil havuzda (7) ilgili veritabanları nı silmek bu yana değişti DR havuzunda her veritabanı için.
* Güncelleştirilmiş veritabanlarını DR havuzundan birincil havuza kopyalayın (8).
* DR havuzunu silme (9)

Bu noktada uygulamanız birincil bölgede çevrimiçi dir ve birincil havuzda bulunan tüm kiracı veritabanları vardır.

![Şekil 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Bu stratejinin en önemli **yararı,** veri katmanı artıklığı için devam eden düşük maliyettir. Yedeklemeler, uygulamayeniden yazmadan ve ek bir ücret ödemeden SQL Veritabanı hizmeti tarafından otomatik olarak alınır.  Maliyet, yalnızca elastik veritabanları geri yüklendiğinde tahakkuk eder. **Takas,** tüm kiracı veritabanlarının tamamen kurtarılmasının önemli ölçüde zaman almasıdır. Süre, DR bölgesinde başlattığınız toplam geri yükleme sayısına ve kiracı veritabanlarının genel boyutuna bağlıdır. Bazı kiracıların geri yüklemelerine diğerlerine göre öncelik verseniz bile, varolan müşterilerin veritabanları üzerindeki genel etkiyi en aza indirmek için hizmet tahkim ve azaltmayla aynı bölgede başlatılan diğer tüm geri yüklemelerle rekabet emiş oluyorsunuz. Ayrıca, kiracı veritabanlarının kurtarılması, DR bölgesindeki yeni elastik havuz oluşturulana kadar başlayamaz.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Senaryo 2. Katmanlı hizmet ile olgun uygulama

Ben katmanlı hizmet teklifleri ve deneme müşterileri için farklı SLA'lar ve ödeme müşteriler için olgun bir SaaS uygulaması duyuyorum. Deneme müşterileri için, maliyeti mümkün olduğunca azaltmam gerekiyor. Deneme müşterileri kapalı kalma süresini alabilir ama ben bunun olasılığını azaltmak istiyorum. Ödeme yapan müşteriler için, herhangi bir kapalı kalma süresi bir uçuş riskidir. Bu yüzden, ödeme yapan müşterilerin verilerine her zaman erişebilmelerini sağlamak istiyorum.

Bu senaryoyu desteklemek için, deneme kiracılarını ayrı elastik havuzlara koyarak ücretli kiracılardan ayırın. Deneme müşterilerinin kiracı başına daha düşük eDTU veya vCores ve daha uzun bir kurtarma süresi ile daha düşük SLA var. Ödeme yapan müşteriler, kiracı başına daha yüksek eDTU veya vCore'lara ve daha yüksek bir SLA'ya sahip bir havuzdabulunmaktadır. En düşük kurtarma süresini garanti etmek için, ödeme yapan müşterilerin kiracı veritabanları coğrafi olarak çoğaltılır. Bu yapılandırma sonraki diyagramda gösterilmiştir.

![Şekil 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

İlk senaryoda olduğu gibi, yönetim veritabanları oldukça etkinolduğundan, bunun için tek bir coğrafi olarak çoğaltılmış veritabanı kullanırsınız (1). Bu, yeni müşteri abonelikleri, profil güncelleştirmeleri ve diğer yönetim işlemleri için öngörülebilir performansı sağlar. Yönetim veritabanlarının ön seçimlerinin bulunduğu bölge birincil bölge ve yönetim veritabanlarının ikincilerinin bulunduğu bölge DR bölgesidir.

Ödeme yapan müşterilerin kiracı veritabanları, birincil bölgede sağlanan "ücretli" havuzda etkin veritabanlarına sahiptir. DR bölgesinde aynı ada sahip ikincil bir havuz sağlama. Her kiracı ikincil havuza coğrafi olarak çoğaltılır (2). Bu, failover kullanarak tüm kiracı veritabanlarının hızlı bir şekilde kurtarılmasını sağlar.

Birincil bölgede bir kesinti oluşursa, uygulamanızı çevrimiçi duruma getirmek için kurtarma adımları sonraki diyagramda gösterilmiştir:

![Şekil 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

* Hemen DR bölgesine yönetim veritabanları üzerinde başarısız (3).
* Uygulamanın bağlantı dizesini DR bölgesini işaret etmek için değiştirin. Artık tüm yeni hesaplar ve kiracı veritabanları DR bölgesinde oluşturulur. Varolan deneme müşterileri verilerinin geçici olarak kullanılamada olduğunu görür.
* Dr bölgesinde havuza ödenen kiracı veritabanları üzerinde hemen kullanılabilirlik (4) geri başarısız. Failover hızlı bir meta veri düzeyi değişikliği olduğundan, tek tek failovers son kullanıcı bağlantıları tarafından talep üzerine tetiklenir bir optimizasyon düşünün.
* İkincil veritabanları yalnızca ikinci basamak iken değişiklik günlüklerini işleme kapasitesine ihtiyaç duyduğundan, ikincil havuz eDTU boyutunuz veya vCore değeriniz birincilden daha düşükse, tam iş yükünü karşılamak için havuz kapasitesini hemen şimdi artırın tüm kiracıların (5).
* Deneme müşterilerinin veritabanları için DR bölgesinde aynı ada ve aynı yapılandırmaya sahip yeni elastik havuzu oluşturun (6).
* Deneme müşterileri havuzu oluşturulduktan sonra, tek tek deneme kiracı veritabanlarını yeni havuza geri yüklemek için coğrafi geri yüklemeyi kullanın (7). Tek tek geri yüklemeleri son kullanıcı bağlantıları tarafından tetiklemeyi düşünün veya uygulamaya özgü başka bir öncelik düzeni kullanın.

Bu noktada uygulamanız DR bölgesinde tekrar çevrimiçi dir. Tüm ödeme yapan müşteriler verilerine erişebilirken, deneme müşterileri verilerine erişirken gecikme yaşarlar.

Birincil bölge, uygulamayı DR bölgesinde geri yükledikten *sonra* Azure tarafından kurtarıldığında, uygulamayı o bölgede çalıştırmaya devam edebilir veya birincil bölgeye geri dönmeye karar verebilirsiniz. Başarısızlık işlemi tamamlanmadan *önce* birincil bölge kurtarılırsa, hemen başarısız olmayı düşünün. Failback sonraki diyagramda gösterilen adımları alır:

![Şekil 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

* Bekleyen tüm coğrafi geri yükleme isteklerini iptal edin.
* Yönetim veritabanları üzerinde başarısız (8). Bölgenin iyileşmesinden sonra, eski birincil otomatik olarak ikincil olur. Şimdi tekrar birincil olur.  
* Ücretli kiracı veritabanları üzerinde başarısız (9). Benzer şekilde, bölgenin iyileşmesinden sonra, eski ön seçimler otomatik olarak ikinci ler haline gelir. Şimdi tekrar ön seçim oldular.
* DR bölgesinde değiştirilen geri yüklenen deneme veritabanlarını salt okunur olarak ayarlayın (10).
* Kurtarma, yeniden adlandırma veya deneme müşterileri birincil havuzunda ilgili veritabanını silmek (11) bu yana değişti deneme müşterileri DR havuzunda her veritabanı için.
* Güncelleştirilmiş veritabanlarını DR havuzundan birincil havuza kopyalayın (12).
* DR havuzunu silin (13).

> [!NOTE]
> Başarısız işlem asynchronous olduğunu. Kurtarma süresini en aza indirmek için, kiracı veritabanlarının başarısız komutunu en az 20 veritabanlarından oluşan toplu olarak yürütmeniz önemlidir.

Bu stratejinin en önemli **yararı,** ödeme yapan müşteriler için en yüksek SLA'yı sağlamasıdır. Ayrıca, deneme DR havuzu oluşturulur oluşturulmaz yeni denemelerin engelinin kaldırıldığını da garanti eder. **Takas,** bu kurulumun ücretli müşteriler için ikincil DR havuzunun maliyetine göre kiracı veritabanlarının toplam maliyetini artırmasıdır. Buna ek olarak, ikincil havuz farklı bir boyuta sahipse, ödeme yapan müşteriler DR bölgesindeki havuz yükseltmesi tamamlanana kadar başarısız olduktan sonra daha düşük performans la karşılaşır.

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Senaryo 3. Katmanlı hizmet ile coğrafi olarak dağıtılmış uygulama

Katmanlı hizmet teklifleri ile olgun bir SaaS uygulama var. Ücretli müşterilerime çok agresif bir SLA sunmak ve kesintiler meydana geldiğinde etki riskini en aza indirmek istiyorum, çünkü kısa süreli kesintiler bile müşteri memnuniyetsizliğine neden olabilir. Ödeme yapan müşterilerin verilerine her zaman erişebiliyor olması çok önemlidir. Denemeler ücretsizdir ve deneme süresi boyunca SLA sunulmaz.

Bu senaryoyu desteklemek için üç ayrı elastik havuz kullanın. Ücretli müşterilerin kiracı veritabanlarını içermek için iki farklı bölgede veritabanı başına yüksek eDTUs veya vCore içeren iki eşit boyutlu havuz sağlama. Deneme kiracılarını içeren üçüncü havuz, veritabanı başına daha düşük eDT'lere veya vCore'lara sahip olabilir ve iki bölgeden birinde kullanılabilir.

Kesintiler sırasında en düşük kurtarma süresini garanti etmek için, ödeme yapan müşterilerin kiracı veritabanları, iki bölgenin her birinde birincil veritabanlarının %50'si ile coğrafi olarak çoğaltılır. Benzer şekilde, her bölge ikincil veritabanlarının %50'si vardır. Bu şekilde, bir bölge çevrimdışıysa, ücretli müşterilerin veritabanlarının yalnızca %50'si etkilenir ve başarısız olmak zorunda kalır. Diğer veritabanları bozulmadan kalır. Bu yapılandırma aşağıdaki diyagramda gösterilmiştir:

![Şekil 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Önceki senaryolarda olduğu gibi, yönetim veritabanları oldukça etkindir, bu nedenle bunları tek bir coğrafi olarak çoğaltılmış veritabanları olarak yapılandırın (1). Bu, yeni müşteri aboneliklerinin, profil güncelleştirmelerinin ve diğer yönetim işlemlerinin öngörülebilir performansını sağlar. Bölge A, yönetim veritabanlarının birincil bölgesidir ve B bölgesi yönetim veritabanlarının kurtarılması için kullanılır.

Ödeme yapan müşterilerin kiracı veritabanları da coğrafi olarak çoğaltılır, ancak a bölgesi ile B bölgesi (2) arasında ayrılan öncelikler ve ikincilikler ile birlikte. Bu şekilde, kesintiden etkilenen kiracı birincil veritabanları diğer bölgeye başarısız olabilir ve kullanılabilir hale gelebilir. Kiracı veritabanlarının diğer yarısı hiç etkilenmez.

Sonraki diyagram, A bölgesinde bir kesinti oluşursa yapılacak kurtarma adımlarını gösterir.

![Şekil 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

* B (3) bölgesine yönetim veritabanları üzerinde hemen başarısız olur.
* Uygulamanın bağlantı dizesini, B bölgesindeki yönetim veritabanlarını işaret etmek için değiştirin. Yeni hesapların ve kiracı veritabanlarının B bölgesinde oluşturulduğundan ve varolan kiracı veritabanlarının da bulunduğundan emin olmak için yönetim veritabanlarını değiştirin. Varolan deneme müşterileri verilerinin geçici olarak kullanılamada olduğunu görür.
* Ücretli kiracının veritabanları üzerinde hemen kullanılabilirliklerini geri yüklemek için B bölgesinde 2 havuz alamamak (4). Failover hızlı bir meta veri düzeyi değişikliği olduğundan, tek tek failovers son kullanıcı bağlantıları tarafından talep üzerine tetiklenir bir optimizasyon düşünebilirsiniz.
* Havuz 2 yalnızca birincil veritabanları içerdiğinden, havuzdaki toplam iş yükü artar ve eDTU boyutunu (5) veya vCore sayısını hemen artırabilir.
* Deneme müşterilerinin veritabanları için B bölgesinde aynı ada ve aynı yapılandırmaya sahip yeni elastik havuzu oluşturun (6).
* Havuz oluşturulduktan sonra, tek tek deneme kiracı veritabanını havuza geri yüklemek için coğrafi geri yüklemeyi kullanın (7). Tek tek geri yüklemeleri son kullanıcı bağlantıları tarafından tetiklemeyi veya uygulamaya özgü başka bir öncelik düzeni kullanmayı düşünebilirsiniz.

> [!NOTE]
> Başarısız işlem asynchronous olduğunu. Kurtarma süresini en aza indirmek için, kiracı veritabanlarının başarısız komutunu en az 20 veritabanlarından oluşan toplu olarak yürütmeniz önemlidir.

Bu noktada başvurunuz B bölgesinde tekrar çevrimiçi dir. Tüm ödeme yapan müşteriler verilerine erişebilirken, deneme müşterileri verilerine erişirken gecikme yaşarlar.

Bölge A kurtarıldığında, deneme müşterileri için B bölgesini kullanmak mı yoksa A bölgesindeki deneme müşterileri havuzunu kullanmakta başarısız olmak mı istediğinize karar vermeniz gerekir. Bir ölçüt, kurtarma dan bu yana değiştirilen deneme kiracı veritabanlarının % olabilir. Bu kararne bakılmaksızın, ödenen kiracıları iki havuz arasında yeniden dengelemeniz gerekir. sonraki diyagram, deneme kiracı veritabanlarıa A bölgesine geri başarısız olduğunda işlemi göstermektedir.  

![Şekil 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

* Deneme DR havuzuna bekleyen tüm coğrafi geri yükleme isteklerini iptal edin.
* Yönetim veritabanı (8) üzerinde başarısız. Bölgenin iyileşmesinden sonra, eski birincil otomatik olarak ikincil oldu. Şimdi tekrar birincil olur.  
* Ücretli kiracı veritabanlarının havuz 1'e geri dönmediğini seçin ve ikincillerine başarısız olunmasını başlatın (9). Bölgenin kurtarılmasından sonra, havuz 1'deki tüm veritabanları otomatik olarak ikinci oldu. Şimdi bunların %50'si tekrar ön seçim oluyor.
* Havuz 2'nin boyutunu orijinal eDTU'ya (10) veya vCore sayısına küçültün.
* B bölgesindeki tüm geri yüklenen deneme veritabanlarını salt okunur (11) olarak ayarlayın.
* Kurtarma, yeniden adlandırma veya deneme birincil havuzu (12) ilgili veritabanı nı silmek bu yana değişti deneme DR havuzunda her veritabanı için.
* Güncelleştirilmiş veritabanlarını DR havuzundan birincil havuza kopyalayın (13).
* DR havuzunu silin (14).

Bu stratejinin temel **yararları** şunlardır:

* Bir kesintinin kiracı veritabanlarının %50'sinden fazlasını etkilememesini sağladığından, ödeme yapan müşteriler için en agresif SLA'yı destekler.
* Kurtarma sırasında dr havuzu oluşturulur oluşturulmaz yeni denemelerin engelinin kaldırıldığını garanti eder.
* Havuz 1 ve havuz 2'deki ikincil veritabanlarının %50'sinin birincil veritabanlarından daha az aktif olması garanti olduğundan, havuz kapasitesinin daha verimli kullanılmasını sağlar.

Ana **dengeler** şunlardır:

* Yönetim veritabanlarına karşı CRUD işlemleri, yönetim veritabanlarının birincil karşı yürütülür gibi bölge B bağlı son kullanıcılar için daha fazla gecikme sonu vardır.
* Bu yönetim veritabanının daha karmaşık tasarım gerektirir. Örneğin, her kiracı kaydının, başarısız olma ve geri dönüş sırasında değiştirilmesi gereken bir konum etiketi vardır.  
* Ödeme yapan müşteriler, B bölgesindeki havuz yükseltmesi tamamlanana kadar normalden daha düşük performans la karşılaşabilirler.

## <a name="summary"></a>Özet

Bu makalede, bir SaaS ISV çok kiracı uygulaması tarafından kullanılan veritabanı katmanı için olağanüstü durum kurtarma stratejileri üzerinde duruluyor. Seçtiğiniz strateji, iş modeli, müşterilerinize sunmak istediğiniz SLA, bütçe kısıtlaması vb. gibi uygulamanın ihtiyaçlarına bağlıdır. Açıklanan her strateji, bilinçli bir karar verebilmeniz için yararları ve değiş tokuşları özetler. Ayrıca, özel uygulamanız büyük olasılıkla diğer Azure bileşenlerini içerir. Bu nedenle, iş sürekliliği kılavuzlarını gözden geçirip veritabanı katmanının kurtarılmasını onlarla birlikte düzenleyebilirsiniz. Azure'da veritabanı uygulamalarının kurtarılmasını yönetme hakkında daha fazla bilgi edinmek [için, olağanüstü durum kurtarma için bulut çözümleri tasarlama bölümüne](sql-database-designing-cloud-solutions-for-disaster-recovery.md)bakın.  

## <a name="next-steps"></a>Sonraki adımlar

* Azure SQL Veritabanı otomatik yedeklemeleri hakkında bilgi edinmek için [SQL Veritabanı otomatik yedeklemelerine](sql-database-automated-backups.md)bakın.
* İş sürekliliğine genel bakış ve senaryolar için [İş sürekliliğine genel bakış](sql-database-business-continuity.md)alabakın.
* Kurtarma için otomatik yedeklemekullanma hakkında bilgi edinmek [için, hizmet tarafından başlatılan yedeklemelerden bir veritabanıgeri yükleme'ye](sql-database-recovery-using-backups.md)bakın.
* Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) ve Otomatik başarısız [gruplarına](sql-database-auto-failover-group.md)bakın.
* Arşivleme için otomatik yedekleme kullanma hakkında bilgi edinmek için [veritabanı kopyasına](sql-database-copy.md)bakın.
