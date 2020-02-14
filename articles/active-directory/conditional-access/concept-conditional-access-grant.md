---
title: Koşullu erişim ilkesinde denetim verme-Azure Active Directory
description: Azure AD koşullu erişim ilkesinde izin verme denetimleri
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
ms.openlocfilehash: 89063cc8131c28f20153c6fe9b4c71b58794e609
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77192122"
---
# <a name="conditional-access-grant"></a>Koşullu erişim: ver

Bir yönetici, koşullu erişim ilkesinde, kaynaklara erişim izni vermek veya erişimi engellemek için erişim denetimlerini kullanabilir.

![Multi-Factor Authentication gerektiren bir izin denetimi olan koşullu erişim ilkesi](./media/concept-conditional-access-grant/conditional-access-grant.png)

## <a name="block-access"></a>Erişimi engelle

Blok tüm atamaları hesaba ayırır ve koşullu erişim ilkesi yapılandırmasına bağlı olarak erişimi engeller.

Blok, uygun bilgilerle silinmeli güçlü bir denetimdir. Bir yönetici, etkinleştirmeden önce test etmek için [yalnızca rapor modunu](concept-conditional-access-report-only.md) kullanmalıdır.

## <a name="grant-access"></a>Erişim izni verin

Yöneticiler, erişim izni verirken bir veya daha fazla denetimi zorlamayı tercih edebilir. Bu denetimler aşağıdaki seçenekleri içerir: 

- [Multi-Factor Authentication gerektir (Azure Multi-Factor Authentication)](../authentication/concept-mfa-howitworks.md)
- [Cihazın uyumlu olarak işaretlenmesini gerektir (Microsoft Intune)](https://docs.microsoft.com/intune/protect/device-compliance-get-started)
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

Microsoft Intune dağıtılan kuruluşlar, belirli uyumluluk gereksinimlerini karşılayan cihazları tanımlamak için cihazlarından döndürülen bilgileri kullanabilir. Bu ilke uyumluluk bilgileri, Intune 'dan Azure AD 'ye iletilir ve Koşullu erişimin kaynaklara erişim izni vermek veya erişimi engellemek için kararlar verebilecekleri bir karardır. Uyumluluk ilkeleri hakkında daha fazla bilgi için, [Intune kullanarak kuruluşunuzdaki kaynaklara erişime izin vermek üzere cihazlarda kuralları ayarlama](https://docs.microsoft.com/intune/protect/device-compliance-get-started)makalesine bakın.

### <a name="require-hybrid-azure-ad-joined-device"></a>Karma Azure AD 'ye katılmış cihaz gerektir

Kuruluşlar, koşullu erişim ilkesinin bir parçası olarak cihaz kimliğini kullanmayı seçebilir. Kuruluşlar, bu onay kutusu kullanılarak cihazların karma Azure AD 'ye katılmış olmasını gerektirebilir. Cihaz kimlikleri hakkında daha fazla bilgi için, [cihaz kimliği nedir?](../devices/overview.md)makalesine bakın.

### <a name="require-approved-client-app"></a>Onaylı istemci uygulaması gerektir

Kuruluşlar, onaylanan bir istemci uygulamasından seçili bulut uygulamalarına yönelik bir erişim denemesinin yapılması gerektiğini gerektirebilir.

Bu ayar, aşağıdaki istemci uygulamaları için geçerlidir:

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

### <a name="require-app-protection-policy"></a>Uygulama koruma ilkesi gerektir

Koşullu erişim ilkenizde, seçili bulut uygulamalarına erişim izni vermeden önce istemci uygulamada bir uygulama koruma ilkesinin mevcut olmasını zorunlu kılabilirsiniz. 

![Uygulama koruma ilkesiyle erişimi denetleme](./media/technical-reference/22.png)

Bu ayar, aşağıdaki istemci uygulamaları için geçerlidir:

- Microsoft Cortana
- Microsoft OneDrive
- Microsoft Outlook
- Microsoft Planner

**Açıklamalar**

- Uygulama koruma ilkesi için uygulamalar, ilke korumasıyla Intune mobil uygulama yönetimi özelliğini destekler.
- **Uygulama koruma ilkesi gereksinimlerini gerektir** :
    - Yalnızca iOS ve Android for Device platform koşulunu destekler.

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim: oturum denetimleri](concept-conditional-access-session.md)

- [Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

- [Yalnızca rapor modu](concept-conditional-access-report-only.md)
