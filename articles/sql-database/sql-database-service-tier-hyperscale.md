---
title: Azure SQL Veritabanı Hiper ölçekli genel bakış | Microsoft Dokümanlar
description: Bu makalede, Azure SQL Veritabanı'ndaki vCore tabanlı satın alma modelindeki Hyperscale hizmet katmanı açıklanır ve genel amaç ve iş açısından kritik hizmet katmanlarından nasıl farklı olduğunu açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 10/01/2019
ms.openlocfilehash: 074a28af8c80c109dbe97306900e8f00618e435a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81411694"
---
# <a name="hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanı

Azure SQL Veritabanı, altyapı hataları durumunda bile %99,99 kullanılabilirlik sağlamak için bulut ortamı için ayarlanan SQL Server Database Engine mimarisine dayanır. Azure SQL Veritabanı'nda kullanılan üç mimari model vardır:
- Genel Amaç/Standart 
-  Hiper Ölçek
-  İş Kritik / Premium

Azure SQL Veritabanı'ndaki Hyperscale hizmet katmanı, vCore tabanlı satın alma modelindeki en yeni hizmet katmanıdır. Bu hizmet katmanı, Azure SQL Veritabanı'nın depolama ve işlem kaynaklarını Genel Amaç ve İş Açısından Kritik hizmet katmanları için mevcut sınırların önemli ölçüde üzerinde ölçeklendirmek için Azure mimarisinden yararlanan son derece ölçeklenebilir bir depolama ve işlem performansı katmanıdır.

> 
> [!NOTE]
> vCore tabanlı satın alma modelindeki Genel Amaç ve İş Açısından Kritik hizmet katmanları hakkında ayrıntılı bilgi için [Genel Amaç](sql-database-service-tier-general-purpose.md) ve İş [Açısından Kritik](sql-database-service-tier-business-critical.md) hizmet katmanlarına bakın. vCore tabanlı satın alma modelinin DTU tabanlı satın alma modeliyle karşılaştırılması için [Azure SQL Veritabanı satın alma modelleri ve kaynaklarına](sql-database-service-tiers.md)bakın.


## <a name="what-are-the-hyperscale-capabilities"></a>Hiperölçek özellikleri nelerdir

Azure SQL Veritabanı'ndaki Hyperscale hizmet katmanı aşağıdaki ek özellikleri sağlar:

- 100 TB'a kadar veritabanı boyutu desteği
- İşlem kaynakları üzerinde IO etkisi olmadan boyutundan bağımsız olarak neredeyse anında veritabanı yedeklemeleri (Azure Blob depolama alanında depolanan dosya anlık görüntülerine dayalı)  
- Hızlı veritabanı geri yüklemeleri (dosya anlık görüntülerine dayalı olarak) saat veya gün yerine dakika içinde (veri işlemi boyutu değil)
- Veri hacimlerinden bağımsız olarak daha yüksek günlük veri girişi ve daha hızlı işlem işleme süreleri nedeniyle daha yüksek genel performans
- Hızlı ölçeklendirme - okuma iş yükünüzü boşaltmak ve sıcak bekleme olarak kullanmak için salt okunur düğümleri bir veya daha fazla kullanılabilir
- Hızlı Ölçeklendir - gerektiğinde ağır iş yüklerini karşılamak için hesaplama kaynaklarınızı sabit bir zamanda ölçeklendirebilir ve gerekli olmadığında bilgi işlem kaynaklarını küçültebilirsiniz.

Hyperscale hizmet katmanı, bulut veritabanlarında geleneksel olarak görülen pratik sınırların çoğunu kaldırır. Diğer veritabanlarının çoğunun tek bir düğümde bulunan kaynaklarla sınırlı olduğu durumlarda, Hyperscale hizmet katmanındaki veritabanlarının böyle bir sınırı yoktur. Esnek depolama mimarisi ile depolama gerektiğinde büyür. Aslında, Hyperscale veritabanları tanımlı bir maksimum boyutu ile oluşturulmaz. Bir Hyperscale veritabanı gerektiği gibi büyür ve yalnızca kullandığınız kapasite için faturalandırılır. Okuma yoğun iş yükleri için, Hyperscale hizmet katmanı, okuma iş yüklerini boşaltmak için gerektiğinde ek okuma yinelemeleri sağlayarak hızlı ölçeklendirme sağlar.

