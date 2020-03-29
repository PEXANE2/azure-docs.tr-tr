---
title: Denetim Günlüğü Biçimi
description: SQL Veritabanı denetim günlüklerinin nasıl yapılandırıldığıhakkında bir şey yok.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 01/03/2019
ms.openlocfilehash: 13746b86eed75055ceb5203afafb2d27a78ce1d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76722093"
---
# <a name="sql-database-audit-log-format"></a>SQL Veritabanı Denetim Günlüğü Biçimi

[Azure SQL Veritabanı denetimi](sql-database-auditing.md) veritabanı olaylarını izler ve bunları Azure depolama hesabınızdaki bir denetim günlüğüne yazar veya akış aşağı işleme ve analizi için Event Hub veya Log Analytics'e gönderir.

## <a name="naming-conventions"></a>Adlandırma Kuralları

### <a name="blob-audit"></a>Blob Denetimi

Blob depolama alanında depolanan denetim günlükleri Azure `sqldbauditlogs` Depolama hesabında adı geçen bir kapsayıcıda depolanır. Kapsayıcı içindeki dizin hiyerarşisi formdur. `<ServerName>/<DatabaseName>/<AuditName>/<Date>/` Blob dosya adı `<CreationTime>_<FileNumberInSession>.xel`biçimi, `CreationTime` UTC `hh_mm_ss_ms` biçiminde nerede `FileNumberInSession` ve birden çok Blob dosyaları arasında oturum günlükleri yayılan bir çalışan dizin.

Örneğin, aşağıdaki `Database1` veritabanı `Server1` için olası bir geçerli yoltur:

    Server1/Database1/SqlDbAuditing_ServerAudit_NoRetention/2019-02-03/12_23_30_794_0.xel

[Salt okunur Yinelemeler](sql-database-read-scale-out.md) Denetim günlükleri aynı kapsayıcıda depolanır. Kapsayıcı içindeki dizin hiyerarşisi formdur. `<ServerName>/<DatabaseName>/<AuditName>/<Date>/RO/` Blob dosya adı aynı biçimi paylaşır. Salt Okunur Yinelemelerin Denetim Günlükleri aynı kapsayıcıda depolanır.


### <a name="event-hub"></a>Olay Hub'ı

