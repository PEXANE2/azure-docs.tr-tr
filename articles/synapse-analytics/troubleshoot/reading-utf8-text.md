---
title: 'Sorun giderme: sunucusuz SQL havuzu kullanarak CSV veya PARQUET dosyalarından UTF-8 metnini okuma'
description: Azure SYNAPSE Analytics 'te sunucusuz SQL havuzu kullanarak CSV veya PARQUET dosyalarından UTF-8 metnini okuma
author: julieMSFT
ms.author: jrasnick
ms.topic: troubleshooting
ms.service: synapse-analytics
ms.subservice: sql
ms.date: 11/24/2020
ms.openlocfilehash: 238880cb3f3628df7591e8d08e3057ebfd885900
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96466940"
---
# <a name="troubleshoot-reading-utf-8-text-from-csv-or-parquet-files-using-serverless-sql-pool-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te sunucusuz SQL havuzu kullanarak CSV veya Parquet dosyalarından UTF-8 metnini okuma sorunlarını giderme

Bu makalede, Azure SYNAPSE Analytics 'te sunucusuz SQL havuzu kullanarak CSV veya Parquet dosyalarından UTF-8 metnini okumak için sorun giderme adımları sağlanmaktadır.

UTF-8 metni sunucusuz SQL havuzu kullanarak bir CSV veya PARQUET dosyasından Okunmuşsa, sorgu UTF8 olmayan harmanlamalara sahip VARCHAR sütunları döndürürse, ü ve ö gibi bazı özel karakterler yanlış dönüştürülür. Bu, SQL Server ve Azure SQL 'de bilinen bir sorundur. UTF8 olmayan SYNAPSE harmanlaması, müşteri sorgularının etkileneceğini sağlamak için SQL 'de varsayılandır. Standart Ingilizce karakterler ve genişletilmiş Latin karakterlerinden oluşan bir alt küme kullanan müşteriler dönüştürme hatalarını fark etmez. Yanlış dönüştürme, [sunucusuz SQL havuzunda UTF-8 metnini okumak Için her zaman UTF-8 harmanlamalarını kullanma](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/always-use-utf-8-collations-to-read-utf-8-text-in-serverless-sql/ba-p/1883633) bölümünde daha ayrıntılı olarak açıklanmıştır

## <a name="workaround"></a>Geçici çözüm

Bu sorunun geçici çözümü, CSV veya PARQUET dosyalarından UTF-8 metnini okurken her zaman UTF-8 harmanlaması kullanmaktır.

-   Çoğu durumda, veritabanı üzerinde UTF8 harmanlaması ayarlamanız yeterlidir (meta veri işlemi).
-   UTF8 verilerini okuyan dış tablolarda UTF8 harmanlaması belirtmediyseniz, etkilenen dış tabloları yeniden oluşturmanız ve VARCHAR sütunlarında UTF8 harmanlaması ayarlamanız gerekir (meta veri işlemi).


## <a name="next-steps"></a>Sonraki adımlar

* [SYNAPSE SQL ile CETAS](../sql/develop-tables-cetas.md)
* [Hızlı başlangıç: sunucusuz SQL havuzu kullanma](../quickstart-sql-on-demand.md)
