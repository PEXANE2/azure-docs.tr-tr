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
ms.date: 01/05/2019
ms.author: curtand
ms.reviewer: hanki
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0db4828ef7a6d3392ce53b2f2f95b0f7f014d65
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804535"
---
# <a name="email-notifications-in-pim"></a>PıM 'de e-posta bildirimleri

Azure Active Directory (Azure AD) Privileged Identity Management (PıM), bir rolün atanma veya etkinleştirilme gibi önemli olayların ne zaman meydana geldiğinden haberdar olmanızı sağlar. PıM size ve diğer katılımcı e-posta bildirimlerini göndererek sizi bilgilenmektedir. Bu e-postalar, bir rolün etkinleştirilmesi veya yenilenmesi gibi ilgili görevlere ait bağlantıları da içerebilir. Bu makalede, bu e-postaların ne zaman gönderileceğini ve kimlerin bunları alacağını açıklanmaktadır.

## <a name="sender-email-address-and-subject-line"></a>Gönderenin e-posta adresi ve konu satırı

Azure AD ve Azure Kaynak rolleri için PıM 'den gönderilen e-postaların aşağıdaki Gönderici e-posta adresi vardır:

- E-posta adresi: **Azure-\@noreply Microsoft.com**
- Görünen ad: Microsoft Azure

Bu e-postalar konu satırında bir **PIM** öneki içerir. Bir örneği aşağıda verilmiştir:

- PIM Alain Charon, yedekleme okuyucusu rolüne kalıcı olarak atandı

## <a name="pim-emails-for-azure-ad-roles"></a>Azure AD rolleri için PıM e-postaları

PıM, Azure AD rolleri için aşağıdaki olaylar gerçekleştiğinde e-posta gönderir:

- Ayrıcalıklı bir rol etkinleştirmesi onay bekliyor olduğunda
- Ayrıcalıklı bir rol etkinleştirme isteği tamamlandığında
- Azure AD PıM etkinleştirildiğinde

Bu e-postaları Azure AD rolleri için alan rolünüz, olayınıza ve bildirimler ayarlarına bağlıdır:

| Kullanıcı | Rol etkinleştirme onay bekliyor | Rol etkinleştirme isteği tamamlandı | PıM etkin |
| --- | --- | --- | --- |
| Ayrıcalıklı Rol Yöneticisi</br>(Etkin/uygun) | Evet</br>(yalnızca açık onaylayanlar belirtilmemişse) | Evet* | Evet |
| Güvenlik Yöneticisi</br>(Etkin/uygun) | Hayır | Evet* | Evet |
| Genel Yönetici</br>(Etkin/uygun) | Hayır | Evet* | Evet |

\*[ **Bildirimler** ayarı](pim-how-to-change-default-settings.md#notifications) **Etkinleştir**olarak ayarlandıysa.

Aşağıda, bir Kullanıcı kurgusal contoso organizasyonu için bir Azure AD rolünü etkinleştirdiğinde gönderilen örnek bir e-posta gösterilmektedir.

![Azure AD rolleri için yeni PıM e-postası](./media/pim-email-notifications/email-directory-new.png)

### <a name="weekly-pim-digest-email-for-azure-ad-roles"></a>Azure AD rolleri için haftalık PıM Özeti e-postası

Azure AD rolleri için haftalık bir PıM Özet e-postası, PıM 'yi etkinleştirmiş olan ayrıcalıklı rol yöneticilerine, güvenlik yöneticilerine ve küresel yöneticilere gönderilir. Bu haftalık e-posta, hem hafta hem de ayrıcalıklı rol atamaları için PıM etkinliklerinin anlık görüntüsünü sağlar. Yalnızca genel buluttaki kiracılar için kullanılabilir. Örnek bir e-posta aşağıda verilmiştir:

![Azure AD rolleri için haftalık PıM Özeti e-postası](./media/pim-email-notifications/email-directory-weekly.png)

E-postada dört kutucuk bulunur:

| Kutucuk | Açıklama |
| --- | --- |
| **Etkinleştirilen kullanıcılar** | Kullanıcıların kiracı içinde uygun rollerini etkinleştirme sayısı. |
| **Kullanıcılar kalıcı yaptı** | Uygun atamaya sahip kullanıcıların kalıcı hale getirilme sayısı. |
| **PıM 'de rol atamaları** | Kullanıcının PıM içinde uygun bir rolün atandığı zaman sayısı. |
| **PıM dışında rol atamaları** | Kullanıcının PıM dışında kalıcı bir rol atama sayısı (Azure AD içinde). |

**En üst roller bölümünün genel bakışı** , kiracınızdaki ilk beş rolü her bir rol için toplam kalıcı ve uygun yönetici sayısına göre listeler. **Eylem al** bağlantısı, kalıcı yöneticileri toplu gruplar halinde uygun yöneticilere dönüştürebileceğiniz [PIM Sihirbazı](pim-security-wizard.md) ' nı açar.

## <a name="pim-emails-for-azure-resource-roles"></a>Azure Kaynak rolleri için PıM e-postaları

PıM, Azure Kaynak rolleri için aşağıdaki olaylar gerçekleştiğinde Sahibe ve Kullanıcı erişimi yöneticilerine e-posta gönderir:

- Bir rol ataması onay bekliyor olduğunda
- Bir rol atandığında
- Bir rolün kullanım süresini yakında dolacak
- Bir rol genişletmeye uygun olduğunda
- Bir rol Son Kullanıcı tarafından yenilendiğinde
- Bir rol etkinleştirme isteği tamamlandığında

PıM, Azure Kaynak rolleri için aşağıdaki olaylar gerçekleştiğinde son kullanıcılara e-posta gönderir:

- Kullanıcıya bir rol atandığında
- Bir kullanıcının rolü süre dolduğunda
- Bir kullanıcının rolü genişletildiğinde
- Bir kullanıcının rol etkinleştirme isteği tamamlandığında

Aşağıda, bir kullanıcıya kurgusal contoso organizasyonu için bir Azure Kaynak rolü atandığında gönderilen örnek bir e-posta gösterilmektedir.

![Azure Kaynak rolleri için yeni PıM e-postası](./media/pim-email-notifications/email-resources-new.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure AD rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [PıM 'de Azure AD rolleri için istekleri onaylama veya reddetme](azure-ad-pim-approval-workflow.md)