Denetim olayları, denetim yapılandırması sırasında tanımlanan ad alanı ve etkinlik merkezine yazılır ve [Apache Avro](https://avro.apache.org/) olaylarının gövdesinde yakalanır ve UTF-8 kodlaması ile JSON biçimlendirmesi kullanılarak depolanır. Denetim günlüklerini okumak için, [Avro Araçları'nı](../event-hubs/event-hubs-capture-overview.md#use-avro-tools) veya bu biçimi işleyen benzer araçları kullanabilirsiniz.

### <a name="log-analytics"></a>Log Analytics

Denetim olayları, denetim yapılandırması sırasında tanımlanan Log Analytics `AzureDiagnostics` çalışma alanına, kategorideki `SQLSecurityAuditEvents`tabloya yazılır. Log Analytics arama dili ve komutları hakkında daha fazla yararlı bilgi için Log [Analytics arama referansına](../log-analytics/log-analytics-log-search.md)bakın.

## <a name="audit-log-fields"></a><a id="subheading-1"></a>Denetim Günlüğü Alanları

| İsim (Blob) | Ad (Olay Hub'ları/Günlük Analizi) | Açıklama | Blob Tipi | Olay Hub'ları/Günlük Analizi Türü |
|-------------|---------------------------------|-------------|-----------|-------------------------------|
| action_id | action_id_s | Eylemin kimliği | varchar(4) | string |
| action_name | action_name_s | Eylemin adı | Yok | string |
| additional_information | additional_information_s | Olay hakkında herhangi bir ek bilgi, XML olarak depolanır | nvarchar(4000) | string |
| affected_rows | affected_rows_d | Sorgudan etkilenen satır sayısı | bigint | int |
| Uygulama_adı | application_name_s| İstemci başvurusunun adı | nvarchar(128) | string |
| audit_schema_version | audit_schema_version_d | Her zaman 1 | int | int |
| class_type | class_type_s | Denetimin gerçekleştiği denetlenebilir varlık türü | varchar(2) | string |
| class_type_desc | class_type_description_s | Denetimin gerçekleştiği denetlenebilir varlığın tanımı | Yok | string |
| client_ip | client_ip_s | İstemci uygulamasının kaynak IP'si | nvarchar(128) | string |
| connection_id | Yok | Sunucudaki bağlantının kimliği | GUID | Yok |
| data_sensitivity_information | data_sensitivity_information_s | Veritabanındaki sınıflandırılmış sütunları temel alarak denetlenen sorgu tarafından döndürülen bilgi türleri ve duyarlılık etiketleri. [Azure SQL Veritabanı verilerini keşfetme ve sınıflandırma](sql-database-data-discovery-and-classification.md) hakkında daha fazla bilgi edinin | nvarchar(4000) | string |
| Veritabanı_adı | database_name_s | Eylemin oluştuğu veritabanı bağlamı | Sysname | string |
| database_principal_id | database_principal_id_d | Eylemin gerçekleştirildiği veritabanı kullanıcı bağlamının kimliği | int | int |
| database_principal_name | database_principal_name_s | Eylemin gerçekleştirildiği veritabanı kullanıcı bağlamının adı | Sysname | string |
| duration_milliseconds | duration_milliseconds_d | Yürütme süresini milisaniye cinsinden sorgula | bigint | int |
| event_time | event_time_t | Denetlenebilir eylemin yapıldığı tarih ve saat | datetime2 | datetime |
| Host_name | Yok | İstemci ana bilgisayar adı | string | Yok |
| is_column_permission | is_column_permission_s | Bu bir sütun düzeyi izni olup olmadığını belirten bayrak. 1 = true, 0 = yanlış | bit | string |
| Yok | is_server_level_audit_s | Bu denetimin sunucu düzeyinde olup olmadığını belirten bayrak | Yok | string |
| object_ kimliği | object_id_d | Denetimin gerçekleştiği varlığın kimliği. Bu şunları içerir: sunucu nesneleri, veritabanları, veritabanı nesneleri ve şema nesneleri. Varlık sunucunun kendisiyse veya denetim nesne düzeyinde gerçekleştirilmiyorsa 0 | int | int |
| Object_name | object_name_s | Denetimin gerçekleştiği varlığın adı. Bu şunları içerir: sunucu nesneleri, veritabanları, veritabanı nesneleri ve şema nesneleri. Varlık sunucunun kendisiyse veya denetim nesne düzeyinde gerçekleştirilmiyorsa 0 | Sysname | string |
| permission_bitmask | permission_bitmask_s | Gerektiğinde, verilen, reddedilen veya iptal edilen izinleri gösterir | varbinary(16) | string |
| response_rows | response_rows_d | Sonuç kümesinde döndürülen satır sayısı | bigint | int |
| Schema_name | schema_name_s | Eylemin gerçekleştiği şema bağlamı. Şema dışında meydana gelen denetimler için NULL | Sysname | string |
| Yok | securable_class_type_s | Denetlenen class_type eşlenen securable nesne | Yok | string |
| sequence_group_id | sequence_group_id_g | Benzersiz tanımlayıcı | Varbinary | GUID |
| sequence_number | sequence_number_d | Denetimler için yazma arabelleğisığdırılamayacak kadar büyük olan tek bir denetim kaydındaki kayıtların sırasını izler | int | int |
| server_instance_name | server_instance_name_s | Denetimin gerçekleştiği sunucu örneğinin adı | Sysname | string |
| server_principal_id | server_principal_id_d | Eylemin gerçekleştirildiği giriş bağlamının kimliği | int | int |
| server_principal_name | server_principal_name_s | Geçerli giriş | Sysname | string |
| server_principal_sid | server_principal_sid_s | Geçerli giriş SID | Varbinary | string |
| Session_id | session_id_d | Olayın meydana geldiği oturumun kimliği | smallint | int |
| session_server_principal_name | session_server_principal_name_s | Oturum için sunucu sorumlusu | Sysname | string |
| Deyim | statement_s | Yürütülen T-SQL deyimi (varsa) | nvarchar(4000) | string |
| başarılı | succeeded_s | Olayı tetikleyen eylemin başarılı olup olmadığını gösterir. Oturum açma ve toplu iş dışındaki olaylar için bu, yalnızca izin denetiminin işlem değil, başarılı olup olmadığını bildirir. 1 = başarı, 0 = başarısız | bit | string |
| target_database_principal_id | target_database_principal_id_d | GRANT/DENY/REVOKE işleminin yapıldığı veritabanı sorumlusu. Geçerli değilse 0 | int | int |
| target_database_principal_name | target_database_principal_name_s | Hedef eylem kullanıcısı. Geçerli değilse NULL | string | string |
| target_server_principal_id | target_server_principal_id_d | GRANT/DENY/REVOKE işleminin yapıldığı sunucu sorumlusu. Yoksa 0'ı döndürür | int | int |
| target_server_principal_name | target_server_principal_name_s | Hedef eylem girişi. Geçerli değilse NULL | Sysname | string |
| target_server_principal_sid | target_server_principal_sid_s | Hedef girişSID. Geçerli değilse NULL | Varbinary | string |
| transaction_id | transaction_id_d | Yalnızca SQL Server (2016'dan itibaren) - 0 Azure SQL DB için | bigint | int |
| user_defined_event_id | user_defined_event_id_d | Kullanıcı tanımlı olay kimliği sp_audit_write için bir argüman olarak geçti. Sistem olayları (varsayılan) için NULL ve kullanıcı tanımlı olay için sıfır olmayan. Daha fazla bilgi için [bkz: sp_audit_write (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | smallint | int |
| user_defined_information | user_defined_information_s | Kullanıcı tanımlı bilgiler sp_audit_write için bir argüman olarak geçirilen. Sistem olayları (varsayılan) için NULL ve kullanıcı tanımlı olay için sıfır olmayan. Daha fazla bilgi için [bkz: sp_audit_write (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | nvarchar(4000) | string |

## <a name="next-steps"></a>Sonraki Adımlar

[Azure SQL Veritabanı denetimi](sql-database-auditing.md)hakkında daha fazla bilgi edinin.
