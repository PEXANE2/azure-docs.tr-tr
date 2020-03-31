---
title: Azure Yedekleme tanılama olayları için veri modeli
description: Bu veri modeli, Log Analytics'e (LA) tanılama olayları göndermenin Kaynağa Özgü Modu'na başvurur.
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 267753ee1647739e36d92b64f50d8a8be87537d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77583393"
---
# <a name="data-model-for-azure-backup-diagnostics-events"></a>Azure Yedekleme Tanılama Etkinlikleri için Veri Modeli

## <a name="coreazurebackup"></a>CoreAzureYedekleme

Bu tablo, kasalar ve yedekleme öğeleri gibi çekirdek yedekleme varlıkları hakkında bilgi sağlar.

| **Alan**                         | **Veri Türü** | **Açıklama**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ResourceId                        | Metin          | Toplanan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği. |
| ThrottledRequests                     | Metin          | Bu alan geçerli işlemin adını temsil eder - BackupItem, BackupItemAssociation veya ProtectedContainer. |
| Kategori                          | Metin          | Bu alan, Azure Monitor günlüklerine itilen tanılama verileri kategorisini temsil eder. Örneğin, CoreAzureBackup. |
| AgentVersion                      | Metin          | Aracı Yedekleme veya Koruma Aracısı sürüm numarası (SC DPM ve MABS durumunda) |
| AzureBackupAgentVersion           | Metin          | Yedekleme Yönetim Sunucusundaki Azure Yedekleme Aracısının Sürümü |
| AzureDataCenter                   | Metin          | Kasanın bulunduğu veri merkezi                       |
| BackupItemAppVersion              | Metin          | Yedek öğenin uygulama sürümü                       |
| BackupItemFriendlyName            | Metin          | Yedekleme öğesinin dostu adı                             |
| BackupItemName                    | Metin          | Yedek öğenin adı                                      |
| BackupItemProtectionState         | Metin          | Yedekleme Öğesinin Koruma Durumu                          |
| BackupItemFrontEndSize            | Metin          | Yedekleme öğesinin ön uç boyutu                            |
| BackupItemType                    | Metin          | Yedekleme öğesi türü. Örneğin: VM, FileFolder             |
| BackupItemUniqueId                | Metin          | Yedekleme öğesinin benzersiz tanımlayıcısı                         |
| BackupManagementServerType        | Metin          | MABS, SC DPM'de olduğu gibi Yedekleme Yönetim Sunucusunun türü     |
| BackupManagementServerUniqueId    | Metin          | Yedekleme Yönetim Sunucusu'nu benzersiz bir şekilde tanımlama alanı      |
| BackupManagementType              | Metin          | Yedekleme işi yapan sunucu için sağlayıcı türü. Örneğin, IaaSVM, FileFolder |
| BackupManagementServerName        | Metin          | Yedekleme Yönetim Sunucusunun Adı                         |
| BackupManagementServerOSVersion   | Metin          | Yedekleme Yönetim Sunucusu'nun işletim sistemi sürümü                   |
| BackupManagementServerVersion     | Metin          | Yedekleme Yönetim Sunucusu Sürümü                      |
| Son RecoveryPointLocation       | Metin          | Yedekleme öğesi için en son kurtarma noktasının konumu    |
| Son RecoveryPointTime           | DateTime      | Yedekleme öğesi için en son kurtarma noktasının tarih saati   |
| En Eski Kurtarma Noktası Konumu       | Metin          | Yedekleme öğesi için en eski kurtarma noktasının konumu    |
| En Eski Kurtarma Noktası Zamanı           | DateTime      | Yedekleme öğesi için en son kurtarma noktasının tarih saati   |
| PolicyUniqueId                    | Metin          | İlkeyi tanımlamak için benzersiz kimlik                             |
| ProtectedContainerFriendlyName    | Metin          | Korumalı sunucunun dostu adı                        |
| Korumalı Konteyner Konumu        | Metin          | Korumalı Kapsayıcı'nın şirket içinde mi yoksa Azure'da mı bulunduğu |
| Korumalı Konteyner Adı            | Metin          | Korunan Konteynerin Adı                            |
| Korumalı ContainerOSType          | Metin          | Korumalı Konteynerin İşletim Sistemi Tipi                          |
| ProtectedContainerOSVersion       | Metin          | Korumalı Konteynerin İşletim Sistemi Sürümü                        |
| Korumalı KonteynerKoruma Durumu | Metin          | Korumalı Konteynerin Koruma Durumu                  |
| Korumalı Konteyner Tipi            | Metin          | Korumalı Kapsayıcı sunucu veya kapsayıcı olsun  |
| Korumalı KonteynerBenzersizId        | Metin          | DPM, MABS kullanılarak yedeklenen VM'ler dışında her şey için korumalı kapsayıcıyı tanımlamak için kullanılan benzersiz kimlik |
| Korumalı Konteyner İş Yükü Türü    | Metin          | Korunan Konteyner türü yedeklenir. Örneğin, IaaSVMContainer |
| ProtectionGroupName               | Metin          | Koruma Grubunun adı Yedek Öğe, varsa SC DPM ve MABS için korunur |
| ResourceGroupName                 | Metin          | Toplanan veriler için kaynağın kaynak grubu (örneğin, Kurtarma Hizmetleri kasası) |
| ŞemaVersiyon                     | Metin          | Bu alan şema geçerli sürümünü gösterir, bu **V2** olduğunu |
| İkincilYedeklemeKoruma Durumu    | Metin          | Yedekleme öğesi için ikincil korumanın etkin olup olmadığı  |
| Durum                             | Metin          | Yedekleme öğesi nesnesinin durumu. Örneğin, Etkin, Silinmiş |
| DepolamaÇoğaltmaTürü            | Metin          | Kasa için depolama çoğaltma türü. Örneğin, GeoRedundant |
| SubscriptionId                    | Metin          | Verilerin toplandığı kaynağın abonelik tanımlayıcısı (örneğin, Kurtarma Hizmetleri kasası) |
| VaultName                         | Metin          | Kasanın adı                                            |
| VaultEtiketler                         | Metin          | Kasa kaynağıyla ilişkili etiketler                    |
| VaultUniqueId                     | Metin          | Kasanın Benzersiz Tanımlayıcısı                             |
| SourceSystem                      | Metin          | Geçerli verilerin kaynak sistemi - Azure                  |

