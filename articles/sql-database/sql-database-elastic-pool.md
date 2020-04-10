---
title: Elastik havuzlarla birden çok veritabanını yönetme
description: Elastik havuzları kullanarak birden çok SQL veritabanını -yüzlerce ve binlerce- yönetin ve ölçeklendirin. Gerektiğinde dağıtabileceğiniz kaynaklar için bir fiyat.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: ninarn, carlrab
ms.date: 04/09/2020
ms.openlocfilehash: 3252ecb030234e4c5543c07dfb4fc702f850a73e
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998984"
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-azure-sql-databases"></a>Elastik havuzlar birden fazla Azure SQL veritabanını yönetmenize ve ölçeklendirmenize yardımcı olur

SQL Veritabanı elastik havuzları, kullanım talepleri değişken olan ve öngörülmeyen birden çok veritabanını yönetmeye ve ölçeklendirmeye yönelik kolay ve ekonomik bir çözümdür. Elastik havuz içindeki veritabanları tek bir Azure SQL Veritabanı sunucusunda bulunur ve sabit fiyatla belirli bir kaynak kümesini paylaşır. Azure SQL Veritabanındaki elastik havuzlar, SaaS geliştiricilerinin bir veritabanı grubuna ait fiyat performansını belirtilen bütçe dahilinde iyileştirmesini ve aynı zamanda her veritabanı için performans Elastikliği sunmasını sağlar.

## <a name="what-are-sql-elastic-pools"></a>SQL elastik havuzları nelerdir

SaaS geliştiricileri, birden fazla veritabanından oluşan büyük ölçekli veri katmanlarının üzerinde uygulamalar oluşturur. Her müşteri için tek veritabanı sağlanması yaygın bir uygulama modelidir. Ancak farklı müşterilerin genellikle farklı ve öngörülemeyen kullanım şekilleri vardır ve her bir veritabanı kullanıcısının kaynak gereksinimlerini tahmin etmek zordur. Geleneksel olarak, iki seçeneğiniz vardı:

- En yüksek kullanıma ve fazla ödemeye dayalı aşırı tedarik kaynakları veya
- Zirvelerde performans ve müşteri memnuniyeti pahasına, maliyet tasarrufu için under-provision.

Elastik havuzlar, veritabanlarının gereksinim duydukları performans kaynaklarını ihtiyaç duydukları nda almalarını sağlayarak bu sorunu çözer. Bunlar, tahmin edilebilir bir bütçe içinde basit bir kaynak ayırma mekanizması sağlar. Elastik havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

> [!IMPORTANT]
> Elastik havuzlar için veritabanı başına ücret alınmaz. Kullanımdan veya havuzun bir saatten kısa bir süre için etkin olup olmadığına bakılmaksızın, en yüksek eDTU veya vCores'ta bulunan her saat için faturalandırılırsınız.

Esnek havuzlar, geliştiricinin tek tek veritabanları tarafından öngörülemeyen kullanım sürelerini karşılamak için birden çok veritabanları tarafından paylaşılan bir havuz için kaynak satın almasına olanak tanır. Havuz için kaynakları [DTU tabanlı satın alma modeline](sql-database-service-tiers-dtu.md) veya [vCore tabanlı satın alma modeline](sql-database-service-tiers-vcore.md)göre yapılandırabilirsiniz. Bir havuz için kaynak gereksinimi, veritabanlarının toplu kullanımı yla belirlenir. Havuziçin kullanılabilir kaynak miktarı geliştirici bütçesi tarafından denetlenir. Geliştirici sadece havuza veritabanları ekler, veritabanları için minimum ve maksimum kaynakları ayarlar (ya minimum ve maksimum DTUs veya minimum veya maksimum vCores kaynak modeli seçtiğiniz bağlı olarak) ve sonra bütçelerine göre havuz kaynaklarını ayarlar. Geliştirici, hizmetini zayıf bir başlangıçtan sürekli artan ölçekte olgun bir işletmeye sorunsuzca büyütmek için havuzları kullanabilir.

