---
title: Data Sync
description: Azure SQL Data Sync bu genel bakış sunar
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
ms.date: 08/20/2019
ms.openlocfilehash: 1ee2efbb8aebfc2f1a94c89edef6166898946d8a
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422536"
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync"></a>SQL Data Sync ile birden fazla Bulut ve şirket içi veritabanı arasında veri eşitleme

SQL Data Sync, birden çok SQL veritabanları ve SQL Server örnekleri arasında çift seçin verileri eşitleyin olanak sağlayan Azure SQL veritabanı üzerinde oluşturulmuş bir hizmettir.

> [!IMPORTANT]
> Azure SQL Data Sync Şu anda Azure SQL veritabanı yönetilen örneğini desteklemez.

## <a name="when-to-use-data-sync"></a>Veri Eşitleme kullanıldığı durumlar

Veri eşitleme, verilerin birkaç Azure SQL veritabanı veya SQL Server veritabanı genelinde güncelleştirilmesi gereken durumlarda faydalıdır. Veri eşitleme için ana kullanım örnekleri şunlardır:

- **Karma veri eşitleme:** Veri eşitleme ile, karma uygulamaları etkinleştirmek için verileri şirket içi veritabanlarınız ve Azure SQL veritabanları arasında eşitlenmiş halde tutabilirsiniz. Bu özellik, buluta geçiş düşünüyorsanız ve bazı uygulamalarını Azure'da koymak istediğiniz müşterileri itiraz.
- **Dağıtılmış uygulamalar:** Birçok durumda, farklı veritabanları arasında farklı iş yüklerini ayırmak yararlı olur. Örneğin, bir büyük üretim veritabanına sahip, ancak Ayrıca bu veriler üzerinde raporlama veya Analiz iş yükü çalıştırmak gerekir, bu ek iş yükü için ikinci bir veritabanı yararlıdır. Bu yaklaşım, üretim iş yükünüz üzerindeki performans etkisini en aza indirir. Veri eşitleme, bu iki veritabanı eşitlenmiş tutmak için kullanabilirsiniz.
- **Küresel olarak dağıtılan uygulamalar:** Birçok işletme birçok bölgeye ve hatta birkaç ülkeye/bölgeye yayılamaz. Ağ gecikmesini en aza indirmek için size yakın bir bölgede verilerinizin sağlamak en iyisidir. Data Sync ile eşitlenmiş dünyanın dört bir yanındaki bölgelerdeki veritabanlarına kolayca tutabilirsiniz.

Veri eşitleme, aşağıdaki senaryolar için tercih edilen çözüm değildir:

| Senaryo | Bazı önerilen çözümler |
|----------|----------------------------|
| Olağanüstü Durum Kurtarma | [Azure coğrafi olarak yedekli yedeklemeler](sql-database-automated-backups.md) |
| Okuma ölçeği | [Salt okunurdur ve salt okuma sorgu iş yüklerinin yükünü dengelemek için salt okuma çoğaltmaları kullanın (Önizleme)](sql-database-read-scale-out.md) |
| ETL (OLTP OLAP için) | [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) veya [SQL Server Integration Services](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) |
| Şirket içi SQL Server'dan Azure SQL veritabanına geçiş | [Azure Veritabanı Geçiş Hizmeti](https://azure.microsoft.com/services/database-migration/) |
|||

## <a name="overview-of-sql-data-sync"></a>SQL Data Sync genel bakış

Veri eşitleme, bir eşitleme grubu kavramını temel alır. Bir eşitleme grubu, eşitlemek istediğiniz veritabanlarını grubudur.

Veri eşitleme, verileri eşitlemek için bir hub ve bağlı bileşen topolojisi kullanır. Veritabanlarından birini eşitleme grubunda Hub veritabanı tanımlarsınız. Veritabanlarını geri kalanını olan üye veritabanları. Yalnızca Hub ve tek tek üyeleri arasında eşitleme gerçekleşir.

