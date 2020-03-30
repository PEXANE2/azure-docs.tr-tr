---
title: Koşullu Erişim ilkesinde hibe denetimleri - Azure Etkin Dizini
description: Azure AD Koşullu Erişim ilkesinde hibe denetimleri nelerdir
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
ms.openlocfilehash: 02ec8dace971cd4dc1407c9e8d20839504c9ecc3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331848"
---
# <a name="conditional-access-grant"></a>Koşullu Erişim: Hibe

Koşullu Erişim ilkesi içinde, yönetici kaynaklara erişimi vermek veya engellemek için erişim denetimlerinden yararlanabilir.

![Çok faktörlü kimlik doğrulaması gerektiren hibe denetimine sahip Koşullu Erişim ilkesi](./media/concept-conditional-access-grant/conditional-access-grant.png)

## <a name="block-access"></a>Hizmete erişimi

Blok, tüm atamaları dikkate alır ve Koşullu Erişim ilkesi yapılandırmasına dayalı erişimi engeller.

Blok uygun bilgi ile wielded gereken güçlü bir kontroldür. Bu, yöneticilerin etkinleştirmeden önce sınamak için [yalnızca Rapor modunu](concept-conditional-access-report-only.md) kullanmaları gereken bir şeydir.

## <a name="grant-access"></a>Erişim verme

Yöneticiler erişim verirken bir veya daha fazla denetim uygulamayı seçebilir. Bu denetimler aşağıdaki seçenekleri içerir: 

