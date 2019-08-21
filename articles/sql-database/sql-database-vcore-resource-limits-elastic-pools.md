---
title: Azure SQL veritabanı sanal çekirdek tabanlı kaynak limitleri-elastik havuzlar | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanı 'nda esnek havuzlar için bazı yaygın sanal çekirdek tabanlı kaynak sınırlarını açıklamaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
ms.date: 06/26/2019
ms.openlocfilehash: aa35cb1ae0ad035bdef6ff8e19cf115188ba7f3a
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69640849"
---
# <a name="resource-limits-for-elastic-pools-using-the-vcore-based-purchasing-model-limits"></a>Sanal çekirdek tabanlı satın alma modeli sınırlarını kullanan elastik havuzlar için kaynak sınırları

Bu makalede, sanal çekirdek tabanlı satın alma modeli kullanılarak Azure SQL veritabanı elastik havuzları ve havuza alınmış veritabanları için ayrıntılı kaynak sınırları sağlanmaktadır.

DTU tabanlı satın alma modeli sınırları için bkz. [SQL VERITABANı DTU tabanlı kaynak limitleri-elastik havuzlar](sql-database-dtu-resource-limits-elastic-pools.md).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

Hizmet katmanını, işlem boyutunu ve depolama miktarını [Azure Portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [POWERSHELL](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases)veya [REST API](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases)kullanarak ayarlayabilirsiniz.

> [!IMPORTANT]
> Ölçeklendirme Kılavuzu ve değerlendirmeleri için bkz. [elastik havuz ölçekleme](sql-database-elastic-pool-scale.md)
> [!NOTE]
> Elastik havuzlardaki ayrı veritabanlarının kaynak sınırları, aynı işlem boyutuna sahip havuzlar dışındaki tek veritabanları için genellikle aynıdır. Örneğin, bir GP_Gen4_1 veritabanı için en fazla eş zamanlı çalışan 200 çalışandır. Bu nedenle, bir GP_Gen4_1 havuzundaki bir veritabanının maksimum eş zamanlı çalışanları da 200 çalışanlardır. GP_Gen4_1 havuzundaki eşzamanlı çalışanların toplam sayısı 210 ' dir.

## <a name="general-purpose-service-tier-storage-sizes-and-compute-sizes"></a>Genel Amaçlı hizmet katmanı: Depolama boyutları ve işlem boyutları

> [!IMPORTANT]
> Yeni 4. nesil veritabanları artık Avustralya Doğu veya Brezilya Güney bölgelerinde desteklenmez.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Genel Amaçlı hizmet katmanı: Nesil 4 işlem platformu (Bölüm 1)

|İşlem boyutu|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1\.|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Havuz başına en fazla veritabanı|100|200|500|500|500|500|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|512|756|756|1536|1536|1536|
|En büyük günlük boyutu|154|227|227|461|461|461|
|TempDB boyutu (GB)|32|64|96|128|160|192|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Hedef ıOPS (64 KB)|500|1000|1500|2000|2500|3000|
|Günlük hızı sınırları (MBps)|4,6875|9,375|14,0625|18,75|23,4375|28,125|
|Havuz başına en fazla eş zamanlı çalışan (istek) * |210|420|630|840|1050|1260|
|Havuz başına en fazla eşzamanlı oturum açma * |210|420|630|840|1050|1260|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1... 3|0, 0,25, 0,5, 1... 4|0, 0,25, 0,5, 1... 5|0, 0,25, 0,5, 1... 6|
|Çoğaltma sayısı|1\.|1\.|1\.|1\.|1\.|1\.|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçeği okuyun|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*Her veritabanı için en fazla eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ise, en fazla eş zamanlı çalışan 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 ise, en fazla eş zamanlı çalışan 5. nesil ' den itibaren, sanal çekirdek başına en fazla 100 ' 50 dir.  Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Genel Amaçlı hizmet katmanı: Nesil 4 işlem platformu (Bölüm 2)

|İşlem boyutu|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|168|
|Havuz başına en fazla veritabanı|500|500|500|500|500|500|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|1536|2048|2048|2048|3584|4096|
|En fazla günlük boyutu (GB)|461|614|614|614|1075|1229|
|TempDB boyutu (GB)|224|256|288|320|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Hedef ıOPS (64 KB)|3500|4000|4500|5000|7000|7000|
|Günlük hızı sınırları (MBps)|32,8125|37,5|37,5|37,5|37,5|37,5|
|Havuz başına en fazla eş zamanlı çalışan (istek) *|1470|1680|1890|2100|3360|5040|
|En fazla eşzamanlı oturum açma Havuzu (istek) *|1470|1680|1890|2100|3360|5040|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1... 7|0, 0,25, 0,5, 1... 8|0, 0,25, 0,5, 1... 9|0, 0,25, 0,5, 1... 10|0, 0,25, 0,5, 1... 10, 16|0, 0,25, 0,5, 1... 10, 16, 24|
|Çoğaltma sayısı|1\.|1\.|1\.|1\.|1\.|1\.|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçeği okuyun|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*Her veritabanı için en fazla eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ise, en fazla eş zamanlı çalışan 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 ise, en fazla eş zamanlı çalışan 5. nesil ' den itibaren, sanal çekirdek başına en fazla 100 ' 50 dir.  Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Genel Amaçlı hizmet katmanı: Nesil 5 işlem platformu (Bölüm 1)

|İşlem boyutu|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|6|8|10|12|14|
|Bellek (GB)|10.2|20,4|30,6|40,8|51|61,2|71,4|
|Havuz başına en fazla veritabanı|100|200|500|500|500|500|500|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|512|756|756|1536|1536|1536|
|En fazla günlük boyutu (GB)|154|227|227|461|461|461|461|
|TempDB boyutu (GB)|64|128|192|256|320|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Hedef ıOPS (64 KB)|1000|2000|3000|4000|5000|6000|7000|
|Günlük hızı sınırları (MBps)|4,6875|9,375|14,0625|18,75|23,4375|28,125|32,8125|
|Havuz başına en fazla eş zamanlı çalışan (istek) *|210|420|630|840|1050|1260|1470|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) *|210|420|630|840|1050|1260|1470|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|30000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1... 4|0, 0,25, 0,5, 1... 6|0, 0,25, 0,5, 1... 8|0, 0,25, 0,5, 1... 10|0, 0,25, 0,5, 1... 12|0, 0,25, 0,5, 1... 14|
|Çoğaltma sayısı|1\.|1\.|1\.|1\.|1\.|1\.|1\.|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçeği okuyun|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*Her veritabanı için en fazla eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ise, en fazla eş zamanlı çalışan 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 ise, en fazla eş zamanlı çalışan 5. nesil ' den itibaren, sanal çekirdek başına en fazla 100 ' 50 dir.  Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Genel Amaçlı hizmet katmanı: Nesil 5 işlem platformu (Bölüm 2)