## <a name="addonazurebackupalerts"></a>AddonAzureBackupAlerts

Bu tablo, uyarı ile ilgili alanlar hakkında ayrıntılı bilgi sağlar.

| **Alan**                      | **Veri Türü** | **Açıklama**                                              |
| :----------------------------- | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Metin          | Hangi verilerin toplandığı hakkında kaynak için benzersiz tanımlayıcı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| ThrottledRequests                  | Metin          | Geçerli işlemin adı. Örneğin, Uyarı            |
| Kategori                       | Metin          | Azure Monitor günlüklerine itilen tanılama verileri kategorisi - AddonAzureBackupAlerts |
| AlertCode                      | Metin          | Bir uyarı türünü benzersiz olarak tanımlamak için kod                     |
| AlertConsolidationStatus       | Metin          | Uyarının birleştirilmiş bir uyarı olup olmadığını belirleme         |
| AlertOccurrenceDateTime        | DateTime      | Uyarının oluşturulduğu tarih ve saat                     |
| AlertRaisedOn                  | Metin          | Uyarının yükseltildiği varlık türü                        |
| AlertSeverity                  | Metin          | Uyarının şiddeti. Örneğin, Kritik                 |
| AlertStatus                    | Metin          | Uyarının durumu. Örneğin, Etkin                     |
| AlertTimeToResolveinMinutes    | Sayı        | Bir uyarıyı çözmek için zaman alınır. Etkin uyarılar için boş.     |
| Alerttype                      | Metin          | Alarm türü. Örneğin, Yedekleme                           |
| AlertUniqueId                  | Metin          | Oluşturulan uyarının benzersiz tanımlayıcısı                    |
| BackupItemUniqueId             | Metin          | Uyarıyla ilişkili yedekleme öğesinin benzersiz tanımlayıcısı |
| BackupManagementServerUniqueId | Metin          | Yedekleme Yönetim Sunucusu'nu benzersiz bir şekilde tanımlamak için alan, varsa Yedek Öğe tarafından korunur |
| BackupManagementType           | Metin          | Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin, IaaSVM, FileFolder |
| CountOfAlertsKonsolide      | Sayı        | Birleştirilmiş bir uyarıysa birleştirilmiş uyarı sayısı  |
| Korumalı KonteynerBenzersizId     | Metin          | Uyarıyla ilişkili korumalı sunucunun benzersiz tanımlayıcısı |
| Önerilen Eylem              | Metin          | Uyarıyı gidermek için önerilen eylem                      |
| ŞemaVersiyon                  | Metin          | Şemanın geçerli sürümü, örneğin **V2**            |
| Durum                          | Metin          | Uyarı nesnesinin geçerli durumu, örneğin, Etkin, Silinmiş |
| DepolamaBenzersizId                | Metin          | Depolama varlığını tanımlamak için kullanılan benzersiz kimlik                |
| VaultUniqueId                  | Metin          | Uyarıyla ilgili kasayı tanımlamak için kullanılan benzersiz kimlik    |
| SourceSystem                   | Metin          | Geçerli verilerin kaynak sistemi - Azure                    |

