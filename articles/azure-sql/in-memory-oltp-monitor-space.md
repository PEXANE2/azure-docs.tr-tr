---
title: XTP bellek Içi depolamayı izleme
description: XTP bellek Içi depolama kullanımı, kapasiteyi tahmin edin ve izleyin. Kapasite çözme hatası 41823
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: how-to
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: sstein
ms.date: 01/25/2019
ms.openlocfilehash: 5717c2479c1d894117bae44826a814c3cfeaa98a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96493398"
---
# <a name="monitor-in-memory-oltp-storage-in-azure-sql-database-and-azure-sql-managed-instance"></a>Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde In-Memory OLTP depolama alanını izleme
[!INCLUDE[appliesto-sqldb-sqlmi](includes/appliesto-sqldb-sqlmi.md)]

[Bellek ıçı OLTP](in-memory-oltp-overview.md)kullanılırken, bellek için iyileştirilmiş tablolardaki ve tablo değişkenlerinin VERILERI In-Memory OLTP depolamada bulunur.

## <a name="determine-whether-data-fits-within-the-in-memory-oltp-storage-cap"></a>Verilerin In-Memory OLTP depolama Cap içinde uygun olup olmadığını belirleme

Farklı hizmet katmanlarının depolama büyük harflerini belirleme. Her Premium ve İş Açısından Kritik hizmet katmanının en yüksek In-Memory OLTP depolama boyutu vardır.

- [DTU tabanlı kaynak limitleri-tek veritabanı](database/resource-limits-dtu-single-databases.md)
- [DTU tabanlı kaynak limitleri-elastik havuzlar](database/resource-limits-dtu-elastic-pools.md)
- [Sanal çekirdek tabanlı kaynak limitleri-tek veritabanları](database/resource-limits-vcore-single-databases.md)
- [Sanal çekirdek tabanlı kaynak limitleri-elastik havuzlar](database/resource-limits-vcore-elastic-pools.md)
- [Sanal çekirdek tabanlı kaynak limitleri-yönetilen örnek](managed-instance/resource-limits.md)

Bellek için iyileştirilmiş bir tablo için bellek gereksinimleri tahmin etmek, Azure SQL veritabanı ve Azure SQL yönetilen örneği gibi SQL Server aynı şekilde çalışmaktadır. [Tahmini bellek gereksinimlerini](/sql/relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables)gözden geçirmek için birkaç dakikanızı alın.

Tablo ve tablo değişkeni satırları, ayrıca dizinler, en fazla Kullanıcı veri boyutuna doğru sayılır. Ayrıca, ALTER TABLE 'ın tüm tablo ve dizinlerinin yeni bir sürümünü oluşturmak için yeterli yere ihtiyacı vardır.

Bu sınır aşıldığında, INSERT ve Update işlemleri, Azure SQL veritabanı 'nda tek veritabanları ve Azure SQL yönetilen örneğindeki veritabanları için 41823 hatası ve Azure SQL veritabanı 'nda esnek havuzlar için hata 41840 ile başarısız olabilir. Bu noktada, belleği geri kazanmak için verileri silmeniz veya hizmet katmanını ya da veritabanınızın işlem boyutunu yükseltmeniz gerekir.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı

Bellek Içi depolama kullanımını, [Azure Portal](https://portal.azure.com/)işlem boyutunuz için depolama ucunun yüzdesi olarak izleyebilirsiniz:

1. Veritabanı dikey penceresinde kaynak kullanımı kutusunu bulun ve Düzenle ' ye tıklayın.
2. Ölçümü seçin `In-Memory OLTP Storage percentage` .
3. Uyarı eklemek için kaynak kullanımı kutusuna tıklayarak ölçüm dikey penceresini açın ve uyarı Ekle ' ye tıklayın.

Ya da bellek Içi depolama kullanımını göstermek için aşağıdaki sorguyu kullanın:

```sql
    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats
```

## <a name="correct-out-of-in-memory-oltp-storage-situations---errors-41823-and-41840"></a>Bellek dışı OLTP depolama durumlarını düzeltme-hatalar 41823 ve 41840

Veritabanınızda In-Memory OLTP depolama Cap 'e vurarak, ekleme, GÜNCELLEŞTIRME, DEĞIŞTIRME ve oluşturma işlemlerinde hata iletisi 41823 (tek veritabanları için) veya 41840 hatası (elastik havuzlar için) ile sonuçlanır. Her iki hata da etkin işlemin durmasına neden olur.

41823 ve 41840 hata iletileri, veritabanı veya havuzdaki bellek için iyileştirilmiş tabloların ve tablo değişkenlerinin en büyük In-Memory OLTP depolama boyutuna ulaştığından emin olduğunu gösterir.

Bu hatayı çözmek için şunlardan birini yapın:

- Bellek için iyileştirilmiş tablolardaki verileri silme, büyük olasılıkla verileri geleneksel, disk tabanlı tablolara boşaltma; veya
- Bellek için iyileştirilmiş tablolarda tutmanız gereken veriler için hizmet katmanını, bellek içi yeterli depolama alanı ile bir birine yükseltin.

> [!NOTE]
> Nadir durumlarda, 41823 ve 41840 hataları geçici olabilir, yani OLTP depolaması In-Memory kullanılabilir ve işlem başarılı bir şekilde yeniden denenmelidir. Bu nedenle, hem kullanılabilir In-Memory OLTP depolama alanını hem de ilk kez 41823 veya 41840 hatasıyla karşılaşıldığında yeniden denemek önerilir. Yeniden deneme mantığı hakkında daha fazla bilgi için bkz. [In-Memory OLTP Ile çakışma algılama ve yeniden deneme mantığı](/sql/relational-databases/In-memory-oltp/transactions-with-memory-optimized-tables#conflict-detection-and-retry-logic).

## <a name="next-steps"></a>Sonraki adımlar

İzleme Kılavuzu için bkz. [dinamik yönetim görünümlerini kullanarak izleme](database/monitoring-with-dmvs.md).