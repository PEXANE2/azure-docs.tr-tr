---
title: Azure Etkin Dizin kimlik doğrulamaya genel bakış
description: Azure Active Directory ile kullanıcı oturum açmaiçin farklı kimlik doğrulama yöntemleri ve güvenlik özellikleri hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: overview
ms.date: 01/17/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f4c2fa8488490561e8f11746e8e737718ee9f37
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76261271"
---
# <a name="what-is-azure-active-directory-authentication"></a>Azure Etkin Dizin kimlik doğrulaması nedir?

Kimlik platformunun temel özelliklerinden biri, kullanıcı bir aygıta, uygulamaya veya hizmete giriş yaptığında kimlik bilgilerini doğrulamak veya *doğrulamaktır.* Azure Etkin Dizini'nde (Azure AD) kimlik doğrulama, bir kullanıcı adı ve parolanın doğrulanmasından daha fazlasını içerir. Güvenliği artırmak ve yardım masası yardımı gereksinimini azaltmak için Azure AD kimlik doğrulaması aşağıdaki bileşenleri içerir:

* Self servis parola sıfırlama
* Azure Multi-Factor Authentication
* Parola değişikliklerini şirket içi ortama geri yazmak için karma tümleştirme
* Şirket içi ortam için parola koruma ilkelerini uygulamak için karma tümleştirme
* Parolasız kimlik doğrulaması

## <a name="improve-the-end-user-experience"></a>Son kullanıcı deneyimini geliştirme

Azure AD, kullanıcının kimliğini korumaya ve oturum açma deneyimini basitleştirmeye yardımcı olur. Self servis parola sıfırlama gibi özellikler, kullanıcıların herhangi bir cihazdan bir web tarayıcısı kullanarak parolalarını güncelleştirmelerine veya değiştirmelerine izin verir. Bu özellik özellikle kullanıcı parolasını unuttuğunda veya hesabı kilitlendiğinde kullanışlıdır. Bir yardım masasının veya yöneticinin destek sağlamasını beklemeden, kullanıcı engelini kaldırabilir ve çalışmaya devam edebilir.

Azure Çok Faktörlü Kimlik Doğrulama, kullanıcıların oturum açma sırasında telefon görüşmesi veya mobil uygulama bildirimi gibi ek bir kimlik doğrulama biçimi seçmelerini sağlar. Bu, donanım belirteci gibi tek, sabit bir ikincil kimlik doğrulama biçimi gereksinimini azaltır. Kullanıcının şu anda tek bir ek kimlik doğrulama biçimi yoksa, farklı bir yöntem seçebilir ve çalışmaya devam edebilir.

![Oturum açma ekranında kullanılan kimlik doğrulama yöntemleri](media/concept-authentication-methods/overview-login.png)

Parolasız kimlik doğrulaması, kullanıcının güvenli bir parola oluşturma ve hatırlama gereksinimini ortadan kaldırır. Windows Hello for Business veya FIDO2 güvenlik anahtarları gibi özellikler, kullanıcıların parola olmadan bir aygıtta veya uygulamada oturum açmalarına olanak tanır. Bu yetenek, farklı ortamlarda parola ları yönetmenin karmaşıklığını azaltabilir.

## <a name="self-service-password-reset"></a>Self servis parola sıfırlama

Self servis parola sıfırlama, kullanıcılara yönetici veya yardım masası katılımı olmadan parolalarını değiştirme veya sıfırlama olanağı sağlar. Bir kullanıcının hesabı kilitlenirse veya parolasını unuturlarsa, kendilerinin engelini kaldırmak ve işe geri dönmek için istemleri izleyebilirler. Bu yetenek, bir kullanıcı cihazında veya bir uygulamada oturum açamaması durumunda yardım masası aramalarını ve üretkenlik kaybını azaltır.

Self servis parola sıfırlama aşağıdaki senaryolarda çalışır:

* **Parola değişikliği -** bir kullanıcı parolasını biliyorsa ancak yeni bir şeyle değiştirmek istediğinde.
* **Parola sıfırlama -** kullanıcı parolayı unuttuğu ve parolasını sıfırlamak istediği gibi oturum açamıyorsa.
* **Hesap kilidini niçin** kapatan bir kullanıcı, hesabı kilitlendiğinden ve hesabının kilidini açmak istediğinde oturum açamadığında.

Bir kullanıcı self servis parola sıfırlama kullanarak parolasını güncelleştirir veya sıfırladığında, bu parola şirket içi Active Directory ortamına da yazılabilir. Parola yazma, kullanıcının güncelleştirilmiş kimlik bilgilerini şirket içi aygıtlar ve uygulamalarla anında kullanabilmesini sağlar.

## <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

Çok faktörlü kimlik doğrulama, kullanıcının oturum açma işlemi sırasında cep telefonuna kod girme veya parmak izi taradığı gibi ek bir kimlik formu için istendiği bir işlemdir.

