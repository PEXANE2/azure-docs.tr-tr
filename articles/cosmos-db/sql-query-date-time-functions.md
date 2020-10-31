---
title: Azure Cosmos DB sorgu dilinde tarih ve saat işlevleri
description: Tarih ve saat SQL sistem işlevlerini tarih saat ve zaman damgası işlemleri gerçekleştirmek için Azure Cosmos DB hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/18/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a7f19a7c3cc7337f0e1c53cf387abb35b96d1e38
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93100329"
---
# <a name="date-and-time-functions-azure-cosmos-db"></a>Tarih ve saat işlevleri (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Tarih ve saat işlevleri Azure Cosmos DB tarih saat ve zaman damgası işlemleri gerçekleştirmenize olanak sağlar.

## <a name="functions-to-obtain-the-date-and-time"></a>Tarih ve saati elde etmek için işlevler

Aşağıdaki skaler işlevler üç biçimde geçerli UTC Tarih ve saatini almanıza olanak sağlar: ISO 8601 biçimine uygun bir dize, değeri, UNIX dönemi 'nden bu yana geçen milisaniye sayısı veya değeri, UNIX dönemi bu yana geçen 100 nanosaniyelik Ticks sayısı olan sayısal bir zaman damgası olan sayısal bir zaman damgasıdır. :

* [GetCurrentDateTime](sql-query-getcurrentdatetime.md)
* [GetCurrentTimestamp](sql-query-getcurrenttimestamp.md)
* [GetCurrentTicks](sql-query-getcurrentticks.md)

## <a name="functions-to-work-with-datetime-values"></a>DateTime değerleriyle çalışacak işlevler

Aşağıdaki işlevler, DateTime, timestamp ve Tick değerlerini kolayca yönetmenize olanak sağlar:

* [DateTimeAdd](sql-query-datetimeadd.md)
* [Tarih TimeDiff](sql-query-datetimediff.md)
* [DateTimeFromParts](sql-query-datetimefromparts.md)
* [DateTimePart](sql-query-datetimepart.md)
* [DateTimeToTicks](sql-query-datetimetoticks.md)
* [DateTimeToTimestamp](sql-query-datetimetotimestamp.md)
* [TicksToDateTime](sql-query-tickstodatetime.md)
* [TimestampToDateTime](sql-query-timestamptodatetime.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
- [Kullanıcı tanımlı Işlevler](sql-query-udfs.md)
- [Toplamlar](sql-query-aggregates.md)
