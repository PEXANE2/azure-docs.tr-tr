---
title: Data Sync
description: Bu genel bakış Azure SQL Veri Eşitlemeyi'ni
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 08/20/2019
ms.openlocfilehash: 8708c458e1064e4b9ea7dc67f1a4d4fbce1547b0
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81481959"
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync"></a>Verileri birden çok bulut ve şirket içi veritabanları arasında SQL Veri Eşitleme ile eşitleme

SQL Data Sync, seçtiğiniz verileri birden çok SQL veritabanı ve SQL Server örnekleri arasında çift yönlü olarak eşitlemenize olanak tanıyan Azure SQL Veritabanı'nda oluşturulmuş bir hizmettir.

> [!IMPORTANT]
> Azure SQL Veri Eşitlemesi şu anda Azure SQL Veritabanı Yönetilen Örneği'ni desteklemez.

## <a name="when-to-use-data-sync"></a>Veri Eşitleme ne zaman kullanılır

Veri Eşitleme, verilerin birkaç Azure SQL veritabanında veya SQL Server veritabanlarında güncel tutulması gereken durumlarda yararlıdır. Veri Eşitleme için ana kullanım örnekleri şunlardır:

- **Karma Veri Senkronizasyonu:** Veri Eşitleme ile, karma uygulamaları etkinleştirmek için verileri şirket içi veritabanlarınız ve Azure SQL veritabanlarınız arasında senkronize edebilirsiniz. Bu özellik, buluta taşınmayı düşünen ve uygulamalarından bazılarını Azure'a koymak isteyen müşterileriçin cazip olabilir.
- **Dağıtılan Uygulamalar:** Çoğu durumda, farklı veritabanları arasında farklı iş yüklerini ayırmak yararlıdır. Örneğin, büyük bir üretim veritabanınız varsa, ancak bu verilerde raporlama veya analiz iş yükünü çalıştırmanız gerekiyorsa, bu ek iş yükü için ikinci bir veritabanına sahip olmanız yararlı olur. Bu yaklaşım, üretim iş yükünüz üzerindeki performans etkisini en aza indirir. Bu iki veritabanını eşitlenmiş tutmak için Veri Eşitleme'yi kullanabilirsiniz.
- **Genel Olarak Dağıtılan Uygulamalar:** Birçok işletme çeşitli bölgeleri ve hatta çeşitli ülkeleri/bölgeleri kapsar. Ağ gecikmesini en aza indirmek için verilerinizin size yakın bir bölgede olması en iyisidir. Veri Eşitleme ile, dünyanın çeşitli bölgelerindeki veritabanlarını kolayca senkronize edebilirsiniz.

Veri Eşitleme aşağıdaki senaryolar için tercih edilen çözüm değildir:

