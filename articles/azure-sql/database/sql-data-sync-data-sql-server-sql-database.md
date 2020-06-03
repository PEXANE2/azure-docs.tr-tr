---
title: Azure için SQL Data Sync nedir?
description: Bu genel bakış, Azure için SQL Data Sync sunarak verileri birden çok bulutta ve şirket içi veritabanlarında eşitlemenize olanak tanır.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync, sqldbrb=1, fasttrack-edit
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 08/20/2019
ms.openlocfilehash: c2c0e6d1d3ffd9ec3091e92530ec5c191f3f7ca6
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84297964"
---
# <a name="what-is-sql-data-sync-for-azure"></a>Azure için SQL Data Sync nedir?
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

SQL Data Sync, hem şirket içinde hem de bulutta birden çok veritabanı üzerinde çift yönlü olarak seçtiğiniz verileri eşitlemenize olanak sağlayan Azure SQL veritabanı üzerinde oluşturulmuş bir hizmettir. 

> [!IMPORTANT]
> Azure SQL Data Sync Şu anda Azure SQL yönetilen örneğini desteklemez.


## <a name="overview"></a>Genel Bakış 

Veri eşitleme, bir eşitleme grubu kavramını temel alarak. Eşitleme grubu, eşitlemek istediğiniz veritabanlarının bir grubudur.

Veri eşitleme, verileri eşitlemek için bir hub ve bağlı bileşen topolojisi kullanır. Eşitleme grubundaki veritabanlarından birini hub veritabanı olarak tanımlarsınız. Veritabanlarının geri kalanı üye veritabanlarıdır. Eşitleme yalnızca Hub ve bireysel Üyeler arasında gerçekleşir.

- **Merkez veritabanı** BIR Azure SQL veritabanı olmalıdır.
- **Üye veritabanları** , Azure SQL veritabanı 'nda ya da SQL Server örneklerinde veritabanları olabilir.
- **Eşitleme veritabanı** , veri eşitleme için meta verileri ve günlüğü içerir. Eşitleme veritabanı, hub veritabanıyla aynı bölgede bulunan bir Azure SQL veritabanı olmalıdır. Eşitleme veritabanı müşteri tarafından oluşturulur ve müşterinin sahibi olur.

