---
title: Azure SQL veritabanı sanal çekirdek tabanlı kaynak limitleri-tek veritabanı | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanı 'nda tek bir veritabanı için bazı yaygın sanal çekirdek tabanlı kaynak sınırlarını açıklamaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 04/22/2019
ms.openlocfilehash: 661ac9ea3fd87268c43bf0a0eba66e30f636fc77
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566206"
---
# <a name="resource-limits-for-single-databases-using-the-vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modelini kullanan tek veritabanlarına yönelik kaynak sınırları

Bu makale, sanal çekirdek tabanlı satın alma modelini kullanarak Azure SQL veritabanı tekil veritabanları için ayrıntılı kaynak sınırları sağlar.

SQL veritabanı sunucusundaki tek veritabanlarına yönelik DTU tabanlı satın alma modeli sınırları için bkz. [SQL veritabanı sunucusundaki kaynak sınırlarına genel bakış](sql-database-resource-limits-database-server.md).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

[Azure Portal](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-sql-database-servers-and-single-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases), [Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases)veya [REST API](sql-database-single-databases-manage.md#rest-api-manage-sql-database-servers-and-single-databases)kullanarak tek bir veritabanı için hizmet katmanını, işlem boyutunu ve depolama miktarını ayarlayabilirsiniz.

> [!IMPORTANT]
> Ölçeklendirme Kılavuzu ve konuları için bkz. [tek bir veritabanını ölçeklendirme](sql-database-single-database-scale.md).

## <a name="general-purpose-service-tier-storage-sizes-and-compute-sizes"></a>Genel Amaçlı hizmet katmanı: Depolama boyutları ve işlem boyutları

> [!IMPORTANT]
> Yeni 4. nesil veritabanları artık AustraliaEast bölgesinde desteklenmez.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Genel Amaçlı hizmet katmanı: Nesil 4 işlem platformu (Bölüm 1)

|İşlem boyutu|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1\.|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|1024|1024|1024|1536|1536|1536|
|En fazla günlük boyutu (GB)|307|307|307|461|461|461|
|TempDB boyutu (GB)|32|64|96|128|160|192|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Hedef ıOPS (64 KB)|500|1000|1500|2000|2500|3000|
|Günlük hızı sınırları (MBps)|3.75|7.5|11.25|15|18,75|22.5|
|Maksimum eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1\.|1\.|1\.|1\.|1\.|1\.|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçeği okuyun|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Genel Amaçlı hizmet katmanı: Nesil 4 işlem platformu (Bölüm 2)

|İşlem boyutu|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|1536|3072|3072|3072|4096|4096|
|En fazla günlük boyutu (GB)|461|922|922|922|1229|1229|
|TempDB boyutu (GB)|224|256|288|320|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)
|Hedef ıOPS (64 KB)|3500|4000|4500|5000|7000|7000|
|Günlük hızı sınırları (MBps)|26,25|30|30|30|30|30|
|Maksimum eş zamanlı çalışan (istek)|1400|1600|1800|2000|3200|4800|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1\.|1\.|1\.|1\.|1\.|1\.|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçeği okuyun|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Genel Amaçlı hizmet katmanı: Nesil 5 işlem platformu (Bölüm 1)

|İşlem boyutu|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|6|8|10|12|14|
|Bellek (GB)|10.2|20,4|30,6|40,8|51|61,2|71,4|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|1024|1024|1536|1536|1536|3072|3072|
|En fazla günlük boyutu (GB)|307|307|307|461|461|461|461|
|TempDB boyutu (GB)|64|128|192|256|320|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Hedef ıOPS (64 KB)|1000|2000|3000|4000|5000|6000|7000|
|Günlük hızı sınırları (MBps)|3.75|7.5|11.25|15|18,75|22.5|26,25|
|Maksimum eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|1400|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1\.|1\.|1\.|1\.|1\.|1\.|1\.|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçeği okuyun|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Genel Amaçlı hizmet katmanı: Nesil 5 işlem platformu (Bölüm 2)