|İşlem boyutu|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|16|18|20|24|32|40|80|
|Bellek (GB)|81,6|91,8|102|122,4|163,2|204|408|
|Havuz başına en fazla veritabanı|500|500|500|500|500|500|500|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|2048|2048|3072|3072|4096|4096|4096|
|En fazla günlük boyutu (GB)|614|614|922|922|1229|1229|1229|
|TempDB boyutu (GB)|384|384|384|384|384|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Hedef ıOPS (64 KB)|7000|7000|7000|7000|7000|7000|7000|
|Günlük hızı sınırları (MBps)|37,5|37,5|37,5|37,5|37,5|37,5|37,5|
|Havuz başına en fazla eş zamanlı çalışan (istek) *|1680|1890|2100|2520|33600|4200|8400|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) *|1680|1890|2100|2520|33600|4200|8400|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1... 16|0, 0,25, 0,5, 1... 18|0, 0,25, 0,5, 1... 20|0, 0,25, 0,5, 1... 20, 24|0, 0,25, 0,5, 1... 20, 24, 32|0, 0,25, 0,5, 1... 16, 24, 32, 40|0, 0,25, 0,5, 1... 16, 24, 32, 40, 80|
|Çoğaltma sayısı|1\.|1\.|1\.|1\.|1\.|1\.|1\.|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçeği okuyun|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*Her veritabanı için en fazla eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ise, en fazla eş zamanlı çalışan 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 ise, en fazla eş zamanlı çalışan 5. nesil ' den itibaren, sanal çekirdek başına en fazla 100 ' 50 dir.  Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

## <a name="business-critical-service-tier-storage-sizes-and-compute-sizes"></a>İş Açısından Kritik hizmet katmanı: Depolama boyutları ve işlem boyutları

> [!IMPORTANT]
> Yeni 4. nesil veritabanları artık Avustralya Doğu veya Brezilya Güney bölgelerinde desteklenmez.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-1"></a>İş Açısından Kritik hizmet katmanı: Nesil 4 işlem platformu (Bölüm 1)

