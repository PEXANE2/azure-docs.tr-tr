---
title: XTP Bellek İçi depolamayı izleyin
description: XTP Bellek İçi depolama kullanımını, kapasitesini tahmin edin ve izleyin; kapasite hatalarını giderme 41823
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: genemi
ms.date: 01/25/2019
ms.openlocfilehash: 22ff83b1ccd009624082e45073123a45006df70f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79209424"
---
# <a name="monitor-in-memory-oltp-storage"></a>Bellek İçi OLTP depolamasını izleyin

[In-Memory OLTP](sql-database-in-memory.md)kullanırken, bellek için optimize edilmiş tablolardaki veriler ve tablo değişkenleri Bellek Içi OLTP depolama alanında bulunur. Her Premium ve Business Critical hizmet katmanı maksimum Bellek IÇI OLTP depolama boyutuna sahiptir. Bkz. [DTU tabanlı kaynak sınırları - tek veritabanı,](sql-database-dtu-resource-limits-single-databases.md) [DTU tabanlı kaynak sınırları - elastik havuzlar,](sql-database-dtu-resource-limits-elastic-pools.md)[vCore tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [vCore tabanlı kaynak sınırları - elastik havuzlar.](sql-database-vcore-resource-limits-elastic-pools.md)

Bu sınır aşıldıktan sonra, tek veritabanları için 41823 hatası, elastik havuzlar için hata 41840 ile ekleme ve güncelleştirme işlemleri başarısız lığa başlayabilir. Bu noktada, belleği geri almak için verileri silmeniz veya veritabanınızın hizmet katmanını veya işlem boyutunu yükseltmeniz gerekir.

## <a name="determine-whether-data-fits-within-the-in-memory-oltp-storage-cap"></a>Verilerin Bellek İçi OLTP depolama kapağına uygun olup olmadığını belirleme

Farklı hizmet katmanlarının depolama kapaklarını belirleyin. Bkz. [DTU tabanlı kaynak sınırları - tek veritabanı,](sql-database-dtu-resource-limits-single-databases.md) [DTU tabanlı kaynak sınırları - elastik havuzlar,](sql-database-dtu-resource-limits-elastic-pools.md)[vCore tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [vCore tabanlı kaynak sınırları - elastik havuzlar.](sql-database-vcore-resource-limits-elastic-pools.md)

Bellek için optimize edilmiş bir tablonun bellek gereksinimlerini tahmin etmek, SQL Server için Azure SQL Veritabanı'nda olduğu gibi çalışır. [MSDN](https://msdn.microsoft.com/library/dn282389.aspx)bu makaleyi gözden geçirmek için birkaç dakika ayırın.

Tablo ve tablo değişken satırlarının yanı sıra dizinler, maksimum kullanıcı veri boyutuna doğru sayılır. Buna ek olarak, ALTER TABLE tüm tablo ve dizinleri yeni bir sürümünü oluşturmak için yeterli oda gerekir.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
[Azure portalında,](https://portal.azure.com/)kullanıcı nızın hesaplama boyutuiçin depolama kapağının yüzdesi olarak Bellek içi depolama kullanımını izleyebilirsiniz: 

1. Veritabanı bıçağında, Kaynak kullanım kutusunu bulun ve Edit'e tıklayın.
2. Metrik `In-Memory OLTP Storage percentage`seçin.
3. Uyarı eklemek için, Metrik bıçağı açmak için Kaynak Kullanımı kutusunu tıklatın ve ardından uyarı ekle'ye tıklayın.

Veya Bellek içi depolama kullanımını göstermek için aşağıdaki sorguyu kullanın:

```sql
    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats
```

## <a name="correct-out-of-in-memory-oltp-storage-situations---errors-41823-and-41840"></a>Bellek dışı OLTP depolama durumlarını düzeltin - Hatalar 41823 ve 41840

Veritabanınızdaki Bellek İçi OLTP depolama kapağına vurmak, INSERT, UPDATE, ALTER ve CREATE işlemlerinde hata iletisi 41823 (tek veritabanları için) veya 41840 (elastik havuzlar için) hatasıyla sonuçlanır. Her iki hata da etkin işlemin iptaline neden olur.

Hata iletileri 41823 ve 41840, veritabanı veya havuzdaki bellek için optimize edilmiş tablolar ve tablo değişkenlerinin en yüksek Bellek Içi OLTP depolama boyutuna ulaştığını gösterir.

Bu hatayı gidermek için aşağıdakileri de:

* Bellek için optimize edilmiş tablolardaki verileri silmek, verileri geleneksel, disk tabanlı tablolara boşaltma potansiyeline göre; Veya
* Hizmet katmanını, bellek için optimize edilmiş tablolarda tutmanız gereken veriler için yeterli bellek içi depolama alanına sahip bir e yükseltin.

> [!NOTE] 
> Nadir durumlarda, 41823 ve 41840 hataları geçici olabilir, bu da yeterli kullanılabilir Bellek IÇI OLTP depolama alanı olduğu anlamına gelir ve işlemi yeniden denemek başarılı olur. Bu nedenle hem genel olarak mevcut Olan Bellek İçi OLTP depolamasını izlemenizi hem de 41823 veya 41840 hatasıyla ilk karşılaştığınızda yeniden denemenizi öneririz. Yeniden deneme mantığı hakkında daha fazla bilgi için, Bellek [IÇI OLTP ile Çakışma Algılama ve Yeniden İşleme Mantığı'na](https://docs.microsoft.com/sql/relational-databases/In-memory-oltp/transactions-with-memory-optimized-tables#conflict-detection-and-retry-logic)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Kılavuzu izlemek için [dinamik yönetim görünümlerini kullanarak Azure SQL Veritabanını İzleme'ye](sql-database-monitoring-with-dmvs.md)bakın.
