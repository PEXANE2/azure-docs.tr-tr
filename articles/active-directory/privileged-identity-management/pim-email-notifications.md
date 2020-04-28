---
title: PIM'deki e-posta bildirimleri - Azure Active Directory | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) e-posta bildirimlerini açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/21/2020
ms.author: curtand
ms.reviewer: hanki
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 201abd24bc4056337f1ffecd2dabd002ae352c74
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81866429"
---
# <a name="email-notifications-in-pim"></a>PIM'deki e-posta bildirimleri

Ayrıcalıklı Kimlik Yönetimi (PIM), Azure Etkin Dizin (Azure AD) kuruluşunuzda bir rolün atanması veya etkinleştirilmeleri gibi önemli olayların ne zaman meydana geldiğini bilmenizi sağlar. Ayrıcalıklı Kimlik Yönetimi size ve diğer katılımcılara e-posta bildirimleri göndererek sizi bilgilendirir. Bu e-postalar, bir rolü etkinleştirme veya yenileme gibi alakalı görevlere bağlantılar da içerebilir. Bu makalede, bu e-postaların neye benzedikleri, ne zaman gönderildikleri ve kimlerin aldığı açıklanmaktadır.

## <a name="sender-email-address-and-subject-line"></a>Gönderen e-posta adresi ve konu satırı

Hem Azure AD hem de Azure kaynak rolleri için Ayrıcalıklı Kimlik Yönetimi'nden gönderilen e-postalar aşağıdaki gönderene e-posta adresine sahiptir:

- E-posta adresi: **azure-noreply\@microsoft.com**
- Görüntü adı: Microsoft Azure

Bu e-postalar, konu satırında bir **PIM** öneki içerir. Bir örneği aşağıda verilmiştir:

- PIM: Alain Charon kalıcı Yedekleme Okuyucu rolü atandı

## <a name="notifications-for-azure-ad-roles"></a>Azure AD rolleri için bildirimler

Ayrıcalıklı Kimlik Yönetimi, Azure AD rolleri için aşağıdaki olaylar oluştuğunda e-posta gönderir:

- Ayrıcalıklı bir rol etkinleştirme onay bekliyor
- Ayrıcalıklı bir rol etkinleştirme isteği tamamlandığında
- Azure AD Ayrıcalıklı Kimlik Yönetimi etkinleştirildiğinde

Azure AD rolleri için bu e-postaları kimlerin aldığı rolünüze, etkinliğinize ve bildirim ayarlarına bağlıdır:

| Kullanıcı | Rol etkinleştirme onay bekliyor | Rol etkinleştirme isteği tamamlandı | PIM etkin |
| --- | --- | --- | --- |
| Ayrıcalıklı Rol Yöneticisi</br>(Etkinleştirildi/Uygun) | Evet</br>(yalnızca açık onaylayıcı belirtilmemişse) | Evet* | Evet |
| Güvenlik Yöneticisi</br>(Etkinleştirildi/Uygun) | Hayır | Evet* | Evet |
| Genel Yönetici</br>(Etkinleştirildi/Uygun) | Hayır | Evet* | Evet |

\*Bildirimler ayarı **Etkinleştir'e** [ **ayarlanmışsa.** ](pim-how-to-change-default-settings.md#notifications)

Aşağıda, bir kullanıcı kurgusal Contoso kuruluşu için bir Azure REKLAM rolünü etkinleştirdiğinde gönderilen örnek bir e-posta gösterilmektedir.

![Azure AD rolleri için yeni Ayrıcalıklı Kimlik Yönetimi e-postası](./media/pim-email-notifications/email-directory-new.png)

### <a name="weekly-privileged-identity-management-digest-email-for-azure-ad-roles"></a>Azure AD rolleri için Haftalık Ayrıcalıklı Kimlik Yönetimi e-postayı sindirir

Azure AD rolleri için haftalık Ayrıcalıklı Kimlik Yönetimi özet e-postası, Ayrıcalıklı Kimlik Yönetimi'ni etkinleştiren Ayrıcalıklı Rol Yöneticileri, Güvenlik Yöneticileri ve Global Yöneticilere gönderilir. Bu haftalık e-posta, hafta için Ayrıcalıklı Kimlik Yönetimi etkinliklerinin yanı sıra ayrıcalıklı rol atamalarının anlık görüntüsünü sağlar. Yalnızca genel bulutta kiracılar için kullanılabilir. İşte örnek bir e-posta:

