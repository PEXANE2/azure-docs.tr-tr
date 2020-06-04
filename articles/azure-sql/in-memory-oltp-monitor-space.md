---
title: XTP bellek Içi depolamayı izleme
description: XTP bellek Içi depolama kullanımı, kapasiteyi tahmin edin ve izleyin. Kapasite çözme hatası 41823
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: genemi
ms.date: 01/25/2019
ms.openlocfilehash: a4747fcd3a68c91e10d13a03adcbc4930bd9d759
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84345266"
---
# <a name="monitor-in-memory-oltp-storage-in-azure-sql-database-and-azure-sql-managed-instance"></a>Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde bellek Içi OLTP depolama alanını izleme
[!INCLUDE[appliesto-sqldb-sqlmi](includes/appliesto-sqldb-sqlmi.md)]

Bellek içi [OLTP](in-memory-oltp-overview.md)kullanılırken bellek için iyileştirilmiş tablolardaki ve tablo değişkenlerinin verileri bellek içi OLTP depolaması 'nda bulunur.

## <a name="determine-whether-data-fits-within-the-in-memory-oltp-storage-cap"></a>Verilerin bellek Içi OLTP depolama Cap içinde uygun olup olmadığını belirleme

Farklı hizmet katmanlarının depolama büyük harflerini belirleme. Her Premium ve İş Açısından Kritik hizmet katmanının en fazla bellek Içi OLTP depolama boyutu vardır.

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

Veritabanınızdaki bellek Içi OLTP depolama Cap 'e ulaşarak, ekleme, GÜNCELLEŞTIRME, DEĞIŞTIRME ve oluşturma işlemlerinde hata iletisi 41823 (tek veritabanları için) veya 41840 (elastik havuzlar için) hatası ile başarısız olur. Her iki hata da etkin işlemin durmasına neden olur.

41823 ve 41840 hata iletileri, veritabanı veya havuzdaki bellek için iyileştirilmiş tabloların ve tablo değişkenlerinin maksimum bellek Içi OLTP depolama boyutuna ulaştığından emin olduğunu gösterir.

Bu hatayı çözmek için şunlardan birini yapın:

- Bellek için iyileştirilmiş tablolardaki verileri silme, büyük olasılıkla verileri geleneksel, disk tabanlı tablolara boşaltma; veya
- Bellek için iyileştirilmiş tablolarda tutmanız gereken veriler için hizmet katmanını, bellek içi yeterli depolama alanı ile bir birine yükseltin.

> [!NOTE]
> Nadir durumlarda, 41823 ve 41840 hataları geçici olabilir, yani bellek Içi OLTP depolama alanı üzerinde yeterli kullanılabilir ve işlem başarılı bir şekilde yeniden denenmelidir. Bu nedenle, her iki bellek Içi OLTP depolama alanını izlemenizi ve ilk olarak 41823 veya 41840 hatası ile karşılaşıldığında yeniden denemeyi öneririz. Yeniden deneme mantığı hakkında daha fazla bilgi için bkz. [bellek ıçı OLTP Ile çakışma algılama ve yeniden deneme mantığı](https://docs.microsoft.com/sql/relational-databases/In-memory-oltp/transactions-with-memory-optimized-tables#conflict-detection-and-retry-logic).

## <a name="next-steps"></a>Sonraki adımlar

İzleme Kılavuzu için bkz. [dinamik yönetim görünümlerini kullanarak izleme](database/monitoring-with-dmvs.md).
