---
title: Azure Monitor günlük sorgularında yararlı operatörler | Microsoft Dokümanlar
description: Azure Monitor günlük sorgularında farklı senaryolar için kullanılacak yaygın işlevler.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/21/2018
ms.openlocfilehash: ff63b9b7027e99c70971230936ed98186c2208e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75397718"
---
# <a name="useful-operators-in-azure-monitor-log-queries"></a>Azure Monitor günlük sorgularında yararlı operatörler

Aşağıdaki tablo, Azure Monitor günlük sorgularında farklı senaryolar için kullanılacak bazı ortak işlevler sağlar.

## <a name="useful-operators"></a>Yararlı işleçler

Kategori                                |İlgili Analitik Fonksiyonu
----------------------------------------|----------------------------------------
Seçim ve Sütun takma adları            |`project`, `project-away`, `extend`
Geçici tablolar ve sabitler          |`let scalar_alias_name = …;` <br> `let table_alias_name =  …  …  … ;`| 
Karşılaştırma ve String Operatörleri         |`startswith`, `!startswith`, `has`, `!has` <br> `contains`, `!contains`, `containscs` <br> `hasprefix`, `!hasprefix`, `hassuffix`, `!hassuffix`, `in`, `!in` <br> `matches regex` <br> `==`, `=~`, `!=`, `!~`
Ortak dize işlevleri                 |`strcat()`, `replace()`, `tolower()`, `toupper()`, `substring()`, `strlen()`
Ortak matematik fonksiyonları                   |`sqrt()`, `abs()` <br> `exp()`, `exp2()`, `exp10()`, `log()`, `log2()`, `log10()`, `pow()` <br> `gamma()`, `gammaln()`
Metni ayrışdırma                            |`extract()`, `extractjson()`, `parse`, `split()`
Çıktıyı sınırlama                         |`take`, `limit`, `top`, `sample`
Tarih işlevleri                          |`now()`, `ago()` <br> `datetime()`, `datepart()`, `timespan` <br> `startofday()`, `startofweek()`, `startofmonth()`, `startofyear()` <br> `endofday()`, `endofweek()`, `endofmonth()`, `endofyear()` <br> `dayofweek()`, `dayofmonth()`, `dayofyear()` <br> `getmonth()`, `getyear()`, `weekofyear()`, `monthofyear()`
Gruplandırma ve toplama                |`summarize by` <br> `max()`, `min()`, `count()`, `dcount()`, `avg()`, `sum()` <br> `stddev()`, `countif()`, `dcountif()`, `argmax()`, `argmin()` <br> `percentiles()`, `percentile_array()`
Birleştirmeler ve Sendikalar                        |`join kind=leftouter`, `inner`, `rightouter`, `fullouter`, `leftanti` <br> `union`
Sıralama, sipariş                             |`sort`, `order` 
Dinamik nesne (JSON ve dizi)         |`parsejson()` <br> `makeset()`, `makelist()` <br> `split()`, `arraylength()` <br> `zip()`, `pack()`
Mantıksal işleçler                       |`and`, `or`, `iff(condition, value_t, value_f)` <br> `binary_and()`, `binary_or()`, `binary_not()`, `binary_xor()`
Makine öğrenimi                        |`evaluate autocluster`, `basket`, `diffpatterns`, `extractcolumns`


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Monitor'da yazma günlüğü sorguları](get-started-queries.md)hakkında bir ders ten geç.
