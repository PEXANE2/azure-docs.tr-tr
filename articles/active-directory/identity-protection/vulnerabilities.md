---
title: Azure Active Directory Kimlik Koruması tarafından algılanan güvenlik açıkları
description: Azure Active Directory Kimlik Koruması tarafından algılanan güvenlik açıklarına genel bakış.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 446e2ef33b714afe06f24f3dbfc46bc6a42790b8
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335158"
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Azure Active Directory Kimlik Koruması tarafından algılanan güvenlik açıkları

Güvenlik açıkları, bir saldırgan tarafından yararlanılabilen bir ortamda zayıf zayıflardır. Yöneticilerin, kuruluşlarındaki güvenlik duruşunu geliştirmek için bu güvenlik açıklarını ele sunmalarını öneririz.

![Kimlik koruması tarafından bildirilen güvenlik açıkları](./media/vulnerabilities/identity-protection-vulnerabilities.png)

Aşağıdaki bölümler, kimlik koruması tarafından bildirilen güvenlik açıklarına genel bir bakış sağlar.

## <a name="multi-factor-authentication-registration-not-configured"></a>Multi-Factor Authentication kaydı yapılandırılmadı

Bu güvenlik açığı, kuruluşunuzda Azure Multi-Factor Authentication 'ın dağıtımını değerlendirmenize yardımcı olur.

Azure Multi-Factor Authentication, Kullanıcı kimlik doğrulamasına yönelik ikinci bir güvenlik katmanı sağlar. Kullanıcıların basit bir oturum açma işlemi taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya yardımcı olur. Azure Multi-Factor Authentication, şunun gibi kullanımı kolay doğrulama seçenekleri sunar:

* Telefon araması
* SMS Mesajı
* Mobil uygulama bildirimi
* OTP doğrulama kodu

Kullanıcı oturum açma işlemleri için Azure Multi-Factor Authentication gerektirmenizi öneririz. Multi-Factor Authentication, kimlik koruması aracılığıyla kullanılabilen risk tabanlı koşullu erişim ilkelerinde önemli bir rol oynar.

Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication nedir?](../authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Yönetilmeyen bulut uygulamaları

Bu güvenlik açığı, kuruluşunuzda yönetilmeyen bulut uygulamalarını belirlemenize yardımcı olur.

BT personeli, genellikle kuruluşlarındaki tüm bulut uygulamalarının farkında değildir. Yöneticilerin, şirket verilerine yetkisiz erişim, olası veri sızıntısı ve diğer güvenlik riskleri hakkında nasıl kaygılara neden olduğunu görmek kolaydır.

Yönetilmeyen bulut uygulamalarını bulmaya ve Azure Active Directory kullanarak bu uygulamaları yönetmeye yönelik Cloud Discovery dağıtmanız önerilir.

Daha fazla bilgi için bkz. [Cloud Discovery](/cloud-app-security/set-up-cloud-discovery).

## <a name="security-alerts-from-privileged-identity-management"></a>Privileged Identity Management'tan Güvenlik Uyarıları

Bu güvenlik açığı, kuruluşunuzdaki ayrıcalıklı kimlikler hakkındaki uyarıları keşfetmenize ve çözmenize yardımcı olur.  

Kullanıcıların ayrıcalıklı işlemleri gerçekleştirmesini sağlamak için kuruluşların Azure AD, Azure veya Office 365 kaynakları veya diğer SaaS uygulamalarında kullanıcılara geçici veya kalıcı ayrıcalıklı erişim vermesi gerekir. Bu ayrıcalıklı kullanıcıların her biri, kuruluşunuzun saldırı yüzeyini artırır. Bu güvenlik açığı, gereksiz erişimi olan kullanıcıları belirlemenize ve bu işlemleri yaştıkları riski azaltmak ya da ortadan kaldırmak için gerekli işlemleri yapmanıza yardımcı olur.

Kuruluşların Azure AD 'deki ayrıcalıklı kimlikleri ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetler yönetmek, denetlemek ve izlemek için Azure AD Privileged Identity Management kullanmalarını öneririz.

Daha fazla bilgi için [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md).

## <a name="see-also"></a>Ayrıca bkz.

[Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
