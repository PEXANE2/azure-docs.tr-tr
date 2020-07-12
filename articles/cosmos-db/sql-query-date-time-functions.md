---
title: Azure Cosmos DB sorgu dilinde tarih ve saat işlevleri
description: Tarih ve saat SQL sistem işlevlerini tarih saat ve zaman damgası işlemleri gerçekleştirmek için Azure Cosmos DB hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: e3666f58b12855c19dd9b8ecf5519ab772c49743
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86246946"
---
# <a name="date-and-time-functions-azure-cosmos-db"></a>Tarih ve saat işlevleri (Azure Cosmos DB)

Tarih ve saat işlevleri Azure Cosmos DB tarih saat ve zaman damgası işlemleri gerçekleştirmenize olanak sağlar.

## <a name="functions-to-obtain-the-date-and-time"></a>Tarih ve saati elde etmek için işlevler

Aşağıdaki skaler işlevler, Şu anki UTC Tarih ve saatini iki şekilde almanızı sağlar: ISO 8601 biçimine uygun bir dize veya değeri, değeri UNIX dönemi olan bir sayısal zaman damgasına (milisaniye olarak) sahip olur:

* [GetCurrentDateTime](sql-query-getcurrentdatetime.md)
* [GetCurrentTimestamp](sql-query-getcurrenttimestamp.md)

## <a name="functions-to-work-with-datetime-values"></a>DateTime değerleriyle çalışacak işlevler

Aşağıdaki işlevler, DateTime değerlerini kolayca yönetmenize olanak sağlar:

* [DateTimeAdd](sql-query-datetimeadd.md)
* [Tarih TimeDiff](sql-query-datetimediff.md)
* [DateTimeFromParts](sql-query-datetimefromparts.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
- [Kullanıcı tanımlı Işlevler](sql-query-udfs.md)
- [Toplamlar](sql-query-aggregates.md)