Havuz içerisinde tek tek veritabanlarına belirli parametreler içinde otomatik olarak ölçeklendirme esnekliği tanınır. Ağır yük altında, bir veritabanı talebi karşılamak için daha fazla kaynak tüketebilir. Hafif yükler altındaki veritabanları daha az, yük altında olmayan veritabanları ise kaynak tüketmez. Tek tek veritabanları yerine tüm havuz için kaynak sağlamak, yönetim görevlerinizi basitleştirir. Ayrıca, havuz için öngörülebilir bir bütçeniz var. Veritabanlarının yeni eDTU rezervasyonu için ek bilgi işlem kaynakları sağlamak için taşınması gerekebileceği dışında, veritabanı kapalı kalma süresi olmayan varolan bir havuza ek kaynaklar eklenebilir. Benzer şekilde, ek kaynaklara artık ihtiyaç yoksa, herhangi bir zamanda varolan bir havuzdan kaldırılabilirler. Ayrıca havuza veritabanları ekleyebilir veya havuzdan veritabanları kaldırabilirsiniz. Bir veritabanı kaynakları tahmin edilebilir bir şekilde normalden az kullanıyorsa bu veritabanını havuzdan çıkarın.

> [!NOTE]
> Veritabanlarını elastik bir havuza veya elastik havuza movingrirken, veritabanı bağlantıları bırakıldığında işlemin sonunda (saniye sırasına göre) kısa bir süre dışında kesinti yoktur.

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>SQL Veritabanı elastik havuzune ne zaman dikkat edinmelisiniz?

Havuzlar, belirli kullanım düzenlerine sahip çok sayıda veritabanı bulunan durumlar için çok uygundur. Söz konusu kullanım düzeni, belirli bir veritabanı için ortalama düşük düzeyde kullanım ile nispeten nadir zamanlarda kullanımın ani olarak artması şeklindedir.

Bir havuza ekleyebileceğiniz veritabanı sayısı arttıkça, tasarruflarınız artar. Uygulama kullanım deseninize bağlı olarak, iki S3 veritabanına kadar az tasarruf görmek mümkündür.

Aşağıdaki bölümler veritabanı koleksiyonunuzun bir havuzda olmasının yararlarını nasıl değerlendireceğini anlamanıza yardımcı olabilir. Örneklerde Standart havuzlar kullanılmaktadır, ancak aynı ilkeler Temel ve Premium havuzlar için de geçerlidir.

### <a name="assessing-database-utilization-patterns"></a>Veritabanı kullanım modellerini değerlendirme

Aşağıdaki şekilde zamanın büyük bölümünü boşta geçiren, ancak düzenli olarak ani etkinlikler sergileyen bir veritabanı örneğini göstermektedir. Bu model bir havuz için uygun olan kullanım modelidir:

   ![havuz için uygun bir tek veritabanı](./media/sql-database-elastic-pool/one-database.png)

Gösterilen beş dakikalık süre boyunca Veritabanı1, 90 DTU’ya kadar yükselir, ancak genel ortalama kullanım beş DTU’dan azdır. Bu iş yükünü tek bir veritabanında çalıştırmak için Bir S3 işlem boyutu gereklidir, ancak bu, kaynakların çoğunu düşük etkinlik dönemlerinde kullanılmaz bırakır.

Havuz bu kullanılmayan DTU’ların birden fazla veritabanında paylaşılmasına olanak tanır ve böylece gereken DTU ile genel maliyeti azaltır.

Önceki örnekten devam ederek, Veritabanı1 ile benzer kullanım modellerine sahip ek veritabanları olduğunu varsayalım. Aşağıdaki sonraki iki rakamda, dtu tabanlı satın alma modelini kullanarak zaman içinde kullanımlarının örtüşmeyen doğasını göstermek için dört veritabanları ve 20 veritabanının kullanımı aynı grafik üzerine katmanlanmıştır:

   ![bir havuz için uygun kullanım modeli ile dört veritabanı](./media/sql-database-elastic-pool/four-databases.png)

   ![bir havuz için uygun kullanım modeli ile yirmi veritabanı](./media/sql-database-elastic-pool/twenty-databases.png)

