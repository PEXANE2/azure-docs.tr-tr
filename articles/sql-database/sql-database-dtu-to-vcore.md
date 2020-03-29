---
title: DTU'dan Sanal Çekirdeğe geçiş
description: DTU modelinden vCore modeline geçirin. vCore'a geçiş, standart ve premium katmanlar arasında yükseltme veya düşürmeye benzer.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 03/09/2020
ms.openlocfilehash: 693065046f92e0e9eade14c43e9942772440937d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945412"
---
# <a name="migrate-from-the-dtu-based-model-to-the-vcore-based-model"></a>DTU tabanlı modelden vCore tabanlı modele geçiş

## <a name="migrate-a-database"></a>Veritabanını geçirme

Bir veritabanını DTU tabanlı satın alma modeline geçirmek, DTU tabanlı satın alma modelindeki standart ve premium hizmet katmanları arasında yükseltme veya düşürmeye benzer.

## <a name="migrate-databases-that-use-geo-replication"></a>Coğrafi çoğaltma kullanan veritabanlarını geçirin

DTU tabanlı modelden vCore tabanlı satın alma modeline geçiş yapmak, standart ve premium hizmet katmanlarındaki veritabanları arasındaki coğrafi çoğaltma ilişkilerini yükseltmeye veya düşürmeye benzer. Geçiş sırasında, coğrafi çoğaltmayı durdurmanız gerekmese de aşağıdaki sıralama kurallarına uymanız gerekir:

- Yükseltme yaparken, önce ikincil veritabanını yükseltmeniz ve ardından birincil veritabanını yükseltmeniz gerekir.
- Düşürme yaparken, sırayı tersine çevirin: önce birincil veritabanını düşürmeniz, sonra ikincil veritabanını düşürmeniz gerekir.

İki elastik havuz arasında coğrafi çoğaltma kullanırken, bir havuzu birincil, diğerini ikincil havuz olarak belirlemenizi öneririz. Bu durumda, elastik havuzları geçirdiğinizde aynı sıralama kılavuzunu kullanmalısınız. Ancak, hem birincil hem de ikincil veritabanları içeren elastik havuzları varsa, birincil olarak daha yüksek kullanım ile havuz tedavi ve buna göre sıralama kurallarına uyun.  

Aşağıdaki tablo, belirli geçiş senaryoları için kılavuz sağlar:

|Geçerli hizmet katmanı|Hedef servis katmanı|Geçiş türü|Kullanıcı eylemleri|
|---|---|---|---|
|Standart|Genel amaçlı|Lateral|Herhangi bir sırada göç edebilir, ancak uygun vCore boyutlandırma* sağlamanız gerekir|
|Premium|İş kritik|Lateral|Herhangi bir sırada göç edebilir, ancak uygun vCore boyutlandırma* sağlamanız gerekir|
|Standart|İş kritik|Yükseltme|Önce ikincil geçiş yapmalı|
|İş kritik|Standart|Eski sürüme düşür|Önce birincil geçiş gerekir|
|Premium|Genel amaçlı|Eski sürüme düşür|Önce birincil geçiş gerekir|
|Genel amaçlı|Premium|Yükseltme|Önce ikincil geçiş yapmalı|
|İş kritik|Genel amaçlı|Eski sürüme düşür|Önce birincil geçiş gerekir|
|Genel amaçlı|İş kritik|Yükseltme|Önce ikincil geçiş yapmalı|
||||

\*Kural olarak, standart katmandaki her 100 DTÜ en az 1 vCore gerektirir ve premium katmandaki her 125 DTÜ en az 1 vCore gerektirir. Daha fazla bilgi için [vCore tabanlı satın alma modeline](https://docs.microsoft.com/azure/sql-database/sql-database-purchase-models#vcore-based-purchasing-model)bakın.

## <a name="migrate-failover-groups"></a>Başarısız grupları geçir

Birden çok veritabanı olan failover gruplarının geçişi, birincil ve ikincil veritabanlarının tek tek geçişini gerektirir. Bu işlem sırasında, aynı hususlar ve sıralama kuralları geçerlidir. Veritabanları vCore tabanlı satın alma modeline dönüştürüldükten sonra, başarısız grup aynı ilke ayarlarıyla etkin kalır.

### <a name="create-a-geo-replication-secondary-database"></a>Coğrafi çoğaltma ikincil veritabanı oluşturma

Yalnızca birincil veritabanı için kullandığınız aynı hizmet katmanını kullanarak bir coğrafi çoğaltma ikincil veritabanı (bir jeo-ikincil) oluşturabilirsiniz. Yüksek günlük oluşturma oranına sahip veritabanları için, birincil işlem boyutuyla aynı işlem boyutuna sahip jeo-ikincil veritabanı oluşturmanızı öneririz.

Tek bir birincil veritabanı için elastik havuzda bir jeo-ikincil `maxVCore` oluşturuyorsanız, havuz ayarının birincil veritabanının bilgi işlem boyutuyla eşleştiğinden emin olun. Başka bir elastik havuzdaki birincil bir birincil için bir jeo-ikincil oluşturuyorsanız, havuzların aynı `maxVCore` ayarlara sahip olması önerilir.

## <a name="use-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>DTU tabanlı bir veritabanını vCore tabanlı veritabanına dönüştürmek için veritabanı kopyasını kullanma

DTU tabanlı işlem boyutuna sahip herhangi bir veritabanını, hedef işlem boyutu kaynak veritabanının maksimum veritabanı boyutunu desteklediği sürece kısıtlamalar veya özel sıralama olmaksızın vCore tabanlı işlem boyutuna sahip bir veritabanına kopyalayabilirsiniz. Veritabanı kopyası, kopyalama işleminin başlangıç saati itibariyle verilerin anlık görüntüsünü oluşturur ve kaynak ve hedef arasındaki verileri eşitlemez.

## <a name="next-steps"></a>Sonraki adımlar

- Tek veritabanları için kullanılabilen belirli bilgi işlem boyutları ve depolama boyutu seçenekleri [için, tek veritabanları için SQL Database vCore tabanlı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md)bakın.
- Elastik havuzlar için kullanılabilen belirli bilgi işlem boyutları ve depolama boyutu seçenekleri [için, elastik havuzlar için SQL Database vCore tabanlı kaynak sınırlarına](sql-database-vcore-resource-limits-elastic-pools.md)bakın.
