---
title: vCore kaynak limitleri - elastik havuzlar
description: Bu sayfada Azure SQL Veritabanı'ndaki esnek havuzlar için bazı yaygın vCore kaynak sınırları açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab, sstein
ms.date: 03/03/2020
ms.openlocfilehash: b3c5594b8eef76dcb57903408dd1e77c96890eab
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346275"
---
# <a name="resource-limits-for-elastic-pools-using-the-vcore-purchasing-model"></a>vCore satın alma modelini kullanarak elastik havuzlar için kaynak sınırları

Bu makalede, vCore satın alma modelini kullanarak Azure SQL Veritabanı elastik havuzları ve havuzlu veritabanları için ayrıntılı kaynak sınırları sağlar.

DTU satın alma modeli sınırları için [SQL Veritabanı DTU kaynak limitlerine - esnek havuzlara](sql-database-dtu-resource-limits-elastic-pools.md)bakın.

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri almak için bir veritabanını küçültmeniz gerekebilir. Daha fazla bilgi için Azure [SQL Veritabanı'nda dosya alanını yönet'e](sql-database-file-space-management.md)bakın.

[Azure portalı,](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases) [PowerShell,](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases) [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases)veya [REST API'yi](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases)kullanarak hizmet katmanını, işlem boyutunu ve depolama miktarını ayarlayabilirsiniz.

> [!IMPORTANT]
> Ölçeklendirme kılavuzu ve dikkat edilmesi gerekenler için [bkz.](sql-database-elastic-pool-scale.md)

## <a name="general-purpose---provisioned-compute---gen4"></a>Genel amaç - sağlanan hesaplama - Gen4

> [!IMPORTANT]
> Yeni Gen4 veritabanları artık Avustralya Doğu veya Brezilya Güney bölgelerinde desteklenmemektedir.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Genel amaçlı hizmet katmanı: Nesil 4 işlem platformu (bölüm 1)

|İşlem boyutu|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|1|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Havuz başına maksimum sayı DBs <sup>1</sup>|100|200|500|500|500|500|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (GB)|512|756|1536|1536|1536|2048|
|Maksimum günlük boyutu|154|227|461|461|461|614|
|TempDB maksimum veri boyutu (GB)|32|64|96|128|160|192|
|Depolama türü|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|
|Havuz başına maksimum veri IOPS <sup>2</sup> |400|800|1200|1600|2000|2400|
|Havuz başına maksimum günlük hızı (MBps)|4.7|9.4|14.1|18.8|23.4|28.1|
|Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>3</sup> |210|420|630|840|1050|1260|
|Havuz başına maksimum eşzamanlı <sup>giriş3</sup> |210|420|630|840|1050|1260|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|
|Veritabanı başına Min/max elastik havuz vCore seçenekleri|0, 0.25, 0.5, 1|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...3|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...5|0, 0.25, 0.5, 1...6|
|Yineleme sayısı|1|1|1|1|1|1|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> 8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

<sup>3</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Genel amaçlı hizmet katmanı: Nesil 4 işlem platformu (bölüm 2)

|İşlem boyutu|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|159.5|
|Havuz başına maksimum sayı DBs <sup>1</sup>|500|500|500|500|500|500|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (GB)|2048|2048|2048|2048|3584|4096|
|Maksimum günlük boyutu (GB)|614|614|614|614|1075|1229|
|TempDB maksimum veri boyutu (GB)|224|256|288|320|512|768|
|Depolama türü|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|
|Havuz başına maksimum veri IOPS <sup>2</sup>|2800|3200|3600|4000|6400|9600|
|Havuz başına maksimum günlük hızı (MBps)|32.8|37.5|37.5|37.5|37.5|37.5|
|Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>3</sup>|1470|1680|1890|2100|3360|5040|
|Maksimum eşzamanlı giriş havuzu (istek) <sup>3</sup>|1470|1680|1890|2100|3360|5040|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|
|Veritabanı başına Min/max elastik havuz vCore seçenekleri|0, 0.25, 0.5, 1...7|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...9|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...10, 16|0, 0.25, 0.5, 1...10, 16, 24|
|Yineleme sayısı|1|1|1|1|1|1|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> 8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)    

<sup>3</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

## <a name="general-purpose---provisioned-compute---gen5"></a>Genel amaç - sağlanan hesaplama - Gen5

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Genel amaçlı hizmet katmanı: Nesil 5 işlem platformu (bölüm 1)