20 veritabanının tamamındaki toplam DTU kullanımı, önceki şekilde siyah çizgi ile gösterilmiştir. Bu şekil, toplam DTU kullanımının 100 DTU’yu hiçbir zaman aşmadığını ve 20 veritabanının bu süre boyunca 100 eDTU’yu paylaşabileceğini gösterir. Bu dus bir 20x azalma ve 13x fiyat indirimi tek veritabanları için S3 işlem boyutlarında her veritabanları yerleştirerek karşılaştırıldığında sonuçlanır.

Bu örnek aşağıdaki nedenlerle idealdir:

- Bir veritabanındaki en yüksek kullanım ile ortalama kullanım arasında büyük farklar mevcuttur.
- Bir veritabanının en yüksek kullanımı zamanın farklı noktalarında gerçekleşir.
- eDTU'lar birden fazla veritabanı arasında paylaşılır.

Bir havuzun fiyatı, havuz eDTU'larının bir işlevidir. Bir havuzun eDTU birim fiyatı, tek veritabanının DTU birim fiyatından 1,5 kat fazladır. Bununla birlikte **havuz eDTU'ları çok sayıda veritabanı tarafından paylaşılabilir ve toplam eDTU sayısı gereklidir**. Fiyatlandırma ve eDTU paylaşımındaki bu farklılıklar, havuzların sağlayabileceği tasarruf potansiyelinin temelini oluşturur.

Veritabanı sayısı ve veritabanı kullanımı ile ilgili aşağıdaki başparmak kuralları, bir havuzun tek veritabanları için işlem boyutları kullanmaya kıyasla daha düşük maliyet sağladığından emin olunmaya yardımcı olur.

### <a name="minimum-number-of-databases"></a>En az veritabanı sayısı

Tek veritabanları için toplam kaynak miktarı havuz için gereken kaynakların 1,5 katından fazlaysa, elastik bir havuz daha uygun maliyetlidir.

***DTU tabanlı satın alma modeli örneği***<br>
100 eDTU havuzunun tek veritabanları için işlem boyutları kullanmaktan daha uygun maliyetli olması için en az iki S3 veritabanı veya en az 15 S0 veritabanı gereklidir.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Eşzamanlı olarak en üst seviyeye çıkan en fazla veritabanı sayısı

Kaynakları paylaşarak, havuzdaki tüm veritabanları aynı anda tek veritabanları için kullanılabilir sınıra kadar kaynakları kullanamaz. Aynı anda en yüksek veritabanı sayısı ne kadar azolursa, havuz kaynakları o kadar düşük olarak ayarlanabilir ve havuz o kadar uygun maliyetli hale gelir. Genel olarak, en fazla 2/3 (veya% 67) havuzdaki veritabanlarının aynı anda kendi kaynakları sınırına zirve gerekir.

***DTU tabanlı satın alma modeli örneği***

200 eDTU içeren bir havuzdaki üç S3 veritabanının maliyetlerini azaltmak için, bu veritabanlarının en fazla iki tanesi kullanım sırasında en üst seviyeye çıkabilir. Aksi takdirde, bu dört S3 veritabanının ikiden fazlası eşzamanlı olarak en üst seviyeye çıkarsa, havuzun boyutu 200 eDTU’dan fazla olmak zorundadır. Havuz 200'den fazla eDTUs'a yeniden boyutlandırılırsa, maliyetleri tek veritabanlarıiçin işlem boyutlarından daha düşük tutmak için havuza daha fazla S3 veritabanı eklenmesi gerekir.

Bu örnek, havuzdaki diğer veritabanlarının kullanımını dikkate almaz. Herhangi bir zamanda tüm veritabanlarının kullanımı aynı olursa, veritabanlarının 2/3’ünden (veya %67) daha azı eşzamanlı olarak en üst seviyeye çıkabilir.

### <a name="resource-utilization-per-database"></a>Veritabanı başına kaynak kullanımı

Bir veritabanının en yüksek ile ortalama kullanımı arasında büyük bir fark olması, uzun süreli düşük kullanımı ve kısa süreli yüksek kullanımı ifade eder. Bu kullanım modeli, veritabanları arasında kaynakların paylaşılması için idealdir. Bir veritabanının en yüksek kullanımı ortalama kullanımından 1,5 kat fazla olduğunda, veritabanı havuz için düşünülmelidir.

