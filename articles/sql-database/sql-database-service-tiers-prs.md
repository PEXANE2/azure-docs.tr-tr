---
title: Premium RS hizmet kademesi emeklilik
description: Premium RS hizmet katmanı kullanımdan kaldırılmakta ve destek sona eriyor - geçiş seçeneklerine bakın.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 02/07/2019
ms.openlocfilehash: f00ecd19877ba6236bde5de73d450967abc1fe15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73821037"
---
# <a name="azure-sql-database-premium-rs-service-tier-preview-is-being-retired---options-for-migration"></a>Azure SQL Database Premium RS hizmet katmanı (önizleme) kullanımdan kaldırıluyor - geçiş seçenekleri

Şubat 2018'de Microsoft, Azure SQL Veritabanı'ndaki Premium RS hizmet katmanının genel kullanılabilirlik için yayımlanmayacağını ve 31 Ocak 2019'dan sonra artık desteklenmeyeceğini duyurdu. Bu son destek süresi 30 Haziran 2019'a uzatıldı. Bu makalede, Premium RS hizmet katmanından başka bir hizmet katmanına geçiş seçenekleriniz açıklanmaktadır. 30 Haziran 2019'dan sonra Microsoft, Premium RS veritabanlarınızı otomatik olarak Premium RS veritabanınızın performans gereksinimleriyle en yakın eşleşen genel olarak kullanılabilen bir hizmet katmanına geçirecektir.

Premium RS müşterileri için uygun olabilecek geçiş noktaları ve fiyatlandırma seçenekleri şunlardır:

- vCore hizmet katmanları

  [VCore tabanlı satın alma modelinde](sql-database-service-tiers-vcore.md) **Genel Amaç** ve **İş Kritik** hizmet katmanları. Bu iki hizmet katmanı genel olarak kullanılabilir durumdadır. vCore tabanlı satın alma modeli, veritabanı başına 100 TB'a kadar otomatik ölçeklendirme yle isteğe bağlı olarak iş yükünüzün ihtiyaçlarına uyum sağlayan **Hyperscale** hizmet katmanını da sunar. Hyperscale hizmet katmanı, [DTU tabanlı satın alma modelindeki](sql-database-service-tiers-dtu.md) Premium hizmet katmanıyla karşılaştırılabilir IO performansı sağlar ve Premium RS hizmet katmanına daha yakın bir fiyatla.
