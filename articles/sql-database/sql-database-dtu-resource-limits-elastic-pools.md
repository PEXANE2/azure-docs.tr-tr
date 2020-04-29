---
title: DTU kaynağı Esnek havuzları sınırlar
description: Bu sayfada Azure SQL veritabanı 'nda esnek havuzlar için bazı yaygın DTU kaynak sınırları açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
ms.date: 03/14/2019
ms.openlocfilehash: 76085dc29d40944cf704dbc5efc578b3314f499a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81419247"
---
# <a name="resources-limits-for-elastic-pools-using-the-dtu-purchasing-model"></a>DTU satın alma modelini kullanarak elastik havuzlar için kaynak limitleri

Bu makalede, DTU satın alma modelini kullanarak Azure SQL veritabanı elastik havuzları ve havuza alınmış veritabanları için ayrıntılı kaynak sınırları sağlanmaktadır.

Tek veritabanlarına yönelik DTU satın alma modeli kaynak sınırları için bkz. [DTU kaynak limitleri-tek veritabanları](sql-database-vcore-resource-limits-elastic-pools.md). VCore kaynak sınırları için bkz. [Vcore kaynak limitleri-tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [sanal çekirdek kaynak limitleri-elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md).

## <a name="elastic-pool-storage-sizes-and-compute-sizes"></a>Elastik havuz: depolama boyutları ve işlem boyutları

SQL veritabanı elastik havuzlar için aşağıdaki tablolarda, her bir hizmet katmanında ve işlem boyutunda kullanılabilen kaynaklar gösterilmektedir. Hizmet katmanını, işlem boyutunu ve depolama miktarını [Azure Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [POWERSHELL](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases)veya [REST API](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases)kullanarak ayarlayabilirsiniz.

> [!IMPORTANT]
> Ölçeklendirme Kılavuzu ve değerlendirmeleri için bkz. [elastik havuz ölçekleme](sql-database-elastic-pool-scale.md)
> [!NOTE]
> Elastik havuzlardaki ayrı veritabanlarının kaynak sınırları, DTU 'lar ve hizmet katmanını temel alan havuzlar dışındaki tek veritabanları için genellikle aynıdır. Örneğin, bir S2 veritabanının en fazla eşzamanlı çalışanları 120 çalışandır. Bu nedenle, standart havuzdaki bir veritabanı için en fazla eş zamanlı çalışan, havuzdaki veritabanı başına maksimum DTU değeri 50 DTU ise (S2 ile eşdeğerdir) 120 çalışanlardır.

> [!NOTE]
> Aşağıdaki tabloların her birinde havuz başına depolama kaynağı sınırı tempdb ve günlük depolama alanı içermez.

### <a name="basic-elastic-pool-limits"></a>Temel esnek havuz sınırları

| Havuz başına eDTU | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Havuz başına dahil edilen depolama alanı (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Havuz başına en fazla depolama seçimi (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Havuz başına maks. bellek Içi OLTP depolaması (GB) | Yok | Yok | Yok | Yok | Yok | Yok | Yok | Yok |
| Havuz başına en fazla veritabanı sayısı <sup>1</sup> | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Her havuz <sup>2</sup> için maksimum eş zamanlı çalışan (istek) | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Her havuz için en fazla eşzamanlı oturum <sup>2</sup> | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Veritabanı başına en az eDTU seçimi | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Veritabanı başına en fazla eDTU seçimi | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Veritabanı başına maks. depolama alanı (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
||||||||

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="standard-elastic-pool-limits"></a>Standart esnek havuz sınırları

| Havuz başına eDTU | **50** | **100** | **200** | **300** | **400** | **800**|
|:---|---:|---:|---:| ---: | ---: | ---: |
| Havuz başına dahil edilen depolama alanı (GB) | 50 | 100 | 200 | 300 | 400 | 800 |
| Havuz başına en fazla depolama seçimi (GB) | 50, 250, 500 | 100, 250, 500, 750 | 200, 250, 500, 750, 1024 | 300, 500, 750, 1024, 1280 | 400, 500, 750, 1024, 1280, 1536 | 800, 1024, 1280, 1536, 1792, 2048 |
| Havuz başına maks. bellek Içi OLTP depolaması (GB) | Yok | Yok | Yok | Yok | Yok | Yok |
| Havuz başına en fazla veritabanı sayısı <sup>1</sup> | 100 | 200 | 500 | 500 | 500 | 500 |
| Her havuz <sup>2</sup> için maksimum eş zamanlı çalışan (istek) | 100 | 200 | 400 | 600 | 800 | 1600 |
| Her havuz için en fazla eşzamanlı oturum <sup>2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Veritabanı başına en az eDTU seçimi | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Veritabanı başına en fazla eDTU seçimi | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 |
| Veritabanı başına maks. depolama alanı (GB) | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="standard-elastic-pool-limits-continued"></a>Standart esnek havuz limitleri (devam)

| Havuz başına eDTU | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Havuz başına dahil edilen depolama alanı (GB) | 1200 | 1600 | 2000 | 2500 | 3000 |
| Havuz başına en fazla depolama seçimi (GB) | 1200, 1280, 1536, 1792, 2048, 2304, 2560 | 1600, 1792, 2048, 2304, 2560, 2816, 3072 | 2000, 2048, 2304, 2560, 2816, 3072, 3328, 3584 | 2500, 2560, 2816, 3072, 3328, 3584, 3840, 4096 | 3000, 3072, 3328, 3584, 3840, 4096 |
| Havuz başına maks. bellek Içi OLTP depolaması (GB) | Yok | Yok | Yok | Yok | Yok |
| Havuz başına en fazla veritabanı sayısı <sup>1</sup> | 500 | 500 | 500 | 500 | 500 |
| Her havuz <sup>2</sup> için maksimum eş zamanlı çalışan (istek) | 2400 | 3200 | 4000 | 5000 | 6000 |
| Her havuz için en fazla eşzamanlı oturum <sup>2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| Veritabanı başına en az eDTU seçimi | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Veritabanı başına en fazla eDTU seçimi | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Veritabanı başına en fazla depolama seçimi (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="premium-elastic-pool-limits"></a>Premium esnek havuz sınırları

| Havuz başına eDTU | **125** | **250** | **500** | **1000** | **1500**|
|:---|---:|---:|---:| ---: | ---: |
| Havuz başına dahil edilen depolama alanı (GB) | 250 | 500 | 750 | 1024 | 1536 |
| Havuz başına en fazla depolama seçimi (GB) | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 1536 |
| Havuz başına maks. bellek Içi OLTP depolaması (GB) | 1 | 2 | 4 | 10 | 12 |
| Havuz başına en fazla veritabanı sayısı <sup>1</sup> | 50 | 100 | 100 | 100 | 100 |
| Havuz başına en fazla eş zamanlı çalışan (istek) <sup>2</sup> | 200 | 400 | 800 | 1600 | 2400 |
| Her havuz için en fazla eşzamanlı oturum <sup>2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| Veritabanı başına Min. eDTU | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000|
| Veritabanı başına Maks. eDTU | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000|
| Veritabanı başına maks. depolama alanı (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="premium-elastic-pool-limits-continued"></a>Premium esnek havuz limitleri (devam)

| Havuz başına eDTU | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: |
| Havuz başına dahil edilen depolama alanı (GB) | 2048 | 2560 | 3072 | 3548 | 4096 |
| Havuz başına en fazla depolama seçimi (GB) | 2048 | 2560 | 3072 | 3548 | 4096|
| Havuz başına maks. bellek Içi OLTP depolaması (GB) | 16 | 20 | 24 | 28 | 32 |
| Havuz başına en fazla veritabanı sayısı <sup>1</sup> | 100 | 100 | 100 | 100 | 100 |
| Her havuz <sup>2</sup> için maksimum eş zamanlı çalışan (istek) | 3200 | 4000 | 4800 | 5600 | 6400 |
| Her havuz için en fazla eşzamanlı oturum <sup>2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| Veritabanı başına en az eDTU seçimi | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| Veritabanı başına en fazla eDTU seçimi | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| Veritabanı başına maks. depolama alanı (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

> [!IMPORTANT]
> Premium katmanda 1 TB 'den fazla depolama alanı şu anda tüm bölgelerde kullanılabilir: Çin Doğu, Çin Kuzey, Almanya Orta, Almanya Kuzeydoğu, Orta Batı ABD, US DoD bölgeleri ve ABD Devlet Merkezi. Bu bölgelerde, Premium katmanda en fazla depolama alanı 1 TB ile sınırlıdır.  Daha fazla bilgi için bkz. [P11-P15 geçerli sınırlamalar](sql-database-single-database-scale.md#p11-and-p15-constraints-when-max-size-greater-than-1-tb).

Bir elastik havuzun tüm DTU’ları kullanılırsa, sorguları işlemek üzere havuzdaki her bir veritabanı eşit miktarda kaynak alır. SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Elastik havuz kaynak paylaşımı eşitliği, veritabanı başına DTU dakikası sıfır olmayan bir değere ayarlandığında her bir veritabanı için garanti edilen herhangi bir kaynak miktarına ek niteliktedir.

> [!NOTE]
> Sınırlar `tempdb` için bkz. [tempdb sınırları](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database).

### <a name="database-properties-for-pooled-databases"></a>Havuza alınmış veritabanları için veritabanı özellikleri

Aşağıdaki tabloda, havuza alınmış veritabanlarının özellikleri açıklanmaktadır.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına Maks. eDTU |Havuzdaki diğer veritabanlarının kullanımına göre mevcutsa, havuzdaki herhangi bir veritabanının kullanabileceği en fazla eDTU sayısı. Veritabanı başına en fazla eDTU, veritabanı için kaynak garantisi anlamına gelmez. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımının en üst seviyeye çıktığı durumlarla baş edebilmek için veritabanı başına en fazla eDTU sayısını yeterince yüksek bir değere ayarlayın. Havuz genellikle tüm veritabanlarının eşzamanlı olarak en üst kullanım seviyesine çıkmadığı sıcak ve soğuk kullanım modellerini varsaydığından, bir miktar aşırı ayırma beklenir. Örneğin, veritabanı başına en yüksek kullanımın 20 eDTU olduğunu ve havuzdaki 100 veritabanının yalnızca %20’sinin aynı anda en yüksek seviyeye çıktığını varsayalım. Veritabanı başına en fazla eDTU değeri 20 eDTU’ya ayarlanırsa, havuzun 5 kat fazla kullanılması ve havuz başına eDTU sayısının 400’e ayarlanması makuldür. |
| Veritabanı başına Min. eDTU |Havuzdaki herhangi bir veritabanının kullanabileceği en az eDTU sayısı garanti edilir. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına en az eDTU sayısı 0’a ayarlanabilir; bu değer aynı zamanda varsayılan değerdir. Bu özellik, 0 ile veritabanı başına ortalama eDTU kullanımı arasında bir değere ayarlanır. Havuzdaki veritabanı sayısının veritabanı başına en az eDTU sayısıyla çarpımı, havuz başına eDTU sayısını aşamaz. Örneğin, bir havuzda 20 veritabanı varsa ve veritabanı başına en az eDTU sayısı 10 eDTU’ya ayarlanırsa, havuzdaki eDTU sayısı en az 200 eDTU olmalıdır. |
| Veritabanı başına en fazla depolama |Bir havuzdaki bir veritabanı için Kullanıcı tarafından ayarlanan en büyük veritabanı boyutu. Ancak, havuza alınmış veritabanları ayrılmış havuz depolama alanını paylaşır. *Veritabanı başına* toplam en fazla depolama alanı, havuzun toplam kullanılabilir depolama *alanından*daha büyük olarak ayarlansa bile, tüm veritabanları tarafından kullanılan Toplam alan, kullanılabilir havuz sınırını aşamayacak. En fazla veritabanı boyutu, veri dosyalarının boyut üst sınırını ifade eder ve günlük dosyaları tarafından kullanılan alanı içermez. |
|||

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için sanal çekirdek kaynak sınırları için bkz [. Vcore satın alma modelini kullanarak tek veritabanları için kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md)
- Tek bir veritabanı için DTU kaynak sınırları için bkz [. DTU satın alma modelini kullanarak tek veritabanları için kaynak limitleri](sql-database-dtu-resource-limits-single-databases.md)
- Elastik havuzların sanal çekirdek kaynak sınırları için bkz [. Vcore satın alma modelini kullanarak elastik havuzlar için kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md)
- Yönetilen örnekler için kaynak sınırları için bkz. [yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md).
- Genel Azure limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md).
- Bir veritabanı sunucusundaki kaynak sınırları hakkında daha fazla bilgi için bkz. sunucu ve abonelik düzeylerindeki sınırlamalar hakkında bilgi için bkz. [SQL veritabanı sunucusundaki kaynak sınırlarına genel bakış](sql-database-resource-limits-database-server.md) .