| Senaryo | Bazı önerilen çözümler |
|----------|----------------------------|
| Olağanüstü Durum Kurtarma | [Azure coğrafi yedekyedeklemeleri](sql-database-automated-backups.md) |
| Okuma Ölçeği | [Yalnızca okunur sorgu iş yüklerini yüklemek için salt okunur yinelemeleri kullanma (önizleme)](sql-database-read-scale-out.md) |
| ETL (OLTP'den OLAP'a) | [Azure Veri Fabrikası](https://azure.microsoft.com/services/data-factory/) veya [SQL Server Tümleştirme Hizmetleri](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) |
| Şirket içi SQL Server'dan Azure SQL Veritabanına geçiş | [Azure Veritabanı Geçiş Hizmeti](https://azure.microsoft.com/services/database-migration/) |
|||

## <a name="overview-of-sql-data-sync"></a>SQL Veri Eşitleme Genel Bakış

Veri Eşitleme, Eşitleme Grubu kavramına dayanır. Eşitleme Grubu, eşitlemek istediğiniz bir veritabanı grubudur.

Veri Eşitleme, verileri eşitlemek için hub ve kollu topoloji kullanır. Eşitleme grubundaki veritabanlarından birini Hub Veritabanı olarak tanımlarsınız. Veritabanlarının geri kalanı üye veritabanlarıdır. Eşitleme yalnızca Hub ve tek tek üyeler arasında gerçekleşir.

- **Hub Veritabanı** bir Azure SQL Veritabanı olmalıdır.
- **Üye veritabanları** SQL Veritabanları, şirket içi SQL Server veritabanları veya Azure sanal makinelerdeki SQL Server örnekleri olabilir.
- **Eşitleme Veritabanı** meta verileri ve Veri Eşitleme için günlük içerir. Eşitleme Veritabanı, Hub Veritabanı ile aynı bölgede bulunan bir Azure SQL Veritabanı olmalıdır. Eşitleme Veritabanı müşteri tarafından oluşturulur ve müşteriye aittir.

> [!NOTE]
> Bir şirket içi veritabanını üye veritabanı olarak kullanıyorsanız, [yerel bir eşitleme aracısı yüklemeniz ve yapılandırmanız](sql-database-get-started-sql-data-sync.md#add-on-prem)gerekir.

![Veritabanları arasında verileri eşitleme](media/sql-database-sync-data/sync-data-overview.png)

Eşitleme Grubu aşağıdaki özelliklere sahiptir:

- **Eşitleme Şeması,** hangi verilerin eşitlendiğini açıklar.
- **Eşitleme Yönü** çift yönlü olabilir veya yalnızca bir yönde akabilir. Diğer bir şey, Eşitleme Yönü *Üye'ye Hub*veya *Üyeden Hub'a*veya her ikisine de hub olabilir.
- **Eşitleme Aralığı,** eşitlemenin ne sıklıkta oluştuğunu açıklar.
- **Çakışma Çözümlemesi İlkesi,** *Hub'ın kazanması* veya *Üye kazanması*olabilecek bir grup düzeyi ilkesidir.

## <a name="how-does-data-sync-work"></a>Veri Eşitleme nasıl çalışır?

- **Veri değişikliklerini izleme:** Veri Eşitleme, ekleme, güncelleştirme ve silme tetikleyicilerini kullanarak değişiklikleri izler. Değişiklikler kullanıcı veritabanındaki bir yan tabloya kaydedilir. TOPLU INSERT'in varsayılan olarak tetikleyicileri ateşlemediğini unutmayın. FIRE_TRIGGERS belirtilmemişse, hiçbir ekleme tetikleyicileri yürütme. Veri Eşitleme bu ekler izleyebilirsiniz böylece FIRE_TRIGGERS seçeneği ekleyin. 
- **Verileri eşitleme:** Veri Eşitleme Hub ve Spoke modelinde tasarlanmıştır. Hub, her üyeyle ayrı ayrı eşitler. Hub'daki değişiklikler üyeye indirilir ve üyeden gelen değişiklikler Hub'a yüklenir.
- **Çakışmaları çözme:** Veri Eşitleme çakışma çözümü için iki seçenek sağlar, *Hub kazanır* veya *Üye kazanır.*
  - *Hub kazanır'ı*seçerseniz, hub'daki değişiklikler her zaman üyedeki değişikliklerin üzerine yazılır.
  - *Üye kazanır*seçerseniz, üye değişiklikleri hub değişiklikleri üzerine yazmak. Birden fazla üye varsa, son değer ilk eşitlenen üyeye bağlıdır.

## <a name="compare-data-sync-with-transactional-replication"></a>İşlemsel Çoğaltma ile Veri Eşitleme karşılaştırın

| | Data Sync | İşlem Çoğaltması |
|---|---|---|
| Yararları | - Aktif-aktif destek<br/>- Şirket içi ve Azure SQL Veritabanı arasında çift yönlü | - Daha düşük gecikme<br/>- İşlemsel tutarlılık<br/>- Göç sonrası mevcut topolojiyi yeniden kullanma |
| Dezavantajlar | - 5 dk veya daha fazla gecikme<br/>- İşlemsel tutarlılık yok<br/>- Daha yüksek performans etkisi | - Azure SQL Veritabanı'ndan tek veritabanıveya havuzlu veritabanından yayımlanamıyor<br/>- Yüksek bakım maliyeti |

## <a name="get-started-with-sql-data-sync"></a>SQL Veri Eşitlemeyi ile başlayın

### <a name="set-up-data-sync-in-the-azure-portal"></a>Azure portalında Veri Eşitleme'yi ayarlama

- [Azure SQL Data Sync’i ayarlama](sql-database-get-started-sql-data-sync.md)
- Veri Eşitleme Aracısı - [Azure SQL Veri Eşitleme için Veri Eşitleme Aracısı](sql-database-data-sync-agent.md)

### <a name="set-up-data-sync-with-powershell"></a>PowerShell ile Veri Eşitlemeyi'ni ayarlama

- [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
- [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)

### <a name="review-the-best-practices-for-data-sync"></a>Veri Eşitleme için en iyi uygulamaları gözden geçirin

- [Azure SQL Data Sync için en iyi yöntemler](sql-database-best-practices-data-sync.md)

### <a name="did-something-go-wrong"></a>Bir şeyler ters gitti mi?

- [Azure SQL Data Sync ile ilgili sorun giderme](sql-database-troubleshoot-data-sync.md)

## <a name="consistency-and-performance"></a>Tutarlılık ve performans

### <a name="eventual-consistency"></a>Nihai tutarlılık

Veri Eşitleme tetikleyici tabanlı olduğundan, işlem tutarlılığı garanti değildir. Microsoft, tüm değişikliklerin sonunda yapıldığını ve Veri Eşitlemesi'nin veri kaybına neden olmadığını garanti eder.

### <a name="performance-impact"></a>Performans etkisi

Veri Eşitleme değişiklikleri izlemek için ekleme, güncelleştirme ve silme tetikleyicileri kullanır. Değişiklik izleme için kullanıcı veritabanında yan tablolar oluşturur. Bu değişiklik izleme etkinlikleri veritabanı iş yükünüz üzerinde bir etkiye sahiptir. Gerekirse servis katmanınızı değerlendirin ve yükseltin.

Eşitleme grubu oluşturma, güncelleştirme ve silme sırasında sağlama ve deprovisioning veritabanı performansını da etkileyebilir.

## <a name="requirements-and-limitations"></a><a name="sync-req-lim"></a>Gereksinimler ve sınırlamalar

### <a name="general-requirements"></a>Genel gereksinimler

- Her tablonun birincil anahtarı olmalıdır. Birincil anahtarın değerini herhangi bir satırda değiştirmeyin. Birincil anahtar değerini değiştirmeniz gerekiyorsa, satırı silin ve yeni birincil anahtar değeriyle yeniden oluşturun.

> [!IMPORTANT]
> Varolan birincil anahtarın değerini değiştirmek aşağıdaki hatalı davranışa neden olur:
> - Eşitleme herhangi bir sorun bildirmese de hub ve üye arasındaki veriler kaybedilebilir.
> - İzleme tablosunda birincil anahtar değişikliği nedeniyle kaynaktan var olmayan bir satır olduğundan eşitleme başarısız olabilir.

- Anlık görüntü yalıtımı etkinleştirilmelidir. Daha fazla bilgi için bkz. [SQL Server'da Anlık Görüntü Yalıtımı](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server).

### <a name="general-limitations"></a>Genel sınırlamalar

- Bir tabloda birincil anahtar olmayan bir kimlik sütunu olamaz.
- Birincil anahtar aşağıdaki veri türlerine sahip olamaz: sql_variant, ikili, varikili, görüntü, xml.
- Desteklenen kesinlik yalnızca ikinciye geldiğinden, aşağıdaki veri türlerini birincil anahtar olarak kullandığınızda dikkatli olun: saat, datetime, datetime2, datetimeoffset.
- Nesnelerin adları (veritabanları, tablolar ve sütunlar) yazdırılabilir karakterler dönemi (.), sol kare ayraç ([) veya sağ kare ayraç (]) içeremez.
- Azure Etkin Dizin kimlik doğrulaması desteklenmez.
- Aynı ada sahip ancak farklı şema olan tablolar (örneğin, dbo.customers ve sales.customers) desteklenmez.
- Kullanıcı Tanımlı Veri Türleri içeren sütunlar desteklenmiyor
- Sunucuları farklı abonelikler arasında taşıma desteklenmez. 

#### <a name="unsupported-data-types"></a>Desteklenmeyen veri türleri

- Fılestream
- SQL/CLR UDT
- XMLSchemaCollection (XML destekli)
- İmleç, RowVersion, Zaman Damgası, Hiyerarşik

#### <a name="unsupported-column-types"></a>Desteklenmeyen sütun türleri

Veri Eşitleme salt okunur veya sistem tarafından oluşturulan sütunları eşitleme zedemez. Örneğin:

- Hesaplanmış sütunlar.
- Zamansal tablolar için sistem tarafından oluşturulan sütunlar.

#### <a name="limitations-on-service-and-database-dimensions"></a>Hizmet ve veritabanı boyutlarındaki sınırlamalar

| **Boyutlar**                                                  | **Sınırı**              | **Geçi -ci çözüm**              |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| Herhangi bir veritabanının ait olabileceği maksimum eşitleme grubu sayısı.       | 5                      |                             |
| Tek bir eşitleme grubunda ki maksimum uç nokta sayısı              | 30                     |                             |
| Tek bir eşitleme grubunda ki en fazla şirket içi uç nokta sayısı. | 5                      | Birden çok eşitleme grubu oluşturma |
| Veritabanı, tablo, şema ve sütun adları                       | Ad başına 50 karakter |                             |
| Eşitleme grubundaki tablolar                                          | 500                    | Birden çok eşitleme grubu oluşturma |
| Eşitleme grubundaki tablodaki sütunlar                              | 1000                   |                             |
| Tablodaki veri satır boyutu                                        | 24 Mb                  |                             |
| Minimum eşitleme aralığı                                           | 5 Dakika              |                             |

> [!NOTE]
> Yalnızca bir eşitleme grubu varsa, tek bir eşitleme grubunda en fazla 30 uç nokta olabilir. Birden fazla eşitleme grubu varsa, tüm eşitleme gruplarındaki toplam uç nokta sayısı 30'u geçemez. Bir veritabanı birden çok eşitleme grubuna aitse, birden çok uç nokta olarak sayılır, bir değil.

## <a name="faq-about-sql-data-sync"></a>SQL Veri Eşitlemi hakkında SSS

### <a name="how-much-does-the-sql-data-sync-service-cost"></a>SQL Veri Eşitleme hizmeti nin maliyeti nedir

SQL Data Sync hizmetinin kendisi için herhangi bir ücret alınmaz. Ancak, SQL Veritabanı örneğinizin içinde ve dışında veri hareketi için veri aktarım ücretleri toplamaya devam emzebilirsiniz. Daha fazla bilgi için [SQL Veritabanı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/sql-database/)bakın.

### <a name="what-regions-support-data-sync"></a>Veri Eşitlemeyi hangi bölgeleri destekler

SQL Veri Eşitlemi tüm bölgelerde kullanılabilir.

### <a name="is-a-sql-database-account-required"></a>BIR SQL Veritabanı hesabı gerekli mi

Evet. Hub Veritabanı'nı barındırmak için bir SQL Veritabanı hesabınız olması gerekir.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a>Yalnızca SQL Server şirket içi veritabanları arasında eşitlemek için Veri Eşitlemeyi'ni kullanabilir miyim?

Doğrudan değil. Ancak, Azure'da bir Hub veritabanı oluşturup şirket içi veritabanlarını eşitleme grubuna ekleyerek SQL Server şirket içi veritabanları arasında dolaylı olarak eşitleyebilirsiniz.

### <a name="can-i-use-data-sync-to-sync-between-sql-databases-that-belong-to-different-subscriptions"></a>Farklı aboneliklere ait SQL Veritabanları arasında eşitlemek için Veri Eşitlemeyi'ni kullanabilir miyim?

Evet. Farklı aboneliklere ait kaynak gruplarına ait SQL Veritabanları arasında eşitleyebilirsiniz.

- Abonelikler aynı kiracıya aitse ve tüm aboneliklere izin verdiyseniz, Azure portalındaki eşitleme grubunu yapılandırabilirsiniz.
- Aksi takdirde, farklı aboneliklere ait eşitleme üyelerini eklemek için PowerShell'i kullanmanız gerekir.

### <a name="can-i-use-data-sync-to-sync-between-sql-databases-that-belong-to-different-clouds-like-azure-public-cloud-and-azure-china-21vianet"></a>Farklı bulutlara ait SQL Veritabanları arasında eşitlemek için Veri Eşitlemeyi'ni kullanabilir miyim (Azure Genel Bulutu ve Azure China 21Vianet gibi)

Evet. Farklı bulutlara ait SQL Veritabanları arasında eşitleyebilirsiniz, farklı aboneliklere ait eşitleme üyeleri eklemek için PowerShell kullanmanız gerekir.

### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-sync-them"></a>Veri Eşitlemeyi'ni üretim veritabanımdaki verileri boş bir veritabanına tohumlamak ve sonra eşitlemek için kullanabilir miyim?

Evet. Şeayı orijinalinden komut dosyası oluşturarak yeni veritabanında el ile oluşturun. Şemayı oluşturduktan sonra, verileri kopyalamak ve eşitlemek için tabloları eşitleme grubuna ekleyin.

### <a name="should-i-use-sql-data-sync-to-back-up-and-restore-my-databases"></a>Veritabanlarımı yedeklemek ve geri yüklemek için SQL Data Sync kullanmalı mıyım?

Verilerinizin yedekini oluşturmak için SQL Data Sync'i kullanmanız önerilmez. SQL Veri Eşitleme eşitlemeleri sürülmediği için zaman içinde belirli bir noktaya yedekleyip geri yükleyemezsiniz. Ayrıca, SQL Veri Eşitlemesi, depolanan yordamlar gibi diğer SQL nesnelerini yedeklemez ve bir geri yükleme işleminin eşdeğerini hızlı bir şekilde yapmaz.

Önerilen yedekleme tekniğinden biri [için](sql-database-copy.md)bkz.

### <a name="can-data-sync-sync-encrypted-tables-and-columns"></a>Veri Eşitleme şifreli tablo ve sütunlar

- Bir veritabanı Her Zaman Şifrelenmiş kullanıyorsa, yalnızca *şifrelenmemiş* tabloları ve sütunları eşitleyebilirsiniz. Veri Eşitleme verilerinşifresini çözemediğinden, şifrelenmiş sütunları eşitleme yapamazsınız.
- Bir sütun Sütun Düzeyi Şifreleme (CLE) kullanıyorsa, satır boyutu 24 Mb'ın maksimum boyutundan küçük olduğu sürece sütunu eşitleyebilirsiniz. Veri Eşitleme, anahtar (CLE) tarafından şifrelenmiş sütunu normal ikili veri olarak ele alar. Diğer eşitleme üyelerindeki verilerin şifresini çözmek için aynı sertifikaya sahip olmanız gerekir.

### <a name="is-collation-supported-in-sql-data-sync"></a>SQL Veri Eşitleme'de collation desteklenir mi?

Evet. SQL Data Sync aşağıdaki senaryolarda harmanlama destekler:

- Seçili eşitleme şema tabloları hub veya üye veritabanlarınızda zaten yoksa, eşitleme grubunu dağıttığınızda, hizmet boş hedef veritabanlarında seçilen harmanlama ayarlarıyla ilgili tabloları ve sütunları otomatik olarak oluşturur.
- Eşitlenecek tablolar hem hub hem de üye veritabanlarınızda zaten varsa, SQL Veri Eşitlemi, eşitleme grubunu başarıyla dağıtmak için birincil anahtar sütunlarının hub ve üye veritabanları arasında aynı kolatoya sahip olmasını gerektirir. Sütunlarda birincil anahtar sütunları dışında harmanlama kısıtlaması yoktur.

### <a name="is-federation-supported-in-sql-data-sync"></a>SQL Data Sync'de federasyon desteklenir mi?

Federasyon Kök Veritabanı, SQL Veri Eşitleme Hizmeti'nde herhangi bir sınırlama olmaksızın kullanılabilir. Federe Veritabanı bitiş noktasını SQL Veri Eşitlemesi'nin geçerli sürümüne ekemezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="update-the-schema-of-a-synced-database"></a>Eşitlenmiş bir veritabanının şeasını güncelleştirme

Eşitleme grubundaki bir veritabanının şemasını güncelleştirmek zorunda mısın? Şema değişiklikleri otomatik olarak çoğaltılamaz. Bazı çözümler için aşağıdaki makalelere bakın:

- [Azure SQL Veri Eşitleme'de şema değişikliklerinin çoğaltılması otomatikleştirin](sql-database-update-sync-schema.md)
- [Mevcut bir eşitleme grubunda eşitleme şemasını güncelleştirmek için PowerShell kullanma](scripts/sql-database-sync-update-schema.md)

### <a name="monitor-and-troubleshoot"></a>İzleme ve sorun giderme

SQL Data Sync beklendiği gibi yapıyor mu? Etkinliği izlemek ve sorunları gidermek için aşağıdaki makalelere bakın:

- [Azure Monitor günlükleriyle Azure SQL Veri Eşitlemesini Izleyin](sql-database-sync-monitor-oms.md)
- [Azure SQL Data Sync ile ilgili sorun giderme](sql-database-troubleshoot-data-sync.md)

### <a name="learn-more-about-azure-sql-database"></a>Azure SQL Veritabanı hakkında daha fazla bilgi edinin

SQL Veritabanı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
- [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
