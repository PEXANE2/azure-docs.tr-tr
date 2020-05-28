---
title: Koşullu erişim-risk tabanlı koşullu erişim-Azure Active Directory
description: İlkeler için kimlik koruması geliştirmelerini etkinleştirmek üzere koşullu erişim ilkeleri oluşturma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9cfba377aba30d4687bab4ba7c5a311c70c4905
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83995165"
---
# <a name="conditional-access-risk-based-conditional-access"></a>Koşullu erişim: risk tabanlı koşullu erişim

Azure AD Premium P2 lisanslarına sahip kuruluşlar, Azure AD Kimlik Koruması riskli algılamaları dahil koşullu erişim ilkeleri oluşturabilir. Kutudan çıkan üç varsayılan ilke vardır. 

* Tüm kullanıcıların Azure Multi-Factor Authentication kaydolması gerekir.
* Yüksek riskli kullanıcılar için parola değişikliği gerektir.
* Orta veya yüksek oturum açma riski olan kullanıcılar için Multi-Factor Authentication gerektir.

## <a name="require-all-users-to-register-for-azure-multi-factor-authentication"></a>Tüm kullanıcıların Azure Multi-Factor Authentication kaydolmesini gerektir

Bu ilkeyi etkinleştirmek, tüm kullanıcıların Azure Multi-Factor Authentication için 14 gün içinde kaydolmanızı gerektirir. 

1. **Azure portalında** oturum açın.
1. **Tüm hizmetler**'e tıklayıp **Azure AD Kimlik Koruması**'na gidin.
1. **MFA kaydı**'na tıklayın.
1. **Atamalar**altında **Kullanıcılar**' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Dışla**altında hariç **tutulan kullanıcıları seç**' i seçin, kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin ve **Seç**' i seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Ilke uygulanmasını** **Açık**olarak ayarlayın.
1. **Kaydet**’e tıklayın.

## <a name="require-a-password-change-high-risk-users"></a>Parola değiştirme yüksek riskli kullanıcılar

Microsoft, kullanıcı adı ve parola çiftlerini bulma amacıyla araştırmacılar, kolluk kuvvetleri, Microsoft'taki çeşitli güvenlik ekipleri ve diğer güvenilir kaynaklarla birlikte çalışmalar yapmaktadır. Bu çiftlerden biri ortamınızdaki bir hesapla eşleştiğinde aşağıdaki ilkeyi kullanarak risk tabanlı parola değişikliğinin tetiklenmesini sağlayabilirsiniz.

1. **Azure portalında** oturum açın.
1. **Tüm hizmetler**'e tıklayıp **Azure AD Kimlik Koruması**'na gidin.
1. **Kullanıcı risk ilkesi**' ne tıklayın.
1. **Atamalar**altında **Kullanıcılar** ' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Dışla**altında hariç **tutulan kullanıcıları seç**' i seçin, kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin ve **Seç**' i seçin.
   1. **Done** (Bitti) öğesini seçin.
1. **Koşullar**' ın altında, **Kullanıcı riski**' nı seçin ve **yüksek**' ı seçin
   1. **Seç** **' e tıklayın**.
1. **Denetimler**  >  **erişimi**altında, erişime **izin ver**' i seçin ve ardından **parola değişikliği gerektir**' i seçin.
   1. **Seç**' e tıklayın.
1. **Ilke uygulanmasını** **Açık**olarak ayarlayın.
1. **Kaydet**’e tıklayın.

## <a name="require-mfa-medium-or-high-sign-in-risk-users"></a>MFA orta veya yüksek oturum açma risk kullanıcıları gerektir

Çoğu kullanıcı, takip edilebilen normal bir davranışa sahiptir ve davranışları normalin dışına çıktığında oturum açmalarına izin vermek riskli olabilir. Bu kullanıcıyı engellemek veya belki de gerçekten söylediklerini kanıtlamak üzere çok faktörlü kimlik doğrulaması gerçekleştirmesini istemeniz gerekebilir. Riskli oturum açma algılandığında MFA gerektiren ilkeyi kullanmak için aşağıdaki ilkeyi etkinleştirin.

1. **Azure portalında** oturum açın.
1. **Tüm hizmetler**'e tıklayıp **Azure AD Kimlik Koruması**'na gidin.
1. **Oturum açma risk ilkesine** tıklayın
1. **Atamalar**altında **Kullanıcılar** ' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Dışla**altında hariç **tutulan kullanıcıları seç**' i seçin, kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin ve **Seç**' i seçin.
   1. **Done** (Bitti) öğesini seçin.
1. **Koşullar**' ın altında, **oturum açma riski**' nı **ve ardından orta ve üst**' i seçin.
   1. **Seç** **' e tıklayın**.
1. **Denetimleri**  >  **erişimi**altında, erişime **izin ver**' i seçin ve ardından **çok faktörlü kimlik doğrulaması gerektir**' i seçin.
   1. **Seç**' e tıklayın.
1. **Ilke uygulanmasını** **Açık**olarak ayarlayın.
1. **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)

[Nasıl çalışır: Azure Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md)

[Azure Active Directory Kimlik Koruması nedir?](../identity-protection/overview.md)
