---
title: Azure portal Azure Active Directory Kullanıcı etkinliği raporlarını bul | Microsoft Docs
description: Azure Active Directory Kullanıcı etkinliği raporlarının nerede Azure portal olduğunu öğrenin.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 356412bfca19e72d09338faf8c8bcac5ec8f273a
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68988297"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Azure portal etkinlik raporlarını bulma

Bu makalede, Azure portal Azure Active Directory (Azure AD) Kullanıcı etkinliği raporlarının nasıl bulunacağını öğreneceksiniz.

## <a name="audit-logs-report"></a>Denetim günlükleri raporu

Denetim günlükleri raporu, uygulama etkinlikleri etrafında çeşitli raporları bağlam tabanlı raporlama için tek bir görünümde birleştirir. Denetim günlükleri raporuna erişmek için:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sağ üst köşedeki dizininizi seçin, sonra sol gezinti bölmesinden **Azure Active Directory** dikey penceresini seçin.
3. Azure Active Directory dikey penceresinin **etkinlik** bölümünden **Denetim günlükleri** ' ni seçin. 

    ![Denetim günlükleri](./media/howto-find-activity-reports/482.png "Denetim günlükleri")

Denetim günlükleri raporu aşağıdaki raporları birleştirir:

* Denetim raporu
* Parola sıfırlama etkinliği
* Parola sıfırlama kayıt etkinliği
* Self servis grupları etkinliği
* Office365 grup adı değişiklikleri
* Hesap sağlama etkinliği
* Parola geçiş durumu
* Hesap hazırlama hataları

### <a name="filtering-on-audit-logs"></a>Denetim günlüklerinde filtreleme

Denetim verileri kategori filtresinde belirterek belirli bir denetim verileri kategorisine erişmek için denetim raporundaki Gelişmiş filtreleme ' yi kullanabilirsiniz. Örneğin, kullanıcılarla ilgili tüm etkinlikleri görüntülemek için **Usermanagement** kategorisini seçin. 

Kategoriler şunlardır:

- Tümü
- AdministrativeUnit
- ApplicationManagement
- Authentication
- Authorization
- İlgili kişi
- Cihaz
- DeviceConfiguration
- DirectoryManagement
- EntitlementManagement
- GroupManagement
- Diğer
- İlke
- ResourceManagement
- RoleManagement
- Kullanıcı Yönetimi

Ayrıca **hizmet** açılan filtresini kullanarak belirli bir hizmete filtre uygulayabilirsiniz. Örneğin, self servis parola yönetimiyle ilgili tüm denetim olaylarını almak için **self servis parola yönetimi** filtresini seçin.

Hizmetlere şunlar dahildir:

- Tümü
- Erişim Gözden Geçirmeleri
- Hesap Sağlama 
- Uygulama SSO 'SU
- Kimlik Doğrulaması Yöntemleri
- B2C
- Koşullu Erişim
- Çekirdek Dizin
- Yetkilendirme Yönetimi
- Identity Protection
- Davetli Kullanıcılar
- PIM
- Self Servis Grup Yönetimi
- Self Servis Parola Yönetimi
- Kullanım Koşulları

## <a name="sign-ins-report"></a>Oturum açma işlemleri raporu 

**Oturum açma** görünümü, tüm Kullanıcı oturum açma işlemlerinin yanı sıra **uygulama kullanımı** raporunu içerir. Ayrıca, uygulama kullanım bilgilerini **kurumsal uygulamalara** genel bakış ' ın **Yönet** bölümünde görüntüleyebilirsiniz.

Oturum açma raporuna erişmek için:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sağ üst köşedeki dizininizi seçin, sonra sol gezinti bölmesinden **Azure Active Directory** dikey penceresini seçin.
3. Azure Active Directory dikey penceresinin **etkinlik** bölümünden **signins** ' i seçin. 

    ![Oturum açma işlemleri görünümü](./media/howto-find-activity-reports/483.png "Oturum açma işlemleri görünümü")


### <a name="filtering-on-application-name"></a>Uygulama adında filtreleme