|İşlem boyutu|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|2|4|6|8|10|12|14|
|Bellek (GB)|10.4|20.8|31.1|41.5|51.9|62.3|72.7|
|Havuz başına maksimum sayı DBs <sup>1</sup>|100|200|500|500|500|500|500|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (GB)|512|756|1536|1536|1536|2048|2048|
|Maksimum günlük boyutu (GB)|154|227|461|461|461|614|614|
|TempDB maksimum veri boyutu (GB)|64|128|192|256|320|384|448|
|Depolama türü|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|
|Havuz başına maksimum veri IOPS <sup>2</sup>|800|1600|2400|3200|4000|4800|5600|
|Havuz başına maksimum günlük hızı (MBps)|9.4|18.8|28.1|37.5|37.5|37.5|37.5|
|Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>3</sup>|210|420|630|840|1050|1260|1470|
|Havuz başına maksimum eşzamanlı girişler (istekler) <sup>3</sup>|210|420|630|840|1050|1260|1470|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Veritabanı başına Min/max elastik havuz vCore seçenekleri|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...6|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...12|0, 0.25, 0.5, 1...14|
|Yineleme sayısı|1|1|1|1|1|1|1|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> 8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

<sup>3</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Genel amaçlı hizmet katmanı: Nesil 5 işlem platformu (bölüm 2)

|İşlem boyutu|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|16|18|20|24|32|40|80|
|Bellek (GB)|83|93.4|103.8|124.6|166.1|207.6|415.2|
|Havuz başına maksimum sayı DBs <sup>1</sup>|500|500|500|500|500|500|500|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Maksimum veri boyutu (GB)|2048|3072|3072|3072|4096|4096|4096|
|Maksimum günlük boyutu (GB)|614|922|922|922|1229|1229|1229|
|TempDB maksimum veri boyutu (GB)|512|576|640|768|1024|1280|2560|
|Depolama türü|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|Premium (Uzak) Depolama|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|5-7 ms (yazma)<br>5-10 ms (okundu)|
|Havuz başına maksimum veri IOPS <sup>2</sup> |6,400|7,200|8,000|9.600|12,800|16.000|32,000|
|Havuz başına maksimum günlük hızı (MBps)|37.5|37.5|37.5|37.5|37.5|37.5|37.5|
|Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>3</sup>|1680|1890|2100|2520|3360|4200|8400|
|Havuz başına maksimum eşzamanlı girişler (istekler) <sup>3</sup>|1680|1890|2100|2520|3360|4200|8400|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Veritabanı başına Min/max elastik havuz vCore seçenekleri|0, 0.25, 0.5, 1...16|0, 0.25, 0.5, 1...18|0, 0.25, 0.5, 1...20|0, 0.25, 0.5, 1...20, 24|0, 0.25, 0.5, 1...20, 24, 32|0, 0.25, 0.5, 1...16, 24, 32, 40|0, 0.25, 0.5, 1...16, 24, 32, 40, 80|
|Yineleme sayısı|1|1|1|1|1|1|1|
|Çoklu AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> 8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

<sup>3</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

## <a name="general-purpose---provisioned-compute---fsv2-series"></a>Genel amaç - verilen bilgi işlem - Fsv2 serisi

### <a name="fsv2-series-compute-generation-preview"></a>Fsv2 serisi bilgi işlem üretimi (önizleme)

|İşlem boyutu|GP_Fsv2_72|
|:--- | --: |
|İşlem oluşturma|Fsv2 serisi|
|vCores|72|
|Bellek (GB)|136.2|
|Havuz başına maksimum sayı DBs <sup>1</sup>|500|
|Columnstore desteği|Evet|
|Bellek içi OLTP depolama (GB)|Yok|
|Maksimum veri boyutu (GB)|4096|
|Maksimum günlük boyutu (GB)|1024|
|TempDB maksimum veri boyutu (GB)|333|
|Depolama türü|Premium (Uzak) Depolama|
|IO gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okundu)|
|Havuz başına maksimum veri IOPS <sup>2</sup>|16.000|
|Havuz başına maksimum günlük hızı (MBps)|37.5|
|Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>3</sup>|3780|
|Havuz başına maksimum eşzamanlı girişler (istekler) <sup>3</sup>|3780|
|Maks. eş zamanlı oturum|30,000|
|Veritabanı başına Min/max elastik havuz vCore seçenekleri|0-72|
|Yineleme sayısı|1|
|Çoklu AZ|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|
|Dahil edilen yedekleme depolama|1X DB boyutu|

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> 8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

