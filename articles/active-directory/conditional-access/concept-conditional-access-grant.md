---
title: Koşullu erişim ilkesinde denetim verme-Azure Active Directory
description: Azure AD koşullu erişim ilkesinde izin verme denetimleri
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0716c2d4475bb538c06b9a591521fbdcfc0c80e4
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79263445"
---
# <a name="conditional-access-grant"></a>Koşullu erişim: ver

Bir yönetici, koşullu erişim ilkesinde, kaynaklara erişim izni vermek veya erişimi engellemek için erişim denetimlerini kullanabilir.

![Multi-Factor Authentication gerektiren bir izin denetimi olan koşullu erişim ilkesi](./media/concept-conditional-access-grant/conditional-access-grant.png)

## <a name="block-access"></a>Erişimi engelle

Blok tüm atamaları hesaba ayırır ve koşullu erişim ilkesi yapılandırmasına bağlı olarak erişimi engeller.

Blok, uygun bilgilerle silinmeli güçlü bir denetimdir. Bir yönetici, etkinleştirmeden önce test etmek için [yalnızca rapor modunu](concept-conditional-access-report-only.md) kullanmalıdır.

## <a name="grant-access"></a>Erişim verme

Yöneticiler, erişim izni verirken bir veya daha fazla denetimi zorlamayı tercih edebilir. Bu denetimler aşağıdaki seçenekleri içerir: 