Kullanıcı adına veya uygulama adına filtreleyerek uygulama kullanımı hakkındaki ayrıntıları görüntülemek için oturum açma işlemleri raporunu kullanabilirsiniz.

![Oturum açma olaylarını filtrele sayfası](./media/howto-find-activity-reports/07.png "Oturum açma olaylarını filtrele sayfası")

## <a name="security-reports"></a>Güvenlik raporları

### <a name="anomalous-activity-reports"></a>Anormal etkinlik raporları

Anormal etkinlik raporları, Azure AD 'nin tespit ve rapor etbildiği güvenlikle ilgili risk olayları hakkında bilgi sağlar.

Aşağıdaki tabloda Azure AD anormal etkinlik güvenlik raporları ve Azure Portal ilgili risk olay türleri listelenmektedir. Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](concept-risk-events.md).  


| Azure AD anormal etkinlik raporu |  Kimlik koruması risk olayı türü|
| :--- | :--- |
| Sızan kimlik bilgilerine sahip kullanıcılar | Sızdırılan kimlik bilgileri |
| Düzensiz oturum açma etkinliği | Alışılmadık konumlara imkansız seyahat |
| Muhtemelen virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri | Bulaşma olan cihazlardan oturum açma işlemleri|
| Bilinmeyen kaynaklardan gerçekleştirilen oturum açma işlemleri | Anonim IP adreslerinden oturum açma işlemleri |
| Şüpheli etkinliğin olduğu IP adreslerinden oturum açma işlemleri | Şüpheli etkinliğin olduğu IP adreslerinden oturum açma işlemleri |
| - | Alışılmadık konumlardan oturum açma işlemleri |

Aşağıdaki Azure AD anormal etkinlik güvenlik raporları Azure Portal risk olayları olarak dahil edilmez:

* Birden çok hatadan sonra gerçekleştirilen oturum açma işlemleri
* Birden çok coğrafyadan gerçekleştirilen oturum açma işlemleri


### <a name="detected-risk-events"></a>Algılanan risk olayları

