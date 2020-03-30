---
title: vCore kaynak sınırları - tek veritabanı
description: Bu sayfada, Azure SQL Veritabanı'ndaki tek bir veritabanı için bazı yaygın vCore kaynak sınırları açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/11/2020
ms.openlocfilehash: 38d44f5a2ce22de15cb14ea5aa7a9ca1ea7c03cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481074"
---
# <a name="resource-limits-for-single-databases-using-the-vcore-purchasing-model"></a>vCore satın alma modelini kullanarak tek veritabanları için kaynak sınırları

Bu makalede, vCore satın alma modelini kullanarak Azure SQL Veritabanı tek veritabanları için ayrıntılı kaynak sınırları sağlar.

Bir SQL Veritabanı sunucusundaki tek veritabanları için DTU satın alma modeli sınırları için, [SQL Veritabanı sunucusundaki kaynak sınırlarına genel bakış](sql-database-resource-limits-database-server.md)bölümüne bakın.

[Azure portalı,](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server) [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-sql-database-servers-and-single-databases), [PowerShell,](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases) [Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases)veya [REST API'yi](sql-database-single-databases-manage.md#rest-api-manage-sql-database-servers-and-single-databases)kullanarak tek bir veritabanı için hizmet katmanını, işlem boyutunu ve depolama tutarını ayarlayabilirsiniz.

> [!IMPORTANT]
> Kılavuz ve dikkatleri ölçeklendirmek için [bkz.](sql-database-single-database-scale.md)

## <a name="general-purpose---serverless-compute---gen5"></a>Genel amaç - sunucusuz bilgi işlem - Gen5

[Sunucusuz bilgi işlem katmanı](sql-database-serverless.md) şu anda yalnızca Gen5 donanımında kullanılabilir.

### <a name="gen5-compute-generation-part-1"></a>Gen5 işlem üretimi (bölüm 1)

|İşlem boyutu|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:--- | --: |--: |--: |--: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|Gen5|
|Min-max vCores|0.5-1|0.5-2|0.5-4|0.75-6|1.0-8|
|Min-max bellek (GB)|2.02-3|2.05-6|2.10-12|2.25-18|3.00-24|
|Min otomatik duraklatma gecikmesi (dakika)|60|60|60|60|60|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (GB)|512|1024|1024|1024|1536|
|Maksimum günlük boyutu (GB)|154|307|307|307|461|
|TempDB maksimum veri boyutu (GB)|32|64|128|192|256|
|Depolama türü|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|
|Maksimum veri IOPS *|320|640|1280|1920|2560|
|Maksimum günlük oranı (MBps)|3.8|7,5|15|22.5|30|
|Maksimum eşzamanlı çalışanlar (istekler)|75|150|300|450|600|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|
|Yineleme sayısı|1|1|1|1|1|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

### <a name="gen5-compute-generation-part-2"></a>Gen5 işlem üretimi (bölüm 2)

|İşlem boyutu|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:--- | --: |--: |--: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|
|Min-max vCores|1.25-10|1.50-12|1.75-14|2.00-16|
|Min-max bellek (GB)|3.75-30|4.50-36|5.25-42|6.00-48|
|Min otomatik duraklatma gecikmesi (dakika)|60|60|60|60|
|Columnstore desteği|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (GB)|1536|3072|3072|3072|
|Maksimum günlük boyutu (GB)|461|461|461|922|
|TempDB maksimum veri boyutu (GB)|320|384|448|512|
|Depolama türü|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|
|Maksimum veri IOPS *|3200|3840|4480|5120|
|Maksimum günlük oranı (MBps)|30|30|30|30|
|Maksimum eşzamanlı çalışanlar (istekler)|750|900|1050|1200|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|
|Yineleme sayısı|1|1|1|1|
|Çoklu AZ|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

## <a name="hyperscale---provisioned-compute---gen4"></a>Hyperscale - sağlanan hesaplama - Gen4

### <a name="gen4-compute-generation-part-1"></a>Gen4 bilgisayar üretimi (bölüm 1)

|Performans düzeyi|HS_Gen4_1|HS_Gen4_2|HS_Gen4_3|HS_Gen4_4|HS_Gen4_5|HS_Gen4_6|
|:--- | --: |--: |--: |---: | --: |--: |
|İşlem oluşturma|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|1|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|[RBPEX](sql-database-service-tier-hyperscale.md#compute) Boyutu|3X Bellek|3X Bellek|3X Bellek|3X Bellek|3X Bellek|3X Bellek|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (TB)|100 |100 |100 |100 |100 |100|
|Maksimum günlük boyutu (TB)|1 |1 |1 |1 |1 |1 |
|TempDB maksimum veri boyutu (GB)|32|64|96|128|160|192|
|Depolama türü| [Not 1](#notes) |[Not 1](#notes)|[Not 1](#notes) |[Not 1](#notes) |[Not 1](#notes) |[Not 1](#notes) |
|Maksimum veri IOPS *|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|
|Maksimum günlük oranı (MBps)|100 |100 |100 |100 |100 |100 |
|IO gecikmesi (yaklaşık)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|
|Maksimum eşzamanlı çalışanlar (istekler)|200|400|600|800|1000|1200|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|
|İkincil çoğaltmalar|0-4|0-4|0-4|0-4|0-4|0-4|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama tutma|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|
|||

### <a name="gen4-compute-generation-part-2"></a>Gen4 bilgisayar üretimi (bölüm 2)

|Performans düzeyi|HS_Gen4_7|HS_Gen4_8|HS_Gen4_9|HS_Gen4_10|HS_Gen4_16|HS_Gen4_24|
|:--- | ---: |--: |--: | --: |--: |--: |
|İşlem oluşturma|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|159.5|
|[RBPEX](sql-database-service-tier-hyperscale.md#compute) Boyutu|3X Bellek|3X Bellek|3X Bellek|3X Bellek|3X Bellek|3X Bellek|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (TB)|100 |100 |100 |100 |100 |100 |
|Maksimum günlük boyutu (TB)|1 |1 |1 |1 |1 |1 |
|TempDB maksimum veri boyutu (GB)|224|256|288|320|512|768|
|Depolama türü| [Not 1](#notes) |[Not 1](#notes) |[Not 1](#notes) |[Not 1](#notes) |[Not 1](#notes) |[Not 1](#notes) |
|Maksimum veri IOPS *|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|
|Maksimum günlük oranı (MBps)|100 |100 |100 |100 |100 |100 |
|IO gecikmesi (yaklaşık)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|
|Maksimum eşzamanlı çalışanlar (istekler)|1400|1600|1800|2000|3200|4800|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|
|İkincil çoğaltmalar|0-4|0-4|0-4|0-4|0-4|0-4|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama tutma|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|
|||

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

## <a name="hyperscale---provisioned-compute---gen5"></a>Hyperscale - sağlanan hesaplama - Gen5

### <a name="gen5-compute-generation-part-1"></a>Gen5 işlem üretimi (bölüm 1)

|Performans düzeyi|HS_Gen5_2|HS_Gen5_4|HS_Gen5_6|HS_Gen_8|HS_Gen5_10|HS_Gen5_12|HS_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|2|4|6|8|10|12|14|
|Bellek (GB)|10.4|20.8|31.1|41.5|51.9|62.3|72.7|
|[RBPEX](sql-database-service-tier-hyperscale.md#compute) Boyutu|3X Bellek|3X Bellek|3X Bellek|3X Bellek|3X Bellek|3X Bellek|3X Bellek|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (TB)|100 |100 |100 |100 |100 |100 |100|
|Maksimum günlük boyutu (TB)|1 |1 |1 |1 |1 |1 |1 |
|TempDB maksimum veri boyutu (GB)|64|128|192|256|320|384|448|
|Depolama türü| [Not 1](#notes) |[Not 1](#notes)|[Not 1](#notes) |[Not 1](#notes) |[Not 1](#notes) |[Not 1](#notes) |[Not 1](#notes) |
|Maksimum veri IOPS *|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|
|Maksimum günlük oranı (MBps)|100 |100 |100 |100 |100 |100 |100 |
|IO gecikmesi (yaklaşık)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|
|Maksimum eşzamanlı çalışanlar (istekler)|200|400|600|800|1000|1200|1400|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|İkincil çoğaltmalar|0-4|0-4|0-4|0-4|0-4|0-4|0-4|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama tutma|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|
|||

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

### <a name="gen5-compute-generation-part-2"></a>Gen5 işlem üretimi (bölüm 2)

|Performans düzeyi|HS_Gen5_16|HS_Gen5_18|HS_Gen5_20|HS_Gen_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: |--: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|16|18|20|24|32|40|80|
|Bellek (GB)|83|93.4|103.8|124.6|166.1|207.6|415.2|
|[RBPEX](sql-database-service-tier-hyperscale.md#compute) Boyutu|3X Bellek|3X Bellek|3X Bellek|3X Bellek|3X Bellek|3X Bellek|3X Bellek|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (TB)|100 |100 |100 |100 |100 |100 |100 |
|Maksimum günlük boyutu (TB)|1 |1 |1 |1 |1 |1 |1 |
|TempDB maksimum veri boyutu (GB)|512|576|640|768|1024|1280|2560|
|Depolama türü| [Not 1](#notes) |[Not 1](#notes)|[Not 1](#notes)|[Not 1](#notes) |[Not 1](#notes) |[Not 1](#notes) |[Not 1](#notes) |
|Maksimum veri IOPS *|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|[Not 2](#notes)|
|Maksimum günlük oranı (MBps)|100 |100 |100 |100 |100 |100 |100 |
|IO gecikmesi (yaklaşık)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|[Not 3](#notes)|
|Maksimum eşzamanlı çalışanlar (istekler)|1600|1800|2000|2400|3200|4000|8000|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|İkincil çoğaltmalar|0-4|0-4|0-4|0-4|0-4|0-4|0-4|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama tutma|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|
|||

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

#### <a name="notes"></a>Notlar

**Not 1**: Hiperölçek, ayrı bir işlem ve depolama bileşenlerine sahip çok katmanlı bir [mimaridir: Hyperscale Service Tier Architecture](sql-database-service-tier-hyperscale.md#distributed-functions-architecture)

**Not 2**: Hiper ölçekli çok katmanlı mimari birden fazla düzeyde önbelleğe sahiptir. Etkili IOPS iş yüküne bağlıdır.

**Not 3**: Gecikme, en çok kullanılan veri sayfalarını önbelleğe alan, bilgi işlem yinelemelerinde RBPEX SSD tabanlı önbellekteki veriler için 1-2 ms'dir. Sayfa sunucularından alınan veriler için daha yüksek gecikme.

## <a name="general-purpose---provisioned-compute---gen4"></a>Genel amaç - sağlanan hesaplama - Gen4

> [!IMPORTANT]
> Yeni Gen4 veritabanları artık Avustralya Doğu veya Brezilya Güney bölgelerinde desteklenmemektedir.

### <a name="gen4-compute-generation-part-1"></a>Gen4 bilgisayar üretimi (bölüm 1)

|İşlem boyutu|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|1|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (GB)|1024|1024|1536|1536|1536|3072|
|Maksimum günlük boyutu (GB)|307|307|461|461|461|922|
|TempDB maksimum veri boyutu (GB)|32|64|96|128|160|192|
|Depolama türü|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|
|Maksimum veri IOPS *|320|640|960|1280|1600|1920|
|Maksimum günlük oranı (MBps)|3,75|7,5|11.25|15|18.75|22.5|
|Maksimum eşzamanlı çalışanlar (istekler)|200|400|600|800|1000|1200|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|
|Yineleme sayısı|1|1|1|1|1|1|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

### <a name="gen4-compute-generation-part-2"></a>Gen4 bilgisayar üretimi (bölüm 2)

|İşlem boyutu|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|159.5|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (GB)|3072|3072|3072|3072|4096|4096|
|Maksimum günlük boyutu (GB)|922|922|922|922|1229|1229|
|TempDB maksimum veri boyutu (GB)|224|256|288|320|512|768|
|Depolama türü|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)
|Maksimum veri IOPS *|2240|2560|2880|3200|5120|7680|
|Maksimum günlük oranı (MBps)|26.3|30|30|30|30|30|
|Maksimum eşzamanlı çalışanlar (istekler)|1400|1600|1800|2000|3200|4800|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|
|Yineleme sayısı|1|1|1|1|1|1|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

## <a name="general-purpose---provisioned-compute---gen5"></a>Genel amaç - sağlanan hesaplama - Gen5

### <a name="gen5-compute-generation-part-1"></a>Gen5 işlem üretimi (bölüm 1)

|İşlem boyutu|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|2|4|6|8|10|12|14|
|Bellek (GB)|10.4|20.8|31.1|41.5|51.9|62.3|72.7|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (GB)|1024|1024|1536|1536|1536|3072|3072|
|Maksimum günlük boyutu (GB)|307|307|461|461|461|922|922|
|TempDB maksimum veri boyutu (GB)|64|128|192|256|320|384|384|
|Depolama türü|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|
|Maksimum veri IOPS *|640|1280|1920|2560|3200|3840|4480|
|Maksimum günlük oranı (MBps)|7,5|15|22.5|30|30|30|30|
|Maksimum eşzamanlı çalışanlar (istekler)|200|400|600|800|1000|1200|1400|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Yineleme sayısı|1|1|1|1|1|1|1|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

### <a name="gen5-compute-generation-part-2"></a>Gen5 işlem üretimi (bölüm 2)

|İşlem boyutu|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|16|18|20|24|32|40|80|
|Bellek (GB)|83|93.4|103.8|124.6|166.1|207.6|415.2|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (GB)|3072|3072|3072|4096|4096|4096|4096|
|Maksimum günlük boyutu (GB)|922|922|922|1229|1229|1229|1229|
|TempDB maksimum veri boyutu (GB)|512|576|640|768|1024|1280|2560|
|Depolama türü|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|Uzaktan SSD|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|
|Maksimum veri IOPS *|5120|5760|6400|7680|10240|12800|25600|
|Maksimum günlük oranı (MBps)|30|30|30|30|30|30|30|
|Maksimum eşzamanlı çalışanlar (istekler)|1600|1800|2000|2400|3200|4000|8000|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Yineleme sayısı|1|1|1|1|1|1|1|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

## <a name="general-purpose---provisioned-compute---fsv2-series"></a>Genel amaç - verilen bilgi işlem - Fsv2 serisi

### <a name="fsv2-series-compute-generation-preview"></a>Fsv2 serisi bilgi işlem üretimi (önizleme)

|İşlem boyutu|GP_Fsv2_72|
|:--- | --: |
|İşlem oluşturma|Fsv2 serisi|
|vCores|72|
|Bellek (GB)|136.2|
|Columnstore desteği|Evet|
|Bellek içi OLTP depolama (GB)|Yok|
|Maksimum veri boyutu (GB)|4096|
|Maksimum günlük boyutu (GB)|1024|
|TempDB maksimum veri boyutu (GB)|333|
|Depolama türü|Uzaktan SSD|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|
|Maksimum veri IOPS *|12,800|
|Maksimum günlük oranı (MBps)|30|
|Maksimum eşzamanlı çalışanlar (istekler)|3600|
|Maks. eş zamanlı oturum|3600|
|Maks. eş zamanlı oturum|30,000|
|Yineleme sayısı|1|
|Çoklu AZ|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

## <a name="business-critical---provisioned-compute---gen4"></a>İş kritik - sağlanan hesaplama - Gen4

> [!IMPORTANT]
> Yeni Gen4 veritabanları artık Avustralya Doğu veya Brezilya Güney bölgelerinde desteklenmemektedir.

### <a name="gen4-compute-generation-part-1"></a>Gen4 bilgisayar üretimi (bölüm 1)

|İşlem boyutu|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|1|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|1|2|3|4|5|6|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|Maksimum veri boyutu (GB)|1024|1024|1024|1024|1024|1024|
|Maksimum günlük boyutu (GB)|307|307|307|307|307|307|
|TempDB maksimum veri boyutu (GB)|32|64|96|128|160|192|
|IO gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|
|Maksimum veri IOPS *|4.000|8,000|12.000|16.000|20.000|24,000|
|Maksimum günlük oranı (MBps)|8|16|24|32|40|48|
|Maksimum eşzamanlı çalışanlar (istekler)|200|400|600|800|1000|1200|
|Maks. eş zamanlı oturum|200|400|600|800|1000|1200|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|
|Yineleme sayısı|4|4|4|4|4|4|
|Çoklu AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

### <a name="gen4-compute-generation-part-2"></a>Gen4 bilgisayar üretimi (bölüm 2)

|İşlem boyutu|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|159.5|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|7|8|9.5|11|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|Maksimum veri boyutu (GB)|1024|1024|1024|1024|1024|1024|
|Maksimum günlük boyutu (GB)|307|307|307|307|307|307|
|TempDB maksimum veri boyutu (GB)|224|256|288|320|512|768|
|IO gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|
|Maksimum veri IOPS |28,000|32,000|36,000|40,000|64,000|76.800|
|Maksimum günlük oranı (MBps)|56|64|64|64|64|64|
|Maksimum eşzamanlı çalışanlar (istekler)|1400|1600|1800|2000|3200|4800|
|Maksimum eşzamanlı girişler (istekler)|1400|1600|1800|2000|3200|4800|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|
|Yineleme sayısı|4|4|4|4|4|4|
|Çoklu AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

## <a name="business-critical---provisioned-compute---gen5"></a>İş kritik - sağlanan hesaplama - Gen5

### <a name="gen5-compute-generation-part-1"></a>Gen5 işlem üretimi (bölüm 1)

|İşlem boyutu|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|2|4|6|8|10|12|14|
|Bellek (GB)|10.4|20.8|31.1|41.5|51.9|62.3|72.7|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|1.57|3.14|4.71|6.28|8.65|11.02|13.39|
|Maksimum veri boyutu (GB)|1024|1024|1536|1536|1536|3072|3072|
|Maksimum günlük boyutu (GB)|307|307|461|461|461|922|922|
|TempDB maksimum veri boyutu (GB)|64|128|192|256|320|384|448|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|IO gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|
|Maksimum veri IOPS *|8000|16.000|24,000|32,000|40,000|48,000|56,000|
|Maksimum günlük oranı (MBps)|24|48|72|96|96|96|96|
|Maksimum eşzamanlı çalışanlar (istekler)|200|400|600|800|1000|1200|1400|
|Maks. eş zamanlı oturum|200|400|600|800|1000|1200|1400|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Yineleme sayısı|4|4|4|4|4|4|4|
|Çoklu AZ|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

### <a name="gen5-compute-generation-part-2"></a>Gen5 işlem üretimi (bölüm 2)

|İşlem boyutu|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|16|18|20|24|32|40|80|
|Bellek (GB)|83|93.4|103.8|124.6|166.1|207.6|415.2|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|15,77|18.14|20.51|25.25|37.94|52.23|131.64|
|Maksimum veri boyutu (GB)|3072|3072|3072|4096|4096|4096|4096|
|Maksimum günlük boyutu (GB)|922|922|922|1229|1229|1229|1229|
|TempDB maksimum veri boyutu (GB)|512|576|640|768|1024|1280|2560|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|IO gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|
|Maksimum veri IOPS *|64,000|72,000|80,000|96,000|128,000|160.000|204,800|
|Maksimum günlük oranı (MBps)|96|96|96|96|96|96|96|
|Maksimum eşzamanlı çalışanlar (istekler)|1600|1800|2000|2400|3200|4000|8000|
|Maks. eş zamanlı oturum|1600|1800|2000|2400|3200|4000|8000|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Yineleme sayısı|4|4|4|4|4|4|4|
|Çoklu AZ|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

## <a name="business-critical---provisioned-compute---m-series"></a>İş kritik - sağlanan hesaplama - M-serisi

### <a name="m-series-compute-generation-preview"></a>M serisi işlem oluşturma (önizleme)

|İşlem boyutu|BC_M_128|
|:--- | --: |
|İşlem oluşturma|M serisi|
|vCores|128|
|Bellek (GB)|3767.1|
|Columnstore desteği|Evet|
|Bellek içi OLTP depolama (GB)|1768|
|Maksimum veri boyutu (GB)|4096|
|Maksimum günlük boyutu (GB)|2048|
|TempDB maksimum veri boyutu (GB)|4096|
|Depolama türü|Yerel SSD|
|IO gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okundu)|
|Maksimum veri IOPS *|160.000|
|Maksimum günlük oranı (MBps)|264|
|Maksimum eşzamanlı çalışanlar (istekler)|12,800|
|Maks. eş zamanlı oturum|12,800|
|Maks. eş zamanlı oturum|30000|
|Yineleme sayısı|4|
|Çoklu AZ|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|
|Dahil edilen yedekleme depolama|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri almak için bir veritabanını küçültmeniz gerekebilir. Daha fazla bilgi için Azure [SQL Veritabanı'nda dosya alanını yönet'e](sql-database-file-space-management.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için DTU kaynak sınırları için, [DTU satın alma modelini kullanarak tek veritabanları için kaynak sınırlarına](sql-database-dtu-resource-limits-single-databases.md) bakın
- Elastik havuzlar için vCore kaynak sınırları için, [vCore satın alma modelini kullanarak elastik havuzlar için kaynak sınırlarına](sql-database-vcore-resource-limits-elastic-pools.md) bakın
- Elastik havuzlar için DTU kaynak sınırları için, [DTU satın alma modelini kullanan elastik havuzlar için kaynak sınırlarına](sql-database-dtu-resource-limits-elastic-pools.md) bakın
- Yönetilen örnekler için kaynak sınırları için [yönetilen örnek kaynak sınırlarına](sql-database-managed-instance-resource-limits.md)bakın.
- Genel Azure sınırları hakkında bilgi için Azure [abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md)abakın.
- Veritabanı sunucusundaki kaynak sınırları hakkında bilgi için, sunucu ve abonelik düzeylerindeki sınırlar hakkında bilgi için [bir SQL Veritabanı sunucusundaki kaynak sınırlarına genel bakış](sql-database-resource-limits-database-server.md) bakın.
