---
title: Azure Active Directory Kimlik Koruması bildirimleri
description: Bildirimlerin araştırma etkinliklerinizi nasıl desteklediğini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 06/05/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9090ca5b8057179b0cbef1d0a87ae563303ed2c1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85130441"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Kimlik Koruması bildirimleri

Azure AD Kimlik Koruması, Kullanıcı riskini ve risk algılamalarını yönetmenize yardımcı olmak üzere iki tür otomatik bildirim e-postası gönderir:

- Risk altındaki kullanıcılar e-posta algıladı
- Haftalık Özet e-postası

Bu makalede her iki bildirim e-postası için bir genel bakış sunulmaktadır.

## <a name="users-at-risk-detected-email"></a>Risk altındaki kullanıcılar e-posta algıladı

Risk altında algılanan bir hesaba yanıt olarak Azure AD Kimlik Koruması, **risk altındaki kullanıcılar** konu olarak algılanan bir e-posta uyarısı oluşturur. E-posta, **[risk için Işaretlenmiş kullanıcılar](../reports-monitoring/concept-user-at-risk.md)** raporu için bir bağlantı içerir. En iyi uygulama olarak, risk altındaki kullanıcıları hemen araştırmanız gerekir.

Bu uyarının yapılandırması, uyarının oluşturulmasını istediğiniz Kullanıcı risk düzeyini belirtmenize olanak tanır. Kullanıcının risk düzeyi belirtdiklerinize ulaştığında e-posta oluşturulacaktır. Örneğin, ilkeyi orta Kullanıcı riski üzerine uyarı olarak ayarlarsanız ve Kullanıcı John 'un Kullanıcı risk puanı, gerçek zamanlı bir oturum açma riski nedeniyle orta riske geçerse, risk altında algılanan kullanıcılara e-posta gönderilir. Kullanıcı risk düzeyi hesaplamasının belirtilen risk düzeyi (veya üzeri) olmasına neden olan bir sonraki risk algılamamız varsa, Kullanıcı risk puanı yeniden hesaplandığında, tehlikede daha fazla Kullanıcı görürsünüz. Örneğin, bir Kullanıcı 1 Ocak 'ta orta riske geçerse, ayarlarınız Orta risk üzerine uyarı olarak ayarlandıysa, bir e-posta bildirimi alırsınız. Aynı kullanıcı daha sonra da orta ölçekli bir risk algılamasında daha fazla risk algılamasına sahipse ve Kullanıcı risk puanı yeniden hesaplanmışsa ve hala orta düzeydeyse, başka bir e-posta bildirimi alırsınız. 

Bununla birlikte, yalnızca risk algılaması gerçekleştiği zaman (Kullanıcı risk düzeyindeki değişikliğe neden olan) son e-postanın gönderildiği zamana kıyasla daha yeni bir e-posta bildirimi gönderilir. Örneğin, 1 Ocak 'tan 5 ' te bir Kullanıcı oturum açma işlemleri ve gerçek zamanlı risk yoktur (Bu oturum açma nedeniyle hiçbir e-posta üretilmez). On dakika sonra, 5:10 ' de aynı Kullanıcı yeniden oturum açar ve gerçek zamanlı riske sahiptir ve Kullanıcı risk düzeyinin gönderilmek üzere yüksek ve e-posta gönderilmesine neden olur. Daha sonra, 5:15 ' de, özgün oturum açma için 5. adımda çevrimdışı risk puanı, çevrimdışı risk işleme nedeniyle yüksek riskli olarak değişir. İlk oturum açma zamanı, zaten bir e-posta bildirimi tetikleyen ikinci oturum açma işleminden önce olduğundan, risk e-postası için işaretlenen ilave Kullanıcı gönderilmez.

E-postaların aşırı yüklenmesini engellemek için, risk altında 5 saniyelik bir süre içinde yalnızca bir Kullanıcı tespit edersiniz. Bu, birden çok kullanıcı aynı 5 saniyelik bir dönemde belirtilen risk düzeyine taşındığında, hepsi için risk düzeyindeki değişikliği temsil eden bir e-posta toplayacağız ve gönderebiliyoruz.

![Risk altındaki kullanıcılar e-posta algıladı](./media/howto-identity-protection-configure-notifications/01.png)

### <a name="configure-users-at-risk-detected-alerts"></a>Risk halinde algılanan kullanıcıları yapılandırma uyarıları

Yönetici olarak şunları yapabilirsiniz:

- **Bu e-postanın oluşturulmasını tetikleyen Kullanıcı risk düzeyi** -varsayılan olarak, risk düzeyi "yüksek" riskli olarak ayarlanır.
- **Bu e-postanın alıcıları** varsayılan olarak tüm genel yöneticileri içerir. Genel Yöneticiler, diğer genel Yöneticiler, güvenlik yöneticileri, güvenlik okuyucularını da alıcı olarak ekleyebilir.
   - İsteğe bağlı olarak, **uyarı bildirimleri almak için ek e-postalar ekleyebilirsiniz** . Bu özellik bir önizlemedir ve tanımlanan kullanıcılar Azure Portal bağlantılı raporları görüntülemek için uygun izinlere sahip olmalıdır.

Risk altındaki **Azure Portal** Kullanıcı e-postalarını **Azure Active Directory**  >  **güvenlik**  >  **kimlik koruması**  >  **kullanıcıları tarafından algılanan uyarıları**yapılandırın.

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

**Azure portal** **Azure Active Directory**  >  **güvenlik**  >  **kimlik koruması**  >  **haftalık Özeti**altındaki Azure Portal Haftalık Özet e-postasını yapılandırın.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
