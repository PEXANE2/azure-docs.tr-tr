---
title: SQL'den Azure Monitor'a günlük sorgu hile sayfası | Microsoft Dokümanlar
description: Azure Monitor'da günlük sorguları yazarken SQL'e aşina olan kullanıcılar için yardım.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/21/2018
ms.openlocfilehash: c76ab145fd2fdd077075b345ecac9c6a473f2369
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75365198"
---
# <a name="sql-to-azure-monitor-log-query-cheat-sheet"></a>SQL 'den Azure Monitor günlük sorgu hile sayfasına 

Aşağıdaki tablo, SQL'e aşina olan kullanıcıların Azure Monitor'a günlük sorguları yazmak için Kusto sorgu dilini öğrenmelerine yardımcı olur. Azure Monitor günlük sorgusunda sık karşılaşılan senaryoları ve eşdeğeri çözmek için T-SQL komutuna göz atın.

## <a name="sql-to-azure-monitor"></a>SQL'den Azure Monitöre

Açıklama                             |SQL Sorgusu                                                                                          |Azure Monitör günlük sorgusu
----------------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------
Tablodaki tüm verileri seçme            |`SELECT * FROM dependencies`                                                                       |<code>dependencies</code>
Tablodan belirli sütunları seçme    |`SELECT name, resultCode FROM dependencies`                                                        |<code>dependencies <br>&#124; project name, resultCode</code>
Tablodan 100 kayıt seçme         |`SELECT TOP 100 * FROM dependencies`                                                               |<code>dependencies <br>&#124; take 100</code>
Null değerlendirme                         |`SELECT * FROM dependencies WHERE resultCode IS NOT NULL`                                          |<code>dependencies <br>&#124; where isnotnull(resultCode)</code>
Dize karşılaştırması: eşitlik             |`SELECT * FROM dependencies WHERE name = "abcde"`                                                  |<code>dependencies <br>&#124; where name == "abcde"</code>
String karşılaştırması: substring            |`SELECT * FROM dependencies WHERE name like "%bcd%"`                                                   |<code>dependencies <br>&#124; where name contains "bcd"</code>
Dize karşılaştırması: joker karakter             |`SELECT * FROM dependencies WHERE name like "abc%"`                                                |<code>dependencies <br>&#124; where name startswith "abc"</code>
Tarih karşılaştırması: son 1 gün             |`SELECT * FROM dependencies WHERE timestamp > getdate()-1`                                         |<code>dependencies <br>&#124; where timestamp > ago(1d)</code>
Tarih karşılaştırması: tarih aralığı             |`SELECT * FROM dependencies WHERE timestamp BETWEEN '2016-10-01' AND '2016-11-01'`                 |<code>dependencies <br>&#124; where timestamp between (datetime(2016-10-01) .. datetime(2016-10-01))</code>
Boolean karşılaştırması                      |`SELECT * FROM dependencies WHERE !(success)`                                                      |<code>dependencies <br>&#124; where success == "False" </code>
Sırala                                    |`SELECT name, timestamp FROM dependencies ORDER BY timestamp asc`                                  |<code>dependencies <br>&#124; order by timestamp asc </code>
Distinct                                |`SELECT DISTINCT name, type  FROM dependencies`                                                    |<code>dependencies <br>&#124; summarize by name, type </code>
Gruplandırma, Toplama                   |`SELECT name, AVG(duration) FROM dependencies GROUP BY name`                                       |<code>dependencies <br>&#124; summarize avg(duration) by name </code>
Sütun takma adları, Genişlet                  |`SELECT operation_Name as Name, AVG(duration) as AvgD FROM dependencies GROUP BY name`             |<code>dependencies <br>&#124; summarize AvgD=avg(duration) by operation_Name <br>&#124; project Name=operation_Name, AvgD</code>
Ölçüye göre en iyi n kayıtları                |`SELECT TOP 100 name, COUNT(*) as Count FROM dependencies GROUP BY name ORDER BY Count asc`        |<code>dependencies <br>&#124; summarize Count=count() by name <br>&#124; top 100 by Count asc</code>
Birleşim                                   |`SELECT * FROM dependencies UNION SELECT * FROM exceptions`                                        |<code>union dependencies, exceptions</code>
Birlik: koşullar ile                  |`SELECT * FROM dependencies WHERE value > 4 UNION SELECT * FROM exceptions WHERE value < 5`                |<code>dependencies <br>&#124; where value > 4 <br>&#124; union (exceptions <br>&#124; where value < 5)</code>
Birleştir                                    |`SELECT * FROM dependencies JOIN exceptions ON dependencies.operation_Id = exceptions.operation_Id`|<code>dependencies <br>&#124; join (exceptions) on operation_Id == operation_Id</code>


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Monitor'da günlük sorguları yazma derslerine](get-started-queries.md)katılın.