![Azure AD rolleri için Haftalık Ayrıcalıklı Kimlik Yönetimi e-postayı sindirir](./media/pim-email-notifications/email-directory-weekly.png)

E-posta dört kutucuk içerir:

| Kutucuk | Açıklama |
| --- | --- |
| **Kullanıcılar etkinleştirildi** | Kullanıcıların kiracı içindeki uygun rollerini etkinleştirme sayısı. |
| **Kullanıcılar kalıcı hale** | Uygun bir atamaya sahip kullanıcıların kaç kez kalıcı hale getirilir. |
| **Ayrıcalıklı Kimlik Yönetiminde Rol Atamaları** | Ayrıcalıklı Kimlik Yönetimi içinde kullanıcılara uygun bir rol atananma sayısı. |
| **PIM dışındaki rol atamaları** | Kullanıcılara Ayrıcalıklı Kimlik Yönetimi dışında (Azure AD içinde) kalıcı bir rol atananma sayısı. |

**En iyi roller bölümünüze genel bakış,** her rol için toplam kalıcı ve uygun yönetici sayısına bağlı olarak kiracınızdaki en iyi beş rolü listeler. **Eyleme Geç** bağlantısı, kalıcı yöneticileri toplu olarak uygun yöneticilere dönüştürebileceğiniz [PIM sihirbazını](pim-security-wizard.md) açar.

## <a name="email-timing-for-activation-approvals"></a>Etkinleştirme onayları için e-posta zamanlaması

Kullanıcılar rollerini etkinleştirdiğinde ve rol ayarı onay gerektiriyorsa, onaylayanlar her onay için üç e-posta alır:

- Kullanıcının etkinleştirme isteğini onaylama veya reddetme isteği (istek onay motoru tarafından gönderilir)
- Kullanıcının isteği onaylanır (istek onay motoru tarafından gönderilir)
- Kullanıcının rolü etkinleştirilir (Ayrıcalıklı Kimlik Yönetimi tarafından gönderilir)

İstek onay motoru tarafından gönderilen ilk iki e-posta gecikebilir. Şu anda, e-postaların% 90 üç ila on dakika sürer, ama% 1 müşteriler için çok daha uzun olabilir, on beş dakikakadar.

İlk e-posta gönderilmeden önce Azure portalında bir onay isteği onaylanırsa, ilk e-posta artık tetiklenmez ve diğer onaylayanlar onay isteğinin e-postayoluyla bildirilmez. Onlar bir e-posta alamadım ama beklenen davranış gibi görünebilir.

## <a name="pim-emails-for-azure-resource-roles"></a>Azure kaynak rolleri için PIM e-postaları

Ayrıcalıklı Kimlik Yönetimi, Azure kaynak rolleri için aşağıdaki olaylar meydana geldiğinde Sahipsahiplerine ve Kullanıcı Erişim Yöneticilerine e-posta gönderir:

- Bir rol ataması onay beklerken
- Bir rol atandığında
- Bir rolün süresi yakında sona erdiğinde
- Bir rolü n
- Bir rol son kullanıcı tarafından yenileniyorsa
- Rol etkinleştirme isteği tamamlandığında

Ayrıcalıklı Kimlik Yönetimi, Azure kaynak rolleri için aşağıdaki olaylar oluştuğunda son kullanıcılara e-posta gönderir:

- Bir rol kullanıcıya atandığında
- Bir kullanıcının rolü sona erdiğinde
- Bir kullanıcının rolü genişletildiğinde
- Bir kullanıcının rol etkinleştirme isteği tamamlandığında

Aşağıda, bir kullanıcıya kurgusal Contoso organizasyonu için bir Azure kaynak rolü atandığında gönderilen örnek bir e-posta gösterilmektedir.

![Azure kaynak rolleri için yeni Ayrıcalıklı Kimlik Yönetimi e-postası](./media/pim-email-notifications/email-resources-new.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rolleri için istekleri onaylama veya reddetme](azure-ad-pim-approval-workflow.md)
