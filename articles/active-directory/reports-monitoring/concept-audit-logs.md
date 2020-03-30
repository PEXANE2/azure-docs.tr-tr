---
title: Azure Active Directory portalındaki denetim etkinliği raporları | Microsoft Docs
description: Azure Active Directory portalındaki denetim etkinliği raporlarına giriş
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 07/17/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad399fc24b2cdfbdc51e7feccba2c05786216b19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253240"
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki denetim etkinliği raporları 

Azure Etkin Dizin (Azure AD) raporlarıyla, ortamınızın nasıl olduğunu belirlemek için ihtiyacınız olan bilgileri alabilirsiniz.

Raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma -** [Oturum açma raporu,](concept-sign-ins.md) yönetilen uygulamaların kullanımı ve kullanıcı oturum açma etkinlikleri hakkında bilgi sağlar.
    - **Denetim günlükleri** - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Denetim günlüklerine örnek olarak, Azure AD'deki kullanıcılar, uygulamalar, gruplar, roller ve ilkeler ekleme veya kaldırma gibi kaynaklarda yapılan değişiklikler verilebilir.
- **Güvenlik** 
    - **Riskli oturum açmalar** - [Riskli oturum](concept-risky-sign-ins.md) açma, kullanıcı hesabının yasal sahibi olmayan biri tarafından gerçeklegelmiş olabilecek oturum açma girişiminin göstergesidir. 
    - **Risk için işaretlenmiş kullanıcılar** - Riskli bir [kullanıcı,](concept-user-at-risk.md) gizliliği ihlal edilmiş olabilecek bir kullanıcı hesabı için bir göstergedir.

Bu makalede, denetim raporu genel bir bakış sağlar.
 
## <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?

* **Güvenlik Yöneticisi, Güvenlik** **Okuyucu,** **Rapor Okuyucu,** **Global Reader** veya **Global Administrator** rollerindeki kullanıcılar

## <a name="audit-logs"></a>Denetim günlükleri

Azure AD denetim günlükleri, uyumluluk için sistem etkinliklerinin kayıtlarını sağlar. Denetim raporuna erişmek için **Azure Active Directory'nin** **İzleme** bölümünde **Denetim günlüklerini** seçin. Denetim günlüklerinin bir saate kadar bir gecikme sarası olabileceğini unutmayın, bu nedenle görevi tamamladıktan sonra denetim etkinliği verilerinin portalda gösterilmesi o kadar uzun sürebilir.



Denetim günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- Olayın tarihi ve saati
- oluşumu günlüğe kaydeden hizmet
- etkinliğin kategorisi ve adı (*ne*) 
- etkinliğin durumu (başarı veya başarısızlık)
- Hedef
- Bir etkinliğin başlatıcısı/aktörü (kim)

![Denetim günlükleri](./media/concept-audit-logs/listview.png "Denetim günlükleri")

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Denetim günlükleri](./media/concept-audit-logs/columns.png "Denetim günlükleri")

Bu sayede ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz.

![Denetim günlükleri](./media/concept-audit-logs/columnselect.png "Denetim günlükleri")

Daha ayrıntılı bilgi almak için liste görünümünde bir öğe seçin.

![Denetim günlükleri](./media/concept-audit-logs/details.png "Denetim günlükleri")


## <a name="filtering-audit-logs"></a>Denetim günlüklerini filtreleme

Denetim verilerini aşağıdaki alanlarda filtreleyebilirsiniz:

- Hizmet
- Kategori
- Etkinlik
- Durum
- Hedef
- Başlatan (Aktör)
- Tarih aralığı

![Denetim günlükleri](./media/concept-audit-logs/filter.png "Denetim günlükleri")

**Hizmet** filtresi, aşağıdaki hizmetlerin açılır listesinden seçim yapmanızı sağlar:

- Tümü
- AAD Yönetim UX
- Erişim Gözden Geçirmeleri
- Hesap Sağlama
- Uygulama Ara Sunucusu
- Kimlik Doğrulaması Yöntemleri
- B2C
- Koşullu Erişim
- Çekirdek Dizin
- Yetki Yönetimi
- Karma Kimlik Doğrulama
- Kimlik Koruması
- Davetli Kullanıcılar
- MIM Hizmeti
- MyApps
- PIM
- Self Servis Grup Yönetimi
- Self Servis Parola Yönetimi
- Kullanım Koşulları

**Kategori** filtresi aşağıdaki filtrelerden birini seçmenize olanak tanır:

- Tümü
- AdministrativeUnit
- Uygulama Yönetimi
- Kimlik doğrulaması
- Yetkilendirme
- İletişim
- Cihaz
- DeviceConfiguration
- DizinYönetimi
- Yetki Yönetimi
- Grup Yönetimi
- KerberosDomain
- Anahtar Yönetimi
- Etiketle
- Diğer
- İzinGrantPolitikası
- İlke
- Kaynak Yönetimi
- RoleManagement
- Kullanıcı Yönetimi

**Etkinlik** filtresi, yaptığınız kategori ve etkinlik kaynağı türü seçimini temel almaktadır. Görmek istediğiniz belirli bir etkinliği ya da tüm etkinlikleri seçebilirsiniz. 

Grafik API'sini kullanarak tüm Denetim Etkinliklerinin listesini alabilirsiniz:`https://graph.windows.net/<tenantdomain>/activities/auditActivityTypesV2?api-version=beta`