|İşlem boyutu|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|16|18|20|24|32|40|80|
|Bellek (GB)|81,6|91,8|102|122,4|163,2|204|408|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|3072|3072|3072|4096|4096|4096|4096|
|En fazla günlük boyutu (GB)|922|922|922|1229|1229|1229|1229|
|TempDB boyutu (GB)|384|384|384|384|384|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Hedef ıOPS (64 KB)|7000|7000|7000|7000|7000|7000|7000|
|Günlük hızı sınırları (MBps)|30|30|30|30|30|30|30|
|Maksimum eş zamanlı çalışan (istek)|1600|1800|2000|2400|3200|4000|8000|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1\.|1\.|1\.|1\.|1\.|1\.|1\.|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçeği okuyun|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

### <a name="serverless-compute-tier"></a>Sunucusuz işlem katmanı

[Sunucusuz işlem katmanı](sql-database-serverless.md) önizleme aşamasındadır ve yalnızca sanal çekirdek satın alma modelini kullanan tek veritabanları içindir.

#### <a name="generation-5-compute-platform"></a>Nesil 5 işlem platformu

|İşlem boyutu|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|
|:--- | --: |--: |--: |
|H/W oluşturma|5|5|5|
|En fazla sanal çekirdek|0,5-1|0,5-2|0,5-4|
|En fazla bellek (GB)|2.02-3|2.05-6|2.10-12|
|En az otomatik duraklatma gecikmesi (saatler)|6|6|6|
|Columnstore desteği|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|
|En fazla veri boyutu (GB)|512|1024|1024|
|En fazla günlük boyutu (GB)|12|24|48|
|TempDB boyutu (GB)|32|64|128|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|5-7 MS (yazma)<br>5-10 MS (okuma)|
|Hedef ıOPS (64 KB)|500|1000|2000|
|Günlük hızı sınırları (MBps)|2,5|5.6|10|
|Maksimum eş zamanlı çalışan (istek)|75|150|300|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|
|Çoğaltma sayısı|1\.|1\.|1\.|
|Çoklu-AZ|Yok|Yok|Yok|
|Ölçeği okuyun|Yok|Yok|Yok|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|

## <a name="business-critical-service-tier-for-provisioned-compute-tier"></a>Sağlanan işlem katmanının hizmet katmanını İş Açısından Kritik

> [!IMPORTANT]
> Yeni 4. nesil veritabanları artık AustraliaEast bölgesinde desteklenmez.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-1"></a>İş Açısından Kritik hizmet katmanı: Nesil 4 işlem platformu (Bölüm 1)

|İşlem boyutu|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1\.|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|1\.|2|3|4|5|6|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En fazla veri boyutu (GB)|650|650|650|650|650|650|
|En fazla günlük boyutu (GB)|195|195|195|195|195|195|
|TempDB boyutu (GB)|32|64|96|128|160|192|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef ıOPS (64 KB)|5000|10000|15000|20000|25000|30000|
|Günlük hızı sınırları (MBps)|8|16|24|32|40|48|
|Maksimum eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|
|Maks. eş zamanlı oturum|200|400|600|800|1000|1200|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçeği okuyun|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>İş Açısından Kritik hizmet katmanı: Nesil 4 işlem platformu (Bölüm 2)

|İşlem boyutu|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|7|8|9,5|11|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En fazla veri boyutu (GB)|650|650|650|650|1024|1024|
|En fazla günlük boyutu (GB)|195|195|195|195|307|307|
|TempDB boyutu (GB)|224|256|288|320|384|384|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef ıOPS (64 KB)|35000|40000|45000|50000|80000|120000|
|Günlük hızı sınırları (MBps)|56|64|64|64|64|64|
|Maksimum eş zamanlı çalışan (istek)|1400|1600|1800|2000|3200|4800|
|Maksimum eşzamanlı oturum açma sayısı (istek)|1400|1600|1800|2000|3200|4800|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçeği okuyun|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>İş Açısından Kritik hizmet katmanı: Nesil 5 işlem platformu (Bölüm 1)