<sup>3</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

## <a name="business-critical---provisioned-compute---gen4"></a>İş kritik - sağlanan hesaplama - Gen4

> [!IMPORTANT]
> Yeni Gen4 veritabanları artık Avustralya Doğu veya Brezilya Güney bölgelerinde desteklenmemektedir.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-1"></a>İş açısından kritik hizmet katmanı: Nesil 4 işlem platformu (bölüm 1)

|İşlem boyutu|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|2|3|4|5|6|
|Bellek (GB)|14|21|28|35|42|
|Havuz başına maksimum sayı DBs <sup>1</sup>|50|100|100|100|100|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|2|3|4|5|6|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|Maksimum veri boyutu (GB)|1024|1024|1024|1024|1024|
|Maksimum günlük boyutu (GB)|307|307|307|307|307|
|TempDB maksimum veri boyutu (GB)|64|96|128|160|192|
|IO gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|
|Havuz başına maksimum veri IOPS <sup>2</sup>|9,000|13,500|18.000|22,500|27,000|
|Havuz başına maksimum günlük hızı (MBps)|20|30|40|50|60|
|Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>3</sup>|420|630|840|1050|1260|
|Havuz başına maksimum eşzamanlı girişler (istekler) <sup>3</sup>|420|630|840|1050|1260|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|
|Veritabanı başına Min/max elastik havuz vCore seçenekleri|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...3|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...5|0, 0.25, 0.5, 1...6|
|Yineleme sayısı|4|4|4|4|4|
|Çoklu AZ|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> 8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

<sup>3</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>İş açısından kritik hizmet katmanı: Nesil 4 işlem platformu (bölüm 2)

|İşlem boyutu|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|159.5|
|Havuz başına maksimum sayı DBs <sup>1</sup>|100|100|100|100|100|100|
|Columnstore desteği|Yok|Yok|Yok|Yok|Yok|Yok|
|Bellek içi OLTP depolama (GB)|7|8|9.5|11|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|Maksimum veri boyutu (GB)|1024|1024|1024|1024|1024|1024|
|Maksimum günlük boyutu (GB)|307|307|307|307|307|307|
|TempDB maksimum veri boyutu (GB)|224|256|288|320|512|768|
|IO gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|
|Havuz başına maksimum veri IOPS <sup>2</sup>|31,500|36,000|40,500|45.000|72,000|96,000|
|Havuz başına maksimum günlük hızı (MBps)|70|80|80|80|80|80|
|Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>3</sup>|1470|1680|1890|2100|3360|5040|
|Havuz başına maksimum eşzamanlı girişler (istekler) <sup>3</sup>|1470|1680|1890|2100|3360|5040|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|
|Veritabanı başına Min/max elastik havuz vCore seçenekleri|0, 0.25, 0.5, 1...7|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...9|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...10, 16|0, 0.25, 0.5, 1...10, 16, 24|
|Yineleme sayısı|4|4|4|4|4|4|
|Çoklu AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> 8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

<sup>3</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

## <a name="business-critical---provisioned-compute---gen5"></a>İş kritik - sağlanan hesaplama - Gen5

### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>İş açısından kritik hizmet katmanı: Nesil 5 işlem platformu (bölüm 1)

|İşlem boyutu|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|4|6|8|10|12|14|
|Bellek (GB)|20.8|31.1|41.5|51.9|62.3|72.7|
|Havuz başına maksimum sayı DBs <sup>1</sup>|50|100|100|100|100|100|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|3.14|4.71|6.28|8.65|11.02|13.39|
|Maksimum veri boyutu (GB)|1024|1536|1536|1536|3072|3072|
|Maksimum günlük boyutu (GB)|307|307|461|461|922|922|
|TempDB maksimum veri boyutu (GB)|128|192|256|320|384|448|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|IO gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|
|Havuz başına maksimum veri IOPS <sup>2</sup>|18.000|27,000|36,000|45.000|54,000|63,000|
|Havuz başına maksimum günlük hızı (MBps)|60|90|120|120|120|120|
|Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>3</sup>|420|630|840|1050|1260|1470|
|Havuz başına maksimum eşzamanlı girişler (istekler) <sup>3</sup>|420|630|840|1050|1260|1470|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|
|Veritabanı başına Min/max elastik havuz vCore seçenekleri|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...6|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...12|0, 0.25, 0.5, 1...14|
|Yineleme sayısı|4|4|4|4|4|4|
|Çoklu AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> 8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