Ayrıca, veritabanı yedeklemeleri oluşturmak veya küçültmek için gereken süre artık veritabanındaki veri hacmine bağlı değildir. Hiper ölçekli veritabanları anında yedeklenebilir. Ayrıca, bir veritabanını dakikalar içinde yukarı veya aşağı onlarca terabayt tasnif edebilirsiniz. Bu özellik, ilk yapılandırma seçimleriniz tarafından kutulanma endişelerinden sizi kurtarır.

Hyperscale hizmet katmanının bilgi işlem boyutları hakkında daha fazla bilgi için [Hizmet katmanı özelliklerine](sql-database-service-tiers-vcore.md#service-tiers)bakın.

## <a name="who-should-consider-the-hyperscale-service-tier"></a>Hiperölçekli hizmet katmanını kimler dikkate almalıdır?

Hiper ölçeklendirme hizmet katmanı, bağımsız olarak ölçeklenebilir işlem ve depolama kaynaklarıyla büyük esneklik ve yüksek performans sağladığından çoğu iş yükü için tasarlanmıştır. 100 TB'a kadar otomatik depolama alanı ölçeklendirme özelliği sayesinde, aşağıdakiler için mükemmel bir seçimdir:

- Şirket içinde büyük veritabanlarına sahip olun ve buluta taşınarak uygulamalarını modernize etmek istiyorum
- Bulutta zaten ve diğer hizmet katmanlarının maksimum veritabanı boyutu kısıtlamalarıyla sınırlıdır (1-4 TB)
- Daha küçük veritabanlarına sahip, ancak hızlı dikey ve yatay işlem ölçekleme, yüksek performans, anında yedekleme ve hızlı veritabanı geri yükleme gerektirir.

Hyperscale hizmet katmanı, saf OLTP'den saf analitike kadar çok çeşitli SQL Server iş yüklerini destekler, ancak öncelikle OLTP ve karma işlem ve analitik işleme (HTAP) iş yükleri için optimize edilse de.

> [!IMPORTANT]
> Elastik havuzlar Hyperscale servis katmanını desteklemez.

## <a name="hyperscale-pricing-model"></a>Hiper ölçekli fiyatlandırma modeli

Hyperscale hizmet katmanı sadece [vCore modelinde](sql-database-service-tiers-vcore.md)kullanılabilir. Yeni mimariyle uyumlu olmak için fiyatlandırma modeli Genel Amaç veya İş Açısından Kritik hizmet katmanlarından biraz farklıdır:

- **Hesaplama**:

  Hyperscale işlem birim fiyatı çoğaltma başına. [Azure Karma Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) fiyatı, ölçek yinelemelerini otomatik olarak okumak için uygulanır. Varsayılan olarak, varsayılan olarak, bir birincil yineleme ve Hyperscale veritabanı başına salt okunur çoğaltma oluştururuz.  Kullanıcılar birincil dahil olmak üzere toplam çoğaltma sayısını 1-5 ayarlayabilir.

- **Depolama**:

  Bir Hiper ölçekli veritabanını yapılandırırken maksimum veri boyutunu belirtmeniz gerekmez. Hiper ölçekli katmanda, gerçek tahsisata dayalı olarak veritabanınız için depolama için ücretlendirilirsiniz. Depolama, 40 GB ile 100 TB arasında, 10 GB'lık artışlarla otomatik olarak tahsis edilir. Gerekirse birden çok veri dosyası aynı anda büyüyebilir. Bir Hyperscale veritabanı 10 GB başlangıç boyutu ile oluşturulur ve 40 GB boyutuna ulaşana kadar, her 10 dakikada bir 10 GB büyümeye başlar.

Hiper ölçek fiyatlandırması hakkında daha fazla bilgi için Azure [SQL Veritabanı Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/sql-database/single/) bakın

## <a name="distributed-functions-architecture"></a>Dağıtılmış işlevler mimarisi

Tüm veri yönetimi işlevlerini tek bir konum/işlemde merkezileştiren geleneksel veritabanı altyapılarının aksine (günümüzde dağıtılan veritabanları nın tek bir veri motorunun birden fazla kopyası vardır), Hyperscale veritabanı sorgu işleme motorlarını ayırır ve çeşitli veri motorlarının semantiklerinin veri için uzun süreli depolama ve dayanıklılık sağlayan bileşenlerden ayrıştırılır. Bu şekilde, depolama kapasitesi gerektiği kadar sorunsuz bir şekilde ölçeklenebilir (ilk hedef 100 TB'dir). Salt okunur yinelemeler aynı depolama bileşenlerini paylaşır, bu nedenle yeni okunabilir bir yinelemeyi döndürmek için veri kopyası gerekmez. 

Aşağıdaki diyagram, Bir Hiper ölçek veritabanında farklı düğüm türlerini gösterir:

![architecture](./media/sql-database-hyperscale/hyperscale-architecture.png)

Hiper ölçek veritabanı aşağıdaki farklı bileşen türlerini içerir:

### <a name="compute"></a>İşlem

İşlem düğümü ilişkisel altyapının yaşadığı yerdir, bu nedenle tüm dil öğeleri, sorgu işleme ve benzeri, oluşur. Bir Hyperscale veritabanı ile tüm kullanıcı etkileşimleri bu bilgi işlem düğümleri üzerinden olur. İşlem düğümlerinde, bir sayfa veri getirmek için gereken ağ tur gezileri sayısını en aza indirmek için SSD tabanlı önbellekler (önceki diyagramda RBPEX - Esnek Arabellek Havuzu Uzantısı olarak etiketlenmiştir). Tüm okuma-yazma iş yüklerinin ve işlemlerinin işlendiği bir birincil işlem düğümü vardır. Başarısız amaçlar için sıcak bekleme düğümleri olarak hareket eden bir veya daha fazla ikincil işlem düğümü ve okuma iş yüklerini boşaltmak için salt okunur bilgi işlem düğümleri olarak hareket eden (bu işlevsellik isteniyorsa).

### <a name="page-server"></a>Sayfa sunucusu

Sayfa sunucuları, ölçeklenmiş depolama motorlarını temsil eden sistemlerdir.  Her sayfa sunucusu, veritabanındaki sayfaların bir alt kümesinden sorumludur.  Nominal olarak, her sayfa sunucusu 128 GB ile 1 TB veri arasında kontrol eder. Birden fazla sayfa sunucusunda (artıklık ve kullanılabilirlik için tutulan yinelemeler dışında) veri paylaşılmaz. Bir sayfa sunucusunun görevi, veritabanı sayfalarını talep üzerine bilgi işlem düğümlerine sunmak ve işlemler verileri güncelleştirerken sayfaları güncel tutmaktır. Sayfa sunucuları, günlük hizmetinden günlük kayıtları oynatılarak güncel tutulur. Sayfa sunucuları, performansı artırmak için SSD tabanlı önbellekleri de korur. Ek güvenilirlik için veri sayfalarının uzun süreli depolaması Azure Depolama'da tutulur.

### <a name="log-service"></a>Günlük hizmeti

Günlük hizmeti birincil bilgi işlem yinelemesinden günlük kayıtlarını kabul eder, bunları dayanıklı bir önbellekte tutar ve günlük kayıtlarını, verilerin orada güncelleştirilebilmeleri için, önbelleklerini (önbelleklerini güncelleştirebilmeleri için) diğer bilgi işlem yinelemelerine (önbelleklerini güncelleştirebilmeleri için) iletir. Bu şekilde, birincil bilgi işlem yinelemesinden tüm veri değişiklikleri günlük hizmeti aracılığıyla tüm ikincil bilgi işlem yinelemelerine ve sayfa sunucularına yayılır. Son olarak, günlük kayıtları neredeyse sonsuz bir depolama deposu olan Azure Depolama'da uzun süreli depolamaya itilir. Bu mekanizma sık sık günlük kesilme gereksinimini kaldırır. Günlük hizmeti, günlük kayıtlarına erişimi hızlandırmak için yerel önbelleğe de sahiptir.

### <a name="azure-storage"></a>Azure Storage

Azure Depolama, veritabanındaki tüm veri dosyalarını içerir. Sayfa sunucuları veri dosyalarını Azure Depolama'da güncel tutar. Bu depolama, yedekleme amaçlarının yanı sıra Azure bölgeleri arasında çoğaltma için de kullanılır. Yedeklemeler, veri dosyalarının depolama anlık görüntüleri kullanılarak uygulanır. Anlık görüntüleri kullanarak işlemleri geri yükleme veri boyutu ne olursa olsun hızlıdır. Veriler, veritabanının yedekleme bekletme süresi içinde herhangi bir noktaya geri yüklenebilir.

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Yedeklemeler dosya anlık görüntüsüne dayanır ve bu nedenle neredeyse anlık olarak dırlar. Depolama ve bilgi işlem ayrımı, birincil işlem yinelemesindeki işlem yükünü azaltmak için yedekleme/geri yükleme işlemini depolama katmanına itmenizi sağlar. Sonuç olarak, veritabanı yedeklemesi birincil işlem düğümünün performansını etkilemez; benzer şekilde, geri yüklemeler dosya anlık görüntülerine geri döndürülerek yapılır ve bu nedenle veri işlemi boyutu değildir. Geri yükleme sürekli bir işlemdir ve hatta çoklu terabayt veritabanları saat veya gün yerine dakika içinde geri yüklenebilir. Varolan bir yedeklemeyi geri kazanarak yeni veritabanlarının oluşturulması da bu özelliktan yararlanır: terabayt boyutundaki veritabanlarının bile geliştirme veya sınama amacıyla veritabanı kopyalarının oluşturulması birkaç dakika içinde mümkündür.

## <a name="scale-and-performance-advantages"></a>Ölçek ve performans avantajları

Yalnızca salt okunur işlem düğümlerini hızla yukarı/aşağı döndürme yeteneğiyle, Hyperscale mimarisi önemli okuma ölçeği yeteneklerine izin verir ve aynı zamanda daha fazla yazma isteği sunan birincil işlem düğümlerini de serbest bırakabiliyor. Ayrıca, Hyperscale mimarisinin paylaşılan depolama mimarisi sayesinde işlem düğümleri hızla yukarı/aşağı ölçeklendirilebilir.

## <a name="create-a-hyperscale-database"></a>Hiper ölçekli veritabanı oluşturma

[Azure portalı,](https://portal.azure.com) [T-SQL,](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current) [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) veya [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)kullanılarak Bir Hiper ölçekli veritabanı oluşturulabilir. Hiper ölçekli veritabanları yalnızca [vCore tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)kullanılarak kullanılabilir.

Aşağıdaki T-SQL komutu bir Hyperscale veritabanı oluşturur. `CREATE DATABASE` İfadede hem sürüm hem de hizmet hedefini belirtmeniz gerekir. Geçerli hizmet hedefleri listesi için [kaynak sınırlarına](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases#hyperscale---provisioned-compute---gen4) bakın.

```sql
-- Create a Hyperscale Database
CREATE DATABASE [HyperscaleDB1] (EDITION = 'Hyperscale', SERVICE_OBJECTIVE = 'HS_Gen5_4');
GO
```
Bu 4 çekirdekli Gen5 donanım bir Hyperscale veritabanı oluşturacaktır.

## <a name="migrate-an-existing-azure-sql-database-to-the-hyperscale-service-tier"></a>Varolan bir Azure SQL Veritabanını Hiper ölçek hizmet katmanına geçirin

Mevcut Azure SQL veritabanlarınızı [Azure portalı](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current), [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [CLI'yi](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)kullanarak Hyperscale'e taşıyabilirsiniz. Şu anda, bu tek yönlü bir geçiş. Veri dışa aktarma ve alma dışında veritabanlarını Hyperscale'den başka bir hizmet katmanına taşıyamazsınız. Kavram kanıtları (POCs) için, üretim veritabanlarınızın bir kopyasını oluşturmanızı ve kopyayı Hyperscale'e geçirmenizi öneririz. Varolan bir Azure SQL veritabanını Hiper ölçek katmanına geçirmek, veri işleminin boyutudur.

Aşağıdaki T-SQL komutu bir veritabanını Hyperscale hizmet katmanına taşır. `ALTER DATABASE` İfadede hem sürüm hem de hizmet hedefini belirtmeniz gerekir.

```sql
-- Alter a database to make it a Hyperscale Database
ALTER DATABASE [DB2] MODIFY (EDITION = 'Hyperscale', SERVICE_OBJECTIVE = 'HS_Gen5_4');
GO
```

## <a name="connect-to-a-read-scale-replica-of-a-hyperscale-database"></a>Hiper ölçek veritabanının okuma ölçeği yinelemesine bağlanma

Hyperscale veritabanlarında, `ApplicationIntent` istemci tarafından sağlanan bağlantı dizesinde bağımsız değişken, bağlantının yazma yinelemesine mi yoksa salt okunur ikincil yinelemeye mi yönlendirildiğini belirler. Ayarlanan `ApplicationIntent` `READONLY` ve veritabanında ikincil bir yineleme yoksa, bağlantı birincil yinelemeye yönlendirilecek `ReadWrite` ve varsayılan olarak davranışa yönlendirilecektir.

```cmd
-- Connection string with application intent
Server=tcp:<myserver>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Hiper ölçekli ikincil yinelemeler, birincil yineleme yle aynı Hizmet Düzeyi Hedefi'ni kullanarak aynıdır. Birden fazla ikincil yineleme varsa, iş yükü kullanılabilir tüm ikincil lere dağıtılır. Her ikincil yineleme bağımsız olarak güncelleştirilir, böylece farklı yinelemeler birincil yinelemeye göre farklı veri gecikmesi olabilir.

## <a name="database-high-availability-in-hyperscale"></a>Hiper ölçekte Veritabanı Yüksek Kullanılabilirlik

Diğer tüm hizmet katmanlarında olduğu gibi, Hyperscale işlem yineleme kullanılabilirliğinden bağımsız olarak taahhüt edilen hareketler için veri dayanıklılığını garanti eder. Birincil yinelemenin kullanılamamasınedeniyle kesinti süresinin kapsamı, başarısızlığın türüne (planlandı vs. planlanmamış) ve en az bir ikincil yinelemenin varlığına bağlıdır. Planlı bir hatada (yani bir bakım olayında), sistem bir hata başlatmadan önce yeni birincil yinelemeyi oluşturur veya başarısız hedef olarak varolan ikincil yinelemeyi kullanır. Planlanmamış bir hatada (örn. birincil yinelemedeki bir donanım hatası), sistem varsa başarısız hedef olarak ikincil bir yineleme kullanır veya kullanılabilir bilgi işlem kapasitesi havuzundan yeni bir birincil yineleme oluşturur. İkinci durumda, yeni birincil yinelemeyi oluşturmak için gereken ek adımlar nedeniyle kesinti süresi daha uzundur.

Hyperscale SLA [için Azure SQL Veritabanı için SLA'ya](https://azure.microsoft.com/support/legal/sla/sql-database/)bakın.

## <a name="disaster-recovery-for-hyperscale-databases"></a>Hiper ölçekli veritabanları için Olağanüstü Durum Kurtarma

### <a name="restoring-a-hyperscale-database-to-a-different-geography"></a>Hiper ölçekli veritabanını farklı bir coğrafyaya geri verme
Bir Azure SQL Veritabanı Hiper ölçekli DB'yi, olağanüstü durum kurtarma işleminin veya sondajın, yer değiştirmenin veya başka bir nedenle bir parçası olarak, şu anda barındırılan bölge dışında bir bölgeye geri yüklemeniz gerekiyorsa, birincil yöntem veritabanının coğrafi geri yüklemesini yapmaktır.  Bu, diğer AZURE SQL DB'yi farklı bir bölgeye geri yüklemek için kullanacağınız adımlarla tam olarak aynı adımları içerir:
1. Zaten uygun bir sunucunuz yoksa hedef bölgede bir Azure SQL Veritabanı sunucusu oluşturun.  Bu sunucu, özgün (kaynak) sunucuyla aynı aboneye sahip olmalıdır.
2. Azure SQL Veritabanlarını otomatik yedeklemelerden geri yükleme yle ilgili sayfanın [coğrafi geri yükleme](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups#geo-restore) konusundaki yönergeleri izleyin.

> [!NOTE]
> Kaynak ve hedef ayrı bölgelerde olduğundan, veritabanı son derece hızlı bir şekilde tamamlanan coğrafi olmayan geri yüklemelerde olduğu gibi anlık görüntü depolamasını kaynak veritabanıyla paylaşamaz. Bir Hiper ölçekli veritabanının coğrafi geri yükünün geri yüklanması durumunda, hedef coğrafi olarak çoğaltılan depolamanın eşleştirilmiş bölgesinde olsa bile, veri boyutu işlemi olacaktır.  Bu, bir coğrafi geri yükleme yapmanın, geri yüklenen veritabanının boyutuyla orantılı olarak zaman alacağı anlamına gelir.  Hedef eşleştirilmiş bölgedeyse, kopya bir bölge arası kopyadan önemli ölçüde daha hızlı olacak bir bölge içinde olacaktır, ancak yine de veri boyutu işlemi olacaktır.

## <a name="available-regions"></a><a name=regions></a>Kullanılabilir bölgeler

Azure SQL Veritabanı Hiper ölçek katmanı şu anda aşağıdaki bölgelerde kullanılabilir:

- Doğu Avustralya
- Güneydoğu Avustralya
- Güney Brezilya
- Orta Kanada
- Orta ABD
- Çin Doğu 2
- Çin Kuzey 2
- Doğu Asya
- Doğu ABD
- Doğu Abd 2
- Orta Fransa
- Doğu Japonya
- Batı Japonya
- Güney Kore - Orta
- Güney Kore - Güney
- Orta Kuzey ABD
- Kuzey Avrupa
- Güney Afrika Kuzey
- Orta Güney ABD
- Güneydoğu Asya
- Güney Birleşik Krallık
- Batı Birleşik Krallık
- Batı Avrupa
- Batı ABD
- Batı ABD 2

Desteklenen olarak listelenmeyen bir bölgede Hiperölçekli veritabanı oluşturmak istiyorsanız, Azure portalı üzerinden bir onboarding isteği gönderebilirsiniz. Yönergeler için, yönergeler [için Azure SQL Veritabanı için İstek kotası artışlarına](quota-increase-request.md) bakın. Talebinizi gönderirken aşağıdaki yönergeleri kullanın:

- Diğer [kota isteği](quota-increase-request.md#other) SQL veritabanı kota türünü kullanın.
- Metin ayrıntılarında, okunabilir yinelemeler de dahil olmak üzere işlem SKU/toplam çekirdekleri ekleyin.
- Ayrıca tahmini TB belirtin.

## <a name="known-limitations"></a>Bilinen sınırlamalar

Bunlar, GA itibariyle Hyperscale hizmet katmanına geçerli sınırlamalar getirilir.  Bu sınırlamaların mümkün olduğunca çoğunu kaldırmak için etkin bir şekilde çalışıyoruz.

| Sorun | Açıklama |
| :---- | :--------- |
| Mantıksal bir sunucu için Yedekleri Yönet bölmesi Hiperölçek veritabanlarını göstermez, bunlar görünümden filtrelenir  | Hyperscale yedekleme leri yönetmek için ayrı bir yönteme sahiptir ve bu nedenle Uzun Süreli Bekletme ve Zaman Noktasında Nokta yedekleme bekletme ayarları geçerli değildir / geçersiz kılınmıştır. Buna göre, Hyperscale veritabanları Yedeklemeyi Yönet bölmesinde görünmez. |
| Belirli bir noktaya geri yükleme | Hiperölçekli olmayan veritabanı tutma süresi içinde, Hiperölçekli olmayan bir veritabanına Bir Hiperölçekli veritabanı geri yükleyebilirsiniz. Hiperölçek olmayan bir veritabanını Bir Hiper ölçek veritabanına geri yükleyemezsiniz.|
| Bir veritabanında 1 TB'den büyük bir veya daha fazla veri dosyası varsa, geçiş başarısız olur | Bazı durumlarda, büyük dosyaları 1 TB'den az olacak şekilde küçülterek bu sorunu çözmek mümkün olabilir. Geçiş işlemi sırasında kullanılan bir veritabanını geçirtiyorsanız, hiçbir dosyanın 1 TB'dan büyük olmadığından emin olun. Veritabanı dosyalarının boyutunu belirlemek için aşağıdaki sorguyu kullanın. `SELECT *, name AS file_name, size * 8. / 1024 / 1024 AS file_size_GB FROM sys.database_files WHERE type_desc = 'ROWS'`;|
| Yönetilen Örnek | Azure SQL Veritabanı Yönetilen Örnek şu anda Hyperscale veritabanları ile desteklenmez. |
| Esnek Havuzlar |  Elastik Havuzlar şu anda SQL Veritabanı Hyperscale ile desteklenmez.|
| Hiper ölçeklere geçiş şu anda tek yönlü bir işlemdir | Bir veritabanı Hyperscale'e geçirildikten sonra, doğrudan Hiperölçekli olmayan bir hizmet katmanına geçirilemez. Şu anda, bir veritabanını Hyperscale'den Hiperölçek olmayana geçirmenin tek yolu, bir BACPAC dosyası veya diğer veri hareketi teknolojilerini (Toplu Kopya, Azure Veri Fabrikası, Azure Veri Tuğlaları, SSIS, vb.) kullanarak dışa aktarma/alma işlemidir.|
| Kalıcı bellek nesneleri ile veritabanlarının geçişi | Hiperölçek yalnızca kalıcı olmayan Bellek nesneleri (tablo türleri, yerel SP'ler ve işlevler) destekler.  Kalıcı Bellek İçi tablolar ve diğer nesneler, bir veritabanını Hyperscale hizmet katmanına geçirmeden önce Bellek içi olmayan nesneler olarak bırakılmalı ve yeniden oluşturulmalıdır.|
| Coğrafi Çoğaltma  | Azure SQL Veritabanı Hyperscale için coğrafi çoğaltmayı henüz yapılandıramazsınız. |
| Veritabanı Kopyası | Azure SQL Hyperscale'de yeni bir veritabanı oluşturmak için Veritabanı Kopyası'nı henüz kullanamazsınız. |
| TDE/AKV Entegrasyonu | Azure Key Vault (genellikle Kendi Anahtarını Getir veya BYOK olarak da adlandırılır) kullanarak Saydam Veritabanı Şifrelemesi, Azure SQL Veritabanı Hyperscale için henüz desteklenmez, ancak Hizmet Yönetilen Anahtarlarla TDE tam olarak desteklenir. |
|Akıllı Veritabanı Özellikleri | "Kuvvet Planı" seçeneği dışında, diğer tüm Otomatik ayar seçenekleri Hyperscale'de henüz desteklenmez: seçenekler etkin leştirilmiş gibi görünebilir, ancak herhangi bir öneri veya eylem yapılmaz. |
|Sorgu Performansı İçgörüleri | Sorgu Performans Öngörüleri şu anda Hiper ölçekli veritabanları için desteklenmez. |
| Veritabanını Küçült | DBCC SHRINKDATABASE veya DBCC SHRINKFILE şu anda Hiper ölçekli veritabanları için desteklenmez. |
| Veritabanı bütünlüğü denetimi | DBCC CHECKDB şu anda Hyperscale veritabanları için desteklenmez. Azure [SQL Veritabanı'ndaki](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/) veri bütünlüğü yönetimi yle ilgili ayrıntılar için Azure SQL Veritabanı'nda Veri Bütünlüğü'ne bakın. |

## <a name="next-steps"></a>Sonraki adımlar

- Hiper ölçekte bir SSS için, [Hiperölçek hakkında sık sorulan sorulara](sql-database-service-tier-hyperscale-faq.md)bakın.
- Hizmet katmanları hakkında bilgi için [Hizmet katmanlarına](sql-database-service-tiers.md) bakın
- Bkz. Sunucu ve abonelik düzeylerindeki sınırlar hakkında bilgi için [mantıksal bir sunucudaki kaynak sınırlarına genel bakış.](sql-database-resource-limits-logical-server.md)
- Tek bir veritabanı için model sınırları satın almak [için, tek bir veritabanı için Azure SQL Veritabanı vCore tabanlı satın alma modeli sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın.
- Özellikler ve karşılaştırma listesi için [SQL ortak özelliklerine](sql-database-features.md)bakın.