Yalnızca bir kullanıcının kimliğini doğrulamak için parola kullanıyorsanız, saldırı için güvenli olmayan bir vektör bırakır. Parola zayıfsa veya başka bir yerde ortaya çıktıysa, gerçekten kullanıcı adı ve parolayla oturum açıyormu, yoksa saldırgan mı? İkinci bir kimlik doğrulama biçimi ne zaman gerekiyorsa, bu ek faktör saldırganın elde etmesi veya çoğaltması kolay bir şey olmadığından güvenlik artırılır.

![Çok faktörlü kimlik doğrulamanın farklı biçimlerinin kavramsal görüntüsü](./media/concept-mfa-howitworks/methods.png)

Azure Çok Faktörlü Kimlik Doğrulama, aşağıdaki kimlik doğrulama yöntemlerinden iki veya daha fazlasını gerektirerek çalışır:

* Bildiğiniz bir şey, genellikle bir şifre.
* Telefon veya donanım anahtarı gibi kolayca çoğaltılamayan güvenilir bir aygıt gibi sahip olduğunuz bir şey.
* Bir şey - bir parmak izi veya yüz taradığı gibi biyometri.

Kullanıcılar, biniş deneyimini basitleştirmek için hem self servis parola sıfırlama hem de Azure Çok Faktörlü Kimlik Doğrulama için tek adımda kaydolabilirler. Yöneticiler ikincil kimlik doğrulama formlarının kullanılabileceğini tanımlayabilir. Azure Çok Faktörlü Kimlik Doğrulaması, kullanıcılar bu işlemi daha da güvenli hale getirmek için self servis parola sıfırlama gerçekleştirdiğinde de gerekli olabilir.

## <a name="password-protection"></a>Parola koruması

Varsayılan olarak, Azure AD *Parola1*gibi zayıf parolaları engeller. Genel olarak yasaklanan parola listesi otomatik olarak güncelleştirilir ve bilinen zayıf parolaları içeren zorlanır. Bir Azure AD kullanıcısı parolasını bu zayıf parolalardan birine ayarlamaya çalışırsa, daha güvenli bir parola seçmek için bir bildirim alır.

Güvenliği artırmak için özel parola koruma ilkeleri tanımlayabilirsiniz. Bu ilkeler, *örneğin Contoso* veya *Londra*gibi bir konum gibi bir ad içeren bir parolanın herhangi bir varyasyonunu engellemek için filtreleri kullanabilir.

Karma güvenlik için Azure AD parola korumayı şirket içi Etkin Dizin ortamıyla tümleştirebilirsiniz. Ön hazırlık ortamında yüklü bir bileşen, Azure AD'den genel yasaklı parola listesini ve özel parola koruma ilkelerini alır ve etki alanı denetleyicileri bunları parola değiştirme olaylarını işlemek için kullanır. Bu karma yaklaşım, bir kullanıcı nın kimlik bilgilerini nasıl veya nerede değiştirirse değiştirmez, güçlü parolaların kullanımını zorunlu kıldığınızı sağlar.

## <a name="passwordless-authentication"></a>Parolasız kimlik doğrulaması

Birçok ortam için son hedef, oturum açma olaylarının bir parçası olarak parola kullanımını kaldırmaktır. Azure parola koruması veya Azure Çok Faktörlü Kimlik Doğrulama gibi özellikler güvenliği artırmaya yardımcı olur, ancak kullanıcı adı ve parola, açığa çıkabilir veya kaba kuvvet saldırısına uğrayabilecek zayıf bir kimlik doğrulama biçimi olmaya devam eder.

![Şifresiz yol açan kimlik doğrulama işlemi ile güvenlik ve kolaylık](./media/concept-authentication-passwordless/passwordless-convenience-security.png)

Parolasız bir yöntemle oturum açtığınızda, kimlik bilgileri Windows Hello for Business veya FIDO2 güvenlik anahtarı yla biyometri gibi yöntemler kullanarak sağlanır. Bu kimlik doğrulama yöntemleri bir saldırgan tarafından kolayca çoğaltılamaz.

Azure AD, kullanıcılar için oturum açma deneyimini basitleştirmek ve saldırı riskini azaltmak için parolasız yöntemler kullanarak yerel olarak kimlik doğrulaması yapmanın yollarını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için self servis parola sıfırlama ve Azure [Çok Faktörlü Kimlik Doğrulama][tutorial-mfa-applications] [öğreticisi için hızlı başlangıç'a][quickstart-sspr] bakın.

Self servis parola sıfırlama kavramları hakkında daha fazla bilgi edinmek için [Azure AD self servis parola sıfırlamanın nasıl çalıştığını][concept-sspr]öğrenin.

Çok faktörlü kimlik doğrulama kavramları hakkında daha fazla bilgi edinmek için [Azure Çok Faktörlü Kimlik Doğrulama'nın nasıl çalıştığını][concept-mfa]öğrenin.

<!-- INTERNAL LINKS -->
[quickstart-sspr]: quickstart-sspr.md
[tutorial-mfa-applications]: tutorial-mfa-applications.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md
