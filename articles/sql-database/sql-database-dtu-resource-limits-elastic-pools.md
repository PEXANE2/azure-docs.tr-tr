---
title: DTU kaynak limitleri elastik havuzları
description: Bu sayfada, Azure SQL Veritabanı'ndaki esnek havuzlar için bazı yaygın DTU kaynak sınırları açıklanmaktadır.
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
ms.openlocfilehash: 7b7ef3b6f2d400dafb28cfb7a15cf95cbbe2c457
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351021"
---
# <a name="resources-limits-for-elastic-pools-using-the-dtu-purchasing-model"></a>DTU satın alma modelini kullanan esnek havuzlar için kaynak limitleri

Bu makalede, DTU satın alma modelini kullanarak Azure SQL Veritabanı elastik havuzları ve havuzlu veritabanları için ayrıntılı kaynak sınırları sağlar.

Tek veritabanları için DTU satın alma modeli kaynak sınırları için Bkz. [DTU kaynak sınırları - tek veritabanları.](sql-database-vcore-resource-limits-elastic-pools.md) vCore kaynak sınırları için [vCore kaynak limitlerine](sql-database-vcore-resource-limits-single-databases.md) bakın - tek veritabanları ve [vCore kaynak sınırları - elastik havuzlar.](sql-database-vcore-resource-limits-elastic-pools.md)

## <a name="elastic-pool-storage-sizes-and-compute-sizes"></a>Elastik havuz: Depolama boyutları ve bilgi işlem boyutları

