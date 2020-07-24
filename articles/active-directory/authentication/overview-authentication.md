---
title: Azure Active Directory kimlik doğrulamasına genel bakış
description: Azure Active Directory ile Kullanıcı oturum açma işlemlerinin farklı kimlik doğrulama yöntemleri ve güvenlik özellikleri hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: overview
ms.date: 07/13/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5e65149b2524d2785039be78caad4ce84924e7e3
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87035120"
---
# <a name="what-is-azure-active-directory-authentication"></a>Azure Active Directory kimlik doğrulaması nedir?

Bir kimlik platformunun ana özelliklerinden biri, bir Kullanıcı bir cihazda, uygulamada veya hizmette oturum açtığında kimlik bilgilerini doğrulamak veya *kimliğini*doğrulamak için kullanılır. Azure Active Directory (Azure AD) içinde, kimlik doğrulaması yalnızca Kullanıcı adı ve parola doğrulamasından daha fazlasını içerir. Güvenliği artırmak ve yardım masası yardımı ihtiyacını azaltmak için, Azure AD kimlik doğrulaması aşağıdaki bileşenleri içerir:

* Self servis parola sıfırlama
* Azure Multi-Factor Authentication
* Parola değişikliklerini şirket içi ortama geri yazmaya yönelik karma tümleştirme
* Şirket içi bir ortamda parola koruma ilkelerini zorlamak için karma tümleştirme
* Parolasız kimlik doğrulaması

## <a name="improve-the-end-user-experience"></a>Son Kullanıcı deneyimini geliştirme

Azure AD, kullanıcının kimliğini korumaya ve oturum açma deneyimini basitleştirmeye yardımcı olur. Self servis parola sıfırlama gibi özellikler, kullanıcıların herhangi bir cihazdan bir Web tarayıcısı kullanarak parolalarını güncelleştirmesine veya değiştirmesine izin verir. Bu özellik özellikle Kullanıcı parolasını unutursa veya hesapları kilitlenmişse yararlıdır. Bir yardım masası veya yöneticinin destek sağlamasına gerek kalmadan, bir Kullanıcı kendi kendilerini engellemeyi kaldırabilir ve çalışmaya devam edebilir.

Azure Multi-Factor Authentication, kullanıcıların oturum açma sırasında telefon araması veya mobil uygulama bildirimi gibi ek bir kimlik doğrulama biçimi seçmesini sağlar. Bu özellik, bir donanım belirteci gibi tek, sabit bir ikincil kimlik doğrulama biçimi gereksinimini azaltır. Kullanıcının şu anda bir ek kimlik doğrulama formu yoksa, farklı bir yöntem seçip çalışmaya devam edebilirler.

![Oturum açma ekranında kullanılan kimlik doğrulama yöntemleri](media/concept-authentication-methods/overview-login.png)

Passwordless kimlik doğrulaması, kullanıcının her seferinde güvenli parola oluşturma ve hatırlama gereksinimini ortadan kaldırır. Iş için Windows Hello veya FIDO2 güvenlik anahtarları gibi yetenekler, kullanıcıların parola olmadan bir cihazda veya uygulamada oturum açmalarına olanak tanır. Bu özellik, farklı ortamlarda parolaları yönetmenin karmaşıklığını azaltabilir.

## <a name="self-service-password-reset"></a>Self servis parola sıfırlama

Self servis parola sıfırlama, kullanıcılara yönetici veya yardım masası katılımı olmadan parolasını değiştirme veya sıfırlama yeteneği sağlar. Bir kullanıcının hesabı kilitliyse veya parolalarını unutduklarında, kendi kendilerini engellemeyi kaldırmak ve çalışmaya geri dönmek için istemleri izleyebilir. Bu özellik, bir Kullanıcı cihazlarındaki veya bir uygulamada oturum açarken yardım masası çağrılarını ve üretkenlik kaybını azaltır.

Self servis parola sıfırlama aşağıdaki senaryolarda çalışmaktadır:

* **Parola değiştirme-** Kullanıcı parolasını bilir ancak yeni bir şekilde değiştirmek istediğinde.
* **Parola sıfırlama-** bir Kullanıcı, parolayı unutduklarında olduğu gibi oturum açamaz ve parolasını sıfırlamak ister.
* **Hesap kilit açma-** bir Kullanıcı, hesabı kilitlendiğinden ve hesaplarının kilidini açmak istediğlerinden oturum açamadığı zaman.

Bir Kullanıcı self servis parola sıfırlama kullanarak parolalarını güncelleştirdiğinde veya sıfırladığında, bu parola bir şirket içi Active Directory ortamına da geri yazılabilir. Parola geri yazma özelliği, kullanıcının şirket içi cihazlar ve uygulamalarla güncelleştirilmiş kimlik bilgilerini hemen kullanabilmesini sağlar.

## <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

