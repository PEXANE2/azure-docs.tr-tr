---
title: Denetim günlükleri-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı 'nda kullanılabilen Denetim günlüklerini ve günlük düzeylerini etkinleştirmek için kullanılabilen parametreleri açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/09/2019
ms.openlocfilehash: eae7e434ce21b5f9d9f3e6c40f94261df8baa426
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74972362"
---
# <a name="audit-logs-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda denetim günlükleri

MySQL için Azure veritabanı 'nda, denetim günlüğü kullanıcılar tarafından kullanılabilir. Denetim günlüğü, veritabanı düzeyindeki etkinlikleri izlemek için ve genellikle uyumluluk için kullanılır.

> [!IMPORTANT]
> Denetim günlüğü işlevselliği Şu anda önizleme aşamasındadır.

## <a name="configure-audit-logging"></a>Denetim günlüğünü yapılandırma

Varsayılan olarak, denetim günlüğü devre dışıdır. Etkinleştirmek için `audit_log_enabled` açık olarak ayarlayın.

Ayarlayabileceğiniz diğer parametreler şunlardır:

- `audit_log_events`: günlüğe kaydedilecek olayları denetler. Belirli denetim olayları için aşağıdaki tabloya bakın.
- `audit_log_include_users`: günlük kaydı için dahil edilecek MySQL kullanıcıları. Bu parametre için varsayılan değer boştur; bu, günlüğe kaydedilecek tüm kullanıcıları içerir. Bu, `audit_log_exclude_users`daha yüksek önceliğe sahiptir. Parametrenin uzunluk üst sınırı 512 karakterdir.
> [!Note]
> `audit_log_include_users` `audit_log_exclude_users`daha yüksek önceliğe sahiptir. Örneğin, `audit_log_include_users` = `demouser` ve `audit_log_exclude_users` = `demouser`, `audit_log_include_users` daha yüksek önceliğe sahip olduğu için Kullanıcı denetim günlüklerine dahil edilir.
- `audit_log_exclude_users`: MySQL kullanıcılarının günlüğe kaydedilmesini hariç tutulacak. Parametrenin uzunluk üst sınırı 512 karakterdir.

> [!Note]
> `sql_text`için günlük 2048 karakteri aşarsa kesilir.

| **Olay** | **Açıklama** |
|---|---|
| `CONNECTION` | -Bağlantı başlatma (başarılı veya başarısız) <br> -Oturum sırasında farklı kullanıcı/parola ile Kullanıcı yeniden kimlik doğrulaması <br> -Bağlantı sonlandırma |
| `DML_SELECT`| Sorguları Seç |
| `DML_NONSELECT` | EKLEME/SILME/GÜNCELLEŞTIRME sorguları |
| `DML` | DML = DML_SELECT + DML_NONSELECT |
| `DDL` | "VERITABANıNı bırak" gibi sorgular |
| `DCL` | "Izın ver" gibi sorgular |
| `ADMIN` | "Durumu göster" gibi sorgular |
| `GENERAL` | DML_SELECT, DML_NONSELECT, DML, DDL, DCL ve ADMIN içinde tümü |
| `TABLE_ACCESS` | -Yalnızca MySQL 5,7 için kullanılabilir <br> -SELECT veya INSERT gibi tablo okuma deyimleri... SEÇIN <br> -DELETE veya TRUNCATE TABLE gibi tablo silme deyimleri <br> -INSERT veya REPLACE gibi tablo ekleme deyimleri <br> -UPDATE gibi tablo güncelleştirme deyimleri |

## <a name="access-audit-logs"></a>Denetim günlüklerine erişme

Denetim günlükleri Azure Izleyici tanılama günlükleriyle tümleşiktir. MySQL sunucunuzda denetim günlüklerini etkinleştirdikten sonra, bunları Azure Izleyici günlüklerine, Event Hubs veya Azure Storage 'a yerleştirebilirsiniz. Azure portal tanılama günlüklerinin nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için [Denetim günlüğü portalı makalesine](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs)bakın.