<sup>3</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>İş açısından kritik hizmet katmanı: Nesil 5 işlem platformu (bölüm 2)

|İşlem boyutu|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|16|18|20|24|32|40|80|
|Bellek (GB)|83|93.4|103.8|124.6|166.1|207.6|415.2|
|Havuz başına maksimum sayı DBs <sup>1</sup>|100|100|100|100|100|100|100|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|15,77|18.14|20.51|25.25|37.94|52.23|131.68|
|Maksimum veri boyutu (GB)|3072|3072|3072|4096|4096|4096|4096|
|Maksimum günlük boyutu (GB)|922|922|922|1229|1229|1229|1229|
|TempDB maksimum veri boyutu (GB)|512|576|640|768|1024|1280|2560|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|IO gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|1-2 ms (yazma)<br>1-2 ms (okundu)|
|Havuz başına maksimum veri IOPS <sup>2</sup>|72,000|81,000|90,000|108,000|144,000|180,000|256,000|
|Havuz başına maksimum günlük hızı (MBps)|120|120|120|120|120|120|120|
|Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>3</sup>|1680|1890|2100|2520|3360|4200|8400|
|Havuz başına maksimum eşzamanlı girişler (istekler) <sup>3</sup>|1680|1890|2100|2520|3360|4200|8400|
|Maks. eş zamanlı oturum|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Veritabanı başına Min/max elastik havuz vCore seçenekleri|0, 0.25, 0.5, 1...16|0, 0.25, 0.5, 1...18|0, 0.25, 0.5, 1...20|0, 0.25, 0.5, 1...20, 24|0, 0.25, 0.5, 1...20, 24, 32|0, 0.25, 0.5, 1...20, 24, 32, 40|0, 0.25, 0.5, 1...20, 24, 32, 40, 80|
|Yineleme sayısı|4|4|4|4|4|4|4|
|Çoklu AZ|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolama|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> 8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

<sup>3</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

## <a name="business-critical---provisioned-compute---m-series"></a>İş kritik - sağlanan hesaplama - M-serisi

### <a name="m-series-compute-generation-preview"></a>M serisi işlem oluşturma (önizleme)

|İşlem boyutu|BC_M_128|
|:--- | --: |
|İşlem oluşturma|M serisi|
|vCores|128|
|Bellek (GB)|3767.1|
|Havuz başına maksimum sayı DBs <sup>1</sup>|100|
|Columnstore desteği|Evet|
|Bellek içi OLTP depolama (GB)|1768|
|Maksimum veri boyutu (GB)|4096|
|Maksimum günlük boyutu (GB)|2048|
|TempDB maksimum veri boyutu (GB)|4096|
|Depolama türü|Yerel SSD|
|IO gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okundu)|
|Havuz başına maksimum veri IOPS <sup>2</sup>|200,000|
|Havuz başına maksimum günlük hızı (MBps)|333|
|Havuz başına maksimum eşzamanlı çalışanlar (istekler) <sup>3</sup>|13,440|
|Havuz başına maksimum eşzamanlı girişler (istekler) <sup>3</sup>|13,440|
|Maks. eş zamanlı oturum|30,000|
|Veritabanı başına Min/max elastik havuz vCore seçenekleri|0-128|
|Yineleme sayısı|4|
|Çoklu AZ|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|
|Dahil edilen yedekleme depolama|1X DB boyutu|

<sup>1</sup> Ek hususlar için [yoğun elastik havuzlarda Kaynak yönetimine](sql-database-elastic-pool-resource-management.md) bakın.