**DTU tabanlı satın alma modeli örneği**: 100 DTU'ya ulaşan ve ortalama 67 DTU veya daha az kullanan bir S3 veritabanı, bir havuzda eDUs paylaşımı için iyi bir adaydır. Alternatif olarak, en yüksek kullanımı 20 DTU’ya varan ve ortalama olarak en fazla 13 DTU kullanan bir S1 veritabanı da havuz için iyi bir adaydır.

## <a name="how-do-i-choose-the-correct-pool-size"></a>Doğru havuz boyutunu nasıl seçerim?

Havuz için en iyi boyut, havuzdaki tüm veritabanları için gereken toplu kaynaklara bağlıdır. Bu, aşağıdakileri belirlemeyi içerir:

- Havuzdaki tüm veritabanları tarafından kullanılan maksimum kaynaklar (kaynak modeli seçiminize bağlı olarak maksimum DTUs veya maksimum vCores).
- Havuzdaki tüm veritabanları tarafından kullanılan en fazla depolama baytı sayısı.

Her kaynak modeli için kullanılabilir hizmet katmanları ve sınırları için [DTU tabanlı satın alma modeline](sql-database-service-tiers-dtu.md) veya [vCore tabanlı satın alma modeline](sql-database-service-tiers-vcore.md)bakın.

Aşağıdaki adımlar, bir havuzun tek veritabanlarından daha uygun maliyetli olup olmadığını tahmin edebilirsiniz:

1. Havuz için gereken eDT'leri veya vCore'ları aşağıdaki gibi tahmin edin:

   DTU tabanlı satın alma modeli için: MAX(<DB başına *ortalama DBs* X *ortalama DTU kullanımı* sayısı>,<br>  
   <*Eşzamanlı olarak en üst seviyeye çıkan veritabanı sayısı* X *Veritabanı başına en yüksek DTU kullanımı*)

   vCore tabanlı satın alma modeli için: MAX(<DB başına *Toplam DBs* X *ortalama vCore kullanımı*>,<br>  
   <*Aynı anda tepeleyen DB sayısı* DB başına X *Peak vCore kullanımı*)

2. Havuzdaki tüm veritabanları için gereken bayt sayısını ekleyerek havuz için gereken depolama alanını tahmin edin. Ardından, bu depolama miktarını sağlayan eDTU havuz boyutunu belirleyin.
3. DTU tabanlı satın alma modeli için, adım 1 ve adım 2 eDTU tahminlerinin daha büyük almak. vCore tabanlı satın alma modeli için, Adım 1 vCore tahmini atın.
4. SQL [Veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/) bakın ve Adım 3'teki tahminden daha büyük en küçük havuz boyutunu bulun.
5. Adım 5'teki havuz fiyatını, tek veritabanları için uygun işlem boyutlarını kullanma fiyatıyla karşılaştırın.

> [!IMPORTANT]
> Bir havuzdaki veritabanlarının sayısı desteklenen maksimuma yaklaşıyorsa, [yoğun elastik havuzlarda Kaynak yönetimini](sql-database-elastic-pool-resource-management.md)dikkate almayı unutmayın.
> 

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Elastik havuzlu diğer SQL Veritabanı özelliklerini kullanma

### <a name="elastic-jobs-and-elastic-pools"></a>Elastik işler ve elastik havuzlar

Bir havuz kullanılarak **[esnek işlerde](elastic-jobs-overview.md)** betik çalıştırma yoluyla yönetim görevleri kolaylaştırılır. Elastik iş, çok sayıda veritabanından kaynaklanan sorunların çoğunu ortadan kaldırır.

Birden fazla veritabanıyla çalışmak için kullanılabilen diğer veritabanı araçları hakkında daha fazla bilgi için bkz. [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md).

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Elastik havuzdaki veritabanları için iş sürekliliği seçenekleri

Havuza alınan veritabanları genellikle tek veritabanları için kullanılabilen [iş sürekliliği özelliklerinin](sql-database-business-continuity.md) aynılarını destekler.

- **Belirli bir noktaya geri yükleme**

  Zamanında geri yükleme, havuzdaki bir veritabanını belirli bir noktaya kurtarmak için otomatik veritabanı yedeklemelerini kullanır. Bkz. [Belirli Bir Noktaya Geri Yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)

