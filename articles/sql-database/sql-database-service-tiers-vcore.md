---
title: Azure SQL veritabanı hizmeti-sanal çekirdek | Microsoft Docs
description: Sanal çekirdek tabanlı satın alma modeli, işlem ve depolama kaynaklarını bağımsız olarak ölçeklendirmenize, şirket içi performansı eşleşmenize ve fiyatı iyileştirmenize olanak tanır.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 10/01/2019
ms.openlocfilehash: af2e8826c40fb0d16844b6c67f151b0affbf3efd
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72034991"
---
# <a name="choose-among-the-vcore-service-tiers-and-migrate-from-the-dtu-service-tiers"></a>Sanal çekirdek hizmet katmanları arasında seçim yapın ve DTU hizmeti katmanlarından geçiş yapın

Sanal çekirdek (vCore) tabanlı satın alma modeli, işlem ve depolama kaynaklarını bağımsız olarak ölçeklendirmenize, şirket içi performansı eşleşmenize ve fiyatı iyileştirmenize olanak tanır. Ayrıca, donanım üretimini seçmenizi sağlar:

- **4. nesil**: Intel E5-2673 v3 (Haswell) 2,4-GHz Işlemcileri, sanal çekirdek = 1 PP (fiziksel çekirdek), her Vcore IÇIN 7 GB, ekli SSD 'yi temel alan en fazla 24 mantıksal CPU
- **5. nesil**: Intel E5-2673 v4 (çok iyi) 2,3-GHz Işlemciler, vCore = 1 LP (hiper iş parçacığı) 5,1, sağlanan işlem Için her vcore IÇIN 80 GB ve sunucusuz Işlem Için Vcore BAŞıNA 24 GB 'a kadar mantıksal CPU

4\. nesil donanım, vCore başına önemli ölçüde daha fazla bellek sunar. Ancak, 5. nesil donanım, işlem kaynaklarını çok daha yüksek bir şekilde ölçeklendirmenize olanak tanır.

> [!IMPORTANT]
> Yeni 4. nesil veritabanları artık Avustralya Doğu veya Brezilya Güney bölgelerinde desteklenmez.
> [!NOTE]
> DTU tabanlı hizmet katmanları hakkında daha fazla bilgi için bkz. [DTU tabanlı satın alma modeli Için hizmet katmanları](sql-database-service-tiers-dtu.md). DTU tabanlı ve sanal çekirdek tabanlı satın alma modelleriyle ilgili hizmet katmanları arasındaki farklar hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı satın alma modelleri](sql-database-purchase-models.md).

## <a name="service-tier-characteristics"></a>Hizmet katmanı özellikleri

Sanal çekirdek tabanlı satın alma modeli üç hizmet katmanı sunar: genel amaçlı, hiper ölçek ve iş açısından kritik. Bu hizmet katmanları, bir dizi işlem boyutuna, yüksek kullanılabilirliğe sahip tasarımlara, hata yalıtımı yöntemlerine, tür ve depolama ve g/ç aralıklarına göre farklılaştırılabilir.

Yedeklemeler için gereken depolama ve bekletme süresini ayrı olarak yapılandırmanız gerekir. Yedekleme bekletme süresini ayarlamak için, Azure portal açın, sunucuya gidin (veritabanına değil) ve ardından **Yedeklemeleri Yönet**' e gidin  > **İlkeyi Yapılandır** > **zaman yükleme yapılandırma** > **7-35 gün**.

Aşağıdaki tabloda üç katman arasındaki farklar açıklanmaktadır:

||**Genel amaçlı**|**İş açısından kritik**|**Hiper ölçekli**|
|---|---|---|---|
|Şunlar için en iyisidir:|Bütçe odaklı dengeli işlem ve depolama seçenekleri sunar.|Yüksek işlem hızına sahip OLTP uygulamaları ve düşük GÇ gecikme süresi. Birden çok zaman uyumlu olarak güncellenen çoğaltmaları kullanarak hatalara en yüksek esnekliği ve hızlı yük devretme olanağı sunar.|Birçok iş yükü. Depolama boyutunu 100 TB 'a kadar otomatik ölçeklendirme, sıvı dikey ve yatay işlem ölçekleme, hızlı veritabanı geri yükleme.|
|İşlem|**Sağlanan işlem**:<br/>4\. nesil: 1-24 sanal çekirdek<br/>5\. nesil: 2-80 sanal çekirdek<br/>**Sunucusuz işlem**:<br/>5\. nesil: 0,5-16 sanal çekirdek|**Sağlanan işlem**:<br/>4\. nesil: 1-24 sanal çekirdek<br/>5\. nesil: 2-80 sanal çekirdek|**Sağlanan işlem**:<br/>4\. nesil: 1-24 sanal çekirdek<br/>5\. nesil: 2-80 sanal çekirdek|
|Hafıza|**Sağlanan işlem**:<br/>4\. nesil: vCore başına 7 GB<br/>5\. nesil: vCore başına 5,1 GB<br/>**Sunucusuz işlem**:<br/>5\. nesil: vCore başına en fazla 24 GB|**Sağlanan işlem**:<br/>4\. nesil: vCore başına 7 GB<br/>5\. nesil: vCore başına 5,1 GB |**Sağlanan işlem**:<br/>4\. nesil: vCore başına 7 GB<br/>5\. nesil: vCore başına 5,1 GB|
|Depolama|Uzak depolamayı kullanır.<br/>**Tek veritabanı ve elastik havuz sağlanan işlem**:<br/>5 GB – 4 TB<br/>**Sunucusuz işlem**:<br/>5 GB-3 TB<br/>**Yönetilen örnek**: 32 GB-8 TB |Yerel SSD depolama kullanır.<br/>**Tek veritabanı ve elastik havuz sağlanan işlem**:<br/>5 GB – 4 TB<br/>**Yönetilen örnek**:<br/>32 GB-4 TB |Gerektiğinde depolamanın esnek otomatik büyümesi. 100 TB 'a kadar depolamayı destekler. Yerel ara havuz önbelleği ve yerel veri depolaması için yerel SSD depolama kullanır. Son uzun süreli veri deposu olarak Azure uzak depolama kullanır. |
|G/ç verimlilik (yaklaşık)|**Tek veritabanı ve elastik havuz**: 500 IOPS, vCore başına en fazla 40000 IOPS.<br/>**Yönetilen örnek**: [dosyanın boyutuna](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)bağlıdır.|çekirdek başına 5000 ıOPS, en fazla 200.000 maksimum ıOPS|Hiper ölçek, birden çok düzeyde önbelleğe alma özelliği olan çok katmanlı bir mimaridir. Etkin IOPS iş yüküne bağlı olacaktır.|
|Erişilebilirlik|1 çoğaltma, okuma ölçeğinde çoğaltmalar yok|3 çoğaltma, 1 [okuma ölçeği çoğaltma](sql-database-read-scale-out.md),<br/>bölge yedekli yüksek kullanılabilirlik (HA)|1 okuma-yazma çoğaltması, artı 0-4 [okuma ölçekli çoğaltmalar](sql-database-read-scale-out.md)|
|Yedeklemeler|[Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 gün (varsayılan olarak 7 gün)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 gün (varsayılan olarak 7 gün)|Azure uzak depolama 'da anlık görüntü tabanlı yedeklemeler. Geri yükleme bu anlık görüntüleri hızlı kurtarma için kullanır. Yedeklemeler anında gerçekleşir ve işlem g/ç performansını etkilemez. Geri yükleme işlemleri hızlıdır ve veri boyutu (saatler veya günler yerine dakikalar içinde).|
|Bellek içi|Desteklenmiyor|Desteklenen|Desteklenmiyor|
|||

> [!NOTE]
> Azure Ücretsiz hesabıyla birlikte, temel hizmet katmanında ücretsiz bir Azure SQL veritabanı edinebilirsiniz. Daha fazla bilgi için bkz. [Azure Ücretsiz hesabınızla yönetilen bulut veritabanı oluşturma](https://azure.microsoft.com/free/services/sql-database/).

- VCore kaynak limitleri hakkında daha fazla bilgi için, [yönetilen bir örnekteki](sql-database-managed-instance.md#vcore-based-purchasing-model) [tek bir veritabanındaki sanal çekirdek kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md) ve sanal çekirdek kaynak sınırlarına bakın.
- Genel amaçlı ve iş açısından kritik hizmet katmanları hakkında daha fazla bilgi için bkz. [genel amaçlı ve iş açısından kritik hizmet katmanları](sql-database-service-tiers-general-purpose-business-critical.md).
- Sanal çekirdek tabanlı satın alma modelindeki hiper ölçek hizmet katmanı hakkında daha fazla bilgi için bkz. [hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md).  

## <a name="azure-hybrid-benefit"></a>Azure Hibrit Avantajı

Sanal çekirdek tabanlı satın alma modelinin sağlanan işlem katmanında, [SQL Server için Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/)kullanarak SQL veritabanı 'nda indirimli ücretler için mevcut lisanslarınızı Exchange 'e dönüştürebilirsiniz. Bu Azure avantajı, Yazılım Güvencesi kapsamındaki şirket içi SQL Server lisanslarınızı kullanarak Azure SQL veritabanında yüzde 30 ' a varan tasarruf etmenize olanak tanır.

![Fiyat](./media/sql-database-service-tiers/pricing.png)

Azure Hibrit Avantajı ile yalnızca SQL veritabanı altyapısının kendisi için mevcut SQL Server lisansınızı kullanarak (temel Işlem fiyatlandırması) yalnızca temeldeki Azure altyapısı için ödeme yapmayı seçebilirsiniz veya hem temel alınan altyapı hem de SQL Server için ödeme yapabilirsiniz Lisans (lisans dahil fiyatlar).

Azure portal kullanarak veya aşağıdaki API 'lerden birini kullanarak lisanslama modelinizi seçebilir veya değiştirebilirsiniz:

- PowerShell kullanarak lisans türünü ayarlamak veya güncelleştirmek için:

  - [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase)
  - [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase)
  - [New-Azsqlınstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)
  - [Set-Azsqlınstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)

- Azure CLı kullanarak lisans türünü ayarlamak veya güncelleştirmek için:

  - [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)
  - [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)
  - [az SQL mı oluştur](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create)
  - [az SQL mı güncelleştirmesi](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)

- REST API kullanarak lisans türünü ayarlamak veya güncelleştirmek için:

  - [Veritabanları-oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)
  - [Veritabanları-Güncelleştir](https://docs.microsoft.com/rest/api/sql/databases/update)
  - [Yönetilen örnekler-oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)
  - [Yönetilen örnekler-güncelleştirme](https://docs.microsoft.com/rest/api/sql/managedinstances/update)

## <a name="migrate-from-the-dtu-based-model-to-the-vcore-based-model"></a>DTU tabanlı modelden sanal çekirdek tabanlı modele geçiş

### <a name="migrate-a-database"></a>Veritabanını geçirme

DTU tabanlı satın alma modelinden bir veritabanını sanal çekirdek tabanlı satın alma modeline geçirmek, DTU tabanlı satın alma modelindeki standart ve Premium hizmet katmanları arasında yükseltme veya eski sürüme düşürme ile benzerdir.

### <a name="migrate-databases-with-geo-replication-links"></a>Coğrafi çoğaltma bağlantılarıyla veritabanlarını geçirme

DTU tabanlı modelden sanal çekirdek tabanlı satın alma modeline geçiş yapmak, standart ve Premium hizmet katmanlarındaki veritabanları arasında coğrafi çoğaltma ilişkilerini yükseltmekle veya eski sürüme düşürmeye benzer. Geçiş sırasında Coğrafi çoğaltmayı durdurmanız gerekmez, ancak şu sıralama kurallarını izlemeniz gerekir:

- Yükseltme sırasında öncelikle ikincil veritabanını yükseltmeniz ve ardından birincili yükseltmeniz gerekir.
- Eski sürüme düşürme sırasında sırayı tersine çevirin: önce birincil veritabanını indirgemeniz ve sonra ikincili indirgemeniz gerekir.

İki elastik havuz arasında coğrafi çoğaltma kullanırken, bir havuzu birincil ve diğeri, ikincil olarak atamanız önerilir. Bu durumda, elastik havuzları geçirirken aynı sıralama kılavuzunu kullanmanız gerekir. Ancak, hem birincil hem de ikincil veritabanları içeren elastik havuzlarınız varsa, havuzu birincil olarak daha yüksek kullanım ile değerlendirin ve sıralama kurallarını uygun şekilde izleyin.  

Aşağıdaki tablo, belirli geçiş senaryoları için rehberlik sağlar:

|Geçerli hizmet katmanı|Hedef hizmet katmanı|Geçiş türü|Kullanıcı eylemleri|
|---|---|---|---|
|Standart|Genel amaç|Geni|Herhangi bir sırada geçirebilir, ancak uygun sanal çekirdek boyutlandırmayı sağlamak için gerekir *|
|Premium|İş açısından kritik|Geni|Herhangi bir sırada geçirebilir, ancak uygun sanal çekirdek boyutlandırmayı sağlamak için gerekir *|
|Standart|İş açısından kritik|Yükseltin|Önce ikinciye geçirilmesi gerekiyor|
|İş açısından kritik|Standart|Eski sürüme düşür|Önce birincil önce geçirilmesi gerekir|
|Premium|Genel amaç|Eski sürüme düşür|Önce birincil önce geçirilmesi gerekir|
|Genel amaç|Premium|Yükseltin|Önce ikinciye geçirilmesi gerekiyor|
|İş açısından kritik|Genel amaç|Eski sürüme düşür|Önce birincil önce geçirilmesi gerekir|
|Genel amaç|İş açısından kritik|Yükseltin|Önce ikinciye geçirilmesi gerekiyor|
||||

\* her 100 DTU Standart katmanda en az 1 sanal çekirdek gerektirir ve Premium katmandaki her 125 DTU en az 1 sanal çekirdek gerektirir.

### <a name="migrate-failover-groups"></a>Yük devretme gruplarını geçirme

Birden çok veritabanına sahip yük devretme gruplarının geçirilmesi, birincil ve ikincil veritabanlarının tek tek geçirilmesini gerektirir. Bu işlem sırasında, aynı noktalar ve sıralama kuralları da geçerlidir. Veritabanları sanal çekirdek tabanlı satın alma modeline dönüştürüldükten sonra, yük devretme grubu aynı ilke ayarları ile geçerli olmaya devam edecektir.

### <a name="create-a-geo-replication-secondary-database"></a>Coğrafi çoğaltma ikincil veritabanı oluşturma

Yalnızca birincil veritabanı için kullandığınız hizmet katmanını kullanarak coğrafi çoğaltma ikincil veritabanı (coğrafi-ikincil) oluşturabilirsiniz. Yüksek günlük oluşturma hızına sahip veritabanları için, birincil ile aynı işlem boyutuyla coğrafi ikincil oluşturmanız önerilir.

Tek bir birincil veritabanı için elastik havuzda bir coğrafi ikincil oluşturuyorsanız, havuzun `maxVCore` ayarının birincil veritabanının işlem boyutuyla eşleştiğinden emin olun. Başka bir elastik havuzda birincil için bir coğrafi-ikincil oluşturuyorsanız, havuzların aynı `maxVCore` ayarlarına sahip olmasını öneririz.

### <a name="use-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>DTU tabanlı bir veritabanını sanal çekirdek tabanlı veritabanına dönüştürmek için veritabanı kopyasını kullanma

Hedef işlem boyutu, kaynak veritabanının en büyük veritabanı boyutunu desteklediği sürece, DTU tabanlı işlem boyutu olan bir veritabanını, kısıtlama olmadan veya özel sıralamaya sahip bir veritabanı olarak bir veritabanına kopyalayabilirsiniz. Veritabanı kopyalama işlemi, kopyalama işleminin başlangıç saatinden itibaren verilerin bir anlık görüntüsünü oluşturur ve kaynak ile hedef arasında veri eşitlenmez.

## <a name="next-steps"></a>Sonraki adımlar

- Tek veritabanları için kullanılabilen belirli işlem boyutları ve depolama boyutu seçimleri için bkz. [tek veritabanları Için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md).
- Elastik havuzlara yönelik belirli işlem boyutları ve depolama boyutu seçimleri için bkz. [elastik havuzlar Için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
