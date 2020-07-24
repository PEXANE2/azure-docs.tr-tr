---
title: Elastik havuz sanal çekirdek kaynak sınırları
description: Bu sayfa, Azure SQL veritabanı 'nda esnek havuzlar için bazı yaygın sanal çekirdek kaynak sınırlarını açıklamaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab, sstein
ms.date: 07/21/2020
ms.openlocfilehash: f71daab55139f6b4690df50472928db466774cb3
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87128276"
---
# <a name="resource-limits-for-elastic-pools-using-the-vcore-purchasing-model"></a>Sanal çekirdek satın alma modelini kullanarak elastik havuzlar için kaynak sınırları
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu makalede, sanal çekirdek satın alma modeli kullanılarak Azure SQL veritabanı elastik havuzları ve havuza alınmış veritabanları için ayrıntılı kaynak sınırları sağlanmaktadır.

DTU satın alma modeli sınırları için bkz. [SQL VERITABANı DTU kaynak limitleri-elastik havuzlar](resource-limits-dtu-elastic-pools.md).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanını daraltmanız gerekebilir. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda dosya alanını yönetme](file-space-manage.md).

Hizmet katmanını, işlem boyutunu (hizmet hedefi) ve depolama miktarını [Azure Portal](elastic-pool-manage.md#azure-portal), [POWERSHELL](elastic-pool-manage.md#powershell), [Azure CLI](elastic-pool-manage.md#azure-cli)veya [REST API](elastic-pool-manage.md#rest-api)kullanarak ayarlayabilirsiniz.

> [!IMPORTANT]
> Ölçeklendirme Kılavuzu ve değerlendirmeleri için bkz. [elastik havuz ölçekleme](elastic-pool-scale.md).

## <a name="general-purpose---provisioned-compute---gen4"></a>Genel amaçlı-sağlanan işlem-4. nesil

> [!IMPORTANT]
> Yeni 4. nesil veritabanları artık Avustralya Doğu veya Brezilya Güney bölgelerinde desteklenmez.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Genel amaçlı hizmet katmanı: nesil 4 işlem platformu (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|
|Sanal çekirdek|1|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|100|200|500|500|500|500|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|512|756|1536|1536|1536|2048|
|En büyük günlük boyutu|154|227|461|461|461|614|
|TempDB en fazla veri boyutu (GB)|32|64|96|128|160|192|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup> |400|800|1200|1600|2000|2400|
|Havuz başına en fazla günlük hızı (MBps)|4,7|9.4|14,1|18,8|23,4|28,1|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup> |210|420|630|840|1050|1260|
|Havuz başına en fazla eşzamanlı oturum açma <sup>3</sup> |210|420|630|840|1050|1260|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1... 3|0, 0,25, 0,5, 1... 4|0, 0,25, 0,5, 1... 5|0, 0,25, 0,5, 1... 6|
|Çoğaltma sayısı|1|1|1|1|1|1|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Genel amaçlı hizmet katmanı: nesil 4 işlem platformu (Bölüm 2)

|İşlem boyutu (hizmet hedefi)|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|
|Sanal çekirdek|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|159,5|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|500|500|500|500|500|500|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|2048|2048|2048|2048|3584|4096|
|En fazla günlük boyutu (GB)|614|614|614|614|1075|1229|
|TempDB en fazla veri boyutu (GB)|224|256|288|320|512|768|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|2800|3200|3600|4000|6400|9600|
|Havuz başına en fazla günlük hızı (MBps)|32,8|37,5|37,5|37,5|37,5|37,5|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|1470|1680|1890|2100|3360|5040|
|Maksimum eşzamanlı oturum açma Havuzu (istek) <sup>3</sup>|1470|1680|1890|2100|3360|5040|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1... 7|0, 0,25, 0,5, 1... 8|0, 0,25, 0,5, 1... 9|0, 0,25, 0,5, 1... 10|0, 0,25, 0,5, 1... 10, 16|0, 0,25, 0,5, 1... 10, 16, 24|
|Çoğaltma sayısı|1|1|1|1|1|1|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).    

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

## <a name="general-purpose---provisioned-compute---gen5"></a>Genel amaçlı-sağlanan işlem-5. nesil

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Genel amaçlı hizmet katmanı: nesil 5 işlem platformu (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|Sanal çekirdek|2|4|6|8|10|12|14|
|Bellek (GB)|10,4|20,8|31,1|41,5|51,9|62,3|72,7|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|100|200|500|500|500|500|500|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|512|756|1536|1536|1536|2048|2048|
|En fazla günlük boyutu (GB)|154|227|461|461|461|614|614|
|TempDB en fazla veri boyutu (GB)|64|128|192|256|320|384|448|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|800|1600|2400|3200|4000|4800|5600|
|Havuz başına en fazla günlük hızı (MBps)|9.4|18,8|28,1|37,5|37,5|37,5|37,5|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|210|420|630|840|1050|1260|1470|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|210|420|630|840|1050|1260|1470|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1... 4|0, 0,25, 0,5, 1... 6|0, 0,25, 0,5, 1... 8|0, 0,25, 0,5, 1... 10|0, 0,25, 0,5, 1... 12|0, 0,25, 0,5, 1... 14|
|Çoğaltma sayısı|1|1|1|1|1|1|1|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Genel amaçlı hizmet katmanı: nesil 5 işlem platformu (Bölüm 2)

|İşlem boyutu (hizmet hedefi)|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|Sanal çekirdek|16|18|20|24|32|40|80|
|Bellek (GB)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|500|500|500|500|500|500|500|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|2048|3072|3072|3072|4096|4096|4096|
|En fazla günlük boyutu (GB)|614|922|922|922|1229|1229|1229|
|TempDB en fazla veri boyutu (GB)|512|576|640|768|1024|1280|2560|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup> |6.400|7.200|8,000|9.600|12.800|16.000|16.000|
|Havuz başına en fazla günlük hızı (MBps)|37,5|37,5|37,5|37,5|37,5|37,5|37,5|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|1680|1890|2100|2520|3360|4200|8400|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|1680|1890|2100|2520|3360|4200|8400|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1... 16|0, 0,25, 0,5, 1... 18|0, 0,25, 0,5, 1... 20|0, 0,25, 0,5, 1... 20, 24|0, 0,25, 0,5, 1... 20, 24, 32|0, 0,25, 0,5, 1... 16, 24, 32, 40|0, 0,25, 0,5, 1... 16, 24, 32, 40, 80|
|Çoğaltma sayısı|1|1|1|1|1|1|1|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

## <a name="general-purpose---provisioned-compute---fsv2-series"></a>Genel amaçlı-sağlanan işlem-Fsv2-Series

### <a name="fsv2-series-compute-generation-part-1"></a>Fsv2 serisi işlem oluşturma (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14| GP_Fsv2_16|
|:---| ---:|---:|---:|---:|---:|
|İşlem oluşturma|Fsv2 serisi|Fsv2 serisi|Fsv2 serisi|Fsv2 serisi|Fsv2 serisi|
|Sanal çekirdek|8|10|12|14|16|
|Bellek (GB)|15,1|18,9|22,7|26,5|30,2|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|500|500|500|500|500|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|1024|1024|1024|1024|1536|
|En fazla günlük boyutu (GB)|336|336|336|336|512|
|TempDB en fazla veri boyutu (GB)|333|333|333|333|333|
|Depolama türü|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|2560|3200|3840|4480|5120|
|Havuz başına en fazla günlük hızı (MBps)|30|30|30|30|30|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|400|500|600|700|800|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|800|1000|1200|1400|1600|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0-8|0-10|0-12|0-14|0-16|
|Çoğaltma sayısı|1|1|1|1|1|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|


<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="fsv2-series-compute-generation-part-2"></a>Fsv2 serisi işlem oluşturma (2. bölüm)

|İşlem boyutu (hizmet hedefi)|GP_Fsv2_18|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32| GP_Fsv2_36|GP_Fsv2_72|
|:---| ---:|---:|---:|---:|---:|---:|
|İşlem oluşturma|Fsv2 serisi|Fsv2 serisi|Fsv2 serisi|Fsv2 serisi|Fsv2 serisi|Fsv2 serisi|
|Sanal çekirdek|18|20|24|32|36|72|
|Bellek (GB)|34,0|37,8|45,4|60,5|68,0|136,0|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|500|500|500|500|500|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|1536|1536|1536|3072|3072|4096|
|En fazla günlük boyutu (GB)|512|512|512|1024|1024|1024|
|TempDB en fazla veri boyutu (GB)|83,25|92,5|111|148|166,5|333|
|Depolama türü|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|5760|6400|7680|10240|11520|23040|
|Havuz başına en fazla günlük hızı (MBps)|30|30|30|30|30|30|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|900|1000|1200|1600|1800|3600|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|1800|2000|2400|3200|3600|7200|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0-18|0-20|0-24|0-32|0-36|0-72|
|Çoğaltma sayısı|1|1|1|1|1|1|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Okuma Amaçlı Ölçeği Genişletme|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

## <a name="business-critical---provisioned-compute---gen4"></a>İş açısından kritik-sağlanan işlem-4. nesil

> [!IMPORTANT]
> Yeni 4. nesil veritabanları artık Avustralya Doğu veya Brezilya Güney bölgelerinde desteklenmez.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-1"></a>İş açısından kritik hizmet katmanı: nesil 4 işlem platformu (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|
|Sanal çekirdek|2|3|4|5|6|
|Bellek (GB)|14|21|28|35|42|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|50|100|100|100|100|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|2|3|4|5|6|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En fazla veri boyutu (GB)|1024|1024|1024|1024|1024|
|En fazla günlük boyutu (GB)|307|307|307|307|307|
|TempDB en fazla veri boyutu (GB)|64|96|128|160|192|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|9,000|13.500|18.000|22.500|27.000|
|Havuz başına en fazla günlük hızı (MBps)|20|30|40|50|60|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|420|630|840|1050|1260|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|420|630|840|1050|1260|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1... 3|0, 0,25, 0,5, 1... 4|0, 0,25, 0,5, 1... 5|0, 0,25, 0,5, 1... 6|
|Çoğaltma sayısı|4|4|4|4|4|
|Çoklu-AZ|Yes|Yes|Yes|Yes|Yes|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>İş açısından kritik hizmet katmanı: nesil 4 işlem platformu (Bölüm 2)

|İşlem boyutu (hizmet hedefi)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|
|Sanal çekirdek|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|159,5|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|100|100|100|100|100|100|
|Columnstore desteği|Yok|Yok|Yok|Yok|Yok|Yok|
|Bellek içi OLTP depolaması (GB)|7|8|9,5|11|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En fazla veri boyutu (GB)|1024|1024|1024|1024|1024|1024|
|En fazla günlük boyutu (GB)|307|307|307|307|307|307|
|TempDB en fazla veri boyutu (GB)|224|256|288|320|512|768|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|31.500|36.000|40.500|45.000|72.000|96.000|
|Havuz başına en fazla günlük hızı (MBps)|70|80|80|80|80|80|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|1470|1680|1890|2100|3360|5040|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|1470|1680|1890|2100|3360|5040|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1... 7|0, 0,25, 0,5, 1... 8|0, 0,25, 0,5, 1... 9|0, 0,25, 0,5, 1... 10|0, 0,25, 0,5, 1... 10, 16|0, 0,25, 0,5, 1... 10, 16, 24|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çoklu-AZ|Yes|Yes|Yes|Yes|Yes|Yes|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

## <a name="business-critical---provisioned-compute---gen5"></a>İş açısından kritik-sağlanan işlem-5. nesil

### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>İş açısından kritik hizmet katmanı: nesil 5 işlem platformu (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|Sanal çekirdek|4|6|8|10|12|14|
|Bellek (GB)|20,8|31,1|41,5|51,9|62,3|72,7|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|50|100|100|100|100|100|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|3,14|4,71|6,28|8,65|11,02|13,39|
|En fazla veri boyutu (GB)|1024|1536|1536|1536|3072|3072|
|En fazla günlük boyutu (GB)|307|307|461|461|922|922|
|TempDB en fazla veri boyutu (GB)|128|192|256|320|384|448|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|18.000|27.000|36.000|45.000|54.000|63.000|
|Havuz başına en fazla günlük hızı (MBps)|60|90|120|120|120|120|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|420|630|840|1050|1260|1470|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|420|630|840|1050|1260|1470|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1... 4|0, 0,25, 0,5, 1... 6|0, 0,25, 0,5, 1... 8|0, 0,25, 0,5, 1... 10|0, 0,25, 0,5, 1... 12|0, 0,25, 0,5, 1... 14|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çoklu-AZ|Yes|Yes|Yes|Yes|Yes|Yes|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>İş açısından kritik hizmet katmanı: nesil 5 işlem platformu (Bölüm 2)

|İşlem boyutu (hizmet hedefi)|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|Sanal çekirdek|16|18|20|24|32|40|80|
|Bellek (GB)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|100|100|100|100|100|100|100|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|15,77|18,14|20,51|25,25|37,94|52,23|131,68|
|En fazla veri boyutu (GB)|3072|3072|3072|4096|4096|4096|4096|
|En fazla günlük boyutu (GB)|922|922|922|1229|1229|1229|1229|
|TempDB en fazla veri boyutu (GB)|512|576|640|768|1024|1280|2560|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|72.000|81.000|90,000|108.000|144.000|180.000|256.000|
|Havuz başına en fazla günlük hızı (MBps)|120|120|120|120|120|120|120|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|1680|1890|2100|2520|3360|4200|8400|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|1680|1890|2100|2520|3360|4200|8400|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0, 0,25, 0,5, 1... 16|0, 0,25, 0,5, 1... 18|0, 0,25, 0,5, 1... 20|0, 0,25, 0,5, 1... 20, 24|0, 0,25, 0,5, 1... 20, 24, 32|0, 0,25, 0,5, 1... 20, 24, 32, 40|0, 0,25, 0,5, 1... 20, 24, 32, 40, 80|
|Çoğaltma sayısı|4|4|4|4|4|4|4|
|Çoklu-AZ|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

## <a name="business-critical---provisioned-compute---m-series"></a>İş açısından kritik-sağlanan işlem-a serisi

### <a name="m-series-compute-generation-part-1"></a>D serisi işlem oluşturma (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:---| ---:|---:|---:|---:|---:|---:|
|İşlem oluşturma|M serisi|M serisi|M serisi|M serisi|M serisi|M serisi|
|Sanal çekirdek|8|10|12|14|16|18|
|Bellek (GB)|235,4|294,3|353,2|412,0|470,9|529,7|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|100|100|100|100|100|100|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|64|80|96|112|128|150|
|En fazla veri boyutu (GB)|512|640|768|896|1024|1152|
|En fazla günlük boyutu (GB)|171|213|256|299|341|384|
|TempDB en fazla veri boyutu (GB)|256|320|384|448|512|576|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|12.499|15.624|18.748|21.873|24.998|28.123|
|Havuz başına en fazla günlük hızı (MBps)|48|60|72|84|96|108|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|800|1.000|1.200|1.400|1.600|1.800|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|800|1.000|1.200|1.400|1.600|1.800|
|Maks. eş zamanlı oturum|30000|30000|30000|30000|30000|30000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0-8|0-10|0-12|0-14|0-16|0-18|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çoklu-AZ|Hayır|Hayır|Hayır|Hayır|Hayır|Hayır|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

Elastik havuzun tüm sanal çekirdekleri meşgulse, havuzdaki her veritabanı, sorguları işlemek için eşit miktarda işlem kaynağı alır. Azure SQL veritabanı, işlem zamanının eşit dilimlerini sağlayarak veritabanları arasında kaynak paylaşımı eşitliği sağlar. Elastik havuz kaynak paylaşımı eşitliği, veritabanı başına vCore en düşük değeri sıfır olmayan bir değere ayarlandığında her bir veritabanı için garanti edilen herhangi bir kaynak miktarına ek olarak yapılır.



### <a name="m-series-compute-generation-part-2"></a>D serisi işlem oluşturma (2. bölüm)

|İşlem boyutu (hizmet hedefi)|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:---| ---:|---:|---:|---:|---:|
|İşlem oluşturma|M serisi|M serisi|M serisi|M serisi|M serisi|
|Sanal çekirdek|20|24|32|64|128|
|Bellek (GB)|588,6|706,3|941,8|1883,5|3767,0|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|100|100|100|100|100|100|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|172|216|304|704|1768|
|En fazla veri boyutu (GB)|1280|1536|2048|4096|4096|
|En fazla günlük boyutu (GB)|427|512|683|1024|1024|
|TempDB en fazla veri boyutu (GB)|4096|2048|1024|768|640|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|31.248|37.497|49.996|99.993|160.000|
|Havuz başına en fazla günlük hızı (MBps)|120|144|192|264|264|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|2.000|2.400|3.200|6.400|12.800|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|2.000|2.400|3.200|6.400|12.800|
|Maks. eş zamanlı oturum|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|4|4|4|4|4|
|Çoklu-AZ|Hayır|Hayır|Hayır|Hayır|Hayır|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

Elastik havuzun tüm sanal çekirdekleri meşgulse, havuzdaki her veritabanı, sorguları işlemek için eşit miktarda işlem kaynağı alır. Azure SQL veritabanı, işlem zamanının eşit dilimlerini sağlayarak veritabanları arasında kaynak paylaşımı eşitliği sağlar. Elastik havuz kaynak paylaşımı eşitliği, veritabanı başına vCore en düşük değeri sıfır olmayan bir değere ayarlandığında her bir veritabanı için garanti edilen herhangi bir kaynak miktarına ek olarak yapılır.


## <a name="database-properties-for-pooled-databases"></a>Havuza alınmış veritabanları için veritabanı özellikleri

Aşağıdaki tabloda, havuza alınmış veritabanlarının özellikleri açıklanmaktadır.

> [!NOTE]
> Elastik havuzlardaki ayrı veritabanlarının kaynak sınırları genellikle aynı işlem boyutuna (hizmet hedefi) sahip olan havuzların dışındaki tek veritabanları için de aynıdır. Örneğin, bir GP_Gen4_1 veritabanı için en fazla eş zamanlı çalışan 200 çalışandır. Bu nedenle, bir GP_Gen4_1 havuzundaki bir veritabanı için en fazla eş zamanlı çalışan da 200 çalışandır. GP_Gen4_1 havuzundaki eşzamanlı çalışanların toplam sayısı 210 ' dir.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına en fazla sanal çekirdek |Havuzdaki diğer veritabanlarının kullanımına bağlı olarak varsa, havuzdaki herhangi bir veritabanının kullanabileceği maksimum sanal çekirdek sayısı. Veritabanı başına en fazla sanal çekirdek, bir veritabanı için kaynak garantisi değildir. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımında en üst düzey sanal çekirdekleri işlemek için, veritabanı başına maksimum sanal çekirdek ayarlayın. Havuz genellikle tüm veritabanlarının eşzamanlı olarak en fazla düzeyde olmadığı veritabanları için sık ve soğuk kullanım düzenlerini varsaydığından, bazı aşırı işleme beklenmektedir.|
| Veritabanı başına en az sanal çekirdek |Havuzdaki herhangi bir veritabanının garanti edilmesi gereken en az sayıda sanal çekirdek. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına en düşük sanal çekirdekler 0 olarak ayarlanabilir ve aynı zamanda varsayılan değerdir. Bu özellik 0 ve veritabanı başına ortalama sanal çekirdek kullanımı arasındaki herhangi bir yere ayarlanır. Havuzdaki veritabanlarının sayısı ve veritabanı başına en düşük sanal çekirdekler, havuz başına sanal çekirdekleri aşamaz.|
| Veritabanı başına en fazla depolama |Bir havuzdaki bir veritabanı için Kullanıcı tarafından ayarlanan en büyük veritabanı boyutu. Havuza alınmış veritabanları, ayrılmış havuz depolama alanını paylaşır, bu nedenle bir veritabanının ulaşabileceği boyut, kalan havuz depolama ve veritabanı boyutundan daha küçük olacak şekilde sınırlıdır. En fazla veritabanı boyutu, veri dosyalarının boyut üst sınırını ifade eder ve günlük dosyaları tarafından kullanılan alanı içermez. |
|||

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için sanal çekirdek kaynak sınırları için bkz [. Vcore satın alma modelini kullanarak tek veritabanları için kaynak sınırları](resource-limits-vcore-single-databases.md)
- Tek bir veritabanı için DTU kaynak sınırları için bkz [. DTU satın alma modelini kullanarak tek veritabanları için kaynak limitleri](resource-limits-dtu-single-databases.md)
- Elastik havuzların DTU kaynak sınırları için bkz [. DTU satın alma modelini kullanarak elastik havuzlar için kaynak limitleri](resource-limits-dtu-elastic-pools.md)
- Yönetilen örnekler için kaynak sınırları için bkz. [yönetilen örnek kaynak sınırları](../managed-instance/resource-limits.md).
- Genel Azure limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md).
- Mantıksal bir SQL Server üzerindeki kaynak limitleri hakkında bilgi için bkz. sunucu ve abonelik düzeylerindeki sınırlamalar hakkında bilgi için bkz. [MANTıKSAL SQL Server 'da kaynak sınırlarına genel bakış](resource-limits-logical-server.md) .