Multi-Factor Authentication, bir kullanıcıdan, Cellphone 'a kod girmek veya parmak izi taraması sağlamak gibi ek bir tanımlama formu için oturum açma işlemi sırasında istenen bir işlemdir.

Yalnızca bir kullanıcının kimliğini doğrulamak için parola kullanıyorsanız, saldırı için güvenli olmayan bir vektör bırakır. Parola zayıfmıdır veya başka bir yerde sunulduktan sonra Kullanıcı Kullanıcı adı ve parolayla oturum açmasını gerçekten mi, yoksa bir saldırgan mi? İkinci bir kimlik doğrulaması formu gerektirdiğinde, bu ek faktörün bir saldırganın alması veya yinelemesi kolay olmadığı için güvenlik artar.

![Multi-Factor Authentication 'ın farklı biçimlerinin kavramsal resmi](./media/concept-mfa-howitworks/methods.png)

Azure Multi-Factor Authentication, aşağıdaki kimlik doğrulama yöntemlerinin iki veya daha fazlasını gerektirerek işe yarar:

* Bildiğiniz bir şey, genellikle bir paroladır.
* Telefon veya donanım anahtarı gibi kolayca tekrarlanmayan güvenilir bir cihaz gibi sahip olduğunuz bir şey.
* Parmak izi veya yüz tarama gibi bir biyometri vardır.

Kullanıcılar kendi self servis parola sıfırlama ve Azure Multi-Factor Authentication için kendilerini tek bir adımda kaydedebilir ve böylece yerleşik deneyimi basitleştirir. Yöneticiler, hangi ikincil kimlik doğrulama biçimlerinin kullanılabileceğini tanımlayabilir. Ayrıca, kullanıcılar bu işlemi daha güvenli hale getirmek için bir self servis parola sıfırlaması gerçekleştirirken Azure Multi-Factor Authentication de gerekebilir.

## <a name="password-protection"></a>Parola koruması

Varsayılan olarak, Azure AD *Parola1*gibi zayıf parolaları engeller. Genel yasaklanmış parola listesi otomatik olarak güncelleştirilir ve bilinen zayıf parolalar dahil zorlanır. Bir Azure AD kullanıcısı parolalarını bu zayıf parolalardan birine ayarlamaya çalışırsa, daha güvenli bir parola seçmek üzere bir bildirim alırlar.

Güvenliği artırmak için, özel parola koruma ilkeleri tanımlayabilirsiniz. Bu ilkeler, *contoso* gibi bir ad veya *Londra*gibi bir konum içeren bir parola varyasyonlarını engellemek için filtreleri kullanabilir.

Karma güvenlik için Azure AD parola korumasını bir şirket içi Active Directory ortamıyla tümleştirebilirsiniz. Şirket içi ortamda yüklü olan bir bileşen, genel yasaklanmış parola listesini ve Azure AD 'den özel parola koruma ilkelerini alır ve etki alanı denetleyicileri, parola değiştirme olaylarını işlemek için bunları kullanır. Bu karma yaklaşım, bir kullanıcının kimlik bilgilerini ne şekilde değiştirdiğine ve güçlü parolaların kullanımını zorunlu kılmaz emin olmanızı sağlar.

## <a name="passwordless-authentication"></a>Parolasız kimlik doğrulaması

Birçok ortam için son hedef, oturum açma olaylarının bir parçası olarak parolaların kullanımını kaldırmaktır. Azure parola koruması veya Azure Multi-Factor Authentication gibi özellikler güvenliği artırmaya yardımcı olur, ancak Kullanıcı adı ve parola, açığa çıkarılabilecek veya deneme yanılma saldırısı yapan zayıf bir kimlik doğrulama biçimi olarak kalır.

![Güvenlik, passwordless 'a yol gösteren kimlik doğrulama işlemi ile karşı daha kolay](./media/concept-authentication-passwordless/passwordless-convenience-security.png)

Parolasız bir yöntem ile oturum açtığınızda, kimlik bilgileri Windows Hello for Business ile Biyometri gibi yöntemlerin kullanımı veya bir FIDO2 güvenlik anahtarı aracılığıyla sağlanır. Bu kimlik doğrulama yöntemleri bir saldırgan tarafından kolayca çoğaltılamaz.

Azure AD, kullanıcılar için oturum açma deneyimini basitleştirmek ve saldırı riskini azaltmak için passwordless yöntemlerini kullanarak yerel olarak kimlik doğrulaması yapmak için yollar sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için [self servis parola sıfırlama (SSPR)][tutorial-sspr] ve [Azure Multi-Factor Authentication][tutorial-azure-mfa]öğreticisine bakın.

Self servis parola sıfırlama kavramları hakkında daha fazla bilgi edinmek için bkz. [Azure AD self servis parola sıfırlamasının nasıl çalıştığı][concept-sspr].

Multi-Factor Authentication kavramları hakkında daha fazla bilgi edinmek için bkz. [Azure Multi-Factor Authentication 'In nasıl çalıştığı][concept-mfa].

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md
