---
title: Koşullu Erişim ilkesi oluşturma - Azure Etkin Dizini
description: Koşullu Erişim ilkesi oluşturmak için kullanılabilen seçenekler nelerdir ve bunlar ne anlama gelir?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 907ad8705742e4b2e38b13c3c675ebd333bd27d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295316"
---
# <a name="building-a-conditional-access-policy"></a>Koşullu Erişim ilkesi oluşturma

Makalede açıklandığı gibi [Koşullu Erişim nedir](overview.md), Koşullu Erişim ilkesi atamalar ve **Erişim denetimleri**bir if-then deyimidir. **Assignments** Koşullu Erişim ilkesi sinyalleri bir araya getirir, kararlar verir ve kuruluş ilkelerini uygular.

Bir kuruluş bu ilkeleri nasıl oluşturur? Ne gereklidir?

![Koşullu Erişim (Sinyaller + Kararlar + Uygulama = İlkeler)](./media/concept-conditional-access-policies/conditional-access-signal-decision-enforcement.png)

## <a name="assignments"></a>Atamalar

Atamalar bölümü Koşullu Erişim ilkesinin kim, ne ve nerede olduğunu denetler.

### <a name="users-and-groups"></a>Kullanıcılar ve gruplar

[Kullanıcılar ve gruplar,](concept-conditional-access-users-groups.md) poliçenin kimi dahil edeceğini veya hariç tutacağını atar. Bu atama tüm kullanıcıları, belirli kullanıcı gruplarını, dizin rollerini veya dış konuk kullanıcıları içerebilir. 

### <a name="cloud-apps-or-actions"></a>Bulut uygulamaları veya eylemleri

[Bulut uygulamaları veya eylemleri,](concept-conditional-access-cloud-apps.md) ilkeye tabi olacak bulut uygulamalarını veya kullanıcı eylemlerini içerebilir veya hariç tutabilir.

### <a name="conditions"></a>Koşullar

Bir ilke birden çok [koşul](concept-conditional-access-conditions.md)içerebilir.

#### <a name="sign-in-risk"></a>Oturum açma riski

[Azure AD Kimlik Koruması](../identity-protection/overview.md)olan kuruluşlar için, burada oluşturulan risk algılamaları Koşullu Erişim ilkelerinizi etkileyebilir.

#### <a name="device-platforms"></a>Cihaz platformları

Birden çok aygıt işletim sistemi platformuna sahip kuruluşlar, farklı platformlarda belirli ilkeleri uygulamak isteyebilir. 

Aygıt platformını hesaplamak için kullanılan bilgiler, değiştirilebilen kullanıcı aracı dizeleri gibi doğrulanmamış kaynaklardan gelir.

#### <a name="locations"></a>Konumlar

Konum verileri IP coğrafi konum verileri tarafından sağlanır. Yöneticiler konumları tanımlamayı ve bazılarını kuruluşlarının ağ konumları için güvenilir olarak işaretlemeyi seçebilir.

#### <a name="client-apps"></a>İstemci uygulamaları

Varsayılan olarak Koşullu Erişim ilkeleri, modern kimlik doğrulamasını destekleyen tarayıcı uygulamaları, mobil uygulamalar ve masaüstü istemcileri için geçerlidir. 

Bu atama koşulu, Koşullu Erişim ilkelerinin modern kimlik doğrulaması kullanmayan belirli istemci uygulamalarını hedeflemesine olanak tanır. Bu uygulamalar Exchange ActiveSync istemcilerini, modern kimlik doğrulamasını kullanmayan eski Office uygulamalarını ve IMAP, MAPI, POP ve SMTP gibi posta protokollerini içerir.

#### <a name="device-state"></a>Aygıt durumu

Bu denetim, karma Azure AD'sı bağlanan veya Intune'da uyumlu olarak işaretlenmiş aygıtları hariç tutmak için kullanılır. Bu dışlama, yönetilmeyen aygıtları engellemek için yapılabilir. 