- [Çok faktörlü kimlik doğrulama (Azure Çok Faktörlü Kimlik Doğrulama) gerektirir](../authentication/concept-mfa-howitworks.md)
- [Aygıtın uyumlu olarak işaretlemesi gerekir (Microsoft Intune)](/intune/protect/device-compliance-get-started)
- [Karma Azure AD'nin birleştirilmiş aygıta katılmasını gerektirin](../devices/concept-azure-ad-join-hybrid.md)
- [Onaylı istemci uygulaması gerektirir](app-based-conditional-access.md)
- [Uygulama koruma ilkesi gerektir](app-protection-based-conditional-access.md)

Yöneticiler bu seçenekleri birleştirmeyi seçtiklerinde aşağıdaki yöntemleri seçebilirler:

- Seçili tüm denetimleri (kontrol **ve** kontrol) gerektirir
- Seçili denetimlerden birini (kontrol **VEYA** denetim) gerektirir

Varsayılan olarak Koşullu Erişim tüm seçili denetimleri gerektirir.

### <a name="require-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulama gerektirir

Bu onay kutusunun seçilmesi, kullanıcıların Azure Çok Faktörlü Kimlik Doğrulaması gerçekleştirmesini gerektirir. Azure Çok Faktörlü Kimlik Doğrulama'yı dağıtma hakkında daha fazla bilgiyi [bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulama dağıtımını Planlama](../authentication/howto-mfa-getstarted.md)makalesinde bulabilirsiniz.

### <a name="require-device-to-be-marked-as-compliant"></a>Aygıtın uyumlu olarak işaretlemesi

Microsoft Intune'u dağıtan kuruluşlar, aygıtlarından döndürülen bilgileri, belirli uyumluluk gereksinimlerini karşılayan aygıtları tanımlamak için kullanabilir. Bu ilke uyumluluk bilgileri, Koşullu Erişim'in kaynaklara erişim izni vermek veya engellemek için kararlar alabilmesi için Intune'dan Azure AD'ye iletilir. Uyumluluk ilkeleri hakkında daha fazla bilgi için, [Intune'u kullanarak kuruluşunuzdaki kaynaklara erişime izin vermek için aygıtlarda kuralları ayarla makalesine](/intune/protect/device-compliance-get-started)bakın.

Aygıt, Intune (herhangi bir aygıt işletim sistemi için) veya Windows 10 aygıtları için üçüncü taraf MDM sistemi tarafından uyumlu olarak işaretlenebilir. Jamf pro desteklenen tek üçüncü taraf MDM sistemidir. Entegrasyon hakkında daha fazla bilgi makalede bulunabilir, [Uyum için Intune ile Jamf Pro Entegre](/intune/protect/conditional-access-integrate-jamf).

Aygıtların uyumlu olarak işaretlenebilmeleri için Azure AD'ye kaydedilmesi gerekir. Cihaz kaydı hakkında daha fazla bilgi makalede bulunabilir, [Bir cihaz kimliği nedir](../devices/overview.md).

### <a name="require-hybrid-azure-ad-joined-device"></a>Karma Azure AD'nin birleştirilmiş aygıta katılmasını gerektirin

Kuruluşlar, Koşullu Erişim ilkesinin bir parçası olarak aygıt kimliğini kullanmayı seçebilir. Kuruluşlar, aygıtların bu onay kutusunu kullanarak birleştirilmiş karma Azure AD'sı olduğunu gerektirebilir. Aygıt kimlikleri hakkında daha fazla bilgi için makaleye bakın [Aygıt kimliği nedir?](../devices/overview.md)

### <a name="require-approved-client-app"></a>Onaylı istemci uygulaması gerektirir

Kuruluşlar, seçili bulut uygulamalarına erişim girişiminin onaylanmış bir istemci uygulamasından yapılmasını gerektirebilir. Bu onaylı istemci uygulamaları, herhangi bir mobil cihaz yönetimi (MDM) çözümünden bağımsız olarak [Intune uygulama koruma ilkelerini](/intune/app-protection-policy) destekler.

Bu hibe denetiminden yararlanmak için Koşullu Erişim, aygıtın bir broker uygulamasının kullanılmasını gerektiren Azure Etkin Dizini'ne kaydolmasını gerektirir. Aracı uygulama, iOS için Microsoft Authenticator ya da Android cihazlar için Microsoft Şirket portalı olabilir. Kullanıcı kimlik doğrulaması yapmaya çalıştığında cihaza bir broker uygulaması yüklenmezse, kullanıcı broker uygulamasını yüklemek için uygulama mağazasına yönlendirilir.

Bu ayar aşağıdaki iOS ve Android uygulamaları için geçerlidir:

- Microsoft Azure Bilgi Koruması
- Microsoft Rezervasyonları
- Microsoft Cortana
- Microsoft Dynamics 365
- Microsoft Edge
- Microsoft Excel
- Microsoft Flow
- Microsoft Intune Yönetilen Tarayıcı
- Microsoft Faturalama
- Microsoft Kaizala
- Microsoft Başlatıcısı
- Microsoft Office
- Microsoft Office Hub
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planlayıcısı
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
- Microsoft Beyaz Tahta

**Açıklamalar**

- Onaylanan istemci uygulamaları Intune mobil uygulama yönetimi özelliğini destekler.
- Onaylı istemci uygulaması gereksinimi **gerektirir:**
   - Yalnızca aygıt platformu durumu için iOS ve Android'i destekler.
   - Cihazı kaydetmek için bir broker uygulaması gereklidir. iOS'ta, broker uygulaması Microsoft Authenticator ve Android, bu Intune Company Portal uygulamasıdır.
- Koşullu Erişim, InPrivate modunda Microsoft Edge'i onaylanmış bir istemci uygulaması olarak kabul edemez.

Makaleye bakın, Nasıl Yapılışı: Yapılandırma örnekleri [için Koşullu Erişim ile bulut uygulaması erişimi için onaylı istemci uygulamalarını zorunlu kılmasını sağlayın.](app-based-conditional-access.md)

### <a name="require-app-protection-policy"></a>Uygulama koruma ilkesi gerektir

Koşullu Erişim politikanızda, seçili bulut uygulamalarına erişilmeden önce istemci uygulamasında bir [Intune uygulaması koruma ilkesinin](/intune/app-protection-policy) bulunmasını talep edebilirsiniz. 

Bu hibe denetiminden yararlanmak için Koşullu Erişim, aygıtın bir broker uygulamasının kullanılmasını gerektiren Azure Etkin Dizini'ne kaydolmasını gerektirir. Aracı uygulama, iOS için Microsoft Authenticator ya da Android cihazlar için Microsoft Şirket portalı olabilir. Kullanıcı kimlik doğrulaması yapmaya çalıştığında cihaza bir broker uygulaması yüklenmezse, kullanıcı broker uygulamasını yüklemek için uygulama mağazasına yönlendirilir.

Bu ayar aşağıdaki istemci uygulamaları için geçerlidir:

- Microsoft Cortana
- Microsoft OneDrive
- Microsoft Outlook
- Microsoft Planlayıcısı

**Açıklamalar**

- Uygulama koruma ilkesi ne yönelik uygulamalar, intune mobil uygulama yönetimi özelliğini politika korumasıyla destekler.
- Uygulama koruma ilkesi gereksinimleri **gerektirir:**
    - Yalnızca aygıt platformu durumu için iOS ve Android'i destekler.
    - Cihazı kaydetmek için bir broker uygulaması gereklidir. iOS'ta, broker uygulaması Microsoft Authenticator ve Android, bu Intune Company Portal uygulamasıdır.

Makaleye bakın, [Nasıl YapılSın: Yapılandırma örnekleri için Koşullu Erişim ile bulut uygulamasıerişimi için uygulama koruma ilkesini ve onaylı bir istemci uygulamasını zorunlu kılmasını sağlayın.](app-protection-based-conditional-access.md)

### <a name="terms-of-use"></a>Kullanım koşulları

Kuruluşunuz kullanım koşulları oluşturduysa, hibe denetimleri altında ek seçenekler görülebilir. Bu seçenekler, yöneticilerin ilke tarafından korunan kaynaklara erişim koşulu olarak kullanım koşullarının kabul edilmesini gerektirmesine olanak sağlar. Kullanım koşulları hakkında daha fazla bilgi makalede bulunabilir, [Azure Active Directory kullanım koşulları](terms-of-use.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu Erişim: Oturum denetimleri](concept-conditional-access-session.md)

- [Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

- [Yalnızca rapor modu](concept-conditional-access-report-only.md)
