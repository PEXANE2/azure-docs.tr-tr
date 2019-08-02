---
title: Azure Backup için veri modeli
description: Bu makalede, Azure Backup raporlarının Power BI veri modeli ayrıntıları ele bir şekilde konuşuyor.
ms.reviewer: adigan
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/26/2017
ms.author: dacurwin
ms.openlocfilehash: 53051c88d5723f61fa08e431bdf7ab531e88123e
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68689144"
---
# <a name="data-model-for-azure-backup-reports"></a>Azure Backup raporları için veri modeli
Bu makalede Azure Backup rapor oluşturmak için kullanılan Power BI veri modeli açıklanmaktadır. Bu veri modelini kullanarak, mevcut raporları ilgili alanlara göre filtreleyebilir ve daha da önemlisi, modeldeki tabloları ve alanları kullanarak kendi raporlarınızı oluşturabilirsiniz. 

## <a name="creating-new-reports-in-power-bi"></a>Power BI yeni raporlar oluşturma
Power BI [veri modelini kullanarak rapor oluşturabileceğiniz](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)özelleştirme özellikleri sağlar.

## <a name="using-azure-backup-data-model"></a>Azure Backup veri modelini kullanma
Rapor oluşturmak ve mevcut raporları özelleştirmek için veri modelinin bir parçası olarak sunulan aşağıdaki alanları kullanabilirsiniz.

### <a name="alert"></a>Uyarı
Bu tablo, uyarı ile ilgili çeşitli alanlarla ilgili temel alanları ve toplamaları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Tam sayı |Seçilen dönemde oluşturulan uyarı sayısı |
| % Activealertscreatedınperiod |Yüzde |Seçilen dönemdeki etkin uyarıların yüzdesi |
| % Criticalalertscreatedınperiod |Yüzde |Seçilen dönemdeki kritik uyarıların yüzdesi |
| AlertOccurrenceDate |Date |Uyarının oluşturulduğu tarih |
| AlertSeverity |Text |Örneğin kritik, uyarının önem derecesi |
| AlertStatus |Text |Örneğin etkin olduğu uyarının durumu |
| AlertType |Text |Oluşturulan uyarının türü, örneğin, yedekleme |
| Alertuniqueıd |Text |Oluşturulan uyarının benzersiz kimliği |
| AsOnDateTime |Date/Time |Seçili satır için en son yenileme zamanı |
| Avgresolutiontimeınminsforalertscreatedınperiod |Ondalık sayı |Seçilen zaman aralığı için uyarıyı çözümlemek için geçen ortalama süre (dakika cinsinden) |
| EntityState |Text |Uyarı nesnesinin geçerli durumu, örneğin, etkin, silindi |

### <a name="backup-item"></a>Yedekleme öğesi
Bu tablo, çeşitli yedekleme öğesiyle ilgili alanlarla ilgili temel alanları ve toplamaları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #BackupItems |Tam sayı |Yedekleme öğesi sayısı |
| #UnprotectedBackupItems |Tam sayı |Koruma için durdurulan veya yedeklemeler için yapılandırılan yedekleme öğelerinin sayısı ancak yedeklemeler başlatılmadı|
| AsOnDateTime |Date/Time |Seçili satır için en son yenileme zamanı |
| Backupıtemfriendlyname |Text |Yedekleme öğesinin kolay adı |
| Backupıtemıd |Text |Yedekleme öğesi kimliği |
| Backupıtemname |Text |Yedekleme öğesinin adı |
| Backupıtemtype |Text |Yedekleme öğesinin türü örneğin, VM, dosya klasörü |
| EntityState |Text |Yedekleme öğesi nesnesinin geçerli durumu Örneğin, etkin, silindi |
| LastBackupDateTime |Date/Time |Seçili yedekleme öğesi için son yedekleme zamanı |
| LastBackupState |Text |Seçili yedekleme öğesi için son yedeklemenin durumu, örneğin, başarılı, başarısız |
| Lastbaşarılı Fulbackupdatetime |Date/Time |Seçili yedekleme öğesi için son başarılı yedeklemenin saati |
| ProtectionState |Text |Yedekleme öğesinin geçerli koruma durumu Örneğin, korumalı, Protectiondurduruldu |

### <a name="calendar"></a>Takvim
Bu tablo, takvimle ilgili alanlarla ilgili ayrıntıları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| Date |Date |Verilerin filtrelenmesi için seçilen tarih |
| DateKey |Text |Her bir tarih öğesi için benzersiz anahtar |
| DayDiff |Ondalık sayı |Verilerin filtrelenmesi için gün farkı Örneğin, 0 geçerli günün verilerini,-1 önceki bir günün verilerini gösterir, 0 ve-1 geçerli ve önceki günün verilerini gösterir  |
| Ay |Text |Verilerin filtrelenmesi için seçilen yılın ayı, ayın ilk gününde başlaması ve 31. gün tarihinde sona erecek |
| MonthDate | Date |Ayın bitiş tarihi, verilerin filtrelenmesi için seçildi |
| MonthDiff |Ondalık sayı |Verilerin filtrelenmesi için aydaki fark Örneğin, 0 geçerli aya ait verileri gösterir,-1 önceki aya ait verileri gösterir, 0 ve-1 geçerli ve önceki aya ait verileri gösterir |
| Hafta |Text |Verilerin filtrelenmesi için seçilen hafta, hafta Pazar günü başlar ve Cumartesi tarihinde sona erer |
| WeekDate |Date |Haftalık bitiş tarihi, verilerin filtrelenmesi için seçildi |
| WeekDiff |Ondalık sayı |Verilerin filtrelenmesi için haftanın farkı (örneğin, 0) geçerli haftanın verilerini gösterir,-1 önceki haftaki verileri gösterir, 0 ve-1 geçerli ve önceki hafta verilerini gösterir |
| Yıl |Text |Verilerin filtrelenmesi için takvim yılı seçildi |
| Yıltarihi |Date |Yılın bitiş tarihinden sonra, verileri filtrelemek için seçilen yıl |

