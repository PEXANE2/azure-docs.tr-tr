---
title: Azure Backup için Azure Izleyici günlükleri veri modeli
description: Bu makalede, Azure Backup verileri için Azure Izleyici günlükleri veri modeli ayrıntıları ele bildirilir.
ms.reviewer: adigan
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: dacurwin
ms.openlocfilehash: 878e4e7508d82f78e82f1fd8bda69079d9468e9f
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68689231"
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Azure Backup verileri için Log Analytics veri modeli

Log Analytics özel uyarılar oluşturmak için Log Analytics veri modelini kullanın.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="using-azure-backup-data-model"></a>Azure Backup veri modelini kullanma

Gereksinimlerinize göre görseller, özel sorgular ve panolar oluşturmak için veri modelinin bir parçası olarak sunulan aşağıdaki alanları kullanabilirsiniz.

### <a name="alert"></a>Uyarı

Bu tablo, uyarı ile ilgili alanlarla ilgili ayrıntıları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| AlertUniqueId_s |Text |Oluşturulan uyarının benzersiz tanıtıcısı |
| AlertType_s |Text |Uyarının türü, örneğin, yedekleme |
| AlertStatus_s |Text |Uyarının durumu, örneğin, etkin |
| AlertOccurrenceDateTime_s |Date/Time |Uyarının oluşturulduğu tarih ve saat |
| AlertSeverity_s |Text |Uyarının önem derecesi, örneğin, kritik |
|AlertTimeToResolveInMinutes_s    | Number        |Bir uyarının çözülmesi için geçen süre. Etkin uyarılar için boş.         |
|AlertConsolidationStatus_s   |Text         |Uyarının birleştirilmiş bir uyarı olup olmadığını tanımla         |
|CountOfAlertsConsolidated_s     |Number         |Birleştirilmiş bir uyarıda birleştirilmiş uyarı sayısı          |
|AlertRaisedOn_s     |Text         |Uyarının üzerinde ortaya çıkarılan varlık türü         |
|AlertCode_s     |Text         |Uyarı türünü benzersiz şekilde tanımlamak için kod         |
|RecommendedAction_s   |Text         |Uyarıyı çözümlemek için önerilen eylem         |
| EventName_s |Text |Etkinliğin adı. Always AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |Uyarıyla ilişkili yedekleme öğesinin benzersiz tanımlayıcısı |
| SchemaVersion_s |Text |Şemanın geçerli sürümü, örneğin **v2** |
| State_s |Text |Uyarı nesnesinin geçerli durumu, örneğin, etkin, silindi |
| BackupManagementType_s |Text |Yedekleme gerçekleştirmek için sağlayıcı türü, örneğin, bu uyarının ait olduğu ıaasvm, FileFolder |
| OperationName |Text |Geçerli işlemin adı, örneğin, uyarı |
| Category |Text |Azure Izleyici günlüklerine gönderilen tanılama verileri kategorisi. Always AzureBackupReport |
| Resource |Text |Bu, verilerin toplandığı kaynaktır, kurtarma hizmetleri kasasının adını gösterir |
| ProtectedContainerUniqueId_s |Text |Uyarıyla ilişkili korumalı sunucunun benzersiz tanıtıcısı (v1 'de ProtectedServerUniqueId_s)|
| VaultUniqueId_s |Text |Uyarıyla ilişkili korumalı kasasının benzersiz tanıtıcısı |
| SourceSystem |Text |Geçerli verilerin kaynak sistemi-Azure |
| RESOURCEID |Text |Verilerin toplandığı kaynak için benzersiz tanımlayıcı. Örneğin, bir kurtarma hizmetleri Kasası kaynak kimliği |
| SubscriptionId |Text |Kaynağın abonelik tanımlayıcısı (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceGroup |Text |Kaynağın kaynak grubu (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceProvider |Text |Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft. RecoveryServices |
| KaynakTürü |Text |Verilerin toplandığı kaynak türü. Örneğin, kasa |

### <a name="backupitem"></a>Backupıtem

Bu tablo, yedekleme öğesiyle ilgili alanlarla ilgili ayrıntıları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Text |Etkinliğin adı. Always AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |Yedekleme öğesinin benzersiz tanımlayıcısı |
| BackupItemId_s |Text |Yedekleme öğesinin tanımlayıcısı (Bu alan yalnızca v1 şeması içindir) |
| BackupItemName_s |Text |Yedekleme öğesinin adı |
| BackupItemFriendlyName_s |Text |Yedekleme öğesinin kolay adı |
| BackupItemType_s |Text |Yedekleme öğesinin türü, örneğin, VM, dosya klasörü |
| BackupItemProtectionState_s |Text |Yedekleme öğesinin koruma durumu |
| BackupItemAppVersion_s |Text |Yedekleme öğesinin uygulama sürümü |
| ProtectionState_s |Text |Yedekleme öğesinin geçerli koruma durumu, örneğin korumalı, Protectiondurduruldu |
| ProtectionGroupName_s |Text | Yedekleme öğesinin korunan koruma grubunun adı, SC DPM ve varsa MABS için|
| SecondaryBackupProtectionState_s |Text |Yedekleme öğesi için ikincil korumanın etkinleştirilip etkinleştirilmediği|
| SchemaVersion_s |Text |Şemanın sürümü, örneğin, **v2** |
| State_s |Text |Yedekleme öğesi nesnesinin durumu, örneğin, etkin, silindi |
| BackupManagementType_s |Text |Yedekleme gerçekleştirmek için sağlayıcı türü, örneğin, bu yedekleme öğesinin ait olduğu ıaasvm, FileFolder |
| OperationName |Text |İşlemin adı, örneğin, Backupıtem |
| Category |Text |Azure Izleyici günlüklerine gönderilen tanılama verileri kategorisi. Always AzureBackupReport |
| Resource |Text |Verilerin toplandığı kaynak (örneğin, kurtarma hizmetleri Kasası adı) |
| SourceSystem |Text |Geçerli verilerin kaynak sistemi-Azure |
| RESOURCEID |Text |Toplanmakta olan verilerin kaynak kimliği, örneğin kurtarma hizmetleri Kasası kaynak kimliği |
| SubscriptionId |Text |Kaynağın abonelik tanımlayıcısı (örn. Toplanan veriler için kurtarma hizmetleri Kasası) |
| ResourceGroup |Text |Kaynağın kaynak grubu (örn. Toplanan veriler için kurtarma hizmetleri Kasası) |
| ResourceProvider |Text |Toplanmakta olan veriler için kaynak sağlayıcı, örneğin, Microsoft. RecoveryServices |
| KaynakTürü |Text |Toplanmakta olan veriler için kaynak türü, örneğin Kasakasaları |

### <a name="backupitemassociation"></a>BackupItemAssociation

Bu tablo, çeşitli varlıklarla yedekleme öğesi ilişkilendirmeleri hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Text |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |Yedekleme öğesinin benzersiz kimliği |
| SchemaVersion_s |Text |Bu alan, şemanın geçerli sürümünü belirtir, **v2** 'dir |
| State_s |Text |Yedekleme öğesi ilişkilendirme nesnesinin geçerli durumu, örneğin, etkin, silindi |
| BackupManagementType_s |Text |Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin ıaasvm, FileFolder |
| BackupItemSourceSize_s |Text | Yedekleme öğesinin ön uç boyutu |
| BackupManagementServerUniqueId_s |Text | Yedekleme yönetim sunucusunu benzersiz şekilde tanımlamak için alan, varsa yedekleme öğesi tarafından korunur |
| Category |Text |Bu alan, Log Analytics gönderilen tanılama verilerinin kategorisini temsil eder, AzureBackupReport |
| OperationName |Text |Bu alan geçerli işlemin adını temsil eder-BackupItemAssociation |
| Resource |Text |Bu, verilerin toplandığı kaynaktır, kurtarma hizmetleri kasasının adını gösterir |
| ProtectedContainerUniqueId_s |Text |Yedekleme öğesiyle ilişkili korumalı sunucunun benzersiz tanıtıcısı (v1 'de ProtectedServerUniqueId_s) |
| VaultUniqueId_s |Text |Yedekleme öğesini içeren kasanın benzersiz tanımlayıcısı |
| SourceSystem |Text |Geçerli verilerin kaynak sistemi-Azure |
| RESOURCEID |Text |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, kurtarma hizmetleri Kasası kaynak kimliği |
| SubscriptionId |Text |Kaynağın abonelik tanımlayıcısı (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceGroup |Text |Kaynağın kaynak grubu (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceProvider |Text |Toplanmakta olan veriler için kaynak sağlayıcı, örneğin, Microsoft. RecoveryServices |
| KaynakTürü |Text |Verilerin toplandığı kaynak türü, örneğin kasa |

### <a name="backupmanagementserver"></a>BackupManagementServer

Bu tablo, çeşitli varlıklarla yedekleme öğesi ilişkilendirmeleri hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
|BackupManagementServerName_s     |Text         |Yedekleme yönetim sunucusunun adı        |
|AzureBackupAgentVersion_s     |Text         |Yedekleme yönetim sunucusundaki Azure Backup aracısının sürümü          |
|BackupManagementServerVersion_s     |Text         |Yedekleme yönetim sunucusunun sürümü|
|BackupManagementServerOSVersion_s    |Text            |Yedekleme yönetim sunucusunun işletim sistemi sürümü|
|BackupManagementServerType_s     |Text         |Yedekleme yönetim sunucusunun türü, MABS, SC DPM olarak|
|BackupManagementServerUniqueId_s     |Text         |Yedekleme yönetim sunucusunu benzersiz olarak tanımlamak için alan       |
| SourceSystem |Text |Geçerli verilerin kaynak sistemi-Azure |
| RESOURCEID |Text |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, kurtarma hizmetleri Kasası kaynak kimliği |
| SubscriptionId |Text |Kaynağın abonelik tanımlayıcısı (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceGroup |Text |Kaynağın kaynak grubu (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceProvider |Text |Toplanmakta olan veriler için kaynak sağlayıcı, örneğin, Microsoft. RecoveryServices |
| KaynakTürü |Text |Verilerin toplandığı kaynak türü, örneğin kasa |

### <a name="job"></a>İş

Bu tablo, işle ilgili alanlarla ilgili ayrıntıları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Text |Etkinliğin adı. Always AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |Yedekleme öğesinin benzersiz tanımlayıcısı |
| SchemaVersion_s |Text |Şemanın sürümü, örneğin, **v2** |
| State_s |Text |İş nesnesinin geçerli durumu, örneğin, etkin, silindi |
| BackupManagementType_s |Text |Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin ıaasvm, FileFolder |
| OperationName |Text |Bu alan geçerli işlemin adını temsil eder-Iş |
| Category |Text |Bu alan, Azure Izleyici günlüklerine gönderilen tanılama verilerinin kategorisini temsil eder, AzureBackupReport |
| Resource |Text |Bu, verilerin toplandığı kaynaktır, kurtarma hizmetleri kasasının adını gösterir |
| ProtectedServerUniqueId_s |Text |İşle ilişkili korumalı sunucunun benzersiz tanıtıcısı |
| ProtectedContainerUniqueId_s |Text | İşin çalıştırıldığı korunan kapsayıcıyı belirlemek için benzersiz kimlik |
| VaultUniqueId_s |Text |Korumalı kasanın benzersiz tanıtıcısı |
| JobOperation_s |Text |İşin çalıştırıldığı işlem Örneğin, yedekleme, geri yükleme, yedeklemeyi yapılandırma |
| JobStatus_s |Text |Tamamlanmış işin durumu, örneğin, tamamlandı, başarısız |
| JobFailureCode_s |Text |İş hatası oluştuğundan hata kodu dizesi |
| JobStartDateTime_s |Date/Time |İşin çalışmaya başladığı tarih ve saat |
| BackupStorageDestination_s |Text |Yedekleme depolamanın hedefi, örneğin, bulut, disk  |
| AdHocOrScheduledJob_s |Text | İşin geçici veya zamanlanmış olduğunu belirten alan |
| JobDurationInSecs_s | Number |Saniye cinsinden toplam iş süresi |
| DataTransferredInMB_s | Number |Bu iş için MB olarak aktarılan veriler|
| JobUniqueId_g |Text |İşi tanımlamak için benzersiz kimlik |
| RecoveryJobDestination_s |Text | Verilerin kurtarıldığı bir kurtarma işinin hedefi |
| RecoveryJobRPDateTime_s |Datetime | Kurtarılan kurtarma noktasının oluşturulduğu tarih, saat |
| RecoveryJobRPLocation_s |Text | Kurtarılan kurtarma noktasının depolandığı konum|
| SourceSystem |Text |Geçerli verilerin kaynak sistemi-Azure |
| RESOURCEID |Text |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, kurtarma hizmetleri Kasası kaynak kimliği|
| SubscriptionId |Text |Kaynağın abonelik tanımlayıcısı (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceGroup |Text |Kaynağın kaynak grubu (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceProvider |Text |Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft. RecoveryServices |
| KaynakTürü |Text |Verilerin toplandığı kaynak türü. Örneğin, kasa |

### <a name="policy"></a>İlke

Bu tablo ilkeyle ilgili alanlarla ilgili ayrıntıları sağlar.

| Alan | Veri Türü | Uygulanabilir sürümler | Açıklama |
| --- | --- | --- | --- |
| EventName_s |Text ||Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport |
| SchemaVersion_s |Text ||Bu alan, şemanın geçerli sürümünü belirtir, **v2** 'dir |
| State_s |Text ||İlke nesnesinin geçerli durumu, örneğin, etkin, silindi |
| BackupManagementType_s |Text ||Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin ıaasvm, FileFolder |
| OperationName |Text ||Bu alan geçerli işlemin adını temsil eder-Ilke |
| Category |Text ||Bu alan, Azure Izleyici günlüklerine gönderilen tanılama verilerinin kategorisini temsil eder, AzureBackupReport |
| Resource |Text ||Bu, verilerin toplandığı kaynaktır, kurtarma hizmetleri kasasının adını gösterir |
| PolicyUniqueId_g |Text ||İlkeyi tanımlamak için benzersiz kimlik |
| PolicyName_s |Text ||Tanımlanan ilkenin adı |
| BackupFrequency_s |Text ||Yedeklemelerin çalıştırıldığı sıklık (örneğin, günlük, haftalık) |
| BackupTimes_s |Text ||Yedeklemelerin zamanlandığı tarih ve saat |
| BackupDaysOfTheWeek_s |Text ||Yedeklemelerin zamanlandığı haftanın günleri |
| RetentionDuration_s |Tam sayı ||Yapılandırılan yedeklemeler için bekletme süresi |
| DailyRetentionDuration_s |Tam sayı ||Yapılandırılan yedeklemeler için gün cinsinden toplam saklama süresi |
| DailyRetentionTimes_s |Text ||Günlük bekletmenin yapılandırıldığı tarih ve saat |
| WeeklyRetentionDuration_s |Ondalık sayı ||Yapılandırılan yedeklemeler için hafta cinsinden toplam haftalık saklama süresi |
| WeeklyRetentionTimes_s |Text ||Haftalık bekletmenin yapılandırıldığı tarih ve saat |
| WeeklyRetentionDaysOfTheWeek_s |Text ||Haftalık saklama için seçilen haftanın günleri |
| MonthlyRetentionDuration_s |Ondalık sayı ||Yapılandırılan yedeklemeler için aylık toplam bekletme süresi |
| MonthlyRetentionTimes_s |Text ||Aylık bekletmenin yapılandırıldığı tarih ve saat |
| MonthlyRetentionFormat_s |Text ||Aylık saklama için yapılandırma türü, örneğin günlük tabanlı için günlük, haftalık tabanlı için haftalık |
| MonthlyRetentionDaysOfTheWeek_s |Text ||Aylık saklama için seçilen haftanın günleri |
| MonthlyRetentionWeeksOfTheMonth_s |Text ||Aylık bekletme yapılandırıldığında ayın haftası (örneğin, First, Last vb.) |
| YearlyRetentionDuration_s |Ondalık sayı ||Yapılandırılan yedeklemeler için yıl cinsinden toplam saklama süresi |
| YearlyRetentionTimes_s |Text ||Yıllık bekletmenin yapılandırıldığı tarih ve saat |
| YearlyRetentionMonthsOfTheYear_s |Text ||Yıllık bekletme için seçilen yılın ayı |
| YearlyRetentionFormat_s |Text ||Yıllık saklama için yapılandırma türü, örneğin, günlük tabanlı için günlük, haftalık tabanlı için haftalık | |
| YearlyRetentionDaysOfTheMonth_s |Text ||Yıllık bekletme için seçilen ayın tarihleri |
| SynchronisationFrequencyPerDay_s |Tam sayı |v2|SC DPM ve MABS için bir dosya yedeklemesinin eşitlendiği gün sayısı |
| DiffBackupFormat_s |Text |v2|Azure VM yedeklemesi 'nde SQL için değişiklik yedeklemelerinin biçimi |
| DiffBackupTime_s |Time |v2|Azure VM yedeklemesi 'nde SQL için değişiklik yedeklemeleri süresi|
| DiffBackupRetentionDuration_s |Ondalık sayı |v2|Azure VM yedeklemesi 'nde SQL için değişiklik yedeklemeleri bekletme süresi|
| LogBackupFrequency_s |Ondalık sayı |v2|SQL için günlük yedeklemeleri sıklığı|
| LogBackupRetentionDuration_s |Ondalık sayı |v2|Azure VM yedeklemesi 'nde SQL için günlük yedeklemeleri bekletme süresi|
| DiffBackupDaysofTheWeek_s |Text |v2|Azure VM yedeklemesi 'nde SQL için değişiklik yedeklemeleri için haftanın günleri|
| SourceSystem |Text ||Geçerli verilerin kaynak sistemi-Azure |
| RESOURCEID |Text ||Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, kurtarma hizmetleri Kasası kaynak kimliği |
| SubscriptionId |Text ||Kaynağın abonelik tanımlayıcısı (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceGroup |Text ||Kaynağın kaynak grubu (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceProvider |Text ||Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft. RecoveryServices |
| KaynakTürü |Text ||Verilerin toplandığı kaynak türü. Örneğin, kasa |

### <a name="policyassociation"></a>Poliyassociation

Bu tablo, çeşitli varlıklarla ilke ilişkilendirmeleri hakkında ayrıntılar sağlar.

| Alan | Veri Türü | Uygulanabilir sürümler | Açıklama |
| --- | --- | --- | --- |
| EventName_s |Text ||Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport |
| SchemaVersion_s |Text ||Bu alan, şemanın geçerli sürümünü belirtir, **v2** 'dir |
| State_s |Text ||İlke nesnesinin geçerli durumu, örneğin, etkin, silindi |
| BackupManagementType_s |Text ||Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin ıaasvm, FileFolder |
| OperationName |Text ||Bu alan geçerli işlemin adını temsil eder-PolicyAssociation |
| Category |Text ||Bu alan, Azure Izleyici günlüklerine gönderilen tanılama verilerinin kategorisini temsil eder, AzureBackupReport |
| Resource |Text ||Bu, verilerin toplandığı kaynaktır, kurtarma hizmetleri kasasının adını gösterir |
| PolicyUniqueId_g |Text ||İlkeyi tanımlamak için benzersiz kimlik |
| VaultUniqueId_s |Text ||Bu ilkenin ait olduğu kasanın benzersiz kimliği |
| BackupManagementServerUniqueId_s |Text |v2 |Yedekleme yönetim sunucusunu benzersiz şekilde tanımlamak için alan, varsa yedekleme öğesi tarafından korunur        |
| SourceSystem |Text ||Geçerli verilerin kaynak sistemi-Azure |
| RESOURCEID |Text ||Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, kurtarma hizmetleri Kasası kaynak kimliği |
| SubscriptionId |Text ||Kaynağın abonelik tanımlayıcısı (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceGroup |Text ||Kaynağın kaynak grubu (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceProvider |Text ||Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft. RecoveryServices |
| KaynakTürü |Text ||Verilerin toplandığı kaynak türü. Örneğin, kasa |

### <a name="protected-container"></a>Korumalı kapsayıcı

Bu tablo, korumalı kapsayıcılar hakkında temel alanlar sağlar. (V1 'de ProtectedServer idi)

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| ProtectedContainerUniqueId_s |Text | Korumalı kapsayıcıyı benzersiz olarak tanımlamak için alan |
| ProtectedContainerOSType_s |Text |Korumalı kapsayıcının işletim sistemi türü |
| ProtectedContainerOSVersion_s |Text |Korumalı kapsayıcının işletim sistemi sürümü |
| AgentVersion_s |Text |Aracı yedeklemesinin veya koruma aracısının sürüm numarası (SC DPM ve MABS durumunda) |
| BackupManagementType_s |Text |Yedekleme gerçekleştirmek için sağlayıcı türü. Örneğin, ıaasvm, FileFolder |
| EntityState_s |Text |Korumalı sunucu nesnesinin geçerli durumu. Örneğin, etkin, silindi |
| ProtectedContainerFriendlyName_s |Text |Korumalı sunucunun kolay adı |
| ProtectedContainerName_s |Text |Korumalı kapsayıcının adı |
| ProtectedContainerWorkloadType_s |Text |Yedeklenen korumalı kapsayıcının türü. Örneğin, ıaasvmcontainer |
| ProtectedContainerLocation_s |Text |Korumalı kapsayıcının şirket içinde mi yoksa Azure 'da mı bulunduğu |
| ProtectedContainerType_s |Text |Korumalı kapsayıcının bir sunucu mı yoksa bir kapsayıcı mi olduğunu belirtir |
| ProtectedContainerProtectionState_s'  |Text |Korumalı kapsayıcının koruma durumu |

### <a name="storage"></a>Depolama

Bu tablo, depolama ile ilgili alanlarla ilgili ayrıntıları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| CloudStorageInBytes_s |Ondalık sayı |Yedeklemeler tarafından kullanılan bulut yedekleme depolaması, en son değere göre hesaplanır (Bu alan yalnızca v1 şeması içindir)|
| ProtectedInstances_s |Ondalık sayı |En son değere göre hesaplanan, faturalandırma aşamasında ön uç depolamayı hesaplamak için kullanılan korumalı örnek sayısı |
| EventName_s |Text |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport |
| SchemaVersion_s |Text |Bu alan, şemanın geçerli sürümünü belirtir, **v2** 'dir |
| State_s |Text |Depolama nesnesinin geçerli durumu, örneğin, etkin, silindi |
| BackupManagementType_s |Text |Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin ıaasvm, FileFolder |
| OperationName |Text |Bu alan, geçerli işlem-depolama alanının adını temsil eder |
| Category |Text |Bu alan, Azure Izleyici günlüklerine gönderilen tanılama verilerinin kategorisini temsil eder, AzureBackupReport |
| Resource |Text |Bu, verilerin toplandığı kaynaktır, kurtarma hizmetleri kasasının adını gösterir |
| ProtectedServerUniqueId_s |Text |Depolama alanının hesaplandığı korumalı sunucunun benzersiz kimliği |
| VaultUniqueId_s |Text |Depolama kasasının benzersiz kimliği hesaplanır |
| SourceSystem |Text |Geçerli verilerin kaynak sistemi-Azure |
| RESOURCEID |Text |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, kurtarma hizmetleri Kasası kaynak kimliği |
| SubscriptionId |Text |Kaynağın abonelik tanımlayıcısı (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceGroup |Text |Kaynağın kaynak grubu (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceProvider |Text |Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft. RecoveryServices |
| KaynakTürü |Text |Verilerin toplandığı kaynak türü. Örneğin, kasa |
| StorageUniqueId_s |Text |Depolama varlığını tanımlamak için kullanılan benzersiz kimlik |
| StorageType_s |Text |Depolama türü, örneğin bulut, birim, disk |
| StorageName_s |Text |Depolama varlığının adı, örneğin E:\ |
| StorageTotalSizeInGBs_s |Text |Depolama varlığı tarafından tüketilen, GB cinsinden toplam depolama alanı boyutu|

### <a name="storageassociation"></a>StorageAssociation

Bu tablo, depolamayı diğer varlıklara bağlayan temel depolama ile ilgili alanları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- |  --- |
| StorageUniqueId_s |Text |Depolama varlığını tanımlamak için kullanılan benzersiz kimlik |
| SchemaVersion_s |Text |Bu alan, şemanın geçerli sürümünü belirtir, **v2** 'dir |
| BackupItemUniqueId_s |Text |Depolama varlığıyla ilgili yedekleme öğesini tanımlamak için kullanılan benzersiz kimlik |
| BackupManagementServerUniqueId_s |Text |Depolama varlığıyla ilgili yedekleme yönetim sunucusunu tanımlamak için kullanılan benzersiz kimlik|
| VaultUniqueId_s |Text |Depolama varlığıyla ilgili kasayı tanımlamak için kullanılan benzersiz kimlik|
| StorageConsumedInMBs_s |Number|Karşılık gelen depolama alanındaki karşılık gelen yedekleme öğesi tarafından tüketilen depolamanın boyutu |
| StorageAllocatedInMBs_s |Number |Disk türünde karşılık gelen yedekleme öğesi tarafından ayrılan depolama alanı boyutu|

### <a name="vault"></a>Kasa

Bu tablo, kasala ilgili alanlarla ilgili ayrıntıları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Text |Bu alan, bu olayın adını temsil eder, her zaman AzureBackupCentralReport |
| SchemaVersion_s |Text |Bu alan, şemanın geçerli sürümünü belirtir, **v2** 'dir |
| State_s |Text |Kasa nesnesinin geçerli durumu, örneğin, etkin, silindi |
| OperationName |Text |Bu alan, geçerli işlem kasasının adını temsil eder |
| Category |Text |Bu alan, Azure Izleyici günlüklerine gönderilen tanılama verilerinin kategorisini temsil eder, AzureBackupReport |
| Resource |Text |Bu, verilerin toplandığı kaynaktır, kurtarma hizmetleri kasasının adını gösterir |
| VaultUniqueId_s |Text |Kasanın benzersiz kimliği |
| VaultName_s |Text |Kasasının adı |
| AzureDataCenter_s |Text |Kasanın bulunduğu veri merkezi |
| StorageReplicationType_s |Text |Kasa için depolama çoğaltma türü (örneğin, Geoyedekli) |
| SourceSystem |Text |Geçerli verilerin kaynak sistemi-Azure |
| RESOURCEID |Text |Toplanmakta olan veriler için kaynak tanımlayıcısı. Örneğin, kurtarma hizmetleri Kasası kaynak kimliği |
| SubscriptionId |Text |Kaynağın abonelik tanımlayıcısı (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceGroup |Text |Kaynağın kaynak grubu (örn. Verilerin toplandığı kurtarma hizmetleri Kasası) |
| ResourceProvider |Text |Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft. RecoveryServices |
| KaynakTürü |Text |Verilerin toplandığı kaynak türü. Örneğin, kasa |

### <a name="backup-management-server"></a>Yedekleme yönetim sunucusu

Bu tablo yedekleme yönetim sunucuları hakkında temel alanlar sağlar.

|Alan  |Veri Türü  | Açıklama  |
|---------|---------|----------|
|BackupManagementServerName_s     |Text         |Yedekleme yönetim sunucusunun adı        |
|AzureBackupAgentVersion_s     |Text         |Yedekleme yönetim sunucusundaki Azure Backup aracısının sürümü          |
|BackupManagementServerVersion_s     |Text         |Yedekleme yönetim sunucusunun sürümü|
|BackupManagementServerOSVersion_s     |Text            |Yedekleme yönetim sunucusunun işletim sistemi sürümü|
|BackupManagementServerType_s     |Text         |Yedekleme yönetim sunucusunun türü, MABS, SC DPM olarak|
|BackupManagementServerUniqueId_s     |Text         |Yedekleme yönetim sunucusunu benzersiz olarak tanımlamak için alan       |

### <a name="preferredworkloadonvolume"></a>PreferredWorkloadOnVolume

Bu tablo, bir birimin ilişkilendirildiği iş yüklerini belirtir.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| StorageUniqueId_s |Text |Depolama varlığını tanımlamak için kullanılan benzersiz kimlik |
| BackupItemType_s |Text |Bu birimin tercih edilen depolama alanı olduğu iş yükleri|

### <a name="protectedinstance"></a>Protectedınstance

Bu tablo, temel korumalı örneklerle ilgili alanları sağlar.

| Alan | Veri Türü |Uygulanabilir sürümler | Açıklama |
| --- | --- | --- | --- |
| BackupItemUniqueId_s |Text |v2|DPM, MABS kullanılarak yedeklenen VM 'Ler için yedekleme öğesini tanımlamak üzere kullanılan benzersiz kimlik|
| ProtectedContainerUniqueId_s |Text |v2|DPM, MABS kullanılarak yedeklenen VM 'Ler hariç her şey için korunan kapsayıcıyı tanımlamak üzere kullanılan benzersiz kimlik|
| ProtectedInstanceCount_s |Text |v2|İlişkili yedekleme öğesi veya korunan kapsayıcı için bu tarih-saat için korunan örnek sayısı|

### <a name="recoverypoint"></a>RecoveryPoint

Bu tablo, temel kurtarma noktasıyla ilgili alanları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| BackupItemUniqueId_s |Text |DPM, MABS kullanılarak yedeklenen VM 'Ler için yedekleme öğesini tanımlamak üzere kullanılan benzersiz kimlik|
| OldestRecoveryPointTime_s |Text |Yedekleme öğesi için en eski kurtarma noktasının tarih saati|
| OldestRecoveryPointLocation_s |Text |Yedekleme öğesi için en eski kurtarma noktası konumu|
| LatestRecoveryPointTime_s |Text |Yedekleme öğesi için en son kurtarma noktasının tarih saati|
| LatestRecoveryPointLocation_s |Text |Yedekleme öğesi için en son kurtarma noktasının konumu|

## <a name="next-steps"></a>Sonraki adımlar

Veri modelini gözden geçirdikten sonra, kendi panonuzu oluşturmak için Azure Izleyici günlüklerinde [özel sorgular oluşturmaya](../azure-monitor/learn/tutorial-logs-dashboards.md) başlayabilirsiniz.
