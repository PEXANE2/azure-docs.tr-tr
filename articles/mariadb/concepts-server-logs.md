---
title: Yavaş sorgu günlükleri-MariaDB için Azure veritabanı
description: MariaDB için Azure veritabanı 'nda kullanılabilen günlükleri ve farklı günlük düzeylerini etkinleştirmek için kullanılabilen parametreleri açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: 651094f043162cdc5f6d522c90c7567ae94a4274
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75746666"
---
# <a name="slow-query-logs-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda yavaş sorgu günlükleri
MariaDB için Azure veritabanı 'nda yavaş sorgu günlüğü kullanıcılar tarafından kullanılabilir. İşlem günlüğüne erişim desteklenmez. Yavaş sorgu günlüğü, sorun giderme için performans sorunlarını belirlemek için kullanılabilir.

Yavaş sorgu günlüğü hakkında daha fazla bilgi için, [yavaş sorgu günlüğü](https://mariadb.com/kb/en/library/slow-query-log-overview/)Için MariaDB belgelerine bakın.

## <a name="access-slow-query-logs"></a>Yavaş sorgu günlüklerine erişin
Azure portal ve Azure CLı kullanarak MariaDB yavaş sorgu günlükleri için Azure veritabanı 'nı listeleyebilir ve indirebilirsiniz.

Azure portal, MariaDB sunucusu için Azure veritabanınızı seçin. **İzleme** başlığı altında, **sunucu günlükleri** sayfasını seçin.

Azure CLı hakkında daha fazla bilgi için bkz. [Azure CLI kullanarak sunucu günlüklerini yapılandırma ve erişme](howto-configure-server-logs-cli.md).

Benzer şekilde, tanılama günlüklerini kullanarak günlükleri Azure Izleyici 'ye kanal oluşturarak aktarabilirsiniz. Daha fazla bilgi için [aşağıya](concepts-server-logs.md#diagnostic-logs) bakın.

## <a name="log-retention"></a>Günlük tutma
Günlükler, oluşumlarından yedi güne kadar kullanılabilir. Kullanılabilir günlüklerin toplam boyutu 7 GB 'yi aşarsa, alan kullanılabilir olana kadar en eski dosyalar silinir.

Günlükler her 24 saatte bir veya 7 GB döndürülür, hangisi önce gelir.

## <a name="configure-slow-query-logging"></a>Yavaş sorgu günlüğünü yapılandırma
Varsayılan olarak, yavaş sorgu günlüğü devre dışıdır. Etkinleştirmek için slow_query_log açık olarak ayarlayın.

Ayarlayabileceğiniz diğer parametreler şunlardır:

- **long_query_time**: sorgu günlüğe kaydedilen sorgu long_query_time daha uzun sürer (saniye cinsinden). Varsayılan olarak 10 saniyedir.
- **log_slow_admin_statements**: varsa, slow_query_log YAZıLAN deyimlerde ALTER_TABLE ve ANALYZE_TABLE gibi yönetim deyimlerini içerir.
- **log_queries_not_using_indexes**: dizinleri kullanmayan sorguların slow_query_log kaydedilip kaydedilmeyeceğini belirler
- **log_throttle_queries_not_using_indexes**: Bu parametre, yavaş sorgu günlüğüne yazılabilen Dizin dışı sorguların sayısını sınırlar. Bu parametre log_queries_not_using_indexes açık olarak ayarlandığında devreye girer.
- **log_output**: "dosya" ise, yavaş sorgu günlüğünün hem yerel sunucu depolamasına hem de Azure Izleyici tanılama günlüklerine yazılmasına izin verir. "None" ise, yavaş sorgu günlüğü yalnızca Azure Izleyici tanılama günlüklerine yazılır. 

Yavaş sorgu günlüğü parametrelerinin tam açıklamaları için MariaDB [yavaş sorgu günlüğü belgelerine](https://mariadb.com/kb/en/library/slow-query-log-overview/) bakın.

## <a name="diagnostic-logs"></a>Tanılama günlükleri
MariaDB için Azure veritabanı, Azure Izleyici tanılama günlükleriyle tümleşiktir. MariaDB sunucunuzda yavaş sorgu günlüklerini etkinleştirdikten sonra, bunların Azure Izleyici günlüklerine, Event Hubs veya Azure depolama 'ya yayılmasını tercih edebilirsiniz. Tanılama günlüklerinin nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için [tanılama günlükleri belgelerinin](../azure-monitor/platform/platform-logs-overview.md)nasıl yapılır bölümüne bakın.

> [!IMPORTANT]
> Sunucu günlükleri için bu tanılama özelliği yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş [fiyatlandırma katmanlarında](concepts-pricing-tiers.md)kullanılabilir.

Aşağıdaki tabloda her günlükte neler olduğu açıklanmaktadır. Çıkış yöntemine bağlı olarak, dahil edilen alanlar ve göründükleri sıralama farklılık gösterebilir.

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı KIMLIĞINIZ |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Günlük kaydedildiği zaman damgası (UTC) |
| `Type` | Günlüğün türü. Her zaman `AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu abonelik için GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubunun adı |
| `ResourceProvider` | Kaynak sağlayıcının adı. Her zaman `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI 'SI |
| `Resource` | Sunucunun adı |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Sunucunun adı |
| `start_time_t` [UTC] | Sorgunun başladığı zaman |
| `query_time_s` | Sorgunun yürütülmesi için geçen toplam süre |
| `lock_time_s` | Sorgunun kilitlenme toplam süre |
| `user_host_s` | Kullanıcı adı |
| `rows_sent_s` | Gönderilen satır sayısı |
| `rows_examined_s` | İncelenen satır sayısı |
| `last_insert_id_s` | [last_insert_id](https://mariadb.com/kb/en/library/last_insert_id/) |
| `insert_id_s` | KIMLIĞI Ekle |
| `sql_text_s` | Tam sorgu |
| `server_id_s` | Sunucu KIMLIĞI |
| `thread_id_s` | İş Parçacığı Kimliği |
| `\_ResourceId` | Kaynak URI 'SI |

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Portal sunucu günlüklerini yapılandırma ve erişme](howto-configure-server-logs-portal.md).
