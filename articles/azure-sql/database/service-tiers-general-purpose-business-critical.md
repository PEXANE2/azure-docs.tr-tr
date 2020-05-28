---
title: Genel amaçlı ve iş açısından kritik hizmet katmanları
titleSuffix: Azure SQL Database & SQL Managed Instance
description: Bu makalede, Azure SQL veritabanı ve Azure SQL yönetilen örneği tarafından kullanılan sanal çekirdek tabanlı satın alma modelindeki genel amaçlı ve iş açısından kritik hizmet katmanları ele alınmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 01/30/2020
ms.openlocfilehash: 1783285704870dbcaeac731dc085bddf8851c7be
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84049830"
---
# <a name="azure-sql-database-and-azure-sql-managed-instance-service-tiers"></a>Azure SQL veritabanı ve Azure SQL yönetilen örnek hizmeti katmanları
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure SQL veritabanı ve Azure SQL yönetilen örneği, bir altyapı hatası olsa bile yüzde 99,99 kullanılabilirlik sağlamak için bulut ortamı için ayarlanmış SQL Server veritabanı altyapısı mimarisini temel alır. İki hizmet katmanı, Azure SQL veritabanı ve Azure SQL yönetilen örneği tarafından her biri farklı bir mimari modeli ile kullanılır. Bu hizmet katmanları şunlardır:

- Bütçe odaklı iş yükleri için tasarlanan [genel amaçlı](service-tier-general-purpose.md).
- Hatalara ve hızlı yük devretmeyle yüksek dayanıklılık sağlayan düşük gecikmeli iş yükleri için tasarlanan [iş açısından kritik](service-tier-business-critical.md).

Azure SQL veritabanı ek bir hizmet katmanına sahiptir: 

- Yüksek düzeyde ölçeklenebilir depolama, okuma ölçeği genişletme ve hızlı veritabanı geri yükleme özellikleri sağlayan, iş yükleri için tasarlanan [hiper ölçek](service-tier-hyperscale.md).

Bu makalede, sanal çekirdek tabanlı satın alma modelindeki genel amaçlı ve iş açısından kritik hizmet katmanlarına yönelik hizmet katmanları, depolama ve yedekleme konuları arasındaki farklılıklar ele alınmaktadır.

## <a name="service-tier-comparison"></a>Hizmet katmanı karşılaştırması

Aşağıdaki tabloda, en son nesil (5. nesil) için hizmet katmanları arasındaki temel farklılıklar açıklanmaktadır. Hizmet katmanı özelliklerinin SQL veritabanı ve SQL yönetilen örneği 'nde farklı olabileceğini unutmayın.

