---
title: Genel amaç ve iş kritik
description: Makalede, vCore tabanlı satın alma modelinde genel amaç ve iş açısından kritik hizmet katmanları tartışılmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 01/30/2020
ms.openlocfilehash: 09cc9e1475616700aa77cdf92fd7ca808cd4290c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76937850"
---
# <a name="azure-sql-database-service-tiers"></a>Azure SQL Veritabanı hizmet katmanları

Azure SQL Veritabanı, bir altyapı hatası olsa bile bulut ortamı için yüzde 99,99 kullanılabilirlik sağlamak üzere ayarlanmış SQL Server veritabanı altyapısı mimarisine dayanır. Azure SQL Veritabanı'nda her biri farklı bir mimari modele sahip üç hizmet katmanı kullanılır. Bu hizmet katmanları şunlardır:

- Bütçe odaklı iş yükleri için tasarlanmış [genel amaç.](sql-database-service-tier-general-purpose.md)
- Çoğu iş yükü için tasarlanmış olan ve yüksek ölçeklenebilir depolama sağlayan, ölçeklendirmeyi okuyan ve hızlı veritabanı geri yükleme yetenekleri sağlayan [hiper ölçeklendirme.](sql-database-service-tier-hyperscale.md)
- [İş açısından kritik](sql-database-service-tier-business-critical.md),arızalara ve hızlı arızalara karşı yüksek esneklik ile düşük gecikmeli iş yükleri için tasarlanmıştır.

Bu makalede, vCore tabanlı satın alma modelinde genel amaç ve iş açısından kritik hizmet katmanları için hizmet katmanları, depolama ve yedekleme hususları için farklar tartışılmaktadır.

## <a name="service-tier-comparison"></a>Hizmet katmanı karşılaştırması

Aşağıdaki tabloda, en son nesil (Gen5) için hizmet katmanları arasındaki temel farklar açıklanmaktadır. Hizmet katmanı özelliklerinin Tek Veritabanı ve Yönetilen Örnek'te farklı olabileceğini unutmayın.

