---
title: Yavaş sorgu günlükleri-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı 'nda kullanılabilen yavaş sorgu günlüklerini ve farklı günlük düzeylerini etkinleştirmek için kullanılabilen parametreleri açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: 9a3a58cab2d9673a4660967e3a11d7f88900e718
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76844964"
---
# <a name="slow-query-logs-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda yavaş sorgu günlükleri
MySQL için Azure veritabanı 'nda, yavaş sorgu günlüğü kullanıcılar tarafından kullanılabilir. İşlem günlüğüne erişim desteklenmez. Yavaş sorgu günlüğü, sorun giderme için performans sorunlarını belirlemek için kullanılabilir.

MySQL yavaş sorgu günlüğü hakkında daha fazla bilgi için MySQL Reference el ile [yavaş sorgu günlüğü bölümüne](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)bakın.

## <a name="access-slow-query-logs"></a>Yavaş sorgu günlüklerine erişin
MySQL için Azure veritabanı 'nı, Azure portal ve Azure CLı kullanarak yavaş sorgu günlüklerini listeleyebilir ve indirebilirsiniz.

Azure portalında MySQL için Azure veritabanı sunucunuza seçin. **İzleme** başlığı altında, **sunucu günlükleri** sayfasını seçin.

Azure CLı hakkında daha fazla bilgi için bkz. [Azure CLI kullanarak yavaş sorgu günlüklerini yapılandırma ve erişme](howto-configure-server-logs-in-cli.md).

Benzer şekilde, tanılama günlüklerini kullanarak günlükleri Azure Izleyici 'ye kanal oluşturarak aktarabilirsiniz. Daha fazla bilgi için [aşağıya](concepts-server-logs.md#diagnostic-logs) bakın.

## <a name="log-retention"></a>Günlük tutma
Günlükler, oluşumlarından yedi güne kadar kullanılabilir. Kullanılabilir günlüklerin toplam boyutu 7 GB 'yi aşarsa, alan kullanılabilir olana kadar en eski dosyalar silinir. 

Günlükler her 24 saatte bir veya 7 GB döndürülür, hangisi önce gelir.

## <a name="configure-slow-query-logging"></a>Yavaş sorgu günlüğünü yapılandırma 
Varsayılan olarak, yavaş sorgu günlüğü devre dışıdır. Etkinleştirmek için slow_query_log açık olarak ayarlayın.

Ayarlayabileceğiniz diğer parametreler şunlardır:

- **long_query_time**: sorgu günlüğe kaydedilen sorgu long_query_time daha uzun sürer (saniye cinsinden). Varsayılan değer 10 saniyedir.
- **log_slow_admin_statements**: varsa, slow_query_log YAZıLAN deyimlerde ALTER_TABLE ve ANALYZE_TABLE gibi yönetim deyimlerini içerir.
- **log_queries_not_using_indexes**: dizinleri kullanmayan sorguların slow_query_log kaydedilip kaydedilmeyeceğini belirler
- **log_throttle_queries_not_using_indexes**: Bu parametre, yavaş sorgu günlüğüne yazılabilen Dizin dışı sorguların sayısını sınırlar. Bu parametre log_queries_not_using_indexes açık olarak ayarlandığında devreye girer.
- **log_output**: "dosya" ise, yavaş sorgu günlüğünün hem yerel sunucu depolamasına hem de Azure Izleyici tanılama günlüklerine yazılmasına izin verir. "None" ise, yavaş sorgu günlüğü yalnızca Azure Izleyici tanılama günlüklerine yazılır. 

> [!IMPORTANT]
> Tablolarınızın dizini oluşturulmamış ise, bu dizinlenmemiş tablolarda çalışan tüm sorgular yavaş sorgu günlüğüne yazılacak olduğundan `log_queries_not_using_indexes` ve `log_throttle_queries_not_using_indexes` parametrelerinin ON olarak ayarlanması MySQL performansını etkileyebilir.<br><br>
> Uzun bir süre için yavaş sorguları günlüğe kaydetmeyi planlıyorsanız, `log_output` "none" olarak ayarlamanız önerilir. "Dosya" olarak ayarlanırsa, bu günlükler yerel sunucu depolamasına yazılır ve MySQL performansını etkileyebilir. 