|İşlem boyutu|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1\.|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Havuz başına en fazla veritabanı|Bu işlem boyutu için yalnızca tek bir DBs destekleniyor|50|100|100|100|100|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|1\.|2|3|4|5|6|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En fazla veri boyutu (GB)|650|650|650|650|650|650|
|En fazla günlük boyutu (GB)|195|195|195|195|195|195|
|TempDB boyutu (GB)|32|64|96|128|160|192|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef ıOPS (64 KB)|5000|10000|15000|20000|25000|30000|
|Günlük hızı sınırları (MBps)|10|20|30|40|50|60|
|Havuz başına en fazla eş zamanlı çalışan (istek) *|210|420|630|840|1050|1260|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) *|210|420|630|840|1050|1260|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|Yok|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1... 3|0, 0,25, 0,5, 1... 4|0, 0,25, 0,5, 1... 5|0, 0,25, 0,5, 1... 6|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçeği okuyun|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*Her veritabanı için en fazla eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ise, en fazla eş zamanlı çalışan 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 ise, en fazla eş zamanlı çalışan 5. nesil ' den itibaren, sanal çekirdek başına en fazla 100 ' 50 dir.  Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>İş Açısından Kritik hizmet katmanı: Nesil 4 işlem platformu (Bölüm 2)

|İşlem boyutu|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|7|8|9|10|16|24|
|Bellek (GB)|81,6|91,8|102|122,4|163,2|204|
|Havuz başına en fazla veritabanı|100|100|100|100|100|100|
|Columnstore desteği|Yok|Yok|Yok|Yok|Yok|Yok|
|Bellek içi OLTP depolaması (GB)|7|8|9,5|11|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En fazla veri boyutu (GB)|650|650|650|650|1024|1024|
|En fazla günlük boyutu (GB)|195|195|195|195|307|307|
|TempDB boyutu (GB)|224|256|288|320|384|384|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef ıOPS (64 KB)|35000|40000|45000|50000|80000|120000|
|Günlük hızı sınırları (MBps)|70|80|80|80|80|80|
|Havuz başına en fazla eş zamanlı çalışan (istek) *|1470|1680|1890|2100|3360|5040|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) *|1470|1680|1890|2100|3360|5040|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1... 7|0, 0,25, 0,5, 1... 8|0, 0,25, 0,5, 1... 9|0, 0,25, 0,5, 1... 10|0, 0,25, 0,5, 1... 10, 16|0, 0,25, 0,5, 1... 10, 16, 24|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçeği okuyun|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*Her veritabanı için en fazla eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ise, en fazla eş zamanlı çalışan 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 ise, en fazla eş zamanlı çalışan 5. nesil ' den itibaren, sanal çekirdek başına en fazla 100 ' 50 dir.  Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

#### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>İş Açısından Kritik hizmet katmanı: Nesil 5 işlem platformu (Bölüm 1)

|İşlem boyutu|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|6|8|10|12|14|
|Bellek (GB)|10.2|20,4|30,6|40,8|51|61,2|71,4|
|Havuz başına en fazla veritabanı|Bu işlem boyutu için yalnızca tek bir DBs destekleniyor|50|100|100|100|100|100|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|1,571|3,142|4,713|6,284|8,655|11,026|13,397|
|En fazla veri boyutu (GB)|1024|1024|1536|1536|1536|3072|3072|
|En fazla günlük boyutu (GB)|307|307|307|461|461|922|922|
|TempDB boyutu (GB)|64|128|192|256|320|384|384|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef ıOPS (64 KB)|5000|10000|15000|20000|25000|30000|35000|
|Günlük hızı sınırları (MBps)|15|30|45|60|75|90|105|
|Havuz başına en fazla eş zamanlı çalışan (istek) *|210|420|630|840|1050|1260|1470|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) *|210|420|630|840|1050|1260|1470|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|30000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|Yok|0, 0,25, 0,5, 1... 4|0, 0,25, 0,5, 1... 6|0, 0,25, 0,5, 1... 8|0, 0,25, 0,5, 1... 10|0, 0,25, 0,5, 1... 12|0, 0,25, 0,5, 1... 14|
|Çoğaltma sayısı|4|4|4|4|4|4|4|
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçeği okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*Her veritabanı için en fazla eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ise, en fazla eş zamanlı çalışan 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 ise, en fazla eş zamanlı çalışan 5. nesil ' den itibaren, sanal çekirdek başına en fazla 100 ' 50 dir.  Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

#### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>İş Açısından Kritik hizmet katmanı: Nesil 5 işlem platformu (Bölüm 2)

