---
title: Tek veritabanı sanal çekirdek kaynak sınırları
description: Bu sayfa, Azure SQL veritabanı 'nda tek bir veritabanı için bazı yaygın sanal çekirdek kaynak sınırlarını açıklamaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 06/10/2020
ms.openlocfilehash: 9dfa45e463ecd53524e7516160324a80824e4d8d
ms.sourcegitcommit: eeba08c8eaa1d724635dcf3a5e931993c848c633
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84669537"
---
# <a name="resource-limits-for-single-databases-using-the-vcore-purchasing-model"></a>Sanal çekirdek satın alma modelini kullanan tek veritabanlarına yönelik kaynak sınırları
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu makalede, sanal çekirdek satın alma modelini kullanarak Azure SQL veritabanı 'nda tek veritabanlarına yönelik ayrıntılı kaynak limitleri sağlanmaktadır.

Bir sunucudaki tek veritabanları için DTU satın alma model sınırları için bkz. [bir sunucudaki kaynak sınırlarına genel bakış](resource-limits-logical-server.md).

[Azure Portal](single-database-manage.md#the-azure-portal), [Transact-SQL](single-database-manage.md#transact-sql-t-sql), [PowerShell](single-database-manage.md#powershell), [Azure CLI](single-database-manage.md#the-azure-cli)veya [REST API](single-database-manage.md#rest-api)kullanarak tek bir veritabanı için hizmet katmanını, işlem boyutunu (hizmet hedefi) ve depolama miktarını ayarlayabilirsiniz.

> [!IMPORTANT]
> Ölçeklendirme Kılavuzu ve konuları için bkz. [tek bir veritabanını ölçeklendirme](single-database-scale.md).

## <a name="general-purpose---serverless-compute---gen5"></a>Genel amaçlı-sunucusuz işlem-5. nesil

[Sunucusuz bilgi işlem katmanı](serverless-tier-overview.md) Şu anda yalnızca 5. nesil donanımında kullanılabilir.

### <a name="gen5-compute-generation-part-1"></a>5. nesil işlem oluşturma (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:--- | --: |--: |--: |--: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|En fazla sanal çekirdek|0.5-1|0.5-2|0,5-4|0.75-6|1.0-8|
|En fazla bellek (GB)|2.02-3|2.05-6|2.10-12|2.25-18|3.00-24|
|Min-en fazla otomatik duraklatma gecikmesi (dakika)|60-10080|60-10080|60-10080|60-10080|60-10080|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|
|En fazla veri boyutu (GB)|512|1024|1024|1024|1536|
|En fazla günlük boyutu (GB)|154|307|307|307|461|
|TempDB en fazla veri boyutu (GB)|32|64|128|192|256|
|Depolama türü|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|En fazla veri ıOPS *|320|640|1280|1920|2560|
|En fazla günlük hızı (MBps)|3,8|7,5|15|22,5|30|
|Maksimum eş zamanlı çalışan (istek)|75|150|300|450|600|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|
|Çoğaltma sayısı|1|1|1|1|1|
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|YOK|YOK|YOK|YOK|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>5. nesil işlem oluşturma (2. bölüm)

|İşlem boyutu (hizmet hedefi)|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:--- | --: |--: |--: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|
|En fazla sanal çekirdek|1,25-10|1.50-12|1,75-14|2,00-16|
|En fazla bellek (GB)|3,75-30|4.50-36|5.25-42|6.00-48|
|Min-en fazla otomatik duraklatma gecikmesi (dakika)|60-10080|60-10080|60-10080|60-10080|
|Columnstore desteği|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|
|En fazla veri boyutu (GB)|1536|3072|3072|3072|
|En fazla günlük boyutu (GB)|461|461|461|922|
|TempDB en fazla veri boyutu (GB)|320|384|448|512|
|Depolama türü|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|En fazla veri ıOPS *|3200|3840|4480|5120|
|En fazla günlük hızı (MBps)|30|30|30|30|
|Maksimum eş zamanlı çalışan (istek)|750|900|1050|1200|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|
|Çoğaltma sayısı|1|1|1|1|
|Çoklu-AZ|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|YOK|YOK|YOK|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

### <a name="gen5-compute-generation-part-3"></a>5. nesil işlem oluşturma (Bölüm 3)

|İşlem boyutu (hizmet hedefi)|GP_S_Gen5_18|GP_S_Gen5_20|GP_S_Gen5_24|GP_S_Gen5_32|GP_S_Gen5_40|
|:--- | --: |--: |--: |--: |--:|
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|En fazla sanal çekirdek|2.25-18|2,5-20|3-24|4-32|5-40|
|En fazla bellek (GB)|6.75-54|7,5-60|9-72|12-96|15-120|
|Min-en fazla otomatik duraklatma gecikmesi (dakika)|60-10080|60-10080|60-10080|60-10080|60-10080|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|
|En fazla veri boyutu (GB)|3072|3072|4096|4096|4096|
|En fazla günlük boyutu (GB)|922|922|1229|1229|1229|
|TempDB en fazla veri boyutu (GB)|576|640|768|1024|1280|
|Depolama türü|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|En fazla veri ıOPS *|5760|6400|7680|10240|12800|
|En fazla günlük hızı (MBps)|30|30|30|30|30|
|Maksimum eş zamanlı çalışan (istek)|1350|1500|1800|2400|3000|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|
|Çoğaltma sayısı|1|1|1|1|1|
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|YOK|YOK|YOK|YOK|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).


