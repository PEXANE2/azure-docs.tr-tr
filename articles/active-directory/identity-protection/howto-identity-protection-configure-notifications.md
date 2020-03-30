---
title: Azure Active Directory Identity Protection bildirimleri
description: Bildirimlerin araştırma faaliyetlerinizi nasıl desteklediğini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c83aa6e476bbd898999fb6efe490c7847a809ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77120119"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Identity Protection bildirimleri

Azure AD Kimlik Koruması, kullanıcı risk ve risk algılamalarını yönetmenize yardımcı olmak için iki tür otomatik bildirim e-postası gönderir:

- Risk altındaki kullanıcılar e-posta algılandı
- Haftalık özet e-posta

Bu makalede, her iki bildirim e-postaları genel bir bakış sağlar.

## <a name="users-at-risk-detected-email"></a>Risk altındaki kullanıcılar e-posta algılandı

Azure AD Kimlik Koruması, risk altındaki bir hesaba yanıt olarak, konu olarak **algılanması riski olan Kullanıcılarla** ilgili bir e-posta uyarısı oluşturur. E-posta, risk raporu **[için işaretlenen Kullanıcılar'a](../reports-monitoring/concept-user-at-risk.md)** bir bağlantı içerir. En iyi uygulama olarak, risk altındaki kullanıcıları derhal araştırmanız gerekir.

Bu uyarının yapılandırması, uyarının oluşturulmasını istediğiniz kullanıcı risk düzeyinde belirtmenize olanak tanır. E-posta, kullanıcının risk düzeyi belirttiğiniz seviyeye ulaştığında oluşturulur; ancak, bu kullanıcı risk düzeyine geçtikten sonra bu kullanıcı için algılanan e-posta uyarıları riski olan yeni kullanıcılar almazsınız. Örneğin, politikayı orta kullanıcı riski konusunda uyaracak şekilde ayarlarsanız ve kullanıcınız John orta riske taşınırsa, risk altındaki kullanıcıları John için algılanan e-posta alırsınız. Ancak, John daha sonra yüksek risk taşır veya ek risk algılamaları varsa risk tespit uyarı ikinci bir kullanıcı almazsınız.

![Risk altındaki kullanıcılar e-posta algılandı](./media/howto-identity-protection-configure-notifications/01.png)

### <a name="configure-users-at-risk-detected-alerts"></a>Algılanan uyarıları risk altındaki kullanıcılara yapılandırma

Yönetici olarak şunları ayarlayabilirsiniz:

- **Bu e-postanın oluşumunu tetikleyen kullanıcı risk düzeyi** - Varsayılan olarak, risk düzeyi "Yüksek" risk olarak ayarlanır.
- **Bu e-postanın alıcıları** - Varsayılan olarak, alıcılar tüm Global Yöneticileri içerir. Global Yöneticiler ayrıca diğer Global Yöneticileri, Güvenlik Yöneticilerini, Güvenlik Okuyucularını alıcı olarak da ekleyebilir.
   - İsteğe bağlı olarak **uyarı bildirimleri almak için ek e-postalar ekleyebilirsiniz** bu özellik bir önizlemedir ve tanımlanan kullanıcıların Azure portalında bağlantılı raporları görüntülemek için uygun izinlere sahip olması gerekir.

Azure Active **Directory** > **Security** > **Identity Protection** > **Users kapsamında** **Azure portalında** risk altındaki kullanıcıları e-posta yla yapılandırın

## <a name="weekly-digest-email"></a>Haftalık özet e-posta

Haftalık özet e-posta, yeni risk tespitlerinin bir özetini içerir.  
Bu içerir:

- Risk altındaki kullanıcılar
- Şüpheli faaliyetler
- Algılanan güvenlik açıkları
- Kimlik Koruması'nda ilgili raporlara bağlantılar

![Haftalık özet e-posta](./media/howto-identity-protection-configure-notifications/400.png)

Varsayılan olarak, alıcılar tüm Global Yöneticileri içerir. Global Yöneticiler ayrıca diğer Global Yöneticileri, Güvenlik Yöneticilerini, Güvenlik Okuyucularını alıcı olarak da ekleyebilir.

### <a name="configure-weekly-digest-email"></a>Haftalık özet e-postayı yapılandırma

Yönetici olarak, haftalık özet e-posta göndermeyi açabilir veya kapatabilir ve e-postayı almak için atanan kullanıcıları seçebilirsiniz.

Azure Active **Directory** > **Security** > **Identity Protection** > **Weekly özeti**altında **Azure portalında** haftalık özet e-postasını yapılandırın.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