- [Multi-Factor Authentication gerektir (Azure Multi-Factor Authentication)](../authentication/concept-mfa-howitworks.md)
- [Cihazın uyumlu olarak işaretlenmesini gerektir (Microsoft Intune)](/intune/protect/device-compliance-get-started)
- [Karma Azure AD 'ye katılmış cihaz gerektir](../devices/concept-azure-ad-join-hybrid.md)
- [Onaylanan istemci uygulaması gerektir](app-based-conditional-access.md)
- [Uygulama koruma ilkesi gerektir](app-protection-based-conditional-access.md)

Yöneticiler bu seçenekleri birleştirmeyi seçerken, aşağıdaki yöntemleri seçebilirler:

- Seçili tüm denetimleri gerektir (Denetim **ve** denetim)
- Seçili denetimlerden birini gerektir (Denetim **veya** denetim)

Varsayılan olarak, koşullu erişim tüm seçili denetimleri gerektirir.

### <a name="require-multi-factor-authentication"></a>Multi-Factor Authentication gerektir

Bu onay kutusunun belirlenmesi, kullanıcıların Azure Multi-Factor Authentication gerçekleştirmesini gerektirir. Azure Multi-Factor Authentication dağıtımı hakkında daha fazla bilgi, [bulut tabanlı Azure Multi-Factor Authentication dağıtımını planlama](../authentication/howto-mfa-getstarted.md)makalesinde bulunabilir.

### <a name="require-device-to-be-marked-as-compliant"></a>Cihazın uyumlu olarak işaretlenmesini gerektir

Microsoft Intune dağıtılan kuruluşlar, belirli uyumluluk gereksinimlerini karşılayan cihazları tanımlamak için cihazlarından döndürülen bilgileri kullanabilir. Bu ilke uyumluluk bilgileri, Intune 'dan Azure AD 'ye iletilir ve Koşullu erişimin kaynaklara erişim izni vermek veya erişimi engellemek için kararlar verebilecekleri bir karardır. Uyumluluk ilkeleri hakkında daha fazla bilgi için, [Intune kullanarak kuruluşunuzdaki kaynaklara erişime izin vermek üzere cihazlarda kuralları ayarlama](/intune/protect/device-compliance-get-started)makalesine bakın.

Bir cihaz, Intune (herhangi bir cihaz işletim sistemi için) veya Windows 10 cihazları için üçüncü taraf MDM sistemi tarafından uyumlu olarak işaretlenebilir. JAMF Pro yalnızca desteklenen üçüncü taraf MDM sistemidir. Tümleştirme hakkında daha fazla bilgi için bkz. Bu makalede, [Uyumluluk Için JAMF Pro 'Yu Intune Ile tümleştirin](/intune/protect/conditional-access-integrate-jamf).

Cihazların uyumlu olarak işaretlenmeleri için önce Azure AD 'de kayıtlı olmaları gerekir. Cihaz kaydı hakkında daha fazla bilgi, makalesinde [cihaz kimliği nedir?](../devices/overview.md)makalesinde bulunabilir.

### <a name="require-hybrid-azure-ad-joined-device"></a>Karma Azure AD 'ye katılmış cihaz gerektir

Kuruluşlar, koşullu erişim ilkesinin bir parçası olarak cihaz kimliğini kullanmayı seçebilir. Kuruluşlar, bu onay kutusu kullanılarak cihazların karma Azure AD 'ye katılmış olmasını gerektirebilir. Cihaz kimlikleri hakkında daha fazla bilgi için, [cihaz kimliği nedir?](../devices/overview.md)makalesine bakın.

### <a name="require-approved-client-app"></a>Onaylı istemci uygulaması gerektir

Kuruluşlar, onaylanan bir istemci uygulamasından seçili bulut uygulamalarına yönelik bir erişim denemesinin yapılması gerektiğini gerektirebilir. Bu onaylanan istemci uygulamaları, mobil cihaz yönetimi (MDM) çözümünden bağımsız olarak [Intune uygulama koruma ilkelerini](/intune/app-protection-policy) destekler.

Bu ayar, aşağıdaki iOS ve Android uygulamaları için geçerlidir:

- Microsoft Azure Information Protection
- Microsoft kayıtları
- Microsoft Cortana
- Microsoft Dynamics 365
- Microsoft Edge
- Microsoft Excel
- Microsoft Flow
- Microsoft Intune Managed Browser
- Microsoft faturalama
- Microsoft Kaizala
- Microsoft Başlatıcısı
- Microsoft Office
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft PowerApps
- Microsoft Power BI
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype Kurumsal
- Microsoft StaffHub
- Microsoft Stream
- Microsoft Teams
- Microsoft To-Do
- Microsoft Visio
- Microsoft Word
- Microsoft Yammer

**Açıklamalar**

- Onaylanan istemci uygulamaları, Intune mobil uygulama yönetimi özelliğini destekler.
- **Onaylanan istemci uygulaması gereksinimini gerektir** :
   - Yalnızca iOS ve Android for Device platform koşulunu destekler.
- Koşullu erişim, onaylanan bir istemci uygulamasında Microsoft Edge 'i InPrivate modunda kabul edemez.

Yapılandırma örnekleri için [koşullu erişimle bkz. nasıl yapılır: Cloud App Access için onaylanan istemci uygulamaları gerektirme](app-based-conditional-access.md) .

### <a name="require-app-protection-policy"></a>Uygulama koruma ilkesi gerektir

Koşullu erişim ilkenizde, seçili bulut uygulamalarına erişim izni vermeden önce istemci uygulamada bir [Intune uygulama koruma ilkesinin](/intune/app-protection-policy) mevcut olmasını zorunlu kılabilirsiniz. 

Bu ayar, aşağıdaki istemci uygulamaları için geçerlidir:

- Microsoft Cortana
- Microsoft OneDrive
- Microsoft Outlook
- Microsoft Planner

**Açıklamalar**

- Uygulama koruma ilkesi için uygulamalar, ilke korumasıyla Intune mobil uygulama yönetimi özelliğini destekler.
- **Uygulama koruma ilkesi gereksinimlerini gerektir** :
    - Yalnızca iOS ve Android for Device platform koşulunu destekler.

Yapılandırma örnekleri için [koşullu erişimle bkz. nasıl yapılır: uygulama koruma Ilkesi gerektirme ve bulut uygulaması erişimi için onaylanan bir istemci uygulaması](app-protection-based-conditional-access.md) .

### <a name="terms-of-use"></a>Kullanım koşulları

Kuruluşunuz kullanım koşulları oluşturmışsa, denetim ver altında ek seçenekler görünür olabilir. Bu seçenekler, yöneticilerin ilke tarafından korunan kaynaklara erişme koşulu olarak kullanım koşulları onayını gerektirmesini sağlar. Kullanım koşulları hakkında daha fazla bilgi makalesinde [Azure Active Directory kullanım koşulları](terms-of-use.md)' nda bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim: oturum denetimleri](concept-conditional-access-session.md)

- [Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

- [Yalnızca rapor modu](concept-conditional-access-report-only.md)
