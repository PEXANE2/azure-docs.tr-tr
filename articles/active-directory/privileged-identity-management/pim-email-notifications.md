---
title: PıM-Azure Active Directory 'de e-posta bildirimleri | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) ' de e-posta bildirimlerini açıklar.
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
ms.date: 04/28/2020
ms.author: curtand
ms.reviewer: hanki
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02fbfc83c16cb13376cce820f19b247a7cd7db59
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82232317"
---
# <a name="email-notifications-in-pim"></a>PıM 'de e-posta bildirimleri

Privileged Identity Management (PıM), bir rolün atanma veya etkinleştirilme gibi, Azure Active Directory (Azure AD) kuruluşunuzda önemli olayların ne zaman meydana geldiğinden haberdar olmanızı sağlar. Privileged Identity Management sizi ve diğer katılımcı e-posta bildirimlerini göndererek sizi bilgilendirerek haberdar olmanızı sağlayabilirsiniz. Bu e-postalar, bir rolün etkinleştirilmesi veya yenilenmesi gibi ilgili görevlere ait bağlantıları da içerebilir. Bu makalede, bu e-postaların ne zaman gönderileceğini ve kimlerin bunları alacağını açıklanmaktadır.

## <a name="sender-email-address-and-subject-line"></a>Gönderenin e-posta adresi ve konu satırı

Hem Azure AD hem de Azure Kaynak rolleri için Privileged Identity Management gönderilen e-postaların aşağıdaki Gönderici e-posta adresi vardır:

- E-posta adresi: **Azure-noreply \@ Microsoft.com**
- Görünen ad: Microsoft Azure

Bu e-postalar konu satırında bir **PIM** öneki içerir. İşte bir örnek:

- PıM: Alain Charon, yedekleme okuyucusu rolüne kalıcı olarak atandı

## <a name="notifications-for-azure-ad-roles"></a>Azure AD rolleri için bildirimler

Privileged Identity Management, Azure AD rolleri için aşağıdaki olaylar gerçekleştiğinde e-posta gönderir:

- Ayrıcalıklı bir rol etkinleştirmesi onay bekliyor olduğunda
- Ayrıcalıklı bir rol etkinleştirme isteği tamamlandığında
- Azure AD Privileged Identity Management etkinleştirildiğinde

Bu e-postaları Azure AD rolleri için alan rolünüz, olayınıza ve bildirimler ayarlarına bağlıdır:

| Kullanıcı | Rol etkinleştirme onay bekliyor | Rol etkinleştirme isteği tamamlandı | PıM etkin |
| --- | --- | --- | --- |
| Ayrıcalıklı rol yöneticisi</br>(Etkin/uygun) | Yes</br>(yalnızca açık onaylayanlar belirtilmemişse) | Evet* | Yes |
| Güvenlik Yöneticisi</br>(Etkin/uygun) | No | Evet* | Yes |
| Genel Yönetici</br>(Etkin/uygun) | No | Evet* | Yes |

