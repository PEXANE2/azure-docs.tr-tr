---
title: DTU kaynak sınırları tek veritabanları
description: Bu sayfada, Azure SQL Veritabanı'ndaki tek veritabanları için bazı yaygın DTU kaynak sınırları açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/20/2019
ms.openlocfilehash: a4c435b4874301fe6fb804085c5b265954cd4f5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256321"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-purchasing-model"></a>DTU satın alma modeli kullanıldığında tek veritabanları için kaynak sınırları

Bu makalede, DTU satın alma modelini kullanarak Azure SQL Veritabanı tek veritabanları için ayrıntılı kaynak sınırları sağlar.

Elastik havuzlar için DTU satın alma modeli kaynak sınırları için [DTU kaynak limitlerine bakın - elastik havuzlar.](sql-database-dtu-resource-limits-elastic-pools.md) vCore kaynak sınırları için [vCore kaynak limitlerine](sql-database-vcore-resource-limits-single-databases.md) bakın - tek veritabanları ve [vCore kaynak sınırları - elastik havuzlar.](sql-database-vcore-resource-limits-elastic-pools.md) Farklı satın alma modelleri ile ilgili daha fazla bilgi için Satın [Alma modelleri ve hizmet katmanlarına](sql-database-purchase-models.md)bakın.

## <a name="single-database-storage-sizes-and-compute-sizes"></a>Tek veritabanı: Depolama boyutları ve işlem boyutları

Aşağıdaki tablolar, her hizmet katmanı ve işlem boyutunda tek bir veritabanı için kullanılabilir kaynakları gösterir. [Azure portalı,](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server) [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-sql-database-servers-and-single-databases), [PowerShell,](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases) [Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases)veya [REST API'yi](sql-database-single-databases-manage.md#rest-api-manage-sql-database-servers-and-single-databases)kullanarak tek bir veritabanı için hizmet katmanını, işlem boyutunu ve depolama tutarını ayarlayabilirsiniz.

> [!IMPORTANT]
> Kılavuz ve dikkatleri ölçeklendirmek için [bkz.](sql-database-single-database-scale.md)

### <a name="basic-service-tier"></a>Temel hizmet katmanı

| **İşlem boyutu** | **Temel** |
| :--- | --: |
| Maks. DTU | 5 |
| Dahil edilen depolama alanı (GB) | 2 |
| Maksimum depolama seçenekleri (GB) | 2 |
| Maks. bellek içi OLTP depolama alanı (GB) |Yok |
| Maksimum eşzamanlı çalışanlar (istekler) | 30 |
| Maks. eş zamanlı oturum | 300 |
|||

> [!IMPORTANT]
> Temel hizmet katmanı birden az vCore (CPU) sağlar.  CPU yoğun iş yükleri için, S3 veya daha büyük bir hizmet katmanı önerilir. 
>
>Veri depolama ile ilgili olarak, Temel hizmet katmanı Standart Sayfa Blobs yerleştirilir. Standart Sayfa Blob'ları sabit disk sürücüsü (HDD) tabanlı depolama ortamını kullanır ve performans değişkenliğine daha az duyarlı geliştirme, sınama ve seyrek erişilen diğer iş yükleri için en uygun olanlardır.
>

### <a name="standard-service-tier"></a>Standart hizmet katmanı

| **İşlem boyutu** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|
| Maks. DTU | 10 | 20 | 50 | 100 |
| Dahil edilen depolama alanı (GB) | 250 | 250 | 250 | 250 |
| Maksimum depolama seçenekleri (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |
| Maksimum eşzamanlı çalışanlar (istekler)| 60 | 90 | 120 | 200 |
| Maks. eş zamanlı oturum |600 | 900 | 1200 | 2400 |
||||||

> [!IMPORTANT]
> Standart S0, S1 ve S2 katmanları birden az vCore (CPU) sağlar.  CPU yoğun iş yükleri için, S3 veya daha büyük bir hizmet katmanı önerilir. 
>
>Veri depolama ile ilgili olarak, Standart S0 ve S1 hizmet katmanları Standart Sayfa Blobs yerleştirilir. Standart Sayfa Blob'ları sabit disk sürücüsü (HDD) tabanlı depolama ortamını kullanır ve performans değişkenliğine daha az duyarlı geliştirme, sınama ve seyrek erişilen diğer iş yükleri için en uygun olanlardır.
>

### <a name="standard-service-tier-continued"></a>Standart hizmet katmanı (devam)

| **İşlem boyutu** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|
| Maks. DTU | 200 | 400 | 800 | 1600 | 3000 |
| Dahil edilen depolama alanı (GB) | 250 | 250 | 250 | 250 | 250 |
| Maksimum depolama seçenekleri (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |Yok |
| Maksimum eşzamanlı çalışanlar (istekler)| 400 | 800 | 1600 | 3200 |6000 |
| Maks. eş zamanlı oturum |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Premium hizmet katmanı

| **İşlem boyutu** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** |
| :--- |---:|---:|---:|---:|---:|---:|
| Maks. DTU | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Dahil edilen depolama alanı (GB) | 500 | 500 | 500 | 500 | 4096* | 4096* |
| Maksimum depolama seçenekleri (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096* | 4096* |
| Maks. bellek içi OLTP depolama alanı (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Maksimum eşzamanlı çalışanlar (istekler)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||

\*1024 GB'dan 4096 GB'a kadar 256 GB'lık artışlarla

> [!IMPORTANT]
> Premium katmanında 1 TB'den fazla depolama şu anda Çin Doğu, Çin Kuzey, Almanya Orta, Almanya Kuzeydoğu, Batı Orta ABD, ABD DoD bölgeleri ve ABD Hükümeti Merkezi hariç tüm bölgelerde kullanılabilir. Bu bölgelerde, Premium katmandaki depolama limiti 1 TB ile sınırlıdır.  Daha fazla bilgi için [P11-P15 geçerli sınırlamaları'na](sql-database-single-database-scale.md#p11-and-p15-constraints-when-max-size-greater-than-1-tb)bakın.  
> [!NOTE]
> Limitler için `tempdb` [tempdb limitlerine](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için vCore kaynak sınırları [için, vCore satın alma modelini kullanarak tek veritabanları için kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md) bakın
- Elastik havuzlar için vCore kaynak sınırları için, [vCore satın alma modelini kullanarak elastik havuzlar için kaynak sınırlarına](sql-database-vcore-resource-limits-elastic-pools.md) bakın
- Elastik havuzlar için DTU kaynak sınırları için, [DTU satın alma modelini kullanan elastik havuzlar için kaynak sınırlarına](sql-database-dtu-resource-limits-elastic-pools.md) bakın
- Yönetilen örnekler için kaynak sınırları için [yönetilen örnek kaynak sınırlarına](sql-database-managed-instance-resource-limits.md)bakın.
- Genel Azure sınırları hakkında bilgi için Azure [abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md)abakın.
- Veritabanı sunucusundaki kaynak sınırları hakkında bilgi için, sunucu ve abonelik düzeylerindeki sınırlar hakkında bilgi için [bir SQL Veritabanı sunucusundaki kaynak sınırlarına genel bakış](sql-database-resource-limits-database-server.md) bakın.