- **Coğrafi Geri Yükleme**

  Coğrafi geri yükleme, veritabanının barındırıldığı bölgede meydana gelen bir olay nedeniyle veritabanı kullanılamadığında varsayılan kurtarma seçeneğini sağlar. Bkz. [Bir Azure SQL Veritabanını geri yükleme veya ikincil veritabanına yük devretme](sql-database-disaster-recovery.md)

- **Etkin coğrafi çoğaltma**

  Coğrafi geri yüklemenin sunabileceğinden daha agresif kurtarma gereksinimleriolan uygulamalar [için, Etkin coğrafi çoğaltmayı](sql-database-active-geo-replication.md) veya [otomatik arıza grubunu](sql-database-auto-failover-group.md)yapılandırın.

## <a name="creating-a-new-sql-database-elastic-pool-using-the-azure-portal"></a>Azure portalını kullanarak yeni bir SQL Veritabanı elastik havuzu oluşturma

Azure portalında esnek bir havuz oluşturmanın iki yolu vardır.

1. Esnek bir havuz oluşturmak için [Azure portalına](https://portal.azure.com) gidin. **Azure SQL'i**arayın ve seçin.
2. SELECT SQL dağıtım **seçeneği** sayfasını açmak için **+Ekle'yi** seçin. **Veritabanları** döşemesindeki **ayrıntıları göster'i** seçerek esnek havuzlar hakkında ek bilgileri görüntüleyebilirsiniz.
3. **Veritabanları** döşemesinde, **Kaynak türü** açılır düşüşünde **Elastik havuzu** seçin ve ardından **Oluştur'u**seçin:

   ![Elastik havuz oluşturma](./media/sql-database-elastic-pool/create-elastic-pool.png)


1. Veya varolan bir Azure SQL sunucusuna gidip **+ Yeni havuzu** tıklatarak doğrudan bu sunucuya bir havuz oluşturabilirsiniz.

> [!NOTE]
> Sunucuda birden çok havuz oluşturabilirsiniz, ancak farklı sunuculardan aynı havuza veritabanları ekemezsiniz.

Havuzun servis katmanı, havuzdaki elastik özellikler ve her veritabanı için kullanılabilen maksimum kaynak miktarını belirler. Ayrıntılar için, [DTU modelindeki](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes)elastik havuzlar için Kaynak sınırlarına bakın. Elastik havuzlar için vCore tabanlı kaynak sınırları için [vCore tabanlı kaynak limitlerine bakın - elastik havuzlar.](sql-database-vcore-resource-limits-elastic-pools.md)

Havuzun kaynaklarını ve fiyatlandırmasını yapılandırmak için **havuzu yapılandır'ı**tıklatın. Ardından bir hizmet katmanı seçin, havuza veritabanları ekleyin ve havuz ve veritabanları için kaynak sınırlarını yapılandırın.

Havuzu yapılandırmayı tamamladığınızda, 'Uygula'yı tıklatabilir, havuzu adlandırabilir ve havuzu oluşturmak için 'Tamam'ı tıklatabilirsiniz.

## <a name="monitor-an-elastic-pool-and-its-databases"></a>Elastik havuzu ve veritabanlarını izleme

Azure portalında, elastik bir havuzun ve bu havuziçindeki veritabanlarının kullanımını izleyebilirsiniz. Ayrıca, elastik havuzunuzda bir dizi değişiklik yapabilir ve tüm değişiklikleri aynı anda gönderebilirsiniz. Bu değişiklikler arasında veritabanları ekleme veya kaldırma, elastik havuz ayarlarınızı değiştirme veya veritabanı ayarlarınızı değiştirme yer almaktadır.

Elastik havuzunuzu izlemeye başlamak için portalda elastik bir havuz bulun ve açın. Önce elastik havuzunuzun durumuna genel bir bakış sağlayan bir ekran görürsünüz. Buna aşağıdakiler dahildir:

- Elastik havuzun kaynak kullanımını gösteren grafiklerin izlenmesi
- Elastik havuz için varsa, en son uyarılar ve öneriler

Aşağıdaki grafik örnek bir elastik havuz gösterir:

![Havuz görünümü](./media/sql-database-elastic-pool-manage-portal/basic.png)

Havuz hakkında daha fazla bilgi istiyorsanız, bu genel bakışta mevcut bilgilerden herhangi birini tıklayabilirsiniz. **Kaynak kullanım** grafiğine tıkladığınızda, grafikte gösterilen ölçümleri ve zaman penceresini özelleştirebileceğiniz Azure İzleme görünümüne götürün. Kullanılabilir bildirimleri tıklattığınızda, bu uyarının veya önerinin tüm ayrıntılarını gösteren bir bıçak sizi götürür.

Havuzunuzun içindeki veritabanlarını izlemek isterseniz, soldaki kaynak menüsünün **İzleme** **bölümündeveritabanı kaynak kullanımını** tıklatabilirsiniz.

![Veritabanı kaynak kullanım sayfası](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

### <a name="to-customize-the-chart-display"></a>Grafik ekranını özelleştirmek için

CPU yüzdesi, veri IO yüzdesi ve kullanılan günlük IO yüzdesi gibi diğer ölçümleri görüntülemek için grafiği ve metrik sayfayı edinebilirsiniz.

Grafik **Edit** formunda, son iki hafta içinde herhangi bir 24 saatlik pencereyi seçmek için sabit bir zaman aralığı seçebilir veya **özel'i** tıklatabilir ve ardından izlenecek kaynakları seçebilirsiniz.

### <a name="to-select-databases-to-monitor"></a>İzlenecek veritabanlarını seçmek için

Varsayılan olarak, Veritabanı **Kaynak Kullanımı** bıçak grafik DTU veya CPU tarafından en iyi 5 veritabanları (hizmet katmanıbağlı olarak) gösterecektir. Soldaki onay kutuları aracılığıyla grafiğin altındaki listeden veritabanlarını seçip seçerek bu grafikteki veritabanlarını değiştirebilirsiniz.

Veritabanları performansınızın daha eksiksiz bir görünümünü elde etmek için bu veritabanı tablosunda yan yana görüntülemek için daha fazla ölçüm seçebilirsiniz.

Daha fazla bilgi için Azure [portalında SQL Veritabanı uyarıları oluşturma](sql-database-insights-alerts-portal.md)bilgisine bakın.

## <a name="customer-case-studies"></a>Müşteri örnek olay incelemeleri

- [SnelStart](https://azure.microsoft.com/resources/videos/azure-sql-database-case-study-snelstart/)

  SnelStart, ayda 1.000 yeni Azure SQL Veritabanı oranında iş hizmetlerini hızla genişletmek için Azure SQL Veritabanı ile esnek havuzlar kullandı.

- [Umbraco](https://azure.microsoft.com/resources/videos/azure-sql-database-case-study-umbraco/)

  Umbraco, buluttaki binlerce kiracıya hızlı bir şekilde hizmet sağlamak ve ölçeklendirmek için Azure SQL Veritabanı'na sahip esnek havuzlar kullanır.

- [Daxko/CSI](https://customers.microsoft.com/story/726277-csi-daxko-partner-professional-service-azure)    

   Daxko/CSI, geliştirme döngüsünü hızlandırmak ve müşteri hizmetlerini ve performansını artırmak için Azure SQL Veritabanı ile esnek havuzlar kullanır.    

## <a name="next-steps"></a>Sonraki adımlar

- Fiyatlandırma bilgileri için [Elastik havuz](https://azure.microsoft.com/pricing/details/sql-database/elastic)fiyatlandırması'na bakın.
- Elastik havuzları [ölçeklendirmek için elastik havuzları ölçeklendirme](sql-database-elastic-pool-scale.md) ve [elastik havuzu ölçekleme - örnek kodu](scripts/sql-database-monitor-and-scale-pool-powershell.md)
- Elastik havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).
- Elastik havuzları kullanan bir SaaS öğreticisi [için Wingtip SaaS uygulamasına giriş](sql-database-wtp-overview.md)ebak.'a bakın.
- Birçok veritabanına sahip elastik havuzlarda kaynak yönetimi hakkında bilgi edinmek için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md)bakın.