### <a name="job"></a>İş
Bu tablo, işle ilgili çeşitli alanlar üzerinde temel alanları ve toplamaları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #JobsCreatedInPeriod |Tam sayı |Seçilen dönemde oluşturulan iş sayısı |
| % Failuıresforjobscreatedınperiod |Yüzde |Seçilen dönemdeki toplam iş hatalarının yüzdesi |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Ondalık sayı |Seçilen dönemde oluşturulan **yedekleme** ışlerı için MB cinsinden aktarılan verilerin 500. yüzdebirlik değeri |
| AsOnDateTime |Date/Time |Seçili satır için en son yenileme zamanı |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Ondalık sayı |Seçilen dönemde oluşturulan **Tamamlanan yedekleme** işlerinin ortalama süresi (dakika) |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Ondalık sayı |Seçilen dönemde oluşturulan **Tamamlanan geri yükleme** işlerinin ortalama süresi (dakika) |
| BackupStorageDestination |Text |Yedekleme depolamanın hedefi Örneğin, bulut, disk  |
| EntityState |Text |İş nesnesinin geçerli durumu, örneğin, etkin, silindi |
| JobFailureCode |Text |İş hatası oluştuğundan hata kodu dizesi |
| JobOperation |Text |İşin çalıştırıldığı işlem Örneğin, yedekleme, geri yükleme, yedeklemeyi yapılandırma |
| JobStartDate |Date |İşin çalışmaya başladığı tarih |
| JobStartTime |Time |İşin çalışmaya başladığı zaman |
| JobStatus |Text |Tamamlanan işin durumu Örneğin tamamlandı, başarısız |
| Jobuniqueıd |Text |İşi tanımlamak için benzersiz kimlik |

### <a name="policy"></a>İlke
Bu tablo, ilkeyle ilgili çeşitli alanlarla ilgili temel alanları ve toplamaları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #Policies |Tam sayı |Sistemde var olan yedekleme ilkelerinin sayısı |
| #PoliciesInUse |Tam sayı |Yedeklemeleri yapılandırmak için şu anda kullanılan ilke sayısı |
| AsOnDateTime |Date/Time |Seçili satır için en son yenileme zamanı |
| Backupdayısoftheweek |Text |Yedeklemelerin zamanlandığı haftanın günleri |
| BackupFrequency |Text |Yedeklemelerin çalıştırıldığı sıklık (örneğin, günlük, haftalık) |
| BackupTimes |Text |Yedeklemelerin zamanlandığı tarih ve saat |
| DailyRetentionDuration |Tam sayı |Yapılandırılan yedeklemeler için gün cinsinden toplam saklama süresi |
| DailyRetentionTimes |Text |Günlük bekletmenin yapılandırıldığı tarih ve saat |
| EntityState |Text |İlke nesnesinin geçerli durumu, örneğin, etkin, silindi |
| MonthlyRetentionDaysOfTheMonth |Text |Aylık saklama için seçilen ayın tarihleri |
| MonthlyRetentionDaysOfTheWeek |Text |Aylık saklama için seçilen haftanın günleri |
| MonthlyRetentionDuration |Ondalık sayı |Yapılandırılan yedeklemeler için aylık toplam bekletme süresi |
| MonthlyRetentionFormat |Text |Aylık saklama için yapılandırma türü; günlük tabanlı, haftalık, haftalık tabanlı |
| MonthlyRetentionTimes |Text |Aylık bekletmenin yapılandırıldığı tarih ve saat |
| MonthlyRetentionWeeksOfTheMonth |Text |Aylık bekletme yapılandırıldığında Örneğin, Ilk, son vb. ayın haftası |
| PolicyName |Text |Tanımlanan ilkenin adı |
| Policyuniqueıd |Text |İlkeyi tanımlamak için benzersiz kimlik |
| RetentionType |Text |Örneğin günlük, haftalık, aylık, yıllık bekletme ilkesi türü |
| WeeklyRetentionDaysOfTheWeek |Text |Haftalık saklama için seçilen haftanın günleri |
| WeeklyRetentionDuration |Ondalık sayı |Yapılandırılan yedeklemeler için hafta cinsinden toplam haftalık saklama süresi |
| WeeklyRetentionTimes |Text |Haftalık bekletmenin yapılandırıldığı tarih ve saat |
| YearlyRetentionDaysOfTheMonth |Text |Yıllık bekletme için seçilen ayın tarihleri |
| YearlyRetentionDaysOfTheWeek |Text |Yıllık bekletme için seçilen haftanın günleri |
| YearlyRetentionDuration |Ondalık sayı |Yapılandırılan yedeklemeler için yıl cinsinden toplam saklama süresi |
| YearlyRetentionFormat |Text |Yıllık bekletme için yapılandırma türü; günlük tabanlı, haftalık, haftalık tabanlı |
| YearlyRetentionMonthsOfTheYear |Text |Yıllık bekletme için seçilen yılın ayı |
| YearlyRetentionTimes |Text |Yıllık bekletmenin yapılandırıldığı tarih ve saat |
| YearlyRetentionWeeksOfTheMonth |Text |Yıllık bekletme yapılandırıldığında Örneğin, Ilk, son vb. ayın haftası |

