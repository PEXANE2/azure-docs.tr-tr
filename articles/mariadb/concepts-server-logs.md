---
title: Yavaş sorgu günlükleri - MariaDB için Azure Veritabanı
description: MariaDB için Azure Veritabanı'nda bulunan günlükleri ve farklı günlük düzeylerini etkinleştirmek için kullanılabilir parametreleri açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 2c07e5eeedd2e4f42ec7b165bf161e142421df58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527903"
---
# <a name="slow-query-logs-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı'nda yavaş sorgu günlükleri
MariaDB için Azure Veritabanı'nda, yavaş sorgu günlüğü kullanıcılar tarafından kullanılabilir. Hareket günlüğüne erişim desteklenmez. Yavaş sorgu günlüğü, sorun giderme için performans sorunlarını tanımlamak için kullanılabilir.

Yavaş sorgu günlüğü hakkında daha fazla bilgi için, [yavaş sorgu günlüğü](https://mariadb.com/kb/en/library/slow-query-log-overview/)için MariaDB belgelerine bakın.

## <a name="access-slow-query-logs"></a>Yavaş sorgu günlüklerine erişin
Azure portalını ve Azure CLI'yi kullanarak MariaDB yavaş sorgu günlükleri için Azure Veritabanı'nı listeleyebilir ve indirebilirsiniz.

Azure portalında, MariaDB sunucusu için Azure Veritabanınızı seçin. **İzleme** başlığı altında Sunucu **Günlükleri** sayfasını seçin.

Azure CLI hakkında daha fazla bilgi için Azure [CLI'yi kullanarak sunucu günlüklerini yapılandırma ve erişim](howto-configure-server-logs-cli.md)bilgisine bakın.

Benzer şekilde, günlükleri Tanılama Günlükleri'ni kullanarak Azure Monitor'a taşıyabilirsiniz. Daha fazla bilgi için [aşağıya](concepts-server-logs.md#diagnostic-logs) bakın.

## <a name="log-retention"></a>Günlük tutma
Günlükler, oluşturulduktan sonra yedi güne kadar kullanılabilir. Kullanılabilir günlüklerin toplam boyutu 7 GB'ı aşarsa, alan kullanılabilir olana kadar en eski dosyalar silinir.

Günlükler her 24 saatte bir veya 7 GB'da bir döndürülür, hangisi önce gelirse.

## <a name="configure-slow-query-logging"></a>Yavaş sorgu günlüğe kaydetmeyi yapılandırma
Varsayılan olarak yavaş sorgu günlüğü devre dışı bırakılır. Etkinleştirmek için slow_query_log'ı AÇIYA ayarlayın.

Ayarlayabileceğiniz diğer parametreler şunlardır:

- **long_query_time**: Bir sorgu long_query_time 'den (saniye cinsinden) uzun sürüyorsa, bu sorgu günlüğe kaydedilir. Varsayılan değer 10 saniyedir.
- **log_slow_admin_statements**: slow_query_log yazılan ifadelerde ALTER_TABLE ve ANALYZE_TABLE gibi idari ifadeler yer alıyorsa.
- **log_queries_not_using_indexes**: dizin kullanmayan sorguların slow_query_log
- **log_throttle_queries_not_using_indexes**: Bu parametre, yavaş sorgu günlüğüne yazılabilir dizin olmayan sorguların sayısını sınırlar. Bu parametre, log_queries_not_using_indexes A.Ş. olarak ayarlandığında etkin olur.
- **log_output**: "Dosya" ise, yavaş sorgu günlüğünün hem yerel sunucu depolamasına hem de Azure Monitor Tanı Günlüklerine yazılmasını sağlar. "Yok" ise, yavaş sorgu günlüğü yalnızca Azure MonitörÜ Tanılama Günlükleri'ne yazılır. 

> [!IMPORTANT]
> Tablolarınız dizine eklenmezse, bu dizine ekili olmayan tablolara karşı çalışan tüm sorgular yavaş sorgu günlüğüne yazılacağına göre, tabloların ve `log_queries_not_using_indexes` `log_throttle_queries_not_using_indexes` parametrelerin On olarak ayarlanması MariaDB performansını etkileyebilir.<br><br>
> Uzun bir süre için yavaş sorguları günlüğe kaydetmeyi planlıyorsanız, "Yok" olarak ayarlanmanız `log_output` önerilir. "Dosya" olarak ayarlanırsa, bu günlükler yerel sunucu depolamasına yazılır ve MariaDB performansını etkileyebilir. 

Yavaş sorgu günlüğü parametrelerinin tam açıklamaları için MariaDB [yavaş sorgu günlüğü belgelerine](https://mariadb.com/kb/en/library/slow-query-log-overview/) bakın.

## <a name="diagnostic-logs"></a>Tanılama günlükleri
MariaDB için Azure Veritabanı, Azure Monitor Tanı Günlükleri ile tümleşiktir. MariaDB sunucunuzdaki yavaş sorgu günlüklerini etkinleştirdikten sonra, bunları Azure Monitor günlüklerine, Etkinlik Hub'larına veya Azure Depolama'ya yaymayı seçebilirsiniz. Tanılama günlüklerini etkinleştirme hakkında daha fazla bilgi edinmek için [tanılama günlükleri belgelerinin](../azure-monitor/platform/platform-logs-overview.md)nasıl bölümle yapılacağını görün.

Aşağıdaki tabloda her günlükte ne olduğu açıklanmaktadır. Çıktı yöntemine bağlı olarak, dahil edilen alanlar ve göründükleri sıra değişebilir.

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı kimliğiniz |
| `SourceSystem` | `Azure` |
| `TimeGenerated`[UTC] | Günlük UTC'de kaydedildiğinde zaman damgası |
| `Type` | Günlük türü. Her zaman`AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu abonelik için GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubunun adı |
| `ResourceProvider` | Kaynak sağlayıcısının adı. Her zaman`MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI |
| `Resource` | Sunucunun adı |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Sunucunun adı |
| `start_time_t`[UTC] | Sorgunun başladığı saat |
| `query_time_s` | Sorgunun yürütülmesi için gereken toplam süre |
| `lock_time_s` | Sorgunun kilitlenditoplam süresi |
| `user_host_s` | Kullanıcı adı |
| `rows_sent_s` | Gönderilen satır sayısı |
| `rows_examined_s` | İncelenen satır sayısı |
| `last_insert_id_s` | [last_insert_id](https://mariadb.com/kb/en/library/last_insert_id/) |
| `insert_id_s` | Kimlik ekle |
| `sql_text_s` | Tam sorgu |
| `server_id_s` | Sunucu Kimliği |
| `thread_id_s` | İş Parçacığı Kimliği |
| `\_ResourceId` | Kaynak URI |

## <a name="analyze-logs-in-azure-monitor-logs"></a>Azure Monitör Günlükleri'ndeki günlükleri analiz edin

Yavaş sorgu günlükleriniz, Tanılama Günlükleri aracılığıyla Azure Monitör Günlükleri'ne aktadıktan sonra, yavaş sorgularınızın daha fazla analizini gerçekleştirebilirsiniz. Aşağıda, başlamanıza yardımcı olacak bazı örnek sorgular verilmiştir. Aşağıdaki leri sunucu adınız ile güncelleştirin.

- Belirli bir sunucuda 10 saniyeden uzun sorgular

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```

- Belirli bir sunucudaki en uzun 5 sorguları listele

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | order by query_time_d desc
    | take 5
    ```

- Belirli bir sunucudaki en az, maksimum, ortalama ve standart sapma sorgu süresine göre yavaş sorguları özetle

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count(), min(query_time_d), max(query_time_d), avg(query_time_d), stdev(query_time_d), percentile(query_time_d, 95) by LogicalServerName_s
    ```

- Belirli bir sunucudaki yavaş sorgu dağılımını grafik

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count() by LogicalServerName_s, bin(TimeGenerated, 5m)
    | render timechart
    ```

- Tanılama Günlükleri etkinleştirilmiş tüm MariaDB sunucularında sorguları 10 saniyeden uzun görüntüleme

    ```Kusto
    AzureDiagnostics
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```    
    
## <a name="next-steps"></a>Sonraki Adımlar
- [Azure portalından yavaş sorgu günlükleri nasıl yapılandırılabilen](howto-configure-server-logs-portal.md)
- [Azure CLI'den yavaş sorgu günlükleri nasıl yapılandırılabilen](howto-configure-server-logs-cli.md)