- Geliştirme/Test fiyatlandırması

  [Geliştirme/test fiyatlandırması,](https://azure.microsoft.com/pricing/dev-test/) Visual Studio aboneliğinizde lisans dahil fiyatlara göre %55'e varan tasarruf sağlar.
- Azure Karma Avantajı ve ayrılmış kapasite fiyatlandırması

  [Azure Karma Avantajı ve ayrılmış kapasite fiyatlandırması,](https://azure.microsoft.com/pricing/details/sql-database/) lisans dahil fiyatlara göre %80'e varan tasarruf sağlar. Bu seçenekler hakkında daha fazla bilgi için, SQL Server ve [Azure SQL Veritabanı](sql-database-reserved-capacity.md)için Azure Karma [Avantajı'na](https://azure.microsoft.com/pricing/hybrid-benefit/) bakın.

## <a name="act-now-to-migrate-your-premium-rs-databases-to-alternative-sql-database-service-tiers"></a>Premium RS veritabanlarınızı alternatif SQL Veritabanı hizmet katmanlarına geçirmek için şimdi harekete geçin

Premium RS iş yükleriniz için doğru geçiş hedefini belirlemek için fiyatlandırma ve belgelerimizle birlikte bu makaledeki kılavuzu gözden geçirin.

## <a name="migrate-compute-intensive-workloads-and-save"></a>İşlem yoğun iş yüklerini geçirin ve kaydedin

İşlem yoğun Premium RS iş yükleri için, genel olarak kullanılabilen vCore tabanlı Genel Amaçlı hizmet katmanımıza geçiş yapmanızı ve SQL Server için Azure Karma Avantajı ve ayrılmış kapasite tekliflerini kullanarak lisans dahil edilen fiyatlara göre daha fazla tasarruf etmenizi öneririz. DTU tabanlı satın alma seçeneğinde kalmayı tercih ederseniz, işlem yoğun Premium RS veritabanlarınızı standart bir hizmet katmanına geçirebilir ve yine de Premium RS genel kullanılabilirlik fiyatlandırmasına göre kaydedebilirsiniz (genel kullanılabilirliğe uysaydı).

> [!WARNING]
> Premium RS iş yüklerinizi DTU tabanlı Premium hizmet katmanlarına geçirmek, geçerli Premium RS fiyatlandırmasına göre aylık maliyetleri artırabilir. Premium RS'den benzer veya daha düşük maliyetleri korumak için Azure Hibrit Avantajı ve ayrılmış kapasite fiyatlandırması ile Hiper ölçek veya İş Açısından Kritik katmanları göz önünde bulundurmanızı öneririz.

### <a name="premium-rs-databases"></a>Premium RS veritabanları

|**Şu anda üzerinde iseniz ...**|**Karşılaştırılabilir vCore tabanlı geçirin ...**|**Karşılaştırılabilir DTU tabanlı geçiş ...**|
|---|---|---|
|Premium RS 1|Genel Amaçlı 1 vCore (Gen4)|Standart 3|
|Premium RS 2|Genel Amaçlı 2 vCores (Gen4)|Standart 4|
|Premium RS 4|Genel Amaçlı 4 vCores (Gen4)|Standart 6|
|Premium RS 6|Genel Amaçlı 6 vCores (Gen4)|Standart 7|

### <a name="premium-rs-pools"></a>Premium RS havuzları

|**Şu anda üzerinde iseniz ...**|**Karşılaştırılabilir vCore tabanlı geçirin ...**|**Karşılaştırılabilir DTU tabanlı geçiş ...**|
|---|---|---|
|Premium RS havuzu 125 DTU|Genel Amaçlı 1 vCore (Gen4)|Standart havuz 100 eDTUs|
|Premium RS havuzu 250 DTU|Genel Amaçlı 2 vCores (Gen4)|Standart havuz 250 eDTUs|
|Premium RS havuzu 500 DTU|Genel Amaçlı 4 vCores (Gen4)|Standart havuz 500 eDTUs|
|Premium RS havuzu 1000 DTU|Genel Amaçlı 8 vCores (Gen4)|Standart havuz 1000 eDTUs|

## <a name="optimize-savings-and-performance-for-your-io-intensive-workloads"></a>IO yoğun iş yükleri için tasarruf ve performansı optimize edin

IO yoğun tek veritabanlarınızı, şu anda önizlemede olan vCore tabanlı Hiper ölçek katmanımıza ve IO yoğun veritabanı havuzlarınızı performans ve maliyetin en iyi kombinasyonu için genel olarak kullanılabilir Business Critical katmanımıza geçirmenizi öneririz.  Aşağıdaki vCore tabanlı seçenekler mevcut performansınızı koruyacak veya geliştirecek ve Azure Karma Avantajı ve ayrılmış kapasite fiyatlandırması ile birleştirildiğinde paradan tasarruf etmenizi sağlayabilir.

|**Şu anda üzerinde iseniz ...**|**Karşılaştırılabilir vCore tabanlı geçirin ...**|**Karşılaştırılabilir DTU tabanlı geçiş ...**|
|---|---|---|
|Premium RS 1| Hyperscale 1 vCore (Gen4) veya Business Critical 1 vCore (Gen4)|Premium 1|
|Premium RS 2| Hyperscale 2 vCores (Gen4) veya Business Critical 2 vCores (Gen4|Premium 2|
|Premium RS 4| Hyperscale 4 vCores (Gen4) veya Business Critical 4 vCores (Gen4)|Premium 4
|Premium RS 6| Hyperscale 6 vCores (Gen4) veya Business Critical 6 vCores (Gen4)|Premium 6|

|**Şu anda üzerinde iseniz ...**|**Karşılaştırılabilir vCore tabanlı geçirin ...**|**Karşılaştırılabilir DTU tabanlı geçiş ...**|
|---|---|---|
|Premium RS havuzu 125 DTU|İş Kritik 2 vCores (Gen4)|Premium havuz 125 eDTUs|
|Premium RS havuzu 250 DTU|İş Kritik 2 vCores (Gen4)|Premium havuz 250 eDTUs|
|Premium RS havuzu 500 DTU|İş Kritik 4 vCores (Gen4)|Premium havuz 500 eDTUs|
|Premium RS havuzu 1000 DTU|İş Kritik 8 vCores (Gen4)|Premium havuz 1000 eDTUs|

## <a name="take-advantage-of-our-new-offers"></a>Yeni tekliflerimizden yararlanın

vCore tabanlı satın alma modelindeki hizmet katmanlarımız, lisans dahil fiyatlandırmaya göre %80'e kadar tasarruf etmenizi sağlayan özel teklifler için uygundur. SQL Server Için [Azure Karma Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/)ile lisansdahil fiyatlandırmaya karşılık %55'e kadar tasarruf sağlamak için etkin Yazılım Güvencesi ile SQL Server Standard veya Enterprise sürüm lisanslarınızı kullanın. Karma avantajı [Azure SQL Veritabanı ayrılmış kapasite](sql-database-reserved-capacity.md) fiyatlandırmasıyla birleştirebilir ve bir veya üç yıllık bir dönem için ön taahhütte bulunarak %80'e kadar tasarruf edebilirsiniz.  Azure portalından her iki avantajı da bugün etkinleştirin.

Bu değişiklikle ilgili herhangi bir sorunuz veya endişeniz varsa veya geçiş yardımına ihtiyaç duyarsanız, [Microsoft'a](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)başvurun.

## <a name="migration-from-a-premium-rs-service-tier-to-a-service-tier-in-either-the-dtu-or-the-vcore-model"></a>DTU veya vCore modelinde Premium RS hizmet katmanından hizmet katmanına geçiş

### <a name="migration-of-a-database"></a>Veritabanının geçişi

Bir veritabanını Premium RS hizmet katmanından DTU veya vCore modelindeki bir hizmet katmanına geçirmek, Premium RS hizmet katmanındaki hizmet katmanları arasında yükseltme veya düşürmeye benzer.

### <a name="using-database-copy-to-convert-a-premium-rs-database-to-a-dtu-based-or-vcore-based-database"></a>Premium RS veritabanını DTU tabanlı veya vCore tabanlı veritabanına dönüştürmek için veritabanı kopyasını kullanma

Hedef işlem boyutu kaynak veritabanının maksimum veritabanı boyutunu desteklediği sürece, Premium RS işlem boyutuna sahip herhangi bir veritabanını, kısıtlama lar veya özel sıralama olmaksızın DTU tabanlı veya vCore tabanlı işlem boyutuna sahip bir veritabanına kopyalayabilirsiniz. Veritabanı kopyası, kopyalama işleminin başlangıç saati itibariyle verilerin anlık görüntüsünü oluşturur ve kaynak ve hedef arasında veri eşitleme gerçekleştirmez.

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için kullanılabilen belirli bilgi işlem boyutları ve depolama boyutu seçenekleri hakkında ayrıntılı bilgi için, [tek veritabanları için SQL Database vCore tabanlı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md) bakın
- Elastik havuzlar için belirli bilgi işlem boyutları ve depolama boyutu seçenekleri hakkında ayrıntılı bilgi için, [elastik havuzlar için SQL Database vCore tabanlı kaynak sınırlarına](sql-database-vcore-resource-limits-elastic-pools.md)bakın.