**Durum** filtresi, denetim işleminin durumuna göre filtreleme yapmanızı sağlar. Durum aşağıdakilerden biri olabilir:

- Tümü
- Başarılı
- Hata

**Hedef** filtresi, belirli bir hedefi ad veya kullanıcı anaadının (UPN) başlayarak aramayapmanızı sağlar. Hedef adı ve UPN büyük/küçük harf duyarlıdır. 

**Başlatılmış filtre,** bir aktörün adının veya evrensel bir anaadın (UPN) neyle başladığını tanımlamanızı sağlar. Adı ve UPN büyük/küçük harf duyarlıdır.

**Tarih aralığı** filtresi, döndürülen veriler için bir zaman dilimi tanımlamanızı sağlar.  
Olası değerler şunlardır:

- 7 gün
- 24 saat
- Özel

Özel bir zaman çerçevesi seçerken başlangıç ve bitiş zamanını yapılandırabilirsiniz.

Ayrıca, **İndir** düğmesini seçerek 250.000'e kadar kayıt olan filtrelenmiş verileri indirmeyi de seçebilirsiniz. Günlükleri CSV veya JSON formatında indirebilirsiniz. İndirebileceğiniz kayıt sayısı [Azure Etkin Dizin raporu bekletme ilkeleriyle](reference-reports-data-retention.md)sınırlandırılmıştır.

![Denetim günlükleri](./media/concept-audit-logs/download.png "Denetim günlükleri")

## <a name="audit-logs-shortcuts"></a>Denetim günlükleri kısayolları

Azure portalı, **Azure Active Directory**’ye ek olarak verileri denetlemeniz için fazladan iki giriş noktası sağlar:

- Kullanıcılar ve gruplar
- Kurumsal uygulamalar

### <a name="users-and-groups-audit-logs"></a>Kullanıcı ve gruplara yönelik denetim günlükleri

Kullanıcı ve grup tabanlı denetim raporları ile aşağıdakiler gibi soruların yanıtlarını alabilirsiniz:

- Kullanıcılara hangi tür güncelleştirmeler uygulanmış?

- Kaç adet kullanıcı değiştirildi?

- Kaç adet parola değiştirildi?

- Bir yönetici bir dizinde neler yaptı?

- Eklenmiş olan gruplar hangileridir?

- Üyelik değişiklikleri olan gruplar var mı?

- Grubun sahipleri değişti mi?

- Bir grup veya kullanıcıya hangi lisanslar atanmış?

Yalnızca kullanıcılarla ilgili denetim verilerini gözden geçirmek istiyorsanız, **Kullanıcılar** sekmesinin **İzleme** bölümünde **Denetim günlükleri** altında filtreuygulanmış bir görünüm bulabilirsiniz. Bu giriş noktası, **UserManagement'ı** önceden seçilmiş kategori olarak sunar.

![Denetim günlükleri](./media/concept-audit-logs/users.png "Denetim günlükleri")

Yalnızca gruplarla ilgili denetim verilerini gözden geçirmek istiyorsanız, **Gruplar** sekmesinin **Denetim** bölümünde **Denetim günlükleri** altında filtreuygulanmış bir görünüm bulabilirsiniz. Bu giriş **noktası, GroupManagement'ı** önceden seçilmiş kategori olarak alır.

![Denetim günlükleri](./media/concept-audit-logs/groups.png "Denetim günlükleri")

### <a name="enterprise-applications-audit-logs"></a>Kurumsal uygulamaların denetim günlükleri

Uygulama tabanlı denetim raporları ile aşağıdakiler gibi soruların yanıtlarını alabilirsiniz:

* Hangi uygulamalar eklendi veya güncelleştirildi?
* Hangi uygulamalar kaldırıldı?
* Bir uygulama için hizmet sorumlusu değişti mi?
* Uygulamaların adları değiştirildi mi?
* Belirli bir uygulama için kim onay verdi?

Uygulamalarınız ile ilgili denetim verilerini gözden geçirmek istiyorsanız, **Kurumsal uygulamalar** bıçağının **Etkinlik** bölümünde **Denetim günlükleri** altında filtrelenmiş bir görünüm bulabilirsiniz. Bu giriş **noktası, Uygulama** **Türü**olarak önceden seçilmiş Enterprise uygulamaları vardır.

![Denetim günlükleri](./media/concept-audit-logs/enterpriseapplications.png "Denetim günlükleri")

## <a name="office-365-activity-logs"></a>Office 365 etkinlik günlükleri

[Microsoft 365 yönetici merkezinden Office 365](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center)etkinlik günlüklerini görüntüleyebilirsiniz. Office 365 etkinliği ve Azure AD etkinlik günlükleri çok fazla dizin kaynaklarını paylaşsa da, yalnızca Microsoft 365 yönetici merkezi Office 365 etkinlik günlüklerinin tam görünümünü sağlar. 

Ayrıca, [Office 365 Yönetim API'lerini](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview)kullanarak Office 365 etkinlik günlüklerine programlı olarak erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD denetim etkinliği başvurusu](reference-audit-activities.md)
- [Azure AD, bekletme başvurusu rapor etti](reference-reports-data-retention.md)
- [Azure AD günlük gecikmeleri başvurusu](reference-reports-latencies.md)
