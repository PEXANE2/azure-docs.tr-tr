---
title: Azure AD Kimlik Koruması ile kullanıcı deneyimleri
description: Azure AD Kimlik Koruması kullanıcı deneyimi
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
ms.openlocfilehash: cc10fb4f9894a355c9eed024ae9f87747214999b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72887006"
---
# <a name="user-experiences-with-azure-ad-identity-protection"></a>Azure AD Kimlik Koruması ile kullanıcı deneyimleri

Azure Active Directory Identity Protection ile şunları yapabilirsiniz:

* Kullanıcıların Azure Çok Faktörlü Kimlik Doğrulamasına (MFA) kaydolmasını zorunlu kılmasını zorunlu kılmaktadır
* Riskli oturum açma ve tehlikeye atan kullanıcıların düzeltilmesini otomatikleştirin

Kimlik Koruma ilkelerinin tümü, kullanıcılar için oturum açma deneyimi üzerinde etkilidir. Kullanıcıların Azure MFA ve self servis parola sıfırlama gibi araçlara kaydolmasına ve kullanmasına izin vermek etkisini azaltabilir. Bu araçlar, uygun ilke seçenekleriyle birlikte kullanıcılara ihtiyaç duyduklarında kendi kendini düzeltme seçeneği sunar.

## <a name="multi-factor-authentication-registration"></a>Çok faktörlü kimlik doğrulama kaydı

Çok faktörlü kimlik doğrulama kaydı gerektiren kimlik koruma ilkesini etkinleştirmek ve tüm kullanıcılarınızı hedeflemek, gelecekte kendi kendini düzeltmek için Azure MFA'yı kullanma olana sahip olmasını sağlar. Bu ilkeyi yapılandırmak, kullanıcılarınıza kayıt yaptırmayı seçebilecekleri ve sonunda kayıt yaptırmak zorunda kabilecekleri 14 günlük bir süre verir. Kullanıcılar için deneyim aşağıda özetlenmiştir. Daha fazla bilgi makalede son kullanıcı belgeleri bulunabilir, [iki faktörlü doğrulama ve iş veya okul hesabı için Genel Bakış](../user-help/user-help-two-step-verification-overview.md).

### <a name="registration-interrupt"></a>Kayıt kesintisi

1. Azure AD tümleşik bir uygulamasında oturum açma sırasında, kullanıcı çok faktörlü kimlik doğrulama için hesabı ayarlama gereksinimi hakkında bir bildirim alır. Bu ilke, yeni bir aygıta sahip yeni kullanıcılar için Windows 10 Out of Box Deneyimi'nde de tetiklenir.
   
    ![Daha fazla bilgi gerekli](./media/concept-identity-protection-user-experience/identity-protection-experience-more-info-mfa.png)

1. Azure Çok Faktörlü Kimlik Doğrulama'ya kaydolmak ve oturum açmanızı tamamlamak için kılavuzlu adımları tamamlayın.

## <a name="risky-sign-in-remediation"></a>Riskli oturum açma düzeltmesi

Bir yönetici oturum açma riskleri için bir ilke yapılandırdığında, etkilenen kullanıcılar oturum açmaya ve ilkeleri risk düzeyini tetiklemeye çalıştıklarında bildirilir. 

### <a name="risky-sign-in-self-remediation"></a>Riskli oturum açma self-düzeltme

1. Kullanıcı, oturum açma konusunda yeni bir konumdan, cihazdan veya uygulamadan oturum açma gibi olağandışı bir şeyin algılandığını bildirilir.
   
    ![Olağandışı bir şey istemi](./media/concept-identity-protection-user-experience/120.png)

1. Kullanıcının Azure MFA'sını daha önce kaydettiği yöntemlerden biriyle tamamlayarak kimliğini kanıtlaması gerekir. 

### <a name="risky-sign-in-administrator-unblock"></a>Riskli oturum açma yöneticisi engellemeyi kaldırma

Yöneticiler, risk düzeylerine bağlı olarak oturum açma ları üzerine kullanıcıları engellemeyi seçebilir. Engelinin kalkması için, son kullanıcıların BT personeliyle iletişime geçmesi veya tanıdık bir konumdan veya cihazdan oturum açmayı denemeleri gerekir. Çok faktörlü kimlik doğrulaması gerçekleştirerek kendi kendini düzeltme bu durumda bir seçenek değildir.

![Oturum açma risk ilkesi tarafından engellendi](./media/concept-identity-protection-user-experience/200.png)

BT personeli, kullanıcıların tekrar oturum açmalarına izin vermek için [kullanıcıların engelini kaldırma](howto-identity-protection-remediate-unblock.md#unblocking-based-on-sign-in-risk) bölümündeki yönergeleri izleyebilir.

## <a name="risky-user-remediation"></a>Riskli kullanıcı düzeltmesi

Bir kullanıcı risk ilkesi yapılandırıldığında, kullanıcı risk düzeyi nin uzlaşma olasılığını karşılayan kullanıcıların oturum açabilmesi için kullanıcı uzlaşma kurtarma akışından geçmesi gerekir. 

### <a name="risky-user-self-remediation"></a>Riskli kullanıcı kendini düzeltme

1. Kullanıcı, şüpheli etkinlik veya sızdırılan kimlik bilgileri nedeniyle hesap güvenliğinin risk altında olduğu konusunda bilgilendirilir.
   
    ![Düzeltme](./media/concept-identity-protection-user-experience/101.png)

1. Kullanıcının Azure MFA'sını daha önce kaydettiği yöntemlerden biriyle tamamlayarak kimliğini kanıtlaması gerekir. 
1. Son olarak, başka biri hesabına erişimi olduğundan, kullanıcı self servis parola sıfırlama kullanarak parolasını değiştirmek zorunda kalır.

## <a name="risky-sign-in-administrator-unblock"></a>Riskli oturum açma yöneticisi engellemeyi kaldırma

Yöneticiler, risk düzeylerine bağlı olarak oturum açma ları üzerine kullanıcıları engellemeyi seçebilir. Engelinin kalkması için, son kullanıcıların BT personeline başvurmaları gerekir. Çok faktörlü kimlik doğrulama ve self servis parola sıfırlama gerçekleştirerek kendi kendine düzeltme bu durumda bir seçenek değildir.

![Kullanıcı risk ilkesi tarafından engellendi](./media/concept-identity-protection-user-experience/104.png)

BT personeli, kullanıcıların tekrar oturum açmalarına izin vermek için [kullanıcıların engelini kaldırma](howto-identity-protection-remediate-unblock.md#unblocking-based-on-user-risk) bölümündeki yönergeleri izleyebilir.

## <a name="see-also"></a>Ayrıca bkz.

- [Riskleri düzeltin ve kullanıcıların engelini kaldırın](howto-identity-protection-remediate-unblock.md)

- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md) 
