---
title: Azure Monitor veri modelini kaydeder
description: Bu makalede, Azure Yedekleme verileri için Azure Monitor Log Analytics veri modeli ayrıntıları hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 02/26/2019
ms.openlocfilehash: d14634c5e317682462e77e0549f064c75059f15c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586385"
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Azure Yedekleme verileri için Günlük Analizi veri modeli

Log Analytics'ten özel uyarılar oluşturmak için Günlük Analizi veri modelini kullanın.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

> [!NOTE]
>
> Bu veri modeli, Log Analytics'e (LA) tanılama olayları göndermek için Azure Tanılama Modu'na başvurur. Yeni Kaynak Özel Modu'nun veri modelini öğrenmek için aşağıdaki makaleye bakabilirsiniz: [Azure Yedekleme Tanılama Etkinlikleri için Veri Modeli](https://docs.microsoft.com/azure/backup/backup-azure-reports-data-model)

## <a name="using-azure-backup-data-model"></a>Azure Yedekleme veri modelini kullanma

Gereksinimlerinize göre görseller, özel sorgular ve pano oluşturmak için veri modelinin bir parçası olarak sağlanan aşağıdaki alanları kullanabilirsiniz.

### <a name="alert"></a>Uyarı

Bu tablo, uyarı ile ilgili alanlar hakkında ayrıntılı bilgi sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| AlertUniqueId_s |Metin |Oluşturulan uyarının benzersiz tanımlayıcısı |
| AlertType_s |Metin |Uyarı türü, örneğin, Yedekleme |
| AlertStatus_s |Metin |Uyarının durumu, örneğin, Etkin |
| AlertOccurrenceDateTime_s |Tarih/Saat |Uyarının oluşturulduğu tarih ve saat |
| AlertSeverity_s |Metin |Uyarının önem derecesi, örneğin, Kritik |
|AlertTimeToResolveInMinutes_s    | Sayı        |Bir uyarıyı çözmek için zaman alınır. Etkin uyarılar için boş.         |
|AlertConsolidationStatus_s   |Metin         |Uyarının birleştirilmiş bir uyarı olup olmadığını belirleme         |
|CountOfAlertsConsolidated_s     |Sayı         |Birleştirilmiş bir uyarıysa birleştirilmiş uyarı sayısı          |
|AlertRaisedOn_s     |Metin         |Uyarının yükseltildiği varlık türü         |
|AlertCode_s     |Metin         |Bir uyarı türünü benzersiz olarak tanımlamak için kod         |
|RecommendedAction_s   |Metin         |Uyarıyı gidermek için önerilen eylem         |
| EventName_s |Metin |Olayın adı. Her Zaman AzureBackupCentralReport |
| BackupItemUniqueId_s |Metin |Uyarıyla ilişkili yedekleme öğesinin benzersiz tanımlayıcısı |
| SchemaVersion_s |Metin |Şemanın geçerli sürümü, örneğin **V2** |
| State_s |Metin |Uyarı nesnesinin geçerli durumu, örneğin, Etkin, Silinmiş |
| BackupManagementType_s |Metin |Yedekleme gerçekleştirmek için sağlayıcı türü, örneğin, IaaSVM, FileFolder hangi bu uyarı ait |
| ThrottledRequests |Metin |Geçerli işlemin adı, örneğin, Uyarı |
| Kategori |Metin |Azure Monitor günlüklerine itilen tanılama verileri kategorisi. Her Zaman AzureBackupReport |
| Kaynak |Metin |Bu, verilerin toplandığı kaynaktır, Kurtarma Hizmetleri kasa adını gösterir |
| ProtectedContainerUniqueId_s |Metin |Uyarıyla ilişkili korumalı sunucunun benzersiz tanımlayıcısı (V1'de ProtectedServerUniqueId_s)|
| VaultUniqueId_s |Metin |Uyarı yla ilişkili korumalı kasanın benzersiz tanımlayıcısı |
| SourceSystem |Metin |Geçerli verilerin kaynak sistemi - Azure |
| ResourceId |Metin |Hangi verilerin toplandığı hakkında kaynak için benzersiz tanımlayıcı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| SubscriptionId |Metin |Kaynağın abonelik tanımlayıcısı (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceGroup |Metin |Kaynağın kaynak grubu (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceProvider |Metin |Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Verilerin toplandığı kaynak türü. Örneğin, Vaults |

### <a name="backupitem"></a>Yedekleme Öğesi

Bu tablo, yedekleme öğesi ile ilgili alanlar hakkında ayrıntılı bilgi sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Olayın adı. Her Zaman AzureBackupCentralReport |  
| BackupItemUniqueId_s |Metin |Yedekleme öğesinin benzersiz tanımlayıcısı |
| BackupItemId_s |Metin |Yedekleme öğesinin tanımlayıcısı (Bu alan sadece v1 şeması içindir) |
| BackupItemName_s |Metin |Yedek öğenin adı |
| BackupItemFriendlyName_s |Metin |Yedekleme öğesinin dostu adı |
| BackupItemType_s |Metin |Yedekleme öğesi türü, örneğin, VM, FileFolder |
| BackupItemProtectionState_s |Metin |Yedekleme Öğesinin Koruma Durumu |
| BackupItemAppVersion_s |Metin |Yedek öğenin uygulama sürümü |
| ProtectionState_s |Metin |Yedek öğenin geçerli koruma durumu, örneğin, Korumalı, Koruma Durduruldu |
| ProtectionGroupName_s |Metin | Koruma Grubunun adı Yedek Öğe, varsa SC DPM ve MABS için korunur|
| SecondaryBackupProtectionState_s |Metin |Yedekleme öğesi için ikincil korumanın etkin olup olmadığı|
| SchemaVersion_s |Metin |Şema sürümü, örneğin, **V2** |
| State_s |Metin |Yedek öğe nesnesinin durumu, örneğin, Etkin, Silinmiş |
| BackupManagementType_s |Metin |Yedekleme gerçekleştirmek için sağlayıcı türü, örneğin, IaaSVM, FileFolder hangi bu yedekleme öğesia ait |
| ThrottledRequests |Metin |Operasyonun adı, örneğin, BackupItem |
| Kategori |Metin |Azure Monitor günlüklerine itilen tanılama verileri kategorisi. Her Zaman AzureBackupReport |
| Kaynak |Metin |Verilerin toplandığı kaynak, örneğin, Kurtarma Hizmetleri kasa adı |
| SourceSystem |Metin |Geçerli verilerin kaynak sistemi - Azure |
| ResourceId |Metin |Toplanan veriler için kaynak kimliği, örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| SubscriptionId |Metin |Kaynağın abonelik tanımlayıcısı (eski için. Toplama Hizmetleri kasası) toplanan veriler için |
| ResourceGroup |Metin |Kaynağın kaynak grubu (eski için. Toplama Hizmetleri kasası) toplanan veriler için |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı , örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Toplanan veriler için kaynağın türü, örneğin, Vaults |

### <a name="backupitemassociation"></a>BackupItemAssociation

Bu tablo, çeşitli varlıklar ile yedekleme öğesi ilişkileri hakkında ayrıntıları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan bu olayın adını temsil eder, her zaman AzureBackupCentralReport |  
| BackupItemUniqueId_s |Metin |Yedekleme öğesinin benzersiz kimliği |
| SchemaVersion_s |Metin |Bu alan şema geçerli sürümünü gösterir, bu **V2** olduğunu |
| State_s |Metin |Yedekleme öğesi ilişkilendirme nesnesinin geçerli durumu, örneğin, Etkin, Silinmiş |
| BackupManagementType_s |Metin |Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin, IaaSVM, FileFolder |
| BackupItemSourceSize_s |Metin | Yedekleme öğesinin ön uç boyutu |
| BackupManagementServerUniqueId_s |Metin | Yedekleme Yönetim Sunucusu'nu benzersiz bir şekilde tanımlamak için alan, varsa Yedek Öğe tarafından korunur |
| Kategori |Metin |Bu alan, Log Analytics'e itilen tanılama verileri kategorisini temsil eder, azureBackupReport'dur |
| ThrottledRequests |Metin |Bu alan geçerli işlemin adını temsil eder - BackupItemAssociation |
| Kaynak |Metin |Bu, verilerin toplandığı kaynaktır, Kurtarma Hizmetleri kasa adını gösterir |
| ProtectedContainerUniqueId_s |Metin |Yedek öğeyle ilişkili korumalı sunucunun benzersiz tanımlayıcısı (V1'de ProtectedServerUniqueId_s) |
| VaultUniqueId_s |Metin |Yedek öğeyi içeren kasanın benzersiz tanımlayıcısı |
| SourceSystem |Metin |Geçerli verilerin kaynak sistemi - Azure |
| ResourceId |Metin |Toplanan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| SubscriptionId |Metin |Kaynağın abonelik tanımlayıcısı (eski için. Kurtarma Hizmetleri kasası) verilerin toplandığı kasa |
| ResourceGroup |Metin |Kaynağın kaynak grubu (eski için. Kurtarma Hizmetleri kasası) verilerin toplandığı kasa |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı , örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Veri için kaynağın türü toplanıyor, örneğin, Vaults |

### <a name="backupmanagementserver"></a>BackupManagementServer

Bu tablo, çeşitli varlıklar ile yedekleme öğesi ilişkileri hakkında ayrıntıları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
|BackupManagementServerName_s     |Metin         |Yedekleme Yönetim Sunucusunun Adı        |
|AzureBackupAgentVersion_s     |Metin         |Yedekleme Yönetim Sunucusundaki Azure Yedekleme Aracısının Sürümü          |
|BackupManagementServerVersion_s     |Metin         |Yedekleme Yönetim Sunucusu Sürümü|
|BackupManagementServerOSVersion_s    |Metin            |Yedekleme Yönetim Sunucusu'nun işletim sistemi sürümü|
|BackupManagementServerType_s     |Metin         |MABS, SC DPM olarak Yedekleme Yönetim Sunucusu nun türü|
|BackupManagementServerUniqueId_s     |Metin         |Yedekleme Yönetim Sunucusu'nu benzersiz bir şekilde tanımlama alanı       |
| SourceSystem |Metin |Geçerli verilerin kaynak sistemi - Azure |
| ResourceId |Metin |Toplanan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| SubscriptionId |Metin |Kaynağın abonelik tanımlayıcısı (eski için. Kurtarma Hizmetleri kasası) verilerin toplandığı kasa |
| ResourceGroup |Metin |Kaynağın kaynak grubu (eski için. Kurtarma Hizmetleri kasası) verilerin toplandığı kasa |
| ResourceProvider |Metin |Toplanan veriler için kaynak sağlayıcısı , örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Veri için kaynağın türü toplanıyor, örneğin, Vaults |

### <a name="job"></a>İş

Bu tablo, işle ilgili alanlar hakkında ayrıntılı bilgi sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Olayın adı. Her Zaman AzureBackupCentralReport |
| BackupItemUniqueId_s |Metin |Yedekleme öğesinin benzersiz tanımlayıcısı |
| SchemaVersion_s |Metin |Şema sürümü, örneğin, **V2** |
| State_s |Metin |İş nesnesinin geçerli durumu, örneğin, Etkin, Silinmiş |
| BackupManagementType_s |Metin |Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin, IaaSVM, FileFolder |
| ThrottledRequests |Metin |Bu alan geçerli işlemin adını temsil eder - İş |
| Kategori |Metin |Bu alan, Azure Monitor günlüklerine itilen tanılama verileri kategorisini temsil eder, AzureBackupReport'dur |
| Kaynak |Metin |Bu, verilerin toplandığı kaynaktır, Kurtarma Hizmetleri kasa adını gösterir |
| ProtectedServerUniqueId_s |Metin |İşle ilişkili korumalı sunucunun benzersiz tanımlayıcısı |
| ProtectedContainerUniqueId_s |Metin | İşin çalıştırılacağı korumalı kapsayıcıyı tanımlamak için benzersiz kimlik |
| VaultUniqueId_s |Metin |Korunan kasanın benzersiz tanımlayıcısı |
| JobOperation_s |Metin |Örneğin hangi işin çalıştırıldığı için çalışma, Yedekleme, Geri Yükleme, Yedekleme yi Yapılandırma |
| JobStatus_s |Metin |Bitmiş işin durumu, örneğin, Tamamlanmış, Başarısız |
| JobFailureCode_s |Metin |İş hatası nedeniyle Hata Kodu dizesi |
| JobStartDateTime_s |Tarih/Saat |İşin çalışmaya başladığı tarih ve saat |
| BackupStorageDestination_s |Metin |Yedekleme depolama noktası, örneğin, Bulut, Disk  |
| AdHocOrScheduledJob_s |Metin | İşin Geçici mi yoksa Zamanlanmış mı olduğunu belirtmek için alan |
| JobDurationInSecs_s | Sayı |Saniye cinsinden toplam iş süresi |
| DataTransferredInMB_s | Sayı |Bu iş için MB'ye aktarılan veriler|
| JobUniqueId_g |Metin |İşi tanımlamak için benzersiz kimlik |
| RecoveryJobDestination_s |Metin | Verilerin kurtarıldığı kurtarma işinin hedefi |
| RecoveryJobRPDateTime_s |DateTime | Kurtarılmakta olan kurtarma noktasının oluşturulduğu tarih, Saat |
| RecoveryJobRPLocation_s |Metin | Kurtarılan kurtarma noktasının depolandığı konum|
| SourceSystem |Metin |Geçerli verilerin kaynak sistemi - Azure |
| ResourceId |Metin |Toplanan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği|
| SubscriptionId |Metin |Kaynağın abonelik tanımlayıcısı (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceGroup |Metin |Kaynağın kaynak grubu (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceProvider |Metin |Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Verilerin toplandığı kaynak türü. Örneğin, Vaults |

### <a name="policy"></a>İlke

Bu tablo, ilke ile ilgili alanlar hakkında ayrıntılı bilgi sağlar.

| Alan | Veri Türü | Geçerli Sürümler | Açıklama |
| --- | --- | --- | --- |
| EventName_s |Metin ||Bu alan bu olayın adını temsil eder, her zaman AzureBackupCentralReport |
| SchemaVersion_s |Metin ||Bu alan şema geçerli sürümünü gösterir, bu **V2** olduğunu |
| State_s |Metin ||İlke nesnesinin geçerli durumu, örneğin, Etkin, Silinmiş |
| BackupManagementType_s |Metin ||Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin, IaaSVM, FileFolder |
| ThrottledRequests |Metin ||Bu alan geçerli işlemin adını temsil eder - İlke |
| Kategori |Metin ||Bu alan, Azure Monitor günlüklerine itilen tanılama verileri kategorisini temsil eder, AzureBackupReport'dur |
| Kaynak |Metin ||Bu, verilerin toplandığı kaynaktır, Kurtarma Hizmetleri kasa adını gösterir |
| PolicyUniqueId_g |Metin ||İlkeyi tanımlamak için benzersiz kimlik |
| PolicyName_s |Metin ||Tanımlanan ilkenin adı |
| BackupFrequency_s |Metin ||Yedeklemelerin çalıştırılan sıklık, örneğin, günlük, haftalık |
| BackupTimes_s |Metin ||Yedeklemelerin zamanlandığı tarih ve saat |
| BackupDaysOfTheWeek_s |Metin ||Yedeklemelerin zamanlandığı haftanın günleri |
| RetentionDuration_s |Tam Sayı ||Yapılandırılmış yedeklemeler için bekletme süresi |
| DailyRetentionDuration_s |Tam Sayı ||Yapılandırılmış yedeklemeler için gün içinde toplam bekletme süresi |
| DailyRetentionTimes_s |Metin ||Günlük bekletme yapılandırıldığı tarih ve saat |
| WeeklyRetentionDuration_s |Ondalık Sayı ||Yapılandırılmış yedeklemeler için haftalar içinde toplam haftalık bekletme süresi |
| WeeklyRetentionTimes_s |Metin ||Haftalık bekletme yapılandırıldığı tarih ve saat |
| WeeklyRetentionDaysOfTheWeek_s |Metin ||Haftalık bekletme için seçilen haftanın günleri |
| MonthlyRetentionDuration_s |Ondalık Sayı ||Yapılandırılmış yedeklemeler için aylar daki toplam bekletme süresi |
| MonthlyRetentionTimes_s |Metin ||Aylık bekletme yapılandırıldığı tarih ve saat |
| MonthlyRetentionFormat_s |Metin ||Aylık bekletme için yapılandırma türü, örneğin, günlük tabanlı günlük, haftalık haftalık tabanlı |
| MonthlyRetentionDaysOfTheWeek_s |Metin ||Aylık saklama için seçilen haftanın günleri |
| MonthlyRetentionWeeksOfTheMonth_s |Metin ||Aylık saklamanın yapılandırıldığı ayın haftaları, örneğin, İlk, Son vb. |
| YearlyRetentionDuration_s |Ondalık Sayı ||Yapılandırılmış yedeklemeler için yıllar içinde toplam bekletme süresi |
| YearlyRetentionTimes_s |Metin ||Yıllık bekletme yapılandırıldığı tarih ve saat |
| YearlyRetentionMonthsOfTheYear_s |Metin ||Yıllık bekletme için seçilen yılın ayları |
| YearlyRetentionFormat_s |Metin ||Yıllık bekletme için yapılandırma türü, örneğin, günlük tabanlı günlük, haftalık haftalık tabanlı | |
| YearlyRetentionDaysOfTheMonth_s |Metin ||Yıllık bekletme için seçilen ayın tarihleri |
| SynchronisationFrequencyPerDay_s |Tam Sayı |v2|SC DPM ve MABS için bir dosya yedeklemesi günde kaç kez eşitlenir |
| DiffBackupFormat_s |Metin |v2|Azure VM yedeklemesinde SQL için Diferansiyel yedeklemebiçimleri |
| DiffBackupTime_s |Zaman |v2|Azure VM Yedekleme'de SQL için Diferansiyel yedekleme zamanı|
| DiffBackupRetentionDuration_s |Ondalık Sayı |v2|Azure VM Yedekleme'de SQL için Diferansiyel yedeklemeler için bekletme süresi|
| LogBackupFrequency_s |Ondalık Sayı |v2|SQL için Günlük yedeklemeleri için sıklık|
| LogBackupRetentionDuration_s |Ondalık Sayı |v2|Azure VM Yedeklemesinde SQL için Günlük yedeklemeleri için bekletme süresi|
| DiffBackupDaysofTheWeek_s |Metin |v2|Azure VM Yedekleme'de SQL için Diferansiyel yedeklemeler için haftanın günleri|
| SourceSystem |Metin ||Geçerli verilerin kaynak sistemi - Azure |
| ResourceId |Metin ||Toplanan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| SubscriptionId |Metin ||Kaynağın abonelik tanımlayıcısı (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceGroup |Metin ||Kaynağın kaynak grubu (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceProvider |Metin ||Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin ||Verilerin toplandığı kaynak türü. Örneğin, Vaults |

### <a name="policyassociation"></a>Politikalar Birliği

Bu tablo, çeşitli kuruluşlarla ilke ilişkileri hakkında ayrıntılı bilgi sağlar.

| Alan | Veri Türü | Geçerli Sürümler | Açıklama |
| --- | --- | --- | --- |
| EventName_s |Metin ||Bu alan bu olayın adını temsil eder, her zaman AzureBackupCentralReport |
| SchemaVersion_s |Metin ||Bu alan şema geçerli sürümünü gösterir, bu **V2** olduğunu |
| State_s |Metin ||İlke nesnesinin geçerli durumu, örneğin, Etkin, Silinmiş |
| BackupManagementType_s |Metin ||Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin, IaaSVM, FileFolder |
| ThrottledRequests |Metin ||Bu alan geçerli işlemin adını temsil eder - PolicyAssociation |
| Kategori |Metin ||Bu alan, Azure Monitor günlüklerine itilen tanılama verileri kategorisini temsil eder, AzureBackupReport'dur |
| Kaynak |Metin ||Bu, verilerin toplandığı kaynaktır, Kurtarma Hizmetleri kasa adını gösterir |
| PolicyUniqueId_g |Metin ||İlkeyi tanımlamak için benzersiz kimlik |
| VaultUniqueId_s |Metin ||Bu iipolitikasın ait olduğu kasanın benzersiz kimliği |
| BackupManagementServerUniqueId_s |Metin |v2 |Yedekleme Yönetim Sunucusu'nu benzersiz bir şekilde tanımlamak için alan, varsa Yedek Öğe tarafından korunur        |
| SourceSystem |Metin ||Geçerli verilerin kaynak sistemi - Azure |
| ResourceId |Metin ||Toplanan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| SubscriptionId |Metin ||Kaynağın abonelik tanımlayıcısı (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceGroup |Metin ||Kaynağın kaynak grubu (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceProvider |Metin ||Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin ||Verilerin toplandığı kaynak türü. Örneğin, Vaults |

### <a name="protected-container"></a>Korumalı Konteyner

Bu tablo, Korumalı Kapsayıcılar hakkında temel alanlar sağlar. (V1'de ProtectedServer oldu)

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| ProtectedContainerUniqueId_s |Metin | Korumalı Kapsayıcıyı benzersiz bir şekilde tanımlamak için alan |
| ProtectedContainerOSType_s |Metin |Korumalı Konteynerin İşletim Sistemi Tipi |
| ProtectedContainerOSVersion_s |Metin |Korumalı Konteynerin İşletim Sistemi Sürümü |
| AgentVersion_s |Metin |Aracı Yedekleme veya Koruma Aracısı sürüm numarası (SC DPM ve MABS durumunda) |
| BackupManagementType_s |Metin |Yedekleme gerçekleştirmek için sağlayıcı türü. Örneğin, IaaSVM, FileFolder |
| EntityState_s |Metin |Korumalı sunucu nesnesinin geçerli durumu. Örneğin, Etkin, Silinmiş |
| ProtectedContainerFriendlyName_s |Metin |Korumalı sunucunun dostu adı |
| ProtectedContainerName_s |Metin |Korunan Konteynerin Adı |
| ProtectedContainerWorkloadType_s |Metin |Korunan Konteyner türü yedeklenir. Örneğin, IaaSVMContainer |
| ProtectedContainerLocation_s |Metin |Korumalı Kapsayıcı'nın şirket içinde mi yoksa Azure'da mı bulunduğu |
| ProtectedContainerType_s |Metin |Korumalı Kapsayıcı sunucu veya kapsayıcı olsun |
| ProtectedContainerProtectionState_s'  |Metin |Korumalı Konteynerin Koruma Durumu |

### <a name="storage"></a>Depolama

Bu tablo, depolama yla ilgili alanlar hakkında ayrıntılı bilgi sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| CloudStorageInBytes_s |Ondalık Sayı |Yedeklemeler tarafından kullanılan, en son değere göre hesaplanan bulut yedekleme depolama alanı (Bu alan yalnızca v1 şeması içindir)|
| ProtectedInstances_s |Ondalık Sayı |Faturalandırmada ön uç depolamasını hesaplamak için kullanılan, en son değere göre hesaplanan korumalı örnek sayısı |
| EventName_s |Metin |Bu alan bu olayın adını temsil eder, her zaman AzureBackupCentralReport |
| SchemaVersion_s |Metin |Bu alan şema geçerli sürümünü gösterir, bu **V2** olduğunu |
| State_s |Metin |Depolama nesnesinin geçerli durumu, örneğin, Etkin, Silinmiş |
| BackupManagementType_s |Metin |Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin, IaaSVM, FileFolder |
| ThrottledRequests |Metin |Bu alan geçerli işlemin adını temsil eder - Depolama |
| Kategori |Metin |Bu alan, Azure Monitor günlüklerine itilen tanılama verileri kategorisini temsil eder, AzureBackupReport'dur |
| Kaynak |Metin |Bu, verilerin toplandığı kaynaktır, Kurtarma Hizmetleri kasa adını gösterir |
| ProtectedServerUniqueId_s |Metin |Depolama alanının hesaplandığı korumalı sunucunun benzersiz kimliği |
| VaultUniqueId_s |Metin |Depolama için kasanın benzersiz kimliği hesaplanır |
| SourceSystem |Metin |Geçerli verilerin kaynak sistemi - Azure |
| ResourceId |Metin |Toplanan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| SubscriptionId |Metin |Kaynağın abonelik tanımlayıcısı (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceGroup |Metin |Kaynağın kaynak grubu (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceProvider |Metin |Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Verilerin toplandığı kaynak türü. Örneğin, Vaults |
| StorageUniqueId_s |Metin |Depolama varlığını tanımlamak için kullanılan benzersiz kimlik |
| StorageType_s |Metin |Depolama Türü, örneğin Bulut, Hacim, Disk |
| StorageName_s |Metin |Depolama varlığının adı, örneğin E:\ |
| StorageTotalSizeInGBs_s |Metin |Depolama varlığı tarafından tüketilen GB'deki toplam depolama boyutu|

### <a name="storageassociation"></a>Depolama Derneği

Bu tablo, depolamayı diğer varlıklara bağlayan depolamayla ilgili temel alanları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- |  --- |
| StorageUniqueId_s |Metin |Depolama varlığını tanımlamak için kullanılan benzersiz kimlik |
| SchemaVersion_s |Metin |Bu alan şema geçerli sürümünü gösterir, bu **V2** olduğunu |
| BackupItemUniqueId_s |Metin |Depolama varlığıyla ilgili yedekleme öğesini tanımlamak için kullanılan benzersiz kimlik |
| BackupManagementServerUniqueId_s |Metin |Depolama varlığıyla ilgili yedekleme yönetim sunucusunu tanımlamak için kullanılan benzersiz kimlik|
| VaultUniqueId_s |Metin |Depolama varlığıyla ilgili kasayı tanımlamak için kullanılan benzersiz kimlik|
| StorageConsumedInMBs_s |Sayı|İlgili depolama alanında ilgili yedekleme maddesi tarafından tüketilen depolama alanının boyutu |
| StorageAllocatedInMBs_s |Sayı |İlgili yedekleme maddesi tarafından, ilgili depolama türü Disk'in depolama alanında ayrılan depolama alanının boyutu|

### <a name="vault"></a>Kasa

Bu tablo, kasayla ilgili alanlar hakkında ayrıntılı bilgi sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| EventName_s |Metin |Bu alan bu olayın adını temsil eder, her zaman AzureBackupCentralReport |
| SchemaVersion_s |Metin |Bu alan şema geçerli sürümünü gösterir, bu **V2** olduğunu |
| State_s |Metin |Kasa nesnesinin geçerli durumu, örneğin, Etkin, Silinmiş |
| ThrottledRequests |Metin |Bu alan geçerli işlemin adını temsil eder - Vault |
| Kategori |Metin |Bu alan, Azure Monitor günlüklerine itilen tanılama verileri kategorisini temsil eder, AzureBackupReport'dur |
| Kaynak |Metin |Bu, verilerin toplandığı kaynaktır, Kurtarma Hizmetleri kasa adını gösterir |
| VaultUniqueId_s |Metin |Kasanın benzersiz kimliği |
| VaultName_s |Metin |Kasanın adı |
| AzureDataCenter_s |Metin |Kasanın bulunduğu veri merkezi |
| StorageReplicationType_s |Metin |Tonoz için depolama çoğaltma türü, örneğin, GeoRedundant |
| SourceSystem |Metin |Geçerli verilerin kaynak sistemi - Azure |
| ResourceId |Metin |Toplanan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| SubscriptionId |Metin |Kaynağın abonelik tanımlayıcısı (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceGroup |Metin |Kaynağın kaynak grubu (ör. Kurtarma Hizmetleri kasası) verilerin toplandığı |
| ResourceProvider |Metin |Verilerin toplandığı kaynak sağlayıcısı. Örneğin, Microsoft.RecoveryServices |
| ResourceType |Metin |Verilerin toplandığı kaynak türü. Örneğin, Vaults |

### <a name="backup-management-server"></a>Yedekleme Yönetim Sunucusu

Bu tablo, Yedekleme Yönetimi Sunucuları hakkında temel alanları sağlar.

|Alan  |Veri Türü  | Açıklama  |
|---------|---------|----------|
|BackupManagementServerName_s     |Metin         |Yedekleme Yönetim Sunucusunun Adı        |
|AzureBackupAgentVersion_s     |Metin         |Yedekleme Yönetim Sunucusundaki Azure Yedekleme Aracısının Sürümü          |
|BackupManagementServerVersion_s     |Metin         |Yedekleme Yönetim Sunucusu Sürümü|
|BackupManagementServerOSVersion_s     |Metin            |Yedekleme Yönetim Sunucusu'nun işletim sistemi sürümü|
|BackupManagementServerType_s     |Metin         |MABS, SC DPM olarak Yedekleme Yönetim Sunucusu nun türü|
|BackupManagementServerUniqueId_s     |Metin         |Yedekleme Yönetim Sunucusu'nu benzersiz bir şekilde tanımlama alanı       |

### <a name="preferredworkloadonvolume"></a>Tercih Edilen İş Yükü

Bu tablo, bir Birim ile ilişkili iş yükü(ler) belirtir.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| StorageUniqueId_s |Metin |Depolama varlığını tanımlamak için kullanılan benzersiz kimlik |
| BackupItemType_s |Metin |Bu birimin tercih edilen depolama alanı olduğu iş yükleri|

### <a name="protectedinstance"></a>KorumaÖrneği

Bu tablo, temel korumalı örneklerle ilgili alanları sağlar.

| Alan | Veri Türü |Geçerli Sürümler | Açıklama |
| --- | --- | --- | --- |
| BackupItemUniqueId_s |Metin |v2|DPM, MABS kullanılarak yedeklenen VM'lerin yedekleme öğesini tanımlamak için kullanılan benzersiz kimlik|
| ProtectedContainerUniqueId_s |Metin |v2|DPM, MABS kullanılarak yedeklenen VM'ler dışında her şey için korumalı kapsayıcıyı tanımlamak için kullanılan benzersiz kimlik|
| ProtectedInstanceCount_s |Metin |v2|İlgili yedekleme öğesi veya korumalı kapsayıcı için o tarih saatinde Korunan Örneklerin Sayısı|

### <a name="recoverypoint"></a>Kurtarma Noktası

Bu tablo temel kurtarma noktası ile ilgili alanları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| BackupItemUniqueId_s |Metin |DPM, MABS kullanılarak yedeklenen VM'lerin yedekleme öğesini tanımlamak için kullanılan benzersiz kimlik|
| OldestRecoveryPointTime_s |Metin |Yedekleme öğesi için en eski kurtarma noktasının tarih saati|
| OldestRecoveryPointLocation_s |Metin |Yedekleme öğesi için en eski kurtarma noktasının konumu|
| LatestRecoveryPointTime_s |Metin |Yedekleme öğesi için en son kurtarma noktasının tarih saati|
| LatestRecoveryPointLocation_s |Metin |Yedekleme öğesi için en son kurtarma noktasının konumu|

## <a name="sample-kusto-queries"></a>Örnek Kusto Sorguları

Aşağıda, Azure Tanılama tablosunda bulunan Azure Yedekleme verilerine sorgu yazmanıza yardımcı olacak birkaç örnek verilmiştir:

- Tüm başarılı yedekleme işleri

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Completed"
    ````

- Tüm başarısız yedekleme işleri

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Failed"
    ````

- Tüm başarılı Azure VM yedekleme işleri

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "VM" and BackupManagementType_s == "IaaSVM"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

- Tüm başarılı SQL günlük yedekleme işleri

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s == "Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "SQLDataBase" and BackupManagementType_s == "AzureWorkload"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

- Tüm başarılı Azure Yedekleme aracı işleri

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "FileFolder" and BackupManagementType_s == "MAB"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````
    
## <a name="next-steps"></a>Sonraki adımlar

Veri modelini gözden geçirdikten sonra, kendi panonuzu oluşturmak için Azure Monitor günlüklerinde [özel sorgular oluşturmaya](../azure-monitor/learn/tutorial-logs-dashboards.md) başlayabilirsiniz.
