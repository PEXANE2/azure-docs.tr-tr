---
title: Koşullu erişim ilkesi oluşturma-Azure Active Directory
description: Koşullu erişim ilkesi oluşturmak için kullanılabilecek tüm seçenekler nelerdir ve ne anlama geliyor?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d2ebcc885b4018f4d9c3ff1b525ffc19b1abdda
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78671919"
---
# <a name="building-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

[Koşullu erişim olan](overview.md)makalede açıklandığı gibi, koşullu erişim ilkesi **atamalar** ve **erişim denetimleri**için bir if-then deyimidir. Koşullu erişim ilkesi, kararları almak, kararlar almak ve kuruluş ilkelerini zorlamak için sinyalleri bir araya getirir.

Bir kuruluş bu ilkeleri nasıl oluşturur? Ne gerekir?

![Koşullu erişim (sinyaller + kararlar + zorlama = Ilkeler)](./media/concept-conditional-access-policies/conditional-access-signal-decision-enforcement.png)

## <a name="assignments"></a>Atamalar

Atamalar bölümü, koşullu erişim ilkesinin kim, ne olduğunu ve nerede olduğunu denetler.

### <a name="users-and-groups"></a>Kullanıcılar ve gruplar

[Kullanıcılar ve gruplar](concept-conditional-access-users-groups.md) , ilkenin kimlerin ekleneceğini veya dışlanacağını atar. Bu atama tüm kullanıcıları, belirli kullanıcı gruplarını, Dizin rollerini veya dış Konuk kullanıcıları içerebilir. 

### <a name="cloud-apps-or-actions"></a>Bulut uygulamaları veya eylemleri

[Bulut uygulamaları veya eylemleri](concept-conditional-access-cloud-apps.md) , ilke ile ilgili olacak bulut uygulamalarını veya kullanıcı eylemlerini içerebilir veya hariç tutabilir.

### <a name="conditions"></a>Koşullar

Bir ilke birden çok [koşul](concept-conditional-access-conditions.md)içerebilir.

#### <a name="sign-in-risk"></a>Oturum açma riski

[Azure AD kimlik koruması](../identity-protection/overview.md)olan kuruluşlar için, üretilen risk algılamaları koşullu erişim ilkelerinizi etkileyebilir.

#### <a name="device-platforms"></a>Cihaz platformları

Birden çok cihaz işletim sistemi platformu olan kuruluşlar, farklı platformlarda belirli ilkeleri zorlamak isteyebilir. 

Cihaz platformunu hesaplamak için kullanılan bilgiler, değiştirilebilen Kullanıcı Aracısı dizeleri gibi doğrulanmamış kaynaklardan gelir.

#### <a name="locations"></a>Konumlar

Konum verileri, IP coğrafi konum verileri tarafından sağlanır. Yöneticiler, konumları tanımlayabilir ve bazı içerikleri kuruluşun ağ konumları gibi güvenilir olarak işaretlemek için seçim yapabilir.

#### <a name="client-apps"></a>İstemci uygulamaları

Varsayılan olarak, koşullu erişim ilkeleri tarayıcı uygulamaları, mobil uygulamalar ve modern kimlik doğrulamasını destekleyen masaüstü istemcileri için geçerlidir. 

Bu atama koşulu, koşullu erişim ilkelerinin modern kimlik doğrulaması kullanmayan belirli istemci uygulamalarını hedeflemesini sağlar. Bu uygulamalar Exchange ActiveSync istemcileri, modern kimlik doğrulaması kullanmayan eski Office uygulamaları ve IMAP, MAPI, POP ve SMTP gibi posta protokolleri içerir.

#### <a name="device-state"></a>Cihaz durumu

Bu denetim, karma Azure AD 'ye katılmış olan veya Intune 'da uyumlu bir şekilde işaretlenmiş cihazları dışlamak için kullanılır. Bu dışlama, yönetilmeyen cihazları engellemek için yapılabilir. 

## <a name="access-controls"></a>Erişim denetimleri

Koşullu erişim ilkesinin erişim denetimleri bölümü bir ilkenin nasıl uygulanacağını denetler.

### <a name="grant"></a>İzin verme

[Grant](concept-conditional-access-grant.md) , yöneticilere, erişimi engelleyebilecekleri veya izin verebileceği bir ilke zorlamasına yol sunar.

#### <a name="block-access"></a>Erişimi engelle

Erişimi engelle, yalnızca belirtilen atamalar altındaki erişimi engeller. Blok denetimi güçlüdür ve uygun bilgilerle silinmeli.

#### <a name="grant-access"></a>Erişim verme

İzin denetimi bir veya daha fazla denetimin zorlanmasını tetikleyebilir. 

- Multi-Factor Authentication gerektir (Azure Multi-Factor Authentication)
- Cihazın uyumlu olarak işaretlenmesini gerektir (Intune)
- Karma Azure AD 'ye katılmış cihaz gerektir
- Onaylı istemci uygulaması gerektir
- Uygulama koruma ilkesi gerektir

Yöneticiler, aşağıdaki seçenekleri kullanarak önceki denetimlerden birini veya seçili tüm denetimleri zorunlu kılabilir. Birden çok denetim için varsayılan değer, tümü için gerekli değildir.

- Seçili tüm denetimleri gerektir (denetim ve denetim)
- Seçili denetimlerden birini gerektir (denetim veya denetim)

### <a name="session"></a>Oturum

[Oturum denetimleri](concept-conditional-access-session.md) deneyimi sınırlayabilir 

- Uygulama tarafından zorlanan kısıtlamaları kullan
   - Şu anda yalnızca Exchange Online ve SharePoint Online ile birlikte çalışıyor.
      - Tam veya sınırlı erişim verme deneyiminin denetimine izin vermek için cihaz bilgilerini geçirir.
- Koşullu Erişim Uygulama Denetimi Kullan
   - , Şunun gibi işlemleri yapmak için Microsoft Cloud App Security sinyalleri kullanır: 
      - Gizli belgeleri indirme, kesme, kopyalama ve yazdırma.
      - Riskli oturum davranışını izleyin.
      - Gizli dosyaların etiketlenmesini gerektir.
- Oturum açma sıklığı
   - Modern kimlik doğrulaması için varsayılan oturum açma sıklığını değiştirme özelliği.
- Kalıcı tarayıcı oturumu
   - Kullanıcıların tarayıcı pencerelerini kapatıp yeniden açtıktan sonra oturum açmasına izin verir.

## <a name="simple-policies"></a>Basit ilkeler

Koşullu erişim ilkesi, zorlanmak için en azından aşağıdakileri içermelidir:

- İlkenin **adı** .
- **Atamalar**
   - İlkenin uygulanacağı **Kullanıcılar ve/veya gruplar** .
   - İlkeyi uygulamak için **bulut uygulamaları veya eylemleri** .
- **Erişim denetimleri**
   - Denetimleri **verme** veya **engelleme**

![Boş koşullu erişim ilkesi](./media/concept-conditional-access-policies/conditional-access-blank-policy.png)

[Genel koşullu erişim ilkeleri](concept-conditional-access-policy-common.md) makalesi, çoğu kuruluş için faydalı olacağını düşündük bazı ilkeleri içerir.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)

[Bulut tabanlı bir Azure Multi-Factor Authentication dağıtımı planlama](../authentication/howto-mfa-getstarted.md)

[Intune ile cihaz uyumluluğunu yönetme](/intune/device-compliance-get-started)

[Microsoft Cloud App Security ve koşullu erişim](/cloud-app-security/proxy-intro-aad)