Yavaş sorgu günlüğü parametrelerinin tam açıklamaları için MySQL 'in [yavaş sorgu günlüğü belgelerini](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) inceleyin.

## <a name="diagnostic-logs"></a>Tanılama günlükleri
MySQL için Azure veritabanı, Azure Izleyici tanılama günlükleriyle tümleşiktir. MySQL sunucunuzda yavaş sorgu günlüklerini etkinleştirdikten sonra Azure Izleyici günlüklerine, Event Hubs veya Azure depolama 'ya yayılmasını seçebilirsiniz. Tanılama günlüklerinin nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için [tanılama günlükleri belgelerinin](../azure-monitor/platform/platform-logs-overview.md)nasıl yapılır bölümüne bakın.

Aşağıdaki tabloda her günlükte neler olduğu açıklanmaktadır. Çıkış yöntemine bağlı olarak, dahil edilen alanlar ve göründükleri sıralama farklılık gösterebilir.

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı KIMLIĞINIZ |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Günlük kaydedildiği zaman damgası (UTC) |
| `Type` | Günlüğün türü. Her zaman `AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu abonelik için GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubunun adı |
| `ResourceProvider` | Kaynak sağlayıcının adı. Her zaman `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI 'SI |
| `Resource` | Sunucunun adı |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Sunucunun adı |
| `start_time_t` [UTC] | Sorgunun başladığı zaman |
| `query_time_s` | Sorgunun yürütülmesi için geçen saniye cinsinden toplam süre |
| `lock_time_s` | Sorgunun kilitlenme saniye cinsinden toplam süre |
| `user_host_s` | Kullanıcı adı |
| `rows_sent_s` | Gönderilen satır sayısı |
| `rows_examined_s` | İncelenen satır sayısı |
| `last_insert_id_s` | [last_insert_id](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_last-insert-id) |
| `insert_id_s` | KIMLIĞI Ekle |
| `sql_text_s` | Tam sorgu |
| `server_id_s` | Sunucunun KIMLIĞI |
| `thread_id_s` | İş parçacığı KIMLIĞI |
| `\_ResourceId` | Kaynak URI 'SI |

> [!Note]
> `sql_text`için günlük 2048 karakteri aşarsa kesilir.

## <a name="analyze-logs-in-azure-monitor-logs"></a>Azure Izleyici günlüklerinde günlükleri analiz etme

Yavaş sorgu günlüklerinizin tanılama günlükleri aracılığıyla Azure Izleyici günlüklerine bir kez alındıktan sonra, yavaş sorgularınızı daha fazla analiz gerçekleştirebilirsiniz. Aşağıda, başlamanıza yardımcı olacak bazı örnek sorgular verilmiştir. Aşağıdaki öğesini sunucu adınızla güncelleştirdiğinizden emin olun.

- Belirli bir sunucuda 10 saniyeden uzun sorgu

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```

- Belirli bir sunucuda ilk 5 en uzun sorguyu listeleme

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | order by query_time_d desc
    | take 5
    ```

- Belirli bir sunucuda minimum, maksimum, ortalama ve standart sapma sorgu zamanına göre yavaş sorguları özetleme

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count(), min(query_time_d), max(query_time_d), avg(query_time_d), stdev(query_time_d), percentile(query_time_d, 95) by LogicalServerName_s
    ```

- Belirli bir sunucuda yavaş sorgu dağıtımını grafiktir

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count() by LogicalServerName_s, bin(TimeGenerated, 5m)
    | render timechart
    ```

- Tanılama günlükleri etkin olan tüm MySQL sunucularında 10 saniyeden uzun sorguları görüntüle

    ```Kusto
    AzureDiagnostics
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```    
    
## <a name="next-steps"></a>Sonraki Adımlar
- [Azure portal yavaş sorgu günlüklerini yapılandırma](howto-configure-server-logs-in-portal.md)
- [Azure CLI 'dan yavaş sorgu günlüklerini yapılandırma](howto-configure-server-logs-in-cli.md).