## <a name="diagnostic-logs-schemas"></a>Tanılama günlükleri şemaları

Aşağıdaki bölümlerde, olay türüne göre MySQL denetim günlükleri tarafından yapılan çıktılar açıklanır. Çıkış yöntemine bağlı olarak, dahil edilen alanlar ve göründükleri sıralama farklılık gösterebilir.

### <a name="connection"></a>Bağlantı

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı KIMLIĞINIZ |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Günlük kaydedildiği zaman damgası (UTC) |
| `Type` | Günlüğün türü. Her zaman `AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu abonelik için GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubunun adı |
| `ResourceProvider` | Kaynak sağlayıcının adı. Her zaman `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI 'SI |
| `Resource` | Sunucunun adı |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Sunucunun adı |
| `event_class_s` | `connection_log` |
| `event_subclass_s` | `CONNECT`, `DISCONNECT`, `CHANGE USER` (yalnızca MySQL 5,7 için kullanılabilir) |
| `connection_id_d` | MySQL tarafından oluşturulan benzersiz bağlantı KIMLIĞI |
| `host_s` | Boş |
| `ip_s` | MySQL 'e bağlanan istemcinin IP adresi |
| `user_s` | Sorguyu yürüten kullanıcının adı |
| `db_s` | Bağlanılan veritabanının adı |
| `\_ResourceId` | Kaynak URI 'SI |

### <a name="general"></a>Genel

Aşağıdaki şema genel, DML_SELECT, DML_NONSELECT, DML, DDL, DCL ve yönetıcı olay türleri için geçerlidir.

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı KIMLIĞINIZ |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Günlük kaydedildiği zaman damgası (UTC) |
| `Type` | Günlüğün türü. Her zaman `AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu abonelik için GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubunun adı |
| `ResourceProvider` | Kaynak sağlayıcının adı. Her zaman `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI 'SI |
| `Resource` | Sunucunun adı |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Sunucunun adı |
| `event_class_s` | `general_log` |
| `event_subclass_s` | `LOG`, `ERROR`, `RESULT` (yalnızca MySQL 5,6 için kullanılabilir) |
| `event_time` | UTC zaman damgasında sorgu başlangıç saati |
| `error_code_d` | Sorgu başarısız olduysa hata kodu. `0` hata olmadığı anlamına gelir |
| `thread_id_d` | Sorguyu yürüten iş parçacığının KIMLIĞI |
| `host_s` | Boş |
| `ip_s` | MySQL 'e bağlanan istemcinin IP adresi |
| `user_s` | Sorguyu yürüten kullanıcının adı |
| `sql_text_s` | Tam sorgu metni |
| `\_ResourceId` | Kaynak URI 'SI |

### <a name="table-access"></a>Tablo erişimi

| **Özellik** | **Açıklama** |
|---|---|
| `TenantId` | Kiracı KIMLIĞINIZ |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Günlük kaydedildiği zaman damgası (UTC) |
| `Type` | Günlüğün türü. Her zaman `AzureDiagnostics` |
| `SubscriptionId` | Sunucunun ait olduğu abonelik için GUID |
| `ResourceGroup` | Sunucunun ait olduğu kaynak grubunun adı |
| `ResourceProvider` | Kaynak sağlayıcının adı. Her zaman `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Kaynak URI 'SI |
| `Resource` | Sunucunun adı |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Sunucunun adı |
| `event_class_s` | `table_access_log` |
| `event_subclass_s` | `READ`, `INSERT`, `UPDATE` veya `DELETE` |
| `connection_id_d` | MySQL tarafından oluşturulan benzersiz bağlantı KIMLIĞI |
| `db_s` | Erişilen veritabanının adı |
| `table_s` | Erişilen tablonun adı |
| `sql_text_s` | Tam sorgu metni |
| `\_ResourceId` | Kaynak URI 'SI |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal denetim günlüklerini yapılandırma](howto-configure-audit-logs-portal.md)