|İşlem boyutu|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|6|8|10|12|14|
|Bellek (GB)|10.2|20,4|30,6|40,8|51|61,2|71,4|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|1,571|3,142|4,713|6,284|8,655|11,026|13,397|
|En fazla veri boyutu (GB)|1024|1024|1536|1536|1536|3072|3072|
|En fazla günlük boyutu (GB)|307|307|307|461|461|922|922|
|TempDB boyutu (GB)|64|128|192|256|320|384|384|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef ıOPS (64 KB)|8000|16000|24000|32000|40000|48000|56000|
|Günlük hızı sınırları (MBps)|12|24|36|48|60|72|84|
|Maksimum eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|1400|
|Maks. eş zamanlı oturum|200|400|600|800|1000|1200|1400|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|4|4|4|4|4|4|4|
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçeği okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>İş Açısından Kritik hizmet katmanı: Nesil 5 işlem platformu (Bölüm 2)

|İşlem boyutu|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|16|18|20|24|32|40|80|
|Bellek (GB)|81,6|91,8|102|122,4|163,2|204|408|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|15,768|18,139|20,51|25,252|37,936|52,22|131,64|
|En fazla veri boyutu (GB)|3072|3072|3072|4096|4096|4096|4096|
|En fazla günlük boyutu (GB)|922|922|922|1229|1229|1229|1229|
|TempDB boyutu (GB)|384|384|384|384|384|384|384|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef ıOPS (64 KB)|64000|72000|80000|96000|128000|160000|320000|
|Günlük hızı sınırları (MBps)|96|96|96|96|96|96|96|
|Maksimum eş zamanlı çalışan (istek)|1600|1800|2000|2400|3200|4000|8000|
|Maks. eş zamanlı oturum|1600|1800|2000|2400|3200|4000|8000|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|4|4|4|4|4|4|4|
|Çoklu-AZ|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçeği okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolaması|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|1X DB boyutu|

## <a name="hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanı

### <a name="generation-5-compute-platform"></a>Nesil 5 işlem platformu

|Performans düzeyi|HS_Gen5_2|HS_Gen5_4|HS_Gen5_8|HS_Gen5_16|HS_Gen5_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|8|16|24|32|40|80|
|Bellek (GB)|10.2|20,4|40,8|81,6|122,4|163,2|204|408|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolaması (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En fazla veri boyutu (TB)|100 |100 |100 |100 |100 |100 |100 |100 |
|En fazla günlük boyutu (TB)|1\. |1\. |1\. |1\. |1\. |1\. |1\. |1\. |
|TempDB boyutu (GB)|64|128|256|384|384|384|384|384|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|Hedef ıOPS (64 KB)| [1. nota](#note-1) |[1. nota](#note-1)|[1. nota](#note-1) |[1. nota](#note-1) |[1. nota](#note-1) |[1. nota](#note-1) |[1. nota](#note-1) | [1. nota](#note-1) |
|GÇ gecikmesi (yaklaşık)|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|
|Maksimum eş zamanlı çalışan (istek)|200|400|800|1600|2400|3200|4000|8000|
|İzin verilen en fazla oturum sayısı|30000|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|2|2|2|2|2|2|2|2|
|Çoklu-AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçeği okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolaması |7|7|7|7|7|7|7|7|
|||

### <a name="note-1"></a>1\. nota

Hiper ölçek, birden çok düzeyde önbelleğe alma özelliği olan çok katmanlı bir mimaridir. Etkin ıOPS iş yüküne bağlı olacaktır.

### <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için DTU kaynak sınırları için bkz [. DTU tabanlı satın alma modelini kullanarak tek veritabanları için kaynak limitleri](sql-database-dtu-resource-limits-single-databases.md)
- Elastik havuzlar için sanal çekirdek kaynak sınırları için bkz [. sanal çekirdek tabanlı satın alma modelini kullanarak elastik havuzlar için kaynak limitleri](sql-database-vcore-resource-limits-elastic-pools.md)
- Elastik havuzların DTU kaynak sınırları için bkz [. DTU tabanlı satın alma modelini kullanarak elastik havuzlar için kaynak limitleri](sql-database-dtu-resource-limits-elastic-pools.md)
- Yönetilen örnekler için kaynak sınırları için bkz. [yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md).
- Genel Azure limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
- Bir veritabanı sunucusundaki kaynak sınırları hakkında daha fazla bilgi için bkz. sunucu ve abonelik düzeylerindeki sınırlamalar hakkında bilgi için bkz. [SQL veritabanı sunucusundaki kaynak sınırlarına genel bakış](sql-database-resource-limits-database-server.md) .
