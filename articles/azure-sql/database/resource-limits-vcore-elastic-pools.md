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
ms.date: 06/10/2020
ms.openlocfilehash: 4ffd92c0641b74682a74ffd2898e226999ac2dd4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84668466"
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
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|
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
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|YOK|YOK|YOK|YOK|YOK|
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
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|
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
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|YOK|YOK|YOK|YOK|YOK|
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
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
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
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
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
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
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
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

## <a name="general-purpose---provisioned-compute---fsv2-series"></a>Genel amaçlı-sağlanan işlem-Fsv2-Series

### <a name="fsv2-series-compute-generation-preview"></a>Fsv2 serisi işlem oluşturma (Önizleme)

|İşlem boyutu (hizmet hedefi)|GP_Fsv2_72|
|:--- | --: |
|İşlem oluşturma|Fsv2 serisi|
|Sanal çekirdek|72|
|Bellek (GB)|136,2|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|500|
|Columnstore desteği|Evet|
|Bellek içi OLTP depolaması (GB)|YOK|
|En fazla veri boyutu (GB)|4096|
|En fazla günlük boyutu (GB)|1024|
|TempDB en fazla veri boyutu (GB)|333|
|Depolama türü|Premium (uzak) depolama|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|16.000|
|Havuz başına en fazla günlük hızı (MBps)|37,5|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|3780|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|3780|
|Maks. eş zamanlı oturum|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0-72|
|Çoğaltma sayısı|1|
|Çoklu-AZ|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|
|Dahil edilen yedekleme depolaması|1X DB boyutu|

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
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|
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
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|
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
|Columnstore desteği|YOK|YOK|YOK|YOK|YOK|YOK|
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
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|
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
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
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
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|
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
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
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
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

<sup>1</sup> daha fazla dikkat edilmesi için [yoğun esnek havuzlarda kaynak yönetimine](elastic-pool-resource-management.md) bakın.

<sup>2</sup> GÇ boyutları IÇIN 8 kb Ile 64 KB arasında değişen en büyük değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

<sup>3</sup> tek bir veritabanı için maksimum eşzamanlı çalışan (istek) için bkz. [tek veritabanı kaynak limitleri](resource-limits-vcore-single-databases.md). Örneğin, elastik havuz 5. nesil kullanıyorsa ve veritabanı başına en fazla sanal çekirdek 2 ' ye ayarlanırsa, en fazla eşzamanlı çalışan değeri 200 olur.  Veritabanı başına en fazla sanal çekirdek 0,5 olarak ayarlanırsa, 5. nesil ' den itibaren maksimum eşzamanlı çalışan 50 değeri, sanal çekirdek başına en 100 fazla eşzamanlı çalışan olur. Daha az 1 sanal çekirdek veya daha az 1 olan veritabanı başına diğer en fazla sanal çekirdek ayarları için, en fazla eş zamanlı çalışan sayısı benzer şekilde ölçeklendirildi.

## <a name="business-critical---provisioned-compute---m-series"></a>İş açısından kritik-sağlanan işlem-a serisi

### <a name="m-series-compute-generation-preview"></a>A serisi işlem oluşturma (Önizleme)

|İşlem boyutu (hizmet hedefi)|BC_M_128|
|:--- | --: |
|İşlem oluşturma|M serisi|
|Sanal çekirdek|128|
|Bellek (GB)|3767,1|
|Havuz başına en fazla veritabanı sayısı <sup>1</sup>|100|
|Columnstore desteği|Evet|
|Bellek içi OLTP depolaması (GB)|1768|
|En fazla veri boyutu (GB)|4096|
|En fazla günlük boyutu (GB)|2048|
|TempDB en fazla veri boyutu (GB)|4096|
|Depolama türü|Yerel SSD|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Havuz başına en fazla veri ıOPS <sup>2</sup>|200,000|
|Havuz başına en fazla günlük hızı (MBps)|333|
|Havuz başına en fazla eş zamanlı çalışan (istek) <sup>3</sup>|13.440|
|Havuz başına en fazla eşzamanlı oturum açma sayısı (istek) <sup>3</sup>|13.440|
|Maks. eş zamanlı oturum|30.000|
|Veritabanı başına en az/en fazla elastik havuz sanal çekirdek seçimi|0-128|
|Çoğaltma sayısı|4|
|Çoklu-AZ|Evet|
|Okuma Amaçlı Ölçeği Genişletme|Evet|
|Dahil edilen yedekleme depolaması|1X DB boyutu|

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