## <a name="addonazurebackupprotectedinstance"></a>AddonAzureBackupProtectedInstance

Bu tablo, temel korumalı örneklerle ilgili alanları sağlar.

| **Alan**                      | **Veri Türü** | **Açıklama**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Metin          | Hangi verilerin toplandığı hakkında kaynak için benzersiz tanımlayıcı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| ThrottledRequests                  | Metin          | Örneğin ProtectedInstance gibi işlemin adı         |
| Kategori                       | Metin          | Azure Monitor günlüklerine itilen tanılama verileri kategorisi - AddonAzureBackupProtectedInstance |
| BackupItemUniqueId             | Metin          | Yedekleme öğesinin benzersiz kimliği                                 |
| BackupManagementServerUniqueId | Metin          | Yedekleme Yönetim Sunucusu'nu benzersiz bir şekilde tanımlamak için alan, varsa Yedek Öğe tarafından korunur |
| BackupManagementType           | Metin          | Yedekleme işi yapan sunucu için sağlayıcı türü, örneğin, IaaSVM, FileFolder |
| Korumalı KonteynerBenzersizId     | Metin          | İşin çalıştırılacağı korumalı kapsayıcıyı tanımlamak için benzersiz kimlik |
| KorumalıInstanceCount         | Metin          | İlgili yedekleme öğesi veya korumalı kapsayıcı için o tarih saatinde Korunan Örneklerin Sayısı |
| ŞemaVersiyon                  | Metin          | Şemanın geçerli sürümü, örneğin **V2**            |
| Durum                          | Metin          | Yedek öğe nesnesinin durumu, örneğin, Etkin, Silinmiş |
| VaultUniqueId                  | Metin          | Korunan örnekle ilişkili korumalı kasanın benzersiz tanımlayıcısı |
| SourceSystem                   | Metin          | Geçerli verilerin kaynak sistemi - Azure                    |

## <a name="addonazurebackupjobs"></a>AddonAzureBackupJobs

Bu tablo, işle ilgili alanlar hakkında ayrıntılı bilgi sağlar.