\*[ **Bildirimler** ayarı](pim-how-to-change-default-settings.md#notifications) **Etkinleştir**olarak ayarlandıysa.

Aşağıda, bir Kullanıcı kurgusal contoso organizasyonu için bir Azure AD rolünü etkinleştirdiğinde gönderilen örnek bir e-posta gösterilmektedir.

![Azure AD rolleri için yeni Privileged Identity Management e-posta](./media/pim-email-notifications/email-directory-new.png)

### <a name="weekly-privileged-identity-management-digest-email-for-azure-ad-roles"></a>Azure AD rolleri için haftalık Privileged Identity Management Özet e-postası

Azure AD rolleri için haftalık Privileged Identity Management Özet e-postası, Privileged Identity Management etkinleştirilmiş ayrıcalıklı rol yöneticilerine, güvenlik yöneticilerine ve genel yöneticilere gönderilir. Bu haftalık e-posta, hem hafta hem de ayrıcalıklı rol atamaları için Privileged Identity Management etkinliklerin anlık görüntüsünü sağlar. Yalnızca genel buluttaki Azure AD kuruluşları için kullanılabilir. Örnek bir e-posta aşağıda verilmiştir:

![Azure AD rolleri için haftalık Privileged Identity Management Özet e-postası](./media/pim-email-notifications/email-directory-weekly.png)

E-postada dört kutucuk bulunur:

| Kutucuk | Description |
| --- | --- |
| **Etkinleştirilen kullanıcılar** | Kullanıcıların kuruluş içinde uygun rollerini etkinleştirme sayısı. |
| **Kullanıcılar kalıcı yaptı** | Uygun atamaya sahip kullanıcıların kalıcı hale getirilme sayısı. |
| **Privileged Identity Management rol atamaları** | Privileged Identity Management içinde kullanıcılara uygun bir rol atama sayısı. |
| **PıM dışında rol atamaları** | Kullanıcıların Privileged Identity Management dışında kalıcı bir rol atanma sayısı (Azure AD içinde). |

**En üst rolleriniz bölümüne genel bakış** bölümünde her bir rol için toplam kalıcı ve uygun yönetici sayısına göre kuruluşunuzdaki ilk beş rol listelenir. **Eylem al** bağlantısı, kalıcı yöneticileri toplu gruplar halinde uygun yöneticilere dönüştürebileceğiniz [PIM Sihirbazı](pim-security-wizard.md) ' nı açar.

## <a name="email-timing-for-activation-approvals"></a>Etkinleştirme onayları için e-posta zamanlaması

Kullanıcılar rolünü etkinleştirdiğinde ve rol ayarı onay gerektiriyorsa, onaylayanlar her onay için üç e-posta alır:

- Kullanıcının etkinleştirme isteğini onaylama veya reddetme isteği (istek onay altyapısı tarafından gönderilir)
- Kullanıcının isteği onaylandı (istek onay altyapısı tarafından gönderilir)
- Kullanıcının rolü etkinleştirilir (Privileged Identity Management tarafından gönderilir)

İstek onay altyapısı tarafından gönderilen ilk iki e-posta gecikiyor olabilir. Şu anda, e-postaların %90 ' üne üç ila on dakika kaldı, ancak %1 müşteri için en fazla on beş dakikaya kadar çok daha uzun sürebilir.

İlk e-posta gönderilmeden önce Azure portal bir onay isteği onaylanırsa, ilk e-posta artık tetiklenmeyecektir ve diğer onaylayanlara onay isteğinin e-postası tarafından bildirilmeyecektir. E-posta almadıkları gibi görünebilir, ancak beklenen davranıştır.

## <a name="pim-emails-for-azure-resource-roles"></a>Azure Kaynak rolleri için PıM e-postaları

Privileged Identity Management, Azure Kaynak rolleri için aşağıdaki olaylar gerçekleştiğinde Sahibe ve Kullanıcı erişimi yöneticilerine e-posta gönderir:

- Bir rol ataması onay bekliyor olduğunda
- Bir rol atandığında
- Bir rolün kullanım süresini yakında dolacak
- Bir rol genişletmeye uygun olduğunda
- Bir rol Son Kullanıcı tarafından yenilendiğinde
- Bir rol etkinleştirme isteği tamamlandığında

Privileged Identity Management, Azure Kaynak rolleri için aşağıdaki olaylar gerçekleştiğinde son kullanıcılara e-posta gönderir:

- Kullanıcıya bir rol atandığında
- Bir kullanıcının rolü süre dolduğunda
- Bir kullanıcının rolü genişletildiğinde
- Bir kullanıcının rol etkinleştirme isteği tamamlandığında

Aşağıda, bir kullanıcıya kurgusal contoso organizasyonu için bir Azure Kaynak rolü atandığında gönderilen örnek bir e-posta gösterilmektedir.

![Azure Kaynak rolleri için yeni Privileged Identity Management e-posta](./media/pim-email-notifications/email-resources-new.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management Azure AD rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [Privileged Identity Management Azure AD rolleri için istekleri onaylama veya reddetme](azure-ad-pim-approval-workflow.md)
