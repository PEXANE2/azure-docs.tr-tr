---
title: 'Azure AD Connect: sonraki adımlar ve Azure AD Connect yönetme | Microsoft Docs'
description: Azure AD Connect için varsayılan yapılandırmayı ve işletimsel görevleri genişletmeyi öğrenin.
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
ms.topic: how-to
ms.date: 04/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7d6689b4b67b8462e983ae9b111e0fbc60c422b7
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89657115"
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Sonraki adımlar ve Azure AD Connect yönetme
Kuruluşunuzun gereksinimlerini ve gereksinimlerini karşılamak üzere Azure Active Directory (Azure AD) ile bağlantı kurmak için bu makaledeki işletimsel yordamları kullanın.  

## <a name="add-additional-sync-admins"></a>Ek eşitleme yöneticileri ekleme
Varsayılan olarak, yalnızca yüklemeyi ve yerel yöneticileri yapan Kullanıcı yüklü eşitleme altyapısını yönetebilir. Daha fazla kişinin eşitleme altyapısına erişip yönetebilmesi için, yerel sunucuda ADSyncAdmins adlı grubu bulun ve bu gruba ekleyin.

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a>Azure AD Premium ve Enterprise Mobility Suite kullanıcılarına lisans atama
Kullanıcılarınız buluta eşitlendiklerinde, Microsoft 365 gibi bulut uygulamalarına ulaşmak için onlara bir lisans atamanız gerekir.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Azure AD Premium veya Enterprise Mobility Suite lisansı atamak için

1. Azure portal yönetici olarak oturum açın.
2. Sol tarafta **Active Directory**' yi seçin.
3. **Active Directory** sayfasında, kurmak istediğiniz kullanıcıları içeren dizine çift tıklayın.
4. Dizin sayfasının en üst kısmındaki **Lisanslar** öğesini seçin.
5. **Lisanslar** sayfasında, Premium veya **Enterprise Mobility Suite** **Active Directory** seçin ve ardından **ata**' ya tıklayın.
6. İletişim kutusunda, lisans atamak istediğiniz kullanıcıları seçin ve ardından değişiklikleri kaydetmek için onay işareti simgesine tıklayın.

## <a name="verify-the-scheduled-synchronization-task"></a>Zamanlanan eşitleme görevini doğrulama
Bir eşitlemenin durumunu denetlemek için Azure portal kullanın.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Zamanlanan eşitleme görevini doğrulamak için
1. Azure portal yönetici olarak oturum açın.
2. Sol tarafta **Active Directory**' yi seçin.
3. Sol tarafta **Azure AD Connect** ' yi seçin.
4. Sayfanın üst kısmında, son eşitlemeye göz önünde.

![Dizin eşitleme süresi](./media/how-to-connect-post-installation/verify2.png)

## <a name="start-a-scheduled-synchronization-task"></a>Zamanlanmış bir eşitleme görevi başlatma
Bir eşitleme görevi çalıştırmanız gerekiyorsa bunu şu şekilde yapabilirsiniz:

1. Sihirbazı başlatmak için Azure AD Connect masaüstü kısayoluna çift tıklayın.
2. **Yapılandır**'a tıklayın.
3. Görevler ekranında **eşitleme özelleştirme seçeneklerini** belirleyin ve **İleri** ' ye tıklayın.
4. Azure AD kimlik bilgilerinizi girin
5. **İleri**’ye tıklayın. **İleri**’ye tıklayın.  **İleri**’ye tıklayın.
5.  **Yapılandırmaya hazırlanma** ekranında, **yapılandırma tamamlandığında eşitleme işlemini başlat** kutusunun seçili olduğundan emin olun.
6.  **Yapılandır**'a tıklayın.

Azure AD Connect eşitleme Zamanlayıcısı hakkında daha fazla bilgi için bkz. [Azure AD Connect Scheduler](how-to-connect-sync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Azure AD Connect ek görevler var
Azure AD Connect ilk yüklemesinden sonra, Azure AD Connect başlangıç sayfasından veya masaüstü kısayolundan Sihirbazı dilediğiniz zaman yeniden başlatabilirsiniz.  Sihirbazın daha sonra ek görevler biçiminde bazı yeni seçenekler sağladığını fark edersiniz.  

Aşağıdaki tabloda, bu görevlerin bir özeti ve her görevin kısa bir açıklaması verilmiştir.

![Ek görevlerin listesi](./media/how-to-connect-post-installation/addtasks2.png)

| Ek görev | Description |
| --- | --- |
|**Gizlilik Ayarları**|Microsoft ile hangi Telemetri verilerinin paylaşıldığını görüntüleyin.|
|**Geçerli yapılandırmayı görüntüle**|Geçerli Azure AD Connect çözümünüzü görüntüleyin.  Bu, genel ayarları, eşitlenen dizinleri ve eşitleme ayarlarını içerir. |
| **Eşitleme seçeneklerini özelleştirme** |Yapılandırmaya ek Active Directory ormanları ekleme veya Kullanıcı, Grup, cihaz veya parola geri yazma gibi eşitleme seçeneklerinin etkinleştirilmesi gibi geçerli yapılandırmayı değiştirin. |
|**Cihaz seçeneklerini yapılandır**|Eşitleme için kullanılabilir cihaz seçenekleri|
|**Dizin şemasını Yenile**|Eşitleme için yeni şirket içi dizin nesneleri eklemenize olanak tanır|
|**Hazırlama modunu Yapılandır** |Hemen eşitlenmemiş ve Azure AD 'ye veya şirket içi Active Directory aktarılmayan aşama bilgileri.  Bu özellikle, eşitlemelere başlamadan önce önizleme yapabilirsiniz. |
|**Kullanıcı oturum açma değiştirme**|Kullanıcıların oturum açmak için kullandığı kimlik doğrulama yöntemini değiştirme|
|**Federasyonu yönetme**|AD FS altyapınızı yönetin, sertifikaları yenileyin ve AD FS sunucuları ekleyin|
|**Sorun giderme**|Sorunları gidermeye yönelik yardım Azure AD Connect|

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory tümleştirme](whatis-hybrid-identity.md)hakkında daha fazla bilgi edinin.
