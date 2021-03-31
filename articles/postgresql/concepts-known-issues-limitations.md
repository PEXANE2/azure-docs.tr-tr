---
title: Bilinen sorunlar ve sınırlamalar-PostgreSQL için Azure veritabanı-tek sunucu ve esnek sunucu (Önizleme)
description: Müşterilerin farkında olması gereken bilinen sorunları listeler.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/05/2020
ms.openlocfilehash: 3c26368e6281ad74d617891ba15c980117b25a6e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100106425"
---
# <a name="azure-database-for-postgresql---known-issues-and-limitations"></a>PostgreSQL için Azure veritabanı-bilinen sorunlar ve sınırlamalar

Bu sayfa, PostgreSQL için Azure veritabanı 'nda uygulamanızı etkileyebilecek bilinen sorunların bir listesini sağlar. Ayrıca, soruna geçici çözüm için tüm hafifletme ve önerileri de listeler.

## <a name="intelligent-performance---query-store"></a>Akıllı performans-sorgu deposu

PostgreSQL için Azure veritabanı-tek sunucu.

| Uygunsa | Nedeni | Düzeltme|
| ----- | ------ | ---- | 
| PostgreSQL 9,6, 10, 11 | Sunucu parametresini açmak, `pg_qs.replace_parameter_placeholders` nadir bir senaryoda sunucu kapatmaya yol açabilir. | Azure portalı, sunucu parametreleri bölümünde parametre `pg_qs.replace_parameter_placeholders` değerini açın `OFF` ve kaydedin.   | 

## <a name="server-parameters"></a>Sunucu Parametreleri

PostgreSQL için Azure veritabanı-tek sunucu ve esnek sunucu

| Uygunsa | Nedeni | Düzeltme| 
| ----- | ------ | ---- | 
| PostgreSQL 9,6, 10, 11 | Sunucu parametresini `max_locks_per_transaction` Beklenenden daha yüksek bir değere değiştirmek sunucunun yeniden başlatma [](https://www.postgresql.org/docs/11/kernel-resources.html) işleminden sonra dönememesine neden olabilir. | Varsayılan değere (32 veya 64) bırakın veya PostgreSQL [belgeleri](https://www.postgresql.org/docs/11/kernel-resources.html)başına makul bir değere değiştirin. <br> <br> Hizmet tarafında, bu, SKU 'ya bağlı olarak yüksek değeri sınırlamak için üzerinde çalışıyor.  | 

## <a name="next-steps"></a>Sonraki adımlar
- Bkz. sorgu deposu [en iyi uygulamaları](./concepts-query-store-best-practices.md)
