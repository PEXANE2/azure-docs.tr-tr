---
title: Azure Multi-Factor Authentication için güvenlik kılavuzu-Azure Active Directory
description: Bu belgede Azure MFA ile Azure hesaplarının kullanılmasıyla ilgili rehberlik sunulmaktadır
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2756d39a93751271c8c7bf2a51108b9fe5b09b1e
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74208442"
---
# <a name="security-guidance-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Azure AD hesaplarıyla Azure Multi-Factor Authentication kullanmaya yönelik güvenlik kılavuzu

İki aşamalı doğrulama, kimlik doğrulama sürecini geliştirmek isteyen birçok kuruluş için tercih edilen seçenektir. Azure Multi-Factor Authentication (MFA), şirketlerin kullanıcıları için basit bir oturum açma deneyimi sağlarken bunların güvenlik ve uyumluluk gereksinimlerini karşılamasına yardımcı olur. Bu makalede, Azure MFA benimseme için planlama yaparken göz önünde bulundurmanız gereken bazı ipuçları ele alınmaktadır.

## <a name="deploy-azure-mfa-in-the-cloud"></a>Bulutta Azure MFA 'yı dağıtma

[Tüm kullanıcılarınız Için Azure MFA 'yı etkinleştirmenin](howto-mfa-getstarted.md)iki yolu vardır.

* Her Kullanıcı için lisans satın alın (Azure MFA, Azure AD Premium veya Enterprise Mobility + Security)
* Multi-Factor auth sağlayıcısı oluşturun ve Kullanıcı başına veya kimlik doğrulama başına ödeme yapın

### <a name="licenses"></a>Lisanslarının

![Lisansları kullanıcılara uygulama, etkinleştirme, bildirme](./media/multi-factor-authentication-security-best-practices/ems.png)

Azure AD Premium veya Enterprise Mobility + Security lisanslarınız varsa, zaten Azure MFA 'ya sahipsiniz. Kuruluşunuzun iki adımlı doğrulama özelliğini tüm kullanıcılara genişletmek için ek bir şey gerekmez. Yalnızca bir kullanıcıya lisans atamanız gerekir ve ardından MFA 'yı açabilirsiniz.

Multi-Factor Authentication ayarlarken aşağıdaki ipuçlarını göz önünde bulundurun:

* Kimlik doğrulama Multi-Factor auth sağlayıcısı oluşturmayın. Bunu yaparsanız, zaten lisanslarına sahip olan kullanıcılardan gelen doğrulama istekleri için ödeme yapabilirsiniz.
* Tüm kullanıcılarınız için yeterli lisans yoksa, kuruluşunuzun geri kalanını karşılamak için Kullanıcı başına Multi-Factor auth sağlayıcısı oluşturabilirsiniz. 
* Azure AD Connect yalnızca şirket içi Active Directory ortamınızı bir Azure AD diziniyle eşitliyorsanız gereklidir. Active Directory Şirket içi bir örneğiyle eşitlenmemiş bir Azure AD dizini kullanıyorsanız, Azure AD Connect gerekmez.

### <a name="multi-factor-auth-provider"></a>Multi-Factor auth sağlayıcısı

![Multi-Factor Authentication sağlayıcı](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Azure MFA içeren lisansınız yoksa, [BIR MFA kimlik doğrulama sağlayıcısı oluşturabilirsiniz](concept-mfa-authprovider.md).

Kimlik doğrulama sağlayıcısını oluştururken bir dizin seçmeniz ve aşağıdaki ayrıntıları göz önünde bulundurmanız gerekir:

* Multi-Factor auth sağlayıcısı oluşturmak için bir Azure AD dizini gerekmez, ancak bir tane ile daha fazla işlevsellik alırsınız. Kimlik doğrulama sağlayıcısını bir Azure AD diziniyle ilişkilendirdiğinizde aşağıdaki Özellikler etkinleştirilir:
  * İki aşamalı doğrulamayı tüm kullanıcılarınıza genişletme
  * Yönetim Portalı, özel Tebrikler ve raporlar gibi genel yöneticilerinize ek özellikler sunun.
* Şirket içi Active Directory ortamınızı bir Azure AD diziniyle eşitlerseniz DirSync veya Azure AD Eşitleme gerekir. Active Directory Şirket içi bir örneğiyle eşitlenmemiş bir Azure AD dizini kullanıyorsanız, DirSync veya Azure AD Eşitleme gerekmez.
* İşletmenize en uygun tüketim modelini seçin. Kullanım modelini seçtiğinizde, bunu değiştiremezsiniz. İki model şunlardır:
  * Kimlik doğrulaması başına: her doğrulama için sizi ücretlendirir. Belirli kullanıcılar için değil, belirli bir uygulamaya erişen herkes için iki aşamalı doğrulama istiyorsanız bu modeli kullanın.
  * Etkin Kullanıcı başına: Azure MFA için etkinleştirdiğiniz her bir kullanıcı için sizi ücretlendirir. Azure AD Premium veya Enterprise Mobility Suite lisanslarına sahip bazı kullanıcılarınız varsa ve bazıları olmadan bu modeli kullanın.

### <a name="supportability"></a>Desteklenebilirlik

Çoğu Kullanıcı, kimlik doğrulamak için yalnızca parolaları kullanmaya alışkın olduğundan, şirketiniz bu işlemle ilgili tüm kullanıcılara bir tanıma getirdiğinden önemlidir. Bu tanıma, kullanıcıların MFA ile ilgili küçük sorunlar için yardım masasına çağrı olasılığını azaltabilir. Ancak, MFA 'nın geçici olarak devre dışı bırakılması gerektiği bazı senaryolar vardır. Bu senaryoların nasıl işleneceğini anlamak için aşağıdaki yönergeleri kullanın:

* Mobil uygulama veya telefon bir bildirim veya telefon araması almadığı için, kullanıcının oturum açması gereken senaryoları işlemek üzere teknik destek personelinize eğitme. Teknik destek, bir kullanıcının iki aşamalı doğrulamayı "atlayarak" bir kez kimlik doğrulamasına izin vermek için [tek seferlik bir geçişe olanak](howto-mfa-mfasettings.md#one-time-bypass) tanıyabilir. Atlama geçicidir ve belirtilen sayıda saniye sonra süresi dolar.
* İki aşamalı doğrulamayı en aza indirmenin bir yolu olarak Azure MFA 'da [güvenilir IP 'ler özelliğini](howto-mfa-mfasettings.md#trusted-ips) göz önünde bulundurun. Bu özellik ile, yönetilen veya federal bir kiracının yöneticileri şirketin yerel intranetinden oturum açan kullanıcılar için iki aşamalı doğrulamayı atlayabilir. Özellikler Azure AD Premium, Enterprise Mobility Suite veya Azure Multi-Factor Authentication lisanslarına sahip Azure AD kiracılarında kullanılabilir.

## <a name="best-practices-for-an-on-premises-deployment"></a>Şirket içi dağıtım için en iyi uygulamalar

Şirketiniz MFA 'yı etkinleştirmek için kendi altyapısını kullanmaya karar verdiyseniz, Şirket [içi bir Azure Multi-Factor Authentication sunucusu dağıtmanız](howto-mfaserver-deploy.md)gerekir. MFA sunucusu bileşenleri aşağıdaki diyagramda gösterilmiştir:

Varsayılan MFA sunucusu bileşenleri](./media/multi-factor-authentication-security-best-practices/server.png) \*varsayılan olarak yüklenmeyen \** yüklü değil, varsayılan olarak etkinleştirilmemiştir ![

Azure Multi-Factor Authentication Sunucusu, Federasyon kullanarak bulut kaynaklarını ve şirket içi kaynakları güvenlik altına alabilir. AD FS sahip olmanız ve Azure AD kiracınızla Federasyon oluşturmanız gerekir.
Multi-Factor Authentication Sunucusu ayarlarken aşağıdaki ayrıntıları göz önünde bulundurun:

* Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanarak Azure AD kaynaklarını güvenli hale getirirken, ilk doğrulama adımı şirket içinde AD FS kullanılarak gerçekleştirilir. İkinci adım, talebin onaylanmasıyla şirket içinde gerçekleştirilir.
* AD FS Federasyon sunucunuza Azure Multi-Factor Authentication Sunucusu yüklemenize gerek yoktur. Ancak, AD FS için Multi-Factor Authentication bağdaştırıcısının AD FS çalıştıran bir Windows Server 2012 R2 üzerine yüklenmesi gerekir. Sunucuyu desteklenen bir sürüm olduğu sürece farklı bir bilgisayara yükleyebilirsiniz ve AD FS bağdaştırıcıyı AD FS Federasyon sunucunuza ayrı olarak yüklersiniz. 
* Multi-Factor Authentication AD FS bağdaştırıcısı yükleme Sihirbazı, Active Directory PhoneFactor Admins adlı bir güvenlik grubu oluşturur ve ardından AD FS hizmet hesabınızı bu gruba ekler. Etki alanı denetleyicinizde PhoneFactor Admins grubunun oluşturulduğunu ve AD FS hizmet hesabının bu gruba üye olduğunu doğrulayın. Gerekirse, AD FS hizmeti hesabınızı etki alanı denetleyicinizde PhoneFactor Admins grubuna el ile ekleyin.

### <a name="user-portal"></a>Kullanıcı Portalı

Kullanıcı Portalı, self servis özelliklerine izin verir ve tam bir kullanıcı yönetimi özellikleri kümesi sağlar. Internet Information Server (IIS) Web sitesinde çalışır. Bu bileşeni yapılandırmak için aşağıdaki yönergeleri kullanın:

* IIS 6 veya üstünü kullanın
* ASP.NET v 2.0.507207 'i yükleyip kaydetme
* Bu sunucunun bir çevre ağında dağıtılabildiğinden emin olun

### <a name="app-passwords"></a>Uygulama parolaları

Kuruluşunuz Azure AD ile SSO için federe ise ve Azure MFA 'yı kullanacaksanız, aşağıdaki ayrıntıları göz önünde bulundurun:

* Uygulama parolası Azure AD tarafından doğrulanır ve bu nedenle Federasyonu atlar. Federasyon yalnızca uygulama parolaları ayarlanırken kullanılır.
* Federasyon (SSO) kullanıcıları için, parolalar kuruluş KIMLIĞINDE depolanır. Kullanıcı şirketten ayrılırsa, bu bilgi DirSync kullanarak kuruluş KIMLIĞINE akacaktır. Hesabın devre dışı bırakılması/silinmesi üç saate kadar zaman alabilir. Bu, Azure AD 'de uygulama parolalarının devre dışı bırakılması/silinmesi gecikmeyebilir.
* Şirket için İstemci Erişimi Denetimi ayarları Uygulama Parolası tarafından onaylanmaz.
* Uygulama parolaları için şirket içi kimlik doğrulama günlüğü/Denetim yeteneği kullanılamaz.
* Bazı gelişmiş mimari tasarımları, kimlik doğrulamalarına bağlı olarak istemcilerle iki aşamalı doğrulamayı kullanırken kurumsal Kullanıcı adı ve parolaların ve uygulama parolalarının bir birleşimini kullanmanızı gerektirebilir. Şirket içi altyapıya karşı kimlik doğrulaması yapan istemciler için bir kuruluş Kullanıcı adı ve parolası kullanırsınız. Azure AD kimlik doğrulaması yapan istemciler için uygulama parolasını kullanırsınız.
* Varsayılan olarak, kullanıcılar uygulama parolaları oluşturamaz. Kullanıcıların uygulama parolaları oluşturmalarına izin vermeniz gerekiyorsa, **kullanıcıların tarayıcı olmayan uygulamalarda oturum açmak için uygulama parolaları oluşturmasına Izin ver** seçeneğini belirleyin.

## <a name="additional-considerations"></a>Ek konular

Şirket içinde dağıtılan her bir bileşene ilişkin ek konular ve yönergeler için bu listeyi kullanın:

* Azure Multi-Factor Authentication’ı [Active Directory Federasyon Hizmetleri](multi-factor-authentication-get-started-adfs.md) ile ayarlayın.
* [RADIUS Kimlik Doğrulaması](howto-mfaserver-dir-radius.md) ile Azure MFA Sunucusu’nu kurun ve yapılandırın.
* Azure MFA sunucusunu [IIS kimlik doğrulamasıyla](howto-mfaserver-iis.md)ayarlayın ve yapılandırın.
* Azure MFA sunucusunu [Windows kimlik doğrulamasıyla](howto-mfaserver-windows.md)ayarlayın ve yapılandırın.
* Azure MFA sunucusunu [LDAP kimlik doğrulamasıyla](howto-mfaserver-dir-ldap.md)ayarlayın ve yapılandırın.
* RADIUS kullanarak Azure MFA sunucusunu [Uzak Masaüstü Ağ Geçidi ve azure Multi-Factor Authentication sunucusu](howto-mfaserver-nps-rdg.md)ayarlayın ve yapılandırın.
* Azure MFA sunucusu ile [Windows Server Active Directory](howto-mfaserver-dir-ad.md)arasında eşitleme ayarlayın ve yapılandırın.
* [Azure Multi-Factor Authentication Sunucusu Mobil Uygulama Web Hizmeti’ni dağıtın](howto-mfaserver-deploy-mobileapp.md).
* Cisco ASA, Citrix NetScaler ve Juniper/Pulse Secure VPN gereçlerinin LDAP veya RADIUS kullanılarak [GELIŞMIŞ vpn Multi-Factor Authentication yapılandırması](howto-mfaserver-nps-vpn.md) .

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure MFA için bazı en iyi yöntemler vurgularken, MFA dağıtımınızı planlarken de kullanabileceğiniz başka kaynaklar vardır. Aşağıdaki listede, bu işlem sırasında size yardımcı olabilecek bazı önemli makaleler bulunur:

* [Azure Multi-Factor Authentication raporları](howto-mfa-reporting.md)
* [İki adımlı doğrulama kayıt deneyimi](../user-help/multi-factor-authentication-end-user-first-time.md)
* [Azure Multi-Factor Authentication SSS](multi-factor-authentication-faq.md)