| **Alan**                      | **Veri Türü** | **Açıklama**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Metin          | Toplanan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| ThrottledRequests                  | Metin          | Bu alan geçerli işlemin adını temsil eder - İş    |
| Kategori                       | Metin          | Bu alan, Azure Monitor günlüklerine itilen tanılama verileri kategorisini temsil eder - AddonAzureBackupJobs |
| AdhocOrScheduledJob            | Metin          | İşin Geçici mi yoksa Zamanlanmış mı olduğunu belirtmek için alan           |
| BackupItemUniqueId             | Metin          | Depolama varlığıyla ilgili yedekleme öğesini tanımlamak için kullanılan benzersiz kimlik |
| BackupManagementServerUniqueId | Metin          | Depolama varlığıyla ilgili yedekleme yönetim sunucusunu tanımlamak için kullanılan benzersiz kimlik |
| BackupManagementType           | Metin          | Yedekleme gerçekleştirmek için sağlayıcı türü, örneğin, IaaSVM, FileFolder hangi bu uyarı ait |
| DataTransferInMB            | Sayı        | Bu iş için MB'ye aktarılan veriler                          |
| JobDurationInSecs              | Sayı        | Saniye cinsinden toplam iş süresi                                |
| İşFailureKodu                 | Metin          | İş hatası nedeniyle Hata Kodu dizesi    |
| İş Operasyonu                   | Metin          | Örneğin hangi işin çalıştırıldığı için çalışma, Yedekleme, Geri Yükleme, Yedekleme yi Yapılandırma |
| İşİşlemi Alt Türü            | Metin          | İş İşleminin Alt Türü. Örneğin, 'Log', Günlük Yedekleme İşi durumunda |
| JobStartDateTime               | DateTime      | İşin çalışmaya başladığı tarih ve saat                       |
| JobStatus                      | Metin          | Bitmiş işin durumu, örneğin, Tamamlanmış, Başarısız   |
| JobUniqueId                    | Metin          | İşi tanımlamak için benzersiz kimlik                                |
| Korumalı KonteynerBenzersizId     | Metin          | Uyarıyla ilişkili korumalı sunucunun benzersiz tanımlayıcısı |
| KurtarmaJobDestination         | Metin          | Verilerin kurtarıldığı kurtarma işinin hedefi   |
| RecoveryJobRPDateTime          | DateTime      | Kurtarılan kurtarma noktasının oluşturulduğu tarih, saat |
| Kurtarmaİş Konumu            | Metin          | Kurtarılan kurtarma noktasının depolandığı konum |
| Kurtarma Konum Türü           | Metin          | Kurtarma Konumu Türü                                |
| ŞemaVersiyon                  | Metin          | Şemanın geçerli sürümü, örneğin **V2**            |
| Durum                          | Metin          | Uyarı nesnesinin geçerli durumu, örneğin, Etkin, Silinmiş |
| VaultUniqueId                  | Metin          | Uyarı yla ilişkili korumalı kasanın benzersiz tanımlayıcısı |
| SourceSystem                   | Metin          | Geçerli verilerin kaynak sistemi - Azure                    |

## <a name="addonazurebackuppolicy"></a>AddonAzureBackupPolicy

Bu tablo, ilke ile ilgili alanlar hakkında ayrıntılı bilgi sağlar.

