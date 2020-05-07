---
title: Azure Active Directory Kimlik Koruması bildirimleri
description: Bildirimlerin araştırma etkinliklerinizi nasıl desteklediğini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 200ede6b4c5565a8eab95b0398abaa1c056c612f
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82853145"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Kimlik Koruması bildirimleri

Azure AD Kimlik Koruması, Kullanıcı riskini ve risk algılamalarını yönetmenize yardımcı olmak üzere iki tür otomatik bildirim e-postası gönderir:

- Risk altındaki kullanıcılar e-posta algıladı
- Haftalık Özet e-postası

Bu makalede her iki bildirim e-postası için bir genel bakış sunulmaktadır.

## <a name="users-at-risk-detected-email"></a>Risk altındaki kullanıcılar e-posta algıladı

Risk altında algılanan bir hesaba yanıt olarak Azure AD Kimlik Koruması, **risk altındaki kullanıcılar** konu olarak algılanan bir e-posta uyarısı oluşturur. E-posta, **[risk için Işaretlenmiş kullanıcılar](../reports-monitoring/concept-user-at-risk.md)** raporu için bir bağlantı içerir. En iyi uygulama olarak, risk altındaki kullanıcıları hemen araştırmanız gerekir.

Bu uyarının yapılandırması, uyarının oluşturulmasını istediğiniz Kullanıcı risk düzeyini belirtmenize olanak tanır. Bu e-posta, kullanıcının risk düzeyi belirtdiklerinize ulaştığında oluşturulacaktır; Bununla birlikte, risk bu Kullanıcı risk düzeyine geçtikten sonra bu kullanıcı için e-posta uyarılarını tespit eden yeni kullanıcıları almazsınız. Örneğin, ilkeyi orta Kullanıcı riski üzerine uyarı olarak ayarlarsanız ve Kullanıcı John, orta riske geçerse, kullanıcılardan John için algılanan risk için e-posta olduğunu görürsünüz. Ancak, John daha sonra yüksek riske geçerse veya ek risk algılamaları varsa, risk tespit eden ikinci bir Kullanıcı almazsınız.

![Risk altındaki kullanıcılar e-posta algıladı](./media/howto-identity-protection-configure-notifications/01.png)

### <a name="configure-users-at-risk-detected-alerts"></a>Risk halinde algılanan kullanıcıları yapılandırma uyarıları

Yönetici olarak şunları yapabilirsiniz:

- **Bu e-postanın oluşturulmasını tetikleyen Kullanıcı risk düzeyi** -varsayılan olarak, risk düzeyi "yüksek" riskli olarak ayarlanır.
- **Bu e-postanın alıcıları** varsayılan olarak tüm genel yöneticileri içerir. Genel Yöneticiler, diğer genel Yöneticiler, güvenlik yöneticileri, güvenlik okuyucularını da alıcı olarak ekleyebilir.
   - İsteğe bağlı olarak, **uyarı bildirimleri almak için ek e-postalar ekleyebilirsiniz** . Bu özellik bir önizlemedir ve tanımlanan kullanıcılar Azure Portal bağlantılı raporları görüntülemek için uygun izinlere sahip olmalıdır.

Risk altındaki **Azure Portal** Kullanıcı e-postalarını **Azure Active Directory** > **güvenlik** > **kimlik koruması** > **kullanıcıları tarafından algılanan uyarıları**yapılandırın.

## <a name="weekly-digest-email"></a>Haftalık Özet e-postası

Haftalık Özet e-postası yeni risk algılamaları özetini içerir.  
Şunları içerir:

- Yeni riskli kullanıcılar algılandı
- Yeni riskli oturum açma işlemleri algılandı (gerçek zamanlı)
- Kimlik koruması 'nda ilgili raporların bağlantıları

![Haftalık Özet e-postası](./media/howto-identity-protection-configure-notifications/weekly-digest-email.png)

Varsayılan olarak, alıcılar tüm genel yöneticileri içerir. Genel Yöneticiler, diğer genel Yöneticiler, güvenlik yöneticileri, güvenlik okuyucularını da alıcı olarak ekleyebilir.

### <a name="configure-weekly-digest-email"></a>Haftalık Özet e-postasını Yapılandırma

Yönetici olarak, haftalık bir Özet e-postası göndermeyi veya kapatmayı değiştirebilir ve e-postayı almak için atanan kullanıcıları seçebilirsiniz.

 >  **Azure Active Directory****Security**güvenlik > **kimlik**koruması > **haftalık Özeti**altındaki **Azure Portal** Haftalık Özet e-postasını yapılandırın.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
