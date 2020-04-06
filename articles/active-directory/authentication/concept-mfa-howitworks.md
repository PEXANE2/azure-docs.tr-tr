---
title: Azure Çok Faktörlü Kimlik Doğrulamaya genel bakış
description: Azure Çok Faktörlü Kimlik Doğrulama'nın, basit bir oturum açma işlemi için kullanıcı talebini karşılarken verilere ve uygulamalara erişimi korumaya nasıl yardımcı olduğunu öğrenin.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: c50232abd12c8c0390409bd7bf72833b4f153e02
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80667356"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Nasıl çalışır: Azure Multi-Factor Authentication

Çok faktörlü kimlik doğrulama, kullanıcının oturum açma işlemi sırasında cep telefonuna kod girme veya parmak izi taradığı gibi ek bir kimlik formu için istendiği bir işlemdir.

Yalnızca bir kullanıcının kimliğini doğrulamak için parola kullanıyorsanız, saldırı için güvenli olmayan bir vektör bırakır. Parola zayıfsa veya başka bir yerde ortaya çıktıysa, gerçekten kullanıcı adı ve parolayla oturum açıyormu, yoksa saldırgan mı? İkinci bir kimlik doğrulama biçimi ne zaman gerekiyorsa, bu ek faktör saldırganın elde etmesi veya çoğaltması kolay bir şey olmadığından güvenlik artırılır.

![Çok faktörlü kimlik doğrulamanın farklı biçimlerinin kavramsal görüntüsü](./media/concept-mfa-howitworks/methods.png)

Azure Çok Faktörlü Kimlik Doğrulama, aşağıdaki kimlik doğrulama yöntemlerinden iki veya daha fazlasını gerektirerek çalışır:

* Bildiğiniz bir şey, genellikle bir şifre.
* Telefon veya donanım anahtarı gibi kolayca çoğaltılamayan güvenilir bir aygıt gibi sahip olduğunuz bir şey.
* Bir şey - bir parmak izi veya yüz taradığı gibi biyometri.

Kullanıcılar, biniş deneyimini basitleştirmek için hem self servis parola sıfırlama hem de Azure Çok Faktörlü Kimlik Doğrulama için tek adımda kaydolabilirler. Yöneticiler ikincil kimlik doğrulama formlarının kullanılabileceğini tanımlayabilir. Azure Çok Faktörlü Kimlik Doğrulaması, kullanıcılar bu işlemi daha da güvenli hale getirmek için self servis parola sıfırlama gerçekleştirdiğinde de gerekli olabilir.

![Oturum açma ekranında kullanılan kimlik doğrulama yöntemleri](media/concept-authentication-methods/overview-login.png)

Azure Çok Faktörlü Kimlik Doğrulama, kullanıcılar için basitliği korurken verilere ve uygulamalara erişimi korumaya yardımcı olur. İkinci bir kimlik doğrulama biçimi gerektirerek ek güvenlik sağlar ve kullanımı kolay [kimlik doğrulama yöntemleri](concept-authentication-methods.md)ile güçlü kimlik doğrulaması sağlar. Kullanıcılar, yöneticinin verdiği yapılandırma kararlarına dayanarak MFA için meydan okuyabilir veya sorgulanmayabilir.

Uygulamalarınızın veya hizmetlerinizin Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmak için herhangi bir değişiklik yapması gerekmez. Doğrulama istemleri, gerektiğinde MFA meydan okumasını otomatik olarak isteyen ve işleyen Azure AD oturum açma etkinliğinin bir parçasıdır.

## <a name="available-verification-methods"></a>Kullanılabilir doğrulama yöntemleri

Bir kullanıcı bir uygulama veya hizmete girdiğinde ve bir MFA istemi aldığında, kayıtlı ek doğrulama formlarından birini seçebilir. Bir yönetici bu Azure Çok Faktörlü Kimlik Doğrulama doğrulama yöntemlerinin kaydedilmesini gerektirebilir veya kullanıcı doğrulama yöntemlerini düzenlemek veya eklemek için kendi [Profilime](https://myprofile.microsoft.com) erişebilir.

Aşağıdaki ek doğrulama biçimleri Azure Çok Faktörlü Kimlik Doğrulama ile kullanılabilir:

* Microsoft Authenticator uygulaması
* OATH Donanım belirteci
* SMS
* Sesli arama

## <a name="how-to-enable-and-use-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama'yı etkinleştirme ve kullanma

Oturum açma etkinliği sırasında ek doğrulama için komut istemi için Azure Çok Faktörlü Kimlik Doğrulaması için kullanıcılar ve gruplar etkinleştirilebilir. [Güvenlik varsayılanları,](../fundamentals/concept-fundamentals-security-defaults.md) tüm Kullanıcılar için Microsoft Authenticator uygulamasının kullanımını hızla etkinleştirmek için tüm Azure AD kiracıları için kullanılabilir.

Daha ayrıntılı denetimler için [Koşullu Erişim](../conditional-access/overview.md) ilkeleri, MFA gerektiren olayları veya uygulamaları tanımlamak için kullanılabilir. Bu ilkeler, kullanıcı şirket ağında veya kayıtlı bir aygıtta yken düzenli oturum açma olaylarına izin verebilir, ancak uzaktan veya kişisel bir cihazda ek doğrulama faktörleri için istekte bulunabilir.

![Oturum açma işlemini güvence altına almak için Koşullu Erişim'in nasıl çalıştığına genel bakış diyagramı](media/tutorial-enable-azure-mfa/conditional-access-overview.png)

## <a name="next-steps"></a>Sonraki adımlar

Lisanslama hakkında bilgi edinmek [için Azure Çok Faktörlü Kimlik Doğrulama özellikleri ve lisanslarına](concept-mfa-licensing.md)bakın.

MFA'yı iş başında görmek için, aşağıdaki öğreticide bir dizi test kullanıcısı için Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirin:

> [!div class="nextstepaction"]
> [Azure Multi-Factor Authentication’ı etkinleştirme](tutorial-mfa-applications.md)