| **Alan**                       | **Veri Türü**  | **Açıklama**                                              |
| ------------------------------- | -------------- | ------------------------------------------------------------ |
| ResourceId                      | Metin           | Hangi verilerin toplandığı hakkında kaynak için benzersiz tanımlayıcı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| ThrottledRequests                   | Metin           | Operasyonun adı, örneğin, Politika veya Politika Derneği |
| Kategori                        | Metin           | Azure Monitor günlüklerine itilen tanılama verileri kategorisi - AddonAzureBackupPolicy |
| BackupDaysOfTheWeek             | Metin           | Yedeklemelerin zamanlandığı haftanın günleri            |
| Yedekleme Frekansı                 | Metin           | Yedeklerin çalıştırılan sıklık. Örneğin, günlük, haftalık |
| BackupManagementType            | Metin           | Yedekleme işi yapan sunucu için sağlayıcı türü. Örneğin, IaaSVM, FileFolder |
| BackupManagementServerUniqueId  | Metin           | Yedekleme Yönetim Sunucusu'nu benzersiz bir şekilde tanımlamak için alan, varsa Yedek Öğe tarafından korunur |
| Yedekleme Süreleri                     | Metin           | Yedeklemelerin zamanlandığı tarih ve saat                     |
| Günlük TutmaSüresi          | Tam Sayı   | Yapılandırılmış yedeklemeler için gün içinde toplam bekletme süresi      |
| DailyRetentionTimes             | Metin           | Günlük bekletme yapılandırıldığı tarih ve saat            |
| DiffBackupDaysOfTheWeek         | Metin           | Azure VM Yedekleme'de SQL için Diferansiyel yedeklemeler için haftanın günleri |
| DiffBackupFormat                | Metin           | Azure VM yedeklemesinde SQL için Diferansiyel yedeklemebiçimleri   |
| DiffBackup Bekletme Süresi     | Ondalık Sayı | Azure VM Yedekleme'de SQL için Diferansiyel yedeklemeler için bekletme süresi |
| DiffBackupTime                  | Zaman           | Azure VM Yedekleme'de SQL için Diferansiyel yedekleme zamanı     |
| LogBackupFrequency              | Ondalık Sayı | SQL için Günlük yedeklemeleri için sıklık                            |
| LogBackupRetentionDuration      | Ondalık Sayı | Azure VM Yedeklemesinde SQL için Günlük yedeklemeleri için bekletme süresi |
| Aylık TutmaGünleri  | Metin           | Aylık bekletme yapılandırıldığı ayın haftaları.  Örneğin, İlk, Son, vb. |
| Haftalık BekletmeGünleri   | Metin           | Aylık saklama için seçilen haftanın günleri              |
| Aylık TutmaSüresi        | Metin           | Yapılandırılmış yedeklemeler için aylar daki toplam bekletme süresi    |
| Aylık Tutma Biçimi          | Metin           | Aylık bekletme için yapılandırma türü. Örneğin, günlük tabanlı günlük, haftalık tabanlı haftalık |
| Aylık Tutma Süreleri           | Metin           | Aylık bekletme yapılandırıldığı tarih ve saat           |
| Aylık Tutma Haftası | Metin           | Aylık bekletme yapılandırıldığı ayın haftaları.   Örneğin, İlk, Son, vb. |
| PolicyName                      | Metin           | Tanımlanan ilkenin adı                                   |
| PolicyUniqueId                  | Metin           | İlkeyi tanımlamak için benzersiz kimlik                             |
| PolicyTimeZone                  | Metin           | İlke Zaman Alanlarının günlüklerde belirtildiği saat dilimi |
| Bekletme Süresi               | Metin           | Yapılandırılmış yedeklemeler için bekletme süresi                    |
| Bekletme Türü                   | Metin           | Bekletme türü                                            |
| ŞemaVersiyon                   | Metin           | Bu alan şema geçerli sürümünü gösterir, bu **V2** olduğunu |
| Durum                           | Metin           | İlke nesnesinin geçerli durumu. Örneğin, Etkin, Silinmiş |
| SenkronizasyonFrequencyPerDay  | Tam Sayı   | SC DPM ve MABS için bir dosya yedeklemesi günde kaç kez eşitlenir |
| VaultUniqueId                   | Metin           | Bu iipola ait olan kasanın benzersiz kimliği          |
| Haftalık TutmaGünleriOfTheWeek    | Metin           | Haftalık bekletme için seçilen haftanın günleri               |
| Haftalık BekletmeSüresi         | Ondalık Sayı | Yapılandırılmış yedeklemeler için haftalar içinde toplam haftalık bekletme süresi |
| Haftalık Tutma Süreleri            | Metin           | Haftalık bekletme yapılandırıldığı tarih ve saat            |
| YıllıkTutmaDaysOfTheMonth   | Metin           | Yıllık bekletme için seçilen ayın tarihleri             |
| YıllıkTutmaDaysOfTheWeek    | Metin           | Yıllık bekletme için seçilen haftanın günleri               |
| Yıllık TutmaSüresi         | Ondalık Sayı | Yapılandırılmış yedeklemeler için yıllar içinde toplam bekletme süresi     |
| Yıllık RestansiyonBiçimi           | Metin           | Yıllık bekletme için yapılandırma türü, örneğin, günlük tabanlı günlük, haftalık haftalık tabanlı |
| Yıllık TutmaMonthsOfTheYear  | Metin           | Yıllık bekletme için seçilen yılın ayları             |
| Yıllık TutmaTimes            | Metin           | Yıllık bekletme yapılandırıldığı tarih ve saat            |
| Yıllık TutmaWeeksOfTheMonth  | Metin           | Yıllık bekletme için seçilen ayın haftaları             |
| SourceSystem                    | Metin           | Geçerli verilerin kaynak sistemi - Azure                    |