## <a name="access-controls"></a>Erişim denetimleri

Koşullu Erişim ilkesinin erişim denetimleri, bir ilkenin nasıl uygulanacağını denetler.

### <a name="grant"></a>İzin verme

[Grant,](concept-conditional-access-grant.md) yöneticilere erişimi engelleyebilir veya izin verebilecekleri bir ilke zorlama aracı sağlar.

#### <a name="block-access"></a>Hizmete erişimi

Blok erişimi sadece bunu yapar, belirtilen atamalar altında erişimi engeller. Blok kontrolü güçlüdür ve uygun bilgi ile kullanılmalıdır.

#### <a name="grant-access"></a>Erişim verme

Hibe denetimi bir veya daha fazla denetimin uygulanmasını tetikleyebilir. 

- Çok faktörlü kimlik doğrulama (Azure Çok Faktörlü Kimlik Doğrulama) gerektirir
- Aygıtın uyumlu olarak işaretlemesi gerekir (Intune)
- Karma Azure AD'nin birleştirilmiş aygıtı gerektirmesi
- Onaylı istemci uygulaması gerektirir
- Uygulama koruma ilkesi gerektir

Yöneticiler, aşağıdaki seçenekleri kullanarak önceki denetimlerden birini veya seçili tüm denetimlerden birini almayı seçebilir. Birden çok denetim için varsayılan tüm gerektirir.

- Seçili tüm denetimleri (kontrol ve denetim) gerektirir
- Seçili denetimlerden birini (denetim veya denetim) gerektirir

### <a name="session"></a>Oturum

[Oturum denetimleri](concept-conditional-access-session.md) deneyimi sınırlayabilir 

- Uygulama nın zorlanmış kısıtlamalarını kullanma
   - Şu anda yalnızca Exchange Online ve SharePoint Online ile çalışmaktadır.
      - Tam veya sınırlı erişim sağlayan deneyimdenetimine izin vermek için aygıt bilgilerini aktarın.
- Koşullu Erişim Uygulama Denetimini Kullanın
   - Microsoft Cloud App Security'den gelen sinyalleri şu gibi şeyler yapmak için kullanır: 
      - Hassas belgelerin indirin, kesmesini, kopyalatılmasını ve yazdırılmasını engelleyin.
      - Riskli oturum davranışını izleyin.
      - Hassas dosyaların etiketlemesi gerekir.
- Oturum açma sıklığı
   - Modern kimlik doğrulama için varsayılan işareti frekansta değiştirme yeteneği.
- Kalıcı tarayıcı oturumu
   - Kullanıcıların tarayıcı pencerelerini kapatıp yeniden açtıktan sonra oturum açmalarını sağlar.

## <a name="simple-policies"></a>Basit ilkeler

Koşullu Erişim ilkesi, uygulanacak en az aşağıdakileri içermelidir:

- Poliçenin **adı.**
- **Atama**
   - İlkeyi uygulayacak **kullanıcılar ve/veya gruplar.**
   - İlkeyi uygulamak için **bulut uygulamaları veya eylemleri.**
- **Erişim denetimleri**
   - **Hibe** veya **Blok** denetimleri

![Boş Koşullu Erişim ilkesi](./media/concept-conditional-access-policies/conditional-access-blank-policy.png)

Makale [Ortak Koşullu Erişim ilkeleri,](concept-conditional-access-policy-common.md) çoğu kuruluş için yararlı olacağını düşündüğümüz bazı ilkeler içerir.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim Ne Varsa aracını kullanarak oturum açma davranışını simüle edin](troubleshoot-conditional-access-what-if.md)

[Bulut tabanlı Azure Multi-Factor Authentication dağıtımı planlama](../authentication/howto-mfa-getstarted.md)

[Intune ile cihaz uyumluluğunu yönetme](/intune/device-compliance-get-started)

[Microsoft Cloud App Security ve Koşullu Erişim](/cloud-app-security/proxy-intro-aad)
