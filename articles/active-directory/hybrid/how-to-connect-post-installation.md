---
title: "Azure AD Connect: Sonraki adımlar ve Azure AD Connect'i nasıl yönetirediye devam edebilirsiniz | Microsoft Dokümanlar"
description: Azure AD Connect için varsayılan yapılandırmave işletim görevlerini nasıl genişleteceklerini öğrenin.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c204029557a73dc3f02015afb92c0fdbf0d4d50e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261300"
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Sonraki adımlar ve Azure AD Connect'i nasıl yönetileriz
Kuruluşunuzun gereksinimlerini ve gereksinimlerini karşılamak için Azure Active Directory (Azure AD) Connect'i özelleştirmek için bu makaledeki çalışma yordamlarını kullanın.  

## <a name="add-additional-sync-admins"></a>Ek eşitleme yöneticileri ekleme
Varsayılan olarak, yalnızca yüklemeyi yapan kullanıcı ve yerel yöneticiler yüklü eşitleme altyapısını yönetebilir. Eşitleme altyapısına erişebilen ve yönetebilen ek kişilerin yerel sunucuda ADSyncAdmins adlı grubu bulabilmesi ve bunları bu gruba eklemesi için.

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a>Azure AD Premium ve Enterprise Mobility Suite kullanıcılarına lisans atama
Kullanıcılarınız bulutla senkronize edildiğine göre, Office 365 gibi bulut uygulamalarıyla devam edebilmeleri için onlara bir lisans atamanız gerekir.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Azure AD Premium veya Enterprise Mobility Suite Lisansı atamak için

1. Yönetici olarak Azure portalında oturum açın.
2. Solda **Active Directory'yi**seçin.
3. Etkin **Dizin** sayfasında, ayarlamak istediğiniz kullanıcıların olduğu dizini çift tıklatın.
4. Dizin sayfasının en üst kısmındaki **Lisanslar** öğesini seçin.
5. **Lisanslar** sayfasında Active **Directory Premium** veya **Enterprise Mobility Suite'i**seçin ve ardından **Ata'yı**tıklatın.
6. İletişim kutusunda, lisans atamak istediğiniz kullanıcıları seçin ve ardından değişiklikleri kaydetmek için onay işareti simgesine tıklayın.

## <a name="verify-the-scheduled-synchronization-task"></a>Zamanlanan eşitleme görevini doğrulama
Eşitleme durumunu denetlemek için Azure portalını kullanın.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Zamanlanan eşitleme görevini doğrulamak için
1. Yönetici olarak Azure portalında oturum açın.
2. Solda **Active Directory'yi**seçin.
3. Solda Azure **AD Bağlantısı'nı** seçin
4. Sayfanın üst kısmında, son eşitleme not edin.

![Dizin eşitleme süresi](./media/how-to-connect-post-installation/verify2.png)

## <a name="start-a-scheduled-synchronization-task"></a>Zamanlanmış bir eşitleme görevi başlatma
Bir eşitleme görevi çalıştırmanız gerekiyorsa, bunu şunları yapabilirsiniz:

1. Sihirbazı başlatmak için Azure AD Connect masaüstü kısayolunda çift tıklayın.
2. **Yapılandır**'a tıklayın.
3. Görevler ekranında, **eşitleme seçeneklerini özelleştir'i** seçin ve **İleri'yi** tıklatın
4. Azure AD kimlik bilgilerinizi girin
5. **İleri**'ye tıklayın. **İleri**'ye tıklayın.  **İleri**'ye tıklayın.
5.  **Yapılandırmaya Hazır** ekranında, yapılandırma **tamamlandığında eşitleme işlemini başlat'ın** kutusunun seçildiğinden emin olun.
6.  **Yapılandır**'a tıklayın.

Azure AD Connect eşitleme Zamanlayıcısı hakkında daha fazla bilgi için [Azure AD Connect Zamanlayıcısı'na](how-to-connect-sync-feature-scheduler.md)bakın.

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Azure AD Connect'te kullanılabilen ek görevler
Azure AD Connect'i ilk yüklemenizden sonra sihirbazı Azure AD Connect başlangıç sayfasından veya masaüstü kısayolu'ndan her zaman yeniden başlatabilirsiniz.  Sihirbazdan tekrar geçmenin ek görevler şeklinde bazı yeni seçenekler sağladığını fark edeceksiniz.  

Aşağıdaki tablo, bu görevlerin bir özetini ve her görevin kısa bir açıklamasını sağlar.

![Ek görevler listesi](./media/how-to-connect-post-installation/addtasks2.png)

| Ek görev | Açıklama |
| --- | --- |
|**Gizlilik Ayarları**|Telemetri verilerinin Microsoft ile paylaşıldığını görüntüleyin.|
|**Geçerli yapılandırmayı görüntüleme**|Geçerli Azure AD Connect çözümünüzü görüntüleyin.  Buna genel ayarlar, eşitlenmiş dizinler ve eşitleme ayarları dahildir. |
| **Eşitleme seçeneklerini özelleştirme** |Yapılandırmaya ek Active Directory ormanları ekleme veya kullanıcı, grup, aygıt veya parola geri yazma gibi eşitleme seçeneklerini etkinleştirme gibi geçerli yapılandırmayı değiştirin. |
|**Aygıt seçeneklerini yapılandırma**|Senkronizasyon için aygıt seçenekleri kullanılabilir|
|**Dizin şealarını yenile**|Eşitleme için yeni şirket içi dizin nesneleri eklemenize olanak tanır|
|**Evreleme Modunu Yapılandır** |Hemen eşitlenmemiş ve Azure AD'ye veya şirket içi Active Directory'ye dışa aktarılmayan aşama bilgileri.  Bu özellik sayesinde, eşitlemeler oluşmadan önce önizleyebilirsiniz. |
|**Kullanıcı oturum açma'yı değiştirme**|Oturum açma için kullanıcıların kullandığı kimlik doğrulama yöntemini değiştirme|
|**Federasyonu yönet**|AD FS altyapınızı yönetin, sertifikaları yenileyin ve AD FS sunucuları ekleyin|
|**Sorun giderme**|Azure AD Connect sorunlarını giderme konusunda yardım|

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)hakkında daha fazla bilgi edinin.
