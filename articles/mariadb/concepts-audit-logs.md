---
title: Denetim günlükleri - MariaDB için Azure Veritabanı
description: MariaDB için Azure Veritabanı'nda bulunan denetim günlüklerini ve günlük düzeylerini etkinleştirmek için kullanılabilir parametreleri açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/19/2020
ms.openlocfilehash: e8d5abd81feb86ba48fc442ee95615cb52230a24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063823"
---
# <a name="audit-logs-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanında Denetim Günlükleri

MariaDB için Azure Veritabanı'nda denetim günlüğü kullanıcılar tarafından kullanılabilir. Denetim günlüğü veritabanı düzeyindeki etkinliği izlemek için kullanılabilir ve genellikle uyumluluk için kullanılır.

> [!IMPORTANT]
> Denetim günlüğü işlevi şu anda önizlemede.

## <a name="configure-audit-logging"></a>Denetim günlüğe kaydetme

Varsayılan olarak denetim günlüğü devre dışı bırakılır. Etkinleştirmek için `audit_log_enabled` A.A. olarak ayarlayın.

Ayarlayabileceğiniz diğer parametreler şunlardır:

- `audit_log_events`: günlüğe kaydedilecek olayları denetler. Belirli denetim olayları için aşağıdaki tabloya bakın.
- `audit_log_include_users`: MariaDB kullanıcıları günlük için dahil edilecek. Bu parametrenin varsayılan değeri boştur ve bu değer günlüğe kaydetmeye yönelik tüm kullanıcıları içerir. Bu daha `audit_log_exclude_users`yüksek önceliğe sahiptir. Parametrenin maksimum uzunluğu 512 karakterdir.
> [!Note]
> `audit_log_include_users`üzerinde `audit_log_exclude_users`daha yüksek önceliğe sahiptir. Örneğin, daha `audit_log_include_users`  =  `demouser` `audit_log_exclude_users`  =  `demouser`yüksek önceliğe sahip olduğundan, `audit_log_include_users` kullanıcı denetim günlüklerine dahil edilecekse.
- `audit_log_exclude_users`: MariaDB kullanıcıları nın günlük dışı tutulamaz. En fazla dört kullanıcıya izin verir. Parametrenin maksimum uzunluğu 256 karakterdir.

| **Olay** | **Açıklama** |
|---|---|
| `CONNECTION` | - Bağlantı başlatma (başarılı veya başarısız) <br> - Oturum sırasında farklı kullanıcı/parola ile kullanıcı yeniden kimlik doğrulaması <br> - Bağlantı sonlandırma |
| `DML_SELECT`| SORGULARI SEÇ |
| `DML_NONSELECT` | INSERT/DELETE/UPDATE sorguları |
| `DML` | DML = DML_SELECT + DML_NONSELECT |
| `DDL` | "DROP DATABASE" gibi sorgular |
| `DCL` | "GRANT PERMISSION" gibi sorgular |
| `ADMIN` | "DURUM GÖSTER" gibi sorgular |
| `GENERAL` | Tüm DML_SELECT, DML_NONSELECT, DML, DDL, DCL ve ADMIN |

## <a name="access-audit-logs"></a>Denetim günlüklerine erişme

Denetim günlükleri Azure Monitör Tanı Günlükleri ile tümleştirilir. MariaDB sunucunuzdaki denetim günlüklerini etkinleştirdikten sonra bunları Azure Monitor günlüklerine, Etkinlik Hub'larına veya Azure Depolama alanına yatabilirsiniz. Azure portalında tanılama günlüklerini etkinleştirme hakkında daha fazla bilgi edinmek için [denetim günlüğü portalı makalesine](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs)bakın.

## <a name="diagnostic-logs-schemas"></a>Tanı günlükleri şemalar

Aşağıdaki bölümlerde, olay türüne göre MariaDB denetim günlükleri tarafından çıktı nın ne olduğu açıklayınız. Çıktı yöntemine bağlı olarak, dahil edilen alanlar ve göründükleri sıra değişebilir.

