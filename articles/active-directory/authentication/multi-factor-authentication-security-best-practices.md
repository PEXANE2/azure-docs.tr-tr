---
title: Azure Çok Faktörlü Kimlik Doğrulama için güvenlik kılavuzu - Azure Etkin Dizin
description: Bu belge, Azure hesaplarıyla Azure MFA'yı kullanma konusunda kılavuz sağlar
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: e42234e9fcdcfe3ee5ce975babbe03b64a750e36
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74846836"
---
# <a name="security-guidance-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Azure Çok Faktörlü Kimlik Doğrulama'yı Azure AD hesaplarıyla kullanmak için güvenlik kılavuzu

İki aşamalı doğrulama, kimlik doğrulama işlemlerini geliştirmek isteyen çoğu kuruluş için tercih edilen seçimdir. Azure Çok Faktörlü Kimlik Doğrulama (MFA), şirketlerin güvenlik ve uyumluluk gereksinimlerini karşılamalarına yardımcı olurken, kullanıcıları için basit bir oturum açma deneyimi sağlar. Bu makale, Azure MFA'nın benimsenmesini planlarken göz önünde bulundurmanız gereken bazı ipuçlarını kapsar.

## <a name="deploy-azure-mfa-in-the-cloud"></a>Azure MFA'yı bulutta dağıtma

