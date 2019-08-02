---
title: Azure SQL veritabanı-genel amaçlı ve iş açısından kritik | Microsoft Docs
description: Bu makalede, sanal çekirdek tabanlı satın alma modelindeki genel amaçlı ve iş açısından kritik hizmet katmanları ele alınmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 02/23/2019
ms.openlocfilehash: decb4428321d5083d6ba7af134e223eb2fa5a912
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566700"
---
# <a name="azure-sql-database-service-tiers"></a>Azure SQL veritabanı hizmet katmanları

Azure SQL veritabanı, bir altyapı hatası olsa bile yüzde 99,99 kullanılabilirlik sağlamak için bulut ortamı için ayarlanmış SQL Server veritabanı altyapısı mimarisini temel alır. Azure SQL veritabanı 'nda, her biri farklı bir mimari modeliyle üç hizmet katmanı kullanılır. Bu hizmet katmanları şunlardır:

- [Genel amaçlı](sql-database-service-tier-general-purpose.md), genellikle genel iş yükleri için tasarlanmıştır.
- Tek bir okunabilir çoğaltmayla düşük gecikmeli iş yükleri için tasarlanan [iş açısından kritik](sql-database-service-tier-business-critical.md).
- Birden çok okunabilir çoğaltmalarla çok büyük veritabanları (100 TB 'a kadar) için tasarlanan [hiper ölçek](sql-database-service-tier-hyperscale.md).

Bu makalede, sanal çekirdek tabanlı satın alma modelindeki genel amaçlı ve iş açısından kritik hizmet katmanlarına yönelik depolama ve yedekleme konuları ele alınmaktadır.

> [!NOTE]
> Sanal çekirdek tabanlı satın alma modelindeki hiper ölçek hizmet katmanı hakkında bilgi için bkz. [hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md). DTU tabanlı satın alma modeliyle sanal çekirdek tabanlı satın alma modeli karşılaştırması için bkz. [Azure SQL veritabanı satın alma modelleri ve kaynakları](sql-database-purchase-models.md).

## <a name="data-and-log-storage"></a>Veri ve günlük depolama

Aşağıdaki faktörler, veri ve günlük dosyaları için kullanılan depolama miktarını etkiler:

- Ayrılan depolama alanı, veri dosyaları (MDF) ve günlük dosyaları (LDF) tarafından kullanılır.
- Her tek veritabanı işlem boyutu, varsayılan en büyük boyut olan 32 GB olan en büyük veritabanı boyutunu destekler.
- Gerekli tek veritabanı boyutunu (MDF dosyasının boyutu) yapılandırdığınızda, LDF dosyalarını desteklemek için yüzde 30 daha fazla ek depolama alanı otomatik olarak eklenir.
- SQL veritabanı yönetilen örneği için depolama boyutu 32 GB 'ın katları olarak belirtilmelidir.
- 10 GB ve desteklenen maksimum boyut arasında tek bir veritabanı boyutu seçebilirsiniz.
  - Standart veya genel amaçlı hizmet katmanlarında depolama için, boyutu 10 GB 'lik artışlarla artırın veya azaltın.
  - Premium veya iş açısından kritik hizmet katmanlarında depolama için boyutu 250 GB 'lik artışlarla artırın veya azaltın.
- Genel amaçlı hizmet katmanında, `tempdb` bağlı bir SSD kullanır ve bu depolama maliyeti sanal çekirdek fiyatına dahildir.
- İş açısından kritik hizmet katmanında `tempdb` , eklenen SSD 'yi MDF ve LDF dosyalarıyla paylaşır `tempdb` ve depolama maliyeti sanal çekirdek fiyatına dahildir.

> [!IMPORTANT]
> MDF ve LDF dosyaları için ayrılan toplam depolama alanı için ücretlendirilirsiniz.

MDF ve LDF dosyalarınızın geçerli toplam boyutunu izlemek için [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql)kullanın. Tek tek MDF ve LDF dosyalarının geçerli boyutunu izlemek için [sys. database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql)kullanın.

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Yedeklemeler ve depolama

Veritabanı yedeklemeleri için depolama, SQL veritabanı 'nın zaman içinde geri yükleme (ıNR) ve [uzun süreli saklama (LTR)](sql-database-long-term-retention.md) özelliklerini desteklemek için ayrılır. Bu depolama, her veritabanı için ayrı ayrı ayrılır ve iki ayrı veritabanı başına ücret olarak faturalandırılır.

- **SÜR**: Ayrı veritabanı yedeklemeleri, [Okuma Erişimli Coğrafi olarak yedekli (RA-GRS) depolamaya](../storage/common/storage-designing-ha-apps-with-ragrs.md) otomatik olarak kopyalanır. Yeni yedeklemeler oluşturuldıkça depolama boyutu dinamik olarak artar. Depolama, haftalık tam yedeklemeler, günlük değişiklik yedeklemeleri ve 5 dakikada bir kopyalanmış olan işlem günlüğü yedeklemeleri tarafından kullanılır. Depolama alanı tüketimi, veritabanının değişim hızına ve yedeklemeler için Bekletme süresine bağlıdır. Her veritabanı için 7 ila 35 gün arasında ayrı bir bekletme süresi yapılandırabilirsiniz. Veritabanı boyutunun yüzde 100 ' una (1x) eşit olan minimum depolama miktarı ek bir ücret ödemeden sunulmaktadır. Çoğu veritabanı için, bu miktar 7 günlük yedeklemeleri depolamak için yeterlidir.
- **LTR**: SQL veritabanı, 10 yıla kadar tam yedeklemelerin uzun süreli bekletmesini yapılandırma seçeneği sunar. Bir LTR ilkesi ayarlarsanız, bu yedeklemeler RA-GRS depolama alanında otomatik olarak depolanır, ancak yedeklemelerin ne sıklıkla kopyalanacağını kontrol edebilirsiniz. Farklı uyumluluk gereksinimlerini karşılamak için haftalık, aylık ve/veya yıllık yedeklemeler için farklı saklama süreleri seçebilirsiniz. Seçtiğiniz yapılandırma, LTR yedeklemeleri için ne kadar Depolama kullanılacağını belirler. LTR depolama maliyetini tahmin etmek için, LTR Fiyatlandırma hesaplayıcısı ' nı kullanabilirsiniz. Daha fazla bilgi için bkz. [SQL veritabanı uzun süreli saklama](sql-database-long-term-retention.md).

## <a name="next-steps"></a>Sonraki adımlar

- Genel amaçlı ve iş açısından kritik hizmet katmanlarında tek bir veritabanı için kullanılabilen belirli işlem boyutları ve depolama boyutları hakkında ayrıntılı bilgi için bkz. [SQL veritabanı sanal çekirdek tabanlı kaynak sınırları tek veritabanları için](sql-database-vcore-resource-limits-single-databases.md).
- Genel amaçlı ve iş açısından kritik hizmet katmanlarında elastik havuzlar için kullanılabilen belirli işlem boyutları ve depolama boyutları hakkında daha fazla bilgi için bkz. [elastik havuzlar Için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md).