SQL Veritabanı elastik havuzları için aşağıdaki tablolar her hizmet katmanında ve bilgi işlem boyutunda kullanılabilir kaynakları gösterir. [Azure portalı,](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases) [PowerShell,](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases) [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases)veya [REST API'yi](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases)kullanarak hizmet katmanını, işlem boyutunu ve depolama miktarını ayarlayabilirsiniz.

> [!IMPORTANT]
> Ölçeklendirme kılavuzu ve dikkat edilmesi gerekenler için [bkz.](sql-database-elastic-pool-scale.md)
> [!NOTE]
> Elastik havuzlarda tek tek veritabanlarının kaynak sınırları genellikle DT'ler ve hizmet katmanına dayalı havuzların dışındaki tek veritabanları için aynıdır. Örneğin, bir S2 veritabanı için en fazla eşzamanlı çalışanlar 120 işçidir. Yani, standart bir havuzda bir veritabanı için maksimum eşzamanlı işçi de 120 işçi havuzda veritabanı başına maksimum DTU 50 DTU (S2 eşdeğerdir) ise.

### <a name="basic-elastic-pool-limits"></a>Temel esnek havuz sınırları

| Havuz başına eDTU | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Havuz başına depolama (GB) dahil | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Havuz başına maksimum depolama alanı seçenekleri (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Havuz başına Maksimum Bellek IÇI OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok | Yok | Yok | Yok | Yok |
| Havuz başına maksimum sayı DBs <sup>1</sup> | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>2</sup> | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Havuz başına maksimum eşzamanlı <sup>oturumlar 2</sup> | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Veritabanı başına Min eDTUs seçenekleri | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Veritabanı başına maksimum eDTUs seçenekleri | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Veritabanı başına maks. depolama alanı (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
||||||||

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

### <a name="standard-elastic-pool-limits"></a>Standart esnek havuz sınırları

| Havuz başına eDTU | **50** | **100** | **200** | **300** | **400** | **800**|
|:---|---:|---:|---:| ---: | ---: | ---: |
| Havuz başına depolama (GB) dahil | 50 | 100 | 200 | 300 | 400 | 800 |
| Havuz başına maksimum depolama alanı seçenekleri (GB) | 50, 250, 500 | 100, 250, 500, 750 | 200, 250, 500, 750, 1024 | 300, 500, 750, 1024, 1280 | 400, 500, 750, 1024, 1280, 1536 | 800, 1024, 1280, 1536, 1792, 2048 |
| Havuz başına Maksimum Bellek IÇI OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok | Yok | Yok |
| Havuz başına maksimum sayı DBs <sup>1</sup> | 100 | 200 | 500 | 500 | 500 | 500 |
| Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>2</sup> | 100 | 200 | 400 | 600 | 800 | 1600 |
| Havuz başına maksimum eşzamanlı <sup>oturumlar 2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Veritabanı başına Min eDTUs seçenekleri | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Veritabanı başına maksimum eDTUs seçenekleri | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 |
| Veritabanı başına maks. depolama alanı (GB) | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

### <a name="standard-elastic-pool-limits-continued"></a>Standart esnek havuz limitleri (devam)

| Havuz başına eDTU | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Havuz başına depolama (GB) dahil | 1200 | 1600 | 2000 | 2500 | 3000 |
| Havuz başına maksimum depolama alanı seçenekleri (GB) | 1200, 1280, 1536, 1792, 2048, 2304, 2560 | 1600, 1792, 2048, 2304, 2560, 2816, 3072 | 2000, 2048, 2304, 2560, 2816, 3072, 3328, 3584 | 2500, 2560, 2816, 3072, 3328, 3584, 3840, 4096 | 3000, 3072, 3328, 3584, 3840, 4096 |
| Havuz başına Maksimum Bellek IÇI OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok | Yok |
| Havuz başına maksimum sayı DBs <sup>1</sup> | 500 | 500 | 500 | 500 | 500 |
| Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>2</sup> | 2400 | 3200 | 4000 | 5000 | 6000 |
| Havuz başına maksimum eşzamanlı <sup>oturumlar 2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| Veritabanı başına Min eDTUs seçenekleri | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Veritabanı başına maksimum eDTUs seçenekleri | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Veritabanı başına maksimum depolama seçenekleri (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

### <a name="premium-elastic-pool-limits"></a>Premium esnek havuz sınırları

| Havuz başına eDTU | **125** | **250** | **500** | **1000** | **1500**|
|:---|---:|---:|---:| ---: | ---: |
| Havuz başına depolama (GB) dahil | 250 | 500 | 750 | 1024 | 1536 |
| Havuz başına maksimum depolama alanı seçenekleri (GB) | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 1536 |
| Havuz başına Maksimum Bellek IÇI OLTP depolama alanı (GB) | 1 | 2 | 4 | 10 | 12 |
| Havuz başına maksimum sayı DBs <sup>1</sup> | 50 | 100 | 100 | 100 | 100 |
| Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>2</sup> | 200 | 400 | 800 | 1600 | 2400 |
| Havuz başına maksimum eşzamanlı <sup>oturumlar 2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| Veritabanı başına Min. eDTU | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000|
| Veritabanı başına Maks. eDTU | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000|
| Veritabanı başına maks. depolama alanı (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

### <a name="premium-elastic-pool-limits-continued"></a>Premium esnek havuz limitleri (devam)

| Havuz başına eDTU | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: |
| Havuz başına depolama (GB) dahil | 2048 | 2560 | 3072 | 3548 | 4096 |
| Havuz başına maksimum depolama alanı seçenekleri (GB) | 2048 | 2560 | 3072 | 3548 | 4096|
| Havuz başına Maksimum Bellek IÇI OLTP depolama alanı (GB) | 16 | 20 | 24 | 28 | 32 |
| Havuz başına maksimum sayı DBs <sup>1</sup> | 100 | 100 | 100 | 100 | 100 |
| Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>2</sup> | 3200 | 4000 | 4800 | 5600 | 6400 |
| Havuz başına maksimum eşzamanlı <sup>oturumlar 2</sup> | 30000 | 30000 | 30000 | 30000 | 30000 |
| Veritabanı başına Min eDTUs seçenekleri | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| Veritabanı başına maksimum eDTUs seçenekleri | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 |
| Veritabanı başına maks. depolama alanı (GB) | 1024 | 1024 | 1024 | 1024 | 1024 |
|||||||

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

> [!IMPORTANT]
> Premium katmanında 1 TB'den fazla depolama şu anda Çin Doğu, Çin Kuzey, Almanya Orta, Almanya Kuzeydoğu, Batı Orta ABD, ABD DoD bölgeleri ve ABD Hükümeti Merkezi hariç tüm bölgelerde kullanılabilir. Bu bölgelerde, Premium katmandaki depolama limiti 1 TB ile sınırlıdır.  Daha fazla bilgi için [P11-P15 geçerli sınırlamaları'na](sql-database-single-database-scale.md#p11-and-p15-constraints-when-max-size-greater-than-1-tb)bakın.

Bir elastik havuzun tüm DTU’ları kullanılırsa, sorguları işlemek üzere havuzdaki her bir veritabanı eşit miktarda kaynak alır. SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Elastik havuz kaynak paylaşımı eşitliği, veritabanı başına DTU dakikası sıfır olmayan bir değere ayarlandığında her bir veritabanı için garanti edilen herhangi bir kaynak miktarına ek niteliktedir.

> [!NOTE]
> Limitler için `tempdb` [tempdb limitlerine](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database)bakın.

### <a name="database-properties-for-pooled-databases"></a>Havuzlu veritabanları için veritabanı özellikleri

Aşağıdaki tabloda havuza veritabanı özellikleri açıklanmaktadır.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına Maks. eDTU |Havuzdaki diğer veritabanlarının kullanımına göre mevcutsa, havuzdaki herhangi bir veritabanının kullanabileceği en fazla eDTU sayısı. Veritabanı başına en fazla eDTU, veritabanı için kaynak garantisi anlamına gelmez. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımının en üst seviyeye çıktığı durumlarla baş edebilmek için veritabanı başına en fazla eDTU sayısını yeterince yüksek bir değere ayarlayın. Havuz genellikle tüm veritabanlarının eşzamanlı olarak en üst kullanım seviyesine çıkmadığı sıcak ve soğuk kullanım modellerini varsaydığından, bir miktar aşırı ayırma beklenir. Örneğin, veritabanı başına en yüksek kullanımın 20 eDTU olduğunu ve havuzdaki 100 veritabanının yalnızca %20’sinin aynı anda en yüksek seviyeye çıktığını varsayalım. Veritabanı başına en fazla eDTU değeri 20 eDTU’ya ayarlanırsa, havuzun 5 kat fazla kullanılması ve havuz başına eDTU sayısının 400’e ayarlanması makuldür. |
| Veritabanı başına Min. eDTU |Havuzdaki herhangi bir veritabanının kullanabileceği en az eDTU sayısı garanti edilir. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına en az eDTU sayısı 0’a ayarlanabilir; bu değer aynı zamanda varsayılan değerdir. Bu özellik, 0 ile veritabanı başına ortalama eDTU kullanımı arasında bir değere ayarlanır. Havuzdaki veritabanı sayısının veritabanı başına en az eDTU sayısıyla çarpımı, havuz başına eDTU sayısını aşamaz. Örneğin, bir havuzda 20 veritabanı varsa ve veritabanı başına en az eDTU sayısı 10 eDTU’ya ayarlanırsa, havuzdaki eDTU sayısı en az 200 eDTU olmalıdır. |
| Veritabanı başına maksimum depolama |Kullanıcı tarafından havuzdaki bir veritabanı için ayarlanan maksimum veritabanı boyutu. Ancak, havuzlu veritabanları ayrılmış havuz depolama yı paylaşır. *Veritabanı başına* toplam maksimum depolama havuzunun toplam kullanılabilir depolama *alanından*daha büyük olarak ayarlanmış olsa bile, tüm veritabanları tarafından gerçekte kullanılan toplam alan kullanılabilir havuz sınırını aşamaz. Maksimum veritabanı boyutu veri dosyalarının maksimum boyutunu ifade eder ve günlük dosyaları tarafından kullanılan alanı içermez. |
|||

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için vCore kaynak sınırları [için, vCore satın alma modelini kullanarak tek veritabanları için kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md) bakın
- Tek bir veritabanı için DTU kaynak sınırları için, [DTU satın alma modelini kullanarak tek veritabanları için kaynak sınırlarına](sql-database-dtu-resource-limits-single-databases.md) bakın
- Elastik havuzlar için vCore kaynak sınırları için, [vCore satın alma modelini kullanarak elastik havuzlar için kaynak sınırlarına](sql-database-vcore-resource-limits-elastic-pools.md) bakın
- Yönetilen örnekler için kaynak sınırları için [yönetilen örnek kaynak sınırlarına](sql-database-managed-instance-resource-limits.md)bakın.
- Genel Azure sınırları hakkında bilgi için Azure [abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md)abakın.
- Veritabanı sunucusundaki kaynak sınırları hakkında bilgi için, sunucu ve abonelik düzeylerindeki sınırlar hakkında bilgi için [bir SQL Veritabanı sunucusundaki kaynak sınırlarına genel bakış](sql-database-resource-limits-database-server.md) bakın.