| | Kaynak türü | Genel Amaçlı |  Hiper Ölçek | İş Kritik |
|:---:|:---:|:---:|:---:|:---:|
| **En iyisi** | |  Bütçe odaklı dengeli işlem ve depolama seçenekleri sunar. | Çoğu iş yükü. 100 TB'a kadar otomatik ölçekleme depolama boyutu, sıvı dikey ve yatay işlem ölçekleme, hızlı veritabanı geri yükleme. | Yüksek işlem hızı ve düşük IO gecikme süresi ile OLTP uygulamaları. Birden çok eşzamanlı olarak güncellenen yinelemeler kullanarak hatalara ve hızlı hatalara karşı en yüksek esneklik sağlar.|
|  **Kaynak türünde kullanılabilir:** ||Tek veritabanı / elastik havuz / yönetilen örnek | Tek veritabanı | Tek veritabanı / elastik havuz / yönetilen örnek |
| **İşlem boyutu**|Tek veritabanı / elastik havuz | 1 - 80 vCores | 1 - 80 vCores | 1 - 80 vCores |
| | Yönetilen örnek | 4, 8, 16, 24, 32, 40, 64, 80 vCores | Yok | 4, 8, 16, 24, 32, 40, 64, 80 vCores |
| | Yönetilen örnek havuzları | 2, 4, 8, 16, 24, 32, 40, 64, 80 vCores | Yok | Yok |
| **Depolama türü** | Tümü | Premium uzak depolama (örnek başına) | Yerel SSD önbelleğiyle birleştirilmiş depolama alanı (örneğinbaşına) | Süper hızlı yerel SSD depolama (örnek başına) |
| **Veritabanı boyutu** | Tek veritabanı / elastik havuz | 5 GB – 4 TB | 100 TB'a kadar | 5 GB – 4 TB |
| | Yönetilen örnek  | 32 GB – 8 TB | Yok | 32 GB – 4 TB |
| **Depolama boyutu** | Tek veritabanı / elastik havuz | 5 GB – 4 TB | 100 TB'a kadar | 5 GB – 4 TB |
| | Yönetilen örnek  | 32 GB – 8 TB | Yok | 32 GB – 4 TB |
| **TempDB boyutu** | Tek veritabanı / elastik havuz | [vCore başına 32 GB](sql-database-vcore-resource-limits-single-databases.md#general-purpose---provisioned-compute---gen4) | [vCore başına 32 GB](sql-database-vcore-resource-limits-single-databases.md#hyperscale---provisioned-compute---gen5) | [vCore başına 32 GB](sql-database-vcore-resource-limits-single-databases.md#business-critical---provisioned-compute---gen4) |
| | Yönetilen örnek  | [vCore başına 24 GB](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) | Yok | 4 TB'a kadar - [depolama boyutuyla sınırlıdır](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) |
| **Günlük yazma iş** | Tek veritabanı | [VCore başına 1,875 MB/sn (maksimum 30 MB/sn)](sql-database-vcore-resource-limits-single-databases.md#general-purpose---provisioned-compute---gen4) | 100 MB/s | [VCore başına 6 MB/sn (maksimum 96 MB/sn)](sql-database-vcore-resource-limits-single-databases.md#business-critical---provisioned-compute---gen4) |
| | Yönetilen örnek | [VCore başına 3 MB/sn (maksimum 22 MB/sn)](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) | Yok | [Vcore başına 4 MB/sn (maksimum 48 MB/s)](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) |
|**Kullanılabilir -lik**|Tümü| %99,99 |  [%99.95 bir ikincil kopyaile, %99.99 daha fazla kopya](sql-database-service-tier-hyperscale-faq.md#what-slas-are-provided-for-a-hyperscale-database) | %99,99 <br/> [Bölge yedekli tek veritabanı ile % 99.995](https://azure.microsoft.com/blog/understanding-and-leveraging-azure-sql-database-sla/) |
|**Yedeklemeler**|Tümü|RA-GRS, 7-35 gün (varsayılan olarak 7 gün)| RA-GRS, 7 gün, sabit zaman noktası zaman kurtarma (PITR) | RA-GRS, 7-35 gün (varsayılan olarak 7 gün) |
|**Hafıza içi OLTP** | | Yok | Yok | Kullanılabilir |
|**Salt okunur yinelemeler**| | 0 dahili <br> 0 - 4 [coğrafi çoğaltma](sql-database-active-geo-replication.md) kullanarak | 0 - 4 dahili | 1 dahili, fiyata dahil <br> 0 - 4 [coğrafi çoğaltma](sql-database-active-geo-replication.md) kullanarak |
|**Fiyatlandırma/faturalandırma** | Tek veritabanı | [vCore, ayrılmış depolama ve yedekleme depolama](https://azure.microsoft.com/pricing/details/sql-database/single/) ücreti alınır. <br/>IOPS ücretlendirilmez. | [her çoğaltma ve kullanılan depolama için vCore](https://azure.microsoft.com/pricing/details/sql-database/single/) ücretlendirilir. <br/>IOPS henüz şarj edilmedi. | [vCore, ayrılmış depolama ve yedekleme depolama](https://azure.microsoft.com/pricing/details/sql-database/single/) ücreti alınır. <br/>IOPS ücretlendirilmez. |
|| Yönetilen Örnek | [vCore, ayrılmış depolama ve yedekleme depolama](https://azure.microsoft.com/pricing/details/sql-database/managed/) ücreti alınır. <br/>IOPS ücretlendirilmez| Yok | [vCore, ayrılmış depolama ve yedekleme depolama](https://azure.microsoft.com/pricing/details/sql-database/managed/) ücreti alınır. <br/>IOPS ücretlendirilmez.| 
|**İndirim modelleri**| | [Ayrılmış örnekler](sql-database-reserved-capacity.md)<br/>[Azure Karma Avantajı](sql-database-azure-hybrid-benefit.md) (geliştirme/test aboneliklerinde kullanılamaz)<br/>[Kurumsal](https://azure.microsoft.com/offers/ms-azr-0148p/) ve [Öde-You-Go](https://azure.microsoft.com/offers/ms-azr-0023p/) Geliştirme/Test abonelikleri| [Azure Karma Avantajı](sql-database-azure-hybrid-benefit.md) (geliştirme/test aboneliklerinde kullanılamaz)<br/>[Kurumsal](https://azure.microsoft.com/offers/ms-azr-0148p/) ve [Öde-You-Go](https://azure.microsoft.com/offers/ms-azr-0023p/) Geliştirme/Test abonelikleri| [Ayrılmış örnekler](sql-database-reserved-capacity.md)<br/>[Azure Karma Avantajı](sql-database-azure-hybrid-benefit.md) (geliştirme/test aboneliklerinde kullanılamaz)<br/>[Kurumsal](https://azure.microsoft.com/offers/ms-azr-0148p/) ve [Öde-You-Go](https://azure.microsoft.com/offers/ms-azr-0023p/) Geliştirme/Test abonelikleri|

Daha fazla bilgi için, [Tek veritabanı (vCore)](sql-database-vcore-resource-limits-single-databases.md), Tek [veritabanı havuzları (vCore)](sql-database-dtu-resource-limits-single-databases.md), [Tek veritabanı (DTU)](sql-database-dtu-resource-limits-single-databases.md), [Tek veritabanı havuzları (DTU)](sql-database-dtu-resource-limits-single-databases.md)ve [Yönetilen Örnek](sql-database-managed-instance-resource-limits.md) sayfalarında hizmet katmanları arasındaki ayrıntılı farklara bakın.

> [!NOTE]
> vCore tabanlı satın alma modelindeki hiper ölçekli hizmet katmanı hakkında bilgi [için, hiper ölçekli hizmet katmanına](sql-database-service-tier-hyperscale.md)bakın. vCore tabanlı satın alma modelinin DTU tabanlı satın alma modeliyle karşılaştırılması için [Azure SQL Veritabanı satın alma modelleri ve kaynaklarına](sql-database-purchase-models.md)bakın.

## <a name="data-and-log-storage"></a>Veri ve günlük depolama

Aşağıdaki etkenler veri ve günlük dosyaları için kullanılan depolama miktarını etkiler ve Genel Amaç ve İş Kritik için geçerlidir. Hyperscale'deki veri ve günlük depolama ile ilgili ayrıntılar için [Hyperscale hizmet katmanına](sql-database-service-tier-hyperscale.md)bakın.

- Ayrılan depolama veri dosyaları (MDF) ve günlük dosyaları (LDF) tarafından kullanılır.
- Her bir veritabanı işlem boyutu, varsayılan maksimum boyutu 32 GB olan maksimum veritabanı boyutunu destekler.
- Gerekli tek veritabanı boyutunu (MDF dosyasının boyutu) yapılandırdığınızda, LDF dosyalarını desteklemek için otomatik olarak yüzde 30 daha fazla ek depolama alanı eklenir.
- SQL Veritabanı yönetilen örneğinin depolama boyutu 32 GB'ın katları olarak belirtilmelidir.
- 10 GB ile desteklenen maksimum arasında herhangi bir tek veritabanı boyutu seçebilirsiniz.
  - Standart veya genel amaçlı hizmet katmanlarında depolama için, 10 GB'lık artışlarla boyutu artırın veya küçültün.
  - Premium veya iş açısından kritik hizmet katmanlarında depolama için, 250 GB'lık artışlarla boyutu artırın veya küçültün.
- Genel amaçlı hizmet katmanında, `tempdb` ekli bir SSD kullanır ve bu depolama maliyeti vCore fiyatına dahildir.
- İş açısından kritik hizmet `tempdb` katmanında, ekli SSD'yi MDF ve `tempdb` LDF dosyalarıyla paylaşır ve depolama maliyeti vCore fiyatına dahildir.

> [!IMPORTANT]
> MDF ve LDF dosyaları için ayrılan toplam depolama alanı için ücretlendirilirsiniz.

MDF ve LDF dosyalarınızın geçerli toplam boyutunu izlemek için [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql)kullanın. Tek tek MDF ve LDF dosyalarının geçerli boyutunu izlemek [için sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql)kullanın.

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri almak için bir veritabanını küçültmeniz gerekebilir. Daha fazla bilgi için Azure [SQL Veritabanı'nda dosya alanını yönet'e](sql-database-file-space-management.md)bakın.

## <a name="backups-and-storage"></a>Yedeklemeler ve depolama

Veritabanı yedeklemeleri için depolama, SQL Veritabanı'nın zamanında geri yükleme (PITR) ve [uzun süreli bekletme (LTR)](sql-database-long-term-retention.md) yeteneklerini desteklemek için ayrılmıştır. Bu depolama, her veritabanı için ayrı ayrı ayrılır ve veritabanı başına iki ayrı ücret olarak faturalandırılır.

- **PITR**: Tek tek veritabanı yedeklemeleri [okuma-erişim coğrafi (RA-GRS) depolama](../storage/common/storage-designing-ha-apps-with-ragrs.md) otomatik olarak kopyalanır. Yeni yedeklemeler oluşturuldukça depolama boyutu dinamik olarak artar. Depolama, her 5 dakikada bir kopyalanan haftalık tam yedeklemeler, günlük diferansiyel yedeklemeler ve işlem günlüğü yedekleri tarafından kullanılır. Depolama tüketimi, veritabanındaki değişim hızına ve yedeklemeler için bekletme süresine bağlıdır. Her veritabanı için 7 ila 35 gün arasında ayrı bir bekletme süresi yapılandırabilirsiniz. Veritabanı boyutunun yüzde 100'üne (1x) eşit minimum depolama miktarı ek ücret ödemeden sağlanır. Çoğu veritabanları için bu tutar, 7 günlük yedeklemeleri depolamak için yeterlidir.
- **LTR**: SQL Veritabanı, tam yedeklemelerin 10 yıla kadar uzun süreli tutulmasını yapılandırma seçeneği sunar. Bir LTR ilkesi ayarlarsanız, bu yedeklemeler RA-GRS depolama alanında otomatik olarak depolanır, ancak yedeklemelerin ne sıklıkta kopyalandığını denetleyebilirsiniz. Farklı uyumluluk gereksinimlerini karşılamak için, haftalık, aylık ve/veya yıllık yedeklemeler için farklı bekletme dönemleri seçebilirsiniz. Seçtiğiniz yapılandırma, LTR yedeklemeleri için ne kadar depolama alanı kullanılacağını belirler. LTR depolama maliyeti tahmin etmek için, LTR fiyatlandırma hesap makinesini kullanabilirsiniz. Daha fazla bilgi için SQL [Veritabanı uzun süreli bekletme](sql-database-long-term-retention.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Genel amaçlı ve iş açısından kritik hizmet katmanlarında tek bir veritabanı için kullanılabilen belirli bilgi işlem boyutları ve depolama boyutları hakkında ayrıntılı bilgi için, [tek veritabanları için SQL Database vCore tabanlı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın.
- Genel amaçlı ve iş açısından kritik hizmet katmanlarında elastik havuzlar için kullanılabilen belirli bilgi işlem boyutları ve depolama boyutları hakkında ayrıntılı bilgi için, [elastik havuzlar için SQL Database vCore tabanlı kaynak sınırlarına](sql-database-vcore-resource-limits-elastic-pools.md)bakın.