|İşlem boyutu|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|16|18|20|24|32|40|80|
|Bellek (GB)|81,6|91,8|102|122,4|163,2|204|408|
|Havuz başına en fazla veritabanı|100|100|100|100|100|100|100|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|15,768|18,139|20,51|25,252|37,936|52,22|131,64|
|En fazla veri boyutu (GB)|3072|3072|3072|4096|4096|4096|4096|
|En fazla günlük boyutu (GB)|922|922|922|1229|1229|1229|1229|
|TempDB boyutu (GB)|384|384|384|384|384|384|384|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef ıOPS (64 KB)|40000|45000|50000|60000|80000|100000|200000|
|Günlük hızı sınırları (MBps)|120|120|120|120|120|120|120|
|Havuz başına en fazla eş zamanlı çalışan (istek) *|1680|1890|2100|2520|3360|4200|8400|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) *|1680|1890|2100|2520|3360|4200|8400|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|30000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1... 16|0, 0,25, 0,5, 1... 18|0, 0,25, 0,5, 1... 20|0, 0,25, 0,5, 1... 20, 24|0, 0,25, 0,5, 1... 20, 24, 32|0, 0,25, 0,5, 1... 20, 24, 32, 40|0, 0,25, 0,5, 1... 20, 24, 32, 40, 80|
|Çoğaltma sayısı|4|4|4|4|4|4|4|
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçeği okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*Her veritabanı için en fazla eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ise, en fazla eş zamanlı çalışan 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 ise, en fazla eş zamanlı çalışan 5. nesil ' den itibaren, sanal çekirdek başına en fazla 100 ' 50 dir.  Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

Elastik havuzun tüm sanal çekirdekleri meşgulse, havuzdaki her veritabanı, sorguları işlemek için eşit miktarda işlem kaynağı alır. SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Elastik havuz kaynak paylaşımı eşitliği, veritabanı başına vCore en düşük değeri sıfır olmayan bir değere ayarlandığında her bir veritabanı için garanti edilen herhangi bir kaynak miktarına ek olarak yapılır.

## <a name="database-properties-for-pooled-databases"></a>Havuza alınmış veritabanları için veritabanı özellikleri

Aşağıdaki tabloda, havuza alınmış veritabanlarının özellikleri açıklanmaktadır.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına en fazla sanal çekirdek |Havuzdaki diğer veritabanlarının kullanımına bağlı olarak varsa, havuzdaki herhangi bir veritabanının kullanabileceği maksimum sanal çekirdek sayısı. Veritabanı başına en fazla sanal çekirdek, bir veritabanı için kaynak garantisi değildir. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımında en üst düzey sanal çekirdekleri işlemek için, veritabanı başına maksimum sanal çekirdek ayarlayın. Havuz genellikle tüm veritabanlarının eşzamanlı olarak en fazla düzeyde olmadığı veritabanları için sık ve soğuk kullanım düzenlerini varsaydığından, bazı aşırı işleme beklenmektedir.|
| Veritabanı başına en az sanal çekirdek |Havuzdaki herhangi bir veritabanının garanti edilmesi gereken en az sayıda sanal çekirdek. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına en düşük sanal çekirdekler 0 olarak ayarlanabilir ve aynı zamanda varsayılan değerdir. Bu özellik 0 ve veritabanı başına ortalama sanal çekirdek kullanımı arasındaki herhangi bir yere ayarlanır. Havuzdaki veritabanlarının sayısı ve veritabanı başına en düşük sanal çekirdekler, havuz başına sanal çekirdekleri aşamaz.|
| Veritabanı başına en fazla depolama |Bir havuzdaki bir veritabanı için Kullanıcı tarafından ayarlanan en büyük veritabanı boyutu. Havuza alınmış veritabanları, ayrılmış havuz depolama alanını paylaşır, bu nedenle bir veritabanının ulaşabileceği boyut, kalan havuz depolama ve veritabanı boyutundan daha küçük olacak şekilde sınırlıdır. En fazla veritabanı boyutu, veri dosyalarının boyut üst sınırını ifade eder ve günlük dosyaları tarafından kullanılan alanı içermez. |
|||

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için sanal çekirdek kaynak sınırları için bkz [. sanal çekirdek tabanlı satın alma modelini kullanarak tek veritabanları için kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md)
- Tek bir veritabanı için DTU kaynak sınırları için bkz [. DTU tabanlı satın alma modelini kullanarak tek veritabanları için kaynak limitleri](sql-database-dtu-resource-limits-single-databases.md)
- Elastik havuzların DTU kaynak sınırları için bkz [. DTU tabanlı satın alma modelini kullanarak elastik havuzlar için kaynak limitleri](sql-database-dtu-resource-limits-elastic-pools.md)
- Yönetilen örnekler için kaynak sınırları için bkz. [yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md).
- Genel Azure limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
- Bir veritabanı sunucusundaki kaynak sınırları hakkında daha fazla bilgi için bkz. sunucu ve abonelik düzeylerindeki sınırlamalar hakkında bilgi için bkz. [SQL veritabanı sunucusundaki kaynak sınırlarına genel bakış](sql-database-resource-limits-database-server.md) .