### <a name="connection"></a>Bağlantı

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı kimliğiniz |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Günlük UTC'de kaydedildiğinde zaman damgası |
| `Type` | Günlük türü. Her zaman`AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu abonelik için GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubunun adı |
| `ResourceProvider` | Kaynak sağlayıcısının adı. Her zaman`MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI |
| `Resource` | Sunucunun adı |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class_s` | `connection_log` |
| `event_subclass_s` | `CONNECT`, `DISCONNECT` |
| `connection_id_d` | MariaDB tarafından oluşturulan benzersiz bağlantı kimliği |
| `host_s` | Boş |
| `ip_s` | MariaDB'ye bağlanan istemcinin IP adresi |
| `user_s` | Sorguyu yürüten kullanıcının adı |
| `db_s` | Bağlı veritabanının adı |
| `\_ResourceId` | Kaynak URI |

### <a name="general"></a>Genel

Aşağıdaki şema GENEL, DML_SELECT, DML_NONSELECT, DML, DDL, DCL ve ADMIN olay türleri için geçerlidir.

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı kimliğiniz |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Günlük UTC'de kaydedildiğinde zaman damgası |
| `Type` | Günlük türü. Her zaman`AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu abonelik için GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubunun adı |
| `ResourceProvider` | Kaynak sağlayıcısının adı. Her zaman`MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI |
| `Resource` | Sunucunun adı |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Sunucunun adı |
| `event_class_s` | `general_log` |
| `event_subclass_s` | `LOG`, `ERROR`, `RESULT` |
| `event_time` | UNIX zaman damgasında sorgu başlangıç saniyesi |
| `error_code_d` | Sorgu başarısız olduysa hata kodu. `0`hata yok demektir |
| `thread_id_d` | Sorguyu çalıştıran iş parçacığının kimliği |
| `host_s` | Boş |
| `ip_s` | MariaDB'ye bağlanan istemcinin IP adresi |
| `user_s` | Sorguyu yürüten kullanıcının adı |
| `sql_text_s` | Tam sorgu metni |
| `\_ResourceId` | Kaynak URI |

## <a name="analyze-logs-in-azure-monitor-logs"></a>Azure Monitör Günlükleri'ndeki günlükleri analiz edin

Denetim kayıtlarınız, Tanılama Günlükleri aracılığıyla Azure Monitör Günlükleri'ne iletildikten sonra, denetlenen etkinliklerinizin daha fazla analizini gerçekleştirebilirsiniz. Aşağıda, başlamanıza yardımcı olacak bazı örnek sorgular verilmiştir. Aşağıdaki leri sunucu adınız ile güncelleştirin.

- Belirli bir sunucudaki GENEL olayları listele

    ```kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlAuditLogs' and event_class_s == "general_log"
    | project TimeGenerated, LogicalServerName_s, event_class_s, event_subclass_s, event_time_t, user_s , ip_s , sql_text_s 
    | order by TimeGenerated asc nulls last 
    ```

- Belirli bir sunucudaki BAĞLANTı olaylarını listele

    ```kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlAuditLogs' and event_class_s == "connection_log"
    | project TimeGenerated, LogicalServerName_s, event_class_s, event_subclass_s, event_time_t, user_s , ip_s , sql_text_s 
    | order by TimeGenerated asc nulls last
    ```

- Belirli bir sunucuda denetlenen olayları özetle

    ```kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlAuditLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, event_subclass_s, event_time_t, user_s , ip_s , sql_text_s 
    | summarize count() by event_class_s, event_subclass_s, user_s, ip_s
    ```

- Denetim olay türü dağılımını belirli bir sunucuda grafik

    ```kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlAuditLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, event_subclass_s, event_time_t, user_s , ip_s , sql_text_s 
    | summarize count() by LogicalServerName_s, bin(TimeGenerated, 5m)
    | render timechart 
    ```

- Denetim günlükleri için etkin Tanılama Günlükleri ile tüm MariaDB sunucularında denetlenen olayları listele

    ```kusto
    AzureDiagnostics
    | where Category == 'MySqlAuditLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, event_subclass_s, event_time_t, user_s , ip_s , sql_text_s 
    | order by TimeGenerated asc nulls last
    ``` 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalında denetim günlükleri nasıl yapılandırılabilen](howto-configure-audit-logs-portal.md)