> [!NOTE]
> Bir şirket içi veritabanını üye veritabanı olarak kullanıyorsanız, [yerel bir eşitleme Aracısı yükleyip yapılandırmanız](sql-data-sync-sql-server-configure.md#add-on-prem)gerekir.

![Veritabanları arasında verileri eşitleme](./media/sql-data-sync-data-sql-server-sql-database/sync-data-overview.png)

Bir eşitleme grubu aşağıdaki özelliklere sahiptir:

- **Eşitleme şeması** hangi verilerin eşitlendiğini açıklar.
- **Eşitleme yönü** iki yönlü olabilir veya yalnızca bir yönde akabilir. Diğer bir deyişle, eşitleme yönü *hub*veya *hub 'a üye*ya da her ikisi de olabilir.
- Eşitleme **aralığı** , eşitlemenin ne sıklıkla oluştuğunu açıklar.
- **Çakışma çözümleme ilkesi** , *hub WINS* veya *üye WINS*olabilen bir grup düzeyi ilkesidir.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

Veri eşitleme, verilerin Azure SQL veritabanı veya SQL Server çeşitli veritabanları arasında güncelleştirilmesi gereken durumlarda yararlıdır. Veri eşitlemeye yönelik başlıca kullanım durumları aşağıda verilmiştir:

- **Karma veri eşitleme:** Veri eşitleme ile, karma uygulamaları etkinleştirmek için SQL Server ve Azure SQL veritabanı 'nda veritabanlarınız arasında verileri eşitlenmiş halde tutabilirsiniz. Bu özellik, buluta geçmeyi düşünen ve Azure 'a bazı uygulamaları yerleştirmek istediğiniz müşterileri ele alabilir.
- **Dağıtılmış uygulamalar:** Birçok durumda, farklı veritabanları arasında farklı iş yüklerini ayırmak yararlı olur. Örneğin, büyük bir üretim veritabanınız varsa, ancak bu verilerde bir raporlama veya analiz iş yükü çalıştırmanız gerekiyorsa, bu ek iş yükü için ikinci bir veritabanı olması yararlı olacaktır. Bu yaklaşım, üretim iş yükünüzün performans etkisini en aza indirir. Bu iki veritabanını eşitlenmiş halde tutmak için veri eşitleme kullanabilirsiniz.
- **Küresel olarak dağıtılan uygulamalar:** Birçok işletme birçok bölgeye ve hatta birkaç ülkeye/bölgeye yayılamaz. Ağ gecikmesini en aza indirmek için, verilerinizin size yakın bir bölgede olması en iyisidir. Veri eşitleme ile, dünyanın dört bir yanındaki bölgelerde veritabanlarını kolayca koruyabilirsiniz.

Veri eşitleme, aşağıdaki senaryolar için tercih edilen çözüm değildir:

| Senaryo | Önerilen bazı çözümler |
|----------|----------------------------|
| Olağanüstü Durum Kurtarma | [Azure coğrafi olarak yedekli yedeklemeler](automated-backups-overview.md) |
| Ölçeği oku | [Salt okunurdur ve salt okuma sorgu iş yüklerinin yükünü dengelemek için salt okuma çoğaltmaları kullanın (Önizleme)](read-scale-out.md) |
| ETL (OLTP-OLAP) | [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) veya [SQL Server Integration Services](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) |
| SQL Server 'den Azure SQL veritabanı 'na geçiş | [Azure Veritabanı Geçiş Hizmeti](https://azure.microsoft.com/services/database-migration/) |
|||



## <a name="how-it-works"></a>Nasıl çalışır?

- **Veri değişikliklerini izleme:** Veri eşitleme, INSERT, Update ve DELETE tetikleyicilerini kullanarak değişiklikleri izler. Değişiklikler, Kullanıcı veritabanındaki bir yan tabloya kaydedilir. BULK INSERT varsayılan olarak Tetikleyicileri tetikleyeceğini unutmayın. FIRE_TRIGGERS belirtilmemişse, hiçbir ekleme tetikleyicisi yürütülmez. Veri eşitleme 'nin Bu eklemeleri izleyebilmesi için FIRE_TRIGGERS seçeneğini ekleyin. 
- **Veriler eşitleniyor:** Veri eşitleme, hub ve bağlı bileşen modelinde tasarlanmıştır. Hub her üyeyle tek tek eşitlenir. Hub 'daki değişiklikler üyeye indirilir ve Üyeden yapılan değişiklikler hub 'a yüklenir.
- **Çakışmalar çözümleniyor:** Veri eşitleme, çakışma çözümü, *Merkez WINS* veya *üye WINS*için iki seçenek sunar.
  - *Merkez WINS*' i seçerseniz, hub 'daki değişiklikler her zaman üyenin değişikliklerinin üzerine yazar.
  - *Üye WINS*' i seçerseniz, üyedeki değişiklikler hub 'daki değişiklikler üzerine yazılır. Birden fazla üye varsa, son değer öncelikle hangi üyenin eşitlendiği üzerinde değişir.

## <a name="compare-with-transactional-replication"></a>Işlemsel çoğaltma ile karşılaştırın

| | Data Sync | İşlem Çoğaltması |
|---|---|---|
| Yararları | -Etkin-etkin destek<br/>-Şirket içi ve Azure SQL veritabanı arasında çift yönlü | -Düşük gecikme süresi<br/>-İşlemsel tutarlılık<br/>-Geçişten sonra var olan topolojiyi yeniden kullan <br/>-Azure SQL yönetilen örnek desteği |
| Dezavantajlar | -5 dk veya daha fazla gecikme<br/>-İşlem tutarlılığı yok<br/>-Daha yüksek performans etkisi | -Azure SQL veritabanından yayımlanamıyor <br/>-Yüksek bakım maliyeti |

## <a name="get-started"></a>Kullanmaya başlayın 

### <a name="set-up-data-sync-in-the-azure-portal"></a>Azure portal veri eşitlemesini ayarlama

- [Azure SQL Data Sync’i ayarlama](sql-data-sync-sql-server-configure.md)
- Veri eşitleme Aracısı- [Azure SQL Data Sync Için veri eşitleme Aracısı](sql-data-sync-agent-overview.md)

### <a name="set-up-data-sync-with-powershell"></a>PowerShell ile veri eşitlemesini ayarlama

- [Azure SQL veritabanı 'nda birden çok veritabanı arasında eşitleme yapmak için PowerShell 'i kullanma](scripts/sql-data-sync-sync-data-between-sql-databases.md)
- [PowerShell kullanarak Azure SQL veritabanındaki bir veritabanı ile bir SQL Server örneğindeki veritabanları arasında eşitleme](scripts/sql-data-sync-sync-data-between-azure-onprem.md)

### <a name="review-the-best-practices-for-data-sync"></a>Veri eşitleme için en iyi uygulamaları gözden geçirin

- [Azure SQL Data Sync için en iyi yöntemler](sql-data-sync-best-practices.md)

### <a name="did-something-go-wrong"></a>Bir sorun oluştu

- [Azure SQL Data Sync ile ilgili sorun giderme](../../sql-database/sql-database-troubleshoot-data-sync.md)

## <a name="consistency-and-performance"></a>Tutarlılık ve performans

### <a name="eventual-consistency"></a>Nihai tutarlılık

Veri eşitleme, tetikleyici tabanlı olduğundan, işlemsel tutarlılık garanti edilmez. Microsoft, tüm değişikliklerin sonunda yapıldığından ve veri eşitlemenin veri kaybına neden olmadığı garantisi verir.

### <a name="performance-impact"></a>Performans etkisi

Veri eşitleme, değişiklikleri izlemek için INSERT, Update ve DELETE tetikleyicilerini kullanır. Değişiklik izleme için Kullanıcı veritabanında yan tablolar oluşturur. Bu değişiklik izleme etkinliklerinin veritabanı iş yükünüz üzerinde bir etkisi vardır. Hizmet katmanınızı değerlendirin ve gerekirse yükseltin.

Eşitleme grubu oluşturma, güncelleştirme ve silme sırasında sağlama ve sağlamayı kaldırma, veritabanı performansını da etkileyebilir.

## <a name="requirements-and-limitations"></a><a name="sync-req-lim"></a>Gereksinimler ve sınırlamalar

### <a name="general-requirements"></a>Genel gereksinimler

- Her tablo bir birincil anahtara sahip olmalıdır. Herhangi bir satırdaki birincil anahtar değerini değiştirmeyin. Birincil anahtar değerini değiştirmeniz gerekiyorsa, satırı silin ve yeni birincil anahtar değeriyle yeniden oluşturun.

> [!IMPORTANT]
> Mevcut bir birincil anahtarın değerini değiştirmek aşağıdaki hatalı davranışa neden olur:
> - Eşit bir sorun bildirmese de, hub ve üye arasındaki veriler kaybolabilir.
> - Birincil anahtar değişikliği nedeniyle izleme tablosu kaynaktan mevcut olmayan bir satır içerdiğinden eşitleme başarısız olabilir.

- Anlık görüntü yalıtımı etkinleştirilmelidir. Daha fazla bilgi için bkz. [SQL Server'da Anlık Görüntü Yalıtımı](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server).

### <a name="general-limitations"></a>Genel sınırlamalar

- Tablo, birincil anahtar olmayan bir kimlik sütununa sahip olamaz.
- Birincil anahtar şu veri türlerine sahip olamaz: sql_variant, binary, varbinary, Image, XML.
- Aşağıdaki veri türlerini birincil anahtar olarak kullanırken dikkatli olun, çünkü desteklenen duyarlık yalnızca ikinci-saat, DateTime, datetime2, DateTimeOffset olur.
- Nesnelerin (veritabanları, tablolar ve sütunlar) adları, yazdırılabilir karakterler (.), sol köşeli ayraç ([) veya sağ köşeli ayraç (]) içeremez.
- Azure Active Directory kimlik doğrulaması desteklenmiyor.
- Aynı ada ancak farklı şemaya (örneğin, dbo. Customers ve Sales. Customers) sahip tablolar desteklenmez.
- Kullanıcı tanımlı veri türleri olan sütunlar desteklenmiyor
- Sunucuları farklı abonelikler arasında taşıma desteklenmez. 

#### <a name="unsupported-data-types"></a>Desteklenmeyen veri türleri

- Akışı
- SQL/CLR UDT
- XMLSchemaCollection (XML destekleniyor)
- İmleç, RowVersion, timestamp, HierarchyId

#### <a name="unsupported-column-types"></a>Desteklenmeyen sütun türleri

Veri eşitleme, salt okuma veya sistem tarafından oluşturulmuş sütunları eşitleyemiyor. Örneğin:

- Hesaplanan sütunlar.
- Zamana bağlı tablolar için sistem tarafından oluşturulan sütunlar.

#### <a name="limitations-on-service-and-database-dimensions"></a>Hizmet ve veritabanı boyutlarına ilişkin sınırlamalar

| **Boyutlarına**                                                  | **Sınırlı**              | **Sorunu**              |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| Herhangi bir veritabanının ait olduğu en fazla eşitleme grubu sayısı.       | 5                      |                             |
| Tek bir eşitleme grubundaki en fazla uç nokta sayısı              | 30                     |                             |
| Tek bir eşitleme grubundaki en fazla şirket içi uç nokta sayısı. | 5                      | Birden çok eşitleme grubu oluşturma |
| Veritabanı, tablo, şema ve sütun adları                       | ad başına 50 karakter |                             |
| Bir eşitleme grubundaki tablolar                                          | 500                    | Birden çok eşitleme grubu oluşturma |
| Bir eşitleme grubundaki tablodaki sütunlar                              | 1000                   |                             |
| Tablodaki veri satırı boyutu                                        | 24 MB                  |                             |
| En az eşitleme aralığı                                           | 5 dakika              |                             |

> [!NOTE]
> Yalnızca bir eşitleme grubu varsa, tek bir eşitleme grubunda 30 ' a kadar uç nokta olabilir. Birden fazla eşitleme grubu varsa, tüm eşitleme gruplarındaki bitiş noktalarının toplam sayısı 30 ' u aşamaz. Bir veritabanı birden çok eşitleme grubuna aitse, birden fazla uç nokta olarak sayılır.

### <a name="network-requirements"></a>Ağ gereksinimleri

Eşitleme grubu oluşturulduğunda, veri eşitleme hizmetinin hub veritabanına bağlanması gerekir. Eşitleme grubunu oluşturduğunuzda, Azure SQL Server 'ın ayarlarında aşağıdaki yapılandırma olmalıdır `Firewalls and virtual networks` :

 * *Ortak ağ erişimini reddetme* *devre dışı*olarak ayarlanmalıdır.
 * *Azure hizmetlerinin ve kaynaklarının bu sunucuya erişmesine Izin ver* ' in *Evet*olarak ayarlanması veya [veri eşitleme hizmeti tarafından kullanılan IP adresleri](network-access-controls-overview.md#data-sync)için IP kuralları oluşturmanız gerekir.

Eşitleme grubu oluşturulup sağlandıktan sonra bu ayarları devre dışı bırakabilirsiniz. Eşitleme Aracısı doğrudan hub veritabanına bağlanır ve aracının hub sunucusuna erişmesine izin vermek için sunucunun [güvenlik DUVARı IP kurallarını](firewall-configure.md) veya [Özel uç noktalarını](private-endpoint-overview.md) kullanabilirsiniz.

> [!NOTE]
> Eşitleme grubunun şema ayarlarını değiştirirseniz, hub veritabanının yeniden sağlanması için veri eşitleme hizmeti 'nin sunucuya yeniden erişmesine izin vermeniz gerekir.

## <a name="faq-about-sql-data-sync"></a>SQL Data Sync hakkında SSS

### <a name="how-much-does-the-sql-data-sync-service-cost"></a>SQL Data Sync hizmeti maliyeti ne kadar sürer?

SQL Data Sync hizmetin kendisi için ücret alınmaz. Ancak, SQL veritabanı örneğiniz için ve dışında veri taşıma ücretleri de toplaırsınız. Daha fazla bilgi için bkz. [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

### <a name="what-regions-support-data-sync"></a>Veri eşitlemesini destekleyen bölgeler

SQL Data Sync tüm bölgelerde kullanılabilir.

### <a name="is-a-sql-database-account-required"></a>Bir SQL veritabanı hesabı gereklidir

Evet. Hub veritabanını barındırmak için bir SQL veritabanı hesabınızın olması gerekir.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-databases-only"></a>Yalnızca SQL Server veritabanları arasında eşitleme yapmak için veri eşitlemeyi kullanabilir miyim

Doğrudan değil. Ancak, Azure 'da bir hub veritabanı oluşturup sonra şirket içi veritabanlarını eşitleme grubuna ekleyerek SQL Server veritabanları arasında dolaylı olarak eşitleme yapabilirsiniz.

### <a name="can-i-use-data-sync-to-sync-between-sql-databases-that-belong-to-different-subscriptions"></a>Farklı aboneliklere ait olan SQL veritabanları arasında eşitleme yapmak için veri eşitleme kullanabilir miyim

Evet. Farklı aboneliklere ait kaynak gruplarına ait olan SQL veritabanları arasında eşitleme yapabilirsiniz.

- Abonelikler aynı kiracıya aitse ve tüm abonelikler için izniniz varsa, Azure portal eşitleme grubunu yapılandırabilirsiniz.
- Aksi takdirde, farklı aboneliklere ait olan eşitleme üyelerini eklemek için PowerShell kullanmanız gerekir.

### <a name="can-i-use-data-sync-to-sync-between-sql-databases-that-belong-to-different-clouds-like-azure-public-cloud-and-azure-china-21vianet"></a>Farklı bulutlara ait SQL veritabanları arasında eşitleme yapmak için veri eşitleme kullanabilir miyim (Azure genel bulutu ve Azure Çin 21Vianet gibi)

Evet. Farklı bulutlara ait olan SQL veritabanları arasında eşitleme yapabilirsiniz, farklı aboneliklere ait olan eşitleme üyelerini eklemek için PowerShell kullanmanız gerekir.

### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-sync-them"></a>Veri eşitlemesini, üretim veritabanından boş bir veritabanına veri kaynağı için kullanabilir miyim ve sonra bunları eşitleyebilirsiniz

Evet. Şemayı orijinalden komut dosyasıyla yeni veritabanında el ile oluşturun. Şemayı oluşturduktan sonra, verileri kopyalamak ve eşitlenmiş halde tutmak için tabloları bir eşitleme grubuna ekleyin.

### <a name="should-i-use-sql-data-sync-to-back-up-and-restore-my-databases"></a>Veritabanlarınızı yedeklemek ve geri yüklemek için SQL Data Sync kullanmalıyım

Verilerinizin yedeğini oluşturmak için SQL Data Sync kullanılması önerilmez. SQL Data Sync eşitlemeler sürümü oluşturulmadığından belirli bir zaman noktasına yedekleme ve geri yükleme yapamazsınız. Ayrıca, SQL Data Sync saklı yordamlar gibi diğer SQL nesnelerini de yedeklememesini ve geri yükleme işleminin hemen eşdeğerini yapmaz.

Önerilen bir yedekleme tekniği için bkz. [Azure SQL veritabanını kopyalama](database-copy.md).

### <a name="can-data-sync-sync-encrypted-tables-and-columns"></a>Veri eşitleme eşitleme şifrelenmiş tabloları ve sütunları

- Bir veritabanı Always Encrypted kullanıyorsa, *yalnızca şifrelenmemiş tabloları* ve sütunları eşitleyebilirsiniz. Veri eşitlemesi verilerin şifresini çözemediği için şifrelenmiş sütunları eşitleyemezsiniz.
- Bir sütun, sütun düzeyinde şifreleme (CLE) kullanıyorsa, satır boyutu en fazla 24 MB boyutundan daha az olduğu sürece sütunu eşitleyebilirsiniz. Veri eşitleme, anahtar (CLE) tarafından şifrelenmiş sütunu normal ikili veriler olarak değerlendirir. Diğer eşitleme üyelerdeki verilerin şifresini çözmek için aynı sertifikaya sahip olmanız gerekir.

### <a name="is-collation-supported-in-sql-data-sync"></a>SQL Data Sync içinde harmanlama destekleniyor

Evet. SQL Data Sync, aşağıdaki senaryolarda harmanlamayı destekler:

- Seçili eşitleme şeması tabloları hub 'ınızda veya üye veritabanlarında yoksa, eşitleme grubunu dağıttığınızda, hizmet otomatik olarak ilgili tabloları ve sütunları boş hedef veritabanlarında seçilen harmanlama ayarları ile birlikte oluşturur.
- Eşitlenecek tablolar hem hub 'ınızdaki hem de üye veritabanlarında zaten mevcutsa, SQL Data Sync eşitleme grubunu başarılı bir şekilde dağıtmak için birincil anahtar sütunlarının hub ve üye veritabanları arasında aynı harmanlamaya sahip olmasını gerektirir. Birincil anahtar sütunlarından farklı sütunlarda harmanlama kısıtlaması yok.

### <a name="is-federation-supported-in-sql-data-sync"></a>SQL Data Sync sürümünde Federasyon destekleniyor

Federasyon kök veritabanı SQL Data Sync hizmetinde herhangi bir kısıtlama olmadan kullanılabilir. Federasyon veritabanı uç noktasını geçerli SQL Data Sync sürümüne ekleyemezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="update-the-schema-of-a-synced-database"></a>Eşitlenmiş bir veritabanının şemasını güncelleştirme

Bir eşitleme grubundaki bir veritabanının şemasını güncelleştirmeniz mı gerekiyor? Şema değişiklikleri otomatik olarak çoğaltılmaz. Bazı çözümler için aşağıdaki makalelere bakın:

- [Azure 'da SQL Data Sync şema değişikliklerinin çoğaltılmasını otomatikleştirin](../../sql-database/sql-database-update-sync-schema.md)
- [Mevcut bir eşitleme grubunda eşitleme şemasını güncelleştirmek için PowerShell kullanma](scripts/update-sync-schema-in-sync-group.md)

### <a name="monitor-and-troubleshoot"></a>İzleme ve sorun giderme

SQL Data Sync beklendiği gibi yapılıyor mu? Etkinliği izlemek ve sorunları gidermek için aşağıdaki makalelere bakın:

- [Azure Izleyici günlükleriyle SQL Data Sync izleme](../../sql-database/sql-database-sync-monitor-oms.md)
- [Azure SQL Data Sync ile ilgili sorun giderme](../../sql-database/sql-database-troubleshoot-data-sync.md)

### <a name="learn-more-about-azure-sql-database"></a>Azure SQL veritabanı hakkında daha fazla bilgi

Azure SQL veritabanı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [SQL Veritabanı'na Genel Bakış](sql-database-paas-overview.md)
- [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