[Tüm kullanıcılarınız için Azure MFA'yı etkinleştirmenin](howto-mfa-getstarted.md)iki yolu vardır.

* Her kullanıcı için lisans satın alın (Azure MFA, Azure AD Premium veya Kurumsal Mobilite + Güvenlik)
* Çok Faktörlü Auth Sağlayıcısı oluşturun ve kullanıcı başına veya kimlik doğrulama başına ödeme

### <a name="licenses"></a>Lisanslar

![Kullanıcılara lisans uygulayın, etkinleştirin, bildirin](./media/multi-factor-authentication-security-best-practices/ems.png)

Azure AD Premium veya Kurumsal Mobilite + Güvenlik lisanslarınız varsa, Azure MFA'nız zaten vardır. Kuruluşunuzun iki aşamalı doğrulama özelliğini tüm kullanıcılara yaymak için ek bir şeye ihtiyacı yoktur. Yalnızca bir kullanıcıya lisans atamanız gerekir ve ardından MFA'yı açabilirsiniz.

Çok Faktörlü Kimlik Doğrulama'yı ayarlarken aşağıdaki ipuçlarını göz önünde bulundurun:

* Kimlik doğrulama başına Çok Faktörlü Auth Sağlayıcısı oluşturmayın. Bunu yaparsanız, zaten lisansları olan kullanıcılardan gelen doğrulama istekleri ni ödenebilirsiniz.
* Tüm kullanıcılarınız için yeterli lisansınız yoksa, kuruluşunuzun geri kalanını kapsayacak şekilde kullanıcı başına bir Çok Faktörlü Auth Sağlayıcısı oluşturabilirsiniz. 
* Azure AD Connect yalnızca şirket içi Etkin Dizin ortamınızı bir Azure REKLAM diziniyle eşitlemeniz gerekiyorsa gereklidir. Etkin Dizin'in şirket içi bir örneğiyle eşitlenmemiş bir Azure REKLAM dizini kullanıyorsanız, Azure AD Bağlantısı'na ihtiyacınız yoktur.

### <a name="multi-factor-auth-provider"></a>Çok Faktörlü Auth Sağlayıcı

![Çok Faktörlü Kimlik Doğrulama Sağlayıcısı](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Azure MFA içeren lisanslarınız yoksa, [bir MFA Auth Sağlayıcısı oluşturabilirsiniz.](concept-mfa-authprovider.md)

Auth Sağlayıcı oluştururken, bir dizin seçmeniz ve aşağıdaki ayrıntıları göz önünde bulundurmanız gerekir:

* Çok Faktörlü Auth Sağlayıcısı oluşturmak için Azure REKLAM dizinine ihtiyacınız yoktur, ancak bir taneyle daha fazla işlevsellik elde elabilirsiniz. Auth Sağlayıcı'yı bir Azure AD diziniyle ilişkilendirdiğinizde aşağıdaki özellikler etkinleştirilir:
  * İki aşamalı doğrulamayı tüm kullanıcılarınız için genişletme
  * Genel yöneticilerinize yönetim portalı, özel karşılamalar ve raporlar gibi ek özellikler sunun.
* Şirket içi Active Directory ortamınızı bir Azure AD diziniyle eşitlerseniz, DirSync veya Azure AD Eşitlemeyi gerekir. Etkin Dizin'in şirket içi bir örneğiyle eşitlenmemiş bir Azure REKLAM dizini kullanıyorsanız, DirSync veya Azure AD Eşitlemesi gerekmez.
* İşletmenize en uygun tüketim modelini seçin. Kullanım modelini seçtikten sonra değiştiremezsiniz. İki model şunlardır:
  * Kimlik doğrulama başına: her doğrulama için sizden ücret alabilirsiniz. Belirli bir uygulamaya erişen herkes için iki aşamalı doğrulama istiyorsanız, belirli kullanıcılar için değil, bu modeli kullanın.
  * Etkin kullanıcı başına: Azure MFA için etkinleştirdiğiniz her kullanıcı için sizden ücret alabilirsiniz. Azure AD Premium veya Enterprise Mobility Suite lisanslarına sahip bazı kullanıcılarınız varsa ve bazılarını kullanmamışsanız bu modeli kullanın.

### <a name="supportability"></a>Desteklenebilirlik

Çoğu kullanıcı kimlik doğrulaması yapmak için yalnızca parola kullanmaya alışkın olduğundan, şirketinizin bu işlemle ilgili tüm kullanıcılara farkındalık getirmesi önemlidir. Bu farkındalık, kullanıcıların MFA ile ilgili küçük sorunlar için yardım masanızı arama olasılığını azaltabilir. Ancak, Geçici Olarak MFA devre dışı bırakılması gerekli olduğu bazı senaryolar vardır. Bu senaryoların nasıl işleyeceğini anlamak için aşağıdaki yönergeleri kullanın:

* Mobil uygulama veya telefon bildirim veya telefon görüşmesi almadığı için kullanıcının oturum açamadığı senaryoları işlemesi için teknik destek personelinizi eğitin. Teknik destek, bir kullanıcının iki aşamalı doğrulamayı "atlayarak" tek bir kez kimlik doğrulamasını sağlamasına olanak [sağlayabilir.](howto-mfa-mfasettings.md#one-time-bypass) Baypas geçicidir ve belirtilen sayıda saniyeden sonra sona erer.
* İki aşamalı doğrulamayı en aza indirmenin bir yolu olarak Azure MFA'daki [Güvenilir IP'ler özelliğini](howto-mfa-mfasettings.md#trusted-ips) göz önünde bulundurun. Bu özellik sayesinde, yönetilen veya federe kiracının yöneticileri, şirketin yerel intranetinden oturum açan kullanıcılar için iki aşamalı doğrulamayı atlayabilir. Özellikler, Azure AD Premium, Enterprise Mobility Suite veya Azure Çok Faktörlü Kimlik Doğrulama lisanslarına sahip Azure AD kiracıları için kullanılabilir.

## <a name="best-practices-for-an-on-premises-deployment"></a>Şirket içi dağıtım için en iyi uygulamalar

Şirketiniz MFA'yı etkinleştirmek için kendi altyapısından yararlanmaya karar verdiyse, [bir Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nu şirket içinde dağıtmanız](howto-mfaserver-deploy.md)gerekir. MFA Server bileşenleri aşağıdaki diyagramda gösterilmiştir:

![Varsayılan MFA Server](./media/multi-factor-authentication-security-best-practices/server.png) \*bileşenleri varsayılan \*olarak yüklenmedi *Yüklendi mteyi, varsayılan olarak etkin değil

Azure Çok Faktörlü Kimlik Doğrulama Sunucusu, federasyon kullanarak bulut kaynaklarını ve şirket içi kaynakları güvence altına alabilir. AD FS'niz olmalı ve Azure AD kiracınızla birlikte federated olmalıdır.
Çok Faktörlü Kimlik Doğrulama Sunucusu'nu kurarken aşağıdaki ayrıntıları göz önünde bulundurun:

* Active Directory Federation Services (AD FS) kullanarak Azure AD kaynaklarını güvence altına alıyorsanız, ilk doğrulama adımı AD FS kullanılarak şirket içinde gerçekleştirilir. İkinci adım, talebin onaylanmasıyla şirket içinde gerçekleştirilir.
* Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nu AD FS federasyon sunucunuza yüklemeniz gerekmez. Ancak, AD FS için Çok Faktörlü Kimlik Doğrulama Bağdaştırıcısı, AD FS çalıştıran bir Windows Server 2012 R2'ye yüklenmelidir. Desteklenen bir sürüm olduğu sürece sunucuyu farklı bir bilgisayara yükleyebilir ve AD FS bağdaştırıcısını AD FS federasyon sunucunuza ayrı olarak yükleyebilirsiniz. 
* Çok Faktörlü Kimlik Doğrulama AD FS Bağdaştırıcıyükleme sihirbazı, Active Dizininde PhoneFactor Yöneticileri adında bir güvenlik grubu oluşturur ve ardından AD FS hizmet hesabınızı bu gruba ekler. Etki alanı denetleyicinizde PhoneFactor Admins grubunun oluşturulduğunu ve AD FS hizmet hesabının bu gruba üye olduğunu doğrulayın. Gerekirse, AD FS hizmeti hesabınızı etki alanı denetleyicinizde PhoneFactor Admins grubuna el ile ekleyin.

### <a name="user-portal"></a>Kullanıcı Portalı

Kullanıcı portalı self servis yeteneklerisağlar ve kullanıcı yönetimi yetenekleri tam bir set sağlar. Bir Internet Information Server (IIS) web sitesinde çalışır. Bu bileşeni yapılandırmak için aşağıdaki yönergeleri kullanın:

* IIS 6 veya daha büyük kullanın
* v2.0.507207 ASP.NET yükleyin ve kaydedin
* Bu sunucunun çevre ağında dağıtılabilmesini sağlamak

### <a name="app-passwords"></a>Uygulama Şifreleri

Kuruluşunuz Azure AD ile SSO için federe yse ve Azure MFA kullanıyor olacaksanız, aşağıdaki ayrıntılara dikkat edin:

* Uygulama parolası Azure AD tarafından doğrulanır ve bu nedenle federasyonu atlar. Federasyon yalnızca uygulama parolalarını ayarlarken kullanılır.
* Federe (SSO) kullanıcıları için parolalar kuruluş kimliğinde depolanır. Kullanıcı şirketten ayrılırsa, bu bilginin DirSync kullanarak kuruluş kimliğine akması gerekir. Hesabın devre dışı sayılması/silinmesi eşitleme için üç saat kadar sürebilir ve bu da Azure AD'deki uygulama parolalarının devre dışı kaldırılmasını/silinmesini geciktirir.
* Şirket için İstemci Erişimi Denetimi ayarları Uygulama Parolası tarafından onaylanmaz.
* Uygulama parolaları için şirket içinde kimlik doğrulama günlüğü/denetim özelliği yoktur.
* Bazı gelişmiş mimari tasarımlar, kimlik doğruladıkları yere bağlı olarak istemcilerle iki aşamalı doğrulama kullanırken kuruluş kullanıcı adı ve parolaları ile uygulama parolalarının bir birleşimini kullanmayı gerektirebilir. Şirket içi altyapıya karşı kimlik doğrulaması yapan istemciler için kuruluş kullanıcı adı ve parola kullanırsınız. Azure AD'ye karşı kimlik doğrulaması yapan istemciler için uygulama parolasını kullanırsınız.
* Varsayılan olarak, kullanıcılar uygulama parolaları oluşturamaz. Kullanıcıların uygulama parolaları oluşturmasına izin vermeniz gerekiyorsa, **tarayıcı dışı uygulamalarda oturum açmaları için kullanıcıların uygulama parolaları oluşturmasına izin** ver seçeneğini belirleyin.

## <a name="additional-considerations"></a>Ek Hususlar

Şirket içinde dağıtılan her bileşen için ek hususlar ve kılavuzlar için bu listeyi kullanın:

* Azure Multi-Factor Authentication’ı [Active Directory Federasyon Hizmetleri](multi-factor-authentication-get-started-adfs.md) ile ayarlayın.
* [RADIUS Kimlik Doğrulaması](howto-mfaserver-dir-radius.md) ile Azure MFA Sunucusu’nu kurun ve yapılandırın.
* Azure MFA Sunucusu'nu [IIS Kimlik Doğrulaması](howto-mfaserver-iis.md)ile ayarlayın ve yapılandırır.
* Azure MFA Server'ı Windows [Kimlik Doğrulaması](howto-mfaserver-windows.md)ile ayarlayın ve yapılandırır.
* AZURE MFA Sunucusu'nu [LDAP Kimlik Doğrulaması](howto-mfaserver-dir-ldap.md)ile ayarlayın ve yapılandırır.
* Azure MFA Server'ı [RADIUS'u kullanarak Uzak Masaüstü Ağ Geçidi ve Azure Çok Faktörlü Kimlik Doğrulama Sunucusu](howto-mfaserver-nps-rdg.md)ile ayarlayın ve yapılandırın.
* Azure MFA Server ve Windows Server Active [Directory](howto-mfaserver-dir-ad.md)arasındaki eşitlemi ayarlayın ve yapılandırır.
* [Azure Multi-Factor Authentication Sunucusu Mobil Uygulama Web Hizmeti’ni dağıtın](howto-mfaserver-deploy-mobileapp.md).
* LDAP veya RADIUS kullanan Cisco ASA, Citrix Netscaler ve Juniper/Pulse Secure VPN cihazları için [Azure Çok Faktörlü Kimlik Doğrulama](howto-mfaserver-nps-vpn.md) ile gelişmiş VPN Yapılandırması.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure MFA için bazı en iyi uygulamalar vurgulanırken, MFA dağıtımınızı planlarken kullanabileceğiniz başka kaynaklar da vardır. Aşağıdaki listede bu işlem sırasında size yardımcı olabilecek bazı önemli makaleler vardır:

* [Azure Çok Faktörlü Kimlik Doğrulama'daki Raporlar](howto-mfa-reporting.md)
* [İki aşamalı doğrulama kaydı deneyimi](../user-help/multi-factor-authentication-end-user-first-time.md)
* [Azure Çok Faktörlü Kimlik Doğrulama SSS](multi-factor-authentication-faq.md)