| | Kaynak türü | Genel Amaçlı |  Hiper Ölçek | İş Açısından Kritik |
|:---:|:---:|:---:|:---:|:---:|
| **En iyisi** | |  Bütçe odaklı dengeli işlem ve depolama seçenekleri sunar. | Birçok iş yükü. Depolama boyutunu 100 TB 'a kadar otomatik ölçeklendirme, sıvı dikey ve yatay işlem ölçekleme, hızlı veritabanı geri yükleme. | Yüksek işlem hızına sahip OLTP uygulamaları ve düşük GÇ gecikme süresi. Birden çok zaman uyumlu olarak güncellenen çoğaltmaları kullanarak hatalara en yüksek esnekliği ve hızlı yük devretme olanağı sunar.|
|  **Kaynak türünde kullanılabilir:** ||SQL veritabanı/SQL yönetilen örneği | Tek Azure SQL veritabanı | SQL veritabanı/SQL yönetilen örneği |
| **İşlem boyutu**| SQL Veritabanı | 1-80 sanal çekirdek | 1-80 sanal çekirdek | 1-80 sanal çekirdek |
| | SQL Yönetilen Örnek | 4, 8, 16, 24, 32, 40, 64, 80 Vçekirdekler | Yok | 4, 8, 16, 24, 32, 40, 64, 80 Vçekirdekler |
| | SQL yönetilen örnek havuzları | 2, 4, 8, 16, 24, 32, 40, 64, 80 Vçekirdekler | Yok | Yok |
| **Depolama türü** | Tümü | Premium uzak depolama (örnek başına) | Yerel SSD Önbelleği (örnek başına) ile birlikte bağlanmış depolama | Süper hızlı yerel SSD depolaması (örnek başına) |
| **Veritabanı boyutu** | SQL Veritabanı | 5 GB – 4 TB | 100 TB 'a kadar | 5 GB – 4 TB |
| | SQL Yönetilen Örnek  | 32 GB – 8 TB | Yok | 32 GB – 4 TB |
| **Depolama boyutu** | SQL Veritabanı | 5 GB – 4 TB | 100 TB 'a kadar | 5 GB – 4 TB |
| | SQL Yönetilen Örnek  | 32 GB – 8 TB | Yok | 32 GB – 4 TB |
| **TempDB boyutu** | SQL Veritabanı | [vCore başına 32 GB](resource-limits-vcore-single-databases.md#general-purpose---provisioned-compute---gen4) | [vCore başına 32 GB](resource-limits-vcore-single-databases.md#hyperscale---provisioned-compute---gen5) | [vCore başına 32 GB](resource-limits-vcore-single-databases.md#business-critical---provisioned-compute---gen4) |
| | SQL Yönetilen Örnek  | [vCore başına 24 GB](../managed-instance/resource-limits.md#service-tier-characteristics) | Yok | En fazla 4 TB- [sınırlı depolama boyutuna göre](../managed-instance/resource-limits.md#service-tier-characteristics) |
| **Günlüğe yazma aktarım hızı** | SQL Veritabanı | [vCore başına 1,875 MB/s (en fazla 30 MB/sn)](resource-limits-vcore-single-databases.md#general-purpose---provisioned-compute---gen4) | 100 MB/s | [vCore başına 6 MB/s (en fazla 96 MB/sn)](resource-limits-vcore-single-databases.md#business-critical---provisioned-compute---gen4) |
| | SQL Yönetilen Örnek | [Sanal çekirdek başına 3 MB/s (en fazla 22 MB/sn)](../managed-instance/resource-limits.md#service-tier-characteristics) | Yok | [sanal çekirdek başına 4 MB/s (en fazla 48 MB/sn)](../managed-instance/resource-limits.md#service-tier-characteristics) |
|**Kullanılabilirlik**|Tümü| %99,99 |  [bir ikincil çoğaltmayla% 99,95, daha fazla çoğaltmayla% 99,99](service-tier-hyperscale-frequently-asked-questions-faq.md#what-slas-are-provided-for-a-hyperscale-database) | %99,99 <br/> [bölge yedekli tek veritabanı ile% 99,995](https://azure.microsoft.com/blog/understanding-and-leveraging-azure-sql-database-sla/) |
|**Yedeklemeler**|Tümü|RA-GRS, 7-35 gün (varsayılan olarak 7 gün)| RA-GRS, 7 gün, sabit zaman zaman aşımı kurtarma (sür) | RA-GRS, 7-35 gün (varsayılan olarak 7 gün) |
|**Bellek içi OLTP** | | Yok | Yok | Kullanılabilir |
|**Salt okuma çoğaltmaları**| | 0 yerleşik <br> [Coğrafi çoğaltmayı](active-geo-replication-overview.md) kullanarak 0-4 | 0-4 yerleşik | 1 yerleşik, fiyata dahildir <br> [Coğrafi çoğaltmayı](active-geo-replication-overview.md) kullanarak 0-4 |
|**Fiyatlandırma/faturalandırma** | SQL Veritabanı | [sanal çekirdek, ayrılmış depolama ve yedekleme depolaması](https://azure.microsoft.com/pricing/details/sql-database/single/) ücretlendirilir. <br/>IOPS ücretlendirilmez. | [her çoğaltma Için sanal çekirdek ve kullanılan depolama alanı](https://azure.microsoft.com/pricing/details/sql-database/single/) ücretlendirilir. <br/>IOPS henüz ücretlendirilmedi. | [sanal çekirdek, ayrılmış depolama ve yedekleme depolaması](https://azure.microsoft.com/pricing/details/sql-database/single/) ücretlendirilir. <br/>IOPS ücretlendirilmez. |
|| SQL Yönetilen Örnek | [sanal çekirdek, ayrılmış depolama ve yedekleme depolama](https://azure.microsoft.com/pricing/details/sql-database/managed/) alanı ücretlendirilir. <br/>IOPS ücretlendirimedi| Yok | [sanal çekirdek, ayrılmış depolama ve yedekleme depolama](https://azure.microsoft.com/pricing/details/sql-database/managed/) alanı ücretlendirilir. <br/>IOPS ücretlendirilmez.| 
|**İndirim modelleri**| | [Ayrılmış örnekler](reserved-capacity-overview.md)<br/>[Azure hibrit avantajı](../azure-hybrid-benefit.md) (geliştirme ve test aboneliklerinde kullanılamaz)<br/>[Kurumsal](https://azure.microsoft.com/offers/ms-azr-0148p/) ve [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0023p/) geliştirme ve test abonelikleri| [Azure hibrit avantajı](../azure-hybrid-benefit.md) (geliştirme ve test aboneliklerinde kullanılamaz)<br/>[Kurumsal](https://azure.microsoft.com/offers/ms-azr-0148p/) ve [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0023p/) geliştirme ve test abonelikleri| [Ayrılmış örnekler](reserved-capacity-overview.md)<br/>[Azure hibrit avantajı](../azure-hybrid-benefit.md) (geliştirme ve test aboneliklerinde kullanılamaz)<br/>[Kurumsal](https://azure.microsoft.com/offers/ms-azr-0148p/) ve [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0023p/) geliştirme ve test abonelikleri|

Daha fazla bilgi için [Azure SQL veritabanı (vCore)](resource-limits-vcore-single-databases.md), [tek Azure SQL veritabanı (DTU)](resource-limits-dtu-single-databases.md), [havuza alınmış Azure SQL veritabanı (DTU)](resource-limits-dtu-single-databases.md)ve [Azure SQL yönetilen örnek](../managed-instance/resource-limits.md) sayfalarında hizmet katmanları arasındaki ayrıntılı farklar bölümüne bakın.

> [!NOTE]
> Sanal çekirdek tabanlı satın alma modelindeki hiper ölçek hizmet katmanı hakkında bilgi için bkz. [hiper ölçekli hizmet katmanı](service-tier-hyperscale.md). DTU tabanlı satın alma modeliyle sanal çekirdek tabanlı satın alma modeli karşılaştırması için bkz. [model ve kaynak satın alma](purchasing-models.md).

## <a name="data-and-log-storage"></a>Veri ve günlük depolama

Aşağıdaki faktörler, veri ve günlük dosyaları için kullanılan depolama miktarını etkiler ve Genel Amaçlı ve İş Açısından Kritik için geçerlidir. Hiper ölçekte veri ve günlük depolama hakkındaki ayrıntılar için bkz. [hyperscale hizmet katmanı](service-tier-hyperscale.md).

- Ayrılan depolama alanı, veri dosyaları (MDF) ve günlük dosyaları (LDF) tarafından kullanılır.
- Her tek veritabanı işlem boyutu, varsayılan en büyük boyut olan 32 GB olan en büyük veritabanı boyutunu destekler.
- Gerekli tek veritabanı boyutunu (MDF dosyasının boyutu) yapılandırdığınızda, LDF dosyalarını desteklemek için yüzde 30 daha fazla ek depolama alanı otomatik olarak eklenir.
- 10 GB ve desteklenen maksimum boyut arasında tek bir veritabanı boyutu seçebilirsiniz.
  - Standart veya genel amaçlı hizmet katmanlarında depolama için, boyutu 10 GB 'lik artışlarla artırın veya azaltın.
  - Premium veya iş açısından kritik hizmet katmanlarında depolama için boyutu 250 GB 'lik artışlarla artırın veya azaltın.
- Genel amaçlı hizmet katmanında, `tempdb` bağlı BIR SSD kullanır ve bu depolama maliyeti sanal çekirdek fiyatına dahildir.
- İş açısından kritik hizmet katmanında, `tempdb` eklenen SSD 'YI MDF ve LDF dosyalarıyla paylaşır ve `tempdb` depolama maliyeti sanal çekirdek fiyatına dahildir.
- SQL yönetilen örneği için depolama boyutu 32 GB 'ın katları olarak belirtilmelidir.


> [!IMPORTANT]
> MDF ve LDF dosyaları için ayrılan toplam depolama alanı için ücretlendirilirsiniz.

MDF ve LDF dosyalarınızın geçerli toplam boyutunu izlemek için [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql)kullanın. Bireysel MDF ve LDF dosyalarının geçerli boyutunu izlemek için [sys. database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql)kullanın.

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanını daraltmanız gerekebilir. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda dosya alanını yönetme](file-space-manage.md).

## <a name="backups-and-storage"></a>Yedeklemeler ve depolama

Veritabanı yedeklemeleri için depolama, SQL veritabanı ve SQL yönetilen örneği 'nin zaman içinde geri yükleme (ıNR) ve [uzun süreli saklama (LTR)](long-term-retention-overview.md) özelliklerini desteklemek için ayrılır. Bu depolama, her veritabanı için ayrı ayrı ayrılır ve iki ayrı veritabanı başına ücret olarak faturalandırılır.

- **Sür**: ayrı veritabanı yedeklemeleri, [Okuma Erişimli Coğrafi olarak yedekli (RA-GRS) depolamaya](../../storage/common/geo-redundant-design.md) otomatik olarak kopyalanır. Yeni yedeklemeler oluşturuldıkça depolama boyutu dinamik olarak artar. Depolama, haftalık tam yedeklemeler, günlük değişiklik yedeklemeleri ve 5 dakikada bir kopyalanmış olan işlem günlüğü yedeklemeleri tarafından kullanılır. Depolama alanı tüketimi, veritabanının değişim hızına ve yedeklemeler için Bekletme süresine bağlıdır. Her veritabanı için 7 ila 35 gün arasında ayrı bir bekletme süresi yapılandırabilirsiniz. Veritabanı boyutunun yüzde 100 ' una (1x) eşit olan minimum depolama miktarı ek bir ücret ödemeden sunulmaktadır. Çoğu veritabanı için, bu miktar 7 günlük yedeklemeleri depolamak için yeterlidir.
- **LTR**: 10 yıla kadar tam yedeklemelerin uzun süreli bekletmesini yapılandırma seçeneğiniz de vardır (Bu özellik [SQL yönetilen örneği için sınırlı genel önizleme](long-term-retention-overview.md#managed-instance-support)aşamasındadır. Bir LTR ilkesi ayarlarsanız, bu yedeklemeler RA-GRS depolama alanında otomatik olarak depolanır, ancak yedeklemelerin ne sıklıkla kopyalanacağını kontrol edebilirsiniz. Farklı uyumluluk gereksinimlerini karşılamak için haftalık, aylık ve/veya yıllık yedeklemeler için farklı saklama süreleri seçebilirsiniz. Seçtiğiniz yapılandırma, LTR yedeklemeleri için ne kadar Depolama kullanılacağını belirler. LTR depolama maliyetini tahmin etmek için, LTR Fiyatlandırma hesaplayıcısı ' nı kullanabilirsiniz. Daha fazla bilgi için bkz. [SQL veritabanı uzun süreli saklama](long-term-retention-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

Genel amaçlı ve iş açısından kritik hizmet katmanlarında bulunan belirli işlem ve depolama boyutları hakkında daha fazla bilgi için bkz.: 

- [Azure SQL veritabanı Için sanal çekirdek tabanlı kaynak sınırları](resource-limits-vcore-single-databases.md).
- [Azure SQL veritabanı 'nda havuza alınmış veritabanları Için sanal çekirdek tabanlı kaynak sınırları](resource-limits-vcore-elastic-pools.md).
- [Azure SQL yönetilen örneği Için sanal çekirdek tabanlı kaynak sınırları](../managed-instance/resource-limits.md). 