## <a name="addonazurebackupstorage"></a>AddonAzureBackupDepolama

Bu tablo, depolama yla ilgili alanlar hakkında ayrıntılı bilgi sağlar.

| **Alan**                      | **Veri Türü** | **Açıklama**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Metin          | Toplanan veriler için kaynak tanımlayıcısı. Örneğin, Kurtarma Hizmetleri kasa kaynak kimliği |
| ThrottledRequests                  | Metin          | Bu alan geçerli operasyonun adını temsil eder - Depolama veya Depolama Birliği |
| Kategori                       | Metin          | Bu alan, Azure Monitor günlüklerine itilen tanılama verileri kategorisini temsil eder - AddonAzureBackupStorage |
| BackupItemUniqueId             | Metin          | DPM, MABS kullanılarak yedeklenen VM'lerin yedekleme öğesini tanımlamak için kullanılan benzersiz kimlik |
| BackupManagementServerUniqueId | Metin          | Yedekleme Yönetim Sunucusu'nu benzersiz bir şekilde tanımlamak için alan, varsa Yedek Öğe tarafından korunur |
| BackupManagementType           | Metin          | Yedekleme işi yapan sunucu için sağlayıcı türü. Örneğin, IaaSVM, FileFolder |
| Tercih Edilen İş Yükü      | Metin          | Bu birimin tercih edilen depolama alanı olduğu iş yükü      |
| Korumalı KonteynerBenzersizId     | Metin          | Uyarıyla ilişkili korumalı sunucunun benzersiz tanımlayıcısı |
| ŞemaVersiyon                  | Metin          | Şema versiyonu. Örneğin, **V2**                   |
| Durum                          | Metin          | Yedekleme öğesi nesnesinin durumu. Örneğin, Etkin, Silinmiş |
| DepolamaTahsisInMBs          | Sayı        | İlgili yedekleme maddesi tarafından, ilgili depolama türü Disk'in depolama alanında ayrılan depolama alanının boyutu |
| DepolamaConsumedInMBs           | Sayı        | İlgili depolama alanında ilgili yedekleme maddesi tarafından tüketilen depolama alanının boyutu |
| Depolama Adı                    | Metin          | Depolama varlığının adı. Örneğin, E:\                      |
| DepolamaTotalSizeInGBs          | Metin          | Depolama varlığı tarafından tüketilen GB'deki toplam depolama boyutu     |
| Depolama Türü                    | Metin          | Depolama Türü, örneğin Bulut, Hacim, Disk             |
| DepolamaBenzersizId                | Metin          | Depolama varlığını tanımlamak için kullanılan benzersiz kimlik                |
| VaultUniqueId                  | Metin          | Depolama varlığıyla ilgili kasayı tanımlamak için kullanılan benzersiz kimlik |
| VolumeFriendlyName             | Metin          | Depolama hacminin dostu adı                          |
| SourceSystem                   | Metin          | Geçerli verilerin kaynak sistemi - Azure                    |

## <a name="next-steps"></a>Sonraki adımlar

- [Tanılama verilerini Log Analytics'e nasıl göndereceğiz öğrenin](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events)
- [Kaynak belirli tablolarda sorgu yazmayı öğrenin](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor#sample-kusto-queries)