---
title: Kullanıcıların kimliğini doğrulama ve güvenliğini sağlama-Azure Active Directory
description: Azure AD yöneticisi olarak son kullanıcıya fazla yansıtmadan kullanıcı kimlik doğrulamasını nasıl koruyabilirim?
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: overview
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3077851f5feafdec9d46d9365c45f5729ca2b0b1
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74846683"
---
# <a name="what-methods-are-available-for-authentication"></a>Hangi kimlik doğrulama yöntemleri kullanılabilir?

Her gün çalınan parolalar ve gizliliği tehlikeye giren kimliklerle ilgili yeni haberler görüyoruz. Parolaya ek olarak ikinci bir adımın eklenmesi, kuruluşunuzun güvenliğini anında artıran yöntemlerden biridir. Microsoft Azure Active Directory (Azure AD), ek kimlik doğrulama yöntemleri ile yöneticilerin kuruluşlarının ve kullanıcılarının güvenliğini korumasına yardımcı olmak için Azure Multi-Factor Authentication (Azure MFA) ve Azure AD self servis parola sıfırlama (SSPR) gibi özellikler sunmaktadır.

Şunları içeren birçok senaryo vardır: bir uygulamada oturum açmak, parolasını sıfırlamak, Windows Hello 'yu etkinleştirmek ve diğerleri, kullanıcılarınızın söyledikleri kim olduğunu belirten ek doğrulama sağlamaları istenebilir.

Ek doğrulama adımları aşağıdaki gibi kimlik doğrulama yöntemleri olabilir:

* E-posta veya SMS ile iletilen bir kod
* Telefon araması
* Telefona gelen bildirim veya kod
* Güvenlik sorularını yanıtlama

![Chrome'da örnek login.microsoftonline.com oturum açma sayfası](media/overview-authentication/overview-login.png)

Azure MFA ve Azure AD self servis parola sıfırlama özellikleri, yöneticilerin kuruluşlarını korumak için Azure AD ve Azure portal kullanarak yapılandırma, ilke, izleme ve raporlama konularında denetim sahibi olmasını sağlar.

## <a name="self-service-password-reset"></a>Self servis parola sıfırlama

Self servis parola sıfırlama, kullanıcıların istediği yerden ve istediği zaman yönetici müdahalesi olmadan parolalarını sıfırlamasını sağlar.

> [!VIDEO https://www.youtube.com/embed/hc97Yx5PJiM]

Self servis parola sıfırlama şu adımlardan oluşur:

* **Parola değiştirme:** Parolamı biliyorum ancak değiştirmek istiyorum.
* **Parola sıfırlama:** Oturum açamıyorum ve onaylı kimlik doğrulama yöntemlerinden birini veya daha fazlasını kullanarak parolamı sıfırlamak istiyorum.
* **Hesap kilidini açma:** Hesabım kilitlendiğinden oturum açamıyorum ve onaylı kimlik doğrulama yöntemlerinden birini veya daha fazlasını kullanarak hesabımın kilidini açmak istiyorum.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Azure Multi-Factor Authentication (MFA) Microsoft'un iki adımlı doğrulama çözümüdür. Yönetici onaylı kimlik doğrulama yöntemlerini kullanan Azure MFA, oturum açma sürecini karmaşık hale getirmeden verilerinize ve uygulamalarınıza erişimin güvenli hale getirilmesine yardımcı olur.

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım self servis parola sıfırlama ve Azure Multi-Factor Authentication özelliklerini ayrıntılı bir şekilde incelemek ve yapılandırmak olacaktır.

Self servis parola sıfırlama özelliğini kullanmaya başlamak için bkz. [SSPR 'yi etkinleştirme hızlı başlangıç makalesi](quickstart-sspr.md).

Self servis parola sıfırlama özelliği hakkında daha fazla bilgi için bkz. [Nasıl çalışır? Azure AD self servis parola sıfırlama](concept-sspr-howitworks.md)

Azure Multi-Factor Authentication hakkında daha fazla bilgi için bkz. [Nasıl çalışır? Azure Multi-Factor Authentication](concept-mfa-howitworks.md)