Algılanan risk olayları hakkında raporlara, [Azure portal](https://portal.azure.com) **Azure Active Directory** dikey penceresinin **güvenlik** bölümünde erişebilirsiniz. Algılanan risk olayları aşağıdaki raporlarda izlenir:   

- [Risk altındaki kullanıcılar](concept-user-at-risk.md)
- [Riskli oturum açma işlemleri](concept-risky-sign-ins.md)

    ![Güvenlik raporları](./media/howto-find-activity-reports/04.png "Güvenlik raporları")

## <a name="troubleshoot-issues-with-activity-reports"></a>Etkinlik raporlarıyla ilgili sorunları giderme

### <a name="missing-data-in-the-downloaded-activity-logs"></a>İndirilen etkinlik günlüklerinde eksik veriler

#### <a name="symptoms"></a>Belirtiler 

Etkinlik günlüklerini (denetim veya oturum açma) indirdim ve seçtiğim süre için tüm kayıtları göremiyorum. Neden? 

 ![Raporlanıyor](./media/troubleshoot-missing-data-download/01.png)
 
#### <a name="cause"></a>Nedeni

Etkinlik günlüklerini Azure portal indirdiğinizde, ölçeği 250000 kayıt olarak sınırlarız ve en son ilk olarak sıralanır. 

#### <a name="resolution"></a>Çözüm

Belirli bir noktadaki bir milyon kaydı getirmek için [Azure AD Raporlama API’lerini](concept-reporting-api.md) kullanabilirsiniz.

### <a name="missing-audit-data-for-recent-actions-in-the-azure-portal"></a>Azure portal en son işlemler için Denetim verileri eksik

#### <a name="symptoms"></a>Belirtiler

Azure portalında bazı eylemler gerçekleştirdim ve bu eylemlerin denetim günlüklerini `Activity logs > Audit Logs` dikey penceresinde görmeyi umuyordum, ancak bulamıyorum.

 ![Raporlama](./media/troubleshoot-missing-audit-data/01.png)
 
#### <a name="cause"></a>Nedeni

Eylemler, etkinlik günlüklerinde hemen görünmez. Aşağıdaki tabloda etkinlik günlüklerinin gecikme süreleri gösterilmiştir. 

| Rapor | &nbsp; | Gecikme süresi (P95) | Gecikme süresi (P99) |
|--------|--------|---------------|---------------|
| Dizin denetimi | &nbsp; | 2 dk. | 5 dk. |
| Oturum açma etkinliği | &nbsp; | 2 dk. | 5 dk. | 

#### <a name="resolution"></a>Çözüm

15 dakika ile iki saat arasında bekleyin ve eylemlerin günlükte görüntülenip görüntülenmediğine bakın. İki saatten sonra da günlükler görünmüyorsa sorunla ilgilenebilmemiz için [destek bileti oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="missing-logs-for-recent-user-sign-ins-in-the-azure-ad-sign-ins-activity-log"></a>Azure AD oturum açma etkinlik günlüğündeki son kullanıcı oturum açma işlemleri için eksik Günlükler

#### <a name="symptoms"></a>Belirtiler

Azure portalında kısa bir süre önce oturum açtım ve bu oturum açma işleminin günlük girişlerini `Activity logs > Sign-ins` dikey penceresinde görmeyi umuyordum, ancak bulamıyorum.

 ![Raporlanıyor](./media/troubleshoot-missing-audit-data/02.png)
 
#### <a name="cause"></a>Nedeni

Eylemler, etkinlik günlüklerinde hemen görünmez. Aşağıdaki tabloda etkinlik günlüklerinin gecikme süreleri gösterilmiştir. 

| Rapor | &nbsp; | Gecikme süresi (P95) | Gecikme süresi (P99) |
|--------|--------|---------------|---------------|
| Dizin denetimi | &nbsp; | 2 dk. | 5 dk. |
| Oturum açma etkinliği | &nbsp; | 2 dk. | 5 dk. | 

#### <a name="resolution"></a>Çözüm

15 dakika ile iki saat arasında bekleyin ve eylemlerin günlükte görüntülenip görüntülenmediğine bakın. İki saatten sonra da günlükler görünmüyorsa sorunla ilgilenebilmemiz için [destek bileti oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="i-cant-view-more-than-30-days-of-report-data-in-the-azure-portal"></a>Azure portalda 30 günden daha eski rapor verilerini görüntüleyemiyorum

#### <a name="symptoms"></a>Belirtiler

Azure portalda 30 günden daha eski oturum açma ve denetim verilerini görüntüleyemiyorum. Neden? 

 ![Raporlanıyor](./media/troubleshoot-missing-audit-data/03.png)

#### <a name="cause"></a>Nedeni

Lisansınıza bağlı olarak, etkinlik raporları Azure Active Directory Actions tarafından aşağıdaki sürelerde depolanır:

| Rapor           | &nbsp; |  Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| ---              | ----   |  ---           | ---                 | ---                 |
| Dizin Denetimi  | &nbsp; |   7 gün     | 30 gün             | 30 gün             |
| Oturum Açma Etkinliği | &nbsp; | Kullanılamıyor. Kendi oturum açma etkinliklerinize bireysel kullanıcı profili dikey penceresinden 7 gün boyunca erişebilirsiniz | 30 gün | 30 gün             |

Daha fazla bilgi için bkz. [Azure Active Directory rapor bekletme ilkeleri](reference-reports-data-retention.md).  

#### <a name="resolution"></a>Çözüm

Verileri 30 günden daha uzun bir süre boyunca saklamak için iki seçeneğiniz vardır. [Azure AD Raporlama API'lerini](concept-reporting-api.md) kullanarak verileri program aracılığıyla alabilir ve bir veritabanında kaydedebilirsiniz. Alternatif olarak denetim günlüklerini Splunk veya SumoLogic gibi bir üçüncü taraf SIEM sistemiyle tümleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Denetim günlüklerine genel bakış](concept-audit-logs.md)
* [Oturum açma işlemleri genel bakış](concept-sign-ins.md)
* [Riskli olaylara genel bakış](concept-risk-events.md)