### <a name="protected-server"></a>Korumalı sunucu
Bu tablo, korumalı sunucu ile ilgili çeşitli alanlar üzerinde temel alanları ve toplamaları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #ProtectedServers |Tam sayı |Korumalı sunucu sayısı |
| AsOnDateTime |Date/Time |Seçili satır için en son yenileme zamanı |
| AzureBackupAgentOSType |Text |Azure Backup aracısının işletim sistemi türü |
| AzureBackupAgentOSVersion |Text |Azure Backup aracısının işletim sistemi sürümü |
| AzureBackupAgentUpdateDate |Text |Aracı Yedekleme aracısının güncelleştirildiği tarih |
| AzureBackupAgentVersion |Text |Aracı yedekleme sürümünün sürüm numarası |
| BackupManagementType |Text |Yedekleme gerçekleştirmek için sağlayıcı türü örneğin ıaasvm, FileFolder |
| EntityState |Text |Korumalı sunucu nesnesinin geçerli durumu Örneğin, etkin, silindi |
| ProtectedServerFriendlyName |Text |Korumalı sunucunun kolay adı |
| ProtectedServerName |Text |Korumalı sunucunun adı |
| ProtectedServerType |Text |Yedeklenen korumalı sunucu türü örneğin ıaasvmcontainer |
| ProtectedServerName |Text |Yedekleme öğesinin ait olduğu korumalı sunucunun adı |
| RegisteredContainerId |Text |Yedekleme için kaydedilen kapsayıcının kimliği |

### <a name="storage"></a>Depolama
Bu tablo, depolama ile ilgili çeşitli alanlar üzerinde temel alanları ve toplamaları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #ProtectedInstances |Ondalık sayı |Seçili zamanda en son değere göre hesaplanan, faturalandırma aşamasında ön uç depolamayı hesaplamak için kullanılan korumalı örnek sayısı |
| AsOnDateTime |Date/Time |Seçili satır için en son yenileme zamanı |
| Cloudstorageınmb |Ondalık sayı |Seçilen zamanda en son değere göre hesaplanan yedeklemeler tarafından kullanılan bulut yedekleme depolaması |
| EntityState |Text |Nesnenin geçerli durumu Örneğin, etkin, silindi |
| LastUpdatedDate |Date |Seçili satırın son güncelleştirilme tarihi |

### <a name="time"></a>Time
Bu tablo, zaman ile ilgili alanlarla ilgili ayrıntıları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| Saat |Time |Günün saati Örneğin, 1:00:00 PM |
| HourNumber |Ondalık sayı |Günün saat numarası örneğin, 13,00 |
| Dakika |Ondalık sayı |Saatin dakikası |
| PeriodOfTheDay |Text |Günün zaman dilimi yuvası Örneğin, 12-3 |
| Time |Time |Günün saati Örneğin, 12:00:01 |
| TimeKey |Text |Saati temsil eden anahtar değer |

### <a name="vault"></a>Kasa
Bu tablo, kasala ilgili çeşitli alanlarla ilgili temel alanları ve toplamaları sağlar.

| Alan | Veri Türü | Açıklama |
| --- | --- | --- |
| #Vaults |Tam sayı |Kasaların sayısı |
| AsOnDateTime |Date/Time |Seçili satır için en son yenileme zamanı |
| AzureDataCenter |Text |Kasanın bulunduğu veri merkezi |
| EntityState |Text |Kasa nesnesinin geçerli durumu, örneğin, etkin, silindi |
| StorageReplicationType |Text |Kasa için depolama çoğaltma türü; coğrafi olarak yedekli |
| SubscriptionId |Text |Rapor oluşturmak için seçilen müşterinin abonelik kimliği |
| VaultName |Text |Kasasının adı |
| VaultTags |Text |Kasayla ilişkili Etiketler |

## <a name="next-steps"></a>Sonraki adımlar
Azure Backup raporları oluşturmak için veri modelini gözden geçirdikten sonra, Power BI rapor oluşturma ve görüntüleme hakkında daha fazla bilgi için aşağıdaki makalelere bakın.

* [Power BI rapor oluşturma](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Power BI raporları filtreleme](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