## <a name="hyperscale---provisioned-compute---gen4"></a>Hiper ölçek-sağlanan işlem-4. nesil

### <a name="gen4-compute-generation-part-1"></a>4. nesil işlem oluşturma (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|HS_Gen4_1|HS_Gen4_2|HS_Gen4_3|HS_Gen4_4|HS_Gen4_5|HS_Gen4_6|
|:--- | --: |--: |--: |---: | --: |--: |
|İşlem oluşturma|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|
|Sanal çekirdek|1|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|[Rbpex](service-tier-hyperscale.md#compute) Boyutla|3X belleği|3X belleği|3X belleği|3X belleği|3X belleği|3X belleği|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|
|En fazla veri boyutu (TB)|100 |100 |100 |100 |100 |100|
|En fazla günlük boyutu (TB)|1 |1 |1 |1 |1 |1 |
|TempDB en fazla veri boyutu (GB)|32|64|96|128|160|192|
|Depolama türü| [1. nota](#notes) |[1. nota](#notes)|[1. nota](#notes) |[1. nota](#notes) |[1. nota](#notes) |[1. nota](#notes) |
|En fazla veri ıOPS *|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|
|En fazla günlük hızı (MBps)|100 |100 |100 |100 |100 |100 |
|GÇ gecikme süresi (yaklaşık)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|
|Maksimum eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|
|İkincil çoğaltmalar|0-4|0-4|0-4|0-4|0-4|0-4|
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|
|Yedekleme depolama saklama|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|
|||

### <a name="gen4-compute-generation-part-2"></a>4. nesil işlem oluşturma (2. bölüm)

|İşlem boyutu (hizmet hedefi)|HS_Gen4_7|HS_Gen4_8|HS_Gen4_9|HS_Gen4_10|HS_Gen4_16|HS_Gen4_24|
|:--- | ---: |--: |--: | --: |--: |--: |
|İşlem oluşturma|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|
|Sanal çekirdek|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|159,5|
|[Rbpex](service-tier-hyperscale.md#compute) Boyutla|3X belleği|3X belleği|3X belleği|3X belleği|3X belleği|3X belleği|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|
|En fazla veri boyutu (TB)|100 |100 |100 |100 |100 |100 |
|En fazla günlük boyutu (TB)|1 |1 |1 |1 |1 |1 |
|TempDB en fazla veri boyutu (GB)|224|256|288|320|512|768|
|Depolama türü| [1. nota](#notes) |[1. nota](#notes) |[1. nota](#notes) |[1. nota](#notes) |[1. nota](#notes) |[1. nota](#notes) |
|En fazla veri ıOPS *|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|
|En fazla günlük hızı (MBps)|100 |100 |100 |100 |100 |100 |
|GÇ gecikme süresi (yaklaşık)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|
|Maksimum eş zamanlı çalışan (istek)|1400|1600|1800|2000|3200|4800|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|
|İkincil çoğaltmalar|0-4|0-4|0-4|0-4|0-4|0-4|
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|
|Yedekleme depolama saklama|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|
|||

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

## <a name="hyperscale---provisioned-compute---gen5"></a>Hiper ölçek-sağlanan işlem-5. nesil

### <a name="gen5-compute-generation-part-1"></a>5. nesil işlem oluşturma (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|HS_Gen5_2|HS_Gen5_4|HS_Gen5_6|HS_Gen_8|HS_Gen5_10|HS_Gen5_12|HS_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|Sanal çekirdek|2|4|6|8|10|12|14|
|Bellek (GB)|10,4|20,8|31,1|41,5|51,9|62,3|72,7|
|[Rbpex](service-tier-hyperscale.md#compute) Boyutla|3X belleği|3X belleği|3X belleği|3X belleği|3X belleği|3X belleği|3X belleği|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|En fazla veri boyutu (TB)|100 |100 |100 |100 |100 |100 |100|
|En fazla günlük boyutu (TB)|1 |1 |1 |1 |1 |1 |1 |
|TempDB en fazla veri boyutu (GB)|64|128|192|256|320|384|448|
|Depolama türü| [1. nota](#notes) |[1. nota](#notes)|[1. nota](#notes) |[1. nota](#notes) |[1. nota](#notes) |[1. nota](#notes) |[1. nota](#notes) |
|En fazla veri ıOPS *|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|
|En fazla günlük hızı (MBps)|100 |100 |100 |100 |100 |100 |100 |
|GÇ gecikme süresi (yaklaşık)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|
|Maksimum eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|1400|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|İkincil çoğaltmalar|0-4|0-4|0-4|0-4|0-4|0-4|0-4|
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Yedekleme depolama saklama|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|
|||

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>5. nesil işlem oluşturma (2. bölüm)

|İşlem boyutu (hizmet hedefi)|HS_Gen5_16|HS_Gen5_18|HS_Gen5_20|HS_Gen_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: |--: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|Sanal çekirdek|16|18|20|24|32|40|80|
|Bellek (GB)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|[Rbpex](service-tier-hyperscale.md#compute) Boyutla|3X belleği|3X belleği|3X belleği|3X belleği|3X belleği|3X belleği|3X belleği|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|En fazla veri boyutu (TB)|100 |100 |100 |100 |100 |100 |100 |
|En fazla günlük boyutu (TB)|1 |1 |1 |1 |1 |1 |1 |
|TempDB en fazla veri boyutu (GB)|512|576|640|768|1024|1280|2560|
|Depolama türü| [1. nota](#notes) |[1. nota](#notes)|[1. nota](#notes)|[1. nota](#notes) |[1. nota](#notes) |[1. nota](#notes) |[1. nota](#notes) |
|En fazla veri ıOPS *|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|[2. nota](#notes)|
|En fazla günlük hızı (MBps)|100 |100 |100 |100 |100 |100 |100 |
|GÇ gecikme süresi (yaklaşık)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|[3. nota](#notes)|
|Maksimum eş zamanlı çalışan (istek)|1600|1800|2000|2400|3200|4000|8000|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|İkincil çoğaltmalar|0-4|0-4|0-4|0-4|0-4|0-4|0-4|
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Yedekleme depolama saklama|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|7 gün|
|||

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

#### <a name="notes"></a>Notlar

**Note 1**: hiper ölçek, ayrı işlem ve depolama bileşenlerine sahip çok katmanlı bir mimaridir: [hiper ölçek hizmeti katmanı mimarisi](service-tier-hyperscale.md#distributed-functions-architecture)

**2**. Adım: hiper ölçekte çok katmanlı mimaride birden çok düzeyde önbelleğe alma işlemi vardır. Etkin ıOPS iş yüküne bağlı olacaktır.

**3**. Adım: en çok kullanılan veri sayfalarını önbelleğe alan işlem Çoğaltmalarından dolayı RBPEX SSD tabanlı önbellekteki veriler için gecikme 1-2 ms 'dir. Sayfa sunucularından alınan veriler için daha yüksek gecikme.

## <a name="general-purpose---provisioned-compute---gen4"></a>Genel amaçlı-sağlanan işlem-4. nesil

> [!IMPORTANT]
> Yeni 4. nesil veritabanları artık Avustralya Doğu veya Brezilya Güney bölgelerinde desteklenmez.

### <a name="gen4-compute-generation-part-1"></a>4. nesil işlem oluşturma (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|
|Sanal çekirdek|1|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|
|En fazla veri boyutu (GB)|1024|1024|1536|1536|1536|3072|
|En fazla günlük boyutu (GB)|307|307|461|461|461|922|
|TempDB en fazla veri boyutu (GB)|32|64|96|128|160|192|
|Depolama türü|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|En fazla veri ıOPS *|320|640|960|1280|1600|1920|
|En fazla günlük hızı (MBps)|3,75|7,5|11,25|15|18,75|22,5|
|Maksimum eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|
|Çoğaltma sayısı|1|1|1|1|1|1|
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|YOK|YOK|YOK|YOK|YOK|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

### <a name="gen4-compute-generation-part-2"></a>4. nesil işlem oluşturma (2. bölüm)

|İşlem boyutu (hizmet hedefi)|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|
|Sanal çekirdek|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|159,5|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|
|En fazla veri boyutu (GB)|3072|3072|3072|3072|4096|4096|
|En fazla günlük boyutu (GB)|922|922|922|922|1229|1229|
|TempDB en fazla veri boyutu (GB)|224|256|288|320|512|768|
|Depolama türü|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)
|En fazla veri ıOPS *|2240|2560|2880|3200|5120|7680|
|En fazla günlük hızı (MBps)|26,3|30|30|30|30|30|
|Maksimum eş zamanlı çalışan (istek)|1400|1600|1800|2000|3200|4800|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|
|Çoğaltma sayısı|1|1|1|1|1|1|
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|YOK|YOK|YOK|YOK|YOK|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

## <a name="general-purpose---provisioned-compute---gen5"></a>Genel amaçlı-sağlanan işlem-5. nesil

### <a name="gen5-compute-generation-part-1"></a>5. nesil işlem oluşturma (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|Sanal çekirdek|2|4|6|8|10|12|14|
|Bellek (GB)|10,4|20,8|31,1|41,5|51,9|62,3|72,7|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|En fazla veri boyutu (GB)|1024|1024|1536|1536|1536|3072|3072|
|En fazla günlük boyutu (GB)|307|307|461|461|461|922|922|
|TempDB en fazla veri boyutu (GB)|64|128|192|256|320|384|384|
|Depolama türü|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|En fazla veri ıOPS *|640|1280|1920|2560|3200|3840|4480|
|En fazla günlük hızı (MBps)|7,5|15|22,5|30|30|30|30|
|Maksimum eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|1400|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Çoğaltma sayısı|1|1|1|1|1|1|1|
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>5. nesil işlem oluşturma (2. bölüm)

|İşlem boyutu (hizmet hedefi)|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|Sanal çekirdek|16|18|20|24|32|40|80|
|Bellek (GB)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|En fazla veri boyutu (GB)|3072|3072|3072|4096|4096|4096|4096|
|En fazla günlük boyutu (GB)|922|922|922|1229|1229|1229|1229|
|TempDB en fazla veri boyutu (GB)|512|576|640|768|1024|1280|2560|
|Depolama türü|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|Uzak SSD|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|En fazla veri ıOPS *|5120|5760|6400|7680|10240|12800|12800|
|En fazla günlük hızı (MBps)|30|30|30|30|30|30|30|
|Maksimum eş zamanlı çalışan (istek)|1600|1800|2000|2400|3200|4000|8000|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Çoğaltma sayısı|1|1|1|1|1|1|1|
|Çoklu-AZ|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|YOK|YOK|YOK|YOK|YOK|YOK|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

## <a name="general-purpose---provisioned-compute---fsv2-series"></a>Genel amaçlı-sağlanan işlem-Fsv2-Series

### <a name="fsv2-series-compute-generation-preview"></a>Fsv2 serisi işlem oluşturma (Önizleme)

|İşlem boyutu (hizmet hedefi)|GP_Fsv2_72|
|:--- | --: |
|İşlem oluşturma|Fsv2 serisi|
|Sanal çekirdek|72|
|Bellek (GB)|136,2|
|Columnstore desteği|Yes|
|Bellek içi OLTP depolaması (GB)|YOK|
|En fazla veri boyutu (GB)|4096|
|En fazla günlük boyutu (GB)|1024|
|TempDB en fazla veri boyutu (GB)|333|
|Depolama türü|Uzak SSD|
|GÇ gecikme süresi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|En fazla veri ıOPS *|12.800|
|En fazla günlük hızı (MBps)|30|
|Maksimum eş zamanlı çalışan (istek)|3600|
|Maks. eş zamanlı oturum|3600|
|Maks. eş zamanlı oturum|30.000|
|Çoğaltma sayısı|1|
|Çoklu-AZ|YOK|
|Okuma Amaçlı Ölçeği Genişletme|YOK|
|Dahil edilen yedekleme depolaması|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

## <a name="business-critical---provisioned-compute---gen4"></a>İş açısından kritik-sağlanan işlem-4. nesil

> [!IMPORTANT]
> Yeni 4. nesil veritabanları artık Avustralya Doğu veya Brezilya Güney bölgelerinde desteklenmez.

### <a name="gen4-compute-generation-part-1"></a>4. nesil işlem oluşturma (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|
|Sanal çekirdek|1|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|1|2|3|4|5|6|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En fazla veri boyutu (GB)|1024|1024|1024|1024|1024|1024|
|En fazla günlük boyutu (GB)|307|307|307|307|307|307|
|TempDB en fazla veri boyutu (GB)|32|64|96|128|160|192|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|En fazla veri ıOPS *|4.000|8,000|12.000|16.000|20.000|24.000|
|En fazla günlük hızı (MBps)|8|16|24|32|40|48|
|Maksimum eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|
|Maks. eş zamanlı oturum|200|400|600|800|1000|1200|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çoklu-AZ|Yes|Yes|Yes|Yes|Yes|Yes|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

### <a name="gen4-compute-generation-part-2"></a>4. nesil işlem oluşturma (2. bölüm)

|İşlem boyutu (hizmet hedefi)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|İşlem oluşturma|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|4. nesil|
|Sanal çekirdek|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|159,5|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|7|8|9,5|11|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En fazla veri boyutu (GB)|1024|1024|1024|1024|1024|1024|
|En fazla günlük boyutu (GB)|307|307|307|307|307|307|
|TempDB en fazla veri boyutu (GB)|224|256|288|320|512|768|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Maksimum veri ıOPS 'si |28.000|32.000|36.000|40,000|64.000|76.800|
|En fazla günlük hızı (MBps)|56|64|64|64|64|64|
|Maksimum eş zamanlı çalışan (istek)|1400|1600|1800|2000|3200|4800|
|Maksimum eşzamanlı oturum açma sayısı (istek)|1400|1600|1800|2000|3200|4800|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çoklu-AZ|Yes|Yes|Yes|Yes|Yes|Yes|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

## <a name="business-critical---provisioned-compute---gen5"></a>İş açısından kritik-sağlanan işlem-5. nesil

### <a name="gen5-compute-generation-part-1"></a>5. nesil işlem oluşturma (Bölüm 1)

|İşlem boyutu (hizmet hedefi)|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|Sanal çekirdek|2|4|6|8|10|12|14|
|Bellek (GB)|10,4|20,8|31,1|41,5|51,9|62,3|72,7|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|1,57|3,14|4,71|6,28|8,65|11,02|13,39|
|En fazla veri boyutu (GB)|1024|1024|1536|1536|1536|3072|3072|
|En fazla günlük boyutu (GB)|307|307|461|461|461|922|922|
|TempDB en fazla veri boyutu (GB)|64|128|192|256|320|384|448|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|En fazla veri ıOPS *|8000|16.000|24.000|32.000|40,000|48.000|56.000|
|En fazla günlük hızı (MBps)|24|48|72|96|96|96|96|
|Maksimum eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|1400|
|Maks. eş zamanlı oturum|200|400|600|800|1000|1200|1400|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Çoğaltma sayısı|4|4|4|4|4|4|4|
|Çoklu-AZ|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

### <a name="gen5-compute-generation-part-2"></a>5. nesil işlem oluşturma (2. bölüm)

|İşlem boyutu (hizmet hedefi)|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|İşlem oluşturma|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|5. nesil|
|Sanal çekirdek|16|18|20|24|32|40|80|
|Bellek (GB)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|Columnstore desteği|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Bellek içi OLTP depolaması (GB)|15,77|18,14|20,51|25,25|37,94|52,23|131,64|
|En fazla veri boyutu (GB)|3072|3072|3072|4096|4096|4096|4096|
|En fazla günlük boyutu (GB)|922|922|922|1229|1229|1229|1229|
|TempDB en fazla veri boyutu (GB)|512|576|640|768|1024|1280|2560|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|En fazla veri ıOPS *|64.000|72.000|80,000|96.000|128.000|160.000|204.800|
|En fazla günlük hızı (MBps)|96|96|96|96|96|96|96|
|Maksimum eş zamanlı çalışan (istek)|1600|1800|2000|2400|3200|4000|8000|
|Maks. eş zamanlı oturum|1600|1800|2000|2400|3200|4000|8000|
|Maks. eş zamanlı oturum|30.000|30.000|30.000|30.000|30.000|30.000|30.000|
|Çoğaltma sayısı|4|4|4|4|4|4|4|
|Çoklu-AZ|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Okuma Amaçlı Ölçeği Genişletme|Yes|Yes|Yes|Yes|Yes|Yes|Yes|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

## <a name="business-critical---provisioned-compute---m-series"></a>İş açısından kritik-sağlanan işlem-a serisi

### <a name="m-series-compute-generation-preview"></a>A serisi işlem oluşturma (Önizleme)

|İşlem boyutu (hizmet hedefi)|BC_M_128|
|:--- | --: |
|İşlem oluşturma|M serisi|
|Sanal çekirdek|128|
|Bellek (GB)|3767,1|
|Columnstore desteği|Yes|
|Bellek içi OLTP depolaması (GB)|1768|
|En fazla veri boyutu (GB)|4096|
|En fazla günlük boyutu (GB)|2048|
|TempDB en fazla veri boyutu (GB)|4096|
|Depolama türü|Yerel SSD|
|GÇ gecikme süresi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|En fazla veri ıOPS *|160.000|
|En fazla günlük hızı (MBps)|264|
|Maksimum eş zamanlı çalışan (istek)|12.800|
|Maks. eş zamanlı oturum|12.800|
|Maks. eş zamanlı oturum|30000|
|Çoğaltma sayısı|4|
|Çoklu-AZ|Yes|
|Okuma Amaçlı Ölçeği Genişletme|Yes|
|Dahil edilen yedekleme depolaması|1X DB boyutu|

\*8 KB ile 64 KB arasında değişen GÇ boyutları için maksimum değer. Gerçek ıOPS iş yüküne bağımlıdır. Ayrıntılar için bkz. [Data IO idare](resource-limits-logical-server.md#resource-governance).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanını daraltmanız gerekebilir. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda dosya alanını yönetme](file-space-manage.md).

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için DTU kaynak sınırları için bkz [. DTU satın alma modelini kullanarak tek veritabanları için kaynak limitleri](resource-limits-dtu-single-databases.md)
- Elastik havuzların sanal çekirdek kaynak sınırları için bkz [. Vcore satın alma modelini kullanarak elastik havuzlar için kaynak sınırları](resource-limits-vcore-elastic-pools.md)
- Elastik havuzların DTU kaynak sınırları için bkz [. DTU satın alma modelini kullanarak elastik havuzlar için kaynak limitleri](resource-limits-dtu-elastic-pools.md)
- SQL yönetilen örneği için kaynak sınırları için bkz. [SQL yönetilen örnek kaynak sınırları](../managed-instance/resource-limits.md).
- Genel Azure limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md).
- Bir sunucudaki kaynak sınırları hakkında bilgi için bkz. sunucu ve abonelik düzeylerindeki sınırlamalar hakkında bilgi için bkz. [bir sunucudaki kaynak sınırlarına genel bakış](resource-limits-logical-server.md) .