<sup>2</sup> 8 KB ile 64 KB arasında değişen IO boyutları için maksimum değer. Gerçek IOPS iş yüküne bağlıdır. Ayrıntılar için [Bkz. Veri IO Yönetişimi.](sql-database-resource-limits-database-server.md#resource-governance)

<sup>3</sup> Herhangi bir veritabanı için en fazla eşzamanlı çalışanlar (istekler) için [Tek veritabanı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın. Örneğin, elastik havuz Gen5 kullanıyorsa ve veritabanı başına maksimum vCore 2 olarak ayarlanmışsa, en fazla eşzamanlı çalışan değeri 200'dür.  Veritabanı başına maksimum vCore 0,5 olarak ayarlanırsa, o zaman maksimum eşzamanlı işçi değeri 50 gen5 vCore başına 100 eşzamanlı işçi max beri. Daha az 1 vCore veya daha az veritabanı başına diğer max vCore ayarları için, max eşzamanlı işçi sayısı benzer şekilde yeniden ölçeklendirilir.

Elastik bir havuzun tüm vCore'ları meşgulse, havuzdaki her veritabanı sorguları işlemek için eşit miktarda bilgi işlem kaynağı alır. SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Elastik havuz kaynak paylaşımı adaleti, veritabanı başına vCore min sıfır olmayan bir değere ayarlandığında, aksi takdirde her veritabanıiçin garanti edilen herhangi bir kaynak miktarına ek olarak verilir.

## <a name="database-properties-for-pooled-databases"></a>Havuzlu veritabanları için veritabanı özellikleri

Aşağıdaki tabloda havuza veritabanı özellikleri açıklanmaktadır.

> [!NOTE]
> Elastik havuzlarda tek tek veritabanlarının kaynak sınırları genellikle aynı bilgi işlem boyutuna sahip havuzların dışındaki tek veritabanları için aynıdır. Örneğin, bir GP_Gen4_1 veritabanı için en fazla eşzamanlı çalışan 200 işçidir. Yani, GP_Gen4_1 havuzunda bir veritabanı için maksimum eşzamanlı işçi de 200 işçi. GP_Gen4_1 havuzundaki toplam eşzamanlı işçi sayısı 210'dur.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına max vCores |Havuzdaki diğer veritabanlarıtarafından kullanılmasına bağlı olarak varsa, havuzdaki herhangi bir veritabanının kullanabileceği maksimum vCore sayısı. Veritabanı başına Max vCores bir veritabanı için bir kaynak garantisi değildir. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımında zirveleri işlemek için yeterince yüksek veritabanı başına max vCores ayarlayın. Havuz genellikle tüm veritabanları aynı anda tepe değil veritabanları için sıcak ve soğuk kullanım desenleri varsayar beri aşırı taahhüt bir dereceye kadar bekleniyor.|
| Veritabanı başına Min vCores |Havuzdaki herhangi bir veritabanının garanti ettiği minimum vCore sayısı. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına min vCores 0 olarak ayarlanabilir ve aynı zamanda varsayılan değerdir. Bu özellik, 0 ile veritabanı başına ortalama vCores kullanımı arasında her yerde ayarlanır. Havuzdaki veritabanlarının ve veritabanı başına min vCores sayısının ürünü, havuz başına vCores'u aşamaz.|
| Veritabanı başına maksimum depolama |Kullanıcı tarafından havuzdaki bir veritabanı için ayarlanan maksimum veritabanı boyutu. Havuza ayrılmış veritabanları ayrılmış havuz depolamasını paylaşır, böylece veritabanının ulaşabileceği boyut kalan havuz depolama alanı ve veritabanı boyutuyla sınırlıdır. Maksimum veritabanı boyutu veri dosyalarının maksimum boyutunu ifade eder ve günlük dosyaları tarafından kullanılan alanı içermez. |
|||

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için vCore kaynak sınırları [için, vCore satın alma modelini kullanarak tek veritabanları için kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md) bakın
- Tek bir veritabanı için DTU kaynak sınırları için, [DTU satın alma modelini kullanarak tek veritabanları için kaynak sınırlarına](sql-database-dtu-resource-limits-single-databases.md) bakın
- Elastik havuzlar için DTU kaynak sınırları için, [DTU satın alma modelini kullanan elastik havuzlar için kaynak sınırlarına](sql-database-dtu-resource-limits-elastic-pools.md) bakın
- Yönetilen örnekler için kaynak sınırları için [yönetilen örnek kaynak sınırlarına](sql-database-managed-instance-resource-limits.md)bakın.
- Genel Azure sınırları hakkında bilgi için Azure [abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md)abakın.
- Veritabanı sunucusundaki kaynak sınırları hakkında bilgi için, sunucu ve abonelik düzeylerindeki sınırlar hakkında bilgi için [bir SQL Veritabanı sunucusundaki kaynak sınırlarına genel bakış](sql-database-resource-limits-database-server.md) bakın.