- **Merkez veritabanı** BIR Azure SQL veritabanı olmalıdır.
- **Üye VERITABANLARı** SQL veritabanları, şirket içi SQL Server veritabanları ya da Azure sanal makinelerinde SQL Server örnekleri olabilir.
- **Eşitleme veritabanı** , veri eşitleme için meta verileri ve günlüğü içerir. Eşitleme veritabanı, hub veritabanıyla aynı bölgede bulunan bir Azure SQL veritabanı olmalıdır. Eşitleme müşteri oluşturuldu ve müşterinin sahip olduğu bir veritabanıdır.

> [!NOTE]
> Bir şirket içi veritabanını üye veritabanı olarak kullanıyorsanız, [yerel bir eşitleme Aracısı yükleyip yapılandırmanız](sql-database-get-started-sql-data-sync.md#add-on-prem)gerekir.

![Veritabanları arasında verileri eşitleme](media/sql-database-sync-data/sync-data-overview.png)

Bir eşitleme grubu, aşağıdaki özelliklere sahiptir:

- **Eşitleme şeması** hangi verilerin eşitlendiğini açıklar.
- **Eşitleme yönü** iki yönlü olabilir veya yalnızca bir yönde akabilir. Diğer bir deyişle, eşitleme yönü *hub*veya *hub 'a üye*ya da her ikisi de olabilir.
- Eşitleme **aralığı** , eşitlemenin ne sıklıkla oluştuğunu açıklar.
- **Çakışma çözümleme ilkesi** , *hub WINS* veya *üye WINS*olabilen bir grup düzeyi ilkesidir.

## <a name="how-does-data-sync-work"></a>Veri eşitleme nasıl çalışır?

- **Veri değişikliklerini izleme:** Veri eşitleme, INSERT, Update ve DELETE tetikleyicilerini kullanarak değişiklikleri izler. Bir kullanıcı veritabanındaki bir yan tablodaki değişiklikler kaydedilir. BULK INSERT varsayılan olarak Tetikleyicileri tetikleyeceğini unutmayın. FIRE_TRIGGERS belirtilmemişse, hiçbir ekleme tetikleyicisi yürütülmez. Böylece Data Sync'i Bu eklemeleri izleyebilirsiniz fıre_trıggers seçeneğini ekleyin. 
- **Veriler eşitleniyor:** Veri eşitleme, hub ve bağlı bileşen modelinde tasarlanmıştır. Hub'ı ayrı ayrı her bir üyeyi eşitler. Üye değişiklikleri Hub'ından indirilir ve hub'a üye değişikliklerden sonra yüklenir.
- **Çakışmalar çözümleniyor:** Veri eşitleme, çakışma çözümü, *Merkez WINS* veya *üye WINS*için iki seçenek sunar.
  - *Merkez WINS*' i seçerseniz, hub 'daki değişiklikler her zaman üyenin değişikliklerinin üzerine yazar.
  - *Üye WINS*' i seçerseniz, üyedeki değişiklikler hub 'daki değişiklikler üzerine yazılır. Birden fazla üyesi ise, hangi üye eşitler son değeri bağlıdır.

## <a name="compare-data-sync-with-transactional-replication"></a>Veri eşitlemesini Işlemsel çoğaltma ile karşılaştırın

| | Data Sync | İşlem Çoğaltması |
|---|---|---|
| Yararları | -Etkin-etkin destek<br/>-Şirket içi ve Azure SQL veritabanı arasında çift yönlü | -Düşük gecikme süresi<br/>-İşlemsel tutarlılık<br/>-Geçişten sonra var olan topolojiyi yeniden kullan |
| Olumsuz | -5 dk veya daha fazla gecikme<br/>-İşlem tutarlılığı yok<br/>-Daha yüksek performans etkisi | -Azure SQL veritabanı tek veritabanı veya havuza alınmış veritabanından yayımlanamıyor<br/>-Yüksek bakım maliyeti |

## <a name="get-started-with-sql-data-sync"></a>SQL Data Sync'yi kullanmaya başlayın

### <a name="set-up-data-sync-in-the-azure-portal"></a>Azure portalında Data Sync'i Ayarla

- [Azure SQL Data Sync’i ayarlama](sql-database-get-started-sql-data-sync.md)
- Veri eşitleme Aracısı- [Azure SQL Data Sync Için veri eşitleme Aracısı](sql-database-data-sync-agent.md)

### <a name="set-up-data-sync-with-powershell"></a>PowerShell ile Data Sync'i ayarlama

- [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
- [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)

### <a name="review-the-best-practices-for-data-sync"></a>Data Sync için en iyi uygulamaları gözden geçirin

- [Azure SQL Data Sync için en iyi yöntemler](sql-database-best-practices-data-sync.md)

### <a name="did-something-go-wrong"></a>Bir sorun oluştu

- [Azure SQL Data Sync ile ilgili sorun giderme](sql-database-troubleshoot-data-sync.md)

## <a name="consistency-and-performance"></a>Tutarlılık ve performans

### <a name="eventual-consistency"></a>Nihai tutarlılık

Veri eşitleme, tetikleyici tabanlı olduğundan, işlemsel tutarlılık garanti edilmez. Microsoft, tüm değişikliklerin sonunda yapıldığından ve veri eşitlemenin veri kaybına neden olmadığı garantisi verir.

### <a name="performance-impact"></a>Performans etkisi

Veri Eşitleme kullanır ekleyin, güncelleştirin ve değişiklikleri izlemek için Tetikleyiciler silin. Değişiklik izleme için kullanıcı veritabanında tablolar oluşturur. Bu değişiklik izleme etkinliklerin veritabanı iş yükünüz etkisi. Hizmet katmanınızı değerlendirmek ve gerekirse yükseltme.

Sağlama ve eşitleme grubu oluşturma, güncelleştirme ve silme işlemi sırasında sağlamayı kaldırma, veritabanı performansını da etkileyebilir.

## <a name="sync-req-lim"></a>Gereksinimler ve sınırlamalar

### <a name="general-requirements"></a>Genel gereksinimler

- Her tabloda bir birincil anahtarı olmalıdır. Herhangi bir satırın birincil anahtarı değerini değiştirmeyin. Bir birincil anahtar değerini değiştirmeniz gerekiyorsa, satır silin ve yeni birincil anahtar değeriyle yeniden oluşturun.

> [!IMPORTANT]
> Mevcut bir birincil anahtarın değerini değiştirmek aşağıdaki hatalı davranışa neden olur:
> - Eşit bir sorun bildirmese de, hub ve üye arasındaki veriler kaybolabilir.
> - Birincil anahtar değişikliği nedeniyle izleme tablosu kaynaktan mevcut olmayan bir satır içerdiğinden eşitleme başarısız olabilir.

- Anlık görüntü yalıtımı etkinleştirilmiş olması gerekir. Daha fazla bilgi için bkz. [SQL Server'da Anlık Görüntü Yalıtımı](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server).

### <a name="general-limitations"></a>Genel sınırlamalar

- Tablo, birincil anahtar olmayan bir kimlik sütununa sahip olamaz.
- Birincil anahtar şu veri türlerine sahip olamaz: sql_variant, binary, varbinary, Image, XML.
- Yalnızca desteklenen duyarlık olduğu için birincil anahtar olarak, aşağıdaki veri türleri kullanırken dikkatli olun: zaman, datetime, datetime2, datetimeoffset.
- Nesnelerin (veritabanları, tablolar ve sütunlar) adları, yazdırılabilir karakterler (.), sol köşeli ayraç ([) veya sağ köşeli ayraç (]) içeremez.
- Azure Active Directory kimlik doğrulaması desteklenmiyor.
- Aynı ada ancak farklı şemaya (örneğin, dbo. Customers ve Sales. Customers) sahip tablolar desteklenmez.
- Kullanıcı tanımlı veri türleri olan sütunlar desteklenmiyor

#### <a name="unsupported-data-types"></a>Desteklenmeyen veri türleri

- FILESTREAM
- SQL/CLR UDT
- XMLSchemaCollection (desteklenen XML)
- İmleç, RowVersion, zaman damgası, Hierarchyid

#### <a name="unsupported-column-types"></a>Desteklenmeyen sütun türü

Veri eşitleme, salt okunur veya sistem tarafından oluşturulan sütunları eşitleme yapılamıyor. Örneğin:

- Hesaplanan sütunlar.
- Zamana bağlı tablolar için sistem tarafından oluşturulan sütunları.

#### <a name="limitations-on-service-and-database-dimensions"></a>Hizmet ve veritabanı boyut sınırlamaları

| **Boyutlarına**                                                  | **Sınır**              | **Geçici çözüm**              |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| Eşitleme grubu sayısı için herhangi bir veritabanına ait olabilir.       | 5                      |                             |
| Bir tek eşitleme grubundaki uç noktaları sayısı              | 30                     |                             |
| Şirket içi Uç noktalara tek bir eşitleme grubunda en fazla sayısı. | 5                      | Birden çok eşitleme grubu oluşturma |
| Veritabanı, tablo, şema ve sütun adları                       | ad başına 50 karakter |                             |
| Bir eşitleme grubunda tabloları                                          | 500                    | Birden çok eşitleme grubu oluşturma |
| Bir eşitleme grubunda bir tablodaki sütunları                              | 1000                   |                             |
| Bir tablodaki verileri satır boyutu                                        | 24 mb                  |                             |
| En düşük eşitleme aralığı                                           | 5 Dakika              |                             |

> [!NOTE]
> Olabilir en fazla 30 uç noktaları bir tek eşitleme grubunda yoksa yalnızca bir eşitleme grubu. Birden fazla eşitleme grubu varsa, uç noktalar genelinde tüm eşitleme gruplarını toplam sayısı 30 aşamaz. Bir veritabanının birden çok eşitleme gruplarına ait birden fazla uç noktası, bir geçersiz sayılır.

## <a name="faq-about-sql-data-sync"></a>SQL Data Sync hakkında SSS

### <a name="how-much-does-the-sql-data-sync-service-cost"></a>SQL Data Sync hizmeti maliyeti ne kadar sürer?

SQL Data Sync hizmetin kendisi için ücret alınmaz. Ancak, SQL veritabanı örneğiniz için ve dışında veri taşıma ücretleri de toplaırsınız. Daha fazla bilgi için bkz. [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

### <a name="what-regions-support-data-sync"></a>Veri eşitlemesini destekleyen bölgeler

SQL Data Sync tüm bölgelerde kullanılabilir.

### <a name="is-a-sql-database-account-required"></a>Bir SQL veritabanı hesabı gereklidir

Evet. Hub veritabanı barındırmak için bir SQL veritabanı hesabınızın olması gerekir.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a>Yalnızca şirket içi SQL Server veritabanları arasında eşitleme yapmak için veri eşitlemeyi kullanabilir miyim

Doğrudan yönetilemez. SQL Server şirket içi veritabanı arasında dolaylı olarak, ancak Azure Hub veritabanı oluşturarak ve sonra şirket içi veritabanları eşitleme grubuna ekleyerek eşitleyebilirsiniz.

### <a name="can-i-use-data-sync-to-sync-between-sql-databases-that-belong-to-different-subscriptions"></a>Farklı aboneliklere ait olan SQL veritabanları arasında eşitleme yapmak için veri eşitleme kullanabilir miyim

Evet. Kaynak grupları tarafından farklı aboneliklere ait ait bir SQL veritabanı arasında eşitleyebilirsiniz.

- Aboneliklerin aynı kiracıda ve tüm abonelikleri iznine sahip, eşitleme grubunun Azure portalında yapılandırabilirsiniz.
- Aksi takdirde, farklı aboneliklere ait eşitleme üyesi ekleme için PowerShell kullanmanız gerekir.

### <a name="can-i-use-data-sync-to-sync-between-sql-databases-that-belong-to-different-clouds-like-azure-public-cloud-and-azure-china-21vianet"></a>Farklı bulutlara ait SQL veritabanları arasında eşitleme yapmak için veri eşitleme kullanabilir miyim (Azure genel bulutu ve Azure Çin 21Vianet gibi)

Evet. Farklı bulutlara ait bir SQL veritabanı arasında eşitleme, farklı aboneliklere ait eşitleme üyesi ekleme için PowerShell kullanmanız gerekir.

### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-sync-them"></a>Veri eşitlemesini, üretim veritabanından boş bir veritabanına veri kaynağı için kullanabilir miyim ve sonra bunları eşitleyebilirsiniz

Evet. Şema el ile özgün komut dosyası tarafından yeni veritabanı oluşturun. Şema oluşturduktan sonra tabloları, verileri kopyalayın ve eşitlenmiş kalmasını sağlamak için bir eşitleme grubuna ekleyin.

### <a name="should-i-use-sql-data-sync-to-back-up-and-restore-my-databases"></a>Veritabanlarınızı yedeklemek ve geri yüklemek için SQL Data Sync kullanmalıyım

Verilerinizin yedeğini oluşturmak için SQL Data Sync kullanılması önerilmez. SQL Data Sync eşitlemeler sürümü oluşturulmadığından belirli bir zaman noktasına yedekleme ve geri yükleme yapamazsınız. Ayrıca, SQL Data Sync saklı yordamlar gibi diğer SQL nesnelerini de yedeklememesini ve geri yükleme işleminin hemen eşdeğerini yapmaz.

Önerilen bir yedekleme tekniği için bkz. [Azure SQL veritabanını kopyalama](sql-database-copy.md).

### <a name="can-data-sync-sync-encrypted-tables-and-columns"></a>Veri eşitleme eşitleme şifrelenmiş tabloları ve sütunları

- Bir veritabanı Always Encrypted kullanıyorsa, *yalnızca şifrelenmemiş tabloları* ve sütunları eşitleyebilirsiniz. Veri Eşitleme verilerin şifresini çözemez için şifrelenmiş sütunları eşitleyemiyor.
- Bir sütun sütun düzeyinde şifrelemeyi (CLE) kullanıyorsa, satır boyutu 24 MB maksimum boyuttan daha az olduğu sürece sütun eşitleyebilirsiniz. Veri eşitleme, normal bir ikili veri olarak (CLE) anahtar ile şifrelenmiş sütun değerlendirir. Diğer eşitleme üyeler verilerin şifresini çözmek için aynı sertifika olması gerekir.

### <a name="is-collation-supported-in-sql-data-sync"></a>SQL Data Sync içinde harmanlama destekleniyor

Evet. SQL Data Sync, aşağıdaki senaryolarda harmanlamasını destekler:

- Seçili eşitleme şeması tabloları hub 'ınızda veya üye veritabanlarında yoksa, eşitleme grubunu dağıttığınızda, hizmet otomatik olarak ilgili tabloları ve sütunları boş hedef veritabanlarında seçilen harmanlama ayarları ile birlikte oluşturur.
- Zaten eşitlenmiş tabloları hub ve üye veritabanlarında varsa, SQL Data Sync birincil anahtar sütunlarını eşitleme grubu başarıyla dağıtmak için hub ve üye veritabanları arasında aynı harmanlamaya sahip olmasını gerektirir. Sütunlarda birincil anahtar sütunları dışındaki harmanlama kısıtlama yoktur.

### <a name="is-federation-supported-in-sql-data-sync"></a>SQL Data Sync sürümünde Federasyon destekleniyor

Federasyon kök veritabanı, SQL veri eşitleme hizmetinde hiçbir sınırlama olmadan kullanılabilir. Federasyon veritabanı uç noktasını geçerli SQL Data Sync sürümüne ekleyemezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="update-the-schema-of-a-synced-database"></a>Eşitlenen bir veritabanı şemasını güncelleştirme

Bir eşitleme grubunda bir veritabanının şemasını güncelleştirmek zorunda mıyım? Şema değişiklikleri otomatik olarak çoğaltılmaz. Bazı çözümler için aşağıdaki makalelere bakın:

- [Azure SQL Data Sync şema değişikliklerinin çoğaltılmasını otomatikleştirin](sql-database-update-sync-schema.md)
- [Mevcut bir eşitleme grubundaki eşitleme şemasını güncelleştirmek için PowerShell 'i kullanma](scripts/sql-database-sync-update-schema.md)

### <a name="monitor-and-troubleshoot"></a>İzleme ve sorun giderme

SQL Data Sync beklendiği gibi yapılıyor mu? Etkinlik izleme ve sorunlarını gidermek için aşağıdaki makalelere bakın:

- [Azure Izleyici günlükleri ile Azure SQL Data Sync izleme](sql-database-sync-monitor-oms.md)
- [Azure SQL Data Sync ile ilgili sorun giderme](sql-database-troubleshoot-data-sync.md)

### <a name="learn-more-about-azure-sql-database"></a>Azure SQL Veritabanı hakkında daha fazla bilgi edinin

SQL veritabanı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
- [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
